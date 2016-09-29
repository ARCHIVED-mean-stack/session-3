#MEAN Session Three

##A Simple Gulp Workflow

Here we abandon Koala and enable automatic browser refresh. Make sure that Node, Ruby and GIT are installed.

Install [Gulp] globally for all users: 
`sudo npm install -g gulp`

`sudo gem install sass`

`npm init`

`sudo npm install --save-dev gulp`

Create a gulpfile.js at the root level

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




