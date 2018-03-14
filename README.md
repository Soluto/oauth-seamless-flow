# oauth-jwt-otp-client-assertion
Draft RFC for a new OAuth client assertion protocl. 
The draft is based on an authentication solution developed at Soluto.

## Why a new protocol?
Authentication is important, but not always simple. 
All current authentication mechanism are based on any user interaction - usually by showing a screen to user where she can enter her credentials.
But user intereaction is not always an option, as it affect the user experience.
App developers need to choose between security and useability, not a simple choice.
This protocl allows them to add strong authentication solution, without affecting the user experience.
To find more about this protocol, you can read this [blog post](https://blog.solutotlv.com/userless-mobile-authentication/?utm_source=github), or watch this [recording](https://www.youtube.com/watch?v=57FrvVvIq6I&index=21&list=PLpr-xdpM8wG-mJASEZ4TqFYtiRgasd-ki&t=0s).
The current version is published as github pages [here](https://soluto.github.io/oauth-jwt-otp-client-assertion/)

## What about the new device flow?
The device flow [protocol](https://tools.ietf.org/html/draft-ietf-oauth-device-flow-07) is for "Browserless and Input Constrained Devices". 
This is another use case where user authentication is not an option, but it solved by authenticating the user on a different device.
This flow allows the app to authenticate withut any user interaction at all.

## Contributing
The draft is developed base on [I-D template](https://github.com/martinthomson/i-d-template).
To add your changes, fork the repo and than create a PR.
