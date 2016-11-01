# solr-master
基于solr的数据检索引擎,dubbo+zookeeper分布式


项目技术
1.采用solr5.2 实现网站数据搜索
  --  支持拼音分词
  --  支持动态词典扩展
  --  数据量大的情况下多线程全量索引，缩短全量索引建立消耗时间
  --  代码高度抽象实现，减少后期的代码量
2.采用dubbo 技术solr服务与数据库服务分离开来
  -- gysolr-product-provider 既是服务消费者 又是solr操作服务方
  -- 作为服务消费方主要是为了获取原始数据比如从数据库获取商品等数据
  -- 作为solr操作的服务方只要是提供索引变更，索引查询的服务，以供其他项目消费
3.项目都可在tomcat下运行
  -- 将solr自身独立出来，放在一个单独的容器中运行。这个容器运行起立你就可以把它当做一个数据库，
4.采用文件锁在一定程度上避免发出多条全量索引的指令
  -- 在遇到dubbo负载均衡的情况下，会将gysolr-product-provider部署多份，gysolr-product-provider一启动
     一般会全量更新或重建索引，这样可能会出现重复重建的弊端，所以需要一个机制来控制这种弊端的发生
	 我是采用的文件锁，如果更加好的方式，不妨给我邮件
5.采用spring定时器进行增量索引的更新
