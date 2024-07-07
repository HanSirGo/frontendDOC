# Nginx配置Keepalived高可用

## 介绍

Keepalived是一个开源的高可用性(HA, High Availability)解决方案，主要设计用于实现服务器的故障切换(failover)。它通过VRRP协议(Virtual Router Redundancy Protocol，虚拟路由器冗余协议)来实现这一目标，从而保证关键服务的不间断运行。Keepalived不仅能够监测服务器健康状态，还能管理IP地址的虚拟浮动，确保在主服务器发生故障时，备份服务器能够无缝接管服务，提高服务的稳定性和可靠性。

**核心功能包括：**

1. **高可用性（HA）**: Keepalived通过VRRP提供高可用性服务，可以配置为主机(MASTER)和备份主机(BACKUP)角色。当主服务器出现故障时，备份服务器会自动接管VIP（虚拟IP地址）及相关服务，实现无中断切换。
2. **健康检查**: Keepalived能够定期对服务器上的服务或系统状态进行健康检查，如检查HTTP服务、TCP端口、脚本执行结果等，确保服务正常运行。
3. **负载均衡**: 虽然Keepalived最初不是专门作为负载均衡器设计的，但其VRRP功能可以与LVS（Linux Virtual Server，Linux虚拟服务器）集成，实现基于IP层或内容请求的负载均衡，进一步提升系统的扩展性和可靠性。
4. **配置灵活性**: Keepalived使用配置文件(通常是`/etc/keepalived/keepalived.conf`)来定义VRRP实例、优先级、通告间隔、健康检查脚本等，便于管理和调整。
5. **通知机制**: 在状态变化时，Keepalived可以通过电子邮件、SNMP陷阱等方式通知管理员，以便及时了解系统状态。

总的来说，Keepalived是构建高度可靠互联网服务架构的关键组件，广泛应用于Web服务器、数据库服务、邮件服务等需要持续运行的关键业务场景中。

## 架构图

首先需要准备两台一样的Nginx代理服务器，从而代理到后端的服务。

我们不可能将某个单独的Nginx代理服务器的IP提供给用户访问，因为一旦某个Nginx代理出现问题不能及时处理。所以需要让用户通过访问VIP的方式来提供代理服务。

如果后端Nginx代理或者Keepalived出现故障那么VIP也会随之漂移到从节点，图中也就是Nginx02，从而保障Nginx代理服务器的高可用，而用户侧无感知。

![1720267948384](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267948384.png)

Nginx01+Keepalived01出现故障VIP自动漂移到Nginx02+Keeplived02节点上，从而保障高可用性

![1720267959028](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267959028.png)

下面，就来具体实现上图的功能

## 配置Nginx

配置nginx.conf（两台主机）

```
[root@localhost html]# cat ../conf/nginx.conf

worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       80;
        server_name  localhost;

        location / {
            root html;
            index index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}

```

第一台主机，编辑index.html

```
[root@localhost html]# cat index.html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
</head>
<body>
<p>个人博客站点：www.qiuyl.com<br/><br/>
主机01 IP：192.168.200.7
</p>
</body>
</html>
```

第二台主机，编辑index.html

```
[root@localhost html]# cat index.html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
</head>
<body>
<p>个人博客站点：www.qiuyl.com<br/><br/>
主机02 IP：192.168.200.10
</p>
</body>
</html>
```

## 配置Keepalived

1）安装Keepalived（两台主机）

```
[root@localhost]# yum install -y keepalived
```

2）配置keepalived.conf

配置第一台主机

> 这里只做keepalived最基本配置

```
[root@localhost ~]# cat /etc/keepalived/keepalived.conf
! Configuration File for keepalived

global_defs {
   router_id LVS_7
}

vrrp_instance VI_7 {
    state MASTER
    interface ens33
    virtual_router_id 51
    priority 100  # 优先级高于从
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.200.254
    }
}

# 启动
[root@localhost]# systemctl start keepalived
```

配置第二台主机

```
[root@localhost ~]# cat /etc/keepalived/keepalived.conf
! Configuration File for keepalived

global_defs {
   router_id LVS_DEVEL
}

vrrp_instance VI_10 {
    state MASTER
    interface ens33
    virtual_router_id 51
    priority 50  # 优先级低于主
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.200.254
    }
}

# 启动
[root@localhost]# systemctl start keepalived
```

由于，咱们将第一台主机作为keepalived的Master，他的优先级要高于第二台主机，所有只要第一台keepalived服务存活那么VIP就会在该节点

```
[root@Nginx01 keepalived]# ip -4 a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 192.168.200.7/24 brd 192.168.200.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet 192.168.200.254/32 scope global ens33   # VIP
       valid_lft forever preferred_lft forever
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
```

访问VIP，由于VIP节点在第一台主机上，那么访问的也将是第一台主机

![1720268011097](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720268011097.png)

访问第一台主机

![1720268021379](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720268021379.png)

访问第二台主机

![1720268032184](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720268032184.png)

将第一台主机的Keepalived停止后，可以看见咱们的VIP已经切换

```
# 将第一台主机的Keepalived停止
[root@Nginx01 ~]# systemctl stop keepalived

# 在第二台主机上查看ip
[root@Nginx02 ~]# ip -4 a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 192.168.200.10/24 brd 192.168.200.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet 192.168.200.254/32 scope global ens33
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
```

访问到第二台主机Nginx代理

![1720268046929](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720268046929.png)

## 编写脚本检查Nginx状态

上面，我们是通过默认的Keeplived的健康检查Keeplived服务状态，来实现VIP切换的。

那么，如果我们Nginx服务故障了而Keeplived服务没有故障，那么我们VIP是不会进行切换的，这个时候我们可以通过编写脚本的方式实现检查Nginx服务状态来判定VIP是否切换。

有兴趣的朋友可以做下。

