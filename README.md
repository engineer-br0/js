### Callback Functions
* A callback is a function passed as an argument to another function, which gets invoked after the main function completes its execution. You pass the callback function to the main function as an argument, and once the main function finishes its task, it calls the callback function to deliver a result.

```js
setTimeout(function () {
    console.log("Timer");
}, 1000) // first argument is callback function.
```

#### Why use Callbacks?
* JS is a synchronous and single threaded language. But due to callbacks, we can do async things in JS. Callbacks enable you to handle the outcomes of asynchronous operations like network requests or database queries, take time to finish in a non-blocking way. This means your program can keep running while the operation is ongoing.

* ```js
  setTimeout(function () {
    console.log("timer");
  }, 5000);
  function x(y) {
    console.log("x");
    y();
  }
  x(function y() {
    console.log("y");
  });
  // OUTPUT- x y timer
  ```

* However, there are also some drawbacks to using callbacks:

#### Callback Hell:
* When a callback function is kept inside another function, which in turn is kept inside another function. (in short, a lot of nested callbacks). This causes a pyramid of doom structure causing our code to grow horizontally, making it tough to manage our code.

```js
api.createOrder(cart, function () {
  api.proceedToPayment(function () {
    api.showOrderSummary(function () {
      api.updateWallet();
    });
  });
});
// Callback Hell
```

#### Inversion of control:
This happens when the control of program is no longer in our hands, that may cause some problems like:

* 		A Callback may be called multiple times.
* 		A Callback would never get called.
* 		A Callback may be called synchronously.

####  How to solve these problems ?
### Using Promises
* A promise is an object that represents eventual completion/failure of an asynchronous operation.
* A promise has 3 states: pending | fulfilled | rejected.
* As soon as promise is fulfilled/rejected => It updates the empty object which is assigned undefined in pending state.
* A promise resolves only once and it is immutable. 

```js
const p = new Promise((resolve, reject) => {
  resolve('Promise resolved value!!');
})

function getData() {
  p.then(res => console.log(res));
}

getData(); // Promise resolved value!!
```

* So we are the ones in control this time and can easily make sure that our function gets called only once.
* To avoid callback hell (Pyramid of doom) => We use promise chaining. This way our code expands vertically instead of horizontally. Chaining is done using '.then()'

```js
// Callback Hell
createOrder(cart, function (orderId) {
  proceedToPayment(orderId, function (paymentInf) {
    showOrderSummary(paymentInf, function (balance) {
      updateWalletBalance(balance);
    });
  });
});

// Promise fixes the Callback hell issue using `Promise Chaining`
createOrder(cart)
  .then(function (orderId) {
    proceedToPayment(orderId);
  })
  .then(function (paymentInf) {
    showOrderSummary(paymentInf);
  })
  .then(function (balance) {
    updateWalletBalance(balance);
  });
```


### async await
* Async and Await in JavaScript are powerful keywords used to handle asynchronous operations with promises.
* Async functions always return a promise. If a value is returned that is not a promise, JavaScript automatically wraps it in a resolved promise.
* The await keyword is used to wait for a promise to resolve. It can only be used within an async block.
* Await makes the code wait until the promise returns a result, allowing for cleaner and more manageable asynchronous code.
  
```js
const p = new Promise((resolve, reject) => {
  resolve('Promise resolved value!!');
})

async function handlePromise() {
  const val = await p;
  console.log(val);
}
handlePromise(); // Promise resolved value!!
```

### Real World example of async/await (fetch API)

```js
async function handlePromise() {
  try {
    const data = await fetch('https://api.github.com/users/alok722');
    const res = await data.json();
    console.log(res);
  } catch (err) {
    console.log(err)
  }
};
handlePromise()
```
