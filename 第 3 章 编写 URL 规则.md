## 第 3 章 编写 URL 规则

URL（Uniform Resource Locator，统一资源定位符）是对可以从互联网上得到的资源位置和访问方法的一种简洁的表示，**是互联网上标准资源的地址**。**互联网上的每个文件都有一个唯一的 URL，用于指出文件的路径位置。**简单地说，URL 就是常说的网站，每个地址代表不同的网页，在 Django 中，URL 也称为 URLConf。

## 3.1 URL 编写规则

需要先对 MyDjango 项目的目录进行调整，使其更符合开发规范性。在每一个 App 中设置独立的**静态资源**和**模板文件夹**并添加一个**urls.py** 文件。

**此处有图片，请自行脑补！！！**

在 App 里添加 urls.py 是将属于 App 的 URL 都写入到该文件中，而项目根目录的 urls.py 是**将每个 App 的 urls.py 统一管理**。当程序收到用户请求的时候，首先会在根目录的 urls.py 查找该 URL 是属于哪个 App，然后再从 App 的 urls.py 找到具体的 URL 信息。在根目录的 urls.py 编写 URL 规则，如下：

```python
# 根目录的 urls.py
from django.contrib import admin
from django.urls import path,include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('',include('index.urls'))
]
```

上面的代码中设定了两个 URL 地址：

- Admin 站点管理：创建项目时已自动生成的，一般情况下无须更改。
- 首页地址

urls.py 的代码解释：

- from django.contrib import admin：导入 Admin 功能模块。
- from django.urls import path,include：导入 URL 编写模块。
- urlpatterns：整个项目的 URL 集合，每个元素代表一条 URL 信息。
- path('admin/', admin.site.urls)：设定 Admin 的 URL。'admin/'代表 127.0.0.1:8000/admin 这个地址信息，admin 后面的斜杠是**路径分隔符**；admin.site.urls是 URL 的处理函数，也称为视图函数。
- path('',include('index.urls'))：URL 为空，代表为网站的域名。即 127.0.0.1:8000，**通常是网站的首页**；**include 将该 URL 分发给 index 的 urls.py 处理**。

注：根目录下的 urls.py 相当于是总路由，每个 app 的路由相当于是分路由。

由于首页的地址**分发给 index 的 urls.py 处理**，因此下一步需要对 index 的 urls.py 编写 URL 信息，代码如下：

```python
# index 的 urls.py 的编写规则与根目录的 urls.py 大致相同，基本上所有的 URL 都有固定编写格式的。
from djang.urls import path
# 导入了同一目录下的 views.py 文件
from . import views
urlpatterns = [
    path('',view.index)
]
```

在 views.py 中编写 index 函数的处理过程，**该文件是用于编写视图函数，处理 URL 请求信息并返回网页内容给用户。**代码如下：

```python
# index 的 views.py
from django.http import HttpResponse
# Create your views here.
def index(request):
    # request：当前用户的请求对象
    return HttpResponse("Hello world")
```

index 函数必须设置参数 request，该参数代表**当前用户的请求对象**，该对象包含**用户名、请求内容和请求方式**等信息，视图函数执行完成后必须使用 return 将处理结果返回，否则程序会抛出异常信息。启动 MyDjango 项目，在浏览器中打开 http://127.0.0.1:8000/，即可看到页面效果。

## 3.2 带变量的 URL

在日常开发过程中，有时候一个 URL 可以代表多个不同的页面，如编写带有日期的 URL。Django 在编写 URL 时，可以对 URL 设置变量值，使 URL 具有多样性。

URL 的变量类型有字符类型、整型、slug 和 uuid，最为常用的是**字符类型和整型**。各个类型说明如下：

- 字符类型：匹配任何非空字符串，但不含斜杠。如果没有指定类型，**默认使用该类型**。
- 整型：匹配 0 和 正整数。
- slug：可理解为注释、后缀或附属等概念，常作为 URL 的解释性字符。可匹配任何 ASCII 字符以及连接符和下划线，能使 URL 更加清晰移动。比如网页的标题是 "13 岁的孩子"，其 URL 地址可以设置为 "13-sui-de-hai-zi"。
- uuid：匹配一个 uuid 格式的对象。为了防止冲突，**规定必须使用破折号并且所有字母必须小写**。

在 index 的 urls.py 里添加带有字符类型、整型和 slug 的 URL 地址信息，代码如下：

```python
# index 的 urls.py 
from django.urls import path
from . import views
urlpatterns = [
    path('',views.index),
    # 添加带有字符类型、整型和slug 的 URL 新增的
    path('<year>/<int:month>/<slug:day>',views.mydate)

]
```

在 URL 中使用变量符号 "<>" 可以为 URL 设置变量，在括号里面以冒号划分为两部分，前面代表的是**变量的数据类型**，后面代表的是**变量名**，变量名可自行命名。对上面代码的变量说明，如下：

- `<year>`：变量名为 year，数据格式为字符类型，与`<str:year>`的含义一样。
- `<int:month>`：变量名为 month，数据格式为整型。
- `<slug:day>`：变量名为 day，数据格式为 slug。

在 views.py 中编写视图函数 mydate 的处理方法，代码如下：

```python
# views.py 的 mydate 函数
def mydate(request, year, month, day):
    # 参数year、month、day来自于 URL 的变量，URL 的变量和视图函数的参数要一一对应
    return HttpResponse(str(year)+ '/' + str(month) + '/' + str(day))
```

注：如果视图函数的参数与 URL 的变量对应不上，那么程序会抛出参数不相符的报错信息。Page not found (404)

为了进一步规范日期格式，可以使用正则表达式限制 URL 的可变范围。正则表达式的 URL 编写规则如下：

```python
# index 的 urls.py 
from django.urls import path,re_path
from . import views
urlpatterns = [
    path('',views.index),
    # 添加带有字符类型、整型和slug 的 URL
    # path('<year>/<int:month>/<slug:day>',views.mydate)
    re_path('(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/(?P<day>[0-9]{2}).html',views.mydate)
]
```

在 URL 中引入正则表达式，首先需要导入 **re_path 功能模块**，正则表达式的作用是对 URL 的变量进行截取与判断，以小括号表示，每个小括号的前后可以使用斜杠或其他字符将其分隔。每个变量以一个小括号为单位。

- ?P 是固定格式。
- `<year>` 为变量的编写规则。
- [0-9]{4} 是正则表达式的匹配模式，代表变量的长度为 4，只允许取 0 - 9 的值。

注：如果URL 的末端使用正则表达式，那么在该 URL 的末端应加上斜杠或其他字符，否则正则表达式无法生效。例如上述例子的变量 day，若在末端没有设置“.html”，则在浏览器上输入无限长的字符串，程序也能正常访问。

## 3.3 设置参数 name

Django 还可以对 URL 进行命名。在 index 的 urls.py、views.py  和 模板 myyear.html 中添加以下代码：

```python
# 在 urls.py 添加新的 URL 信息 记得在此之前加逗号
re_path('(?P<year>[0-9]{4}).html',views.myyear,name='myyear')

# 在 views.py 添加对应的视图函数
from django.shortcuts import render
def myyear(request, year):
    return render(request,'myyear.html')

# 在 templates 文件夹添加 myyear.html 文件：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div><a href="/2018.html">2018 old Archive</a></div>
    <div><a href="{% url 'myyear' 2018 %}">2018 old Archive</a></div>
</body>
</html>
```

整个执行流程如下：

- 当用户访问该 URL 时，项目根据 URL 信息选择视图函数 myyear 处理，并将该 URL 命名为 myyear。
- 视图函数 myyear 将模板 myyear.html 作为响应内容并生成相应的网页返回给用户。
- 在模板 myyear.html 中分别设置两个标签a，虽然两个标签 a 的 href 属性值的写法有所不同，但实质上两者都指向命名为 myyear 的 URL 地址信息。
- 第 2 个标签 a 的 href 是 Django 的模板语法，模板语法以 {%  %} 表示，其中 url 'myyear' 是将命名为 myyear 的 URL 地址信息作为 href 属性值；2018 是该 URL 的变量year，若 URL 没有设置变量值，则无须添加。

上面例子中，模板中的 myyear 与 urls.py 所设置的参数 name 是 一一对应的。参数 name 的作用是**对该 URL 地址信息进行命名，然后在 HTML 模板中使用生成相应的 URL 信息。**

注：在 URL 中设置参数 name，只要参数 name 的值不变，无论 URL 地址信息如何修改都无须修改模板中标签a 的href 属性值，有利于 URL 的变更和维护。

## 3.4 设置额外参数

除了参数 name 之外，还有一种参数类型是以字典的数据类型传递的，该参数没有具体命名，只要是字典形式即可，而且该参数只能在视图函数中读取和使用。其代码如下：

```python
# 参数为字典的 URL
    re_path('dict/(?P<year>[0-9]{4}).htm',views.myyear_dict,{'month':'05'},name='myyear_dict')
    
# 参数为字典的 URL 的视图函数
def myyear_dict(request, year, month):
    return render(request, 'myyear_dict.html',{'month':month})

# 在 templates 文件夹添加 myyear_dict.html 文件：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div><a href="{% url 'myyear_dict' 2018 %}">2018 {{month}} Archive</a></div>
</body>
</html>
```

上述代码分别从 URL、视图函数和 HTML 模板来说明 URL 额外参数的具体作用，说明如下：

- 除了在 URL 地址信息中设置参数 name 之外，还加入了参数{'month':'05'}，该参数用于设置参数 month，参数值为 05.
- 然后视图函数 myyear_dict 获取了变量 year 和参数 month，**前者设置在 URL 地址中，而后者在 URL 地址外**。
- 最后视图函数将参数 month 的值传递到 HTML 模板并生成 HTML 网页返回给用户。

在编写 URL 规则时，如果需要设置额外的参数，设置规则如下：

- 参数只能是以字典的形式表示。
- 设置的参数只能在视图含读取和使用。
- 字典的一个键值对代表一个参数，键代表参数名，值代表参数值。
- 参数值没有数据格式限制，可以为**某个对象、字符串或列表（元组）**等。

