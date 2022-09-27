---
title: Java
categories:
- tech
---

# IDEA

| 快捷键               | 功能                                 |
| -------------------- | ------------------------------------ |
| `Alt+Enter`          | 导入包、自动修正代码、接口自动载入   |
| `Ctrl+Y`             | 删除光标所在行                       |
| `Ctrl+D`             | 复制光标所在行内容，插入光标位置下面 |
| `Ctrl+Alt+L`         | 格式化代码                           |
| `Ctrl+Shift+/`       | 选中代码注释，多行注释，再按取消注释 |
| `Ctrl+n`             | 搜索包                               |
| `Alt+Ins`            | 自动生成代码，toString,get,set等方法 |
| `Alt+Shift+上下箭头` | 移动当前代码行                       |
| `ctrl+f12`           | 在类中搜索方法或常量                 |
| `alt + shift + e`    | 批量修改名称                         |
| `alt+6`              | 查找TODO注释                         |

idea中，默认当前路径是project的根目录

| Java速写缩略语 | 代码                                     |
| -------------- | ---------------------------------------- |
| psvm           | public static void main(String[] args){} |
| sout           | System.out()                             |
| array.fori     | 数组正序遍历                             |
| array.forr     | 数组倒序遍历                             |

# Java

## 数据类型四类八种

| 整形       | **byte(1)、short(2)、int(4) 、long(8)** |
| ---------- | --------------------------------------- |
| **浮点型** | **float(4)、double(8)**                 |
| **字符型** | **char(2)**                             |
| **布尔型** | **boolean(1)**                          |

整数可以表示为：123_221_333类似于书面中的逗号123,221,333

| 基本类型 | 包装类    |
| -------- | --------- |
| byte     | Byte      |
| short    | Short     |
| int      | Integer   |
| long     | Long      |
| float    | Float     |
| double   | Double    |
| char     | Character |
| boolean  | Boolean   |

包装类都在java.lang包下



## 编译器的两种优化

- 变量的常量优化
- 对于byte/short/char三种类型，若右侧数值未超范围，则自动补上(byte/short/char)，即强制转移

## switch语句

1. 多个case后面的数值不可以重复

2. switch后面小括号中只能是下列数据类型

   基本数据类型：byte/short/char/int

   引用数据类型：String字符串、enum枚举

## 重载

1. 参数个数不同
2. 参数类型不同
3. 参数的多类型顺序不同

## 数组

格式：数据类型[] 数组名称 = new 数据类型[数组长度]{元素1、元素2...}

```java
//静态初始化
int[] arrayA = new int[300]{element1、element2、...};
int[] arrayB = {element1、element2、...};
//动态初始化
int[] arrayA = new int[300];
```

初始化数组有两种格式：标准格式/省略格式

一维数组的输出：System.out.println(Arrays.toString(array))

多维数组的输出：System.out.println(Arrays.deepToString(array))

建立空数组时，数组内的数据会有默认值。

- 整数类型，默认为0
- 浮点类型，默认为0.0
- 字符类型，默认为"\u0000"
- 引用类型，默认为null

---

> 一个数组的内存图

![image-20200803212355839](C:\Users\Jet\AppData\Roaming\Typora\typora-user-images\image-20200803212355839.png)

---

数组一旦创建，长度不可再改变

> 数组方法

`array.length`：得到数组长度

`array.fori`：快捷生成数组遍历

> 数组调用

```java
public static void printArray(int[] array){}
```

> 对象数组

类名[] array = new 类名[length]

### Arrays类工具

`java.util.Arrays`是一个与数组相关的工具类，里面提供了大量静态方法，用来实现数组常见的操作。

`public static string tostring(数组)`：将参数数组变成字符串（按照默认格式：[元素1，元素2，元素3...]）
`public static void sort(数组)`：按照默认升序（从小到大）对数组的元素进行排序。

1. 如果是数值，sort默认按照升序从小到大
2. 如果是字符串，sort默认按照字母升序
3. 如果是自定义的类型，那么这个自定义的类需要有Comparable或者Comparator接口的支持。

## 字符串

- 字符串的内容并不能被改变
- String str所创建的str中存放的是字符串的地址值

```java
//字符串和数字相加的类型转换
String x = "1.1";
double y = Double.parseDouble(x) + 2 
//字符串的创建(3+1)
public static void main(String[]args){
    //使用空参构造
    String strl =new String()；//小括号留空，说明字符串什么内容都没有。
    System.out.println(“第1个字符串：“+str1);
    //根据字符数组创建字符串
    char[] charArray={'A','B','C'};
    String str2=new String（charArray）;
    System.out.println(“第2个字符串：“+str2);
    //根据字节数组创建字符串
    byte[]byteArray={97，98，99};
    String str3=new String（byteArray）;
    System.out.println(“第3个字符串：“+str3);
    //直接创建
    String str4="Hello";
    system.out.printin(“第4个字符串：“+str4);
}}
```

> 字符串的内存调用

1. 对于引用类型来说，==进行的是地址值的比较
2. 双引号直接写的字符串在常量池当中，new的不在池当中

![image-20200809190009177](C:\Users\Jet\AppData\Roaming\Typora\typora-user-images\image-20200809190009177.png)



> 方法

`String.equals(str)`：对比两个字符串内容是否相等，推荐将常量写在前面

`String.equalsIgnoreCase(str)`：忽略大小写进行比较

`public int Length()`：获取字符串当中含有的字符个数，拿到字符串长度。

`public String concat(String str)`：将当前字符串和参数字符串拼接成为返回值新的字符串。

`public char charAt(int index)`：获取指定索引位置的单个字符。

`public int indexof(String str)`：查找参数字符串在本字符串当中首次出现的索引位置，如果没有返回-1值。

`public String substring(int index)`：截取从参数位置一直到字符串末尾，返回新字符串。

`public String substring(int begin，int end)`：截取从begin开始，一直到end结束，中间的字符串。备注：[begin，end），包含左边，不包含右边。

`public char[]tocharArray()`：将当前字符串拆分成为字符数组作为返回值。

`public byte[]getBytes()`：获得当前字符串底层的字节数组。

`public String replace(Charsequence oldString，Charsequence newstring)`：将所有出现的老字符串替换成为新的字符串，返回替换之后的结果新字符串。

`public String[] split(String regex)`：按照参数的规则，将字符串切分成为若干部分。

<u>注意事项</u>：split方法的参数其实是一个正则表达式”，今后学习。今天要注意：如果按照英文句点“.”进行切分，必须写"\\\\."

## 系统输入输出

```java
#系统输入
Scanner scanner = new Scanner(System.in);
byte age = scanner.nextByte();
#系统输出
System.out
```

## 类的定义

一个标准的类通常要有下面四个组成部分

1. 所有的成员变量都要使用private关键字修饰
2. 为每一个成员变量编写一对Getter/Setter方法
3. 编写一个无参数的构造方法

```java
public class Student {
    String name;
    int age;
    private boolean male;  //定义private关键字
    
    //---------------------------------//
    //为private关键字定义getter/setter方法
    //对于boolean类型，其getter方法要用isXxx()
    public void setMale(boolean b) {
        male = b;
    }
    public void isMale() {
        return male;
    }
	//---------------------------------//
    public void SetName(String str) {
        name = str;
    }
    public String getName() {
        return name;
    }
    //---------------------------------//
    public void eat(){}
    public void sleep(){}
}
```

### 抽象类

```java
//抽象类
public abstract class Animal{
    //抽象方法
    public abstract void eat();
}
```

子类必须覆盖重写父类的抽象方法

关于抽象类的使用，以下为语法上要注意的细节，虽然条目较多，但若理解了抽象的本质，无需死记硬背。

1. 抽象类不能创建对象，如果创建，编译无法通过而报错。只能创建其非抽象子类的对象。

> 理解：假设创建了抽象类的对象，调用抽象的方法，而抽象方法没有具体的方法体，没有意义。

2. 抽象类中，可以有构造方法是供子类创建对象时，初始化父类成员使用的。

> 理解：子类的构造方法中，有默认的super()，需要访问父类构造方法。

3. 抽象类中，不一定包含抽象方法，但是有抽象方法的类必定是抽象类。

> 理解：未包含抽象方法的抽象类，目的就是不想让调用者创建该类对象，通常用于某些特殊的类结构设计。

4. 抽象类的子类，必须重写抽象父类中所有的抽象方法，否则，编译无法通过而报错。除非该子类也是抽象类。

> 理解：假设不重写所有抽象方法，则类中可能包含抽象方法。那么创建对象后，调用抽象的方法，没有意义。

### 内部类

定义在一个类内部的类

1. 成员内部类
2. 局部内部类（包含匿名内部类）

#### 成员内部类

```java
成员内部类的定义格式：
修饰符 class 外部类名称{
	修饰符 cLass 内部类名称{
//…
}}
…
```

注意：内用外，随意访问；外用内，需要内部类对象

> 如何使用成员内部类？有两种方式：

1. 间接方式：在外部类的方法当中，使用内部类；然后main只是调用外部类的方法。

2. 直接方式，公式：
   类名称 对象名 = new 类名称()；

`【外部类名称.内部类名称 对象名 = new 外部类名称().new 内部类名称();】`

> 如果出现了重名现象，格式为：外部类.this.外部类成员变量名

```java
public class Outer{
	int num=10；//外部类的成员变量
	public class Inner{
		int num=2e；//内部类的成员变量
		public void methodInner(){
			int num=30；//内部类方法的局部变量
                System.out.print1n（num）；//局部变量，就近原则
                System.out.print1n（this.num）；//内部类的成员变量
                System.out.print1n（Outer.this.num）；//外部类的成员变量
        }}}             
```

#### 局部内部类

如果一个类是定义在一个方法内部的，那么这就是一个局部内部类。
“局部”：只有当前所属的方法才能使用它，出了这个方法外面就不能用了。

定义格式：

```java
修饰符 class 外部类名称{
    修饰符 返回值 类型 外部类方法名称(参数列表){
        class局部内部类名称（
			//…
}}}
```

> 类的权限修饰符：

public>protected>（default）>private定义一个类的时候，权限修饰符规则：

1. 外部类：public/（default）

2. 成员内部类：public/protected/（default）/private

3. 局部内部类：什么都不能写

> 局部内部类，如果希望访间所在方法的局部变量，那么这个局部变量必须是【有效final的】。即只赋值一次

备注：从Java 8+开始，只要局部变量事实不变，那么final关键字可以省略。
原因：

1. new出来的对象在堆内存当中。

2. 局部变量是跟着方法走的，在栈内存当中。

3. 方法运行结束之后，立刻出栈，局部变量就会立刻消失。

4. 但是new出来的对象会在堆当中持续存在，直到垃圾回收消失。所以需要不变的变量

#### 匿名内部类

如果接口的实现类（或者是父类的子类）只需要使用唯一的一次，那么这种情况下就可以省略掉该类的定义，而改为使用【匿名内部类】。

匿名内部类的定义格式：
`接口名称 对象名 = new 接口名称(){//覆盖重写所有抽象方法}`

对格式`new接口名称(){...}`进行解析：
1.new代表创建对象的动作

2.接口名称就是匿名内部类需要实现哪个接口

3.{.…}这才是匿名内部类的内容

另外还要注意几点问题：

1. 匿名内部类，在【创建对象】的时候，只能使用唯一一次。如果希望多次创建对象，而且类的内容一样的话，那么就必须使用单独定义的实现类了。

2. 匿名对象，在【调用方法】的时候，只能调用唯一一次。如果希望同一个对象，调用次方法，那么必须给对象起个名字。

3. 匿名内部类是省略了【实现类/子类名称】，但是匿名对象是省略了【对象名称】

强调：匿名内部类和匿名对象不是一回事！！！

#### 静态内部类



## 面向对象

> 一个对象的内存图

![image-20200804210233751](C:\Users\Jet\AppData\Roaming\Typora\typora-user-images\image-20200804210233751.png)

### 三大特征

封装性、继承性、多态性

#### 继承

Java中只有单继承

![image-20200813182824341](C:\Users\Jet\AppData\Roaming\Typora\typora-user-images\image-20200813182824341.png)

`public class 子类 extends 父类`

> 继承关系中，父子类构造方法的访间特点

1. 子类构造方法当中有一个默认隐含的“super（）“调用，所以一定是先调用的父类构造，后执行的子类构造。

2. 子类构造可以通过super关键字来调用父类重载构造。

3. super的父类构造调用，必须是子类构造方法的第一个语句。不能一个子类构造调用多次super构造。

- 总结：
  子类必须调用父类构造方法，不写则赠送super（）；写了则用写的指定的super调用，super只能有一个，还必须是第一个。

在父子类的继承关系当中，如果成员变量重名，则创建子类对象时，访间有两种方式：

1. 直接通过子类对象访问成员变量：

- 等号左边是谁，就优先用谁，没有则向上找

2. 间接通过成员方法访问成员变量：

- 该方法属于谁，就优先用谁，没有则向上找。

> 子类方法中重名的三种变量的访问

- 局部变量：直接访问
- 子类变量：this.name
- 父类变量：super.name

> 继承中的覆盖

方法覆盖重写的注意事项：
1. 必须保证父子类之间方法的名称相同，参数列表也相同。
   `@Override`：写在方法前面，用来检测是不是有效的正确覆盖重写。
   这个注解就算不写，只要满足要求，也是正确的方法覆盖重写。

2. 子类方法的返回值必须【小于等于】父类方法的返回值范围。
   小扩展提示：`java.Lang.object`类是所有类的公共最高父类（祖宗类），`java.Lang.string`就是object的子类。

3. 子类方法的权限必须【大于等于】父类方法的权限修饰符。
   小扩展提示：`public`>`protected`>(default)>`private`备注：（default）不是关键字`default`，而是什么都不写，留空。

![image-20200813233711604](C:\Users\Jet\AppData\Roaming\Typora\typora-user-images\image-20200813233711604.png)

#### 多态

代码中体现多态性：父类引用指向子类对象

格式：

父类名称 对象名 = new 子类名称()；

或：

接口名称 对象名 = new 实现类名称();

>多态中访问成员变量

- 直接通过对象名称访问，看等号左边是谁，优先用谁，没有则向上找
- 间接通过成员方法访问，看该方法属于谁，优先用谁，没有则向上找

> 多态中访问成员方法

看new得到是谁，就优先用谁，没有则向上找

---

口诀：（对于定义对象语句）

- 成员变量：编译看左边，运行还看左边
- 成员方法：编译看左边，运行看右边

> 为什么用多态？

![image-20200817201232145](C:\Users\Jet\AppData\Roaming\Typora\typora-user-images\image-20200817201232145.png)

比较统一

## 变量

局部变量和成员变量

1. **定义的位置不一样**
   局部变量：在方法的内部
   成员变量：在方法的外部，直接写在类当中

2. **作用范围不一样**
   局部变量：只有方法当中才可以使用，出了方法就不能再用成员变量：整个类全都可以通用。

3. **默认值不一样**
   局部变量：没有默认值，如果要想使用，必须手动进行赋值成员变量：如果没有赋值，会有默认值，规则和数组一样

4. 内存的位置不一样（了解）

   局部变量：位于栈内存；成员变量：位于堆内存

5. 生命周期不一样（了解）

   局部变量：随着方法进栈而诞生，随着方法出栈而消失

   成员变量，随着对象创建而诞生，随着对象呗垃圾回收而消失

## 方法

三要素：方法名、参数列表、返回值

### 构造方法

public 类名称(参数类型 参数名称) {

​		方法体

}

```jav
public class Student {

	public Student() {
	
	}
}
```

## 匿名对象

```java
new 类名();

public class Demoe2Anonymous{
    public static void main（String[]args）{
        //普通使用方式
        Scanner sc=new Scanner（System.in）；
        int num=sc.nextInt（）；
        //匿名对象的方式
        int num=new Scanner（System.in）.nextInt（）；
        System.out.printtn（"输入的是："+num）；
        //使用一般写法传入参数
        Scanner sc=new Scanner（System.in）；
        methodParam（sc）；
        //使用匿名对象来进行传参
        methodParam（new Scanner（System.in））；

        Scanner sc=methodReturn（）；
        int num=sc.nextInt（）；
        System.out.println（“输入的是："+num）；
    }
    public static void methodParam（Scanner sc）{
    	int num=sc.nextInt（）；
        System.out.println（“输入的是：“+num）；
    }
    public static Scanner methodReturn（）{
    	//Scanner sc=new Scanner（System.in）；
        return new Scanne(System.in);
```

## 集合

### ArrayList集合、泛型

其长度并没有限制

<>中写的是泛型，泛型只能是引用类型（包装类），不能是基本类型

```java
ArrayList<String> list = new ArrayList<>();
public static void main(String[] args){
        System.out.println("Hello world");
        ArrayList<String> list =new ArrayList<String>();
        //添加元素
        boolean success = list.add("赵又廷");
        System.out.println(success);
        list.add("迪丽热巴");
        System.out.println(list);
        //删除某元素
        String who = list.remove(1);
        System.out.printf("将%s删除",who);
        System.out.println("\n"+list);
        //获取某元素
        String name = list.get(0);
        System.out.println(name);
    	//获取集合长度
    	int size = list.size();
    }
```

## 关键字

### `static`

- 内容不再属于对象，而属于类

静态变量：类名称，静态变量
静态方法：类名称.静态方法（）

注意事项：

1. 静态不能直接访问非静态。
   原因：因为在内存当中是【先】有的静态内容，【后】有的非静态内容。
   “先人不知道后人，但是后人知道先人。”

2. 静态方法当中不能用this。
   原因：this代表当前对象，通过谁调用的方法，谁就是当前对象。

```java
public class Demoe2StaticMethod{
	public static void main（String[]args）{
        MyClass obj=new MyClass()；//首先创建对象
        //然后才能使用没有static关键字的内容
        obj.method()；
        //对干静态方法来说，可以通过对象名进行调用，也可以直接通过类名称来调用。
        obj.methodstatic()；//正确，不推荐，这种写法在编译之后也会被javac翻译成为“类名称.静态方法名”
        MyClass.methodStatic()；//正确，推荐
        //对于本来当中的静态方法，可以省略类名称
        myNethod()；
        Demoe2StaticMethod.myMethod()；//完全等效
        public static void myMethod(){
        System.out.printin("自己的方法！")；
}}
```

> static内存图

![image-20200813100228928](C:\Users\Jet\AppData\Roaming\Typora\typora-user-images\image-20200813100228928.png)

> 静态代码块

```java
//静态代码块的内容
public class 类名称{
    static{}
}
```

特点：

- 当第一次用到本类时，静态代码块执行唯一的一次。
- 静态内容总是优先于非静态，所以静态代码块比构造方法先执行。

### `super`

`super`关键字的用法有三种：

1. 在子类的成员方法中，访问父类的成员变量。

2. 在子类的成员方法中，访问父类的成员方法。

3. 在子类的构造方法中，访问父类的构造方法。`super()`

### `this`

`super`关键字用来访问父类内容，而`this`关键字用来访间本类内容。用法也有三种：

1.在本类的成员方法中，访问本类的成员变量。

2.在本类的成员方法中，访问本类的另一个成员方法。

3.在本类的构造方法中，访问本类的另一个构造方法。`this()`

在第三种用法当中要注意：

- `this(…)`调用也必须是构造方法的第一个语句。
- super和this两种构造调用，不能同时使用

### `instanceof`

`对象 instanceof 类名称`：会返回一个boolean值结果，也就是判断前面的对象能不能当作后面类型的实例。

### `final`

`final`关键字代表最终、不可改变的

1. 修饰一个类

   ```java
   public final class 类名称{
       //...
   }
   ```

   此类不能有任何子类

2. 修饰一个方法

   ```java
   public final void method(){
   	//...
   }
   ```

   该方法不能被覆盖重写

3. 修饰一个局部变量

   ```java
   final in num = 100;//仅能有唯一一次赋值
   ```

   该变量不能再进行更改

   对基本类型来说，其值不可变

   对引用类型来说，其地址不可变 

4. 修饰一个成员变量

   - 使用`final`之后必须手动赋值，不会再给默认值
   - 可以进行直接赋值或通过构造方法赋值
   - 必须保证类当中所有重载的构造方法，都最终会对`final`的成员变量进行赋值

## 常用类

### Math类

`java.lang.Math`类是数学相关的工具类，里面提供了大量的静态方法，完成与数学运算相关的操作

`public static double abs(double num)`：获取绝对值。有多种重载

`public static double ceil(double num)`：向上取整

`peblic static double floor(double num)`：向下取整

`public static Long round(double num)`：四舍五入

## 接口

接口就是多个类的公共规范。
接口是一种引用数据类型，最重要的内容就是其中的：抽象方法

如何定义一个接口的格式：
public interface 接口名称（
	//接口内容
}
<u>备注</u>：换成了关键字interface之后，编译生成的字节码文件仍然是：java-->.class。

> 如果是Java7，那么接口中可以包含的内容有：

1. 常量

2. 抽象方法

> 如果是Java8，还可以额外包含有：

3. 默认方法

4. 静态方法

> 如果是Java9，还可以额外包含有：

5. 私有方法

### 接口定义

在任何版本的Java中，接口都能定义抽象方法。
格式：

public abstract 返回值类型方法名称（参数列表）；注意事项：

1. 接口当中的抽象方法，修饰符必须是两个固定的关键字：public abstract

2. 这两个关键字修饰符，可以选择性地省略。（今天刚学，所以不推荐。）

3. 方法的三要素，可以随意定义。

```java
public interface MyInterfaceAbstract{
    //这是一个抽象方法
    public abstract void methodAbs1（）；
    //这也是抽象方法
    abstract void methodAbs2（）；
    //这也是抽象方法
    public void methodAbs3（）；
    //这也是抽象方法
    void methodAbs4（）；
}
```

### 接口实现

接口使用步骤：

1. 接口不能直接使用，必须有一个实现类来实现该接口。

格式：

```java
public class 实现类名称 implements 接口名称{
    //...
}
```



2. 接口的实现类必须覆盖重写（实现）接口中所有的抽象方法。
   实现：去掉abstract关键字，加上方法体大括号。
3. 创建实现类的对象，进行使用。
4. 若不想重写所有方法，则该实现类必须是抽象类

### 接口的默认方法

```java
public default void methodDefault(){
    //
}
```

使用默认方法，在接口升级时，可避免没有重写该方法的实现类报错

### 接口的静态方法

```java
public static void methodstatic(){
    //
}
```

<u>注意事项</u>：不能通过接口实现类的对象来调用接囗当中的静态方法。

正确用法：通过接口名称，直接调用其中的静态方法。

格式：接口名称.静态方法名（参数）；

### 接口的常量定义

接囗当中也可以定义“成员变量”，但是必须使用public static final三个关键字进行修饰。

从效果上看，这其实就是接口的【常量】

格式：
`public static final 数据类型常量名称=数据值`；

备注：
一旦使用final关键字进行修饰，说明不可改变。

注意事项：

1. 接口当中的常量，可以省略public static final，注意：不写也照样是这样。
2. 接口当中的常量，必须进行赋值；不能不赋值。
3. 接口中常量使用完全大写的命名，并用下划线进行分割

### 继承父类并实现接口

```java
public class Zi extends Fu implements MyInterface {
    
}
```

使用接口的时候，需要注意：

1. 接口是没有静态代码块或者构造方法的。

2. 一个类的直接父类是唯一的，但是一个类可以同时实现多个接口。

格式：

```java
public class MyInterfaceImpl implements MyInterfaceA，MyInterfaceB{
//覆盖重写所有抽象方法
}
```



3. 如果实现类所实现的多个接口当中，存在重复的抽象方法，那么只需要覆盖重写一次即可

4. 如果实现类没有覆盖重写所有接口当中的所有抽象方法，那么实现类就必须是一个抽象类

5. 如果实现类所实现的多个接口当中，存在重复的默认方法，那么实现类一定要对冲突的默认方法进行覆盖重写

6. 一个类如果直接父类当中的方法，和接口当中的默认方法产生了冲突，优先用父类当中的方法



### 对象的转型

> 对象的向上转型

对象的向上转型，其实就是多态写法：

格式：`父类名称对象名=new子类名称()`；`Animal animal =new Cat()`；

含义：右侧创建一个子类对象，把它当做父类来看待使用。创建了一只猫，当做动物看待，没问题。

注意事项：向上转型一定是安全的。从小范围转向了大范围，从小范围的猫，向上转换成为更大范围的动物。

类似于：`double num=100`;//正确，int-->double，自动类型转换。

> 对象的向下转型

2.对象的向下转型，其实是一个【还原】的动作。

格式：子类名称对象名=（子类名称）父类对象；

含义：将父类对象，【还原】成为本来的子类对象。

`Animal animal=new Cat()`；//本来是猫，向上转型成为动物

`Cat cat=(Cat)animal`；//本来是猫，已经被当做动物了，还原回来成为本来的猫

注意事项：
a. 必须保证对象本来创建的时候，就是猫，才能向下转型成为猫。

b. 如果对象创建的时候本来不是猫，现在非要向下转型成为猫，就会报错。

类似于：`int num=(int)10.0`；//可以     int num=(int)10.5；//不可以，精度损失

## 四种权限修饰符

| 能否使用     | public  > | protected  > | (default  > | private |
| ------------ | --------- | ------------ | ----------- | ------- |
| 同一个类     | √         | √            | √           | √       |
| 同一个包     | √         | √            | √           | ×       |
| 不同包子类   | √         | √            | ×           | ×       |
| 不同包非子类 | √         | ×            | ×           | ×       |

### 更新和删除数据

> 更新UPDATE

```mysql
UPDATE IGNORE customers
SET cust_name  = 'The Fudds',
	cust_email = 'elmer@fudd.com'
WHERE cust_id = 10005;
/* IGNORE 关键字表示当发生错误时，整个UPDATE操作被取消*/
```

> 删除

```mysql
/*DELETE*/
DELETE FROM custmers
WHERE cust_id = 10006;
/*TRUNCATE 清空某张表*/
TRUNCATE TABLE 
```

### 异常处理

```java
//打印异常树
try{
    ...
}catch(Exception e){
    e.printStackTrace();
}
```

