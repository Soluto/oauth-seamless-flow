---
title: An OAuth authentication flow for applications that does not require user interaction.
docname: draft-oauth-JWS-otp-client-assertion-latest
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
This specification defines the use of a One Time Password, encoded as JSON Web Token (JWS) Bearer Token, as a means for requesting an OAuth 2.0 access token as well as for client authentication.

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

Development of this draft takes place on Github at:  https://github.com/Soluto/oauth-JWS-otp-client-assertion
# HTTP Parameter Bindings for Transporting Assertions
The OAuth Assertion Framework {{!RFC7521}} defines generic HTTP parameters for transporting assertions (a.k.a. security tokens) during interactions with a token endpoint. This section defines specific parameters and treatments of those parameters for use with JWS Bearer Tokens.

## Using OTP JWS for client authentication
To use a OTP JWS, the client first need to generate the OTP as defined in section <>. Than, the client need to use the following parameter values and encodings.

The value of the "client_assertion_type" is "urn:ietf:params:oauth:client-assertion-type:JWS-otp".

The value of the "client_assertion" parameter contains a single JWS. It MUST NOT contain more than one JWS.

The following example demonstrates client authentication using a JWS during the presentation of an authorization code grant in an access token request (with extra line breaks for display purposes only):

~~~~~~~~~~
     POST /token.oauth2 HTTP/1.1
     Host: as.example.com
     Content-Type: application/x-www-form-urlencoded

     grant_type=token id_token&&
     client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3A
     client-assertion-type%3AJWS-otp&
     client_assertion=eyJhbGciOiJSUzI1NiIsImtpZCI6IjIyIn0.
     eyJpc3Mi[...omitted for brevity...].
     cC4hiUPo[...omitted for brevity...]
~~~~~~~~~~

# JWS format and request processing

## One Time Password generation
To generate one time password (OTP), the client use it's state, created during the registration request, which is not covered in this document. The state consist from 2 numbers: `previous` and `next`, 64 byte. In order to generate a new JWS, the client has to roll this payload. The rolling is done by setting the value of `previous` to the value of `current`, and setting new crypto random value to `next`. For example, assuming this is the current state of the app:
~~~~~~~~~~
previous: 1
next: 2
~~~~~~~~~~
After rolling, this will be the payload:
~~~~~~~~~~
previous: 2
next: 5
~~~~~~~~~~

## Creating the JWS
After rolling the payload, the client can create the JWS.
This is the format of the JWS payload:
~~~~~~~~~~
{
    previous: 2
    next: 5
    client-id: 89
}
~~~~~~~~~~
Where `client-id` is the id used when this client first registered.
To sign the JWS, the client use it's own key, as the one that was genereated during the registration of this client. 
All the fields are required. Any other fields besides those will be ignored.

## Request processing
When the server will recieve the request, it will first extract the client-id from the request.
Then, it will fetch the client information from a storage.
The client information contains the payload used by the client on the last request, and the key needed to verify the signature of the JWS.
The server first verify the signature of the JWS using the matching key.
If the signature is valid, the server can validate the payload:
* If the client's `previous` is equals to the server `new`, the request is valid. The server will issue a token, as specific on RFC []
* If the client `previous` equals to the server `previous`, and the client `next` equals to the server `next` the server an error response as defined on []
* Any other case will be threated by the server as an indication of malicious attack, and should be reported accordenly.
