---
title: Github基础学习
tags: 
   - Git
   - 学习
categories:
   - Git基础学习
keywords: "Git基础学习"
abbrlink: ""
date: 2023-08-27 10:57:09
decription:
top_img:
cover: https://s2.loli.net/2023/08/30/YdFOAtwaJNbSMxH.png
---

#### 1.	配置与使用

##### 1.1	配置github用户和邮箱

--->创建一个空的目录文件夹，右键打开Git Bash Here 

--->配置全局用户名：git 	config 	--global	user.name "自己的用户名"

###### 注意：这里的自己的用户名可以随便设

--->配置全局邮箱： git	config	--global	user.email	"自己邮箱"

--->生成密钥：ssh-keygen		-c 	rsa,然后一直回车，然后找到C盘下有个id_rsa.pub公钥文件。用记事本打开复制里面内容

--->配置本地与gitee的密钥和激活

###### 1.	登录gitee，然后点击右上方头像的设置

###### 2.	配置云端与本机密钥，找到安全设置SSH公钥-输入标题-粘贴公钥-输入密码验证

#### 2.	客户端验证

-->验证本机与gitee是否连接成功

输入：ssh	git@gitee.com 	-->yes,出现successfully表示本机与云端连接成功

#### 3.	文件提交和数据上传

1.	首先找到你的仓库url地址。点击code复制https地址或者ssh的地址。

--->在客户端添加一个远程仓库地址

​			2. git 	init		

-->git	remote	add	origin 	你的仓库url地址

--->本地缓存区添加文件：git 	add 	文件名

--->本地提交添加的文件：git	commit	-m 	"注解"

--->向云端的master分支进行推送：git	push	origin	master	-f

--->推送成功后，在云端仓库就可以看到上传的文件

--->查看暂存区中的文件列表：	git 	ls-files

--->查看本地版本库中的文件列表：	git	ls-files	--with-tree=HEAD

--->查看追踪状态：	git	status

--->版本回退：git 	reset 	HEAD

--->暂存区删除xxx文件：	git 	rm 	--cached	xxx

--->查看工作区中的文件列表：	ll

--->删除工作区和暂存区的文件：	git	rm	filename    (-r)递归删除文件

#### 注意：

如果在分支推送执行后，出现红色报错

--->打开控制面板--->用户账号--->凭据管理器--->Windows凭据--->找到git开头gitee.com的，输入你的正确用户名和密码，保存，然后提交，git	push 	origin	master	-f

###### 

#### 问题：

1.	使用git添加远程github仓库时，提示error: remote origin already exists.

-->(先删除远程Git仓库)git	remote 	rm	origin	-->（再添加远程Git仓库）git	remote	add	origin	仓库地址Url 

如果执行git 	remote	rm 	origin报错，手动修改gitconfig文件内容	-->vi 	.git/config	-->把[remote “origin”]那行删除----OK