# learn-webpack2

1. why webpack is so popular?

We need to understand a little bit more about ecosystem around web applications.

Server side Templating: 

Back end server createds an html document and sends it to the server

Single Page App: 

Server sneds a bare-bones html docments to the user, javascript runs on the users machine to assemble a full webpage.

In single page application world: we are relying upon JavaScript code that is being executed on our user's browser to assemble the entire web application. And we have a huge pile of JavaScript that is being shipped down to our user's browser.
Compared to the server-side rendering world or server-side templating world, however there used to be very little Javascript code that we would send down to a client because the obly Javascript code we would have would be stuff like handle a fancy event.


2. Purpose of Webpack?

If our code is all spread out into seperate modules, we need to start thinking about a lot about the order in which our code is excueted. We need to make sure every time our application runs that order is exactly the same.

We have to be really concerned about the performance over loading many different files up. This is exactly where webpack comes into play.

Webpack is to solve the issues related to having multiple javascript files.

So the basic purpose of webpack is to take our big collection of tiny little javascript modules and merge them all into one big javascript files while also ensuring that each modules is executed in the correct order.
Tha's the code of what webpack does.

It also can handle css or handle turning ES6 code into ES5 compatible code.


3. Specify the entry point :

const config = {
    entry: '/src/index.js',
    output: {
        path: require('path').resolve(__dirname, 'build),//helper from nodejs
        filename: 'bundle.js'
    }
}


4. loaders:

one of the most popular loaders is Babel, loaders can be applied to any types of file we wish but sometimes it only
makes sense to apply a loader to a very specific type of file.

If we are using babel, we dont want to try to make babel transpile like maybe a CSS file.
We are going to ensure that we only apply babel to javascript file inside of our project.


```
const config = {
    entry: '/src/index.js',
    output: {
        path: require('path').resolve(__dirname, 'build),//helper from nodejs
        filename: 'bundle.js'
    },
    module:{
        rules: [
            {
                use: 'babel-loader',
                test: /\.js$/, //this test property will be taken by webpack and applied to the file name of everyfile that we import into our project. This regular expression right here specifically looks to see if the file ends with .js.
            }
        ]
    }
}
```

inside of .babelrc

```
{
    "presets":["babel-preset-env"]
}

```


5. Syntax

CommonJs : const sum = require('./sum');  module.exports = sum
ES6: import sum from './sum'; export default sum;



6. Handle CSS files

The purpose of css-loader is to just to teach webpack how to import and parse css files.
Once it is included into webpack, however webpack still don't know how to deal with them. Then we are going to use style-loader. Style loader will take any imported css file and inject it into a style tag into our html document.

```
const config = {
    entry: '/src/index.js',
    output: {
        path: require('path').resolve(__dirname, 'build'),//helper from nodejs
        filename: 'bundle.js'
    },
    module:{
        rules: [
            {
                use: 'babel-loader',
                test: /\.js$/, //this test property will be taken by webpack and applied to the file name of everyfile that we import into our project. This regular expression right here specifically looks to see if the file ends with .js.
            },
            {
                use: ['style-loader', 'css-loader'],// loaders will be applied from right to left
                test: /\.css$/
            }
        ]
    }
}

```


save and combine css file into one css files :

```
const config = {
    entry: './src/index.js',
    output:{
        path:path.resolve(__dirname,'build'),
        filename:'bundle.js',
        publicPath: ' build/'
    },
    module:{
        rules:[
            {
                use:'babel-loader',
                test:/\.js$/ 
            },
            {
                loader: ExtractTextPlugin.extract({
                    loader: 'css-loader',

                }),
                test:/\.css$/
            }
        ]
    },
    plugins: [
        new ExtractTextPlugin('style.css')
    ]
};

```

7. image(image-webpack-loader, url-loader)

If image is small, include the image in bundle.js as raw data, 
if image is big, include the raw image in the output directory.

```
const config = {
    entry: './src/index.js',
    output:{
        path:path.resolve(__dirname,'build'),
        filename:'bundle.js',
        publicPath: ' build/'
    },
    module:{
        rules:[
            {
                use:'babel-loader',
                test:/\.js$/ 
            },
            {
                loader: ExtractTextPlugin.extract({
                    loader: 'css-loader',

                }),
                test:/\.css$/
            },
            {
                use:[
                    {
                        loader:'url-loader',
                        options:{limit: 4000}//4000 right here means look for any images that are 4000 bytes large, if image is larger than 4 kilobytes, save as seperate file, otherwise include it into our bundle.js output.
                    },
                    'image-webpack-loader'
                ],
                test:/\.(jpg?e|png|git|svg)$/
            }
        ]
    },
    plugins: [
        new ExtractTextPlugin('style.css')
    ]
};
```



8. public path

As we have learned , the url loaders purpose is to take the image that we have included inside of our project and copy it over to our build folder.
It can prepend it to the url that gets passed back from that import statement.


```
const config = {
    entry: './src/index.js',
    output:{
        path:path.resolve(__dirname,'build'),
        filename:'bundle.js',
        publicPath: ' build/'
    },
    module:{
        rules:[
            {
                use:'babel-loader',
                test:/\.js$/ 
            },
            {
                loader: ExtractTextPlugin.extract({
                    loader: 'css-loader',

                }),
                test:/\.css$/
            },
            {
                use:[
                    {
                        loader:'url-loader',
                        options:{limit: 4000}//4000 right here means look for any images that are 4000 bytes large, if image is larger than 4 kilobytes, save as seperate file, otherwise include it into our bundle.js output.
                    },
                    'image-webpack-loader'
                ],
                test:/\.(jpg?e|png|git|svg)$/
            }
        ]
    },
    plugins: [
        new ExtractTextPlugin('style.css')
    ]
};
```



9. What is code spliting?

Webpack allows us to split bundle.js output into seperate individual files. And we can control exactly when we load up different modules to show different code inside of our project.


10. webpack-dev-server

It can help rebuild webpack without rebuild all the files.


11. Deploy on AWS

AWS elastic beaanstalk encapsulates several other services.

eb init (select area)

eb create 

eb open

eb setEnv NODE_ENV=production

eb terminate