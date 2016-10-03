#MEAN Session Three

##A Simple Gulp Workflow

Today we abandon Koala and enable SASS and browser refresh using Gulp. We will be using [Node's Package Manager](https://www.npmjs.com/) (NPM) extensively. Make sure that Node, Ruby and GIT are installed first. 

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

```js
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

```js
var gulp = require('gulp');
var sass = require('gulp-sass');

gulp.task('sass', function(){
	return gulp.src('scss/styles.scss')
    .pipe(sass())
    .pipe(gulp.dest('app/css'))
});

```
Run `$ gulp sass` in terminal.

Add SASS options as a viariable and reference them in the pipe:
```js
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

```js
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

```
gulp.task('serve', ['sass'], function(){
	gulp.watch('scss/**/*.scss', ['sass']) 
});
```

Run `$ gulp serve` and note that the process continues to run. However, if we make an error in our SASS file the process stops and we need to retart. Fix this by adding error handling to the sass pipe. Now, even if we make an error, the watch continues to run and the error is reported in the terminal.

```
...
.pipe(sass(sassOptions).on('error', sass.logError))
```

Test this by intentionally creating an error in the sass file. The error should be reported by the watch continues to run.

We should store some of the paths in variables and use them in our statements. This will make it easier to reuse our gulpfile in other projects:
```js
var sassSources = './scss/**/*.scss';
var sassOutput = './app/css';
var htmlSource = 'app/**/*.html';
```

###Adding Browser Refresh

Globally install npm browser-sync 
```sudo npm install -g browser-sync``` 

and then as a dev dependency 

```$ npm install browser-sync --save-dev``` 

and create a variable container for its methods and functions:

```js
var gulp = require('gulp');
var browserSync = require('browser-sync').create();
var sass = require('gulp-sass');
var sourcemaps = require('gulp-sourcemaps');
```
Its worth taking a moment to check out the features and gulp instructions at [Browser Sync](https://www.browsersync.io/docs/gulp)

Edit our gulp task for syncing:
```js
gulp.task('serve', ['sass'], function() {

    browserSync.init({
        server: "./app"
    });

    gulp.watch(sassSources, ['sass']);
    gulp.watch(htmlSource).on('change', browserSync.reload);
});
```
and edit our sass task to use the reload method:
```js
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

Add a .gitignore file in sushi so that all the node_modules do not push. 

```
node_modules
sass-cache
.DS_Store
```

Here is the complete gulpfile:
```js
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

In a new terminal cd to the top level of today's files - run `ls -al` and remove any existing .git repos `rm -rf .git` Initialize a repo for the folder and push to a new github repo.

##JavaScript DOM Scripting

Note the directory structure. Examine the `package.json` file in the `scripting` directory. Note the main and start entries.

Examine the `app.js` file in the app directory. Run this in the terminal:

```sudo npm install```
```node app/app.js```

###The DOM

```html
<a onclick="showPicOne(this); return false;" href="img/pagoda.jpg" title="Temple by a river">Pagoda</a>
```
Create scripts.js in the public/js directory and link to it.

```js
function showPicOne(whichpic) {
  console.log(whichpic);
}
```

* `document.getElementById(string id)` returns a reference to the element with the specified ID.
* `document.getElementsByTagName(string tagName)` returns a collection (array) of all elements of the specified tag name.
* `getAttribute(string name)` returns the value of the attribute specified by the name argument
* `setAttribute(string name, string value)` adds a new attribute of the specified name and value, or sets the existing attribute to the specified value.
* `nodeValue` a DOM property that can get and set the value of a node.

```js
function showPicOne(whichpic) {
  var source = whichpic.getAttribute("href");
  console.log(source);
}

function showPicOne(whichpic) {
  var source = whichpic.getAttribute("href");
  var placeholder = document.getElementById("placeholder");
  console.log(placeholder);
}

function showPicOne(whichpic) {
  var source = whichpic.getAttribute("href");
  var placeholder = document.getElementById("placeholder");
  placeholder.setAttribute("src",source);
}

function showPicOne(whichpic) {
  var source = whichpic.getAttribute("href");
  var placeholder = document.getElementById("placeholder");
  placeholder.setAttribute("src",source);
  var text = whichpic.getAttribute("title");
  var description = document.getElementById("description");
  console.log(description.childNodes[0].nodeValue); 
}
```
A common mistake is to think that the text inside an element is that element's node value, but that isn't the case. The element itself has no node value — it has a child text node, and that has a node value equal to its content.

Thus, `description.childNodes[0].nodeValue` gets the first child node of the element with the id of "description"

```js
function showPicOne(whichpic) {
  var source = whichpic.getAttribute("href");
  var placeholder = document.getElementById("placeholder");
  placeholder.setAttribute("src",source);
  var text = whichPic.getAttribute("title");
  var description = document.getElementById("description");
  description.firstChild.nodeValue = text; 
}
```

Remove the onclick and add `<body onLoad="prepareGallery()">` 

```js
function prepareGallery() {
  var gallery = document.getElementById("imageGallery");
  var links = gallery.getElementsByTagName("a");
  console.log(links.length);
  for ( var i=0; i < links.length; i++ ) {
    console.log(i);
    links[i].onclick = function() {
    showPicOne(this);
    return false;
  }
 }
}
```

Remove `onLoad="prepareGallery()"` and add:
```js
window.onload = function() {
  prepareGallery();
}
```
###Working with CSS

Add .active class to the first list item (prepareGallery):
```js
...
    return false;
  }
  links[0].parentNode.className = 'active';
  showPicOne(links[0]);
 }
...
```
Pass the links variable to showPicOne (prepareGallery):
```js
...
    links[i].onclick = function() {
        showPicOne(this, links);
        return false;
  }
...
```
Edit (showPicOne) to receive the links and use DOM traversal to set the class of the enclosing <li> element:

```js
function showPicOne(whichPic, links){
    if(links){
        for ( var i=0; i < links.length; i++ ) {
            links[i].parentNode.className = '';
        }
    }
    whichPic.parentNode.className = 'active';
...
```

##Homework

1. add a image gallery viewer to last week's homework
2. add a gulp-based workflow to last week's homework

##Reading

[Sitepoint on gulp workflows](https://www.sitepoint.com/simple-gulpy-workflow-sass/) - might be worth a look, didn't work for me. Maybe too old.

[CSS Tricks on gulp workflows](https://css-tricks.com/gulp-for-beginners/) - a better workflow tutorial

Dickey - Write Modern Web Apps with the MEAN Stack: Mongo, Express, AngularJS and Node.js, chapters 1-2

[Mozilla on DOM Scripting](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)




