## QMUI Android

来自腾讯的开源Android ui框架

[官方地址](https://qmuiteam.com/android)

### 引入方法

1. 在 app/build.gradle中引入

```groovy
 implementation 'com.qmuiteam:qmui:2.0.0-alpha07'
```

2. 配置主题

把项目的`theme`的`parent`指向`QMUI.Compat`。

`theme`位置在 app/main/AndroidManifest.xml中

3. 覆盖组件的默认表现

可以通过在项目中的 `theme` 中用 `(value)` 的形式来覆盖 QMUI 组件的默认表现。具体可指定的属性名请参考 `@style/QMUI.Compat` 或 `@style/QMUI` 中的属性。



