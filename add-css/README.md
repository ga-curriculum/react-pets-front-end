# ![React - Pets Front-End - Add CSS](./assets/hero.png)

**Learning Objective**: By the end of this lesson, the learner will be able to style their React components using the provided CSS styles.

## Adding CSS

Great job completing your first MERN stack application! Let's take it a step further by polishing up that front-end dashboard with some CSS.

We've created the following styles to give your application a dashboard layout. Add them to `App.css`:

```css
#root {
  height: 100vh;
  width: 100%;
  display: grid;
  grid-template-columns: fit-content(375px) 1fr;
}

.sidebar-container {
  padding: 2rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  background-color: rgb(42, 42, 42);
  min-width: 0;
}

.list-container {
  display: flex;
  justify-content: flex-start;
  overflow-y: overlay;
  max-height: 700px;
  scrollbar-color: #ffffff40 #00000000;
}

ul {
  list-style-type: none;
  max-width: 300px;
  display: inline-block;
}

li {
  font-size: 2rem;
  white-space: wrap;
  text-overflow: ellipsis;
  overflow: hidden;
  margin-bottom: 1.2rem;
  cursor: pointer;
}

.details-container {
  padding: 2rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.form-container {
  display: flex;
  justify-content: center;
  align-items: center;
}

form {
  display: flex;
  flex-direction: column;
  align-items: center;
}

label {
  font-size: 1.2rem;
}

input {
  height: 2rem;
  width: 12rem;
  margin-bottom: 1rem;
}

button {
  width: 200px;
  margin: 1rem;
}

.button-container {
  padding: 2rem;
  outline: 0;
}
```

## Add classes to components

Now we'll add some classes to our components to apply these styles. Before you begin, make sure to import `App.css` at the top of `App.jsx`.

```jsx
import './App.css';
```

### 1. PetList Component

**File**: `PetList.jsx`

- **Class `sidebar-container`**: Add this class to the outermost `div` of the `PetList` component to style the sidebar where the pet list will appear.
- **Class `list-container`**: Add this class to the `div` wrapping the `ul` element that contains the list of pets. This will style the container of the list, particularly its scrolling behavior.

```jsx
// PetList.jsx

<div className="sidebar-container">
  <h1>Pet List</h1>
  <div className="list-container">
    {!props.petList.length ? (
      <h2>No Pets Yet!</h2>
    ) : (
      <ul role="list">{pets}</ul>
    )}
  </div>
  <button onClick={props.handleFormView}>
    {props.isFormOpen ? 'Close Form' : 'New Pet'}
  </button>
</div>
```

### 2. PetDetail Component

**File**: `PetDetail.jsx`

- **Class `details-container`**: Add this class to the outermost `div`'s of the `PetDetail` component. This will apply styles to the details display sections, enhancing alignment and padding.
- **Class `button-container`**: Add this class to the `div` that wraps the edit and delete buttons to apply specific styling like button padding.

```jsx
// PetDetail.jsx

if (!props.selected)
  return (
    <div className="details-container">
      <h1>NO DETAILS</h1>
    </div>
  );

return (
  <div className="details-container">
    <h1>{props.selected.name}</h1>
    <h2>Breed: {props.selected.breed}</h2>
    <h2>
      Age: {props.selected.age} year{props.selected.age > 1 ? 's' : ''} old
    </h2>

    <div className="button-container">
      <button onClick={() => props.handleFormView(props.selected)}>Edit</button>
      <button onClick={() => props.handleRemovePet(props.selected._id)}>
        Delete
      </button>
    </div>
  </div>
);
```

### 3. PetForm Component

**File**: `PetForm.jsx`

- **Class `form-container`**: Add this class to the outermost `div` that wraps the form element. This ensures that the form is centered and the flexbox properties are applied correctly.
- Additionally, ensure all inputs and labels are properly aligned and spaced by the inherited CSS from the `form` tag.

```jsx
// PetForm.jsx

<div className="form-container">
  <form onSubmit={handleSubmit}>
    <label htmlFor="name"> Name </label>
    <input
      id="name"
      name="name"
      value={formData.name}
      onChange={handleChange}
      required
    />
    <label htmlFor="age"> Age </label>
    <input id="age" name="age" value={formData.age} onChange={handleChange} />
    <label htmlFor="breed"> Breed </label>
    <input
      id="breed"
      name="breed"
      value={formData.breed}
      onChange={handleChange}
    />
    <button type="submit">
      {props.selected ? 'Update Pet' : 'Add New Pet'}
    </button>
  </form>
</div>
```

And you're done - thats a great looking dashboard! 