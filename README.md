## Typescript cheat-sheet

In this repo, I will be sharing some most commonly used typescript types, this list will be biased because it will depend mostly on what I encounter the most in typescript 🚀

## Code examples

1. Question

Params has a missing type here

```
export const addTwoNumbers = (params) => {
  return params.first + params.second;
};
```

Here are some solutions.

```
 // ---------------------First-------------------------------

export const addTwoNumbers = (params: { first: number; second: number }) => {
  return params.first + params.second;
};



 // ---------------------Second-------------------------------
type AddTwoNumbersArgs = {
  first: number;
  second: number;
};

export const addTwoNumbers = (params: AddTwoNumbersArgs) => {
  return params.first + params.second;
};


 // ---------------------Third--------------------------------


interface AddTwoNumbersArgs {
  first: number;
  second: number;
}

export const addTwoNumbers = (params: AddTwoNumbersArgs) => {
  return params.first + params.second;
};
```

2. Arrays

```
Array<string> or string[]
Array<customType> or customType[]
```

3. For objects

use `Record`

```

const createCache = () => {
  const cache: Record<string, string> = {};

  const add = (id: string, value: string) => {
    cache[id] = value;
  };

  const remove = (id: string) => {
    delete cache[id];
  };

  return {
    cache,
    add,
    remove,
  };
};


```

or

```
  const cache: {
    [id: string]: string;
  } = {};

```

or

```
interface Cache {
  [id: string]: string;
}

  const cache: Cache = {};
```

4. Function parameters

```
// Here amount is either number or an object { amount: number }

const myAmount = (amount: number | { amount: number }) => {
  if (typeof amount === "number") {
    return amount;
  }
  return amount.amount;
};
```

5. Error Handling

- Check if e is an instance of error then only return the e.message
- Otherwise e might not have message property

```
const tryCatchDemo = (state: "fail" | "succeed") => {
  try {
    if (state === "fail") {
      throw new Error("Failure!");
    }
  } catch (e) {
    if (e instanceof Error) {
      return e.message;
    }
  }
};

```

6. Inheritance of types/interface

- When your types/interface have something in common, we can use extend keyword to inherit types/interfaces from it.

```
interface Base {
  id: string;
}

interface User extends Base {
  firstName: string;
  lastName: string;
}

interface Post extends Base {
  title: string;
  body: string;
}

```

7. Using & for combining types

```

interface User {
  id: string;
  firstName: string;
  lastName: string;
}

interface Post {
  id: string;
  title: string;
  body: string;
}

const myFunction = (): User & { posts: Post[] }  => {
  // Do something
}


```

8. utility types

We can take certain fields from one type to make another type.

```
interface User {
  id: string;
  firstName: string;
  lastName: string;
}

type MyType = Omit<User, "id">;
or
type MyType = Pick<User, "firstName" | "lastName">;
```

9 Async await functions type

They should return `Promise`

```
createUser: () => Promise<string>,

```

## React Typescript

- `React.FC` for functional components

```
 export const MyComponent: React.FC<Props> = (props: Props) => {
  return <div>Hello world! </div>
};
```

- children in react components => `React.ReactNode`

```
export const Button = (props: {children : React.ReactNode}) => {
  return <button>{props.children}</button>;
};
```

- onClick handlers => `React.MouseEventHandler` or `React.MouseEventHandler<HTMLButtonElement>` or `React.MouseEventHandler<HTMLElement>`

```
interface ButtonProps {
  className: string;
  children: React.ReactNode;
  onClick: React.MouseEventHandler;
}

```
