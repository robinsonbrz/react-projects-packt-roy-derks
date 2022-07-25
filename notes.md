# Creating a Single-Page Application in React
• Setting up a new React project
• Structuring a project

Project overview
In this chapter, we will create a single-page application in React that retrieves data from an API and runs in the browser with Webpack and Babel.
Styling will be done using Bootstrap.
The application that you'll build will show information about the popular TV show Rick and Morty, along with images.

Git author repo https://github.com/PacktPublishing/React-Projects-Second-Edition/tree/main/Chapter01

at least Node.js v14.17.0

check the installed versions

• For Node.js (which should be v14.17.0 or higher), use this:
```bash
node -v
```
• For npm (which should be v6.14.3 or higher), use this:
```bash
npm -v
```

Also, you should have installed the **React Developer Tools plugin** (for Chrome andFirefox) and added it to your browser. 
This plugin can be installed from the 
[Chrome Web Store](https://chrome.google.com/webstore) 
[Firefox Add-ons](https://addons.mozilla.org)

## Setting up a project


Every time you create a new React project, the first step is to create a new directory on your local machine. 
Inside this new directory, execute the following from the command line:

```bash
mkdir chapter-1
npm init -y
```

Running this command will create a fresh package.json file with the information needed to run a JavaScript/React project. 
By adding the -y flag to the command, we can automatically skip the steps where we set information such as the name, version, and description.


To learn more about the workings of package.json, make sure to read the documentation from npm: https://docs.npmjs.com/cli/v6/ configuring-npm/package-json.


### Setting up Webpack

To run the React application, we need to install Webpack

and the Webpack CLI as devDependencies. 
Webpack is a library that lets us create a bundle out of JavaScript/React code that can be used in a browser.

Install the required packages from npm using the following command:

```bash
npm install --save-dev webpack webpack-cli
```
create a folder "src"
create a file inside "index.js"

inside javascript index.js file:

```javascript
console.log('Rick and Morty');
```

To run the preceding code, we will add the start and build scripts to our application using Webpack.

Edit the package.json file

Remove the following line to the package.json:
    "test": "echo \"Error: no test specified\" && exit 1"
Add the following to the package.json:
    "start": "webpack --mode development",
    "build": "webpack --mode production"

The **npm start** command will run Webpack in development mode, 
while **npm run** build will create a production bundle using Webpack. The biggest difference is that running Webpack in production mode will minimize our code and decrease the size of the project bundle.

We now run the **npm start** or **npm build** command from the command line; Webpack will start up and create a new directory called dist:

Inside **dist** directory, there will be a file called main.js that includes our project code and is also known as our bundle.

Depending on whether we've run Webpack in development or production mode,
the code will be minimized in this file.

You can check whether your code is working by running the main.js file in your
bundle from the command line:

```bash
node dist/main.js
```

This command runs the bundled version of our application and should return the
following output:
```bash
> node dist/main.js
Rick and Morty
```

### Configuring Webpack to work with React

Installing the packages we need in order to run any React application.

These packages are **react** and **react-dom**, where the former is the generic core package
for React and the latter provides an entry point to the browser's DOM and renders React.

```bash
npm install react react-dom
``````

Installing only the dependencies for **React** is not sufficient to run it, since, by default, not every browser can read the format (such as **ES2015+ or React**) that your JavaScript code is written in. 

Therefore, we need to **compile the JavaScript** code into a readable format for
every browser.

For this, we'll use **Babel** and its related packages to create a toolchain to use React in the browser with Webpack. 

These packages can be installed as **devDependencies** by running the following command:

```bash
npm install --save-dev @babel/core babel-loader @babel/preset-env @babel/preset-react
```

With the packages for React and the correct compilers installed, the next step is to make them work with Webpack so that they are used when we run our application.

To do this, configuration files for both Webpack and Babel need to be created in the **root** directory of the project:

babel.config.json
webpack.config.js


The configuration for Webpack is added to the webpack.config.js file to use
babel-loader:
```json
module.exports = {
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: {
                    loader: 'babel-loader'
                },
            },
        ],
    },
};
```


The configuration in this file tells Webpack to use babel-loader for every file that
has the .js extension and excludes files in the node_modules directory for the
Babel compiler.
To use the Babel presets, the following configuration must be added to babel.config.
json:
```json
{
    "presets": [
        [
            "@babel/preset-env",
            {
                "targets": {
                    "esmodules": true
                }
            }
        ],
        [
            "@babel/preset-react",
            {
                "runtime": "automatic"
            }
        ]
    ]
}
```

### Rendering a React project

After setting up Babel and Webpack, we need to create an actual React component that can be compiled and run.

Edit the **index.js** file that already exists in our src directory so that we can use react and react-dom.

```javascript
import ReactDOM from 'react-dom/client';
function App() {
    return <h1>Rick and Morty</h1>;
}
const container = document.getElementById('app');
const root = ReactDOM.createRoot(container);
root.render(<App />);
```
create a file that has this element in a new directory called public and
name that file index.html:

The final step in rendering our React component is **extending Webpack** so
that it adds the minified bundle code to the body tags as scripts when running.

Therefore, we should install the **html-webpack-plugin package** into
our **devDependencies**:

```bash
npm install --save-dev html-webpack-plugin
```

To use this new package to render our files with React, the Webpack configuration
in the **webpack.config.js** file must be extended:

```json
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  module: {
      rules: [
          {
              test: /\.js$/,
              exclude: /node_modules/,
              use: {
                  loader: 'babel-loader'
              },
          },
      ],
  },

  plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html',
      filename: './index.html',
    }),
  ],
};

```

```bash
npm start
```
Now, if we run **npm start** again, Webpack will start in development mode and add the index.html file to the dist directory. 
Inside this file, we'll see that, inside our body tag, a new scripts tag has been inserted that directs us to our application bundle – that is, the dist/main.js file. 
We can do the same when running the npm run build command to start Webpack in production mode; the only difference is that our code will be minified:

