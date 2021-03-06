# Public and Private Key Message Signing

This project is a proof of concept for verification of private-key-signed messages strings with Node.js.

`client.js` generates signatures for messages with a private key, and `server.js` provides an endpoint for verification
of if `client.js`'s private key was used to create a signature.

## Installation and Setup

Clone this repository and run `npm i` to install:

```
git clone git@github.com:brucewsinc/Bruce-2018.git
npm i
```

**You will need to generate SSH keys within the keys directory.** You can use `npm run generate` to do so automatically on OSX.

```
cd keys
ssh-keygen -b 2048 -t rsa -f test
openssl rsa -in test -pubout -outform PEM -out test.pub
```

## Usage

**To start the server:**

```
npm start yourPasswordHere # Replace with a password of your choosing
```

**Using the client:**

```
npm run send-key yourPasswordHere    # Updating the public key requires authentication
npm run sign 'Your message here'     # This will return a JavaScript Object including the original message and signature
npm run verify 'message' 'signature' # Requests a signature verification from `server.js`
```

## Endpoints

`server.js` has two `POST` endpoints:

#### **POST** `/` For verification of signed messages:

- Required POST parameters:
	- `message` - The plaintext message that was signed
	- `signature` - The signature related to the above message

**Endpoint returns JSON array** with boolean property of `verify`

#### **POST** `/set-public-key` for updating public key:

- Required GET paramters:
	- `pass` The password set when the `server.js` file was initialized

- Required POST parameters:
	- `key` - The public key you would like to set on the server

**Endpoint returns JSON array** with boolean property of `success`

## Initial Requirements

- [x] Node.js only
- [x] Make a server and a client
  - [x] Client should just be a javascript file we can call with different arguments from command line, or a series of js files which are specialized to each endpoint
- [x] Client should authenticate to the Server with a password
  - [x] No real registration needed, just have an endpoint where we can set a password for the user, or let us set it via an argument when starting the server
  - [x] Once submitted, the password should be stored on the server securely
- [x] Allow an authenticated user to store a public key of some kind on the server
- [x] **Client** should be able to sign a given message with its private key
- [x] Allow **anyone** to submit a signed message to the server to verify if it has been signed by the private key associated with a specified user’s public key
- [x] For data storage you can either keep everything in memory or, if you want to use a database, use mongo.
- [x] Upload to a git repository named YourFirstName-2018
  - [x] Include a Readme file which describes how to use your project
