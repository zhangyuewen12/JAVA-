# JAVA 反射(Reflection教程)
Java 反射机制可以让我们在编辑器(Compile Time) 之外的运行期(Runtime) 检查类，接口，变量以及方法的信息。
反射还可以让我们在运行期实例化对象，调用方法，通过调用get/set方法获取变量的值。

## Class对象
```
在你想检查一个类的信息之前，你首先需要获取类的Class对象。java中所有类型包括基本类型(int,long,float等等)，即使是数组都有与之关联的Class类的对象。

如果你在编译期知道一个类的名字的话。那么你可以使用如下的方法或一个类的Class对象。
1. Class myObejectClass = MyObject.class;

如果你在编译器不知道类的名字，但是你可以在运行期获得类名的字符串，可以通过下面这钟方式：
  String className = ...;// 在运行期的获取的类名字符串
  Class class = Class.forName(className);
  
  在使用Class.forName()方法时，你必须提供一个类的全名，这个全名包括类所在包的名字。例如MyObject类位于com.jencov.myapp包。那么他的类全名就是：com.jencov.myapp.MyObject.
  如果在调用Class.fornName()方法时，没有在编译路径下(classpath)找到对应的类，那么将会抛出ClassNotFoundException。
```

## 类名
```
 你可以从Class对象中获取两个版本的类名。

通过getName() 方法返回类的全限定类名（包含包名）：
    Class aClass = ... //获取Class对象，具体方式可见Class对象小节
    String className = aClass.getName();


如果你仅仅只是想获取类的名字(不包含包名)，那么你可以使用getSimpleName()方法:
    Class aClass = ... //获取Class对象，具体方式可见Class对象小节
    String simpleClassName = aClass.getSimpleName();
```

## 修饰符
```
可以通过Class对象来访问一个类的修饰符，即public,private,static等等的关键字，你可以使用如下方法来获取类的修饰符：

Class  aClass = ... //获取Class对象，具体方式可见Class对象小节
int modifiers = aClass.getModifiers();

修饰符都被包装成一个int类型的数字，这样每个修饰符都是一个位标识(flag bit)，这个位标识可以设置和清除修饰符的类型。
可以使用java.lang.reflect.Modifier类中的方法来检查修饰符的类型：

Modifier.isAbstract(int modifiers);

Modifier.isFinal(int modifiers);

Modifier.isInterface(int modifiers);

Modifier.isNative(int modifiers);

Modifier.isPrivate(int modifiers);

Modifier.isProtected(int modifiers);

Modifier.isPublic(int modifiers);

Modifier.isStatic(int modifiers);

Modifier.isStrict(int modifiers);

Modifier.isSynchronized(int modifiers);

Modifier.isTransient(int modifiers);

Modifier.isVolatile(int modifiers);

```
## 父类
```
通过Class对象你可以访问类的父类，如下例：

Class superclass = aClass.getSuperclass();

可以看到superclass对象其实就是一个Class类的实例，所以你可以继续在这个对象上进行反射操作。
```

## 实现的接口
```
可以通过如下方式获取指定类所实现的接口集合：

Class  aClass = ... //获取Class对象，具体方式可见Class对象小节
Class[] interfaces = aClass.getInterfaces();

由于一个类可以实现多个接口，因此getInterfaces();
方法返回一个Class数组，在Java中接口同样有对应的Class对象。
注意：getInterfaces()方法仅仅只返回当前类所实现的接口。当前类的父类如果实现了接口，这些接口是不会在返回的Class集合中的，尽管实际上当前类其实已经实现了父类接口。
```

## 构造器
```
你可以通过如下方式访问一个类的构造方法：

Constructor[] constructors = aClass.getConstructors();
```
## 方法
```
你可以通过如下方式访问一个类的所有方法：

Method[] method = aClass.getMethods();
```

## 变量
```
你可以通过如下方式访问一个类的成员变量：

Field[] method = aClass.getFields();
```
## 注解
```
你可以通过如下方式访问一个类的注解：

Annotation[] annotations = aClass.getAnnotations();

```
## Java Reflection - Getters and Setters
```
Using JAVA Reflection you can inspect the methods of classes and invoke them at runtime. this can be useed detect what getters and setters a given class has. You cannot ask for getters and setters explicitly. so you will have to scan throught all the methods of a  class and check if each method is a getter or setter.

Getter
A getter method have its name start with "get", take 0 parameters, and returns a value.

Setter
A setter method have its name start with "set", and takes 1 parameter.
不能直接获得get set方法 可以自己定义去遍历。

public static void printGettersSetters(Class aClass){
  Method[] methods = aClass.getMethods();

  for(Method method : methods){
    if(isGetter(method)) System.out.println("getter: " + method);
    if(isSetter(method)) System.out.println("setter: " + method);
  }
}

public static boolean isGetter(Method method){
  if(!method.getName().startsWith("get"))      return false;
  if(method.getParameterTypes().length != 0)   return false;  
  if(void.class.equals(method.getReturnType()) return false;
  return true;
}

public static boolean isSetter(Method method){
  if(!method.getName().startsWith("set")) return false;
  if(method.getParameterTypes().length != 1) return false;
  return true;
}
```
## JAVA Reflection 泛型

```
运用泛型反射的经验法则

下面是两个典型的使用泛型的场景：
1、声明一个需要被参数化（parameterizable）的类/接口。
2、使用一个参数化类。

当你声明一个类或者接口的时候你可以指明这个类或接口可以被参数化，java.util.List接口就是典型的例子。你可以运用泛型机制创建一个标明存储的是String类型list，这样比你创建一个Object的list要更好。

当你想在运行期参数化类型本身，比如你想检查java.util.List类的参数化类型，你是没有办法能知道他具体的参数化类型是什么。这样一来这个类型就可以是一个应用中所有的类型。但是，当你检查一个使用了被参数化的类型的变量或者方法，你可以获得这个被参数化类型的具体参数。

总之：你不能在运行期获知一个被参数化的类型的具体参数类型是什么，但是你可以在用到这个被参数化类型的方法以及变量中找到他们，换句话说就是获知他们具体的参数化类型。

在下面的段落中会向你演示这类情况。

泛型方法返回类型
如果你获得了java.lang.reflect.Method对象，那么你就可以获取到这个方法的泛型返回类型信息。如果方法是在一个被参数化类型之中（译者注：如T fun()）那么你无法获取他的具体类型，但是如果方法返回一个泛型类（译者注：如List fun()）那么你就可以获得这个泛型类的具体参数化类型。你可以在“Java Reflection: Methods”中阅读到有关如何获取Method对象的相关内容。

下面这个例子定义了一个类这个类中的方法返回类型是一个泛型类型：

  public class MyClass {
  protected List<String> stringList = ...;
  public List<String> getStringList(){
    return this.stringList;
  }
}

我们可以获取getStringList()方法的泛型返回类型，换句话说，我们可以检测到getStringList()方法返回的是List而不仅仅只是一个List。如下例：

Method method = MyClass.class.getMethod("getStringList", null);
Type returnType = method.getGenericReturnType();
if(returnType instanceof ParameterizedType){
    ParameterizedType type = (ParameterizedType) returnType;
    Type[] typeArguments = type.getActualTypeArguments();
    for(Type typeArgument : typeArguments){
        Class typeArgClass = (Class) typeArgument;
        System.out.println("typeArgClass = " + typeArgClass);
    }
}
这段代码会打印出 “typeArgClass = java.lang.String”，Type[]数组typeArguments只有一个结果 – 一个代表java.lang.String的Class类的实例。Class类实现了Type接口
```