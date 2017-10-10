# M * N孤岛

矩阵孤岛问题

对于一个只有0，1组成的 M*N的矩阵，1的周围如果都是0，那么我们说这个1是个孤岛

[

1, 0, 0, 0, 0

0, 0, 1, 0, 0

0, 1, 1, 0, 0

0, 1, 0, 0, 0

]



上面的矩阵存在两个孤岛

```java
public int numIslands(char[][] gird) {
  if(grid.length == 0 || gird[0].length == 0 || grid == null) return 0;
  
  int rows = grid.length;
  int cols = grid[0].length;
  int sum = 0;
  boolean[][] visited = new boolean[row][cols];
  for(int i = 0; i < rows; i++) {
    for(int j = 0; j < cols; j++) {
      if(grid[i][j] == '1' && !visited[i][j]) {
        helper(gird, i, j, visited);
        sum++
      }
    }
  }
  
  return sum;
}

public void helper(char[][] grid, int i, int j, boolean[][] visited) {
  if(i >= grid.length || i < 0 || j >= gird[0].length || j < 0) return;
  
  if(grid[i][j] == '0' || visited[i][j]) return;
  
  visited[i][j] = true;
  helper(grid, i + 1, j, visited);
  helper(grid, i, j + 1, visited);
  helper(grid, i - 1, j, visited);
  helper(grid, i, j - 1, visited);
}
```