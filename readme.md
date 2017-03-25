<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [1.Intro](#1intro)
	- [Needs to make a web server](#needs-to-make-a-web-server)
- [2.Modules, Export and Require](#2modules-export-and-require)
	- [Modules](#modules)
	- [Build a module](#build-a-module)
			- [Name value pair](#name-value-pair)
			- [Object](#object)
			- [Object Literal](#object-literal)
	- [Prototypal inheritance](#prototypal-inheritance)
	- [Function constructor](#function-constructor)
	- [Immediately Invoked Functions Expressions (IIFEs)](#immediately-invoked-functions-expressions-iifes)
	- [How Modules Work](#how-modules-work)
	- [Require](#require)
	- [Module Patterns](#module-patterns)
			- [Use always  `module.exports` instead of `export` although they do the same.](#use-always-moduleexports-instead-of-export-although-they-do-the-same)
	- [Native Modules](#native-modules)
	- [Modules and ES6](#modules-and-es6)
- [3.Events And EventEmitter](#3events-and-eventemitter)
	- [Events](#events)
		- [Object Properties and Methods](#object-properties-and-methods)
		- [Functions and Arrays](#functions-and-arrays)
	- [Node Event Emitter](#node-event-emitter)
		- [Event Listener](#event-listener)
		- [Our Event Emitter](#our-event-emitter)
		- [Magic String](#magic-string)
	- [Prototype chain](#prototype-chain)
	- [Inheriting from event emitter](#inheriting-from-event-emitter)
	- [ES5](#es5)
		- [Template literals](#template-literals)
		- [call() and apply()](#call-and-apply)
	- [Inheriting pattern 2](#inheriting-pattern-2)
	- [ES6 Classes](#es6-classes)
	- [Inheriting in ES6 Classes](#inheriting-in-es6-classes)
- [4.Synchrony](#4synchrony)
			- [libuv](#libuv)
			- [Buffer](#buffer)
			- [Stream](#stream)
			- [Binary data](#binary-data)
			- [Character sets](#character-sets)
			- [Encoding](#encoding)
	- [Buffers](#buffers)
	- [ArrayBuffer](#arraybuffer)
	- [Callbacks](#callbacks)
	- [Files](#files)
			- [Error First Callback](#error-first-callback)
	- [Streams](#streams)
			- [Chunk](#chunk)
	- [Pipes](#pipes)
			- [Method chaining](#method-chaining)
- [5.HTTP](#5http)
			- [Protocol](#protocol)
			- [Port](#port)
			- [HTTP](#http)
			- [MIME Type](#mime-type)
			- [http_parser](#httpparser)
	- [HTTP server](#http-server)
	- [Output HTML and Templates](#output-html-and-templates)
			- [Template](#template)
	- [Streams and Performance](#streams-and-performance)
			- [API](#api)
			- [Endpoint](#endpoint)
	- [Outputting a JSON](#outputting-a-json)
			- [Serialize](#serialize)
	- [Routing](#routing)
- [6.NPM](#6npm)
			- [Semantic Versioning](#semantic-versioning)
	- [Package.json](#packagejson)
	- [Global dependencies](#global-dependencies)
			- [nodemon](#nodemon)
- [7.Express](#7express)
			- [Environment variables](#environment-variables)
			- [Http method (verbs)](#http-method-verbs)
	- [Routes](#routes)
	- [Static Files and Middleware](#static-files-and-middleware)
			- [Midleware](#midleware)
	- [Templates and Template Engines](#templates-and-template-engines)
	- [Querystring and Post Parameters](#querystring-and-post-parameters)
		- [Querystring](#querystring)
		- [Form Post](#form-post)
		- [Post as json](#post-as-json)
	- [RESTful API and JSON](#restful-api-and-json)
			- [REST](#rest)
	- [Structuring the Application](#structuring-the-application)
		- [Structuring with Express](#structuring-with-express)
		- [Our own Structure](#our-own-structure)
- [8.MEAN](#8mean)
			- [DOM](#dom)
	- [Angular](#angular)
	- [Separating Server code from Client code](#separating-server-code-from-client-code)
- [9.Build an app](#9build-an-app)
	- [1. Init](#1-init)
	- [2. Connect to the Database](#2-connect-to-the-database)
	- [3. Adding Data](#3-adding-data)
	- [4. Create the API](#4-create-the-api)

<!-- /TOC -->
# 1.Intro
## Needs to make a web server

* Better ways to organize our code into reusable pieces
* Ways to deal with files
* Ways to deal with databases
* The ability to communicate over the Internet
* The ability to accept requests and send responses
* A way to deal with work that takes long time


# 2.Modules, Export and Require
## Modules
Reusable block of code whose existence does not accidentally impact other code

**Common JS Modules**
An agreed upon standard for how code modules should be structured

**First class Functions**
Everything you can do with other types you can do with functions

**Expression**
A block of code that results in a value


```javascript

	// function statement
	function greet() {
		console.log('hi');
	}
	greet();

	// functions are first-class
	function logGreeting(fn) {
		fn();
	}
	logGreeting(greet);

	// function expression
	var greetMe = function() {
		console.log('Hi Tony!');
	}
	greetMe();

	// it's first-class
	logGreeting(greetMe);

	// use a function expression to create a function on the fly
	logGreeting(function() {
		console.log('Hello Tony!');
	});
```

## Build a module

**app.js**
```javascript

	var greet = require('./greet'); // returns what we have in `module.exports`
	greet();
```
**greet.js**
```javascript

	var greet = function() {
		console.log('Hello!');
	};

	module.exports = greet;
```
#### Name value pair
A name which maps to a value.
The name can be defined more than once, but only can have one value in any given **context**
`Address = "Sesam street"`
#### Object
Collection of name/value pairs
#### Object Literal
Name/value pairs separated by commas and surrounded by curly braces
```javascript

	var person = {
		name: 'Peter',
		lastname:'Doe',
		workplace:{
			Street:'Sesam street',
			Number: '1'
		},
		greet: function(){
				console.log('Hello ' + this.name + ' ' + this.lastname )
		}
	};

	person.greet();
	console.log(person['name'])
```
## Prototypal inheritance
Every object points to a `proto{}` object

## Function constructor
A function used to build objects using `new`
```javascript

	function Person(firstname, lastname) {
		this.firstname = firstname;
		this.lastname = lastname;
	}


	Person.prototype.greet = function() {
		console.log('Hello, ' + this.firstname + ' ' + this.lastname);
	};

	var john = new Person('John', 'Doe');
	john.greet();

```

## Immediately Invoked Functions Expressions (IIFEs)
Run the function at the point it is created. Add a `()` after its declaration.
```javascript

	var firstname = 'Jane';

	(function (lastname) {

		var firstname = 'John';
		console.log(firstname);
		console.log(lastname);

	}('Doe'));

	console.log(firstname);
```

## How Modules Work
`require` is a function, that you pass a 'path' to
`module.exports` is what the require function returns
this works because your code is actually wrapped in a function that is given these things as function parameters

**app.js**
```javascript

	var greet = require('./greet'); // returns what we have in `module.exports`
	greet();
```
**greet.js**
```javascript

	var greet = function() {
		console.log('Hello!');
	};

	module.exports = greet;
```

## Require

**app.js**
```javascript

	var greet = require('./greet');

	greet.english();
	greet.spanish();
```
**greet/index.js**
```javascript

	var english = require('./english');
	var spanish = require('./spanish');

	module.exports = {
		english: english,
		spanish: spanish
	};
```
**greet/english.js**
```javascript

	var greetings = require('./greetings.json');

	var greet = function() {
		console.log(greetings.en);
	}

	module.exports = greet;
```

**greet/spanish.js**
```javascript

	var greetings = require('./greetings.json');

	var greet = function() {
		console.log(greetings.es);
	}

	module.exports = greet;
```
**greet/greetings.json**
```json

	{
		"en": "Hello",
		"es": "Hola"
	}
```

## Module Patterns

**app.js**
```javascript

	var greet = require('./greet1');
	greet();

	var greet2 = require('./greet2').greet;
	greet2();

	var greet3 = require('./greet3');
	greet3.greet();
	greet3.greeting = 'Changed hello world!';

	var greet3b = require('./greet3');
	greet3b.greet(); // Returns 'Changed hello world!' -> The result of module.export is cached so the whole greet3 is only called once

	var Greet4 = require('./greet4');
	var grtr = new Greet4();
	grtr.greet();

	var greet5 = require('./greet5').greet;
	greet5();
```

**greet1.js**
```javascript

	module.exports = function() {
		console.log('Hello world');
	};
```

**greet2.js**
```javascript

	module.exports.greet = function() {
		console.log('Hello world!');
	};
```

**greet3.js**
```javascript

	function Greetr() {
		this.greeting = 'Hello world!!';
		this.greet = function() {
			console.log(this.greeting);
		}
	}

	module.exports = new Greetr();
```

**greet4.js**
```javascript

	function Greetr() {
		this.greeting = 'Hello world!!!';
		this.greet = function() {
			console.log(this.greeting);
		}
	}

	module.exports = Greetr;
```

**greet5.js**
Revealing module pattern: Exposing only the properties and methods you want via a returned object
```javascript

	var greeting = 'Hello world!!!!';

	function greet() {
		console.log(greeting);
	}

	module.exports = {
		greet: greet // Only the greet is "exposed" (public)
	}
```
#### Use always  `module.exports` instead of `export` although they do the same.

## Native Modules
[nodejs.org/api](nodejs.org/api)

```javascript

	var util = require('util');

	var name = 'Tony';
	var greeting = util.format('Hello, %s', name);
	util.log(greeting);
```
## Modules and ES6

**greet.js**
```javascript

	export function greet(){
		console.log('Hello');
	}
```

**app.js**
```javascript

	import * as greetr from 'greet';
	greetr.greet();
```
# 3.Events And EventEmitter
## Events
Something that has happened in our application that we can respond to

* **System Events**: C++ core (libuv)
* **Custom Events**: Javascript core (Event Emitter)

### Object Properties and Methods
```javascript

	var obj = {
		greet: 'Hello'
	}

	console.log(obj.greet);
	console.log(obj['greet']);
	var prop = 'greet';
	console.log(obj[prop]);
```

### Functions and Arrays
```javascript

	var arr = [];

	arr.push(function() {
		console.log('Hello world 1');
	});
	arr.push(function() {
		console.log('Hello world 2');
	});
	arr.push(function() {
		console.log('Hello world 3');
	});

	arr.forEach(function(item) {
		item();
	});
```

## Node Event Emitter
### Event Listener
The code that responds to an event
### Our Event Emitter
**Emitter.js**
```javascript

	function Emitter() {
		this.events = {};
	}

	Emitter.prototype.on = function(type, listener) {
		this.events[type] = this.events[type] || [];
		this.events[type].push(listener);
	}

	Emitter.prototype.emit = function(type) {
		if (this.events[type]) {
			this.events[type].forEach(function(listener) {
				listener();
			});
		}
	}

	module.exports = Emitter;
```
**app.js**
```javascript

	var Emitter = require('./emitter');

	var emtr = new Emitter();

	emtr.on('greet', function() {
		console.log('Somewhere, someone said hello.');
	});

	emtr.on('greet', function() {
		console.log('A greeting occurred!');
	});

	console.log('Hello!');
	emtr.emit('greet');
```
### Magic String
A String that has some special meaning in our code. Bad because of typos.
Use config files instead.
```javascript

	module.exports = {
		events: {
			GREET: 'greet'
		}
	}
```

```javascript

	var Emitter = require('events');
	var eventConfig = require('./config').events;

	var emtr = new Emitter();

	emtr.on(eventConfig.GREET, function() {
		console.log('Somewhere, someone said hello.');
	});

	emtr.on(eventConfig.GREET, function() {
		console.log('A greeting occurred!');
	});

	console.log('Hello!');
	emtr.emit(eventConfig.GREET);
```
## Prototype chain
Another way to create objects `Object.create(...)`

```javascript

	var person = {
		firstname: '',
		lastname: '',
		greet: function() {
			return this.firstname + ' ' + this.lastname;
		}
	}

	var john = Object.create(person);
	john.firstname = 'John';
	john.lastname = 'Doe';

	var jane = Object.create(person);
	jane.firstname = 'Jane';
	jane.lastname = 'Doe';

	console.log(john.greet());
	console.log(jane.greet());
```

## Inheriting from event emitter
Set to our object a prototype of the Emitter
Allow us create objects with the Emitter properties

```javascript

	var EventEmitter = require('events');
	var util = require('util');

	function Greetr() { // This is call to a "function constructor"
		this.greeting = 'Hello world!';
	}

	util.inherits(Greetr, EventEmitter); //<--

	Greetr.prototype.greet = function(data) {
		console.log(this.greeting + ': ' + data);
		this.emit('greet', data);
	}

	var greeter1 = new Greetr();

	greeter1.on('greet', function(data) {
		console.log('Someone greeted!: ' + data);
	});

	greeter1.greet('Tony');
```

## ES5
### Template literals
A way to concatenate strings in ES6
```javascript

	var greet2 = `Hello ${ name }`;
```
```javascript

	var a = 5;
	var b = 10;
	console.log(`Fifteen is ${a + b} and\nnot ${2 * a + b}.`);
```
### call() and apply()
```javascript

	var obj = {
		name: 'John Doe',
		greet: function() {
			console.log(`Hello ${ this.name }`);
		}
	}

	obj.greet();
	obj.greet.call({ name: 'Jane Doe'}); // this will point the what is passed in call
	obj.greet.apply({ name: 'Jane Doe'}); // idem

	// With params we see the difference
	var obj = {
		name: 'John Doe',
		greet: function(param1,param2) {
			console.log(`Hello ${ this.name }`);
		}
	}

	obj.greet.call({ name: 'Jane Doe'},param1,param2);
	obj.greet.apply({ name: 'Jane Doe'},[param1,param2]);
```
## Inheriting pattern 2
We need the object that inherits to also have the `this`  methods and properties of the inherited object.
```javascript

	function Greetr() { // This is call to a "function constructor"
		EventEmitter.call(this) //<-- also called "super constructor"
		this.greeting = 'Hello world!';
	}

```

```javascript

	var util = require('util');

	function Person() {
		this.firstname = 'John';
		this.lastname = 'Doe';
	}

	Person.prototype.greet = function() {
		console.log('Hello ' + this.firstname + ' ' + this.lastname);
	}

	function Policeman() {
		Person.call(this);// Without this line `this` object wont have firstname and lastname.
		this.badgenumber = '1234';
	}

	util.inherits(Policeman, Person);
	var officer = new Policeman();
	officer.greet();
```

## ES6 Classes
```javascript

	'use strict'; // like 'lint' for javascript

	class Person {
		constructor(firstname, lastname) {
			this.firstname = firstname;
			this.lastname = lastname;
		}

		greet() {
			console.log('Hello, ' + this.firstname + ' ' + this.lastname);
		}
	}

```
## Inheriting in ES6 Classes

```javascript

	var EventEmitter = require('events');

	class Greetr extends EventEmitter {
		constructor() {
			super();
			this.greeting = 'Hello world!';
		}

		greet(data) {
			console.log(`${ this.greeting }: ${ data }`);
			this.emit('greet', data);
		}
	}
```

# 4.Synchrony
Javascript is synchronous
Node.js  is asynchronous
#### libuv
A C library inside node that deal events occurring in the operating system
#### Buffer
Limited size, temporary place for data being moved from one place to another
#### Stream
A sequence of data made available over time
#### Binary data
Data store in binary
#### Character sets
A representation of characters as numbers (Unicode, Ascii...)
#### Encoding
How characters are stored in binary (UTF-8). Bits used to represent each number
## Buffers
Needed because javascript was not used to deal with binary data. In ES6 it does using ArrayBuffer
```javascript

	var buf = new Buffer('Hello', 'utf8');
	console.log(buf); //<Buffer 48 65 6c 6c 6f>
	console.log(buf.toString()); //Hello
	console.log(buf.toJSON()); // { type: 'Buffer', data: [ 72, 101, 108, 108, 111 ] }
	console.log(buf[2]); //108

	buf.write('wo');
	console.log(buf.toString()); // wollo
```
## ArrayBuffer
[ArrayBuffer Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays)
```javascript

	var buffer = new ArrayBuffer(8);
	var view = new Int32Array(buffer);
	view[0] = 5;
	view[1] = 15;
	console.log(view);	//Int32Array { '0': 5, '1': 15 }
```
## Callbacks
```javascript

	function greet(callback) {
		console.log('Hello!');
		var data = {
			name: 'John Doe'
		};

		callback(data);
	}

	greet(function(data) {
		console.log('The callback was invoked!');
		console.log(data);
	});

	greet(function(data) {
		console.log('A different callback was invoked!');
		console.log(data.name);
	});

	//Hello!
	//The callback was invoked!
	//{ name: 'John Doe' }
	//Hello!
	//A different callback was invoked!
	//John Doe
```
## Files
```javascript

	var fs = require('fs');

	//synchronous
	var greet = fs.readFileSync(__dirname + '/greet.txt', 'utf8');
	console.log(greet); //#1

	//asynchronous
	var greet2 = fs.readFile(__dirname + '/greet.txt', 'utf8', function(err, data) {
		console.log(data);//#3
	});

	console.log('Done!');//#2
```
#### Error First Callback
Callbacks take en error object as their first parameter.
The Standard.
null if no error.

## Streams
#### Chunk
A piece of data being sent through a Stream
Data is split in 'chunks' and streamed
```javascript

	var fs = require('fs');

	var readable = fs.createReadStream(__dirname + '/greet.txt', { encoding: 'utf8', highWaterMark: 16 * 1024 }); // highWaterMark numebr of the buffer

	var writable = fs.createWriteStream(__dirname + '/greetcopy.txt');

	readable.on('data', function(chunk) { // Event emitter, just listenting the event starts the buffer
		console.log(chunk.length);
		// console.log(chunk);
		writable.write(chunk);
	});
```
## Pipes
Conn ecting two streams by writing o one stream what is being read from another
In node read from Readable stream to a Writable stream

``` javascript

	var fs = require('fs');
	var zlib = require('zlib');

	var readable = fs.createReadStream(__dirname + '/greet.txt');

	var writable = fs.createWriteStream(__dirname + '/greetcopy.txt');

	var compressed = fs.createWriteStream(__dirname + '/greet.txt.gz');

	var gzip = zlib.createGzip();

	readable.pipe(writable); // Same as forehead sample

	readable.pipe(gzip).pipe(compressed); // Chain pipes
```
#### Method chaining
A Method returns  an object so we can keep calling more methods.
When it returns the parent object is called "cascading".

# 5.HTTP
#### Protocol
A set of rules two sides agree on the use communicating.
IP : Internet Protocol. Addresses
TCP: Transmission Control Protocol. Send Packets
#### Port
Which program in a computer handles a packet received.
#### HTTP
A set of rules and format for data being transferred on the web
'Hypertext Transfer Protocol' (how the messages are written)
#### MIME Type
A standard for specifying the type of the data being sent
'Multipurpose Internet Mail Extensions'
Examples: application/json, text/html, image/jpeg

#### http_parser
C program that parse http requests and responses

## HTTP server

```javascript

	var http = require('http');

	http.createServer(function (req, res) {

	  res.writeHead(200, {'Content-Type': 'text/plain'});
	  res.end('Hello World\n');

	}).listen(1337, '127.0.0.1');

```
## Output HTML and Templates
```javascript

	var http = require('http');
	var fs = require('fs');

	http.createServer(function(req, res) {

	    res.writeHead(200, { 'Content-Type': 'text/html' });
	    var html = fs.readFileSync(__dirname + '/index.htm', 'utf8');
	    var message = 'Hello world...';
	    html = html.replace('{Message}', message);
	    res.end(html);

	}).listen(1337, '127.0.0.1');
```
#### Template
Text designed to be the basis for final text or content after being processed
## Streams and Performance
We can send chunks of data instead all at once using Streams and piping it to the response.
Use it always.
```javascript

	var http = require('http');
	var fs = require('fs');

	http.createServer(function(req, res) {

	    res.writeHead(200, { 'Content-Type': 'text/html' });
	    fs.createReadStream(__dirname + '/index.htm').pipe(res);

	}).listen(1337, '127.0.0.1');
```

#### API
Application Programing Interface.
In node's field URLs which accept and send only data via HTTP and TCP/IP

#### Endpoint
One URL in a web API

## Outputting a JSON
```javascript

	var http = require('http');
	var fs = require('fs');

	http.createServer(function(req, res) {

	    res.writeHead(200, { 'Content-Type': 'application/json' });
	    var obj = {
	        firstname: 'John',
	        lastname: 'Doe'
	    };
	    res.end(JSON.stringify(obj));

	}).listen(1337, '127.0.0.1');
```
#### Serialize
Translating and object into a format that can be stored or transferred.
JSON, CSV, XML are popular.
'Deserialize' is the opposite (converting the format back into an object)

## Routing
Mapping HTTP requests to content.
Get data in the server from parameters in the http call.

```javascript

	var http = require('http');
	var fs = require('fs');

	http.createServer(function(req, res) {

	    if (req.url === '/') {
	        fs.createReadStream(__dirname + '/index.htm').pipe(res);
	    }

	    else if (req.url === '/api') {
	        res.writeHead(200, { 'Content-Type': 'application/json' });
	        var obj = {
	            firstname: 'John',
	            lastname: 'Doe'
	        };
	        res.end(JSON.stringify(obj));
	    }
	    else {
	        res.writeHead(404);
	        res.end();
	    }

	}).listen(1337, '127.0.0.1');
```
# 6.NPM
#### Semantic Versioning
MAYOR.MINOR.PATCH
## Package.json
`npm init`
`npm install moment --save` The `--save` writes a reference in package.json

```javascript

	"dependencies": {
		"moment": "^2.13.0" // ^ automatically updates minors ~ updates only patches
	}
```


`npm install` : Once we have the package.json we dont need to copy the modules in a new environment, just calling `npm install` will download everything for us

`require` will look directly in the `node_modules` folder

```javascript

	var moment = require('moment');
	console.log(moment().format("ddd, hA"));
```
## Global dependencies
Install dependencies that we only need while developing
`npm install jasmine-node --save-dev`
Install globally, if we kwow we will us a package in many apps in our computer
`npm install nodemon -g`
They are stored at `/usr/local/lib/node_modules/npm/node_modules`

#### nodemon
It watches changes in the folder

# 7.Express
```javascript

	var express = require('express');
	var app = express();

	var port = process.env.PORT || 3000;

	app.get('/', function(req, res) {
		res.send('<html><head></head><body><h1>Hello world!</h1></body></html>');
	});

	app.get('/api', function(req, res) {
		res.json({ firstname: 'John', lastname: 'Doe' });
	});

	app.listen(port);
```
#### Environment variables
Global variables specific to the environment (server) our code is living in.
#### Http method (verbs)
Specific type of action the request wishes to make.
GET, POST, DELETE...
## Routes
```javascript

	app.get('/person/:id', function(req, res) {
		res.send('<html><head></head><body><h1>Person: ' + req.params.id + '</h1></body></html>');
	});
```

## Static Files and Middleware
#### Midleware
Code that sit between 2 layers of software.
In Express:  sitting between request and response
```javascript

	app.use('/assets', express.static(__dirname + '/public'));

	app.use('/', function (req, res, next) {
		console.log('Request Url:' + req.url);
		next(); // call the next Middleware
	});
	app.get('/', function(req, res) {
		res.send('<html><head><link href=assets/style.css type=text/css rel=stylesheet /></head><body><h1>Hello world!</h1></body></html>');
	});
```
## Templates and Template Engines
```javascript

	...

	app.set('view engine', 'ejs');

	app.get('/', function(req, res) {
		res.render('index');
	});

	app.get('/person/:id', function(req, res) {
		res.render('person', { ID: req.params.id });
	});
	...

```
```html

	<html>
		<head>
			<link href="/assets/style.css" type="text/css" rel="stylesheet" /> // important the / before the path of the css
		</head>
		<body>
			<h1>Person: <%= ID %></h1>
		</body>
	</html>
```

## Querystring and Post Parameters
### Querystring
```javascript

	GET /?id=4&page=3 HTTP/1.1
	Host:www.learnweb.net
	Cookie:username=abc:name:Tony
```
```javascript

	app.get('/person/:id', function(req, res) {
		res.render('person', { ID: req.params.id, Qstr: req.query.qstr }); //req.query get the querystring
	});
```
### Form Post
Data in the body as objects
Use body-parser

```javascript

	POST HTTP/1.1
	Host:www.learnweb.net
	Content-Type:application/x-www-form-urlencoded
	Cookie:num=4;page=3
	username=Tony&password=pwd
```

```javascript

	var bodyParser = require('body-parser');
	var urlencodedParser = bodyParser.urlencoded({ extended: false });

	app.post('/person', urlencodedParser, function(req, res) {
		res.send('Thank you!');
		console.log(req.body.firstname);
		console.log(req.body.lastname);
	});
```
```html

	<form method="POST" action="/person">
		First name: <input type="text" id="firstname" name="firstname" /><br />
		Last name: <input type="text" id="lastname" name="lastname" /><br />
		<input type="submit" value="Submit" />
	</form>
```
### Post as json
Data in the body as json
```javascript

	POST HTTP/1.1
	Host:www.learnweb.net
	Content-Type:application/json
	Cookie:num=4;page=3
	{
		'username'='Tony',
		'password'='pwd'
	}
```

```javascript

	var jsonParser = bodyParser.json();

	app.post('/personjson', jsonParser, function(req, res) {
		res.send('Thank you for the JSON data!');
		console.log(req.body.firstname);
		console.log(req.body.lastname);
	});
```

```html

	<script>
		$.ajax({
			type: "POST",
			url: "/personjson",
			data: JSON.stringify({ firstname: 'Jane', lastname: 'Doe' }),
			dataType: 'json',
			contentType: 'application/json'
		});
	</script>
```
## RESTful API and JSON
#### REST
Representational State Transfer. Which HTTP verbs and URLs do what.

```javascript

	app.get('/api/person/:id', function(req, res) {
		// get that data from database
		res.json({ firstname: 'John', lastname: 'Doe' });
	});

	app.post('/api/person', jsonParser, function(req, res) {
		// save to the database
	});

	app.delete('/api/person/:id', function(req, res) {
		// delete from the database
	});
```

## Structuring the Application
### Structuring with Express
`install express-generator -g` // do it global
`express <my app name>`// Create the architecture
`cd <my app name>` // go to the app
`npm install` // install the packages

### Our own Structure
Using controllers

**app.js**
```javascript

	var express = require('express');
	var app = express();

	var apiController = require('./controllers/apiController');
	var htmlController = require('./controllers/htmlController');

	var port = process.env.PORT || 3000;

	app.use('/assets', express.static(__dirname + '/public'));

	app.set('view engine', 'ejs');

	app.use('/', function (req, res, next) {
		console.log('Request Url:' + req.url);
		next();
	});

	htmlController(app);
	apiController(app);

	app.listen(port);
```
**htmlController**
```javascript

	var bodyParser = require('body-parser');

	var urlencodedParser = bodyParser.urlencoded({ extended: false });

	module.exports = function(app) {

		app.get('/', function(req, res) {
			res.render('index');
		});

		app.get('/person/:id', function(req, res) {
			res.render('person', { ID: req.params.id, Qstr: req.query.qstr });
		});

		app.post('/person', urlencodedParser, function(req, res) {
			res.send('Thank you!');
			console.log(req.body.firstname);
			console.log(req.body.lastname);
		});
	}
```

**apiController**
```javascript

	module.exports = function(app) {

		app.get('/api/person/:id', function(req, res) {
		// get that data from database
			res.json({ firstname: 'John', lastname: 'Doe' });
		});

		app.post('/api/person', function(req, res) {
			// save to the database
		});

		app.delete('/api/person/:id', function(req, res) {
			// delete from the database
		});
	}
```
# 8.MEAN
#### DOM
Document Object Model
The structure browsers use to store and manage web pages
Browsers give JavaScript the ability to manipulate the DOM
## Angular

**app.js**
```javascript

	var express = require('express');
	var app = express();

	var port = process.env.PORT || 3000;

	app.set('view engine', 'ejs');
	app.use('/assets', express.static(__dirname + '/public'));

	app.get('/', function(req, res) {

		res.render('index');

	});

	app.listen(port);
```
**view/index.ejs**
```html

	<html ng-app="TestApp">
		<head>
			<title>The MEAN stack</title>
	    	<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.4.5/angular.js"></script>
		</head>
		<body ng-controller="MainController as vm">

			<input type="text" ng-model="vm.message" />
			<br />
			{{ vm.message }}
			<br />
			<ul>
				<li ng-repeat="person in vm.people">
					{{ person.name }}
				</li>
			</ul>

			<script src="assets/js/app.js"></script>
		</body>
	</html>
```
**public/js/app.js**
```javascript

	angular.module('TestApp', []);

	angular.module('TestApp')
		.controller('MainController', ctrlFunc);

	function ctrlFunc() {
		this.message = "Hello";

		this.people = [
			{
				name: 'John Doe'
			},
			{
				name: 'Jane Doe'
			},
			{
				name: 'Jim Doe'
			}
		]
	}
```
## Separating Server code from Client code
We can set in the server what we show in client
**app.js**
```javascript

	var express = require('express');
	var app = express();

	var port = process.env.PORT || 3000;

	var people = [
		{
			name: 'John Doe'
		},
		{
			name: 'Jane Doe'
		},
		{
			name: 'Jim Doe'
		}
	];

	app.set('view engine', 'ejs');
	app.use('/assets', express.static(__dirname + '/public'));

	app.get('/', function(req, res) {

		res.render('index', { serverPeople: people }); //<--

	});

	app.listen(port);
```
**view/index.ejs**
```html

	<html ng-app="TestApp">
		<head>
			<title>The MEAN stack</title>
			<script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.4.7/angular.js"></script>
		</head>
		<body ng-controller="MainController as vm">
			<ul>
				<li ng-repeat="person in vm.people">
					{{ person.name }}
				</li>
			</ul>
			<script>
				var clientPeople = <%- JSON.stringify(serverPeople) %>; //<--
			</script>
			<script src="/assets/js/app.js"></script>
		</body>
	</html>
```
**public/js/app.js**
```javascript

	angular.module('TestApp', []);

	angular.module('TestApp')
		.controller('MainController', ctrlFunc);

	function ctrlFunc() {
		this.people = clientPeople;
	}
```
# 9.Build an app
## 1. Init
*1 `npm init`
*2 `npm install express --save-dev`
*3 `npm install ejs --save-dev`
*4 `npm install body-parser --save-dev`
*5 `npm install mongoose --save-dev`

**app.js**
```javascript

	var express = require('express');
	var app = express();

	var port = process.env.PORT || 3000;

	app.use('/assets', express.static(__dirname + '/public'));

	app.set('view engine', 'ejs');

	app.listen(port);
```
## 2. Connect to the Database
mLab.com
**config/config.json**
```javascript

	{
		"uname":"test",
		"pwd":"test"
	}
```
**config/index.js**
```javascript

	var configValues = require('./config');

	module.exports = {
	    getDbConnectionString: function() {
	        return `mongodb://${configValues.uname}:${configValues.pwd}@ds013212.mlab.com:13212/nodetodo`;
	    }
	}
```
**models/todoModel.js**
```javascript

	var mongoose = require('mongoose');

	var Schema = mongoose.Schema;

	var todoSchema = new Schema({
	    username: String,
	    todo: String,
	    isDone: Boolean,
	    hasAttachment: Boolean
	});

	var Todos = mongoose.model('Todos', todoSchema);

	module.exports = Todos;
```

**app.js**
```javascript

	var express = require('express');
	var app = express();
	var mongoose = require('mongoose');
	var config = require('./config');

	var port = process.env.PORT || 3000;

	app.use('/assets', express.static(__dirname + '/public'));

	app.set('view engine', 'ejs');

	mongoose.connect(config.getDbConnectionString());

	app.listen(port);
```
## 3. Adding Data

**controllers/setupController.js**
```javascript

	var Todos = require('../models/todoModel');

	module.exports = function(app) {

	   app.get('/api/setupTodos', function(req, res) {

	       // seed database
	       var starterTodos = [
	           {
	               username: 'test',
	               todo: 'Buy milk',
	               isDone: false,
	               hasAttachment: false
	           },
	           {
	               username: 'test',
	               todo: 'Feed dog',
	               isDone: false,
	               hasAttachment: false
	           },
	           {
	               username: 'test',
	               todo: 'Learn Node',
	               isDone: false,
	               hasAttachment: false
	           }
	       ];
	       Todos.create(starterTodos, function(err, results) {
	           res.send(results);
	       });
	   });

	}
```
**app.js**
```javascript

	var express = require('express');
	var app = express();
	var mongoose = require('mongoose');
	var config = require('./config');
	var setupController = require('./controllers/setupController');

	var port = process.env.PORT || 3000;

	app.use('/assets', express.static(__dirname + '/public'));

	app.set('view engine', 'ejs');

	mongoose.connect(config.getDbConnectionString());
	setupController(app);

	app.listen(port);
```
Run
`http://localhost:3000/api/setupTodos`

## 4. Create the API
**controllers/apiController**
```javascript

	var Todos = require('../models/todoModel');
	var bodyParser = require('body-parser');

	module.exports = function(app) {

	    app.use(bodyParser.json()); // Middleware
	    app.use(bodyParser.urlencoded({ extended: true })); // Middleware

	    app.get('/api/todos/:uname', function(req, res) {

	        Todos.find({ username: req.params.uname }, function(err, todos) {
	            if (err) throw err;

	            res.send(todos);
	        });

	    });

	    app.get('/api/todo/:id', function(req, res) {

	       Todos.findById({ _id: req.params.id }, function(err, todo) {
	           if (err) throw err;

	           res.send(todo);
	       });

	    });

	    app.post('/api/todo', function(req, res) {

	        if (req.body.id) { // if i received an id I know that is an update
	            Todos.findByIdAndUpdate(req.body.id, { todo: req.body.todo, isDone: req.body.isDone, hasAttachment: req.body.hasAttachment }, function(err, todo) {
	                if (err) throw err;

	                res.send('Success');
	            });
	        }

	        else {

	           var newTodo = Todos({
	               username: 'test',
	               todo: req.body.todo,
	               isDone: req.body.isDone,
	               hasAttachment: req.body.hasAttachment
	           });
	           newTodo.save(function(err) {
	               if (err) throw err;
	               res.send('Success');
	           });

	        }

	    });

	    app.delete('/api/todo', function(req, res) {

	        Todos.findByIdAndRemove(req.body.id, function(err) {
	            if (err) throw err;
	            res.send('Success');
	        })

	    });

	}
```
**app.js**
```javascript

	var express = require('express');
	var app = express();
	var mongoose = require('mongoose');
	var config = require('./config');
	var setupController = require('./controllers/setupController');
	var apiController = require('./controllers/apiController');

	var port = process.env.PORT || 3000;

	app.use('/assets', express.static(__dirname + '/public'));

	app.set('view engine', 'ejs');

	mongoose.connect(config.getDbConnectionString());
	setupController(app);
	apiController(app);

	app.listen(port);
```
