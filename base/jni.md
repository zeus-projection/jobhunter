# JNI

##### JNI数组传递

- 对于小量的，固定大小的数组，应该选择Get／SetArrayRegion函数来操作数组元素效率最高，因为这对函数要求提前分配一个C的临时缓冲区来存储数组元素，我们可以直接在Stack上或用malloc在堆上来动态申请，当然在栈上申请最快。 **访问数组如果将原始数据全部拷贝一份到临时缓冲区才能访问，效率会不会很低？不会，这种复制少量数组元素的代价是很小的，几乎可以忽略。** 这对函数的另外一个优点是，允许你传入一个开始索引和长度来对子数组元素的访问和操作。
- 如果不想预先分配C缓冲区，并且原始数组长度也不确定，而本地代码又不想再获取数组元素指针时被阻塞的话，使用Get／ReleasePrimitiveArrayCritical函数对，就像使用Get／ReleaseStringCritical对一样，这对函数要非常小心，以免死锁。
- Get/Release<type>ArrayElements系列函数永远是安全的，JVM会选择性的返回一个指针，这个指针可能指向原始数据，也可能指向原始数据的复制。
- 访问对象数组，GetObjectArrayElement返回数组中指定位置的元素，SetObjectArrayElement修改数组中指定位置的元素。