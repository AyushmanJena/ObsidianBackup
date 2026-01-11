Parent : `index / 2`
Left node : `index * 2 + 1`
Right Node : `index * 2 + 2`

#### Min Heap  : 
- The smallest number is at the first and can be accessed in constant time
- Every child node is larger than its parent node

Inserting into heap
- add new node to the end
- check if (parent node > current node) swap 
- no need to check the other child as it is already < the parent
- repeat this with iteration or recursion (index == 0(root is reached))
- this process is called UP-HEAP approach

Removing from heap :
- You can only remove the top (smallest element) thats the rule
- Replace the top element with the last element
- Keep comparing the parent, left child and right child, untill parent < both childs
- This process of moving downwards is called DOWN-HEAP approach

Inserting code : 
```java
public void insert(int value){
	list.add(value);
	upheap(list.size() - 1);
}
private void upheap(int index){
	if(index == 0){
		return;
	}
	int parent = index/2;
	if(list.get(index).compareTo(list.get(parent)) < 0){
		swap(index, parent);
		upheap(parent);
	}
}
```

Removing code : 
```java
public int remove(){
	if(list.isEmpty()) { 
		System.out.println("Removing from empty heap!!!");
		return -1;
	}
	int temp = list.get(0); // the head to remove
	int last = list.remove(list.size() -1);
	if(!list.isEmpty()){
		list.set(0, last); // store the last element in front
		downHeap(0);
	}
	return temp;
}

private void downHeap(int index){
	int min = index;
	int left = index*2 + 1;
	int right = index * 2+ 2;

	if(left < list.size() && list.get(min).compareTo(list.get(left)) < 0){
		min = left;
	}
	if(right < list.size() && list.get(min).compareTo(list.get(right)) < 0){
		min = right;
	}
	if(min != index){
		swap(min, index);
		downHeap(min);
	}
}
```

### HeapSort :
- If we keep removing the element from the top of the heap and store them in another array starting from backwards we would have a sorted array
```java
public ArrayList<Integer> heapSort(){
	ArrayList<Integer> data = new ArrayList<>();
	while(!list.isEmpty()){
		data.add(this.remove());
	}
	return data;
}
```


# LEETCODE QUESTIONS

[1584. Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)
#revise 
```java
class Solution {

    public int minCostConnectPoints(int[][] points) {
        int N = points.length;

        // Build adjacency list: {cost, node}
        List<int[]>[] adj = new ArrayList[N];
        for (int i = 0; i < N; i++) {
            adj[i] = new ArrayList<>();
        }

        for (int i = 0; i < N; i++) {
            int x1 = points[i][0];
            int y1 = points[i][1];

            for (int j = i + 1; j < N; j++) {
                int x2 = points[j][0];
                int y2 = points[j][1];

                int dist = Math.abs(x1 - x2) + Math.abs(y1 - y2);

                adj[i].add(new int[]{dist, j});
                adj[j].add(new int[]{dist, i});
            }
        }

        // Apply Prim's Algorithm
        return primMST(adj, N);
    }

    private int primMST(List<int[]>[] adj, int N) {
        boolean[] visited = new boolean[N];

        // Min-heap based on cost
        PriorityQueue<int[]> pq = new PriorityQueue<>(
                (a, b) -> a[0] - b[0]
        );

        // {cost, node}
        pq.offer(new int[]{0, 0});

        int totalCost = 0;
        int edgesUsed = 0;

        while (!pq.isEmpty() && edgesUsed < N) {
            int[] curr = pq.poll();
            int cost = curr[0];
            int node = curr[1];

            if (visited[node]) continue;

            visited[node] = true;
            totalCost += cost;
            edgesUsed++;

            // Add neighbors
            for (int[] next : adj[node]) {
                int nextCost = next[0];
                int nextNode = next[1];

                if (!visited[nextNode]) {
                    pq.offer(new int[]{nextCost, nextNode});
                }
            }
        }

        return totalCost;
    }
}

```