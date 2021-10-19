---
title: How To Manage Global State Without Redux
description: Checkout a cool technique for handling data that is shared between pages.
date: 2021-10-18
tags:
  - Hard Skills
  - React
layout: layouts/post.njk
---

## Description

In this post, I'd like to share a technique for managing state within React, specifically,
**state shared between different pages.**  This is, especially, a nice technique for those of you who are using `Next.js`.

It's important to emphasize that last point above: **state shared between different pages**.

Within `React`, you of course have many different options when it comes to state.  There are different patterns and APIs for different problems.  As an example, it's perfectly okay to use the standard `useState` API for certain use cases.  One that comes to mind is where you have a "page" component (e.g. `Home`), and that component has a state variable for a banner (i.e. a sub-component that can hide or show based on conditions).  For this imaginary use case, it's okay for that piece of state to live within `Home`, since no other page cares about it.  This is important to keep in mind.

With this post, and many other to come, I don't seek to take an opinionated stance.  I believe there's many different approaches for many different problems.  Instead, I just seek to add another tool to our metaphorical tool belt.

So, again, this post simply shows **a different way of sharing state between different pages/routes within a React app, without Redux -- again, especially for developers using Next.js**.

**Note:** At the end of this post, I'll share a link to an example project using this approach.

**Note:** I learned this pattern from an amazing YouTube and YouTuber.  [Here](https://www.youtube.com/watch?v=MR43-gYVQbI&t=504s) is the link to that video (thank him for me).

## Prerequisites

Familiarity with the points below is needed before moving forward:

1. React
2. React Context

Nice to haves:

1. Redux Concepts (i.e. reducers, actions, dispatchers)

## Approach

### High Level Overview

This approach (as hinted to above), will be using React Context.  This context will basically wrap the entire application, and thus, allow the "page" components to read from it.

Along with reading state from the context, the page components will then have the ability to dispatch actions to an application reducer, which of course, is responsible for taking an action and then computing a new state.

The flow chart below sums up the approach.

<img class="post-2-flow-chart-img" src="https://bryans-blog.s3.amazonaws.com/State+Flow+-+Export.png"/>

Above, the first shape (i.e. `App Component`) shows how the pages are contained within said element.

Next, starting from the `Page (Example: Home)` square, the flow chart walks us through what happens when the state needs to change.

To see a small project that implements this, please click [here](https://github.com/bryg217/countries).

### Technical Design

There are going to be three files we care about for now:

1. State File
2. `App.js`
3. Page Component

#### State File

At a high level, this file contains all of the code associated to the API for managing state (i.e. Reducer, hooks for state and the provider).  Of course, it doesn't have to be this way for a production level app.  This is just all done within the same file for demonstrations purposes.

Below, I've included what this code might look like (e.g. `state.js`):

```js
import { useReducer, useContext, createContext } from 'react'

const AppStateContext = createContext()
const AppDispatchContext = createContext()

const reducer = (state, action) => {
  switch (action.type) {
    case 'SET_LOADED':
      const { loaded } = action.payload
      return {
        ...state,
        loaded
      }
    default:
      throw new Error(`Unknown action: ${action.type}`)
  }
}

const initialState = {
  feed: [],
  loaded: false
}

export const AppProvider = ({ children }) => {
  const [state, dispatch] = useReducer(reducer, initialState)

  return (
    <AppDispatchContext.Provider value={dispatch}>
      <AppStateContext.Provider value={state}>
        {children}
      </AppStateContext.Provider>
    </AppDispatchContext.Provider>
  )
}

export const useApp = () => useContext(AppStateContext)
export const useAppDispatch = () => useContext(AppDispatchContext)
```

Let's walk through the file.

The first part is we `import` all of our dependencies; that's pretty straight forward.

The next couple of lines are where we declare the two different contexts.

```js
const AppStateContext = createContext()
const AppDispatchContext = createContext()
```

As you can tell, there are two different contexts: One that contains the state and one that contains the dispatch function to be used for dispatching actions (separation of concerns).

The next thing that is done is the declaration of the `reducer`, a simple one albeit.  As you can see, it currently handles one type of action -- updating the loaded property of the state.  Again, simplicity used for demo purposes.

```js
const reducer = (state, action) => {
  switch (action.type) {
    case 'SET_LOADED':
      const { loaded } = action.payload
      return {
        ...state,
        loaded
      }
    default:
      throw new Error(`Unknown action: ${action.type}`)
  }
}
```

Next, we start to get into the exports.  The first export is the `AppProvider`:

```js
// initial state above
export const AppProvider = ({ children }) => {
  const [state, dispatch] = useReducer(reducer, initialState)

  return (
    <AppDispatchContext.Provider value={dispatch}>
      <AppStateContext.Provider value={state}>
        {children}
      </AppStateContext.Provider>
    </AppDispatchContext.Provider>
  )
}
```

As you'll see, this is to be used in `App.js` to wrap the pages.  As a quick overview, this is used as a wrapper for declaring the reducer, initial state, and finally, passing said data to their respective providers.

Lastly, we export some hooks:

```js
export const useApp = () => useContext(AppStateContext)
export const useAppDispatch = () => useContext(AppDispatchContext)
```

This is a pattern that helps avoid the following within a page component:

```js
// without hooks - import
import React, { useContext } from 'react'
import AppStateContext from './state'

// without hooks - within component
const state = useContext(AppStateContext)

// with the hooks
import { useApp } from './state'

// with the hooks - in component
const state = useApp()
```

And that pretty much sums up the "state" page.

Next, let's take a look at how we can begin to use it.

#### App.js

Now that everything is setup, `App.js` can look like:

```js
// Other imports
import { AppProvider } from './state'

function App() {
  // other code
  return {
    <AppProvider>
      <Home/>
      // etc.
    </AppProvider>
  }
}
```

Now, our components can hook into the state, as shown in the next section.

#### Page Component

```js
// Other imports
import { useApp, useAppDispatch } from './state'

function Home() {
  // other code
  const dispatch = useAppDispatch()
  const { loaded } = useApp()

  function onClick() {
    dispatch({
      type: 'SOME_TYPE',
      payload: {
        // some data
      }
    })
  }

  return {
    <>
      <span>Loaded: {loaded}</span>
      <button onClick={onClick}>Click</button>
    </>
  }
}
```

And there you go.  That is how we can **read from** and **dispatch actions** from a page component.

## Conclusion

Well, that pretty much concludes this post.  Hope you got a lot of out it!

If you are interested in seeing a mini project that implements this, please checkout the project that was referenced above ([here](https://github.com/bryg217/countries) is the link to avoid scrolling).

All the best!