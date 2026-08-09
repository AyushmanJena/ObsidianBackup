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
import React from 'react'
import { useState } from 'react';
import { useTodo } from '../contexts/TodoContext';

function TodoForm() {

  const [todo, setTodo] = useState("")
  const {addTodo} = useTodo();


  const add = (e) => {
    e.preventDefault();
    if(!todo) return;

    addTodo({todo: todo, completed: false});
    setTodo("");
  }

  return (
    <form onSubmit={add} className="flex">
      <input
        type="text"
        placeholder="Write Todo..."
        className="w-full border border-black/10 rounded-l-lg px-3 outline-none duration-150 bg-white/20 py-1.5"
        value={todo} 
        onChange={(e) => setTodo(e.target.value)}
      />
      <button type="submit" className="rounded-r-lg px-3 py-1 bg-green-600 text-white shrink-0">
        Add
      </button>
    </form>
  );
}

export default TodoForm;

```

TodoItem.jsx
```jsx
import React from 'react'
import { useState } from 'react';
import { useTodo } from '../contexts/TodoContext';

function TodoItem({ todo }) {

  const [isTodoEditable, setIsTodoEditable] = useState(false);
  const [todoMsg, setTodoMsg] = useState(todo.todo);

  const { updateTodo, deleteTodo, toggleComplete } = useTodo();

  const editTodo = () => {
    updateTodo(todo.id, { ...todo, todo: todoMsg })
    setIsTodoEditable(false);
  }

  const toggleCompleted = () => {
    toggleComplete((todo.id));
  }


  return (
    <div
      className={`flex border border-black/10 rounded-lg px-3 py-1.5 gap-x-3 shadow-sm shadow-white/50 duration-300  text-black ${todo.completed ? "bg-[#c6e9a7]" : "bg-[#ccbed7]"
        }`}
    >
      <input
        type="checkbox"
        className="cursor-pointer"
        checked={todo.completed}
        onChange={toggleCompleted}
      />
      <input
        type="text"
        className={`border outline-none w-full bg-transparent rounded-lg ${isTodoEditable ? "border-black/10 px-2" : "border-transparent"
          } ${todo.completed ? "line-through" : ""}`}
        value={todoMsg}
        onChange={(e) => setTodoMsg(e.target.value)}
        readOnly={!isTodoEditable}
      />
      {/* Edit, Save Button */}
      <button
        className="inline-flex w-8 h-8 rounded-lg text-sm border border-black/10 justify-center items-center bg-gray-50 hover:bg-gray-100 shrink-0 disabled:opacity-50"
        onClick={() => {
          if (todo.completed) return;

          if (isTodoEditable) {
            editTodo();
          } else setIsTodoEditable((prev) => !prev);
        }}
        disabled={todo.completed}
      >
        {isTodoEditable ? "📁" : "✏️"}
      </button>
      {/* Delete Todo Button */}
      <button
        className="inline-flex w-8 h-8 rounded-lg text-sm border border-black/10 justify-center items-center bg-gray-50 hover:bg-gray-100 shrink-0"
        onClick={() => deleteTodo(todo.id)}
      >
        ❌
      </button>
    </div>
  );
}

export default TodoItem;

```

App.jsx
```jsx
import { useState, useEffect } from 'react'
import {TodoProvider} from './contexts'
import './App.css'
import TodoForm from './components/TodoForm'
import TodoItem from './components/TodoItem'

function App() {
  const [todos, setTodos] = useState([])

  const addTodo = (todo) => {
    setTodos((prev) => [{id: Date.now(), ...todo}, ...prev] )
  }

  const updateTodo = (id, todo) => {
    setTodos((prev) => prev.map((prevTodo) => (prevTodo.id === id ? todo : prevTodo )))

    
  }

  const deleteTodo = (id) => {
    setTodos((prev) => prev.filter((todo) => todo.id !== id))
  }

  const toggleComplete = (id) => {
    //console.log(id);
    setTodos((prev) => 
    prev.map((prevTodo) => 
      prevTodo.id === id ? { ...prevTodo, 
        completed: !prevTodo.completed } : prevTodo))
  }

  useEffect(() => {
    const todos = JSON.parse(localStorage.getItem("todos"))

    if (todos && todos.length > 0) {
      setTodos(todos)
    }
  }, [])

  useEffect(() => {
    localStorage.setItem("todos", JSON.stringify(todos))
  }, [todos])
  



  return (
    <TodoProvider value={{todos, addTodo, updateTodo, deleteTodo, toggleComplete}}>
      <div className="bg-[#172842] min-h-screen py-8">
                <div className="w-full max-w-2xl mx-auto shadow-md rounded-lg px-4 py-3 text-white">
                    <h1 className="text-2xl font-bold text-center mb-8 mt-2">Manage Your Todos</h1>
                    <div className="mb-4">
                        {/* Todo form goes here */} 
                        <TodoForm />
                    </div>
                    <div className="flex flex-wrap gap-y-3">
                        {/*Loop and Add TodoItem here */}
                        {todos.map((todo) => (
                          <div key={todo.id}
                          className='w-full'
                          >
                            <TodoItem todo={todo} />
                          </div>
                        ))}
                    </div>
                </div>
            </div>
    </TodoProvider>
  )
}

export default App
```

Loop multiple items in the todo array 

key to let react know where do do dom manipulations.

Advantage of using key with id instead of index: 
- when  an element from middle is deleted, for id based loop only that element would be removed, 
- but for index based loops the whole array would need to be restructured.
- id is preferred but not absolute necessity


# Redux
Class based components instead of function based components
Redux is  a JavaScript library that provides a central place to store, update and manage data across an entire web application.

Redux Toolkit : More abstraction, less configurations and can help you do stuff more easily

### Concepts in Redux and Redux Toolkit : 
**Store** :  Single store, similar to global variables which we can access from anywhere. Can have multiple stores for different functionalities like auth, product, etc.
**Reducers** : Writing and updating to specific stores is done through reducers
**Slice** : A slice in Redux Toolkit is a collection of redux reducer logic and actions for a single feature of an app.

useSelector() : When you have to select a value from a store
useDispatch() : When you have to dispatch a value from a store

Installing redux and redux toolkit into existing project : 
```
npm install @reduxjs/toolkit
```

```
npm install react-redux
```

todo : 
1. configureStore()
2. createReducer()
3. createAction()
4. createSlice()
5. then async setup stuff

the store file can be placed anywhere, we choose to place it in /src/app
file  : store.js

configureStore
configureStore() takes an object

store.js
```js

```

now create reducers
createReducer() 

/src/feature folder : 
multiple features in it

create slice in the folder
/src/feature/todo/todoSlice.js
feature : todo
slice: todoSlice

todoSlice.js
```js
import {createSlice, nanoid} from '@reduxjs/toolkit';

const initialState = {
    todos: [{id: 1, text: "Hello World"}]
}

export const todoSlice = createSlice({
    name: 'todo', // these properties exists in redux toolkit
    initialState,
    reducers: { // reducers take properties and function 
        addTodo: (state, action) => {
            const todo = {
                id: nanoid(),
                 text: action.payload
            }
            state.todos.push(todo)
        },
        removeTodo: (state, action) => {
            state.todos = state.todos.filter((todo) => {todo.id !== action.payload})
        },

    } 
})

export const {addTodo, removeTodo} = todoSlice.actions; // export all slice actions that will perform all actions

export default todoSlice.reducer; // export all reducers 
```

nanoid -> creates unique ids 
initialState -> How the store would look initially, can be an array or object as well
createSlice -> takes object and creates a slice.

each function in reducer will have two things : state and action from redux toolkit
state -> state of the data
action -> the value which would change the state e.g. id, username, etc.
payload can accept objects with multiple values, etc. 

In context API we used to give only function declaration but did not define the context in the same place. 
but in Redux Toolkit we will be writing definition in the slice itself.

Now accept the slice in the store.js
```js
import {configureStore} from '@reduxjs/toolkit';
import todoReducer from '../features/todo/todoSlice';

export const store = configureStore({
    reducer: todoReducer
})
```

