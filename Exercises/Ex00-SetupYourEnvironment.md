# Setting up your environment
This training workshop is organized on a series of hands-on labs. Hands-on labs are sets of step-by-step guides that are designed to help you learn your ways around Continous Delivery and automation. Each Lab provides instructions to guide you through the process of developing a step of the build automation process.

## Installing Node (with NPM)
Our sample code is based on a Node.js application, which we're going to run end to end. It's a modified version of the HTML5Boiler plate.

![](https://i.cloudup.com/5EEpEtc8vt.png)

- Navigate to [http://nodejs.org](http://nodejs.org) with your Web Browser
- Download the installer for your operating system
- Voila! You can now run Node.js applications.

## Installing Grunt CLI
In order to use grunt (which we'll add to our project later on), you'll want to install Grunt's command line interface (CLI) globally. You may need to use sudo (for OSX, *nix, BSD etc) or run your command shell as Administrator (for Windows) to do this.

```
npm install -g grunt-cli
```

This will put the `grunt` command in your system path, allowing it to be run from any directory.

**Note**. Installing grunt-cli does not install the Grunt task runner! The job of the Grunt CLI is simple: run the version of Grunt which has been installed next to a Gruntfile. This allows multiple versions of Grunt to be installed on the same machine simultaneously.

## Installing GIT
We need Git to work with our project repository where there code of the workshop will reside. 

If you have OS X you probably have `git` installed, if don't go to [http://git-scm.com/](http://git-scm.com/).

## Create a public Github repository for our project
As we evolve our app all its change will be stored on this repo which is the one that we're going to monitor using our CI Server.

**NOTE: If don't have a Github account, get one.**

- Go to [Github.com](http://github.com)
- Click on **New Repository**.
- Give it a name like _AutomateAllTheThingsWorkspace_
- Make it **public**
- Add **.gitignore** for **node.js**
- Click on the create button.

![](https://i.cloudup.com/s84dA1Kna1.gif)

## Initialize your local workspace
- Clone the Github repository that you've just created somewhere on your hard drive.
- Copy from the contents Code/BeginSolution from the [AutomateAllThings](https://github.com/johnnyhalife/AutomateAllThings) repository to the root of your recently created repository.
- `git add .`
- `git commit -am "initial code commit"`
- `git push origin master`

## Verification (or testing that everything works...)
- On your workspace repository, run `npm install`
- Then do, `node app.js`, and you should see 

```
$ node app.js 
Application server running on 4000
```

- Open a Web Browser and **navigate to http://localhost:4000**, you should see the following 

![](https://i.cloudup.com/5EEpEtc8vt.png)


## Done!

Now we're ready to start working on automating our _devployment_ process.


