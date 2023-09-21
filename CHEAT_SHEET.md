## Typescript cheat-sheet



```
interface User {
  id: string;
  firstName: string;
  lastName: string;
}
```

- `type MyType = Omit<User, "id">;`
- `type MyType = Pick<User, "firstName" | "lastName">;`