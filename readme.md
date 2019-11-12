1) Create project repository

2) Cd into repository and git init

3) Initialize npm package manager in terminal: npm init -y

4) Install webpack as a dependency in terminal: npm install webpack@4.39.3 --save-dev --save-exact

5) Install package to have access to CLI (command line interface) for webpack: $ npm install webpack-cli@3.3.8 --save-dev

6) Create .gitignore file in root of project repository

7) Add files you want ignored to .gitignore example :
node_modules/
.DS_Store
dist/

8) Get rid of the test script in package.json for now and replace with:
"scripts": {
    "build": "webpack"
  },

9) Configure webpack in root of repository. In terminal: touch webpack.config.ls

10) Paste following code into webpack.config.ls:
const path = require('path');

module.exports = {
  entry: './src/main.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
};

11) In root directory, create the following files and paths:
project-name/
├── dist
│   └── index.html
├── package-lock.json
├── package.json
├── src
│   ├── main.js
│   └── ping-pong.js
├── webpack.config.js
└── .gitignore

12) Business Logic is created in project-name.js

13) User Interface Logic is in main.js

14) Example of importing and exporting js code:

Business
export function pingPong(goal) {
  var output = [];
  for (var i = 1; i <= goal; i++) {
    if (i % 15 === 0) {
      output.push("ping-pong");
    } else if (i % 3 === 0) {
      output.push("ping");
    } else if (i % 5 === 0) {
      output.push("pong");
    } else  {
      output.push(i);
    }
  }
  return output;
}


Interface

import { pingPong } from './ping-pong';

$(document).ready(function() {
  $('#ping-pong-form').submit(function(event) {
    event.preventDefault();
    var goal = $('#goal').val();
    var output = pingPong(goal);
    output.forEach(function(element) {
      $('#solution').append("<li>" + element + "</li>");
    });
  });
});

15) Now run the following in terminal to create bundle.js: npm run build

16) Add the following two dependencies in terminal: npm install style-loader@1.0.0 css-loader@3.2.0 --save-dev

17) Update your webpack.config.js to include css module:

const path = require('path');

module.exports = {
  entry: './src/main.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          'style-loader',
          'css-loader'
        ]
      }
    ]
  }
};

18) Create styles.css in the src folder

19) Import styles.css into the main.js file so it's available to bundle:
In main.js type the following:

import './styles.css';

20) Run the following to see updated styles in index.html:
npm run build

21) Move our index.html file from dist to src and let webpack handle outputting it to dist

22) Install the html webpack plug in:
npm install html-webpack-plugin@3.2.0 --save-dev

23) const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/main.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: 'Ping Pong',
      template: './src/index.html',
      inject: 'body'
    })
  ],
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          'style-loader',
          'css-loader'
        ]
      }
    ]
  }
};

24) Now run $ npm run build. Webpack will add index.html to our dist folder. If you take a look, you'll see that webpack has added a script tag for our bundled JavaScript to the bottom of our HTML file.

25) Install a plugin that will clean up our webpack:
npm install clean-webpack-plugin@3.0.0 --save-dev

26) Update webpack.config.js to include the new plugins:
...

const { CleanWebpackPlugin } = require('clean-webpack-plugin'); //new line

module.exports = {

  ...

  plugins: [
    new CleanWebpackPlugin(),   // new line
    new HtmlWebpackPlugin({
      title: 'Ping Pong',
      template: './src/index.html',
      inject: 'body'
    })
  ],

  ...

}

27) Minify or Uglify code with by installing the following plug in:
 npm install uglifyjs-webpack-plugin@2.2.0 --save-dev

28) Update webpack.config.js to include the new plugins:
...
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');  // new line

module.exports = {
  ...
  plugins: [
    new UglifyJsPlugin(),    // new line
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      title: 'Ping Pong',
      template: './src/index.html',
      inject: 'body'
    })
  ],
  ...
}

29) Install webpack development server:
npm install webpack-dev-server@3.8.0 --save-dev

30) Update webpack.config.js to include the new plugins:
...

module.exports = {
  entry: './src/main.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  devtool: 'eval-source-map',  // new line
  devServer: {                 // new line
    contentBase: './dist'      // new line
  },

  ...

};

31) Update plugin section of webpack.config.js:

...
module.exports = {
  ...
  plugins: [
    new UglifyJsPlugin({ sourceMap: true }),
    ...
  ],
  ...
}

32) Update scripts section of package.json:

...
  "scripts": {
    "build": "webpack",
    "start": "webpack-dev-server --open"
  },
...

33) Update build section in package.json to:
...
  "build": "webpack --mode development",
...

34) Update scripts section of package.json:
...
  "scripts": {
    "build": "webpack --mode development",
    "start": "npm run build; webpack-dev-server --open --mode development"
  },
...

35) Install JS Linter:
npm install eslint@6.3.0 --save-dev
npm install eslint-loader@3.0.0 --save-dev

36) Update webpack.config.js:
module.exports = {
  ...
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          'style-loader',
          'css-loader'
        ]
      },
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: "eslint-loader"
      }
    ]
  }
};

38) Create .eslintrc file in the root of project directory:
touch .eslintrc

39) Paste the following into the .eslintrc file:
{
    "parserOptions": {
        "ecmaVersion": 2017,
        "sourceType": "module"
    },
    "extends": "eslint:recommended",
    "env": {
      "es6": true,
      "browser": true,
      "jquery": true
    },
    "rules": {
        "semi": 1,
        "indent": ["warn", 2]
    }
}

40) Optionally add the following rules to rules section in .eslintrc:

"rules": {
     "semi": 1,
     "indent": ["warn", 2],
     "no-console": "warn",
     "no-debugger": "warn"
 }

 41) add lint to scripts in package.json :

 ...
  "scripts": {
    "build": "webpack --mode development",
    "start": "npm run build; webpack-dev-server --open --mode development",
    "lint": "eslint src/*.js"
  },
...

42) run the following in terminal to check for lint:

npm run lint

43)  Your package.json file should resemble the following:
{
  "name": "",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "webpack --mode development",
    "start": "npm run build; webpack-dev-server --open --mode development",
    "lint": "eslint src/*.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "clean-webpack-plugin": "3.0.0",
    "css-loader": "^3.2.0",
    "eslint": "^6.3.0",
    "eslint-loader": "^3.0.0",
    "html-webpack-plugin": "^3.2.0",
    "style-loader": "^1.0.0",
    "uglifyjs-webpack-plugin": "^2.2.0",
    "webpack": "4.39.3",
    "webpack-cli": "^3.3.8",
    "webpack-dev-server": "^3.8.0"
  }
}
