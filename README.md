### Callback Functions
* A callback is a function passed as an argument to another function, which gets invoked after the main function completes its execution. You pass the callback function to the main function as an argument, and once the main function finishes its task, it calls the callback function to deliver a result.

```js
setTimeout(function () {
    console.log("Timer");
}, 1000) // first argument is callback function and second is timer.
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
  // x y timer
  ```

* However, there are also some drawbacks to using callbacks:

* #### Callback Hell:
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

* #### Inversion of control:
This happens when the control of program is no longer in our hands, that may cause some problems like:

* 		A Callback may be called multiple times.
* 		A Callback would never get called.
* 		A Callback may be called synchronously.

* #### HOW DO I SOLVE THIS PROBLEM ?
* ### Using Promises
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

//📌 Till now we have been using Promise.then/.catch to handle promise.
// Now let's see how async await can help us and how it is different

// The rule is we have to use keyword await in front of promise.
async function handlePromise() {
  const val = await p;
  console.log(val);
}
handlePromise(); // Promise resolved value!!
```

* So we are the ones in control this time and can easily make sure that our function gets called only once.
* To avoid callback hell (Pyramid of doom) => We use promise chaining. This way our code expands vertically instead of horizontally. Chaining is done using '.then()'

```js
// Callback Hell Example
createOrder(cart, function (orderId) {
  proceedToPayment(orderId, function (paymentInf) {
    showOrderSummary(paymentInf, function (balance) {
      updateWalletBalance(balance);
    });
  });
});
// And now above code is expanding horizontally and this is called pyramid of doom.
// Callback hell is ugly and hard to maintain.

// Promise fixes this issue too using `Promise Chaining`
// Example Below is a Promise Chaining
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


* ## async await
* 
