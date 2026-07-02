```js

import { Prisma } from "@prisma/client";

try {

    await prisma.user.create({

        data:{

            email:"abc@gmail.com",

            name:"Prashant"

        }

    });

}
catch(error){

    if(error instanceof Prisma.PrismaClientKnownRequestError){

        switch(error.code){

            case "P2002":

                console.log("Email already exists");

                break;

            case "P2003":

                console.log("Foreign key error");

                break;

            case "P2011":

                console.log("Null value not allowed");

                break;

            default:

                console.log("Database error");

        }

    }

}

```





## Why do we write `error instanceof Prisma.PrismaClientKnownRequestError`?

`instanceof` checks whether an object is an instance of a particular class.

When an error is caught in a `catch` block, it can be **any type of error** (JavaScript error, Prisma error, network error, etc.).

```ts
if (error instanceof Prisma.PrismaClientKnownRequestError) {
  // Safe to access error.code
}
```

Here, we are checking whether the `error` object belongs to the `PrismaClientKnownRequestError` class.

If the check returns `true`, Prisma guarantees that the error contains a `code` property (such as `P2002`, `P2003`, `P2011`, etc.), which tells us the exact database error.

This allows us to safely write:

```ts
switch (error.code) {
  case "P2002":
    console.log("Unique constraint failed");
    break;

  case "P2003":
    console.log("Foreign key constraint failed");
    break;
}
```

Without the `instanceof` check, the caught error might not be a Prisma error, and trying to access `error.code` could be incorrect or unsafe.


