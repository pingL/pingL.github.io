---
title: Ruby-on-Rails-7
date: 2017-07-03 00:17:14
tags: Rails
---

由于不断的深入，遇到的问题也越来越难以解决。现在遇到的一个问题是，由于命名引起的，用户路径名应该使用users的，但是我误写成user，导致后面新建用户信息的注册表单报错，LOG信息如下：

➜  sample_app git:(sign-up) ✗ rails test
Running via Spring preloader in process 18618
Run options: --seed 28778
---
 # Running:

....E

Error:
UserControllerTest#test_should_get_new:
  ActionView::Template::Error: undefined method `users_path' for #<#<Class:0x007fc8721ba2c8>:0x007fc872179bd8>
  Did you mean?  user_path
  app/views/user/new.html.erb:5:in _app_views_user_new_html_erb___12467323558183319_70249442303760'
  test/controllers/user_controller_test.rb:5:in `block in <class:UserControllerTest>'


bin/rails test test/controllers/user_controller_test.rb:4

..........

Finished in 0.443527s, 33.8198 runs/s, 51.8571 assertions/s.
15 runs, 23 assertions, 0 failures, 1 errors, 0 skips

---
查看教程中估计和各个有关系：
7.3 节会介绍，Rails 会以这些 name 属性的值为键，用户输入的内容为值，构成一个名为 params 的散列，用来创建用户。

另外一个重要的标签是 form 自身。Rails 使用 @user 对象创建这个 form 元素，因为每个 Ruby 对象都知道它所属的类（4.4.1 节），所以 Rails 知道 @user 所属的类是 User，而且，@user 是一个新用户，因此 Rails 知道要使用 post 方法——这正是创建新对象所需的 HTTP 请求（参见旁注 3.2）：

<form action="/users" class="new_user" id="new_user" method="post">
这里的 class 和 id 属性并不重要，重要的是 action="/users" 和 method="post"。设定这两个属性后，Rails 会向 /users 发送 POST 请求。接下来的两节会介绍这个请求的效果。

### 涉及的目录及文件名

+ routes.rb
+ new.html.erb
+ user.controller.rb
+ custom.scss


### 下一步尝试解决办法

目前能想到的就是，更改用户测试目录及用户功能目录。使系统或者是数据库对特殊路径名做出正确判断。
