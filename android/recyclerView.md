# RecyclerView

#### ItemDecoration

RecyclerView 没有提供ListView的divider属性，相对应的提供了

```java
recyclerView.addItemDecoration()
```

我们通过重载3个方法实现分割效果（具体不展开了）

```java
public void onDraw(Canvas c, RecyclerView parent, State state);
public void onDrawOver(Canvas c, RecyclerView parent, State state);
public void getItemOffsets(Rect outRect, View view, RecyclerView parent, State state);
```

