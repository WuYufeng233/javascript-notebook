## JSON.parse() and JSON.stringify()
The **JSON** object, available in all modern browsers, has two very useful methods to deal with JSON-formatted content: parse and stringify. **JSON.parse()** takes a JSON string and transforms it into a JavaScript object. **JSON.stringify()** takes a JavaScript object and transforms it into a JSON string.

#### Here's an example
```javascirpt
const myObj = {
  name: 'Skip',
  age: 2,
  favoriteFood: 'Steak'
};

const myObjStr = JSON.stringify(myObj);

console.log(myObjStr);
// "{"name":"Skip","age":2,"favoriteFood":"Steak"}"

console.log(JSON.parse(myObjStr));
// Object {name:"Skip",age:2,favoriteFood:"Steak"}"
```
And although the methods are usually used on objects, they can also be used on arrays:
```javascript
const myArr = ['bacon', 'letuce', 'tomatoes'];

const myArrStr = JSON.stringify(myArr);

console.log(myArrStr);
// "["bacon","letuce","tomatoes"]"

console.log(JSON.parse(myArrStr));
// ["bacon","letuce","tomatoes"]
```
---
#### JSON.parse()
JSON.parse() can take an second argument for a **reviver** function that can transform the object values **before they are returned**. Here the object’s values are uppercased in the returned object of the parse method:
```javascript
const user = {
  name: 'John',
  email: 'john@awesome.com',
  plan: 'Pro'
};

const userStr = JSON.stringify(user);

JSON.parse(userStr, (key, value) => {
  if (typeof value === 'string') {
    return value.toUpperCase();
  }
  return value;
});
```
---
#### JSON.stringify()
**JSON.stringify()** can take two additional arguments, the first one being a **replacer** function and the second a String or Number value to use as a **space** in the returned string.

The replacer function can be used to filter-out values because any value returned as undefined will be out of the returned string:
```javascript
onst user = {
  id: 229,
  name: 'John',
  email: 'john@awesome.com'
};

function replacer(key, value) {
  console.log(typeof value);
  if (key === 'email') {
    return undefined;
  }
  return value;
}

const userStr = JSON.stringify(user, replacer);
// "{"id":229,"name":"John"}"
```
And an example with a **space** argument passed-in:
```javascript
const user = {
  name: 'John',
  email: 'john@awesome.com',
  plan: 'Pro'
};

const userStr = JSON.stringify(user, null, '...');
// "{
// ..."name": "John",
// ..."email": "john@awesome.com",
// ..."plan": "Pro"
// }"
```

#### (附：更多详细知识可以参考MDN文档：[JSON.parse()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)、[JSON.stringify()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify))
