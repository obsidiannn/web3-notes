
# Arbitrum 

### 特性
* 以太坊L2方案
* 支持EVM标准
* 以太坊计算 & 存储转移到链下
* 网络
    * Arbitrum 主网： One & Nova
    * 测试网： Sepolia & stylus testnet


* Arbitrum Classic 是第一个rollup 链

* 虚拟机 AVM： 用来执行 & 管理 Arbitrum 链
* 状态机： ArbOS： 处理Arbitrum链逻辑跟状态更新

* Arbitrum 排序器按照交易先后顺序执行，以太坊是按照交易费用

### Arbitrum One 特性

* 排序 + 确定性执行
* Geth为核心
* 执行与证明分离
* 交互式欺诈证明： 缩小到最小分歧

### Arbitrum Nova 
* 相比One ，使用了Any Trust 模式， 创建了一个被信任的小组


### Arbitrum Orbit: 一个Layer3 方案
* 允许替代erc20 支付gas

### Arbitrum Stylus 

* 使用 webassembly 编译
* 性能相比 solidity 更强
* 支持 One、Nova 和 Orbit 