# Foundry

* 一个solidity 框架
* 测试、调试、部署
* [link](https://learnblockchain.cn/docs/foundry/i18n/zh/)
* [油管教程](https://www.youtube.com/watch?v=tgs5q-GJmg4&list=PLO5VPQH6OWdUrKEWPF07CSuVm3T99DQki&ab_channel=SmartContractProgrammer)
### 模块

* Forge 测试。
* Cast 合约进行交互，发交易，查询链上数据。
* Anvil 模拟一个私有节点。
* Chisel 命令行快速的有效的实时的写合约，测试合约。


### 安装 

```shell
curl -L https://foundry.paradigm.xyz | bash
foundryup
forge --version 
```

* forge init xx
* forge build 编译项目
* forge test --match-path test/Counter.t.sol
* forge create 部署

```shell
forge create src/BoboNft.sol:BoboNFT --broadcast --rpc-url=http://127.0.0.1:8545 --private-key=0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80 --constructor-args "boboNFT" "BONFT" 
```
* forge remappings 重映射依赖
```
引入依赖（github源码）
forge install transmissions11/solmate
forge install OpenZeppelin/openzeppelin-contracts@v5.2.0
```

### 使用soldeer 作为包管理器

* [soldeer 仓库地址](https://soldeer.xyz/)
* forge soldeer init
* forge soldeer install @openzeppelin-contracts~5.0.2

常用配置
```toml
[profile.default]
solc = "0.8.17"
src = "src"
out = "out"
libs = ["dependencies"]
gas_reports = ["*"] 

[dependencies]
forge-std = "1.9.6"
"@openzeppelin-contracts" = {version= "5.2.0"}
solmate = "6.8.0"

[soldeer]
remappings_generate = true
remappings_regenerate = false
remappings_version = false
remappings_location = "txt"
recursive_deps = true
```

### forge 验证gas消耗

* gas 测试报告
    * forge test --gas-report

```
gas_reports = ["*"]
```

* gas 函数快照

```shell
forge snapshot
forge snapshot --match-path contracts/test/ERC721.t.sol
```

---
## Cast模块
>  用于执行以太坊 RPC 调用的命令行工具, 可以远程调用合约函数

```
cast call 0x6b175474e89094c44da98b954eedeac495271d0f "totalSupply()(uint256)" --rpc-url https://eth-mainnet.alchemyapi.io/v2/Lc7oIGYeL_QvInzI0Wiu_pOZZDEKBrdf

```

---

## Anvil 模块

> 附带的本地测试网节点。 你可以使用它从前端测试你的合约或通过 RPC 进行交互。

```
anvil
```

--- 

### chisel 模块

> 高级 Solidity REPL。它可用于在本地或分叉网络上快速测试 Solidity 片段

---

### 分叉测试

> 模拟主网分叉：把活跃的区块状态复制到本地，在本地使用，环境更真实

* 复制anvil fork

```
anvil --fork-url https://eth.merkle.io --fork-block-number 19000000
```

* 创建分叉
```solidity
function setUp() public {
    mainnetFork = vm.createFork(MAINNET_RPC_URL);
    optimismFork = vm.createFork(OPTIMISM_RPC_URL);
}
```

* 选择分叉

```solidity
function testCanSelectFork() public {
    vm.selectFork(mainnetFork);
    assertEq(vm.activeFork(), mainnetFork);
}
```

```solidity
contract SimpleStorageContract {
    uint256 public value;

    function set(uint256 _value) public {
        value = _value;
    }
}

// 创建并测试持久性合约
function testCreatePersistentContract() public {
    // 首先，选择一个分叉环境
    vm.selectFork(mainnetFork);
    // 在该分叉环境中部署并初始化合约
    SimpleStorageContract simple = new SimpleStorageContract();
    simple.set(100);
    // 确认合约的状态设置正确
    assertEq(simple.value(), 100);

    // 接下来，将合约标记为持久性
    vm.makePersistent(address(simple));
    // 验证合约已被正确标记为持久性
    assert(vm.isPersistent(address(simple)));

    // 然后，切换到另一个分叉环境
    vm.selectFork(optimismFork);
    // 验证即使在新的分叉环境中，合约仍被标记为持久性
    assert(vm.isPersistent(address(simple)));

    // 最后，确认持久性合约的状态在新的分叉环境中保持不变
    assertEq(simple.value(), 100);
}
```


### 模糊测试

```solidity
// 取amount > 0.1的参数
vm.assume(amount > 0.1 ether);
```

### 不变性测试
