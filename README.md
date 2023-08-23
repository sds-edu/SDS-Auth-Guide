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

# 3. Auth0
Auth0 is a service that provides authentication and authorization as a service. You can connect any app to Auth0 and define identity providers like Google, Facebook, GitHub etc. Auth0 has many login forms, but the Universal Login form is the most popular one. With Universal Login, the user is redirected to the login page of Auth0, authenticated by Auth0 servers and then redirected back to the app.

When Auth0 redirects the user back to the app, the redirect URL carries details about the authenticated user. This information allows us to access the user's profile and other details. We can also use this information to authorize the user to access certain resources.

The redirect URL contains:
1. **access token** - authorize user access to a resource
2. **id token (in JWT format)** - contains information about the user
3. **expires in** - how many seconds until the access token expires
4. **scope** - to authorize app access to user attributes