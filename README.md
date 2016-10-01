#MEAN Session Three

##SVG



##A Simple Gulp Workflow

Here we abandon Koala and enable SASS and  browser refresh using Gulp. We will also be using [Node's Package Manager](https://www.npmjs.com/) (NPM) extensively. Make sure that Node, Ruby and GIT are installed first. 

Install [Gulp](https://www.npmjs.com/package/gulp) globally for all users:

`$ sudo npm install -g gulp`

If you haven't got SASS installed you will need to run a gem command. Gem is Ruby's version of package management.

`$ sudo gem install sass`

CD into the working directory and initialize npm. Accept the default settings (you can change them later by editing the resulting package.json). 

`$ npm init`

[What is JSON?](https://en.wikipedia.org/wiki/JSON)

Now install gulp as a development dependency for the project and examine package.json afterwards. Also note the addition of a node_modules directory in your project. 

`$ sudo npm install --save-dev gulp`

Typically we add a .gitignore file which instructs git not to include these files when pushing to github. They are re-installed by running `$ npm install`.

Create a gulpfile.js file at the root level of the project.

```
var gulp = require('gulp');
gulp.task('test', function() {
  console.log('testing 1 2 3');
});

```

"require" tells node to look in node_modules for a package and assigns its methods to a variable called gulp. "task" is one such method.

`$ gulp test`

###Adding gulp pipes for SASS

`$ sudo npm install --save-dev gulp-sass`

[Documentation](https://github.com/dlmanning/gulp-sass)


Create the gulp task

```
var gulp = require('gulp');
var sass = require('gulp-sass');

gulp.task('sass', function(){
	return gulp.src('scss/styles.scss')
    .pipe(sass())
    .pipe(gulp.dest('app/css'))
});

```
Run `$ gulp sass` in terminal.

Add SASS options and use them in the pipe:
```
var sassOptions = {
  errLogToConsole: true,
  outputStyle: 'expanded'
};
...
.pipe(sass(sassOptions))

```
Add sourcemaps and error display

`$ sudo npm install --save-dev gulp-sourcemaps`

[Docs](https://github.com/floridoo/gulp-sourcemaps)

```
var gulp = require('gulp');
var sass = require('gulp-sass');
var sourcemaps = require('gulp-sourcemaps');

gulp.task('sass', function(){
	return gulp.src('scss/styles.scss')
	.pipe(sourcemaps.init())
    .pipe(sass(sassOptions))
    .pipe(sourcemaps.write('.'))
    .pipe(gulp.dest('app/css'))
});
```

Run `$ gulp sass` in terminal

```
gulp.task('serve', ['sass']);
```

Run `$ gulp` in terminal

```
gulp.task('serve', ['sass'], function(){
	gulp.watch('scss/**/*.scss', ['sass']) 
});
```

Run `$ gulp watch` and note that the process continues to run. However, if we make an error in our SASS file the process stops and we need to retart. Fix this by adding error handling to the sass pipe. Now, even if we make an error, the watch continues to run and the error is reported in the terminal.

```
...
.pipe(sass(sassOptions).on('error', sass.logError))
```

We should store some of the paths in variables and use them in our statements. Something like:
```
var sassSources = './scss/**/*.scss';
var sassOutput = './app/css';
var htmlSource = 'app/**/*.html';
```

###Adding Browser Refresh

Globally install npm browser-sync `sudo npm install -g browser-sync` (opt) and then as a dev dependency `$ npm install browser-sync --save-dev` and create a variable container for its methods and functions:
```
var gulp = require('gulp');
var browserSync = require('browser-sync').create();
var sass = require('gulp-sass');
var sourcemaps = require('gulp-sourcemaps');
```
Its worth taking a moment to check out the features and gulp instructions at [Browser Sync](https://www.browsersync.io/docs/gulp)

Edit our gulp task for syncing:
```
gulp.task('serve', ['sass'], function() {

    browserSync.init({
        server: "./app"
    });

    gulp.watch(sassSources, ['sass']);
    gulp.watch(htmlSource).on('change', browserSync.reload);
});
```
and edit our sass task to use the reload method:
```
gulp.task('sass', function() {
    return gulp.src(sassSources)
      .pipe(sourcemaps.init())
      .pipe(sass(sassOptions).on('error', sass.logError))
      .pipe(sourcemaps.write('.'))
      .pipe(gulp.dest(sassOutput))
      .pipe(browserSync.stream());
});
```
Let's use it as our default task:
```
gulp.task('default', ['serve']);
```
Run `gulp` in the terminal and test refresh upon editing sass, html and check that errors in your sass are output to the console and do not stop the watch task.

Here is the complete gulpfile:
```
var gulp = require('gulp');
var browserSync = require('browser-sync').create();
var sass = require('gulp-sass');
var sourcemaps = require('gulp-sourcemaps');

var sassOptions = {
  errLogToConsole: true,
  outputStyle: 'expanded'
};

var sassSources = './scss/**/*.scss';
var sassOutput = './app/css';
var htmlSource = 'app/**/*.html';

gulp.task('serve', ['sass'], function() {

    browserSync.init({
        server: "./app"
    });

    gulp.watch(sassSources, ['sass']);
    gulp.watch(htmlSource).on('change', browserSync.reload);
});

gulp.task('sass', function() {
    return gulp.src(sassSources)
    .pipe(sourcemaps.init())
    .pipe(sass(sassOptions).on('error', sass.logError))
    .pipe(sourcemaps.write('.'))
    .pipe(gulp.dest(sassOutput))
    .pipe(browserSync.stream());
});

gulp.task('default', ['serve']);
```

##GIT and GITHUB

Initialize a repo for this and add a .gitignore file so that all the node_modules do not push.
```
node_modules
sass-cache
.DS_Store
```


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




