# Meet Mr Jenkins
Jenkins is an application for building/testing software projects continuously, just like CruiseControl
or DamageControl. In a nutshell, Jenkins provides an easy-to-use so-called continuous integration system,
making it easier for developers to integrate changes to the project, and making it easier for users
to obtain a fresh build. The automated, continuous build increases the productivity.

We're going to use Jenkins to hook it up to our repository, listen to changes, and iteratively
build our project throughout the rest of the workshop.

## Installing Jenkins on your machine
We're going to run a local version of Jenkins but it can be hosted anywhere from Windows to Mac OS,
from a local server, your datacenter or any major IaaS provider.

- Go to [jenkins-ci.org](http://jenkins-ci.org/) and download the installer for your operating system.

- Follow the wizard steps to get it installed on your computer

![](https://i.cloudup.com/wiCEqxG6Ob.png)

- Once completed, Jenkins will open your Web browser and it will be running on [http://localhost:8080](http://localhost:8080)

- On Jenkins home page, go to "Manage Jenkins"

![](https://i.cloudup.com/PTWwIvqeA6.png)

- Select the "Manage plugins options"

![](https://i.cloudup.com/kNBTQd_TtX.png)

- We're now going to install the GIT plugin so we can check for changes on our Github project.

- Go to "Available" tab and filter for "Git Plugin"

![](http://puu.sh/8XC0H.png)

- Select the option for "Install without restart"

- On the install page mark the option for "Restart Jenkins when installation is complete and no jobs are running"

![](http://puu.sh/8XC4i.png)

- Jenkins will restart and then we'll be ready to go

![](https://i.cloudup.com/9tE3I08ZQo.png)

## Configuring your first build project
Jenkins projects are a build process instance, they point to a repo, branch(es) and
have a set of directives to run (in our case grunt). On this step we're going to configure
a project that will watch our workshop "workspace" on the `master` branch.

- On your Jenkins home page, click on **"Welcome to Jenkins! Please create new jobs to get started."**.

- Give your project a name (e.g. AutomateAllTheThingsWorkspace), select that it's a "freestyle software project" and click OK.

- Now let's configure the Source Code management.

- Select **Git**.

- On the **repository url complete with your workspace url**. (e.g. https://github.com/johnnyhalife/AutomateAllTheThingsWorkspace.git)

- As our project is public we don't need to specify credentials but we can if we wanted to.

- On branches to build leave `master`

- Click on save, and go to your project.

- Now, let's trigger a build by clicking on **Build Now**.

- As you can see it succeded, we fetched our project. Now let's add the `grunt` piece.

- Here comes the tricky part, specially on Mac OS.

- We need to update Jenkins user's path, so we need to go the terminal and do

```
sudo vim  /Library/LaunchDaemons/org.jenkins-ci.plist
```

- The look for the line that says "Environment Variables", and set that as

```xml
<key>EnvironmentVariables</key>
<dict>
        <key>PATH</key>
        <string>/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin</string>
        <key>JENKINS_HOME</key>
        <string>/Users/Shared/Jenkins/Home</string>
</dict>
```

- Save and close vim.

- Now let's stop Jenkins by doing

```
sudo launchctl unload -w /Library/LaunchAgents/org.jenkins-ci.plist
```

- And then run this command to start it again

```
sudo launchctl load -w /Library/LaunchAgents/org.jenkins-ci.plist
```

- Go back to your Web Browser, and let's configure `grunt` on Jenkins.

- Click on your project and select "Configure"

- Under "Build", select "Add Build Step" and choose "Execute Shell"

- Your Build Step should look like this

```
npm install && grunt
```

- Click on Save.

- Let's trigger another build, now with grunt, by clicking on **Build Now**.

- Success! Now it's running our own build process.

## Configure your build process to auto-trigger
This is normally hanlded with Github Plugin and Webhooks, since our Build Server is local (
or not internet accessible as an enterprise server) we're going to use SCM Polling (a.k.a asking GIT
if there's anything new).

- Click on your project and select "Configure"

- Under "Build Triggers", select "Poll SCM".

- On the Schedule textbox type `* * * * *` (which means every 1 minute in crontab)

- Click on Save.

- On your project workspace do a commit (if nothing to commit do `git commit --allow-empty -m "testing ci"`)

- That's it, you have a continuous integration server ready!

## Excercise completed
Prepare for the final challenge...
