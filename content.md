
<!-- .slide: style="text-align:left" -->
# More Advanced JQuery

Twitter: [#quintons](https://twitter.com/quintons)
<br />
Github: [https://github.com/quintons](https://github.com/quintons)
<br />
Stackoverflow: [http://stackoverflow.com/users/684855/quinton](http://stackoverflow.com/users/684855/quinton)
<br /><br /><br /><br />
 'The most misunderstood language has become the worlds most popular programming language'<br />
*Douglas Crockford - Javascript architect, yahoo*
<br /><br />
 'JavaScript like the browser is starting to move again'<br />
*Alex Russell - Software engineer, Chrome team and (ECMA)TC39 representative*

Note:
This will be my notes
- - -

# Agenda

- [Coding conventions](/#/2 "")
- [What is function()](/#/2 "")
- [Dimensions](/#/2 "")
- [Basics of Promises/Deferreds](/#/2 "")
- [$.Ajax](/#/2 "")
- [More animation](/#/2 "")
- [Plugins &amp; RequireJS](/#/2 "")
- [Data manipulation](/#/2 "")
- [More Promises/Deferreds](/#/2 "")
- [$.Callbacks()](/#/2 "")

Note:
This will be my notes
- - -
<!-- .slide: class="fragment fade-in" -->
## Coding conventions and best practices (in brief)
<br />

#### Conditional statement - shorthand

Good example
```js
// single line
var result = (foo === true) ? console.log('true') : console.log('false');

// multiple lines
var result = (foo === true && bar === false) ?
            console.log('true') : me === true ?
            console.log('me true') : console.log('me false');
```
Bad example
```js
// multiple lines
var result = (foo === true && bar === false) ?
    console.log('true') : me === true ? console.log('me true') :
    console.log('me false');
```


#### Opening brace

Good example
```js
function Foo() {
    return {
        'bar': 'shot'
    };
}
```
Bad Example
```js
function Foo()
{
    return
    {
        'bar': 'shot'
    }
}
```


#### Closing brace
Good example
```js
function Foo() {
    return {
        'bar': 'shot'
    };
}
```
Bad Example
```js
function Foo(){
    return {
            'bar': 'shot'
           }
}
```


#### Named function declaration location

Good example
```js
// code...
function namedFunction(){
    return;
}

return {
    'namedFunction': function(){
        return;
    }
}
```
Bad example
```js
// code...
return {
    'namedFunction': function namedFunction(){
        return;
    }
}
```


#### Functions and Constructors

```js
// Constructor always starts with a capital letter
function MyNewConstructor(){
    this.foo = null;
}

// functions start with a small letter
function myNewFunction(){
    return;
}
```

function expressions can only be used when applied to non-constructor functions
Good example
```js
function MyNewConstructor(){
    this.foo = null;
}
```
Bad example
```js
var MyNewConstructor = function(){
    this.foo = null;
}
```


#### Implied private members
Good example
```js
var myObjectLiteral = {
    _privateFoo: function(){},
    _privateBar: function(){}.
    publicBar: function(){}
}
```
Bad example
```js
var myObjectLiteral = {
    privateFoo: function(){},
    privateBar: function(){},
    publicBar: function(){}
}
```


#### Enforcing private members - modular pattern
```js
    var module = (function(){
        var _privateFunction = function(){
            return;
        }

        var _privateMember = null;

        return {
            foo: function(){
                return _privateFunction();
            }
        };
    })()

    module.foo();
```


#### Using Comments
```js
// return users full name as string
function getFullName(i){
    return names[i];
}
```
Do not waffle!
```js
// this will return the users full name as a string
function getFullName(i){
    return names[i];
}
```

Even better way
```js
/**
 * Return users full name as string
 *
 * @param  {number} pass index of 'names' array
 * @return {string} Return name string
 */
function getFullName(i) {
  return names[i];
};
```


#### Naming conventions

- Use logical names for functions and variables (don't worry about length...but don't go crazy!)
```js
var TheFirstUserNameInThePastArrayFromJSON = getUserName(arr_userNames);
```
- Variable names should be nouns (i.e. person, place, thing, event, substance, quality or quantity)
- Function names should begin with a verb (i.e. getNames())
- Functions that return booleans should begin with 'is' or 'has', such as isValid() or hasItems()
- Avoid useless names such as foo or temp



#### Naming conventions<br />
Camel Casing<br />

what the...???!
```js
var foo = document.getElementById('MyNewID'); // <-- camel 'Id'

var bar = foo.innerHTML(); //<-- capital 'HTML'

var xhr = new XMLHttpResponse(); //<-- camel 'Http'
```

Variables/functions examples
```js
function Person(name){ //<-- object
    this.name = name;
}

var myPerson = new Person('frank'); //<-- variable
```

Constants

```js
var MAX_SPEED = 2000000;
```


#### Other useful things to know...

- Keep HTML out of Javascript
```js
var html = $('<div class="myClass">' + myContent + '</div>'); //<-- better to be in a template.
```
- Keep CSS out of Javscript, exception being class names
```js
var html = $('<div style="width:' + myWidth + ';height:' + myHeight + ';">dollar sit</div>');
```
- don't modify objects you don't own
- Avoid global objects
- Throw your own errors, don't let Javascript throw them for you
- Avoid null comparisions (issue typeof null === 'object')


#### Other useful things to know...continued
- Seperate application logic from event handlers (do not pass the event handler around, it gets garbage collected quickly!)

```js
$('.myButton').on('click', function(event){
    popup(event)
})

var popUp = function(event){
    var that = $('.popup');
    that.css({
        left: event.clientX + 'px',
        top: event.clientY + 'px'
    })
    that.show();
}

// better way...
$('.myButton').on('click', function(event){
    popup(event.clientX, event.clientY)
})

var popUp = function(x, y){
    var that = $('.popup');
    that.css({
        left: x + 'px',
        top: y + 'px'
    })
    that.show();
}
```


#### Config data<br />
Separate config data from application logic<br />

- all URL's
- Any strings that are displayed to the user
- Any HTML that needs to be created from javascript
- Settings (i.e. items per page)
- Repeated unique values
- Any value that may change in the future

```js
var globalConfig = {
    initialUrl: '/index',
    companyName: 'foobar',
    timeoutMs: '2000',
    cache: true
}

var fundListConfig = {
    maxItems: 100,
    charLength: 200,
    buyButton: false
}
```

Note:
This will be my notes
- - -
<!-- .slide: class="fragment fade-in" -->
## function()?
<br />

#### little reminder - function is king

- Functions are first class
- Can be passed as an argument
- Can be returned from a function
- Can be assigned to a variable
- Can be stored in an object or array
- arguments passed by value
- Function objects inherit from 'Function.prototype'


Closure - basic example<br />
what is happening?
```js
function addMe(a){
    var n = (a*2) || 0;

    return function(b){
        return (n += (b || 0));
    }
}

var myAdd1 = addMe();
myAdd1(5);
var res1 = myAdd1(5); // <-- what is res1?

var myAdd2 = addMe(5);
myAdd2();
myAdd2(5);
var res2 = myAdd2(5); // <-- what is res2?
```


Closure - example 1<br />
what is happening? what is the error?
```js
function buildList(list) {
    var result = [];
    for (var i = 0; i < list.length; i++) {
        result.push( function() {alert('item' + list[i] + ' ' + list[i])} );
    }
    return result;
}
function testList() {
    var fnlist = buildList([1,2,3]);
    for (var j = 0; j < fnlist.length; j++) {
        fnlist[j]();
    }
}
buildList(['one','two','three','four']);
testList();
```


Closure - example 1 (fix)
```js
function buildList(list) {
    var result = [];
    for (var i = 0; i < list.length; i++) {
        (function(i){ // <-- an auto executing anynymous function creates scope!
            result.push( function() {alert('item' + list[i] + ' ' + list[i])} );
        })(i);
    }
    return result;
}
function testList() {
    var fnlist = buildList([1,2,3]);
    for (var j = 0; j < fnlist.length; j++) {
        fnlist[j]();
    }
}
buildList(['one','two','three','four']);
testList();
```

Note:
This will be my notes
- - -
<!-- .slide: class="fragment fade-in" -->
## Dimensions
<br />

#### The box model
![alt text](/images/boxmodel.png "")


## Dimensions
<br />
#### list of available methods
- height()
- innerHeight()
- innerWidth()
- outerHeight()
- outerWidth()
- width()


#### Dimension methods
<b>height()</b>, innerHeight(), innerWidth(), outerHeight(), outerWidth(), width()

![alt text](/images/boxmodel-numbers.png "")

```html
<div class='someContent' style='height: 50px;width: 200px;padding: 10px;border: 5px;margin: 2px;'>
    <!-- some content -->
</div>
```

```js
console.log($('.someContent').height()); // returns '50'
```


#### Dimension methods
height(), <b>innerHeight()</b>, innerWidth(), outerHeight(), outerWidth(), width()

![alt text](/images/boxmodel-numbers.png "")

```html
<div class='someContent' style='height: 50px;width: 200px;padding: 10px;border: 5px;margin: 2px;'>
    <!-- some content -->
</div>
```

```js
console.log($('.someContent').innerHeight()); // returns '70'
```


#### Dimension methods
height(), innerHeight(), <b>innerWidth()</b>, outerHeight(), outerWidth(), width()

![alt text](/images/boxmodel-numbers.png "")

```html
<div class='someContent' style='height: 50px;width: 200px;padding: 10px;border: 5px;margin: 2px;'>
    <!-- some content -->
</div>
```

```js
console.log($('.someContent').innerWidth()); // returns '220'
```


#### Dimension methods
height(), innerHeight(), innerWidth(), <b>outerHeight()</b>, outerWidth(), width()

![alt text](/images/boxmodel-numbers.png "")

```html
<div class='someContent' style='height: 50px;width: 200px;padding: 10px;border: 5px;margin: 2px;'>
    <!-- some content -->
</div>
```

```js
console.log($('.someContent').outerHeight()); // returns '80'
console.log($('.someContent').outerHeight(true)); // returns '84' (inc margin)
```


#### Dimension methods
height(), innerHeight(), innerWidth(), outerHeight(), <b>outerWidth()</b>, width()

![alt text](/images/boxmodel-numbers.png "")

```html
<div class='someContent' style='height: 50px;width: 200px;padding: 10px;border: 5px;margin: 2px;'>
    <!-- some content -->
</div>
```

```js
console.log($('.someContent').outerWidth()); // returns '230'
console.log($('.someContent').outerWidth(true)); // returns '234' (inc margins)
```


#### Dimension methods
height(), innerHeight(), innerWidth(), outerHeight(), outerWidth(), <b>width()</b>

![alt text](/images/boxmodel-numbers.png "")

```html
<div class='someContent' style='height: 50px;width: 200px;padding: 10px;border: 5px;margin: 2px;'>
    <!-- some content -->
</div>
```

```js
console.log($('.someContent').width()); // returns '200'
```

Note:
This will be my notes
- - -
## Basics of Promises/Deferreds
<br />

- $.ajaxSetup()
- accepts
- async
- beforeSend
- cache
- complete
- contents
- contentType
- context
- converters
- crossDomain
- data
- dataFilter
- dataType
- error
- global
- headers
- ifModified
- isLocal
- jsonp


- jsonpCallback
- mimeType
- password
- processData
- scriptCharset
- statusCode
- success (can be an array of functions as of version 1.5)
- timeout
- type
- url
- username
- xhr
- xhrFields


Note:
This will be my notes
- - -
<!-- .slide: class="fragment fade-in" -->
## $.Ajax
<br />

Note:
This will be my notes
- - -
<!-- .slide: class="fragment fade-in" -->
## More animation
<br />

Note:
This will be my notes
- - -
<!-- .slide: class="fragment fade-in" -->
## Plugins &amp; RequireJS
<br />

Note:
This will be my notes
- - -
<!-- .slide: class="fragment fade-in" -->
## Data manipulation
<br />

Note:
This will be my notes
- - -
<!-- .slide: class="fragment fade-in" -->
## Promises/Deferreds
<br />

Note:
This will be my notes
- - -
<!-- .slide: class="fragment fade-in" -->
## $.Callbacks()
<br />

Note:
This will be my notes
- - -
## Material references
<br/>

- JQuery site (http://jquery.com/)
- JQ Fundamentals by Rebecca Murphey (http://jqfundamentals.com/)
- JSDoc 3 Documentation (http://usejsdoc.org/)