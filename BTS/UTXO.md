
UTXO（Unspent Transaction Output） 未花费的交易输出


当一笔交易输出是， 费用来源 分为两种，一种是当前一次的挖矿奖励。另外一种是别人的转账。
当时挖矿奖励时，为该币第一次被花费，为合法的。
当为别人转账时，需要验证，你持有的该笔金额，是否被花费。（UTXO）。
比特币系统能够验证该笔金额是否被花费。 支出方确认拥有该笔utxo的所有权。
两个天剑都满足，则说明该笔支出成立。


除创世区块中的交易（genesis block)夕卜，每笔交易必须要有资金来源。资金来源有两种，一种
是挖矿奖励（依照固定算法实现的货币发行），出现在每个block的第一笔交易中；另一种是先前的交易中未曾使用的某个Tx_out(交易输出),即UTXO。
 支出方要出示证据来证明自己对该Tx_out 拥有所有权，而比特币系统则要验证该Tx_out是真的未被花费（是否是UTXO )以及支
出方是否有权将其花费。


http://www.sohu.com/a/113402732_467759


http://www.cocoachina.com/blockchain/20180409/22949.html


什么是UTXO？浅谈比特币账户模型
http://new.qq.com/omn/20180309/20180309G10YO0.html


1.4.5　比特币账户模型：UTXO

http://book.51cto.com/art/201711/558936.htm



