## 新建项目dir
```
mkdir my-network
cd my-network
```
## 创建crypto-config.yaml文件写入组织配置
```
touch crypto-config.yaml
vim crypto-config.yaml
```
## 在crypto-config.yaml输入内容:
```
OrdererOrgs:
  - Name: Orderer
    Domain: example.com
    Specs:
      - Hostname: orderer
PeerOrgs:
  - Name: Org1
    Domain: org1.example.com
    Template:
      Count: 2
    Users:
      Count: 1
  - Name: Org2
    Domain: org2.example.com
    Template:
      Count: 2
    Users:
      Count: 1
```
### 配置说明：
- OrdererOrgs：排序节点所在的组织
  - Name： 组织名称
  - Domain： 域名
  - Specs：详细描述
  - Hostname：主机名
- PeerOrgs：peer节点的组织
  - Name：组织名称
  - Domain：域名
  - Template.Count:组织内包含多少个peer节点

> hyperledger可以有多个组织
> 一个组织可以有多个peer，peer的作用是记账、同步账本
> 一个组织可以有一个或者多个user，user是可以操作peer的
> 一个组织中必须要有一个admin管理员

## 配置临时环境变量，使用cryptogen工具生成证书
```
export PATH=${PWD}/../bin:${PWD}:$PATH
cryptogen generate --config=./crypto-config.yaml
```
执行完指令输出显示（同时生成crypto-config文件夹，里边存放的是一大堆公钥私钥）：
```
org1.example.com
org2.example.com
```
> 定义的orderer以及peer信息会被写入到创世区块中，防止黑客攻击

## 将forst-network中的configtx.taml拷入到当前文件夹
```
cd ../first-network
cp configtx.yaml ../my-network/
```
## 创建Orderer的创世区块,参数名字要与configtx.yaml中定义的名字相同，output后边参数是创世区块写到那个文件夹
```
configtxgen -profile TwoOrgsOrdererGenesis -outputBlock ./channel-artifacts/genesis.block
```
显示输出:
```
2020-01-08 17:43:22.441 CST [common/tools/configtxgen] main -> INFO 001 Loading configuration
2020-01-08 17:43:22.452 CST [common/tools/configtxgen] doOutputBlock -> INFO 002 Generating genesis block
2020-01-08 17:43:22.452 CST [common/tools/configtxgen] doOutputBlock -> INFO 003 Writing genesis block
```
## 生成channel.tx文件
```
configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel-artifacts/channel.tx -channelID mychannel
```
显示输出：
```
2020-01-08 17:47:46.088 CST [common/tools/configtxgen] main -> INFO 001 Loading configuration
2020-01-08 17:47:46.098 CST [common/tools/configtxgen] doOutputChannelCreateTx -> INFO 002 Generating new channel configtx
2020-01-08 17:47:46.119 CST [common/tools/configtxgen] doOutputChannelCreateTx -> INFO 003 Writing new channel tx
```
## 指定Org1的锚节点
```
configtxgen -profile TwoOrgsChannel -outputAnchorPeersUpdate ./channel-artifacts/Org1MSPanchors.tx -channelID mychannel -asOrg Org1MSP
```
显示输出:
```
2020-01-08 17:51:03.414 CST [common/tools/configtxgen] main -> INFO 001 Loading configuration
2020-01-08 17:51:03.423 CST [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 002 Generating anchor peer update
2020-01-08 17:51:03.424 CST [common/tools/configtxgen] doOutputAnchorPeersUpdate -> INFO 003 Writing anchor peer update
```
##指定o
