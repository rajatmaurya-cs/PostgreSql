
## Models

model User {
  id        String   @id @default(cuid())
  name      String
  email     String   @unique
  age       Int
  city      String
  isActive  Boolean
  createdAt DateTime @default(now())

  blogs     Blog[]
}

model Blog {
  id        String   @id @default(cuid())
  title     String
  content   String
  published Boolean

  authorId  String
  author    User @relation(fields: [authorId], references: [id])
}


## DataBase Tables

| id | name     | email                                     | age | city   | isActive |
| -- | -------- | ----------------------------------------- | --- | ------ | -------- |
| u1 | Prashant | [p@gmail.com](mailto:p@gmail.com)         | 20  | Delhi  | ✅        |
| u2 | Rahul    | [r@gmail.com](mailto:r@gmail.com)         | 24  | Mumbai | ✅        |
| u3 | Aman     | [a@gmail.com](mailto:a@gmail.com)         | 18  | Delhi  | ❌        |
| u4 | Karan    | [k@gmail.com](mailto:k@gmail.com)         | 28  | Pune   | ✅        |
| u5 | Priya    | [priya@gmail.com](mailto:priya@gmail.com) | 22  | Delhi  | ❌        |


## DataBase Tables

| id | title            | published | authorId |
| -- | ---------------- | --------- | -------- |
| b1 | Prisma Guide     | ✅         | u1       |
| b2 | Next.js Mastery  | ❌         | u1       |
| b3 | Docker Basics    | ✅         | u2       |
| b4 | PostgreSQL Guide | ✅         | u4       |


---------------------------------------------------------------------------------------------------------------------------

## 🍎 Where


## Querry


```js
await prisma.user.findMany({
  select:{
    name:true,
    email:true
  }
})
```

## output

```json
[
  {
    "name":"Prashant",
    "email":"p@gmail.com"
  },
  {
    "name":"Rahul",
    "email":"r@gmail.com"
  }
]

```

--------------------------------------------------------------------------------------------------------------------------

## 🍎 Include

## Querry

```ts
await prisma.user.findMany({
  include:{
    blogs:true
  }
})
```

## Output

```json
{
  "id":"u1",
  "name":"Prashant",
  "email":"p@gmail.com",
  "age":20,
  "city":"Delhi",
  "isActive":true,
  "blogs":[
    {
      "title":"Prisma Guide"
    },
    {
      "title":"Next.js Mastery"
    }
  ]
}
```

---------------------------------------------------------------------------------------------------------------------------

##  🍎 Nested Select


## Querry

```js
await prisma.user.findMany({
  select:{
    name:true,
    blogs:{
      select:{
        title:true
      }
    }
  }
})

```

## Output


```json 
[
  {
    "name":"Prashant",
    "blogs":[
      {
        "title":"Prisma Guide"
      },
      {
        "title":"Next.js Mastery"
      }
    ]
  }
]

```
---------------------------------------------------------------------------------------------------------------------------


##  🍎 Nested Include

## Querry

```js
await prisma.user.findMany({
  include:{
    blogs:{
      include:{
        comments:true
      }
    }
  }
})
```


## Output

```json 
{
  "name":"Prashant",
  "blogs":[
    {
      "title":"Prisma Guide",
      "comments":[
        {
          "text":"Great article!"
        },
        {
          "text":"Very helpful."
        }
      ]
    }
  ]
}

```
---------------------------------------------------------------------------------------------------------------------------

## Real-World Query (Blog Dashboard)

Suppose you need:

Only active users
Age ≥ 18
City is Delhi or Mumbai
Name contains "pr"
Show only user name
Show only blog titles
Sort by newest users

## Querry

```js
await prisma.user.findMany({
  where:{
    isActive:true,
    age:{
      gte:18
    },
    city:{
      in:["Delhi","Mumbai"]
    },
    name:{
      contains:"pr",
      mode:"insensitive"
    }
  },

  select:{
    name:true,
    blogs:{
      select:{
        title:true
      }
    }
  },

  orderBy:{
    createdAt:"desc"
  }
})


```


## Output

```json 
[
  {
    "name":"Prashant",
    "blogs":[
      {
        "title":"Prisma Guide"
      },
      {
        "title":"Next.js Mastery"
      }
    ]
  }
]


```