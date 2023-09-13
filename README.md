# CS3219 Toolbox - Authentication and Authorization
The CS3219 SE Toolbox is a collection of guides and resources to help you get started with the various tools and technologies used CS3219 - Software Engineering Principles and Patterns.

The guides and resources below focus on authentication and authorization. Specifically, JSON Web Tokens (JWT) and Auth0. 

# 1. Introduction
This guide aims to help you learn the basics of different authentication and authorization mechanisms. Specifically, JSON Web Tokens (JWT) and Auth0. These technologies play a crucial role in securing web applications by ensuring that only authorized users can access certain resources. 

<sup> ^This text was generated with the help of [ChatGPT](https://chat.openai.com/). </sup>

## 1.1. Authentication vs Authorization
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

In this guide, we will be focusing on JWT and Auth0. We will use both of these technologies to implement authentication and authorization in a sample web application. Section 2 will cover JWT and section 3 will cover Auth0.

# 2. JSON Web Tokens (JWT)
A JSON Web Token (JWT) is an open standard for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed.

In this section, we will be using JWT to implement authentication and authorization in a sample web application. We will also gain an understanding of managing public routes, ensuring the security of authenticated routes, and effectively employing the axios library to execute API requests while utilizing the authentication token.

We will put together a simple React app with an Express backend. The app will have a login page and a conditionally rendered homepage. The login page will have a form to enter the username and password. The home page displays a message based on whether the user is logged in or not.

To get started fork/clone this repository -> [LINK HERE]

As you can see, the project uses a separate React app for the frontend and a separate Express app for the backend. The React app will be served on `localhost:3000` and the Express app will be served on `localhost:8080`. The project file structure looks like this:
```bash
your-project-folder
├── frontend
    ├── React app
├── backend
    ├── Express app
```
> 📝**Note:** The app is not complete yet. As you read on you will complete it - but first lets have a look at the code that is already there.

## 2.1. Frontend Setup and Explanation
_Prerequisites: Install NodeJS (with npm) and yarn if you haven't already. We suggest that you use this node version for the purposes of this module => LTS v18.17.0, npm is v9.6.7 and yarn v1.22.19_

In the frontend folder, install the following dependencies:
```bash
npm install react-router-dom axios
```
The react-router-dom package will be used to implement routing in our app. The axios package will be used to make API requests to the backend.

**Frontend File Structure (for relevant files only)**

```bash
frontend
├── public
├── src
    ├── Home.js
    ├── Login.js
    ├── App.js
    ├── NavBar.js
    ├── Register.js
├── package.json
├── package-lock.json
```
### 2.1.1. App.js

```jsx
