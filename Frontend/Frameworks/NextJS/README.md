
- [Introduction](#introduction)
- [Setup](#setup)
- [Pre-Rendering](#pre-rendering)
  - [When to Use SSG vs SSR](#when-to-use-ssg-vs-ssr)
- [Pages](#pages)
  - [Pages with Static Routes](#pages-with-static-routes)
  - [Pages with Dynamic Routes](#pages-with-dynamic-routes)
  - [Static Generated Page without Data](#static-generated-page-without-data)
  - [Static Generated Page with Data](#static-generated-page-with-data)
  - [Server-Side Rendering](#server-side-rendering)
  - [getStaticProps()](#getstaticprops)
  - [getStaticPaths()](#getstaticpaths)
  - [getServerSideProps()](#getserversideprops)
- [Code Splitting and Prefetching](#code-splitting-and-prefetching)
- [Static Assets](#static-assets)
- [Metadata / `<Head>`](#metadata--head)
- [Styling](#styling)
  - [Global Styles](#global-styles)
  - [Customizing Sass Options](#customizing-sass-options)
- [Special Pages](#special-pages)
  - [pages/_app.js](#pages_appjs)
  - [pages/_document.js](#pages_documentjs)
  - [pages/404.js](#pages404js)
  - [pages/_error.js](#pages_errorjs)
- [Environment Variables](#environment-variables)
  - [Loading Environment Variables](#loading-environment-variables)
  - [Exposing Environment Variables to the Browser](#exposing-environment-variables-to-the-browser)
- [Routing](#routing)
  - [Index Routes](#index-routes)
  - [Nested Routes](#nested-routes)
  - [Dynamic Route Segments](#dynamic-route-segments)
    - [Catch all routes](#catch-all-routes)
    - [Optional catch all routes](#optional-catch-all-routes)
  - [Linking Between Pages](#linking-between-pages)
  - [Injecting the Router](#injecting-the-router)
  - [Router Object](#router-object)
    - [router.push()](#routerpush)
    - [router.replace()](#routerreplace)
    - [router.prefetch()](#routerprefetch)
    - [router.beforePopState()](#routerbeforepopstate)
    - [router.back()](#routerback)
    - [router.reload()](#routerreload)
    - [router.events](#routerevents)
  - [Shallow Routing](#shallow-routing)
- [API Routes](#api-routes)
  - [Dynamic API Routes](#dynamic-api-routes)
  - [API Middlewares](#api-middlewares)
  - [Custom Config](#custom-config)
- [Automatic Static Optimization](#automatic-static-optimization)
- [Custom Server](#custom-server)
- [next.config.js](#nextconfigjs)

# Introduction
* Next.js provides a solution to all of the above problems:
  * An intuitive **page-based** routing system (with support for **dynamic routes**)
  * Pre-rendering, both **static generation (SSG)** and **server-side rendering (SSR)** are supported on a per-page basis
  * Automatic **code splitting** for faster page loads
  * **Client-side routing** with optimized prefetching
  * Built-in **CSS and Sass** support, and support for any CSS-in-JS library
  * Development environment with **Fast Refresh** support
  * **API routes** to build API endpoints with Serverless Functions
  * Fully extendable


# Setup

```sh
npx create-next-app YOUR_APP_NAME --use-npm --example "https://github.com/vercel/next-learn-starter/tree/master/learn-starter"
```

* So far, we get:
  * Automatic compilation and bundling (with webpack and babel)
  * React Fast Refresh
  * Static generation and server-side rendering of `./pages/`
  * Static file serving. `./public/` is mapped to `/`

# Pre-Rendering
* By default, Next.js pre-renders every page. 
* This means that Next.js generates HTML for each page **in advance**, instead of having it all done by **client-side JavaScript**. 
* Pre-rendering can result in **better performance and SEO**.
* Next.js has two forms of pre-rendering: 
  * **Static Generation (SSG)**
  * **Server-side Rendering (SSR)**
* The difference is in **when** it generates the HTML for a page.
  * **Static Generation**:
    * is the pre-rendering method that generates the HTML at **build time** and will be **reused** on each request. 
    * It can be **cached** by a CDN.
    * you can statically generate pages **with or without data**.
  * **Server-side Rendering**:
    * is the pre-rendering method that generates the HTML **on each request**.
* Next.js lets you choose which pre-rendering form to use **for each page**. 
* You can create a **"hybrid"** Next.js app by using Static Generation for most pages and using Server-side Rendering for others.
* You can also use **Client-side Rendering** along with Static Generation or Server-side Rendering. That means some parts of a page can be rendered entirely by **client side JavaScript**.

![static-generation.png](./static-generation.png)
![server-side-rendering.png](./server-side-rendering.png)

## When to Use SSG vs SSR

* We recommend using Static Generation (with and without data) **whenever possible** 
* Because your page can be **built once** and served by CDN, which makes it much **faster** than having a server render the page on every request.
* You should ask yourself: "*Can I pre-render this page ahead of a user's request?*". 
  * If the answer is yes, then you should choose **Static Generation**.
* On the other hand, Static Generation is **not a good idea** if the page shows frequently updated data, and the page content changes on every request.
* In that case, you can use **Server-side Rendering**. It will be **slower**, but the pre-rendered page will **always be up-to-date**. Or you can skip pre-rendering and use **client-side JavaScript** to populate frequently updated data.

# Pages

* A page is a React Component exported from a file (`.jsx` or `.tsx`) in the `pages` directory.
* The component can have any name, but you must export it as a `default export`.

## Pages with Static Routes
* Pages are **associated with a route** based on their **file name**. For example:
  * **pages/index.jsx** is associated with the `/` route.
  * **pages/posts/first-post.jsx** is associated with the `/posts/first-post` route.

## Pages with Dynamic Routes
* Next.js supports pages with **dynamic routes**. 
* For example, if you create a file called `pages/posts/[id].js`, then it will be accessible at:
  * `posts/1`, 
  * `posts/2`, 
  * etc.

## Static Generated Page without Data
* By default, Next.js pre-renders pages using Static Generation without fetching data.

```tsx
export default function About() {
  return <div>About</div>;
}
```

* This page does not need to fetch any external data to be pre-rendered. 
* In cases like this, Next.js generates **a single HTML file per page during build time**.


## Static Generated Page with Data
* Some pages require fetching external data for pre-rendering. 
* There are two scenarios, and one or both might apply. 
* In each case, you can use a **special function** Next.js provides:
  * Your **page content** depends on external data: Use `async getStaticProps()`.
  * Your **page paths** depend on external data: Use `async getStaticPaths()` (usually in addition to `getStaticProps()`).
* **Scenario 1**: Your **page content** depends on external data

```tsx
// FILE: ./pages/blog.tsx

// This function gets called at build time
export async function getStaticProps() {
  const res = await fetch('https://.../posts');
  const posts = await res.json();

  // By returning { props: posts }, the Blog component will receive `posts` as a prop at build time
  return { props: { posts } };
}

export default function Blog({ posts }) {
  // Render posts...
}
```

* **Scenario 2**: Your **page paths** depend on external data
  * suppose that you've only added one blog post (with `id: 1`) to the database. 
  * In this case, you'd only want to pre-render `posts/1` at **build time**.
  * Later, you might add the second post with `id: 2`. Then you'd want to pre-render `posts/2` as well.
  * So your **page paths** that are pre-rendered **depend on external data**


```tsx
// FILE: ./pages/posts/[id].tsx

export async function getStaticPaths() {
  // ...
}

// This also gets called at build time
export async function getStaticProps({ params }) {
  // params contains the post `id`.
  // If the route is like /posts/1, then params.id is 1
  const res = await fetch(`https://.../posts/${params.id}`);
  const post = await res.json();

  // Pass post data to the page via props
  return { props: { post } };
}

export default function Post({ post }) {
  // Render post...
}
```


## Server-Side Rendering
* If a page uses Server-side Rendering, the page HTML is generated on **each request**.
* To use Server-side Rendering for a page, you need to export a function called `async getServerSideProps()`. 
* This function will be **called by the server on every request**.


```tsx
// FILE: ./pages/blog.tsx

// This gets called on every request
export async function getServerSideProps() {
  const res = await fetch('https://.../posts');
  const posts = await res.json();

  // By returning { props: posts }, the Blog component will receive `posts` as a prop
  return { props: { posts } };
}

export default function Blog({ posts }) {
  // Render posts...
}
```



## getStaticProps()

```ts
export async function getStaticProps(context) {
  return {
    props: {} // will be passed to the page component as props
  };
}
```

```ts
// Usage in typescript
import { GetStaticProps } from 'next';

export const getStaticProps: GetStaticProps = async (context) => {
  // ...
}
```

* The `context` parameter is an **object** containing the following keys:
  * `params` contains the route parameters for pages using dynamic routes. 
    * For example, if the page name is `[id].js` , then params will look like `{ id: ... }`. 
    * You should use this together with `getStaticPaths()`, which we’ll explain later.
  * `preview` is true if the page is in the preview mode and undefined otherwise.
  * `previewData` contains the preview data set by `setPreviewData`.
* `getStaticProps` should **return an object** with:
  * `props` - A required object with the props that will be received by the page component. 
    * It should be a serializable object
  * `revalidate` - An optional amount in seconds after which a page re-generation can occur.
* You can write **server-side code directly in `getStaticProps()`**. This includes reading from the filesystem or a database etc.
* You should use `getStaticProps()` if:
  * The data required to render the page is available at **build time** ahead of a user’s request.
  * The data comes from a headless CMS.
  * The data can be publicly cached (not user-specific).
  * The page must be pre-rendered (for SEO) and be very fast — `getStaticProps` generates HTML and JSON files, both of which can be cached by a CDN for performance.
* Note that `getStaticProps()` runs **only on the server-side**. 
  * It will never be run on the client-side. 
  * It won’t even be included in the JS bundle for the browser.
* `getStaticProps()` can **only** be exported **from a page**. You can’t export it from non-page files.
* In development (`next dev`), `getStaticProps()` will be called on **every request**.
* **Incremental Static Regeneration:**
  * With `getStaticProps()` you don't have to stop relying on dynamic content, as **static content can also be dynamic**.
  * Incremental Static Regeneration allows you to **update existing pages** by re-rendering them in the **background** as traffic comes in.
  * Pages never go offline. If the background page re-generation fails, the old page remains unaltered.


```tsx
// This function gets called at build time on server-side.
// It may be called again, on a serverless function, if revalidation is enabled and a new request comes in
export async function getStaticProps() {
  const res = await fetch('https://.../posts');
  const posts = await res.json();

  return {
    props: { posts },
    // Next.js will attempt to re-generate the page:
    // - When a request comes in
    // - At most once every second
    revalidate: 1, // In seconds
  }
}

export default function Blog({ posts }) {
  // render posts
};
```


## getStaticPaths()
* If a page has **dynamic routes** and uses `getStaticProps()` it needs to **define a list of paths** that have to be rendered to HTML at **build time**.
* `getStaticPaths()` only runs at **build time on server-side**.
* You **cannot** use `getStaticPaths()` with `getServerSideProps()`
* In development (`next dev`), `getStaticPaths()` will be called on **every request**.


```ts
export async function getStaticPaths() {
  return {
    paths: [
      { params: { ... } }
    ],
    fallback: boolean;
  };
}
```

```ts
// Usage in typescript
import { GetStaticPaths } from 'next';

export const getStaticPaths: GetStaticPaths = async () => {
  // ...
}
```

* The `paths` key determines **which paths will be pre-rendered**. 
* For example, suppose that you have a page that uses **dynamic routes** named `pages/posts/[id].js`. 
* If you export `getStaticPaths()` from this page and return the following for paths:

```tsx
return {
  paths: [
    { params: { id: '1' } },
    { params: { id: '2' } }
  ],
  fallback: ...
}
```

* Then Next.js will statically generate `posts/1` and `posts/2` at build time using the page component in `pages/posts/[id].js`
* Note that the value for each params **must match the parameters used in the page name**:
  * If the page name is `pages/posts/[postId]/[commentId]`, 
    * then params should contain `postId` and `commentId`.
  * If the page name uses **catch-all routes**, for example `pages/[...slug]`, 
    * then params should contain `slug` which is an array. 
    * For example, if this array is `['foo', 'bar']`, then Next.js will statically generate the `pages/foo/bar`.
  * If the page uses an **optional catch-all route**, 
    * supply `null`, `[]`, `undefined` or `false` to render the root-most route. 
    * For example, if you supply slug: `false` for `pages/[[...slug]]`, Next.js will statically generate the page `pages/`.


* The object returned by `getStaticPaths()` must contain a **boolean** `fallback` key.
* If `fallback` is `false`; 
  * then any paths not returned by `getStaticPaths()` will result in a **404 page**. 
  * You can do this;
    * if you have a small number of paths to pre-render - so they are all statically generated during build time. 
    * It’s also useful when the new pages are not added often. 
  * If you add more items to the data source and need to render the new pages, you’d need to **run the build again**.
* If `fallback` is `true`; 
  * then the behavior of `getStaticProps()` changes:
    * The paths returned from `getStaticPaths()` **will be rendered to HTML at build time**.
    * The paths that **have not been generated at build time** will **not** result in a 404 page. 
    * Instead, Next.js will serve a **“fallback” version** of the page on the **first request** to such a path 
    * In the background, Next.js **will statically generate** the requested path HTML and JSON. This includes running `getStaticProps()`.
    * When that’s done, the browser receives the JSON for the generated path. 
    * This will be used to automatically render the page with the required props. 
    * From the user’s perspective, the page will be swapped from the fallback page to the full page.
    * At the same time, Next.js **adds** this path to the list of **pre-rendered pages**. 
    * Subsequent requests to the same path will serve **the generated page**, just like other pages pre-rendered at build time.


* **Fallback pages**
  * In the “fallback” version of a page:
    * The page’s **`props` will be empty**
    * Using the **router**, you can detect if the fallback is being rendered, `router.isFallback` will be `true`.

```tsx
// FILE: pages/posts/[id].js
import { useRouter } from 'next/router';

// This function gets called at build time
export async function getStaticPaths() {
  return {
    // Only `/posts/1` and `/posts/2` are generated at build time
    paths: [{ params: { id: '1' } }, { params: { id: '2' } }],
    fallback: true, // Enable statically generating additional pages
  };
}

// This also gets called at build time
export async function getStaticProps({ params }) {
  const res = await fetch(`https://.../posts/${params.id}`);
  const post = await res.json();

  return {
    props: { post },
    // Re-generate the post at most once per second
    // if a request comes in
    revalidate: 1,
  };
}

export default function Post({ post }) {
  const router = useRouter();

  // If the page is not yet generated, this will be displayed
  // initially until getStaticProps() finishes running
  if (router.isFallback) {
    return <div>Loading...</div>;
  }

  // Render post...
}
```

* `fallback: true` is useful if your app has a very large number of static pages that depend on data (think: a very large e-commerce site). 
* You want to pre-render all product pages, but then **your builds would take forever**.
* Instead, you may statically generate a small subset of pages and use `fallback: true` for the **rest**. 
* When someone requests a page that’s not generated yet, the user will see the page with a loading indicator. 
* Shortly after, `getStaticProps()` finishes and the page will be rendered with the requested data. 
* From now on, **everyone** who requests the same page will get the **statically pre-rendered page**.
* This ensures that users always have a fast experience while preserving fast builds and the benefits of Static Generation.
* `fallback: true` **will not update generated pages**, for that take a look at **Incremental Static Regeneration** (`revalidate: 1`).



## getServerSideProps()
* Next.js will **pre-render** this page **on each request** using the data returned by `getServerSideProps()`.
* You can write **server-side code directly in** `getServerSideProps()`. This includes reading from the filesystem or a database, etc.
* You should use `getServerSideProps()` only if you need to pre-render a page whose data **must be fetched at request time**.
* Time to first byte (TTFB) will be slower than `getStaticProps()` because;
  * the server must compute the result on every request
  * and the result cannot be cached by a CDN without extra configuration.
* `getServerSideProps()` only **runs on server-side** and never runs on the browser.

```ts
export async function getServerSideProps(context) {
  return {
    props: {}, // will be passed to the page component as props
  };
}
```

```ts
// Usage in typescript
import { GetServerSideProps } from 'next';

export const getServerSideProps: GetServerSideProps = async (context) => {
  // ...
}
```

* The `context` parameter is an object containing the following keys:
  * `params`: If this page uses a dynamic route, params contains the route parameters. 
    * If the page name is `[id].js` , then params will look like `{ id: ... }`.
  * `req`: The HTTP `IncomingMessage` object.
  * `res`: The HTTP `response` object.
  * `query`: The query string.
  * `preview`: preview is true if the page is in the preview mode and false otherwise.
  * `previewData`: The preview data set by `setPreviewData`.
  * `resolvedUrl`: A normalized version of the request URL that strips the _next/data prefix for client transitions and includes original query values.





# Code Splitting and Prefetching
* Next.js does code splitting automatically, so each page only loads what’s necessary for that page. 
* That means when the homepage is rendered, the code for other pages is not served initially.
* Only loading the code for the page you request also means that pages become **isolated**. If a certain page throws an error, the rest of the application would still work.
* Furthermore, in a production build of Next.js, whenever Link components appear in the browser’s viewport, **Next.js automatically prefetches the code for the linked page in the background**. 


# Static Assets

* Next.js can serve static files, like images, under the **top-level `public` directory**. 
* Files inside `public` can be referenced from the root of the application similar to `pages`.
* The `public` directory is also useful for `robots.txt`, Google Site Verification, and any other static assets

```tsx
<img src="/logo.svg" alt="Logo" className="logo" />

// refers to: ./public/logo.svg
```

# Metadata / `<Head>`

* `<title>` is part of the `<head>` HTML tag and it can not be modified in a standart Single-Page Applications due to our application is bound into the `<div>` element withing `<body>` section of page.
* `<Head>` is a React Component that is built into Next.js. 
* It allows you to modify the `<head>` of a page.
* You can import the **Head component** from the [next/head](https://nextjs.org/docs/api-reference/next/head) module.


```tsx
// FILE: ./pages/index.tsx
import Head from 'next/head';

export default function IndexPage() {
  return (
    <div>
      <Head>
        <title>My page title</title>
        <meta property="og:title" content="My page title" key="title" />
      </Head>
      <Head>
        <meta property="og:title" content="My new title" key="title" />
      </Head>
      <p>Hello world!</p>
    </div>
  );
}
```

* To avoid **duplicate tags** in your head you can use the `key` property, which will make sure the tag is **only rendered once**.
* `title`, `meta` or any other elements (e.g. `script`) need to be contained as **direct children** of the `Head` element, or wrapped into maximum one level of `<React.Fragment>` or **arrays**.

> The contents of head get cleared upon **unmounting the component**, so make sure each page completely defines what it needs in head, **without making assumptions** about what other pages added.


# Styling

* Next.js has built-in support for [styled-jsx](https://github.com/vercel/styled-jsx), but you can also use other popular CSS-in-JS libraries such as `styled-components` or `emotion`.
* Next.js has built-in support for **CSS**, **CSS Modules** and **Sass** which allows you to **import** `.css` and `.scss` files.

```scss
// FILE: ./components/Layout/layout.module.scss

.container {
  max-width: 36rem;
  padding: 0 1rem;
  margin: 3rem auto 6rem;
}
```

```tsx
// FILE: ./components/Layout/Layout.tsx

import styles from './layout.module.scss';

export default function Layout({ children }) {
    return <div className={styles.container}>{children}</div>;
}
```

* Out of the box, Next.js allows you to import **Sass** using both the `.scss` and `.sass` extensions. 
* Before you can use Next.js' built-in Sass support, be sure to **install sass**:

```sh
npm install sass
```

## Global Styles
* **CSS Modules** are useful for **component-level** styles. But if you want some CSS to be loaded by **every page**, Next.js has support for that as well.
* To load global CSS files, create a file called `_app.js` under `pages` and add the following content:

```tsx
// FILE: ./pages/_app.tsx

import { AppProps } from 'next/app';
import '../styles/global.scss'

export default function MyApp({ Component, pageProps }: AppProps) {
  return <Component {...pageProps} />
};
```

* You **cannot** import global CSS anywhere else.
* The reason that global CSS **can't be imported** outside of `pages/_app.js` is that global CSS affects all elements on the page.

## Customizing Sass Options
* If you want to configure the Sass compiler you can do so by using `sassOptions` in `next.config.js`.

```js
// FILE: /next.config.js
const path = require('path')

module.exports = {
  sassOptions: {
    includePaths: [path.join(__dirname, 'styles')],
  },
}
```

# Special Pages

## pages/_app.js
* Next.js uses the `App` component to initialize pages. 
* You can override it and control the page initialization. 
* Which allows you to do amazing things like:
  * Persisting layout between page changes
  * Keeping state when navigating pages
  * Custom error handling using `componentDidCatch`
  * Inject additional data into pages
  * Add global CSS
* To override the default `App`, create the file `./pages/_app.js` as shown below:

```tsx
// FILE: ./pages/_app.tsx

// import App from 'next/app';
import { AppProps } from 'next/app';
import '../styles/global.scss'

function MyApp({ Component, pageProps }: AppProps) {
  return <Component {...pageProps} />
};

// Only uncomment this method if you have blocking data requirements for
// every single page in your application. This disables the ability to
// perform automatic static optimization, causing every page in your app to
// be server-side rendered.
//
// MyApp.getInitialProps = async (appContext) => {
//   // calls page's `getInitialProps` and fills `appProps.pageProps`
//   const appProps = await App.getInitialProps(appContext);
//
//   return { ...appProps }
// }

export default MyApp;
```

* The `Component` prop is the **active page**, so whenever you navigate between routes, `Component` will change to the new page. 
* Therefore, any props you send to `Component` will be **received by the page**.
* `pageProps` is an object with the initial props that were preloaded for your page by one of our data fetching methods, otherwise it's an empty object.
* Adding a custom `getInitialProps` in your App will **disable Automatic Static Optimization** in pages without Static Generation.


## pages/_document.js
* A custom `Document` is commonly used to enhance your application's `<html>` and `<body>` tags. 
* To override the default `Document`, create the file `./pages/_document.js` and extend the `Document` class as shown below:

```tsx
import Document, { Html, Head, Main, NextScript, DocumentContext } from 'next/document';

class MyDocument extends Document {
  static async getInitialProps(ctx: DocumentContext) {
    const initialProps = await Document.getInitialProps(ctx);
    return { ...initialProps };
  }

  render() {
    return (
      <Html>
        <Head />
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    );
  }
}

export default MyDocument;
```

* `<Html>`, `<Head />`, `<Main />` and `<NextScript />` are **required** for the page to be properly rendered.
* Custom attributes are allowed as **props**, like `lang`:

```tsx 
<Html lang="en">
```

* The `<Head />` component used here is **not the same** one from `next/head`. 
* The `<Head />` component used here should only be used for any `<head>` code that is common for all pages. 
* For all other cases, such as `<title>` tags, we recommend using `next/head` in your pages or components.
* `Document` is **only rendered in the server**, event handlers like `onClick` **won't work**.



## pages/404.js
* A 404 page may be accessed very often. 
* Server-rendering an error page for every visit increases the load of the Next.js server. 
* This can result in increased costs and slow experiences.
* To avoid the above pitfalls, Next.js provides a static 404 page by default without having to add any additional files.
* To create a custom 404 page you can create a `pages/404.js` file. This file is statically generated at build time.

```tsx
// FILE: pages/404.js
export default function Custom404() {
  return <h1>404 - Page Not Found</h1>
};
```


## pages/_error.js
* By default Next.js provides a 500 error page that matches the default 404 page’s style. 
* This page is not statically optimized as it allows **server-side errors to be reported**. 
* **This is why 404 and 500 (other errors) are separated.**
* 500 errors are handled both **client-side** and **server-side** by the `Error` component.
* `pages/_error.js` is **only** used in **production**. In development you’ll get an error with the call stack to know where the error originated from.
* If you wish to override it, define the file `pages/_error.js` and add the following code:

```tsx
// FILE: pages/_error.js

function Error({ statusCode }) {
  return (
    <p>
      {statusCode
        ? `An error ${statusCode} occurred on server`
        : 'An error occurred on client'}
    </p>
  );
}

Error.getInitialProps = ({ res, err }) => {
  const statusCode = res ? res.statusCode : err ? err.statusCode : 404;
  return { statusCode };
}

export default Error;
```


# Environment Variables
* Next.js comes with built-in support for environment variables, which allows you to do the following:
  * Use `.env.local` to load environment variables
  * Expose environment variables to the **browser**

## Loading Environment Variables
* Next.js has built-in support for loading environment variables from `.env.local` into `process.env` automatically allowing you to use them in Next.js **data fetching methods** and **API routes**.
* Next.js will automatically expand variables (`$VAR`) inside of your `.env*` files. 
* This allows you to **reference** other secrets, like so:
 
```yaml
# .env
HOSTNAME=localhost
PORT=8080
HOST=http://$HOSTNAME:$PORT
```

* If you are trying to use a variable with a `$` in the actual value, it needs to be escaped like so: `\$`


## Exposing Environment Variables to the Browser
* By default all environment variables loaded through `.env.local` are **only available in the Node.js environment**, meaning they **won't** be exposed to the browser.
* In order to expose a variable to the browser you have to prefix the variable with `NEXT_PUBLIC_`. For example:

```yml
NEXT_PUBLIC_ANALYTICS_ID=abcdefghijk
```

* This loads `process.env.NEXT_PUBLIC_ANALYTICS_ID` into the Node.js environment automatically. 
* Allowing you to use it **anywhere in your code**. 
* The value will be inlined into JavaScript sent to the browser because of the NEXT_PUBLIC_ prefix.



# Routing
* Next.js has a **file-system based router**
* When a file is added to the pages directory it's automatically available as a route.

## Index Routes
* The router will automatically route files named index to the root of the directory.
  * `pages/index.js` → `/`
  * `pages/blog/index.js` → `/blog`


## Nested Routes
* The router supports nested files. 
* If you create a nested folder structure files will be automatically routed in the same way still.
  * `pages/blog/first-post.js` → `/blog/first-post`
  * `pages/dashboard/settings/username.js` → `/dashboard/settings/username`


## Dynamic Route Segments
* To match a dynamic segment you can use the bracket syntax. 
* This allows you to match named parameters.
  * `pages/blog/[slug].js` → `/blog/:slug` (/blog/hello-world)
  * `pages/[username]/settings.js` → `/:username/settings` (/foo/settings)
  * `pages/post/[...all].js` → `/post/*` (/post/2020/id/title)
* **Precedence:**
  * Predefined routes take precedence over dynamic routes, and dynamic routes over catch all routes.
  * Take a look at the following examples:
    * `pages/post/create.js`
      * Will match `/post/create`
    * `pages/post/[pid].js` 
      * Will match `/post/1`, `/post/abc`, etc. 
      * But **not** `/post/create`
    * `pages/post/[...slug].js`
      * Will match `/post/1/2`, `/post/a/b/c`, etc. 
      * But **not** `/post/create`, `/post/abc`
* **Example:**
  * Consider the following page `pages/post/[pid].js`


```tsx
import { useRouter } from 'next/router';

export default const Post = () => {
  const router = useRouter()
  const { pid } = router.query

  return <p>Post: {pid}</p>;
};
```
* Any route like `/post/1`, `/post/abc`, etc. will be matched by `pages/post/[pid].js`
* The matched path parameter;
  * will be **sent as a query parameter** to the page, 
  * and it will be **merged with the other query parameters**.
* For example, the route `/post/abc` will have the following query object: `{ "pid": "abc" }`
* Similarly, the route `/post/abc?foo=bar` will have the following query object: `{ "foo": "bar", "pid": "abc" }`
* However, **route parameters will override query parameters with the same name**. 
  * For example, the route `/post/abc?pid=123` will have the following query object: `{ "pid": "abc" }`


### Catch all routes
* Dynamic routes can be extended to **catch all paths** by adding three dots (`...`) inside the brackets. 
* For example `pages/post/[...slug].js` matches 
  * `/post/a`
  * `/post/a/b`
  * `/post/a/b/c` 
  * and so on.
* You can use names other than `slug`, such as: `[...param]`
* Matched parameters 
  * will be sent as a **query parameter** (`slug` in the example) to the page, 
  * and it will always be an array
  * The path `/post/a` will have the following query object: `{ "slug": ["a"] }`
  * And in the case of `/post/a/b`, new parameters will be added to the array, like: `{ "slug": ["a", "b"] }`


### Optional catch all routes
* Catch all routes **can be made optional** by including the parameter in **double brackets** `[[...slug]]`
* For example `pages/post/[[...slug]].js` matches 
  * `/post`
  * `/post/a`
  * `/post/a/b`
  * `/post/a/b/c` 
  * and so on.


## Linking Between Pages

* In Next.js, you use the Link Component from [next/link](https://nextjs.org/docs/api-reference/next/link) to wrap the `<a>` tag. 
* `<Link>` allows you to do **client-side navigation** to a different page in the application.
* If you need to add **attributes** like, for example, `className`, add it to the `a` tag, **not** to the `Link` tag. 


```tsx
import Link from 'next/link';

export default function Home() {
  return (
    <ul>
      <li>
        <Link href="/">
          <a>Home</a>
        </Link>
      </li>
      <li>
        <Link href="/about">
          <a>About Us</a>
        </Link>
      </li>
      <li>
        <Link href="/blog/hello-world">
          <a>Blog Post</a>
        </Link>
      </li>
    </ul>
  )
}
```


## Injecting the Router
* To access the `router` object in a React component you can use `useRouter` hook or `withRouter` HOC.
* Recommended solution is using `useRouter` hook.

```tsx
import { useRouter } from 'next/router';

export default function ActiveLink({ children, href }) {
  const router = useRouter()
  const style = {
    marginRight: 10,
    color: router.pathname === href ? 'red' : 'black',
  }

  const handleClick = (e) => {
    e.preventDefault()
    router.push(href)
  }

  return (
    <a href={href} onClick={handleClick} style={style}>
      {children}
    </a>
  );
}
```


## Router Object
* The following is the definition of the router object returned by both `useRouter` and `withRouter`:
  * `pathname`: String 
    * Current route. 
    * That is the path of the page in `/pages`
  * `query`: Object 
    * The query string parsed to an object. 
    * It will be an empty object during prerendering if the page doesn't have data fetching requirements. 
    * Defaults to `{}`
  * `asPath`: String 
    * Actual path (including the query) shown in the browser

### router.push()
* Handles client-side transitions, this method is useful for cases where `next/link` is not enough.
* You don't need to use `router.push` for external URLs. `window.location` is better suited for those cases.

```ts
router.push(url, as, options)
```

* `url` - The URL to navigate to
* `as` - Optional decorator for the URL that will be shown in the browser.
* `options` - Optional object with the following configuration options:
  * `shallow`: Update the path of the current page without rerunning `getStaticProps`, `getServerSideProps` or `getInitialProps`. Defaults to `false`.



### router.replace()
* `router.replace` will prevent adding a new URL entry into the **history stack**.

```ts
router.replace(url, as, options)
```


### router.prefetch()
* Prefetch pages for faster client-side transitions. 
* This method is only useful for navigations without `next/link`
* `next/link` takes care of prefetching pages **automatically**.
* This is a **production only feature**. Next.js doesn't prefetch pages on development.

```ts
router.prefetch(url, as)
```


### router.beforePopState()
* You may wish to listen to [popstate](https://developer.mozilla.org/en-US/docs/Web/Events/popstate) and do something before the router acts on it.

```ts
router.beforePopState(cb);
```

* `cb`: Function
  * The function to run on incoming popstate events. 
  * The function receives the state of the event as an object with the following props:
    * `url`: String - the route for the new state. This is usually the name of a page
    * `as`: String - the url that will be shown in the browser
    * `options`: Object - Additional options sent by `router.push`
  * If cb returns `false`, the Next.js router **will not handle popstate**, and you'll be responsible for handling it in that case.


### router.back()
* Navigate back in history. 
* Equivalent to clicking the browser’s back button. 
* It executes `window.history.back()`


### router.reload()
* Reload the current URL. 
* Equivalent to clicking the browser’s refresh button. 
* It executes `window.location.reload()`


### router.events
* You can listen to different events happening inside the Next.js Router. 
* Here's a list of supported events:
  * `routeChangeStart(url)` - Fires when a route starts to change
  * `routeChangeComplete(url)` - Fires when a route changed completely
  * `routeChangeError(err, url)` - Fires when there's an error when changing routes, or a route load is cancelled
    * `err.cancelled` - Indicates if the navigation was cancelled
  * `beforeHistoryChange(url)` - Fires just before changing the browser's history
  * `hashChangeStart(url)` - Fires when the hash will change but not the page
  * `hashChangeComplete(url)` - Fires when the hash has changed but not the page
* `url` is the URL shown in the browser, **including the basePath**.



## Shallow Routing
* Shallow routing allows you to change the URL **without running data fetching methods** again, that includes `getServerSideProps`, `getStaticProps`, and `getInitialProps`.


# API Routes
* API routes provide a straightforward solution to build your API with Next.js.
* Any file inside the folder `pages/api` is mapped to `/api/*` and will be **treated as an API endpoint instead of a page**. 
* They are **server-side only bundles** and won't increase your client-side bundle size.

```ts
// FILE: pages/api/user.ts
import { NextApiRequest, NextApiResponse } from 'next'

export default (req: NextApiRequest, res: NextApiResponse ) => {
  res.statusCode = 200
  res.setHeader('Content-Type', 'application/json')
  res.end(JSON.stringify({ name: 'John Doe' }))
};
```

* For an API route to work, you need to **export as default a function**, which then receives the following parameters:
  * `req`: An instance of `http.IncomingMessage`, plus some **pre-built middlewares**
  * `res`: An instance of `http.ServerResponse`, plus some **helper functions**
* To handle different HTTP methods in an API route, you can use `req.method` in your request handler:

```ts
import { NextApiRequest, NextApiResponse } from 'next'

export default (req: NextApiRequest, res: NextApiResponse ) => {
  if (req.method === 'POST') {
    // Process a POST request
  } else {
    // Handle any other HTTP method
  }
}
```

## Dynamic API Routes
* API routes support dynamic routes, and follow the **same file naming rules** used for `pages`.
* It supports path matching exactly same like `pages`
  * Catch all
  * Optionally catch all
  * Precedence

```ts
// FILE: pages/api/post/[pid].ts

import { NextApiRequest, NextApiResponse } from 'next'

export default (req: NextApiRequest, res: NextApiResponse ) => {
  const {
    query: { pid },
  } = req;

  res.end(`Post: ${pid}`);
  // A request to /api/post/abc will respond with the text: "Post: abc".
}
```

## API Middlewares
* API routes provide **built in middlewares** which parse the incoming request (`req`). 
* Those middlewares are:
  * `req.cookies` - An object containing the cookies sent by the request. Defaults to `{}`
  * `req.query` - An object containing the query string. Defaults to `{}`
  * `req.body` - An object containing the body parsed by `content-type`, or `null` if no body was sent


## Custom Config
* Every API route can export a `config` object to change the default configs:

```ts
export const config = {
  api: {
    bodyParser: {
      sizeLimit: '1mb'
    }
  }
}
```


# Automatic Static Optimization
* Next.js automatically determines that a page is static (can be prerendered) if it has no blocking data requirements. 
* This determination is made by the **absence** of `getServerSideProps` and `getInitialProps` in the page.
* This feature allows Next.js to emit **hybrid applications** that contain both server-rendered and statically generated pages.
* **Statically generated pages are still reactive**: Next.js will hydrate your application client-side to give it full interactivity.
* One of the main benefits of this feature is that **optimized pages require no server-side computation**, and can be instantly streamed to the end-user from multiple CDN locations. The result is an ultra fast loading experience for your users.
* If `getServerSideProps` or `getInitialProps` is present in a page, Next.js will switch to render the page **on-demand, per-request** (meaning **Server-Side Rendering**).
* If the above is not the case, Next.js will **statically optimize your page** automatically by **prerendering the page to static HTML.**
* During prerendering, the router's `query` object will be **empty** since we do not have query information to provide during this phase. 
* After hydration, Next.js will **trigger an update** to your application to provide the route parameters in the `query` object.
* This optimization will be **turned off**:
  * If you have a **custom App** with `getInitialProps`
  * If you have a **custom server**


# Custom Server
* It's possible, to start a server 100% programmatically in order to use **custom route patterns**.
* Before deciding to use a custom server please keep in mind that it should **only be used when the integrated router can't meet your app requirements**. 
* A custom server **will remove** important performance optimizations, like:
  * serverless functions 
  * Automatic Static Optimization.



```ts
// FILE: server.js
const { createServer } = require('http');
const { parse } = require('url');
const next = require('next');

const dev = process.env.NODE_ENV !== 'production';
const app = next({ dev });
const handle = app.getRequestHandler();

app.prepare().then(() => {
  createServer((req, res) => {
    // Be sure to pass `true` as the second argument to `url.parse`.
    // This tells it to parse the query portion of the URL.
    const parsedUrl = parse(req.url, true)
    const { pathname, query } = parsedUrl;

    if (pathname === '/a') {
      app.render(req, res, '/a', query);
    } else if (pathname === '/b') {
      app.render(req, res, '/b', query);
    } else {
      handle(req, res, parsedUrl);
    }
  })
  .listen(3000, (err) => {
    if (err) throw err
    console.log('> Ready on http://localhost:3000')
  });
});
```

```json
    // ...
    "scripts": {
        "dev": "node server.js",
        "build": "next build",
        "start": "NODE_ENV=production node server.js"
    },
    // ...
```

* `server.js` **doesn't go through** `babel` or `webpack`. Make sure the syntax and sources this file requires are compatible with the current node version you are running.
* The above `next` import is a function that receives an object with the following options:
  * `dev: Boolean` - Whether or not to launch Next.js in dev mode. Defaults to `false`
  * `dir: String` - Location of the Next.js project. Defaults to `'.'`
  * `quiet: Boolean` - Hide error messages containing server information. Defaults to `false`
  * `conf: object` - The same object you would use in next.config.js. Defaults to `{}`
* The returned app can then be used to let Next.js handle requests as required.



# next.config.js
* For custom advanced behavior of Next.js, you can create a `next.config.js` in the root of your project directory.
* `next.config.js` is a regular Node.js module, not a JSON file. 
* It gets used by the `server` and `build phases`, and it's **not included in the browser build**.
* Avoid using new JavaScript features not available in your target Node.js version. `next.config.js` will **not be parsed** by **Webpack**, **Babel** or **TypeScript**.
* There is so much config to note those in here, so you can check the [official document](https://nextjs.org/docs/api-reference/next.config.js/introduction) for this.


```ts
module.exports = {
  /* config options here */
}
```
or

```ts
module.exports = (phase, { defaultConfig }) => {
  return {
    /* config options here */
  }
}
```

* `phase` is the **current context** in which the configuration is loaded. 
* You can see the available phases [here](https://github.com/vercel/next.js/blob/canary/packages/next/next-server/lib/constants.ts#L1-L4). 
* Phases can be imported from `next/constants`:

```ts
const { PHASE_DEVELOPMENT_SERVER } = require('next/constants');

module.exports = (phase, { defaultConfig }) => {
  if (phase === PHASE_DEVELOPMENT_SERVER) {
    return {
      /* development only config options here */
    };
  }

  return {
    /* config options for all phases except development here */
  };
}
```
