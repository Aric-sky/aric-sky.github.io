layout: post
title: 管理多个git生成的ssh key
comment: true
tags: [git,前端]
date: 2018-11-22 14:17:18
updated: 2018-11-22 14:17:18
---

------
<!-- more -->

> 经常我们可能需要上传github，和gitlab，或者你有多个github账号，我们需要对应不同的账号上传，我们需要配置多个ssh key
这里我们就以配置github，gitlab，两个ssh key 为案例

## 1.生成两个不同的ssh
生成第一个ssh key

```bash
ssh-keygen -t rsa -C "yourmail@gmail.com" 
```
- 这里不要一路回传，让你选择在哪里选择存放key的时候写个名字，比如 id_rsa_github，之后的两个可以回车。

![shhkey1](http://cdn.wangyuanqi.xyz/sshkey1.png)
- 上图的红色框框是自己输入的

## 生成第二个ssh key

```bash
ssh-keygen -t rsa -C "yourmail@gmail.com"
```
一样不要一路回车

![shhkey2](http://cdn.wangyuanqi.xyz/shhkey2.png)

最终结果是这样子的：
![shhkey3](http://cdn.wangyuanqi.xyz/sshkey3.png)
图中的config文件是我自己建的，也就是接下来要说的

## 2.配置config
新建文件config文件，打开输入一下
```bash
# gitlab
Host gitlab.com
    HostName gitlab.com  
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_gitlab
    User xiaqijian // 输入自己账号名
    
# github
Host github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_github
    User xiaqijian  // 这里输入自己的账号名

```
注意：如果拷贝我的，要把后面的注释去掉然后保存起来

## 3.分别在github，gitlab填上ssh key
- 方法一：
> 手动复制从ssh-rsa 开始，以your_email@example.com结束，然后粘贴到你登录的github账号上面Settings -->SSH keys -->Add SSH key 保存即可 Title 可以随便写，Key粘贴刚复制内容，这样SSH key 就添加到你的Github上了。

```
直接 终端输入：vim ~/.ssh/id_rsa.pub
```

- 方法二：(在终端输入命令)
```
pbcopy < ~.ssh/id_rsa.pub
```
> 然后粘贴到你登录的github账号上面Settings -->SSH keys -->Add SSH key 保存即可 Title 可以随便写，Key粘贴刚复制内容，这样SSH key 就添加到你的Github上了。

![shhkey4](http://cdn.wangyuanqi.xyz/sshkey4.png)

## 4.测试
填上刚刚生成的，然后你就可以上传文件试试或者用下面方法测试

```bash
ssh -T git@github.com
```
![shhkey5](http://cdn.wangyuanqi.xyz/sshkey5.png)

