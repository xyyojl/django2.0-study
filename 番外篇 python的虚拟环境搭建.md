## 背景

- 期待实现的效果：安装virtualenv虚拟环境，进而获取到requirments.txt 依赖文件
- 系统：windows 10，64bit
- 版本等环境配置是：python 3.6.7

## 搭建虚拟环境

注：安装 Python 的时候，最好安装在默认目录，也就是C 盘

1、安装 virtualenv

```python
pip install virtualenv
```

2、创建虚拟环境

```python
virtualenv env
```

3、启动环境

注：由于本人的电脑系统是win的，所以就会不一样的

```python
# 进入 env 这个目录
cd env
# 启动虚拟环境
.\Scripts\activate
# 错误写法，原因：win的坑
./Scripts/activate
```

解释：

> `source` 是一个shell命令，专为在Linux上运行的用户（或任何Posix，但不管是什么，而不是Windows）而设计。
>
> 在Windows上，virtualenv会创建一个批处理文件，因此您应该运行`venv\Scripts\activate`（根据[激活脚本上的](https://virtualenv.pypa.io/en/stable/userguide/#activate-script) virtualenv [文档](https://virtualenv.pypa.io/en/stable/userguide/#activate-script)）。

4、退出当前虚拟环境

```python
.\Scripts\deactivate.bat
```

5、生成 requirments.txt 依赖文件

```python
pip freeze > requirments.txt
```

## 安装 virtualenvwrapper 扩展包

>Virtaulenvwrapper 是 virtualenv 的扩展包，可以把新创建的环境记录下来，不需要每次启动虚拟环境时都执行一遍 source 命令，可以更方便的管理虚拟环境。
>它可以实现：
>1、将所有虚拟环境整合在一个目录下
>2、管理（新增，删除，复制）虚拟环境
>3、快速切换虚拟环境

1、安装 virtualenvwrapper

```python
pip install virtualenvwrapper-win
```

2、设置workon_home环境变量

3、新建虚拟环境

```python
mkvirtualenv env2
```

4、查看安装的所有虚拟环境

```python
workon
```

5、进入虚拟环境

```python
workon env2
```

6、退出虚拟环境

```python
# 别人的做法，但是我不能使用，下面这个我觉得是linux系统的操作
deactivate
# 我的做法
cd 虚拟环境的目录
.\Scripts\deactivate
```

## 参考资料：

- [最全的Python虚拟环境使用方法](https://zhuanlan.zhihu.com/p/60647332)
- [virtualenv的问题 - 无法激活](https://xbuba.com/questions/8921188)
- [Windows下搭建Python虚拟环境](https://www.jianshu.com/p/ad2d8ee4a679)
- [python的虚拟环境](https://segmentfault.com/a/1190000015885508)
- [Python（入门）学会创建以及管理Python虚拟环境](https://zhuanlan.zhihu.com/p/45037497)
- [deactivate不能关闭venv](https://blog.csdn.net/weixin_43793888/article/details/89214096)