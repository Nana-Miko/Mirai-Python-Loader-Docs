# Mirai Python Loader

**基于Mirai Conole Loader 与 Mirai-Api-Http 打造的Mirai-Python插件加载框架**



## 特性

* 轻量化，资源开销少
* 支持多Bot登录，掉线自动重连
* 各插件间文件，日志，配置，数据库分离，方便统一管理

## 插件开发方便

只需手敲两行代码即可完成最简单的插件！

- 使用开发包在项目中生成插件项目模板

```bash
F:\项目\MyPlugin>python -m mplapi -n MyPlugin
```

- 重写PyPlugin类方法

```python
async def get_group_msg(self, bot: Bot, source: msg.Source, message: msg.MsgChain):
	bot.send_group_msg(msg.PlainMsg('Hello,World'), source.group)
```



------



## <center>支持</center>

<center>[![image](https://img.shields.io/badge/github-MCL-blue.svg)](https://github.com/iTXTech/mirai-console-loader)[![image](https://img.shields.io/badge/github-MAH-green.svg)](https://github.com/project-mirai/mirai-api-http)</center>

