---
layout:     post
title:      "如何在同一台电脑上连接不通的github账号"
subtitle:   "多个github账号怎么办？"
date:       2016-07-20
author:     "p0pfan"
header-img: "img/posts/2016-07-20-how-to-connect-two-github-accounts-in-one-computer.jpg"
catalog:    true
tags:
    - github
---

当有多个github账号的时候，我该怎么办？

#### 1.需求来源
在公司实习的时候，我想趁着空闲的时间弄一下自己的项目。所以就需要让公司的电脑可以通过ssh来连接自己的github。

#### 2.问题
由于公司的电脑上已经通过`ssh-keygen -t rsa -C 'email'`设置了与公司的电脑的连接。那么问题就来了，**如何在同一台电脑上连接不通的github账号？**

#### 3. 解决办法
1. 得到两个github账号的SSH key
    - 指令：`ssh-keygen -t rsa -C "your-email-address"`
        - NOTE:上面指令执行后，会出现一个提示：`Enter file in which to save the key (路径.ssh/id_rsa): ***` 。此时你可以输入一个名字来命名你存储的key的名称（*** 星号处输入）。如：`work` 或者`personal`
        - 当然，你需要将相应的公钥放置到对应的github下面
2. 使用`ssh-add -l`查看是否将私钥添加到了设置当中。
    - 如果没有，则使用`ssh-add xxx`来添加私钥。其中`xxx`表示私钥的名称。

3. 在`.ssh`文件下添加一个`config`文件。其中内容为：
```shell
Host NAME1
    HostName 对应的域名(eg:github.com)
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/私钥的名称

Host NAME2
    HostName 对应的域名(eg:github.com)
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/私钥的名称

```
- **NOTE**
    - config的文件配置需要注意的是NAME1和NAME2；
    - NAME1和NAME2配置的不和对应的`HOSTNAME`对应的话，
        - 比如说`NAME1`是`work`;`HostName`是`github.com`,那么在clone的时，如果使用`git clone git@github.com:xxx`时可能会出现没有权限克隆的问题。但是使用`git clone git@work:xxx`则可以正常。
        （**这个没准是个别情况，我没有做过多的研究，刚好使用的时候碰到了，就说明一下。**）

#### 4. 测试连接情况
- 指令：`ssh -T git@xxx` 或者`ssh -T v git@xxx` (其中xxx表示config中的NAME1或者NAME2,其实你们也可以测试一下xxx写HostName的情况，按理说应该也是可以的。但是也有可能出现我上面note说明的情况)
- 成功的标志是：`Hi ***! You've successfully authenticated, but GitHub does not provide shell access.
`

###### 参考连接：
https://help.github.com/articles/generating-an-ssh-key/
http://www.cnblogs.com/foxracle/archive/2012/07/20/2599830.html
http://xuyuan923.github.io/2014/11/04/github-gitlab-ssh/


###### 图片地址：
http://i.blogs.es/ea935a/yeswecode/450_1000.jpg
