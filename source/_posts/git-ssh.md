---
title: git配置ssh登录
index_img: /blog/img/git.jpg
date: 2020-03-12 10:30:52
categories: 
- git
tags:
- git
- ssh
---

## 1. 生成ssh密钥

```bash
$  ssh-keygen -o -t rsa -b 4096 -C "email@example.com"
```

## 2. 复制公钥

```bash
$ cat ~/.ssh/id_rsa.pub
```

## 3. gitlab或github管理页面添加ssh-key
## 4. 配置本机使用的私钥 - 创建并编辑`~/.ssh/config`

```bash
# 注意如果git私有仓库配置了其他ssh端口，下面需配置PORT
Host gitlab.example.com         // git仓库域名
	User git                    // git
	Hostname gitlab.example.com // git仓库域名
	PORT 8022                   // 默认22，如果私有仓库使用其他端口需配置如：8022
	IdentityFile ~/.ssh/id_rsa  // 私钥路径
```

