Make notes from DOM 1 and 2

![[assets/Pasted image 20260827100423.png]]

```
<html> -> document.documentElement
<body> -> document.body
<head> -> document.head
```

childNodes, firstChild and lastChild

```
elem.hasChildNodes() -> checks whether there are any child nodes
elem.childNodes[0] === elem.firstChild
elem.childNodes[elem.childNodes.length - 1] === elem.lastChild
```

show all child nodes : (its a collection, not an array)
```js
for (let node of document.body.childNodes) {
  alert(node); // shows all nodes from the collection
}
```

these collections are read only and cannot be replaced/modified

Parent and sibling nodes : 
```
elem.parentNode -> returns the parent node 
elem.nextSibling -> next sibling node
elem.previousSibling 
```

