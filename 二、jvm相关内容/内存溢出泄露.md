# 一、GC

## 1、什么是反射？

​	java反射就是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法，对于任意一个对象，都能够调用它的任意方法和属性；并且能改变它的属性。而这也是java被视为动态语言的一个关键性质。



## 2、作用

​	反射机制允许程序在运行时取得任何一个已知名称的class的内部信息，包括其modifiers(修饰符)，fields(属性),methods(方法)等，并可于运行时改变fields内容或调用methods。那么我们便可以更灵活的编写代码，代码可以在运行时装配，无需在组件之间进行源代码链接，降低代码的耦合度；还有动态代理的实现等等；但是需要注意的是反射使用不当会造成很高的资源消耗。



## 3、反射的具体实现

### 	a. 三种获取Class方式：

```java
//1、通过对象调用getClass()方法获取（例如传过来一个Object类型的对象，而我不知道具体类型，用这种方法）
Person p1 = new Person();
Class c1 = p1.getClass();
```

```java
//2、直接通过类名.class方式获取，该方法最为安全可靠，程序性能更高，这说明任何一个类都有一个隐含的静态成员变量 
Class c2 = Person.class;
```

```java
//3、通过Class对象的forName()静态方法来获取，用的最多，但可能抛出ClassNotFoundException异常
try {    
    Class c3 = 		 Class.forName("com.origin.basics.reflectDemo.Person");    
} catch (ClassNotFoundException e) {   
    e.printStackTrace();
}
```

```java
//*****注：一个类在JVM中只会有一个Class实例
System.out.println(c1.equals(c2)+c2.equals(c3)+c1.equals(c3)); //true true true
```

### 	b. API中Class方法：

```java
java.lang.Class; //类               
java.lang.reflect.Constructor;//构造方法 
java.lang.reflect.Field; //类的成员变量       
java.lang.reflect.Method;//类的方法
java.lang.reflect.Modifier;//访问权限

String className = c1.getName(); //获取类的完整名字
System.out.println("className "+className);
Field[] fields = c1.getFields();//获取类的public类型的属性
Field[] declaredFields = c1.getDeclaredFields();//获取类的所有属性，包括private声明的
Method[] methods =c1.getMethods();//获取类和继承类的public方法
Method[] declaredMethods = c1.getDeclaredMethods();//获得类的所有方法，包括private声明的
Constructor[] constructors = c1.getConstructors();//获得类的public类型的构造器方法
Object o = c1.newInstance(); //通过类的不带参数的构造方法创建这个类的一个对象
  
//反射机制可以打破封装性，导致了java对象的属性不安全
//获取名为name的私有属性
Field f1 = c1.getDeclaredField("name");
System.out.println("f1 = [" + f1 + "]");
f1.setAccessible(true); //启用和禁用访问安全检查的开关，值为true,则表示反射的对象在使用时应该取消java语言的访问检查
Object obj = c1.newInstance();
System.out.println(f1.get(obj)); //获取私有属性的值
f1.set(obj,"BOB"); //私有属性赋值，使用反射机制可以打破封装性，导致了java对象的属性不安全
System.out.println(f1.get(obj));

//反射调用private修饰方法
Object obj1 = c1.newInstance();
boolean flag = obj1 instanceof Person;
if(flag){
	Method method = c1.getDeclaredMethod("say");
	method.setAccessible(true); //如果是private修饰，不加这行报错
	method.invoke(obj1);
}
```



## 4、反射的应用场景

- 逆向代码 ，例如反编译
- 与注解相结合的框架 例如Retrofit
- 单纯的反射机制应用框架 例如EventBus 2.x
- 动态生成类框架 例如Gson



## 5、反射的优缺点

优点：运行期类型的判断，动态类加载，动态代理使用反射

缺点：性能是一个问题，反射相当于一系列解释操作，通知jvm要做的事情，性能比直接java代码要慢