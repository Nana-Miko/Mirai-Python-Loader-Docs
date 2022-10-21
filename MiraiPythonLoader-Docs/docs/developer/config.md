插件往往需要保存和获取一些自定义的配置信息

MPL统一管理了所有插件的配置信息，并在PyPlugin中提供了操作配置文件的方法

## 获取配置信息

使用`PyPlugin`类的`get_config`方法

```python
def get_config(self, conf_name:str = 'plugin') -> dict:
	pass
```

`get_config`方法接收一个参数：

- `conf_name`:配置文件名，默认值为`‘plugin’`
- 返回一个字典对象

```python
class MyPluginClass(PyPlugin):

    async def on_login(self, bot: Bot):
        conf = self.get_config()
```

------

## 保存配置信息

使用`PyPlugin`类的`set_config`方法

```python
def set_config(self, conf:dict, conf_name:str = 'plugin'):
	pass
```

`set_config`方法接收两个参数：

- `conf`:配置信息
- `conf_name`:配置文件名，默认值为`‘plugin’`

```python
class MyPluginClass(PyPlugin):

    async def on_login(self, bot: Bot):
        conf = self.get_config() # 获取配置文件
        conf[bot.bot_qq] = bot.login_time  # 更改一些配置
        self.set_config(conf) # 保存配置
```

更改完后请一定要使用`set_config`方法保存你更改后的配置

否则当MPL重启后，配置文件将不会被改变