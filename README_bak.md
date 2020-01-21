- [此文是实践中的Python3与Python2的不同，快乐并痛苦](#此文是实践中的Python3与Python2的不同，快乐并痛苦)   
- [升级python2到python3](#升级python2到python3)   
 - [Mac版](#Mac版)   
     - [安装homebrew](#安装homebrew)   
     - [安装python3](#安装python3)   
     - [升级pip](#升级pip)   
     - [虚机环境virtualenv](#虚机环境virtualenv)   
 - [2to3转换](#2to3转换)   
 - [代码](#代码)   
     - [print](#print)   
     - [UNICODE字符串](#UNICODE字符串)   
     - [全局函数UNICODE](#全局函数UNICODE)   
     - [比较运算符](#比较运算符)   
     - [返回列表的字典类方法](#返回列表的字典类方法)   
     - [map-filter-reduce](#map-filter-reduce)   
     - [TRY-EXCEPT](#TRY-EXCEPT)   
     - [XRANGE](#XRANGE)   
     - [INPUT](#INPUT)   
     - [函数属性FUNC](#函数属性FUNC)   
     - [IO方法XREADLINES()](#IO方法XREADLINES())   
     - [lambda函数](#lambda函数)   
     - [全局函数ZIP](#全局函数ZIP)   
     - [TYPES模块中的常量](#TYPES模块中的常量)   
     - [对元组的列表解析](#对元组的列表解析)   
     - [元类](#元类)   
     - [INPUT](#INPUT)   
     - [INPUT](#INPUT)   
     - [INPUT](#INPUT)   
     - [INPUT](#INPUT)   
     - [INPUT](#INPUT)   
- [包差异](#包差异)   
 - [pylint](#pylint)   
# 此文是实践中的Python3与Python2的不同，快乐并痛苦


# 升级python2到python3

## Mac版

查看下当前python版本， 如果已经是Python 3，则跳过
```
python --version
```

###### 安装homebrew
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

###### 安装python3
```
brew install python
```

安装完成后查看下新python版本和位置 ，发现python3会成本默认版本。我的是/usr/local/opt/python/libexec/bin/python
```
which python
```


###### 升级pip
```
 pip3 install --upgrade pip setuptools wheel
```


```
If you need Homebrew's Python 2.7 run
  brew install python@2

Pip, setuptools, and wheel have been installed. To update them run
  pip3 install --upgrade pip setuptools wheel

You can install Python packages with
  pip3 install <package>
They will install into the site-package directory
  /usr/local/lib/python3.6/site-packages
```
###### 虚机环境virtualenv
之后python的2/3使用都可以自由切换
```
virtualenv venv  //python3
virtualenv venv -p python2.7 //python2
```

***

## 2to3转换
python3内置2to3命令， 简单的代码可以直接转3
```
2to3  -w gmc.py 生成转换的bake文件
2to3  -w gmc.py 直接覆盖原文件
2to3  --help
```
```
Options:
  -h, --help            show this help message and exit
  -d, --doctests_only   Fix up doctests only
  -f FIX, --fix=FIX     Each FIX specifies a transformation; default: all
  -j PROCESSES, --processes=PROCESSES
                        Run 2to3 concurrently
  -x NOFIX, --nofix=NOFIX
                        Prevent a transformation from being run
  -l, --list-fixes      List available transformations
  -p, --print-function  Modify the grammar so that print() is a function
  -v, --verbose         More verbose logging
  --no-diffs            Don't show diffs of the refactoring
  -w, --write           Write back modified files
  -n, --nobackups       Don't write backups for modified files
  -o OUTPUT_DIR, --output-dir=OUTPUT_DIR
                        Put output files in this directory instead of
                        overwriting the input files.  Requires -n.
  -W, --write-unchanged-files
                        Also write files even if no changes were required
                        (useful with --output-dir); implies -w.
  --add-suffix=ADD_SUFFIX
                        Append this string to all output filenames. Requires
                        -n if non-empty.  ex: --add-suffix='3' will generate
                        .py3 files.

```

## 代码
###### print
python2 | python3 | 备注 |
:--- | :--- | :--- |
print | print() | 输出一个空白行，python3需要调用不带参数的print() |
print 1, 2, |	print(1,2, end=' ') |	python2中如果使用一个,作为print结尾，将会用空格分割输出的结果，然后在输出一个尾随的空格，而不输回车。python3里，把end=' ' 作为一个关键字传给print()可以实现同样的效果，end默认值为'\n',所以通过重新指定end参数的值，可以取消在末尾输出回车符号
print >> sys.stderr, 1, 2, 3 |	print(1, 2, 3, file=sys.stderr	 |python2中，可以通过>>pipe_name语法，把输出重定向到一个管道，比如sys.stderr.在python3里，可以通过将管道作为关键字参数file的值传递给print()完成同样的功能。

###### UNICODE字符串
python2中有两种字符串类型：Unicode字符串和非Unicode字符串
Python3中只有一种类型：Unicode字符串

python2 | python3 | 备注 |
:--- | :--- | :--- |
u'PapayaWhip' |	'PapayaWhip' |	python2中的Unicode字符串在python3即为普通字符串
ur'PapayaWhip\foo' |	r'PapayWhip\foo'|	Unicode原始字符串(使用这种字符串，python不会自动转义反斜线"\")也被替换为普通的字符串，因为在python3里，所有原始字符串都是以unicode编码的。

###### 全局函数UNICODE

python2 | python3 | 备注 |
:--- | :--- | :--- |
unicode()把对象转换成unicode字符串 <br> str()把对象转换为非Unicode字符串 |  unicode字符串，所以str()函数即可完成所有的功能


###### 比较运算符

python2 | python3 | 备注 |
:--- | :--- | :--- |
<> or != |   != |


###### 返回列表的字典类方法
python2里，许多字典类方法的返回值是列表。最常用方法有keys, items和values
python3，所有以上方法的返回值改为动态试图。在一些上下文环境里，这种改变不会产生影响。如果这些方法的返回值被立即传递给另外一个函数，而且那个函数会遍历整个序列，那么以上方法的返回值是列表或视图并不会产生什么不同。如果你期望获得一个被独立寻址元素的列表，那么python3的这些改变将会使你的代码卡住，因为视图不支持索引


python2 | python3 | 备注 |
:--- | :--- | :--- |
a_dictionary.keys()	| list(a_dictionary.keys())	| 使用list()将keys 返回值转换为一个静态列表
a_dictionary.items()  | list(a_dictionary.items()) | 将items返回值转为列表
a_dictionary.iterkeys()	| 	iter(a_dictionary.keys()) | python3不再支持iterkeys,使用iter()将keys()的返回值转换为一个迭代器
[i for i in a_dictionary.iterkeys()]	| 	[ i for i in a_dictonary.keys()]	| 	不需要使用额外的iter()，keys()方法返回的是可迭代的
min(a_dictionary.keys())	| 	no change	| 	对min(),max(),sum(),list(),tuple(),set(),sorted(),any()和all()同样有效
重命名或重新组织的模块:


###### map-filter-reduce
python2里，filter()方法返回一个列表，这个列表是通过一个返回值为True或False的函数来检测序列里的每一项的值

python3中，filter()函数返回一个迭代器，不再是列表

跟filter()的改变一样，map()函数现在返回一个迭代器,python2中返回一个列表

在python3里，reduce()函数已经从全局名字空间移除，现在被放置在fucntools模块里

python2 | python3 | 备注 |
:--- | :--- | :--- |
filter(a_function, a_sequence) |	list(filter(a_function, a_sequence))
map(a_function,'PapayaWhip'	| list(map(a_function, 'PapayaWhip'))
reduce(a,b,c)	| from functools import reduce reduce(a, b, c)

###### TRY-EXCEPT

python2 | python3 | 备注 |
:--- | :--- | :--- |
except (RuntimeError, ImportError), e pass``` |   except(RuntimeError, ImportError) as e
ex.message | 没有message属性
支持raise MyException, 'error message' | 只支持 raise MyException('error message') |


###### XRANGE
python２里，有两种方法获得一定范围内的数字：range(),返回一个列表，还有xrange(),返回一个迭代器

python3　里，range()返回迭代器，xrange()不再存在

python2 | python3 | 备注 |
:--- | :--- | :--- |
a_list = range(10)	| a_list= list(ange(10))


###### INPUT
python2有两个全局函数，用在命令行请求用户输入。
第一个叫input()，它等待用户输入一个python表达式(然后返回结果)。用户输入什么他就返回什么,
第二个叫做raw_input()，输入结果为字符串形式
python3 通过input替代了他们，输入结果为字符串形式

python2 | python3 | 备注 |
:--- | :--- | :--- |
raw_input()	| input	|

###### 函数属性FUNC
python2,函数的代码可用访问到函数本身的特殊属性

python3为了一致性，这些特殊属性被重命名了

python2 | python3 | 备注 |
:--- | :--- | :--- |
a_function.func_name    |	a_function.__name__  |	__name__属性包含了函数的名字
a_function.func_doc	  |a_function.__doc__	  | __doc__包含了函数源代码定义的文档字符串
a_function.func_defaults  |	a_function.__defaults__	  |是一个保存参数默认值的元组
a_function.func_dict  |	a_function.__dict__	  |__dict__属性是一个支持任意函数属性的名字空间
a_function.func_closure	  |a_function.__closure__	  |__closure__属性是由cell对象组成的元组，包含了函数对自由变量的绑定
a_function.func_globals	  |a_function.__globals__	  |是对模块全局名字空间的引用
a_function.func_code	  |a_function.__code__	  |是一个代码对象，表示编译后的函数体


###### IO方法XREADLINES()
python2中，文件对象有一个xreadlines()方法，返回一个迭代器，一次读取文件的一行。这在for循环中尤其实用

python3中，xreadlines()方法不再可用


###### lambda函数

python2 | python3 | 备注 |
:--- | :--- | :--- |
lambda (x,): x + f(x)|	lambda x1 : x1[0] + f(x1[0])	|注１
lambda (x,y): x + f(y)	|lambda x_y : x_y[0] + f(x_y[1])	|注２
lambda (x,(y,z)): x + y + z	|lambda x_y_z: x_y_z[0] + x_y_z[1][0]+ x_y_z[1][1]	|注３
lambda x,y,z: x+y+z |	unchanged	|注４

注１：如果定义了一个lambda函数，使用包含一个元素的元组作为参数，python3中，会被转换成一个包含到x1[0]的引用的lambda函数。x1是2to3脚本基于原来元组里的命名参数自动生成的。

注２：使用含有两个元素的元组(x,y)作为参数的lambda函数被转换为x_y,它有两个位置参数，即x_y[0]和x_y[1]

注３：2to3脚本可以处理使用嵌套命名参数的元组作为参数的lambda函数。产生的结果有点晦涩，但python3下和python2的效果是一样的。

注４：可以定义使用多个参数的lambda函数。语法在python3同样有效


###### 全局函数ZIP

python2，zip()可以使用任意多个序列作为参数，它返回一个由元组构成的列表。第一个元组包含了每个序列的第一个元素，第二个元组包含了每个序列的第二个元素，依次递推

python３中返回一个迭代器，而非列表

python2 | python3 | 备注 |
:--- | :--- | :--- |
zip(a,b,c)	| list(zip(a,b,c)	| python3中可以通过list函数遍历zip()返回的迭代器，形成列表返回
d.join(zip(a,b,c))	| no change	| 在已经会遍历所有元素的上下文环境里，zip()本身返回的迭代器能够正常工作，2to3脚本检测到后，不再修改


###### TYPES模块中的常量

python2 | python3 | 备注 |
:--- | :--- | :--- |
types.UnicodeType |	str
types.StringType	| bytes
types.DictType |		dict
types.IntType |		int
types.LongType |		int
types.ListType |		list
types.NoneType |		type(None
types.BooleanType |		bool
types.BufferType |		memoryview
types.ClassType	 |	type
types.ComplexType |		complex
types.EllipsisType	 |	type(Ellipsis)
types.FloatType	 |	float
types.ObjectType |		object
types.NotImplementedType |		type(NotImplemented)
types.SliceType	 |	slice
types.TupleType	 |	tuple
types.TypeType	 |	type
types.XRangeType |		range


###### 对元组的列表解析
python2，如果需要编写一个遍历元组的列表解析，不需要在元组值周围加上括号

python3里，这些括号是必需的

python2 | python3 | 备注 |
:--- | :--- | :---  |
[ i for i in 1, 2]' | 	'[i for i in (1,2)]

###### 元类
python2里，可以通过在类的声明中定义metaclass参数，或者定义一个特殊的类级别(class-level__metaclass__属性，来创建元类

python3中，__metaclass__属性被取消了

python2 | python3 | 备注 |
:--- | :--- | :--- |
class C(metaclass=PapayaMeta): pass	 |  unchanged
class Whip: __metaclass__ = PapayMeta	 | class Whip(metaclass=PapayaMeta): pass
class C(Whipper, Beater): __metaclass__ = PapayaMeta | 	class C(Whipper, Beater, metaclass=PapayMeta): pass


###### INPUT

python2 | python3 | 备注 |
:--- | :--- | :--- |




###### INPUT

python2 | python3 | 备注 |
:--- | :--- | :--- |




###### INPUT

python2 | python3 | 备注 |
:--- | :--- | :--- |




###### INPUT

python2 | python3 | 备注 |
:--- | :--- | :--- |




###### INPUT

python2 | python3 | 备注 |
:--- | :--- | :--- |



***

# 包差异

## pylint


python2 | python3 | 备注 |
:--- | :--- | :--- |
pip install pylint | pip install pylint --upgrade | [pylint]((https://github.com/PyCQA/pylint)) |

Pylint can be simply installed by running:
```
pip install pylint
```

If you are using Python 3.6+, upgrade to get full support for your version:
```
pip install pylint --upgrade
```

***

