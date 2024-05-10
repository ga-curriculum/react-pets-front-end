# ![React - Pets Front-End - Setup](./assets/hero.png)

## Setup

Open your Terminal application and navigate to your **`~/code/ga/lectures`** directory:

```bash
cd ~/code/ga/lectures
```

Create a new Vite project for your React app:

```bash
npm create vite@latest
```

You'll be prompted to choose a project name. Let's name it `react-pets-front-end`.

- Select a framework. Use the arrow keys to choose the `React` option and hit `Enter`.

- Select a variant. Again, use the arrow keys to choose `JavaScript` and hit `Enter`.

Navigate to your new project directory and install the necessary dependencies:

```bash
cd react-pets-front-end
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
  return <h1>Hello world!</h1>;
};

export default App;
```

### Running the development server

To start the development server and view our app in the browser, we'll use the following command: 

```bash
npm run dev
```

You should see that `Vite` is available on port number 5173: 

```plaintext
localhost:5173
```

### Create file structure

Now let's setup the file and folder structure for our project:

- Create a new folder called `components` in the `src` directory.
- Create a new folder called `services` in the `src` directory.
- Inside of `services` create a new file called `petService.js`.
- Inside of `petService.js` create add the base URL for our API.

We are going to keep all components we create inside the `components` directory. This will help us keep our project organized and easy to navigate.`petService.js` will hold the base URL for our API, and any functions that make calls to the server for data. This structure will help us keep our code DRY and easy to maintain.

### Using environment variables with Vite

With a SPA like React, we want the backend and frontend to be as loosely coupled as possible. This means, instead of hard-coding our `localhost:3000` address into the front-end codebase for our server calls, we want to use an environment variable that can change depending on context.

That way, if we deploy an app, all we need to do is tell the React app where the backend is located, using an environment variable.

So, we need to create a `.env` file on the front-end that will contain our server url:

```bash
touch .env
```

Add the following:

```plaintext
VITE_BACK_END_SERVER_URL='http://localhost:3000'
```

If this variable name seems awkward, it's because [Vite's environment variables](https://vitejs.dev/guide/env-and-mode) are required to start with `VITE_` in order to be readable from the client application.

Be sure to also add `.env` to the list in your `.gitignore` file, so that our local version doesn't get pushed to GitHub.

Now, we can access this variable for use in all fetch calls to the server:

```js
const BASE_URL = `${import.meta.env.VITE_BACK_END_SERVER_URL}/pets`;
```

### GitHub setup

To add this project to GitHub, initialize a Git repository:

```bash
git init
git add .
git commit -m "init commit"
```

Make a new repository on [GitHub](https://github.com/) named react-pets-front-end. 

Link your local project to your remote GitHub repo:

```bash
git remote add origin https://github.com/<github-username>/react-pets-front-end.git
git push origin main
```

> 🚨 Do not copy the above command. It will not work. Your GitHub username will replace `<github-username>` (including the `<` and `>`) in the URL above.

## Running the Express backend

Before diving into our React app development, you'll need to ensure that the Express backend server is operational. This backend will handle requests from your React app. You will be using the back-end server you created in the `Express API - Pets Back-End` lesson as the API for this lesson.

Follow these steps to set up the server:

Open your Terminal application and navigate to your **`~/code/ga/lectures/express-api-pets-back-end`** directory:

```bash
cd ~/code/ga/lectures/express-api-pets-back-end
```

Once there, run your server with `nodemon`:

```bash
nodemon server.js
```

> Note: If your `express-api-pets-back-end` is incomplete, you can obtain a fully implemented version from the [solution code repo](https://git.generalassemb.ly/modular-curriculum-all-courses/express-api-pets-back-end-solution). Remember to install all necessary dependencies with `npm i` and establish a connection to your MongoDB Atlas by adding a connection string in a `.env` file.

### Create your server .env

To set up your server using the provided starter code, you'll need to create a `.env` file that includes a `MONGODB_URI` connection string. This connection string links your application to a MongoDB Atlas database. First, ensure you have a MongoDB Atlas account and create a database cluster.

Add a `.env` file to your server application and add the following secret key:

```text
MONGODB_URI=your_mongodb_atlas_connection_string_here
```

Start the server and you are ready to start on the React front-end!

```bash
nodemon start
```
