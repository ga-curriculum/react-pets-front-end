# ![[React Front End Pets Lab] - Delete](./assets/tktk-hero.png)

In this section we want to complete the following tasks:

- [ ] Delete a Pet
  - [ ] Add a button to delete a Pet
  - [ ] Remove the Pet from the UI

### Step 1: Add a button to delete a Pet

- Inside of `PetDetail.jsx` create a new button that will call the function to delete the pet.
  - This button should say "Delete Pet".
- In `petService.js` create a new function called `deletePet`. This function should:
  - Accept the `id` of the pet as an argument.
  - Use `fetch` to make a `DELETE` request to the base URL with the `id` of the pet.
  - Use a `try...catch` block to handle any errors.
  - Return the response.
  - Make sure to export this function.
- In `App.jsx` create a new function to handle the deletion of a pet.
  - This function should accept the `id` of the pet as an argument.
  - This function should call the `deletePet` function and pass the `id` to it.
  - This function should remove the pet from the state variable that holds the pets.
  - This function should handle any errors.

### Step 2: Remove the Pet from the UI

- Pass the function to delete a pet as a prop to the `PetDetail` component.
- Inside of `PetDetail.jsx` call the function to delete the pet when the "Delete Pet" button is clicked.

Test it out in the browser and make sure you can delete a pet and see it removed from the UI.