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

``If you now run:``

```npm run build```

