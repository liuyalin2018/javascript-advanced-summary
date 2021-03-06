### 正则表达式

##### 量词

- ？表示从未匹配或只匹配一次
- *表示匹配零次或多次
- +表示匹配一次或多次
- {n}表示完全匹配n次
- {n，}表示匹配n次或多次
- {n，m}表示匹配最少n次，最多匹配m次

##### 断言

断言，用来检测输入的当前位置

- ^只匹配输入的开始位置
- $只匹配输入结束位置
- \b只匹配单词的边界。不要与[\b]混淆，他匹配一个退格
- \B只匹配非单词边界
- (?=exp)：正向肯定断言：只匹配exp所匹配的接下来的内容。exp只是用来向前查找，但会忽略匹配的pattern部分
- (?!exp)：正向否定断言：只匹配exp不匹配的接下来的内容。exp只是用来向前查找，但会忽略匹配的pattern部分

##### 创建正则表达式

1. 字面量与构造函数

   字面量：/xyz/i  加载时编译

   构造函数（第二个参数是可选的） new RegExp('xyz','i')  运行时编译

   字面量在加载时编译，下面的代码求值时会产生异常：

   > function foo(){
   >
   > ​	/[/;
   >
   > }

   构造函数在调用时编译正则表达式。下面的代码不会导致异常，但调用foo()会导致异常

   > function foo(){
   >
   > ​	new RegExp('[');
   >
   > }

2. 标识

   标识是正则表达式字面量的后缀和正则表达式构造函数的参数；它修改正则表达式的匹配行为。

   g——global：给定的正则表达式可以匹配多次，他会影响几种方法，尤其是replace()

   i——ignoreCase：试图匹配给定的正则表达式时忽略大小写

   m——multiline：在多行模式时，开始操作符^和结束操作符$匹配每一行，而不是输入的整个字符串

##### RegExp.prototype.test:是否存在匹配

test()方法用来检查正则表达式regex是否匹配字符串str：regex.test(str)

test()根据是否设置/g标识，运行结果有所不同。如果没有设置/g标识，则该方法检查是否在str某处存在匹配；如果设置了/g标识，则该方法在str中会多次匹配regex并返回，属性regex.lastIndex包含最后一次匹配之后的索引。

##### String.prototype.search:匹配位置的索引

search()方法查找str匹配regex的位置：str.search(regex)

如果存在匹配，返回发现匹配位置的索引。否则，返回值-1。进行查找时，regex的global和lastIndex属性被忽略。如果search()的参数不是正则表达式，它被转换为正则表达式，search（）不支持全局检索。

##### RegExp.prototype.exec：捕获分组

首次分配（不设置标识/g），则只返回第一次的匹配结果

全部匹配（设置标识/g），会反复调用exec()返回所有的匹配项，返回值null表示没有任何匹配，属性lastIndex表示下次匹配从哪儿继续。

##### String.prototype.match：捕获分组或返回所有匹配的子字符串

var matchData = str.match(regex);

##### String.prototype.replace:查找和替换

replace()方法在字符串str中进行查找，匹配search并用replacement替换匹配项：str.replace（search，replacement）

search字符串或正则表达式，字符串：在输入字符串中找到匹配的字面量；正则表达式：对输入字符串进行匹配。

replacement字符串或函数，字符串：描述如何替换找到的匹配项；函数：执行替换并通过参数提供匹配信息。

##### 正则表达式备忘单

.（点）匹配除了行结束符（换行符、回车符等）的一切字符。使用[\s\S]可以真正匹配一切

- 转移字符类
  1. \d 匹配数字([0-9]);\D匹配非数字
  2. \w 匹配拉丁字母数字的字符以及下划线([A-Za-z0-9_])；\W匹配所有其他字符
  3. \s 匹配所有空白字符(空格、制表符、换行符等)；\S匹配所有非空白字符

- 字符类（字符集合）：[...]和[’^‘  ...]
  1. 源字符：[abc]  (除了\]-的所有匹配其本身的字符)
  2. 转义字符类：[\d\w]
  3. 范围：[A-Za-z0-9]

- 分组
  1. 捕获组：（...）；反向引用：\1
  2. 非捕获组：（？：...）

##### 直接量字符

字母和数字字符      匹配自身

\o     NUL字符（\u0000）

\t      制表符（\u0009）

\n     换行符 （\u000A）

\v      垂直制表符（\u000B）

\f       换页符（\u000C）

\r        回车符（\u000D）

\xnn    由十六进制nn指定的拉丁字符，\x0A等价于\n

\uxxx   由十六进制数xxxx指定的Unicode字符,例如\u0009等价于\t

\cX        控制字符^X，例如\cJ等价于换行符\n

##### 选择、分组和引用

子表达式分组和引用前一子表达式的特殊字符，字符“|”用于分隔供选择的字符。

eg：/ab|cd|ef/可以匹配ab、cd、ef；/\d{3}|[a-z]{4}/匹配三位数字或者四个小写字母。

圆括号的作用：①把单独的项组合成子表达式，以便可以向处理一个独立的单元那样用“|”“*”“+”“？”等来对单元内的项进行处理；②在完整的模式中定义子模式；③带圆括号的表达式的另一个用途是允许在同一正则表达式的后部引用前面的子表达式。

##### 修饰符

正则表达式的修饰符，用以说明高级匹配模式的规则，修饰符放在/之外，i用以说明模式匹配是不区分大小写的；g说明模式匹配应该是全局的；m用以在多行模式中执行匹配。

##### RegExp的属性

source：只读字符串，包含正则表达式的文本

global：是一个只读的布尔值，用以说明这个正则表达式是否带有修饰符g

ignoreCase：只读的布尔值，用以说明正则表达式是否带有修饰符i

multiline：只读的布尔值，用以说明正则表达式是否带有修饰符m

lastIndex：可读可写的整数，存储字符串中下一次检索的开始位置