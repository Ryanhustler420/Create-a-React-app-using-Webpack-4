# Create a React app from scratch using Webpack 4


> aim is to set up a simple configuration which can then be grown upon.

our planning to do is to make a search functionality with some fake JSON data (or real),
We'll go through the set up of our project


``Make a new project and cd into it:``

```

mkdir react_search
cd react_search

```

``Create a package.json file:``

```npm init```

> If we want to skip all the questions, add the -y flag:

```npm init -y```

We need to install **webpack** as a dev dependency and **webpack-cli** so that we can use webpack in the command line:

```npm i webpack webpack-cli -D```

- i: install
- D: — save-dev

> Create a src folder with index.js and place the following code as an example:

```console.log("hello");```

> Now add the following scripts to our package.json **(in bold)**:

```
{
  "name": "react_search",
  "version": "1.0.0",
  "description": "Search app using React",
  "main": "index.js",
  "scripts": {
    "start": "webpack --mode development",
    "build": "webpack --mode production"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.0.1",
    "webpack-cli": "^2.0.10"
  }
}
```

``important``

Webpack 4 now has two modes, **development** and **production** where code is minimised in the latter.

> time for See it for yourself by running:

```
 npm run start
```
> This will create a dist folder with main.js file inside **(containing our src code)**.

![main.js](https://cdn-images-1.medium.com/max/1000/1*Fz0Ulaqaz1K4jSQCYL13zg.png)


``If you now run:``

```npm run build```

> The following output is now like this:

![main.js](https://cdn-images-1.medium.com/max/1000/1*P3Mq87Jp9w0iaT8Sqb0jfw.png)


## Setting Up React and Babel

To work with React, we need to install it along with Babel. This will **transpile** the code from ES6 to ES5, as not all browsers support ES6 yet **(for example Internet Explorer)**.

> Install react and react-dom as a dependency

```
npm i react react-dom -S
```

- S: — save

> Then install **babel-core**, **babel-loader**, **babel-preset-env** and **babel-preset-react** as a dev dependency:

```
npm i babel-core babel-loader babel-preset-env babel-preset-react -D

```

- **babel-core:** Transforms your ES6 code into ES5

- **babel-loader:** Webpack helper to transform your JavaScript dependencies (for example, when you import your components into other                       components) with Babel
- **babel-preset-env:** Determines which transformations/plugins to use and polyfills (provide modern functionality on older browsers                       that do not natively support it) based on the browser matrix you want to support
- **babel-preset-react:** Babel preset for all React plugins, for example turning JSX into functions

> Now We need to create a webpack.config.js file to state the rules for our babel-loader.

```javaScript

module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      }
    ]
  }


```

We then need to make a separate file called **.babelrc** to provide the options for **babel-loader.** You can include it in the **webpack.config.js** file, When we state that we’re using babel-loader in our webpack config, it will look for **.babelrc** file if there is one.

```javaScript

{
  "presets": ["env", "react"]
}

```


> Next, change our index.js file to render a component:

```javaScript

import React from "react";
import ReactDOM from "react-dom";

const Index = () => {
  return <div>Hello React!</div>;
};

ReactDOM.render(<Index />, document.getElementById("index"));

```

We will also need to create an **index.html** file in the **src** folder where we can add our section element with id index. This is where we render our main react component:

```html

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>React and Webpack4</title>
</head>
  <body>
    <section id="index"></section>
  </body>
</html>

```

Now we need to install **html-webpack-plugin** and use this in our webpack config file.
This plugin generates an HTML file with < script > injected,
writes this to **dist/index.html**, and minifies the file.

> Install html-webpack-plugin as a dev dependency:

```
npm i html-webpack-plugin -D
```

> Update the webpack config like so:

```javaScript

const HtmlWebPackPlugin = require("html-webpack-plugin");

const htmlPlugin = new HtmlWebPackPlugin({
  template: "./src/index.html",
  filename: "./index.html"
});

module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      }
    ]
  },
}

```
> You can also input the plugin like this:

```
plugins: [
    new HtmlWebPackPlugin({
    template: "./src/index.html",
    filename: "./index.html"
  });
]
```

> But, the prefer way is to extract this into a variable so we can see the list of plugins we are using.

The value we are giving the ``template`` key is where we are looking for our HTML file. The filename value is the name of the minified HTML that will be generated in the dist folder.

> If we now run npm run start we should see **index.html** being generated in the dist folder.

![SolutionExplorer](https://cdn-images-1.medium.com/max/1000/1*rF3Cnp3lRfgfZfFbn3CWEw.png)

Now run open ``dist/index.html`` and you should see “Hello React” in your browser.


## Setting up webpack-dev-server

It is a bit tedious to keep running this command every time you want to see your changes in the browser.
To have webpack “watch” our changes and thus refresh whenever we have made changes to any of our components,
we can use **webpack-dev-server** module.

Go ahead and install this as a dev dependency

```

npm i webpack-dev-server -D

```

> Then change your package.json start scripts like so (in bold):


```javaScript
hange your package.json start scripts like so (in bold):

{
  "name": "react_search",
  "version": "1.0.0",
  "description": "Search app using React",
  "main": "index.js",
  "scripts": {
    "start": "webpack-dev-server --mode development --open", // <-- this line should be added
    "build": "webpack --mode production"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "react": "^16.2.0",
    "react-dom": "^16.2.0"
  "devDependencies": {
    "babel-core": "^6.26.0",
    "babel-loader": "^7.1.4",
    "babel-preset-env": "^1.6.1",
    "babel-preset-react": "^6.24.1",
    "html-webpack-plugin": "^3.0.6",
    "webpack": "^4.1.1",
    "webpack-cli": "^2.0.10",
    "webpack-dev-server": "^3.1.0"
  }
}

```

If we now run ``npm run start`` you should see **localhost:8080** open up in our default browser — that’s what the ``—-open`` flag is for. Now everytime you make changes, it will refresh the page.

You can also add a ``--hot`` flag to your npm start script which will allow you to only reload the component that you’ve changed instead of doing a full page reload. 


## Setting up CSS

The last part involves setting up our CSS. As we will be importing CSS files into our React components,
we need **css-loader** module to resolve them. Once that’s resolved, we also need a **style-loader** to inject
this into our DOM — adding a < style > tag into the < head > element of our HTML.

> Go ahead and install both of these modules as a dev dependency:

```
npm i css-loader style-loader -D
```

> We then need to update our webpack.config.js file like so:


```javaScript

const HtmlWebPackPlugin = require("html-webpack-plugin");

const htmlWebpackPlugin = new HtmlWebPackPlugin({
  template: "./src/index.html",
  filename: "./index.html"
});

module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      },
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"]
      }
    ]
  },
  plugins: [htmlWebpackPlugin]
};

```
**Note that the order of adding these loaders is important. First, 
  we need to resolve the CSS files before adding them to the DOM with the style-loader.
  By default, webpack uses the loaders from the right (last element in the array)
  to the left (first element in the array).**

> Making CSS modular

We can also make CSS modular using webpack. This means class name will be scoped locally and specific to only the component in question.

> To do this, we can provide some options to css-loader:

```javaScript

const HtmlWebPackPlugin = require("html-webpack-plugin");

const htmlWebpackPlugin = new HtmlWebPackPlugin({
  template: "./src/index.html",
  filename: "./index.html"
});

module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      },
      {
        test: /\.css$/,
        use: [
          {
            loader: "style-loader"
          },
          {
            loader: "css-loader",
            options: {
              modules: true,
              importLoaders: 1,
              localIdentName: "[name]_[local]_[hash:base64]",
              sourceMap: true,
              minimize: true
            }
          }
        ]
      }
    ]
  },
  plugins: [htmlWebpackPlugin]
};

```

As we need to give options, each loader is now an object with a key-value pair.
To enable CSS modules, we need to set **module** option for css-loader to be **true**.
The **importLoaders** option configures how many loaders before css-loader should be applied.
For example, sass-loader would have to come before css-loader.

``The localIdentName allows you to configure the generated identification.``

- **[name]** will take the name of your component
- **[local]** is the name of your class/id
- **[hash:base64]** is the randomly generated hash which will be unique in every component’s CSS

To make this a bit more visual, I’ll give you an example.
Say I have a component named ``Form`` and I have a button with a CSS class ``primaryButton``.
I also have another component called ``Search`` and a button in it with a CSS class ``primaryButton``.
However, both of these classes have different CSS:

```css
  Form button
  .primaryButton {
    background-color: green;
  }
  Search button
  .primaryButton {
    background-color: blue;
  }
```

When webpack bundles our application, depending on which CSS comes latest, both of our buttons could have the color green or blue instead of Form having green and Search having blue.

This is where the localIdentName comes into place. With this, once our application is bundled, our buttons will have a unique class name!

![DevToolCrome](https://cdn-images-1.medium.com/max/1000/1*r9EZ9GzG_Xkya_ON1uGqIQ.png)

As you can see, the button class name in the Form component is different to the one in the Search component — their naming starts with the name of the component, class name, and unique hash code.

So with this, you won’t have to worry about whether you have given the same class name throughout your whole application — you only have to worry about whether you have used it in the same component.
