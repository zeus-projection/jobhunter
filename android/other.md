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

