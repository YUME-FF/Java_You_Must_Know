# OOP基础

## Object Oriented VS Procedure Oriented
OOP:
把问题分解成各个对象，所有的对象都继承自 *java.lang.Object* ，用类实现功能模块，最终组装成完整的系统
+ Keep the Java code DRY "Don't Repeat Yourself"
+ Faster and easier to execute
+ Provide a clear structure for the programs
+ Make it possible to create full reusable applications with less code and shorter development time

POP:
以事件的过程/步骤为核心，按顺序调用函数

## OOP特性

+ 继承：提高代码复用
+ 封装：把一个对象的属性私有化，同时提供一些可以被外界访问的属性的方法
+ 多态：指程序中 定义的引用变量所指向的具体类型 和 通过该引用变量发出的方法调用 在编程时并不确定，而是在程序运行期间才确定

*重载（overload）实现的是编译时的多态性（也称为前绑定）

*重写（override）实现的是运行时的多态性（也称为后绑定）

## 基本原则

### 单一职责原则(Single Responsibility Principle)
类的功能要单一

```Java
```

### 开放封闭原则(Open－Close Principle)
开放拓展，封闭修改
```Java
```

### 里式替换原则(Liskov Substitution Principle)
子类可以替换父类出现在父类能够出现的任何地方
```Java
void drawShape(Shape shape) {
    // if/else过多，且增加shape时需要修改该方法
    if (shape.type == Shape.Circle ) {
        drawCircle((Circle) shape);
    } else if (shape.type == Shape.Square) {
        drawSquare((Square) shape);
    } else {
        ……
    }
}

//推荐写法
public abstract Shape{
  public abstract void draw();
}
...
void drawShape(Shape shape) {
  shape.draw();
}
```

### 依赖倒置原则(Dependency Inversion Principle)
高层次的模块不应该依赖于低层次的模块，他们都应该依赖于抽象。抽象不应该依赖于具体实现，具体实现应该依赖于抽象
```Java
```

### 接口分离原则(Interface Segregation Principle)
设计时采用多个与特定客户类有关的接口比采用一个通用的接口要好
```Java
```