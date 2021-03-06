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

# 进程和线程
## **asyncio**  

协程通过 async/await来声明

```python
    import asyncio

    async def main():
        print('hello')
        await asyncio.sleep(1)
        print('world')
```
真正运行一个协程，asyncio提供了三种机制：

- asyncio.run(main()) 运行函数入口点
- 等待一个协程
 ```python
    import asyncio
    import time 

    async def say_after(delay, what):
        await asyncio.sleep(delay)
        print(what)

    async def main():
        print(f"started at {time.strftime('%X')}")
        await say_after(1, 'hello')
        await say_after(2, 'world')
        print(f"finished at {time.strftime('%X')}")
    asyncio.run(main())
 ```
- asyncio.create_task() 用来并发运行作为asyncio任务的多个协程
```python
    import asyncio
    import time 

    async def say_after(delay, what):
        await asyncio.sleep(delay)
        print(what)

    async def main():
        task1 = asyncio.create_task(say_after(1,'hello'))
        task2 = asyncio.create_task(say_after(2,'world'))

        print(f"started at {time.strftime('%X')}")
        await task1
        await task1
        print(f"finished at {time.strftime('%X')}")
    asyncio.run(main())
```
**可等待对象(awaitables)**  
如果一个对象可以被await声明，那么我们就称这个对象为可等待对象。(We say that an object is an awaitable object if it can be uesed in an await expression.)
There are three main types of awaitable objects: coroutines, tasks, and Futures.  

**协程**概念：

- 协程函数. 定义形式为async def的函数
- 协程对象. 调用协程函数所返回的对象

**任务(task)**  
任务被用来设置日程以便并发执行协程。  
当一个协程通过asyncio.create_task()等函数打包成一个任务,该协程将自动排入日程准备立即运行。

**Future 对象**  
Future 是一种特殊的**底层级**可等待对象，表示一个异步操作的**最终结果**.

当一个 Future 对象 *被等待*，这意味着协程将保持等待直到该 Future 对象在其他地方操作完毕。

在 asyncio 中需要 Future 对象以便允许通过 async/await 使用基于回调的代码。

通常情况下 **没有必要** 在应用层级的代码中创建 Future 对象。



## Theading

### Lock

**class.Lock** 实现原始锁对象的类。一旦一个线程获得一个锁，会阻塞随后尝试获得锁的所有线程，直到它被释放，任何线程都可以释放它。

Lock是个工厂函数。

**acquire(blocking=ture, timeout=1)**

**release()**

### Event

判断一个线程是否已经启动。



# with

[with语句详情](<https://www.ibm.com/developerworks/cn/opensource/os-cn-pythonwith/index.html>)

# class 类

## \__slots__魔法方法

在实例化对象时，节省内存使用。

在Python中，每个类都有实例属性。默认使用一个字典来存储一个对象的实例属性。这样便允许我们在运行时取设置任意的新属性。

然而，对于有着已知属性的小类来说，它可能是个瓶颈。这个字典浪费了很多内存。Python不能在对象创建时直接分配一个固定量的内存来保存所有的属性。因此如果你创建许多对象（我指的是成千上万个），它会消耗掉很多内存。
不过还是有一个方法来规避这个问题。这个方法需要使用`__slots__`来告诉Python不要使用字典，而且只给一个固定集合的属性分配空间。

测试方法 使用该类：<https://github.com/ianozsvald/ipython_memory_usage>

# pip 国内源

阿里云 http://mirrors.aliyun.com/pypi/simple/ 
中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/ 
豆瓣(douban) http://pypi.douban.com/simple/ 
清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/ 

```
pip install -i 国内源
```

# 日志的运用

logging

TimedRotatingFileHandler



```python
import logging
from logging.handlers import TimedRotatingFileHandler

def init_logger():
	# 不存在log文件夹时，自动创建
    # 对logging进行初始化配置
    # 
    if not os.path.exists(LOG_DIR):
        os.makedirs(LOG_DIR)

    log_file = os.path.join(LOG_DIR, MANAGER_CONFIG['log_file'])
    fmt = '%(asctime)s %(filename)s[line:%(lineno)d] [Thread ID:%(thread)d] [%(levelname)s]' \
          ' -- %(funcName)s(). %(message)s'
    logging.basicConfig(format=fmt)

    filehandler = TimedRotatingFileHandler(log_file, 'D', 1, 0, encoding='utf-8')
    filehandler.suffix = '%Y-%m-%d.log'
    filehandler.setFormatter(logging.Formatter(fmt))

    my_logger = logging.getLogger('ms_logger')
    my_logger.setLevel(MANAGER_CONFIG['log_level'].upper())
    my_logger.addHandler(filehandler)

    return my_logger

    
```

# 装饰器的使用
```python
from functools import wraps

'''总需求 构建一个装饰器其可以被传入参数。而且也可以在不传入参数的时候其拥有默认值。并且可以通过@deco进行直接调用'''
"""wraps的使用"""


def deco1(a):
    """这是构造一个可以传入参数的装饰器，但是参数必须进行传入"""
    print(a)

    def deco2(func):
        # print(b)
        @wraps(func)
        def wrapper(*args, **kwargs):
            print("this is deco1")
            func(*args, **kwargs)

        return wrapper
    return deco2

def deco2(func):
    # print(b)
    def wrapper(*args, **kwargs):
        print("this is deco2")
        func(*args, **kwargs)
    return wrapper


def deco2_1(a=None):
    def deco2(func):
        print(a)
        def wrapper(*args, **kwargs):
            print("this is deco2")
            func(*args, **kwargs)
        return wrapper
    return deco2

def deco3(function=None):
    """这是对deco1构造的装饰添加参数后 并进行返回
        通过if function 以及wraps的使用让deco1装饰器变为可以@deco1而不需要@deco1()
    """
    print("deco3")
    act_deco = deco1("deco3")
    if function:
        return act_deco(function)

    return act_deco

# @deco1
#@deco2
@deco2_1("哈哈")
# @deco3
def func(a):
    print("cccc" + a)



func("aaa")
```

