BitShares 2.0 ��ڵ�˽������
https://blog.csdn.net/ggq89/article/details/80234262

������  
sudo apt-get install gcc-4.9 g++-4.9 cmake make \
                     libbz2-dev libdb++-dev libdb-dev \
                     libssl-dev openssl libreadline-dev \
                     autoconf libtool git libcurl4-openssl-dev


application::initialize �����ļ�

witness_node --create-genesis-json=my-genesis.json
create_example_genesis��

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
���

broadcast
http://docs.bitshares.org/integration/network-setup.html


block_production_loop

node_delegate  �������  

������� 


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


������ 
on_message
{
 node_impl::process_ordinary_message
  {
	application_impl::handle_transaction
  }
}




peer_connection  �˶Զ�������Ϣ��

node_delegate  reports status to client or fetch data from client
message ��Ϣ��


�������ֲ�ʱ��ξ���
//�ᵽ�ֲ�
application_impl::handle_block
{
}



reset_websocket_server 


Graphene Դ���Ķ� ~ ����֪ʶ
https://steemit.com/bitshares/@cifer/graphene

Graphene Դ���Ķ� ~ ���ݿ�ƪ ~ ����ģ��
https://steemit.com/bitshares/@cifer/4o1u9e-graphene

Graphene Դ���Ķ� ~ ���ݿ�ƪ ~ ������
https://steemit.com/bitshares/@cifer/5b42mk-graphene

Graphene Դ���Ķ� ~ ���ݿ�ƪ ~ �������л�
https://steemit.com/bitshares/@cifer/vdnvb-graphene

Graphene Դ���Ķ� ~ ���ݿ�ƪ ~ ����������
https://steemit.com/bitshares/@cifer/7hnz25-graphene

Graphene Դ���Ķ� ~ RPC ƪ ~ ͨ��Э��������ʵ��
https://steemit.com/bitshares/@cifer/graphene-rpc

Graphene Դ���Ķ� - RPC ƪ - API ע�����
https://steemit.com/bitshares/@cifer/graphene-rpc-api


Graphene Դ���Ķ� ~ ���ݿ�ƪ ~ �������ݿ�
https://steemit.com/bitshares/@cifer/2uwh3m-graphene

Graphene Դ���Ķ� ~ ����ƪ ~ �����ж��߼�
https://steemit.com/bitshares/@cifer/69trzr-graphene

Graphene Դ���Ķ� - ����ƪ - ���׷���
https://steemit.com/bitshares/@cifer/6ai6f6-graphene

Graphene Դ���Ķ� - �ܹ�ƪ - �ڵ�ʵ��������
https://steemit.com/bitshares/@cifer/graphene---0w1jzg9j8v

���ع�ȫ�ڵ�witness_node��������
https://www.jianshu.com/p/9a58ad875cc3

http://docs.bitshares.org/integration/index.html#basic-knowledge
