---
title:  "终端开发组Android编程规范(Java)"
---
# 2 源文件
## 2.1 文件名
文件名应该与`public`类名或最顶层的类名相同。
## 2.2 字符集
所有源文件使用`UTF-8`字符集。
## 2.3 特殊字符
###2.3.1 空格
源文件中除字符串内容以外处，出现的空格都应该是`半角空格`，即ASCII 0x20。
除字符串内容外，不应使用`Tab`制表符，即`\t`。
### 2.3.2 转义字符
相对于其他形式(如`\010`，`\u000a`)，优先使用转义字符(如`\n`)。
## 2.4 源文件结构
### 2.4.1 package语句
`package`语句独占一行，不换行。
### 2.4.1 import语句
1. 每行一句，单句不换行。
2. 不要使用通配符，即`import xxx.*`。
3. 不要使用`import static`。
# 3 代码格式
## 3.1 缩进
使用4个空格，而不是制表符`\t`。
## 3.2 换行
一行代码的最大长度为150个半角字符，超过则需要换行。换行缩进8个空格（即比普通缩进多1级）。
1. 在每条语句结尾处换行。
```java
// 正例
doSomething();
doSomethingElse();

// 反例
doSomething(); doSomethingElse();
```
2. 应在`&&`，`||`，`&`，`|`，`.`，`::`之前，或关键字及其他符号之后换行。注意去掉行尾多余的空格。
3. 方法名跟其后的`(`之间不换行。`,`跟其前的单词之间不换行。
4. 不要在句尾的`)`和`;`处换行。
5. 优先对外层的结构换行。
```java

// 正例
public abstract void methodWithLongName(String id, String url,// 这里没有空格
        CallbackClassWithLongName callback);

// 正例
public abstract void methodWithLongName(// 这里没有空格
        String id,// 这里没有空格
        String url,// 这里没有空格
        CallbackClassWithLongName callback);

// 反例，句尾的);不应换行
public abstract void methodWithLongName(String id, String url,
        CallbackClassWithLongName callback
);


// 正例
objectWithLongName.methodWithLongName(
        id, url == null ? DEFAULT_URL : URLEncoder.encode(url), callback);

// 正例
objectWithLongName.methodWithLongName(id,
        url == null ? DEFAULT_URL : URLEncoder.encode(url), callback);

// 正例
objectWithLongName.methodWithLongName(
        id,
        url == null ? DEFAULT_URL : URLEncoder.encode(url),
        callback);

// 反例
// 参数表包含了3个参数，应该优先对外层的参数表换行，而不是内层的单个参数。
objectWithLongName.methodWithLongName(id,
        url == null ?
        DEFAULT_URL : URLEncoder.encode(url), callback);

// 一行不能容纳这个三元表达式，则把它拆成多行。
// 正例
ClassWithLongName objectWithLongName = booleanMethodWithLongName() ?
        new ClassWithLongName(parameter) : new ClassWithLongName(anotherParameter);

// 正例
ClassWithLongName objectWithLongName = booleanMethodWithLongName() ?
        new ClassWithLongName(parameter) :
        new ClassWithLongName(anotherParameter);

// 正例
ClassWithLongName objectWithLongName =
        booleanMethodWithLongName() ?
        new ClassWithLongName(parameter) :
        new ClassWithLongName(anotherParameter);

// 反例
// 三元表达式里有两个构造器，应该优先对外层的三元表达式换行，而不是内层的构造器。
ClassWithLongName objectWithLongName = booleanMethodWithLongName() ? new ClassWithLongName(
        parameter) : new ClassWithLongName(anotherParameter);
```
## 3.3 空格
### 3.3.1 普通语句
非缩进处使用1个空格。
1. `.`和`::`的前后，以及句尾的`;`之前不应有空格。
```java
// 正例
something.doSomething(something::aMethod);

// 反例
something . doSomething(something :: aMethod) ;
```
2. 声明语句的各部分之间用1个空格分隔。
```java
// 正例
public class User {
    // 无需添加空格使单词对齐
    private long id;
    private String name;
    private int age;
}

// 反例
public  class  User {// 空格太多了
    // 这样不方便维护
    private long   id;
    private String name;
    private int    age;
}
```
3. `(`之后和`)`之前不应有空格。
```java
// 正例
doSomething();

// 反例
doSomething( );
```
4. 方法参数表的`(...)`前后不应有空格。
```java
// 正例
abstract void doSomething();
doSomething();

// 反例
abstract void doSomething ();
doSomething ();
```
5. 所有二目和三目运算符跟其运算数之间用1个空格分隔。
```java
// 正例
oneDayMillis = 24 * 60 * 60 * 1000;
return value == null ? "" : value;

// 反例
oneDayMillis=24*60*60*1000;
return value==null?"":value;
```
6. 数组初始化的`{`前不应添加空格。
```java
// 正例
int[] array = new int[]{1, 2, 3};
int array = {1, 2, 3};// 二目的赋值号跟其左右值之间用1个空格分隔

// 反例
int[] array = new int[] {1, 2, 3};
int array ={1, 2, 3};
```
7. lambda表达式的`->`左右各有1个空格
```java
// 正例
() -> doSomething()
() -> {
    doSomething();
    doSomethingElse();
}

// 反例
()->doSomething()
()->{
    doSomething();
    doSomethingElse();
}
```
8. 控制语句的关键字，`(...)`和`{...}`之间用1个空格分隔。
```java
// 正例
do {
    if (condition()) {
        doSomething();
    } else if (otherCondition()) {
        doSomethingElse();
    } else {
        doLastThing();
    }
} while (loopCondition());
try {
    return someValue();
} catch (SomeException e) {
    return defaultValue();
}

// 反例
if(condition()){
    doSomething();
}
```
9. 当`;`或`,`出现在非句尾位置时，其前不应有空格，其后应该有1个空格。
```java
// 正例
for (int i = 0; i < max; i++) {
    doSomething(i, max);// 句尾没有空格
}

// 反例
for (int i = 0;i < max;i++) {
    doSomething(i,max);
}
for (int i = 0 ;i < max ;i++) {
    doSomething(i ,max);
}
for (int i = 0 ; i < max ; i++) {
    doSomething(i , max);
}
```
10. for-each语句中的`:`前后各应该有1个空格。
```java
// 正例
for (int i : values) {
    doSomething();
}

// 反例
for (int i:values) {
    doSomething();
}
```
## 3.4 大括号
1. 控制语句必须使用大括号，即使其代码块只有一条语句。
2. 大括号遵循K & R Style，即在左大括号后，右大括号前换行。
```java
// 正例
public class MyClass {
    public Runnable method() {
        return () -> {
            if (condition()) {
                try {
                    doSomething();
                } catch (SomeException e) {
                    rollback();
                } finally {
                    cleanUp();
                }
            } else if (otherCondition()) {
                synchronized(MyClass.class) {
                    doSomethingElse();
                }
            } else {
                doLastThing();
            }
        }
    }
}

// 反例
if (condition())
    doSomething();
else if (otherCondition())
{
    doSomethingElse();
}
else
    doLastThing();
```
3. 特殊情况下可能出现这样的代码块，`{`和`}`应各占1行。
```java
// 正例
{
    int result = method(0);
    Assert.assertEquals(0, result);
}
{
    int result = method(1);
    Assert.assertEquals(1, result);
}

// 反例
{
    int result = method(0);
    Assert.assertEquals(0, result);
} {
    int result = method(1);
    Assert.assertEquals(1, result);
}
```
## 3.5 小括号
在复杂表达式中使用小括号来明确优先级。