# React Server Components

React Server Components is a new feature introduced in React in version 18 and quickly embraced by Next.js.

This architecture introduces a new way of creating React components, splitting them into two types:
- Client components
- Server components

Server components can run tasks like reading files or fetching data from a database.

Server components, however, cannot use hooks or handle user interactions.

**In Next.js, all components are server components by default.***

To convert a component to a client component, you need to add `"use client"` at the top of the component file.

Unlike server components, client components cannot perform tasks like reading files or interacting with a database, 
but they can use hooks and handle user interactions.