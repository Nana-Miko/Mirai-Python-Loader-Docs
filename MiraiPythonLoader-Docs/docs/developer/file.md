MPL统一管理了文件保存的目录，在`PyPlugin`中提供方法

## 获取文件保存目录

使用PyPlugin类的`get_files_path`方法

```
def get_files_path(self) -> str:
	pass
```

`get_files_path`方法不接收任何参数，返回文件保存路径**（最后带分隔符'/'）**

```python
class MyPluginClass(PyPlugin):

    @catch_async_exception
    async def get_group_msg(self, bot: Bot, source: msg.Source, message: msg.MsgChain):
        for plain_msg in message.get_plain_msg(): # 遍历文字消息列表
            file_path = self.get_files_path() + '群友的话.txt'
            with open(file_path, 'a', encoding='utf-8') as file:
                file.write(plain_msg.text+'\n')
```