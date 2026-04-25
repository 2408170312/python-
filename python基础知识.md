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
1.调用方式                  生成的序列	                               示例输出
range(stop)	                0, 1, ..., stop-1	                        range(3) → 0, 1, 2
range(start, stop)	         start, start+1, ..., stop-1	             range(2, 5) → 2, 3, 4
range(start, stop, step)	   按步长递增	                             range(1, 10, 2) → 1, 3, 5, 7, 9

四.callable函数
callable() 是 Python 的一个内置函数，用于判断一个对象是否可以被调用（即是否可以像函数一样使用 () 来执行）。
# 函数是可调用的
def my_func():    #def 是 Python 中用来定义函数的关键字。它是英文 define（定义）的缩写
    pass      #先占个位置，以后再来实现
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

# 定义了 __call__ 方法的对象是可调用的
class Greeting:
    def __call__(self):
        print("Hello!")

obj = Greeting()
print(callable(obj))        # True
obj()                       # 输出：Hello!
