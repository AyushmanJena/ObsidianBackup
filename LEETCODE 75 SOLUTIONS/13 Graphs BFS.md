
[1926. Nearest Exit from Entrance in Maze](https://leetcode.com/problems/nearest-exit-from-entrance-in-maze/)
```java
class Solution {
    public class Node {
        int r, c, steps;
        
        public Node(int row, int col, int steps) {
            r = row;
            c = col;
            this.steps = steps;
        }
    }

    public int nearestExit(char[][] maze, int[] entrance) {
        Queue<Node> queue = new LinkedList<>();
        int rows = maze.length, cols = maze[0].length;
        
        queue.add(new Node(entrance[0], entrance[1], 0));
        maze[entrance[0]][entrance[1]] = '+';

        while (!queue.isEmpty()) {
            Node node = queue.remove();

            if (isExit(node.r, node.c, entrance, rows, cols)) {
                return node.steps;
            }

            // Right
            if (isValid(maze, node.r, node.c + 1)) {
                maze[node.r][node.c + 1] = '+';
                queue.add(new Node(node.r, node.c + 1, node.steps + 1));
            }

            // Down
            if (isValid(maze, node.r + 1, node.c)) {
                maze[node.r + 1][node.c] = '+';
                queue.add(new Node(node.r + 1, node.c, node.steps + 1));
            }

            // Left
            if (isValid(maze, node.r, node.c - 1)) {
                maze[node.r][node.c - 1] = '+';
                queue.add(new Node(node.r, node.c - 1, node.steps + 1));
            }

            // Up
            if (isValid(maze, node.r - 1, node.c)) {
                maze[node.r - 1][node.c] = '+';
                queue.add(new Node(node.r - 1, node.c, node.steps + 1));
            }
        }
        
        return -1;
    }

    private boolean isValid(char[][] maze, int r, int c) {
        return (r >= 0 && r < maze.length && c >= 0 && c < maze[0].length && maze[r][c] == '.');
    }

    private boolean isExit(int r, int c, int[] entrance, int rows, int cols) {
        if (r == entrance[0] && c == entrance[1]) return false;
        return r == 0 || c == 0 || r == rows - 1 || c == cols - 1;
    }
}
```

[994. Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)
```java
class Solution {
    public int orangesRotting(int[][] grid) {
        Queue<Node> queue = new LinkedList<>();
        int freshCount = 0;
        int time = 0;

        for(int i = 0; i<grid.length; i++){
            for(int j =0; j<grid[0].length; j++){
                if(grid[i][j] == 1){
                    freshCount++;
                }
                if(grid[i][j] == 2){
                    queue.add(new Node(i, j));
                }
            }
        }

        if(freshCount == 0) return 0;

        while(!queue.isEmpty() && freshCount >0){
            int size = queue.size();
            for(int i = 0; i< size ; i++){
                Node node = queue.poll();
                if(isValid(grid, node.r, node.c+1) && grid[node.r][node.c+1]== 1){
                    grid[node.r][node.c+1] = 2;
                    freshCount--;
                    queue.add(new Node(node.r, node.c+1));
                }
                if(isValid(grid, node.r, node.c-1) && grid[node.r][node.c-1]== 1){
                    grid[node.r][node.c-1] = 2;
                    freshCount--;
                    queue.add(new Node(node.r, node.c-1));
                }
                if(isValid(grid, node.r+1, node.c) && grid[node.r+1][node.c]== 1){
                    grid[node.r+1][node.c] = 2;
                    freshCount--;
                    queue.add(new Node(node.r+1, node.c));
                }
                if(isValid(grid, node.r-1, node.c) && grid[node.r-1][node.c]== 1){
                    grid[node.r-1][node.c] = 2;
                    freshCount--;
                    queue.add(new Node(node.r-1, node.c));
                }
            }
            time++;
        }

        if(freshCount != 0){
            return -1;
        }
        return time;
    }

    public boolean isValid(int[][] grid, int r, int c){
        if(r>=0 && r< grid.length && c>=0 && c<grid[0].length){
            return true;
        }
        return false;
    }

    public class Node{
        int r;
        int c;
        public Node(int i, int j ){
            r = i;
            c = j;
        }
    }
}
```