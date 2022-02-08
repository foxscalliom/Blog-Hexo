---
slug: vercel
title: Hexo与Vercel自动部署
abbrlink: e287b8f6
date: 2020-07-22 01:57:00
---

### 前言
因为域名的备案申请还没有通过，想着能够通过免费的CDN来加速网站，同时又能够满足免费的域名，偶尔在别人博客网站上看到了
![image.png](https://upimage.alexhchu.com/2020/08/22/79568580a9a94.png)
于是我就通过百度发现了这个vercel是真的真的好用，全自动部署，只要你提交GitHub代码到vercel就可以了，几天前我就想注册个vercel，只是一直苦于无法注册，如图
<!--more-->
![image.png](https://upimage.alexhchu.com/2020/08/22/134a4b94b57c8.png)

于是我用谷歌空间注册了foxscallion@gmail.com账号，然后重新注册GitHub，通过这个样子，终于vercel成功登入了
## 通过邮箱重新注册个GitHub

- GitHub重新注册完，添加token密钥

![image.png](https://upimage.alexhchu.com/2020/08/22/10d1db5fec896.png)

- 将这些框框全部✔，然后确定

![image.png](https://upimage.alexhchu.com/2020/08/22/f231ed7f4ea27.png)

- 然后新建仓库（怎么建就不说了）

- 把密钥token添加到博客根目录_config.yml的repo
- 这里先复制密钥（密钥☞存在一次）先复制出来

<img src="https://upimage.alexhchu.com/2020/08/22/5760c4dbd9cb0.png" alt="image.png"  />

注意https://密钥@仓库地址

![image.png](https://upimage.alexhchu.com/2020/08/22/b87e5d50a59c2.png)

- 复制你的仓库git地址

![image.png](https://upimage.alexhchu.com/2020/08/22/0ee0afaafd797.png)

- 修改博客根目录的_config.yml，将你的地址换过来

![image.png](https://upimage.alexhchu.com/2020/08/22/e139f278d18d3.png)


![image.png](https://upimage.alexhchu.com/2020/08/22/6eeed180a55c0.png)

- 然后hexo clean ，hexo g ，hexo deploy，提交到你刚刚建的GitHub仓库

## 注册你的vercel

- 访问[Vercel](https://link.zhihu.com/?target=https%3A//vercel.com/)官网，根据提示注册账号。个人用户可以免费使用。

我的是用谷歌的邮箱注册GitHub，然后通过GitHub绑定vercel的

* 我的是用谷歌的邮箱注册GitHub，然后通过GitHub绑定vercel的

* ![image.png](https://upimage.alexhchu.com/2020/08/22/427251e0b6ad4.png)


### 

- 点击`Import Project` -> `Import Git Repository`，输入git仓库地址，vercel与Github基本上是无缝衔接，绑定过程非常轻松。 

* <img src="https://upimage.alexhchu.com/2020/08/22/e663a887019bb.png" alt="image.png" style="zoom:150%;" />

- ### 绑定完成后是项目配置。
- Vercel会自动识别开发框架，这里可以自定义编译代码和输出文件夹，你可以在编译代码中添加自己的逻辑；**使用内置框架一般不要修改输出文件夹**。
- 配置完成后点击`Deploy`，耐心等待即可
- 等待一会vercel就部署成功了

![image.png](https://upimage.alexhchu.com/2020/08/22/64b1e7f6734fd.png)

- 点击visit通过浏览器访问

![image.png](https://upimage.alexhchu.com/2020/08/22/8639112b9b872.png)

