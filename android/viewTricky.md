# ViewTricky

#### EditText初始不弹出软键盘

在另外一个View中强制获取焦点

```xml
android:focusableInTouchMode="true"
```

在activity中设置属性

```xml
android:windowSoftInputMode="stateHidden"
```

代码中设置

```java
InputMethodManager inputMethodManager = (InputMethodManager)this.getSystemService(Context.INPUT_METHOD_SERVICE);
inputMethodManager.hideSoftInputFromWindow(et.getWindowToken(), 0);
```

