# Slither 用法

* 安装

```
python3 -m pip install slither-analyzer
# 在 .bash_profile 增加：
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"  # 如果使用虚拟环境
# 执行
pip rehash 
slither --version
```

* 安装 solidity 编译包

    * solc-select install 0.8.24
    * solc-select use 0.8.24

* 进行检测

```
slither src/BoboNft.sol  --json res.json
```

* 梳理合约关系

```
slither src/BoboNft.sol --print inheritance-graph

# 转为图片
dot -Tpng src/BoboNft.sol.inheritance-graph.dot -o test.png
```
