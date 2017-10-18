### Java反射的概念
		 Java反射机制是在运行状态中，对于任意一个类（class文件），都能够知道这个类的所有属性和方法，对于任意一个对象，都能够调用它的任意一个方法和属性。这种动态获取的信息以及动态调用对象的方法的功能成为java语言的反射机制，动态获取类中信息，就是java反射。
### Java反射的功能
　 1)可以判断运行时对象所属的类；
　 2)可以判断运行时对象所具有的成员变量和方法；
　 3)通过反射甚至可以调用到private的方法；
　 4)生成动态代理。
### 实现Java反射的类
　　1) Class：它表示正在运行的Java应用程序中的类和接口
　　2) Field：提供有关类或接口的属性信息，以及对它的动态访问权限
　　3) Constructor：提供关于类的单个构造方法的信息以及对它的访问权限
　　4) Method：提供关于类或接口中某个方法信息
　　注意：Class类是Java反射中最重要的一个功能类，所有获取对象的信息(包括：方法/ 属性/构造方法/访问权限)都需要它来实现。
####  具体功能实现：
1，反射机制获取类有三种方法，我们来获取Employee类型

```
//第一种方式：

Classc1 = Class.forName("Employee");

//第二种方式：

//java中每个类型都有class 属性.

Classc2 = Employee.class;

//第三种方式：

//java语言中任何一个java对象都有getClass 方法

Employeee = new Employee();

Classc3 = e.getClass(); //c3是运行时类 (e的运行时类是Employee)
```
2，创建对象：获取类以后我们来创建它的对象，利用newInstance：

```
Class c =Class.forName("Employee");

//创建此Class 对象所表示的类的一个新实例

Objecto = c.newInstance(); //调用了Employee的无参数构造方法.
```
3,获取属性：分为所有的属性和指定的属性：

a，先看获取所有的属性的写法：

```
//获取整个类

            Class c = Class.forName("java.lang.Integer");

         //获取所有的属性?

            Field[] fs = c.getDeclaredFields();

     //定义可变长的字符串，用来存储属性

            StringBuffer sb = new StringBuffer();

     //通过追加的方法，将每个属性拼接到此字符串中

            //最外边的public定义

            sb.append(Modifier.toString(c.getModifiers()) + " class " + c.getSimpleName() +"{\n");

     //里边的每一个属性

            for(Field field:fs){

                sb.append("\t");//空格

                sb.append(Modifier.toString(field.getModifiers())+" ");//获得属性的修饰符，例如public，static等等

                sb.append(field.getType().getSimpleName() + " ");//属性的类型的名字

                sb.append(field.getName()+";\n");//属性的名字+回车

            }

            sb.append("}");

            System.out.println(sb);```
 b，获取特定的属性，对比着传统的方法来学习：
```
public static void main(String[] args) throws Exception{

//以前的方式：

    /*

    User u = new User();

    u.age = 12; //set

    System.out.println(u.age); //get

    */

    //获取类

    Class c = Class.forName("User");

    //获取id属性

    Field idF = c.getDeclaredField("id");

    //实例化这个类赋给o

    Object o = c.newInstance();

    //打破封装

    idF.setAccessible(true); //使用反射机制可以打破封装性，导致了java对象的属性不安全。

    //给o对象的id属性赋值"110"

    idF.set(o, "110"); //set

    //get

    System.out.println(idF.get(o));

}
```
4，获取方法，和构造方法，不再详细描述，只来看一下关键字：
```
|       方法关键字                    |                  含义                     |
| getDeclaredMethods()           |            获取所有的方法           |
| getReturnType()                   |          获得方法的放回类型  |
| getParameterTypes()            |        获得方法的传入参数类型   |
| getDeclaredMethod("方法名",参数类型.class,……) |     获得特定的方法 |
|  |  |
| 构造方法关键字                   |                  含义                       |
| getDeclaredConstructors()    |                获取所有的构造方法       |
| getDeclaredConstructor(参数类型.class,……) |    获取特定的构造方法|
|  |  |
| 父类和父接口                      |                  含义                         |
| getSuperclass()                   |                 获取某类的父类          |
| getInterfaces()                    |                获取某类实现的接口    |
```
这样我们就可以获得类的各种内容，进行了反编译。对于JAVA这种先编译再运行的语言来说，反射机制可以使代码更加灵活，更加容易实现面向对象。

#### 反射加配置文件，使我们的程序更加灵活：

在设计模式学习当中，学习抽象工厂的时候就用到了反射来更加方便的读取数据库链接字符串等，当时不是太理解，就照着抄了。看一下.NET中的反射+配置文件的使用：

当时用的配置文件是app.config文件，内容是XML格式的，里边填写链接数据库的内容:

```
<configuration>
<appSettings>
<add key="" value=""/>
</appSettings>
</configuration>
```
反射的写法：

1.  assembly.load("当前程序集的名称").CreateInstance("当前命名空间名称".要实例化的类名);  

assembly.load("当前程序集的名称").CreateInstance("当前命名空间名称".要实例化的类名);

这样的好处是很容易的方便我们变换数据库，例如我们将系统的数据库从SQL Server升级到Oracle，那么我们写两份D层，在配置文件的内容改一下，或者加条件选择一下即可，带来了很大的方便。

当然了，JAVA中其实也是一样，只不过这里的配置文件为.properties,称作属性文件。通过反射读取里边的内容。这样代码是固定的，但是配置文件的内容我们可以改，这样使我们的代码灵活了很多！

   综上为，JAVA反射的再次学习，灵活的运用它，能够使我们的代码更加灵活，但是它也有它的缺点，就是运用它会使我们的软件的性能降低，复杂度增加，所以还要我们慎重的使用它。
