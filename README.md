# node-expose-sspi



Expose the Microsoft Windows SSPI (SSP Interface) to [Node.js®](https://nodejs.org/).

:smirk: Use cases:
- **NTLM** and **Kerberos** SSO authentication, both server and client inside a private windows organization network, for instance an ERP in a private company.
- **Active Directory** access to users for detailed info.

Requirements: Microsoft Windows OS, NodeJS version >=12.16.1. Architecture: x64 or ia32.

## Install

[![license](https://img.shields.io/badge/license-ISC-green.svg)](./LICENSE) [![npm version](https://badge.fury.io/js/node-expose-sspi.svg)](https://badge.fury.io/js/node-expose-sspi)

Just do:

```
npm i node-expose-sspi
```

Note: There is a prebuilt binary node addon that will be installed.

## Usage

### SSO Authentication server use case

```bat
mkdir myproject
cd myproject
npm init -y
npm i express
npm i node-expose-sspi
```

Make an express web server by doing the `server.js` file:

```js
const express = require('express');
const { sso } = require('node-expose-sspi');

const app = express();
app.use(sso.auth());

app.use((req, res, next) => {
  res.json({
    sso: req.sso,
  });
});

app.listen(3000, () => console.log('Server started on port 3000'));
```

```
node server.js
```

Open a Google Chrome web browser and go to `http://localhost:3000`.

**Command line client:** 
- Git Bash: `curl --negotiate -u : http://localhost:3000`
- Powershell: `Invoke-WebRequest -uri http://localhost:3000 -UseDefaultCredentials`.

You should see the JSON result with the browser user authentication info and the authentication method used (NTLM or Kerberos).
```
{
  "sso": {
    "method": "NTLM",
    "user": {
      "domain": "JLG",
      "name": "jlouis",
      "displayName": "Jean-Louis P. GUÉNÉGO",
      "groups": [
        "JLG\\Domain Users",
        "\\Everyone",
// ...
      ],
      "sid": "S-1-5-21-2022955591-1730574677-3210790899-1103",
      "adUser": {
        // adUser filled only if Active Directory is reachable.
        "objectClass": ["top", "person", "organizationalPerson", "user"],
        "cn": ["Jean-Louis P. GUÉNÉGO"],
        "sn": ["GUÉNÉGO"],
        "c": ["FR"],
        "l": ["TORCY"],
        "title": ["IT Consultant"],
        "description": ["My microsoft domain account for demonstrating SSO"],
        "postalCode": ["77200"],
        "physicalDeliveryOfficeName": ["Office of my lovely wife Suzana"],
        "telephoneNumber": ["+33612131415"],
        "givenName": ["Jean-Louis"],
        "initials": ["P"],
        "distinguishedName": [
          "CN=Jean-Louis P. GUÉNÉGO,OU=JLG_LOCAL,DC=jlg,DC=local"
        ],
        "instanceType": [4],
        "whenCreated": ["3/19/2020 10:58:19 AM"],
        "whenChanged": ["3/19/2020 5:40:06 PM"],
        "displayName": ["Jean-Louis P. GUÉNÉGO"],
// ...
        "co": ["France"],
        "company": ["JLG Consulting"],
        "streetAddress": ["2 allée du Commandant Charcot"],
        "wWWHomePage": ["www.jlg-consulting.com"],
// ...
        "sAMAccountName": ["jlouis"],
// ...
        "mail": ["jlguenego@gmail.com"],
        "ADsPath": [
          "LDAP://CN=Jean-Louis P. GUÉNÉGO,OU=JLG_LOCAL,DC=jlg,DC=local"
        ]
      }
    },
// ...
  }
}
```

Note: To read JSON file on Chrome, you should use the [JSON Formatter Chrome Extension](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa).

### SSO Authentication client use case

**[See the complete example](./doc/use-case/client.md)**

## Browsers

### Chrome

No conf. It just works.

### Firefox

Unlike Chrome, NTLM and Kerberos are not activated by default in Firefox. To make it working, you need to follow these steps:

1. Navigate to the URL `about:config`.
2. Click past the warning of harmful consequences.
3. Type `negotiate-auth` into the filter at the top of the page, in order to remove most of the irrelevant settings from the list.
4. Double-click on `network.negotiate-auth.trusted-uris`. A dialogue box for editing the value should appear.
5. Enter the required hostname(s) and/or URL prefix(es) then click OK. For the above example, it is `http://localhost:3000`

[More detailed info here](http://www.microhowto.info/howto/configure_firefox_to_authenticate_using_spnego_and_kerberos.html).

### Edge

Edge does not require any configuration. But the browser ask the credentials to the user each time it is started.

## API

The API is automatically documented with [typedoc](https://github.com/TypeStrong/typedoc).

**[Access to the detailed API document](./doc/api/README.md)**. 

You should read all the [sso source code](./src). You will see how powerfull the native primitive exposed in `api.sspi`, and `api.adsi` can bring to you.

You can also read the `mocha` [unit tests](./test) to see small examples.

## Typescript

This module is written in two part: one native in C++ (Windows toolchain) and the other one in Typescript.
So anything else is needed to use this module in typescript code.

[Typescript example](./doc/typescript.md)

## Authentication protocols

### Kerberos

Kerberos is recommanded for production running. For running with Kerberos protocol, both client and server needs to be joined on a [Windows Domain](https://en.wikipedia.org/wiki/Windows_domain).

3 conditions must be met for running Kerberos:
- The node server, running `node-expose-sspi` needs to be run as a domain user with service principal name (SPN) declared in Active Directory.
- The client browser needs to be run on a windows domain account.
- The website url needs to be declared in a white list of intranet website.

[You can find more detail in the Kerberos dedicated documentation](./doc/Kerberos.md).

### NTLM

If you are not on a Windows Domain, `node-expose-sspi` will use the NLTM authentication protocol.

If both the server and the client are on a Windows Domain, NTLM will be used if the Kerberos conditions are not met. [See the Kerberos chapter of this README](#Kerberos).

The NTLM protocol is less secure than Kerberos and not secure at all if you are not under an HTTPS connection. This is because both login and password hash go on the HTTP request, just encoded in base64...

Another thing bad in NTLM is that browsers sometimes popup a dialog box to ask credentials to the user. Your users don't like that. This is a bad user experience.

## Production running

### Server behind a reverse proxy

[Example: node server behind an IIS proxy](./doc/use-case/production-windows.md)

## Examples

To run the examples, just clone this project.

```
git clone https://github.com/jlguenego/node-expose-sspi.git
npm i
cd node-expose-sspi
cd examples
cd <***example-name***>
```

Look also at the `README.md` of the example.

Examples :
- [Express simple](./examples/express-simple/)
- [Koa simple](./examples/koa-simple/)
- [Fastify simple](./examples/fastify-simple/)
- [Restify simple](./examples/restify-simple/)
- [Reverse proxy example](./examples/reverse-proxy/)
- [Angular SSO example](https://github.com/jlguenego/angular-sso-example)
- [React SSO example](https://github.com/jlguenego/react-sso-example)
- [Vue SSO example](https://github.com/jlguenego/vue-sso-example)
- [HTTP(S) fetch API with SSO](./doc/use-case/client.md)

## Development

As a prerequisites, you need node-gyp and a C++ toolchain installed on your environment.

If you did not installed node-gyp and the C++ toolchain,
please open a **PowerShell command line as an administrator** and do :

```
npm i -g windows-build-tools
```

To compile the native node module, do the following:

```
git clone https://github.com/jlguenego/node-expose-sspi.git
cd node-expose-sspi
npm run build
npm run test
```

There are 2 dev areas :
- **C++ code**: run `npm run dev` to watch the modifications and recompile ASAP.
- **Typescript code**: run `npm run build:tsw` to recompile while watching.

All tests are done with [mocha](https://github.com/mochajs/mocha).

The module [debug](https://github.com/visionmedia/debug) is used for printing some debug info.

## Hardware architecture

Both ia32 and x64 are compiled. At the `postinstall` stage, the `api.node` file matching the computer architecture is copied into the `/lib/api.node` location.

## TODO

Any idea of new features ? Please tell me and raise an issue. :blush:

- write a nestjs example
- write a medium article
- publish on Facebook
- Reddit
- Funding
- Integrate with passport?
- Test with 10000 users.
- UTF8 everywhere


Scenario:
- linux trial.

## Thanks

Thanks to all the people who wrotes the npm modules required by this project.

And a very special thanks to the authors of the [node-sspi](https://github.com/abbr/NodeSSPI) project that helped me writing this one. I considere node-sspi to be the father of node-expose-sspi.

## Author

Jean-Louis GUENEGO <jlguenego@gmail.com> (http://jlg-consulting.com/)

You may participate to complete this project. You can improve this doc, or check the code (memory leak, etc.), create new usefull business cases, etc.

Contributors are welcome!


