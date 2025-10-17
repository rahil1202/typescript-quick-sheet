
# **Topic 5: TypeScript Decorators & Metadata**

Decorators in TypeScript are **special annotations** that can be attached to classes, methods, properties, accessors, or parameters. They allow you to **modify behavior, inject dependencies, or add metadata** at runtime.

---

## **1️⃣ Core Concepts**

### 1. Enabling Decorators

Decorators are **experimental**, so enable them in `tsconfig.json`:

```json
{
  "experimentalDecorators": true,
  "emitDecoratorMetadata": true
}
```

---

### 2. Class Decorators

A class decorator is a function that **receives the constructor** of the class.

```ts
function Logger(constructor: Function) {
  console.log(`Class created: ${constructor.name}`);
}

@Logger
class User {
  constructor(public name: string) {}
}

const u = new User("Rahil"); // Logs: Class created: User
```

---

### 3. Property Decorators

A property decorator is a function that receives **target** and **property name**.

```ts
function Readonly(target: any, key: string) {
  Object.defineProperty(target, key, { writable: false });
}

class Employee {
  @Readonly
  name = "Rahil";
}

const e = new Employee();
// e.name = "New Name"; // ❌ Error: cannot assign
```

---

### 4. Method Decorators

A method decorator receives **target, method name, and descriptor**. It can modify behavior.

```ts
function Log(target: any, key: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;
  descriptor.value = function (...args: any[]) {
    console.log(`Calling ${key} with args: ${args}`);
    return original.apply(this, args);
  };
}

class Calculator {
  @Log
  add(a: number, b: number) { return a + b; }
}

const calc = new Calculator();
calc.add(2, 3); // Logs: Calling add with args: 2,3
```

---

### 5. Parameter Decorators

A parameter decorator receives **target, method name, and parameter index**.

```ts
function Required(target: any, key: string, index: number) {
  console.log(`Parameter at index ${index} in ${key} is required`);
}

class Form {
  submit(@Required name: string, @Required email: string) {}
}
```

---

### 6. Accessor Decorators

Used on getters/setters.

```ts
function LogAccessor(target: any, key: string, descriptor: PropertyDescriptor) {
  const original = descriptor.get;
  descriptor.get = function () {
    console.log(`Getting ${key}`);
    return original?.apply(this);
  };
}

class Person {
  private _age = 25;

  @LogAccessor
  get age() { return this._age; }
}

const p = new Person();
console.log(p.age); // Logs: Getting age
```

---

### 7. Metadata Reflection

TypeScript can emit **type metadata**, useful for **DI frameworks** like NestJS.

```ts
import "reflect-metadata";

function LogType(target: any, key: string) {
  const type = Reflect.getMetadata("design:type", target, key);
  console.log(`${key} type: ${type.name}`);
}

class User {
  @LogType
  name: string;
}
```

---

## **2️⃣ Real-World Production Use Cases**

1. **Dependency Injection (NestJS)** – use decorators to inject services into classes.
2. **ORM Mapping (TypeORM, Sequelize)** – decorators mark entity properties and relationships.
3. **Validation (class-validator)** – decorators for required fields, min/max lengths.
4. **Logging** – method decorators for automatic logging of calls.
5. **Authorization** – class/method decorators enforce role-based access control.
6. **Caching** – method decorators automatically cache return values.
7. **API Documentation (Swagger)** – decorators describe endpoints and parameters.
8. **Event Handling** – decorators bind methods to events dynamically.
9. **Metrics / Telemetry** – method decorators count usage, track performance.
10. **Serialization / Transformation** – property decorators control JSON mapping.
11. **Observable / Reactive Properties** – decorators create reactive getters/setters.
12. **Performance Timing** – method decorators measure execution time.

---

### ✅ Summary

* Decorators provide **runtime meta-programming** capabilities.
* Types: **class, property, method, accessor, parameter**.
* Commonly used in **frameworks (NestJS, TypeORM)**, **logging**, **validation**, **authorization**, and **metrics**.
* Combined with **metadata reflection**, decorators enable advanced patterns like **DI containers**.


