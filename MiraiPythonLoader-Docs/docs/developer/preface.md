## 开发环境

- Python≥3.7
- 优秀的Python IDE （本文档使用PyCharm演示）



## 构建插件项目

- 使用PyCharm新建项目，在项目下打开终端
- 输入<code>pip install mplapi</code>下载MPL的插件开发包
- 输入<code>python -m mplapi -n 你的插件名称</code>生成插件项目模板
- 打开**你的插件名称**目录下的**\_\_init\_\_.py**文件开始编写插件

## MPL插件项目结构

> 项目目录         
> ├─ plugins            
> │  ├─ 你的插件名称        
> │  │  └─ \_\_init_\_\.py  
> │  └─ \_\_init\_\_.py     