# movielens 项目进行

## 1. 项目环境包依赖安装

1. 安装papermill、scrapbook包

2. 安装recommenders包。

   安装失败，提示如下

   > error: Microsoft Visual C++ 14.0 or greater is required. Get it with "Microsoft C++ Build Tools": https://visualstudio.microsoft.com/visual-cpp-build-tools/

   解决：安装c++ build tools生成工具。大约8g大小，安装在目录cppbuild下。提示安装了如下包

   > Successfully installed category-encoders-1.3.0 cornac-1.14.2 lightfm-1.17 lightgbm-3.3.5 pandera-0.14.5 powerlaw-1.5 recommenders-1.1.1 scikit-learn-1.0.2 scikit-surprise-1.1.3


## 2. wikidata数据抽取问题

在使用find_wikidata_id方法抽取wikidata中的数据时遇到错误

```python
 entity_id = find_wikidata_id(name)
    if entity_id == "entityNotFound":
        continue
 """       
  返回结果
  	ENTITY NOT FOUND
    ENTITY NOT FOUND
    ENTITY NOT FOUND
    ENTITY NOT FOUND
    ENTITY NOT FOUND
    ENTITY NOT FOUND
    ENTITY NOT FOUND
 """
```

find_wikidata_id函数包含：

```python
except Exception:
        # TODO: distinguish between connection error and entity not found
        logger.error("ENTITY NOT FOUND")
        return "entityNotFound"
```

初步推断：是url出了问题，本地挂梯子也没用。

> 已解决：是终端为配置代理原因
> 将代理配置在环境变量中就可以解决，配置如下：

![image-20230330210303089](C:\Users\河马\AppData\Roaming\Typora\typora-user-images\image-20230330210303089.png)

## 3. 读取movies.dat数据的第二列，电影名

- 在使用`pd.read_csv`时要把 `encoding`设置为'ISO-8859-1'，否则读不出来
- 读出来的是array格式吗，要转换成list
- `list0`是一个二维list，而代码处理的是一维list，因此要将二维list转为一维的。

```python
list0 = pd.read_csv('./movies.dat', sep='::', usecols=[1], encoding='ISO-8859-1', header=None).values.tolist()
names = [i for item in list0 for i in item]
```

## 4. 连接服务器

说来有点惭愧，现在还没在服务器上跑过代码，麻

跑3800多条数据大概要十个小时，cpu时间少，walltime时间多，想办法降低wall time

## 5. 保存数据

保存dataframe数据

## 6. neo4j的使用

报错解决：

1. Cannot decode response content as JSON：

   [Neo4j报错Cannot decode response content as JSON 解决方案_](https://blog.csdn.net/qq_23044461/article/details/127879715) 

   graph_name为空时的问题，加`name='neo4j'`字段

   ```python
   graph = Graph("http://localhost:7474/", auth=('neo4j','Zkti888'), name='neo4j')
   ```

2. java.lang.OutOfMemoryError: Java heap space