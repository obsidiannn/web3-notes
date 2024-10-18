# bluePrint 

* github https://github.com/ton-org/blueprint

## 命令

* 把 .fc 文件编译为 .cell 文件

```shell   
    npx func-js contracts/counter.fc --boc build/counter.cell
```
会生成一个 二进制文件，包含tvm字节码，可以不是到链上


# 部署到测试网络 

npx blueprint run deployCounterContract