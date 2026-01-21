## React + Axios + JWT Authentication Cheat Sheet

A complete reference for register, login, logout, and CRUD operations using Axios with JWT in React.

1️⃣ Axios Setup (api.js)

Create a single Axios instance and attach JWT automatically via interceptor.

```jsx
import axios from "axios";

const API = axios.create({
  baseURL: "http://localhost:5000/api", // Your backend URL
});

// Automatically add token to every request
API.interceptors.request.use((req) => {
  const token = localStorage.getItem("token");
  if (token) {
    req.headers.Authorization = `Bearer ${token}`;
  }
  return req;
});

export default API;
```
---
--
##  Pros:

# Token is added automatically to all requests.

# No need to manually attach headers every time.

## 2️ Auth Functions (authService.js)

#Handles register, login, and logout.

```jsx
import API from "./api";

// Register user
export const registerUser = async (userData) => {
  const res = await API.post("/register", userData);
  if (res.data.token) {
    localStorage.setItem("token", res.data.token);
    localStorage.setItem("user", JSON.stringify(res.data.user));
  }
  return res.data;
};

// Login user
export const loginUser = async (userData) => {
  const res = await API.post("/login", userData);
  if (res.data.token) {
    localStorage.setItem("token", res.data.token);
    localStorage.setItem("user", JSON.stringify(res.data.user));
  }
  return res.data;
};

// Logout user
export const logoutUser = () => {
  localStorage.removeItem("token");
  localStorage.removeItem("user");
};
```
---
---
## 3️ React Components
# Register Form
```jsx
import React, { useState } from "react";
import { registerUser } from "../services/authService";

const Register = () => {
  const [form, setForm] = useState({ name: "", email: "", password: "" });
  const [message, setMessage] = useState("");

  const handleChange = (e) => {
    setForm({ ...form, [e.target.name]: e.target.value });
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      const data = await registerUser(form);
      setMessage(`Welcome ${data.user.name}!`);
    } catch (error) {
      setMessage(error.response?.data?.message || "Registration failed");
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="name" placeholder="Name" onChange={handleChange} />
      <input name="email" placeholder="Email" onChange={handleChange} />
      <input
        name="password"
        type="password"
        placeholder="Password"
        onChange={handleChange}
      />
      <button type="submit">Register</button>
      <p>{message}</p>
    </form>
  );
};

export default Register;
```
---
---
## Login Form
```jsx
import React, { useState } from "react";
import { loginUser } from "../services/authService";

const Login = () => {
  const [form, setForm] = useState({ email: "", password: "" });
  const [message, setMessage] = useState("");

  const handleChange = (e) => setForm({ ...form, [e.target.name]: e.target.value });

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      const data = await loginUser(form);
      setMessage(`Welcome back ${data.user.name}!`);
    } catch (error) {
      setMessage(error.response?.data?.message || "Login failed");
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="email" placeholder="Email" onChange={handleChange} />
      <input
        name="password"
        type="password"
        placeholder="Password"
        onChange={handleChange}
      />
      <button type="submit">Login</button>
      <p>{message}</p>
    </form>
  );
};

export default Login;
```
---
---
# Logout Button
``` jsx
 import React from "react";
import { logoutUser } from "../services/authService";

const Logout = () => {
  const handleLogout = () => {
    logoutUser();
    window.location.reload(); // reset app state
  };

  return <button onClick={handleLogout}>Logout</button>;
};

export default Logout;
```
---
---
# Using Axios Interceptor (Automatic JWT)
```jsx
import API from "./api";

// GET posts
export const getPosts = () => API.get("/posts");

// POST new post
export const createPost = (post) => API.post("/posts", post);

// PUT update post
export const updatePost = (id, post) => API.put(`/posts/${id}`, post);

// DELETE post
export const deletePost = (id) => API.delete(`/posts/${id}`);
```
* Token is automatically attached via interceptor.
---
---
# Optional: Add Headers Manually per Request
```jsx
import axios from "axios";

const API = axios.create({
  baseURL: "http://localhost:5000/api",
});

// GET posts with token manually
export const getPostsManual = () => {
  const token = localStorage.getItem("token");
  return API.get("/posts", { headers: { Authorization: `Bearer ${token}` } });
};
```