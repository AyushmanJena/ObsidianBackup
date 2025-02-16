
[841. Keys and Rooms](https://leetcode.com/problems/keys-and-rooms/)
```java
class Solution {
    public boolean canVisitAllRooms(List<List<Integer>> rooms) {
        boolean[] unlocked = new boolean[rooms.size()];
        visitRooms(0, rooms, unlocked);

        // loop around all rooms to check if all are true'
        for(boolean f : unlocked){
            if(!f){
                return false;
            }
        }
        return true;
    }

    public void visitRooms(int curr, List<List<Integer>> rooms, boolean[] unlocked)
    {
        unlocked[curr] = true;
        // list.add(curr);

        for(int i : rooms.get(curr)){
            if(!unlocked[i]){
                visitRooms(i, rooms, unlocked);
            }
        }
    }
}
```

[547. Number of Provinces](https://leetcode.com/problems/number-of-provinces/) #todo 
```java
class Solution {
    public static void dfs(int[][] isConnected,int curr,boolean vis[]){
        vis[curr]=true;
        for(int i=0;i<isConnected.length;i++){
            if(!vis[i]&&isConnected[curr][i]==1){
                dfs(isConnected,i,vis);
            }
        }
    }
    public int findCircleNum(int[][] isConnected) {
        boolean vis[]=new boolean[isConnected.length];
       int  count=0;
        for(int i=0;i<isConnected.length;i++){
            if(!vis[i]){
                count++;
                dfs(isConnected,i,vis);
            }
        }
        return count;
    }
}
```

[1466. Reorder Routes to Make All Paths Lead to the City Zero](https://leetcode.com/problems/reorder-routes-to-make-all-paths-lead-to-the-city-zero/) #todo 
```java
class Solution {
    public int minReorder(int n, int[][] connections) {
        HashMap<Integer, ArrayList<Pair>> adj = new HashMap<>();
		for (int[] path : connections) {
		    adj.putIfAbsent(path[0], new ArrayList<>());
		    adj.putIfAbsent(path[1], new ArrayList<>());
		    adj.get(path[0]).add(new Pair(path[1], 1));
		    adj.get(path[1]).add(new Pair(path[0], 0));
		}
		return minOrderRec(adj, -1, 0);
    }
    public int minOrderRec(HashMap<Integer, ArrayList<Pair>> adj, int parent, int curr){
		int reorderCount = 0;

		for(Pair pair : adj.get(curr)){
			int nextCity = pair.from;
			int needsReorder = pair.to;

			if(nextCity != parent){
				reorderCount += needsReorder;
				reorderCount += minOrderRec(adj, curr, nextCity);
			}
		}

		return reorderCount;
	}
    public class Pair{
		int from;
		int to;
		Pair(int x, int y){
			from = x;
			to = y;
		}
	}


}
```

[399. Evaluate Division](https://leetcode.com/problems/evaluate-division/) #hard 
```java
class Solution {
	class Node{
		String key;
		double val;
		public Node(String k, double v){
			key = k;
			val = v;
		}
	}

	public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries){
		Map<String, List<Node>> g = buildGraph(equations, values);
		double[] result = new double[queries.size()];
		for(int i = 0; i<queries.size(); i++){
			result[i] = dfs(queries.get(i).get(0), queries.get(i).get(1), new HashSet(), g);
		}
		return result;
	}

	private double dfs(String s, String d, Set<String> visited, Map<String, List<Node>> g){
		if(!(g.containsKey(s) && g.containsKey(d))){
			return -1.0;
		}
		if(s.equals(d)){
			return 1.0;
		}
		visited.add(s);
		for(Node ng : g.get(s)){
			if(!visited.contains(ng.key)){
				double ans = dfs(ng.key, d, visited, g);
				if(ans != -1.0){
					return ans * ng.val;
				}
			}
		}
		return -1.0;
	}

	private Map<String, List<Node>> buildGraph(List<List<String>> eq, double[] values){
		Map<String, List<Node>> g = new HashMap<>();
		for(int i = 0; i<values.length; i++){
			String src = eq.get(i).get(0);
			String des = eq.get(i).get(1);
			g.putIfAbsent(src, new ArrayList<>());
			g.putIfAbsent(des, new ArrayList<>());
			g.get(src).add(new Node(des, values[i]));
			g.get(des).add(new Node(src, 1/values[i]));
		}
		return g;
	}
}
```