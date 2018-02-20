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

This document defines how a One Time Password, encoded in a JWS, can be used to authenticate the client. The document will discuss the entire protocol, including the first registration request.

## Terminology

In this document, the key words "MUST", "MUST NOT", "REQUIRED",
"SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY",
and "OPTIONAL" are to be interpreted as described in {{!RFC2119}}.

# Note to Readers

> **Note to the RFC Editor:**  Please remove this section prior
> to publication.

Development of this draft takes place on Github at:  https://github.com/Soluto/oauth-jwt-otp-client-assertion
# HTTP Parameter Bindings for Transporting Assertions
The OAuth Assertion Framework {{!RFC7521}} defines generic HTTP parameters for transporting assertions (a.k.a. security tokens) during interactions with a token endpoint. This section defines specific parameters and treatments of those parameters for use with JWT Bearer Tokens.

## Using OTP JWT for client authentication
To use a OTP JWT, the client first need to generate the OTP as defined in section <>. Than, the client need to use the following parameter values and encodings.

The value of the "client_assertion_type" is "urn:ietf:params:oauth:client-assertion-type:jwt-otp".

The value of the "client_assertion" parameter contains a single JWT. It MUST NOT contain more than one JWT.

The following example demonstrates client authentication using a JWT during the presentation of an authorization code grant in an access token request (with extra line breaks for display purposes only):

~~~~~~~~~~
     POST /token.oauth2 HTTP/1.1
     Host: as.example.com
     Content-Type: application/x-www-form-urlencoded

     grant_type=authorization_code&
     code=n0esc3NRze7LTCu7iYzS6a5acc3f0ogp4&
     client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3A
     client-assertion-type%3Ajwt-otp&
     client_assertion=eyJhbGciOiJSUzI1NiIsImtpZCI6IjIyIn0.
     eyJpc3Mi[...omitted for brevity...].
     cC4hiUPo[...omitted for brevity...]
~~~~~~~~~~

# JWT format and request processing

## OTP generation
To generate OTP, the client use it's state, created during the registration request as defined in section <>. The state consist from 2 numbers, and to generate a new the client has to roll this payload. The rolling is done by setting the value of Old to the current value of New, and setting new random value to New. For example, assuming this is the current state of the app:

~~~~~~~~~~
Old: 1
New: 2
~~~~~~~~~~

After rolling, this will be the payload:
~~~~~~~~~~
Old: 2
New: 1
~~~~~~~~~~