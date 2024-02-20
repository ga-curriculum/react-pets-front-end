# ![[React Front End Pets Lab] - Read](./assets/tktk-hero.png)

## Read

In this lab we wan to complete both the index and show read actions. This means we want to:

- [ ] Read all Pets
  - [ ] Display all Pets in the UI
- [ ] Read a single Pet
  - [ ] Add a button or wrap Pet in an `a` element to view the details of a single Pet
  - [ ] Conditionally render the details of a single Pet

### Step 1: Read all Pets

Let's read all the pets. We want a list of pets to be displayed in the UI.

- Create a new function inside of `petService.js` called `getAllPets`. This function should:
  - Use `fetch` to make a `GET` request to the base URL.
  - Use a `try...catch` block to handle any errors.
  - Use `.json()` method to parse the response. Ex: `res.json()`.
  - Return the parsed response.
  - Make sure to export this function.
- Inside of `App.jsx` create a new state vaiable to hold the pets and one to hold any error messages. This should be an array and a string respectively.
- Still inside of `App.jsx` create a `useEffect` hook to call the `getAllPets` function and set the state variable to the response.
  - Make sure to import the `getAllPets` function from `petService.js`.
  - Make sure to add an empty dependency array to the `useEffect` hook.
  - Make sure to handle any errors. You can do this by setting the error message state variable to the error message and displaying it in the UI.

We can use our Chrome React Dev tools or log statements to make sure we are getting the data we expect. Once we have the data we can move on to displaying it in the UI.

We have our response from the server to let's set up our UI to display the pets.

- Inside of `components` create a new file called `PetList.jsx`.
- Create a new file called `PetList.jsx` inside of the `components` folder. This should be a function component.
  - Make sure to accept `props` as an argument.
- Using `.map()` and the `props` render a list of pets in the UI. This should be a list of `li` elements.
  - You will need a `key` for each `li` element. This should be the `id` of the pet.
- Inside the return statement of `PetList.jsx` create a `ul` element and render the list of pets inside of it.
  - Handle the case where there are no pets to display. This should be handled as a ternary checking to see if the length of the pets array is greater than 0. If so we display the list of pets, if not we display a message that there are no pets to display.
- Make sure to export the `PetList` component at the end of the file.

Let's add the finishing touches here and import the `PetList` component into `App.jsx` and render it.

- Import the `PetList` component into `App.jsx`.
- Inside of the return statement of `App.jsx` render the `PetList` component. 
  - Make sure to pass the pets state variable as a prop to the `PetList` component.
  - If you used the `props` object to map over the pets in the `PetList` component, make sure you name the prop you are passing to the `PetList` component the same as the prop you are using in the `PetList` component.

Test it out in the browser and make sure you are seeing the list of pets.

### Step 2: Read a single Pet

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