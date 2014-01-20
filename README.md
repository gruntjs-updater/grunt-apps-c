#grunt-apps-c

CoffeeScript, JavaScript, Eco, Mustache as CommonJS/1.1 Modules. AMD/CommonJS/window external interface.

[ ![Codeship Status for radekstepan/grunt-apps-c](https://www.codeship.io/projects/7c42c200-2543-0131-75e4-3aa0f2c98596/status?branch=master)](https://www.codeship.io/projects/8915)

##Quick start

Example `Gruntfile`:

```coffeescript
module.exports = (grunt) ->
    grunt.initConfig
        pkg: grunt.file.readJSON("package.json")
        
        apps_c:
            commonjs:
                src: [ 'src/**/*.{coffee,js,eco,mustache}' ]
                dest: 'build/app.js'
                options:
                    main: 'src/index.js'
                    name: 'MyApp'

    grunt.loadNpmTasks('grunt-apps-c')

    grunt.registerTask('default', [ 'apps_c' ])

```

You can now include the `build/app.js` file and, depending on your surrounding environment, you will be able to load it using RequireJS/AMD, CommonJS or straight from `window` under the `MyApp` key.

##Config

The `options.main` property specifies which file will be considered the "main" one for your package. Somehow, the external world needs to know what to get when they call `require(package_name)`. If you do not specify this property the following actions are taken:

1. We try make use of the property `main` as specified in your app's `package.json` file. Failing that, we...
1. try to find the `index.[js|coffee]` file that is closest to the root of your sources.

The `options.name` overrides the name of the package in `package.json`. It specified the name of the exported package as in: `require(name)`. One can pass in an array of names, as alternatives, as well.

##Types

The filetypes you can use (handlers) are listed in `tasks/`. The base filename of a handler represents the extension we match against.

###Templates

####Eco

Eco templates come precompiled so when you require them, you get back a function that accepts a `context` and returns a string back that you inject into DOM.

An example:

```javascript
var template = require('./templates/layout');
$('body').html(template({ 'name': 'Radek' }));
```

####Mustache

Mustache templates are transpiled into an exported string, so you need to use a library to compile it into a template that can actually be rendered.

For example, to use them inside [canJS](http://canjs.com/guides/Mustache.html) you can do the following:

```javascript
var template = require('./templates/layout');
can.view.mustache('layout', template);
```

##CommonJS/1.1 Modules

The following template wraps your modules:

```javascript
// filename.coffee
require.register('package/path.js', function(exports, require, module) {
    // ...
});
```

##Changelog

####0.1.11-1

- Showing line number when error is thrown in a task.

####0.1.10

- Support for [Mustache](http://mustache.github.io/) logic-less templates. Templates are transpiled into an exported string so you need to then feed it into your particular implementation like [canJS](http://canjs.com/guides/Mustache.html).

##Used by

1. [intermine-apps-c](https://github.com/intermine/intermine-apps-c) - collection of InterMine apps
1. [pomme.js](https://github.com/radekstepan/pomme.js) - JS frames communication
1. [deadmonton](https://github.com/radekstepan/deadmonton) - visualizing crime in Edmonton
1. [github-burndown-chart](https://github.com/radekstepan/github-burndown-chart)