# Prisma CRUD Methods

---

## 1. `create()`

**Use:** Creates **one new record** in the database.

### Prisma Query

```ts
const user = await prisma.user.create({
  data: {
    name: "Prashant",
    email: "prashant@gmail.com",
  },
});
```

---

## 2. `createMany()`

**Use:** Creates **multiple records** in a single database query.

### Prisma Query

```ts
await prisma.user.createMany({
  data: [
    {
      name: "Prashant",
      email: "p@gmail.com",
    },
    {
      name: "Rahul",
      email: "r@gmail.com",
    },
  ],
});
```

---

## 3. `findUnique()`

**Use:** Finds **exactly one record** using a **unique field** like `id` or `email`.

### Prisma Query

```ts
const user = await prisma.user.findUnique({
  where: {
    email: "p@gmail.com",
  },
});
```

> ✅ Works only with fields marked as `@unique` or `@id`.

---

## 4. `findFirst()`

**Use:** Returns the **first record** that matches a condition.

### Prisma Query

```ts
const user = await prisma.user.findFirst({
  where: {
    name: "Prashant",
  },
});
```

---

## 5. `findMany()`

**Use:** Returns **all matching records**.

### Prisma Query

```ts
const users = await prisma.user.findMany({
  where: {
    role: "USER",
  },
});
```

---

## 6. `update()`

**Use:** Updates **one record** using a unique field.

### Prisma Query

```ts
await prisma.user.update({
  where: {
    id: 1,
  },
  data: {
    name: "Kevin",
  },
});
```

---

## 7. `updateMany()`

**Use:** Updates **all records** that match the condition.

### Prisma Query

```ts
await prisma.user.updateMany({
  where: {
    role: "USER",
  },
  data: {
    isActive: true,
  },
});
```

---

## 8. `delete()`

**Use:** Deletes **one record** using a unique field.

### Prisma Query

```ts
await prisma.user.delete({
  where: {
    id: 1,
  },
});
```

---

## 9. `deleteMany()`

**Use:** Deletes **all matching records**.

### Prisma Query

```ts
await prisma.user.deleteMany({
  where: {
    isActive: false,
  },
});
```

---

## 10. `upsert()`

**Use:** Updates the record **if it exists**, otherwise **creates a new one**.

### Prisma Query

```ts
await prisma.user.upsert({
  where: {
    email: "p@gmail.com",
  },
  update: {
    name: "Kevin",
  },
  create: {
    name: "Kevin",
    email: "p@gmail.com",
  },
});
```

### Equivalent Logic

```ts
const user = await prisma.user.findUnique({
  where: {
    email: "p@gmail.com",
  },
});

if (user) {
  await prisma.user.update({
    where: {
      email: "p@gmail.com",
    },
    data: {
      name: "Kevin",
    },
  });
} else {
  await prisma.user.create({
    data: {
      name: "Kevin",
      email: "p@gmail.com",
    },
  });
}
```