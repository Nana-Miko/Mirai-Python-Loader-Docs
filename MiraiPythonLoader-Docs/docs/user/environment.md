**MPL**需要以下环境支持运行

> - [Mirai Console Loader](https://github.com/iTXTech/mirai-console-loader) 以下简称**mcl**
> - [mirai-api-http](https://github.com/project-mirai/mirai-api-http) 以下简称**mah**

------

## 配置 mirai-api-http

在安装好mah插件后，启动mcl，找到mah的配置文件<code>setting.yml</code>

mah的官方仓库readme中有<code>setting.yml</code>的配置模板

**必须**的配置项有：

```yaml
adapters:
  - http
enableVerify: true
singleMode: false
```

若无自定义需求，也可以复制粘贴以下模板

```yaml
adapters:
  - http
enableVerify: true
verifyKey: abcd1234efgh  # 该值为验证用密钥，请自行修改
debug: false
singleMode: false
cacheSize: 4096
adapterSettings:
  http:
    host: 0.0.0.0
    port: 8081 # 遇端口号占用时，请修改该值
    cors: ["*"]
    unreadQueueMaxSize: 100

```

出现如下字样时，表明mah启动成功

![mah-success](/user/img/mah-success.png)

若出现如下字样，表明mah的端口被占用，请修改端口号后重新尝试

![mah-success](/user/img/mah-fail.png)

------



## 安装Python

以windows系统为例，linux系统基本自带python

前往[python官网](https://www.python.org/)下载python并安装**（版本≥3.7）**

- [x] Add Python xx to PATH 👈 **切记**安装时此项要选中（错选请自行查找将Python添加至环境变量的方法）

打开CMD，输入<code>python --version</code>>出现如下结果时表示成功！

```bash
C:\Users\user>python --version
Python 3.9.6

#👇未添加至环境变量

C:\Users\user>python --version
'python' 不是内部或外部命令，也不是可运行的程序
或批处理文件。
```

