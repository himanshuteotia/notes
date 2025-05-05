**Mapped Types** in TypeScript — one of the most powerful features that helps you transform types based on existing ones.

---

## 🧠 What Are Mapped Types?

**Mapped types** allow you to create new types by transforming properties of an existing type using a loop-like syntax.

You can think of it like:

> “For every key in an object, do something with its type.”

---

## ✅ Basic Syntax

```ts
type NewType = {
  [K in Keys]: Type;
}
```

* `K` is a type variable (usually key name)
* `Keys` is usually a union of strings like `"a" | "b" | "c"`
* `Type` can refer to the original property type or be transformed

---

### 🔹 Example 1: Convert all properties to `string`

```ts
type Person = {
  name: string;
  age: number;
};

type StringifiedPerson = {
  [K in keyof Person]: string;
};

// Result:
// type StringifiedPerson = {
//   name: string;
//   age: string;
// }
```

---

### 🔹 Example 2: Make all fields optional

```ts
type Partial<T> = {
  [K in keyof T]?: T[K];
};

type Person = {
  name: string;
  age: number;
};

type OptionalPerson = Partial<Person>;
```

This is **exactly** how TypeScript internally implements `Partial<T>`.

---

### 🔹 Example 3: Make all fields readonly

```ts
type Readonly<T> = {
  readonly [K in keyof T]: T[K];
};
```

---

### 🔹 Example 4: Rename keys with a string template

```ts
type PrefixProps<T> = {
  [K in keyof T as `prop_${string & K}`]: T[K];
};

type Person = {
  name: string;
  age: number;
};

type Prefixed = PrefixProps<Person>;
// Result:
// {
//   prop_name: string;
//   prop_age: number;
// }
```

---

## 🔧 `keyof`, `in`, `as`, and modifiers

| Keyword     | Meaning                           |
| ----------- | --------------------------------- |
| `keyof T`   | Gets union of property names of T |
| `in`        | Loops through each key in a union |
| `as`        | Renames keys                      |
| `readonly`  | Makes properties readonly         |
| `?`         | Makes properties optional         |
| `-readonly` | Removes readonly                  |
| `-?`        | Removes optional                  |

---

## ✨ Real World Example: API Response Cleaner

Let's say you want to convert all properties in a response to be optional:

```ts
type ApiResponse<T> = {
  success: boolean;
  data: T;
  error?: string;
};

type MakeDataOptional<T> = {
  [K in keyof T]: T[K] extends object ? Partial<T[K]> : T[K];
};

type Response = ApiResponse<{ name: string; age: number }>;

type Transformed = MakeDataOptional<Response['data']>;
// Result:
// {
//   name?: string;
//   age?: number;
// }
```

---

## 📦 Built-in Mapped Types in TypeScript

* `Partial<T>` – makes all properties optional
* `Required<T>` – makes all properties required
* `Readonly<T>` – makes all properties readonly
* `Pick<T, K>` – picks a subset of properties
* `Record<K, T>` – creates object with keys `K` and value type `T`

---

## ✅ Summary

Mapped types are:

* Used to dynamically create or transform types
* Useful in utilities like `Partial`, `Readonly`, `Record`
* Can rename keys using `as` + template literals
* Work with modifiers like `readonly`, `?`, `-?`

Let’s go deeper with **more practical examples** of mapped types in TypeScript, from basic to advanced.

---

## 🔁 1. Convert All Properties to Boolean

```ts
type Flags<T> = {
  [K in keyof T]: boolean;
};

type Features = {
  darkMode: () => void;
  newUI: () => void;
};

type FeatureFlags = Flags<Features>;
// Result:
// {
//   darkMode: boolean;
//   newUI: boolean;
// }
```

---

## 🔄 2. Remove Optional Modifier (`-?`)

```ts
type User = {
  name?: string;
  age?: number;
};

type RequiredUser = {
  [K in keyof User]-?: User[K];
};

// Result:
// {
//   name: string;
//   age: number;
// }
```

---

## 🛠️ 3. Remove `readonly` (`-readonly`)

```ts
type ReadonlyUser = {
  readonly name: string;
  readonly age: number;
};

type WritableUser = {
  -readonly [K in keyof ReadonlyUser]: ReadonlyUser[K];
};

// Result:
// {
//   name: string;
//   age: number;
// }
```

---

## 🏷️ 4. Rename Keys Using `as`

```ts
type User = {
  id: number;
  name: string;
};

type SnakeCase<T> = {
  [K in keyof T as `db_${string & K}`]: T[K];
};

type DBUser = SnakeCase<User>;
// Result:
// {
//   db_id: number;
//   db_name: string;
// }
```

---

## 🔍 5. Filter Properties by Type (e.g., string-only fields)

```ts
type FilterByType<T, U> = {
  [K in keyof T as T[K] extends U ? K : never]: T[K];
};

type Mixed = {
  id: number;
  name: string;
  active: boolean;
};

type OnlyStrings = FilterByType<Mixed, string>;
// Result:
// {
//   name: string;
// }
```

---

## 🔐 6. Make Certain Keys Optional and Rest Required

```ts
type MakeOptional<T, K extends keyof T> = {
  [P in K]?: T[P];
} & {
  [P in Exclude<keyof T, K>]-?: T[P];
};

type User = {
  name: string;
  age: number;
  email: string;
};

type SemiOptionalUser = MakeOptional<User, 'email'>;
// Result:
// {
//   email?: string;
//   name: string;
//   age: number;
// }
```

---

## ⚠️ 7. Custom Error Map Type

Let’s say you want to create a structure that maps each field to an error message (or undefined if no error):

```ts
type FieldErrors<T> = {
  [K in keyof T]: string | undefined;
};

type FormValues = {
  username: string;
  password: string;
};

type FormErrors = FieldErrors<FormValues>;
// Result:
// {
//   username: string | undefined;
//   password: string | undefined;
// }
```

---

## 🧪 8. Deep Partial (Recursive)

```ts
type DeepPartial<T> = {
  [K in keyof T]?: T[K] extends object ? DeepPartial<T[K]> : T[K];
};

type Nested = {
  a: string;
  b: {
    c: number;
    d: {
      e: boolean;
    };
  };
};

type PartialNested = DeepPartial<Nested>;
// Allows:
// {
//   a?: string;
//   b?: {
//     c?: number;
//     d?: {
//       e?: boolean;
//     }
//   }
// }
```

Here's another set of **advanced and real-world mapped type examples** to help you master the concept.

---

## 🔢 9. `ToNumberFields` – Convert all values to `number`

```ts
type ToNumberFields<T> = {
  [K in keyof T]: number;
};

type Sizes = {
  width: string;
  height: string;
  depth: string;
};

type NumericSizes = ToNumberFields<Sizes>;
// Result:
// {
//   width: number;
//   height: number;
//   depth: number;
// }
```

---

## 📝 10. `Nullable<T>` – Allow all fields to be `null`

```ts
type Nullable<T> = {
  [K in keyof T]: T[K] | null;
};

type User = {
  id: number;
  name: string;
};

type NullableUser = Nullable<User>;
// {
//   id: number | null;
//   name: string | null;
// }
```

---

## 📦 11. `Record` with Dynamic Keys

```ts
type LangCodes = 'en' | 'hi' | 'fr';

type Translations<T> = {
  [K in LangCodes]: T;
};

type Message = {
  title: string;
  subtitle: string;
};

type AllLangMessages = Translations<Message>;
```

---

## 🧠 12. Get Function Return Types from Object

```ts
type FunctionMap = {
  getUser: () => string;
  getAge: () => number;
};

type ReturnTypes<T> = {
  [K in keyof T]: ReturnType<T[K]>;
};

type Result = ReturnTypes<FunctionMap>;
// {
//   getUser: string;
//   getAge: number;
// }
```

---

## 🧪 13. Deep Readonly

```ts
type DeepReadonly<T> = {
  readonly [K in keyof T]: T[K] extends object ? DeepReadonly<T[K]> : T[K];
};

type Nested = {
  name: string;
  details: {
    age: number;
    address: {
      city: string;
    };
  };
};

type ReadonlyNested = DeepReadonly<Nested>;
```

---

## 🧪 14. Deep Required

```ts
type DeepRequired<T> = {
  [K in keyof T]-?: T[K] extends object ? DeepRequired<T[K]> : T[K];
};
```

---

## 🏷️ 15. Rename keys by uppercasing

```ts
type UppercaseKeys<T> = {
  [K in keyof T as Uppercase<string & K>]: T[K];
};

type User = {
  name: string;
  email: string;
};

type UpperUser = UppercaseKeys<User>;
// {
//   NAME: string;
//   EMAIL: string;
// }
```

---

## 🧹 16. Remove fields by type

```ts
type RemoveByType<T, V> = {
  [K in keyof T as T[K] extends V ? never : K]: T[K];
};

type Data = {
  name: string;
  age: number;
  isAdmin: boolean;
};

type WithoutBooleans = RemoveByType<Data, boolean>;
// {
//   name: string;
//   age: number;
// }
```

---

## 📄 17. Required & Readonly combo

```ts
type Strict<T> = {
  readonly [K in keyof T]-?: T[K];
};

type Settings = {
  theme?: string;
  darkMode?: boolean;
};

type StrictSettings = Strict<Settings>;
// {
//   readonly theme: string;
//   readonly darkMode: boolean;
// }
```

---

## 🔐 18. Encrypt fields ending with `_password`

```ts
type EncryptPasswords<T> = {
  [K in keyof T]: K extends `${string}_password` ? 'ENCRYPTED' : T[K];
};

type Credentials = {
  user_password: string;
  admin_password: string;
  username: string;
};

type EncryptedCreds = EncryptPasswords<Credentials>;
// {
//   user_password: 'ENCRYPTED';
//   admin_password: 'ENCRYPTED';
//   username: string;
// }
```
Here’s another batch of **mapped type examples** — pushing deeper into real-world patterns, constraints, and creative use cases:

---

## 🚦 19. **Make all string keys uppercase, skip symbol keys**

```ts
type UpperStringKeys<T> = {
  [K in keyof T as K extends string ? Uppercase<K> : never]: T[K];
};

type Data = {
  id: number;
  name: string;
  [Symbol.iterator]: () => void;
};

type UpperKeys = UpperStringKeys<Data>;
// {
//   ID: number;
//   NAME: string;
// }
```

---

## 📑 20. **Map form field types based on input**

```ts
type FormFieldTypes = 'text' | 'number' | 'checkbox';

type FieldMap<T extends Record<string, FormFieldTypes>> = {
  [K in keyof T]: T[K] extends 'text'
    ? string
    : T[K] extends 'number'
    ? number
    : T[K] extends 'checkbox'
    ? boolean
    : never;
};

type FormSchema = {
  username: 'text';
  age: 'number';
  acceptTerms: 'checkbox';
};

type MappedForm = FieldMap<FormSchema>;
// {
//   username: string;
//   age: number;
//   acceptTerms: boolean;
// }
```

---

## ⚡ 21. **Extract Async Return Types**

```ts
type AsyncFunctions = {
  fetchUser: () => Promise<{ name: string }>;
  fetchPosts: () => Promise<string[]>;
};

type UnwrapPromise<T> = T extends Promise<infer R> ? R : T;

type ExtractReturn<T> = {
  [K in keyof T]: UnwrapPromise<ReturnType<T[K]>>;
};

type Extracted = ExtractReturn<AsyncFunctions>;
// {
//   fetchUser: { name: string };
//   fetchPosts: string[];
// }
```

---

## 📌 22. **Set all object values to a constant**

```ts
type Constify<T, V> = {
  [K in keyof T]: V;
};

type Config = {
  port: number;
  env: string;
};

type AllTrue = Constify<Config, true>;
// {
//   port: true;
//   env: true;
// }
```

---

## 🏷️ 23. **Prefix and Suffix All Keys**

```ts
type AddPrefixSuffix<T, Prefix extends string, Suffix extends string> = {
  [K in keyof T as `${Prefix}${string & K}${Suffix}`]: T[K];
};

type EventHandlers = {
  click: () => void;
  hover: () => void;
};

type WrappedHandlers = AddPrefixSuffix<EventHandlers, 'on', 'Handler'>;
// {
//   onclickHandler: () => void;
//   onhoverHandler: () => void;
// }
```

---

## ⛔ 24. **Make fields required if their name starts with `is_`**

```ts
type SmartRequired<T> = {
  [K in keyof T]-?: K extends `is_${string}` ? T[K] : T[K] | undefined;
};

type Flags = {
  is_enabled?: boolean;
  name?: string;
};

type Modified = SmartRequired<Flags>;
// {
//   is_enabled: boolean;
//   name?: string;
// }
```

---

## 🔁 25. **Tuple to Object**

```ts
type TupleToObj<T extends readonly string[]> = {
  [K in T[number]]: K;
};

const tuple = ['red', 'green', 'blue'] as const;

type Colors = TupleToObj<typeof tuple>;
// {
//   red: 'red';
//   green: 'green';
//   blue: 'blue';
// }
```

---

## 📂 26. **Flatten Nested Object Keys**

```ts
type Flatten<T, Prefix extends string = ''> = {
  [K in keyof T & string as `${Prefix}${K}`]: T[K] extends object
    ? never
    : T[K];
};

type Nested = {
  name: string;
  details: {
    age: number;
    city: string;
  };
};

// This gives only first-level flatten, advanced recursion needs more complex logic
type Flat = Flatten<Nested, 'user_'>;
// {
//   user_name: string;
// }
```

---

## 🎛️ 27. **Make keys based on value type**

```ts
type KeyByType<T, Type> = {
  [K in keyof T as T[K] extends Type ? K : never]: T[K];
};

type Mixed = {
  a: string;
  b: number;
  c: string;
};

type OnlyStrings = KeyByType<Mixed, string>;
// {
//   a: string;
//   c: string;
// }
```


