<!--
{
"name" : "npm",
"version" : "0.0.1",
"title" : "NPM – Node.js package manager",
"description" : "The most important things you need to know in order to use NPM effectively.",
"author" : "Aleksandar Olić",
"homepage" : "https://semaphoreci.com/community/tutorials/npm-node-js-package-manager",
"canonicalSource" : "https://semaphoreci.com/community/tutorials/npm-node-js-package-manager",
"freshnessDate" : 2015-04-01,
"license" : "CC BY-NC-SA"
}
-->

<!-- @section -->

# Introduction

[NPM](https://www.npmjs.com/) is a [Node.js](http://nodejs.org/) package manager that comes bundled with the installation of Node.js. It keeps track of all dependencies for your Node.js projects and gives you full control over managing them.

NPM makes is easy to reuse thousands of existing open source packages that solve a multitude of common problems, as well as share your own code with the community through the NPM registry.

Since JavaScript is now being used both for client and server-side code, there are lots of different packages: some for server side, some that are in fact command line programs and some that are made for front-end development. All those packages are listed on the official NPM website and in the NPM registry, the database of all the available open source packages.

This tutorial is an introduction to what you can do with NPM and a reference to its most frequently used commands.


<!-- @section -->

## Initializing a project

When you start a new project utilizing Node.js, you need to create a `package.json` file. It is a manifest file which tracks your dependencies, provides some metadata for your project and can optionally define some project-specific tasks to perform from command line.

If you have Node.js installed, `npm` command will be available. To check if it is installed just run:

```
npm -v
```

You can create a `package.json` file manually or bootstrap it interactively from the command line. The process will guide you through some common questions about the project, let you preview the file and then generate it.

```bash
$ mkdir ~/myproject
$ cd ~/myproject
$ npm init
name: (a) myapp
version: (1.0.0)
description: A very cool app
entry point: (index.js)
test command:
git repository:
keywords:
author: mrfoo@acme.com
license: (ISC)
About to write to /home/mrfoo/myproject/package.json:

{
  "name": "myapp",
  "version": "1.0.0",
  "description": "A very cool app",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Mr Foo <mrfoo@acme.com>",
  "license": "ISC"
}

Is this ok? (yes)
```

**Note**: mark the application as private if you don’t plan to put it in an actual repository by adding the "private" property to `package.json`:

```json
"private": true
```

<!-- @task, "hasDeliverable" : true, "text" : "Submit the contents of your \'package.json\' file."-->

The [official documentation](https://docs.npmjs.com/files/package.json) provides a good overview of all the available properties in `package.json`. We will touch upon the most useful ones.

You can also check out an interactive example of a full `package.json` below.

<!-- @link, "url" : "http://browsenpm.org/package.json", "text": "Explore what different fields do." -->

<!-- @section -->

## Installing dependencies

You can find over 100,000 projects available on [the official NPM website](https://www.npmjs.com/) where they are ranked by different criteria. A common thing to do is check if a package is actively maintained.

You can also search for packages from the command line:

```
npm search <search term>
```

You can visit the homepage of the package in your default browser by running:

```
npm home <package name>
```
<!-- @task, "text" : "Find one package that looks interesting to you."-->

<!-- @section -->

### Local installation

Now that you have a `package.json`, you can add a `dependencies` section and list everything you need:

```json
"dependencies": {
  "express": "*",
  "grunt": "*",
  "bower": "*"
}
```

To install all packages listed in `package.json` and their dependencies, navigate to your project directory and run the `install` command which will create a `node_modules` directory and download all packages there:

```
npm install
```

You can also install new packages and record them from the terminal by running:

```
npm install --save <package name>
```

Some other useful options:

*   Uninstall package and remove it from `package.json`: `npm uninstall --save <packagename>`
*   Install directly from the git repository: `npm install git://github.com/mrfoo/mypackage.git`
*   Install a specific version of the package: `npm install somepackage@1.1.0` or `npm install somepackage@latest`

It's important to note that the above method will install packages **locally**, meaning they will be stored and be usable only inside your project. This is good as it lets each project define its modules, their specific version and download them separately.

<!-- @task, "text" : "Add in some dependencies to your project and install them using \'npm install\'."-->

<!-- @section -->

### Global instalation

Another way to install a package is by doing a **global** installation. It is commonly used for packages that provide command line functionality, such as `grunt`,`bower` and `yo`.

For example, to install [JSHint](https://www.npmjs.com/package/jshint) globally, run the `install` command with `-g` flag and root privilege:

```
sudo npm install -g jshint
```

JShint should now be installed as a global package on the computer and can be ran from anywhere on a JavaScript file to lint it in a terminal.

```
jshint hello.js
```

It's not advised to install every package globally. That method is reserved mostly for command line utilities.


<!-- @section -->

## Managing dependencies

NPM lets you manage your project dependencies and the versions you need. Open source projects often move fast, and versions change frequently. You can control which kind of updates your project accepts in the `package.json` file.

NPM uses [semantic versioning](http://semver.org/), a standard to communicate what kind of changes are implemented in the release as it affects code stability.

A release number typically contains three elements:

*   Major release which breaks backwards compatibility, e.g. 2.0.0
*   Minor release with new features that don't break existing features, e.g. 1.1.0
*   Patch release with bug fixes and other minor changes, e.g. 1.0.1


<!-- @section -->

### Defining package versions

Let's say you start developing with certain versions of the packages. You can update them and still preserve compatibility by specifying the range of accepted updates.

```javascript
"dependencies": {
  "package1": "1.0.0",         // accepts only 1.0.0 version
  "package2": "1.0.x",         // any patch release in version 1.0
  "package3": "*",             // latest version, not recommended
  "package4": ">=1.0.0",       // any changes after 1.0.0
  "package5": "<1.9.0",        // anything less than 1.9.0
  "package6": "~1.8.0",        // shorthand for >= 1.8.0 < 1.9.0
  "package7": "^1.1.0",        // shorthand for >=1.1.0 < 2.0.0
  "package8": "latest",        // tag name for latest version
  "package9": "",              // same as * i.e. latest version
  "packageX": "<1.0.0 || >=2.3.1 <2.4.5 || >=2.5.2 <3.0.0"  
}
```

For a more granular approach consult the [NPM semantic version documentation](https://docs.npmjs.com/misc/semver).

Be sure to follow [best-practices](http://blog.nodejitsu.com/package-dependencies-done-right/) and patch versions of the packages so your code won't break.

**Note**: If you are not sure which version to use, just put `"*"` and run `npm update --save`. It will install the modules and change the asterisks to installed version number with `^` allowing for minor and patch updates.


<!-- @section -->

### Upgrading packages

To see all locally installed packages use `ls` command and add `-l` flag for short description and `--depth=0` for brevity and scope (add `-g` flag if you wish to see global packages):

```
npm ls -l --depth=0
```

To check out which packages can be updated locally and globally, run respectively:

```
npm outdated
npm outdated -g --depth=0
```

To **upgrade** to the highest acceptable version locally just run (to update globally just add `-g` flag and run as root):

```
npm update
```

If you removed a package in `package.json` just run the `prune` command to delete it from `node_modules` directory too.

```
npm prune
```

To upgrade NPM itself you need to install it as a global module again.

```
sudo npm install -g npm@latest
```

You can even update Node.js with NPM itself by installing a special module `n` and running it with either or `stable` as argument.

```
sudo npm install n -g
sudo n stable
```

You can run just `npm` to get a list of all available commands and help, or consult the [official website for documentation](https://docs.npmjs.com/).

<!-- @task, "text" : "Install a few out-of-date packages locally and try updating them."-->


<!-- @section -->

## Setting up NPM scripts

NPM has a built-in `run-script` command (or `npm run` for short). It works similarly to an alias in an operating system.

You define alias name as a property in `scripts` object which will execute the property's value as a command in the operating system's default shell.

You define you custom scripts in `package.json` like this (assuming you have the necessary dependencies listed and installed):

```json
"scripts": {
    "lint": "jshint **.js",
    "test": "mocha test/"
  }
```

So if we run `npm run lint` in the terminal now, it will be the same as we executed `jshint **.js` in the terminal. This makes `scripts` property very flexible for programming, chaining commands, calling other modules, using system commands or anything just as you would do in a regular shell script.

There are some shortcuts available in NPM: `npm test`, `npm start`, `npm stop` and `npm restart`.

If we run `npm test` it's the same as if we ran in default system shell `npm run-script test`, or `npm run test`, i.e. `mocha test/`.

Start scripts are usually defined so that they start main `app.js` file in Node.js `"start": "node app.js"`.

When we run `npm run lint` (or any other custom scipt) NPM also runs the command with _pre-_ and _post-_ hooks like this: `npm run prelint`, `npm run postlint`. This gives us a chance to programmatically control what happens before the command and after.

You can even eliminate the need of build tools like [Grunt](http://gruntjs.com/) or [Gulp](http://gulpjs.com/) by configuring the scripts. You can check out an [article on how to use NPM as a build tool here](http://blog.keithcirkel.co.uk/how-to-use-npm-as-a-build-tool/).


<!-- @section -->

## Development only dependencies

You can add a property for separately listing dependencies which are only used for development:

```json
devDependencies : {
  "packageX": "*"
}
```

`devDependencies` are meant to be used during development of a package, like unit testing, linting, minification and similar. In most cases if you are an end-user of a package, you just need `dependencies`.

By default when you run `npm install` from a project directory it installs both `dependencies` as well as `devDependencies` (as it assumes you are in source directory so you must be the developer).

If you add `--production` flag it will install only `dependencies`.

```
npm install --production
```

If you install from the public registry with `npm install <package>` it will install only `dependencies` (becasue you are just a user of the package).

To install a package from the terminal and save it in `package.json` under `devDependencies`, add `--save-dev` flag.

```
npm install --save-dev <package>
```

Stackoverflow provides a good [clarification on a difference between `dependencies` and `devDependencies`](http://stackoverflow.com/questions/18875674/whats-the-difference-between-dependencies-devdependencies-and-peerdependencies).

<!-- @task, "text" : "Install JSHint with the flag \'--production\'."-->


<!-- @section -->

## Publishing Node.js packages

Because every package is just a directory with `package.json` file and some other files, you can package your project, share it with the community and collaborate.

First you need to register yourself within NPM registry from the terminal and provide username, password and email address.

```
npm adduser
```

Now that you have an account you go to root of your to-be-published project and do:

```
npm publish
```


<!-- @section -->

## Summary

NPM makes it easy to install, manage and publish your packages. You start a new project by creating a `package.json` file, listing all the dependencies and installing them. For command line tools install packages globally.

While you work, periodically check for updates, add new dependencies and write custom scripts. You can also segregate your dependencies into `devDependencies` and `dependencies` making a distinction between production and development environment.

After finishing you can publish your project in the NPM registry for others to use.

For more information consult:

*   [The official NPM documentation](https://docs.npmjs.com/)
*   [Properties specification for a `package.json`](https://docs.npmjs.com/files/package.json)
*   [Interactive `package.json`](http://browsenpm.org/package.json)
*   [Semantic versioner for NPM](https://docs.npmjs.com/misc/semver)
*   [Most depended-upon packages](https://www.npmjs.com/browse/depended)
