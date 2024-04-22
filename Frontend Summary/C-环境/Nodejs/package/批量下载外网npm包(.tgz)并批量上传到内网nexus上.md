> **背景**：内网环境基于nexus搭建好了npm仓库，但是没有任何办法连接互联网。
>
> **痛点**：前端项目依赖多且碎，怎么解决npm包批量下载和上传到私有仓库的问题?
### 在互联网环境下载
> 基于npm view及递归算法命令获取依赖包列表或基于--package-lock-only生成pack-lock.json文件从中读取依赖包列表；

#### 1. 将依赖文件放到当前文件夹（和 .ps1 文件同级）
**将"yarn.lock","package-lock.json","package.json" 放到当前文件夹 **
##### index.ps1 (数字1)

```javascript
param (
	[parameter(Mandatory=$true)]
	[ValidateSet("yarn.lock","package.json","package-lock.json")]
	[string] $Source_file
)

switch($Source_file){
  "yarn.lock" {
    ./download-lock.ps1 -Source_file yarn.lock
  }
  "package-lock.json" {
    ./download-lock.ps1 -Source_file package-lock.json
  }
  "package.json" {
    ./download-package.ps1 -Source_file package.json
  }
}
```
##### download-lock.ps1
```javascript
param (
	[parameter(Mandatory=$true)]
	[ValidateSet("yarn.lock","package-lock.json")]
	[string] $Source_file
)

$Reg1=[regex]'resolved.*'
$Reg2=[regex]'https?:\/\/.*\.\w+'
$Reg3=[regex]'([^/]+)?\.(\w+)$'

[System.Collections.ArrayList]$Lines=@()

# create new folder
new-item "./packs" -Type Directory -Force | Out-Null

foreach ($line in Get-Content $Source_file) {
	if($line -match $Reg1) {
		$url=$line | Select-String $Reg2 -AllMatches | ForEach-Object {$_.Matches.Value}
        if($url){
		    $Lines.Add($url) | Out-Null
        }
	}
}
# remove duplicates
$Lines=$Lines | select -Unique
echo "$($Lines.Count) packages"

foreach ($url in $Lines) {
    # extract file name
	$File_name=$url | Select-String $Reg3 -AllMatches | ForEach-Object{$_.Matches.Value}
    echo "downloading $($url)"
    # request url
	Invoke-WebRequest $url -OutFile "./packs/$($File_name)"
}
```
##### download-package.ps1
```javascript
param (
	[parameter(Mandatory=$true)]
	[ValidateSet("package.json")]
	[string] $Source_file
)

# parse json to hashtable
$jsonObj=Get-Content -Raw $Source_file | ConvertFrom-Json 

$jsonHash=@{}

# search package.json and extract package name & version from dependencies/devDependencies

# add package.dependencies to hashtable
foreach($property in $jsonObj.dependencies.psobject.properties) {
  $jsonHash[$property.name]=$property.value
}

# add package.devDependencies to hashtable
foreach($property in $jsonObj.devDependencies.psobject.properties) {
  $jsonHash[$property.name]=$property.value
}

echo "package count: $($jsonHash.keys.count)"

# create new folder
new-item "./packs" -Type Directory -Force | Out-Null

# send GET requests and download packages
function request($url,$output){
  return Invoke-WebRequest $url -OutFile "./packs/$($output)"
}

# loop hashtable
foreach($key in $jsonHash.keys){
  # concat strings to url link
  $matches= $key | select-string '([^\/]+)+' -allmatches | ForEach-Object{$_.Matches.Value}
  $dep=$key
  $pack=$key
  if($matches.count -gt 1){
    $pack=$matches[-1]
  }

  # find exact version number based on version mark rules
  $ver=$jsonHash[$key] | select-string '[\d]+\.[\d]+\.[\d]+' -allmatches | ForEach-Object{$_.Matches.Value}
  
  $url = "https://registry.npmjs.org/${dep}/-/${pack}-${ver}.tgz"
  $file_name="${pack}-${ver}.tgz"

  echo "downloading $($file_name)"
  $res = request -url $url -output $file_name
  # echo "$($file_name) response: $($res)"
}
```
#### 2. powershell 运行 index.ps1 文件
**右键 index.ps1 文件 ；使用 PowerShell 运行，输入参数  "yarn.lock","package-lock.json","package.json"**
> 参数 -source_file 接受三种类型 “yarn.lock“，”package-lock.json“，”package.json” （package.json 文件有点问题）

### 上传
> 利用npm publish命令上传

#### 1. 将压缩包放到 ./transfer 文件夹
#### 2. powershell 运行 upload.ps1
##### upload.ps1
```javascript

$source_file='./transfer'

# read file
foreach($file in (get-childitem -path $source_file -name)){
  $path="$($source_file)/$($file)"
  # publish 
  npm -silent publish $path --registry=http://nexus.safewx.cn/repository/npm_hosted/ 
  echo "published $($file) success!"
}

```