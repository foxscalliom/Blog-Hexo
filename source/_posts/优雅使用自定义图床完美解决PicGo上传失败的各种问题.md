---
title: 自定义图床解决PicGo上传失败
slug: picgo
abbrlink: 8588bfe2
date: 2021-09-01 12:42:48
---
经常碰到使用PicGo上传失败等问题，今天教程来了，使用自定义图床完美解决上传失败问题

## 准备
### 番茄
前提必须要梯子，实在不会，可以看我这篇，不需要梯子，也是白嫖，[彩虹云白嫖图床教程](https://www.foxscallion.top/posts/b3352ee7.html)
我准备了一个手机的，小编目前没有电脑的，不过手机勉强够用
 这个是`GitHub仓库`的下载地址[Calyx](https://github.com/foxscallion11/CalyxVPN.git)
 考虑到GitHub下载太慢，我还上传了一个`Gitee仓库`的下载地址[Calyx](https://gitee.com/dada69/CalyxVPN.git)
### 国外邮箱
一个国外邮箱，比如google或者outlook（建议谷歌邮箱）
<!--more-->
## 创建虚拟主机
### 注册000webhost
中转服务器需要国外服务器,只能使用国外的服务器，这里有个免费的国外服务器
注册[000webhost](https://www.000webhost.com/)这个是国外的地址
当然还有个免费的国内地址[彩虹云](https://www.cccyun.net/)，移步看我这篇[彩虹云白嫖图床教程](https://www.foxscallion.top/posts/b3352ee7.html)
我这里主要介绍国外的服务器，彩虹云可以自己去尝试
使用第三个谷歌邮箱进行导入
![](https://cdn.jsdelivr.net/gh/foxscallion11/sc1@master/2020/08/30/8465e830c590bc53557f471fd693ac56.png)
### 开始创建
选择第一个免费
![](https://cdn.jsdelivr.net/gh/foxscallion11/sc1@master/2020/08/30/5e7cf8a4161399c91fa1e07f476925e9.png)
选择other
![](https://cdn.jsdelivr.net/gh/foxscallion11/sc1@master/2020/08/30/718fe22585a1e76c81f40e36c8dff4e0.png)
接下来填入网站名称和密码
![](https://cdn.jsdelivr.net/gh/foxscallion11/sc1@master/2020/08/30/88bf179ce0c4ff69e2bfdf7d0d4f1ac5.png)
选择第三个上传你的网站
![](https://cdn.jsdelivr.net/gh/foxscallion11/sc1@master/2020/08/30/c9132ca4ab1af7abae3ea5b26d636ee1.png)
### 登入管理页面
先不要操作
![](https://cdn.jsdelivr.net/gh/foxscallion11/sc1@master/2020/08/30/4ca6e781bd8bac391b2fa5081d41aeb8.png)
## 配置环境
项目地址在这里：双击进去下载
[autoPicCdn](https://github.com/foxscallion11/autoPicCdn.git)
然后修改up.php文件
按照提示修改信息
```
define("REPO","仓库");//必须是下面用户名下的公开仓库
define("USER","github仓库名");//必须是当前GitHub用户名
define("MAIL","yumusb@foxmail.com");//
define("TOKEN","token");//https://github.com/settings/tokens 去这个页面生成一个有写权限的token（write:packages前打勾）
```
然后保存
### 上传up.php文件
回到刚才登入界面
![](https://cdn.jsdelivr.net/gh/foxscallion11/sc1@master/2020/08/30/4ca6e781bd8bac391b2fa5081d41aeb8.png)
进入public_html
上传文件
![](https://cdn.jsdelivr.net/gh/foxscallion11/sc1@master/2020/08/30/080ed058abcf77633d29b791b8bcc713.png)
当然index.html也可以上传
访问下面的地址：
[https://www.000webhost.com/members/website/list](https://www.000webhost.com/members/website/list)
然后复制访问地址
![](https://cdn.jsdelivr.net/gh/foxscallion11/sc1@master/2020/08/30/ecedb1d16175d2b8a301cdd0eb2a1ed8.png)
## 比如我的地址
### 图片上传地址
[https://foxscallion.000webhostapp.com/index.html](https://foxscallion.000webhostapp.com/index.html)
![](https://cdn.jsdelivr.net/gh/foxscallion11/sc1@master/2020/08/30/dc49f6f30ea9914fa049d8398c8b1d65.png)
### 图片上传up.php文件
[https://foxscallion.000webhostapp.com/up.php](https://foxscallion.000webhostapp.com/up.php)
## 打开PicGo
选择插件设置
搜索web-uploader插件
![](https://cdn.jsdelivr.net/gh/foxscallion11/sc1@master/2020/09/01/90a444fbb8fcf346ce8f7395c2231721.png)
选择最后一个配置
![](https://cdn.jsdelivr.net/gh/foxscallion11/sc1@master/2020/09/01/28450c82c773b4dc76a7d8b887bfb3cf.png)
配置web-uploader
![](https://cdn.jsdelivr.net/gh/foxscallion11/sc1@master/2020/09/01/3621a06f1317cab050be650ef51ffd9d.png)
选择自定义web图床
![](https://cdn.jsdelivr.net/gh/foxscallion11/sc1@master/2020/09/01/ab7374d4fc3dfeb541037c043bde0ded.png)
## 完成
测试
![](https://cdn.jsdelivr.net/gh/foxscallion11/sc1@master/2020/09/01/5c11e7abce25e8c12088c61eaa044bc1.png)

## 温馨提示

{% blockquote  %}
本文写作不易，给个评论支持一下噢，蟹蟹啦
我的博客即将同步至腾讯云+社区，邀请大家一同入驻：https://cloud.tencent.com/developer/support-plan?invite_code=13pvtu60b99k7
{% endblockquote %}