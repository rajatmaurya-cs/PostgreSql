# Prisma Phase 7: Aggregation

## Model Used

``` prisma
model User {
  id     String @id @default(cuid())
  name   String
  age    Int
  salary Int
  city   String
}
```

Sample Data:

  Name         Age   Salary City
  ---------- ----- -------- --------
  Prashant      21    50000 Delhi
  Rahul         22    60000 Delhi
  Amit          23    55000 Mumbai
  Mohit         21    70000 Mumbai

------------------------------------------------------------------------

# 1. `_count`

Counts records.

``` ts
const result = await prisma.user.aggregate({
  _count: true,
});
```

Output

``` json
{
  "_count": 4
}
```

Count a specific field:

``` ts
const result = await prisma.user.aggregate({
  _count: {
    salary: true,
  },
});
```

------------------------------------------------------------------------

# 2. `_avg`

Calculates the average.

``` ts
const result = await prisma.user.aggregate({
  _avg: {
    salary: true,
  },
});
```

Output

``` json
{
  "_avg": {
    "salary": 58750
  }
}
```

------------------------------------------------------------------------

# 3. `_sum`

Adds all values.

``` ts
const result = await prisma.user.aggregate({
  _sum: {
    salary: true,
  },
});
```

Output

``` json
{
  "_sum": {
    "salary": 235000
  }
}
```

------------------------------------------------------------------------

# 4. `_min`

Returns the smallest value.

``` ts
const result = await prisma.user.aggregate({
  _min: {
    salary: true,
  },
});
```

Output

``` json
{
  "_min": {
    "salary": 50000
  }
}
```

------------------------------------------------------------------------

# 5. `_max`

Returns the largest value.

``` ts
const result = await prisma.user.aggregate({
  _max: {
    salary: true,
  },
});
```

Output

``` json
{
  "_max": {
    "salary": 70000
  }
}
```

------------------------------------------------------------------------

# Multiple Aggregations

``` ts
const stats = await prisma.user.aggregate({
  _count: true,
  _avg: { salary: true },
  _sum: { salary: true },
  _min: { salary: true },
  _max: { salary: true },
});
```

------------------------------------------------------------------------

# Aggregation with `where`

``` ts
const result = await prisma.user.aggregate({
  where: {
    city: "Delhi",
  },
  _avg: {
    salary: true,
  },
});
```

Average salary in Delhi = **55000**

------------------------------------------------------------------------

# 6. `groupBy()`

Groups records before applying aggregations.

## Average Salary by City

``` ts
const result = await prisma.user.groupBy({
  by: ["city"],
  _avg: {
    salary: true,
  },
});
```

Output

``` json
[
  {
    "city": "Delhi",
    "_avg": {
      "salary": 55000
    }
  },
  {
    "city": "Mumbai",
    "_avg": {
      "salary": 62500
    }
  }
]
```

------------------------------------------------------------------------

## Count Users by City

``` ts
const result = await prisma.user.groupBy({
  by: ["city"],
  _count: true,
});
```

Output

``` json
[
  {
    "city": "Delhi",
    "_count": 2
  },
  {
    "city": "Mumbai",
    "_count": 2
  }
]
```

------------------------------------------------------------------------

## Sum Salary by City

``` ts
const result = await prisma.user.groupBy({
  by: ["city"],
  _sum: {
    salary: true,
  },
});
```

Output

``` json
[
  {
    "city": "Delhi",
    "_sum": {
      "salary": 110000
    }
  },
  {
    "city": "Mumbai",
    "_sum": {
      "salary": 125000
    }
  }
]
```

------------------------------------------------------------------------

## Group by Multiple Columns

``` ts
const result = await prisma.user.groupBy({
  by: ["city", "age"],
  _count: true,
});
```

Example Output

``` json
[
  {
    "city": "Delhi",
    "age": 21,
    "_count": 1
  },
  {
    "city": "Delhi",
    "age": 22,
    "_count": 1
  },
  {
    "city": "Mumbai",
    "age": 21,
    "_count": 2
  }
]
```

------------------------------------------------------------------------

# Real-World Examples

### Blog Dashboard

``` ts
_count: {
  comments: true
}
```

### E-commerce

``` ts
_sum: {
  totalPrice: true
}
```

### School

``` ts
_avg: {
  marks: true
}
```

### Employee System

``` ts
_max: {
  salary: true
}
```

### Analytics

``` ts
groupBy({
  by: ["category"],
  _count: true
})
```

------------------------------------------------------------------------

# Quick Revision

  Method      Purpose                            Example
  ----------- ---------------------------------- -----------------
  `_count`    Count records                      Number of users
  `_avg`      Average                            Average salary
  `_sum`      Total                              Total revenue
  `_min`      Smallest value                     Lowest price
  `_max`      Largest value                      Highest salary
  `groupBy`   Group records before aggregation   Users per city

------------------------------------------------------------------------
