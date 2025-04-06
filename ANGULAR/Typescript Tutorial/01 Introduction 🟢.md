- Typescript does not add new features to javascript but makes it uniform. Makes it more structured. So that you make less errors.
- At the end all code is executed in javascript.
- It's all about Type Safety.
Ex : 
In js : `2 + "3" gives ''23'` which should not be the case

### What TypeScript is
- TS is a superset of JS
- TypeScript has one job : static checking.
- Data types, object does not exist, etc.
- Does not make code lesser compared to JS
- Typescript is a development tool, your project still runs in JS. TS is not a standalone language

in TS 
```ts
let user  {name : "Ayush", age : 10};
let email = user.email;
```
- This code gives an error in TS but not in JS


Running ts code :
`tsc intro.ts` 
this converts the ts code to js code (intro.js)