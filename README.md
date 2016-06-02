# 12 Extremely Useful Hacks for JavaScript

12-javascripts-hacks
In this post I will share 12 extremely useful hacks for JavaScript. These hacks reduce the code and will help you to run optimized code. So let’s start hacking!
 

### 1) Converting to boolean using `!!` operator

Sometimes we need to check if some variable exists or if it has a valid value, to consider them as true value. For do this kind of validation, you can use the `!!` (Double negation operator) a simple `!!variable`, which will automatically convert any kind of data to boolean and this variable will return false only if it has some of these values: `0`, `null`, `""`, `undefined` or `NaN`, otherwise it will return `true`. To understand it in practice, take a look this simple example:

```javascript
function Account(cash) {
    this.cash = cash;
    this.hasMoney = !!cash;
}

var account = new Account(100.50);
console.log(account.cash); // 100.50
console.log(account.hasMoney); // true

var emptyAccount = new Account(0);
console.log(emptyAccount.cash); // 0
console.log(emptyAccount.hasMoney); // false
```

In this case, if an `account.cash` value is greater than zero, the `account.hasMoney` will be `true`.
 

### 2) Converting to number using `+` operator

This magic is awesome! And it’s very simple to be done, but it only works with string numbers, otherwise it will return `NaN` (Not a Number). Have a look on this example:

```javascript
function toNumber(strNumber) {
    return +strNumber;
}

console.log(toNumber("1234")); // 1234
console.log(toNumber("ACB")); // NaN
This magic will work with Date too and, in this case, it will return the timestamp number:

console.log(+new Date()) // 1461288164385
```

### 3) Short-circuits conditionals

If you see a similar code:

```javascript
if (conected) {
    login();
}
```

You can shorten it by using the combination of a variable (which will be verified) and a function using the `&&` (AND operator) between both. For example, the previous code can become smaller in one line:

```javascript
conected && login();
```

You can do the same to check if some attribute or function exists in the object. Similar to the below code:

```javascript
user && user.login();
```

### 4) Default values using `||` operator

Today in ES6 there is the default argument feature. In order to simulate this feature in old browsers you can use the `||` (OR operator) by including the default value as a second parameter to be used. If the first parameter returns `false` the second one will be used as a default value. See this example:

```javascript
function User(name, age) {
    this.name = name || "Oliver Queen";
    this.age = age || 27;
}

var user1 = new User();
console.log(user1.name); // Oliver Queen
console.log(user1.age); // 27

var user2 = new User("Barry Allen", 25);
console.log(user2.name); // Barry Allen
console.log(user2.age); // 25
``` 

### 5) Caching the `array.length` in the loop

This tip is very simple and causes a huge impact on the performance when processing large arrays during a loop. Basically, almost everybody writes this synchronous for to iterate an array:

```javascript
for(var i = 0; i < array.length; i++) {
    console.log(array[i]);
}
```

If you work with smaller arrays – it’s fine, but if you process large arrays, this code will recalculate the size of array in every iteration of this loop and this will cause a bit of delays. To avoid it, you can cache the `array.length` in a variable to use it instead of invoking the `array.length` every time during the loop:

```javascript
var length = array.length;
for(var i = 0; i < length; i++) {
    console.log(array[i]);
}
````

To make it smaller, just write this code:

```javascript
for(var i = 0, length = array.length; i < length; i++) {
    console.log(array[i]);
}
```

### 6) Detecting properties in an object

This trick is very useful when you need to check if some attribute exists and it avoids running undefined functions or attributes. If you are planning to write cross-browser code, probably you will use this technique too. For example, let’s imagine that you need to write code that is compatible with the old Internet Explorer 6 and you want to use the `document.querySelector()`, to get some elements by their `id`s. However, in this browser this function doesn’t exist, so to check the existence of this function you can use the in operator, see this example:

```javascript
if ('querySelector' in document) {
    document.querySelector("#id");
} else {
    document.getElementById("id");
}
```

In this case, if there is no `querySelector` function in the document object, we can use the `document.getElementById()` as fallback.
 

### 7) Getting the last item in the array

The `Array.prototype.slice(begin, end)` has the power to cut arrays when you set the begin and end arguments. But if you don’t set the end argument, this function will automatically set the max value for the array. I think that few people know that this function can accept negative values, and if you set a negative number as begin argument you will get the last elements from the array:

```javascript
var array = [1,2,3,4,5,6];
console.log(array.slice(-1)); // [6]
console.log(array.slice(-2)); // [5,6]
console.log(array.slice(-3)); // [4,5,6]
```

### 8) Array truncation

This technique can lock the array’s size, this is very useful to delete some elements of the array based on the number of elements you want to set. For example, if you have an array with 10 elements, but you want to get only the first five elements, you can truncate the array, making it smaller by setting the `array.length = 5`. See this example:

```javascript
var array = [1,2,3,4,5,6];
console.log(array.length); // 6
array.length = 3;
console.log(array.length); // 3
console.log(array); // [1,2,3]
```

### 9) Replace all

The `String.replace()` function allows using String and Regex to replace strings, natively this function only replaces the first occurrence. But you can simulate a `replaceAll()` function by using the `/g` at the end of a `Regex`:

```javascript
var string = "john john";
console.log(string.replace(/hn/, "ana")); // "joana john"
console.log(string.replace(/hn/g, "ana")); // "joana joana"
```

### 10) Merging arrays

If you need to merge two arrays you can use the `Array.concat()` function:

```javascript
var array1 = [1,2,3];
var array2 = [4,5,6];
console.log(array1.concat(array2)); // [1,2,3,4,5,6];
```

However, this function is not the most suitable to merge large arrays because it will consume a lot of memory by creating a new array. In this case, you can use `Array.push.apply(arr1, arr2)` which instead creates a new array – it will merge the second array in the first one reducing the memory usage:

```javascript
var array1 = [1,2,3];
var array2 = [4,5,6];
console.log(array1.push.apply(array1, array2)); // [1,2,3,4,5,6];
 ```

### 11) Converting `NodeList` to `Arrays`

If you run the `document.querySelectorAll("p")` function, it will probably return an array of DOM elements, the `NodeList` object. But this object doesn’t have all array’s functions, like: `sort()`, `reduce()`, `map()`, `filter()`. In order to enable these and many other native array’s functions you need to convert `NodeList` into `Arrays`. To run this technique just use this function: `[].slice.call(elements)`:

```javascript
var elements = document.querySelectorAll("p"); // NodeList
var arrayElements = [].slice.call(elements); // Now the NodeList is an array
var arrayElements = Array.from(elements); // This is another way of converting NodeList to Array
 ```

### 12) Shuffling array’s elements

To shuffle the array’s elements without using any external library like Lodash, just run this magic trick:

```javascript
var list = [1,2,3];
console.log(list.sort(function() { Math.random() - 0.5 })); // [2,1,3]
 ```

Conclusion

Now you learned some useful JS hacks which are largely used to minify JavaScript code and some of these tricks are used in many popular JS frameworks like Lodash, Underscore.js, Strings.js, among others. If you want to go even deeper and learn more about how you can minify your code even more and even protect it from prying eyes talk to us. I hope you enjoyed this post and if you know other JS hacks, please leave your comment on this post!

This entry was posted in JavaScript, Tutorials and tagged hacks, javascript by Caio Ribeiro Pereira. Bookmark the permalink.
