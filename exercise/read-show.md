# ![React Front End Pets Lab - Read - Show](./assets/exercise-read.png)

## Read - Show

As with the index section, let's look at the minimum requirements for showing a single pet: 

#### Read a single Pet
  - Add a link to view the details of a single Pet
  - Conditionally render the details of a single Pet

And the accompanying User Story: 

- As a user, I want to be able to click a button to view the details of a single Pet. 

### Adding the UI for showing a single pet

For this next section we are going to implement the ability to show a single pet in the UI. We want to be able to click on a pet and see the details of that pet.

Inside of `PetList.jsx` where you `.map()` over the pets, wrap each `li` element in an `a` tag.

Because the `a` tag is now the top-level element, you'll need to move the `key` attribute from the `li` to the `a` element.

Next, add an `onClick` event to the `a` element. For testing purposes, add an inline function that logs the clicked pet to the console.

```jsx
// src/components/PetList.jsx

const pets = props.petList.map((pet) => (
    <a 
      key={pet._id}
      onClick={() => console.log(pet)}
    >
      <li>{pet.name}</li>
    </a>
  ))
```

You should be able to click on each pet name and see the corresponding pet object log to the console. If not, debug before moving on to the next step! 

### Handling the click event

When designing a webpage with routes, we would normally create a show page which could be linked to at `/pets/:petId`. This would in effect keep track of which specific pet the user had selected. 

This app will not be using routing, so we're going to need a way to keep track of which pet the user has selected. Sounds like a job for state!

Create a new state variable that will hold a single pet. This should be an object that represents the selected pet, or `null` if no pet is selected:

```jsx
// src/App.jsx
const [selected, setSelected] = useState(null)
```

Next, we'll need a create a new function to handle the click event we set up in the `PetList` component. We'll build it inside of `App.jsx`, as it will need to access to the `setSelected` method we just set up. 

This function should accept a pet object as an argument, and set the selected pet to the state variable.

```jsx
// src/App.jsx
function App() {
  const [petList, setPetList] = useState([])
  const [selected, setSelected] = useState(null)

  useEffect(() => {
    const fetchPets = async () => {
      const data = await petService.index()
      setPetList(data.pets)
    }
    fetchPets()
  }, [])
  // Add the following: 
  const updateSelected = (pet) => {
    setSelected(pet)
  }

```

Next, pass this function as a prop to the `PetList` component and call it when a pet is clicked. This should replace the log statement we added in the `PetList` component.

```jsx
// src/App.jsx

  return (
    <PetList 
      petList={petList} 
      updateSelected={updateSelected} 
    />
  )
```

```jsx
// src/components/PetList.jsx

 const pets = props.petList.map((pet) => (
    <a 
      key={pet._id}
      onClick={() => props.updateSelected(pet)}
    >
      <li>{pet.name}</li>
    </a>
  ))
```


Before moving on, test that you can click on a pet and have the state variable update with the selected pet. You can do this with the Chrome React Dev Tools.

Now that we have the selected pet in the state variable we can conditionally render the details of the pet in the UI.

- Inside of `components` create a new file called `PetDetails.jsx`.
  - In this file create a function component called `PetDetails`.
  - This component should accept `props` as an argument.
  - From the `PetDetails` component we should render the details of the pet.
  - Make sure to export the `PetDetails` component at the end of the file.
- Back in `App.jsx` import the `PetDetails` component.
  - Conditionally render the `PetDetails` component based on the state of the selected pet.

Test it out in the browser and make sure you can click on a pet and see the details of that pet.