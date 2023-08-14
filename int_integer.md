# Int和Integer的区别

## 数据类型区别
Int是基本数据类型，存储的是数值。
Integer是引用类型（包装类），存储的是引用对象的地址，因此也会占用更大内存。

基本数据类型有：boolean、byte、int、char、long、short、double、float；

其对应的引用类型有：Boolean、Byte、Integer、Character、Long、Short、Double、Float；

## 基本代码比较结果

1. i1和i2的地址不同，因此为false
```Java
Integer i1 = new Integer(10);
Integer i2 = new Integer(10);
System.out.print(i1 == i2); //false
```
2. i1比较的时候会继续自动拆箱(unboxing)，实际比较的是数值大小。
```Java
Integer i1 = new Integer(10);
int i2 = new Integer(10);
System.out.print(i1 == i2); //true
```
3. 没有new生成的值在常量池内，new生成的值在堆空间中，即地址不同。

    没有new生成的值且范围在[-128,127]内时，在编译的时候 => *Integer.valueOf(value)*。且会自动装箱->缓存(*IntegerCache*)，即同样的值的地址相同。
    
    当值超出此范围，会在堆中new出一个对象来存储。
```Java
Integer i1 = new Integer(10);
Integer i2 = 10;
System.out.print(i1 == i2); //false
Integer i3 = 10;
System.out.print(i2 == i3); //true
Integer i4 = 128;
Integer i5 = 128;
System.out.print(i4 == i5); //false
```

## 补充

### 常量池

### 堆空间

### Integer源码分析
[Click me](/Source_Code_Analysis/integer.md)