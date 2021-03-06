#  1 函数
## 1.1 函数概述,
    内涵
        结构化 or 过程化, 对程序逻辑进行  
        基本单元  面向过程程序设计 
    定义
        def
        return
            随时返回 函数结果
            返回形式
                无返回值
                   方法,不用return
                   仅return , 没有返回值
                返回 None
                一个值/对象
                    返回多个对象,默认为元组返回
                规律:
                    运行时对象
                    0, None
                    1, Obeject
                    >1, tuple
            执行到return, 停止执行函数内余下的语句
        空函数  
           pass  占位符,让代码先运行,通常在初始定义类时使用
    补充
        函数的引用
            引用
            别名变量
                函数名    本质即变量
                可用多个别名    对同一函数对象的引用
        函数的调用
        def foo()
            新建函数对象
            赋值给foo
        bar = foo bar [引用] 同一函数对象
        bar() 同 foo() [调用] 函数,会返回函数的return值(如果有)
## 2 装饰器,
### 2.1 Decorator  
       在代码运行期间,给函数动态增加功能
       本质 一个返回函数的高阶函数
       内涵 
           输入参数, 返回值
           不改变函数原值,仅添加功能
       形式
          @Decorator
          def func():
       应用
          引入日志
          增加计时逻辑, 检测性能
### 2.2 多个装饰器堆叠
        @deco2(args)
        @deco1
        def func()
        等价于:
            def func():
                func = deco2(deco_args)(deco1(func()))
### 2.3 定义装饰器
        def log()
           def wrapper():
               print("%s"%func__name__)
               return func
           return wrapper
### 2.4 使用装饰器
        @log放到now()函数定义处
        相当于执行了语句 now = log(now)
        @log
        def now():
           print()
## 3 参数,
### 3.1 完整语法  
#### 参数定义的顺序
            传递方式:位置--关键字--包裹位置--包裹关键字
            参数类型:必选--默认--可变--关键字
### 3.2 参数传递方式
#### 位置传递:
            位置参数,必填参数
            默认参数
#### 关键字传递
            对有默认值的参数传递
            无序,提供参数名传递
            按位置传递,不提供参数名
            参数一般指向不可变的值, 如def A(a=None) 好于 def A(a=[])
#### 包裹传递
            目的:传入不定个数的参数, 无关键字元组, 有关键字 字典
##### 包裹位置参数
                定义函数
                    def func(*arg_name):封装成元组任意数量的位置参数
                    arg_name 搜集所有参数,根据位置合并成一个元组
                使用函数
                    先组装list/tuple再传入如:a=[1,2,3], def func(*arg_name)
                    直接传入:如, func(1,2,3)
##### 包裹关键字传递
                字典,关键字参数
                定义函数
                    def func(**dict_name)
                    搜集所有参数,任意数量封装成字典,允许参数缺失,无序
                使用函数
                    先组装dict再传入,如: **dict, a = {k:v1,k2:v2}, func(**a)
                    直接传入多个key, 如: func(a=1,b=2)


##### 解包裹
                目的:
                    如:func(*args,**kw)
                    1, 先拆解 *args,按顺序传入:必选参数-->默认参数-->可变参数(若还有价值)
                    2, 再拆解 **kw, 关键字参数传递
                    3, 单个参数,用来拆解list/tuple & dict
    
                方法:
                    对应一个位置参数,tuple的每一个元素 func(*args)
                    作为关键字参数, 字典的每个键值对
                    包裹,定义函数时使用,
                    解包裹,调用函数时使用,两者相对独立


###  4 处理参数传递的二种形式
#### 值传递
            参数: 基本数据类型
            含义: 复制一个新变量,不影响原有变量, 函数在内存中,变量传递给函数后,指向新的引用对象
#### 指针传递
            参数: list
            含义: 变量传递给函数的是指针,指针指向序列在内存中的位置
                  函数对list的操作,在原有内存中进行,从而影响原有变量
            原引用对象被改变
      其他说法：
          位置传参，默认参数，关键字参数，可变参数
#### 可先检查参数类型  isinstance()
##   5 函数式编程,
###  释义
        编程范式,如何编写程序的方法论, 面向对象, 面向过程
        属于结构化编程,采用子程序,程序代码区块, for/while 循环
### 思想:
        尽量把运算过程写成一系列嵌套的函数调用
### 特点:
        允许把函数作为参数,传入另一个函数,返回一个函数
### 6 Python并非函数式编程语言
        Python支持一些函数式
            匿名函数, BIF,偏函数
        本质是通过封装对象实现函数式编程
## 7 匿名函数 lambda, 
    不需要专门声明
    表达式: lambda[arg1[arg2...argN]]:expression
    同一行:
        定义体 + 声明,不用写return
    使用:
        方式一: 
           赋值给变量: func = lambda x,y:x + y
               含义: lambda生成一个函数对象,参数为x, y返回值为x + y, 函数对象赋给func
           使用变量来调用该函数 func(3,4),  func的调用与正常函数无异
        方式二:
           作为返回值返回:
               def build(x,y):
                   return lambda:x*x + y*y
## 8 高阶函数 ,
    定义:一个函数接收另一个函数作为[参数], 与lambda完美匹配
    fliter()
        将函数对象依次作用与每一个元素,根据返回值True/False 决定保留/丢弃元素
        filter(lambda:n:n%2, allNum)
        等同与列表解析 [n for n in allNums if n%2]
    map()
        map(func, seq1,[..seqN])
            将函数对象,依次作用于每一个元素
            每次作用结果存储在返回在list中
            可有N个列表,对应函数的N个参数
        map(lambda x:x**2, xrange(5)) 等价于 [x**2 for x in range(5)]
        map(lambda x, y:x + y, [1,2,3],[6,7,9])  等价于[7,9,12]
        map(None, [1,3,5].[2,4,6])  等价于[(1,2), (3,4), (5,6)], 类似思想:zip()
    reduce() 返回一个值
        导入: from functools import reduce
        reduce(func, seq(, init)) #累加作用
            func 二元函数,接收两个参数, 前一次的返回值和运算结果,
                序列下一个元素
            init 初始化器
                若设定 第一次计算为 初始化器&第一个序列元素
        e.g.  reduce(lambda x,y:x+y, range(5)), 等价于 (((0+1)+2)+3)+4
            reduce(func, [1,2,3]), 等价于 func(func(1,2), 3)
## 9 偏函数 ,
### 概述:
           偏函数应用 PFA, Partital funcation application
           结合, 函数式编程,默认参数,可变参数
           固定某些参数的默认值,返回一个新的函数以供调用

### 应用:
           场景: 
               1, 函数参数太多,需要简化
               2, 创建一个新的函数,以固化源函数的部分参数
           模块:
               functools
           创建:
               from functools import partial
               partial(func, *args, **kw)
               固定位置参数:
                   # add1(x) == add(1, x)
                   add1 = partial(add, 1)
                   # int new == int(2, base)
                   int_new = partial(int, 2)
               重设关键字参数
                   int2 = partial(int, base=2)
                   18, int2("10010")
                   int2("10010", base = 10)
## 10 变量作用域,
        标识符的作用域: 在程序里声明可应用范围
### 全局变量
        函数内定义,函数内可引用, 除非声明 global; 
        ** 除非删除,否则存活到脚本允许结束
### 局部变量
        函数定义,只允许函数内访问
        存在与否取决与函数的运行状态
### 变量搜索顺序
        局部作用域-->全局  由小到大的顺序
        全局变量可能 隐藏局部变量

## 11 返回回调函数,
### 特点:
        函数作为结果值返回
        即使参数每次不一样, 每次调用都会返回一个新的函数, 互不影响调用结果
### 闭包程序结构:
        内部函数 引用外部函数的参数和局部变量
        调用外部函数, 返回已保存[参数和变量]的内部函数
### 重点:
        返回闭包时,返回函数不要引用 任何循环变量 或 后续会发生变化的变量.
        如果一定要引用 循环变量, 再创建一个函数,用该函数的参数绑定循环变量当前的值
## 12 递归函数,
### 函数内部调用[自身]
### 防止[栈溢出]
        原因: 每进入函数调用, 栈就会加一层栈帧
             每当函数返回时,栈就会减一层栈帧
             递归调用次数过多会导致栈溢出
        解决:
            尾递归优化, 本质上效果等同与循环
            函数返回时,调用自身本身
            return 不能包含表达式
            缺陷
                若没有针对尾递归继续做优化,任何递归调用函数都存在[栈溢出]的问题 



# 13 矩阵的处理

### 13.1 列表推导式

```python
#　有矩阵如下
>>>ｍatrix = [
  [1,2,3,4],
  [5,6,7,8],
  [9,10,11,12]
]
# 将行与列调换
>>> [[row[i] for row in matrix] for i in range(4)]
[[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]

# 等同于zip
>>>list(zip(*matrix))
[(1, 5, 9), (2, 6, 10), (3, 7, 11), (4, 8, 12)]

```




