#MEAN Session Three

##A Simple Gulp Workflow

Here we abandon Koala and enable automatic browser refresh. We will be using [Node's Package Manager](https://www.npmjs.com/) (NPM) extensively. Make sure that Node, Ruby and GIT are installed first. 

Install [Gulp](https://www.npmjs.com/package/gulp) globally for all users:

`$ sudo npm install -g gulp`

If you haven't got SASS installed you will need to run a gem command. Gem is Ruby's version of package management.

`$ sudo gem install sass`

CD into the working directory and initialize npm. Accept the default settings (you can change them later by editing the resulting package.json). 

`$ npm init`

[What is JSON?](https://en.wikipedia.org/wiki/JSON)

Now install gulp as a development dependency for the project and examine package.json afterwards. You will also note the addition of a node_modules directory in your project. 

`sudo npm install --save-dev gulp`

Typically we add a .gitignore file which instructs git not to include these files when pushing to github. They are re-installed by running `$ npm install`.

Create a gulpfile.js file at the root level of the project.

```
var gulp = require('gulp');
gulp.task('log', function(){
  
});

```
`$ sudo npm install --save-dev gulp-util`

```
var gulp = require('gulp'),
    gutil = require('gulp-util');

gulp.task('log', function(){
  gutil.log('Gulp is awesome');
});

```
`$ gup log`

Adding gulp pipes for SASS

`$ sudo npm install --save-dev gulp-ruby-sass`

[Documentation](https://github.com/sindresorhus/gulp-ruby-sass)


Create the gulp task

```
var gulp = require('gulp'),
gutil = require('gulp-util'),
sass = require('gulp-ruby-sass');

gulp.task('sass', function(){
  sass('./scss/styles.scss')
  .pipe(sourcemaps.write())
  .pipe(gulp.dest('./app/css'))
});

```
Add sourcemaps and error display

`$ sudo npm install --save-dev gulp-sourcemaps`

[Docs](https://github.com/floridoo/gulp-sourcemaps)

```
var gulp = require('gulp'),
    gutil = require('gulp-util'),
    sourcemaps = require('gulp-sourcemaps'),
    sass = require('gulp-ruby-sass');

var sassSources = ['./scss/styles.scss'];

gulp.task('sass', function(){
  sass(sassSources, {sourcemap: true})
  .on('error', sass.logError)
  .pipe(sourcemaps.write())
  .pipe(sourcemaps.write('maps', {
    includeContent: false,
    sourceRoot: 'source'
  }))
  .on('error', gutil.log)
  .pipe(gulp.dest('./app/css'))
});

```

Run `$ gulp sass` in terminal

```
gulp.task('default', ['sass']);
```

Run `$ gulp` in terminal

```
gulp.task('watch', function(){
  gulp.watch(sassSources, [sass]);
});
```

`$ gulp watch`

  
##GIT and GITHUB

##SVG

###SASS responsive design of new navbar with SVG


##Second Page

###Static Pages
* static app - anchor in browser > request to server > database > html > browser
* AJAX - ability to refresh data after the app is running (mapquest vs google maps)
* Progressive enhancement - a trap?
* Spaghetti Javascript - code modifies existing DOM vs code that creates the DOM
* Mobile native (receives json) vs browser native (receives html)

###Thick client
* Modern web architecture - browser requests html > page contains JS that builds the base structure > ajax loads content via json > JS modifies DOM to present content.
* Benefits - same code for mobile and web, performance leverages the client (distributed), standardization on JS, html and css, speed and increased modularity.
* Routing

##Homework

1. 

##Reading

[Sitepoint on gulp workflows](https://www.sitepoint.com/simple-gulpy-workflow-sass/)

[CSS Tricks on gulp workflows](https://css-tricks.com/gulp-for-beginners/)




