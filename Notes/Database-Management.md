```js
1. npx prisma migrate dev

Use when?

✅ You are developing your project and you changed schema.prisma.

Examples:

Added a new field
Removed a field
Created a new model
Changed a relation

Example:

model User {
  id    String @id @default(cuid())
  name  String
  email String
}

Run:

npx prisma migrate dev --name add-email

What does it do?

✅ Creates a migration file


Now your project contains:

prisma/
└── migrations/
    ├── 001_create_user
    ├── 002_create_blog
    └── 003_add_email


✅ Updates the development database

✅ Generates the Prisma Client

Remember
Development = migrate dev
```



```js

2. npx prisma migrate deploy


Use when?

✅ Your application is going to production (Vercel, Railway, Render, AWS, etc.).

After pushing your code:

GitHub
   │
   ▼
Vercel Deploy

Run:

npx prisma migrate deploy
What does it do?
✅ Applies existing migration files to the production database
❌ Does NOT create new migrations
❌ Does NOT generate Prisma Client
Remember

Production = migrate deploy

```

```js
Suppose you already did this:

npx prisma migrate dev --name add-email

This created the migration and applied it to your local database.

Now your migration history looks like:

001_create_user   ✅ Applied
002_add_email     ✅ Applied
Then you run locally:
npx prisma migrate deploy
What happens?

Prisma checks:

"Are there any migration files that have not been applied to this database?"

It sees:

001_create_user   ✅ Already applied
002_add_email     ✅ Already applied

There are no pending migrations.

So it prints something like:

No pending migrations to apply.

and exits.

Nothing changes.

```


```js
3. prisma db push

What is it?

db push directly updates the database to match your schema.prisma.

It does not create migration files.

Think of it as:

"Take my schema and push it to the database."

Example

Current schema

model User {
  id   String @id @default(cuid())
  name String
}

Database

User
---------
id
name

Now you add

model User {
  id    String @id @default(cuid())
  name  String
  email String
}

Run

npx prisma db push

Prisma immediately updates the database.

Database becomes

User
---------
id
name
email
What does db push do?
schema.prisma
      │
      ▼
db push
      │
      ▼
Database Updated

Notice something?

There is no migration folder.

Does it create migration history?

❌ No.

If you check

prisma/

migrations/

Nothing new is created.

Does it generate Prisma Client?

✅ Yes.

Just like migrate dev, db push regenerates the Prisma Client after updating the schema.

When should you use it?

Use it when:

Learning Prisma
Building a prototype
Hackathon
Quickly testing something

Example:

"I just want to add one field quickly."

Use

npx prisma db push
Why not use db push in production?

Suppose after one month your database changes many times.

With db push there is no history.

You don't know:

Who added the column
When it was added
How to recreate the database

Companies want history.

That's why they use migrations.

Difference
migrate dev
Schema
   │
   ▼
Migration
   │
   ▼
Database

History is saved.

db push
Schema
   │
   ▼
Database

No history.


```


```js
4. prisma db pull

This is the opposite.

Think:

"Read the database and create my Prisma schema."

Example

Suppose someone already created a PostgreSQL database.

It contains

Users

Blogs

Comments

But your

schema.prisma

is empty.

Run

npx prisma db pull

Prisma inspects the database.

Automatically creates

model User {
  ...
}

model Blog {
  ...
}

model Comment {
  ...
}


Prisma pulls all the tables into a single file: schema.prisma.

```

```js
Prisma Studio

What is Prisma Studio?

Prisma Studio is a GUI (Graphical User Interface) for your database.

Instead of writing SQL like:

SELECT * FROM User;

you can open a web page and view your data.


How to open it?

Run:

npx prisma studio

Terminal:

Prisma Studio is running on http://localhost:5555

```


```js
5.Database Seeding:inserting data manually

Create seed.ts

Inside your project:

prisma/
│
├── schema.prisma
├── seed.ts


Write the Seed Script
import { PrismaClient } from "@prisma/client";

const prisma = new PrismaClient();

async function main() {
  const user = await prisma.user.create({
    data: {
      name: "Prashant",
      email: "prashant@gmail.com",
      password: "hashed_password",
    },
  });

  console.log("User Created:", user);
}

main()
  .catch((error) => {
    console.error(error);
    process.exit(1);
  })
  .finally(async () => {
    await prisma.$disconnect();
  });


  And run command 

  npx prisma db seed




```