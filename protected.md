# Protected Route in React (Beginner – Step by Step)
# Project Structure (Simple)
# src/
# ├─ components/
# │   ├─ Login.jsx
# │   ├─ Dashboard.jsx
# │   ├─ ProtectedRoute.jsx
# ├─ services/
# │   └─ authService.js
# ├─ App.jsx

 ----
 ---
# 1️ Login Method (authService.js)

# Handles login & stores token

```jsx
// services/authService.js
import axios from "axios";

const API = axios.create({
  baseURL: "http://localhost:5000/api",
});

export const loginUser = async (formData) => {
  const res = await API.post("/login", formData);

  // store token
  localStorage.setItem("token", res.data.token);

  return res.data;
};
```
---
---
# 2️ Login Component (Login.jsx)

#  Calls login method
```jsx
import React, { useState } from "react";
import { loginUser } from "../services/authService";
import { useNavigate } from "react-router-dom";

const Login = () => {
  const [form, setForm] = useState({ email: "", password: "" });
  const navigate = useNavigate();

  const handleChange = (e) =>
    setForm({ ...form, [e.target.name]: e.target.value });

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      await loginUser(form);
      navigate("/dashboard"); // go to protected page
    } catch (error) {
      alert("Login Failed");
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
    </form>
  );
};

export default Login;
```
---
---
# 3️ ProtectedRoute Method (ProtectedRoute.jsx)

# Checks token only
```jsx
import { Navigate } from "react-router-dom";

const ProtectedRoute = ({ children }) => {
  const token = localStorage.getItem("token");

  // NOT logged in
  if (!token) {
    return <Navigate to="/login" replace />;
  }

  // Logged in
  return children;
};

export default ProtectedRoute;
```
# 5️ Routes Setup (App.jsx)

# Wrap protected pages
```jsx
import { Routes, Route } from "react-router-dom";
import Login from "./components/Login";
import Dashboard from "./components/Dashboard";
import ProtectedRoute from "./components/ProtectedRoute";

function App() {
  return (
    <Routes>
      <Route path="/login" element={<Login />} />

      <Route
        path="/dashboard"
        element={
          <ProtectedRoute>
            <Dashboard />
          </ProtectedRoute>
        }
      />
    </Routes>
  );
}

export default App;
```
---
---
# 6️ Logout Method

# Clears token
```jsx
const logout = () => {
  localStorage.removeItem("token");
  window.location.href = "/login";
};

```
# Use in any component:

<button onClick={logout}>Logout</button>

# Full Flow Explained (Simple English)

# 1️ User logs in
# 2️ Token saved in localStorage
# 3️ ProtectedRoute checks token
# 4️ Token exists → show page
# 5️ Token missing → redirect to login
# 6️ Logout removes token