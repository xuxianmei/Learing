

## 效率

保证了准确性后，才需要是否要考虑要优化。大多数情形是不需要优化的，除非运行的非常慢。什么情形正
则表达式运行才慢呢？我们需要考察正则表达式的运行过程（原理）。
正则表达式的运行分为如下的阶段：
1. 编译；
2. 设定起始位置；
3. 尝试匹配；
4. 匹配失败的话，从下一位开始继续第 3 步；
5. 最终结果：匹配成功或失败。

### 使用具体型字符组来代替通配符，来消除回溯

### 使用非捕获型分组
因为括号的作用之一是，可以捕获分组和分支里的数据。那么就需要内存来保存它们。

当我们不需要使用分组引用和反向引用时，此时可以使用非捕获分组。

例如，
```
/^[-]?(\d\.\d+|\d+|\.\d+)$/
``` 
可以修改成：
```
/^[-]?(?:\d\.\d+|\d+|\.\d+)$/
```
### 独立出确定字符
例如，
```
/a+/
```
可以修改成
```
/aa*/
```
因为后者能比前者多确定了字符 "a"。
这样加快判断是否匹配失败，进而加快移位的速度。

6.4.4. 提取分支公共部分
比如，
```
/^abc|^def/
``` 修改成 
```
/^(?:abc|def)/
```
又比如， 
```
/this|that/
```
修改成
```
/th(?:is|at)/
```
这样做，可以减少匹配过程中可消除的重复。
6.4.5. 减少分支的数量，缩小它们的范围
```
/red|read/
```
可以修改成
```
/rea?d/
```
此时分支和量词产生的回溯的成本是不一样的。但这样优化后，可读性会降低的。


