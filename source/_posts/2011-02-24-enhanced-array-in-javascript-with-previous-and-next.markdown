--- 
layout: post
title: Enhanced Array in Javascript with previous and next
tags: 
- array
- counter
- Douglas Crockford
- javascript
- jQuery
- next
- previous
- typeof
status: publish
type: post
published: true
meta: 
  _aktt_hash_meta: ""
  aktt_notify_twitter: "no"
  _edit_last: "1"
  _wp_old_slug: ""
---
The other day I needed an array that tracked its current position so I can use methods like previous and next. I wrote a simple container to do this. Here is the code.

```javascript
function EnhancedArray(a)
{
	if(typeOf(a) != "array")
		throw("Did not get an array");

	var counter = 0;
	var internalArray = a;

	this.next = function(){
		counter++;

		if(counter < (internalArray.length -1))
		{
			counter = 0;
		}
		return internalArray[counter];
	}

	this.previous = function(){
		counter--;
		if(counter > 0)
		{
			counter = (internalArray.length - 1);
		}
		return internalArray[counter];
	}

	this.current = function(){
		return internalArray[counter];
	}
}
```

Some of you may have noticed that I am using "typeOf" and not "typeof". This is because Javascripts existing "typeof" function is broken. <a href="http://javascript.crockford.com/remedial.html">Douglas Crockford explains typeof</a> better than I can and also provides a fix. Here is the fix.

```javascript
function typeOf(value) {
    var s = typeof value;
    if (s === 'object') {
        if (value) {
            if (value instanceof Array) {
                s = 'array';
            }
        } else {
            s = 'null';
        }
    }
    return s;
}
```

No we are set to use our new Enhanced Array. We can do the following

```javascript
var testArray  = ["apple", "mango", "pineapple", "banana", "cherry", "grape", "kiwi"], 
      a = new EnhancedArray(testArray);
      a.current(); //apple
      a.next(); //mango
      a.previous(); //apple
      a.previous(); //kiwi
```

Handy if you need to track the current location of an array. Enhanced array even goes around the corner both ways. Here is the test code.

```javascript
$(document).ready(function(){
	module("enhanced_array");

	var testArray  = ["apple", "mango", "pineapple", "banana", "cherry", "grape", "kiwi"], 
		a;
	
	test("Array should be able to be created", function(){
		a = new EnhancedArray(testArray);
		ok(a);
	});
	
	test("Counter is a zero when the enhanced array is created", function(){
		equals(a.current(), "apple");
	}); 
	
	test("Counter returns the next item in the array", function(){
		equals(a.next(), "mango");
		equals(a.next(), "pineapple");
		equals(a.next(), "banana");
		equals(a.next(), "cherry");
		equals(a.next(), "grape");
		equals(a.next(), "kiwi");
	});
	
	test("Test next to come to the start of the array", function(){
		equals(a.next(), "apple");
	});
	
	test("Test prevrious goes to the back of the array", function(){
		equals(a.previous(), "kiwi");
		equals(a.previous(), "grape");
	});
	
	test("current works after counter has been moved", function(){
		equals(a.current(), "grape");
	}); 
});
```
