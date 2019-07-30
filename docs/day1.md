# Day 1

# Project structure
* ` package.json ` is a file that contains: 
  * your project configurations like its version.

  * **dependencies** which are divided into 2 categories: **production** and **dev** dependencies.
  
  * **scripts** that can be run through ` npm [script_name] `.

* ` .bablerc ` is a file that contains the configurations for ` Babel ` which is the popular ES6 transpiler.

* ` .flowconfig ` this file contains configurations for ` Flow ` which is a static type-checking system developed at **Facebook**  and is usually used with ` React `. It is disabled by default. 
  * A **type-checking system** is a system that enables IDEs to capture errors related to types before you run the code. This helps building robust software and it is highly recommended for large-scale aoolications. The most popular type-checking systems are ` TypeScript ` and ` Flow `. ` TypeScript ` is a programming language developed by **Microsoft** but based on its purpose, people consider it a type-checking system.
  
  * On starting a new project you see at the top of ` App.js ` file:
  ```js
  /**
   * Sample React Native App
   * https://github.com/facebook/react-native
   *
   * @format
   * @flow
   */
  ```
  This ` @flow ` enables ` flow ` to check this file. If you need to tell ` flow ` that this file should be checked using ` flow ` you have to add ` @flow ` commented on the top of that file. And you see something like this:
  ```js
  type Props = {};
  export default class App extends Component<Props> {
    // ...
  ```
  This is a way to define prop types for ` flow ` to check. Also if you want to add type checking to a function arguments, you can do that:
  ```js
  sum( n1: number, n2: number ): number {
    return n1 + n2;
  }
  ```

* ` index.js ` is the entry-point of your project which means that it is the first file executed when your **JavaScript bundle** starts running. In this file the application is registered through ` AppRegistery.registerComponent ` method and then it accepts a name to your app and the initial component to start from. 

* ` App.js ` file contains the initial component registered in ` index.js `.

## What is ` React Native `?
* ` React Native ` is not just limited to building mobile apps for **Android**, **Ios**, and **Windows**. It's now used to build for a variety of platforms including **Desktop** apps. 

* ` React Native ` was built as **learn once, write everywhere** which means you learn one framework and use it to build for a variety of platforms. It's not as ` write once, run everywhere ` whcih means you write one thing and that runs on a veriety of platforms like ` Ionic ` and ` Cordova `. Refer to [this](https://github.com/hossamnasser938/React-Native-The-Practical-Guide-Course-Documentation/blob/1bccda3ac37f638326ec85654c766f5ac0b3d400/documentation%20files/01_getting_started.md#what-is-react-native) for more info.

## Why ` React Native `?
* The enterprise and the community started picking ` React Native ` for a variety of reasons. The most outstanding one is the **cost or speed of development**. In native development if you changed some UI like a button color and you want to test it, you have to recompile the whole project. This takes from 30 seconds to 1 minute for small projects but for large-scale projects such as **Facebook**, it may take 15 minutes and this is why **Facebook** built ` React Native ` for the first place.

* Also with ` React Native ` you end up with one team building for a variety of platforms. This decreases the cost of development massively.

* The development environment of ` React Native ` and the high **reusability** you gain is what makes it super fast in development.

* One of the reasons that pushes companies to adopt ` React Native ` is the availability of **JavaScript** developers over native developers. This is because you may learn **JavaScript** for front-end, back-end, desktop development. On the other hand, if you decided to learn **ObjectiveC** for example, ofcourse you learn it for **Ios** only.

* Another reason is ` Over-the-air Updates ` which is the ability to refresh the **JavaScript** bundle while the app is deployed without the need to deploy a new version. This helps a lot when you ship your app and then you catch a bug. One of the tools used to do that is ` Code Push `.

## How does ` React Native ` work?
* A normal ` React Native ` app has 3 threads and a bridge:
  * **Native thread** which does all native things including displaying native UI components.
  
  * **JavaScript thread** which runs your **JavaScript bundle** which has your app business logic.
  
  * **Layout thread** which handles layout defined using **flexbox**.
  
  * a **bridge** that handles communication between the **JavaScript** thread and the **native** thread.

* The **bridge** facilitates two-way communication between the **JavaScript** thread and the **UI** thread. The communication is **asynchronous**. The **bridge** maintains 2 messages queue on each side.

* The **JavaScript** thread runs in an environment known as the **JavaScript core** which is an engine similar to the **V8** used by chrome.

## ` React Native ` Api
* The ` React Native ` api is divided into 2 categories:
  * **Primitive UI components** which are components exposed by ` React Native ` such as ` View `, ` Text `, ` Button `. These primitive components ` React Native ` knows well how to convert them into native UI components.
  
  * **Native modules** which are modules exposed by ` React Native ` to provide native features like the ` Dimensions ` module which provides features related to the device's dimensions.

* ` React Native ` uses **flexbox** for its layout. Actually **flexbox** is a layout module for the web so for **flexbox** to be used on a different platform it needs an engine to sit inbetween. **Yoga** is this layout engine that enables flexbox to be adopted by multiple frameworks. ` React Native ` is one of those frameworks that uses **Yoga**.
 
## Lifecycle methods
* In a later version of ` React ` some changes exist on lifecycle methods:
  * ` componentWillMount ` is removed. This is because any change to state occurs in this method will not cause **rerender** as this method is called before the initial ` render `. This makes ` componentWillMount ` with no benefit since any thing can be placed in either the ` constructor ` or ` componentDidUpdate `.
  
  * ` componentWillReceiveProps ` is replaced with the ` static ` method ` getDerivedStateFromProps ` which accepts ` props ` and ` state ` and returns an object to update the ` state ` due to any change in ` props `. Recall that this is a ` static ` method so you do not have any reference to the component object. This method is get called before the initial ` render ` as well as on every update to ` props ` or ` state `. 
  
  * A new method named ` getSnapshotBeforeUpdate ` is added. This method sets between ` render ` and ` componentDidUpdate `. Refer to this [article](https://reactjs.org/docs/react-component.html) or this [diagram](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/) for more info.  

* The component lifecycle can be divided into threee scenarios:
  1. **Mounting**.
    1. ` constructor( props ) `.
    2. ` static getDerivedStateFromProps( props, state ) `.
    3. ` render() `.
    4. ` componentDidMount() `.
  
  2. **Updating**.
    1. ` static getDerivedStateFromProps( props, state ) `.
    2. ` shouldComponentUpdate( prevProps, prevState ) `. Only if this method returned ` true ` the subsequent methods will get called.
    3. ` render() `
    4. ` getSnapshotBeforeUpdate( prevProps, prevState ) `. The returned object from this method will be passed to ` componentDidUpdate ` method.
    5. ` componentDidUpdate( prevProps, prevState, snapshot ) `.
  
  3. **Unmounting**.
    1. ` componentWillUnmount() `. 

## Side notes
* The ` Text ` component is **clickable**. You can set the ` onPress ` props with no need to wrap it with a ` Touchable ` component.

* **Metro** is a **JavaScript Bundler** or the **packager**. It takes a set of options and the **JavaScript** source code of your project and **bundle** it into a single file. You can run the **packager** alone by hitting ` npm start `. However, if you hit ` react-native run-android ` or ` react-native run-ios `, this script first runs the packager before trying to run the app on the emulator or device connected.  

* Using Visual studio code you can hit ` code . ` to open vscode with the curren directory opened.

* In later versions of ` npm `, you do not need anymore to add ` --save ` to hint that the package is a **production** one. If you did not add any thing, ` npm ` knows that it is a **production** one. However, if you want to install a **dev** dependency, you need to add ` --save-dev `. 

* If you need to publish your project through ` npm ` to be used by others as a package, you can do that through running ` npm publish `. The ` name ` and ` version ` at ` package.json ` will be used by ` npm ` to uniquely identify your package. 

## Coding
* ` Platform ` api provided from ` React Native ` contains a method named ` select `. ` select ` accepts an object with keys the possible platforms and values: the value you want for a specific platform. ` select ` will return the value assigned to the key that represents the platform running. For example:
```js
import { Platform } from 'react-native';

const plat = Platform.select( {
  android: 'and',
  ios: 'ios',
  default: 'not and not ios'
} );
```
` plat ` variable will hold:
  * ` and ` if the application is running on **android**.
  * ` ios ` if the application is running on **ios**.
  * ` not and not ios ` if the application is running on a different **os** such as **apple tv** or **windows**.

* You can also use ` Platform.select ` with **spread operator**. We usually use that syntax with styling:
```js
StyleSheet.create( {
  text: {
    color: 'black',
    ...Platform.select( {
      ios: {
        fontSize: 18,
        fontWeight: 'bold'
      },
      android: {
        fontSize: 16,
        fontWeight: 'normal'
      }
    } )
  }
} );
```

* The **JSX** you write is identified by ` React `. So for example if your ` render ` method looks like this:
```js
render() {
  return(
    <View>
      <Text>Hello world</Text>
    </View>
  );
}
```
This is exactly equal to this:
```js
render() {
  return(
    <View>
      {
        React.createElement( Text, null, 'Hello world' )
      }
    </View>
  );
}
```
` React.createElement ` method accepts 3 arguments: the component, the props, and the children. So whenever you want to use **JSX** you must import ` React ` 
```js
import React from 'react';
```
Otherwise you will get an error.

* We can initialize ` state ` in 2 different ways:
  * Through the ` constructor `.
  ```js
  class Component extends React.Component {
    constructor( props ) {
      super( props );

      this.state = {
        greeting: 'hello'
      };
    }

    // rest of methods
  } 
  ```
  * Through **property initializer**.
  ```js
  class Component extends React.Component {
    state = {
      greeting: 'hello'
    };

    // rest of methods
  }
  ```
For some reason the instructor recommended using **property initializer** unless we need to do something other that initializing ` state ` in the ` constructor `.

## Expressions
* **Boilerplate** means a standardized text that is used over and over. If there is a code snippet that you use in each project, so this is a boilerplate code.

* **bread and butter** is something you see all the time. 
