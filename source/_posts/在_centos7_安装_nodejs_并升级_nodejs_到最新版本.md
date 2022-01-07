---
 title: 在_centos7_安装_nodejs_并升级_nodejs_到最新版本.md
---
## 1. 安装 nodejs
------------

### 1.1 使用 EPEL 安装

EPEL（Extra Packages for Enterprise Linux）企业版 Linux 的额外软件包，是 Fedora 小组维护的一个软件仓库项目，为 RHEL/CentOS 提供他们默认不提供的软件包。  
先确认系统是否已经安装了 epel-release 包：
```
$ yum info epel-release

```

如果有输出有关 epel-release 的已安装信息，则说明已经安装，如果提示没有安装或可安装，则安装

```
$ yum install epel-release
```

安装完后，就可以使用 yum 命令安装 nodejs 了，安装的一般会是 6.x 的版本，并且会将 npm(3.x) 作为依赖包一起安装
```
$ sudo yum install nodejs
```

安装完成后，验证是否正确的安装，`node -v`，如果输出如下版本信息，说明成功安装

```
v6.13.3
```

问题来了，现在 nodejs 发的版本比较快，有些新的框架需要 node 的新版本，那如何升级。到现在，node 的最新版本是`10.4.1`，那么，下面介绍如何升级 nodejs

### 1.2 卸载 nodejs

> 注意：这里卸载并非必要步骤。只是提供卸载的方案，请按需操作，不要安装后又删除又进行安装掉进死循环了。

1.2.1 使用 yum 先删除一次

```
yum remove nodejs npm -y
```

1.2.2 手动删除残留

*   进入 /usr/local/lib 删除所有 node 和 node_modules 文件夹
*   进入 /usr/local/include 删除所有 node 和 node_modules 文件夹
*   检查 ~ 文件夹里面的 "local" "lib" "include" 文件夹，然后删除里面的所有 "node" 和 "node_modules" 文件夹
*   可以使用以下命令查找 `$ find ~/ -name node` `$ find ~/ -name node_modules`

1.2.3 进入 /usr/local/bin 删除 node 的可执行文件

*   删除: /usr/local/bin/npm
*   删除: /usr/local/share/man/man1/node.1
*   删除: /usr/local/lib/dtrace/node.d
*   删除: rm -rf /home/[homedir]/.npm
*   删除: rm -rf /home/root/.npm

## 2. 升级 nodesj
------------

### 2.1 安装 n

n 是 nodejs 管理工具，是 TJ 写的，Github: [https://github.com/tj/n](https://github.com/tj/n)

```
$ npm install -g n
```

### 2.2 安装 nodejs 版本

安装最新版

```
$ n latest
```

安装指定版本

```
$ n 8.11.3  
```

### 2.3 切换 nodejs 版本

```
$ n
```

选择已安装的版本

```
   node/8.11.3
   node/10.4.1
```

查看当前版本`node -v`，下面表示已切换成功

```
v8.13.3
```

但问题来了，切换后，查看版本还是原来的 v6.13.3，看下面 **使用 n 切换 nodejs 版本失效的解决办法**

## 3 切换失效的解决办法
-----------

### 3.1 查看 node 当前安装路径

```
$ which node
/usr/local/bin/node #举个例子
```

### 3.2 修改默认路径
而 n 默认安装路径是 /usr/local，若你的 node 不是在此路径下，n 切换版本就不能把 bin、lib、include、share 复制该路径中，所以我们必须通过 N_PREFIX 变量来修改 n 的默认 node 安装路径。  
编辑环境配置文件：

```
$ vim ~/.bash_profile
```

### 3.3 将下面两行代码插入到文件末尾：

```
export N_PREFIX=/usr/local #node实际安装位置
export PATH=$N_PREFIX/bin:$PATH
```

### 3.4 `:wq`保存退出

### 3.5 执行 source 使修改生效。

```
$ source ~/.bash_profile
```

### 3.6 这时候再查看`node -v`发现版本切换成功了。
