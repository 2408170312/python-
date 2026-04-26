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
不可变数据（4 个）：Number（数字）、String（字符串）、bool（布尔）、Tuple（元组）  #一旦创建，对象的值就不能被改变
可变数据（3 个）：List（列表）、Dictionary（字典）、Set（集合）
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





