# Integer源码解析

## 构造函数
```Java
public Integer(int value) {
        this.value = value;
    }

public Integer(String s) throws NumberFormatException {
        this.value = parseInt(s, 10);
    }
```
两种构造的方法都不太推荐去用