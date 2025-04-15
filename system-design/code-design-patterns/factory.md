Awesome! Let’s now dive into the **Factory Design Pattern**, which is super useful for creating objects when you want to **hide object creation logic** and **return different types** based on some condition.

---

## 🏭 What is Factory Pattern?

> Factory Pattern provides an **interface for creating objects** in a superclass, but **lets subclasses decide** which class to instantiate.

In short: You call a single method like `create()`, and it decides what object to return.

---

## 🧠 When to Use Factory Pattern?

- When the creation logic is **complex or based on conditions**
- To avoid using `new` keyword everywhere
- To centralize object creation logic
- When object creation involves setup (like config, DB, types)

---

## ✅ Example: Notification Factory

```js
class EmailService {
  send(message) {
    console.log(`Email sent: ${message}`);
  }
}

class SMSService {
  send(message) {
    console.log(`SMS sent: ${message}`);
  }
}

class NotificationFactory {
  static create(type) {
    if (type === 'email') {
      return new EmailService();
    } else if (type === 'sms') {
      return new SMSService();
    } else {
      throw new Error('Invalid notification type');
    }
  }
}
```

### Usage:
```js
const notification = NotificationFactory.create('email');
notification.send("Hello, user!"); // Email sent: Hello, user!
```

---

## 🧪 Real-life Use Case: Creating Payment Gateways

```js
class Razorpay {
  pay(amount) {
    console.log(`Paid ₹${amount} via Razorpay`);
  }
}

class Stripe {
  pay(amount) {
    console.log(`Paid ₹${amount} via Stripe`);
  }
}

class PaymentFactory {
  static create(gateway) {
    switch (gateway) {
      case 'razorpay': return new Razorpay();
      case 'stripe': return new Stripe();
      default: throw new Error("Unsupported payment gateway");
    }
  }
}

// Usage
const payment = PaymentFactory.create('razorpay');
payment.pay(499); // Paid ₹499 via Razorpay
```

---

## 🆚 Factory vs Constructor

| Aspect             | Factory Pattern               | Constructor (`new`)         |
|--------------------|-------------------------------|------------------------------|
| Logic control      | Centralized & flexible        | Tight to class               |
| Polymorphism       | Easy to implement             | Harder                       |
| Maintainability    | Easier to refactor or scale   | More coupling                |
| Example            | `Factory.create('type')`      | `new SomeClass()`            |
