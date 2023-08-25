# A Guide to Authentication and Authorization using JWT and Auth0

# 1. Introduction
This guide aims to help you learn the basics of different authentication and authorization mechanisms. Specifically, JSON Web Tokens (JWT) and Auth0. These technologies play a crucial role in securing web applications by ensuring that only authorized users can access certain resources. 

<sup> ^This text was generated with the help of [ChatGPT](https://chat.openai.com/). </sup>

## 1.1. Prerequisites
- Basic understanding of web development concepts.
- Familiarity with HTTP protocol and RESTful APIs.

## 1.2. Authentication vs Authorization
The table below summarizes the differences between authentication and authorization.
| Authentication | Authorization |
| --- | --- |
| Verifies the identity of a user | Verifies whether a user has access to a resource |
| Usually done before authorization | Usually done after authentication |
| Authentication factors like passwords, tokens, biometrics, etc. are used | Factors like roles, permissions, access control lists, etc. are used |
| Eg. Logging in to a website | Eg. Accessing a file on a server, editing a document |

There are many technologies used to implement authentication and authorization. Some of the most popular ones are:
- JSON Web Tokens (JWT)
- OAuth (eg. Auth0)
- OpenID Connect (OIDC)
- SAML
- ...etc

In this guide, we will be focusing on JWT and OAuth. We will use both of these technologies to implement authentication and authorization in a sample web application. Section 2 will cover JWT and section 3 will cover Auth0.

# 2. JSON Web Tokens (JWT)
A JSON Web Token (JWT) is an open standard for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed.

In this section, we will be using JWT to implement authentication and authorization in a sample web application. We will also gain an understanding of managing public routes, ensuring the security of authenticated routes, and effectively employing the axios library to execute API requests while utilizing the authentication token.

We will put together a simple React app with an Express backend. The app will have a login page and a profile page. The login page will have a form to enter the username and password. The profile page will display the user's name and email address.

## 2.1. Setting up the React App
_Prerequisites: Install NodeJS (with npm) and yarn if you haven't already. We suggest that you use this node version for the purposes of this module => LTS v18.17.0, npm is v9.6.7 and yarn v1.22.19_

Create a new React app using the following command:
```bash
npm create react-app jwt-auth
```
This will create our project directory with the name `jwt-auth`. Navigate to the project directory and install the dependencies:
```bash
npm install
```
Start the development server:
```bash
npm start
```
> 📝**Note:** You can also use `yarn start` instead of `npm start` to start the development server.

You will see the default React app running on `localhost:3000`. 

Install the following dependencies:
```bash
npm install react-router-dom axios
```
The react-router-dom package will be used to implement routing in our app. The axios package will be used to make API requests to the backend.

## 2.2. Create Authentication Provider and Context
We will create an `AuthProvider` component with an associated `AuthContext`. The `AuthProvider` component will be used to manage the authentication state of the user. The AuthContext will be used to access the authentication state from any component in the app.

Create `authProvider.js` in `src` > `provider`. Follow the steps below to create the authProvider component:

1. Import the modules and packages required:
```js
import axios from "axios"; // Handle API requests
import { createContext, useContext, useEffect, useState, useMemo, createElement, React } from "react";
```
2. Initialize the `AuthContext`:
```js
const AuthContext = createContext();
```
3. Create the `AuthProvider` component:
```js
const AuthProvider = ({ children }) => {
    // Put the code from 3 (a) to 3 (e) here
}
```
a) Get the authentication token from the local storage if it exists:
```js
const [token, setToken_] = useState(localStorage.getItem("token"));
```
b) Create a function to set the token in the local storage:
```js
const setToken = (token) => {
        setToken_(token);
    };
```
c) Create effect that runs when the token changes:
```js
useEffect(() => {
        if (token) {
            axios.defaults.headers.common["Authorization"] = "Bearer " + token;
            localStorage.setItem("token", token);
        } else {
            delete axios.defaults.headers.common["Authorization"];
            localStorage.removeItem("token");
        }
    }, [token]);
```

d) Memoize the value of the `AuthContext`. Memoization is a technique used to optimize expensive computations by caching the results of previous computations. In this case, we are memoizing the value of the `AuthContext` so that it is not re-computed every time the component re-renders.
```js
const contextValue = useMemo(() => ({ token, setToken }), [token]);
```

e) Return the `AuthContext.Provider` component with the value set to the memoized `contextValue`:
```js
return React.createElement(AuthContext.Provider, { value: contextValue }, children);
```
> 📝**Note**: The parts after this are supposed to be written outside the AuthProvider component.

4. Create a custom hook to access the `AuthContext`:
```js
export const useAuth = () => {
    return useContext(AuthContext);
};
```
5. Export the AuthProvider component:
```js
export default AuthProvider;
```

Your `authProvider.js` file should look like this:
```js
/**
 * Helps to manage the authentication of the user.
 * Partially generated with the help of GitHub Copilot.
 */

import axios from "axios"; // Handle API requests
import { createContext, useContext, useEffect, useState, useMemo, createElement, React } from "react";

// Initialize the authentication context
const AuthContext = createContext();

// Component to wrap the application with the authentication context
// Child components can access the authentication context
const AuthProvider = ({ children }) => {
    // Get current authentication token from local storage if it exists
    const [token, setToken_] = useState(localStorage.getItem("token")); 
    
    // Set the token in local storage
    const setToken = (token) => {
        setToken_(token);
    };
    // Runs when the token changes
    useEffect(() => {
        if (token) {
            axios.defaults.headers.common["Authorization"] = "Bearer " + token;
            localStorage.setItem("token", token);
        } else {
            delete axios.defaults.headers.common["Authorization"];
            localStorage.removeItem("token");
        }
    }, [token]);

    // Memoize the value of the authentication context (like a cache)
    const contextValue = useMemo(() => ({ token, setToken }), [token]);

    // Return the authentication context provider
    return React.createElement(AuthContext.Provider, { value: contextValue }, children);
}

// Hook to access the authentication context
export const useAuth = () => {
    return useContext(AuthContext);
};

// So the other components can access the AuthProvider component
export default AuthProvider;
```
> ⚠️ **_Warning_** ⚠️: Storing access tokens in `localStorage` is not secure. We are doing this for the sake of simplicity to explain how to use JWT. An alternative approach would be to use cookies to store the access token. Other secure storage mechanisms can also be used.

## 2.3. Create Routes for Authorized Access
To safeguard authenticated routes from unauthorized access, we'll create a `ProtectedRoute` component. It ensures only authenticated users can enter, enhancing security. Make the `ProtectedRoute.js` file at src > routes > ProtectedRoute.js to strengthen our app's security.

1. Import the required modules and packages:
```js
import { Navigate, Outlet } from "react-router-dom";
import { useAuth } from "../provider/authProvider";
import React from "react";
```
2. Create the `ProtectedRoute` component:
```js
// Define ProtectedRoute component
export const ProtectedRoute = () => {
    const { token } = useAuth();
    // If the user is not authenticated, go to the login page
    if (!token) {
        return React.createElement(Navigate, { to: "/login" });
    }
    // Otherwise, render the child components
    return React.createElement(Outlet);
};
```

## 2.4. More on Routes
Now that the `ProtectedRoute` component and AuthContext are ready, we can create the routes for our app. Create a `index.js` file in `src` > `routes`. Follow the steps below to create the routes:

1. Import the required modules and packages:
```js
import { RouterProvider, createBrowserRouter, Route } from "react-router-dom";
import {useAuth} from "../provider/authProvider";
import {ProtectedRoute} from "./ProtectedRoute";
```
