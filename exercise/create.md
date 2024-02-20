# ![[React Front End Pets Lab] - Create](./assets/tktk-hero.png)

## Create

In this section we want to complete the following tasks:

- [ ] Create a new Pet
  - [ ] Conditionally render a form to create a new Pet
  - [ ] Handle the form submission
  - [ ] Display the new Pet in the UI

### Step 1: Conditionally Render the Form

- Create a new component called `PetForm.jsx`. This should be in the `components` folder.
  - Inside of `PetForm.jsx` create a function component that accepts `props` as an argument.
  - Inside of the return statement of `PetForm.jsx` create a `form` element that will be used to create a new pet.
  - Make sure to export the `PetForm` component at the end of the file.
- Create a state variable inside of `App.jsx` to represent the form being open or closed. This should be a boolean value.
- Still in `App.jsx` create a function to toggle the above state variable.
  - Think through this toggle. Do we always want the form to be visible at the start of the application? What do we want the default state to be?
- Now that we have a function to toggle the state variable, create a button that will call this function when clicked. This should be located in the `App.jsx` file.
  - This button should conditionally render the form. If the form is open, the button should say "Close Form". If the form is closed, the button should say "Add Pet".

Test it out in the browser and make sure you can toggle the form open and closed.

### Step 2: Handle the Form Submission and Display the New Pet

- Inside of `petService.js` create a new function called `createPet`. This function should:
  - Accept a pet object as an argument.
  - Use `fetch` to make a `POST` request to the base URL.
  - Use a `try...catch` block to handle any errors.
  - Use `.json()` method to parse the response. Ex: `res.json()`.
  - Return the parsed response.
  - Make sure to export this function.
- In `App.jsx` creact a new function to handle the creation of a new pet.
  - This function should accept the form data as an argument.
  - This function should call the `createPet` function and pass the form data to it.
  - This function should update the state variable that holds the pets with the new pet.
  - This function should close the form.
  - This function should handle any errors.
- Pass this new function as a prop to the `PetForm` component.
- Inside of `PetForm.jsx` create a new function to handle the form submission.
  - This function should accept the `event` as an argument.
  - This function should prevent the default behavior of the form.
  - This function should create a new pet object using the form data.
  - This function should call a function passed down from `App.jsx` to handle the form submission.

Test this out in the browser and make sure you can create a new pet and see it displayed in the UI.