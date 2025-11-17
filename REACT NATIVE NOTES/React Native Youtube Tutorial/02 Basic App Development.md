Basic execution starts from index.js 
```js
AppRegistry.registerComponent(appName, () => App);
```
it calls the App() in App.tsx file

```tsx
/**  
 * Sample React Native App * https://github.com/facebook/react-native * * @format  
 */  
  
import { NewAppScreen } from '@react-native/new-app-screen';  
import { StatusBar, StyleSheet, useColorScheme, View } from 'react-native';  
  
function App() {  
  const isDarkMode = useColorScheme() === 'dark';  
  
  return (  
    <View style={styles.container}>  
      <StatusBar barStyle={isDarkMode ? 'light-content' : 'dark-content'} />  
      <NewAppScreen templateFileName="App.tsx" />  
    </View>  );  
}  
  
const styles = StyleSheet.create({  
  container: {  
    flex: 1,  
  },  
});  
  
export default App;
```

Note : React Native also uses a similar styling as used in the web
Except in React native since it is used for mobile development, the default styling is from top to bottom.

### Building a simple hello world : 
```tsx
import React from 'react';

import {
  View,
  Text,
  SafeAreaView
} from 'react-native'

function App(){
  return (
    <SafeAreaView>
      <View>
        <Text>Hello World !!!</Text>
      </View>
    </SafeAreaView>
  )
}

export default App;
```

Imported from react-native
View -> similar to div in the web
Text  -> text
SafeAreaView -> to avoid notches, cutouts, etc.

- SafeAreaView wraps up the entire content

1) In react-native styles for example flex are from top to bottom by default. 
2) 2 ) Views are similar to div. 
3) SafeAreaView: It is used to avoid the notch. Wrap the entire content in it. 
4) In jsx, a starting tag needs to have an end tag.


CONTINUE FROM 
07 : # Styling React Native Components: The Fundamentals of Stylesheet

https://www.youtube.com/playlist?list=PLRAV69dS1uWSjBBJ-egNNOd4mdblt1P4c

