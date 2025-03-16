### **Security Features in Node.js** 🛡️

Node.js provides several built-in security features and best practices that help in developing **secure applications**. Below are the key **security features** in Node.js:

---

## **1️⃣ Built-in Security Modules**
### **🔹 Crypto Module (Encryption & Hashing)**
Node.js provides a **`crypto`** module for implementing encryption, hashing, and HMAC.

✅ **Example: Hashing passwords securely**
```javascript
const crypto = require("crypto");

const hashPassword = (password) => {
  return crypto.createHash("sha256").update(password).digest("hex");
};

console.log(hashPassword("securepassword"));
```

✅ **Example: Encrypting data using AES-256**
```javascript
const algorithm = "aes-256-cbc";
const key = crypto.randomBytes(32);
const iv = crypto.randomBytes(16);

const encrypt = (text) => {
  const cipher = crypto.createCipheriv(algorithm, key, iv);
  let encrypted = cipher.update(text, "utf8", "hex");
  encrypted += cipher.final("hex");
  return { iv: iv.toString("hex"), encryptedData: encrypted };
};
```

---

### **🔹 TLS/SSL Security (HTTPS)**
Node.js supports **TLS (Transport Layer Security)** for encrypting network communications.

✅ **Example: Creating a secure HTTPS server**
```javascript
const https = require("https");
const fs = require("fs");

const options = {
  key: fs.readFileSync("server-key.pem"),
  cert: fs.readFileSync("server-cert.pem"),
};

https.createServer(options, (req, res) => {
  res.writeHead(200);
  res.end("Secure Server");
}).listen(443);
```

---

## **2️⃣ Authentication & Authorization**
### **🔹 Secure JWT Handling**
Node.js supports secure authentication using **JSON Web Tokens (JWTs)**.

✅ **Example: Generating a JWT securely**
```javascript
const jwt = require("jsonwebtoken");

const token = jwt.sign({ userId: 123 }, "your-secret-key", {
  expiresIn: "1h",
  algorithm: "HS256",
});
console.log(token);
```

---

### **🔹 Session Management & Secure Cookies**
✅ **Set `httpOnly`, `secure`, and `sameSite` flags** to prevent session hijacking.

✅ **Example: Secure session setup in Express.js**
```javascript
const session = require("express-session");

app.use(
  session({
    secret: "secureSecretKey",
    resave: false,
    saveUninitialized: true,
    cookie: { secure: true, httpOnly: true, sameSite: "strict" },
  })
);
```

---

## **3️⃣ Secure Input Handling**
### **🔹 Preventing SQL & NoSQL Injection**
Use **ORMs (TypeORM, Sequelize, Mongoose)** to prevent raw SQL injection.

✅ **Example: Using parameterized queries**
```javascript
await db.query("SELECT * FROM users WHERE email = ?", {
  replacements: ["user@example.com"],
  type: db.QueryTypes.SELECT,
});
```

---

### **🔹 Input Validation**
Use libraries like **express-validator** or **Ajv** for validating user input.

✅ **Example: Using express-validator**
```javascript
const { body, validationResult } = require("express-validator");

app.post(
  "/register",
  [body("email").isEmail(), body("password").isLength({ min: 8 })],
  (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) return res.status(400).json(errors.array());
    res.send("User registered!");
  }
);
```

---

## **4️⃣ Web Security Features**
### **🔹 Prevent Cross-Site Scripting (XSS)**
✅ Use **`xss-clean`** middleware to sanitize user inputs.

```javascript
const xss = require("xss-clean");
app.use(xss());
```

✅ Use **CSP (Content Security Policy) Headers** with `helmet`:
```javascript
const helmet = require("helmet");
app.use(helmet());
```

---

### **🔹 Prevent Cross-Site Request Forgery (CSRF)**
Use **`csurf`** middleware to generate CSRF tokens.

```javascript
const csrf = require("csurf");
app.use(csrf({ cookie: true }));
```

---

## **5️⃣ Network & API Security**
### **🔹 Rate Limiting & DDoS Protection**
✅ Use **express-rate-limit** to limit requests.

```javascript
const rateLimit = require("express-rate-limit");

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Limit each IP to 100 requests per window
});

app.use(limiter);
```

✅ Use **Cloudflare WAF** or **AWS WAF** for DDoS protection.

---

### **🔹 Secure Headers (Prevent Clickjacking & MIME Sniffing)**
Use **Helmet.js** to add security headers.

```javascript
const helmet = require("helmet");
app.use(helmet());
```

---

## **6️⃣ Secure File Handling**
### **🔹 Safe File Uploads**
✅ Use **Multer** for secure file uploads.  
✅ Limit **file size & extensions** to prevent attacks.

```javascript
const multer = require("multer");

const upload = multer({
  limits: { fileSize: 5 * 1024 * 1024 }, // Max 5MB
  fileFilter: (req, file, cb) => {
    if (!file.mimetype.startsWith("image/")) return cb(new Error("Invalid file type"));
    cb(null, true);
  },
});
```

---

## **7️⃣ Dependency Security**
### **🔹 Run Security Audits**
✅ Use **npm audit** and **Snyk** to detect vulnerabilities.

```sh
npm audit fix
npx snyk test
```

---

## **8️⃣ Logging & Monitoring**
### **🔹 Secure Logging**
✅ Use **Winston or Pino** for structured logging.

```javascript
const winston = require("winston");

const logger = winston.createLogger({
  level: "info",
  format: winston.format.json(),
  transports: [new winston.transports.File({ filename: "app.log" })],
});

logger.info("Server started securely");
```

✅ Monitor logs using **ELK Stack**, **Datadog**, or **Prometheus**.

---

## **9️⃣ Secure Deployment Practices**
### **🔹 Docker & Container Security**
✅ Use **non-root user** inside Docker.
```dockerfile
FROM node:18-alpine
RUN addgroup appgroup && adduser -S -G appgroup appuser
USER appuser
```

✅ Scan Docker images using:
```sh
docker scan my-app
```

✅ Enable **automatic security updates** for OS and Node.js.

---

## **🔟 Advanced Security Measures**
✅ **Enable AppArmor or SELinux** for process security.  
✅ **Use an API Gateway (Kong, Traefik, AWS API Gateway)** for secure API handling.  
✅ **Implement Zero Trust Security Architecture** for internal services.  

---

### **🚀 Final Node.js Security Features Summary**
| Feature | Security Mechanism |
|---------|--------------------|
| **Authentication** | JWT, OAuth, Secure Sessions |
| **Encryption** | AES-256, Hashing with bcrypt |
| **Input Validation** | express-validator, Ajv |
| **XSS Protection** | Helmet, xss-clean |
| **CSRF Protection** | csurf middleware |
| **Rate Limiting** | express-rate-limit |
| **HTTPS & TLS** | SSL Certificates, HSTS |
| **Logging** | Winston, Pino, ELK Stack |
| **Dependency Security** | npm audit, Snyk |
| **File Upload Security** | Multer, File Type Validation |
| **Docker Security** | Non-root users, Scanning |

---

Would you like a **detailed security guide in Markdown format** for reference? 🚀

### **Security Features in Node.js** 🛡️

Node.js provides several built-in security features and best practices that help in developing **secure applications**. Below are the key **security features** in Node.js:

---

## **1️⃣ Built-in Security Modules**
### **🔹 Crypto Module (Encryption & Hashing)**
Node.js provides a **`crypto`** module for implementing encryption, hashing, and HMAC.

✅ **Example: Hashing passwords securely**
```javascript
const crypto = require("crypto");

const hashPassword = (password) => {
  return crypto.createHash("sha256").update(password).digest("hex");
};

console.log(hashPassword("securepassword"));
```

✅ **Example: Encrypting data using AES-256**
```javascript
const algorithm = "aes-256-cbc";
const key = crypto.randomBytes(32);
const iv = crypto.randomBytes(16);

const encrypt = (text) => {
  const cipher = crypto.createCipheriv(algorithm, key, iv);
  let encrypted = cipher.update(text, "utf8", "hex");
  encrypted += cipher.final("hex");
  return { iv: iv.toString("hex"), encryptedData: encrypted };
};
```

---

### **🔹 TLS/SSL Security (HTTPS)**
Node.js supports **TLS (Transport Layer Security)** for encrypting network communications.

✅ **Example: Creating a secure HTTPS server**
```javascript
const https = require("https");
const fs = require("fs");

const options = {
  key: fs.readFileSync("server-key.pem"),
  cert: fs.readFileSync("server-cert.pem"),
};

https.createServer(options, (req, res) => {
  res.writeHead(200);
  res.end("Secure Server");
}).listen(443);
```

---

## **2️⃣ Authentication & Authorization**
### **🔹 Secure JWT Handling**
Node.js supports secure authentication using **JSON Web Tokens (JWTs)**.

✅ **Example: Generating a JWT securely**
```javascript
const jwt = require("jsonwebtoken");

const token = jwt.sign({ userId: 123 }, "your-secret-key", {
  expiresIn: "1h",
  algorithm: "HS256",
});
console.log(token);
```

---

### **🔹 Session Management & Secure Cookies**
✅ **Set `httpOnly`, `secure`, and `sameSite` flags** to prevent session hijacking.

✅ **Example: Secure session setup in Express.js**
```javascript
const session = require("express-session");

app.use(
  session({
    secret: "secureSecretKey",
    resave: false,
    saveUninitialized: true,
    cookie: { secure: true, httpOnly: true, sameSite: "strict" },
  })
);
```

---

## **3️⃣ Secure Input Handling**
### **🔹 Preventing SQL & NoSQL Injection**
Use **ORMs (TypeORM, Sequelize, Mongoose)** to prevent raw SQL injection.

✅ **Example: Using parameterized queries**
```javascript
await db.query("SELECT * FROM users WHERE email = ?", {
  replacements: ["user@example.com"],
  type: db.QueryTypes.SELECT,
});
```

---

### **🔹 Input Validation**
Use libraries like **express-validator** or **Ajv** for validating user input.

✅ **Example: Using express-validator**
```javascript
const { body, validationResult } = require("express-validator");

app.post(
  "/register",
  [body("email").isEmail(), body("password").isLength({ min: 8 })],
  (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) return res.status(400).json(errors.array());
    res.send("User registered!");
  }
);
```

---

## **4️⃣ Web Security Features**
### **🔹 Prevent Cross-Site Scripting (XSS)**
✅ Use **`xss-clean`** middleware to sanitize user inputs.

```javascript
const xss = require("xss-clean");
app.use(xss());
```

✅ Use **CSP (Content Security Policy) Headers** with `helmet`:
```javascript
const helmet = require("helmet");
app.use(helmet());
```

---

### **🔹 Prevent Cross-Site Request Forgery (CSRF)**
Use **`csurf`** middleware to generate CSRF tokens.

```javascript
const csrf = require("csurf");
app.use(csrf({ cookie: true }));
```

---

## **5️⃣ Network & API Security**
### **🔹 Rate Limiting & DDoS Protection**
✅ Use **express-rate-limit** to limit requests.

```javascript
const rateLimit = require("express-rate-limit");

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Limit each IP to 100 requests per window
});

app.use(limiter);
```

✅ Use **Cloudflare WAF** or **AWS WAF** for DDoS protection.

---

### **🔹 Secure Headers (Prevent Clickjacking & MIME Sniffing)**
Use **Helmet.js** to add security headers.

```javascript
const helmet = require("helmet");
app.use(helmet());
```

---

## **6️⃣ Secure File Handling**
### **🔹 Safe File Uploads**
✅ Use **Multer** for secure file uploads.  
✅ Limit **file size & extensions** to prevent attacks.

```javascript
const multer = require("multer");

const upload = multer({
  limits: { fileSize: 5 * 1024 * 1024 }, // Max 5MB
  fileFilter: (req, file, cb) => {
    if (!file.mimetype.startsWith("image/")) return cb(new Error("Invalid file type"));
    cb(null, true);
  },
});
```

---

## **7️⃣ Dependency Security**
### **🔹 Run Security Audits**
✅ Use **npm audit** and **Snyk** to detect vulnerabilities.

```sh
npm audit fix
npx snyk test
```

---

## **8️⃣ Logging & Monitoring**
### **🔹 Secure Logging**
✅ Use **Winston or Pino** for structured logging.

```javascript
const winston = require("winston");

const logger = winston.createLogger({
  level: "info",
  format: winston.format.json(),
  transports: [new winston.transports.File({ filename: "app.log" })],
});

logger.info("Server started securely");
```

✅ Monitor logs using **ELK Stack**, **Datadog**, or **Prometheus**.

---

## **9️⃣ Secure Deployment Practices**
### **🔹 Docker & Container Security**
✅ Use **non-root user** inside Docker.
```dockerfile
FROM node:18-alpine
RUN addgroup appgroup && adduser -S -G appgroup appuser
USER appuser
```

✅ Scan Docker images using:
```sh
docker scan my-app
```

✅ Enable **automatic security updates** for OS and Node.js.

---

## **🔟 Advanced Security Measures**
✅ **Enable AppArmor or SELinux** for process security.  
✅ **Use an API Gateway (Kong, Traefik, AWS API Gateway)** for secure API handling.  
✅ **Implement Zero Trust Security Architecture** for internal services.  

---

### **🚀 Final Node.js Security Features Summary**
| Feature | Security Mechanism |
|---------|--------------------|
| **Authentication** | JWT, OAuth, Secure Sessions |
| **Encryption** | AES-256, Hashing with bcrypt |
| **Input Validation** | express-validator, Ajv |
| **XSS Protection** | Helmet, xss-clean |
| **CSRF Protection** | csurf middleware |
| **Rate Limiting** | express-rate-limit |
| **HTTPS & TLS** | SSL Certificates, HSTS |
| **Logging** | Winston, Pino, ELK Stack |
| **Dependency Security** | npm audit, Snyk |
| **File Upload Security** | Multer, File Type Validation |
| **Docker Security** | Non-root users, Scanning |
