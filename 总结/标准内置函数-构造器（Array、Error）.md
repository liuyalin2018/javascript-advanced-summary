## 标准内置对象-构造器（Array、Error）

### 异常捕获

异常捕获的两个原则：如果一处出错的含义不能被描述，那么就抛错；找到一个可以捕获错误的位置，捕获异常。

##### throw

throw的语法：throw “value”；  可以抛出任何的JavaScript值

> if(somethingBadHappened) {
>
> ​	throw new Error('Something bad happened');
>
> }

#####  try-catch-finally

try是必须的，而catch和finally两者至少要有其一

- catch会捕获到try中抛出的任何的异常，无论是直接抛出的异常还是其中函数调用抛出的异常。

- 不论try中发生了什么，finally会永远执行，往往我们都会在finally里面做一些清理工作。

- finally在return语句之后执行

  > var count = 0;
  >
  > function countUp(){
  >
  > ​	try{
  >
  > ​			return count;
  >
  > ​	}finally{
  >
  > ​			count++;
  >
  > ​	}
  >
  > }
  >
  > countUp()    ==>0
  >
  > count     ==>1
  >
  > 在finally执行时，count的值已经被存入队列等待返回

#####  Error构造器

ECMAScript标准化了以下错误构造函数

- Error是通用异常构造器。所有其他的异常构造器都是它的子构造器
- EvalError “在标准中未使用，它只是为了兼容上一版本的标准”
- RangeError“表示一个数值超出了允许的范围。eg：new Array（-1）
- ReferenceError表示发现了一个非法的引用值
- SyntaxError表示产生了一个语法解析错误
- TypeError表示一个被操作值的实际类型与其期望的类型不一致
- URIError表示某个全局的URL控制函数的使用不兼容其定义

异常的属性值

- message：是异常的信息
- name：是异常的名称
- Stack：是栈跟踪，他不是标准的，但很多平台都在使用

#####  栈跟踪

通过栈跟踪，会创建异常对象的调用栈快照。 e.stack

### 数组

##### 数组是映射，不是元组

ECMAScript标准声明数组可以作为从索引到值的映射。数组可能不是连续的，中间可能会有空缺

#####  数组也可以具有属性

数组仍然是对象，并且可以拥有对象的属性。

#####  创建数组

1. 通过数组字面量创建数组

   > var myArray = ['a','b','c'];

2. 数组构造函数

   - 创建给定长度的空数组

     > var arr = new Array(2);

   - 用元素初始化数组（应该避免）

     > var arr1 = new Array('a','b','c');
     >
     > new Array(2)   ==>[,,]

##### 多维数组

如果需要多个维度的元素，必须嵌套数组 。

如果矩阵较小且维度固定，可以通过数组字面量来设置。

>  var rows = [['.','.',',','.'],['.','.','.'],['.','.','.','.']];

##### 数组索引

数组索引的限制：

- 索引i是数字，范围是0<=i<2的32次方-1
- 最大长度是2的32次方-1
- 在这个范围之外的索引被视为普通的属性键，他们不会作为数组元素呈现，且不影响length属性。

操作符in与索引：

操作符in检测对象是否具有某个给定键的属性，他也可以用来判断一个数组中是否存在给定的元素索引。

> var arr = ['a',,'b'];
>
> 0 in arr;    ==>true

删除数组元素：操作符delete，删除元素会产生空缺（不会更新length属性）

##### 长度

属性length的基本功能是追踪数组的最大索引，不计算元素的个数

属性length作为指针指向插入新元素的位置

**手动增加数组的长度**对数组几乎没有明显的影响，只会创建一些空缺

通过**Array构造函数**设置数组初始长度，创建的数组都是空的

如果**减少数组**的长度，在新长度及之后的所有元素都会被删除

**清空数组**：如果把数组的长度设置为0，则数组变为空，可以清空数组元素

把数组长度设置为0会影响共享词数组的对象，可以将数组赋值为空数组 

**数组的最大长度**是2的32次方-1

##### 数组中的“空缺”

索引个数小于数组长度说明数组缺少一些元素，在缺少元素的索引处读取该元素会返回undefined

##### 创建空缺

1. 通过给数组索引赋值来创建空缺

   > var arr = [];
   >
   > arr[0] = 'a';
   >
   > arr[2] = 'c';
   >
   > 1 in arr;    ==>false

2. 在数组字面量中省略值来创建空缺

   > var arr = ['a',,'c'];

##### 稀疏数组和密集数组

含有空缺的数组称为稀疏，不含空缺的数组称为密集。密集数组是连续的，且在每个索引处都存在元素

在相同索引处，空缺和undefined元素很相似，两个数组都有相同的长度；但稀疏数组在索引0处没有元素。

两个数组for遍历的结果相同；

forEach循环会跳过空缺，而不会跳过undefined元素

##### 忽略空缺

1. 数组遍历方法：

   - forEach()遍历时跳过空缺

     > ['a',,'b'].forEach(function(x,i){ console.log(i+'.'+x)})
     >
     > 0.a
     >
     > 2.b

   - every()也会跳过空缺

     > ['a',,'b'].every(function(x){ return typeof x=== 'string'})
     >
     > true

   - map()虽然会跳过，但保留空缺

     > ['a',,'b'].map(function(x,i){ console.log(i+'.'+x)})
     >
     > ['0.a','','2.b']

   - filter()去除了空缺

     > ['a',,'b'].filter(function(x){ return true})
     >
     > ['a','b']

2. 其他数组方法

   - join()把空缺、undefined和null转换为空字符串

     > ['a',,'b'].join('-')
     >
     > 'a--b'

   - sort()在排序时保留空缺

     > ['a',,'b'].sort();
     >
     > ['a','b',,]

3. for-in 循环可以正确的列出属性键

4. Function.prototype.apply():

   apply()把每个空缺转化成值为undefined的参数，可以用apply()创建包含undefined元素的数组

##### 数组构造函数

Array.isArray(obj):如果obj是一个数组，则返回true

#### 数组原型对象

##### 添加和删除元素——破坏性

1. Array.prototype.shift():删除索引0处的元素并返回该元素，随后元素的索引依次减1

2. Array.prototype.unshift(elem？……)：在数组最前面增加给定元素，返回新的数组长度

3. Array.prototype.pop()：移除数组最后的元素并返回该元素

4. Array.prototype.push(elem？……)：在数组尾部增加给定元素，返回新的数组长度

5. Array.prototype.splice(start，deleteCount？，elem？，……)：从索引start开始，移除deleteCount个元素，并插入给定的元素。

   特殊参数值：start可以是负数；deleteCount是可选的

##### 排序和颠倒元素顺序——破坏性

1. Array.prototype.reverse()：颠倒数组中的元素顺序，并返回指向原(修改后的)数组的引用
2. Array.prototype.sort(compareFunction?)：数组排序，并返回排序后的数组，排序是通过把元素转换为字符串再对值进行比较

- 比较数字

  > function compareCanonically(a,b){
  >
  > ​	return return a < b ? -1(a > b ? 1 :0);
  >
  > }
  >
  > [-1,-20,7,50].sort(compareCanonically)
  >
  > [-20,-1,7,50]

- 比较字符串

  > ['c','a','b'].sort(function(a,b){return a.localeCompare(b)})
  >
  > ['a','b','c']

- 比较对象

  > function compareNames(a,b){
  >
  > ​	return a.name.localeCompare(b.name);
  >
  > }// 根据名字进行排序

##### 合并、切分和连接——非破坏性

1. Array.prototype.concat(arr1?,arr2?,……)：创建一个新数组，其中包括接受者的所有元素，其次是数组arr1的所有元素；如果其中一个参数不是数组，它将作为元素添加到结果中.

   > var arr=['a','b'];
   >
   > arr.concat('c',['d','e']);
   >
   > ['a','b','c','d','e']

2. Array.prototype.slice(begin?,end?)：把数组从begin开始到end（不包含end）的元素复制到新数组中；如果任意一个索引值是负值，则该值加上数组长度，-1指向最后一个元素

3. Array.prototype.join(separator?)：通过对所有数组元素应用toString()创建字符串，并用separator连接字符串，默认使用‘，’连接。join()把undefined和null转换为空字符串，数组中的空缺也转化为空字符串。

##### 值的查找——非破坏性

1. Array.prototype.indexOf(searchValue,startIndex?)：从数组startIndex开始，查找searchValue，这个方法返回第一次出现searchValue的索引，没有找到返回-1。查找使用严格相等，不能用indexOf()查找NaN。
2. Array.prototype.lastIndexOf(searchElement,startIndex?)：反向查找

##### 迭代——非破坏性

- 检测方法
  1. Array.prototype.forEach(callback,thisValue?)：遍历数组中的元素，不支持break或者类似于提前终止循环的处理。
  2. Array.prototype.every(callback,thisValue?)：对每个元素，回调函数都返回true，则返回true。一旦回调函数返回false，则停止迭代。没有返回值会导致隐式返回undefined，every()解释为false
  3. Array.prototype.some(callback,thisValue?)：如果回调函数至少有一个元素返回true，则返回true。一旦回调函数返回true，则停止迭代。没有返回值会导致隐式返回undefined，some()解释为false

- 转换方法
  1. Array.prototype.filter(callback,thisValue?)：输出数组只包含callback返回为true的输入元素

-  规约方法 
  1. Array.prototype.reduce(callback,initialValue?)：从左到右进行迭代，并按照之前描述的调用回调函数。这个方法的结果是由回调函数返回的最后的值。
  2. Array.prototype.reduceRight(callback,initialValue?)：与reduce()工作原理相同，但从右到左遍历。