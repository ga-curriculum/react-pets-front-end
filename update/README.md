# ![React - Pets Front-End - Update](./assets/hero.png)

**Learning Objective**: By the end of this lesson, learners will be able to make update calls to an API and refresh the application's state to reflect those changes in a React application.

In this section of the lesson, we will be completing the update functionality. Let's review the requirements laid out in the intro:

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

{
  isFormOpen ? (
    <PetForm handleAddPet={handleAddPet} selected={selected} />
  ) : (
    <PetDetail selected={selected} handleFormView={handleFormView} />
  );
}
```

Before we move on, let's make one more adjustment to our code. Currently, our `handleFormView` function doesn't accept any arguments:

```jsx
// src/App.jsx

const handleFormView = () => {
  setIsFormOpen(!isFormOpen);
};
```

Now that a user can either toggle the form view from `PetList` (to create a new Pet) or toggle the form view from `PetDetails` (to edit an existing Pet), we need a way to distinguish between the two potential uses of this function.

There are a few ways we could do this, but a simple method is to check if the `handleFormView` function has been passed a pet object or not. If not, we can presume the "New Pet" button has been pressed, and set `selected` to null:

```jsx
// src/App.jsx

const handleFormView = (pet) => {
  if (!pet.name) setSelected(null);
  setIsFormOpen(!isFormOpen);
};
```

In `PetForm`, now that we have `selected` being passed as a prop, we can use it to determine the initial state of our form.

To make life easier, let's move the in-line initial state we currently have:

```jsx
// src/components/PetForm.jsx

const [formData, setFormData] = useState({
  name: '',
  age: '',
  breed: '',
});
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

One final step - now that we're handling the initial state of our form whenever the user toggles the form on, we can remove the line that resets form data on submit:

```jsx
// src/components/PetForm.jsx

const handleSubmitForm = (evt) => {
  evt.preventDefault();
  props.handleAddPet(formData);
  // remove: setFormData({ name: '', age: '', breed: '' })
};
```

Test it out in the browser and make sure the form is populated with the selected pet when the "Edit Pet" button is clicked.

## Step 2: Handle the Form Submission and Display the Updated Pet

### 🎓 You Do: Add the `updatePet` service function

Next, we'll need to create the service that updates a Pet! Hopefully by now this is a pattern that is getting more familiar - take some time to try to solve this on your own using the following instructions:

- Inside of `petService.js` create a new function called `updatePet`. This function should:

  - Accept a pet object (form data) and a pet id as an argument.
  - Use `fetch` to make a `PUT` request to the base URL with the `id` of the pet (check your server code if you need clarity on the exact URL).
  - Use a `try...catch` block to handle any errors.
  - Use the `.json()` method to parse the response. Ex: `res.json()`.
  - Return the parsed response.
  - Export this function.

- Next, in `App.jsx` we'll want to create a handler function called `handleUpdatePet` which invokes the service function and updates `petList` state.
  - This function should accept form data and a pet id as an argument.
  - This function should call the `updatePet` service and pass the form data and pet id to it.
  - This function should update `petList` state with the updated pet.
  - This function should close the form and set `selected` state to the `updatedPet` object.
  - If `updatedPet` has an error, throw a new `Error` and log the error.

Once this is done, pass this new function as a prop to the `PetForm` component.

### Update `handleSubmit`

In the `PetForm` component, update the `handleSubmit` function to call the `handleUpdatePet` function if there is a selected pet. If there is no selected pet, call the create pet function.

```jsx
// src/components/PetForm.jsx

const handleSubmit = (evt) => {
  evt.preventDefault();
  if (props.selected) {
    // Don't forget to pass both formData and the ._id!
    props.handleUpdatePet(formData, props.selected._id);
  } else {
    props.handleAddPet(formData);
  }
};
```

Test it out in the browser and make sure you can update a pet and see it displayed in the UI.

### Check your work

If you ran into any issues, you can check your work below:

```js
// src/services/PetService.js

const updatePet = async (formData, petId) => {
  try {
    const res = await fetch(`${BASE_URL}/${petId}`, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(formData),
    });
    return res.json();
  } catch (err) {
    console.log(err);
  }
};
```

There are a few different ways to handle updating state with an edited Pet. Below, we use `.map()` to test each element's `._id`, replacing the existing entry with the updated one only if the id's match.

```jsx
// src/App.jsx

const handleUpdatePet = async (formData, petId) => {
  try {
    const updatedPet = await petService.updatePet(formData, petId);

    // handle potential errors
    if (updatedPet.error) {
      throw new Error(updatedPet.error);
    }

    const updatedPetList = petList.map((pet) =>
      // If the id of the current pet is not the same as the updated pet's id, return the existing pet. If the id's match, instead return the updated pet.
      pet._id !== updatedPet._id ? pet : updatedPet
    );
    // Set petList state to this updated array
    setPetList(updatedPetList);
    // If we don't set selected to the updated pet object, the details page will reference outdated data until the page reloads.
    setSelected(updatedPet);
    setIsFormOpen(false);
  } catch (error) {
    console.log(error);
  }
};
```
