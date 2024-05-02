# ![React - Pets Front-End - Create](./assets/hero.png)

**Learning Objective**: By the end of this lesson, the learner be able to create and manage forms within a React application to add new pet entries to a database.

## Create

In this section of the lesson, we want to complete the create functionality for adding a new pet to our database. Once again, let's review the minimum requirements laid out in the intro:

#### Create a new Pet

- Conditionally render a form to create a new Pet
- Handle the form submission
- Display the new Pet in the UI

And convert them into User Stories:

- As a user, I want to be able to enter information into a form and use it to create a new Pet entry.

## Step 1: Conditionally Render the Form

In this step, we'll set up a form where users can input new data for a pet. Typically, forms for creating new entries, like adding a new pet, are placed on separate pages. However, since we're not using multiple pages or routing in this lesson, we'll manage the visibility of the form using state. This means we'll use state to control whether the form is displayed or hidden based on user interactions.

### Creating the Form UI

Inside the `components` directory, let's create a new component called `PetForm.jsx`.

In `PetForm.jsx`, create a function component that accepts `props` as an argument. Inside of the return statement of `PetForm.jsx`, we'll create a **controlled** `form` element that will be used to create a new pet.

Make sure to export the `PetForm` component at the end of the file.

```jsx
// src/components/PetForm.jsx

import { useState } from 'react';

const PetForm = (props) => {
  // formData state to control the form
  const [formData, setFormData] = useState({
    name: '',
    age: '',
    breed: '',
  });

  // handleChange function to update formData state
  const handleChange = (evt) => {
    setFormData({ ...formData, [evt.target.name]: evt.target.value });
  };

  return (
    <div>
      <form>
        <label htmlFor="name"> Name </label>
        <input
          id="name"
          name="name"
          value={formData.name}
          onChange={handleChange}
          required
        />
        <label htmlFor="age"> Age </label>
        <input
          id="age"
          name="age"
          value={formData.age}
          onChange={handleChange}
        />
        <label htmlFor="breed"> Breed </label>
        <input
          id="breed"
          name="breed"
          value={formData.breed}
          onChange={handleChange}
        />
        <button type="submit">Add New Pet</button>
      </form>
    </div>
  );
};

export default PetForm;
```

### Conditionally Render the Form

Next, create a new state variable inside of `App.jsx` to represent the form component being "open" or "closed". When the page loads initially the form should not be open, so we can set the initial value to `false`:

```jsx
// src/App.jsx

const [isFormOpen, setIsFormOpen] = useState(false);
```

Next, still in `App.jsx`, create a function called `handleFormView` to toggle the above state variable. When the function is called, the Boolean value of `isFormOpen` should change from `false` to `true`, or vice versa.

```jsx
// src/App.jsx

const handleFormView = () => {
  setIsFormOpen(!isFormOpen);
};
```

Once the function is created, we'll pass it down to `PetList` as a prop.

```jsx
// src/App.jsx

<PetList
  petList={petList}
  updateSelected={updateSelected}
  handleFormView={handleFormView}
/>
```

Next, in `PetList.jsx`, let's create a button that will call `handleFormView` when clicked. In order to determine what text the button should display to the user, we'll need to pass `isFormOpen` down to `PetList` as well. 

- If the form is open (`true`), the button should say "Close Form". 
- If the form is closed (`false`), the button should say "Add Pet".

<br>

```jsx
// src/App.jsx

<PetList
  petList={petList}
  updateSelected={updateSelected}
  handleFormView={handleFormView}
  isFormOpen={isFormOpen}
/>
```

```jsx
// src/components/PetList.jsx

<button onClick={props.handleFormView}>
  {props.isFormOpen ? 'Close Form' : 'New Pet'}
</button>
```

Finally, we'll need to conditionally render either the `PetForm` component or the `PetDetail` component based on our `isFormOpen` state:

```jsx
// src/App.jsx

return (
  <>
    <PetList
      petList={petList}
      updateSelected={updateSelected}
      handleFormView={handleFormView}
      isFormOpen={isFormOpen}
    />
    {isFormOpen ? (
      <PetForm />
    ) : (
      <PetDetail selected={selected} />
    )}
  </>
);
```

Test it out in the browser and make sure you can toggle the form open and closed.

## Step 2: Handle the Form Submission and Display the New Pet

### Create the service

Inside of `petService.js` we'll need to add a new function called `create`. 

This function should:

- Accept a pet object as an argument.
- Use `fetch` to make a `POST` request to the base URL.
- Use a `try...catch` block to handle any errors.
- Use `.json()` method to parse the response. Ex: `res.json()`.
- Return the parsed response.

This will largely resemble the service call we made in the Index step, with the exception that we're making a 'POST' request now instead of a 'GET' request. This means we'll need to specify a few things in the options object:

```jsx
const create = async (formData) => {
  try {
    const res = await fetch(BASE_URL, {
      // We specify that this is a 'POST' request
      method: 'POST',
      // We're sending JSON data, so we attach a Content-Type header
      // and specify that the data being sent is type 'application/json'
      headers: {
        'Content-Type': 'application/json',
      },
      // The formData, converted to JSON, is sent as the body
      // This will be received on the backend as req.body
      body: JSON.stringify(formData),
    });
    return res.json();
  } catch (err) {
    console.log(err);
  }
};
```

Make sure to export the `create` function at the bottom of the file.

### Call the service

Now that our `create` function is complete, it's time to write the function that will use it. Because creating a new Pet will impact our `petList` state, we'll want to place the new function where that state lives - in `App.jsx`.

In `App.jsx`, create a new async function named `handleAddPet` which accepts an object (our form data) as an argument. Add the structure for a `try/catch` statement as well:

```jsx
// src/App.jsx

const handleAddPet = async (formData) => {
  try {

  } catch (error) {

  }
};
```

This function will make an `async` call to `petService.create`, passing it `formData`. If everything goes well, `create` returns the created pet object from the database. We'll invoke `setPetList` and pass it a new array comprised of the new pet object, followed by the existing `petList` which we'll add using the spread operator.

```jsx
// src/App.jsx

const handleAddPet = async (formData) => {
  try {
    // Call petService.create, assign return value to newPet
    const newPet = await petService.create(formData);
    // Add the pet object and the current petList to a new array, and
    // set that array as the new petList
    setPetList([newPet, ...petList]);
  } catch (error) {}
};
```

Once the user has submitted the form, we want the form to close - this, along with the new Pet showing up in the `PetList` component, will serve as feedback to the user that the form was submitted. Fortunately, all we need to do to accomplish that is call `setIsFormOpen` and pass it `false` at the end of our function.

```jsx
// src/App.jsx

const handleAddPet = async (formData) => {
  try {
    const newPet = await petService.create(formData);
    setPetList([newPet, ...petList]);
    setIsFormOpen(false);
  } catch (error) {}
};
```

### Error Handling

As usual, we need to also account for any potential errors. Let's check if the response - `newPet` - has an error property. If so, we'll throw a new `Error` and pass it to the `catch` block.

```jsx
// src/App.jsx

const handleAddPet = async (formData) => {
  try {
    const newPet = await petService.create(formData);

    if (newPet.error) {
      throw new Error(newPet.error);
    }

    setPetList([newPet, ...petList]);
    setIsFormOpen(false);
  } catch (error) {
    // Log the error to the console
    console.log(error);
  }
};
```

Finally, pass `handleAddPet` as a prop to the `PetForm` component.

```jsx
// src/App.jsx

return (
  <>
    <PetList
      petList={petList}
      updateSelected={updateSelected}
      handleFormView={handleFormView}
      isFormOpen={isFormOpen}
    />
    {isFormOpen ? (
      <PetForm handleAddPet={handleAddPet} />
    ) : (
      <PetDetail selected={selected} />
    )}
  </>
);
```

### Handle Form Submission

For the final step of the create process, we need to handle the user submitting the form.

Inside of `PetForm.jsx` create a new function called `handleSubmitForm` to handle the form submission.

```jsx
// src/components/PetForm.jsx

const handleSubmitForm = (evt) => {
  evt.preventDefault();
  props.handleAddPet(formData);
};
```

- `handleSubmitForm` will accept the submit event as an argument. To prevent the page from reloading, we'll call `evt.preventDefault()`.

- Then, we'll call `props.handleAddPet` and pass it `formData`.

Big picture, here's what happens when the user hits submit:

- That `formData` object gets passed to our `create` service where it is converted to JSON, sent to our route on the backend, used (as `req.body`) to create a new Pet, and returned as a new JSON Pet object by the controller.
- We convert it back to useable JavaScript, where it is then added to a new array along with our existing state. 
- Our `petList` state is then set to that new array, updating it. Once our state updates, React refreshes the UI, and the user sees their brand new Pet show up in the `PetList` component.

If that feels confusing, take a second to trace the data through your code! It's important to be able to visualize this flow of data when working with CRUD apps, and now that we're passing data both directions this is an excellent time to practice this skill.

Ok, almost done! As one final UI consideration, this function should reset `formData` back to its initial (empty) values.

```jsx
// src/components/PetForm.jsx

import { useState } from 'react';

const PetForm = (props) => {
  const [formData, setFormData] = useState({
    name: '',
    age: '',
    breed: '',
  });

  const handleChange = (evt) => {
    setFormData({ ...formData, [evt.target.name]: evt.target.value });
  };

  const handleSubmitForm = (evt) => {
    evt.preventDefault();
    props.handleAddPet(formData);
    setFormData({ name: '', age: '', breed: '' });
  };

  return (
    <div>
      <form>
        <label htmlFor="name"> Name </label>
        <input
          id="name"
          name="name"
          value={formData.name}
          onChange={handleChange}
          required
        />
        <label htmlFor="age"> Age </label>
        <input
          id="age"
          name="age"
          value={formData.age}
          onChange={handleChange}
        />
        <label htmlFor="breed"> Breed </label>
        <input
          id="breed"
          name="breed"
          value={formData.breed}
          onChange={handleChange}
        />
        <button type="submit">Add New Pet</button>
      </form>
    </div>
  );
};

export default PetForm;
```

Test your form in the browser and make sure you can create a new pet and see it displayed in the UI.
