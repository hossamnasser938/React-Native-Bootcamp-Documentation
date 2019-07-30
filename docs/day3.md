# Day 3


## Native APIs and Components

### Camera Roll
* ` CameraRoll ` is a native api provided by ` react native ` to access user's images and videos. This requires to add a native library on **IOS** only. On **Android** it is available by default.
```js
import { CameraRoll } from 'react-native';
```

* ` CameraRoll ` exposes 2 methods:
  * ` saveToCameraRoll `
  saves an image in the gallery.
  

  * ` getPhotos `
  reads the photos or videos of the user.
  ```js
  CameraRoll.getPhotos( { first: 10 } )
    .then( res => { console.log( res.edges ) } )
    .catch( err => { console.error( err ) } );
  ```
  As we see ` getPhotos ` accepts a configuration object and returns a ` Promise `. The most important attribute of the configuration object is ` first ` which defines how many photos you wanna get. ` getPhotos ` returns a ` Promise ` that resolves with an object holding some attributes one of which is ` edges ` which is an array of photos. If we wanna display these images we can do that by accessing the ` uri ` attribute shipped with each image object getting back in ` edges ` array.
  ```js
  function renderImages = edges => {
    return(
      <FlatList 
        data = { edges }
        renderItem = { ({item}) => (
          <Image 
            uri = { item.node.image.uri }
          />
        ) }
      />
    );
  };
  ```
  This ` item.node.image.uri ` depends on the structure of objects within ` edges ` array. 

### Navigation
* We know that ` React Native ` does not provide out-of-the-box navigation solutions and the most outstanding navigation solutions in ` React Native ` are:
  * ` react-navigation ` which is a JavaScript-based implementation maintained by **the community**, developers from **Facebook** as well as developers from **Expo**.
  * ` react-native-navigation ` which is a native implementation maintained by **Wix**.

* Here we will have a look at ` react-navigation `.

#### React Navigation
* To install ` react-navigation `, we need:
  1. ` npm install --save react-navigation `.
  2. install and link the native library ` react-native-gesture-handler `.
      1. ` npm install --save react-native-gesture-handler `.
      2. ` react-native link react-native-gesture-handler ` or linking it manually.

* ` react-navigation ` provides 4 main types of navigators:
  * **stack** navigator.
  * **tab** navigator.
  * **drawer** navigator.
  * **switch** navigator.

* We're familiar with all these **avigators** except for the **switch** navigator. The **switch** navigator is one that enables you to switch between multiple navigators. This is extremely helpful when you have different experiences in your application like when you have an experience for authenticated users and another completely different experience for non-authenticated users. 

* We simply define a stack navigator using the ` createStackNavigator ` function exposed by ` react-navigation `. ` createStackNavigator ` accepts an object with keys each of which unique to a screen that we can navigate to through the navigator and values that are configuration objects for screens with at least an attribute with the name ` screen ` that should be assigned a ` Component `. Let's see an example.
```js
import React, {Component} from 'react';
import {Text, View} from 'react-native';
import { createAppContainer, createStackNavigator } from 'react-navigation';

class App extends Component {
  static navigationOptions = {
    title: 'App Screen Title' 
  };
  
  render() {
    return (
      <View style={ {flex: 1} }>
        <Text>App Screen</Text>
        <Text 
          onPress = { () => { this.props.navigation.navigate( 'Screen2' ) } }>
          navigate to screen 2
        </Text>
      </View>
    );
  }
}

const Screen2 = props => (
  <View style = { {flex: 1} }> 
    <Text>Screen2</Text> 
    <Text 
      onPress = { () => { props.navigation.goBack() } }>
      Go back to App
    </Text>  
  </View>
);

const nav = createStackNavigator( {
  App: App,
  Screen2: Screen2
} );

export default createAppContainer( nav );
```
Notice that 
  * Each screen within a navigator has an ` static ` attribute named ` navigationOptions ` that can be used to style the nav bar and maybe other things. 
  * In ` react-navigation ` v3 we have to define an app container wrapping our navigator through the method ` createAppContainer `.
  * each screen within the navigator is passed a ` navigation ` props that has a variety of methods that can help in navigation. The most outstanding ones are:
    * ` navigate ` which accepts a key of a screen to navigate to.
    * ` goBack ` which pops the current screen and go back one screen.

* To create a tab navigator, we simply replace ` createStackNavigator ` with the method ` createBottomTabNavigator ` or ` createTopTabNavigator ` and leave everything as it is. 

* To create a drawer navigator, we simply use the method ` createDrawerNavigator ` with the same stuff passed. However, we need to provide a way in each screen opened by the drawer to open the drawer. So, explicitly, we have to replicate a ` Button ` or a clickable ` Text ` in each screen passed to ` createDrawerNavigator ` and bind this clickable item with ` props.navigation.openDrawer ` method.

* To create a switch navigator we simply use ` createSwitchNavigator ` method which accepts an object of key-value pairs representing the available navigators you wanna switch over exactly the same as other navigators and we can navigate between those navigators using ` props.navigation.navigate ` method passing the key of the navigator we wanna navigate to. The difference here is that those navigators are independent meaning that once you navigate to one you do not have the previous in the history where you can back to because it is designed to provide different experiences as we said.

### Authetication
* To provide authentication service, we can use out-of-the-box solution such as **Firebase** or **AWS**(Amazon web service) who are cloud services providers which provide many services including authentication or build our own. 
* We're famliar with **Firebase** and fortunately the instructor will go with **AWS**.
* To use **AWS**, we need first to install ` awsmobile-cli `[certainly after creating a user]:
  1. ` npm install -g awsmobile-cli `.
  2. configure it: ` awsmobile configure `.
  3. Initialize a new aws project: ` awsmobile init ` and fill some info or simply hit ` awsmobile - y ` to go with the defaults.
  4. Enable signin service ` awsmobile user-signin enable ` and push that ` awsmobile push `.
  5. We need to link a native package named ` amazon-cognito-identity-js ` through ` react-native link amazon-cognito-identity-js `.
` aws-amplify ` is the **SDK** we're gonna use to access cloud services provided by **Amazon**. ` user-signin ` is just a service provided by ` aws-amplify ` **SDK**. Now we're ready to go.

* First we need to let our react-native app know that we're gonna use ` awsmobile ` so in ` index.js `:
```js
import {AppRegistry} from 'react-native';
import App from './App';
import {name as appName} from './app.json';

/* congigure aws */
import Amplify from 'aws-amplify'
import config from './aws-exports'
Amplify.configure(config)
/* very easy */

AppRegistry.registerComponent(appName, () => App);
```

* After that we can use the extremely simple solution provided by ` aws-amplify ` for authentication. ` aws-amplify ` exposes a **HOC**(Higher order component) named ` withAuthenticator ` that wraps any component and it will make sure that the user is authenticated before rendering the wrapped component and if the user is not authenticated it renders a screen for authentication and once the user becomes authenticated through signin or signup it will render the wrapped component.
```js
import React from 'react';
import { View, Text } from 'react-native';
import { withAuthenticator } from 'aws-amplify-react-native';

const App = props => <View><Text>You're authenticated</Text></View> ;

export default withAuthenicator(App)
```

* ` with~uthenticator ` component is shipped with its own UI. If we want to provide a custom UI for our app authentication screen, we can use ` Auth ` API provided by ` aws-amplify ` which has methods for signin and signup and all of that stuff that we can use with our own UI.
```js
import { Auth } from 'aws-amplify-react-native';
```


## Tips
* ` margin ` and ` padding ` properties.
  * ` marginVertical ` is a style attribute equivalent to setting both ` marginTop ` and ` marginBottom `.

  * ` marginHorizontal ` is also equivalent to setting both ` marginLeft ` and ` marginRight `.

  * The same applies to ` paddingVertical ` and ` paddingHorizontal `.

* ` flexWrap ` style attribute determines whether to wrap around when the content dows not fit the available space. This can be used to make a **grid** by specifying:
```js
{
  flexDirection: 'row',
  flexWrap: 'wrap'
}
```
It has only 2 values:
  * ` wrap ` .
  * ` nowrap ` which is certainly the default. 

* Attaching a ` static ` attribute to a function-based component is a bit different from attaching it to a class-based one.
  * In a class-based component we can either preced the attribute with the class with ` static ` keyword or attach the attribute to the class name outside the class so these 2 approaches are valid:
  ```js
  class ClassBasedComponent extends Component {
    static staticAttribute = {};
    
    render() {
      return <View />;
    }
  }
  ```

  ```js
  class ClassBasedComponent extends Component {    
    render() {
      return <View />;
    }
  }

  ClassBasedComponent.staticAttribute = {};
  ```

  * In a function-based component we have the previous second approach only available because simply it is not a class so it knows nothing about ` static ` keyword.
  ```js
  const FunctionBasedComponent = props => (<View />);

  FunctionBasedComponent.staticAttribute = {};
  ```