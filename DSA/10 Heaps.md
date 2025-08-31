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