Python

## 基础

### 数据容器

> 通用方法 len()

#### 列表

> 可修改

```python
# 定义
my_list = list()
my_list = []
my_list = ['张三',23,'男']
# 常用操作
index = my_list.index(23)
my_list.insert(1, "学生")
my_list.append('北京')
my_list.expend([1,2,3])		 # 批量添加
del my_list[3]				# 删除
pop = my_list.pop(4)  	 	 # 返回并删除
my_list.remove('男')			# 删除第一个匹配项
my_list3.clear()
count = my_list3.count("张三")
```

#### 元组 

> 不可修改

```python
# 定义，单个元素要加','
t1 = ()
t2 = tuple()
t3 = (1, "Hello", True)
# 
```



#### 集合

> 不可重复，内容无序

~~~python
# 定义
s = set()
# 用法
s.add('张三')
s.remove('张三')
s.pop('张三')  		# 随机取元素并删除
# 两个集合的差集
s1 = {1,2,3}
s2 = {1,5,6}
s1.difference(s2)  # 集合1中在集合2中不存在的元素,结果{2,3}
# 消除两个集合的差集
s1.difference_update(set8)  # 集合1的结果{2,3}
# 集合合并
s1.union(s2)  # {1,2,3,5,6}
~~~



#### 字典

> 键值对，key不重复, 值可嵌套其他类型，通过key索引取值

```python
d = {}
d = dict()
d = {'name': '张三', 'age':23}
# 用法
# 添加 / 更新
d['sex'] = '男'
# 删除
d.pop('age')
d.clear()
d.keys()  # 获取全部key
# 遍历
for key in keys:    # 方式1
for key in d:       # 方式2
```

#### 字符串

```python
s = '12hello world21'
s.index('world')
s.replace('world','python')
s.split(' ')
s.strip('12')
s.count('2')
```

> 格式化：
>
> ~~~python
> # %
> print ("我叫 %s 今年 %d 岁!" % ('小明', 10))
> # format
> print("我叫 {} 今年 {} 岁！".format('小明',10))
> # f-string
> print(f"我叫 {name} 今年 {age} 岁！")
> ~~~

| :    | <填充>       | <对齐>                                  | <宽度>   | ,            | <.精度>              | <类别>                               |
| ---- | ------------ | --------------------------------------- | -------- | ------------ | -------------------- | ------------------------------------ |
|      | 用于填充空白 | < 左对齐(默认) <br>>右对齐<br>^居中对齐 | 输出宽度 | 千分位间隔符 | 小数位数或字符串长度 | 整数：B,c,d,o,x,X <br>浮点数:e,E,f,% |

注：and 和 or 中and具有高优先级

```python
元组只有一个元素时要加 ','
s.[::-1]    # 字符串的反转，即步长-1
```

##  函数

> Ctrl + alt + M

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

#### 基本

> python自带库，模拟浏览器发送请求
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



#### 下载

~~~python
baidu_url = 'http://www.baidu.com/'

request.urlretrieve(baidu_url, 'baidu.html')  # 下载页面，或图片等
~~~

#### 请求对象的定制

==反爬机制1==：UA : User-Agent，中文名为用户代理，是一个特殊字符串头，使的服务器能够识别客户使用的操作系统及版本，CPU类型、浏览器及版本。浏览器内核、浏览器渲染引擎、浏览器语言、浏览器插件。

需要自行包装，即加上UA

> url的组成:
>
> url: https://www.baidu.com/s?wd=周杰伦
>
> | http / https | www.baidu.com | 80/443 | s    | wd=周杰伦 | #    |
> | ------------ | ------------- | ------ | ---- | --------- | ---- |
> | 协议         | 主机          | 端口号 | 路径 | 参数      | 锚点 |
>
> 常用端口号：
>
> - http/https : 80 / 443
> - mysql ：3306
> - oracle : 1521
> - redis ： 6379
> - MongoDB：27017
>
> ~~~python
> import urllib.request
> 
> url = 'https://www.baidu.com/'
> 
> headers = {
>     'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36'
> }
> 
> # 定制请求对象，其中headers参数需要关键字传参
> request = urllib.request.Request(url, headers=headers)
> 
> # 传入请求对象，打开url
> response = urllib.request.urlopen(request)
> 
> print(response.read().decode('utf8'))
> ~~~

#### 编解码

1. get请求方式(单参数) ：urllib.parse.quote()

~~~python
import urllib.request
import urllib.parse

url = 'https://www.baidu.com/s?wd='
# 获取中文的Unicode编码
name = urllib.parse.quote('周杰伦')

url = url + name
print(url)
...
~~~

2. get请求方式(多参数)：urllib.parse.urlencode()

~~~python
import urllib.request
import urllib.parse

url = 'https://www.baidu.com/s?'

data = {
    'wd': '周杰伦',
    'sex': '男',
    'address': '中国台湾省'
}

new_data = urllib.parse.urlencode(data)
url = url + new_data
~~~

3. post请求方式：

~~~python
import urllib.request
import urllib.parse

url = f"{post请求路径}"

data = {
    'query': 'hello'
}
headers = {...}
data = urllib.parse.urlencode().encode('utf-8')    # post请求需要encode编码
request = urllib.request.Request(url,data,headers)
~~~

==反爬机制2==：cookie,请求头中需要携带Cookie

#### 异常

> 包含 `urllib.error.HTTPerror` 和 `urllib.error.URLerror`两个异常类，可以对其进行捕获

#### Cookie登录

> Cookie会携带登录信息等，可以在网页登录后获取他的Cookie，再写到请求头中来爬取登录后的页面

#### handler处理器

> 定制更高级的请求头
>
> ~~~python
> # 获取handler对象
> handler = urllib.request.HTTPHandler()
> # 传入handler对象
> opener = urllib.request.build_opener(handler)
> # 调用open()
> response = opener.open(request)
> ~~~
>
> 代理
>
> ~~~python
> # ...
> proxies = {
>     # 代理ip
>     'http': '117.26.40.99:8888'
> }
> handler = urllib.request.ProxyHandler(proxies=proxies)
> 
> opener = urllib.request.build_opener(handler)
> 
> response = opener.open(request)
> # ...
> ~~~

#### 解析

> 1. XPath 解析
>
>    ~~~
>    安装XPath Helper浏览器插件
>    安装lxml库
>    ~~~
>
>    ~~~python
>    from lxml import etree
>
>    # XPath解析
>    tree = etree.parse('XPath解析示例.html')
>    # //子孙节点    /子节点
>    li_list = tree.xpath('//ul/li') 
>    # 标签内容
>    li_list = tree.xpath('//ul/li/text()') 
>    # 查找有id属性的
>    li_list = tree.xpath('//ul/li[@id]')
>    # 查找id属性值为l1的
>    li_list = tree.xpath('//ul/li[@id="l1"]')
>    # 查找有id属性的class属性值
>    li_list = tree.xpath('//ul/li[@id]/@class')
>    # 逻辑运算
>    li_list = tree.xpath('//ul/li[@id="head" and @class="addr"]')
>    # contains()模糊     starts-with() 
>    li_list = tree.xpath('//ul/li[contains(@id,"l")]')
>    li_list = tree.xpath('//ul/li[starts-with(@id,"c")]')
>    ~~~
>
> 2. jsonpath解析
>
>    ~~~properties
>    安装jsonpath库
>    $	根对象或元素.
>    @	当前对象或元素.
>    . or []	子元素操作符.
>    ..	递归匹配所有子元素.
>    *	通配符. 匹配所有对象或元素.
>    []	下标运算符，JsonPath索引从0开始.
>    [,]	连接运算符，将多个结果拼成数组返回，JSONPath允许使用别名.
>    [start:end:step]	数组切片运算符.
>    ?()	过滤器（脚本）表达式.
>    ()	脚本表达式.
>    ~~~
>
>    ~~~python
>    import json
>    import jsonpath
>    json = json.load(open('书.json', 'r', encoding='utf-8'))
>    
>    # 所有book的author节点
>    title_list = jsonpath.jsonpath(json, '$.store.book[*].author')
>    
>    # 所有author节点
>    author_list = jsonpath.jsonpath(json, '$..author')
>    
>    # store下的所有节点
>    store_list = jsonpath.jsonpath(json,'$.store.*')
>    
>    # store下的所有price节点
>    price_list = jsonpath.jsonpath(json, '$.store..price')
>    
>    # 第三个book节点
>    book_3 = jsonpath.jsonpath(json, '$..book[2]')
>    
>    # 倒数第一个book节点
>    book_last = jsonpath.jsonpath(json, '$..book[(@.length-1)]')
>    # $..book[-1:]
>    
>    # 前两个book节点
>    book_pre2 = jsonpath.jsonpath(json, '$..book[:2]')
>    # $..book[0,1]
>    ~~~
>
> 3. BeautifulSoup解析（简称bs4）
>
>    ~~~
>    
>    ~~~
>
>    

#### Selenium

用于web应用程序测试，驱动真正浏览器

> 1. 下载浏览器驱动
> 2. 添加到环境变量
> 3. `from selenium import webdriver`,获取浏览器对象

[谷歌浏览器驱动]:http://chromedriver.storage.googleapis.com/index.html

元素定位

~~~python
# 根据id来找对象
button = browser.find_element_by_id('su')

# 根据标签属性找对象
input = browser.find_element_by_name('wd')

# 根据Xpath语句获取对象
input1 = browser.find_element_by_xpath('//input[@id="su"]')

# 根据标签名字来获取对象
inputs = browser.find_elements_by_tag_name('input')

# 根据bs4语法来获取对象，即CSS选择器
button1 = browser.find_element_by_css_selector('#su')

# 获取链接对象
links = browser.find_element_by_link_text('图片')
~~~

交互

> 获取对象，执行操作

无界面

1. Phantomjs 

2. Chrome handless

   > 

### requests

#### 基本使用

> ```python
> import requests
> url = "http://www.baidu.com"
> response = requests.get(url)
> 
> response.text
> response.encoding
> response.url    
> response.content
> response.status_code
> response.headers
> ```

#### get请求

~~~python
response = requests.get(url=url, params=data, headers=headers)
~~~

#### post请求

~~~python
response = requests.post(url=url, data=data, headers=headers)
~~~

#### 代理

```python
response = requests.get(url=url, headers=headers, proxies=proxies)
```

#### Cookie登录及验证码

