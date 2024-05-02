# ![React - Pets Front-End - Read - Index](./assets/hero.png)

**Learning Objective**: By the end of this lesson, the learner be able to connect a React application to an API to fetch and display a list of items, specifically showing all pets on the screen

## Read - Index

In this section of the lesson, we want to complete the index read actions. For practice, let's review requirements laid out in the intro:

### Read all Pets

- Display all Pets in the UI

And translate it into a User Story:

- As a user, I want to be able to view all of the Pets.

❗ If you deleted all of the pets in the database at the end of the Express REST API lesson, open up Postman and add a few new pets so that you'll have data for this section of the lesson. ❗

### Creating the Service

First, let's create the service function that will make a [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) call to the API. This is a pattern that will get more familiar with practice, so we'll take some time to detail each step for this first function.

Inside of `petService.js`, create a new async function called `index`:

```js
// src/services/petService.js

const index = async () => {};
```

Why async? All of the service functions we'll build for this lesson will be making asynchronous fetch calls to the API, so in order to `await` the response from our fetch call, we'll need to use an `async` function.

Next, let's go ahead and code the fetch call. Remember that in the setup we created a `BASE_URL` variable and assigned it to the base url of our API (in this case, http://localhost:3000/pets). Since we're making a GET request (the default method for `fetch`) to `/pets`, we just need to pass `BASE_URL` to our fetch call:

```js
// src/services/petService.js

const index = async () => {
  const res = await fetch(BASE_URL);
};
```

Great, in this code we're getting back a Response object and assigning it to a new `res` variable.

In order to get this Response object to resolve to a useable JavaScript object, we need to invoke the [.json()](https://developer.mozilla.org/en-US/docs/Web/API/Response/json) method on it. We also know that, on the front end, we'll want to use this data to populate a list of Pets, so we'd better return the parsed `res` data out of this function:

```js
// src/services/petService.js

const index = async () => {
  const res = await fetch(BASE_URL);
  return res.json();
};
```

Our final step is to handle potential errors - let's wrap our fetch call in a `try/catch` block, and log any errors to the console:

```js
// src/services/petService.js

const index = async () => {
  try {
    const res = await fetch(BASE_URL);
    return res.json();
  } catch (err) {
    console.log(err);
  }
};
```

In summary, this function:

- Uses `fetch` to make a `GET` request to the base URL.
- Uses the `.json()` method to parse the response.
- Uses a `try...catch` block to handle any errors.
- Returns the parsed response.

Make sure to export this function! At this stage, your `petService.js` file should look like this:

```js
// src/services/petService.js

const BASE_URL = `${import.meta.env.VITE_BACK_END_SERVER_URL}/pets`;

const index = async () => {
  try {
    const res = await fetch(BASE_URL);
    return res.json();
  } catch (err) {
    console.log(err);
  }
};

export { index };
```

### Setting State with the service

Next, let's work on the UI that will make the service call and render the data that it returns. In order to do this, we'll be making use of two hooks - `useState` and `useEffect`. Let's go ahead and import those in `App.jsx` now:

```jsx
// src/App.jsx

import { useState, useEffect } from 'react';
```

Inside of `App.jsx` create a new state variable called `petList` to hold the pets. We know that the pets data we'll get back from our API should be an array, so we'll give `petList` the initial value of an empty array:

```jsx
// src/App.jsx

import { useState, useEffect } from 'react';

const App = () => {
  const [petList, setPetList] = useState([]);

  return <h1>Hello world!</h1>;
};

export default App;
```

Next, let's work on creating a `useEffect` hook to call the `index` service function.

First, we'll need to import our `index` function. For now, we only have a single function being exported from `petService`, but eventually we'll have multiple functions all coming from the same file. To save ourselves from having to refactor later, let's go ahead and just import all (`*`) of the exported functions as methods on a new `petService` object:

```jsx
// src/App.jsx

import { useState, useEffect } from 'react';

import * as petService from './services/petService';
```

Now, all of our service functions will live in one tidy object in `App.js`, regardless of how many we export.

Next up, let's work on the `useEffect`.

Big picture, when the page loads we want to make a fetch call to our API, courtesy of our index service. The useEffect hook works great for that purpose, with one notable downside. Our index function is asynchronous, which means that we'll need to `await` any data it may return. Unfortunately, useEffect's `setup` function (that's the anonymous function that houses our effects logic) cannot be made to be `async`:

```js
// This will not work
useEffect(async () => {
  await something;
}, []);
```

So, how can we get around this limitation? Other than abandoning `async/await` syntax and returning to the potential callback hell of `.then()` syntax, a common workaround is to simply create a new `async` function _inside_ of the `setup` function, and then invoke it immediately afterwords:

```js
// This WILL work
useEffect(() => {
  const newFunc = async () => {
    const data = await service();
  };
  newFunc();
}, []);
```

> 🧠 When making a function that will be invoked immediately after creation, using an [Immediately Invoked Function Expression](https://developer.mozilla.org/en-US/docs/Glossary/IIFE) (or IIFE) is also an option.

With that in mind, we can build out our useEffect. Once we have the data back from our index function, we'll use it to set our `petList` state:

```jsx
// src/App.jsx

const App = () => {
  const [petList, setPetList] = useState([]);

  // Create a new useEffect
  useEffect(() => {
    // create a new async function
    const fetchPets = async () => {
      // call on the index function
      const data = await petService.index();
      // Set petList state to the returned pets data
      setPetList(data.pets);
    };
    // invoke the function
    fetchPets();
    // add an empty dependency array to the `useEffect` hook.
  }, []);

  return <h1>Hello world!</h1>;
};
```

### Error Handling

Let's take a second to talk about error handling for our application. In the backend, the following Express controller function is responding to our request to `/pets`:

_Express server code:_

```js
// READ - GET - /pets
router.get('/', async (req, res) => {
  try {
    const foundPets = await Pet.find();
    res.status(200).json(foundPets);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
```

From this, we know that our response will be one of two things:

- An array of pets
- An object with an error property

In the instance that the API sends back an error object, we know something went wrong! Using an `if` statement, we can check if the response has an `error` property, which will only be the case if our controller caught an error. If so, we'll throw a new `Error` - this time in our handler function - passing the error contents to the `catch` block.

Throwing an error also prevents statements after the `throw` from being executed, so we don't have to worry about accidentally setting `petList` state to an error object.

```jsx
// src/App.jsx

useEffect(() => {
  const fetchPets = async () => {
    try {
      const pets = await petService.index();
      // Don't forget the pass the error object to the new Error
      if (pets.error) {
        throw new Error(pets.error);
      }

      setPetList(pets);
    } catch (error) {
      // Log the error object
      console.log(error);
    }
  };
  fetchPets();
}, []);
```

In the `fetchPets`'s `catch` block, we simply log the error. For this app, having any server-side issues show up in the client-side console is enough to help make debugging easier. Big picture, this type of error handling setup also allows us to provide error feedback in the UI as well - however that's not within the scope of this particular lesson.

Check the state of your application in the React Dev tools to confirm your list of pets exists.

### Displaying the Data

Next, let's set up our UI to display the pets in our state.

Inside of `src/components`, create a new file called `PetList.jsx`.

In `PetList.jsx`, create a new functional component also called `PetList`. Make sure it accepts `props` as an argument. For now, it will simply return a header element:

```jsx
// src/components/PetList.jsx

const PetList = (props) => {
  return <h1>Pet List</h1>;
};

export default PetList;
```

Next, we'll use `.map()` to create a list of `<li>` elements. Each `<li>` element will display the `name` property of each pet in `petList`. React will also insist that each top-level element in a list must have a unique key - fortunately, MongoDB generates an id (as `._id`) for us when creating a resource, so we can use `pet._id` for the key:

```jsx
// src/components/PetList.jsx

const PetList = (props) => {
  const pets = props.petList.map((pet) => <li key={pet._id}>{pet.name}</li>);

  return <h1>Pet List</h1>;
};
```

Inside the return statement of `PetList.jsx`, create a `ul` element and render the list of pets inside of it:

```jsx
// src/components/petList.jsx

return (
  <div>
    <h1>Pet List</h1>
    <ul>{pets}</ul>
  </div>
);
```

### Using the PetList component

Let's use our new component by importing `PetList` into `App.jsx`, and rendering it to the browser.

Import the `PetList` component into `App.jsx`.

```jsx
import PetList from './components/PetList';
```

Inside of the return statement of `App.jsx` render the `PetList` component. Make sure to pass the `petList` state variable as a prop to the `PetList` component.

```jsx
// src/App.jsx

import { useState, useEffect } from 'react';
import * as petService from './services/petService';

import PetList from './components/PetList';

const App = () => {
  const [petList, setPetList] = useState([]);

  useEffect(() => {
    const fetchPets = async () => {
      try {
        const pets = await petService.index();

        if (pets.error) {
          throw new Error(pets.error);
        }

        setPetList(pets);
      } catch (error) {
        console.log(error);
      }
    };
    fetchPets();
  }, []);

  return <PetList petList={petList} />;
};

export default App;
```

Test it out in the browser and make sure you are seeing the list of pets. If not, check that your server is running, and that your database is populated with at least one pet!

### No pets?

Finally, let's handle the case where there are no pets to display.

This can be handled with a ternary operator, checking to see if the pets array has a length of 0. If so, we display a message that there are no pets to display, and if not we instead display the list of pets:

```jsx
// src/components/petList.jsx

return (
  <div>
    <h1>Pet List</h1>
    {!props.petList.length ? <h2>No Pets Yet!</h2> : <ul>{pets}</ul>}
  </div>
);
```
