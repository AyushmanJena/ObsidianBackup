# PROJECT : TODO App
Continuing Context APIs

Create a new Context TodoContext :
```js
import {createContext, useContext} from "react";

export const TodoContext = createContext({
  todos: [
    {
      id: 1, 
      todo : "Learn React",
      completed: false,
    },
  ],

  addTodo: (todo) => {},
  updateTodo: (id, todo) => {},
  deleteTodo: (id) => {},
  toggleComplete: (id) => {},
});


export const useTodo = () => {
  return useContext(TodoContext);
}

export const TodoProvider = TodoContext.Provider;

```

Create an index.js in contexts that would export all needed files. 
So when needed we can only import index and have access to all hooks, contexts, etc.

index.js
```js
export {TodoContext, TodoProvider, useTodo} from "./TodoContext";
```

App.jsx
```jsx

```

Second use effect is to avoid fetching from the local storage everytime new todo is added. (This is one way to do it)
With change it todos it is written only and changed in ui but not fetching the whole data again.

index.js in components
```js
import TodoForm from "./TodoForm";
import TodoItem from "./TodoItem";

export {
  TodoForm,
  TodoItem
}
```
index files are used to export items together 

TodoForm.jsx
```jsx

```

TodoItem.jsx
```jsx

```

49:38