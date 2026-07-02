```
Transactions

What is a Transaction?

A transaction is a group of database operations that are treated as one single unit.

It follows the ACID principle:

Atomicity → Either everything succeeds or nothing happens.
Consistency → Database remains valid.
Isolation → Transactions don't interfere with each other.
Durability → Once committed, changes are permanent.

Think of it like a bank transfer.

Suppose Rahul sends ₹1000 to Prashant.

There are two operations:

1. Deduct ₹1000 from Rahul
2. Add ₹1000 to Prashant

If the first succeeds but the second fails...

Rahul = ₹9000
Prashant = ₹5000

₹1000 disappeared!

That's why both operations must happen together.

Both Success ✅

OR

Both Fail ❌

That's exactly what a transaction does.
```

```
Prisma Transactions

Prisma provides two ways:

1. $transaction([])
2. Interactive Transaction

```

```
1. $transaction([])

This is the simplest transaction.

You give Prisma multiple independent queries.

await prisma.$transaction([
    query1,
    query2,
    query3
])

Prisma executes all of them.

If any query fails,

everything is rolled back.

```



```

 (Bank Transfer)

Suppose

Rahul

Balance = 10000

Prashant

Balance = 5000

Transfer ₹1000.
```
```js
await prisma.$transaction([

    prisma.user.update({
        where:{
            id:"rahul"
        },
        data:{
            balance:{
                decrement:1000
            }
        }
    }),

    prisma.user.update({
        where:{
            id:"prashant"
        },
        data:{
            balance:{
                increment:1000
            }
        }
    })

])

```

If second query fails

↓

First update is also undone.

Execution
Start Transaction

↓

Update Rahul

↓

Update Prashant

↓

Success?

↓

YES

↓

Commit

If failure

Start

↓

Update Rahul

↓

Update Prashant

↓

Failed

↓

Rollback

Database returns to previous state.

```