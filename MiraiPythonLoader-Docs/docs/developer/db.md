MPL内置了sqlite数据库且进行统一管理，在`PyPlugin`中提供了连接方法

## 连接数据库

使用`PyPlugin`类的`get_database`方法

```
def get_database(self, db_name='database') -> sqlite3.Connection:
	pass
```

`get_database`方法接收一个参数

- `db_name`:数据库名称，默认值`'database'` **（不需要.db后缀）**
- 返回`sqlite3.Connection`数据库连接对象

```python
class MyPluginClass(PyPlugin):

    async def on_login(self, bot: Bot):
        db_conn = self.get_database()
        cursor = db_conn.cursor()
        sql = 'CREATE TABLE IF NOT EXISTS user(qq integer PRIMARY KEY, conf text)'
        cursor.execute(sql)
        db_conn.commit()
        db_conn.close() # 切记最后断开数据库连接
```

------

数据库保存目录：`MPL目录/data/插件名/`