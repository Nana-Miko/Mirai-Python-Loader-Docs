## 关于异步方法抛出的异常

在`PyPlugin`和`PluginTask`中，都有被`async`标记的异步方法，他们都是无返回值的

因此MPL在调用这些异步方法时，并没有去获取它们的`Future`对象，这意味着异步方法中抛出的异常会被MPL所无视，这不利于插件的调试和后续的bug修复

## catch_async_exception异步装饰器

保证代码没有异常？非常困难！

`try-except`大段代码？一点也不优雅！

没关系，MPL提供了`catch_async_exception`异步装饰器

将`catch_async_exception`装饰到实现了`PluginExceptionCatcher`接口的类异步方法中，MPL就会捕获该方法抛出的异常

`PyPlugin`和`PluginTask`均已实现`PluginExceptionCatcher`接口

```python
from mplapi.plugin import catch_async_exception # 导入

class MyPluginClass(PyPlugin):
    
    @catch_async_exception  # 装饰异步方法
	async def get_group_msg(self, bot: Bot, source: msg.Source, message: msg.MsgChain):
    	raise Exception()
```