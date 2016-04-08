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

