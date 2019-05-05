工欲善其事，必先利其器。

---

## Charles

### 安装

 1. [官方](https://www.charlesproxy.com/download/)下载，安装，有钱买个 `license` 。
 2. 电脑安装证书：**[help]** -> **[SSL proxying]** -> **[Install Charles Root Certificate]** ，之后电脑选择始终信任。
 3. 电脑配置安全域名：**[Proxy]** -> **[SSL Proxying Settings]** -> **[SSL Proxying]** ， 添加 host: * & port: * 。

### 电脑抓包

 1. **[Proxy]** -> **[Mac OS X Proxy]** ， 打开。
 2. 注意电脑本身的代理，翻墙关闭。
 3. 访问任意网页，在 Charles 里查看抓包数据。

### 手机抓包

 1. 在手机网络上手动配置当前电脑的代理，电脑上的 Charles 同意。
 2. 手机安装证书：**[Safari]** -> **[chls.pro/ssl]** -> **[安装证书]** -> **[设置]** -> **[通用]** -> **[关于本机]** -> **[证书信任设置]** ，开启信任。
 3. 访问任意网页，在 Charles 里查看抓包数据。

### Map（待细细研究）

 - Map Remote: 将远程的网络请求，重定向到另一个远程或者本地
 - Map Local: 

### Rewrite

### Breakpoints

### 模拟慢速网络

 1. **[Proxy]** -> **[Throttle Setting]**

### 修改网络请求内容

 1. 选中某个请求，**[右击]** -> **[Compose / Edit]** ，做相应修改，然后运行。

### 压力测试

 1. 选中某个请求，**[右击]** -> **[Repeat Advanced]** ，配置次数。


### 反向代理

### 设置外部代理，解决翻墙冲突



### 参考
 - [Charles](https://www.charlesproxy.com/)
 - [Charles 从入门到精通](https://blog.devtang.com/2015/11/14/charles-introduction/)
 - [Mac上使用Charles抓包](https://zhuanlan.zhihu.com/p/26182135)

---

好记性不如烂笔头。