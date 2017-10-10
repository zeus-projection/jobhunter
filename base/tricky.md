# tricky

#### HashMap 和 ArrayMap的区别

ArrayMap 用两个数组来存放数据，Hash索引数组，数据数组，有多少数据就用多大空间。

HashMap 空间一定是2的指数形式，比如17个元素，至少要占用32个数据空间。



####static import

static import就是允许在代码中直接引用别的类的static变量和方法（当然，在权限许可范围内），我们可以简单的把它当成import的延续。