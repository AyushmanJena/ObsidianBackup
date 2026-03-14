## Note: 
Need to complete Graphs playlist by STRIVER

#### Matrix :

|     | A   | B   | C   |
| --- | --- | --- | --- |
| A   | 0   | 1   | 1   |
| B   | 1   | 0   | 0   |
| C   | 1   | 0   | 0   |
- edges between (a, b), (a,c) exist and 0 means no edges 
- we can replace 1 with integer values to signify weight of the edge
- Takes nxn space, so not appreciable

#### ArrayList
arraylist of arraylist 
two ways : 
1. Indexes represent nodes 
2. Each edge is represented by an internal arraylist

**Index represent nodes :** 
0 -> 
1 -> {2, 3}
2 -> {1, 4, 5}
3 -> {4, 1}
4 -> {3, 2, 5}
5 -> {2, 4}

1 has two connected edges to 2 and 3
2 has 3 connected edges and so on...

Undirected graph : 
```java
adj.get(1).add(2);
adj.get(2).add(1); // add both directional nodes
```
Directed graph : 
```java
adj.get(1).add(2); // edge from 1 to 2
```

**Each edge is represented by an internal Arraylist :** 
{{1,2}{3,4},{2,1},{4,3},{2,4}, {4,2}}

#### Weighted graphs : 
1. Using Matrix to store the weight of the edge
	instead of 1 `adj[u][j] = weight`
2. Using Lists 
	`4 -> {(2,1),(3,4)}` // 2-> connected node, 1-> weight
3. Using node representation list 
	`{2,1,5}` // 2->start node, 1->end node, 5->weight


### BFS Traversal
- Breadth First Search Traversal
```java
public static ArrayList<Integer> bfs( ArrayList<ArrayList<Integer>> adj){
	ArrayList<Integer> bfs = new ArrayList<>();
	boolean vis[] = new boolean[adj.size()];
	Queue<Integer> q = new LinkedList<>();

	q.add(0);
	vis[0] = true;

	while(!q.isEmpty()){
		Integer node = q.poll(); 
		// poll() : similar to remove() but returns null if queue empty
		bfs.add(node);
		// get all adjacent vertices of the dequeued vertex s
		// if a adjacent node has not been visited, then mark it visited and enqueu it
		for(Integer it : adj.get(node)){
			if(!vis[it]){
				vis[it] = true;
				q.add(it);
			}
		}
	}
}
```

### DFS Traversal
- Depth First Search Traversal
```java
public ArrayList<Integer> dfs(ArrayList<ArrayList<Integer>> adj){
	// boolean array to keep track of visited vertices
	boolean vis[] = new boolean[adj.size()];
	vis[0] = true;
	ArrayList<Integer> ls = new ArrayList<>();
	dfs(0, vis, adj, ls);
	return ls;
}

public static void dfs(int node, boolean vis[], ArrayList<ArrayList<Integer>> adj, ArrayList<Integer> ls){
	// marking current node as visited
	vis[node] = true;
	ls.add(node);

	// getting neighbour nodes
	for(Integer it: adj.get(node)){
		if(vis[it] == false){
			dfs(it, vis, adj, ls);
		}
	}
}
```

### Find Number of Provinces in a Graph
- Number of unconnected components
```java
public int numProvinces(ArrayList<ArrayList<Integer>> adj){
	ArrayList<ArrayList<Integer>> adjLs = new ArrayList<>();
	for(int i = 0; i<adj.size() ; i++){
		adjLs.add(new ArrayList<>());
	}

	// to change adjacency matrix to List
	for(int i = 0; i<adj.size(); i++){
		for(int j = 0; j<adj.size(); j++){
			// self nodes are not considered
			if(adj.get(i).get(j) == 1 && i != j){
				adjLs.get(i).add(j);
				adjLs.get(j).add(i);
			}
		}
	}
	
	inf vis[] = new int[adj.size()];
	int count = 0;
	for(int i = 0; i<adj.size(); i++){
		if(vis[i] == 0){
			count++;
			dfs(i, adjLs, vis);
		}
	}
	return count;
}

private void dfs(int node, ArrayList<ArrayList<Integer>> adjLs, int[] vis){
	vis[node] = 1;
	for(Integer it : adjLs.get(node)){
		if(vis[it] == 0){
			dfs(it, adjLs, vis);
		}
	}
}
```
Time Complexity : O(N) + O(V+2E) ~= O(N)
O(N) -> outer for loop
O(V+2E) -> V number of nodes, if each node is an province

Space Complexity : O(N) + O(N) : Nodes and The Recursion Stack

### Find Number of Connected Components in a Graph
- Given n x m grid matrix n(rows) and m(cols)
- where 0-> water and 1-> land
- Find number of islands 
- Islands formed by connecting adjacent lands horizontally, vertically or diagonally in all 8 directions

```java
public static int numIslands(char[][] grid){
	int n = grid.length;
	int m = grid[0].length;
	int[][] vis = new int[n][m];
	int count = 0;
	for(int row = 0; row < n; row++){
		for(int col = 0; col < m; col++){
			if(vis[row][col] == 0 && grid[row][col] == '1'){
				count++;
				bfs(row, col, vis, grid);
			}
		}
	}
	return count;
}

public static void bfs(int ro, int co, int[][] vis, char[][] grid){
	vis[ro][co] = 1; // mark the cell as visited
	Queue<Pair> q = new LinkedList<>();
	q.add(new Pair(ro,co));
	int n = grid.length;
	int m = grid[0].length;

	while(!q.isEmpty()){
		int row = q.peek().r;
		int col = q.peek().c;
		q.remove();

		// traverse in the neighbors and mark them if it is a land
		for(int delRow = -1; delRow <= 1; delRow++){ // check all 8 corners 
			for(int delcol = -1; delcol <= 1; delcol++){
                    int nrow = row+ delrow;
                    int ncol = col + delcol;
                    if(nrow >= 0 && nrow <n && ncol >= 0 && 
	                    ncol <m && grid[nrow][ncol] == '1' &&
	                    vis[nrow][ncol] == 0){
	                        vis[nrow][ncol] = 1;
	                        q.add(new Pair(nrow, ncol));
	                }
                }
            }
		}
	}
}
```
Time Complexity : ~~ O(N<sup>2</sup>)
Space Complexity : Visited Matrix + Queue  : O(N<sup>2</sup>) + O(N<sup>2</sup>)


#### Flood Fill Algorithm 
using DFS
- Given a starting row and starting column with value 1 (sr, sc)
```java
int[][] image = {
                {1, 1, 1},
                {1, 1, 0},
                {1, 0, 1}
        };
    int[][] image = {
                {2, 2, 2},
                {2, 2, 0},
                {2, 0, 1}
        };
```
- And a new color 2 
- color the adjacent cells with the new color and the adjacent cells as well if value of the adjacent cell is not equal to 0
- Note here only move in 4 directions and not in 8 directions like the previous question

```java
public int[][] floodfill(int[][] image, int sr, int sc, int newColor){
	int iniColor = image[sr][sc];
	int[][] ans = image;
	int[] delRow = {-1, 0, 1, 0};
	int[] delCol = {0, 1, 0, -1};
	dfs(sr, sc, ans, image, newColor, delRow, delCol, iniColor);
	return ans;
}

public static void dfs(int row, int col, int[][] ans, int[][] image, int newColor, int delRow[], int delCol[], int iniColor){
	ans[row][col] = newColor;
	int n = image.length;
	int m = image[0].length;

	for(int i = 0; i<4; i++){
		int nrow = row + delRow[i];
		int ncol = col + delCol[i];
		// check for valid coordinates 
		// then check if the same initial color and unvisited pixel
		if(nrow >= 0 && nrow < n && ncol >= 0 && ncol < m &&
			image[nrow][ncol] == iniColor && ans[nrow][ncol] != newColor){
				dfs(nrow, ncol, ans, image, newColor, delRow, delCol, iniColor);
			}
	}
}
```

Time Complexity : O(mxn) // if all nodes are connected
Space Complexity  : O(nxm) + O(nxm) // new matrix + recursive stack


#### Rotten Oranges
0 -> empty cell
1 -> fresh oranges
2 -> rotten oranges

find minimum time to rot all oranges, 
any rotten orange can rot all oranges in left, right, up, down in unit time
```txt
0 1 2
0 1 2 -> ans = 1
2 1 1

if remaining oranges not rotten -> return -1

intuition : 
- use queue for bfs traversal
- visited array for marking oranges

push all rotten orange pos into the queue initially

pick rotten orange remove it from queue
 and add all its neighbouring fresh oranges into the queue
 and repeat
 mark them all rotten in visited array 
 
increment time at each level
```
applying BFS
```java
class Solution {
    public int orangesRotting(int[][] grid) {
        int freshCount = 0;
        int time = 0;
        Queue<Pair> queue = new LinkedList<>();
        int n = grid.length;
        int m = grid[0].length;

        for(int i =0; i< n; i++){
            for(int j = 0; j < m; j++){
                if(grid[i][j] == 1){
                    freshCount++;
                }else if(grid[i][j] == 2){
                    queue.add(new Pair(i, j));
                }
            }
        }

        if(freshCount == 0) return 0;

        while(!queue.isEmpty() && freshCount > 0){
            int size = queue.size();
            for(int i = 0; i< size; i++){
                Pair p = queue.poll();
                if(isValid(p.x + 1, p.y, n, m) && (grid[p.x +1][p.y] == 1)){
                    grid[p.x + 1][p.y] = 2;
                    freshCount--;
                    queue.add(new Pair(p.x+1, p.y));
                }
                if(isValid(p.x - 1, p.y, n, m) && (grid[p.x - 1][p.y] == 1)){
                    grid[p.x - 1][p.y] = 2;
                    freshCount--;
                    queue.add(new Pair(p.x-1, p.y));
                }
                if(isValid(p.x, p.y + 1, n, m) && (grid[p.x][p.y + 1] == 1)){
                    grid[p.x][p.y + 1] = 2;
                    freshCount--;
                    queue.add(new Pair(p.x, p.y+1));
                }
                if(isValid(p.x, p.y - 1, n, m) && (grid[p.x][p.y - 1] == 1)){
                    grid[p.x][p.y - 1] = 2;
                    freshCount--;
                    queue.add(new Pair(p.x, p.y-1));
                }
            }
            time++;
        }

        if(freshCount != 0){
            return -1;
        }
        return time;
    }

    public boolean isValid(int x , int y, int n, int m){
        if(x >= 0 && y >= 0 && x < n && y < m){
            return true;
        }
        return false;
    }

    class Pair{
        int x;
        int y;
        Pair(int x, int y){
            this.x = x;
            this.y = y;
        }
    }
}
```


### Detecting Cycle in an Undirected Graph 
##### Using BFS
- generate adjacency list for the graph
- create a visited array and initialize all nodes as not visited
- For every unvisited node, start BFS (if has disconnected graphs).
- define queue and push the first node as `(startNode, parent = -1)`
- Mark start node visited
- While queue is not empty, pop `(currentNode, parent)`
- For each adjacent node of currentNode : 
	- If it is not visited, mark it visited and push `(adjacentNode, currentNode)` into the queue
	- If it is already visited and not equals to `parent`, then a cycle exists
- If BFS finishes without finding any such cases, then no cycle exists

```java
public boolean hasCycle(int V, List<List<Integer>> adj){
	boolean[] visited = new boolean[V];
	
	// handle disconnected components
	for(int i = 0; i< V; i++){
		if(!visited[i]){
			if(bfsCheck(i, adj, visited)) return true;
		}
	}
	return false;
}


private static boolean bfsCheck(int start, List<List<Integer>> adj, boolean[] visited){
	Queue<int[]> queue = new LinkedList<>();
		
	// int[]  = {node, parent}
	queue.offer(new int[]{start, -1});
	visited[start] = true;
		
	while(!queue.isEmpty()){
		int[] current = queue.poll();
		int node = current[0];
		int parent = current[1];
			
		for(int neighbor : adj.get(node)){
			if(!visited[neightbor]){
				visited[neighbor] = true;
				queue.offer(new int[]{neighbor, node});
			}else if(neighbor != parent){
				return true;
			}
		}
	}
	return false;
}
```
Time complexity : O (V + 2E) : BFS
Space Complexity : O(N) + O(N)  : queue and visited
					~ O(N)

##### Using DFS
- Generate the adjacency list for the graph
-  build a visited array and mark all nodes as unvisited
- For every unvisited node, start DFS (to handle disconnected graphs)
- Call dfs(currentNode, parent) for the unvisited node
- Mark current node visited
- For each adjacent node of currentNode : 
	- If the adjacent node is not visited, recursively call dfs(adjacentNode, currentNode)
	- If the adjacent node is already visited and not equals to parent, then a cycle exists
- If DFS finishes without finding such a case, then no cycle exists

```java
public boolean hasCycle(int V, List<List<Integer>> adj){
	boolean[] visited = new boolean[V];
	
	// handling disconnected graph
	for(int i =0; i< V; i++){
		if(!visited[i]){
			if(dfs(i, -1, visited, adj)){
				return true;
			}
		}
	}
	return false;
}

public boolean dfs(int node, int parent, boolean[] visited, List<List<Integer>> adj){
	visited[node] = true;
	for(int neighbor: adj.get(node)){
		if(!visited[neighbor]){
			if(dfs(neighbor, node, visited, adj)){
				return true;
			}
		}else if(neighbor != parent){
			return true; // cycle detected
		}
	}
	return false;
}
```


Time complexity : O (V + 2E) : BFS
Space Complexity : O(N) + O(N)  : recursion stack and visited
					~ O(N)



### Distance of Nearest Cell having 1
row or col only no diagonal distance
```
1 0 1        for (0, 0) itself so ans = 0
1 1 0    ->   for (0, 1) left 1, right 1 or down 1 so ans = 1
1 0 0         for(2, 2) ans = 2

answer matrix : 
0 1 0
0 0 1
0 1 2
```

Solving using BFS
- Create a `visited[][]` matrix initialized to false
- Create a distance matrix to store the answer
- Create a queue to store `(row, col, dist)`
- Traverse the grid and push all cells having value 1 into the queue with distance = 0
- Mark all these cells as visited
- While the queue is not empty : 
	- Pop(row, col, dist) from the queue
	- Set `distance[row][col] = dist`
	- Move in all 4 directions (up, down, left, right)
	- For each valid neighbor cell : 
		- If it is inside the grid and not visited
		- then mark it visited
		- Push `(newRow, newCol, dist + 1)` into the queue
- After BFS finishes, `distance[][]` will contain the nearest distance to a cell having 1
```java
static class Cell {  
	int row, col, dist;  
  
	Cell(int row, int col, int dist) {  
	this.row = row;  
	this.col = col;  
	this.dist = dist;  
	}  
}

public int[][] nearestOne(int[][] grid){
	int n = grid.length;
	int m = grid[0].length;
	
	int[][] distance = new int[n][m];
	boolean[][] visited = new boolean[n][m];
	
	Queue<Cell> queue = new LinkedList<>();
	
	// push all cells having value 1 into queue
	for(int i = 0; i< n; i++){
		for(int j = 0; j <m; j++){
			if(grid[i][j] == 1){
				queue.offer(new Cell(i, j, 0));
				visited[i][j] = true;
			}
		}
	}
	
	// 4 directions (up, down, left, right)
	while(!queue.isEmpty()){
		Cell current = queue.poll();
		int row = current.row;
		int col = current.col;
		int dist = current.dist;
		
		distance[row][col] = dist;
		
		// UP  
		if (row - 1 >= 0 && !visited[row - 1][col]) {  
			visited[row - 1][col] = true;  
			queue.offer(new Cell(row - 1, col, dist + 1));  
		}  
		  
		// DOWN  
		if (row + 1 < n && !visited[row + 1][col]) {  
			visited[row + 1][col] = true;  
			queue.offer(new Cell(row + 1, col, dist + 1));  
		}  
		  
		// LEFT  
		if (col - 1 >= 0 && !visited[row][col - 1]) {  
			visited[row][col - 1] = true;  
			queue.offer(new Cell(row, col - 1, dist + 1));  
		}  
		  
		// RIGHT  
		if (col + 1 < m && !visited[row][col + 1]) {  
			visited[row][col + 1] = true;  
			queue.offer(new Cell(row, col + 1, dist + 1));  
		}
	}
	return distance;
}
```

Similar leetcode problem :
[542. 01 Matrix](https://leetcode.com/problems/01-matrix/)
Distance of the nearest cell having 0 for each cell


### Replace O's with X's
Given N x M matrix where every element is either O or X 
Replace all O with X that are surrounded by X in all 4 directions
It can also have a set of Os which are together surrounded by X then it will also be converted

```
X X X X             X X X X 
X O X X             X X X X 
X O O X         ->  X X X X 
X O X X             X X X X
X X O O             X X O O

- The below 2 Os are not surrounded by X on right and bottom
```

- Observation : If the island of Os is connected to a boundary then it is invalid, but if it is not connected to any boundary then it is bound to be surrounded by X
- Start from the boundary Os and mark their islands  that they cannot be converted.
- Convert the remaining Os

Algorithm:
- Build a visited matrix initialized with all Os
- Loop over the matrix for all the borders and call DFS on the cell with Os
- Mark the cell visited
- go to all the neighbor Os and mark them visited too and so on recursively
- Now loop over the entire matrix matrix and call DFS on cells with Os 
- Convert the O to X
- Go to all the neighbors, and if any cell is marked visited then do not convert it into X since it is connected to boundary

```java
void solve(char[][] board){
	int m = board.length;
	int n = board[0].length;
	
	boolean[][] visited = new boolean[m][n];
	
	// traverse first and last row
	for(int j = 0; j < n; j++){
		if(board[0][j] == 'O' && !visited[0][j])
			dfs(board, visited, 0, j);
			
		if(board[m-1][j] == 'O' && !visited[m - 1][j]){
			dfs(board, visited, m-1, j);
		}
	}
	
	// traverse first and last column
	for(int i = 0; i < m; i++){
		if(board[i][0] == 'O' && !visited[i][0])
			dfs(board, visited, i, 0);
			
		if(board[i][n-1] == 'O' && !visited[i][n-1])
			dfs(board, visited, i, n-1);
	}
	
	// Convert surrounded O to X  
	for (int i = 0; i < m; i++) {  
		for (int j = 0; j < n; j++) {  
			if (board[i][j] == 'O' && !visited[i][j]) {  
				board[i][j] = 'X';  
			}
		}  
	}
}

void dfs(char[][] board, boolean[][] visited, int r, int c){
	int m = board.length; 
	int n = board[0].length;
	
	if(r < 0 || c < 0 || r >= m || c >= n){
		return;
	}
	
	if(board[r][c] != 'O' || visited[r][c]){
		return;
	}
	
	visited[r][c] = true;
	
	dfs(board, visited, r+1, c);
	dfs(board, visited, r-1, c);
	dfs(board, visited, r, c+1);
	dfs(board, visited, r, c-1);
}
```



### Number of Enclaves
You are given n x m binary matrix, where 0 represents a sea cell and 1 represents a land cell
A move consists of walking from one land cell to another adjacent (4-directionally) land cell or walking off the boundary of the grid
Find the number of land cells in grid for which we cannot walk off the boundary of the grid in any number of moves

```
0 0 0 1
0 1 1 0
0 1 1 0     ->  
0 0 0 1
0 1 1 0

 the middle four 1s
 but for the other 1's you can move out of the boundary 
 
 so answer = 4
```

approach : 
- start from the cells which are land and are connected to boundary
- perform bfs/dfs on those cells and cancel them out
- the remaining land cells which are not reached will be the answers

algorithm : (applying bfs)
- Create a Queue data structure
- Traverse the whole matrix and Push all boundary land cells 
- convert them to 0s / create another visited matrix and mark them visited there 
- pop the first from queue and perform bfs i.e.
	- traverse in all 4 directions that are 
	- not visited and are land 
	- push them into the queue as well
- Continue poping and performing bfs on them untill the queue is empty
- traverse the matrix and count all the remaining lands

```java
public static int numEnclaves(int[][] grid) {

    int n = grid.length;
    int m = grid[0].length;

    Queue<int[]> queue = new LinkedList<>();

    // Push boundary land cells
    for(int i = 0; i < n; i++){
        for(int j = 0; j < m; j++){
            if(i == 0 || j == 0 || i == n - 1 || j == m - 1){
                if(grid[i][j] == 1){
                    queue.add(new int[]{i, j});
                    grid[i][j] = 0;
                }
            }
        }
    }

    bfs(queue, grid, n, m);

    // Count remaining land cells
    int count = 0;
    for(int i = 0; i < n; i++){
        for(int j = 0; j < m; j++){
            if(grid[i][j] == 1){
                count++;
            }
        }
    }

    return count;
}

public static void bfs(Queue<int[]> queue, int[][] grid, int n, int m){

    while(!queue.isEmpty()){

        int[] cell = queue.poll();
        int row = cell[0];
        int col = cell[1];

        // Up
        if(row - 1 >= 0 && grid[row - 1][col] == 1){
            queue.add(new int[]{row - 1, col});
            grid[row - 1][col] = 0;
        }

        // Down
        if(row + 1 < n && grid[row + 1][col] == 1){
            queue.add(new int[]{row + 1, col});
            grid[row + 1][col] = 0;
        }

        // Left
        if(col - 1 >= 0 && grid[row][col - 1] == 1){
            queue.add(new int[]{row, col - 1});
            grid[row][col - 1] = 0;
        }

        // Right
        if(col + 1 < m && grid[row][col + 1] == 1){
            queue.add(new int[]{row, col + 1});
            grid[row][col + 1] = 0;
        }
    }
}
```



### Number of Distinct Islands : 
Given a boolean 2D matrix grid of size `n*m`. You have to find the number of distinct islands where a group of connected 1s (horizontally or vertically) forms an island. 
Two islands are considered to be distinct if and only if one island is equal to another (not rotated or reflected)
```
1 1 0 1 1
1 0 0 0 0
0 0 0 0 1
1 1 0 1 1

There are two 1 1 that are identical
the two islands with 3 ones are rotated and not identical

So answer = 3 total unique distinct islands


1 1 0 1 1
1 0 0 0 0
0 0 0 1 1
1 1 0 1 0

ans = 2
```

Approach : 
add a hashset to store all arraylist of  (row, col) 




# Dijkstra's Algorithm
```
for each vertex v : 
	dist[v]= max_value
	prev[v] = none
dist[source] = 0

set all vertices to unexplored
while destination not explored:
	v = least-valued unexplored vertex
	set v to explored
	for each edge (v, w):
		if dist[v] + len(v, w) < dist[w]:
			dist[w] = dist[v] + len(v, w)
			prev[w] = v
```
code: 
```java
import java.util.*;

public class Main {

    static class Pair {
        int distance;
        int node;

        Pair(int distance, int node) {
            this.distance = distance;
            this.node = node;
        }
    }

    public static void main(String[] args) {

        int V = 5;
        ArrayList<ArrayList<ArrayList<Integer>>> adj = new ArrayList<>();

        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }

        addEdge(adj, 0, 1, 2);
        addEdge(adj, 0, 4, 8);
        addEdge(adj, 1, 2, 3);
        addEdge(adj, 4, 2, 2);
        addEdge(adj, 2, 3, 6);
        addEdge(adj, 4, 3, 4);

        int result = dijkstraFrom0ToN(V, adj);

        System.out.println("Shortest distance from 0 to " + (V - 1) + " is: " + result);
    }

    // Dijkstra from node 0 to node N-1
    static int dijkstraFrom0ToN(int V, ArrayList<ArrayList<ArrayList<Integer>>> adj) {

        PriorityQueue<Pair> pq =
                new PriorityQueue<>((a, b) -> a.distance - b.distance);

        int[] dist = new int[V];
        Arrays.fill(dist, Integer.MAX_VALUE);

        dist[0] = 0; // source is 0
        pq.add(new Pair(0, 0));

        while (!pq.isEmpty()) {

            Pair curr = pq.poll();
            int node = curr.node;
            int dis = curr.distance;

            if (dis > dist[node]) continue;

            // If we reached N-1, we can stop
            if (node == V - 1) {
                return dis;
            }

            for (ArrayList<Integer> edge : adj.get(node)) {
                int adjNode = edge.get(0);
                int weight = edge.get(1);

                if (dis + weight < dist[adjNode]) {
                    dist[adjNode] = dis + weight;
                    pq.add(new Pair(dist[adjNode], adjNode));
                }
            }
        }

        return -1; // destination unreachable
    }

    static void addEdge(ArrayList<ArrayList<ArrayList<Integer>>> adj,
                        int u, int v, int w) {

        adj.get(u).add(new ArrayList<>(Arrays.asList(v, w)));
        adj.get(v).add(new ArrayList<>(Arrays.asList(u, w)));
    }
}

```




# LeetCode Questions

[Minimize the Maximum Edge Weight of Graph](https://leetcode.com/problems/minimize-the-maximum-edge-weight-of-graph/)
Solution Reference :    [Solution Youtube](https://www.youtube.com/watch?v=DXinKbQQDVE)
```java
class Solution {
    public int minMaxWeight(int n, int[][] edges, int threshold) {
        int minW = Integer.MAX_VALUE;
        int maxW = Integer.MIN_VALUE;
        for(int[] e : edges){
            minW = Math.min(minW, e[2]);
            maxW = Math.max(maxW, e[2]);
        }

         // performing binary search for finding the minimum weight 
        int ans = -1;
        int l = minW;
        int h = maxW;
        while(l <= h){
            int mid = l + (h-l)/2;
            if(isPossible(n, edges, mid)){
                ans = mid;
                h = mid - 1;
            }else{
                l = mid + 1;
            }
        }

        return ans;
    }

    public boolean isPossible(int n, int[][] edges, int maxWeight){ 
    // check if all conditions are met with the given maxWeight value
        List<List<Integer>> revGraph = new ArrayList<>();

        for(int i =0; i<n; i++){
            revGraph.add(new ArrayList<>()); // Initializes the graph (start point)
        }

        for(int[] e : edges){
            if(e[2] <= maxWeight){
                revGraph.get(e[1]).add(e[0]); 
                // reverses the graph and stores in the new graph list
            }
        }

        boolean[] visited = new boolean[n]; // to track visited nodes
        Queue<Integer> q = new LinkedList<>();
        q.offer(0); 
        // add to queue without size constraints 
        visited[0] = true;
        int count = 1; 
        // count of number of nodes that can be visited with current maxWeight

        while(!q.isEmpty()){ // usual graph traversal
            int cur = q.poll();
            for(int nxt : revGraph.get(cur)){
                if(!visited[nxt]){
                    visited[nxt] = true;
                    q.offer(nxt);
                    count++;
                }
            }
        }

        return count == n; 
        // returns true if all the nodes can be reached starting from 0
    }
}
```


[695. Max Area of Island](https://leetcode.com/problems/max-area-of-island/)
```java
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int max = 0;
        for(int i = 0; i < grid.length; i++){
            for(int j = 0; j< grid[i].length; j++){
                if(grid[i][j] == 1){
                    int curr = calculateSize(grid, i, j);
                    max = Math.max(curr, max);
                }
            }
        }
        return max;
    }

    public int calculateSize(int[][] grid, int i, int j){

        if( i < 0 || j < 0 || i >= grid.length || j >= grid[i].length || grid[i][j] == 0){
            return 0;
        }

        grid[i][j] = 0;

        return 1 + calculateSize(grid, i+1, j) + calculateSize(grid, i-1, j) +
                calculateSize(grid, i, j+1) + calculateSize(grid, i, j-1);
    }
}
```

[543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)
```java
class Solution {
    int maxPath = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        traverse(root);
        return maxPath;
    }

    public int traverse(TreeNode node){
        if(node == null){ 
            return 0;
        }
 
        int left = traverse(node.left);
        int right = traverse(node.right);

        maxPath = Math.max(maxPath, left+right);

        return 1 + Math.max(left, right);
    }


}
```

[2976. Minimum Cost to Convert String I](https://leetcode.com/problems/minimum-cost-to-convert-string-i/)
```java
class Solution {
    public long minimumCost(String source, String target, char[] original, char[] changed, int[] cost) {
        int[][] dis = new int[26][26];
        for(int i = 0 ; i < 26 ;i++){
            Arrays.fill(dis[i] , Integer.MAX_VALUE);
            dis[i][i] = 0 ;
        }
        
        for(int i = 0 ; i < cost.length ; i++){
            dis[original[i] - 'a'][changed[i] - 'a'] = Math.min(dis[original[i] - 'a'][changed[i] - 'a'],cost[i]);
        }

        for(int k = 0 ; k < 26 ; k++){
            for(int i = 0 ; i < 26 ; i++){
                if(dis[i][k] < Integer.MAX_VALUE){
                    for(int j = 0 ; j < 26 ; j++){
                        if(dis[k][j] < Integer.MAX_VALUE){
                            dis[i][j] = Math.min(dis[i][j] , dis[i][k] + dis[k][j]);
                        }
                    }
                }
            }
        }

        long ans = 0L ;
        for(int i = 0 ; i <source.length() ;i++){
            int c1 = source.charAt(i) - 'a';
            int c2 = target.charAt(i) - 'a';
            if(dis[c1][c2] == Integer.MAX_VALUE){
                return -1L;
            }
            ans += (long)dis[c1][c2];
        }

        return ans ;
    }
}
```