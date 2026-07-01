model Student {
  id      String   @id @default(cuid())
  name    String

  courses Course[]
}

model Course {
  id       String    @id @default(cuid())
  title    String

  students Student[]
}



