### 🧩 **Decorators in JavaScript — Complete Guide**

**Decorators** in JavaScript are a **special kind of declaration** that can be attached to classes, methods, properties, or parameters to **modify their behavior**. They provide a way to **add metadata**, **enhance functionalities**, and implement cross-cutting concerns like **logging**, **caching**, and **validation**.

> 💡 **Note:** As of now, **Decorators** are a **Stage 3 ECMAScript proposal** and are **natively supported** in **TypeScript** and frameworks like **Angular**. To use decorators in JavaScript, you typically need tools like **Babel** or **TypeScript** with the right configuration.

---

## 📊 **1. What Are Decorators?**

A **decorator** is essentially a **function** that can **modify** or **enhance** the behavior of a **class**, **method**, **property**, or **parameter**.

### ✅ **Syntax:**
```javascript
@decorator
class MyClass {}
```

Or for methods:
```javascript
class MyClass {
  @decorator
  myMethod() {}
}
```

---

## ⚡ **2. Enabling Decorators in JavaScript (Babel/TypeScript)**

### ✅ **For TypeScript:**
Add this to your `tsconfig.json`:
```json
{
  "experimentalDecorators": true,
  "emitDecoratorMetadata": true
}
```

### ✅ **For Babel:**
Install the decorator plugin:
```bash
npm install @babel/plugin-proposal-decorators
```
And add this to `.babelrc`:
```json
{
  "plugins": [["@babel/plugin-proposal-decorators", { "legacy": true }]]
}
```

---

## 📋 **3. Types of Decorators in JavaScript**

| **Decorator Type**       | **Applied To**               | **Purpose**                             |
|--------------------------|------------------------------|-----------------------------------------|
| **Class Decorators**      | Classes                      | Modify or extend class behavior        |
| **Method Decorators**     | Class methods                | Modify method behavior                 |
| **Property Decorators**   | Class properties             | Modify or observe property behaviors   |
| **Accessor Decorators**   | Getters/Setters              | Control accessors                      |
| **Parameter Decorators**  | Method parameters            | Add metadata to parameters             |

---

## 🏆 **4. Class Decorators**

### ✅ **Example — Logging Class Creation:**
```javascript
function Logger(constructor) {
  console.log(`Creating instance of ${constructor.name}`);
}

@Logger
class MyClass {}

const obj = new MyClass(); // Logs: Creating instance of MyClass
```

---

## 📡 **5. Method Decorators**

### ✅ **Example — Logging Method Calls:**
```javascript
function logMethod(target, propertyKey, descriptor) {
  const originalMethod = descriptor.value;

  descriptor.value = function (...args) {
    console.log(`Calling ${propertyKey} with`, args);
    return originalMethod.apply(this, args);
  };

  return descriptor;
}

class Calculator {
  @logMethod
  add(a, b) {
    return a + b;
  }
}

const calc = new Calculator();
calc.add(5, 3); // Logs: Calling add with [5, 3]
```

---

## 📁 **6. Property Decorators**

### ✅ **Example — Making a Property Read-Only:**
```javascript
function readOnly(target, propertyKey) {
  Object.defineProperty(target, propertyKey, {
    writable: false
  });
}

class User {
  @readOnly
  id = 123;
}

const user = new User();
user.id = 456; // Error: Cannot assign to read-only property
```

---

## 📊 **7. Accessor Decorators (Getters/Setters)**

### ✅ **Example — Logging Access to Getters:**
```javascript
function logAccessor(target, propertyKey, descriptor) {
  const originalGet = descriptor.get;

  descriptor.get = function () {
    console.log(`Accessing ${propertyKey}`);
    return originalGet.call(this);
  };

  return descriptor;
}

class Product {
  constructor(price) {
    this._price = price;
  }

  @logAccessor
  get price() {
    return this._price;
  }
}

const p = new Product(100);
console.log(p.price); // Logs: Accessing price
```

---

## 📡 **8. Parameter Decorators**

### ✅ **Example — Validating Method Parameters:**
```javascript
function logParameter(target, propertyKey, parameterIndex) {
  console.log(`Parameter at index ${parameterIndex} in ${propertyKey}`);
}

class Service {
  greet(@logParameter name) {
    console.log(`Hello, ${name}`);
  }
}

const s = new Service();
s.greet("Alice"); // Logs: Parameter at index 0 in greet
```

---

## 🧮 **9. Real-World Use Cases of Decorators**

| **Use Case**           | **Decorator Example**                |
|------------------------|---------------------------------------|
| **Logging**            | `@logMethod` for methods             |
| **Authentication**     | `@Authorized` to restrict access     |
| **Data Validation**     | `@ValidateInput` for forms           |
| **Caching**            | `@CacheResult` for heavy functions   |
| **Dependency Injection**| `@Inject` in frameworks like Angular |
| **API Documentation**   | `@ApiProperty` for Swagger           |

---

## ⚡ **10. Combining Multiple Decorators**

You can stack decorators, and they’ll be executed **bottom-up**.

```javascript
function first() {
  return function (target) {
    console.log("First decorator");
  };
}

function second() {
  return function (target) {
    console.log("Second decorator");
  };
}

@first()
@second()
class Demo {}
```

**💡 Output:**
```
Second decorator
First decorator
```

---

## ⚖️ **11. Pros & Cons of Using Decorators**

| ✅ **Advantages**                   | ⚠️ **Disadvantages**               |
|-------------------------------------|------------------------------------|
| Cleaner, DRY code                   | Not yet fully standardized in JS   |
| Separation of concerns              | Requires transpilers (e.g., Babel) |
| Enhances reusability and scalability| Overuse can make code hard to debug|
| Useful in frameworks like Angular   | Steeper learning curve             |

---

## 💥 **12. Decorators in Frameworks**

| **Framework**    | **How Decorators Are Used**                  |
|------------------|----------------------------------------------|
| **Angular**      | `@Component`, `@Injectable`, `@NgModule`    |
| **NestJS**       | `@Controller`, `@Get`, `@Post`              |
| **TypeORM**      | `@Entity`, `@Column`, `@PrimaryGeneratedColumn` |
| **MobX**         | `@observable`, `@action`                    |

---

## 📋 **13. Best Practices for Using Decorators**

1. ✅ **Use them for cross-cutting concerns** (logging, validation, etc.).
2. ⚖️ **Avoid overusing** them — can lead to hard-to-maintain code.
3. 🔄 **Combine multiple decorators** carefully to maintain readability.
4. 🔐 **Use decorators in security layers** (e.g., role-based access control).
5. 📊 **Document custom decorators** clearly to avoid misuse.

---

💥 **Pro Tip:**  
In interviews, emphasize how decorators can improve **modularity** and **reusability** in large-scale applications.