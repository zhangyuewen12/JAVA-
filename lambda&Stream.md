# lambda表达式
```  
例子
Thread td = new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("hello");
    }
});
td.start();
Thread td1 = new Thread(()->{
    System.out.println("hello,I am lambda");
});
td1.start();
```

## lambda表达式语法
1. lambda表达式的一般语法
```
（Type1 param1,Type2 param2,...TypeN paramN）->{
    statment1;
    statment2;
    ...
    return statmentM;
}
```
2. 单参数语法
```
param1->{
    statment1;
    statment2;
    ...
    return statmentM;
}
当lambda表达式的参数个数只有一个，可以省略小括号
```
3. 单语句写法
```
单语句写法

param1 -> statment

当lambda表达式只包含一条语句时，可以省略大括号，return和语句结尾的分号
```

4. 方法引用写法
```

 Class or instance :: method
```

# Stream语法
```
两句话理解Stream:
1.Stream是元素的集合，这点让Stream看起来类似用Iterator;
2.可以支持顺序和并行的对原Stream进行汇聚的操作；

大家可以把Stream 当成一个装饰后的Iterator。原始版本的Iterator，用户只能逐个遍历元素并对其进行某些操作。
包装后的Stream，用户只要给出需要对其包含的元素执行什么操作，比如"过滤"。
具体这些操作如何应用到每个元素，就交给Stream就好了。原先是人告诉计算机一步一步怎么做，现在是告诉计算机做什么，
计算机自己决定怎么做。

例子：
   List<Integer> nums = Lists.newArrayList(1,null,3,4,null,6);
   nums.stream().filter(num -> num != null).count();
   上面这段代码是获取一个List中，元素不为null的个数。这段代码虽然很简短，但是却是一个很好的入门级别的例子来体现如何使用Stream，正所谓“麻雀虽小五脏俱全”。我们现在开始深入解刨这个例子，完成以后你可能可以基本掌握Stream的用法！
   nums.stream():是Stream的生命开始的地方，负责创建一个Stream实例；
   filt(num-> num!=null) ， 是把Stream 转换成另一个Stream。
   count() 是把Stream 里面所包含的内容汇聚成一个值。
```
1. 创建Stream的两钟途径：
>>> 1. 通过Stream接口的静态工厂
>>> 2. 通过Collection接口的默认方法

2. 转换Stream
转换Stream其实就是把一个Stream通过某些行为转换为一个新的Stream。
>>> 1. distinct: 对于Stram中包含的元素进行去重操作,新生成的Stream中没有重复的元素。
>>> 2. filter：对于Stream中包含的元素使用给定的过滤函数进程过滤操作，新生成的Stream中只包含符合条件的元素；
>>> 3. map: 对于Stream 中包含的元素使用给定的转换函数进行转换操作。新生成的Stream中只包含转换生成的元素。有mapToInt,mapToLongm,mapToDouble;
