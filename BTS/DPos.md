BTS��DPoS��ʶ֮����ʵ�ָ���
https://blog.csdn.net/ggq89/article/details/80068306
 �ܹ���N��֤�ˣ�witness��ǩ������Щ����

delegated proof of stake������ ֤�� Ȩ�� ��ע��ί��Ȩ��֤��

[������]DPoS��ί��Ȩ��֤�����ƣ��ٷ���ʶ������⡪��BTS��EOS
https://blog.csdn.net/lsttoy/article/details/80041033

 
 �ɷ���Ȩ֤��(DPOS)����
 https://blog.csdn.net/ggq89/article/details/80188930
 
 
# BitShares��DPoS��ʶ
## 1. ��ʶ���Ƹ���  
�κι�ʶ���ƶ�����ش���������������µ����⣺  

��һ����ӵ����ݿ��������Ӧ����˭�����ɣ�  
��һ����Ӧ�ú�ʱ������  
������Ӧ������Щ���ף�  
�����Ա�Э������޸ģ�  
����ν��������ʷ�ľ������⣿  
��ʶ���Ƶ�Ŀ�����ҵ���Щ����Ĵ𰸣���ȷ���佡׳���Ե��ƹ�������ͼ�������Ŀ���Ȩ��   
ʵ���ϣ���ÿ��ƾ���ζ�Ż���˵�������齻�׵�������
��ʶ����ҲӦ���ܽ�׳�ص��������������ڲ�ͬ������ϵ����ݿ�״̬�е���ʱ��һ���Ի�ȡ�ô���



## 7. ���������⣨Blockchain Reorganizations)
���ڸ��������ϸ���ۿɲο�: DPoS��ʶ�㷨 - ȱʧ�İ�Ƥ��

��������֤�˶���ѡ�ٲ����ģ��߶ȸ����εģ�����������ר�ŵ�ʱ������������飬���Լ��������ܴ��������������������    
�����ӳ�ż����ʹ��ĳ��֤�˲��ܼ�ʱ�յ���һ�����顣 
������������������һ��֤�˽�ͨ�����������������յ�������֮��������������⡣  
 ƾ��99����֤�˲����ʣ�ÿһ�ʽ�����99���Ļ����ڵ���֤��֮��õ���֤��

���ܸ�ϵͳ�ܽ�׳�ص�����Ȼ���������¼�������Ȼ�������µ�һЩ�����ԣ�
������������жϺͲ���ְ������֤�˴������һ������������Ķ��������ʷ�� 
���ʼ��ѡ��֤�˲�������ߵ���һ���������� 
֤�˿����Լ���ÿ����ֻ������һ�����飬����������Ǳȴ�����˵͡� 
û���κε���֤�ˣ�����������֤�ˣ��ܹ��������߲����ʵ�������������
 ������ͨ����Ԥ������������������ʵ������������������Ƚ�������ġ�



DPOS��ʶ�㷨 - ȱʧ�İ�Ƥ��
https://blog.csdn.net/ggq89/article/details/80072876

Ϊ�˰�����������㷨����ֻ����3������������A��B��C��
��Ϊ��ʶ���Ĵ�ɣ���Ҫ2?3+1�����ڵ㣩�����������������������ģ�ͽ�����������C�Ǵ��ƽ��ֵ��Ǹ��ڵ㡣
����ʵ�����У�����21�����������������ߡ�
������֤����PoW��һ����һ����������ʤ����
�κ�ʱ��һ����ʵ�ĶԵȽڵ㿴��һ����Ч�ĸ�������������ӵ�ǰ�ֲ��л���������������

5. �����Ƭ��(Fragmentation)


�������ʵ��
https://blog.csdn.net/ggq89/article/details/80068306

ϴ���㷨

class witness_schedule_object ��

	database::push_block
	{
	    database::_push_block
	    {
	    	apply_block(new_block, skip)
	    	{
	    		   // Are we at the maintenance interval?
	   			if( maint_needed )
	      				perform_chain_maintenance(next_block, global_props);
	      	    update_witness_schedule();//ϴ��
	    	}
	    }
	}



	void database::update_witness_schedule()
	{
	   const witness_schedule_object& wso = witness_schedule_id_type()(*this);
	   const global_property_object& gpo = get_global_properties();
	
	   // ������������ϴ���㷨�����õ�ʱ����������֤����������ת��һȦ��
	   if( head_block_num() % gpo.active_witnesses.size() == 0 )
	   {
	      modify( wso, [&]( witness_schedule_object& _wso )
	      {
	         // ���֤�˼���
	         _wso.current_shuffled_witnesses.clear();
	         _wso.current_shuffled_witnesses.reserve( gpo.active_witnesses.size() );
	
	         // ��ʼ��֤�˼�������
	         for( const witness_id_type& w : gpo.active_witnesses )
	            _wso.current_shuffled_witnesses.push_back( w );
	
	         // ����֤�˵ĵ���˳��
	         auto now_hi = uint64_t(head_block_time().sec_since_epoch()) << 32;
	         for( uint32_t i = 0; i < _wso.current_shuffled_witnesses.size(); ++i )
	         {
	            /// �˴�ʹ�����������������ԭ����ο���http://xorshift.di.unimi.it/
	            uint64_t k = now_hi + uint64_t(i)*2685821657736338717ULL;
	            k ^= (k >> 12);
	            k ^= (k << 25);
	            k ^= (k >> 27);
	            k *= 2685821657736338717ULL;
	
	            uint32_t jmax = _wso.current_shuffled_witnesses.size() - i;
	            uint32_t j = i + k%jmax;
	            // ����N���������
	            std::swap( _wso.current_shuffled_witnesses[i],
	                       _wso.current_shuffled_witnesses[j] );
	         }
	      });
	   }
	}


 3. ͳ��ͶƱ��ѡ��֤�˼���
 
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



 