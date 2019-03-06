	 class application_impl : public net::node_delegate
	{
	      void reset_p2p_node(const fc::path& data_dir)
	      { try {
	         _p2p_network = std::make_shared<net::node>("BitShares Reference Implementation");
	
	         _p2p_network->load_configuration(data_dir / "p2p");
	         _p2p_network->set_node_delegate(this);
	      }
	}
