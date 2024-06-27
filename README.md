# Typescript note

## 1. Typescript Types:

### 1.1 `unknown`

- `unknown` is the type-safe counterpart of any. Anything is assignable to `unknown`, but `unknown` isn’t assignable to anything but **_itself_** and `any` without a type assertion.

#### Example:

```ts
function f1(a: any) {
  a.b(); // OK
}

function f2(a: unknown) {
  // Error: Property 'b' does not exist on type 'unknown'.
  a.b();
}
```

### 1.2 `never`

- The never type represents the type of values that never occur. For instance, never is the return type for a function expression or an arrow function expression that always throws an exception or one that never returns.

#### Example:

```ts
// Function returning never must not have a reachable end point
function error(message: string): never {
  // Never return
  throw new Error(message);
}

// Inferred return type is never
function fail() {
  return error("Something failed");
}

// Function returning never must not have a reachable end point
function infiniteLoop(): never {
  // Never return
  // This can causes event loop blocking
  while (true) {}
}
```

## 2. Non Null Assertion

- The non-null assertion operator `!` is a type assertion in TS that allows you to tell the compiler that a value will never be `null` or `undefined`.

#### Example:

```ts
let name: string | null = null;

// we use the non-null assertion operator to tell the compiler that name will never be null
let nameLength = name!.length;
```

## 3. Type Inference

- Type inference in Typescript refers to the process of automatically determining the type of a variable based on the value you assigned to it.

#### Example:

```ts
let name = "John Doe";
```

- In this example, the TSC automatically infers that the type of the `name` variable is `string`.

- And because the `name` variable has `string` type so you can not assign any other types.

#### Example:

```ts
let name = "John Doe";
name = 123; // Error
```

## 4. Type Compatibility

- Typescript uses structural to determine type compatibility. This means that two types are considered compatible if they have the same structure, regardless of their names.

#### Example:

```ts
interface Point {
  x: number;
  y: number;
}

let p1: Point = { x: 10, y: 20 };
let p2: { x: number; y: number } = p1;

console.log(p2.x); // Output: 10
```

## 5. Combining Types

- In Typescript, you can combine types using **type union** and **type intersection**

### 5.1. Type Union

- The union operator `|` is used to combine two or more types into a single type

#### Example:

```ts
type stringOrNumber = string | number;
let value: stringOrNumber = "hello";

value = 42;
```

### 5.2. Type Intersection

- The intersection operator `&` is used to intersect two or more types into a single type that represents the properties of all the types.

#### Example:

```ts
interface A {
  a: string;
}

interface B {
  b: number;
}

type AB = A & B;
let value: AB = { a: "hello", b: 42 };
```

### 5.3. Type Aliases

- The **Type Aliases** in Typescript allows you to a new name for a type.

#### Example:

```ts
type Name = string;
type Age = number;
type User = { name: Name; age: Age };

const user: User = { name: "John", age: 30 };
```

### 5.4. `keyof` Operator

- The `keyof` operator in Typescript is used to get the union of keys from an object type.

#### Example:

```ts
interface User {
  name: string;
  age: number;
  location: string;
}

type UserKeys = keyof User; // "name" | "age" | "location"
const key: UserKeys = "name";
```

## 6. Type Guards

- Type guards are a way to narrow down the type of a variable. This is useful when you want to do something different depending on the type of variable.

- This operator comes up pretty often in a number of Javascript Libraries and Typescript can understand it to narrow types in different branches.

### 6.1. `typeof` Operator

- The `typeof` operator is used to check the type of a variable. It returns a string value representing the type of the variable.

- This often used in Javascript libraries when a Function takes a lot of parameters.

#### Example:

```ts
let value: string | number = "hello";

if (typeof value === "string") {
  console.log("value is a string");
} else {
  console.log("value is a number");
}
```

### 6.2. `instanceof` Operator

- The `instanceof` operator is a way to narrow down the type of a variable. It is used to check if an object is an instance of a class.

#### Example:

```ts
class Bird {
  fly() {
    console.log("flying...");
  }
  layEggs() {
    console.log("laying eggs...");
  }
}

const pet = new Bird();

// instanceof
if (pet instanceof Bird) {
  pet.fly();
} else {
  console.log("pet is not a bird");
}
```

## 7. Interfaces

### 7.1. Extending Interfaces

- In Typescript, you can extend an interface by creating a new interface that inherits from the original interface using the `extends` keyword. The new interface can include additional _properties_, _methods_, or _redefine_ the members of the original interface.

#### Example:

```ts
interface Shape {
  width: number;
  height: number;
}

interface Square extends Shape {
  sideLength: number;
}

let square: Square = {
  width: 10,
  height: 10,
  sideLength: 10,
};
```

### 7.2. Hybrid Types

- A hybrid type that combines multiple types into a single type.

#### Example:

```ts
type Education = {
  degree: string;
  school: string;
  year: number;
};

type User = {
  name: string;
  age: number;
  email: string;
  education: Education;
};
```

## 8. Classes

### 8.1. Constructor Params

#### A single constructor

```ts
class Example {
  constructor(
    private name: string,
    public age: number
  ) {}
}
```

#### Overloading constructor

```ts
class Point {
  // Overloads
  constructor(x: number, y: string);
  constructor(s: string);
  constructor(xs: any, y?: any) {
    // TBD
  }
}
```

### 8.2. Access Modifiers

- There are 3 access modifiers:

  1. `public`: Default. Can be accessed from anywhere.
  2. `private`: Can only be accessed from within the same class.
  3. `protected`: Can be accessed from within or its subclasses.

### 8.3. Abstract Classes

- Abstract classes are classes that cannot be instantiated on their own and must be subclassed by other classes.

#### Example:

```ts
abstract class Animal {
  abstract makeSound(): void;

  move(): void {
    console.log("moving...");
  }
}

class Dog extends Animal {
  makeSound(): void {
    console.log("bark");
  }
}
```

### 8.4. Inheritance vs Polymorphism

- Inheritance refers to a mechanism where subclass inherits properties and methods from its parent class. This allows a subclass to reuse the code and behavior of its parent class while also adding or modifying its own behavior.

- Polymorphism refers to the ability of an object to take on many forms. This allows objects of different classes to be treated as objects of a common class.

#### Example:

```ts
class Animal {
  makeSound(): void {
    console.log("Making animal sound");
  }
}

class Dog extends Animal {
  makeSound(): void {
    console.log("Bark");
  }
}

class Cat extends Animal {
  makeSound(): void {
    console.log("Meow");
  }
}

let animal: Animal;

animal = new Dog();
animal.makeSound(); // Output: Bark

animal = new Cat();
animal.makeSound(); // Output: Meow
```

### 8.5. Method Overriding

```ts
class Animal {
  makeSound(): void {
    console.log("Making animal sound");
  }
}

class Dog extends Animal {
  makeSound(): void {
    console.log("Bark");
  }
}

let animal: Animal;

animal = new Dog();
animal.makeSound(); // Output: Bark
```


