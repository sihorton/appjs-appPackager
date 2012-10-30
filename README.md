appPackager
===========
A script for creating packages of your appjs applications.

Quick Start
===========

Packager for AppJS to serve application content from a single package file.

    node appPackager data/content
    
appPackager will take all of the files in data/content and create a data/content.appjs package containing the files.

    node appPackager data/content.appjs
    
appPackager will unpack all of the files in the data/content.appjs package into data/content/ folder.

Use with AppJS
==============

To see an example of how to use the application packages see this related project: https://github.com/sihorton/appjs-platform/

To use with vanilla AppJS save [packagedApp.js](https://raw.github.com/sihorton/appjs-appPackager/master/packagedApp.js) to your data directory and then replace the following line in app.js:

    app.serveFilesFrom(__dirname + '/content');
    
with

    var packageRunner = require('./packagedApp.js');
    app.router.use(packageRunner)(__dirname + '/package.appjs');

Copy the node_modules/adm-zip folder from the appPackager directory into your AppJS/data/node_modules/adm-zip folder.

Then run appjs as normal but now files will be served from the package.appjs file instead of the content/ folder.


Routers
======
packagedApp.js contains a new router that can be used to serve files from a package instead of a directory. 
The router yields if it cannot find a matching file enabling you to serve files from a package and a directory:

    app.serveFilesFrom(__dirname + '/content');
    app.router.use(require('./packagedApp.js')(__dirname + '/package.appjs'));

The code above will serve files from the package.appjs unless there is a matching file in the content directory in which case the 
content directory file will be used instead.

Detail
======

appPackager uses [node-native-zip](https://github.com/janjongboom/node-native-zip) and [adm-zip](https://github.com/cthackers/adm-zip)
to pack and unpack the package files. These are written in pure javascript so they should work cross platform. 
The package file is in a zip style format with no compression (so node can serve the files fast without needing to decompress).
