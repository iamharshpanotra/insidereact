## 🧠 What is React?

**React** is a **JavaScript library** used for building user interfaces (UIs).  
It lets you create **reusable UI components** that can update automatically when your data changes.

Instead of directly manipulating the DOM (like in vanilla JavaScript), React creates a **virtual DOM** — a lightweight copy of the real DOM — and efficiently updates only the parts that need to change.

---

## ⚙️ What is JSX?

**JSX** stands for **JavaScript XML**.

It’s a **syntax extension for JavaScript** that allows you to **write HTML-like code inside JavaScript**.  
JSX makes your React components easier to read and maintain.

Example:

```jsx
const element = <h1>Hello, world!</h1>;
````

Under the hood, this JSX code is converted (by tools like **Babel**) into a JavaScript function call:

```js
const element = React.createElement('h1', null, 'Hello, world!');
```

So, JSX is **not required**, but it’s **highly recommended** because it’s clean and intuitive.

---

## 🧩 JSX Syntax Rules

When writing JSX, keep these key rules in mind:

1. **Return a single parent element**

   * You can wrap multiple elements using a parent tag like `<div>` or `<> </>` (React Fragment).

   ```jsx
   return (
     <>
       <h1>Hello</h1>
       <p>Welcome to React!</p>
     </>
   );
   ```

2. **Use `className` instead of `class`**

   * Because `class` is a reserved keyword in JavaScript.

   ```jsx
   <div className="container">Content</div>
   ```

3. **Use `{}` to embed JavaScript expressions**

   * You can insert dynamic values directly inside JSX.

   ```jsx
   const name = "Sir";
   return <h1>Hello, {name}!</h1>;
   ```

4. **Attributes in JSX are written in camelCase**

   * For example: `onClick`, `tabIndex`, `maxLength`, etc.

   ```jsx
   <button onClick={handleClick}>Click Me</button>
   ```

---

## 🧱 Custom React Components

A **component** in React is like a **function** that returns JSX.
They are the **building blocks** of any React app.

There are two main types:

1. **Functional Components** (most common and modern)
2. **Class Components** (older, still used in legacy projects)

---

### ✅ Functional Component Example

```jsx
function Welcome() {
  return <h1>Welcome to React, Sir!</h1>;
}
```

Or using **arrow functions** (modern syntax):

```jsx
const Welcome = () => {
  return <h1>Welcome to React, Sir!</h1>;
};
```

To use this component inside another:

```jsx
function App() {
  return (
    <div>
      <Welcome />
    </div>
  );
}
```

---

### ⚙️ Passing Props (Custom Data to Components)

**Props** (short for *properties*) are used to pass data **from parent to child components**.

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}!</h1>;
}

function App() {
  return <Welcome name="Sir" />;
}
```

This is how React components become **reusable** and **dynamic**.

---

### 🎨 Adding Styles in JSX

You can apply styles in multiple ways:

**1. Inline CSS**

```jsx
const style = { color: 'blue', fontSize: '20px' };
return <h1 style={style}>Styled Heading</h1>;
```

**2. External CSS File**

```jsx
// In App.css
h1 {
  color: red;
}

// In App.js
import './App.css';
return <h1>Hello!</h1>;
```

---

## ⚡ Handling Events in React

React event handling is similar to HTML but follows **camelCase syntax**.

Example:

```jsx
function Button() {
  const handleClick = () => {
    alert('Button Clicked!');
  };

  return <button onClick={handleClick}>Click Me</button>;
}
```

---

## 🔁 Rendering Lists in JSX

To render multiple items, use **JavaScript’s `map()`** method:

```jsx
const fruits = ['Apple', 'Banana', 'Cherry'];

function FruitList() {
  return (
    <ul>
      {fruits.map((fruit, index) => (
        <li key={index}>{fruit}</li>
      ))}
    </ul>
  );
}
```

The `key` helps React identify which items changed, added, or removed — essential for performance.

---

## 🚧 Conditional Rendering

You can conditionally render content in JSX using `if`, `&&`, or the ternary `?` operator.

```jsx
const isLoggedIn = true;

function Greeting() {
  return (
    <div>
      {isLoggedIn ? <h1>Welcome Back!</h1> : <h1>Please Sign In</h1>}
    </div>
  );
}
```

---

## 🧮 Combining Components Together

You can create multiple small components and combine them into a larger one.

```jsx
function Header() {
  return <h1>My App</h1>;
}

function Footer() {
  return <p>© 2025 React Learning</p>;
}

function App() {
  return (
    <div>
      <Header />
      <p>This is the body content.</p>
      <Footer />
    </div>
  );
}
```

---

## 🧰 How JSX Works Behind the Scenes

When you write JSX like this:

```jsx
<h1>Hello, React!</h1>
```

It’s converted by Babel into:

```js
React.createElement("h1", null, "Hello, React!");
```

Then React converts this into a **Virtual DOM Object**, like:

```js
{
  type: 'h1',
  props: {
    children: 'Hello, React!'
  }
}
```

This virtual representation is then compared with the previous one to update only what’s changed — making React **fast and efficient**.

---

## 🧗 Next Steps

Once you understand JSX and custom components, the next topics to explore are:

1. **State Management (useState hook)**
2. **useEffect and Lifecycle Methods**
3. **React Router for navigation**
4. **Context API for global data**
5. **Component Reusability and Performance Optimization**

---

## ✅ Summary

| Concept                   | Description                        |
| ------------------------- | ---------------------------------- |
| **React**                 | Library for building UIs           |
| **JSX**                   | HTML-like syntax inside JavaScript |
| **Component**             | Reusable UI block                  |
| **Props**                 | Data passed to components          |
| **Event Handling**        | Handling user interactions         |
| **Conditional Rendering** | Render based on logic              |
| **Lists & Keys**          | Efficiently render collections     |

---

## 🚀 Notes
1. Prefer <> when you just need to group JSX elements without introducing unnecessary HTML wrappers.
2. map() is a javascript array function, used to React for rendering lists dynamically. 
---