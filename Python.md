Python



and 和 or 中and具有高优先级

```python
元组只有一个元素时要加 ','
s.[::-1]    # 字符串的反转，即步长-1
```

##  函数

```Python
# 多返回值
def test_return():
    return 1, "hello", True

# 多参数
def func1(name,age,sex):
    pass

# 不定长
def func2(*args):
    # args为元组
    pass
def func3(**kwargs):
    # kwargs为字典
    pass
func3(name="张三"，age=15)

# 函数做参数
def func(function):
    sum = function(1,2)
def add(x,y):
    return x+y
func(add)
# 匿名函数Lambda
func(lambda x, y: x + y)

# 声明全局变量
name = "zhangsan"
def func():
    global name
    name = "lisi"
```

> 类型注解
>
> ~~~python
> name: str = "张三"
> is_boy: bool = True
> 
> l: list = [1, 2, 3]
> t: tuple = (1, "nihao", True)
> s: set = {2, 5, 6, 54}
> d: dict = {"zhangsan": 23, "lisi": 25}
> 
> l2: list[int] = [1, 2, 3]
> t2: tuple[int, str, bool] = (1, "nihao", True)
> s2: set[int] = {2, 5, 6, 54}
> d2: dict[str, int] = {"zhangsan": 23, "lisi": 25}
>     
> def func2(data: list) -> list:
>     # 返回值注解
>     return data
> # union类型
> from typing import Union
> 
> l: list[Union[int, str]] = [1, 2, "nihao", "hello"]
> 
> def func(data: Union[str, int]) -> Union[str, int]:
>     pass
> ~~~
>
> 

## 文件

~~~python
f = open("test.txt", "r", encoding="UTF-8") # 其中r:只读，w：写，a：追加
f.read(10)
f.readLine()
f.close
# with open() as f      可以自动关闭文件
with open("test.txt", "r", encoding="UTF-8") as f:
    ...
# 刷新
f.flush()
~~~

## 异常、模块、包

~~~python
try:
    print(1 / 0)
except Exception as e:
    print("出现异常")
else:
    print("没有异常执行")
finally:
    print("无论有无异常,都会执行")
~~~

> 模块、包 
>
> ~~~python
> [from 模块] import [模块 | 类 | 变量 | 函数 | *] [as 别名] 
> # 通过"."来确定层级关系
> ~~~
> 自定义
>
> ~~~python
> __all__ = ['test']   # 若含有此变量，当以 * 导入时只会导入此变量中函数
> 
> def test(x,y):
>     return x + y
> 
> if __name__ == '__name__':  # 用于函数测试
>     test(1, 2)
> ~~~
>
> 

## 面向对象

> 类和对象

~~~python
class Student:
    name = None
    age = None
    # 构造方法
    def __init__(self,name,age):
        self.name = name
        self.age = age
        
    def play(self):
        print("玩游戏")
        
    # 魔术方法
        def __str__(self):
        return f"Student类对象, name={self.name}, age={self.age}"

    # __lt__   小于或大于的比较
    def __lt__(self, other):
        return self.age < other.age

    # __le__   小于等于或大于等于的比较
    def __le__(self, other):
        return self.age <= other.age

    # __eq__  等于的比较
    def __eq__(self, other):
        return self.age == other.age
    
stu = Student("zhangsan",18)
~~~

> 封装继承多态

~~~python
class Phone:  # 父类：手机
    on = True
    
    def call(self):
        print("打电话")
        
    def use(self):
        if self.on:
        	self.call()
            
class Infrared: # 父类：红外
    def control(self):
        print("遥控")
        
class IPhone(Phone,Infrared):      # 多继承
    def use(self):			# 方法重写
        print("玩游戏")
        
class HUAWEI(Phone): 
    def use(self):
        print("看电影")
def usePhone(phone: Phone):
    phone.use()
iphone = IPhone()
huawei = HUAWEI()
usePhone(iphone) # 玩游戏
usePhone(huawei) # 看电影
~~~

## 多任务

*多进程实现多任务*

> `进程（process）`资源分配的最小单位，一个程序即一个进程
>
> - `多进程间全局变量不共享`
>
> - `主进程会等子进程执行完成后再销毁，可有以下两种方式不让主进程等待子进程`
>
> ~~~python
> 子进程对象.daemon = True   # 设置守护主进程
> ~~~

~~~python
# python 多进程实现多任务
import multiprocessing

def coding():
    for i in range(3):
        print("coding.......")
        time.sleep(10)


def music():
    for i in range(3):
        print("music.......")
        time.sleep(10)


if __name__ == '__main__':
    # args:元组传参 ，kwargs:字典传参，与参数列表一致
    coding_process = multiprocessing.Process(target=coding) 
    music_process = multiprocessing.Process(target=music)
    coding_process.start()
    music_process.start()
~~~

*多线程实现多任务*

> `线程 `程序执行的最小单位，可与同进程的线程共享资源
>
> - `全局变量共享`
>
> - `主线程等待子线程结束后再结束`  ->  `daemon = True` 或 `线程对象.setDaemon(True)`
>
> - 为了线程安全可加互斥锁
>
>   ~~~python
>   mutex = threading.Lock()
>   mutex.acquire()  # 上锁
>   mutex.release()  # 释放锁
>   ~~~
>
>   

~~~python
import threading

def coding():
    for i in range(3):
        print("coding.......")
        time.sleep(10)


def music():
    for i in range(3):
        print("music.......")
        time.sleep(10)


if __name__ == '__main__':
    # args:元组传参 ，  kwargs:字典传参，与参数列表一致   
    # target:任务名 ， daemon = True : 守护主线程
    coding_thread = threading.Thread(target=coding) 
    music_thread = threading.Thread(target=music)
    coding_thread.start()
    music_thread.start()
~~~

## socket 网络编程

> `socket(套接字)` 程序间网络通讯的工具
>
> `TCP (Transmission Control Protocol)(传输控制协议)` 网络传输方式，一种`面向连接的`、`可靠的`、`基于字节流的`传输层通信协议



## 爬虫

### urllib

> python自带库，模拟浏览器发送请求
>
> 

~~~python
from urllib import request
url = 'http://www.baidu.com/'
response = request.urlopen(url)  # 返回HTTPResponse对象，此对象以字节形式读取
content = response.read().decode('utf-8')  # 读取 
line = response.readline().decode('utf-8')  # 读一行
lines = response.readlines().decode('utf-8') # 读多行
response.getcode()  # 获取响应码
response.geturl()  # 获取url
response.getheaders()
~~~



下载

~~~python
baidu_url = 'http://www.baidu.com/'

request.urlretrieve(baidu_url, 'baidu.html')  # 下载页面，或图片等
~~~



