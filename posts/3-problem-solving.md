---
title: 3 Step Framework For Solving CS Problems
description: Checkout the cool "framework" for solving most problems.
date: 2021-10-20
tags:
  - Soft Skills
layout: layouts/post.njk
---

## Overview

Today I want to share a formula that has been useful for solving problems at work.

For those of you that love TLDRs (like me), here are the 3 steps:

1. Analysis
2. Requirements
3. Technical Design

Below, I'll be going through each in detail, and then at the end, I'll use an example to show what it looks like in practice.

Without further ado, let's get started.

## Analysis

This is the first step, obviously.  The key theme here is: **understanding the problem and solution in plain English**.

In order to understand the problem and solution, here are the actual steps:

1. State The Problem: Start with a 1 liner, if you can.  The sentence should clearly state your problem in plain English.
2. List Down Use Cases: Once you understand the problem, you can go ahead and move onto the use cases for said problem.
3. Understanding Current Implementation (optional): This one is really about understanding the current component, part of the system, or whatever it is that you will be working on.  As professionals, we spend a lot of time working within a team, and that means, working with your teammates' (or your old) code.  So, this is a step I utilize **a ton** in order to ensure my solution fits within the code base's current state.
4. Algorithms: Once you understand the issue and the use cases, you can move onto thinking about solutions for each of the use cases, in plain English.
5. Questions: This last one is a special one.  This one can be done after step 1, 2, 3 and of course 4.  Just because I listed it here, doesn't mean that it should always go after 4.  In fact, I find myself asking questions all throughout the process, if I am honest.

In practice, this can be dynamic, but that general flow is usually what my thought process follows.

Next, let's take a look at *requirements*.

## Requirements

This step is pretty self explanatory.  Here you need to gather the requirements of said problem/feature that your solution should incorporate.  These are typically the non-negotiable aspects of a given feature.

As you'll see in the example below, I'll be diving into this a little bit more.

## Technical Design

This is where things begin to get interesting.  Here is where I think about **the implementation**.

So, the main things we want to highlight here are:

1. Files Being Created or Modified
2. Code Being Updated or Added to File:  This is typically your functions, objects, classes and more.  Within React, the functions or classes can be the components themselves.
3. High Level Updates: For each of the pieces of code mentioned in the previous step, talk about the high level updates you would make to said pieces of code, and if applicable, the tests you would write for said code.

Now, as I've eluded to in the previous sections, this is dynamic and over time, you can adjust the process to your liking.  Now, with that being said, let's move onto the next section.

## Reality

There are a few key points I'd like to make before moving onto the final section, below.

And those are:

1. Be Flexible: If you've never tried this process out, follow it.  Make sure you do it more than once, though.  Give it a fair shot.  And then, as I eluded to in the previous section, you can adjust certain parts (i.e. add or remove) depending on the problem you are solving and what works for you.  But definitely, the first few times, follow each of the steps.
2. Don't Be Surprised: You may have missed one or two minor things, especially for the bigger features.  That is totally okay.  Better to miss it now than in production.  If one of your teammates has the time and they are familiar with the piece of code you are working on, send them this and get some feedback.  Typically this is a nice way of ruling out anything you may have missed.

Now, let's dive into the fun example that'll pull everything together.

## Example

### Overview

For this example, we're going to keep it **SIMPLE**.

What we're going to do is head to over to [React Bootstrap's Site](https://react-bootstrap.github.io/components/modal/) (awesome library by the way).  And, we're going to take a look at their modal component.  For this component, we are going to pretend that it's already implemented (because it is).

Given the information above, the problem we want to solve is: **give the user the ability to open a modal, like how we do in the GIF below.  Also, note that this is within a React application**.

<img src="https://bryans-blog.s3.amazonaws.com/3-example-modal.gif"/>

### Analysis

**Problem**

The problem more clearly stated is: We have a button within a page component, and when that button is clicked, it needs to show the modal.

*Note: We are going to leave the handling of closing the modal outside of this for now.  Even though it doesn't make sense, let's pretend that once the modal is open, it's open forever.  Horrible user experience, I know.  But, I want to keep things simple and short.*

**Use Cases**

1. User clicks on button
2. User does not click on button

**Questions**

*Note: Given the simplicity of this feature and the fact that I came up with the specs for it, I do not have questions.  But, for the sake of the example, I'll add one.*

1. Hover State: Is there any sort of hover state style configuration that we would need to add?

*Note: Notice that I did ask the question after use cases, not last.*

**Current Implementation**

* The Modal:
  * Already implemented
  * For the sake of this example, the only thing we need to worry about is passing/telling the modal to be visible on button click (i.e. a state variable)
* Button
  * Already implemented as well
  * All I would have to do is update it on click

**Algorithm**

* User clicks button
* Modal becomes visible (i.e. button tells it to show)

### Requirements

1. Permanent Modal: Once the modal shows, it's forever shown.
2. Standard UI: No UI configurations will be needed for this button.
3. Button Click Event Handler: Popup should only show if user clicks on button, not on hover.

### Technical Design

*Note: Again, I am working within an imaginary project, and thus, I am making up these files.*

* `Page.js`
  * State: Add boolean to account for modal visibility; it will be the data that is passed into the Modal.
  * Button - `onClick`: Update the handler to be `() => setVisible(true)`.
  * Modal: Ensure the modal passes the state variable as props.

### Closing Thoughts

And, that is what it looks like in practice.  We have all three parts thought out, and it looks like we are ready to proceed to implementation.  NICE!

## Conclusion

Well, as always, it was a pleasure writing this out.  I hope you got some value out of it.

If you find that I made a mistake, missed something, something needs to be corrected or, you just want to just reach out, definitely feel free to do so at [bryan.guillen217@gmail.com](mailto:bryan.guillen217@gmail.com).


All the best.