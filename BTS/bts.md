cybex链的代码
https://github.com/NebulaCybexDEX/bitshares-core
可以从programs/witness_node/main.cpp开始阅读
witness_node是区块链中节点的代码。

节点的启动参数和运行方式可以参考以下文档
http://docs.bitshares.org/testnet/private-testnet.html
http://docs.bitshares.org/integration/apps/node.html

带着以下问题去读代码
1.交易(transaction)如何接收、传播、落地
2.区块(block)和交易(transaction)的关系
3.区块如何产生、传播
4.区块的打包者如何挑选
5.交易和区块如何签名
6.区块链分叉时如何决策


