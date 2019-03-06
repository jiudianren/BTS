BTS的DPoS共识之代码实现概述
https://blog.csdn.net/ggq89/article/details/80068306
 总共有N个证人（witness）签署了这些区块

delegated proof of stake（代表 证明 权益 赌注）委托权益证明

[区块链]DPoS（委托权益证明机制）官方共识机制详解——BTS、EOS
https://blog.csdn.net/lsttoy/article/details/80041033

 
 股份授权证明(DPOS)概述
 https://blog.csdn.net/ggq89/article/details/80188930
 
 
# BitShares的DPoS共识
## 1. 共识机制概述  
任何共识机制都必须回答包括但不限于如下的问题：  

下一个添加到数据库的新区块应该由谁来生成？  
下一个块应该何时产生？  
该区块应包含哪些交易？  
怎样对本协议进行修改？  
该如何解决交易历史的竞争问题？  
共识机制的目标是找到这些问题的答案，并确保其健壮性以抵制攻击者试图获得网络的控制权。   
实际上，获得控制就意味着获得了单方面审查交易的能力。
共识机制也应当能健壮地抵御攻击者利用在不同计算机上的数据库状态中的临时不一致性获取好处。



## 7. 竞争链问题（Blockchain Reorganizations)
关于该问题的详细讨论可参考: DPoS共识算法 - 缺失的白皮书

由于所有证人都是选举产生的，高度负责任的，并且授予了专门的时间段来生产区块，所以几乎不可能存在两条竞争链的情况。    
网络延迟偶尔会使得某个证人不能及时收到上一个区块。 
如果发生这种情况，下一个证人将通过建立在他们首先收到的区块之上来解决这种问题。  
 凭借99％的证人参与率，每一笔交易有99％的机会在单个证人之后得到验证。

尽管该系统能健壮地抵御自然的链重组事件，但仍然存在如下的一些可能性：
软件错误、网络中断和不称职或恶意的证人创造多于一个到两个区块的多个竞争历史。 
软件始终选择证人参与率最高的那一条区块链。 
证人控制自己在每轮中只能生产一个区块，其参与率总是比大多数人低。 
没有任何单个证人（或少数几个证人）能够做到更高参与率的生产区块链。
 参与率通过将预期生产的区块数量和实际生产的区块数量相比较来计算的。



DPOS共识算法 - 缺失的白皮书
https://blog.csdn.net/ggq89/article/details/80072876

为了帮助解释这个算法，我只假设3个区块生产者A，B和C。
因为共识（的达成）需要2?3+1（个节点）来表决所有情况，所以这个简化模型将假设生产者C是打破僵局的那个节点。
在现实世界中，将有21个或更多的区块生产者。
像工作量证明（PoW）一样，一般规则是最长链胜出。
任何时候当一个诚实的对等节点看到一个有效的更长链，它都会从当前分叉切换到更长的这条链

5. 网络分片化(Fragmentation)


具体代码实现
https://blog.csdn.net/ggq89/article/details/80068306

洗牌算法

class witness_schedule_object ；

	database::push_block
	{
	    database::_push_block
	    {
	    	apply_block(new_block, skip)
	    	{
	    		   // Are we at the maintenance interval?
	   			if( maint_needed )
	      				perform_chain_maintenance(next_block, global_props);
	      	    update_witness_schedule();//洗牌
	    	}
	    }
	}



	void database::update_witness_schedule()
	{
	   const witness_schedule_object& wso = witness_schedule_id_type()(*this);
	   const global_property_object& gpo = get_global_properties();
	
	   // 该条件控制了洗牌算法被调用的时机，必须是证人完整的轮转完一圈后
	   if( head_block_num() % gpo.active_witnesses.size() == 0 )
	   {
	      modify( wso, [&]( witness_schedule_object& _wso )
	      {
	         // 清空证人集合
	         _wso.current_shuffled_witnesses.clear();
	         _wso.current_shuffled_witnesses.reserve( gpo.active_witnesses.size() );
	
	         // 初始化证人集合数据
	         for( const witness_id_type& w : gpo.active_witnesses )
	            _wso.current_shuffled_witnesses.push_back( w );
	
	         // 打乱证人的调度顺序
	         auto now_hi = uint64_t(head_block_time().sec_since_epoch()) << 32;
	         for( uint32_t i = 0; i < _wso.current_shuffled_witnesses.size(); ++i )
	         {
	            /// 此处使用了随机函数，具体原理请参看：http://xorshift.di.unimi.it/
	            uint64_t k = now_hi + uint64_t(i)*2685821657736338717ULL;
	            k ^= (k >> 12);
	            k ^= (k << 25);
	            k ^= (k >> 27);
	            k *= 2685821657736338717ULL;
	
	            uint32_t jmax = _wso.current_shuffled_witnesses.size() - i;
	            uint32_t j = i + k%jmax;
	            // 进行N次随机交换
	            std::swap( _wso.current_shuffled_witnesses[i],
	                       _wso.current_shuffled_witnesses[j] );
	         }
	      });
	   }
	}


 3. 统计投票和选出证人集合
 
	database::perform_chain_maintenance
	{
	   const auto& gpo = get_global_properties();
	   distribute_fba_balances(*this);
	   create_buyback_orders(*this);
	
	   perform_account_maintenance(std::tie(
	      tally_helper,
	      fee_helper
	      ));
	
	   struct clear_canary {
	      clear_canary(vector<uint64_t>& target): target(target){}
	      ~clear_canary() { target.clear(); }
	   private:
	      vector<uint64_t>& target;
	   };
	   clear_canary a(_witness_count_histogram_buffer),
	                b(_committee_count_histogram_buffer),
	                c(_vote_tally_buffer);
	
	   update_top_n_authorities(*this);
	   update_active_witnesses();
	   update_active_committee_members();
	   update_worker_votes();
	
	   modify(gpo, [this](global_property_object& p) {
	      // Remove scaling of account registration fee
	      const auto& dgpo = get_dynamic_global_properties();
	      p.parameters.current_fees->get<account_create_operation>().basic_fee >>= p.parameters.account_fee_scale_bitshifts *
	            (dgpo.accounts_registered_this_interval / p.parameters.accounts_per_fee_scale);
	
	      if( p.pending_parameters )
	      {
	         p.parameters = std::move(*p.pending_parameters);
	         p.pending_parameters.reset();
	      }
	   });
	
	   auto next_maintenance_time = get<dynamic_global_property_object>(dynamic_global_property_id_type()).next_maintenance_time;
	   auto maintenance_interval = gpo.parameters.maintenance_interval;
	
	   if( next_maintenance_time <= next_block.timestamp )
	   {
	      if( next_block.block_num() == 1 )
	         next_maintenance_time = time_point_sec() +
	               (((next_block.timestamp.sec_since_epoch() / maintenance_interval) + 1) * maintenance_interval);
	      else
	      {
	         auto y = (head_block_time() - next_maintenance_time).to_seconds() / maintenance_interval;
	         next_maintenance_time += (y+1) * maintenance_interval;
	      }
	   }
	
	   const dynamic_global_property_object& dgpo = get_dynamic_global_properties();
	
	   if( (dgpo.next_maintenance_time < HARDFORK_613_TIME) && (next_maintenance_time >= HARDFORK_613_TIME) )
	      deprecate_annual_members(*this);
	
	   modify(dgpo, [next_maintenance_time](dynamic_global_property_object& d) {
	      d.next_maintenance_time = next_maintenance_time;
	      d.accounts_registered_this_interval = 0;
	   });
	
	   // Reset all BitAsset force settlement volumes to zero
	   for( const auto& d : get_index_type<asset_bitasset_data_index>().indices() )
	   {
	      modify( d, [](asset_bitasset_data_object& o) { o.force_settled_volume = 0; });
	      if( d.has_settlement() )
	         process_bids(d);
	   }
	
	   // process_budget needs to run at the bottom because
	   //   it needs to know the next_maintenance_time
	   process_budget();
	
	} 



 