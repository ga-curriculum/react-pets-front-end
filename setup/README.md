# ![[tktk Module Name] - Setup](./assets/hero.png)

## Setup

Open your Terminal application and navigate to your **`~/code/ga/lectures`** directory:

```bash
cd ~/code/ga/lectures
```

Create a new Vite project for your React app:

```bash
npm create vite@latest
```

Follow the prompts to create a new React project:

You'll be prompted to provide a project name. `vite-project` is the default, but when you start typing that default will go away. Choose a name that makes sense for this project; in this case, we'll call the app `react-front-end-pets`. Follow the steps to create the project:

- By default the project will be named `vite-project` let's change it to something that will make sense for this project;`react-front-end-pets`.
- Select a framework. Use the arrow keys to choose the `React` option and hit `Enter`.
- Select a variant. Again, use the arrow keys to choose `JavaScript` and hit `Enter`.

Navigate to your new project directory and install the necessary dependencies:

```bash
cd react-front-end-pets
npm i
```

Open the project's folder in your code editor:

```bash
code .
```

### Clear `App.jsx`

Open the `App.jsx` file in the `src` directory and replace the contents of it with the following:

```jsx
// src/App.jsx

const App = () => {

  return (
    <h1>Hello world!</h1>
  );
}

export default App
```

Start the dev server by running `npm run dev` in your terminal and the go to `localhost:5173` in your browser.

Now let's get the file and folder structure set up for our project.

- Create a new folder called `components` in the `src` directory.
- Create a new folder called `services` in the `src` directory.
- Inside of `services` create a new file called `petService.js`.
- Inside of `petService.js` create add the base URL for our API.

```js
const BASE_URL = `${import.meta.env.VITE_BACK_END_SERVER_URL}/pets`
```

- Create a `.env` file in the root of the project and add the following:

```bash
VITE_BACK_END_SERVER_URL='http://localhost:3000'
```

We are going to keep any components we create inside the `components` directory. This will help us keep our project organized and easy to navigate. We are also going to create a file called `petService.js` that will hold the base URL for our API. This will help us keep our code DRY and easy to maintain.