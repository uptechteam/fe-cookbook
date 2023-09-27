# Basic auth

## What is it?

HTTP Basic Authentication is a simple authentication mechanism built into the HTTP protocol. It is a way to protect resources on a web server by requiring users to provide a username and password when attempting to access those resources.

## Example

```typescript
// src/middleware.ts

import { NextRequest, NextResponse } from "next/server";

const [AUTH_USER, AUTH_PASS] = (process.env.HTTP_BASIC_AUTH || ":").split(":");

export function middleware(req: NextRequest) {
  if (!process.env.HTTP_BASIC_AUTH) return NextResponse.next();
  // HTTP_BASIC_AUTH = login:pass

  const basicAuth = req.headers.get("authorization");

  if (basicAuth) {
    const auth = basicAuth.split(" ")[1];
    const [user, pwd] = Buffer.from(auth, "base64").toString().split(":");

    if (user === AUTH_USER && pwd === AUTH_PASS) {
      return NextResponse.next();
    }
  }

  return new Response("Auth required", {
    status: 401,
    headers: {
      "WWW-Authenticate": 'Basic realm="Secure Area"',
    },
  });
}

export const config = {
  matcher: "/",
};
```

### Explanation

This code appears to be a middleware implemented in a Next.js application to handle HTTP Basic Authentication. Let's break down the code step by step:

#### Import Statements:

```typescript
import { NextRequest, NextResponse } from "next/server";
```

The code imports the NextRequest and NextResponse classes from the "next/server" module. These classes are used to handle incoming requests and generate responses in a Next.js application.

#### Environment Variable Parsing:

```typescript
const [AUTH_USER, AUTH_PASS] = (process.env.HTTP_BASIC_AUTH || ":").split(":");
```

This code extracts the values of AUTH_USER and AUTH_PASS from the HTTP_BASIC_AUTH environment variable or sets default values to : if the variable is not defined. The HTTP_BASIC_AUTH environment variable is expected to contain a string in the format username:password, and this line splits it into two parts: the username and password.

#### Middleware Function:

```typescript
export function middleware(req: NextRequest) {
```

This defines a middleware function that takes a NextRequest object as its argument. Middleware functions in Next.js can intercept and process incoming requests before they reach their destination routes.

#### Check for the Presence of HTTP_BASIC_AUTH:

```typescript
if (!process.env.HTTP_BASIC_AUTH) return NextResponse.next();
```

This line checks if the HTTP_BASIC_AUTH environment variable is not defined. If it's not defined, the middleware returns a NextResponse.next() statement. This essentially means that if HTTP Basic Authentication is not configured, the middleware will allow the request to continue to its intended route without any further authentication checks.

#### Extract Basic Authentication Header:

```typescript
const basicAuth = req.headers.get("authorization");
```

This line retrieves the "Authorization" header from the incoming request, which typically contains the Basic Authentication credentials.

#### Parse and Validate Basic Authentication Credentials:

```typescript
if (basicAuth) {
  const auth = basicAuth.split(" ")[1];
  const [user, pwd] = Buffer.from(auth, "base64").toString().split(":");
  if (user === AUTH_USER && pwd === AUTH_PASS) {
    return NextResponse.next();
  }
}
```

If the "Authorization" header is present, the code extracts the Base64-encoded credentials from it. It then decodes these credentials and splits them into a user and pwd (password). It compares these credentials with the AUTH_USER and AUTH_PASS values obtained from the environment variables. If they match, it allows the request to proceed by returning NextResponse.next(). If the credentials do not match, it will proceed to return a 401 Unauthorized response.

#### Return Unauthorized Response:

```typescript
return new Response("Auth required", {
  status: 401,
  headers: {
    "WWW-Authenticate": 'Basic realm="Secure Area"',
  },
});
```

If the credentials do not match or if the "Authorization" header is missing, the middleware returns a 401 Unauthorized response to the client. It includes the "WWW-Authenticate" header, which indicates that Basic Authentication is required and provides a realm name ("Secure Area") for the authentication.

#### Export Configuration:

```typescript
export const config = {
  matcher: "/",
};
```

The middleware exports a config object with a matcher property set to "/". This specifies that this middleware should be applied to all routes.
In summary, this code is a Next.js middleware that checks for HTTP Basic Authentication credentials in incoming requests. It allows or denies access to routes based on the presence and correctness of these credentials, with the credentials and realm name configured via environment variables. If Basic Authentication is not configured, it allows requests to proceed without authentication.
