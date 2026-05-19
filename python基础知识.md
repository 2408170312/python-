一、两种循环（for,while）：
1).
i=0
while i<5:
          print("hello")   #这里用hello用单双引号均可，不会区分。且输出时用print,而不是printf.
          i+=1     
2).
for j in range(5):          #range为内置类，但可以当作函数调动，因此又称为内置函数。      range(stop)——>	0, 1, ..., stop-1   例如：range(3) → 0, 1, 2；	range(2, 5) → 2, 3, 4；
           print("hi")
注意：python是一门通过缩进来判断码块的语言，而在其他语言（如 C、Java）中，通常用花括号 {} 来划分代码块。因此，在1)中，变量i一定要与while对齐且一定要顶格写，因为Python 要求模块级别（不在任何函数、类、循环、条件内部的代码）的代码必须顶格写，不能有前导空格。


二、标识符【第一个字符必须为字母(包括中文、希腊字母等 Unicode 字符)或下划线_】
1.标识符的其他的部分由字母、数字和下划线组成。
2.姓名 = "张三" 与 π = 3.14159   # 合法
3.MAX_SIZE = 1024    # 全大写通常表示“常量”（固定不变的值）。虽然你改动也不会报错就是了，因为全大写为常量只是一种命名约定，告诉程序员“这个值不应该改”，但 Python 语法上没有任何机制禁止你修改它
4.禁止使用保留关键字，如 if、for、class 等不能作为标识符    
5.import keyword
  print(keyword.kwlist)  #输出当前版本的所有关键字，其中kwlist是keyword 模块中的一个列表（list）属性，即print(type(keyword.kwlist))  输出： <class 'list'>
6. 1)keyword.iskeyword()	函数	keyword.iskeyword("if")	判断某个词是否是关键字.例如：print(keyword.iskeyword("if")) ，输出True
    2)keyword.kwlist也可以判断关键字， 例如：print("if" in keyword.kwlist)   ，输出：False
    3)for kw in keyword.kwlist:
                       print(kw)     #输出所有关键字
    4）print(len(keyword.kwlist))   # 35（Python 3.x 有 35 个关键字）
 
   


三.内置类range
1.调用方式                             生成的序列	                       示例输出
range(stop)	                0, 1, ..., stop-1	             range(3) → 0, 1, 2
range(start, stop)	         start, start+1, ..., stop-1	             range(2, 5) → 2, 3, 4
range(start, stop, step)	          按步长递增	                       range(1, 10, 2) → 1, 3, 5, 7, 9

四.callable函数
callable() 是 Python 的一个内置函数，用于判断一个对象是否可以被调用（即是否可以像函数一样使用 () 来执行）。
# 函数是可调用的
def my_func():----#def 是 Python 中用来定义函数的关键字。它是英文 define（定义）的缩写
    pass----------#先占个位置，以后再来实现；即此处定义了一个空函数
print(callable(my_func))    # True

# 类是可调用的（调用类 = 创建实例）
class MyClass:
    pass     #先定义一个空类

print(callable(MyClass))    # True

# 内置函数是可调用的
print(callable(print))      # True
print(callable(len))        # True

# 列表不可调用
my_list = [1, 2, 3]
print(callable(my_list))    # False

# 整数不可调用
num = 10
print(callable(num))        # False

# 字符串不可调用
name = "Alice"
print(callable(name))       # False

# 定义了 __call__ 方法的对象是可调用的       #当你定义一个类时，如果在这个类里面写了 __call__ 方法，那么这个类创建出来的对象就可以像函数一样被调用
class Greeting:
    def __call__(self):      #__call__ 是定义在类内部的函数，这种函数专门叫做方法（method）。self 是一个约定俗成的参数名，类似形参（其实可以叫任何名字，但大家都用 self），它代表调用这个方法的对象本身。
        print("Hello!")
obj = Greeting()            #obj 现在是一个 Greeting 实例
print(callable(obj))        # True，如果没定义_call_方法，这里就是False
obj()                       # 像函数一样调用 obj,Python 在执行时，会自动转换成：Greeting.__call__(obj)， obj 作为 self 传进去；输出：Hello!




五、__call__ 的常见用途     #注意：这里的call前后各两个下划线
1. 带状态的“函数”（记住上次调用的值）
class Counter:
    def __init__(self):
        self.count = 0     #这里创建了 count，把count 这个属性添加到对象上，并赋值为 0
    def __call__(self):
        self.count += 1
        return self.count
c = Counter()
print(c())   # 1
print(c())   # 2
print(c())   # 3

 2. 装饰器（高阶函数）
class MyDecorator:
    def __init__(self, func):
        self.func = func    #这里等号右边的func就是say_hello，say_hello保存在了self.func 中；
    def __call__(self, *args, **kwargs):
        print("调用函数前")
        result = self.func(*args, **kwargs)  #执行 self.func()，即原函数 say_hello()，会输出hello
        print("调用函数后")
        return result

@MyDecorator       # 是 Python 中的装饰器语法糖，它可以让你在不修改函数本身的情况下，给函数添加额外的功能。相当于‘say_hello = MyDecorator(say_hello)【装饰器替换原函数,将say_hello传给func】’
def say_hello():
    print("Hello!")
say_hello()       #调用替换后的对象，调用的是装饰器返回的 MyDecorator 对象，因此会触发 __call__ 方法。
# 输出：
# 调用函数前
# Hello!
# 调用函数后
注意：say_hello = MyDecorator(say_hello)中的MyDecorator(say_hello)创建 MyDecorator 类的一个实例对象，把 say_hello 作为参数传给 __init__


六、可变位置参数（*）和可变关键字参数（**）
1.def demo(*args):
    print(args)      # args 是一个元组
    print(type(args))
demo(1, 2, 3, "hello")
输出：
(1, 2, 3, 'hello')
<class 'tuple'>
*args 把传入的所有“普通参数”收集成一个元组，名字 args 可以任意，但 * 是语法关键


2.**kwargs 的作用
def demo(**kwargs):
    print(kwargs)    # kwargs 是一个字典
    print(type(kwargs))
demo(a=1, b=2, name="Alice")
输出：
{'a': 1, 'b': 2, 'name': 'Alice'}
<class 'dict'>
**kwargs 把传入的所有“关键字参数”收集成一个字典，**kwargs 只能接收关键字参数（key=value 形式），如果是demo(1,2,"Alice"),则会报错



七、exec()内置函数 【exec() 是 Python 的内置函数，可以把字符串当作 Python 代码来执行。例如：exec("print('Hello')")   # 输出：Hello】
1.
def is_valid_identifier(name):
    try:                         # 尝试执行代码块
        exec(f"{name} = None")   #来检验变量名是否合法。f 是 f-string（格式化字符串字面量）的标记，作用是在字符串中嵌入变量.
        return True
    except:                       #捕获异常
        return False

print(is_valid_identifier("2var"))  # False,因为变量不能以数字打头
print(is_valid_identifier("var2"))  # True




七、多行语句
1.Python 通常是一行写完一条语句，但如果语句很长，我们可以使用反斜杠 \ 来实现多行语句，例如：
total = item_one + \        #这里不是转义的意思
        item_two + \
        item_three
实例
item_one = 1
item_two = 2
item_three = 3
total = item_one + \
        item_two + \
        item_three
print(total) # 输出为 6

2.在 [], {}, 或 () 中的多行语句，不需要使用反斜杠 \，例如：
item_one = 1
item_two = 2
item_three = 3
total = [item_one +
          item_two +
        item_three]
print(total) # 输出为[6]


八、数字(Number)类型（它包含在数据类型之中）
1.python中数字有四种类型：整数（int）、布尔型(bool)、浮点数(flot)和复数(complex)。
int (整数), 如 1, 只有一种整数类型 int，表示为长整型，没有 python2 中的 Long。
bool (布尔), 如 True。
float (浮点数), 如 1.23、3E-2
complex (复数) - 复数由实部和虚部组成，形式为 a + bj[也可表示为complex(a,b)]，其中 a 是实部，b 是虚部，j 表示虚数单位。如 1 + 2j、 1.1 + 2.2j，复数的实部 a 和虚部 b 都是浮点型。

2.其中bool 是 int 的子类： issubclass(bool, int)     #True



九、字符串
str='123456789'
print(str)                 # 输出字符串
print(str[0:-1])           # 输出第一个到倒数第二个的所有字符
print(str[0])              # 输出字符串第一个字符
print(str[2:5])               # 输出从第三个开始到第六个的字符（不包含）
print(str[2:])             # 输出从第三个开始后的所有字符
print(str[1:5:2])          # 输出从第二个开始到第五个且每隔一个的字符（步长为2）
print(str * 2)             # 输出字符串两次
print(str + '你好')         # 连接字符串
print('------------------------------')
print('hello\nrunoob')      # 使用反斜杠(\)+n转义特殊字符
print(r'hello\nrunoob')     # 在字符串前面添加一个 r，表示原始字符串，不会发生转义

输出结果：
123456789
12345678
1
345
3456789
24
123456789123456789
123456789你好
------------------------------
hello
runoob
hello\nrunoob



十、按键退出
1.等待用户输入，执行下面的程序在按回车键后就会等待用户输入：
input("\n\n按下 enter 键后退出。")    #你这里无论打什么键（例如tab键），都只能按enter键才能退出
以上代码中 ，\n\n 在结果输出前会输出两个新的空行。一旦用户按下 enter 键时，程序将退出。

2.按tab键退出
import msvcrt    # msvcrt 是 Windows 专属的。如果你的代码需要在 macOS 或 Linux 上运行，应该使用 getpass 等跨平台模块来替代
print("请按 Tab 键继续...")
while True:
    if msvcrt.kbhit() and ord(msvcrt.getch()) == 9:  # 9 是 Tab 的 ASCII 码。其中getch() 和 getche() 返回的是字节串（如 b'a'），而不是字符串 
        break

3.任意键退出
import msvcrt   # Windows only
print("按任意键退出...")
msvcrt.getch()   # 阻塞等待一个按键（不显示输入）
                 #是 Windows 下的底层键盘读取

4.按特定字母键退出（最常见、最简单）
用户按 q 或 Q 退出。
while True:
    key = input("输入 q 退出: ")   #在屏幕上显示提示文字“输入 q 退出: ”，然后等待用户输入一串字符，用户按下回车键后，把用户输入的内容赋值给变量 key。
    if key.lower() == 'q':     #把字符串里所有大写字母变成小写，看是否为q
        print("退出")
        break
仍然用 input()，程序会在用户按回车后判断内容
虽不能做到“按下即退出”，但逻辑简单稳定



十一、print输出
print 默认输出是换行的，如果要实现不换行需要在变量末尾加上 end=""：
x="a"
y="b"
# 换行输出
print( x )
print( y )
 
print('---------')
# 不换行输出
print( x, end=" " )
print( y, end=" " )
print()



十二、import 与 from...import
在 python 用 import 或者 from...import 来导入相应的模块。
将整个模块(somemodule)导入，格式为： import somemodule
从某个模块中导入某个函数,格式为： from somemodule import somefunction
从某个模块中导入多个函数,格式为： from somemodule import firstfunc, secondfunc, thirdfunc
将某个模块中的全部函数导入，格式为： from somemodule import *  （可能导致命名冲突，降低代码可读性，不推荐使用。）

导入 sys 模块
import sys
print('================Python import mode==========================')
print ('命令行参数为:')
for i in sys.argv:
    print (i)
print ('\n python 路径为',sys.path)
导入 sys 模块的 argv,path 成员
from sys import argv,path  #  导入特定的成员
 
print('================python from import===================================')
print('path:',path) # 因为已经导入path成员，所以此处引用时不需要加sys.path






十三、基本数据类型
python3 中有 6 种标准数据类型，以及 bool 布尔类型（bool 是 int 的子类，有时单独列出）：
Number（数字）
String（字符串）
bool（布尔类型）
List（列表）
Tuple（元组）
Set（集合）
Dictionary（字典）

按是否可变，可以分为以下两类：    
1）不可变数据（4 个）：Number（数字）、String（字符串）、bool（布尔）、Tuple（元组）  #一旦创建，对象的值就不能被改变
2）可变数据（3 个）：List（列表）、Dictionary（字典）、Set（集合）
1.多个变量赋值
Python 允许你同时为多个变量赋值。例如：
a = b = c = 1
以上实例创建一个整型对象，值为 1，三个变量被赋予相同的数值。

你也可以为多个变量同时指定不同的值。例如：
a, b, c = 1, 2, "runoob"

2.查询变量所指的对象类型（type和isinstance）
isinstance 和 type 的区别在于：
type() 不会认为子类是一种父类类型。
isinstance() 会认为子类是一种父类类型。
>>> class A:
...     pass
>>> class B(A):
...     pass
>>> isinstance(A(), A)
True
>>> type(A()) == A
True
>>> isinstance(B(), A)
True
>>> type(B()) == A
False


3.数值运算
 2 / 4   # 除法，得到一个浮点数
0.5
 2 // 4  # 整除，得到一个整数
0
 17 % 3  # 取余
2
 2 ** 5  # 乘方
32

4.进制数表示
Python 3 中整数字面量不允许前导零（如 080），二进制使用 0b 前缀 ，八进制数必须使用 0o 前缀（如 0o17），十六进制使用 0x 前缀（如 0x69）。


5.列表（可变）   #写在[]里，其中的元素类型可以不同
1）.[9, 2, 13, 14, 15, 6]
>>> a[2:5] = []   # 将对应的元素值设置为空列表，即删除这些元素
>>> a
[9, 2, 6]


2）.列表的应用：翻转字符串中的单词顺序
def reverseWords(input):
    # 通过空格将字符串分隔，把各个单词分隔为列表
    inputWords = input.split(" ")     #split() 是 Python 字符串的固定方法，只对字符串作用
                                                          #字符串.split()	            按任意空白符（空格、换行、制表符）分割
                                                          # 字符串.split(" ")	            严格按单个空格分割
                                                          #字符串.split(",")	            按逗号分割
                                                          #字符串.split("|")	            按竖线分割
    # inputWords[-1::-1] 三个参数说明：
    # 第一个参数 -1 表示从最后一个元素开始
    # 第二个参数为空，表示移动到列表开头
    # 第三个参数 -1 表示逆向步进（每次向左移动一个位置）
    inputWords = inputWords[-1::-1]

    # 重新用空格拼接单词
    output = ' '.join(inputWords)     #join(inputWords)，字符串的方法，将 inputWords 列表中的元素用分隔符拼接起来，重新组成字符串
                                                                     #print(' '.join(words))      I like runoob（空格分隔）
                                                                      #print(''.join(words))      Ilike runoob（无分隔符）
                                                                      #print(','.join(words))     I,like,runoob（逗号分隔）
                                                                      #print('---'.join(words))   I---like---runoob（三个横线分隔）
    return output

if __name__ == "__main__":
    input = 'I like runoob'
    rw = reverseWords(input)
    print(rw)


3）if __name__ == "__main__"的作用
1））.if __name__ == "__main__" 就像一个“开关”。当你是直接运行这个 .py 文件时，__name__ 这个变量会被自动赋值为 "__main__"，开关打开，其下的代码块就会被执行。
如果这个文件是被其它程序通过 import 导入的，那么 __name__ 的值就是它自己的模块名（即文件名，不含 .py），不等于 "__main__"，所以其下的代码块就不会执行
2））.假设我们有一个文件叫 my_math.py，内容如下：
# 文件名: my_math.py
def add(a, b):
    return a + b
print(f"my_math.py 的 __name__ 现在是: {__name__}")

if __name__ == "__main__":
    print("这是直接运行的 my_math.py 文件，正在执行测试代码...")
    print(f"2 + 3 = {add(2, 3)}")
场景一：直接运行 my_math.py
在命令行执行 python my_math.py，你会看到：

my_math.py 的 __name__ 现在是: __main__
这是直接运行的 my_math.py 文件，正在执行测试代码...
2 + 3 = 5

场景二：作为模块导入
现在创建另一个文件叫 main.py，内容如下：
# 文件名: main.py
import my_math
print(f"main.py 的 __name__ 是: {__name__}")
print(f"1 + 1 = {my_math.add(1, 1)}")
在命令行执行 python main.py，你会看到：

text
my_math.py 的 __name__ 现在是: my_math   # 注意，这里不是 __main__
main.py 的 __name__ 是: __main__
1 + 1 = 2
对比发现：当 my_math 被导入时，由于它的 __name__ 变成了 "my_math"，不等于 "__main__"，所以它内部的测试代码没有被执行，只提供了 add 函数给 main.py 使用。

💎 总结：什么时候用它？
当你的 .py 文件希望既能作为独立工具直接运行，又能作为函数库被别的程序调用时，尤其推荐使用这个写法来组织代码。

在写单元测试或模块自带的示例时，将测试代码放在 if __name__ == "__main__": 下，是 Python 社区的通用做法。



6.元组（Tuple） #写在小括号 () 里，其中的元素类型可以不同，但其中的元素不能修改。
1）构造包含 0 个或 1 个元素的元组比较特殊，有一些额外的语法规则：
tup1 = ()    # 空元组
tup2 = (20,) # 一个元素，需要在元素后添加逗号，以区分它是元组而不是普通的值。因为在没有逗号的情况下，Python 会将括号解释为数学运算中的括号：




7.Set(集合)
1）Python 中的集合（Set）是一种无序、可变的数据类型，用于存储唯一的元素。集合中的元素不会重复，并且可以进行交集、并集、差集等常见的集合操作。
2）在 Python 中，集合使用大括号 {} 表示，元素之间用逗号 , 分隔。也可以使用 set() 函数创建集合。
 注意：创建一个空集合必须用 set() 而不是 {}，因为 {} 创建的是一个空字典。
 创建格式：
 parame = {value01, value02, ...}
 或者
 set(value)
 3）set的应用
sites = {'Google', 'Taobao', 'Runoob', 'Facebook', 'Zhihu', 'Baidu'}
print(sites)   # 输出集合（无序，重复元素会被自动去掉）
# 成员测试
if 'Runoob' in sites:
    print('Runoob 在集合中')
else:
    print('Runoob 不在集合中')

# set 可以进行集合运算
a = set('abracadabra')
b = set('alacazam')

print(a)           # 输出a 中的唯一字符。 结果不唯一，因为集合是无序的

print(a - b)       # a 和 b 的差集（在 a 中但不在 b 中）
print(a | b)       # a 和 b 的并集（在 a 或 b 中）
print(a & b)       # a 和 b 的交集（同时在 a 和 b 中）
print(a ^ b)       # a 和 b 的对称差集（在 a 或 b 中，但不同时存在）

4)添加元素
语法格式如下：
s.add( x )
将元素 x 添加到集合 s 中，如果元素已存在，则不进行任何操作。
实例:
thisset = set(("Google", "Runoob", "Taobao"))  #不能是列表，字典，集合(不可哈希)
thisset.add("Facebook")
print(thisset)
{'Taobao', 'Facebook', 'Google', 'Runoob'}

还有一个方法，也可以添加元素，且参数可以是列表，元组，字典等，语法格式如下：
s.update( x )
x 可以有多个，用逗号分开。
实例:
thisset = set(("Google", "Runoob", "Taobao"))
thisset.update({1,3})
print(thisset)
{1, 3, 'Google', 'Taobao', 'Runoob'}
thisset.update([1,4],[5,6])  
print(thisset)
{1, 3, 4, 5, 6, 'Google', 'Taobao', 'Runoob'}




8.Dictionary（字典）
1）字典是一种映射类型（一种通过“键”来查找“值”的数据结构，就像查字典一样，给定一个输入（键），可以唯一确定一个输出（值）），用 {} 标识，它是一个 键(key) : 值(value) 的集合。键(key) 必须使用不可变类型，且在同一个字典中键必须是唯一的。

2）
my_dict = {}
my_dict['one'] = "1 - 菜鸟教程"
my_dict[2]     = "2 - 菜鸟工具"

tinydict = {'name': 'runoob', 'code': 1, 'site': 'www.runoob.com'}

print(my_dict['one'])       # 输出键为 'one' 的值
print(my_dict[2])           # 输出键为 2 的值
print(tinydict)             # 输出完整的字典
print(tinydict.keys())      # 输出所有键
print(tinydict.values())    # 输出所有值

以上实例输出结果：
1 - 菜鸟教程
2 - 菜鸟工具
{'name': 'runoob', 'code': 1, 'site': 'www.runoob.com'}
dict_keys(['name', 'code', 'site'])             #这不是列表，是“视图对象”。优点：优点：不占用额外内存，可以直接遍历，可以转换为列表list(d.keys())
dict_values(['runoob', 1, 'www.runoob.com'])     #keys()、values() 是字典类型的内置函数


3）字典的键可以是整数、字符串、元组，但不能是列表 / 字典/集合，否则会报错
例如：my_dict[[1, 2]] = "错误"   # 列表不能作为键

4）Python 中创建字典的三种不同方式
      1）） dict([('Runoob', 1), ('Google', 2), ('Taobao', 3)])
         {'Runoob': 1, 'Google': 2, 'Taobao': 3}

      2）） {x: x**2 for x in (2, 4, 6)}
         {2: 4, 4: 16, 6: 36}

      3）） dict(Runoob=1, Google=2, Taobao=3)
         {'Runoob': 1, 'Google': 2, 'Taobao': 3}




十四.字符串
1.
import time                      #导入 time 模块，用于后续的 time.sleep() 制造延迟效果。
for i in range(101):             # 添加进度条图形和百分比,i 从 0 到 100
    bar = '[' + '=' * (i // 2) + ' ' * (50 - i // 2) + ']'  
    print(f"\r{bar} {i:3}%", end='', flush=True)
    time.sleep(0.05)             #暂停 0.05 秒（50 毫秒），让进度条以肉眼可见的速度“填充”，产生动画效果。
print()                          #print() 默认会输出一个换行符 \n，光标移动到下一行开头，命令提示符出现在新的一行，不影响进度条显示

1）这里print()里的 f 是 f-string（格式化字符串字面量）的标志，是 Python 3.6 引入的一种字符串格式化语法。f 告诉 Python：这个字符串里面有大括号 {}，大括号里的内容需要当成表达式来计算，然后把计算结果填进去。
没有 f（普通字符串）
python
bar = "[=====]"
i = 50
print("\r{bar} {i:3}%", end='')

输出：
{bar} {i:3}%
大括号被当作普通字符原样输出，不会替换成变量值。

2){i:3}	#把变量 i 的值填进来，并且占 3 个字符宽度（右对齐）
3)end=''：默认 print 会在末尾加换行符 \n，这里设为空字符串，不换行。
4)flush=True：强制立即输出，不等待缓冲区满，保证实时显示。



2.进制转换
decimal_number = 42
binary_number = bin(decimal_number)  # 十进制转换为二进制，bin()
print('转换为二进制:', binary_number)  # 转换为二进制: 0b101010

octal_number = oct(decimal_number)  # 十进制转换为八进制 oct()
print('转换为八进制:', octal_number)  # 转换为八进制: 0o52

hexadecimal_number = hex(decimal_number)  # 十进制转换为十六进制 hex()
print('转换为十六进制:', hexadecimal_number) # 转换为十六进制: 0x2a


3.原始字符串(r/R 所有的字符串都是直接按照字面的意思来使用，没有转义特殊或不能打印的字符[即某种意义上的失效])
 1)print(r"Hello\nWorld")
Hello\nWorld

2)print("Hello\nWorld")
Hello
World


4.格式字符串(%)
print ("我叫 %s 今年 %d 岁!" % ('小明', 10))   
%s	         占位符，表示这里要放一个字符串
%d	         占位符，表示这里要放一个整数
%	         格式化运算符，把右边的值填入左边的占位符
('小明', 10)	一个元组，按顺序提供要填入的值



5.f-string
f-string 是 python3.6 之后版本添加的，称之为字面量格式化字符串，是新的格式化字符串的语法。

之前我们习惯用百分号 (%):

实例
>>> name = 'Runoob'
>>> 'Hello %s' % name
'Hello Runoob'
f-string 格式化字符串以 f 开头，后面跟着字符串，字符串中的表达式用大括号 {} 包起来，它会将变量或表达式计算后的值替换进去，实例如下：

实例
>>> name = 'Runoob'
>>> f'Hello {name}'  # 替换变量
'Hello Runoob'
>>> f'{1+2}'         # 使用表达式
'3'

>>> w = {'name': 'Runoob', 'url': 'www.runoob.com'}
>>> f'{w["name"]}: {w["url"]}'
'Runoob: www.runoob.com'
用了这种方式明显更简单了，不用再去判断使用 %s，还是 %d。

在 Python 3.8 的版本中可以使用 = 符号来拼接运算表达式与结果：
实例
>>> x = 1
>>> print(f'{x+1}')   # Python 3.6
2

>>> x = 1
>>> print(f'{x+1=}')   # Python 3.8
x+1=2





十五.哈希
哈希（Hash）详解
哈希是一种将任意大小的数据（如字符串、数字、对象）映射为固定大小值（哈希值）的算法。可以理解为给数据生成一个"数字指纹"。

1、通俗理解
类比1：学生证号
# 每个学生信息 → 唯一的学号
"张三, 18岁, 北京" → 2024001  # 哈希值
"李四, 19岁, 上海" → 2024002  # 哈希值
类比2：指纹识别
# 每个人 → 唯一的指纹
"张三" → 指纹: 3f2a1b  # 哈希值
"李四" → 指纹: 5c8d2e  # 哈希值


2、Python中的哈希
# 使用 hash() 函数获取哈希值
print(hash("hello"))      # 输出: 2763187496943622071
print(hash("world"))      # 输出: 507847867688263415
print(hash(42))           # 输出: 42
print(hash(3.14))         # 输出: 322818021289917443
print(hash((1,2,3)))      # 输出: 529344067295497451

# 相同内容产生相同哈希值
print(hash("hello"))      # 2763187496943622071
print(hash("hello"))      # 2763187496943622071 （相同）


3、哈希的特性
1). 确定性
python
# 相同输入 → 相同输出
print(hash("Python"))  # 总是相同的结果
2). 快速计算
python
# 无论字符串多长，计算都非常快
hash("a")          # 立即返回
hash("a" * 10000)  # 也是立即返回
3). 不可逆
python
# 从哈希值无法反推出原始数据
# 已知哈希值 2763187496943622071
# 无法知道原始数据是 "hello"


4、哈希在Python中的作用
1). 字典（dict）的键查找
# 字典使用哈希表实现
my_dict = {
    "apple": 1,    # "apple" 的哈希值 → 存储位置
    "banana": 2,   # "banana" 的哈希值 → 存储位置
    "orange": 3    # "orange" 的哈希值 → 存储位置
}

# 查找过程
print(my_dict["apple"])  # 1. 计算"apple"的哈希值
                         # 2. 直接跳到存储位置
                         # 3. O(1)时间复杂度
2). 集合（set）的元素去重
python
# 集合使用哈希值判断元素是否重复
my_set = {1, 2, 3, 1, 2, 3}  # 重复的会被自动去重
print(my_set)  # {1, 2, 3}

# 判断原理：
# 1. 计算1的哈希值
# 2. 检查是否已存在相同哈希值的元素
# 3. 存在则认为是重复，不添加


5、为什么可变对象不可哈希？
# 列表是可变的
my_list = [1, 2, 3]
print(hash(my_list))  # ❌ TypeError

# 原因：哈希值会变化
my_list = [1, 2, 3]
hash1 = hash(tuple(my_list))  # 假设可哈希

my_list.append(4)  # 列表内容改变了
hash2 = hash(tuple(my_list))  # 哈希值会不同

# 如果列表作为字典的键
d = {my_list: "value"}  # 假设允许
my_list.append(4)  # 修改了键
# 字典就无法找到原来存储的值了！


6、哈希的实际应用
1). 密码存储
import hashlib                        #导入Python内置的哈希算法库，里面包含各种哈希算法
password = "my_password123"           #创建一个字符串变量存储原始密码
hash_value = hashlib.sha256(password.encode()).hexdigest()  #password.encode()：字符串编码为字节(字节数据 b'my_password123')。哈希算法只认字节，不认字符串。hashlib.sha256()---创建SHA-256哈希计算器。   hashlib.sha256().hexdigest()---返回十六进制字符串
print(hash_value)
# 登录时：输入密码 → 计算哈希 → 对比存储的哈希

2). 文件完整性校验
import hashlib
def get_file_hash(filename):             #定义一个函数，用于计算文件的哈希值. filename 是文件路径，即要打开的文件
    with open(filename, 'rb') as f:       #以二进制只读模式打开文件。rb=read binary(二进制读模式)。with	是上下文管理器，可以自动关闭文件，无需手动f.close()
        return hashlib.md5(f.read()).hexdigest()   #读取文件全部内容，计算MD5哈希值，返回十六进制字符串
official_hash = "5d41402abc4b2a76b9719d911017c592"  #官方提供的哈希值
downloaded_hash = get_file_hash("downloaded_file.zip")  #下载后计算
if official_hash == downloaded_hash:
    print("文件完整，未被篡改")
    
3). 快速查找（如您系统中的车辆ID）
# 车辆追踪使用哈希快速查找
tracked_vehicles = {}  # 字典
def update_vehicle_position(vehicle_id, position):
    # vehicle_id 的哈希值用于快速定位车辆
    tracked_vehicles[vehicle_id] = position  # O(1) 时间

    
7、哈希表原理图
字典内部结构（简化版）：
哈希值（索引）        存储的数据
   0      →          [None]
   1      →          ["apple": 1]
   2      →          [None]
   3      →          ["banana": 2]
   4      →          [None]
   5      →          ["orange": 3]
   
查找 "banana":
1). 计算 hash("banana") = 3
2). 直接访问索引3
3). 找到数据！ O(1)时间复杂度


8、关键总结
概念	说明
哈希	将数据转为固定长度的数字指纹
哈希值	计算出来的数字结果
可哈希	对象可以计算哈希值（不可变对象）
不可哈希	对象不能计算哈希值（可变对象）
哈希表	使用哈希值快速存储/查找的数据结构
核心记忆点
哈希 = 给数据生成唯一身份证号
相同数据 → 相同身份证号
不同数据 → 不同身份证号（理想情况）
从身份证号无法反推原数据

可变对象为什么不可哈希？
因为对象变了，"身份证号"也会变

字典和集合依赖不变的身份证号来找数据
这就是哈希！它是Python字典和集合能够快速查找的底层原理。

9. 可哈希类型列表
类型	 示例	                  说明
int         42, -5, 0	        整数
float	  3.14, -0.5, 2.0	        浮点数
str	  "hello", 'Python'	        字符串
bool	  True, False	        布尔值
None	  None	                   空值
tuple	  (1, 2, 3), (1, "a")        元组（所有元素可哈希）
frozenset	  frozenset([1,2,3])         不可变集合
bytes	  b'hello'	         字节串
complex	  1+2j	                   复数


10.不可哈希类型列表
类型	         示例	                         说明
list	       [1, 2, 3], ["a", "b"]	              列表（可变）
set	      {1, 2, 3}, {"a", "b"}	              集合（可变）
dict	      {"a": 1}, {"name": "Tom"}	    字典（可变）
bytearray	      bytearray(b'hello')	              可变字节数组
自定义类的实例	MyClass()	                         默认不可哈希






十六:.pop()函数
1.
thisset = set(("Google", "Runoob", "Taobao", "Facebook"))
x = thisset.pop()
print(f"被删除的元素: {x}")
print(f"剩余集合: {thisset}")
关键点：
pop() 删除的是任意一个元素（不确定是哪个）
因为集合是无序的，所以没有"第一个"的说法
实际由哈希值和内部存储顺序决定。且不同进程间：顺序可能改变（哈希随机化）

2、验证示例
每次运行结果可能不同
# 第一次运行
thisset = set(("Google", "Runoob", "Taobao", "Facebook"))
print(thisset.pop())  # 可能是 Facebook

# 第二次运行（程序重启后）
thisset = set(("Google", "Runoob", "Taobao", "Facebook"))
print(thisset.pop())  # 可能是 Google

查看实际结果
thisset = set(("Google", "Runoob", "Taobao", "Facebook"))
print("原始集合:", thisset)
x = thisset.pop()
print("被删除的元素:", x)
print("剩余集合:", thisset)

# 可能输出：
# 原始集合: {'Taobao', 'Google', 'Runoob', 'Facebook'}
# 被删除的元素: Taobao
# 剩余集合: {'Google', 'Runoob', 'Facebook'}

3、为什么看起来随机？
集合内部存储原理
# 集合基于哈希表存储，元素顺序由哈希值决定
for item in {"Google", "Runoob", "Taobao", "Facebook"}:
    print(f"{item:10} 哈希值: {hash(item)}")
# 输出示例：
# Google     哈希值: 1758375238635770947
# Runoob     哈希值: -5595808781650567816
# Taobao     哈希值: 1807485501461404964
# Facebook   哈希值: 4625581223145945371

# 集合内部按哈希值排序存储
# pop() 删除哈希值最小的那个


4、与列表的对比
1） 列表：有序，pop() 删除最后一个（默认）
my_list = ["Google", "Runoob", "Taobao", "Facebook"]
x = my_list.pop()
print(f"列表删除: {x}")  # 总是删除 "Facebook"（最后一个）
print(f"剩余列表: {my_list}")  # ['Google', 'Runoob', 'Taobao']

2） 集合：无序，pop() 删除任意元素
my_set = {"Google", "Runoob", "Taobao", "Facebook"}
x = my_set.pop()
print(f"集合删除: {x}")  # 不确定是哪个
print(f"剩余集合: {my_set}")  # 剩下3个，不确定哪3个


5、多次 pop() 实验
thisset = set(("Google", "Runoob", "Taobao", "Facebook"))
print("原始集合:", thisset)

# 连续 pop 直到空
while thisset:
    print(f"pop 出: {thisset.pop()}")

# 输出示例（顺序不确定）：
# 原始集合: {'Taobao', 'Google', 'Runoob', 'Facebook'}
# pop 出: Taobao
# pop 出: Google
# pop 出: Runoob
# pop 出: Facebook


6、注意事项
空集合不能用 pop()
empty_set = set()
# empty_set.pop()  # ❌ KeyError: 'pop from an empty set'
# 正确做法：先判断
if empty_set:
    x = empty_set.pop()
else:
    print("集合为空，不能 pop")
    
pop() 的行为在 Python 不同版本可能有差异
# Python 3.6+：字典保持插入顺序，但集合仍然无序
# Python 3.7+：字典有序，集合仍不一定有序
# 建议：不要依赖 pop() 的具体顺序


 
7、总结
数据结构	pop() 行为	           顺序
list	删除最后一个（索引-1）	有序，可预测
set	删除任意一个	          无序，不可预测
dict	删除最后插入的（Python 3.7+）	有序，可预测





十七.python中的输入
Python 中的输入主要通过 input() 函数实现，用于从用户获取键盘输入。
1、基本输入
1). 最简单的输入
name = input()
print(f"你好，{name}")
# 运行示例：
# 用户输入: 张三
# 输出: 你好，张三

2). 带提示信息的输入
# 在括号内添加提示文字
name = input("请输入你的名字: ")
print(f"你好，{name}")
# 运行示例：
# 请输入你的名字: 张三
# 你好，张三


2、输入的数据类型（重要！）
input() 返回的永远是字符串
# 示例1：输入数字
age = input("请输入年龄: ")
print(type(age))  # <class 'str'>
print(age)        # "18"（字符串）

# 示例2：输入数字但当作数字运算会出错
age = input("请输入年龄: ")
# next_year = age + 1  # ❌ TypeError！字符串不能加数字
类型转换
# 转换为整数
age = input("请输入年龄: ")
age = int(age)  # 转换为整数
next_year = age + 1  # ✅ 正确
print(f"明年你{next_year}岁")

# 转换为浮点数（小数）
height = input("请输入身高(米): ")
height = float(height)
print(f"身高: {height}米")

# 一行写法
age = int(input("请输入年龄: "))


3、完整示例集合
1)：收集个人信息
# 获取各种类型的数据
name = input("请输入姓名: ")
age = int(input("请输入年龄: "))
height = float(input("请输入身高(米): "))
is_student = input("是否为学生？(是/否): ")

print("\n=== 个人信息 ===")
print(f"姓名: {name}")
print(f"年龄: {age}")
print(f"身高: {height}米")
print(f"学生: {is_student}")
print(f"类型: 姓名={type(name)}, 年龄={type(age)}, 身高={type(height)}")

2)：多输入一行处理
# 方法1：分别输入
x = int(input("请输入x: "))
y = int(input("请输入y: "))
print(f"x+y={x+y}")

# 方法2：一行输入多个值（用空格分隔）
# 输入示例: 10 20
a, b = map(int, input("请输入两个数字（空格分隔）: ").split())  #.split()将字符串按空格分割成列表，返回：['10', '20']。map(int,...) 函数：将 int() 函数应用到列表的每一个元素。相当于：int('10')，int('20') ，将字符串转换成整数(因为input()返回的永远是字符串)
print(f"a+b={a+b}")

# 方法3：输入列表
# 输入示例: 1 2 3 4 5
numbers = list(map(int, input("请输入多个数字（空格分隔）: ").split()))  #list(),将map对象转换成列表
print(f"数字列表: {numbers}")
print(f"总和: {sum(numbers)}")


3)循环输入
numbers = []
while True:
    num = input("请输入数字（按q退出）: ")
    if num.lower() == 'q':
        break
    numbers.append(int(num))
print(f"平均值: {sum(numbers)/len(numbers)}")







十八.条件控制
1.mach...case
1)
def http_error(status):
    match status:
        case 400:
            return "Bad request"
        case 404:
            return "Not found"
        case 418:
            return "I'm a teapot"
        case _:     #case _: 类似于 C 和 Java 中的 default:，当其他 case 都无法匹配时，匹配这条，保证永远会匹配成功。
            return "Something's wrong with the internet"
 
print(http_error(400))
print(http_error(404))
print(http_error(418))
print(http_error(500))


2)一个 case 也可以设置多个匹配条件，条件使用 | 隔开，例如：
...
    case 401|403|404:
        return "Not allowed"



2.if语句
if condition_1:
    statement_block_1
elif condition_2:  #elif就是c语言中的else if
    statement_block_2
else:
    statement_block_3

3.if嵌套
num=int(input("输入一个数字："))
if num%2==0:
    if num%3==0:
        print ("你输入的数字可以整除 2 和 3")
    else:
        print ("你输入的数字可以整除 2，但不能整除 3")
else:
    if num%3==0:
        print ("你输入的数字可以整除 3，但不能整除 2")
    else:
        print  ("你输入的数字不能整除 2 和 3")





十九.循环语句
循环控制关键字与方法
关键字 / 函数	      说明	                                        示例
for	       迭代循环，用于遍历序列或可迭代对象	                           for i in list:
while	          条件循环，条件为 True 时持续执行	                  while x > 0:
break	             立即终止当前循环	                                        break
continue	        跳过本次循环剩余代码，进入下一次迭代	                   continue
else（循环）	循环正常结束（未被 break）时执行	                  for i in range(3): ... else: ...
pass	            循环中的占位语句（空操作）	                            for i in range(5): pass
range()	         生成整数序列，常与 for 循环配合使用	                  range(0, 5)
enumerate()	遍历时同时获取索引和值	                            for i, v in enumerate(list):

1.while循环

n = 100
sum = 0
counter = 1
while counter <= n:
    sum = sum + counter   
    counter += 1
print("1 到 %d 之和为: %d" % (n,sum))    # %是字符串格式化操作符（旧式格式化），用于将变量值插入到字符串的占位符中。

1）更现代的替代方法（Python 3.6+）：
# f-string（推荐）
print(f"1 到 {n} 之和为: {sum}")

# format() 方法
print("1 到 {} 之和为: {}".format(n, sum))


2).while 循环使用 else 语句
count = 0
while count < 5:
   print (count, " 小于 5")
   count = count + 1
else:
   print (count, " 大于或等于 5")

3)for...else
检查质数：
for n in range(2, 10):
    for x in range(2, n):
        if n % x == 0:
            print(n, '等于', x, '*', n//x)
            break
    else:
        # 循环中没有找到元素
        print(n, ' 是质数')
