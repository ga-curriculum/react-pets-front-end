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
// src/components/PetDetail.jsx

<button onClick={() => props.handleFormView(props.selected)}>Edit</button>
```

Make sure to pass `handleFormView` as a prop to the `PetDetail` component. While we're in `App.jsx`, let's also pass `selected` as a prop to `PetForm`, as we'll need this object to pre-fill our form if the user is editing a Pet: 

```jsx
// src/App.jsx

  {isFormOpen 
        ? <PetForm 
            handleAddPet={handleAddPet} 
            selected={selected} 
          /> 
        : <PetDetail 
            selected={selected} 
            handleFormView={handleFormView} 
          />
  }

```

Before we move on, let's make one more adjustment to our code. Currently, our `handleFormView` function doesn't accept any arguments: 

```jsx
// src/App.jsx

const handleFormView = () => {
    setIsFormOpen(!isFormOpen)
}
```

Now that a user can either toggle the form view from `PetList` (to create a new Pet) or toggle the form view from `PetDetails` (to edit an existing Pet), we need a way to distinguish between the two potential uses of this function.

There are a few ways we could do this, but a simple method is to check if the `handleFormView` function has been passed a pet object or not. If not, we can presume the "New Pet" button has been pressed, and set `selected` to null: 

```jsx
// src/App.jsx

 const handleFormView = (pet) => {
    if(!pet.name) setSelected(null)
    setIsFormOpen(!isFormOpen)
  }
```

In `PetForm`, now that we have `selected` being passed as a prop, we can use it to determine the initial state of our form. 

To make life easier, let's move the in-line initial state we currently have: 
```jsx
// src/components/PetForm.jsx

const [formData, setFormData] = useState({
    name: '',
    age: '',
    breed: ''
  })
```

Into a variable called `initialState`: 

```jsx
// src/components/PetForm.jsx

const PetForm = (props) => {
  const initialState = {
      name: '',
      age: '',
      breed: ''
    }
  const [formData, setFormData] = useState(initialState)
```

Next, we can check if `selected` has a value. If the user called the `handleFormView` from `PetDetail`, `selected` should be whatever Pet the user currently had selected. Otherwise, the user will have clicked on the "New Pet" button in `PetList`, which sets `selected` to null. So, we can set the initial `formData` state based on whether `props.selected` has a truthy (non-null) value: 

```jsx
// src/components/PetForm.jsx

const PetForm = (props) => {
const initialState = {
    name: '',
    age: '',
    breed: ''
  }
  // If pet data has been passed as props, we set formData as that pet object.
  // Otherwise, we can assume this is a new pet form, and use the empty initialState object.
  const [formData, setFormData] = useState(props.selected ? props.selected : initialState)
```

We can use the same ternary logic to provide some minor UI feedback to the user, by conditionally rendering the submit button text: 

```jsx
// src/components/PetForm.jsx

<button type="submit"> {props.selected ? 'Update Pet' : 'Add New Pet'} </button>
```

One final step - now that we're using a variable for `initialState`, we can clean up our `handleSubmitForm` function a bit as well: 

```jsx
// src/components/PetForm.jsx

 const handleSubmitForm = (evt) => {
    evt.preventDefault()
    props.handleAddPet(formData)
    setFormData(initialState)
  }
```

Test it out in the browser and make sure the form is populated with the selected pet when the "Edit Pet" button is clicked.

## Step 2: Handle the Form Submission and Display the Updated Pet



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