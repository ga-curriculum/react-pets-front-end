# ![React Front End Pets Lab - Read](./assets/exercise-read.png)

## Read

In this section of the lab, we want to complete both the index and show read actions. For practice, let's take the minimum requirements we laid out in the intro: 

#### Read all Pets
  - Display all Pets in the UI

#### Read a single Pet
  - Add a button to view the details of a single Pet
  - Conditionally render the details of a single Pet

and translate them into User Stories: 

- As a user, I want to be able to view all of the Pets.

- As a user, I want to be able to click a button to view the details of a single Pet. 

## Step 1: Read all Pets

For this first step, want a list of pets to be displayed in the UI. 

❗ If you deleted all of the pets in the database at the end of the Express REST API lesson, open up Postman and add a few new pets so that you'll have data for this section of the lab. ❗

### Creating the Service

First, let's create the service function that will make a [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) call to the API. This is a pattern that will get more familiar with practice, so we'll take some time to detail each step for this first function. 

Inside of `petService.js`, create a new async function called `index`: 

```js
// src/services/petService.js

async function index() {

}
```

Why async? All of the service functions we'll build for this lab will be making asynchronous fetch calls to the API, so in order to `await` the response from our fetch call, we'll need to use an `async` function. 

Next, let's go ahead and code the fetch call. Remember that in the setup we created a `BASE_URL` variable and assigned it to the base url of our API (in this case, http://localhost:3000/pets). Since we're making a GET request (the default method for `fetch`) to `/pets`, we just need to pass `BASE_URL` to our fetch call: 

```js
// src/services/petService.js

async function index() {
  const res = await fetch(BASE_URL)
}
```

Great, we're getting back a Response object and assigning it to a new `res` variable. 

In order to get this Response object to resolve to a useable JavaScript object, we need to invoke the [.json()](https://developer.mozilla.org/en-US/docs/Web/API/Response/json) method on it. We also know that, on the front end, we'll want to use this data to populate a list of Pets, so we'd better return the parsed `res` data out of this function: 

```js
// src/services/petService.js

async function index() {
  const res = await fetch(BASE_URL)
  return res.json()
}
```

Our final step is to handle potential errors - let's wrap our fetch call in a `try/catch` block, and log any errors to the console: 

```js
// src/services/petService.js

async function index() {
  try {
    const res = await fetch(BASE_URL)
    return res.json()
  } catch (err) {
    console.log(err)
  }
}
```

In summary, this function:
  - Uses `fetch` to make a `GET` request to the base URL.
  - Uses the `.json()` method to parse the response.
  - Uses a `try...catch` block to handle any errors.
  - Returns the parsed response.

Make sure to export this function! At this stage, your `petService.js` file should look like this: 

```js
// src/services/petService.js

const BASE_URL = `${import.meta.env.VITE_BACK_END_SERVER_URL}/pets`

async function index() {
  try {
    const res = await fetch(BASE_URL)
    return res.json()
  } catch (err) {
    console.log(err)
  }
}

export {
  index,
}

```

### Setting State with the service

Next, let's work on the UI that will make the service call and render the data that it returns. In order to do this, we'll be making use of two hooks - `useState` and `useEffect`. Let's go ahead and import those now: 

```jsx
// src/App.jsx

import {useState, useEffect} from 'react'
```

Inside of `App.jsx` create a new state variable called `petList` to hold the pets. We know that the pets data we'll get back from our API should be an array, so we'll give `petList` the initial value of an empty array: 

```jsx
// src/App.jsx

import {useState, useEffect} from 'react'

const App = () => {
  const [petList, setPetList] = useState([])

  return (
    <h1>Hello world!</h1>
  );
}

export default App
```

Next, let's work on creating a `useEffect` hook to call the `index` service function. 

First, we'll need to import our `index` function. For now, we only have a single function being exported from `petService`, but eventually we'll have multiple functions all coming from the same file. To save ourselves from having to refactor later, let's go ahead and just import all (`*`) of the exported functions as methods on a new `petService` object: 

```jsx
// src/App.jsx

import {useState, useEffect} from 'react'

import * as petService from './services/petService'
```

Now, all of our service functions will live in one tidy object in `App.js`, regardless of how many we export. 

Next up, let's work on the useEffect. 

Big picture, when the page loads we want to make a fetch call to our API, courtesy of our index service. The useEffect hook works great for that purpose, with one notable downside. Our index function is asynchronous, which means that we'll need to `await` any data it may return. Unfortunately, useEffect's `setup` function (that's the anonymous function that houses our effects logic) cannot be made to be `async`: 

```js
// This will not work
useEffect(async() => {await something}, [])
```

So, how can we get around this limitation? Other than abandoning `async/await` syntax and returning to the potential callback hell of `.then()` syntax, a common workaround is to simply create a new `async` function _inside_ of the `setup` function, and then invoke it immediately afterwords: 

```js
// This WILL work
useEffect(() => {
  const newFunc = async () => {
    const data = await service()
  }
  newFunc()
}, [])
```

> 🧠 When making a function that will be invoked immediately after creation, using an [Immediately Invoked Function Expression](https://developer.mozilla.org/en-US/docs/Glossary/IIFE) (or IIFE) is also an option. 

With that in mind, we can build out our useEffect. Once we have the data back from our index function, we'll use it to set our `petList` state: 

```jsx
// src/App.jsx

const App = () => {
  const [petList, setPetList] = useState([])

  // Create a new useEffect
  useEffect(() => {
    // create a new async function
    const fetchPets = async () => {
      // call on the index function
      const data = await petService.index()
      // Set petList state to the returned pets data
      setPetList(data.pets)
    }
    // invoke the function
    fetchPets()
  // add an empty dependency array to the `useEffect` hook.
  }, [])

  return (
    <h1>Hello world!</h1>
  );
}
```

### Displaying the Data

Next, let's set up our UI to display the pets in our state.

Inside of `src/components`, create a new file called `PetList.jsx`.

In `PetList.jsx`, create a new functional component also called `PetList`. Make sure it accepts `props` as an argument. For now, it will simply return a header element: 

```jsx
// src/components/PetList.jsx

const PetList = (props) => {
  return (
    <h1>Pet List</h1>
  );
}

export default PetList;
```

Next, we'll use `.map()` to create a list of `<li>` elements. Each `<li>` element will display the `name` property of each pet in `petList`. React will also insist that each top-level element in a list must have a unique key - fortunately, MongoDB generates an id (as `._id`) for us when creating a resource, so we can use `pet._id` for the key: 

```jsx
// src/components/PetList.jsx

const PetList = (props) => {
  const pets = props.petList.map((pet) => (
    <li key={pet._id}>{pet.name}</li>
  ));

  return (
    <h1>Pet List</h1>
  );
}
```

Inside the return statement of `PetList.jsx`, create a `ul` element and render the list of pets inside of it:

```jsx
// src/components/petList.jsx

  return (
    <div>
      <h1>Pet List</h1>
      <ul>
        {pets}
      </ul>
    </div>
  );
```

Refresh the browser and check to see that the pets data is being displayed correctly before moving on to the next step. 

Finally, let's handle the case where there are no pets to display. 

This can be handled with a ternary operator, checking to see if the pets array has a length of 0. If so, we display a message that there are no pets to display, and if not we instead display the list of pets:

```jsx
// src/components/petList.jsx

  return (
    <div>
      <h1>Pet List</h1>
      {!props.petList.length 
          ? <h2>No Pets Yet!</h2> 
          : <ul>{pets}</ul>
      }
    </div>
  );
```

### Using the PetList component

Let's add the finishing touches to this step by importing the `PetList` component into `App.jsx`, and rendering it.

Import the `PetList` component into `App.jsx`.

```jsx
import PetList from './components/PetList'
```

Inside of the return statement of `App.jsx` render the `PetList` component. Make sure to pass the `petList` state variable as a prop to the `PetList` component.

```jsx
// src/App.jsx
import { useState, useEffect } from 'react'
import * as petService from './services/petService'

import PetList from './components/PetList'

const App = () => {
  const [petList, setPetList] = useState([])

  useEffect(() => {
    const fetchPets = async () => {
      const data = await petService.index()
      setPetList(data.pets)
    }
    fetchPets()
  }, [])

  return (
    <PetList petList={petList} />
  )
}

export default App
```

Test it out in the browser and make sure you are seeing the list of pets. If not, check that your server is running, and that your database is populated with at least one pet! 


## Step 2: Read a single Pet

For this next section we are going to show a single pet in the UI. We want to be able to click on a pet and see the details of that pet.

- Inside of `PetList.jsx` where you `.map()` over the pets, wrap each `li` element in an `a` tag.
  - Move the `key` element to the `a` element.
  - Add an `onClick` event to the `a` element. Add an inline function that logs the clicked pet to the console.
- Inside of `App.jsx` create a new function to handle the click event we set up in the `PetList` component.
  - Create a new state variable to hold the single pet. This should be an object that represents the pet or `null` if no pet is selected.
  - This function should accept the pet as an argument.
  - This function should set the selected pet to the state variable.
  - Pass this function as a prop to the `PetList` component and call it when a pet is clicked. This should replace the log statement we added in the `PetList` component.

Before moving on test that you can click on a pet and have the state variable update with the selected pet. You can do this the Chrome React Dev Tools or log statements.

Now that we have the selected pet in the state variable we can conditionally render the details of the pet in the UI.

- Inside of `components` create a new file called `PetDetails.jsx`.
  - In this file create a function component called `PetDetails`.
  - This component should accept `props` as an argument.
  - From the `PetDetails` component we should render the details of the pet.
  - Make sure to export the `PetDetails` component at the end of the file.
- Back in `App.jsx` import the `PetDetails` component.
  - Conditionally render the `PetDetails` component based on the state of the selected pet.

Test it out in the browser and make sure you can click on a pet and see the details of that pet.
