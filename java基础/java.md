# java

## 1. java安装

### 1.JDK,JRE,JVM(掌握)

```text
(1)JVM
	保证Java语言跨平台。针对不同的操心系统提供不同的JVM。
	问题：java语言是跨平台的吗?JVM是跨平台的吗?
(2)JRE
	java程序的运行环境。包括JVM和核心类库
(3)JDK
	java开发环境。包括JRE和开发工具(javac,java)
```
### 2.path环境变量(理解)

```text
(1)为什么要配置path环境变量
		为了让javac和java命令可以在任意目录下使用
(2)如何配置
	A:方式1 直接修改path，在前面追加JDK的bin目录
	B:方式2(掌握) 
		新建JAVA_HOME: JDK的安装目录
		修改path: %JAVA_HOME%\bin;后面是以前的环境变量
```

### 3.classpath环境变量(理解)

```text
	(1)为什么要配置classpath环境变量
		为了让class文件可以在任意目录下运行
	(2)如何配置
		新建：classpath，把你想要在任意目录下运行的class文件所在目录配置过去即可。
		注意：将来在执行的时候，有先后顺序关系
	(3)path和classpath的区别
		path是为了让exe文件可以在任意目录下运行
		classpath是为了让class文件可以在任意目录下运行
```

## 2. java变量

### 1. 数据类型

```text
	(1)数据类型分类
		A:基本类型：4类8种
		B:引用类型：类，接口，数组
	(2)基本类型
		A:整数			占用的内存空间
			byte		1
			short		2
			int			4
			long		8
		B:浮点数
			float		4
			double		8
		C:字符
			char		2
		D:布尔
			boolean		1
			
	(3)float数据在计算机中的存储
		符号位		指数位		底数位
		S			E		M
```

### 2. 类型转换

```text
注意：
	boolean类型不参与。
(1)隐式转换：从小到大
	byte,short,char --> int --> long --> float --> double
(2)强制转换：从大到小
	一般不建议这样做，因为可能有精度的损失。
	格式：
		目标数据类型 变量名 = (目标数据类型)(被转换的数据);

```



### 3.面试题

1. Java中的字符可以存储一个汉字吗?为什么呢?

```text
java中的编码是使用unicode的编码方式,unicode码是16位,java中无论是英文还是中文,都是使用Unicode编码来表示的,而一个char类型变量占据两个字节,所以可以存储一个中文汉字
```

2.long为什么可以到float呢?

```text
	A:因为long和float的底层存储结构不同。
	B:数据范围
		long: 2^63
		float: 3.4*10^38
		3.4*10^38 > 3.4*8^38 = 3.4*2^3^38 = 3.4*2^114 > 2^63
```

3.short s = 1; s = s + 1;short s = 1, s +=1;哪个有问题。

```text
整数的默认类型是整型数据,结果为int类型,赋给short类型会有精度的损失,编译器会提示丢失精度
而s += 1,相当于s = (s的类型)(s+1);
```

## 3. java运算符

### 1.++和--

```text
A:单独使用
		放在数据的前面和后面效果一样。
B:参与操作使用
	放在数据的前面，先数据变化，再参与运算。
	放在数据的后面，先参与运算，再数据变化。
	
int a = 4;
int b = (a++)+(++a)+(a*10);
           4    6     60
 b = 70
```

### 2.位运算符

	(1)&,|,^,~,>>,>>>,<<
			做位运算，需要把数据转换为二进制。
	(2)^的特点：(掌握)
		针对同一个数据异或两次，值不变。
### 3. switch表达式的取值类型

```text
byte,short,int,char
JDK5以后可以是枚举
JDK7以后可以是String
```



### 3. 面试题

1. 交换变量的值int a = 10;int b = 20;

	开发：第三方变量
		int temp = a;
		a = b;
		b = temp;
	面试：^的实现
		a = a ^ b;
		b = a ^ b;
		a = a ^ b;
2. 请用最有效率的方式计算2乘以8的值

		2<<3

3.switch的表达式是可以是byte吗?可以是long吗?可以是String吗?

```text
switch中现在还不支持表达式类型为long,在jdk1.7之后就支持表达式类型为String
```

## 4. 数组创建

### 1. 一维数组

```text
A:动态初始化 只给长度，不给元素
		int[] arr = new int[3];
B:静态初始化 不给长度，给元素
		int[] arr = new int[]{1,2,3};
		简化版：int[] arr = {1,2,3};
```

### 2. 二维数组

```text
A:数据类型[][] 变量名 = new 数据类型[m][n];
B:数据类型[][] 变量名 = new 数据类型[m][];
C:数据类型[][] 变量名 = new 数据类型[][]{{元素...},{元素...},{元素...}};
  数据类型[][] 变量名 = {{元素...},{元素...},{元素...}};
```

## 5. 类

### 1. 修饰符

​	(1)4种权限修饰符
​							本类	同一个包下	不同包下的子类	不同包下的其他类
​		private		Y
​		默认			 Y		  Y
​		protected   Y	 	 Y						Y
​		public		  Y		 Y						 Y							Y
​	(2)常见的修饰
​		A:类	public
​		B:成员变量	private
​		C:构造方法	public
​		D:成员方法	public

### 2. 静态变量,成员变量和局部变量

(1)在类中的位置不同
		A:成员变量 类中，方法外
		B:局部变量 方法的形式参数，或者方法体中

​		C:静态变量：类中,方法外	

(2)在内存中的位置不同
		A:成员变量 在堆中
		B:局部变量 在栈中

​		C:静态变量:方法区,也就是元空间

​	(3)生命周期不同
​		A:成员变量 随着对象的存在而存在，随着对象的消失而消失

​		B:局部变量 随着方法的调用而存在，随着方法的调用完毕而消失

​		C:静态变量:随着类的回收而消失,一般是不会回收加载到内存中的类

​	(4)初始化值不同
​		A:成员变量 有默认初始化值
​		B:局部变量 没有默认值，必须先声明，赋值，最后才能使用

​		C:静态变量:有默认初始化值,在类加载后会对其进行初始化

### 3. 静态代码块，构造代码块，构造方法执行顺序

```text
静态代码块:将类文件加载到内存后,便会执行静态代码块
构造代码块:会被收集到构造方法中,在构造方法前使用
构造方法:最后执行
定义变量时对变量的初始化会最先开始执行
```

### 4. 构造方法

- 如果类中没有构造方法,那么类中便存在一个无参的默认构造方法
- 如果类中已经存在一个有参的构造器,那么将不会为该类构造一个无参构造器

### 5. final关键字

​	(1)final:最终的意思
​	(2)作用：可以修饰类，修饰成员变量，修饰成员方法,修饰静态变量
​	(3)特点：
​		A:修饰类 类不能被继承
​		B:修饰成员变量 变量变成了常量
​		C:修饰成员方法 方法不能被重写

```java
(4)面试题：
	A:final修饰局部变量
		a:基本类型 值不能发生改变
		b:引用类型 地址值不能发送改变，对象的内容是可以改变的
	B:final的初始化时机
		a:在定义时就赋值
		b:在构造方法完毕前赋值
            
public class A{
	private final int a ;
    private static final int b = 33;
	public B(){
		a = 44;
	}
}
普通类型的变量会在构造方法中进行初始化,并且无法再对其进行更改
静态变量会在static代码块中对静态变量进行初始化,也同样无法再更改
```
### 6. 注意

​		当使用new关键字创建出对象后,对象便存储在堆内存中,可以看成是静态的

​		程序是静态对象的使用,并且这个过程中会伴随着状态的变化,也就是程序所占据的内存会发生变化

​		而当多个线程同时操作同一个对象,这个对象所占据的内存同时被两个对象操控,可能导致错误

## 6. 继承与多态

### 1. java中的继承的注意事项

			-  私有成员不能被继承
			- 构造方法不能被继承，想访问，通过super关键字
			- 不能为了部分功能而去使用继承

### 2. 方法的重写与重载

1. 方法的重写是子类重写父类的方法

   - 子类重写方法的返回值类型必须与父类相同,或者是父类方法返回值类型的子类
   - 子类方法不能抛出比父类方法更大类型的异常
   - 不能缩小父类方法的访问权限

2. 方法重载是同一个类中方法名相同的方法

   **参数不同(类型,个数,顺序)的多个方法**,**方法重载可以改变返回值类型,因为方法的重载与返回值类型无关。**

3. 父类不能被子类重写方法的类型

   ```text
   - final:父类中的final方法不允许被子类重写
   - static：static方法是属于某一个类的,是不存在类方法的重写
   - private:私有方法是不会被重写的,即使子类中与父类存在同名同参方法也不是方法的重写
   ```

4. 子类中方法的重写是对具体方法的重写,即使存在重载,也是对具体某一个重载方法进行重写

   ```java
   public class A{
       public void print(){
           System.out.println("这个是父类方法");
       }
       public final void print(int a){
           
       }
   }
   
   public class B extend A{
       @Override
       public void print(){
           System.out.println("这个是重写方法");
       }
   }
   B类对A类中print方法的重写
   ```

   

### 3. 构造方法

- 子类的所有构造方法默认都是访问父类的无参构造方法,也可以指定访问父类特定构造方法

- 如果父类没有无参构造方法，怎么办呢?
       通过super(...)访问父类带参构造方法
       通过this(...)访问本类其他构造方法。(一定要有一个访问了父类的构造方法,另一个构造方法手动访问了父类带参构造方法)

  注意：super或者this只能出现一个，并且只能在语句的第一条语句。
  因为子类可能会访问父类的数据，所以，在子类初始化之前，要先把父类数据初始化完毕。super或者this只能出现一个,当使用super关键字

### 4. this和super关键字

this代表当前类的对象引用。super代表父类存储空间的标识（可以理解为父类的引用，通过这个可以访问父类的成员）。
场景：
    成员变量：
            this.成员变量
            super.成员变量
    构造方法：只能出现在构造方法的第一行
            this(....)
            super(....)
    成员方法：
            this.成员方法
            super.成员方法

### 4. 多态

​	(1)多态：同一个对象，在不同时刻表现出来的多种状态
​		举例：水，猫和动物
​	(2)多态的前提：
​		A:有继承关系
​		B:有方法重写
​		C:有父类引用指向子类对象
​	(3)多态中的成员访问特点：
​		A:成员变量
​			编译看左边，运行看左边
​		B:成员方法
​			编译看左边，运行看右边
​		C:静态方法
​			编译看左边，运行看左边

​	为什么：
​		因为方法有重写，而变量没有。静态方法没有重写一说。

## 7.抽象类与接口

### 1. 抽象类

​	(1)有些时候，我们对事物不能用具体的东西来描述，这个时候就应该把事物定义为抽象类。
​	(2)抽象类的特点：
​		A:抽象类或者抽象方法必须用abstract修饰
​		B:抽象类中不一定有抽象方法，但是有抽象方法的类一定是抽象类
​		C:抽象类不能实例化
​			可以按照多态的方式实例化。
​		D:抽象类的子类
​			a:要么是抽象类
​			b:要么重写抽象类中的所有抽象方法
​	(3)抽象类的成员特点：
​		A:成员变量
​			可以是变量，也可以是常量
​		B:构造方法
​			有。
​			不能实例化，有构造方法有什么意义呢?
​			用于子类访问父类数据的初始化。
​		C:成员方法
​			可以是抽象的，也可以是非抽象的
​	(4)抽象类的案例
​		A:猫狗案例
​		B:老师案例
​		C:学生案例
​		D:员工案例
​	(5)两个小问题
​		A:如果你看到一个抽象类中居然没有抽象方法,这个抽象类的意义何在?
​		  不让别人创建
​		B:abstract不能和哪些关键字共存?
​			a:private 冲突
​			b:final 冲突
​			c:static 无意义
​	

### 2. 接口

​	(1)有些时候，不是事物本身具备的功能，我们就考虑使用接口来扩展。
​	(2)接口的特点：
​		A:定义接口用关键字interface
​			格式是：interface 接口名 {}
​		B:类实现接口用关键字implements 
​			格式是：class 类名 implements 接口名 {}
​		C:接口不能实例化
​		D:接口的子类
​			a:要么是抽象类
​			b:要么重写接口中的所有方法
​	(3)接口的成员特点
​		A:成员变量
​			只能是常量。
​			默认修饰符：public static final
​		B:成员方法
​			只能是抽象方法。
​			默认修饰符：public abstract 
​		推荐：
​			建议自己写接口的时候，把默认修饰符加上。
​	(4)类与接口的关系
​		A:类与类
​			继承关系，只能单继承，可以多层继承。
​		B:类与接口
​			实现关系，可以单实现，也可以多实现。
​			还可以在继承一个类的同时实现多个接口。
​		C:接口与接口
​			继承关系，可以单继承，也可以多继承。
​	(5)抽象类和接口的区别?
​		A:成员区别
​		B:关系区别
​		C:设计理念区别
​			抽象类：父抽象类，里面定义的是共性内容。
​			接口：父接口，里面定义的是扩展内容。

## 8. 内部类