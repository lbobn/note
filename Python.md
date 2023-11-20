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

**类型注解**

~~~python
name: str = "张三"
is_boy: bool = True

l: list = [1, 2, 3]
t: tuple = (1, "nihao", True)
s: set = {2, 5, 6, 54}
d: dict = {"zhangsan": 23, "lisi": 25}

l2: list[int] = [1, 2, 3]
t2: tuple[int, str, bool] = (1, "nihao", True)
s2: set[int] = {2, 5, 6, 54}
d2: dict[str, int] = {"zhangsan": 23, "lisi": 25}
    
def func2(data: list) -> list:
    # 返回值注解
    return data
# union类型
from typing import Union

l: list[Union[int, str]] = [1, 2, "nihao", "hello"]

def func(data: Union[str, int]) -> Union[str, int]:
    pass
~~~

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

- 抛出异常

```python
raise [Exception [, args [, traceback]]]
```
- 捕获异常并处理

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

- **`self`**不是关键字，它表示当前对象的地址

- 类变量前加 **`__`** 表示**私有**
- `@classmethod` **类方法**
- `@staticmethod` **静态方法**  类名调用
- 含有**抽象方法**的类为**抽象类**，抽象方法指方法体为**空实现**

~~~python
class Student:
    # 类变量，被所有类对象共享
    # 公有属性
    teacher = "张三"  		
    #定义私有属性,私有属性在类外部无法直接进行访问
    __id = None
    
    # 构造方法
    def __init__(self,name,age):   # 实例变量
        self.name = name
        self.age = age
    
    # 静态方法
    @staticmethod    
    def play(self):
        print("玩游戏")
        
    # 私有方法
    def __foo(self):          
        print('这是私有方法')
        
    #类方法
    @classmethod
    def setTeacher(cls,name):
        cls.__teacher = name
        
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

对于私有方法可使用`property`来简化

~~~python
class Stu:
    def __init__(self, name):
        self.__name = name

    @property	
    def name(self):
        return self.__name

    @name.setter
    def name(self, name):
        self.__name = name


if __name__ == '__main__':
    s = Stu('zhangsan')
    s.name = 'lisi'
    print(s.name)
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

抽象类

```python
class Animal:
    def speak(self):
        pass
class Dog(Animal):
    def speak(self):
        print("汪汪汪")
```

## 高阶

### 闭包

- `nonlocal`关键字，用于声明全局变量，内部函数可修改此变量

```python
def creat_account(balance=0):
    def ATM(money, deposit=True):
        nonlocal balance  # 声明全局变量
        if deposit:
            balance += money
            print(f"存钱:{money} 余额:{balance}")
        else:
            balance -= money
            print(f"存钱:{money} 余额:{balance}")

    return ATM
```

### 装饰器

> 装饰器 在不破坏原有函数同时加入新功能

```python
def outer(func):
    def inner():
        print("业务函数执行前新功能")
        func()
        print("业务函数执行后新功能")

    return inner

@outer		# 要被包装的函数加上注解
def function():		# 业务函数
    pass
```

### 正则表达式匹配

~~~python
import re
s = "hello world"
re.match("world",s)		# 从头匹配
re.search("world",s)	# 查找第一子串
re.findall("world",s)	# 查找所有子串
r = r"^(\w+(\.\w+)*@(qq|QQ|163|gmail)(\.\w+)+)$"	# 邮箱正则
re.findall(r,"zxcv@qq.com")	
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
>    	ctrl + shift + x
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



### scrapy

#### 安装

`pip install scrapy`

#### 创建项目

`scrapy startproject 项目名称`

项目结构

~~~
项目名称
	项目名称
		spiders
			init
			爬虫文件
		init
		items			定义数据结构的地方
		middleware  	中间件 代理
		pipelines		管道 处理下载的数据
		settings 	 	配置文件   robots协议  ua定义等
~~~

response 的属性和方法

~~~
response.text 	网页源码
response.body 	二进制
response.xpath   
response.extract()		提取selector对象的data属性值 
response.extract_first()	提取selector列表的第一个数据
~~~



#### 创建爬虫文件

`cd ./项目名称/项目名称/spiders`

`scrapy genspider 爬虫名 url`

#### 运行爬虫文件

`scrapy crawl 爬虫名字`

注：需将settings.py文件中的 ROBOTSTXT_OBEY = False  置为False 或 注释掉

Robots协议（也称为爬虫协议、机器人协议等）的全称是“**网络爬虫排除标准**”，robots.txt是搜索引擎访问网站时第一个查看的文件，当我们网站有部分内容不希望收搜索引擎抓取时，就可以通过Robots协议来告诉搜索引擎哪些页面是不能抓取的，大多用来保护网站的隐私，以及一些死链、重复页面等等。

https://www.baidu.com/robots.txt

#### scrapyshell调试

- 安装ipython `pip install ipython`
- `scrapy shell www.baidu.com`   终端直接输入
- 自动进入 ipython 界面

实例：

> `yield`相当return 只返回一次

```python
# 爬虫文件
class DangSpider(scrapy.Spider):
    name = "dang"
    allowed_domains = ["category.dangdang.com"]
    start_urls = ["http://category.dangdang.com/cp01.01.01.00.00.00.html"]

    base_url = 'https://category.dangdang.com/pg'
    page = 1

    def parse(self, response):
        li_list = response.xpath('//ul[@id="component_59"]/li')
        # //ul[@id="component_59"]/li/a/img/@data-original
        # //ul[@id="component_59"]/li/p[@class="price"]/span[1]
        for li in li_list:
            # 存在懒加载，滑到指定位置src才会变为真的
            src = li.xpath('./a/img/@data-original').extract_first()
            if not src:
                src = li.xpath('./a/img/@src').extract_first()
            name = li.xpath('./a/img/@alt').extract_first()
            price = li.xpath('./p[@class="price"]/span[1]/text()').extract_first()
            # print(src, name, price)
            # 导入数据结构类
            book = ScrapyDangdang02Item(src=src, name=name, price=price)
            yield book	# 返回对象
        if self.page < 20:
            self.page += 1
            url = self.base_url + str(self.page) + '-cp01.01.01.00.00.00.html'

            yield scrapy.Request(url=url, callback=self.parse)	# 调用parse
```

```python
# 数据结构
import scrapy


class ScrapyDangdang02Item(scrapy.Item):
    # define the fields for your item here like:
    # name = scrapy.Field()
    # 定义数据结构
    src = scrapy.Field()
    name = scrapy.Field()
    price = scrapy.Field()
    # pass

```

`open_spider(self, spider)`爬虫开启时执行一次

`close_spider(self, spider)`爬虫结束时执行一次

```python
# 管道
from itemadapter import ItemAdapter


class ScrapyDangdang02Pipeline:
    def open_spider(self, spider):
        self.fp = open('book.json', 'w', encoding='utf-8')

    # 设置管道并在settings开启管道
    # item 即为  book 对象
    def process_item(self, item, spider):
        self.fp.write(str(item))
        return item
    
	
    def close_spider(self, spider):
        self.fp.close()


import urllib.request
# 开启多管道（仿照原有类编写）
#  1.定义管道类
#  2.在settings中开启管道
class ScrapyDangdangDownload:
    def process_item(self, item, spider):
        url = 'http:' + item.get('src')
        name = './book/' + item.get('name') + '.jpg'
        urllib.request.urlretrieve(url=url, filename=name)
        return item
```

```python
# Configure item pipelines
# See https://docs.scrapy.org/en/latest/topics/item-pipeline.html
ITEM_PIPELINES = {
   # 管道是可以有多个，有优先级的，数字小优先级高
   "scrapy_dangdang_02.pipelines.ScrapyDangdang02Pipeline": 300,
   "scrapy_dangdang_02.pipelines.ScrapyDangdangDownload": 301,

}
```

#### CrawlSpider 链接提取器

创建爬虫文件

创建命令中加 -t 选项

`scrapy genspider -t crawl read https://www.dushu.com/book/1107_1.html`

```python
rules = (
        Rule(
            LinkExtractor(allow=r"/book/1107_(\d+)\.html"),  # 允许的url,正则表达式
            callback="parse_item",
            follow=False  # 表示是否跟随访问,即访问到第二页后还有满足的URL继续访问
        ),
    )
```

#### 写入数据库

在settings文件中写入如下配置

~~~python
# 数据库连接配置
DB_HOST = 'localhost'
DB_PORT = 3306
DB_USER = 'root'
DB_PASSWORD = '25gn3umb'
DB_NAME = 'scrapy_test'
DB_CHARSET = 'utf8'  # - 不识别
~~~

在管道文件中定义新管道

```python
from scrapy.utils.project import get_project_settings 	# 读取settings文件
import pymysql


class MySQLCrawlspiderPipeline:
    def open_spider(self, spider):
        settings = get_project_settings()
        self.DB_HOST = settings['DB_HOST']
        self.DB_PORT = settings['DB_PORT']
        self.DB_USER = settings['DB_USER']
        self.DB_PASSWORD = settings['DB_PASSWORD']
        self.DB_NAME = settings['DB_NAME']
        self.DB_CHARSET = settings['DB_CHARSET']
        self.__connect()  # 连接数据库

    def process_item(self, item, spider):
        sql = 'insert into book(name,src) values ("{}","{}")'.format(item['name'], item['src'])
        # 执行SQL并提交
        self.cursor.execute(sql)	
        self.conn.commit()
        return item

    def close_spider(self, spider):
        self.cursor.close()
        self.conn.close()

    def __connect(self):
        self.conn = pymysql.Connection(host=self.DB_HOST,
                                       port=self.DB_PORT,
                                       user=self.DB_USER,
                                       password=self.DB_PASSWORD,
                                       db=self.DB_NAME,
                                       charset=self.DB_CHARSET)
        self.cursor = self.conn.cursor()
```

#### post请求

```python
class BaidufanyiSpider(scrapy.Spider):
    name = "baidufanyi"
    allowed_domains = ["fanyi.baidu.com"]

    # post请求 没有参数则请求无意义，parse方法也无意义
    # start_urls = ["https://fanyi.baidu.com/sug"]
    #
    # def parse(self, response):
    #     pass

    def start_requests(self):
        url = 'https://fanyi.baidu.com/sug'

        data = {
            'kw': 'final'
        }

        yield scrapy.FormRequest(url=url, formdata=data, callback=self.parse_second)

    def parse_second(self, response):
        content = response.text

        obj = json.loads(content)
        print(obj)
```

#### 日志

日志等级

- CRITICAL : 严重错误

- ERROR： 一般错误

- WARNING：警告

- INFO：一般信息

- DEBUG：调试信息

  默认为DEBUG

settings.py文件设置

LOG_FILE : 将日志记录到文件中，文件以`.log`结尾

LOG_LEVEL:设置日志等级

```python
# 日志
LOG_LEVER = 'ERROR'
LOG_FILE = 'logdemo.log'
```

