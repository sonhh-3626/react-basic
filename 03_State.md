# Module 3: State & Hook useState (Managing Dynamic Data)

## 1. ReactJS States

### 1.1. Properties vs States in ReactJS

| Attribute     | Props                                   | State                                     |
| :------------ | :-------------------------------------- | :---------------------------------------- |
| Definition    | Values passed from parent to child      | Values managed within the component       |
| Managed by?   | Parent component                        | The component itself                      |
| Can change?   | ❌ Immutable                            | ✅ Mutable (using `setState` or `setX`)   |
| Use case      | Passing data from parent → child (static UI based on data) | Managing dynamic data, user interaction, changing UI over time |
| Example       | `<Button label="OK" />` (label is props) | `const [count, setCount] = useState(0)` (count is state) |

### 1.2. Adding a State Variable

To add a state variable, import `useState` from React at the top of the file:

```javascript
import { useState } from 'react';

const [index, setIndex] = useState(0);
```

**Note:** The `[ ]` syntax here is called array destructuring, and it lets you read values from an array. The array returned by `useState` always has exactly two items.

This is how they work together in `handleClick`:

```javascript
function handleClick() {
  setIndex(index + 1);
}
```

### 1.3. Render and Commit

Imagine that your components are cooks in the kitchen, assembling tasty dishes from ingredients. In this scenario, React is the waiter who puts in requests from customers and brings them their orders. This process of requesting and serving UI has three steps:

1.  **Triggering a render** (delivering the guest’s order to the kitchen)
2.  **Rendering the component** (preparing the order in the kitchen)
3.  **Committing to the DOM** (placing the order on the table)

### 1.4. State as a Snapshot

When React re-renders a component:

1.  React calls your function again.
2.  Your function returns a new JSX snapshot.
3.  React then updates the screen to match the snapshot your function returned.

When React calls your component, it gives you a snapshot of the state for that particular render. Your component returns a snapshot of the UI with a fresh set of props and event handlers in its JSX, all calculated using the state values from that render.

## 2. Real-world Example (Counter App)

```javascript
import { useState } from "react";

function Counter({ step }) {
  // State: stores the count
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + step); // Update state
  }

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={handleClick}>+{step}</button>
    </div>
  );
}

export default function App() {
  return (
    <div>
      {/* step is props */}
      <Counter step={1} />
      <Counter step={5} />
    </div>
  );
}
```

**Explanation:**

*   `step` is `props` (passed from parent to child).
*   `count` is `state` (internal to the `Counter` component).

## 3. Real-world Use Case

**Example: Shopping Cart in e-commerce**

*   **Props:** List of products (from server/parent component).
*   **State:** Quantity of products in the shopping cart (user adds/removes → UI updates immediately).

```javascript
<Cart items={productsFromAPI} />
```

In `<Cart />`:

*   `items` are `props`.
*   `totalPrice`, `quantity` are `state`.

## 4. Common Bugs ⚠️

### 4.1. State not updating immediately

```javascript
setCount(count + 1);
setCount(count + 1);
```

👉 `count` only increases by 1, not 2. This is because React batches updates.

✅ **Fix:**

```javascript
setCount(prev => prev + 1);
setCount(prev => prev + 1); // now increases by 2
```

### 4.2. Confusion between Props and State

*   If data needs to change → use `State`.
*   If data is only for display, passed from parent → use `Props`.

### 4.3. Mutating state directly ❌

```javascript
count++;
setCount(count); // BUG
```

✅ Always use `setCount`.

## 5. Thought-provoking Questions 💡

1.  When should you *not* store data in State?
2.  Why are Props immutable but State can be changed?
3.  If a component only needs to display data without changing it, would you choose to use props or state?

## 6. Mini Exercise 📝

👉 Code a simple Todo List component:

*   Has an input to enter a task.
*   Has an "Add" button to add to the list (stored in state).
*   The list is rendered as a `<ul>`.
*   **Bonus:** Pass a `title` prop from the parent component to display "Todo List of [Name]".

## 7. Module 4: State & Hook useState

### 7.1. Theory

#### 7.1.1. What is State?

State = data that changes within a component.

When state changes, React will re-render the component → the UI is updated.

#### 7.1.2. What are Hooks?

Hooks = special React functions to add features to function components.

Examples: `useState`, `useEffect`, `useContext`, ...

#### 7.1.3. Using `useState`

Used to manage simple state in function components.

**Syntax:**

```javascript
import { useState } from "react";

const [value, setValue] = useState(initialValue);
```

*   `value`: current value.
*   `setValue`: function to update.
*   `initialValue`: initial value.

#### 7.1.4. Updating State & Re-render

When calling `setValue(newValue)` → React re-renders the component → the UI changes.

**Note:** React does not change immediately but batches updates.

### 7.2. Mini Project: Blog ✍️

#### 7.2.1. Converting post data to state

Previously, data was just a regular variable:

```javascript
const posts = [
  { id: 1, title: "Bài viết 1", content: "Nội dung bài viết 1" },
  { id: 2, title: "Bài viết 2", content: "Nội dung bài viết 2" }
];
```

👉 We convert it to state using `useState`:

```javascript
import { useState } from "react";

export default function App() {
  // Manage state for posts
  const [posts, setPosts] = useState([
    { id: 1, title: "Bài viết 1", content: "Nội dung bài viết 1" },
    { id: 2, title: "Bài viết 2", content: "Nội dung bài viết 2" }
  ]);

  return (
    <div>
      <h1>Mini Blog</h1>
      {/* 4.2 Display from state */}
      <ul>
        {posts.map((post) => (
          <li key={post.id}>
            <h3>{post.title}</h3>
            <p>{post.content}</p>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

#### 7.2.2. Explanation

`useState([...])` helps us manage dynamic dat
a.



If we want to add/edit/delete posts later → just call `setPosts(newPosts)`.


React will automatically re-render and the UI will update according to the state.

### 7.3. Extended Exercise 🎯

*   Add an "Add Post" button → when clicked, it adds a new post to `posts`.
*   Add a "Delete" button next to each post → when clicked, it deletes that post from state.

## 8. What is Batch Update?

### 8.1. What is Batch Update in React?

Batch Update = React groups multiple state updates into a single re-render to optimize performance.

If you call `setState` multiple times within the same event handler, React will not re-render immediately each time, but will wait to group all updates and then render only once.

### 8.2. Illustrative Example

```javascript
import { useState } from "react";

export default function App() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
    setCount(count + 1);
    setCount(count + 1);
    console.log("Count in function:", count);
  }

  return (
    <div>
      <h2>{count}</h2>
      <button onClick={handleClick}>+3</button>
    </div>
  );
}
```

👉 When clicking the " +3 " button:

You might expect `count` to increase by 3.

But in reality, it only increases by 1.

Because React batches the 3 `setCount(count + 1)` calls into 1.

### 8.3. How to fix when continuous updates are desired

Use the callback form of `setState` (also called functional update)

```javascript
function handleClick() {
  setCount(prev => prev + 1);
  setCount(prev => prev + 1);
  setCount(prev => prev + 1);
}
```

👉 Now `count` will actually increase by 3 as desired.

### 8.4. What does Batch Update help with?

✅ **Performance Optimization:**

Instead of rendering 3 times → React renders only once.

Reduces the number of DOM manipulations → faster UI.

### 8.5. Thought-provoking Questions for you 🤔

1.  Why would a React app lag if there were no batch updates when many state updates occur continuously?
2.  In what situations should you use `setState(prev => ...)` instead of `setState(value)`?

## 9. Notes when working with `useState`

### 9.1. State is immutable

Do not change state directly.

❌ **Wrong:**

```javascript
const [todos, setTodos] = useState(["Task A"]);
todos.push("Task B"); // direct mutation
```

✅ **Correct:**

```javascript
setTodos([...todos, "Task B"]);
```

### 9.2. State updates are asynchronous

When calling `setState`, the value is not updated immediately.

If you want to use the new value for calculations → use functional updates.

✅ **Example:**

```javascript
setCount(prev => prev + 1);
```

### 9.3. State initialization

`useState(initialValue)` only runs when the component mounts.

If initializing from a heavy computation → use lazy initialization for optimization:

```javascript
const [value, setValue] = useState(() => heavyCalculation());
```

### 9.4. Do not overuse state

Only store what affects the UI.

Example: no need to store derived data (data that can be calculated from other props/state).

❌ **Wrong:**

```javascript
const [doubleCount, setDoubleCount] = useState(count * 2);
```

✅ **Correct:**

```javascript
const doubleCount = count * 2; // calculate directly when rendering
```

### 9.5. Multiple state updates in one event handler → React batch update

`setState` calls within one event will be grouped → only render once.

If you want to update based on the previous value → use `prev`.

### 9.6. State can be any data type

Not just numbers or strings, but also objects/arrays.

When updating objects/arrays, remember to clone first then set.

```javascript
setUser({ ...user, name: "Alice" });
```

### 9.7. State should not be updated directly from render

Calling `setState` directly within the render function will cause an infinite loop.

Only update state in: event handlers, `useEffect`, or valid callbacks.

### 9.8. Use multiple states instead of one complex state

Instead of managing a large object, you can split it into multiple `useState` calls for clearer code.

```javascript
// Not recommended
const [form, setForm] = useState({ name: "", age: 0 });

// Better
const [name, setName] = useState("");
const [age, setAge] = useState(0);
```

### 9.9. `setState` does not merge objects

Unlike class components (`this.setState` merges).

`useState` completely replaces the object.

```javascript
// If done this way -> user will lose age
setUser({ name: "Alice" });

// Correct
setUser(prev => ({ ...prev, name: "Alice" }));
```

### 9.10. Avoid creating unnecessary state from props

If you only need to display props → use them directly.

Only copy props into state if they need to be modified independently.

🎯 **Thought-provoking Question:**

Why is it necessary to clone an object when storing it in state instead of modifying it directly?

Would you use `useState` or `useReducer` when a form has many complex inputs?

## 10. Review Quiz: React State & useState

### Question 1

In React, how do Props differ from State?

A. Props change within the component, State cannot be changed.

B. Props are passed from parent, State is managed within the component.

C. Props are always numbers, State is always objects.

D. There is no difference.

### Question 2

When using `useState`, which syntax is correct?

A. `const [count, setCount] = useState(0);`

B. `const count = useState(0);`

C. `let count = useState(0);`

D. `useState(count, setCount, 0);`

### Question 3

What happens when `setState` (or `setX` in `useState`) is called?

A. React changes the state variable immediately and does not re-render.

B. React updates the state and re-renders the component.

C. State changes synchronously immediately.

D. React only updates the DOM without re-rendering the component.

### Question 4

Why is it necessary to use `prev` when updating state multiple times consecutively?

A. Because `setState` is synchronous.

B. Because React batches updates and state might not have changed yet.

C. Because state cannot store objects.

D. Because props cannot be changed.

### Question 5

In React, when updating an object in state, which method is correct?

A. `setUser({ name: "Alice" });`

B. `setUser(user);`

C. `setUser({ ...user, name: "Alice" });`

D. `user.name = "Alice"; setUser(user);`

### Question 6

Which statement about `useState` is correct?

A. Can only be used in class components.

B. Can be used in function components.

C. Can only be used with number and string data types.

D. Always merges objects like `this.setState`.

### Question 7

When should data *not* be stored in state?

A. When data changes continuously.

B. When data can be calculated from other props/state.

C. When data affects the UI.

D. When data needs to be re-rendered.

---

✅ **Answers:**

1.  B

2.  A

3.  B

4.  B

5.  C

6.  B

7.  B
