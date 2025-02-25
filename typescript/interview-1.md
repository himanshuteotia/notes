### 🧩 **TypeScript Interview Questions & Answers — Beginner to Advanced**

If you're preparing for a **TypeScript interview**, here’s a comprehensive list of questions covering **core concepts**, **advanced features**, and **real-world scenarios**.

---

## 📊 **1. TypeScript Basics**

### ❓ **Q1. What is TypeScript? How is it different from JavaScript?**  
**💡 Answer:**  
- **TypeScript** is a **superset of JavaScript** that adds **static typing** and other modern features.
- It is **compiled** into plain JavaScript using the TypeScript compiler (`tsc`).
  
**Key Differences:**
| **JavaScript**          | **TypeScript**               |
|-------------------------|------------------------------|
| Dynamically typed        | Statically typed            |
| Runtime type checking    | Compile-time type checking  |
| No interfaces or types   | Supports interfaces, types  |

---

### ❓ **Q2. What are the benefits of using TypeScript?**  
**💡 Answer:**
- **Static Typing** → Catches errors at compile time.
- **Better IDE Support** → Autocomplete, refactoring.
- **Object-Oriented Features** → Interfaces, Generics, Enums.
- **Backward Compatibility** → Compiles to ES5/ES6.

---

### ❓ **Q3. How do you install TypeScript?**  
```bash
npm install -g typescript
```
To compile:
```bash
tsc filename.ts
```

---

## 📋 **2. Types & Interfaces**

### ❓ **Q4. What are the basic data types in TypeScript?**  
**💡 Answer:**
- `number`, `string`, `boolean`
- `null`, `undefined`
- `any`, `unknown`
- `void`, `never`
- `tuple`, `enum`, `object`

**Example:**
```typescript
let age: number = 30;
let name: string = "Alice";
let isActive: boolean = true;
```

---

### ❓ **Q5. What is the difference between `any` and `unknown`?**  
| **`any`**                       | **`unknown`**                        |
|----------------------------------|---------------------------------------|
| Disables type checking           | Requires type checks before use      |
| Allows any operation             | Safer, but requires explicit casting |

```typescript
let x: any = 5;
x.toUpperCase(); // No error

let y: unknown = 5;
y.toUpperCase(); // Error: Object is of type 'unknown'
```

---

### ❓ **Q6. When to use `interface` vs. `type`?**  
**💡 Answer:**

| **Interface**                      | **Type Alias**                      |
|------------------------------------|-------------------------------------|
| Best for defining **object shapes** | Best for unions & advanced types   |
| Supports **extends** & **implements** | Cannot be reopened or extended    |
| Preferred for OOP patterns         | More flexible for complex types    |

```typescript
interface User {
  name: string;
  age: number;
}

type Admin = User & { role: string };
```

---

## ⚡ **3. Advanced TypeScript Concepts**

### ❓ **Q7. What are Generics in TypeScript?**  
**💡 Answer:**  
Generics allow creating reusable components that work with multiple data types.

```typescript
function identity<T>(arg: T): T {
  return arg;
}

const num = identity<number>(5); // num is type number
```

---

### ❓ **Q8. Explain Union and Intersection Types.**  
- **Union (`|`)** → A value can be one of many types.
  ```typescript
  let id: number | string;
  id = 101;
  id = "E101";
  ```

- **Intersection (`&`)** → Combines multiple types.
  ```typescript
  type A = { name: string };
  type B = { age: number };
  type Person = A & B; // { name: string, age: number }
  ```

---

### ❓ **Q9. What is Type Inference?**  
**💡 Answer:**  
TypeScript can automatically **infer types** based on assigned values.

```typescript
let count = 10; // inferred as number
```

---

### ❓ **Q10. What is `never` type in TypeScript?**  
**💡 Answer:**  
Represents values that **never occur**.  
Used for:
- **Functions that throw errors**
- **Infinite loops**

```typescript
function error(message: string): never {
  throw new Error(message);
}
```

---

## 📡 **4. Object-Oriented Programming in TypeScript**

### ❓ **Q11. How do you define a class in TypeScript?**  
```typescript
class Animal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  speak(): void {
    console.log(`${this.name} makes a sound.`);
  }
}

const dog = new Animal("Dog");
dog.speak();
```

---

### ❓ **Q12. What is the difference between `public`, `private`, and `protected`?**  
| **Modifier**    | **Access Scope**                         |
|-----------------|------------------------------------------|
| `public`        | Accessible from anywhere (default)       |
| `private`       | Accessible only within the class         |
| `protected`     | Accessible within the class & subclasses |

```typescript
class User {
  public name: string;
  private password: string;
  protected role: string;

  constructor(name: string, password: string, role: string) {
    this.name = name;
    this.password = password;
    this.role = role;
  }
}
```

---

### ❓ **Q13. What are Decorators in TypeScript?**  
**💡 Answer:**  
Decorators are **special annotations** used to modify classes, methods, or properties.

```typescript
function log(target: any, propertyKey: string) {
  console.log(`Property ${propertyKey} has been accessed`);
}

class Car {
  @log
  brand: string = "Toyota";
}
```

---

## 📋 **5. TypeScript Utility Types**

| **Utility Type** | **Description**                                  |
|------------------|--------------------------------------------------|
| `Partial<T>`     | Makes all properties optional                   |
| `Required<T>`    | Makes all properties required                   |
| `Readonly<T>`    | Makes properties read-only                      |
| `Pick<T, K>`     | Selects specific keys from a type               |
| `Omit<T, K>`     | Excludes specific keys from a type              |
| `Record<K, T>`   | Creates an object type with keys K and values T  |

**Example:**
```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

type UserPreview = Pick<User, "name" | "email">;
```

---

## ⚡ **6. Real-World TypeScript Questions**

### ❓ **Q14. How do you handle asynchronous operations in TypeScript?**  
**💡 Answer:**  
Use **`async/await`** with proper type annotations.

```typescript
async function fetchData(): Promise<string> {
  return "Data loaded";
}

fetchData().then(data => console.log(data));
```

---

### ❓ **Q15. How does TypeScript support JSX in React?**  
**💡 Answer:**
- Use `.tsx` files for React components.
- Define **prop types** using `interface`.

```typescript
interface ButtonProps {
  label: string;
}

const Button: React.FC<ButtonProps> = ({ label }) => (
  <button>{label}</button>
);
```

---

### ❓ **Q16. How do you handle third-party libraries without TypeScript definitions?**  
**💡 Answer:**  
Use **DefinitelyTyped** or create a custom type declaration.

```bash
npm install @types/library-name
```

Or create a `.d.ts` file:
```typescript
declare module 'library-name';
```

---

## 🏆 **7. Bonus — Advanced Questions**

1. **What is Type Narrowing?**  
2. **Explain the difference between `interface` merging and `type` alias.**  
3. **How does TypeScript handle `this` in arrow functions?**  
4. **What are Mapped Types in TypeScript?**  
5. **Explain the use of `keyof` and `in` operators.**  

---

💥 **Pro Tip:**  
- In interviews, **highlight TypeScript’s role in improving code maintainability and reducing bugs.**  
- Discuss real-world examples where **type safety** helped avoid runtime issues.