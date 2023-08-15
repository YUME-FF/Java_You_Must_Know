# Integer源码解析

[SourceCode Link](https://github.com/openjdk/jdk/blob/master/src/java.base/share/classes/java/lang/Integer.java)

# Table of Contents
- [类定义](#类定义)
- [构造函数](#构造函数)
- [核心私有类](#核心私有类)
- [主要静态方法](#主要静态方法)

## 类定义

```Java
public final class Integer extends Number
        implements Comparable<Integer>, Constable, ConstantDesc
```
特点：
+ 不可被继承
+ 继承于Number: 可用 *.xxxxValue()*
+ Comparable接口: 可用 *.compareTo(value)* 相等返回0，大于返回正值，小于返回负值
+ Constable(JDK 12): 可以在 JVMS 4.4 中描述的Java类文件的常量池中表示的常量 
+ ConstantDesc(JDK 12): JVMS 4.4中定义的可加载常量值的名义描述符

## 构造函数
```Java
public Integer(int value) {
        this.value = value;
    }

public Integer(String s) throws NumberFormatException {
        this.value = parseInt(s, 10);
    }
```
+ *this.value*是private final，即**不可被改变**。
+ 初始化时只能创建一个十进制整数。

## 核心私有类
### ***IntegerCache***
```Java
private static final class IntegerCache {
        static final int low = -128;
        static final int high; 
        /**
         * 通过VM.getSavedProperty("java.lang.Integer.IntegerCache.high")
         * 获取high值，默认为127
         * 可以在jdk.internal.misc.VM class中修改high值
         */

        @Stable
        static final Integer[] cache;
    ...
}
```
当 Integer 被加载时，就新建了[-128,127]的所有数字并存放在Integer数组cache中。

## 主要静态方法

### *valueOf()*
```Java
public static Integer valueOf(String s, int radix) throws NumberFormatException {
        return Integer.valueOf(parseInt(s,radix));
    }
public static Integer valueOf(String s) throws NumberFormatException {
        return Integer.valueOf(parseInt(s, 10));
    }
public static Integer valueOf(int i) {
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
```
+ 前两个用parseInt方法进行String的进制转化
+ *.valueOf(int)* 如果i超过私有类的cache范围，则会**return一个new的Integer对象**。

反过来思考，当做int转化为Integer或者新建Integer时，我们应该使用 *.valueOf(int)* 或者直接 *Integer i = 100;* 这样可以有明显的时间以及空间上的效率。

### *decode()*
```Java
public static Integer decode(String nm) throws NumberFormatException{
    //1. + - 判断
    //2. 0x,0X，#开头表示十六进制
    //3. 数字部分用valueOf()进行转换
}
```

### *expand()*

```Java
    public static int expand(int i, int mask) {
        // Save original mask
        int originalMask = mask;
        // Count 0's to right
        int maskCount = ~mask << 1;
        int maskPrefix = parallelSuffix(maskCount);
        // Bits to move
        int maskMove1 = maskPrefix & mask;
        // Compress mask
        mask = (mask ^ maskMove1) | (maskMove1 >>> (1 << 0));
        maskCount = maskCount & ~maskPrefix;

        maskPrefix = parallelSuffix(maskCount);
        // Bits to move
        int maskMove2 = maskPrefix & mask;
        // Compress mask
        mask = (mask ^ maskMove2) | (maskMove2 >>> (1 << 1));
        maskCount = maskCount & ~maskPrefix;

        maskPrefix = parallelSuffix(maskCount);
        // Bits to move
        int maskMove3 = maskPrefix & mask;
        // Compress mask
        mask = (mask ^ maskMove3) | (maskMove3 >>> (1 << 2));
        maskCount = maskCount & ~maskPrefix;

        maskPrefix = parallelSuffix(maskCount);
        // Bits to move
        int maskMove4 = maskPrefix & mask;
        // Compress mask
        mask = (mask ^ maskMove4) | (maskMove4 >>> (1 << 3));
        maskCount = maskCount & ~maskPrefix;

        maskPrefix = parallelSuffix(maskCount);
        // Bits to move
        int maskMove5 = maskPrefix & mask;

        int t = i << (1 << 4);
        i = (i & ~maskMove5) | (t & maskMove5);
        t = i << (1 << 3);
        i = (i & ~maskMove4) | (t & maskMove4);
        t = i << (1 << 2);
        i = (i & ~maskMove3) | (t & maskMove3);
        t = i << (1 << 1);
        i = (i & ~maskMove2) | (t & maskMove2);
        t = i << (1 << 0);
        i = (i & ~maskMove1) | (t & maskMove1);

        // Clear irrelevant bits
        return i & originalMask;
    }

    @ForceInline
    private static int parallelSuffix(int maskCount) {
        int maskPrefix = maskCount ^ (maskCount << 1);
        maskPrefix = maskPrefix ^ (maskPrefix << 2);
        maskPrefix = maskPrefix ^ (maskPrefix << 4);
        maskPrefix = maskPrefix ^ (maskPrefix << 8);
        maskPrefix = maskPrefix ^ (maskPrefix << 16);
        return maskPrefix;
    }
```

