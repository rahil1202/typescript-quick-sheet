
# **Topic 2: Advanced Type Features**

TypeScript’s advanced type features let you **manipulate types dynamically**, create **reusable patterns**, and enforce **compile-time safety** for complex applications. These are critical for building large-scale projects.

---

## **1️⃣ Core Concepts**

### 1. Generics

Generics allow types to be **parameterized**, making functions, interfaces, and classes reusable.

```ts
// Generic function
function identity<T>(arg: T): T {
  return arg;
}
const num = identity<number>(42);
const str = identity<string>("Hello");
```

**Generic interface**

```ts
interface ApiResponse<T> {
  data: T;
  error?: string;
}

const response: ApiResponse<User> = { data: { id: 1, name: "Rahil" } };
```

---

### 2. Generic Constraints

Constrain generics to a certain shape or interface.

```ts
interface Lengthwise { length: number; }

function logLength<T extends Lengthwise>(arg: T): T {
  console.log(arg.length);
  return arg;
}

logLength("hello");   // ✅ valid
logLength([1, 2, 3]); // ✅ valid
// logLength(42);     // ❌ error
```

---

### 3. Conditional Types

Conditional types allow you to **compute types based on other types**.

```ts
type IsString<T> = T extends string ? "Yes" : "No";

type Test1 = IsString<string>; // "Yes"
type Test2 = IsString<number>; // "No"
```

**Example with infer:**

```ts
type ElementType<T> = T extends (infer U)[] ? U : T;
type A = ElementType<string[]>; // string
type B = ElementType<number>;   // number
```

---

### 4. Mapped Types

Transform existing types into new types.

```ts
interface Person {
  name: string;
  age: number;
}

// Make all properties optional
type PartialPerson = {
  [K in keyof Person]?: Person[K]
};

// Remap keys
type PrefixedPerson = {
  [K in keyof Person as `my_${string & K}`]: Person[K]
}
```

---

### 5. Template Literal Types

Create new string literal types by combining strings.

```ts
type EventName<T extends string> = `on${Capitalize<T>}Change`;

type ClickEvent = EventName<"click">; // "onClickChange"
type FocusEvent = EventName<"focus">; // "onFocusChange"
```

---

### 6. Key Remapping in Mapped Types

```ts
type Old = { oldKey: string; anotherKey: number };
type NewKeys = { [K in keyof Old as `new_${string & K}`]: Old[K] };
// Result: { new_oldKey: string; new_anotherKey: number }
```

---

### 7. Combining Utility Types with Advanced Types

You can combine `Partial`, `Pick`, `Omit`, and mapped types for **flexible API modeling**.

```ts
type User = { id: number; name: string; email: string };

// API PATCH request type
type UpdateUser = Partial<Pick<User, "name" | "email">>;
```

---

## **2️⃣ Real-World Production Examples (10)**

1. **Generic API Response Wrapper**

```ts
interface ApiResponse<T> { data: T; error?: string; }
```

2. **Generic Redux State Slice**

```ts
interface Slice<T> { items: T[]; loading: boolean; }
```

3. **Form Input Validation**

```ts
function validate<T extends object>(values: T): Partial<Record<keyof T, string>> { return {}; }
```

4. **Dynamic Event Handlers**

```ts
type EventHandler<T extends string> = `on${Capitalize<T>}Change`;
```

5. **Mapping Database Models to DTOs**

```ts
type DTO<T> = { [K in keyof T]?: T[K] };
```

6. **Typed Config Management**

```ts
type ConfigKeys = "host" | "port";
type Config = { [K in ConfigKeys]: string | number };
```

7. **Generic Utility Functions**

```ts
function mergeObjects<T, U>(obj1: T, obj2: U): T & U { return { ...obj1, ...obj2 }; }
```

8. **Polymorphic React Components**

```ts
interface BoxProps<T extends React.ElementType> { as?: T; children?: React.ReactNode; }
```

9. **Key Remapping for API Versioning**

```ts
type Old = { name: string; age: number };
type V2 = { [K in keyof Old as `v2_${string & K}`]: Old[K] };
```

10. **Conditional Types for Error Handling**

```ts
type Result<T> = T extends Promise<any> ? "async" : "sync";
```

---

### ✅ **Summary of Topic 2**

* Generics, constraints, mapped types, template literal types, conditional types, and key remapping are **core advanced TypeScript features**.
* They allow **dynamic, reusable, and type-safe patterns**.
* Real-world usage ranges from **API wrappers, Redux slices, forms, React components, database models**, and **utility functions**.

