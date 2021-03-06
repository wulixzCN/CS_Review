> # JavaSE 复习
>
> #`第一章 Java语言概述`
>
> <img src="/Users/zhenxu2/JAVA/review_prcture/java_picture/java1.png" alt="1" style="zoom:50%;" />

##`Java与C/C++的异同`

> ​        Java与C/C++相同之处：Java确实是从C语言和C++语言继承了许多成份，甚至可以将Java看成是**类C**语言发展和衍生的产物。比如Java语言的变量声明，操作符形式，参数传递，流程控制等方面和C语言、C++语言完全相同。Java是一个**纯粹的面向对象**的程序设计语言，它继承了C++语言面向对象技术的核心
>
> ​        Java与C/C++不同之处：Java舍弃了C语言中容易引起错误的指针（以引用取代）、运算符重载（operator overloading）、多重继承（以接口取代）等特性，增加了垃圾回收器功能用于回收不再被引用的对象所占据的内存空间。JDK1.5又引入了泛型编程（Generic Programming）、类型安全的枚举、不定长参数和自动装/拆箱

##`Java语言的特点`

- 特点一： 面向对象

  ​         两个基本概念：类、对象

​			    三大特性：封装、继承、多态

- 特点二：**健壮性 完善性**

  ​		吸收了C/C++语言的优点，但去掉了其影响程序健壮性的部分（如指针、内存的申请与释放等），提供了一个相对安全的内存管理和访问机制 

- 特点三：**跨平台性** **jvm**

  跨平台性：通过Java语言编写的应用程序在不同的系统平台上都可以运行。**“Write once , Run Anywhere”，一次编写，处处运行**

##`JVM、JRE、JDK关系`

<img src="/Users/zhenxu2/JAVA/review_prcture/java_picture/java2.png" alt="2" style="zoom:50%;" />

----

---

> ### 第二章 Java基本语法
>
> ####`关键字`
>
> <img src="/Users/zhenxu2/JAVA/review_prcture/java_picture/java3.png" alt="2" style="zoom:50%;" />
>
> #### `用于定义访问权限修饰符的关键字`
>
> <img src="/Users/zhenxu2/JAVA/review_prcture/java_picture/java4.png" alt="java4" style="zoom:30%;" />
>
> 
>
> #### `命名规范`
>
> Ø**包名**：多单词组成时所有字母都小写：xxxyyyzzz
>
> Ø**类名、接口名**：多单词组成时，所有单词的首字母大写：XxxYyyZzz
>
> Ø**变量名、方法名**：多单词组成时，第一个单词首字母小写，第二个单词开始每个单词首字母大写：xxxYyyZzz
>
> Ø**常量名**：所有字母都大写。多单词时每个单词用下划线连接：XXX_YYY_ZZZ
>
> 
>
> **在方法体外，类体内声明的变量称为成员变量。**
>
> **在方法体内部声明的变量称为局部变量。**
>
> <img src="/Users/zhenxu2/JAVA/review_prcture/java_picture/java5.png" alt="java5" style="zoom:30%;" />
>
> *_`二者在初始化方面的异同：`_*
>
> **同：**都有生命周期      **异：**局部变量除形参外，需显式初始化。



---

---

---

### boolean类型不可以转换为其它的数据类型

---

---

# `第三章 面向对象编程`

## `面向对象`

> 面向对象的三个特征
>
> - 封装 （Encapsulation）
> - 继承 （Inheritance）
> - 多态 （Polymorphism）
>
> **`Java创建的对象在堆里的 Eden区`**

## `成员变量(属性)和局部变量的区别`

>- 成员变量：
>
>  Ø成员变量定义在类中，在整个类中都可以被访问。
>
>  Ø成员变量分为类成员变量和实例成员变量，实例变量存在于对象所在的堆内存中。
>
>  Ø成员变量有默认初始化值。
>
>  Ø成员变量的权限修饰符可以根据需要，选择任意一个
>
>- 局部变量：
>
>  Ø局部变量只定义在局部范围内，如：方法内，代码块内等。
>
>  Ø局部变量存在于栈内存中。
>
>  Ø作用的范围结束，变量空间会自动释放。
>
>  Ø局部变量没有默认初始化值，每次必须显式初始化。
>
>  Ø局部变量声明时不指定权限修饰符

## `方法的重载`

> - 重载的概念：
>
>   在同一个类中，允许存在一个以上的同名方法，只要它们的参数个数或者参数类型不同即可
>
> - 重载的特点：
>
>   与返回值类型无关，只看**参数列表**，且是**参数列表必须不同**。(参数个数或参数类型)。调用时，根据方法参数列表的不同来区别。

## `方法的参数传递`

> 方法必须**有其所在类或对象调用才有意义**
>
> Ø**形参**：方法声明时的参数
>
> Ø**实参：**方法调用时实际传给形参的参数值
>
> 
>
> _Java的实参值如何传入方法呢？_
>
> - 值传递
>
>   参数类型是int，long等基本数据类型（八大基本数据类型），参数传递的过程采用值拷贝的方式
>
> - 引用传递
>
>   参数类型为引用类型，参数传递的过程采用拷贝引用的方式
>
> **结论：按值传递，不会改变原来的值，引用传递，会改变引用对象的值**
>
> ```java
>   public static void main(String[] args) {
>         Integer a = 5;
>         fun(a);
>         System.out.println(a);// 输出结果为5
>     }
>  
>     private static void fun(Integer a) {
>         a += 1;
>     }
> 
> //注意这里，虽然Integer是 引用类型，但是存在拆箱的过程
> 源码：
>     /**
>      * @param  i an {@code int} value.
>      * @return an {@code Integer} instance representing {@code i}.
>      * @since  1.5
>      */
>     public static Integer valueOf(int i) {
>         if (i >= IntegerCache.low && i <= IntegerCache.high)
>             return IntegerCache.cache[i + (-IntegerCache.low)];
>         return new Integer(i);
>     }
>     //这里没有改变原对象的值，只是将其引用指向了另一个对象，直接改变了栈帧中的地址。
> 
> ```
>
> 

## `Jvm内存模型 `

> **此处为简易版，具体看 Jvm Review专题**
>
> <img src="/Users/zhenxu2/JAVA/review_prcture/java_picture/java6.png" alt="java6" style="zoom:30%;" />
>
> 所以再看 方法的参数传递：
>
> 1. 如果方法的形参是基本数据类型，那么实参(实际的数据)向形参传递参数时，就是直接值传递，把实参的值复制给形参。
>
> 2. 如果方法的形参是对象，那么实参(实际的对象)，向形参传递参数的时候，也是把值传给形参，这个**值** 是实参在栈内存中的值，也就是引用对象在堆内存中的地址。
>
>    **Sum**： 基本数据类型都是保存在栈内存中，引用对象在栈内存中保存的是引用对象的地址，那么方法的参数传递是传递值(是变量在栈内存中的值)。

## `关键字 this`

> this是什么？
>
> - Ø它在方法内部使用，即这个方法所属对象的引用；
>
>   Ø它在构造器内部使用，表示该构造器正在初始化的对象。
>
> - this表示当前对象，可以调用类的属性、方法和构造器
>
> **注意：**
>
> - 使用**this()** **必须放在构造器的首行！**
> - 使用**this** **调用本类中其他的构造器，保证至少有一个构造器是不用** **this** **的。（实际上就是不能出现构造器自己调用自己）**

# `JavaBean`

> - JavaBean是一种Java语言写成的可重用组件
>
> - 所谓JavaBean，是指符合如下标准的Java类：
>
>   Ø类是公共的
>
>   Ø有一个无参的公共的构造器
>
>   Ø有属性，属性一般是私有的，且有对应的get、set方法
>
> - 可以使用JavaBean将功能、处理、值、数据库访问和其他任何可以用java代码创造的对象进行打包，并且其他的开发者可以通过内部的JSP页面、Servlet、其他JavaBean、applet程序或者应用来使用这些对象。可以认为JavaBean提供了一种随时随地的复制和粘贴的功能，而不用关心任何改变。

## `关键字 static`

> **如果想让一个类的所有实例共享数据，就用类变量**
>
>  |  使用范围：
>
> ​        Ø在Java类中，可用static修饰属性、方法、代码块、内部类
>
>  | 被修饰后的成员具备以下特点：
>
> ​		Ø随着类的加载而加载
>
> ​		Ø优先于对象存在
>
> ​		Ø修饰的成员，被所有对象所共享
>
> ​		Ø访问权限允许时，可不创建对象，直接被类调用
>
> 用例：
>
> 1. **单例(Singleton)模式**
>
> 2. **java.lang.Runtime**
>
>    ```python
>    public class Runtime {
>        private static Runtime currentRuntime = new Runtime();
>    
>        public static Runtime getRuntime() {
>            return currentRuntime;
>        }
>      
>        /** Don't let anyone else instantiate this class */
>        private Runtime() {}
>    }
>    ```

## `类的成员： 初始化块`

> |初始化块(代码块)作用：
>
> ​		Ø**对Java对象进行初始化**
>
> 
>
> |程序的执行顺序：
>
> 		1. 声明成员变量的默认值
>   		2. 显式初始化、多个初始化块一次被执行（同级别下按先后顺序执行）
>   		3. 构造器再对成员进行赋值操作
>
> 
>
> |一个类中初始化块若有修饰符，则只能被static修饰，称为**静态代码块**(static block )，当类被载入时，类属性的声明和静态代码块先后顺序被执行，且只被执行一次。
>
> | 非静态初始化块，没有static声明                     | 静态初始化块，用static声明         |
> | :------------------------------------------------- | ---------------------------------- |
> | 可以有输出语句                                     |                                    |
> | 可以对类的属性声明进行初始化操作                   |                                    |
> | 可以调用静态和非静态的变量或方法                   | 不可以调用非静态的属性和方法       |
> | 若有多个代码块，那么按照从上到下的顺序依次执行     |                                    |
> | 每次创建对象的时候，都会执行一次。且先于构造器执行 | 静态代码块的执行要先于非静态代码块 |
> |                                                    | 静态代码块只执行一次               |

## `final 关键字`

> **Ø  final** **标记的类不能被继承。**提高安全性，提高程序的可读性
>
> ​		e.g.  String类     System类      StringBuffer类
> **Øfinal** **标记的方法不能被子类重写**
>
> ​		e.g.  Object类中的getClass()
>
> **Øfinal** **标记的变量** (**成员变量或局部变量**)**即称为常量。**名称大写，且只能被赋值一次。
>
> ​		e.g. final标记的成员变量必须在声明的同时或在每个构造方法中或代码块中显式赋值，然后才能使

## `抽象类（abstract class）`

> | 抽象类不能被实例化。抽象类是用来作为父类被继承的，抽象类的子类必须重写父类的抽象方法，并提供方法体。若没有重写全部的抽象方法，仍为抽象类。
>
> | 不能用abstract修饰属性、私有方法、构造器、静态方法、final的方法
>
> | 抽象类可以有构造方法，只是不能直接创建抽象类的实例对象

## `接口`

>  接口(interface)是抽象方法和常量值的定义的集合
>
> | 从本质上讲，接口是一种特殊的抽象类，这种抽象类中只包含常量和方法的定义，而没有变量和方法的实现。

### `接口的特点：`

> - Ø用interface来定义。
>
>   Ø接口中的所有成员变量都默认是由public static final修饰的。
>
>   Ø接口中的所有方法都默认是由public abstract修饰的。
>
>   Ø接口没有构造器。
>
>   Ø接口采用多层继承机制。
>
>   ---
>
>   实现接口的类中必须提供接口中所有方法的具体实现内容，方可实例化。否则，仍为抽象类。
>
>   与继承关系类似，接口与实现类之间存在多态性

## `内部类`

### `内部类的特性`

> |**Inner class** **作为类的成员：**
>
> ​		Ø可以声明为**final**的
>
> ​		Ø和外部类不同，Inner class可声明为**private**或**protected**；
>
> ​		ØInner class 可以声明为**static**的，但此时就不能再使用外层类的非static的成员变量；	
>
> | **Inner class ** **作为类**
>
> ​		Ø可以声明为**abstract**类 ，因此可以被其它的内部类继承
>
> ---
>
> 【注意】非static的内部类中的成员不能声明为static的，只有在外部类或static的内部类中才可声明static成员。
>
> ---

### `匿名内部类`

>  		匿名内部类不能定义任何静态成员、方法和类，只能创建匿名内部类的一个实例。一个匿名内部类一定是在new的后面，用其隐含实现一个接口或实现一个类。

---

<img src="/Users/zhenxu2/JAVA/review_prcture/java_picture/java7.png" alt="java7" style="zoom:68%;" />