---
title: Entities
---

# Entities

## Server class

A server instance

### Constructor arguments:

1. Port - http server port to run. The server runs only after calling `run` method;
2. Host (optional) - string with IP address server bind to;

### Properties:

- `port` - HTTP server port / number (readonly);
- `host` - HTTP server host / string (readonly);
- `server` - Node.JS standtart HTTP server instance (readonly);

### Methods:

- `run` - method to run the server. Returns promise;
- `on` - method to register a request handler. Gets http method, request path and callback function with Request class
  instance (see below). Function should return Response class instance or a promise that returns Response class
  instance. Also you can provide [formidable](https://npmjs.com/package/formidable) options as fourth parameter;
- `use` - method to [define a middleware](./get-started/#define-middlewares)

## Request class

A request object is in every handler function

### Properties:

- `method` - HTTP request method;
- `path` - HTTP request path;
- `headers` - HTTP request headers;
- `ip` - HTTP request IP;
- `queryParams` - HTTP request query parameters;
- `context` - A Map class instance. To provide data between middlewares and request handlers;
- `fields` - HTTP request data (available if you passed formidable options (or empty) object in your 'on' method at
  Server class;
- `files` - An object with uploaded files info. Is formidable files object (available if you passed formidable options (
  or empty) object in your 'on' method at Server class)

## Response class

Create an instance of response class in every request handler and make your handler to return the instance or Promise
returns the Response class instance

### Constructor arguments:

1. HTTP Status code. Set `200` for regular server responses;
2. Entity to send. May be a string, number, object, boolean, null, Buffer or stream;
3. HTTP headers. Not required

### Properties:

- `body` - Response body (readonly);
- `statusCode` - Response status code (readonly);
- `headers` - Response headers (readonly);
- `contentType` - automatically defined response content type
