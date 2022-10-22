MPL提供了`Permission`权限类，每个bot在登录时，都会为每个插件分配一个`Permission`对象，通过Bot对象方法获取

## 获取权限对象

通过`get_plugin_permission`方法获取权限对象

```python
def get_plugin_permission(self, plugin_name: str) -> Permission:
	pass
```

`get_plugin_permission`接收一个参数：

- `plugin_name`:插件名
- 返回权限对象

```python
class MyPluginClass(PyPlugin):

    async def on_login(self, bot: Bot):
        perm = bot.get_plugin_permission(self.get_plugin_name())
```

------

## 权限模式

`Permission`提供三个权限模式：

- `WHITE_LIST_MODE`: 白名单模式 （在权限列表内的好友/群才有权限使用插件）
- `BLACK_LIST_MODE`: 黑名单模式 （在权限列表外的好友/群才有权限使用插件）
- `ALL_MODE`: 全开放模式 （任意好友/群都有权限使用插件）

群和好友默认全开放模式，如有权限管理需求，可自行设置模式

```python
from mplapi.mirai import Permission # 导入

class MyPluginClass(PyPlugin):

    async def on_login(self, bot: Bot):
        perm = bot.get_plugin_permission(self.get_plugin_name()) # 获取Permission对象
        perm.set_friend_mode(Permission.BLACK_LIST_MODE) # 设置好友模式为黑名单
        perm.set_group_mode(Permission.WHITE_LIST_MODE) # 设置群模式为白名单
        perm.add_group(123465) # 添加群至群权限列表
        perm.add_friend(123456) # 添加好友至群权限列表
        bot.set_plugin_permission(self.get_plugin_name(),perm) # 使权限设置生效
```

------

## 插件管理员

通过`Permission`对象的`add_admin`的方法，可添加插件管理员

管理员发送的消息会调用`PyPlugin`的`get_admin_msg`方法

```python
class MyPluginClass(PyPlugin):

    async def get_admin_msg(self, bot: Bot, source: msg.Source, message: msg.MsgChain):
        self.get_logger().info(f'收到了来自管理员{source.sender}发送的消息')
```

### PIN

当插件无管理员时（例如插件第一次被加载）MPL会在控制台显示一个和Bot对应的**八位PIN**

用户可通过私聊Bot发送这个PIN值来认证成为管理员