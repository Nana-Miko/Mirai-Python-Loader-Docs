每个插件启动时，MPL都会为其分配一个日志对象`MPLLoggerHandler`

## 获取日志对象

在`PyPlugin`中使用`get_logger`方法得到日志对象

MPL已经为其加载了两个`Handle`:

- StreamHandle （输出到控制台）默认等级`DEBUG`
- FileHandle （输出到文件）默认等级`WARNING`

为方便调试，可以使用`MPLLoggerHandler`中的`set_stream_level`和`set_file_level`改变日志等级

```python
from mplapi import MPLLoggerHandler # 导入

class MyPluginClass(PyPlugin):
    
    def on_create(self):
        logger = self.get_logger()
        logger.set_file_level(MPLLoggerHandler.DEBUG) # 设置file等级
        logger.debug('DEBUG INFO') # 输出debug信息
        logger.info('INFO') # 输出info信息
        logger.warning('WARNING INFO') # 输出warning信息
        logger.error('ERROR INFO') # 输出error信息
        logger.critical('CRITICAL INFO') # 输出critical信息
```

------

log文件目录：`MPL目录/logs/插件名/`