# 常见编译问题的解决方案

### Failed to resolve : com.android.support:appcompat-v7:26.1.0

解决方案

在 **project** 层的**build.gradle**中添加 

```xml
maven { url "https://maven.google.com"}
```

完成代码如下

```xml
allprojects {
    repositories {
        jcenter()

		///here
        maven {
            url "https://maven.google.com"
        }
		///
    }

}
```



### Error:(18, 21) No resource found that matches the given name: attr 'android:keyboardNavigationCluster'.

引入API26的问题，解决方案

项目的**build.gradle** 的着三个参数改成**26**

```xml
compileSdkVersion 26
buildToolsVersion "26.0.2"
targetSdkVersion 26
```

