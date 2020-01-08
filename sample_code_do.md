## 下载fabric官方docker镜像
```
dockerFabricPull()函数
docker pull hyperledger/fabric-peer:x86-64-1.1.0
docker pull hyperledger/fabric-orderer:x86-64-1.1.0
docker pull hyperledger/fabric-ccenv:x86-64-1.1.0
docker pull hyperledger/fabric-tools:x86-64-1.1.0
```
## 下载第三方docker镜像
```
dockerThirdPartyImagespull()
拉取第三方镜像couchdb  kafka  zookeeper
docker pull hyperledger/fabric-couchdb:x86_64-0.4.6
docker pull hyperledger/fabric-kafka:x86_64-0.4.6
docker pull hyperledger/fabric-zookeeper:x86_64-0.4.6
```
## 拉取CA镜像
```
dockerCaPull() 函数
拉取的ca的镜像（证书管理中心）
docker pull hyperledger/fabric-ca:x86-64-1.1.0
```
## 安装samples
```
sampleInstall() 函数
先看fabric-samples文件是否存在，不存在的话clone示例代码仓库
```
## 
```
binaryIncrementalDownload() 函数
工具函数，用来下载以及校验
```
## 
```
bianriesInstall()函数
下载平台相关的二进制文件 生成证书  配置channel等
下载CA相关去的二进制文件
```
## 
```
dockerInstall() 函数
没有安装docker给出提示，安装了docker的话执行 dockerFabricpull() dockerCaPull() dockerThirdPartyImagesPull() 
```