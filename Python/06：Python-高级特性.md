###   Python 高级特性

------

[TOC]

##### 01：python3 pip

- pip 是 **Python 包管理工具**，该工具提供了对 Python 包的查找、下载、安装、卸载的功能。

-  pip --version

  - 验证是否安装pip；

- 下载安装包使用以下命令：

  - ```shell
    pip install some-package-name
    ```

- 卸载安装包

  - ```python
    pip uninstall some-package-name
    ```

- 查看我们已经安装的软件包

  - ```python
    pip list
    ```

##### 02：Python Json数据解析

- Python3 中可以**使用 json 模块来对 JSON 数据进行编解码**，它包含了两个函数：

  - **json.dumps():** 对数据进行编码。
  - **json.loads():** 对数据进行解码。

- Python 编码为 JSON 类型转换对应表：

  | Python                                 | JSON   |
  | :------------------------------------- | :----- |
  | dict                                   | object |
  | list, tuple                            | array  |
  | str                                    | string |
  | int, float, int- & float-derived Enums | number |
  | True                                   | true   |
  | False                                  | false  |
  | None                                   | Null   |

###### 示例

- ```python
  #!/usr/bin/python3
  import json
   
  # Python 字典类型转换为 JSON 对象
  data1 = {
      'no' : 1,
      'name' : 'Runoob',
      'url' : 'http://www.runoob.com'
  }
   
  json_str = json.dumps(data1)
  print ("Python 原始数据：", repr(data1))
  print ("JSON 对象：", json_str)
   
  # 将 JSON 对象转换为 Python 字典
  data2 = json.loads(json_str)
  print ("data2['name']: ", data2['name'])
  print ("data2['url']: ", data2['url'])
  ```

##### 03：Python requests

- r = requests.get(url，params=None，**kwargs)

  - `url:页面的URL链接
  - `params`：URL中的额外参数，字典或者字节流格式
  - `kwargs`：12个控制访问的参数

- ```python
  import requests
  
  payload = {'key1': 'value1', 'key2': 'value2'}
  r = requests.get("http://httpbin.org/get", params=payload)
  # json 响应内容
  r.json()
  # 文本
  r.text
  
  # www 方式
  payload = {'key1': 'value1', 'key2': 'value2'}
  r = requests.post("http://httpbin.org/post", data=payload)
  # Json 方式
  response = requests.post('https://httpbin.org/post', json={'id': 1, 'name': 'ram sharma'})
  print(response.status_code)
  ```

##### 04：Python Pandas

- Pandas 一个强大的分析结构化数据的工具集，可以从各种文件格式比如 CSV、JSON、SQL、Microsoft Excel 导入数据。

###### 导入 pandas

- ```python
  import pandas as pd
  ```

###### Pandas 数据结构【Series & DataFrame】

- Pandas Series 类似表格中的一个列（column），类似于一维数组，可以保存任何数据类型。

- pandas.Series( data, index, dtype, name, copy)

  - **data**：一组数据(ndarray 类型)。
  - **index**：数据索引标签，如果不指定，默认从 0 开始。
  - **dtype**：数据类型，默认会自己判断。
  - **name**：设置名称。
  - **copy**：拷贝数据，默认为 False。

- DataFrame 是一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型（数值、字符串、布尔型值），既有行索引又有列索引。

- pandas.DataFrame( data, index, columns, dtype, copy)

  - **data**：一组数据(ndarray、series, map, lists, dict 等类型)。
  - **index**：索引值，或者可以称为行标签。
  - **columns**：列标签，默认为 RangeIndex (0, 1, 2, …, n) 。
  - **dtype**：数据类型。
  - **copy**：拷贝数据，默认为 False。

- ```python
  import pandas as pd
  
  a = ["Google", "Runoob", "Wiki"]
  myvar = pd.Series(a, index = ["x", "y", "z"])
  print(myvar["y"])
  # x Google
  # y Runoob
  
  sites = {1: "Google", 2: "Runoob", 3: "Wiki"}
  myvar = pd.Series(sites)
  print(myvar)
  # 1 Google
  # 2 Runoob
  
  import pandas as pd
  
  data = {'Site':['Google', 'Runoob', 'Wiki'], 'Age':[10, 12, 13]}
  df = pd.DataFrame(data)
  print (df)
  # Site   Age
  # Google 10
  ```

###### CSV 文件

- read_csv：读取CSV 文件；

- to_csv：存储为CSV 文件；

- ```python
  import pandas as pd
     
  # 三个字段 name, site, age
  nme = ["Google", "Runoob", "Taobao", "Wiki"]
  st = ["www.google.com", "www.runoob.com", "www.taobao.com", "www.wikipedia.org"]
  ag = [90, 40, 80, 98]
  # 字典
  dict = {'name': nme, 'site': st, 'age': ag}  
  df = pd.DataFrame(dict)
  # 保存 dataframe
  df.to_csv('site.csv')
  ```

##### 05：



