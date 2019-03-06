
	{
		node->register_plugin<witness_plugin::witness_plugin>();
		见证人功能是在 witness 插件里实现的	
		application::initialize
		{
			database::open
			{
				database::init_genesis
			}
		}
	}
