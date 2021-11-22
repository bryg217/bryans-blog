---
title: React Tip - (Dumb) Page Components
description: Small mindset shift that's made my life a whole lot easier.
date: 2021-10-25
tags:
  - Hard Skills
  - React
layout: layouts/post.njk
---

## Description

In this post, I'd like to share a small shift in mindset that made a big difference.  This small shift is related to the way I think about components and their pages, and thus, it affected how I now develop them.  It's a shift that has allowed me to **write higher quality code** and **enjoy the process** (my job) a whole lot more.

So if writing "better" code and enjoying the process sounds like something you are interested in, keep reading.

## The Problem

I remember first being introduced to React components; it was mind blowing.  I was now able to reason about and work on smaller units of a UI,  which made my life much simpler.  React really did (and still does do) a great job of allowing me to express my components as (mostly) declarative code.  For example, I could think about a Twitter post and write/express that as a function (or class in the good ol' days) -- really awesome stuff.  However, for some reason, I always stopped "there"; meaning, I thought of these components as part of some scary big thing called a page.

I would think about components living within a page, which meant that a page was just a group of smaller components in it, and that, of course, still is true.  But, what is not true, is that the code has to technically reflect that.

Take the following example, we have a `Post` component and a `Home` component.

```jsx
function Post({
  text
}) {
  return ( <div>{post}</div> );
}

function Home() {
  const posts = [{ text: 'Hello world' }];
  return (
    <>
      {posts.map(post => <Post text={post.text}/>)}
    </>
  );
}
```

Now, in the real world, `Home` would be a little more complex, because we have to get that data from the server.  So, it would look like (and mind you this is mostly pseudo-code):

```jsx
function Home() {
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    fetchPosts().then(results => setPosts(results))
  }, []);

  return (
    <>
      {posts.map(post => <Post text={post.text}/>)}
    </>
  );
}
```

Now, the code above is fine -- **it works**.  But, the development workflow isn't at all the best.

See, the problem with this is that: **We need a full blown app to be running** (i.e. front end, back end api, etc.).

And for those of you that haven't made this mistake, the problem with needing a full blown app to test a page out is that **it's more complex than it needs to be**.  I need to make sure that the DB is setup, API is setup, etc.

Given the issues described above, enter a simpler way to live as front end developer.

## Hey, Pages Can Be Components Too

Now, we can see how developing a page component, as we did above, can be complex.  Instead, we need to try to keep our lives as simple as possible -- enter isolation.

Rather than thinking about state, fetching data from server, and more, it's best to just think about having a UI look like what the designer(s) intended.  And to do this (drum roll please): **I like to approach developing pages in the same way I develop components**.

Now, what does this look like?

This means thinking of your "core" page as a component, and developing it in the same way you do for something like a button.

Here is what that looks like, using the "home" example above:

```jsx
// Refactor of Home.jsx

function Home({
  posts
}) {
  return (
    <div>
      {posts.map(post => <Post text={posts.text}/>)}
    </div>
  );
}

function HomeWrapper() {
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    fetchPosts().then(results => setPosts(results))
  }, []);

  return (
    <>
      {<Home posts={posts}/>)}
    </>
  );
}
```

Now, notice that I've implemented a **HomeWrapper**, which handles the getting of data, setting of that data, and finally, passing it down to the core page component.  This is what I meant by "dumb" in the title.

The core page component is **responsible for simply rendering data from the API**.  It is not responsible for doing what the wrapper does now (hence, my nickname for "dumb").

This does a few things:

1. Your Focus: Now, rather than juggling 3+ variables in your head, you're focused on just getting the layout to work as you intend.  You also get to see what that data might look like, which will allow you to communicate with back end development much more effectively too.
2. Component Focus: Your component adheres a bit more to SRP (single responsibility principle).  Rather than doing 3+ things, it simply takes data in from a wrapper.
3. Decoupled From APIs: Now, I can use a tool like Storybook to easily develop these pages and then share them with the team.  ALL WITHOUT HAVING TO BUILD AN APPLICATION.

There are many more benefits, but those are the top ones in my experience.

So again, the key takeaway for today's post is: **think about your page as you would other components -- to a degree**.  Of course, this is nuanced and you need to know how and when to apply it.  But, that is a topic for another discussion.  =)

## Closing Thoughts

There you have it: another tool for your metaphorical tool belt.

As I always say, I don't take an opinionated stance, and surely, there are other patterns that may be more effective.  But again, this is something that has proven useful in my journey.  And now, I hope it helps you on yours.

If you find that I made a mistake, missed something, something needs to be corrected or, you just want to just reach out, definitely feel free to do so at [bryan.guillen217@gmail.com](mailto:bryan.guillen217@gmail.com).

All the best.