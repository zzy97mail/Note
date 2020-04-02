## 创建一个空白程序

![image-20200402140649778](img\image-20200402140649778.png)

![image-20200402140729680](img\image-20200402140729680.png)

![image-20200402140805235](img\image-20200402140805235.png)

## 新建module

![image-20200402141020916](img\image-20200402141020916.png)

![image-20200402141041425](img\image-20200402141041425.png)

![image-20200402141111928](img\image-20200402141111928.png)

***注意红框内的Module name 是后续引用的关键点***

## 引用Module

打开app下面的build.gradle

![image-20200402141323801](img\image-20200402141323801.png)

![image-20200402141709921](img\image-20200402141709921.png)

将刚建好的module引入程序

```groovy
dependencies{
    implementation project(':Module Name')
}
```

## 测试HelloWorld

在新建的module中创建一个HelloWorld程序

![image-20200402142215815](img\image-20200402142215815.png)

在程序中创建一个方法引用一下

![image-20200402142329488](img\image-20200402142329488.png)

创建一个按钮 调用此方法

![image-20200402142448519](img\image-20200402142448519.png)

运行并点击按钮测试

![image-20200402142528376](img\image-20200402142528376.png)