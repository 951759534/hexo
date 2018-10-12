---
title: Hibernate(version4.3基于xml配置)总结
date: 2017-06-14 13:34:00
tags: java ORM框架
---
  又到了学期的末尾,马上要去实习 虽然还没有找到实习的地方 但是我相信 机会肯定会留给我的,顺便总结一波Hibernate基于4.3版本基于xml的总结干货  
虽然我推荐使用Hibernate5.0即以上注解版本 没有xml的繁琐配置 送上一波Hibernate5.0的[用户手册](http://docs.jboss.org/hibernate/orm/5.0/userguide/html_single/Hibernate_User_Guide.html)和[api](http://docs.jboss.org/hibernate/orm/5.0/javadocs/)  
>这次总结仅限学习 项目还是推荐使用5.0基于注解的版本 配置简单 使用方便 开箱即用  


接下来开始讲解4.3的基于xml配置和xml映射文件的使用练习   
1. 从官网下载4.3的包 笔者用的的是Intellij的IDEA编译器 没用maven或者gradle包管理工具  只是纯属用于学习    
2. 在IDEA建立一个普通的JAVA工程导入下载的lib中的required的所有jar包 接下就开始开始配置    
3. 先送上一波官网关于4.3的[用户手册](http://docs.jboss.org/hibernate/orm/4.3/devguide/en-US/html_single/)  
4. 接下来在src中建立xml配置文件 建议命名为 hibernate.cfg.xml不过 在model层的映射文件推荐 类名.hbm.xml  
5. 配置文件怎么写  
>             <!DOCTYPE hibernate-configuration PUBLIC
			        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
			        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
						<hibernate-configuration>
						    <session-factory>
						        <property name="show_sql">true</property>
						        <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
						        <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
						        <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/Hibernate</property>
						        <property name="hibernate.connection.username">root</property>
						        <property name="hibernate.connection.password">root</property>
						        <!-- 当前线程只对应一个Session实例 -->
						        <property name="hibernate.current_session_context_class">thread</property>
								<!--四种属性 create update validate  create-drop-->
						        <property name="hbm2ddl.auto">update</property>
					        <mapping resource="Entity/user.xml"/>
					    </session-factory>
					</hibernate-configuration>
				

			上方四种属性的说明
				create：
				每次加载hibernate时都会删除上一次的生成的表，然后根据你的model类再重新来生成新表，哪怕两次没有任何改变也要这样执行，这就是导致数据库表数据丢失的一个重要原因。
	
				create-drop ：
				每次加载hibernate时根据model类生成表，但是sessionFactory一关闭,表就自动删除。

				update(推荐)：
				最常用的属性，第一次加载hibernate时根据model类会自动建立起表的结构（前提是先建立好数据库），以后加载hibernate时根据 model类自动更新表结构，即使表结构改变了但表中的行仍然存在不会删除以前的行。要注意的是当部署到服务器后，表结构是不会被马上建立起来的，是要等 应用第一次运行起来后才会。

				validate ：
				每次加载hibernate时，验证创建数据库表结构，只会和数据库中的表进行比较，不会创建新表，但是会插入新值。