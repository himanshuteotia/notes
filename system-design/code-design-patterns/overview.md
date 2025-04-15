Here’s a breakdown of the most important **Software Design Patterns** you should know — especially for backend (Node.js, TypeScript) and frontend (React):

---

## 🧠 **Creational Design Patterns**  
> Focus: Object creation logic

### 1. **Singleton**
Ensures a class has only **one instance** globally.

```js
class DB {
  constructor() {
    if (DB.instance) return DB.instance;
    DB.instance = this;
  }
}

const db1 = new DB();
const db2 = new DB(); // same instance
```

---

### 2. **Factory**
Creates objects **without exposing the creation logic**.

```js
class User {
  constructor(name) { this.name = name; }
}

class UserFactory {
  static create(name) {
    return new User(name);
  }
}
```

---

### 3. **Builder**
Separates the construction of a complex object step by step.

```js
class BurgerBuilder {
  constructor() { this.burger = {}; }
  addCheese() { this.burger.cheese = true; return this; }
  addLettuce() { this.burger.lettuce = true; return this; }
  build() { return this.burger; }
}

const burger = new BurgerBuilder().addCheese().addLettuce().build();
```

---

## ⚙️ **Structural Design Patterns**  
> Focus: Composition of objects and classes

### 4. **Adapter**
Allows **incompatible interfaces** to work together.

```js
class OldAPI {
  getData() { return 'old'; }
}

class NewAPIAdapter {
  constructor(oldApi) { this.oldApi = oldApi; }
  fetch() { return this.oldApi.getData(); }
}
```

---

### 5. **Decorator**
Adds functionality to an object **without changing its structure**.

```js
function logDecorator(fn) {
  return (...args) => {
    console.log('Called with', args);
    return fn(...args);
  };
}

const add = (a, b) => a + b;
const loggedAdd = logDecorator(add);
loggedAdd(1, 2); // Logs args
```

---

## 🤖 **Behavioral Design Patterns**  
> Focus: Object communication

### 6. **Observer**
One-to-many dependency. When one object changes, others are notified.

```js
class Subject {
  constructor() { this.observers = []; }
  subscribe(fn) { this.observers.push(fn); }
  notify(data) { this.observers.forEach(fn => fn(data)); }
}
```

---

### 7. **Strategy**
Encapsulate algorithms and make them interchangeable.

```js
class PaymentStrategy {
  pay(amount) {}
}
class PayPal extends PaymentStrategy {
  pay(amount) { console.log('Paying via PayPal'); }
}
class CreditCard extends PaymentStrategy {
  pay(amount) { console.log('Paying via Credit Card'); }
}

function checkout(strategy, amount) {
  strategy.pay(amount);
}
checkout(new PayPal(), 100);
```

---

### 8. **Command**
Encapsulates a request as an object.

```js
class Light {
  turnOn() { console.log('Light ON'); }
}

class TurnOnCommand {
  constructor(light) { this.light = light; }
  execute() { this.light.turnOn(); }
}

const light = new Light();
const command = new TurnOnCommand(light);
command.execute();
```

---

## 📦 Common Real-Life Patterns in Node.js

| Pattern          | Used For                            |
|------------------|-------------------------------------|
| MVC              | API structure                       |
| Repository       | Database logic separation           |
| Middleware       | Express/Koa request pipelines       |
| Strategy         | Pluggable logic (auth, pricing etc) |
| Observer/Event   | EventEmitter, pub/sub systems       |
| Factory/DI       | Service creation in DI frameworks   |
