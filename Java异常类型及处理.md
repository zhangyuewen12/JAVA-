# 一、 异常实现及分类
```
1. 所有的异常都是从Throwable继承而来的，是所有异常的公祖祖先。
2. Throwable 两个子类，Error和EXception。其中Error是错误，对于所有的编译时期的错误以及系统错误都是通过Error抛出的。这些错误表示故障发生于虚拟机自身，或者发生在虚拟机试图执行应用时，如JAVA虚拟机运行错误（Virtual MachineError）,类定于错误(NoClassDefFoundErro)等，这些错误是不可查的，因为他们在应用程序的控制和处理能力之外，而且绝大数是程序运行时不允许出现的状况。
3.Exception，是另外一个非常重要的异常子类。它规定的异常时程序本身可以处理异常。异常和错误的区别是，异常时可以被处理的，而错误是没法处理的。
4. checked Exception ，可检测的异常。这是编码时非常常用的，所有checked exception都是需要在代码中处理的。它们的发生时可以预测的，正常的一种情况，可以合理的处理。比如IOException，或者一些自定义的异常。除了RuntimeException和其子类以外，都是checked exception。
5. unchecked exception ,RuntimeException及其子类都是unchecked exception，比如NPE空指针异常。除数为0的算法异常ArithmeticException等等，这种异常时运行时发生的。无法预先捕捉处理的。Error也是Unchecked exception，也是无法预先处理的。
```
```
Error 类是指Java运行时系统的内部错误和资源耗尽错误，应用程序不会抛出该类对象。如果出现了这样的错误，除了告知用户，剩下的就是尽力使程序安全的终止。
Exception 有两个分支，一个是运行时异常 runtimeException，一个检查异常 checkedException

RuntimeException 如：NullPointException,ClassCastException；
CheckedException 如：I/O 错误导致的IOException，SqlException.

RuntimeException 是那些可能在JAVA虚拟机正常运行期间抛出的异常的超类，如果出现RuntimeException,那么一定是程序员代码导致的错误；

CheckedException：一般是外部错误，这种异常都发生在编辑阶段，Java编译器会强制程序去捕获此类异常，即会出现要求你把这段可能出现异常的程序进行try catch。

```

# 二、异常的处理
```
1. 通过try...catch语句块来处理:
try
{
   // 程序代码
}catch(ExceptionName e1)
{
   //Catch 块
}

2. 另外，也可以在具体位置不处理，直接抛出，通过throws/throw到上层再进行处理，具体的，如果一个方法没有捕获到一个检查性异常，那么该方法必须使用 throws 关键字来声明。throws 关键字放在方法签名的尾部。也可以使用 throw 关键字抛出一个异常，无论它是新实例化的还是刚捕获到的。

下面方法的声明抛出一个 RemoteException 异常：

import java.io.*;
public class className
{
  public void deposit(double amount) throws RemoteException
  {
    // Method implementation
    throw new RemoteException();
  }
}

3. finally关键字

finally 关键字用来创建在 try 代码块后面执行的代码块。

无论是否发生异常，finally 代码块中的代码总会被执行。

在 finally 代码块中，可以运行清理类型等收尾善后性质的语句。

finally 代码块出现在 catch 代码块最后，语法如下：

try{
  // 程序代码
}catch(异常类型1 异常的变量名1){
  // 程序代码
}catch(异常类型2 异常的变量名2){
  // 程序代码
}finally{
  // 程序代码
}
```
# 四、Throw 和 throws 的区别
```

throws 用在函数上，后面跟的是异常类，可以跟多个；

语法：(修饰符)(方法名)([参数列表])[throws(异常类)]{……}
public void doA(int a) throws Exception1,Exception3{……}

throw 用在函数内，后面跟的是异常对象。

throws E1,E2,E3只是告诉程序这个方法可能会抛出这些异常，方法的调用者可能要处理这些异常，而这些异常E1，E2，E3可能是该函数体产生的。
throw则是明确了这个地方要抛出这个异常。

结合来看：

void doA(int a) throws IOException,{
   try{
      ......
   }catch(Exception1 e){
      throw e;
   }catch(Exception2 e){
      System.out.println("出错了！");
   }
   if(a!=b)
      throw new  Exception3("自定义异常");
}
throws 用来声明异常，让调用者知道该功能可能会出现的问题（比如上方的 IO 异常），可以给出预先的处理方式；
throw 抛出具体的问题对象，执行到 throw，功能就已经结束了，跳转到调用者，并将具体的问题对象抛给调用者。
也就是说 throw 语句独立存在时，下面不要定义其他语句，因为执行不到。

概括:
throws 表示出现异常的一种可能性，并不一定会发生这些异常；
throw 则是抛出了异常，执行 throw 则一定抛出了某种异常对象。
```