
# Deep Copy vs Shallow Copy in JavaScript

## Introduction

When working with objects and arrays in JavaScript, understanding the difference between deep copy and shallow copy is crucial. These concepts determine how data structures are duplicated and how changes to copies affect the original data.

### Shallow Copy

A **shallow copy** of an object or array creates a new object/array with references to the same elements as the original. This means that if the original object contains other objects (nested objects), the shallow copy will only duplicate the references to these nested objects, not the objects themselves.

#### Example of Shallow Copy

```javascript
// Creating an object
let originalObject = {
  name: "Ali",
  address: {
    city: "Islamabad",
    country: "Pakistan"
  }
};

// Shallow copying the object using Object.assign
let shallowCopy = Object.assign({}, originalObject);

// Modifying the nested object in the shallow copy
shallowCopy.address.city = "Lahore";

console.log(originalObject.address.city); // Output: "Lahore"
```

In this example, modifying the `city` property in `shallowCopy` also modifies the `city` in `originalObject` because both objects share the same reference to the `address` object.

#### Shallow Copy Methods

Some common methods to create shallow copies in JavaScript include:

- `Object.assign()`
- The spread operator (`...`)

### Deep Copy

A **deep copy** creates a new object or array with copies of all the elements, including nested objects and arrays. This means that changes made to the deep copy do not affect the original object or array.

#### Example of Deep Copy

```javascript
// Creating an object
let originalObject = {
  name: "Ahmed",
  address: {
    city: "Karachi",
    country: "Pakistan"
  }
};

// Deep copying the object using JSON.parse(JSON.stringify())
let deepCopy = JSON.parse(JSON.stringify(originalObject));

// Modifying the nested object in the deep copy
deepCopy.address.city = "Quetta";

console.log(originalObject.address.city); // Output: "Karachi"
```

In this example, modifying the `city` property in `deepCopy` does not affect the `originalObject`, because `deepCopy` contains a complete copy of the `address` object, not just a reference.

#### Deep Copy Methods

To perform a deep copy in JavaScript, you can use:

- `JSON.parse(JSON.stringify())` (simple objects and arrays)
- Libraries like Lodash (`_.cloneDeep`)

## Summary

- **Shallow Copy**: Copies only the references to nested objects, not the objects themselves.
- **Deep Copy**: Creates a full copy of the original object, including all nested objects.

<br>
<br>

 If you want to understand how nested objects behave differently in shallow copy vs deep copy, let's expand on that a bit.

# Nested Objects in Shallow Copy

When you create a shallow copy of an object that contains nested objects, the references to those nested objects are copied, not the objects themselves. This means both the original and the copied object will point to the same nested object.

#### Example: Shallow Copy with Nested Objects

```javascript
let originalObject = {
  name: "Ali",
  address: {
    city: "Islamabad",
    country: "Pakistan"
  },
  hobbies: ["Cricket", "Reading"]
};

// Shallow copying the object using the spread operator
let shallowCopy = { ...originalObject };

// Modifying the nested object in the shallow copy
shallowCopy.address.city = "Lahore";
shallowCopy.hobbies.push("Coding");

console.log(originalObject.address.city); // Output: "Lahore"
console.log(originalObject.hobbies);      // Output: ["Cricket", "Reading", "Coding"]
```

In this example, when we modify `shallowCopy.address.city`, the change is reflected in `originalObject.address.city` as well, because both the original and shallow copy share the same reference to the `address` object. Similarly, adding a new hobby in `shallowCopy.hobbies` also affects `originalObject.hobbies`.

But there is one deawback if you change a property of a nested object within a shallow copy, both the original object and the shallow copy will reflect that change. This happens because the shallow copy only creates a new reference to the nested object, not a new nested object itself.

```javascript
let originalObject = {
  name: "Ali",
  address: {
    city: "Islamabad",
    country: "Pakistan"
  }
};

// Creating a shallow copy using the spread operator
let shallowCopy = { ...originalObject };

// Modifying a property of the nested object in the shallow copy
shallowCopy.address.city = "Lahore";

// Logging the properties
console.log(originalObject.address.city); // Output: "Lahore"
console.log(shallowCopy.address.city);    // Output: "Lahore"
```
### Why Does This Happen?

This happens because the shallow copy does not create a new `address` object; it only copies the reference to the same `address` object from `originalObject`. Therefore, any modification to the `address` object in either `originalObject` or `shallowCopy` will affect both.


### Nested Objects in Deep Copy

When you create a deep copy of an object, all nested objects and arrays are copied as well. This means that the new object is completely independent of the original.

#### Example: Deep Copy with Nested Objects

```javascript
let originalObject = {
  name: "Ahmed",
  address: {
    city: "Karachi",
    country: "Pakistan"
  },
  hobbies: ["Football", "Movies"]
};

// Deep copying the object using JSON.parse(JSON.stringify())
let deepCopy = JSON.parse(JSON.stringify(originalObject));

// Modifying the nested object in the deep copy
deepCopy.address.city = "Quetta";
deepCopy.hobbies.push("Swimming");

console.log(originalObject.address.city); // Output: "Karachi"
console.log(originalObject.hobbies);      // Output: ["Football", "Movies"]
```

In this example, when we modify `deepCopy.address.city`, the change is **not** reflected in `originalObject.address.city`, and adding a new hobby to `deepCopy.hobbies` does not affect `originalObject.hobbies`. This is because `deepCopy` contains a full, independent copy of the original object, including all nested structures.

### Important Note on Deep Copy

While using `JSON.parse(JSON.stringify())` is a quick way to perform a deep copy, it has limitations:
- It does not copy functions, `undefined`, `NaN`, `Infinity`, or special objects like `Date` and `RegExp`.
- Circular references (when an object refers to itself) will cause an error.

For more complex objects or cases where you need to handle special data types, you might want to use a library like Lodash's `_.cloneDeep()`.

<br>
<br>

# Additional Methods for Creating Deep or Shallow Copies

In JavaScript, besides the commonly known methods, there are several other ways to create deep or shallow copies of objects and arrays. Understanding these methods and their use cases will help you manage data structures more effectively.


### Shallow Copy Methods

1. **`Object.assign()`**: 
   - Creates a shallow copy of an object.
   - Copies the properties of one or more source objects into a target object.

   ```javascript
   let originalObject = { name: "Ali", age: 25 };
   let shallowCopy = Object.assign({}, originalObject);
   ```

2. **Spread Operator (`...`)**: 
   - Another way to create a shallow copy of objects or arrays.

   ```javascript
   let originalArray = [1, 2, 3];
   let shallowCopyArray = [...originalArray];

   let originalObject = { name: "Ahmed", age: 30 };
   let shallowCopyObject = { ...originalObject };
   ```

3. **`Array.prototype.slice()`**: 
   - Creates a shallow copy of an array.
   - Does not modify the original array.

   ```javascript
   let originalArray = [1, 2, 3];
   let shallowCopyArray = originalArray.slice();
   ```

4. **`Array.prototype.concat()`**: 
   - Also used to create a shallow copy of arrays.
   - Combines the original array with other elements or arrays.

   ```javascript
   let originalArray = [1, 2, 3];
   let shallowCopyArray = [].concat(originalArray);
   ```

### Deep Copy Methods

1. **`JSON.stringify()` and `JSON.parse()`**: 
   - A straightforward way to perform a deep copy, especially for objects and arrays without functions or special types.
   - Converts the object to a string and then parses it back to an object.

   ```javascript
   let originalObject = { name: "Sara", hobbies: ["Reading", "Swimming"] };
   let deepCopyObject = JSON.parse(JSON.stringify(originalObject));
   ```

2. **Lodash `_.cloneDeep()`**: 
   - A more robust method to create deep copies, especially for complex objects with nested structures, circular references, or special types.
   - Requires the Lodash library.

   ```javascript
   let _ = require('lodash');
   let originalObject = { name: "Sara", hobbies: ["Reading", "Swimming"] };
   let deepCopyObject = _.cloneDeep(originalObject);
   ```

### Considerations

- **Shallow Copies**: Useful when dealing with flat structures or when you don’t mind if changes to nested objects in the copy affect the original.
- **Deep Copies**: Necessary when you want to completely separate the copied structure from the original, especially when working with nested objects or arrays.

Each method has its specific use cases, and understanding when to use shallow vs. deep copy is essential for managing object and array manipulation in JavaScript effectively.

