# 			                                  Nonceblox - Round 1



## Frontend

### 1) Write a custom hook to manage state for <input> tags.
We can create a custom hook that accepts type of input and initial value as parameter and implement State change functions inside the custom hook,.
An implementation of this is below -:

useFormField.js Hook

![image](https://user-images.githubusercontent.com/82509612/162847628-3a8cf782-5620-42db-b254-e19683af7e12.png)


App.js

![image](https://user-images.githubusercontent.com/82509612/162847663-952dade2-df72-49ea-8150-71be902d4e17.png)

Here, I have created 3 inputs and their state is managed by the custom hooks. We can also add other fields such as label,name,errorMessage and isValid field to the custom hook and use the hook in our app.

### 2) What will be the output for the below code if myState is incremented 5 times before scrolling?
```javascript
const [state, setState] = React.useState(0);
const listener = () => {
console.log(`State: ${state}`);
};
React.useEffect(() => {
window.addEventListener("scroll", listener);
}, []);
```
The updated state of the component will be 5, but still the output will always be “Scroll: 0” because when the addEventListener is binded to it, the state of the component is 0, and the useEffect with no dependancy only runs for the initial state. i.e when component gets mounted. And even if there was state in the dependancy of the useEffect, there would be 2 addEventListeners attatched to scrolling as there is no cleanup function in the useEffect, so the output would be “State: 0”and “State: 5”.

### 3) How to implement componentWillUnmount in functional components?
componentWillUnmount in functional component is triggered by the cleanup function in the useEffect, i.e a return function which has the logic for when a component unmounts.
Its implementation is as follows -:

![image](https://user-images.githubusercontent.com/82509612/162847710-faa1950a-120c-435d-b55c-5f0196a5a59a.png)

### 4) What is the difference between the below snippets?
```javascript
const [state, setState] = useState(0);
setState((state) => state + 1);
```
```javascript
const [state, setState] = useState(0);
setState(state + 1);
```

Both the code snippets give the same result, but using the prevState argument can be helpful in the case of the state being in the form of an object and when you need to use or change just some properties of the object instead of making a new object and setting it to the state.
This is implemented as follows -:

![image](https://user-images.githubusercontent.com/82509612/162847739-b6eb4365-8bca-4f73-8b9a-381126e8125e.png)

### 5) How will you wait for all the promises to resolve/reject that are executed inside a loop? Answer with an example.
When there is a promises in a for loop, the loop does not wait for the promises to resolved or rejected. For using a promise in a for loop, we have to use async await. This is done by making the containing function of the loop to be async and using await for the promise call.
An example of this -:

![image](https://user-images.githubusercontent.com/82509612/162847767-f76507d6-b09e-4453-b538-44d9d19d3330.png)

Another efficeint way of using promises in a loop is by using Promise.all method, for this we make an array of promises and push the promises using the loop, and then use the promise.all function to return a single promise and it resolves when all the promises in the array have been resolved

![image](https://user-images.githubusercontent.com/82509612/162847794-bdb2b7d0-394d-42fc-bb7e-400fd77d1194.png)

### 6) Which component will be rendered when you visit /products ?
```javascript
<Swtich>
  <Route path="/" component={Home} />
  <Route path="/products" component={Product} />
</Switch>
```
Here, path of the home page is ‘/’ and the component rendered against it is Home. When you visit ‘/products’, you would just see the “Home” component because v5 of the react-router always finds the first matching route, in this case “/” of “/products” is matched with the “/” route and hence the Home component gets rendered. In order to get the correct result, i.e to render the Product component on the “/products” route, we would have to put “exact” phrase in the “/” route, i.e. 
```javascript
<Route exact path="/" component={Home} />
```


## Backend

### 1) What will be the order of the output?
```javascript
const fs = require("fs");
fs.readFile("/poem.md", (err, data) => {
if (err) return;
console.log(data);
});
setTimeout(() => console.log("Runner!"), 0);
console.log("Hello");
```
There will be 3 processes running when the server is started, asyncronously reading the file, the asyncrononus setTimeout function and the main thread which console logs “Hello”. So, “Hello” will be logged first and then the first process which gets completed first out of the Timeout or Reading file will get executed (In my opinion reading a file takes longer than the 0 second timeout in setTimeout) and then the last one

### 2) Write a program to run 7 threads and create server instances for each thread using the http module with master and child relationships.
Child.js

![image](https://user-images.githubusercontent.com/82509612/162847815-809abf11-0347-4aba-8341-0e3dafbc3f68.png)

Index.js

![image](https://user-images.githubusercontent.com/82509612/162847828-731290db-6bae-4410-8e4c-f8ac0751da8c.png)

Output 

![image](https://user-images.githubusercontent.com/82509612/162847843-8846a183-bce0-4b7f-b478-b57554b482d2.png)

### 3) Write a middleware that does the following tasks:
A. Checks the given JWT and authenticates it. (Assume you’re getting the secret
key from the env so you can use process.env.SECRET_KEY)
B. If the authentication is correct pass the control to the controller (write the required
logic)
C. Write required logic to handle the errors as well and return an adequate
response

authMiddleware.js

![image](https://user-images.githubusercontent.com/82509612/162847854-a8fd4e00-f1e0-4f3c-a232-3b01e76c0b3f.png)

### 4) What will happen with this command?
```javascript
db.sessionlog.createIndex({ lastUpdateTime: 1 }, { expireAfterSeconds: 1800 });
```
The command creates an index in the sessionlog collection of the db
So, the index key is “lastUpdateTime” and its value being “1”
And, { expireAfterSeconds: 1800 } is the options object that means the db retains documents in the collection for 1800 seconds

### 5) What is the fundamental difference between db.copyDatabase() and
db.repairDatabase()?
db.copyDatabse Copies a database from one mongod instance to the same mongod instance or from one mongod instance to another mongod instance within the current mongod. The fromdb and todb parameters are passed to the mongo shell method db.copyDatabase()
repairDatabase is a database repair utility. That is, it tries to clear up any corrupt documents in the database that might be preventing MongoDB from starting up. RepairDatabase could theoretically remove documents from the database, but how it works in detail depends on your storage engine.

### 6) Write the query to perform the following on a MongoDB collection name “Wonder”
a) Specify the maximum number of documents for a collection
-> db.createCollection( "Wonder", { capped: true, size:5242880, max:5000} )
b) Check if the collection is capped or not 
-> db.Wonder.isCapped()
c) Converting a collection to capped Wonder collection
-> db.runCommand({"convertToCapped": " Wonder", size: 5242880});
