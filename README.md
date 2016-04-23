#Python学习笔记
##数据类型和变量
###数据类型：
***
Python可以直接表达的数据类型包括：整数，浮点数，复数，字符串，布尔值和空值。

* 整数

    任意大小，正负皆可。并且可以用0x和0o分别表示十六进制数和八进制数。

* 浮点数

    可使用科学记数法，即1.23x10^9就是1.23e9 ，0.000012可以写成1.2e-5，等等。

    整数运算永远是精确的(结果也是整数)，浮点数运算则有四舍五入的误差。

    Python提供两种除法+求余运算：  
    * 【 **/** 】 结果为浮点数，即使两数整除，结果也是浮点数。  
    * 【 **//** 】 结果为整数，也称地板除，对结果向下取整。  
    * 【 **%** 】 结果为整数，用于计算余数。

* 复数

    Python中复数用j表示虚数部分，比如说 `x=2.4 + 5.6j`， 并且可以通过复数的类属性 `real` 取得实部，类属性 `imag` 取得虚部，通过类方法 `conjugate()` 获得共轭复数。

* 字符串 

    使用单引号或双引号括起。而引号本身需要使用转义符【 \ 】来表达。 用【 \' 】来表示【 ' 】。

    转义符还可以用来转义很多字符，如【 **\n** 】表示换行。【 **\t** 】表示制表符。 【 \ 】本身也需要转义用 **双斜杠** 来代替。

    如果一个字符串中很多字符都要转义就会很麻烦，所以Python又提供一种简便的写法，【 **r''** 】表示两个引号之间的内容是不需要转义的。 对于多行的字符串，为了避免加入【 **\n** 】的不方便，可以使用【 ’’’…’’’ 】的格式，即用三个引号来括起字符串，这样字符串的换行会被保留。

* 布尔值

    只有True和False两种，首字母大写。如果输入一个判断式，则Python会给出布尔值的结果。比方说输入3>2，运行后会得到True。 对于布尔值有三个逻辑运算符，分别是**and**，**or**和**not**。 本质上True用1存储，False用0存储。

* 空值

    用**None**表示，不同于数值0。比方说字符串【 '' 】就是空值。

###变量：
***
Python变量名不可以以数字开头，构成可以包括大小写英文，数字及下划线。变量没有固定类型，可以把任意数据类型复制给一个变量并反复赋值。因此Python是一种动态语言。

当我们写：a=’ABC’ 时，Python解释器干了两件事情：

1. 在内存中创建了一个'ABC'的字符串；  
2. 在内存中创建了一个名为a的变量，并把它指向'ABC'。

###常量：
***
Python中没有常量，因为变量都可以重复赋值。但是一般约定，用全部字母大写的单词来表示一个常量，如：PI=3.14。

##字符串和编码
###字符编码：
***
在计算机存储中内存统一使用Unicode编码，而硬盘保存或者传输数据就用UTF-8编码。比方说打开记事本时，数据会从硬盘中UTF-8编码格式转换为Unicode格式，保存时则相反。 

浏览网页时，服务器端动态生成的内容是Unicode编码，而传输时则会变为传输UTF-8编码格式的网页。所以常常在网页源码中看到<meta charset="UTF-8" />的语句。

* ASCII

    最早的编码，包含大小写英文，数字及一些符号。大写A编码为65，小写a编码为97。字符0编码为48。使用8位二进制组合(1个字节)，可表示128个不同字符(最高位用于校验)。
* GB2312

    处理中文时一个字节显然不足，至少需要用两个字节，并且不与ASCII冲突，因为有了中国自己制定的GB2312编码，可用于编码中文。
* Shift_JIS和Euc-kr

    日本编码为Shift_JIS，韩文编码为Euc-kr。这样各个国家都用自己不同的标准和编码就很容易在多语言的环境产生冲突，变成乱码。因此又有了通用的一套编码。
* Unicode

    Unicode将所有语言统一到一套编码中，一般使用2个字节，部分非常偏僻的字符会用到4个字节。但是使用Unicode编码，如果在大量英文的环境又非常浪费空间，因为存储和传输时大小是使用ASCII编码的两倍，不划算，于是又有了新的方式。
* UTF-8

    UTF-8是可变长编码，对Unicode字符不同的数字大小编码成1-6个字节，英文字母为1个字节，汉字一般是3个字节。需要用到4-6字节的字符非常少，这样比较节省空间。

**Example**：

| 字符 | ASCII | Unicode  | UTF-8 |
|:------:|:------:|:------:|:------:|
| A | 01000001 | 00000000 01000001 | 01000001 |
| 中 | (超出范围) | 01001110 00101101 | 11100100 10111000 10101101 |

###字符串：
***
Python字符串采用Unicode编码，支持多语言。

* ord()和chr()
  
    ord函数可以获取字符的十进制整数表示，chr函数可以将十进制整数转换为Unicode编码下对应的字符。此外使用【\u】转义也表示这是一个Unicode字符。

    比如：【ord(‘中’)】结果是【20013】，而【chr(20013)】结果是【’中’】，并且20013的十六进制表示是0x4e2d，因此使用【’\u4e2d’】得到的结果也是【’中’】。

* bytes
 
    因为Python中字符串用Unicode编码，一个字符对应若干字节，要在网络中传输或存储到硬盘中就要把字符串变为以bytes类型。

    Python显示bytes类型的数据会用b作前缀，如 **b'ABC'**，注意和 **'ABC'** 进行区分，尽管内容一样，但前者的每个字符都只占1个字节，而后者在Python中以Unicode进行编码，每个字符占两个字节。

    Unicode字符串可以通过【 encode() 】方法编码为指定的bytes，如ASCII编码，utf-8编码etc，bytes类型的数据如果字符不属于ASCII码的范围，就用【 **'\x##** 】的格式表示。 相应地，如果读取字节流的数据，就要用【 decode() 】方法解码。 此外，还可以用【 len() 】方法来计算字符串的长度，对bytes类型使用的话则得到字节数。

**例子**：

    >>> 'ABC'.encode('ascii')
    b'ABC'
    >>> '中文'.encode('utf-8')
    b'\xe4\xb8\xad\xe6\x96\x87'
    >>> b'ABC'.decode('ascii')
    'ABC'
    >>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
    '中文'

###格式化：
***
和C语言类似，用【 ％ 】占位符实现。【 ％s 】表示用字符串替换，【 ％d 】，【 ％f 】，【 ％x 】分别表示用整数，浮点数和十六进制数替换。其中【 ％s 】可以将任意数据类型转换为字符串。【 ％d 】和【 ％f 】可以指定整数和小数部分的位数。

**例子**：

    >>> '%2d-%02d' % (3, 1)
    ' 3-01'
    >>> '%.2f' % 3.1415926
    '3.14'

***notice***：

* 只有一个占位符时，后面变量不需要用括号括起。
* 如果字符串本身包含【 ％ 】，则需要转义，用【 ％％ 】来表示百分号。

###格式化：
***
和C语言类似，用【 ％ 】占位符实现。【 ％s 】表示用字符串替换，【 ％d 】，【 ％f 】，【 ％x 】分别表示用整数，浮点数和十六进制数替换。其中【 ％s 】可以将任意数据类型转换为字符串。【 ％d 】和【 ％f 】可以指定整数和小数部分的位数。

**例子**：

    >>> '%2d-%02d' % (3, 1)
    ' 3-01'
    >>> '%.2f' % 3.1415926
    '3.14'

***notice***：

* 只有一个占位符时，后面变量不需要用括号括起。
* 如果字符串本身包含【 ％ 】，则需要转义，用【 ％％ 】来表示百分号。

##使用list和tuple
### list
***
list是一种Python内置的数据类型，表示有序集合，可动态删除和插入。通过索引可以访问列表元素，索引从0开始，即访问第一个列表元素。并且列表是循环的，可以通过索引－1访问最尾的元素，索引－2访问倒数第二个元素。

**例子**：

    >>> classmates = ['Michael', 'Bob', 'Tracy']
    >>> classmates
    ['Michael', 'Bob', 'Tracy']
    >>> classmates[0]
    'Michael'       
                                                                                                                                                                                                         
另外，还可以用「len()」函数获取列表的元素个数。

list 是一个可变的有序表，所以，可以往 list 中追加元素到末尾：

    >>> classmates.append('Adam')
    >>> classmates
    ['Michael', 'Bob', 'Tracy', 'Adam']

也可以把元素插入到指定的位置，比如索引号为 1 的位置：

    >>> classmates.insert(1, 'Jack')
    >>> classmates
    ['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']

要删除 list 末尾的元素，用 pop() 方法：

    >>> classmates.pop()
    'Adam'
    >>> classmates
    ['Michael', 'Jack', 'Bob', 'Tracy']

要删除指定位置的元素，用 pop(i) 方法，其中 i 是索引位置：

    >>> classmates.pop(1)
    'Jack'
    >>> classmates
    ['Michael', 'Bob', 'Tracy']

要把某个元素替换成别的元素，可以直接赋值给对应的索引位置： 

    >>> classmates[1] = 'Sarah'
    >>> classmates
    ['Michael', 'Sarah', 'Tracy']

list 里面的元素的数据类型也可以不同，比如：

    >>> L = ['Apple', 123, True]

list 元素也可以是另一个 list，比如：

    >>> s = ['python', 'java', ['asp', 'php'], 'scheme']
    >>> len(s)
    4

要注意s只有4个元素，s[2]作为一个list类型的元素。要拿到'php'可以用s[2][1]，即把s看作一个二维数组，这样的嵌套可以有很多层。

如果一个 list 中一个元素也没有，就是一个空的 list，它的长度为 0：

    >>> L = []
    >>> len(L)
    0

    ### tuple
***
tuple也是一种有序列表，但tuple一旦初始化就无法再修改，也因为这个特性，所以代码更安全。和list不同，tuple用小括号来括起。

**例子**：

    >>> classmates = ('Michael', 'Bob', 'Tracy')
    定义空的tuple如下如下：
    >>> t = ()
    >>> t
    ()

但是，要定义一个只有1个元素的 tuple，就要注意一下，如果使用t = (1)来定义，则得到的不是一个tuple，而是整数1，因为括号既可以表示tuple又可以表示数学公式中的小括号。这种情况下默认为后者。要定义1个元素的tuple格式如下，使用一个逗号进行区分：

    >>> t = (1,)
    >>> t
    (1,)
    
tuple也有「可变」的例子，如果tuple的其中一个元素是list，则这个list元素的内容是可以修改的，如下：

    >>> t = ('a', 'b', ['A', 'B'])
    >>> t[2][0] = 'X'
    >>> t[2][1] = 'Y'
    >>> t
    ('a', 'b', ['X', 'Y'])
    
这个实际修改的是list而不是tuple，tuple指向位置不会变，而list指向的位置可变，上面的例子实质是创建了字符串X和Y，然后让tuple的第三个元素即list元素指向这两个新的字符串。