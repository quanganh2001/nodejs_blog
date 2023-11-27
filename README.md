# Setup nodejs
Type `npm init`
# Install express
Type `npm install express`
# Install nodemon and inspector
Type `npm install nodemon --save-dev`

It will be add in devDependencies

Add scripts:
```json
"start": "nodemon --inspect index.js",
```

# Install morgan
Type `npm i morgan --save-dev`

Add scripts:
```js
const morgan = require('morgan')

app.use(morgan('combined'))
```
# Template engine (handlebars)
Type: `npm i express-handlebars@5.1.0`
## Restructure
Move `index.js` to src folder, then change file run in `package.json`:
```json
"main": "src/index.js",
"scripts": {
	"start": "nodemon --inspect src/index.js",
	"test": "echo \"Error: no test specified\" && exit 1"
},
```
Add scripts to index.js:
```js
const handlebars = require('express-handlebars');

// Template engine
app.engine('handlebars', handlebars());
app.set('view engine' , 'handlebars');
```
Add path require and path folder :

```js
const path = require('path');

app.set('views', path.join(__dirname, 'resources/views'));
```

It was error: `Error: ENOENT: no such file or directory, open 'nodejs/blog/src/resources/views/layouts/main.handlebars'`, so you should create `layouts/main.handlebars` in views folder

```html
<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<meta name="viewport" content="width=device-width, initial-scale=1.0">
		<title>Document</title>
	</head>
	<body>
		<div class="app">
			<header>
				<h1>HEADER</h1>
			</header>

			<div class="container">
				{{{body}}}
			</div>

			<footer>
				<h1>FOOTER</h1>
			</footer>
		</div>


	</body>
</html>
```
Add more get:

```js
app.get('/news', (req, res) => {
	res.render('news');
})
```

Then create `news.handlebars` in views folder.
## Config filename
```js
// Template engine
app.engine('hbs', handlebars({
	extname: '.hbs'
}));
app.set('view engine' , 'hbs');
```
## Add partial
Add partials in src/resources/views folder then create `footer.hbs`, `header.hbs`
```html
<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<meta name="viewport" content="width=device-width, initial-scale=1.0">
		<title>Document</title>
	</head>
	<body>
		<div class="app">
			{{> header}}

			<div class="container">
				{{{body}}}
			</div>

			{{> footer}}
		</div>


	</body>
</html>
```
# Static files and SCSS
First, download and save to [image](src/public/img/logo.png)

So let's config
```js
app.use(express.static(path.join(__dirname, 'public')));
```
## Install node-sass
```
npm install node-sass --save-dev
```

Add folder src/public/css

## Usage
Usage: `node-sass [options] <input> [output]`. Add script to package.json

Example: 
```js
"scripts": {
	"start": "nodemon --inspect src/index.js",
	"watch": "node-sass src/resources/scss/app.scss src/public/css/app.css",
	"test": "echo \"Error: no test specified\" && exit 1"
},
```

It will be complie to css file.

Command: `npm run watch`. To watch a change file to complier, using `--watch`
```txt
node-sass --watch src/resources/scss/app.scss src/public/css/app.css
```
It will be watch and no exit.

Let's import css folder
```html
<link rel="stylesheet" href="/css/app.css">
```

Now let's create variables scss file
```scss
$red-color: red;
```
In app.scss file import scss:
```scss
@import 'variables';
```
As you can see, because watch is app.scss, the default watching extensions nodemon are: js,mjs,cjs,json so let's create json file to watch file types in original folder: `nodemon.json`
```json
{
    "ext": "js json"
}
```
To watch folder, using command:
```json
"watch": "node-sass --watch src/resources/scss/ --output src/public/css/",
```
# Add bootstrap 4
Link CSS:
```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
```
Add scripts:
```html
<script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/popper.js@1.12.9/dist/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>
```
Here’s an example of all the sub-components included in a responsive light-themed navbar that automatically collapses at the lg (large) breakpoint.
```html
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <a class="navbar-brand" href="#">Navbar</a>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>

  <div class="collapse navbar-collapse" id="navbarSupportedContent">
    <ul class="navbar-nav mr-auto">
      <li class="nav-item active">
        <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Link</a>
      </li>
      <li class="nav-item dropdown">
        <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
          Dropdown
        </a>
        <div class="dropdown-menu" aria-labelledby="navbarDropdown">
          <a class="dropdown-item" href="#">Action</a>
          <a class="dropdown-item" href="#">Another action</a>
          <div class="dropdown-divider"></div>
          <a class="dropdown-item" href="#">Something else here</a>
        </div>
      </li>
      <li class="nav-item">
        <a class="nav-link disabled" href="#">Disabled</a>
      </li>
    </ul>
    <form class="form-inline my-2 my-lg-0">
      <input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search">
      <button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
    </form>
  </div>
</nav>
```
# Query parameters
Example: https://www.google.com/search?q=f8 lap trinh&ref=f8&author=sondn

Let's go to build query search parameters, add search and render search page
```js
app.get('/search', (req, res) => {
	res.render('search');
});
```

As you can see, when I go to search page, it was error, so fix is create search view in public/resources/views folder

So how to get query?

Press F12 to go Developer Tools, click to Nodejs icon, then debug search

So let's comment morgan require package, print query
```js
app.get('/search', (req, res) => {
	console.log(req.query);
	res.render('search');
});
```
Output:
```txt
{ q: 'f8 lap trinh', ref: 'mycv', author: 'sondn' }
```
# Form default behavior
## Create form
Using form bootstrap
```html
<div class="mt-4">
    <form>
        <div class="form-group">
            <label for="search-input">Nhập từ khóa</label>
            <input type="text" name="q" class="form-control" id="search-input" placeholder="Nhập từ khóa">
        </div>

        <button type="submit" class="btn btn-primary">Tìm kiếm</button>
    </form>
</div>
```
When you submit form, it will be create query parameters with GET method default
```html
<div class="mt-4">
    <form>
        <div class="form-group">
            <label for="search-input">Nhập từ khóa</label>
            <input type="text" name="fullname">
            <input type="text" name="age">
            <input type="text" name="q" class="form-control" id="search-input" placeholder="Nhập từ khóa">
        </div>

        <button type="submit" class="btn btn-primary">Tìm kiếm</button>
    </form>
</div>
```
In this case, if you don't input and click search, it still send parameters with nothing.

## How to want POST method?
Add POST method code:
```js
app.post('/search', (req, res) => {
	res.render('search');
});
```
Then reload again, it will be send POST method

## Send to news page
So if you don't want submit search page, let's submit to news page
```html
<div class="mt-4">
    <form method="GET" action="/news">
        <div class="form-group">
            <label for="search-input">Nhập từ khóa</label>
            <input type="text" name="q" class="form-control" id="search-input" placeholder="Nhập từ khóa">
        </div>

        <button type="submit" class="btn btn-primary">Tìm kiếm</button>
    </form>
</div>
```
Console to output
```js
app.get('/news', (req, res) => {
	console.log(req.query.q);
	res.render('news');
});
```
## Using middleware
Add gender:
```hbs
<div class="mt-4">
    <form method="POST" action="">
        <div class="form-group">
            <label for="search-input">Nhập từ khóa</label>
            <input type="text" name="q" class="form-control" id="search-input" placeholder="Nhập từ khóa">
        </div>

        <div class="form-group">
            <label for="gender-input">Nhập từ khóa</label>
            <select name="gender" class="form-control" id="gender-input">
                <option value="">-- Chọn giới tính --</option>
                <option value="male">Nam</option>
                <option value="female">Nữ</option>
            </select>
        </div>

        <button type="submit" class="btn btn-primary">Tìm kiếm</button>
    </form>
</div>
```
Add more imports:
```js
const path = require('path');
const express = require('express');
const morgan = require('morgan');
const handlebars = require('express-handlebars');
const app = express();
const port = 3000;

app.use(express.static(path.join(__dirname, 'public')));

app.use(express.urlencoded({
	extended: true
}));
app.use(express.json());
```
Add new blank post:
```js
app.post('/search', (req, res) => {
	res.send('');
});
```
# Routes & Controllers
Create folder: src/routes, src/app/controllers
## Routes
Create file: src/routes/news.js, src/app/controllers/NewsController.js
## Controllers
Let's set class at the same name <br> and add GET method
**src/app/controllers/NewsController.js**
```js

class NewsController {

	// [GET] /news
	index(req, res) {
		res.render('news');
	}

}

module.exports = new NewsController;
```
It will be add instance and export out. <br>
Let's set configure route for news <br>
**src/index.js**
```js
const route = require('./routes');
// Routes init
route(app);
```
**src/routes/index.js**
```js

function route(app) {

	app.get('/', (req, res) => {
		res.render('home');
	});
	
	app.get('/news', (req, res) => {
		res.render('news');
	});
	
	app.get('/search', (req, res) => {
		res.render('search');
	});
	
	app.post('/search', (req, res) => {
		res.send('');
	});

}

module.exports = route;
```
**src/routes/news.js**
```js
const express = require('express');
const router = express.Router();

const newsController = require('../app/controllers/NewsController');

// newsController.index

router.use('/', newsController.index);

module.exports = router;
```
Add news.js <br>
**src/index.js**
```js
const newsRouter = require('./news');

function route(app) {

	app.use('/news', newsRouter);

	app.get('/', (req, res) => {
		res.render('home');
	});
	
	app.get('/search', (req, res) => {
		res.render('search');
	});
	
	app.post('/search', (req, res) => {
		res.send('');
	});

}

module.exports = route;
```
Add slug PATH variable <br>
**src/app/controllers/NewsController.js**
```js
// [GET] /news/:slug
show(req, res) {
  res.send('NEWS DETAIL!!!');
}
```
**src/routes/news.js**
```js
router.use('/:slug', newsController.show);
```
Copy news.js and change to site.js, copy NewsController.js and change to SiteController.js <br>
**src/routes/site.js**
```js
const express = require('express');
const router = express.Router();

const siteController = require('../app/controllers/SiteController');

router.use('/search', siteController.search);
router.use('/', siteController.index);

module.exports = router;
```
**src/app/controllers/SiteController.js**
```js

class SiteController {

	// [GET] /
	index(req, res) {
		res.render('home');
	}

	// [GET] /search
	search(req, res) {
		res.render('search');
	}

}

module.exports = new SiteController;
```
Update: **src/routes/index.js**
```js
const newsRouter = require('./news');
const siteRouter = require('./site');

function route(app) {

	app.use('/news', newsRouter);

	app.use('/', siteRouter);

}

module.exports = route;
```
# Install MongoDB
Go to: https://www.mongodb.com/try/download/community -> Select package and choose package file type is msi.

Set URI: `127.0.0.1:27017`
# Prettier, lint-staged and husky
Install: `npm i prettier lint-staged husky --save-dev`
## Prettier
To use CLI, type command: `prettier --single-quote --trailing-comma all --tab-width 4 --write 'src/**/*.{js,json,scss}'`
- `--single-quote`: Change double quote to single quote
- `--trailing-comma`: Add comma
- `--tab-width 4`: Tab width is 4
## Lint-staged
Command: 
```json
"lint-staged": {
	"src/**/*.{js,json,scss}": "prettier --single-quote --trailing-comma all --tab-width 4 --write"
},
```
It run when add file to git
## Husky
Command: 
```json
"lint-staged": {
	"src/**/*.{js,json,scss}": "prettier --single-quote --trailing-comma all --tab-width 4 --write"
},
```