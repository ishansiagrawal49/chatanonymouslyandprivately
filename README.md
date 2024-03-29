## chat-e2ee

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/muke1908/chat-e2ee) [![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

The project is still in **development** phase.

Working prototype:  
https://chat-e2ee.herokuapp.com  
https://chate2ee.fun

---

This app will allow two mutually agreed users to have a chat in _end-to-end_ encrypted environment. The app itself doesn't track you or ask any infromation from you. Data is owned by **only you** and **only while chatting**. Your private key is generated on your device and never leaves your device. This is not a replacement of your usual chat application.

## Features

1. The end users are **not tracable**.
2. Data is **not** stored on any remote server.
3. Secure image sharing. [Read more](https://github.com/muke1908/chat-e2ee/wiki/Idea:-Secure-image-sharing)  
   **IMPORTANT:** The image sharing feature is imcomplete!! Images are stored to [imagebb](https://mukesh-biswas.imgbb.com/) server. (27/08/2020)
4. **No history** i.e. once chat is closed the data is not recoverable.
5. **Zero** information sharing -- no login/signup.

---

![Open Source Love](https://img.shields.io/badge/Open%20Source-with%20love-CRIMSON.svg) ![GitHub last commit](https://img.shields.io/github/last-commit/muke1908/chat-e2ee) [![Sonarcloud Status](https://sonarcloud.io/api/project_badges/measure?project=com.lapots.breed.judge:judge-rule-engine&metric=alert_status)](https://sonarcloud.io/dashboard?id=muke1908_chat-e2ee) [![](https://img.shields.io/github/issues/muke1908/chat-e2ee?style=flat)](https://github.com/muke1908/chat-e2ee/issues)

**Contribute:**

- [Frontend issues](https://github.com/muke1908/chat-e2ee/issues?q=is%3Aissue+is%3Aopen+label%3Afrontend)
- [Backend issues](https://github.com/muke1908/chat-e2ee/issues?q=is%3Aissue+is%3Aopen+label%3ABackend)
- [![Code Smells](https://sonarcloud.io/api/project_badges/measure?project=muke1908_chat-e2ee&metric=code_smells)](https://sonarcloud.io/project/issues?id=muke1908_chat-e2ee&resolved=false&types=CODE_SMELL)

For installation instruction, go to [developer section](https://github.com/muke1908/chat-e2ee#for-developers).

**Contributor highlight:**

[![](https://sourcerer.io/fame/muke1908/muke1908/chat-e2ee/images/0)](https://sourcerer.io/fame/muke1908/muke1908/chat-e2ee/links/0)[![](https://sourcerer.io/fame/muke1908/muke1908/chat-e2ee/images/1)](https://sourcerer.io/fame/muke1908/muke1908/chat-e2ee/links/1)[![](https://sourcerer.io/fame/muke1908/muke1908/chat-e2ee/images/2)](https://sourcerer.io/fame/muke1908/muke1908/chat-e2ee/links/2)[![](https://sourcerer.io/fame/muke1908/muke1908/chat-e2ee/images/3)](https://sourcerer.io/fame/muke1908/muke1908/chat-e2ee/links/3)[![](https://sourcerer.io/fame/muke1908/muke1908/chat-e2ee/images/4)](https://sourcerer.io/fame/muke1908/muke1908/chat-e2ee/links/4)[![](https://sourcerer.io/fame/muke1908/muke1908/chat-e2ee/images/5)](https://sourcerer.io/fame/muke1908/muke1908/chat-e2ee/links/5)[![](https://sourcerer.io/fame/muke1908/muke1908/chat-e2ee/images/6)](https://sourcerer.io/fame/muke1908/muke1908/chat-e2ee/links/6)[![](https://sourcerer.io/fame/muke1908/muke1908/chat-e2ee/images/7)](https://sourcerer.io/fame/muke1908/muke1908/chat-e2ee/links/7)

---

### How to initiate chat

1. Generate unique link.
2. Share the link with the person you want to chat with.
3. Both users identify themselves.
4. The messages are end-to-end encrypted hence, no one can decrypt your message other than you.

**How the encryption works**

1. Alice and Bob generate a public and private key pair.
2. Alice and Bob share their public keys with each other.
3. Alice encrypts her message with her private key and Bob's public key and sends it to Bob.
4. Bob receives the encrypted message and decrypts it with his private key and Alice's public key.

In this way, no one else can decrypt the message because your private key is never exposed to the internet.
More detailed explanation: https://www.youtube.com/watch?v=GSIDS_lvRv4&t=1s

> We are using NaCL & [TweetNaCL.js](https://github.com/dchest/tweetnacl-js/) library for asymmetric encryption. The NaCL project is being lead by [Daniel J.Bernstein](http://cr.yp.to/djb.html), one of the most prominent Computer Scientists of our era.

---

### Proposed flow

![flow](https://i.imgur.com/2GrBQMz.jpg)

---

### For developers

FE: This project includes a light weight frontend UI - bootstrapped with [create-react-app](https://reactjs.org/docs/create-a-new-react-app.html). The FE client is located in `./client` folder.  
BE: The backend runs on express/nodejs. In production mode, express server exposes the API endpoints and serve the static frontend from `./client/build`.

### Installation

1. Fork the repo.
2. Run `npm install` in root dir.
3. Run `npm run dev` to spin up your client/server. This will run your react app in dev mode and server in watch mode by nodemon.

NOTE: by default `create-react-app` runs webpack-dev-server on port `3000`. The server is configured to run on `3001` port. So make sure that these ports are not blocked on your system.

**Important:** Check `.env.sample` to configure your `.env` file.

We also provide a Dockerfile to build and run the docker image. More details can be found

### Folder structure

- The FE client is located in `./client` which is coupled with the backend
- All the backend controllers goes to `./backend` folder
- Express instance is on `./app.js`
- Entry point is `./index.js`

### APIs
```endpoint: https://chate2ee.fun/api/<path>```

| url                              | method   | payload                         | filename                  | description                                   |
| -------------------------------- | -------- | ------------------------------ | ------------------------- | --------------------------------------------- |
| `/chat-link`                 | `POST`   | `{token}`                      | `/api/index.js`           | to generate unique link to start chat session |
| `/chat-link/status/:channel` | `GET`    |                                | `/api/index.js`           | to check if a channel is valid                |
| `/chat/message`              | `POST`   | `{ channel, sender, message }` | `/api/messaging/index.js` | to send a message to a specific channel       |
| `/chat-link/:channel`        | `DELETE` |                                | `/api/index.js`           | to delete a channel                           |

---

**Messaging flow**:

- Client encrypts message at client-side and sends via REST call.
- Client receives message in realtime via websocket and decrypt locally at client side.
- Your messages can not be recovered if you lose encryption keys.

---

Please follow the convention for commit message.  
https://github.com/conventional-changelog/commitlint/#what-is-commitlint

Example:  
`git commit -m"feat: some relevant message"`

---

#
