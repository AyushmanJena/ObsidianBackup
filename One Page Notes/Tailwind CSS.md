By Hitesh Choudhary on College Wallah
https://youtu.be/_9mTJ84uL1Q?si=nIxi4NOoLw-NCFzn


Tailwind -> Utility Framework with classes like flex, pt-4, text-center and rotate-90 that can be composed to build any design, directly in your markup

Using Play CDN to try Tailwind works only for development purpose, and is not the best choice for production.

Installation : (Tailwind CLI)
```
// make sure node is installed

npx tailwindcss init 

```
creates a file tailwind.config.js

you need to export in the config file : 
```js
 /** @type {import('tailwindcss').Config}*/
 module.exports = {
	 content: ["./dist/index.html"],
	 theme {
		 extend: {},
	 },
	 plugins: [],
 }
```

`content: ["./dist/*.html"],` to apply tailwind on all html files


create input.css : 
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

compile the tailwind css to get a output css file through a command : 
```
npx tailwindcss -i input_file -o output_file

ex : 
npx tailwindcss -i ./src/input.css -o ./dist/style.css
```

Now link the newly generated css file to the html file 
```html
<head>
	<link rel="stylesheet" href="style.css">
</head>
```

with every class you write in your html, it will be generated in the style.css 
for basic html css, you will have to run the compile command with every change to generate a new output file

to avoid re compiling every time you can watch the file for changes 
```
npx tailwindcss -i ./src/input.css -o ./dist/style.css --watch
```

## Tailwind classes

text-white

text-4xk -> size

m-6 -> margin 6 
- it will display when hovered the values like 24px / 1.5 rem etc.


TEXT SIZE : 
`text-sm`, `text-base`, `text-lg`, `text-xl`, `text-2xl`, etc.

`grid` : grid class 
`place-content-center`: 

viewport property : 
`h-screen` : to make an element span the entire height of the viewport

text color : 
`text-yellow-400` 
- The number 500 represents the color shade or intensity on a scale from 50 to 950
- 50 : very light close to white
- 950 : very dark close to black
- 500 is considered middle shade


#### Building a card 
```html
<body>

    <div class="p-6 max-w-sm mx-auto bg-white rounded-xl shadow-lg flex items-center space">
        <div>
            <div>
                <img class="h-12 2-12" src="address" alt="image">
            </div>
            <div>
                <div class="text-2xl font-medium text-black">
                    Tailwind CSS
                    <p>By Vulcan Bhai</p>
                </div>
            </div>
        </div>
    </div>

</body>
```

`m-2` : margin from all sides
`max-w-sm` : max width small
`mx-auto` : margin x axis auto
`mt-2` : margin top 2 
`my-2` :  margin y axis

`p-2` : padding from all sides
`px-2`
`py-2`
`pt-2`, etc.


rounded border : 
`rounded`, `rounded-md`, `rounded-lg`, `rounded-full`, etc.

shadow : 
`shadow-sm`, `shadow-md`, `shadow`, `shadow-xl`, etc.

`flex` : introduce flexbox 
`flex-row`, `flex-col` : flex direction
`flex-wrap` : flex wrap
`items-center` : aligns child items along cross axis
`justify-between` : justify content between 
similarly we have `justify-start`, `justify-center`,` justify-around`, etc.

`space-x-4` : auto calculates margin left and margin right (bigger number more margin) horizontally
`space-y-*` : applies vertical spacing top and bottom

`h-12`, `w-10` : setting height and width 
`w-full` : full width

text size : 
`text-xl`, `text-2xl`, etc

font thickness : 
`font-medium`, `font-thin`, `font-bold`, `font-light` etc.

text color : 
`text-black`, `text-red`

other font styling classes like : `uppercase`, `lowercase`, `capitalize`, `italic`, `underline`, `overline`, `line-through`,

Managing space between letters using tracking : 
`tracking-tight`, `tracking-normal`, `tracking-wide`, etc.

background color : 
`bg-sky-500`

Change Cursor : 
`cursor-pointer` and other such values

Border :
`border-white` : border color
`border-2` : set border width ; must do it for it to be visible

#### Variables
When the predefined classes are not enough and you want to use custom values
Use square brackets 
`p-[2px] px-[14px]`

### States
States like Hover, Focus, and other states

`hover: ` : Hover state
You need to use the hover tag before every class you want to be available for the state

e.g. : ` text-white bg-blue-500 hover:bg-white hover:text-black`
`dark:bg-red-600` dark: works based on system or browser settings

```html
<button class="bg-sky-500 text-center mt-2 text-white p-2 rounded-xl hover:bg-white hover:text-black dark:bg-red-600">
        Buy Now
 </button>
```

### Responsive Design (Mobile First Approach)
By default tailwind uses a mobile first breakpoint system.

There are five breakpoints by default, inspired by common device resolutions : 
```
sm
md
lg
xl
2xl
```

you can use them as pseudo classes 

```html
<div>
    <p class="text-white md:text-green-500 sm:text-red-600">Hello</p>
</div>
```

since mobile first approach what it means : 
sm:text-red-600 -> for small devices and anything above it set text color red
md:text-green-500 -> for medium devices and anything above it set text color red

so for our code -> for smallest devices it will stay white
if we keep increasing the window size, when we hit small we will have color red till medium
still increasing, when we hit medium we get green color and till end


### Card Design with responsive design

```html
<div class="mt-5">
        <div class="max-w-sm mx-auto bg-white rounded-xl overflow-hidden md:max-w-2xl">
            <div class="md:flex">
                <div>
                    <img class="h-48 w-full object-cover md:h-full md:w-48" src="address" alt="image">
                </div>
                <div class="p-8 ">
                    <div class="uppercase tracking-wider text-sm text-indigo-500 font-semibold">An awesome card</div>
                    <a class="block mt-1 text-lg font-medium text-black hover:text-blue-700"href="#">Tailwind css is amazing once you understand the <span class="bg-yellow-400 p-[2px]">power</span> of it.</a>
                    <p class="mt-2 text-slate">xyz text</p>
                </div>
            </div>
        </div>
    </div>
```

 