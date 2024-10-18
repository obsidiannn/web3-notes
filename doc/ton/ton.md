
1. 官网 https://docs.ton.org/mandarin/develop/dapps/
2. 官方 github https://github.com/ton-blockchain

3. 第一个nft https://docs.ton.org/mandarin/develop/dapps/tutorials/collection-minting


## 测试网

* 浏览器: https://testnet.tonscan.org
* Web 钱包: https://wallet.ton.org?testnet=true
* 浏览器扩展: 使用 mainnet browser extension 并 进行此操作。
* 测试网 TON Center API: https://testnet.toncenter.com
* 测试网 HTTP API: https://testnet.tonapi.io/
* 测试网桥接: https://ton.org/bridge?testnet=true


## 教程
* ton 教程 https://stepik.org/lesson/1298435/step/4?unit=1313186

* 带你写合约 **http://tonhelloworld.com/02-contract/**

* func https://github.com/romanovichim/TonFunClessons_Eng

* func 标准库 https://docs.ton.org/mandarin/develop/func/stdlib
* cell 单元 


```
() recv_internal(int msg_value, cell in_msg, slice in_msg_body) impure {

}
```

* 链上的合约，每mb每年4ton，如果余额不足，则会销毁

* 一个func 函数入参
    * msg_value: 收到了多少ton 币
    * in_msg: 收到的完整请求 （类型是cell）
    * in_msg_body: 信息的body的初始索引

* Ton 合约
    * 持续存储空间（c4 空间）
    * getter 方法，允许外部读取数据
    * 持续存储： set_data
        * 一个cell可以声明多个cell句柄`ref`（4个）

* cell
    * 拆解cell： slice cs = in_msg.begin_parse();
    * 读取数据： cs~load_uint(4) 
        * 如果重复执行两遍 `cs~load_uint(4) `，那么指针就会移动两次

* 类型
    * 存储地址变量 是 slice

* 工具
    * disintar/toncli — toncli是用于构建、部署和测试FunC合约的命令行界面。
    * MyLocalTON — MyLocalTON用于在您的本地环境中运行私有TON区块链。
    * tonwhales.com/tools/boc — BOC解析器
    * tonwhales.com/tools/introspection-id — crc32生成器
    * @orbs-network/ton-access — 去中心化API网关

* 交易费用
    * 跟以太坊一样，使用gas作为费用
    * 1gas = 1000 nanotons = 0.000 001 ton
    * 当前gas 被声明在网络配置内 [param20](https://explorer.toncoin.org/config?workchain=-1&shard=8000000000000000&seqno=22185244&roothash=165D55B3CFFC4043BFC43F81C1A3F2C41B69B33D6615D46FBFD2036256756382&filehash=69C43394D872B02C334B75F59464B2848CD4E23031C03CA7F3B1F98E8A13EE05#configparam20)

    * gas 可以通过66%的验证者投票变更

* 存储费用
    * 合约占用空间而积累的费用，以年为单位结算
    * storage_fee = (cells_count * cell_price + bits_count * bits_price) / 2^16 * time_delta

* ton storage
    * 一种可以用于ton存储的文件服务，需要自行部署

* 通过func 计算费用
    [link](https://github.com/ton-blockchain/token-contract/blob/main/misc/forward-fee-calc.fc)