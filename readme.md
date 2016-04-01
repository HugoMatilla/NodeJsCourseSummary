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

