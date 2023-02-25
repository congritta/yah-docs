---
title: Plugin API
description: About writing plugins
---

# Plugin API

You can write plugins for yet-another-http. Every plugin is a [Middleware function](./get-started/#define-middlewares).

## Starting to write a plugin

```typescript
import {MiddlewareHandler} from "yet-another-http";

const myPlugin: Middleware = (context) => {
  // Plugin code here...
}

export default myPlugin;
```

You can pass data to other server middlewares or regular handlers
via [context data](./get-started/#request-context-data).
