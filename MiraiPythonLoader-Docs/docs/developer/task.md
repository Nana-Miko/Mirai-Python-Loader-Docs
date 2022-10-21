## 使用场景

当你的插件功能是**触发式**或需要**线性交互**时，如：

### 触发式示例

> 路人A：搜图
>
> Bot：请在30s内发送你要搜索的图片
>
> 路人A：[涩图]
>
> Bot：[涩图] 该图出自…………

### 线性交互示例

> 路人B：百度百科 初音
>
> Bot：初音'的搜索结果非唯一，请选择：1.初音未来 2.游戏《公主连结》中的角色
>
> 路人B：2
>
> Bot：以下是'初音'的百科…………

如果在`get_group_msg`方法中定义所有逻辑，将会涉及各种判断、嵌套、变量保存等。会使代码不易读懂，逻辑混乱

因此，MPL提供了**PluginTask**来解决以上问题

------

## 认识PluginTask

PluginTask是一个抽象类，你需要实现其抽象方法

**自定义PluginTask**

```python
from mplapi.plugin import PluginTask # 导入

class MyTask(PluginTask):
    
    def __init__(self, plugin: PyPlugin):
		super().__init__(plugin) # PluginTask没有无参构造方法，你必须调用其超类构造方法
    
    # 任务的执行体
    async def execute_task(self, bot: Bot, source: msg.Source, message: msg.MsgChain):
        self.plugin_instance.get_logger().info('任务执行！')
    
    # 任务超时时调用
    async def on_timeout(self, bot: Bot):
        self.plugin_instance.get_logger().info('任务超时！')
    
    # 判断是否是该任务的执行对象
    def is_task_target(self, group, target) -> bool:
        pass
```

**实例化**

```python
class MyPluginClass(PyPlugin):
    async def get_group_msg(self, bot: Bot, source: msg.Source, message: msg.MsgChain):
        task = MyTask(self) # 传入PyPlugin对象进行实例化
```

MPL也内置了两个PluginTask的子类

- GroupTask （群任务）
- FriendTask （好友任务）

它们都已经实现了`is_task_target`方法

**自定义GroupTask、FriendTask**

```python
from mplapi.plugin import FriendTask
from mplapi.plugin import GroupTask # 导入

class MyGroupTask(GroupTask):
    async def execute_task(self, bot: Bot, source: msg.Source, message: msg.MsgChain):
        self.plugin_instance.get_logger().info('群任务执行！')

    async def on_timeout(self, bot: Bot):
        self.plugin_instance.get_logger().info('群任务超时！')


class MyFriendTask(FriendTask):
    async def execute_task(self, bot: Bot, source: msg.Source, message: msg.MsgChain):
        self.plugin_instance.get_logger().info('好友任务执行！')

    async def on_timeout(self, bot: Bot):
        self.plugin_instance.get_logger().info('好友任务超时！')
```

**实例化**

```python
class MyPluginClass(PyPlugin):

    async def get_group_msg(self, bot: Bot, source: msg.Source, message: msg.MsgChain):
        group: int  # 执行任务的群号
        target: int  # 执行任务的目标qq号
        group_task = MyGroupTask(group, self, target)
        friend_task = MyFriendTask(target, self)
        
```

其中GroupTask实例化时，target参数不是必须，不传入时意味着该群内的任意成员都可以触发此任务

------

## 提交PluginTask

显然，仅仅实例化一个PluginTask对象是没有任何用处的，你需要将其提交至Bot，由Bot对象判断是否执行

**提交至Bot**

```python
class MyPluginClass(PyPlugin):
	async def get_group_msg(self, bot: Bot, source: msg.Source, message: msg.MsgChain):
       task = MyTask(self)
       task.set_timeout(30) # 设置超时时间30s
       bot.add_plugin_task(task)
```

一个PluginTask对象，在执行后或超时后会被MPL所回收

因此在提交前建议通过`set_timeout`方法设置超时时间，以免长时间未被执行导致任务堆积，消耗内存资源