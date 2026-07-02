# Phase 6: Pagination

## Models

```prisma
model User {
  id        String   @id @default(cuid())
  name      String
  email     String   @unique
  age       Int
  city      String
  isActive  Boolean
  createdAt DateTime @default(now())

  blogs Blog[]
}

model Blog {
  id        String @id @default(cuid())
  title     String
  content   String
  published Boolean

  authorId String
  author   User @relation(fields: [authorId], references: [id])
}
```

---

# Blog Table

| id | title | createdAt |
|----|----------------------|-----------|
| b1 | Prisma Guide | 10:00 |
| b2 | Next.js | 10:05 |
| b3 | Docker | 10:10 |
| b4 | PostgreSQL | 10:15 |
| b5 | Redis | 10:20 |
| b6 | Kafka | 10:25 |
| b7 | Nginx | 10:30 |
| b8 | React | 10:35 |
| b9 | TypeScript | 10:40 |
| b10| Tailwind | 10:45 |

---

# What is Pagination?

Instead of returning **all records**, return only a small portion.

Without Pagination

```
1
2
3
4
5
6
7
8
9
10
```

With Pagination

Page 1

```
1
2
3
```

Page 2

```
4
5
6
```

Page 3

```
7
8
9
```

Much faster.

---

# 1. take ⭐⭐⭐⭐⭐

## Meaning

Return only **N records**.

Example

```ts
await prisma.blog.findMany({
  take: 3
})
```

Result

| title |
|--------|
| Prisma Guide |
| Next.js |
| Docker |

Only 3 records are returned.

---

Latest 3 blogs

```ts
await prisma.blog.findMany({
  orderBy: {
    createdAt: "desc"
  },
  take: 3
})
```

Result

```
Tailwind
TypeScript
React
```

---


# 2. skip ⭐⭐⭐⭐⭐

## Meaning

Skip the first N records.

Example

```ts
await prisma.blog.findMany({
  skip: 3
})
```

Result

```
PostgreSQL
Redis
Kafka
Nginx
React
TypeScript
Tailwind
```

The first three blogs are skipped.

---

Use with take

```ts
await prisma.blog.findMany({
  skip: 3,
  take: 3
})
```

Result

```
PostgreSQL
Redis
Kafka
```

Skipped

```
Prisma
Next.js
Docker
```

Then took

```
PostgreSQL
Redis
Kafka
```
---

# Problem with skip

Imagine

Instagram has

```
30 million posts
```

User opens

```
Page 1,000,000
```

Prisma must skip

```
9,999,990 rows
```

Very slow.

This is why large applications don't rely only on `skip`.

---

# 3. Cursor Pagination ⭐⭐⭐⭐⭐

Most important for interviews.

Uses a unique field (usually `id`).

Instead of

```
Skip first 100000 rows
```

Prisma says

```
Start AFTER this record.
```

Much faster.

---

Example

Current blogs

```
b1 Prisma
b2 Next.js
b3 Docker
b4 PostgreSQL
b5 Redis
b6 Kafka
```

First page

```ts
await prisma.blog.findMany({
  take: 3,
  orderBy: {
    id: "asc"
  }
})
```

Result

```
b1
b2
b3
```

Last id

```
b3
```

Next page

```ts
await prisma.blog.findMany({
  cursor: {
    id: "b3"
  },
  skip: 1,
  take: 3,
  orderBy: {
    id: "asc"
  }
})
```

Result

```
b4
b5
b6
```

Why `skip:1`?

Because the cursor points **to** `b3`.

Without `skip:1`

```
b3
b4
b5
```

You would get `b3` again.

With `skip:1`

```
b4
b5
b6
```

Perfect.

---

# Infinite Scroll Example

Instagram

Open App

```
take = 10
```

User scrolls

Last Post ID

```
post_10
```

Next Query

```ts
await prisma.post.findMany({
  cursor: {
    id: "post_10"
  },
  skip: 1,
  take: 10,
  orderBy: {
    createdAt: "desc"
  }
})
```

Next

```
post11
post12
...
post20
```

Scroll again

Cursor

```
post20
```

Next

```
post21
post22
...
```

This continues forever.

---

# Offset Pagination vs Cursor Pagination

| Offset Pagination | Cursor Pagination |
|-------------------|-------------------|
| Uses `skip` | Uses `cursor` |
| Good for page numbers | Good for infinite scroll |
| Can become slow on huge datasets | Very fast |
| Easy to implement | Slightly more complex |
| Used in admin panels | Used by Instagram, Twitter, YouTube, Facebook |

---

# Which one should I use?

### Admin Dashboard

```
Users

Page 1

Page 2

Page 3
```

Use

```
skip + take
```

---

### Blog Website

```
Page 1

Page 2

Page 3
```

Use

```
skip + take
```

---

### Instagram

```
Scroll...

Scroll...

Scroll...
```

Use

```
cursor + take
```

---

### Twitter / LinkedIn / Facebook

Use

```
cursor + take
```

---

# Cheat Sheet

| Keyword | Meaning | SQL Equivalent |
|----------|---------|----------------|
| `take` | Return first N rows | `LIMIT` |
| `skip` | Ignore first N rows | `OFFSET` |
| `cursor` | Start after a specific record | Cursor-based pagination |

---

# Interview Tip ⭐

There are **two main types of pagination**:

1. **Offset Pagination** (`skip` + `take`)
   - Best for numbered pages.
   - Example: page 1, page 2, page 3.

2. **Cursor Pagination** (`cursor` + `take`)
   - Best for infinite scrolling.
   - More efficient on large datasets because it starts after a known record instead of skipping thousands or millions of rows.

As a full-stack developer, you'll use **`skip` + `take`** for dashboards and admin panels, and **`cursor` + `take`** for feeds and social-media-style applications.