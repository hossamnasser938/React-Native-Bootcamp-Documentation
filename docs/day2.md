# Day 2

## FlexBox
* **FlexBox**(Flexible Box) is a module of a set of properties used to layout UI elements. To **layout** elments means to distribute those elements in the available space. This involves whether to distribute elements from left to right, right to left, top to bottom, bottom to top, fill the full space, leave some space inbetween, and all of that stuff.

* It was built originally for the web and then ` React Native ` adopted it. However, in the web it is not implemented by default. To implement it you must explicitly use:
```css
display: flex
```

* In ` React Native `, **FlexBox** is implemented by default and all UI elements inherits its properties.

* Refer to [this](https://github.com/hossamnasser938/React-Native-The-Practical-Guide-Course-Documentation/blob/1bccda3ac37f638326ec85654c766f5ac0b3d400/documentation%20files/02_diving_into_the_basics.md#styling-_-understanding-the-basics) and [this](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) to understand how **FlexBox** works. 


## Coding
* ` setState ` not just updates ` state `. It also takes care of rerendering when necessary. For that reason, you can update ` state ` like this:
```js
this.state.greeting = 'new greeting';
```
Although this will update ` state `, it will not cause **rerendering** so the update will have no effect on the UI.

* A component name **must** start with a **capital letter**.

* ` FlatList ` provides 2 on-the-shelf ` props ` to control **refreshing** functionality( the user pulls the list down to refresh ):
  * ` onRefresh ` this is a callback to be called when the user pulls the list down. Make sure to set a refreshing flag.
  * ` refreshing ` this is a flag that should be set to ` true ` by you in ` onRefresh ` callback. This lets ` Flatlist ` shows an ` ActivityIndicator ` for you while refreshing. Note that you also have to clear it when data is fetched.

```js
import React from 'react';
import { Text, View, FlatList } from 'react-native';

export default class App extends React.Component {
  state = { 
    refreshing: false,
    data: [{name: 'hos'}, {name: 'somaa'}, {name: 'aymon'}] 
  };
  
  onRefresh = () => { 
    this.setState( 
      {refreshing: true}, 
      () => { setTimeout(
        () => this.setState( prevState => ({ 
            refreshing: false,
            data: [...prevState.data, {name: 'mossib'}] 
          }) ) 
        , 3000 )
      }
    ) 
  } 

  render() {
    return (
      <View style={styles.container}>
        <FlatList 
          data = { this.state.data }
          renderItem = { ({item}) => <Text>{ item.name}</Text> }
          keyExtractor = { (item) => item.name }
          onRefresh = { this.onRefresh }
          refreshing = { this.state.refreshing }
        />
      </View>
    );
  }
}
```


## Tips
* We have control over The annoying yellow box of warnings appear in development:
  * We can be ignored through:
  ```js
  console.disableYellowBox = true;
  ```

  * Also specific warnings can be ignored through:
  ```js
  import { YelloBox } from 'react-native';

  YellowBox.ignoreWarnings( ['Remote ...'] );
  ```
  This code can be placed in ` index.js `. Refer to this official [docs](https://facebook.github.io/react-native/docs/debugging.html).

* *JavaScript** ` return ` keyword can be used with as well as without parentethesis. Both of this works:
```js
function sayHello() {
  return 'hello';
}
```

```js
function sayHello() {
  return ( 'hello' );
}
```
For **convention** we usually use ` return ` without parentethesis when we ` return ` one line. If we ` return ` a multiple-line JSX, for example, we may prefer to use paretethesis. Refer to this stackoverflow [question](https://stackoverflow.com/questions/20824558/why-use-parentheses-when-returning-in-javascript).

* ` render ` is just the process of transforming ` React ` components into something undertandable and can be displayed by the UI engine of whatever platform ` React ` runs over like the **DOM** for the web. ` React ` is separated in 2 libraries:
  * ` React ` which is responsible for creating, manipulating, and managing ` React ` elements(components).
  * ` ReactDOM ` which is responsible for communicating with the DOM to ` render ` these ` React ` elements.  
  This separation of concerns separated the logic related to **displaying** so that ` React ` elements can be displayed on a variety of platforms such as ` react-native ` for mobile apps and ` react-vr ` for virtual reality applications. Refer to this Quora [question](https://www.quora.com/What-does-the-term-render-mean-in-ReactJS-Like-render-a-component). 

* For demo purposes, we can use **Start Wars** [API](https://swapi.co/).