BitShares 2.0 多节点私链部署
https://blog.csdn.net/ggq89/article/details/80234262

依赖：  
sudo apt-get install gcc-4.9 g++-4.9 cmake make \
                     libbz2-dev libdb++-dev libdb-dev \
                     libssl-dev openssl libreadline-dev \
                     autoconf libtool git libcurl4-openssl-dev


application::initialize 创建文件

witness_node --create-genesis-json=my-genesis.json
create_example_genesis：

witness_node --data-dir data/ --genesis-json my-genesis.json --seed-nodes "[]"


--seed-nodes
reset_p2p_node
         reset_p2p_node(_data_dir);
         reset_websocket_server();
         reset_websocket_tls_server();

enable_shared_from_this

graphene::chain::cybex::init(node->chain_database().get());
node->startup();
node->startup_plugins();
插件

broadcast
http://docs.bitshares.org/integration/network-setup.html


block_production_loop

node_delegate  区块代表  

区块产生 


	witness_plugin::maybe_produce_block{
		db.generate_block
		p2p_node().broadcast
		{
			node_impl::broadcast
		}
		bool result = _chain_db->push_block( 
								blk_msg.block,
								(_is_block_producer | _force_validate) ?
	                 database::skip_nothing : database::skip_transaction_signatures );
	}
 
 
 node cpp 


处理交易 
on_message
{
 node_impl::process_ordinary_message
  {
	application_impl::handle_transaction
  }
}




peer_connection  端对端链接信息类

node_delegate  reports status to client or fetch data from client
message 消息类


区块链分叉时如何决策
//提到分叉
application_impl::handle_block
{
}



reset_websocket_server 


Graphene 源码阅读 ~ 基础知识
https://steemit.com/bitshares/@cifer/graphene

Graphene 源码阅读 ~ 数据库篇 ~ 对象模型
https://steemit.com/bitshares/@cifer/4o1u9e-graphene

Graphene 源码阅读 ~ 数据库篇 ~ 对象反射
https://steemit.com/bitshares/@cifer/5b42mk-graphene

Graphene 源码阅读 ~ 数据库篇 ~ 对象序列化
https://steemit.com/bitshares/@cifer/vdnvb-graphene

Graphene 源码阅读 ~ 数据库篇 ~ 对象索引库
https://steemit.com/bitshares/@cifer/7hnz25-graphene

Graphene 源码阅读 ~ RPC 篇 ~ 通信协议与服务端实现
https://steemit.com/bitshares/@cifer/graphene-rpc

Graphene 源码阅读 - RPC 篇 - API 注册机制
https://steemit.com/bitshares/@cifer/graphene-rpc-api


Graphene 源码阅读 ~ 数据库篇 ~ 区块数据库
https://steemit.com/bitshares/@cifer/2uwh3m-graphene

Graphene 源码阅读 ~ 番外篇 ~ 出块判断逻辑
https://steemit.com/bitshares/@cifer/69trzr-graphene

Graphene 源码阅读 - 交易篇 - 交易费用
https://steemit.com/bitshares/@cifer/6ai6f6-graphene

Graphene 源码阅读 - 架构篇 - 节点实例启动流
https://steemit.com/bitshares/@cifer/graphene---0w1jzg9j8v

比特股全节点witness_node参数翻译
https://www.jianshu.com/p/9a58ad875cc3

http://docs.bitshares.org/integration/index.html#basic-knowledge
