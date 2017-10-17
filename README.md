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