# python

##### **字典一个键需要对应多个值时**

- 使用collection 里的 defaultdict来创建

  ```
  {
      "a":[1,2,3,3,4],
      "b":{1,2,3,4}
  }
  from collections import defaultdict 
  # key值容器为有序时
  dict_list = defaultdict(list)
  
  # key值容器为去重时
  dict_set = defaultdict(set)
  ```

##### 有序字典

OrderDict()

##### zip（）方法将字典的key, value值 调换

```
min(zip(dict.values(), dict.keys()))
```

##### 查找两个字典的相同键值

```
a = {
    'x':1,
    'y':2,
    'z':3
}
b = {
    'w' : 10,
    'x' : 11,
    'y' : 2
}

使用keys()集合 或者  items()返回的结果集操作
# Find keys in common
a.keys() & b.keys() # { 'x', 'y' }
# Find keys in a that are not in b
a.keys() - b.keys() # { 'z' }
# Find (key,value) pairs in common
a.items() & b.items() # { ('y', 2) }

修改过滤字典元素
# Make a new dictionary with certain keys removed
c = {key:a[key] for key in a.keys() - {'z', 'w'}}
# c is {'x': 1, 'y': 2}
```

##### 使用多个界定符分割字符串

re.split()