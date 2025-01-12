Maze Questions

Return the count of all the possible paths to reach the end
- given that you can only travel right or down 
- and there are no obstacles 
- starting from 3, 3  and ends at 1, 1
```java
static int count(int r, int c){
	if(r == 1 || c == 1){
		return 1;
	}
	int left = count(r, c+1);
	int right = count(r+1, c);
	return left + right;
}
```

Printing all possible paths 
- with the constraints same as above
```java
public static void path(String s, int r, int c){
    if(r==1 && c== 1){
        System.out.println(s);
        return;
    }

    if(r > 1){
        path(s + 'D', r-1, c);
    }
    if(c > 1){
        path(s + 'R', r, c-1);
    }
}
```

Returning arraylist of strings 
- with the constraints same as above
```java
static ArrayList<String> pathRet(String p, int r, int c) {
    if (r == 1 && c == 1) {
        ArrayList<String> list = new ArrayList<>();
        list.add(p);
        return list;
    }

    ArrayList<String> list = new ArrayList<>();
    if (r > 1) {
        list.addAll(pathRet(p + 'D', r-1, c));
    }
    if (c > 1) {
        list.addAll(pathRet(p + 'R', r, c-1));
    }
    return list;
}
```

Including diagonals as a possibility
```java
public static void pathWithDiagonal(String s, int r, int c){
    if(c == 1 && r== 1){
        System.out.println(s);
        return;
    }

    if(r > 1 && c >1){
        pathWithDiagonal(s + 'D', r-1, c-1); //diagonally
    }
    if(r>1){
        pathWithDiagonal(s+'V', r-1, c); // down
    }
    if(c>1){
        pathWithDiagonal(s+'H', r, c-1);//right
    }
}
```

Mazes with closed paths 
- only down and right paths allowed 
- starts from 0, 0 ends at last cell
```java
//    boolean[][] maze = {
//            {true, true, true},
//            {true, false, true},
//            {true, true, true}
//    };
//    path("", maze, 0, 0);


public static void path(String s, boolean[][] maze, int r, int c){
    if(r == maze.length-1 && c == maze[0].length -1){
        System.out.println(s);
        return;
    }

    if(maze[r][c] == false){
        return;
    }

    if(r < maze.length-1){
        path(s + 'D',maze, r+1, c );
    }
    if(c < maze[0].length -1){
        path(s+'R', maze, r, c+1);
    }
}
```


