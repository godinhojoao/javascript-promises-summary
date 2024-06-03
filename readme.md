# Constantly Consuming APIs and Still Struggling with JavaScript Promises? Here's your solution!

## Overview

- In this article, you will understand more about Promises methods in JavaScript and the differences between them.
- This article is for those who already know JavaScript, and want to improve their knowledge about JavaScript Promises.

## Table of Contents

- [What is JavaScript Promise object](#what-is-javascript-promise-object)
- [Promise chaining vs Promise nesting](#promise-chaining-vs-promise-nesting)
- [Promise methods - .all, .allSettled, .race, and .any](#promise-methods---all-allsettled-race-and-any)
- [Promise.race()](#promise-race)
- [References](#references)

## What is a JavaScript Promise object?

- An object representing the eventual completion or failure of an asynchronous operation.
  - For example: Requesting an API the GitHub repositories of someone.
- It was created to avoid callback hell and make the way of handling asynchronous in JavaScript code easier.
- A promise object has one of these three states:
  - pending: initial state
  - resolved: the operation was successful executed
  - rejected: operation failed
- Below there is a simple example of a Promise in JS:

```javascript
const myPromise = new Promise((resolve, reject) => {
  // Asynchronous task:
  setTimeout(() => {
    resolve("Promise resolved after 1 second");
  }, 1000);
});

// using .then(), .catch(), and .finally()
myPromise
  .then((successValue) => console.log(successValue)) // .then to handle success
  .catch((error) => console.error(error)); // .catch to handle error
  .finally(() => console.log('Process completed')); // Handle both success and error cases

// using try/catch and async/await
try {
  const successValue = await myPromise; // your code will be here until the process ends
  console.log(successValue);
} catch (error) {
  console.error(error);
} finally {
  console.log('Process completed.'); // This will run regardless of success or error
}
```

## Promise Chaining vs Promise nesting?

- **Promise chaining**: Handling multiple asynchronous operations in which each `.then()` method returns a new Promise, allowing the next `.then()` to be called and making your code more **readable**.

```javascript
function fetchUser(userId) {} // it returns a promise
function fetchPosts(userId) {} // it returns a promise
function fetchComments(postId) {} // it returns a promise

// Start the promise chain
fetchUser(1)
  .then((user) => {
    console.log("User:", user);
    return fetchPosts(user.id); // Return the next promise
  })
  .then((posts) => {
    console.log("Posts:", posts);
    return fetchComments(posts[0].id); // Return the next promise
  })
  .then((comments) => {
    console.log("Comments:", comments);
  })
  .catch((error) => {
    console.error("Error:", error);
  });
```

- **Promise nesting**: Placing one Promise inside another's `.then()` method. This can lead to more indented code and can make it **harder to read**, especially when dealing with multiple asynchronous operations.

```javascript
function fetchUser(userId) {} // it returns a promise
function fetchPosts(userId) {} // it returns a promise
function fetchComments(postId) {} // it returns a promise

// Start the promise nesting
fetchUser(1)
  .then((user) => {
    console.log("User:", user);
    return fetchPosts(user.id).then((posts) => {
      console.log("Posts:", posts);
      return fetchComments(posts[0].id).then((comments) => {
        console.log("Comments:", comments);
      });
    });
  })
  .catch((error) => {
    console.error("Error:", error);
  });
```

## Promise methods - .all, .allSettled, .race, and .any

- ## Promise.all()

  - MDN Web Docs: "The Promise.all() static method takes an iterable of promises as input and returns a single Promise. This returned promise fulfills when all of the input's promises fulfill (including when an empty iterable is passed), with an array of the fulfillment values. **It rejects when any of the input's promises rejects, with this first rejection reason.**"
  - This highlighted phrase is one of the most important aspects to consider when using Promise.all().

  ```javascript
  const promise1 = Promise.resolve(3);
  const promise2 = 42;
  const promise3 = new Promise((resolve, reject) => {
    setTimeout(resolve, 100, "foo");
  });

  Promise.all([promise1, promise2, promise3]).then((values) => {
    console.log(values);
  });
  // Expected output: Array [3, 42, "foo"]
  ```

- ## Promise.allSettled()

  - MDN Web Docs: "The Promise.allSettled() static method takes an iterable of promises as input and returns a single Promise. This returned promise fulfills when all of the input's promises settle (including when an empty iterable is passed), with **an array of objects that describe the outcome of each promise.**"
  - This highlights the most useful aspect when you need to manage each of your promises independently, handling success cases and errors individually for each promise inside Promise.allSettled().

  ```javascript
  const promise1 = Promise.resolve(3);
  const promise2 = new Promise((resolve, reject) =>
    setTimeout(reject, 100, "foo")
  );
  const promises = [promise1, promise2];

  Promise.allSettled(promises).then((results) =>
    results.forEach((result) => console.log(result.status))
  );

  // Expected output:
  // "fulfilled"
  // "rejected"
  ```

- ## Promise.race()

  - Returns when the first promise settles, (error or success).
  - Imagine you're waiting for responses from multiple servers. You use Promise.race to get the response from the server that responds the quickest.

  ```javascript
  const promise1 = new Promise((resolve, reject) => {
    setTimeout(resolve, 500, "one");
  });
  const promise2 = new Promise((resolve, reject) => {
    setTimeout(resolve, 100, "two");
  });

  Promise.race([promise1, promise2]).then((value) => {
    console.log(value);
    // Both resolve, but promise2 is faster
  });
  // Expected output: "two"
  ```

- ## Promise.any()

  - Returns when the first promise resolves (success). If all promises fails it went to `.catch()`.
  - You're fetching data from multiple APIs, but you're interested in the data from the first API that responds successfully. You use Promise.any to handle this scenario.

  ```javascript
  const fetchFromAPI1 = fetch("https://api.example.com/endpoint1");
  const fetchFromAPI2 = fetch("https://api.example.com/endpoint2");
  const fetchFromAPI3 = fetch("https://api.example.com/endpoint3");

  Promise.any([fetchFromAPI1, fetchFromAPI2, fetchFromAPI3])
    .then((response) => response.json())
    .then((data) => {
      console.log("Received data from the first successful API call:", data);
    })
    .catch((error) => {
      console.error("All API calls failed:", error);
    });
  ```

## References

- [MDN - Using promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
- [Javascript.info - Promise chaining](https://javascript.info/promise-chaining)
- [w3schools - JavaScript Promises](https://www.w3schools.com/js/js_promise.asp)

## Thanks for Reading!

- Feel free to reach out if you have any questions, feedback, or suggestions. Your engagement is appreciated!
