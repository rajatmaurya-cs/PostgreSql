# Prisma Querying Data (Very Important)

---

## 1. `where`

**Use:** Filters records based on one or more conditions.

### Prisma Query

```ts
const users = await prisma.user.findMany({
  where: {
    role: "USER",
  },
});
```

---

## 2. Filtering

**Use:** Retrieves only the records that match specific conditions.

### Prisma Query

```ts
const users = await prisma.user.findMany({
  where: {
    isActive: true,
    age: {
      gte: 18,
    },
  },
});
```

---

## 3. `AND`

**Use:** Returns records only if **all conditions** are true.

### Prisma Query

```ts
const users = await prisma.user.findMany({
  where: {
    AND: [
      { role: "USER" },
      { isActive: true },
    ],
  },
});
```

---

## 4. `OR`

**Use:** Returns records if **at least one condition** is true.

### Prisma Query

```ts
const users = await prisma.user.findMany({
  where: {
    OR: [
      { role: "ADMIN" },
      { role: "MODERATOR" },
    ],
  },
});
```

---

## 5. `NOT`

**Use:** Excludes records that match a condition.

### Prisma Query

```ts
const users = await prisma.user.findMany({
  where: {
    NOT: {
      role: "ADMIN",
    },
  },
});
```

---

## 6. `contains`

**Use:** Finds records where a field contains specific text.

### Prisma Query

```ts
const users = await prisma.user.findMany({
  where: {
    name: {
      contains: "pra",
    },
  },
});
```

---

## 7. `startsWith`

**Use:** Finds records whose value starts with specific text.

### Prisma Query

```ts
const users = await prisma.user.findMany({
  where: {
    name: {
      startsWith: "Pra",
    },
  },
});
```

---

## 8. `endsWith`

**Use:** Finds records whose value ends with specific text.

### Prisma Query

```ts
const users = await prisma.user.findMany({
  where: {
    email: {
      endsWith: "@gmail.com",
    },
  },
});
```

---

## 9. `equals`

**Use:** Finds records whose value exactly matches.

### Prisma Query

```ts
const users = await prisma.user.findMany({
  where: {
    role: {
      equals: "USER",
    },
  },
});
```

---

## 10. `in`

**Use:** Finds records whose value exists in a given list.

### Prisma Query

```ts
const users = await prisma.user.findMany({
  where: {
    role: {
      in: ["ADMIN", "USER"],
    },
  },
});
```

---

## 11. `notIn`

**Use:** Excludes records whose value exists in a given list.

### Prisma Query

```ts
const users = await prisma.user.findMany({
  where: {
    role: {
      notIn: ["ADMIN", "MODERATOR"],
    },
  },
});
```

---

## 12. `gt`

**Use:** Returns records where the value is **greater than**.

### Prisma Query

```ts
const users = await prisma.user.findMany({
  where: {
    age: {
      gt: 18,
    },
  },
});
```

---

## 13. `gte`

**Use:** Returns records where the value is **greater than or equal to**.

### Prisma Query

```ts
const users = await prisma.user.findMany({
  where: {
    age: {
      gte: 18,
    },
  },
});
```

---

## 14. `lt`

**Use:** Returns records where the value is **less than**.

### Prisma Query

```ts
const users = await prisma.user.findMany({
  where: {
    age: {
      lt: 60,
    },
  },
});
```

---

## 15. `lte`

**Use:** Returns records where the value is **less than or equal to**.

### Prisma Query

```ts
const users = await prisma.user.findMany({
  where: {
    age: {
      lte: 60,
    },
  },
});
```

---

## 16. `orderBy`

**Use:** Sorts records in ascending or descending order.

### Prisma Query

### Ascending Order

```ts
const users = await prisma.user.findMany({
  orderBy: {
    age: "asc",
  },
});
```

### Descending Order

```ts
const users = await prisma.user.findMany({
  orderBy: {
    createdAt: "desc",
  },
});
```

---

## 17. `distinct`

**Use:** Returns only unique values and removes duplicates.

### Prisma Query

```ts
const users = await prisma.user.findMany({
  distinct: ["role"],
});
```