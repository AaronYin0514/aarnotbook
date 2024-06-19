# 安卓常见问题

## Android 键盘将底部导航栏/按钮顶起

`android/app/src/main/AndroidManifest.xml`，修改`android:windowSoftInputMode` 属性值

```
android:windowSoftInputMode="adjustResize"
```

修改为：

```
android:windowSoftInputMode="stateAlwaysHidden|adjustPan"
```

