appPackager
===========
A script for creating packages of your appjs applications.

Quick Start Linux:
==================
A debian package is available for linux: <a href="http://appjs.delightfulsoftware.com/platformInstall/appjs-packager.deb">appjs-packager.deb</a>

    appjs-packager <project-dir>
This will create a folder under <project-dir> called deploy, if no package.json is found in the project directory then one will be created.
The packager will then create <project-name>.appjs containing the application and <module-name>-<version>-<platform>.modpack.gz for each module.

    appjs-packager <project-name>.appjs
will extract the contents of the packaged file to <project-name> directory.

Quick Start Windows:
===========

appPackager should be run by node and passed a parameter of the project directory to package:

    node appPackager <project-dir>
    
appPackager will take the files in project-dir and create a project-dir.appjs package containing the files.

    node appPackager <project-name>.appjs
    
appPackager will unpack all of the files in the project-dir.appjs package into data/content/ folder.



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

Module Dependencies
========
In order to support applications that utilise node modules that are not in the vanilla appjs install the script scans the 
node_modules directory. For each module that it finds it creates a binary package file (.modpack) in the node_modules file
for the platform that the script is run on.

When running a packaged application appjs can then access this dependency information and download 

package.json and Dependancies
=======
appPackager will read and update a package.json file in the project directory. Each time it is run it will add any new
dependancies that were found in the node_modules directory. The file is a simple json text
file that you can edit and add any additional properties you like.

An example of the file is given below:

	{
		"name": "new-example",
		"version": 0.1,
		"moduleUrl": "http://example.com/modulePackages/",
		"appUpdateUrl": "http://example.com/new-example/latest.txt",
		"moddeps": {
			"adm-zip": {
				"name": "adm-zip",
				"version": "0.1.5",
				"platforms": {
					"win32": "win32"
				}
			},
			"appjs": {
				"name": "appjs",
				"version": "0.0.19",
				"platforms": {
					"win32": "win32"
				}
			},
			"appjs-win32": {
				"name": "appjs-win32",
				"version": "0.0.19",
				"platforms": {
					"win32": "win32"
				}
			},
			"iconv": {
				"name": "iconv",
				"version": "1.2.3",
				"platforms": {
					"win32": "win32"
				}
			},
			"mime": {
				"name": "mime",
				"version": "1.2.5",
				"platforms": {
					"win32": "win32"
				}
			}
		}
	}

Routers
======
packagedApp.js contains a new router that can be used to serve files from a package instead of a directory. 
The router yields if it cannot find a matching file enabling you to serve files from a package and a directory:

    app.serveFilesFrom(__dirname + '/content');
    app.router.use(require('./packagedApp.js')(__dirname + '/package.appjs'));

The code above will serve files from the package.appjs unless there is a matching file in the content directory in which case the 
content directory file will be used instead.

Cross platform
======
appPackager is written in javascript so is fully cross platform.

Security / Goals
======
The packaging of the application does not add any security at all. It is easy to extract all of the contents of the packaged
application. The goal for this project is to make it easy to take an application on my local disk, bundle it up and then
distribute it cross platform to other users. Doing this saves you having to distribute a 20 meg+ runtime along with your app
for each platform you can run on. It also provides a good solution for node_modules dependancies.
