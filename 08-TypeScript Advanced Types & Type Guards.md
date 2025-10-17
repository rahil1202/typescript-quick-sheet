# **Topic 8: TypeScript Advanced Types & Type Guards**

Advanced types allow you to **create complex type relationships** in TypeScript. Type guards ensure that **runtime checks align with compile-time types**, making your code safer and predictable.

---

## **1️⃣ Core Concepts**

### 1. Union & Intersection Types

```ts
type Admin = { name: string; role: "admin" };
type User = { name: string; role: "user" };

type Person = Admin | User; // Union
type Employee = Admin & { department: string }; // Intersection
```

### 2. Literal Types

```ts
type Direction = "up" | "down" | "left" | "right";
let dir: Direction;
dir = "up";   // ✅
dir = "forward"; // ❌ Error
```

### 3. Type Aliases vs Interfaces

* Aliases can represent **primitive unions, intersections, tuples**
* Interfaces define **object shapes** and can be **extended**

```ts
type ID = string | number;
interface User { id: ID; name: string; }
```

### 4. Type Guards

#### `typeof` Guard

```ts
function printValue(value: string | number) {
  if (typeof value === "string") console.log(value.toUpperCase());
  else console.log(value.toFixed(2));
}
```

#### `instanceof` Guard

```ts
class Dog { bark() {} }
class Cat { meow() {} }

function speak(pet: Dog | Cat) {
  if (pet instanceof Dog) pet.bark();
  else pet.meow();
}
```

#### Custom Type Guards

```ts
interface Fish { swim(): void; }
interface Bird { fly(): void; }

function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}

function move(pet: Fish | Bird) {
  if (isFish(pet)) pet.swim();
  else pet.fly();
}
```

---

### 5. `keyof` & Indexed Access Types

```ts
interface User { id: number; name: string; email: string; }

type UserKeys = keyof User; // "id" | "name" | "email"
type NameType = User["name"]; // string
```

### 6. Conditional Types

```ts
type Check<T> = T extends string ? "Yes" : "No";

type A = Check<string>; // "Yes"
type B = Check<number>; // "No"
```

### 7. Mapped Types

```ts
type PartialUser = { [K in keyof User]?: User[K] };
type ReadonlyUser = { readonly [K in keyof User]: User[K] };
```

---

## **2️⃣ Real-World Production Examples (10+)**

1. **API Response with Union Types**

```ts
type ApiResponse<T> = { data: T } | { error: string };
```

2. **Redux Action Types**

```ts
type Action = { type: "ADD"; payload: number } | { type: "REMOVE"; payload: number };
```

3. **Feature Flags**

```ts
type Features = "beta" | "darkMode";
```

4. **Polymorphic Components**

```ts
interface ButtonProps<T extends React.ElementType> { as?: T; children?: React.ReactNode; }
```

5. **Event Handling**

```ts
interface ClickEvent { x: number; y: number; }
interface KeyEvent { key: string; }

type AppEvent = ClickEvent | KeyEvent;
```

6. **Validation Functions**

```ts
function isString(value: any): value is string { return typeof value === "string"; }
```

7. **Database Models**

```ts
type Entity<T> = T & { id: string; createdAt: Date };
```

8. **Config Options**

```ts
type ConfigOption<T> = T | (() => T);
```

9. **Dynamic Forms**

```ts
type FormField<T> = { value: T; required?: boolean; };
```

10. **Middleware Type Guards**

```ts
function isAuthorized(user: User | null): user is User { return !!user; }
```

11. **Error Handling**

```ts
type ErrorResponse = { message: string; code: number } | null;
```

12. **Generic Type Constraints**

```ts
function merge<T extends object, U extends object>(obj1: T, obj2: U) { return { ...obj1, ...obj2 }; }
```

---

### ✅ **Summary**

* Advanced types: **union, intersection, literal, mapped, conditional**.
* Type guards: **typeof, instanceof, custom**.
* Key for **safe runtime type checks** in production.
* Commonly used for **API responses, Redux actions, event handling, polymorphic components, validation, middleware, database models**.


