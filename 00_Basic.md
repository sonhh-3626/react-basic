# 1. Introducing ReactJS

ReactJS is a Javascript library, created by Facebook and Instagram. It also uses a concept called the Virtual DOM that selectively renders subtrees of nodes based upon state changes. ReactJS is not just another Javascript library; many developers consider it to be the V of the MVC application. With ReactJS, we are building a Single Page Application (SPA).

## 1.1 Virtual DOM (Document Object Model)

**Keyword Explanation:**

*   **DOM:** The HTML structure tree of a web page (document tree).
*   **Virtual DOM:** A "virtual" and lighter copy of the real DOM. React uses the Virtual DOM to calculate changes, then only updates the changed parts instead of re-rendering the entire page, leading to higher performance.

**Code Example:**

```javascript
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Bạn đã bấm: {count} lần</p>
      <button onClick={() => setCount(count + 1)}>Tăng</button>
    </div>
  );
}

export default Counter;
```

When `count` changes, React does not re-render the entire `<div>` but only updates the `<p>` inside, thanks to the Virtual DOM.

**Question:**

What would happen to performance if React updated the real DOM directly every time `setCount` was called, especially on a page with thousands of elements, without using the Virtual DOM?

**Real-world Use Case & Common Issues:**

*   **Use Case:** Using Virtual DOM helps speed up applications with many UI updates.
*   **Common Issue:** Unnecessary re-renders due to state/props changes in a parent component causing child components to re-render.
*   **Solution:** `React.memo`, `useMemo`, `useCallback`.

**Exercise:**

Create a `TodoList` component with an input and a button. Each time the button is clicked, a new item should be added to the list. Observe how React only renders the changed part (adding an `<li>`) instead of redrawing the entire list.

## 1.2 MVC (Model-View-Controller)

**Keyword Explanation:**

*   **Model:** Data (state, API response).
*   **View:** UI (components displaying data).
*   **Controller:** Processing logic (event handlers).

React primarily focuses on the View (the "V" in MVC).

**Code Example:**

```javascript
// Model
const todos = ["Học React", "Học NodeJS"];

// View + Controller in React
function TodoApp() {
  return (
    <ul>
      {todos.map((t, i) => (
        <li key={i}>{t}</li>
      ))}
    </ul>
  );
}
```

**Question:**

Why does React not attempt to implement the full M + V + C, but instead focuses only on the View?

**Real-world Use Case & Common Issues:**

*   **Use Case:** Model (data) is often managed by libraries like Redux, Zustand, or React Query.
*   **Common Issue:** Mixing logic (Controller) with UI makes components difficult to reuse.
*   **Solution:** Separate logic into custom hooks.

**Exercise:**

Write a `Counter` component but separate the counting logic into a custom hook `useCounter()`.

## 1.3 Single Page Application (SPA)

**Keyword Explanation:**

*   **SPA:** A web application that runs on a single page, without reloading the entire page when navigating. Page transitions are handled by client-side routing.
*   React Router is a popular library for building SPAs.

**Code Example (React Router v6):**

```javascript
import { BrowserRouter, Routes, Route, Link } from "react-router-dom";

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Trang chủ</Link>
        <Link to="/about">Giới thiệu</Link>
      </nav>
      <Routes>
        <Route path="/" element={<h1>Trang chủ</h1>} />
        <Route path="/about" element={<h1>Giới thiệu</h1>} />
      </Routes>
    </BrowserRouter>
  );
}
```

**Question:**

What are the advantages of SPA compared to traditional web (multi-page app)?
If SEO (Google search) is important, what are the disadvantages of SPA?

**Real-world Use Case & Common Issues:**

*   **Use Case:** Used in dashboards, web apps, social media.
*   **Common Issue:** Refreshing the `/about` page results in a 404 Not Found error on the server.
*   **Solution:** Configure the server (Apache/Nginx) to redirect to `index.html`.

**Exercise:**

Create an SPA with two pages: Home and Profile. The Profile page should display a name entered from an input on the Home page.

## 1.4 Who uses ReactJS?

**Keyword Explanation:**

*   ReactJS is currently the most popular front-end library in the world.
*   Used by: Facebook, Instagram, Netflix, Airbnb, Uber, Shopify, etc.

**Code Example (Netflix style component):**

```javascript
function MovieCard({ title, year }) {
  return (
    <div className="movie-card">
      <h3>{title}</h3>
      <p>{year}</p>
    </div>
  );
}

export default MovieCard;
```

**Question:**

Why do large companies choose React over Angular or Vue?

**Real-world Use Case & Common Issues:**

*   **Use Case:** React is suitable for large applications with many reusable components.
*   **Common Issue:** Performance issues due to rendering large lists (e.g., 10k rows).
*   **Solution:** Virtualized lists (react-window, react-virtualized).

**Exercise:**

Create a `CompanyList` component that displays 5 large companies using React. Each company should have a logo and a short description.

# 2. Setting up React Environment: Vite

## 2.1 Project Setup: Initialize a new React project named `simple-blog-app` using Vite

**Keyword Explanation:**

*   **Vite:** A next-generation build tool that replaces Create React App (CRA).
*   **Advantages:** Extremely fast, supports ESModules, better hot reload.
*   Your sample project will be named `simple-blog-app`.

**Installation Steps:**

1.  Open your terminal and run:
    ```bash
    npm create vite@latest simple-blog-app
    ```
    (If using yarn, replace with `yarn create vite`).

2.  Select framework:
    ```
    React
    ```

3.  Select variant:
    ```
    JavaScript (or TypeScript if you are familiar).
    ```

4.  Install dependencies:
    ```bash
    cd simple-blog-app
    npm install
    ```

5.  Run dev server:
    ```bash
    npm run dev
    ```
    By default, it will run at `http://localhost:5173`.

## 2.2 Render Basic Content in `App.jsx`

**Keyword Explanation:**

*   The root component in Vite React is `App.jsx` (not `App.js` like CRA).
*   JSX allows writing HTML-like code in JS.

**Code Example (`src/App.jsx`):**

```javascript
function App() {
  return (
    <div>
      <h1>Chào mừng đến với Blog của tôi!</h1>
    </div>
  );
}

export default App;
```

**Result:**

When you visit `http://localhost:5173`, you will see the text:
👉 Chào mừng đến với Blog của tôi!

**Question:**

What is the difference if you change `<h1>` to `<p>` or add many other HTML tags?
Why do we use `className` instead of `class` in React?

**Real-world Use Case & Common Issues:**

*   **Use Case:** This is the first step to building a blog SPA (single page application).
*   **Common Issue 1:** Forgetting `export default App` results in "App is not defined" error.
*   **Common Issue 2:** Running `npm run dev` but still seeing a blank page is often due to modifying the wrong `index.html` file or forgetting to import `App` in `main.jsx`.

**Exercise:**

Try modifying `App.jsx` to display:

*   An `<h1>` title with your blog's name.
*   A short `<p>` introduction (e.g., "Đây là nơi tôi chia sẻ kiến thức ReactJS").
*   Add a `<button>` with the text "Bắt đầu đọc blog".

# 3. `simple-blog-app` README

This is a standard `README.md` file to help you:

*   Clone code from GitHub
*   Install dependencies
*   Run locally using Vite
*   (Optional) How to commit & push code back to GitHub

## 🚀 Environment Requirements

*   [Node.js](https://nodejs.org/) >= 16
*   npm (included with Node.js installation) or [yarn](https://yarnpkg.com/)

## 📦 How to Run the Project After Cloning

### 1. Clone repository

```bash
git clone https://github.com/<your-username>/simple-blog-app.git
cd simple-blog-app
```

### 2. Install dependencies

```bash
npm install
```

(or `yarn install` if using yarn)

### 3. Run local development server

```bash
npm run dev
```

### 4. Build production

```bash

npm run build
```

### 5. Preview build

```bash
npm run preview
```

## 🔗 Connect to GitHub

If you want to push this project to your own GitHub:

### Initialize git (if not already initialized):

```bash
git init
git add .
git commit -m "Initial commit"
```

### Add remote repository:

```bash
git remote add origin https://github.com/<your-username>/simple-blog-app.git
```

### Push code:

```bash
git push -u origin main
```

(if the default branch is `master`, replace `main` with `master`)

## 📖 Learning Guide

*   `src/App.jsx`: The root component displaying the blog title.
*   `src/main.jsx`: The file that bootstraps the React application.
*   Uses Vite for fast execution & optimized builds.

# 4. JSX vs JavaScript

## 4.1 What is JSX?

**JSX (JavaScript XML):** An extension syntax for JavaScript that allows writing HTML-like code within JS.

*   Used by React to describe UI.
*   Before execution, JSX is compiled by Babel into pure JavaScript code (`React.createElement`).

**In short:** JSX is not HTML; it just looks like HTML.

## 4.2 Key Differences Between JSX and JavaScript

| Feature         | JavaScript (JS)                     | JSX                                     |
| :-------------- | :---------------------------------- | :-------------------------------------- |
| Syntax          | Pure logic, no HTML                 | Combines JS logic + UI in one file      |
| Writing UI      | Must use `document.createElement()` or HTML strings | Uses `<div>...</div>` like HTML         |
| Needs Compile?  | No                                  | Yes (thanks to Babel)                   |
| HTML Attributes | `class`, `for`                      | `className`, `htmlFor`                  |
| Expressions     | Only written in pure JS             | Can embed JS within `{}`                |

## 4.3 Illustrative Example

**Pure JavaScript:**

```javascript
const h1 = document.createElement("h1");
h1.textContent = "Hello JS!";
document.body.appendChild(h1);
```

**JSX (in React):**

```javascript
function App() {
  return <h1>Hello JSX!</h1>;
}
```

JSX is much more concise and readable.

## 4.4 Question

If JSX is just "sugar syntax" for JS, can React be written without using JSX?

## 4.5 Real-world Use Case & Common Issues

*   **Use Case:** JSX helps React developers write UI easily and intuitively, like writing HTML.
*   **Common Issue 1:** Forgetting to close tags like `<img>` or `<input />` results in a compile error.
*   **Common Issue 2:** Using `class` instead of `className` causes CSS to not apply correctly.
*   **Common Issue 3:** Confusion between `{}` in JSX (embedding JS) and `""` in strings.

## 4.6 Exercise

In the `App.jsx` file, try writing:

```javascript
function App() {
  const username = "IT 日本語先生";

  return (
    <div>
      <h1>Xin chào {username}!</h1>
      <p>Đây là blog đầu tiên của tôi với React.</p>
    </div>
  );
}
```

Change the `username` variable to see how JSX re-renders.
