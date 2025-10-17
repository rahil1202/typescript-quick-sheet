
# **Topic 9: TypeScript Error Handling & Strict Type Checking**

TypeScript enhances JavaScript’s error handling with **compile-time checks**. Using **strict type options**, you can catch errors early and avoid runtime issues.

---

## **1️⃣ Core Concepts**

### 1. Strict Mode Options (`tsconfig.json`)

```json
{
  "strict": true,
  "noImplicitAny": true,
  "strictNullChecks": true,
  "strictFunctionTypes": true,
  "strictBindCallApply": true,
  "noImplicitThis": true,
  "alwaysStrict": true
}
```

**Benefits:**

* Detects type mismatches
* Prevents `null`/`undefined` errors
* Ensures safe function usage

---

### 2. Null & Undefined Handling

```ts
let name: string | null = null;

function greet(user: string | null) {
  if (user === null) console.log("Hello Guest");
  else console.log(`Hello ${user}`);
}
```

---

### 3. Optional Chaining & Nullish Coalescing

```ts
interface User { profile?: { name?: string } }

const user: User = {};
console.log(user.profile?.name ?? "Anonymous"); // "Anonymous"
```

---

### 4. Error Types

```ts
function throwError(msg: string): never {
  throw new Error(msg);
}
```

* `never` type: function **never returns** (used in critical failures)

---

### 5. Try-Catch with Type Narrowing

```ts
try {
  JSON.parse("invalid json");
} catch (err: unknown) {
  if (err instanceof Error) console.log(err.message);
}
```

* Using `unknown` instead of `any` enforces **type checking** before use.

---

### 6. Assertion Functions

```ts
function assertIsNumber(value: any): asserts value is number {
  if (typeof value !== "number") throw new Error("Not a number");
}

function double(value: any) {
  assertIsNumber(value);
  return value * 2;
}
```

* `asserts value is Type` tells TypeScript the type after assertion.

---

### 7. Exhaustive Checks with `never`

```ts
type Direction = "up" | "down";

function move(dir: Direction) {
  switch(dir) {
    case "up": return "Moving up";
    case "down": return "Moving down";
    default:
      const _exhaustiveCheck: never = dir;
      return _exhaustiveCheck;
  }
}
```

* Ensures **all union cases are handled** at compile-time.

---

## **2️⃣ Real-World Production Examples (10+)**

1. **API Response Error Handling**

```ts
interface ApiResponse<T> { data?: T; error?: string; }
const res: ApiResponse<User> = { error: "Not found" };
if (res.error) throw new Error(res.error);
```

2. **Form Validation**

```ts
function validateEmail(email: string | undefined) {
  if (!email) throw new Error("Email required");
}
```

3. **Database Error Handling**

```ts
async function fetchUser(id: string) {
  try { return await db.findUser(id); }
  catch(err) { console.error("DB error", err); throw err; }
}
```

4. **Optional Chaining in Nested Objects**

```ts
const username = user.profile?.name ?? "Guest";
```

5. **Type Narrowing in Event Handling**

```ts
function handleEvent(e: Event | null) {
  if (e instanceof MouseEvent) console.log(e.clientX);
}
```

6. **Assertion Functions in Utilities**

```ts
function assertArray<T>(value: any): asserts value is T[] {
  if (!Array.isArray(value)) throw new Error("Not an array");
}
```

7. **Exhaustive Checks in Redux Reducers**

```ts
switch(action.type) { case "ADD": ...; default: const x: never = action; }
```

8. **Strict Optional Parameters**

```ts
function greet(name?: string) {
  console.log(`Hello ${name ?? "Guest"}`);
}
```

9. **Network Error Handling in Services**

```ts
try { await fetch("/api"); } catch(err: unknown) { console.error(err); }
```

10. **Critical Runtime Assertions**

```ts
function getElement(id: string) {
  const el = document.getElementById(id);
  if (!el) throw new Error("Element not found");
  return el;
}
```

11. **Type-safe JSON Parsing**

```ts
function parseJSON<T>(json: string): T {
  return JSON.parse(json) as T;
}
```

---

### ✅ **Summary**

* Strict mode improves **type safety**.
* Use **optional chaining**, **nullish coalescing**, and **type guards** to avoid runtime errors.
* `never` and assertion functions help enforce **exhaustive checks**.
* Production usage: **API error handling, form validation, database queries, Redux, optional parameters, network errors**.

---

