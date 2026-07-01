
## Models

```js 
model User {
  id    String @id @default(cuid())
  name  String

  blogs Blog[]
}

model Blog {
  id       String @id @default(cuid())
  title    String

  authorId String
  author   User @relation(fields: [authorId], references: [id])
}
```

## Database


| id | name     |
| -- | -------- |
| u1 | Prashant |
| u2 | Rahul    |
| u3 | Aman     |

---------------------------------------------------------

| id | title        | authorId |
| -- | ------------ | -------- |
| b1 | Prisma Guide | u1       |
| b2 | Next.js      | u1       |
| b3 | Docker       | u2       |


---------------------------------------------

## ✣ connect

--  Connect is used to create a relationship between two existing records (models).


Using connect
```js
await prisma.blog.create({
  data: {
    title: "Learning Prisma",

    author: {
      connect: {
        id: "u1"
      }
    }
  }
})
```

```
Before 

Rahul   Blog(B1)

 After 

Rahul -----------> Blog(B1)
```




What happens internally?

When You Write

```js
author: {
    connect: {
        id: "u1"
    }
}
```


```js 
Prisma does

Find User whose id = u1

↓

If found

↓

Set blog.authorId = u1
```

---------------------------------------------------------------------------------------------------------------------------

## ✣ Disconnect

Remove relation.

Don't delete anything.

Before

Rahul
   │
 Docker

 ```js
await prisma.blog.update({
  where:{
    id:"b3"
  },
  data:{
    author:{
      disconnect:true
    }
  }
})
```
```
After

Rahul

Docker

Blog still exists.

Only relation removed.

```


---------------------------------------------------------------------------------------------------------------------------

## ✣ Create

create is used when the related record does not exist yet.

It creates the related record and automatically creates the relationship.

```js

await prisma.user.create({
  data:{
    name:"Kevin",

    blogs:{
      create:{
        title:"Prisma Basics"
      }
    }
  }
})

```


---------------------------------------------------------------------------------------------------------------------------

## ✣ createMany

Create lots of related records.

```js

await prisma.user.update({
  where:{
    id:"u1"
  },
  data:{
    blogs:{
      createMany:{
        data:[
          {
            title:"Redis"
          },
          {
            title:"Kafka"
          },
          {
            title:"Nginx"
          }
        ]
      }
    }
  }
})

```
---------------------------------------------------------------------------------------------------------------------------
## ✣ createMany

Does it exist?

YES

↓

Connect

NO

↓

Create


```js
await prisma.blog.create({
  data:{
    title:"GraphQL",

    author:{
      connectOrCreate:{
        where:{
          id:"u5"
        },

        create:{
          name:"New User"
        }
      }
    }
  }
})
```

#case1

User exists

↓

Connect.

#Case 2

Create User.

↓

Connect automatically.

---------------------------------------------------------------------------------------------------------------------------
## ✣ update

Update related record.


```js

await prisma.user.update({
  where:{
    id:"u1"
  },

  data:{
    blogs:{
      update:{
        where:{
          id:"b1"
        },

        data:{
          title:"Advanced Prisma"
        }
      }
    }
  }
})

```


---------------------------------------------------------------------------------------------------------------------------
## ✣ updateMany

Update many related records.
```js

Publish every blog.

await prisma.user.update({
  where:{
    id:"u1"
  },

  data:{
    blogs:{
      updateMany:{
        where:{},

        data:{
          published:true
        }
      }
    }
  }
})

All blogs become published.

```
---------------------------------------------------------------------------------------------------------------------------



## ✣ Delete 

Delete ONE related record.

```js
await prisma.user.update({
  where:{
    id:"u1"
  },

  data:{
    blogs:{
      delete:{
        id:"b2"
      }
    }
  }
})
```


---------------------------------------------------------------------------------------------------------------------------

## ✣ deleteMany

Delete many.

```js
await prisma.user.update({
  where:{
    id:"u1"
  },

  data:{
    blogs:{
      deleteMany:{
        published:false
      }
    }
  }
})

Deletes every unpublished blog.

```
---------------------------------------------------------------------------------------------------------------------------

## ✣  set

Replace ALL relations.

Imagine

Current

Prashant

├── Blog1

├── Blog2

└── Blog3

Now
```js
await prisma.user.update({
  where:{
    id:"u1"
  },

  data:{
    blogs:{
      set:[
        {
          id:"b8"
        },
        {
          id:"b9"
        }
      ]
    }
  }
})
```
After

Prashant

├── Blog8

└── Blog9

Old relations removed.

New relations added.


---------------------------------------------------------------------------------------------------------------------------