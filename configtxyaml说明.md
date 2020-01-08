
Profiles:
    #随便起名字，两个组织一个orderer的创世区块，就是定义了orderer以及org的配置
    TwoOrgsOrdererGenesis:
        #配置区块链网络向下兼容性的
        Capabilities:
            <<: *ChannelCapabilities
        Orderer:
        #yaml中这种语法的含义代表将OrdererDefaults的所有数值粘贴到这个位置
            <<: *OrdererDefaults
            Organizations:
                - *OrdererOrg
            Capabilities:
                <<: *OrdererCapabilities
        #单词是联盟的意思，由Org1和Org2构成的私有链
        Consortiums:
            SampleConsortium:
                Organizations:
                    - *Org1
                    - *Org2
    #定义谁可以加入channel
    TwoOrgsChannel:
        Consortium: SampleConsortium
        Application:
            <<: *ApplicationDefaults
            Organizations:
                - *Org1
                - *Org2
            Capabilities:
                <<: *ApplicationCapabilities
Organizations:
    - &OrdererOrg
        Name: OrdererOrg
        ID: OrdererMSP
        MSPDir: crypto-config/ordererOrganizations/example.com/msp
    - &Org1
        Name: Org1MSP
        ID: Org1MSP
        MSPDir: crypto-config/peerOrganizations/org1.example.com/msp
        AnchorPeers:
            - Host: peer0.org1.example.com
              Port: 7051
    - &Org2
        Name: Org2MSP
        ID: Org2MSP
        MSPDir: crypto-config/peerOrganizations/org2.example.com/msp
        AnchorPeers:
            - Host: peer0.org2.example.com
              Port: 7051

Orderer: &OrdererDefaults
    OrdererType: solo
    Addresses:
        - orderer.example.com:7050
    #orderer节点两秒钟提交给peer节点一次更新，没2s产生一个区块
    BatchTimeout: 2s
    BatchSize:
        #最大消息数量，消息池中达到此数量会立即出块
        MaxMessageCount: 10
        #最大区块大小，消息池中数据达到此数值之后也会立即产生区块
        AbsoluteMaxBytes: 99 MB
        #喜欢的区块大小
        PreferredMaxBytes: 512 KB
    #如果打开了kafka，在此配置一下ip地址以及端口
    Kafka:
        Brokers:
            - 127.0.0.1:9092
    Organizations:
Application: &ApplicationDefaults
    Organizations:
Capabilities:
    Global: &ChannelCapabilities
        V1_1: true
    Orderer: &OrdererCapabilities
        V1_1: true
    Application: &ApplicationCapabilities
        V1_1: true
