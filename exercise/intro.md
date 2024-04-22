# ![React Front End Pets Lab - Intro](./assets/exercise-intro.png)

**Learning Objective**: By the end of this lesson, the learner will understand the minimum requirements and component hierarchy of the app.

## Introduction

In this lab, you'll be building a Single Page Application (SPA) using React. You will also be using `fetch` to consume from a previously built RESTful API. You will be able to Create, Read, Update, and Delete (CRUD) pets from the API, and conditionally render the UI based on the state of the application.

## Minimum requirements

#### Read all Pets
  - Display all Pets in the UI

#### Read a single Pet
  - Add a link to view the details of a single Pet
  - Conditionally render the details of a single Pet

#### Create a new Pet
  - Conditionally render a form to create a new Pet
  - Handle the form submission
  - Display the new Pet in the UI

#### Update a Pet
  - Conditionally render a form to update a Pet
  - Handle the form submission
  - Display the updated Pet in the UI

#### Delete a Pet
  - Add a button to delete a Pet
  - Remove the Pet from the UI

## Component Hierarchy

This lab does not feature any routing, meaning that the user will not be navigating between pages in this build. When working with React, however, this doesn't mean that you are limited to the use of a single component - far from it! Your single 'page' will be made up of multiple components, so it's still a good idea to map out the component hierarchy and get an overview of the app layout before you start coding:

[tktk Hunter amazing graphic here]
  
## Items of Note

This is a big lab and it is important to break it down into smaller pieces. The Read section of the instructions will be fairly thorough, to help set you up for success. The other three CRUD steps (Create, Update, Delete) will be more hands off, except whenever introducing new patterns or concepts. 

Some other items to keep in mind:

- You will be using `fetch` to consume from a RESTful API.
- You will be using `useState` and `useEffect` to manage state and side effects.
- You'll want to conditionally render the UI based on the state of the application.
- Do not use outside libraries for state management or routing. **No Redux or React Router.**

Most of all, have fun and build something you are proud of!