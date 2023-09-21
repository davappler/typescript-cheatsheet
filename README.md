## Typescript cheat-sheet

In this repo, I will be sharing some most commonly used typescript types, this list will be based on what I encounter the most in typescript 🚀

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

- `React.ComponentProps` or `ComponentProps`

```
export const Button = ({ className, ...rest }: React.ComponentProps<"button">) => {
  return (
    <button {...rest} className={`default-classname ${className}`}></button>
  );
};s

```

- onChange input field => `e: React.ChangeEvent<HTMLInputElement>`

- If we want to modify an existing type property we can do like this below:

  - In this case below we are omitting the onChange value and adding a new onChange type with our own type value.

    ```
    type InputProps = Omit<ComponentProps<"input">, "onChange"> & {
      onChange: (value: string) => void;
    };


    // a cleaner way to write InputProps
    interface InputProps extends Omit<ComponentProps<"input">, "onChange"> {
      onChange: (value: string) => void;
    }

    const Input = (props: InputProps) => {
      return (
        <input
          {...props}
          onChange={(e) => {
            props.onChange(e.target.value);
          }}
        ></input>
      );
    };
    ```

  - We can also override the props
    ```
      type InputProps = OverrideProps<
        ComponentProps<"input">,
        {
          onChange: (value: string) => void;
        }
      >;
    ```

- Extracting props from a component and saving it into a type, we have this component `NavBar` and we extract props from it and save it in a variable.

  ```
  export const NavBar = (props: {
    title: string;
    links: string[];
    children: React.ReactNode;
  }) => {
    return <div>Some content</div>;
  };

  const MyType= React.ComponentProps<typeof NavBar>;
  ```

- Function comparison is less strict, for example if in setting a state, with a setter function

  - If there is no return type set for function then it allows you to send anything in return
  - Make sure to have a return type on arrow function.

  ```
  interface TagState {
    tagSelected: number | null;
    tags: { id: number; value: string }[];
  }

  const [state, setState] = useState<TagState>({
    tags: [],
    tagSelected: null,
  });

  setState((currentState) => ({
    ...currentState,
    tagSelected: tag.id,
  }));
  ```

- useMemo is pretty good without type arguments, you can avoid typing types in there.
  or
- just type the return type, there is no need to do ()=> string[], if you type string[], this is enough

```
// First way
export const Component = () => {
  const autoGeneratedIds = useMemo(() => {
    // generate 100 random string uuid's
    return Array.from({ length: 100 }, () =>
      Math.random().toString(36).substr(2, 9)
    );
  }, []);

// Second way
export const Component = () => {
  const autoGeneratedIds = useMemo<string[]>(() => {
    // generate 100 random string uuid's
    return Array.from({ length: 100 }, () =>
      Math.random().toString(36).substr(2, 9)
    );
  }, []);
```

- with useRef, we should initialize it with `null`, because by default it will be undefined and ref can not have undefined.

```
export const Component = () => {
  const ref = useRef<HTMLDivElement>(null);

  return <div ref={ref} />;
};

```

- current property of ref is read only, but we can mutate it to change its value. You can do that by passing an initial value to the `useRef<string>("abcd")` or keeping it empty `useRef<string>()`
- Then you can go ahead and change the value of current property
- When you pass null as initial value then it goes to another overload of useRef and makes the current property unMutable, why? because that how it is defined.

```
export const Component = () => {

  const firstOverload = useRef<string>("124123123");
  firstOverload.current = "123j12jhb123jhb"; // This is mutable

  const secondOverload = useRef<string>(null); 
  secondOverload.current = "Hello"; // This is readonly

  const thirdOverload = useRef<string>();
  thirdOverload.current = "123j12jhb123jhb"; // This is mutable

  return null;
};

```
