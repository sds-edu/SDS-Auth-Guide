# A Guide to Authentication and Authorization using JWT and OAuth

# 1. Introduction
This guide aims to help you learn the basics of different authentication and authorization mechanisms. Specifically, JSON Web Tokens (JWT) and OAuth. These technologies play a crucial role in securing web applications by ensuring that only authorized users can access certain resources. 

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
- OAuth
- OpenID Connect (OIDC)
- SAML
- ...etc

In this guide, we will be focusing on JWT and OAuth. We will use both of these technologies to implement authentication and authorization in a sample web application. Section 2 will cover JWT and section 3 will cover OAuth.

# 2. JSON Web Tokens (JWT)
A JSON Web Token (JWT) is an open standard for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed.

