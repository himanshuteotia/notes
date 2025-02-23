Here is a **comprehensive list of JavaScript Math functions** with examples and explanations:

---

## 🧮 **1. Math Constants**

| **Property**      | **Description**                      | **Value**              |
|-------------------|--------------------------------------|------------------------|
| `Math.PI`         | Ratio of a circle’s circumference to its diameter | `3.141592653589793` |
| `Math.E`          | Euler’s number                       | `2.718281828459045`    |
| `Math.LN2`        | Natural logarithm of 2               | `0.6931471805599453`   |
| `Math.LN10`       | Natural logarithm of 10              | `2.302585092994046`    |
| `Math.LOG2E`      | Base 2 logarithm of E                | `1.4426950408889634`   |
| `Math.LOG10E`     | Base 10 logarithm of E               | `0.4342944819032518`   |
| `Math.SQRT1_2`    | Square root of 1/2                   | `0.7071067811865476`   |
| `Math.SQRT2`      | Square root of 2                     | `1.4142135623730951`   |

---

## 🔢 **2. Basic Math Functions**

| **Method**        | **Description**                          | **Example**               | **Output**  |
|-------------------|------------------------------------------|---------------------------|-------------|
| `Math.abs(x)`     | Returns absolute value                   | `Math.abs(-5);`           | `5`         |
| `Math.round(x)`   | Rounds to nearest integer                 | `Math.round(4.7);`        | `5`         |
| `Math.ceil(x)`    | Rounds up to nearest integer              | `Math.ceil(4.2);`         | `5`         |
| `Math.floor(x)`   | Rounds down to nearest integer            | `Math.floor(4.9);`        | `4`         |
| `Math.trunc(x)`   | Removes fractional part                   | `Math.trunc(4.9);`        | `4`         |
| `Math.sign(x)`    | Returns sign of number (-1, 0, 1)         | `Math.sign(-5);`          | `-1`        |
| `Math.clz32(x)`   | Counts leading zeroes in 32-bit integer   | `Math.clz32(1);`          | `31`        |

---

## 📏 **3. Advanced Math Functions**

| **Method**          | **Description**                       | **Example**                | **Output**  |
|---------------------|---------------------------------------|----------------------------|-------------|
| `Math.pow(x, y)`    | Returns `x` to the power of `y`       | `Math.pow(2, 3);`          | `8`         |
| `Math.sqrt(x)`      | Returns square root                   | `Math.sqrt(16);`           | `4`         |
| `Math.cbrt(x)`      | Returns cube root                     | `Math.cbrt(27);`           | `3`         |
| `Math.hypot(...x)`  | Square root of sum of squares         | `Math.hypot(3, 4);`        | `5`         |
| `Math.expm1(x)`     | Returns `e^x - 1`                      | `Math.expm1(1);`           | `1.718...`  |
| `Math.log1p(x)`     | Returns `ln(1 + x)`                   | `Math.log1p(1);`           | `0.693...`  |
| `Math.log(x)`       | Natural logarithm (base e)             | `Math.log(10);`            | `2.302...`  |
| `Math.log10(x)`     | Base 10 logarithm                      | `Math.log10(100);`         | `2`         |
| `Math.log2(x)`      | Base 2 logarithm                       | `Math.log2(8);`            | `3`         |

---

## 🎲 **4. Random Numbers and Rounding**

| **Method**        | **Description**                         | **Example**               | **Output**  |
|-------------------|-----------------------------------------|---------------------------|-------------|
| `Math.random()`   | Returns a random number (0 to <1)       | `Math.random();`          | `0.234...`  |
| `Math.floor()`    | Used with random for integers           | `Math.floor(Math.random() * 10);` | Random 0-9 |
| `Math.min(...x)`  | Returns smallest number in list         | `Math.min(1, 2, 3);`      | `1`         |
| `Math.max(...x)`  | Returns largest number in list          | `Math.max(1, 2, 3);`      | `3`         |

#### ✅ **Example: Random Integer Between Two Numbers**
```javascript
function getRandomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}
console.log(getRandomInt(1, 100)); // Random number between 1 and 100
```

---

## 📐 **5. Trigonometry Functions**

| **Method**        | **Description**                          | **Example**               | **Output**  |
|-------------------|------------------------------------------|---------------------------|-------------|
| `Math.sin(x)`    | Sine of angle (in radians)               | `Math.sin(Math.PI / 2);`  | `1`         |
| `Math.cos(x)`    | Cosine of angle (in radians)             | `Math.cos(0);`            | `1`         |
| `Math.tan(x)`    | Tangent of angle (in radians)            | `Math.tan(Math.PI / 4);`  | `1`         |
| `Math.asin(x)`   | Inverse sine (returns angle in radians)  | `Math.asin(1);`           | `1.570...`  |
| `Math.acos(x)`   | Inverse cosine                           | `Math.acos(1);`           | `0`         |
| `Math.atan(x)`   | Inverse tangent                          | `Math.atan(1);`           | `0.785...`  |
| `Math.atan2(y, x)`| Returns angle from x-axis to point (x,y) | `Math.atan2(1, 1);`       | `0.785...`  |

#### ✅ **Convert Degrees to Radians and Vice Versa**
```javascript
const degreesToRadians = degrees => degrees * (Math.PI / 180);
const radiansToDegrees = radians => radians * (180 / Math.PI);

console.log(degreesToRadians(180)); // 3.1415...
console.log(radiansToDegrees(Math.PI)); // 180
```

---

## 📊 **6. Bitwise and Integer Functions**

| **Method**        | **Description**                          | **Example**               | **Output**  |
|-------------------|------------------------------------------|---------------------------|-------------|
| `Math.clz32(x)`   | Count leading zeros in 32-bit integer    | `Math.clz32(1);`          | `31`        |
| `Math.imul(x, y)` | 32-bit integer multiplication            | `Math.imul(2, 4);`        | `8`         |
| `Math.fround(x)`  | Rounds to nearest 32-bit float           | `Math.fround(1.337);`     | `1.3370000123977661` |
| `Math.trunc(x)`   | Removes decimal part                     | `Math.trunc(4.9);`        | `4`         |

---

## 💡 **7. Practical Examples**

#### ✅ **1. Calculate Hypotenuse Using Pythagoras Theorem**
```javascript
let a = 3, b = 4;
let hypotenuse = Math.hypot(a, b);
console.log(hypotenuse); // 5
```

---

#### ✅ **2. Round to Specific Decimal Places**
```javascript
function roundToDecimal(num, decimals) {
  return Math.round(num * Math.pow(10, decimals)) / Math.pow(10, decimals);
}

console.log(roundToDecimal(3.14159, 2)); // 3.14
```

---

#### ✅ **3. Generate Random Hex Color**
```javascript
let color = '#' + Math.floor(Math.random()*16777215).toString(16);
console.log(color); // #3e2f1b (random color)
```

---

#### ✅ **4. Check if a Number is Prime**
```javascript
function isPrime(num) {
  if (num <= 1) return false;
  for (let i = 2; i <= Math.sqrt(num); i++) {
    if (num % i === 0) return false;
  }
  return true;
}

console.log(isPrime(7)); // true
```

---

## 🚀 **8. Summary of Math Methods in JavaScript**

| **Category**       | **Functions**                                                                                       |
|--------------------|----------------------------------------------------------------------------------------------------|
| **Constants**      | `PI`, `E`, `LN2`, `LN10`, `LOG2E`, `LOG10E`, `SQRT1_2`, `SQRT2`                                     |
| **Basic Math**     | `abs()`, `round()`, `ceil()`, `floor()`, `trunc()`, `sign()`, `clz32()`                            |
| **Advanced Math**  | `pow()`, `sqrt()`, `cbrt()`, `hypot()`, `log()`, `log10()`, `log2()`, `expm1()`, `log1p()`         |
| **Random & Ranges**| `random()`, `min()`, `max()`, `floor(random())` for random integers                               |
| **Trigonometry**   | `sin()`, `cos()`, `tan()`, `asin()`, `acos()`, `atan()`, `atan2()`                                 |
| **Bitwise & Int**  | `imul()`, `fround()`, `trunc()`                                                                    |

---

With these **JavaScript Math methods**, you can handle any numerical computation, from basic arithmetic to complex trigonometry and random number generation. 🚀