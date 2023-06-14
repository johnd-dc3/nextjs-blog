This is a starter template for [Learn Next.js](https://nextjs.org/learn).

Notes:

- Look up Headless CMS later: https://en.wikipedia.org/wiki/Headless_content_management_system
- static generation for WordPress

# File structure

components
|- layout.js (includes header and depending on the props it receives will return a different layout).
If it receives the `home` prop it will return layout for index page
|- layout.module.css
|
lib (utility functions)
|-posts.js (utility functions to parse markdown data from blog posts)
|
pages
|-posts (contains routes for blog posts)
|-[id].js (dynamically generate routes)

# Pre-rendering and Data fetching

## Pre-rendering

- Pre-rendering is Next.js default. No client-side JS, HTML rendered in advance
- Hydration--minimal JS loads with page, only for page interactivity
- Page will load even is client JS is disabled (Unlike create-react-app);
- HOWEVER, components like <Link /> would not work until JS loads

## Two forms of pre-rendering

- Static generation--create at build time, reuse for every request
- Server-side rendering--generate HTML on every request
- Can choose page by page with Next.js
- Example of server-side rendering necessity: page with frequently updated data

## Static generation with data

- If you need to access external data _at build time_ you can do that too.
- To do this export an async function `getStaticProps` with your component (page)
- Inside the function you fetch data and send as props to your page
- I.e.: if a page has data dependencies, use `getStaticProps`

## Simple blog architecture

- This dmeo uses markdown files
- `/lib/posts.js` contains a utility function (`getSortedPostsData`) that will parse the data on fetch.

## Implement `getStaticProps` (for static generation)

- Import the posts from your utility function and execute it inside `getStaticProps`
- return the data inside the `props` object, and it can be passed to the component as a prop
- **database can also be queried directly**
- In development `getStaticProps` runs on every request
- Query parameters and http headers cannot be used when page is statically generated

## Data fetch at request time

- For this, you need server-side rendering, and can use `getServerSideProps`
- The method will be passed specific parameters
- Client-side rendering can be used to statically generate some parts of the page and fetch other parts (good for dashboard pages)

# Dynamic Routes

## Page path depends on external data (overview)

- **Dynamic URLs**--dynamically generate routes (page paths) from statically generated pages. Paths and pages rely on external data.
- This demo creates dynamic routes for blog posts

- Create page called `[id.js]`
- Export `getStaticPaths` function to return the ids of the posts
- Note: returned ID's must be in an array
- Use `getStaticProps` to fetch data for a particular id

## Render markdown

- Inside `posts.js` utility file, import `remark` and call it insde the previously created `getPostData` function.
- Async needs to be added to the function so that it awaits `remark`
- Because this forces `getPostData` to become an aync function, async must also be added to `getStaticProps` inside [id].js
- In Posts component inside [id].js, render using `<div dangerouslySetInnerHTML={{ __html: postData.contentHtml }} />`
