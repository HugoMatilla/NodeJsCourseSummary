#Needs to make a web server

* Better ways to organize our code into reusable pieces
* Ways to deal with files
* Ways to deal with databases
* The ability to communicate over the Internet
* The ability to accept requests and send responses
* A way to deal with work that takes long time


#Modules, Export and Require
##Modules
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

##Build a module

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
###Name value pair
A name which maps to a value.  
The name can be defined more than once, but only can have one value in any given **context**  
`Address = "Sesam street"`
###Object
Collection of name/value pairs
###Object Literal
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

## Module
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

##Events
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
###Magic String
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

# Synchrony
Javascript is synchronous
Node.js  is asynchronous 
## libuv
A C library inside node that deal events occurring in the operating system
### Buffer
Limited size, temporary place for data being moved from one place to another 
### Stream
A sequence of data made available over time
### Binary data
Data store in binary
### Character sets
A representation of characters as numbers (Unicode, Ascii...)
### Encoding
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
### Error First Callback
Callbacks take en error object as their first parameter.   
The Standard.  
null if no error. 

## Streams
### Chunk
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
Connecting two streams by writing o one stream what is being read from another
In node read from Readable stream to a Writable stream




