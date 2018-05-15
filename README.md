
## Overview

This pocket guides provide some best practices, recommended tools/libraries, and main concepts that usually missed when creating react native application.

For general React, ES6, Webpack, Redux learning material, please see our [React learning](https://github.com/mitrais-cdc-js/react-learning).

| Content |
|------|
| [React and Redux](#react-and-redux) |
| [React Native](#react-native) |
| [General](#general) |
| [Ecosystem](#ecosystem) |
| [Windows Issues](#windows-issues) |

---
### React and Redux

* **Container vs Component**

As describe [here](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0),
we can split React components into two basic types: smart components and dumb.

Dumb Component Characteristics:
- Describe how things _look_
- Have no app dependencies
- Receive only props, providing data and callbacks
- Rarely have own state, when they do, it’s just UI state
- Named anything that’s a UI noun
- Named **Component** by convention


Smart Component Characteristics:
- Describe how things _work_
- Provide no DOM markup, styles, or presentation
- Provide application data or do data fetching
- Can connect to the Redux store (usually using the React-Redux connect() method) and call Redux actions
- Know about Redux and state. 
- Live at or near the top of the component tree for a given view or route. 
- Named **Container** by convention


Note that calling a component _dumb_ doesn’t mean it's logic-less. It simply means that it has no connection to the Redux or state. It may still have complex logic, but that logic should be exclusively related to rendering and user-interaction. 


Example from this [gist](https://gist.github.com/chantastic/fc9e3853464dffdb1e3c)

```javascript
// CommentList.js
import React from "react";

const Commentlist = comments => (
  <ul>
    {comments.map(({ body, author }) =>
      <li>{body}-{author}</li>
    )}
  </ul>
)
```

```javascript
// CommentListContainer.js
import React from "react";
import CommentList from "./CommentList";

class CommentListContainer extends React.Component {
  constructor() {
    super();
    this.state = { comments: [] }
  }
  
  componentDidMount() {
    fetch("/my-comments.json")
      .then(res => res.json())
      .then(comments => this.setState({ comments }))
  }
  
  render() {
    return <CommentList comments={this.state.comments} />;
  }
}
```

* **Use PropsType & Default prop type**

As your app grows, you can catch a lot of bugs with typechecking. For some applications, you can use JavaScript extensions like Flow or TypeScript to typecheck your whole application. But even if you don’t use those, React has some built-in typechecking abilities as [official example below](https://reactjs.org/docs/typechecking-with-proptypes.html):


```javascript
import PropTypes from 'prop-types';

class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

// Specifies the default values for props:
Greeting.defaultProps = {
  name: 'Stranger'
};

Greeting.propTypes = {
  name: PropTypes.string
};
```

---

## React Native
* **Styling**

Watch out for styling.
IOS vs Android might have slight style differences. This is contrary to popular believe: we can develop faster because only need to create one single code. 


* **Performance**

Known performance issue on list view, especially Android, for older React Native version. So starting from React Native _0.43_, use _FlatList/SectionList/VirtualizedList_ instead of _ListView_.

Please see official docs for [other performance issue](https://facebook.github.io/react-native/docs/performance.html)

* **Emulator**

Alternative emulator for Android Studio Emulator: [Genymotion](https://www.genymotion.com/)

* **Example login and authentication using JWT**

Most apps require that a user authenticate in some way to have access to data associated with a user or other private content. Typically the flow will look like this:

1. The user opens the app.
2. The app loads some authentication state from persistent storage (for example, _AsyncStorage_).
The reason the token needs to be stored is that we need to be able to recover it every time we have to call a protected API route and later on to create the persistent user session.

3. When the state has loaded, the user is presented with either authentication screens or the main app, depending on whether valid authentication state was loaded.
4. When the user signs out, we clear the authentication state and send them back to authentication screens.

Please see tutorials for detail steps:

[Ref1](https://blog.theodo.fr/2017/03/how-to-create-an-authentication-system-and-a-persistent-user-session-with-react-native/)

[Ref2](https://medium.com/@rajaraodv/securing-react-redux-apps-with-jwt-tokens-fcfe81356ea0)

[Ref3](https://reactnavigation.org/docs/en/auth-flow.html)

[Ref4](https://blog.scalac.io/react-redux-jwt-authentication.html)

---
## General

* **Readable, Reusable, and Refactorable JavaScript**

See [Clean Code Javascript](https://github.com/ryanmcdermott/clean-code-javascript) for software engineering principles, from Robert C. Martin's book Clean Code, adapted for JavaScript. This is not a style guide. It's a guide to producing readable, reusable, and refactorable software in JavaScript.

* **Code Style and Linter**

_Airbnb_ has one of the most popular JavaScript style guides on the internet. It covers nearly every aspect of JavaScript as well. You can view Airbnb’s style guide on [GitHub](https://github.com/airbnb/javascript).

Always use linter for consistent code style and code. For example [ESLint](https://eslint.org/) have extensive configurable style and support many major IDEs.

* **Project, folder, and file structure**

There are numerous ways to organize a project and you can spend a lot of time reading about them and debating the pros and cons of each (none will be perfect). One of structure that work over time and stay simple to understand is the one from [Ignite](https://infinite.red/ignite) boilerplate

App

├── Components

│   └── Styles

├── Config

├── Containers

│   └── Styles

├── Fixtures

├── Images

├── Lib

├── Navigation

├── Redux

├── Services

├── Themes

└── Transforms

Please see further detail and explanation on [Ignite project structure](https://github.com/infinitered/ignite/blob/master/docs/quick-start/project-structure.md).


Even we also can remove _Styles_ folder and put the related style, test, and all its supporting files using below convention:

1. its main file (_name.component.js_)

2. its style file (_name.component.styles.js_)

3. its test file (_name.component.test.js_)

* **Know your debugging and dev tools**

Beside Chrome Dev Tools that already integrated inside Google Chrome browser, you can try below:

| Dev Tool | Description |
|------| ---- |
| [Redux DevTools Extension](http://extension.remotedev.io/) | For inspecting and time travel of Redux state |
| [Reactotron](https://github.com/infinitered/reactotron) | A desktop app for inspecting your React JS and React Native projects. macOS, Linux, and Windows. |
| [React Developer Tools](https://github.com/facebook/react-devtools) | React Developer Tools lets you inspect the React component hierarchy, including component props and state. |

* **Visual Studio Code Extension**

You can choose and use any IDE to develop React Native. But if you choose [Visual Studio Code](https://code.visualstudio.com/), here are some great plugins to improve your coding experience:

| Plugin | Description |
|------| ---- |
| [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) | Integrates ESLint into VS Code |
| [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) | Supercharge the Git capabilities built into Visual Studio Code — Visualize code authorship at a glance via Git blame annotations and code lens |
| [Bracket Pair Colorizer](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer) | This extension allows matching brackets to be identified with colours. |
| [Import Cost](https://marketplace.visualstudio.com/items?itemName=wix.vscode-import-cost) | Display import/require package size in the editor |
| [Path Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense) | Visual Studio Code plugin that autocompletes filenames |
| [npm Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.npm-intellisense) | Visual Studio Code plugin that autocompletes npm modules in import statements |
| [Instant Markdown](https://marketplace.visualstudio.com/items?itemName=dbankier.vscode-instant-markdown) | Simply, edit markdown documents in vscode and instantly preview it in your browser. |
| [VSCode LCOV](https://marketplace.visualstudio.com/items?itemName=alexdima.vscode-lcov) | Renders line and branch coverage |

---
## Ecosystem
One of the great things about React Native is the flexibility. This is great! But can also be daunting, especially to new developers. For example, please see this awesome articles about choosing [react native libraries](https://www.simform.com/react-native-ecosystem-backend-database-best-libraries/)

Other tools that worth to try:


* **Navigation**

Use [React navigation](https://reactnavigation.org/) for navigation.


* **Storybook**

Use [Storybook](https://storybook.js.org/) for interactive UI component dev & test: React, React Native, Vue, Angular.

Storybook is a development environment for UI components. It allows you to browse a component library, view the different states of each component, and interactively develop and test components.
Storybook runs outside of your app. This allows you to develop UI components in isolation, which can improve component reuse, testability, and development speed. You can build quickly without having to worry about application-specific dependencies.

[![Strybook image](
https://storybook.js.org/static/demo.f13d28a7.gif)](https://storybook.js.org/)



* **Unit Test**

Use [Jest](https://facebook.github.io/jest/) for unit test. 
Plus you can use to test other Javascript framework (React, Angular, Vue, etc)

* **Form Management**

Use [Redux form](https://redux-form.com) to manage your form state in Redux.

Or try the new alternative [Formik](https://github.com/jaredpalmer/formik).



* **React Native Playground**

Codepen and JSFiddle are awesome, because if we want to try out something small quickly, we don’t have to setup a new project which can take quite some time. 

The similar thing exist in React Native. You can use [Snack Expo](https://snack.expo.io/) to make sharing demos super-easy, both for the creators and people who want to try it out. You can just tweak an existing demo and see changes in real time, which is great.

 Just head over to _https://snack.expo.io_ and start typing. You can either preview the component on the emulator inside the browser, or open it directly on your device with the Expo app.


* **Functional Javascript**

Use [Ramda](https://github.com/ramda/ramda). It is like _Lodash_/_Underscore_, but in functional programming concept in mind.

Then next level is use [Ramda Adjunct](https://github.com/char0n/ramda-adjunct) to eliminates create own utils and recipes by composing Ramda's functions and creating more complex aggregate function.


---
## Windows Issues

* **Line ending issue**

To prevent line ending issue between Windows and Mac, when `Configuring the line ending convention`,  choose `Checkout as-is, commit as-is`
OR run these commands after installation

```
git config --global core.autocrlf false
git config --global core.eof lf
```


* **Error running adb**

When starting React Native tutorial in https://facebook.github.io/react-native/docs/getting-started.html and found:
```
Error running adb: This computer is not authrorized to debug the device. Please follow the instructions here to enable USB debugging.
```

Then use
```
npm install -g react-native-cli
```
instead of
```
npm install -g create-react-native-app
```



### Contribution
Your contributions and suggestions are heartily♡ welcome. (✿◠‿◠)

---
### License
[![CC0](http://i.creativecommons.org/p/zero/1.0/88x31.png)](http://creativecommons.org/publicdomain/zero/1.0/)
