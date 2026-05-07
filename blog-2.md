# ✂️ Pick vs Omit in TypeScript — DRY কোড ও Real API Example

TypeScript-এ `Pick` এবং `Omit` utility types আমাদেরকে বড় interface থেকে ছোট, প্রয়োজনমতো **“slice”** তৈরি করতে সাহায্য করে।
এতে করে code থাকে **DRY (Don't Repeat Yourself)**, maintainable এবং scalable 🔥

---

# 🧠 Problem: Code Duplication

ধরো তোমার একটা main interface আছে:

```ts
interface User {
  id: string;
  name: string;
  email: string;
  password: string;
  role: "admin" | "user";
}
```

---

## ❌ Without Pick/Omit

```ts
// Public user response
interface PublicUser {
  id: string;
  name: string;
  email: string;
}

// Login payload
interface LoginUser {
  email: string;
  password: string;
}
```

👉 Problem:

* duplication ❌
* future change হলে multiple জায়গায় update করতে হবে ❌

---

# ✂️ Solution 1: `Pick<T, K>`

👉 specific field “pick” করে

```ts
type PublicUser = Pick<User, "id" | "name" | "email">;
```

👉 Result:

```ts
{
  id: string;
  name: string;
  email: string;
}
```

---

# 🚫 Solution 2: `Omit<T, K>`

👉 unwanted field remove করে

```ts
type SafeUser = Omit<User, "password">;
```

👉 Result:

```ts
{
  id: string;
  name: string;
  email: string;
  role: "admin" | "user";
}
```

---

# 🔥 Real API Example (Very Important)

## 🧾 Scenario: User API

### 🔹 1. Database Model

```ts
interface User {
  id: string;
  name: string;
  email: string;
  password: string;
  role: "admin" | "user";
}
```

---

## 🔹 2. Register API (Client → Server)

👉 client password পাঠাবে

```ts
type RegisterPayload = Pick<User, "name" | "email" | "password">;
```

---

## 🔹 3. Login API

```ts
type LoginPayload = Pick<User, "email" | "password">;
```

---

## 🔹 4. API Response (Server → Client)

👉 password কখনো পাঠানো যাবে না ❌

```ts
type UserResponse = Omit<User, "password">;
```

---

## 🔹 5. Admin View

👉 সব field লাগতে পারে

```ts
type AdminUserView = User;
```

---

# 🧠 Code Usage

```ts
function registerUser(data: RegisterPayload) {
  // safe payload
}

function loginUser(data: LoginPayload) {
  // only email + password
}

function getUser(): UserResponse {
  return {
    id: "1",
    name: "Sangam",
    email: "test@mail.com",
    role: "user"
  };
}
```

---

# 🚀 Why This is Powerful

### ✔️ DRY Code

একবার `User` define → সব জায়গায় reuse

---

### ✔️ Single Source of Truth

👉 structure change হলে এক জায়গায় change করলেই সব update

---

### ✔️ Security

👉 `Omit` দিয়ে sensitive data hide করা যায়

---

### ✔️ Maintainability

👉 বড় project manage করা সহজ

---

# ⚔️ Pick vs Omit

| Feature     | Pick            | Omit                  |
| ----------- | --------------- | --------------------- |
| কাজ         | select fields   | remove fields         |
| use case    | specific subset | sensitive data hide   |
| readability | explicit        | cleaner for exclusion |

---

# 🔥 Best Practice

👉 use `Pick` যখন:

* few fields দরকার

👉 use `Omit` যখন:

* few fields বাদ দিতে চাও

---

# 🧠 Pro Tip

তুমি combine করতে পারো:

```ts
type UserSummary = Pick<Omit<User, "password">, "id" | "name">;
```

---

# 🔚 Final Insight

👉 সহজভাবে:

> `Pick` = “এইগুলো দরকার”
> `Omit` = “এইগুলো লাগবে না”

---

# 🚀 Conclusion

TypeScript-এর `Pick` এবং `Omit` utility types:

* duplication কমায়
* code DRY রাখে
* API design secure করে
* বড় project scalable করে


