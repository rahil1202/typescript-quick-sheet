
# **Topic 6: TypeScript Generics & Advanced Utility Types**

Generics allow you to **write reusable, type-safe code** by parameterizing types. Combined with **utility types**, they become extremely powerful in **large-scale applications**.

---

## **1️⃣ Generics**

### 1. Generic Functions

```ts
function identity<T>(arg: T): T {
  return arg;
}

const num = identity<number>(42);   // 42
const str = identity<string>("Hi"); // "Hi"
```

### 2. Generic Interfaces

```ts
interface ApiResponse<T> {
  data: T;
  error?: string;
}

const userResponse: ApiResponse<{ id: number; name: string }> = {
  data: { id: 1, name: "Rahil" }
};
```

### 3. Generic Classes

```ts
class DataStore<T> {
  private items: T[] = [];
  add(item: T) { this.items.push(item); }
  getAll(): T[] { return this.items; }
}

const store = new DataStore<number>();
store.add(10);
store.add(20);
console.log(store.getAll()); // [10, 20]
```

### 4. Generic Constraints

```ts
interface Lengthwise { length: number; }

function logLength<T extends Lengthwise>(arg: T): T {
  console.log(arg.length);
  return arg;
}

logLength("Hello");   // 5
logLength([1, 2, 3]); // 3
```

---

## **2️⃣ Advanced Utility Types**

TypeScript provides **built-in utility types** to manipulate existing types easily.

| Utility          | Description                     |
| ---------------- | ------------------------------- |
| `Partial<T>`     | Makes all properties optional   |
| `Required<T>`    | Makes all properties required   |
| `Readonly<T>`    | Makes all properties readonly   |
| `Pick<T, K>`     | Select subset of properties     |
| `Omit<T, K>`     | Remove certain properties       |
| `Record<K, T>`   | Map keys to type                |
| `Exclude<T, U>`  | Exclude types from union        |
| `Extract<T, U>`  | Extract types from union        |
| `NonNullable<T>` | Exclude `null` & `undefined`    |
| `ReturnType<T>`  | Get return type of function     |
| `Parameters<T>`  | Get parameters type of function |

---

### **Examples of Utility Types**

```ts
interface User { id: number; name: string; email?: string; }

type PartialUser = Partial<User>;   // { id?: number; name?: string; email?: string; }
type RequiredUser = Required<User>; // { id: number; name: string; email: string; }
type ReadonlyUser = Readonly<User>; // Cannot modify properties
type NameEmail = Pick<User, "name" | "email">; // { name: string; email?: string }
type WithoutEmail = Omit<User, "email">;      // { id: number; name: string }

type Roles = "admin" | "user" | "guest";
type NonAdmin = Exclude<Roles, "admin">; // "user" | "guest"
type AdminOrUser = Extract<Roles, "admin" | "user">; // "admin" | "user"
```

---

## **3️⃣ Real-World Production Examples (10+)**

1. **Generic API Response Wrapper**

```ts
interface ApiResponse<T> { data: T; error?: string; }
```

2. **Generic Redux State Slice**

```ts
interface StateSlice<T> { items: T[]; loading: boolean; }
```

3. **Form Validation**

```ts
type FormErrors<T> = { [K in keyof T]?: string };
const userErrors: FormErrors<{ name: string; email: string }> = { name: "Required" };
```

4. **Polymorphic Components in React**

```ts
interface ButtonProps<T extends React.ElementType> { as?: T; children?: React.ReactNode; }
```

5. **Database Repository Class**

```ts
class Repository<T> { private items: T[] = []; add(item: T) { this.items.push(item); } }
```

6. **Utility Mapper Function**

```ts
type Mapper<T, U> = (item: T) => U;
const mapUserName: Mapper<{ name: string }, string> = user => user.name;
```

7. **Event System with Typed Payloads**

```ts
interface EventMap { click: { x: number; y: number }; keypress: { key: string } }
type EventHandler<K extends keyof EventMap> = (event: EventMap[K]) => void;
```

8. **Advanced DTO Transformation**

```ts
type Update<T> = Partial<T> & { updatedAt: Date };
```

9. **Config Management**

```ts
type Config<T> = { [K in keyof T]: T[K] | (() => T[K]) };
```

10. **API Parameter Helper**

```ts
type Params<T> = { [K in keyof T]: string | number };
```

11. **Logging Function Wrapper**

```ts
type Func = (...args: any[]) => any;
type WrappedFunc<F extends Func> = (...args: Parameters<F>) => ReturnType<F>;
```

12. **Dynamic Feature Flags**

```ts
type FeatureFlags<T extends string> = Record<T, boolean>;
const flags: FeatureFlags<"darkMode" | "betaUser"> = { darkMode: true, betaUser: false };
```

---

### ✅ **Summary**

* Generics allow **type-safe reusable code**.
* Advanced utility types reduce **boilerplate** and **increase flexibility**.
* Combined, they are crucial for **real-world apps**: Redux, React components, APIs, DTOs, configs, and feature flags.
* Key interview topics: `Partial`, `Required`, `Readonly`, `Pick`, `Omit`, `Record`, `Exclude`, `Extract`, `ReturnType`, `Parameters`.


