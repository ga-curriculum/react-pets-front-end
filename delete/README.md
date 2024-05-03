# ![React - Pets Front-End - Delete](./assets/hero.png)

**Learning Objective**: By the end of this lesson, learners will be able to implement delete functionality in a React application, enabling them to make delete requests to an API and remove items from the user interface dynamically.

## Delete

In this section of the lesson, you will be completing the delete functionality. Let's review the requirements laid out in the intro:

#### Delete a Pet

- Add a button to delete a Pet
- Remove the Pet from the UI

And convert them into User Stories:

- As a user, I want to be able to click a button on the details view to delete a Pet.

## 🎓 You Do: Add the `delete` service function

Implementing delete functionality will follow similar patterns to the one established throughout this lesson. Take some time to try to complete the following steps by following the below instructions, cross-referencing with your existing code as needed.

### Step 1: Add a button to delete a Pet

Inside of `PetDetail.jsx` create a new button with text "Delete Pet" that will call the function to delete the pet.

In `petService.js` create a new function called `deletePet`.

This function should:

- Accept the `id` of the pet as an argument.
- Use `fetch` to make a `DELETE` request to the base URL with the `id` of the pet.
- Use a `try...catch` block to handle any errors.

Don't forget to export this function so we can use it in the next step!

  In `App.jsx` create a new function called `handleRemovePet` to handle the deletion of a pet.
  - This function should accept the `id` of the pet as an argument.
  - This function should call the `deletePet` function and pass the `id` to it, assigning to response to a `deletedPet` variable.
  - This function should use `.filter()` to remove the pet from `petList` state.
  - This function should set `selected` state to `null`, and `isFormOpen` state to `false`
  - This function should check if `deletedPet` has an `error` property, and throw a new `Error` if so, logging the error to the console.

## Step 2: Remove the Pet from the UI

- Pass `handleRemovePet` as a prop to the `PetDetail` component.
- Inside of `PetDetail.jsx` call `handleRemovePet` when the "Delete Pet" button is clicked.

Test it out in the browser and make sure you can delete a pet and see it removed from the UI.

### Check your work

#### Step 1

```jsx
// src/components/PetDetails.jsx

const PetDetails = (props) => {
  if (!props.selected)
    return (
      <div>
        <h1>NO DETAILS</h1>
      </div>
    );

  return (
    <div>
      <h1>{props.selected.name}</h1>
      <h2>Breed: {props.selected.breed}</h2>
      <h2>
        Age: {props.selected.age} year{props.selected.age > 1 ? 's' : ''} old
      </h2>
      <div>
        <button onClick={() => props.handleFormView(props.selected)}>
          Edit
        </button>
        <button onClick={() => props.handleRemovePet(props.selected._id)}>
          Delete
        </button>
      </div>
    </div>
  );
};

export default PetDetails;
```

```js
// src/services/petService.js

const deletePet = async (petId) => {
  try {
    const deletedPet = await fetch(`${BASE_URL}/${petId}`, {
      method: 'DELETE',
    });
    return deletedPet;
  } catch (err) {
    console.log(err);
  }
};
```

```jsx
// src/App.jsx

import { useState, useEffect } from 'react';
import './App.css';

import * as petService from './services/petService';

import PetDetail from './components/PetDetail';
import PetForm from './components/PetForm';
import PetList from './components/PetList';

function App() {
  const [petList, setPetList] = useState([]);
  const [selected, setSelected] = useState(null);
  const [isFormOpen, setIsFormOpen] = useState(false);

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

  const handleFormView = (pet) => {
    if (!pet.name) setSelected(null);
    setIsFormOpen(!isFormOpen);
  };

  const updateSelected = (pet) => {
    setSelected(pet);
  };

  const handleAddPet = async (formData) => {
    try {
      const newPet = await petService.create(formData);

      if (newPet.error) {
        throw new Error(newPet.error);
      }

      setPetList([newPet, ...petList]);
      setIsFormOpen(false);
    } catch (error) {
      console.log(error);
    }
  };

  const handleUpdatePet = async (formData, petId) => {
    try {
      const updatedPet = await petService.updatePet(formData, petId);

      if (updatedPet.error) {
        throw new Error(updatedPet.error);
      }

      const updatedPetList = petList.map((pet) =>
        pet._id !== updatedPet._id ? pet : updatedPet
      );
      setPetList(updatedPetList);
      setSelected(updatedPet);
      setIsFormOpen(false);
    } catch (error) {
      console.log(error);
    }
  };

  const handleRemovePet = async (petId) => {
    try {
      const deletedPet = await petService.deletePet(petId);

      if (deletedPet.error) {
        throw new Error(deletedPet.error);
      }

      setPetList(petList.filter((pet) => pet._id !== deletedPet._id));
      setSelected(null);
      setIsFormOpen(false);
    } catch (error) {
      console.log(error);
    }
  };

  return (
    <>
      <PetList
        petList={petList}
        isFormOpen={isFormOpen}
        updateSelected={updateSelected}
        handleFormView={handleFormView}
      />
      {isFormOpen ? (
        <PetForm
          selected={selected}
          handleAddPet={handleAddPet}
          handleUpdatePet={handleUpdatePet}
        />
      ) : (
        <PetDetail
          selected={selected}
          handleFormView={handleFormView}
          handleRemovePet={handleRemovePet}
        />
      )}
    </>
  );
}

export default App;
```


### Congratulations! 🎉 

You've successfully completed a full stack application featuring a React front end and an Express API backend, capable of performing complete CRUD operations on a resource. This is a significant achievement in your learning journey. Typically, an application that uses MongoDB, Express, React, and Node.js is referred to as a MERN stack application. ***Well done on building your first MERN stack application!*** This milestone sets a solid foundation for your future projects and development skills. Keep up the great work!