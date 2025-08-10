# React Native 
- You write you code in modular javascript, and the same code is being transferred into android and ios. 
- It automatically takes care of it.
- How? Later.

#### Requirements : 
- IDE
- Java(openjdk)
- Android Studio (specifically for adb and other android tools)

### Setting Up React Native Project
`npx @react-native-community/cli init app-name`

now to run the app 
go into the project and run the command
`npx react-native run-android`

currently working ndk version : 29.0.13599879

### React.js
- React.js is independent from React Native 
- React.js is a js library for building user interfaces.
- Typically used for web development
- Note : react-dom add web support for react js
- React itself is platform agnostic 
### React Native
- Collection of special React components
- Components are compiled to native UI elements
- Native platform APIs exposed to Javascript e.g. camera, etc.
- React native connects react to the specific platfoms


### File Structure 

`_tests_` -> for writing test cases 
`.bundle` -> config file 
`android` and `ios` folders :
90% time you won't need these folders, they just exist so that the config from react native is linked to the android app and ios app
10% of the time you might need build.gradle (or Podfile) for the apk build, etc.
 `node_modules` -> dependencies

`app.json` -> major config file and specifies the name of the file
`app.tsx` -> the root file and renders everything on the screen
`babel.config.js` -> bundler that combines all the files into one runnable file (uses metro in react native)
`index.js` -> the first file that is run 
`metro.config.ja` -> metro configuration (bundler)
`package-lock.json` -> makes sure the same versions are installed with npm
`package.json` -> specifies the dependencies and versions
`tsconfig.json` -> ts config file

`.gitignore` 
`.prettierrc.js` -> dev specifications, nothing to do with code
`.ruby-version` -> ios use case
`.watchmanconfig` -> tool that reupdates your application, (similar to devtools)
`Gemfile` -> for ios development 


