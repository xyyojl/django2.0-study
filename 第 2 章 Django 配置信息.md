# 第 2 章 Django 配置信息

项目配置是根据实际开发需求从而针对整个 Web 框架编写相关配置信息。配置信息主要由**项目的 settings.py 实现**，主要配置有**项目路径、密钥配置、域名访问权限、App 列表、配置静态资源、配置模板文件、数据库配置、中间件和缓存配置**。

## 2.1 基本配置信息

一个简单的项目必须具备的基本配置信息有：**项目路径、密钥配置、域名访问权限、App 列表和中间件**。

```python
import os

# 项目路径
# Build paths inside the project like this: os.path.join(BASE_DIR, ...)
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))


# Quick-start development settings - unsuitable for production
# See https://docs.djangoproject.com/en/2.2/howto/deployment/checklist/

# 密钥配置
# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = '%x=%x7%f%p6)t68=5pffnhef@=^t1ln$^^4a!_z+d+-na%&8@x'

# 调试模式
# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

# 域名访问权限
ALLOWED_HOSTS = []

# Application definition
# App 列表
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

各个配置说明如下：

- 项目路径 BASE_DIR：主要通过 os 模块读取**当前项目在系统的具体路径**，该代码在创建项目时自动生成，一般情况下无须修改。

- 密钥配置 SECRET_KEY：是一个**随机值**，在项目创建的时候自动生成，一般情况下无须修改。主要**用于重要数据的加密处理**，提高系统的安全性，避免遭到攻击者恶意破坏。密钥主要用于用户密钥、CSRF 机制和会话 Session 等数据加密。

  - 用户密码：Django 内置一套用户管理系统，该系统具有**用户认证和存储用户信息**等功能，在创建用户的时候，将用户密码通过密码进行加密处理，保证用户的安全性。
  - CSRF 机制：该机制主要用于表单提交，防止窃取网站的用户信息来制造恶意请求。
  - 会话 Session：**Session 的信息存放在 Cookies**，以一串随机的字符串表示，用于**标识当前访问网站的用户身份，记录相关用户信息**。

- 调试模式 DEBUG：该值为布尔类型。如果在开发调试阶段应设置为 True，在开发调试过程中会**自动检测代码是否发生更改，根据检测结果执行是否刷新重启系统**。如果项目部署上线，应将其改为 False，否则会泄露系统的相关信息。

- 域名访问权限 ALLOWED_HOSTS：设置可访问的域名，默认值为空。

  - 当 DEBUG 为 True并且 ALLOWED_HOSTS 为空时，项目只允许以 localhost 或者 127.0.0.1 在浏览器上访问。

  - 当 DEBUG 为False 时，**ALLOWED_HOSTS 为必填项**，否则程序无法启动，**如果想允许所有域名访问，可设置 ALLOWED_HOSTS = ['*']**。

- App 列表 INSTALLED_APPS：告诉 Django 有哪些 App。在项目创建时已有 admin，auth 和 session 等配置信息，这些都是 Django 内置的应用功能，各个功能说明如下。

    - admin：内置的后台管理系统。
    - auth：内置的用户认证系统。
    - contenttypes：记录项目中所有 model 元数据（Django 的 ORM 框架）。
    - sessions：Session 会话功能，用于标识当前访问网站的用户身份，记录相关用户信息。
    - messages：消息提示功能。
    - staticfiles：查找静态资源路径。

如果在项目创建了 App，必须在 App 列表 INSTALLED_APPS 添加 App 名称。

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'index'
]
```

## 2.2 静态资源

静态资源指的是网站中不会改变的文件。在一般的应用程序中，静态资源包括 **CSS 文件、JavaScript 文件以及图片**等资源文件。

一个项目在开发过程中肯定需要使用 CSS 和 JavaScript 文件，这些静态文件的存放主要由配置文件 settings.py 配置，配置信息如下：

注：下面这个是默认配置的

```python
# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/2.2/howto/static-files/

STATIC_URL = '/static/'
```

注：上述配置将静态资源存放文件夹 static，而**文件夹 static 只能存放在 App 里面**。当项目启动时，Django 会根据静态资源存放路径去查找相关的资源文件，查找功能主要由 App 列表 INSTALLED_APPS 的 staticfiles 实现。

启动项目，在浏览器上访问 http://127.0.0.1:8000/static/index_pic.png，可以看到图片展现在浏览器中。**如果将 static 文件夹放置在 MyDjango 的根目录下，在浏览器上会显示 404 无法访问的异常信息。**

**如果想在 MyDjango 的根目录下存放静态资源，可以在配置文件 settings.py 中设置  STATICFILES_DIRS 属性。**该属性以列表的形式表示，设置方式如下：

```python
# 设置根目录的静态资源文件夹 public_static
# 设置 App（index）的静态资源文件夹 index_static
STATICFILES_DIRS = [os.path.join(BASE_DIR,'public_static'),
                    os.path.join(BASE_DIR,'index/index_static')]
```

分别在**项目的根目录**下添加文件夹 public_static 和**在 App 中添加文件夹 index_static**，在这两个文件夹下放置相应的图片。启动项目后，在浏览器上分别输入地址 http://127.0.0.1:8000/static/public_static.jpg 和 http://127.0.0.1:8000/static/index_static.jpg，可以看到静态资源的内容展现在浏览器上。

配置属性 STATIC_URL 和 STATICFILES_DIRS 的区别：

- STATIC_URL 是**必须配置的属性而且属性值不能为空**。如果没有配置 STATICFILES_DIRS，则STATIC_URL 只能识别 App 里的 static 静态资源文件夹。
- STATICFILES_DIRS 是可选配置属性，属性值为列表或元组代表静态资源文件夹，这些文件夹可自行命名。
- 在**浏览器上访问项目的静态资源**时，无论项目的静态资源文件夹是如何命名的，在浏览器上，**静态资源的上级目录必须为 static**，而 static 是 STATIC_URL 的属性值，因为 STATIC_URL  也是静态资源的起始 URL。

静态资源配置还有 STATIC_ROOT，其作用是**方便在服务器上部署项目，实现服务器和项目之间的映射**。STATIC_ROOT 主要收集整个项目的静态资源并存放在一个新的文件夹，然后由该文件夹与服务器之间构建映射关系。【STATIC_ROOT 用于项目生产部署，在项目开发过程中作用不大】

```python
STATIC_ROOT = os.path.join(BASE_DIR,'all_static')
```

## 2.3 模板路径

在 Web 开发中，**模板是一种较为特殊的 HTML 文档**。这个 HTML 文档嵌入了一些能够让 Python 识别的变量和指令，然后程序解析这些变量和指令，生成完整的 HTML 网页并返回给用户浏览。**模板是 Django 里面的 MTV 框架模式的 T 部分，配置模板路径是告诉 Django 在解析模板时，如何找到模板所在的位置。**

创建项目时，Django 已有初始的模板配置信息，如下：

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

模板配置的每个元素的含义说明如下：

- BACKEND：定义模板引擎，用于识别模板里面的变量和指令。内置的模板引擎有 Django Templates 和 jinja2.Jinja2，每个模板引擎都有自己的变量和指令语法。
- DIRS：设置模板所在路径，告诉 Django 在哪个地方查找模板的位置，默认为空列表。【重点】
- APP_DIRS：是否在 App 里查找模板文件。
- OPTIONS：用于填充在 RequestContext 中上下文的调用函数，一般情况下不做任何修改。

**注：模板配置通常配置 DIRS 的模板路径即可。**

在项目的根目录和 index 下分别**创建 templates 文件夹，并在文件夹下分别创建文件 index.html 和 app_index.html**。

说明如下：

- 根目录的 templates 通常存放**共用的模板文件**，能够供各个 App 的模板文件调用，该模式符合代码重复使用的原则，如 HTML 的 `<head>` 部分
- index 的 templates 是存放**当前 App 所需要使用的模板文件**。

模板配置代码如下：

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR,'template'),
                os.path.join(BASE_DIR,'index/template')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

## 2.4 数据库配置

数据库配置是选择项目所使用的数据库的类型，**不同的数据库需要设置不同的数据库引擎**，数据库引擎用于实现项目与数据库的连接，Django 提供 4 种数据库引擎：

- 'django.db.backends.postgresql'
-  'django.db.backends.mysql'
- 'django.db.backends.sqlite3'
- 'django.db.backends.oracle'

项目创建时默认使用 sqlite3 数据库，常用于嵌入式系统开发，而且占用的资源非常少。sqlite3 数据库的配置信息如下：

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

**默认数据库改成 MySQL 数据库**

- 安装 MySQL 连接模块，由于 mysqldb 不支持 python3，因此 Django2.0 不再使用 mysqldb 作为 MySQL 的连接模块，而选择了 mysqlclient 模块。
- 在配置 MySQL 之前，需要安装 mysqlclient 模块，使用 pip 安装，打开 CMD 窗口并输入安装指令 pip install mysqlclient，等待模板安装完成。然后检测 mysqlclient 的版本信息。
- 问题：mysqlclient 版本信息过低，解决办法：[几个django 2.2和mysql使用的坑](https://www.520mwx.com/view/63408 )

完成 mysqlclient 模块安装后，在项目的配置文件 settings.py 中配置数据库连接信息，代码如下：

```python
# DATABASES 的数据类型是一个 Python 的数据字典
DATABASES = {
    # 只是连接了一个 django_db 数据库
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'django_db',
        'USER': 'root',
        'PASSWORD': 'admin',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    },
    # 可以连接多个数据库
    'MyDjango': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'MyDjango_db',
        'USER': 'root',
        'PASSWORD': 'admin',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    },
}
```

问题：

如果使用的是 5.7 以上的版本，在 Django 连接 MySQL 数据库时会提示 django.db.utils.OperationalError 的错误信息，这是因为 MySQL 8.0 版本的密码加密方式发生了改变，新版本的用户密码采用的是 cha2 加密方法。

解决方法：

通过 SQL 语言将 MySQL 8.0的 版本加密方法改回原来的加密方式，在 MySQL 的可视化工具中运行以下 SQL 语句：

```
# newpassword 是我们设置的用户密码
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'newpassword'
FLUSH PRIVLEGES
```

## 2.5 中间件

**中间件（Middleware）是处理 Django 的 request 和 response 对象的钩子。**当用户在网站中进行点击某个按钮等操作，这个动作是用户向网站发送请求（request），而网页会根据用户的操作返回相关的网页内容，这个过程称为响应处理（response）。从请求到响应的过程中，当 Django 接收到用户请求时， Django 首先经过中间件处理请求信息，执行相关的处理，然后将处理结果返回给用户，中间件执行流程如图：

![](https://images.cnblogs.com/cnblogs_com/ccorz/843343/o_%E4%B8%AD%E9%97%B4%E4%BB%B6%E6%B5%81%E7%A8%8B.png)

中间件的作用主要是处理用户请求信息。开发者可以根据自己的开发需求自定义中间件，只要将自定义的中间件添加到配置属性 MIDDLEWARE 中即可激活。

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    # 添加这个中间件，使得 Django 内置的功能支持中文显示
    'django.middleware.locale.LocaleMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

配置属性 MIDDLEWARE 的数据格式为列表类型，**每个中间件的设置顺序是固定的，如果随意变更中间件很容易导致程序异常。**每个中间件的说明如下：

- SecurityMiddleware：内置的安全机制，保护用户与网站的通信安全。
- SessionMiddleware：会话 Session 功能。
- LocaleMiddleware：支持中文语言。
- CommonMiddleware：处理请求信息，规范化请求内容。
- CsrfViewMiddleware：开启 CSRF 防护功能。
- AuthenticationMiddleware：开启内置的用户认证系统。
- MessageMiddleware：开启内置的信息提示功能。
- XFrameOptionsMiddleware：放置恶意程序点击劫持。