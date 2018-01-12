title: Servlet 一二点
date: 2017-09-27
tags: servelt tomcat IntelliJIdea
---

* Java文件中文字符乱码
	* a 编译器字符编码
	![截图一](https://ws4.sinaimg.cn/large/006tKfTcgy1fjy3ntf3p9j30gu06kdfy.jpg)
	* b servlet 解码
//响应的内容类型
response.setContentType("text/html;charset=GBK");
//设置解码方式
request.setCharacterEncoding("GBK");

* 数据库连接后查询失败
	* 报错提示有
		“The driver has not received any packets from the server.（驱动程序没有从服务器接收任何数据包）”
		![报错截图](https://ws3.sinaimg.cn/large/006tKfTcgy1fjy3shl7m7j30n003kq3t.jpg)
		
				 // 连接数据库
				public Connection getConnection () throws Exception{
        				if (connection == null) {
            					Class.forName(this.driver);
//            connection =  DriverManager.getConnection(url,username,this.password);
          		  connection =  DriverManager.getConnection(url, username, this.password);
       			 }
      			  return  connection;
  				  }
		
		//查询
					public ResultSet query (String sql, Object... args) throws Exception {
 							   PreparedStatement preparedStatement =  getConnection().prepareStatement(sql);
		
 				  		 for (int i = 0; i < args.length; i++) {
 			       		preparedStatement.setObject(i + 1, args[i]);
 				 		  }
				 			   return preparedStatement.executeQuery();
							}
	
				
					ResultSet resultSet = dbDao.query("select password * from user_table" + "where username =?", name);

		
* IntelliJ Idea编译器开放BUG
	* 使用测试Demo可以正常运行，问题消失，编译器正常。

用户提交姓名，性别等个人信息，通过action 提交用户数据到servlet进行处理，案例功能是显示用户提交的各个值，英文字符可以正常显示，但中文无法显示，查询相关资料显示，可能有两点原因造成。

问题层出不穷，又发现一个来自JB编译器的一个BUG，编译运行时，tomcat无法正常启动，manager has finished in 103 ms， 在Stack Overflow了解到这是JB的一个开放BUG，替换JB里一个叫tomcatIntegretion.jar 文件可以解决问题。找到了可以替换的文件时，操蛋的事接着发生，Finder文件无法拖动，放在平时可以进入终端命令来移动替换文件，可是要替换的文件在Application包文件里
 ![截图二](https://ws2.sinaimg.cn/large/006tKfTcgy1fjy3g0kafsj30w20bs40n.jpg)，🤣,人生多苦难，我的人生呀！（机器重启后恢复正常）


