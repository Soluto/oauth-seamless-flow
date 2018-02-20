---
title: An OAuth authentication flow for applications that does not require user interaction.
docname: draft-oauth-jwt-otp-client-assertion-03
ipr: trust200902
cat: info
pi:
  sortrefs: 'yes'
  strict: 'yes'
  symrefs: 'yes'
  toc: 'yes'
author:
- ins: O. Levi Hevroni
  name: Omer Levi Hevroni
  email: omerlh@gmail.com
  organization: Soluto by Asurion

--- abstract
This specification defines the use of a One Time Password, encoded as JSON Web Token (JWT) Bearer Token, as a means for requesting an OAuth 2.0 access token as well as for client authentication.

--- middle

# Introduction

## Motivation

Authentication is a crucial part of modern application. There are various authentication methods for client side applications, and all those methods requires user interaction (e.g. login). This is due to the fact that there is no secure way to embed credentials in the application code.

While asking the user to login in order to authenticate the app is a strong authentication solution, it has impact on the application behavior. A login is just another step the user has to complete in order to use the apps, which users don't always like to fulfill.

Also, there are cases for applications without any UI, for example - Internet of Things applications. For those applications, adding a login steps could be a challenge - see {{!RFC2142}} which discuss another solution for console-based applications.

In this document, we propose an extension to OAuth 2.0 protocol that provides a new authentication grant dedicated for those cases. This grant will allow an application to use strong authentication solution without user interaction. 

## Terminology

In this document, the key words "MUST", "MUST NOT", "REQUIRED",
"SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY",
and "OPTIONAL" are to be interpreted as described in {{!RFC2119}}.

# Note to Readers

> **Note to the RFC Editor:**  Please remove this section prior
> to publication.

Development of this draft takes place on Github at: https://github.com/securitytxt/security-txt

# The Specification

