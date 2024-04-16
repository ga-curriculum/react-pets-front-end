# ![React Front End Pets Lab - Update](./assets/exercise-update.png)

## Update

In this section of the lab, we will be completing the update actions. Let's take the minimum requirement we laid out in the intro: 

#### Update a Pet
  - Conditionally render a form to update a Pet
  - Handle the form submission
  - Display the updated Pet in the UI

And convert them into User Stories: 

- As a user, I want to be able to update a Pet entry via a form. 

## Step 1: Conditionally Render the Form

We already have a form, as well as a function that allows us to toggle the form open and closed. We can use this same form to update a pet, with one adjustment - when a user is updating a pet, we'll want to pass the `PetForm` component the currently selected pet object so that we can pre-fill our form inputs. 

Inside of `PetDetail.jsx`, let's create a new button that will call the function to toggle the form open and closed. As mentioned above, we'll want to pass the `handleFormView` function the currently selected pet: 
  
```jsx
<button onClick={() => props.handleFormView(props.selected)}>Edit</button>
```

Make sure to pass the function to toggle the form as a prop to the `PetDetail` component.

```jsx
// src/App.jsx

<PetDetail 
  selected={selected} 
  handleFormView={handleFormView} 
/>
```

Before we move on, let's make one more adjustment to our code. Currently, our `handleFormView` function doesn't accept any arguments: 

```jsx
// src/App.jsx

const handleFormView = () => {
    setIsFormOpen(!isFormOpen)
}
```



```jsx
// src/App.jsx

 const handleFormView = (pet) => {
    if(!pet.name) setSelected(null)
    setIsFormOpen(!isFormOpen)
  }

```

Test it out in the browser and make sure you can toggle the form open and closed. Before we handle the form submission let's pass in the selected pet to the form.

- In `PetForm.jsx` we need to update the state variable that holds the form data. If we do not pass in the selected pet, we want the form to be empty. If we do pass in the selected pet, we want the form to be populated with the selected pet. Try to use a ternary to handle this.

Test it out in the browser and make sure the form is populated with the selected pet when the "Edit Pet" button is clicked.

### Step 2: Handle the Form Submission and Display the Updated Pet

- Inside of `petService.js` create a new function called `updatePet`. This function should:
  - Accept a pet object as an argument.
  - Use `fetch` to make a `PUT` request to the base URL with the `id` of the pet.
  - Use a `try...catch` block to handle any errors.
  - Use `.json()` method to parse the response. Ex: `res.json()`.
  - Return the parsed response.
  - Make sure to export this function.
- In `App.jsx` create a new function to handle the update of a pet.
  - This function should accept the form data and pet id as an argument.
  - This function should call the `updatePet` function and pass the form data to it.
  - This function should update the state variable that holds the pets with the updated pet.
  - This function should close the form.
  - This function should handle any errors.
- Pass this new function as a prop to the `PetForm` component.
- In the `PetForm` component update the `handleSubmit` function to call the update pet function passed down from `App.jsx` if there is a selected pet. If there is no selected pet, call the create pet function.

Test it out in the browser and make sure you can update a pet and see it displayed in the UI.