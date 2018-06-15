# Setting up a web project
 
1. Initial setup
 
    * Create a folder to contain the project and make it the current directory
 
         ```
         mkdir app
         cd app
         ```
    * Create a directory called ```src``` to contain the code. Then create a javascript file in ```src``` called ```index.js```
 
         ```   
         mkdir src
         touch index.js
         ```
 
    * Create the node ```package.json``` configuration file. Enter whatever values make sense
 
        ```
        npm init
        ```
   
2. Setup the build process
 
   The project will be built using webpack.
 
    * Install webpack
 
        ```      
        npm install webpack@<version> --save-dev
        ```
 
    * Create the webpack configuration file in the root of the project
 
        ```  
        touch webpack.config.js
        ```
 
    * Setup webpack entry point and output
   
        The initial build, will process the ```./src/index.js``` file and place the result into ```./dist/bundle.js```. In the ```webpack.config.js``` file enter the following:

        ```
        const path = require("path")

        module.exports = {
            entry: "./src/index.js",
            output: {
                filename: "bundle.js"
                path: path.join(__dirname. "dist")
        }
        ```
   
    * Setup the build script
   
        In the ```package.json``` file remove the test script, if there is one, and add a build script. The scripts section should look like the following:
   
        ```
        ...
        "scripts": {
             "build": "webpack"
        }
        ...
        ```
        Now building the project can be done like this:
   
        ```
        npm run build
        ```
 
     * Create a script to automatically build when a file changes
   
        We can create a script that 'observes' specified directories and files, and runs the build script when a change is detected. Add a ```watch``` script to ```package.json```:
   
        ```
        ...
        "scripts": {
            "build": "webpack",
            "watch": "webpack --w"
        }
        ...
        ```
   
    * Setup babel
   
        Babel is used to, among other things, transpile javascript 5+ into javascript 5 so that most browsers can run the project. We need to setup webpack to run babel as part of the build.
    
        The first thing to do is to update ```webpack.config.js``` to configure webpack to run babel. In ```webpack.config.js``` add the following after the ```output``` key:
   
        ```
        ...
        module: {
            rules: [
                {
                    test: /.js$/,
                    exclude: /(node_modules)/,
                    use: {
                        loader: "babel-loader",
                        options: {
                            presets: ["env"]
                        }
                    }
                }
            ]
        }
        ...
        ```
   
        The next thing to do is to install the babel packages. To do that run the following command:
   
        ```
        npm install babel-loader babel-core babel-preset-env --save-dev
        ```
 
        The final thing required to setup babel, is to create a babel config file:
   
        ```
        touch .babelrc
        ```
   
        In ```.babelrc``` enter the following:
   
        ```
        {
            "presets": ["env"]
        }
        ```
   
        So at this point, running ```npm run build``` will result in the code in the ```src``` directory being bundled into the ```dist``` directory. The javascript in the ```dist/bundle.js``` is the javascript 5 version of the code in ```src/index.js```.
   
    * Setup Flow
   
        I like types. Having them is better than not having them, IMO. So I'll use Flow to get them.

        Firstly, prepare babel to strip flow types from our flow code

        ```
        npm install babel-cli babel-preset-flow --save-dev
        ```
    
        Add a ```flow``` preset to ```.babelrc```
   
        ```
        {
            "presets": ["env", "flow"]
        }
        ```
   
        Now install flow:
   
        ```
        npm install flow-bin --save-dev
        ```
        
        Add a flow script to ```package.json```:
        
        ```
        ...
        "scripts": {
            "build": "webpack",
            "watch": "webpack --w",
            "flow": "flow"
        }
        ...
        ```
        
        Now initialise flow:
        
        ```
        npm run flow init
        ```
        
        You should now have a file called ```.flowconfig``` next to ```package.json```.
        
