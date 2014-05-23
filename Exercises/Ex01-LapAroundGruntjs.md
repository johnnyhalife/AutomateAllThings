# Lap Around Grunt.js
In this hands-on lab you will explore the basic elements of Grunt.js by creating a simple
Gruntfile, which we'll be using the rest of the Workshop, installing a Grunt plugin and
updating your package.json accordingly.

## Adding Grunt to our existing project
When we setup the project, we've installed the `grunt-cli` which doesn't necessarily mean that our
project is ready to run Grunt tasks. So let's get to it:

- Adding Grunt to an existing project is accomplished by doing `npm install grunt --save-dev`. This will
not only install the grunt (at its latest version) but also it'll add it to the `package.json` of our project.

- Open the `package.json`, and check that it got effectively added to it, you should see something like this
at the end of the file:

```json
"devDependencies": {
    "grunt": "^0.4.5"
  }
```

- Now, we are going to add the `Gruntfile.js`. This file is a valid Javascript
file, belongs in the root directory of your project,  next to the `package.json` and
should be committed with your project source.

- On your favorite text editor, add an empty file called `Gruntfile.js` (it's important to
keep the casing as is).

- Inside the file include the following snippet of code.

```js
module.exports = function(grunt) {

  grunt.registerTask('default', 'Hello World.', function() {
    grunt.log.write('Hello world from grunt...').ok();
  });

};
```

- Now back on your console you simply run `grunt`

- If everything worked as expected you should now see

```
$ grunt
Running "default" task
Hello world from grunt...OK

Done, without errors.
```

## Using gruntplugins
Now that we have the `Gruntfile.js` configured and running, we're going to
install some _gruntplugins_. These plugins are reusable libraries distributed
through `npm`.

As part our journey we're going to install two plugins, one that will _stage_
the files we're going to build, and another that cleans the previous staged files.

- On the terminal/console, run `npm install --save-dev grunt-contrib-copy`. This will install
a task for copying files around.

- Open the `Gruntfile.js` and get rid of everything in there, so it looks

```js
module.exports = function(grunt) {

};
```

- First step for including the task we just installed is adding the `grunt.initConfig({});`
function call. This method takes the configuration for each one of the tasks, mostly under task-named
properties, but may contain any arbitrary data. As long as properties don't conflict with properties
your tasks require.

- Now we need to tell Grunt to load the task we just installed from an npm package, to do so we
will be adding the following line right after `grunt.initConfig({});`

```js
grunt.loadNpmTasks('grunt-contrib-copy');
```

- Finally we're going to include the configuration for the _copy_ task, with a _release_ target. **Copy
is a multi-target task, this means that it can have multiple configurations, defined using arbitrarily
named and we can tell grunt which configuration should use using that name**. In the end it
should look like this:

```js
grunt.initConfig({
	copy: {
		release: {
			files: [ { src: './**', dest: './dist/' } ]
		}
	}
});
```

- Let's test by calling `grunt copy`, if everything goes as expected you should see

```sh
$ grunt copy
Running "copy:release" (copy) task
Created 221 directories, copied 909 files

Done, without errors.
```

- Before going forward, as dist will be our build working folder we better take it our from our
source control. To do so open the `.gitignore` file and add the following lines at the end:

```
# Build artifacts folder
dist
```

- Now, try to run `grunt`, as you will see, it fails with the following error

```
$ grunt
Warning: Task "default" not found. Use --force to continue.

Aborted due to warnings.
```

- Let's define our default target by creating an alias of the `copy` task but named default. At the
end of the file include the following line.

```
grunt.registerTask('default', ['copy']);
```

- Now let's run `grunt` again and the output should be like this

```
$ grunt
Running "copy:release" (copy) task
Created 442 directories, copied 1818 files

Done, without errors.
```

- **Did you notice something strange?** As we copy the files to work on, we're recursively copying
the `dist` folder which will make it really heavy. Let's fix that by adding a new _gruntplugin_ that
gets rid of the `dist` folder at the begining of the process.


- On the terminal/console, run `npm install --save-dev grunt-contrib-clean`. This will install a task
for cleaning up before starting (just in case there's something old there).

- Now let's include the task on the `Gruntfile.js`, right after the
`grunt.loadNpmTasks('grunt-contrib-copy');`,  as follows:

```
grunt.loadNpmTasks('grunt-contrib-clean');
```

- Now let's configure the clean task with following snippet, to be included before the
copy configuration:

```
clean: ['./dist'],
```

- And finally, let's change our `default` task definition so it runs `clean` before `copy`, it
should end up looking like this:

```
grunt.registerTask('default', ['clean', 'copy']);
```

- Run `grunt`, twice, and you will see that the result is always the same

```
$ grunt
Running "clean:0" (clean) task
Cleaning ./dist...OK

Running "copy:release" (copy) task
Created 231 directories, copied 934 files

Done, without errors.
```

- If you want to, you can run `grunt clean` and it will just remove the `dist` folder
without performing any further task.


```
$ grunt clean
Running "clean:0" (clean) task
Cleaning ./dist...OK

Done, without errors.
```

## Optimizing the way we load tasks
As you've seen loading tasks requires you to load each task one by one,
which is unnecessarily cumbersome. Luckily, there's a module that automates that
so every added npm modules becomes _usable_ without explicit inclusion.

The module we will be using reads the dependencies/devDependencies/peerDependencies
in your package.json and load grunt tasks that match the provided patterns.

- On the terminal/console, run `npm install --save-dev load-grunt-tasks`.

- Open your `Gruntfile.js`, delete all the `grunt.loadNpmTasks(...)` lines

- Before the default task declaration include the following line

```
require('load-grunt-tasks')(grunt);
```

- Run `grunt`, and voila! no more _cumbersome-always-forgotten_ registration.

## Excercise completed
As you completed this excercise, commit and push you stuff back to your github repository.

