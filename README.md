appPackage
==========

Router for AppJS that can serve your content files from a package.

Quick Start
===========
Download a pre-built AppJS package from [here](http://dists.appjs.org/) and extract somewhere. 
Then download a [zip of this repository](https://github.com/sihorton/appPackage/zipball/master) and extract the files
into the "data" folder from the AppJS package.
When you then run the demo it will serve the app from the supplied "example.appjs" package.

Detail
======
packagedApp.js contains a new router that can be used to serve files from a package instead of a directory.

    app.router.use(require('./packagedApp.js')(__dirname + '/example.appjs'));

This line loads the packagedApp router and then serves the file (in this case example.appjs in the current directory.
If the router finds a file with the correct pathname then it will serve the file, otherwise it will yield and let another router try to serve the file.

So the following code will serve first from a local directory and if not from the packaged file:-

    app.serveFilesFrom(__dirname + '/content');
    app.router.use(require('./packagedApp.js')(__dirname + '/example.appjs'));

The package file uses adm-zip to extract from a zip file format so you can rename to .zip and view the package contents. A separate project will
supply a program to create the packaged apps. This also means that the serving is done using javascript.


