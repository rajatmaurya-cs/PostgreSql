```js
model Student {
  id      String   @id @default(cuid())
  name    String

  courses Course[]
}
```


```js
model Course {
  id       String    @id @default(cuid())
  title    String

  students Student[]
}
```


