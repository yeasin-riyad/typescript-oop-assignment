# ⚠️ `any` vs `unknown` in TypeScript — Type Safety Hole vs Safe Handling

TypeScript আমাদের type safety দেয়—কিন্তু `any` ব্যবহার করলে সেই নিরাপত্তা ভেঙে যেতে পারে। এজন্যই `any`-কে বলা হয় **“type safety hole”**। অন্যদিকে `unknown` একই কাজের জন্য অনেক বেশি safe।

চল পরিষ্কারভাবে বুঝি 👇

---

# 🧠 `any` কী? (Danger Zone 🚨)

```ts
let value: any;

value = 10;
value = "hello";
value.toUpperCase(); // ❌ runtime error হতে পারে
```

👉 `any` ব্যবহার করলে:

* TypeScript checking বন্ধ হয়ে যায় ❌
* তুমি যেকোনো property/method call করতে পারো ❌
* compile time এ error ধরবে না ❌

---

## 🔥 কেন `any` = Type Safety Hole?

👉 কারণ এটা compiler-কে বলে:

> “আমি যা করছি, সেটা তুমি check কোরো না”

Example:

```ts
let user: any = { name: "Sangam" };

user.age.toFixed(); // ❌ runtime crash
```

👉 TypeScript কিছুই বলবে না 😱
👉 কিন্তু runtime এ crash করবে

---

# 🛡️ `unknown` কী? (Safe Alternative)

```ts
let value: unknown;

value = 10;
value = "hello";

// value.toUpperCase(); ❌ Error
```

👉 এখানে TypeScript বলবে:

> “আগে type check করো, তারপর use করো”

---

# 🔍 Type Narrowing (Key Concept)

👉 `unknown` ব্যবহার করতে হলে **type narrow করতে হয়**

---

## 🔹 Example

```ts
let value: unknown = "hello";

if (typeof value === "string") {
  console.log(value.toUpperCase()); // ✅ safe
}
```

👉 এখানে:

* আগে check করলাম
* তারপর safely ব্যবহার করলাম

---

## 🔹 Another Example (Function)

```ts
function printLength(value: unknown) {
  if (typeof value === "string") {
    console.log(value.length);
  } else {
    console.log("Not a string");
  }
}
```

---

# 🔥 Narrowing Techniques

TypeScript-এ narrowing করার কিছু common উপায়:

### ✔️ `typeof`

```ts
if (typeof x === "number")
```

---

### ✔️ `instanceof`

```ts
if (value instanceof Date)
```

---

### ✔️ Custom Type Guard

```ts
function isUser(obj: any): obj is { name: string } {
  return obj && typeof obj.name === "string";
}
```

---

# ⚔️ `any` vs `unknown`

| Feature           | `any`      | `unknown`        |
| ----------------- | ---------- | ---------------- |
| Type safety       | ❌ নেই      | ✅ আছে            |
| Direct access     | ✅ allowed  | ❌ not allowed    |
| Compiler checking | ❌ off      | ✅ on             |
| Use case          | quick hack | safe handling 🔥 |

---

# 🚀 Real-world Use Case

👉 API response handling:

```ts
async function fetchData(): Promise<unknown> {
  const res = await fetch("...");
  return res.json();
}
```

👉 তারপর:

```ts
const data = await fetchData();

if (typeof data === "object" && data !== null) {
  console.log("Safe to use");
}
```

---

# 🔥 Best Practice

❌ Avoid:

```ts
let data: any;
```

✅ Prefer:

```ts
let data: unknown;
```

---

# 🔚 Final Insight

👉 সহজভাবে:

> `any` = “blind trust” 😅
> `unknown` = “verify then trust” 🔥

---

# 🚀 Conclusion

* `any` ব্যবহার করলে TypeScript-এর power নষ্ট হয়
* `unknown` ব্যবহার করলে safe coding নিশ্চিত হয়
* type narrowing = safe access এর key

👉 Professional TypeScript developer হতে হলে:

> `any` কম ব্যবহার করো, `unknown` + narrowing ব্যবহার করো 💪
