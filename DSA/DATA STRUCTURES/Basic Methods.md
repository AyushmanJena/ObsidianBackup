---
lastSync: Mon Oct 21 2024 22:23:30 GMT+0530 (India Standard Time)
---
1. ~~Arrays~~
2. ~~ArrayList~~
3. ~~LinkedList~~
4. ~~Stack~~
5. ~~Queue~~
6. ~~HashMap~~
7. ~~HashSet~~
8. TreeSet
9. TreeMap
10. Graph
11. Tree

12. String Builder
### Arrays : 
```java
Arrays.sort(arr)
Arrays.equals(arr1, arr2) // compares content
Arrays.toString() 
Arrays.fill(arr, num)

Arrays.deepToString(arr) // for multidimensional arrays
Arrays.binarySearch(arr, target)
Arrays.asList(arr)
Arrays.deepEquals(arr1, arr2)
Arrays.copyOfRange(arr, fromIndex, endIndex)
```

### ArrayList
```java
ArrayList<Integer> arr = new ArrayList<>()
arr.add(val)
arr.add(index, val)
arr.remove(index)
```

### LinkedList (in built)
```java
LinkedList<Integer> list = new LinkedList<>();
list.add(val);
list.addLast(val);
list.addFirst(val)
list.remove(val);
list.removeFirst();
list.removeLast()
```

### Stack : (First In Last Out)
```java
Stack<Integer> stack = new Stack<>();
stack.push(num)
stack.pop()
stack.isEmpty()
stack.peek()
stack.search(target) // returns position
```

### Queue : (First In First Out)
```java
Queue<Integer> queue = new LinkedList<>();
// queue is an interface and obj of queue type cannot be created
queue.add(element)
queue.offer(element) // adds to end, returns false if full
queue.remove()
queue.poll() // returns null if empty
queue.peek()
```

### Deque
```java
Deque<Integer> deque = new ArrayDeque<>();
deque.add(value) -> add at tail 
deque.addFirst(value)
deque.addLast(value)
deque.removeFirst()
deque.removeLast()

deque.contains(value)
deque.getFirst()
deque.getLast()
deque.peek()
deque.size()

deque.pop()
deque.poll() // remove and return first, returns null if empty   
deque.pollFirst()
deque.pollLast()
```

### HashSet
```java
HashSet<Integer> set = new HashSet<>();
HashSet<Integer> set = new HashSet<>(initialCapacity)
set.add(val);
set.remove(val);
set.iterator() // iterating over hashset items
set.isEmpty()
set.size()
```

### HashMaps
```java
HashMap<Integer, String> map = new HashMap<>();
map.put("key", value)
map.get("key")
map.size()
map.containsKey("key")
map.remove("key")
map.containsValue(value)
map.putIfAbsent("key", 0);

map.compute(key, remappingfunction) // either update the value or create new entry if key does not exist

map.isEmpty()
map.keySet() -> returns a set of all keys
map.clear() -> removes all mappings form map
```

