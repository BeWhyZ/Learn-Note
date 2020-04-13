# 编码风格

>  为了统一代码编写的风格，使编写好的代码更有利于自己以及他人进行维护与修改。（目前均是面向过程）
>
> 而下面的语言则是 各个语言具体对编码风格的实现

参考-> [编写易于理解代码的六种方式](https://www.ibm.com/developerworks/cn/linux/l-clear-code/index.html), [The Joel Test: 12 Steps to Better Code 推荐(里面有很多别的一些开发的点可以进行注意)](https://www.joelonsoftware.com/2000/08/09/the-joel-test-12-steps-to-better-code/)

1. **注释**

   > 1. 过程/函数之前写几句话，说明其功能。（**简写**）
   > 2. 对过程中的数据结构进行简单的描述
   >    1. 对传递给它的数值的一个描述。
   >    2. 如果是函数，对其返回结果的一个描述。
   > 3. 在过程/函数内部，能将代码分解为更短小的任务的注释。
   > 4. 对于看起来有些难懂的大块代码，对其成因给与简短的解释。(**详写**：成因)
   >
   > 由上面4点可以看出注释会进行重复描述的。所以将注释的侧重点进行拆分。
   >
   > **注意**： 在进行维护或者修改的时候，添加某一些功能时：必须要根据以上流程加上注释。

2. **常量**

   > 对于一些常量数值，一定要通过定义 一个变量来使用

3. **需求（BUG）定义** (taskType module taskDesr )

   - taskType(issue/bug/reproduce) 任务的类型(新的需求/bug/重构)
   - module 具体为项目中那一块的内容
   - taskDesr 任务的简单描述

4. **BUG**

   > 需要对项目中所见到的bug进行收集。
   >
   > 而对于存储一个bug应该具备以下的数据：
   >
   > 1. 重现bug的步骤 *
   > 2. expected behavior（期望获取的结果 或者 功能）*
   > 3. observed (buggy) behavior （实际的结果）*
   > 4. 被分配给谁
   > 5. 是否已修复
   >
   > 零缺陷理论(zero defects methodology): **在写任何新代码之前，修复已知的bug**。(修复bug的优先级高于新代码的编写，原因：修复bug所需的时间不确定性。修复时间不确定性是由于程序员对于代码的熟悉度，当对于代码越熟悉时修复Bug所需要的时间理论上会更短，否则则会不确定。实现一个新功能则可以有一个大概的时间预估。所以对于每天工作时间任务的预估，在有bug存在的时候，该预估都是不可靠的。)

5. 每天应该有个自己的时间表

   > 时间表 一是：明天所需要的时间表，二是今天所需要的时间表
   >
   > 时间表的好处： 准时完成任务(下班!)；删掉不重要的功能
   >
   > 可以尝试 使用excel进行规划
   >
   > 新功能实现：应该将其划分为多个子任务。具体的Step则需要在将要进行code前才写下。

6. 编写文档



# Python

**注释**

- api

```python
  def connect_to_next_port(self, minimum):
    """Connects to the next available port.

    Args:
      minimum(int): A port value greater or equal to 1024.

    Returns:
      The new minimum port.

    Raises:
      ConnectionError: If no available port is found.
    """
    if minimum < 1024:
      # Note that this raising of ValueError is not mentioned in the doc
      # string's "Raises:" section because it is not appropriate to
      # guarantee this specific behavioral reaction to API misuse.
      raise ValueError('Minimum port must be at least 1024, not %d.' % (minimum,))
    port = self._find_next_open_port(minimum)
    if not port:
      raise ConnectionError('Could not connect to service on %d or higher.' % (minimum,))
    assert port >= minimum, 'Unexpected port %d when minimum was %d.' % (port, minimum)
    return port
```

- Modules--File

  ```python
  # 每一个文件第一行最好有个对该程序的简单介绍
  """A one line summary of the module or program, terminated by a period.
  
  Leave one blank line.  The rest of this docstring should contain an
  overall description of the module or program.  Optionally, it may also
  contain a brief description of exported classes and functions and/or usage
  examples.
  
    Typical usage example:
  
    foo = ClassFoo()
    bar = foo.FunctionBar()
  """
  
  ```

- Class---Func

  ```python
  class SampleClass(object):
      """Summary of class here.
  
      Longer class information....
      Longer class information....
  
      Attributes:
          likes_spam: A boolean indicating if we like SPAM or not.
          eggs: An integer count of the eggs we have laid.
      """
  
      def __init__(self, likes_spam=False):
          """Inits SampleClass with blah."""
          self.likes_spam = likes_spam
          self.eggs = 0
  
      def public_method(self):
          """Performs operation blah."""
          
  def fetch_bigtable_rows(big_table, keys, other_silly_variable=None):
      """Fetches rows from a Bigtable.
  
      Retrieves rows pertaining to the given keys from the Table instance
      represented by big_table.  Silly things may happen if
      other_silly_variable is not None.
  
      Args:
          big_table: An open Bigtable Table instance.
          keys: A sequence of strings representing the key of each table row
              to fetch.
          other_silly_variable: Another optional variable, that has a much
              longer name than the other args, and which does nothing.
  
      Returns:
          A dict mapping keys to the corresponding table row data
          fetched. Each row is represented as a tuple of strings. For
          example:
  
          {'Serak': ('Rigel VII', 'Preparer'),
           'Zim': ('Irk', 'Invader'),
           'Lrrr': ('Omicron Persei 8', 'Emperor')}
  
          If a key from the keys argument is missing from the dictionary,
          then that row was not found in the table.
  
      Raises:
          IOError: An error occurred accessing the bigtable.Table object.
      """
  ```

- 在代码块中注释

  > 如果要进行一些复杂的逻辑时，最好在该代码块之前写一段注释来简单描述一下这次逻辑
  >
  > ```
  > # We use a weighted dictionary search to find out where i is in
  > # the array.  We extrapolate position based on the largest num
  > # in the array and the array size and then do binary search to
  > # get the exact number.
  > ```

  

**import**

```python
# 导入一个方法时，尽量使用full path name防止有第二个同名文件出现

# import 尽可能的分行
# yes
    import os
    import sys
# no
	import os, sys
```

**name**

| Type                       | Public               | Internal                          |
| -------------------------- | -------------------- | --------------------------------- |
| Packages                   | `lower_with_under`   |                                   |
| Modules                    | `lower_with_under`   | `_lower_with_under`               |
| Classes                    | `CapWords`           | `_CapWords`                       |
| Exceptions                 | `CapWords`           |                                   |
| Functions                  | `lower_with_under()` | `_lower_with_under()`             |
| Global/Class Constants     | `CAPS_WITH_UNDER`    | `_CAPS_WITH_UNDER`                |
| Global/Class Variables     | `lower_with_under`   | `_lower_with_under`               |
| Instance Variables         | `lower_with_under`   | `_lower_with_under` (protected)   |
| Method Names               | `lower_with_under()` | `_lower_with_under()` (protected) |
| Function/Method Parameters | `lower_with_under`   |                                   |
| Local Variables            | `lower_with_under`   |                                   |

**Func**

> 每一个函数 尽可能的简单，代码行数要尽可能短，有利于将来修改以及排查bug

```python
# func 形式参数尽可能的分行
# Yes:
    def my_method(
        self, other_arg: Optional[MyLongType],
        a: int = 0
    ) -> Dict[OtherLongType, MyLongType]:
        pass
    # 其中 形参的:后是参数type的描述(desc 期望) = 是默认值
    # ->函数返回结构
```

**TODO**

```python
# 最好备注上 author
# 当完成时候，删除TODO
```





# Java Script

**name**

> 基本都为lowerCamelCase
>
> ```js
> // 枚举或者class name: UpperCamelCase
> 
> // 常量：
>     // Constants
>     const NUMBER = 5;
> 
> // 别名一定要用const 声明
>     const staticHelper = importedNamespace.staticHelper;
>     const CONSTANT_NAME = ImportedClass.CONSTANT_NAME;
>     const {assert, assertInstanceof} = asserts;
> ```

# Reference

[谷歌代码规范参考--python](.\Reference\styleguide _ Style guides for Google-originated open-source projects.html)

[谷歌代码规范参考--Java script](.\Reference\Google JavaScript Style Guide.html)

[谷歌代码规范参考--Java](.\Reference\Google Java Style Guide.html)

[谷歌代码规范参考--C++](.\Reference\Google C++ Style Guide.html)