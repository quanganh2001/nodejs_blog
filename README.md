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
"beautiful": "lint-staged",
"husky": {
	"hooks": {
		"pre-commit": "lint-staged"
	}
},
```
# MVC Model
Create database in MongoDB: `f8_education_dev`, collection name: `courses`
## Install mongoose
Type: `npm install mongoose` and `npm install mongodb`
## Connect to DB
Create file: `src/config/db/index.js`
```js
const mongoose = require('mongoose');

async function connect() {
	try {
		await mongoose.connect('mongodb://127.0.0.1:27017/f8_education_dev', {
			useNewUrlParser: true,
			useUnifiedTopology: true
		});
		console.log('Connection successfully!');
	} catch (error) {
		console.log('Connection failure!!!');
	}
}

module.exports = { connect };
```
Sometimes you should wait 30 seconds to log connection failed, otherwise connection successfully!
**src/index.js**
```js
const route = require('./routes');
const db = require('./config/db');

// Connect to DB
db.connect();
```
## Create a model
Create file: `src/app/models/Course.js`, accessing a model
```js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const Course = new Schema({
	name: { type: String, maxLength: 255 },
	description: { type: String, maxLength: 600 },
	image: { type: String, maxLength: 255 },
	createdAt: { type: Date, default: Date.now },
	updatedAt: { type: Date, default: Date.now },
});

module.exports = mongoose.model('Course', Course);
```
## Exports data
**src/app/controllers/SiteController.js**
```js
const Course = require('../models/Course');

class SiteController {
    // [GET] /
    index(req, res) {

        Course.find({}, function (err, courses) {
            if (!err) {
                res.json(courses);
            } else {
                res.status(400).json({ error: 'ERROR!!!' });
            }
        });

        // res.render('home');
    }

    // [GET] /search
    search(req, res) {
        res.render('search');
    }
}

module.exports = new SiteController();
```
# Install JSON Viewer Extension
You can search json viewer and install extension now.
## Route methods
Fix to GET methods

**src/routes/site.js**
```js
router.get('/search', siteController.search);
router.get('/', siteController.index);
```
**src/routes/news.js**
```js
router.get('/:slug', newsController.show);
router.get('/', newsController.index);
```
## App listen log
**index.js**
```js
app.listen(port, () =>
    console.log(`App listening on port http://localhost:${port}`),
);
```
## Resource path
**index.js**
```js
app.set('views', path.join(__dirname, 'resources', 'views'));
```
# Read from DB
## Promises
**src/app/controllers/SiteController.js**
```js
const Course = require('../models/Course');

class SiteController {
    // [GET] /
    index(req, res, next) {
        Course.find({})
            .then(courses => res.render('home'))
            .catch(next);
    }

    // [GET] /search
    search(req, res) {
        res.render('search');
    }
}

module.exports = new SiteController();

```
It will be render homepage.
## Fix UI page
Keyword this represent element
```hbs
<div class="mt-4">
	<div class="row">

		{{#each courses}}
		<div class="col-sm-6 col-lg-4">
			<div class="card">
				<img class="card-img-top" src="..." alt="Card image cap">
				<div class="card-body">
					<h5 class="card-title">Card title</h5>
					<p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
					<a href="#" class="btn btn-primary">Go somewhere</a>
				</div>
			</div>
		</div>
		{{/each}}

	</div>
</div>
```
**src/resources/scss/app.scss**
```scss
@import 'variables';

.card-course-item {
	margin-top: 16px;
}
```
## Render name
Starting from Handlebars version 4.6 has a security update, so this keyword is a document object by MongoDB created. 

_So how to fix this error?_

Method 1: You can downgrade handlebars version to 4.5.0, go to devDepencies and run `npm install` again.
```json
"devDependencies": {
	"express-handlebars": "^4.5.0",
	"husky": "^8.0.3",
	"lint-staged": "^15.1.0",
	"morgan": "^1.10.0",
	"node-sass": "^9.0.0",
	"nodemon": "^3.0.1",
	"prettier": "^3.1.0"
}
```
Method 2: Edit data using `toObject` method to return literal object.

**src/app/controllers/SiteController.js**
```js
class SiteController {
    // [GET] /
    index(req, res, next) {
        Course.find({})
            .then(courses => {
                courses = courses.map(course => course.toObject())
                res.render('home', { courses });
            })
            .catch(next);
    }

    // [GET] /search
    search(req, res) {
        res.render('search');
    }
}
```

But I want to create function to don't repeat this logic, so I create util folder in the src folder, then create `mongoose.js`. There are 2 variables `multipleMongooseToObject` and `mongooseToObject`
```js
module.exports = {
	multipleMongooseToObject: function (mongooses) {
		return mongooses.map(mongoose => mongoose.toObject());
	},
	mongooseToObject: function (mongoose) {
		return mongoose ? mongoose.toObject() : mongoose;
	}
};
```
Then I import `mongoose.js` from `SiteController.js`

**src/app/controllers/SiteController.js**
```js
const { multipleMongooseToObject } = require('../../util/mongoose');

class SiteController {
    // [GET] /
    index(req, res, next) {
        Course.find({})
            .then(courses => {
                res.render('home', { 
                    courses: multipleMongooseToObject(courses)
                });
            })
            .catch(next);
    }

    // [GET] /search
    search(req, res) {
        res.render('search');
    }
}
```
Output:

![Alt text](https://images2.imgbox.com/ef/e0/TEXG9Q8W_o.png)
# Course detail page
## Add slug
**src/resources/views/home.hbs**
```hbs
<div class="mt-4">
	<div class="row">

		{{#each courses}}
		<div class="col-sm-6 col-lg-4">
			<div class="card card-course-item">
				<a href="/courses/{{this.slug}}">
					<img class="card-img-top" src="{{this.image}}" alt="{{this.name}}">
				</a>
				<div class="card-body">
					<a href="/courses/{{this.slug}}">
						<h5 class="card-title">{{this.name}}</h5>
					</a>
					<p class="card-text">{{this.description}}</p>
					<a href="#" class="btn btn-primary">Mua khóa học</a>
				</div>
			</div>
		</div>
		{{/each}}

	</div>
</div>
```
## Create courses routes
**src/routes/courses.js**
```js
const express = require('express');
const router = express.Router();

const courseController = require('../app/controllers/CourseController');

router.get('/:slug', courseController.show);

module.exports = router;

```
**src/app/controllers/CourseController.js**
```js
const Course = require('../models/Course');
const { mongooseToObject } = require('../../util/mongoose');

class CourseController {
    // [GET] /courses/:slug
    show(req, res, next) {
        Course.findOne({ slug: req.params.slug })
            .then(course => 
                res.render('courses/show', { course: mongooseToObject(course) })
            )
            .catch(next);
    }
}

module.exports = new CourseController();

```
**src/routes/index.js**
```js
const newsRouter = require('./news');
const coursesRouter = require('./courses');
const siteRouter = require('./site');

function route(app) {
    app.use('/news', newsRouter);
    app.use('/courses', coursesRouter);

    app.use('/', siteRouter);
}

module.exports = route;

```
Let's create a description courses: a description and a video
```hbs
<div class="mt-4">
	<div class="row">
		<div class="col-lg-3">
			<button>HỌC NGAY</button>
			<ul>
				<li>{{course.level}}</li>
			</ul>
		</div>

		<div class="col-lg-9">
			<h2>{{course.name}}</h2>
			<iframe width="560" height="315" src="https://www.youtube.com/embed/{{course.videoId}}" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
			<p>{{course.description}}</p>
		</div>
	</div>
</div>
```
# Create new course
## Add create method
**src/routes/courses.js**
```js
router.get('/create', courseController.create);
```
**src/app/controller/CourseController.js**
```js
const Course = require('../models/Course');
const { mongooseToObject } = require('../../util/mongoose');

class CourseController {
    // [GET] /courses/create
    create(req, res, next) {
        res.render('courses/create')
    }
}

module.exports = new CourseController();

```
## Create POST methods
Let's create many fields: name, description, videoId will be generate, level.
**src/resources/views/courses/create.hbs**
```hbs
<div class="mt-4">
	<h3>Đăng khóa học</h3>

	<form method="POST" action="/courses/store">
		<div class="form-group">
			<label for="name">Tên khóa học</label>
			<input type="text" class="form-control" id="name" name="name">
		</div>
		<div class="form-group">
			<label for="description">Mô tả</label>
			<input type="text" class="form-control" id="description" name="description">
		</div>
		<div class="form-group">
			<label for="videoId">Video ID</label>
			<input type="text" class="form-control" id="videoId" name="videoId">
		</div>
		<div class="form-group">
			<label for="level">Trình độ</label>
			<input type="text" class="form-control" id="level" name="level">
		</div>

		<button type="submit" class="btn btn-primary">Thêm khóa học</button>
	</form>
</div>
```
**src/app/controller/CourseController.js**
```js
const Course = require('../models/Course');
const { mongooseToObject } = require('../../util/mongoose');

class CourseController {
    // [POST] /courses/store
    store(req, res, next) {
        res.json(req.body);
    }
}

module.exports = new CourseController();

```
**src/routes/courses.js**
```js
router.post('/store', courseController.store);
```
As you can see, it will be send with POST methods and will be render with JSON format.

[![image.png](https://i.postimg.cc/vHjmQGfF/image.png)](https://postimg.cc/ykmKj4Lv)
## Constructing Documents
Let's create constructing documents using save method, if you want to save all documents, you should define everything must to insert it.

**src/app/models/Course.js**
```js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const Course = new Schema({
	name: { type: String, required: true, },
	description: { type: String },
	image: { type: String },
	videoId: { type: String, required: true, },
	level: { type: String },
}, {
	timestamps: true,
});

module.exports = mongoose.model('Course', Course);

```
`name` and `videoId` are required, then create timestamps to autosave time to create and update documents.

Let's change to create videoId to request send to documents.

**src/app/controller/CourseController.js**
```js
const Course = require('../models/Course');
const { mongooseToObject } = require('../../util/mongoose');

class CourseController {
    // [POST] /courses/store
    store(req, res, next) {
        const formData = req.body;
        formData.image = `https://i.ytimg.com/vi/${req.body.videoId}/sddefault.jpg`
        const course = new Course(req.body);
        course.save();

        res.send('COURSE SAVED!');
    }
}

module.exports = new CourseController();

```

[![image.png](https://i.postimg.cc/jjv2YPhv/image.png)](https://postimg.cc/t1Zph1wV)

As you can see, it will be creates timestamp.

Now let's fix to use promise to switch redirect to homepage and add auto slug. Type `npm i mongoose-slug-generator`.

Import plugin and add plugin

**src/app/models/Course.js**
```js
const slug = require('mongoose-slug-generator');

mongoose.plugin(slug);
```
Usage plugin:

**src/app/models/Course.js**
```js
const Course = new Schema({
	name: { type: String, required: true, },
	description: { type: String },
	image: { type: String },
	videoId: { type: String, required: true, },
	level: { type: String },
	slug: { type: String, slug: 'name', unique: true },
}, {
	timestamps: true,
});
```
It will be generated name and delete space. Add property `unique` to check it exist one.

Change navbar link:

**src/resources/views/partials/header.hbs**
```hbs
<li class="nav-item">
	<a class="nav-link" href="/courses/create">Đăng khóa học</a>
</li>
```
# Update course
## Add dropdown
We need to add class `container` and change position to margin-left, then add divider dropdown.
**src/resources/views/partials/header.hbs**
```hbs
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
  <div class="container">
    <a class="navbar-brand" href="/">F8 education</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav ml-auto">
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
            <img src="https://yt3.googleusercontent.com/UsflU74uvka_3sejOu3LUGwzOhHJV0eIYoWcvOfkOre_c12uIN4ys-QqRlAkbusEmbZjTA-b=s176-c-k-c0x00ffffff-no-rj" alt="" class="user-avatar">
            Sơn Đặng
          </a>
          <div class="dropdown-menu" aria-labelledby="navbarDropdown">
            <a class="dropdown-item" href="/courses/create">Đăng khóa học</a>
            <div class="dropdown-divider"></div>
            <a class="dropdown-item" href="/me/stored/courses">Khóa học của tôi</a>
            <a class="dropdown-item" href="/me/stored/news">Bài viết của tôi</a>
            <div class="dropdown-divider"></div>
            <a class="dropdown-item" href="#">Đăng xuất</a>
          </div>
        </li>
      </ul>
    </div>
  </div>
</nav>
```
Change css for avatar image
**src/resources/scss/app.scss**
```scss
.user-avatar {
    width: 28px;
    height: 28px;
    border-radius: 50%;
    margin-right: 4px;
}
```
## Add routes and controller for me
### Add routes
**src/routes/me.js**
```js
const express = require("express");
const router = express.Router();

const meController = require("../app/controllers/MeController");

router.get("/stored/courses", meController.storedCourses);

module.exports = router;

```
### Add controller
**src/app/controllers/MeController.js**
```js
const Course = require('../models/Course');
const { multipleMongooseToObject } = require('../../util/mongoose');

class MeController {
    // [GET] /me/stored/courses
    storedCourses(req, res) {
        res.render("search");
    }
}

module.exports = new MeController();

```
Import router:

**src/routes/index.js**
```js
const newsRouter = require("./news");
const meRouter = require("./me");
const coursesRouter = require("./courses");
const siteRouter = require("./site");

function route(app) {
    app.use("/news", newsRouter);
    app.use("/me", meRouter);
    app.use("/courses", coursesRouter);

    app.use("/", siteRouter);
}

module.exports = route;

```
Output:

[![image.png](https://i.postimg.cc/C1TFf7YH/image.png)](https://postimg.cc/bGgX7QZs)

[![image.png](https://i.postimg.cc/k4Q7NSgr/image.png)](https://postimg.cc/S2K0kJ9V)
## Create UI
We need to create file in the folder `src/resources/views/me/stored-courses.hbs`, then change path `src/app/controllers/MeController.js`
```js
class MeController {
    // [GET] /me/stored/courses
    storedCourses(req, res) {
        res.render("me/stored-courses");
    }
}
```
## Create table to manage courses
We need to config helpers from `express-handlebar` with add sum function return total a + b.

**src/index.js**
```js
// Template engine
app.engine(
    "hbs",
    handlebars({
        extname: ".hbs",
        helpers: {
            sum: (a, b) => a + b,
        }
    }),
);
```
Add table and render all databases, then add sum helpers config.

**src/resources/views/me/stored-courses.hbs**
```hbs
<div class="mt-4">
    <h3>Khóa học của tôi</h3>

    <table class="table mt-4">
        <thead>
            <tr>
                <th scope="col">#</th>
                <th scope="col">Tên khóa học</th>
                <th scope="col">Trình độ</th>
                <th scope="col">Thời gian tạo</th>
            </tr>
        </thead>
        <tbody>
            {{#each courses}}
            <tr>
                <th scope="row">{{sum @index 1}}</th>
                <td>{{this.name}}</td>
                <td>{{this.level}}</td>
                <td>{{this.createdAt}}</td>
            </tr>
            {{/each}}
        </tbody>
    </table>
</div>
```
Add multiple objects:

**src/app/controllers/MeController.js**
```js
class MeController {
    // [GET] /me/stored/courses
    storedCourses(req, res, next) {
        Course.find({})
            .then(courses => res.render("me/stored-courses", {
                courses: multipleMongooseToObject(courses)
            }))
            .catch(next);
    }
}
```
Output:

[![image.png](https://i.postimg.cc/bNcpC327/image.png)](https://postimg.cc/nMkyMK22)
## Create edit and delete buttons
**src/resources/views/me/stored-courses.hbs**
```hbs
<tbody>
    {{#each courses}}
    <tr>
        <th scope="row">{{sum @index 1}}</th>
        <td>{{this.name}}</td>
        <td>{{this.level}}</td>
        <td>{{this.createdAt}}</td>
        <td>
            <a href="/courses/{{this._id}}/edit" class="btn btn-link">Sửa</a>
            <a href="button" class="btn btn-link">Xóa</a>
        </td>
    </tr>
    {{/each}}
</tbody>
```
We need to get target slug link, which courses want to edit

Add edit router get method:

**src/routes/courses.js**
```js
router.get("/:id/edit", courseController.edit);
```

Add edit GET method:

**src/app/controllers/CourseController.js**
```js
// [GET] /courses/edit
edit(req, res, next) {
    res.render('courses/edit');
}
```

Add edit views:

**src/resources/views/courses/edit.hbs**
```hbs
<div class="mt-4">
    <h3>Cập nhật khóa học</h3>

    <form class="mt-4" method="POST" action="/courses/{{course._id}}">
        <div class="form-group">
            <label for="name">Tên khóa học</label>
            <input type="text" class="form-control" value="{{course.name}}" id="name" name="name">
        </div>
        <div class="form-group">
            <label for="description">Mô tả</label>
            <textarea class="form-control" id="description" name="description">{{course.description}}</textarea>
        </div>
        <div class="form-group">
            <label for="videoId">Video ID</label>
            <input type="text" class="form-control" value="{{course.videoId}}" id="videoId" name="videoId">
        </div>
        <div class="form-group">
            <label for="level">Trình độ</label>
            <input type="text" class="form-control" value="{{course.level}}" id="level" name="level">
        </div>
        <button type="submit" class="btn btn-primary">Lưu lại</button>
    </form>
</div>
```

**Important: To take some codes are the same, press Ctrl + Shift + L, to take IDs press Alt + Shift + turn right**

It will be send slug id with POST method

## Fix to send PUT method
Install package: `npm install method-override`

Import package:

**src/index.js**
```js
const methodOverride = require('method-override');
```
### override using a query value
**src/index.js**
```js
app.use(methodOverride('_method'));
```
### Config courses PUT routes
**src/routes/courses.js**
```js
router.put("/:id", courseController.update);
```
Updating databases:

**src/app/controllers/CourseController.js**
```js
// [PUT] /courses/:id
update(req, res, next) {
    Course.updateOne({ _id: req.params.id }, req.body)
        .then(() => res.redirect('/me/stored/courses'))
        .catch(next);
}
```
# Delete course
## Fix warning
As you can see, when I start the project `npm start`, it was warning: 
```txt
(node:6628) [MONGOOSE] DeprecationWarning: Mongoose: the `strictQuery` option will be switched back to `false` by default in Mongoose 7. Use `mongoose.set('strictQuery', false);` if you want to prepare for this change. Or use `mongoose.set('strictQuery', true);` to suppress this warning.
(Use `node --trace-deprecation ...` to show where the warning was created)
```
To fix this warning, add code: `mongoose.set('strictQuery', false);` at `src/config/db/index.js`
## Add modal bootstrap
Let's go to add modal bootstrap to confim do you want to delete this course

**src/resources/views/me/stored-courses.hbs**
```hbs
        <tbody>
            {{#each courses}}
            <tr>
                <th scope="row">{{sum @index 1}}</th>
                <td>{{this.name}}</td>
                <td>{{this.level}}</td>
                <td>{{this.createdAt}}</td>
                <td>
                    <a href="/courses/{{this._id}}/edit" class="btn btn-link">Sửa</a>
                    <a href="" class="btn btn-link" data-toggle="modal" data-target="#delete-course-modal">Xóa</a>
                </td>
            </tr>
            {{/each}}
        </tbody>
        
{{!-- Confirm delete course --}}
<div id="delete-course-modal" class="modal" tabindex="-1" role="dialog">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">Xóa khóa học?</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
        <p>Bạn chắc chắn muốn xóa khóa học này?</p>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Hủy</button>
        <button type="button" class="btn btn-danger">Xóa bỏ</button>
      </div>
    </div>
  </div>
</div>
```
Output:

[![image.png](https://i.postimg.cc/vmqWK3Yr/image.png)](https://postimg.cc/GBD88Pst)
## Add delete method
Let's go to add delete controller to redirect this URL then delete and redirect to homepage.
First, add delete routes

**src/routes/courses.js**
```js
router.delete("/:id", courseController.destroy);
```
**src/app/controllers/CourseController.js**
```js
// [DELETE] /courses/:id
destroy(req, res, next) {
    Course.deleteOne({ _id: req.params.id })
        .then(() => res.redirect('back'))
        .catch(next);
}
```
So the idea is when I delete course, you should take id course. That is `data-whatever`
```hbs
<a href="" class="btn btn-link" data-toggle="modal" data-id="{{this._id}} "data-target="#delete-course-modal">Xóa</a>
```
Add script when dialog confirm clicked, it will be log id
```hbs
<script>
    document.addEventListener('DOMContentLoaded', function() {

        // When dialog confirm clicked
        $('#delete-course-modal').on('show.bs.modal', function (event) {
            var button = $(event.relatedTarget);
            var id = button.data('id');
            console.log(id);
        });
    });
</script>
```
Add id `btn-delete-course'`, then get id element and create function to alert id course

**src/resources/views/me/stored-courses.hbs**
```hbs
<button id="btn-delete-course" type="button" class="btn btn-danger">Xóa bỏ</button>
```
```hbs
<script>
    document.addEventListener('DOMContentLoaded', function() {
        var courseId;

        // When dialog confirm clicked
        $('#delete-course-modal').on('show.bs.modal', function (event) {
            var button = $(event.relatedTarget);
            courseId = button.data('id');
        });

        var btnDeleteCourse = document.getElementById("btn-delete-course");

        btnDeleteCourse.onclick = function () {
            alert(courseId);
        }
    });
</script>
```
**So, how to click to submit form without alert?**. Let's go to create delete hidden form with `DELETE` method, set name `delete-course-form`

**src/resources/views/me/stored-courses.hbs**
```hbs
{{!-- Delete hidden form --}}
<form name="delete-course-form" method="POST"></form>
```
To test, type: `document.forms['delete-course-form'];` to get form element.

We need to set again attribute of this form. We need to create action delete form with override method

**src/resources/views/me/stored-courses.hbs**
```js
<script>
    document.addEventListener('DOMContentLoaded', function() {
        var courseId;
        var deleteForm = document.forms['delete-course-form'];
        var btnDeleteCourse = document.getElementById("btn-delete-course");

        // When dialog confirm clicked
        $('#delete-course-modal').on('show.bs.modal', function (event) {
            var button = $(event.relatedTarget);
            courseId = button.data('id');
        });

        btnDeleteCourse.onclick = function () {
            deleteForm.action = '/courses/' + courseId + '?_method=DELETE';
        }
    });
</script>
``` 
So as you can see, when click Delete button and confirm to delete, it will be add id that want to delete

[![image.png](https://i.postimg.cc/PqQ26m9h/image.png)](https://postimg.cc/B8tTb1Jw)

The next step is submit this form, it will be override method to go to delete, then match `destroy` controller then redirect.
```js
<script>
    document.addEventListener('DOMContentLoaded', function() {
        var courseId;
        var deleteForm = document.forms['delete-course-form'];
        var btnDeleteCourse = document.getElementById('btn-delete-course');

        // When dialog confirm clicked
        $('#delete-course-modal').on('show.bs.modal', function (event) {
            var button = $(event.relatedTarget);
            courseId = button.data('id');
        });
        
        // When delete course btn clicked
        btnDeleteCourse.onclick = function () {
            deleteForm.action = '/courses/' + courseId + '?_method=DELETE';
            deleteForm.submit();
        }
    });
</script>
```
# Soft delete
## Install and import package
Install mongoose-delete: Type: `npm i mongoose-delete`

Add plugin:

**/src/app/models/Course.js**
```js
const mongooseDelete = require('mongoose-delete');

// Add plugins
Course.plugin(mongooseDelete);
mongoose.plugin(slug);
```
Fix: When you click to delete, you must not real delete.

**src/app/controllers/CourseController.js**
```js
// [DELETE] /courses/:id
destroy(req, res, next) {
    Course.delete({ _id: req.params.id })
        .then(() => res.redirect('back'))
        .catch(next);
}
```

So when you clicked delete button, it not delete permanently, it show delete status is true. It means soft delete.

[![image.png](https://i.postimg.cc/MZVbBWxK/image.png)](https://postimg.cc/SjQMFbJw)
## Method override
Let's go to take override all.

**/src/app/models/Course.js**
```js
// Add plugins
mongoose.plugin(slug);
Course.plugin(mongooseDelete, { overrideMethods: 'all' });
```
So let's go to delete all courses, so as you can see although all courses was deleted, but you don't know what time these courses was deleted?

[![image.png](https://i.postimg.cc/nhDHFwB5/image.png)](https://postimg.cc/Mnzkt5sb)

So let's go to add plugin `deleteAt`

**/src/app/models/Course.js**
```js
// Add plugins
mongoose.plugin(slug);
Course.plugin(mongooseDelete, { 
    deletedAt: true,
    overrideMethods: 'all',
});
```
Sorry, but I am deleted all courses, so you should show message within add else statement. If this array has data, it will be render tables. Otherwise, it will be show message. Then add a link to create course.

**src/resources/views/me/stored-courses.hbs**
```hbs
<tr>
    <td colspan="5" class="text-center">
        Bạn chưa đăng khóa học nào.
        <a href="/courses/create">Đăng khóa học</a>
    </td>
</tr>
```

[![image.png](https://i.postimg.cc/qvbZFkNH/image.png)](https://postimg.cc/ts65ZGdB)
## Create trash page
Let's go to create trash page to restore courses. First, let's go to add trash router

**src/routes/me.js**
```js
router.get("/trash/courses", meController.trashCourses);
```

Second, go to add trash GET method

**src/app/controllers/MeController.js**
```js
// [GET] /me/trash/courses
trashCourses(req, res, next) {
    Course.find({ })
        .then(courses => 
            res.render("me/trash-courses", {
                courses: multipleMongooseToObject(courses)
            }),
        )
        .catch(next);
}
```
Add trash courses view

**src/resources/views/me/trash-courses.hbs**
```hbs
<div class="mt-4">
    <div>
      <a href="/me/stored/courses">Danh sách khóa học</a>
      <h3>Khóa học đã xóa</h3>
    </div>

    <table class="table mt-4">
        <thead>
            <tr>
                <th scope="col">#</th>
                <th scope="col">Tên khóa học</th>
                <th scope="col">Trình độ</th>
                <th scope="col" colspan="2">Thời gian tạo</th>
            </tr>
        </thead>
        <tbody>
            {{#each courses}}
            <tr>
                <th scope="row">{{sum @index 1}}</th>
                <td>{{this.name}}</td>
                <td>{{this.level}}</td>
                <td>{{this.createdAt}}</td>
                <td>
                    <a href="/courses/{{this._id}}/edit" class="btn btn-link">Khôi phục</a>
                    <a href="" class="btn btn-link" data-toggle="modal" data-id="{{this._id}}" data-target="#delete-course-modal">Xóa vĩnh viễn</a>
                </td>
            </tr>
            {{else}}
            <tr>
              <td colspan="5" class="text-center">
                Thùng rác trống.
                <a href="/me/stored/courses">Danh sách khóa học</a>
              </td>
            </tr>
            {{/each}}
        </tbody>
    </table>
</div>
```
Get courses was deleted
```js
// [GET] /me/trash/courses
trashCourses(req, res, next) {
    Course.findDeleted({})
        .then(courses => 
            res.render("me/trash-courses", {
                courses: multipleMongooseToObject(courses)
            }),
        )
        .catch(next);
}
```

Output:

[![image.png](https://i.postimg.cc/KvB1WcQk/image.png)](https://postimg.cc/BXZZL0r4)

When you click Restore, you should submit to `CourseController`, we will using PUT, PATCH methods.

**src/routes/courses.js**
```js
router.patch("/:id/restore", courseController.restore);
```

Add restore PATCH method:

**src/app/controllers/CourseController.js**
```js
// [PATCH] /courses/:id/restore
restore(req, res, next) {
    Course.restore({ _id: req.params.id })
        .then(() => res.redirect('back'))
        .catch(next);
}
```

Change button name with no href

**src/resources/views/me/trash-courses.hbs**
```hbs
<td>
    <a href="" class="btn btn-link btn-restore" data-id="{{this._id}}">Khôi phục</a>
    <a href="" class="btn btn-link" data-toggle="modal" data-id="{{this._id}}" data-target="#delete-course-modal">Xóa vĩnh viễn</a>
</td>

<form name="restore-course-form" method="POST"></form>
...

<script>
    document.addEventListener('DOMContentLoaded', function() {
        ...
        var restoreForm = document.forms['restore-course-form'];
        var restoreBtn = $('.btn-restore');

        ...

        // Restore btn clicked
        restoreBtn.click(function (e) {
            e.preventDefault();

            var courseId = $(this).data('id');
            restoreForm.action = '/courses/' + courseId + '/restore?_method=PATCH';
            restoreForm.submit();
        });
    });
</script>
```
## Add permanently delete
So let's go to add permanently delete button. It will be show message this action cannot restore.

First, add forceDestroy method:

**src/routes/courses.js**
```js
router.delete("/:id/force", courseController.forceDestroy);
```

Then add `forceDestroy` method with DELETE method:

**src/app/controllers/CourseController.js**
```js
// [DELETE] /courses/:id/force
forceDestroy(req, res, next) {
    Course.deleteOne({ _id: req.params.id })
        .then(() => res.redirect('back'))
        .catch(next);
}
```

Change confirm permanently delete courses:

**src/resources/views/me/trash-courses.hbs**
```hbs
{{!-- Confirm delete course --}}
<div id="delete-course-modal" class="modal" tabindex="-1" role="dialog">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">Xóa khóa học?</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
        <p>Hành động này không thể khôi phục. Bạn vẫn muốn xóa khóa học này?</p>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Hủy</button>
        <button id="btn-delete-course" type="button" class="btn btn-danger">Xóa vĩnh viễn</button>
      </div>
    </div>
  </div>
</div>

<script>
    document.addEventListener('DOMContentLoaded', function() {
        var courseId;
        var deleteForm = document.forms['delete-course-form'];
        var restoreForm = document.forms['restore-course-form'];
        var btnDeleteCourse = document.getElementById('btn-delete-course');
        var restoreBtn = $('.btn-restore');

        // When dialog confirm clicked
        $('#delete-course-modal').on('show.bs.modal', function (event) {
            var button = $(event.relatedTarget);
            courseId = button.data('id');
        });
        
        // When delete course btn clicked
        btnDeleteCourse.onclick = function () {
            deleteForm.action = '/courses/' + courseId + '/force?_method=DELETE';
            deleteForm.submit();
        }

        // Rest code
        ...
    });
</script>
```

So as you can see, if you clicked Permanently delete, it will be show message: _This action cannot restored. Do you want still delete this course?_

[![image.png](https://i.postimg.cc/sDvjXqmJ/image.png)](https://postimg.cc/ppMbc0g9)

**Update**: If you delete courses but was still show course was deleted, you can fix code using `findWithDelete` method:

**MeController.js**
```js
// [GET] /me/trash/courses
trashCourses(req, res, next) {
    Course.findWithDeleted({ deleted: true })
        .then((courses) =>
            res.render('me/trash-courses', {
                courses: multipleMongooseToObject(courses),
            }),
        )
        .catch(next);
}
```
# Deleted count documents
We want to show number of deleted courses in trash, otherwise do not show trash page. We use `countDocumentsDeleted` method to count courses was deleted. We use Promise with destructuring JS.
```js
const Course = require('../models/Course');
const { multipleMongooseToObject } = require('../../util/mongoose');

class MeController {
    // [GET] /me/stored/courses
    storedCourses(req, res, next) {
        Promise.all([Course.find({}), Course.countDocumentsDeleted()])
            .then(([courses, deletedCount]) => 
                res.render('me/stored-courses', {
                    deletedCount,
                    courses: multipleMongooseToObject(courses),
                })
            )
            .catch(next);
    }

    // [GET] /me/trash/courses
    trashCourses(req, res, next) {
        Course.findWithDeleted({ deleted: true })
            .then((courses) =>
                res.render('me/trash-courses', {
                    courses: multipleMongooseToObject(courses),
                }),
            )
            .catch(next);
    }
}

module.exports = new MeController();
```
## Add time deleted
Add count courses was deleted

**stores-courses.hbs**
```html
<a href="/me/trash/courses">Thùng rác ({{deletedCount}})</a>
```
Change time was deleted courses

**trash-courses.hbs**
```html
<table class="table mt-4">
	<thead>
		<tr>
			<th scope="col">#</th>
			<th scope="col">Tên khóa học</th>
			<th scope="col">Trình độ</th>
			<th scope="col" colspan="2">Thời gian xóa</th>
		</tr>
	</thead>
	<tbody>
		{{#each courses}}
		<tr>
			<th scope="row">{{sum @index 1}}</th>
			<td>{{this.name}}</td>
			<td>{{this.level}}</td>
			<td>{{this.deletedAt}}</td>
			<td>
				<a href="" class="btn btn-link btn-restore" data-id="{{this._id}}">Khôi phục</a>
				<a href="" class="btn btn-link" data-toggle="modal" data-id="{{this._id}}" data-target="#delete-course-modal">Xóa vĩnh viễn</a>
			</td>
		</tr>
		{{else}}
		<tr>
			<td colspan="5" class="text-center">
				Thùng rác trống.
				<a href="/me/stored/courses">Danh sách khóa học</a>
			</td>
		</tr>
		{{/each}}
	</tbody>
</table>
```

Output:

[![image.png](https://i.postimg.cc/VkByDBsT/image.png)](https://postimg.cc/sMX02Wh9)

**Update**: You should using `countDocumentsWithDeleted` method:

**src/app/controllers/MeController.js**
```js
// [GET] /me/stored/courses
storedCourses(req, res, next) {
    Promise.all([Course.find({}), Course.countDocumentsWithDeleted({ deleted: true })])
        .then(([courses, deletedCount]) => 
            res.render("me/stored-courses", {
                deletedCount,
                courses: multipleMongooseToObject(courses),
            }),
        )
        .catch(next);
}
```
# Delete courses counts
## Add forms bootstrap

Add forms bootstrap with class `mt-4`, add flex and center align items, set button disabled, when choose delete action, action button enabled.

**stored-courses.hbs**
```html
<div class="mt-4 d-flex align-items-center">
    <div class="form-check">
        <input class="form-check-input" type="checkbox" value="" id="defaultCheck1">
        <label class="form-check-label" for="defaultCheck1">
        Chọn tất cả
        </label>
    </div>

    <select class="form-control form-control-sm checkbox-select-all-options" name="action" required>
        <option value="">-- Hành động --</option>
        <option value="delete">Xóa</option>
    </select>

    <button class="btn btn-primary btn-sm disabled">Thực hiện</button>
</div>
```
## Add checkbox

Set name `courseIds[]`
**stored-courses.hbs**
```html
<td>
    <div class="form-check">
        <input class="form-check-input" type="checkbox" name="courseIds[]" value="{{this._id}}">
    </div>
</td>
```
## CSS actions

**src/resources/scss/app.scss**
```scss
.checkbox-select-all-options {
    width: 160px;
    margin: 0 16px;
}
```
## Set select all button
When click select all button, all checkbox checked, when unchecked one checkbox, select all button will cancel. We will use jQuery to set select all button, all checkbox checked, set change listen event.

**stored-courses.hbs**
```html
<input class="form-check-input" type="checkbox" value="" id="checkbox-all">
<label class="form-check-label" for="checkbox-all">
    Chọn tất cả
</label>
...
<script>
    document.addEventListener('DOMContentLoaded', function() {
        var courseId;
        var deleteForm = document.forms['delete-course-form'];
        var btnDeleteCourse = document.getElementById('btn-delete-course');
        var checkboxAll = $('#checkbox-all');
        var courseItemCheckbox = $('input[name="courseIds[]"]');

        ...

        // Checkbox all clicked
        checkboxAll.change(function () {
            var isCheckedAll = $(this).prop('checked');
            courseItemCheckbox.prop('checked', isCheckedAll);
        });
    });
</script>
```
Set when unchecked any one checkbox, select all checkbox button will cancel. It will compare the value of count checked. If equals, checked select all. Otherwise, unchecked select all.

**stored-courses.hbs**
```html
<script>
    document.addEventListener('DOMContentLoaded', function() {
        var courseId;
        var deleteForm = document.forms['delete-course-form'];
        var btnDeleteCourse = document.getElementById('btn-delete-course');
        var checkboxAll = $('#checkbox-all');
        var courseItemCheckbox = $('input[name="courseIds[]"]');

        ...

        // Checkbox all change
        checkboxAll.change(function () {
            var isCheckedAll = $(this).prop('checked');
            courseItemCheckbox.prop('checked', isCheckedAll);
        });

        // Course item checkbox change
        courseItemCheckbox.change(function() {
            var isCheckedAll = courseItemCheckbox.length === $('input[name="courseIds[]"]:checked').length;
            checkboxAll.prop('checked', isCheckedAll);
        })
    });
</script>
```
## Set delete action enable
Set delete action enable when check at least one checkbox, it will be enable. Let's create a function to re-render check all submit button

**stored-courses.hbs**
```js
var checkAllSubmitBtn = $('.check-all-submit-btn');

// Re-render check all submit button
function renderCheckAllSubmitBtn() {
    var checkedCount = $('input[name="courseIds[]"]:checked').length;
    if (checkedCount > 0) {
        checkAllSubmitBtn.removeClass('disabled');
    } else {
        checkAllSubmitBtn.addClass('disabled');
    }
}
```
Change div element to form element:
```html
<form class="mt-4">
    ...
</form>
```
Let's check if has permission submit, you should submit form. Let's go to add name form, using POST method and create path action:
```html
<form class="mt-4" name="container-form" method="POST" action="/courses/handle-form-actions">
    ...
</form>
```
```js
var containerForm = document.forms['container-form'];

// Check all submit button clicked
checkAllSubmitBtn.on('submit', function (e) {
    var isSubmitable = !$(this).hasClass('disabled');
    if (!isSubmitable) {
        e.preventDefault();
    }
});
```
When unchecked, prevent default event cancel.
## Create path action
Let's go to create POST method, create handle form actions method:

**src/routes/courses.js**
```js
router.post("/handle-form-actions", courseController.handleFormActions);
```

**src/app/controllers/CourseController.js**
```js
// [POST] /courses/handle-form-actions
handleFormActions(req, res, next) {
    switch(req.body.action) {
        case 'delete':
            Course.delete({ _id: { $in: req.body.courseIds } })
                .then(() => res.redirect('back'))
                .catch(next);
            break;
        default:
            res.json({ message: 'Action is invalid!' });
    }
}
```
We will delete courses which has ids in the list, when complete, redirect back again.
# Fix bug
We need to fix change delete class to delete attribute, then listen this attribute.

**stored-courses.hbs**
```html
<button class="btn btn-primary btn-sm check-all-submit-btn" disabled>Thực hiện</button>

<script>
	document.addEventListener('DOMContentLoaded', function() {
        ...

		// Re-render check all submit button
		function renderCheckAllSubmitBtn() {
			var checkedCount = $('input[name="courseIds[]"]:checked').length;
			if (checkedCount > 0) {
				checkAllSubmitBtn.attr('disabled', false);
			} else {
				checkAllSubmitBtn.attr('disabled', true);
			}
		}
	});
</script>
```
# Middleware
## Significant
- middleware (standing between components in the software model)

Browser (client) =========== Request ============> Server (Node)

Browser (client) =========== Response ============> Server (Node)
## Role
- Middleware has a role like as "the guard"

Home ===================> The guard 1 (middleware 1): The guard 2 (middleware 2) Event (show me a ticket)

Home <=================== Event

1. Check a ticket (to control -> validation)
2. Don't allow to enter
3. Allow to enter (Validation passed -> Allow)
4. Edit / Change
## Application
- Build authentication function
- Build authorization function
- To share the value of arrays to all views (BE)

Example:
```js
app.use(bacBaoVe);

function bacBaoVe (req, res, next) {
    if (['vethuong', 'vevip'].includes(req.query.ve)) {
        req.face = 'Gach gach gach!!!';
        return next();
    }
    res.status(403).json({
        message: "Access denied"
    });
}
```
# Sort middleware
## Add Open Iconic Bootstrap
Go to [Open Iconic page](https://www.appstudio.dev/app/OpenIconic.html), extract zip file then import Open Iconic Bootstrap CSS
```html
<link rel="stylesheet" href="/vendor/open-iconic-master/font/css/open-iconic-bootstrap.css">
```
## Add sort icon
We will use elevator icon:

**stored-courses.hbs**
```html
<a href="">
    <span class="oi oi-elevator"></span>
</a>
```
We will send query parameter to notification we use this function, which column to sort and which type to sort. For example, desc mean descending

**stored-courses.hbs**
```html
<a href="?_sort&column=name&type=desc">
    <span class="oi oi-elevator"></span>
</a>
```

**src/app/controllers/MeController.js**
```js
class MeController {
    // [GET] /me/stored/courses
    storedCourses(req, res, next) {
        let courseQuery = Course.find({});

        if (req.query.hasOwnProperty('_sort')) {
            courseQuery = courseQuery.sort({
                [req.query.column]: req.query.type
            });
        }

        Promise.all([courseQuery, Course.countDocumentsWithDeleted({ deleted: true })])
            .then(([courses, deletedCount]) => 
                res.render("me/stored-courses", {
                    deletedCount,
                    courses: multipleMongooseToObject(courses),
                }),
            )
            .catch(next);
    }

    ...
}
```
But as you can see, we want to apply some other controllers, we need to copy. So instead copy, we will use middleware

Let's create middleware folder in `src/app/middlewares`. Using `res.locals`. This property is useful for exposing request-level information such as the request path name, authenticated user, user settings, and so on to templates rendered within the application.

Default sort function is off, type is default, mean when the user is not sort, check if has sort query, enable sort function, set type default again.

```js
module.exports = function (req, res, next) {
    res.locals._sort = {
        enabled: false,
        type: 'default'
    };

    if (req.query.hasOwnProperty('_sort')) {
        res.locals._sort.enabled = true;
        res.locals._sort.type = req.query.type;
        res.locals._sort.column = req.query.column;
    }

    next();
}
```
Import `SortMiddleware`:

**src/index.js**
```js
const SortMiddleware = require('./app/middlewares/SortMiddleware');

// Custom middlewares
app.use(SortMiddleware);
```
So let's return json: `res.json(res.locals._sort);`

Other method: Using `Object.assign()`
```js
if (req.query.hasOwnProperty('_sort')) {
    Object.assign(res.locals._sort, {
        enabled: true,
        type: req.query.type,
        column: req.query.column,
    });
}
```
## Write helpers
We need to create call `sortable` helpers instead if-else with field and sort are arguments. Define icons, default using elevator icon, asc using descending icon, desc using ascending icon. (using name open iconic class, not HTML code).

Define types sort, default first time sort using descending, when click to ascending using descending. Otherwise when click to descending using ascending.

Define `sortType`, check field send URL, if correct change `sort.type`. Otherwise using icon default.

**src/index.js**
```js
helpers: {
    sortable: (field, sort) => {
        const sortType = field === sort.column ? sort.type : 'default';

        const icons = {
            default: 'oi oi-elevator',
            asc: 'oi oi-sort-ascending',
            desc: 'oi oi-sort-descending'
        };
        const types = {
            default: 'desc',
            asc: 'desc',
            desc: 'asc',
        };

        
        const icon = icons[sortType];
        const type = types[sort.type];

        return `<a href="?_sort&column=${field}&type=${type}">
            <span class="${icon}"></span>                
        </a>`;
    }
}
```
Call `sortable` handlebar helpers and sort fill databases (according MongoDB)

**stored-courses.hbs**
```html
<th scope="col">
    Tên khóa học
    {{{sortable 'name' _sort}}}
</th>
<th scope="col">
    Trình độ
    {{{sortable 'level' _sort}}}
</th>
<th scope="col" colspan="2">
    Thời gian tạo
    {{{sortable 'createdAt' _sort}}}
</th>
```

Let's to uncomment to change real time

Output:

[![image.png](https://i.postimg.cc/8zrc3vBk/image.png)](https://postimg.cc/jDtRnLFB)
# Sort query helper
## Change to lowercase
Change to lowercase from `SortMiddleware` to `sortMiddleware`
## Logic processing
Do not trust by user because hacker thing to do security. Hacker assume write scirpt anything.

Set constant `isValidtype`:

**src/app/controllers/MeController.js**
```js
if (req.query.hasOwnProperty('_sort')) {
    const isValidtype = ['asc', 'desc'].includes(req.query.type)
    courseQuery = courseQuery.sort({
        [req.query.column]: isValidtype ? req.query.type : 'desc',
    });
}
```
## HTML Escaping
If you use two brackets, the `a` tag element will be leak. We will process URL using `escapeExpression` method and `SafeString` method to safe.

We need helpers function with a file name `handlebars.js`. We should protect href.

**src/helpers/handlebars.js**
```js
const Handlebars = require('handlebars');

module.exports = {
    sum: (a, b) => a + b,
    sortable: (field, sort) => {
        const sortType = field === sort.column ? sort.type : 'default';

        const icons = {
            default: 'oi oi-elevator',
            asc: 'oi oi-sort-ascending',
            desc: 'oi oi-sort-descending'
        };
        const types = {
            default: 'desc',
            asc: 'desc',
            desc: 'asc',
        };

        
        const icon = icons[sortType];
        const type = types[sort.type];

        const href = Handlebars.escapeExpression(`?_sort&column=${field}&type=${type}`)

        const output = `<a href="${href}">
            <span class="${icon}"></span>                
        </a>`;
        return new Handlebars.SafeString(output);
    },
};
```

**src/index.js**
```js
// Template engine
app.engine(
    "hbs",
    handlebars({
        extname: ".hbs",
        helpers: require('./helpers/handlebars')
    }),
);
```

## Query Helpers
We don't need repeat this code. Change `Course` to `CourseSchema`:

**src/app/models/Course.js**
```js
const CourseSchema = new Schema(
    {
        name: { type: String, required: true, },
        description: { type: String },
        image: { type: String },
        videoId: { type: String, required: true, },
        level: { type: String },
        slug: { type: String, slug: 'name', unique: true },
    }, 
    {
    timestamps: true,
    }
);

// Custom query helpers
CourseSchema.query.sortable = function (req) {
    if (req.query.hasOwnProperty('_sort')) {
        const isValidtype = ['asc', 'desc'].includes(req.query.type)
        return this.sort({
            [req.query.column]: isValidtype ? req.query.type : 'desc',
        });
    }
    return this;
}

// Add plugins
mongoose.plugin(slug);
CourseSchema.plugin(mongooseDelete, { 
    deletedAt: true,
    overrideMethods: 'all',
});

module.exports = mongoose.model('Course', CourseSchema);
```
Add query helper:

**src/app/controllers/MeController.js**
```js
storedCourses(req, res, next) {
    Promise.all([
        Course.find({}).sortable(req), 
        Course.countDocumentsWithDeleted({ deleted: true })]
    )
        .then(([courses, deletedCount]) => 
            res.render("me/stored-courses", {
                deletedCount,
                courses: multipleMongooseToObject(courses),
            }),
        )
        .catch(next);
}
```
# Autoincrement _id field
MongoDB id field default is ObjectID, so we can add `_id` field in `CourseSchema`

If you want to increment the _id field which is special to mongoose, you have to explicitly specify it as a Number and tell mongoose to not interfere:
**src/app/models/Course.js**
```js
const CourseSchema = new Schema(
    {
        _id: { type: Number, },
        name: { type: String, required: true, },
        description: { type: String },
        image: { type: String },
        videoId: { type: String, required: true, },
        level: { type: String },
        slug: { type: String, slug: 'name', unique: true },
    }, 
    {
        _id: false,
        timestamps: true,
    }
);
```
In this case you don't have to specify inc_field because the default value is _id
## Install mongoose sequence
Type: `npm install --save mongoose-sequence`

This package will help autoincrement handling for mongoose.

You must pass your DB connection instance for this plugin to work. This is needed in order to create a collection on your DB where to store increment references.

```js
const AutoIncrement = require('mongoose-sequence')(mongoose);
```

In this case you don't have to specify inc_field because the default value is _id
```js
CourseSchema.plugin(AutoIncrement);
```

Let's try to create courses without using loop:

**src/app/controllers/CourseController.js**
```js
// [POST] /courses/store
store(req, res, next) {
    req.body.image = `https://i.ytimg.com/vi/${req.body.videoId}/sddefault.jpg`
    const course = new Course(req.body);
    course
        .save()
        .then(() => res.redirect('/me/stored/courses'))
        .catch(next);
}
```

Due to base on sequence, it will be create `counters` collection.

[![image.png](https://i.postimg.cc/Px5rc93R/image.png)](https://postimg.cc/3yzT0SHX)

Let's change sortable to `_id` sort:
```html
<table class="table mt-4">
    <thead>
        <tr>
            <th scope="col"></th>
            <th scope="col">
                ID
                {{{sortable '_id' _sort}}}
            </th>
            <th scope="col">
                Tên khóa học
                {{{sortable 'name' _sort}}}
            </th>
            <th scope="col">
                Trình độ
            </th>
            <th scope="col" colspan="2">
                Thời gian tạo
            </th>
        </tr>
    </thead>
    <tbody>
        {{#each courses}}
        <tr>
            <td>
                <div class="form-check">
                <input class="form-check-input" type="checkbox" name="courseIds[]" value="{{this._id}}">
                </div>
            </td>
            <th scope="row">{{this._id}}</th>
            <td>{{this.name}}</td>
            <td>{{this.level}}</td>
            <td>{{this.createdAt}}</td>
            <td>
                <a href="/courses/{{this._id}}/edit" class="btn btn-link">Sửa</a>
                <a href="" class="btn btn-link" data-toggle="modal" data-id="{{this._id}}" data-target="#delete-course-modal">Xóa</a>
            </td>
        </tr>
        {{else}}
        <tr>
            <td colspan="5" class="text-center">
            Bạn chưa đăng khóa học nào.
            <a href="/courses/create">Đăng khóa học</a>
            </td>
        </tr>
        {{/each}}
    </tbody>
</table>
```