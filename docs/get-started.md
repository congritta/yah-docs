---
title: Get Started
description: Get started with yet-another-http
sidebar_position: 1
---

# Get Started

In this page we describe how to run yet-another-http. We recommend to use TypeScript

## Let's install yet-another-http

Open your terminal and run:

```bash
yarn add yet-another-http
```

## Create a server instance

You need to create a Server class instance. Every instance has it\`s own running http server with separated handlers and
middlewares

```typescript
import Server from "yet-another-http";

const PORT = 8000;
const HOST = '127.0.0.1';

const server = new Server(PORT, HOST);

// You can create multiple server instances
const server2 = new Server(PORT + 1, HOST);
const server3 = new Server(PORT + 2, HOST);
const server4 = new Server(PORT + 3, HOST);
```

## Run the server

You can start listening server port by executing `server.run()` that returns a Promise.

Example:

```typescript
server.run().then(() => {
  console.log(`HTTP Server started at http://${server.host}:${server.port}`);
}).catch((error) => {
  console.error(error)
});
```

## Define handlers

There are two ways to handle HTTP request: middleware and regular handler.

Let`s define regular handler

```typescript
server.on("GET", "/", () => {
  // Handle client request if it`s method is "GET" and path is "/"

  return new Response(200, "Hello World");
  // Eveny handler function shoild return a Response object
  // (or a Promise with Response object)
});
```

or

```typescript
server.on("GET", "/", async() => {

  return new Response(200, "Hello World");
});
```

You can read about Response object [here](./entities/#response-class).

### Fetching client data

You can get HTTP request data (get query, application/json, multipart/form-data and so on)
via [Context](./entities/#context-class) object provided to every handler

```typescript
server.on("GET", "/", (context) => {

  // Read request data (only for POST, PUT requests):
  console.log(context.fields);

  // Read uploaded files (only for POST, PUT requests)
  console.log(context.files);

  // Read request IP address
  console.log(context.ip);

  // Read request headers
  console.log(context.headers)

  // Read reuest query parameters
  console.log(context.queryParams)

  return new Response(200, "Hello World");
}, {
  // Pass here formidable options or leave the object empty
});
```

## Define middlewares

Middlewares are functions that execute on every http request. Middlewares are useful for, example, implementing user
authorization

### Let\`s implement a middleware

```typescript
server.use((context) => {

  // Your code...

  // To trigger next middlewares or a regular handler
  return;

  // To do nothing after middleware (client request will still idle)
  return null;

  // To respond to client request.
  return new Response(200, "Hello World");

  // Also you can respond with Promise that returns one of values above
});
```

### Logger example:

```typescript
server.use((context) => {
  console.log(`New request from ${context.ip}: ${context.method} ${context.path}`)
});
```

No need to call `next` function

## Request context data

You can pass data from middlewares to regular handlers using context data. This is javascript Map class instance

### Let\`s use context data

```typescript
server.use((context) => {
  context.data.set('my-data', "Congritta is awesome!");
});

server.use((context) => {
  context.data.set('my-number', "21");
});

server.on("GET", "/", () => {

  console.log(context.data.get('my-data')); // Congritta is awesome!
  console.log(context.data.get('my-number')); // 21

  return new Reponse(200, "Hello World");
});
```

## File sending

```typescript
server.on("GET", "/", async() => {

  return new Response(200, fs.createReadStream('path to file'), {
    'content-type': 'audio/mp3' // If we're sending mp3 file
  });
});
```
