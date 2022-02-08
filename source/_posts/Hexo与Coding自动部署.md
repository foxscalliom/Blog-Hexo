---
title: Hexo使用Coding自动部署
slug: coding
abbrlink: ce6df632
date: 2020-08-01 16:39:39
---

在 CODING 中您可以实现代码不落地完成整个项目的开发工作，完善的云端开发环境，让您打开浏览器即可开始编写代码。高可用代码仓库支持 Git/SVN 版本控制，可以进行分支粒度管理、建立代码评审机制

项目完成开发进入测试阶段，CODING 为您提供井然有序的测试协同工具，从编写测试用例、规划测试计划、自动化完成测试，到记录测试结果、产出测试报告并跟踪每一个缺陷，完美衔接开发、测试、产品之间的协同。
<!--more-->

- 持续集成、持续部署、制品库功能为项目的持续交付提供完整的工具服务，支持所有主流语言以及多种目标环境，和代码仓库紧密关联，产出构建可视化报告。释放研发人力，提高生产效率。

- Coding 是一个面向开发者的云端开发平台 [1]  ，目前提供代码托管，运行空间，质量控制，项目管理等功能。此外，还提供社会化协作功能，包含了社交元素，方便开发者进行技术讨论和协作
### coding注册
- 链接<a>https://coding.net/</a>
### coding使用
- 使用你刚刚注册的账号登入
- 创建代码仓库
- ![image.png](https://upimage.alexhchu.com/2020/08/22/260c2ba03b98b.png)
- 创建项目，选择代码托管
- ![image.png](https://upimage.alexhchu.com/2020/08/22/fd5aa189d08fe.png)
- ![image.png](https://upimage.alexhchu.com/2020/08/22/94f0c6ccd3b4e.png)
- 加入项目，点击左下角，项目设置-》项目成员-》功能开关，把持续部署，持续集成打开
- ![image.png](https://upimage.alexhchu.com/2020/08/22/f932cf776ff48.png)
- 点击持续集成-》构建计划
- ![image.png](https://upimage.alexhchu.com/2020/08/22/d2eae3801c60f.png)
- 在最下面选择自定义模板
- ![image.png](https://upimage.alexhchu.com/2020/08/22/164012050e8ea.png)
- 名称自己随便取一个，我的叫test
- ![image.png](https://upimage.alexhchu.com/2020/08/22/affce2d7b5857.png)
- 点击流程配置-》文本编辑-保存
```
 pipeline {
  agent any
  stages {
    stage('克隆项目') {
      steps {
        sh 'git clone https://Github用户名:密码@github.com/用户名/仓库.git .'
        sh 'ls -a'
      }
    }
    stage('安装依赖') {
      steps {
        sh 'ls -a'
        sh 'npm install -g hexo-cli'
        sh 'npm install hexo --save'
      }
    }
    stage('构建发布') {
      steps {
        sh 'hexo clean && hexo g && hexo d'
      }
    }
  }
}
```
- 复制coding里的hexo仓库地址
- ![image.png](https://upimage.alexhchu.com/2020/08/22/e3a1aa66b3d9a.png)
- 复制到你的hexo项目的_config.yml
- ![image.png](https://upimage.alexhchu.com/2020/08/22/7daca55d23e29.png)
- 部署公钥->新建公钥
- ![image.png](https://upimage.alexhchu.com/2020/08/22/e2c82494864ef.png)
- 你的邮箱公钥在c：/用户/用户名/.ssh/id_rsa.pub
## 回到hexo项目
- hexo clean，hexo g， hexo deploy
- 提交到coding
## 回到coding
- coding帮你自动部署了
- ![image.png](https://upimage.alexhchu.com/2020/08/22/35a800d1a152d.png)
## 创建静态网站
- 点击持续集成-》静态网站

- ![image.png](https://upimage.alexhchu.com/2020/08/22/d7a4c65386248.png)

- ![image.png](https://upimage.alexhchu.com/2020/08/22/2da89a2a2803c.png)

- 点击立刻部署

- ![image.png](https://upimage.alexhchu.com/2020/08/22/b4247b40f6b1f.png)

- 现在就可以访问了

- ![image.png](https://upimage.alexhchu.com/2020/08/22/e9c8680de0eae.png)

  

