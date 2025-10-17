
# **Topic 7: TypeScript Modules & Namespaces**

TypeScript supports **modular code architecture**, allowing developers to **split code into reusable files** or **group related code under namespaces**. Modules and namespaces help organize **types, functions, classes, and constants** efficiently.

---

## **1️⃣ Core Concepts**

### 1. ES6 Modules (Recommended)

Modules allow **import/export** of code between files.

```ts
// user.ts
export interface User {
  id: number;
  name: string;
}

export const getUser = (id: number): User => ({ id, name: "Rahil" });

// main.ts
import { User, getUser } from "./user";

const u: User = getUser(1);
console.log(u.name); // "Rahil"
```

**Key points:**

* Each file is a module.
* `export` exposes variables, functions, classes, or interfaces.
* `import` brings them into other modules.

---

### 2. Default Exports

```ts
// logger.ts
export default class Logger {
  log(msg: string) { console.log(`[LOG]: ${msg}`); }
}

// app.ts
import Logger from "./logger";
const logger = new Logger();
logger.log("Hello");
```

---

### 3. Re-exporting Modules

```ts
// utils/index.ts
export * from "./logger";
export * from "./helper";

// app.ts
import { Logger, helperFunction } from "./utils";
```

---

### 4. Namespaces (Legacy)

Namespaces group **related code in a global context**. Rarely used in modern code (modules preferred).

```ts
namespace MyApp {
  export interface User { id: number; name: string; }
  export function greet(user: User) { console.log(`Hello ${user.name}`); }
}

const u: MyApp.User = { id: 1, name: "Rahil" };
MyApp.greet(u);
```

**Key points:**

* Use `export` to expose members.
* Avoid polluting the global scope.

---

### 5. Module Resolution

TypeScript can resolve modules using:

* Relative paths: `./user`
* Node modules: `lodash`
* Aliases in `tsconfig.json`:

```json
{
  "compilerOptions": {
    "baseUrl": "./src",
    "paths": { "@utils/*": ["utils/*"] }
  }
}
```

---

### 6. Dynamic Imports

```ts
async function loadLogger() {
  const { Logger } = await import("./logger");
  const logger = new Logger();
  logger.log("Dynamic import works!");
}
loadLogger();
```

---

## **2️⃣ Real-World Production Use Cases (10+)**

1. **React Components**

```ts
// Button.tsx
export const Button = ({ label }: { label: string }) => <button>{label}</button>;
// App.tsx
import { Button } from "./Button";
```

2. **API Service Layer**

```ts
// api/user.ts
export const fetchUser = async (id: number) => { /* fetch logic */ };
// services.ts
import { fetchUser } from "./api/user";
```

3. **Utility Functions**

```ts
// utils/math.ts
export const sum = (a: number, b: number) => a + b;
```

4. **Constants & Enums**

```ts
// constants.ts
export const API_URL = "https://api.example.com";
export enum Roles { Admin, User, Guest }
```

5. **Configuration Management**

```ts
// config.ts
export default { host: "localhost", port: 3000 };
```

6. **Middleware Modules**

```ts
// middleware/auth.ts
export const authMiddleware = (req, res, next) => { /* logic */ }
```

7. **Third-party Module Wrappers**

```ts
// logger-wrapper.ts
import winston from "winston";
export const logger = winston.createLogger({ /* config */ });
```

8. **Feature Modules in Large Apps**

```ts
// features/chat/index.ts
export * from "./chatService";
export * from "./chatController";
```

9. **Shared Types**

```ts
// types.ts
export interface ApiResponse<T> { data: T; error?: string; }
```

10. **Dynamic Module Loading**

```ts
const moduleName = "logger";
const { Logger } = await import(`./${moduleName}`);
new Logger().log("Loaded dynamically");
```

11. **Aliased Imports for Clean Structure**

```ts
import { fetchUser } from "@api/user";
```

---

### ✅ **Summary**

* **Modules** are the modern, recommended approach.
* **Namespaces** are legacy but still exist for global grouping.
* Proper modularization improves **code maintainability, reusability, and scalability**.
* Production usage: **React components, services, middleware, configs, utilities, shared types, feature modules**.


Do you want me to generate that PDF?
