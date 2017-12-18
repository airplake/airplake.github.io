---
title: git上的配置文件如何加密？
date: 2017-12-18 10:45:38
categories: 工具
author: kelvv
tags:
  - git
  - git-crypt
---





在开发过程中必须会遇到一个问题：

>带敏感信息的配置文件是否应该提交上git代码管理?

常用的解决方法如下：
1. 明文上传到git，方便团队成员合作开发，共同维护配置文件。
2. 不传到git，本地保存，团队内成员需要使用配置文件再发给对方。


<!-- more -->

那么今天将为大家介绍另一种解决方法： 
##### 加密上传git

### 一： 有何特性？
1. 加密后上传git，在git上保存的是二进制文件。
2. 分发密钥给可信开发人员，进行解密，维护配置文件。
3. 解密后为明文内容，如需上传，不用再进行加密，工具自动会生成最新的二进制文件后上传。

### 二： 简单原理分析
该工具名字为： git-crypt ， 由c++编写，可以通过brew等方式下载，它利用了加密工具gpg进行加密处理，并且配合git hook,达到每次git提交都会进行自动加密的效果。

### 三： 使用
##### 1. 安装git-crypt ，gpg
```
brew install git-crypt
brew install gpg
```

##### 2. 配置加密工具gpg
```
# gpg --gen-key // 生成密钥（公钥和私钥），按照流程提示进行
# gpg --list-keys // 会列出当前所有的密钥，检查刚才的密钥是否生成成功 
       
       /Users/jarvin/.gnupg/pubring.kbx
       --------------------------------
       pub   rsa2048 2017-11-29 [SC] [有效至：2019-11-29]
           6B0240D7DFC44C90822F5C4191A6E15AB309D2F5
       uid           [ 绝对 ] kelvv <kelvv@outlook.com>
       sub   rsa2048 2017-11-29 [E] [有效至：2019-11-29]

```

##### 3. 配置git-crypt
```
# cd path/to/project
# git-crypt init   // 类似于git init，安装git-crypt到项目中
# git-crypt add-gpg-user kelvv    // 添加密钥用户，这里以我的用户kelvv为例
```

##### 4. 添加配置文件.gitattributes
```
# vi .gitattributes
```
格式为： * filter=git-crypt diff=git-crypt ， 例如我要加密config文件夹的三个配置文件 , 则在.gitattributes文件内加入：
```
config/production.json filter=git-crypt diff=git-crypt
config/development.json filter=git-crypt diff=git-crypt
config/default.json filter=git-crypt diff=git-crypt
```

##### 5. 上传到git
```
# git rm -r --cached config/     //清理config的git缓存
# git add .
# git commit -m 'git-crypt'
# git push
```

##### 6. 导出密钥
```
# git-crypt export-key ~/Desktop/git-crypt-key   
```

导出了密钥以后，就可以分发给有需要的团队内部人员。

当团队其他成员获取了代码以后，需要修改配置文件，需要先解密，解密动作只需要做一次，往后就不需要再进行解密了。

##### 7. 解密
```
# git-crypt unlock /path/to/git-crypt-key
```

### 四： 总结
利用该方式进行配置文件管理可以保证安全性，只有团队内相关人员才能看到配置文件明文内容，解密只需要第一次进行，之后就没什么改变，直接改配置文件，git提交会自动加密。