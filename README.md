# Rustin Buster Cheatsheet

## Table of Contents
1. [Variable Declaration](#variable-declaration)
2. [Basic Data Types](#basic-data-types)
3. [Structs](#structs)
4. [Implementations (impls)](#implementations-impls)
5. [Enums](#enums)
6. [Pattern Matching](#pattern-matching)
7. [Vectors](#vectors)
8. [Ownership and Borrowing](#ownership-and-borrowing)
9. [Traits](#traits)
10. [Error Handling](#error-handling)
11. [Closures](#closures)
12. [Iterators](#iterators)
13. [Generics](#generics)
14. [Lifetimes](#lifetimes)
15. [Smart Pointers](#smart-pointers)
16. [Concurrency](#concurrency)
17. [Modules](#modules)

## Variable Declaration

In Rust, variables are immutable by default, promoting safer and more predictable code.

```rust
let x = 5; // Immutable variable
let mut y = 10; // Mutable variable
const MAX_POINTS: u32 = 100_000; // Constant
```

- `let` declares an immutable variable.
- `let mut` declares a mutable variable that can be changed later.
- `const` declares a constant, which must have a type annotation and be assigned a constant expression.

## Basic Data Types

Rust has several basic data types to represent numbers, characters, and strings.

```rust
let a: u32 = 42; // Unsigned 32-bit integer
let b: i64 = -100; // Signed 64-bit integer
let c: f32 = 3.14; // 32-bit floating-point number
let d: bool = true; // Boolean
let e: char = 'x'; // Character
let f: &str = "Hello, World!"; // String slice
let g: String = String::from("Hello, World!"); // Owned String
```

- `u32`: Unsigned 32-bit integer (0 to 4,294,967,295)
- `i64`: Signed 64-bit integer (-9,223,372,036,854,775,808 to 9,223,372,036,854,775,807)
- `f32`: 32-bit floating-point number
- `bool`: Boolean (true or false)
- `char`: Unicode scalar value
- `&str`: String slice (borrowed string)
- `String`: Owned string (can be modified)

## Structs

Structs are custom data types that group related data together.

```rust
struct Person {
    name: String,
    age: u32,
}

// Creating an instance of a struct
let person = Person {
    name: String::from("Alice"),
    age: 30,
};

// Accessing struct fields
println!("Name: {}", person.name); // Name: Alice
println!("Age: {}", person.age); // Age: 30

// Mutable struct instance
let mut mutable_person = Person {
    name: String::from("Bob"),
    age: 25,
};
mutable_person.age = 26; // Updating age field

// Tuple Struct
struct Point(i32, i32);
let point = Point(10, 20);

// Unit Struct (marker)
struct Unit;
let unit = Unit;
```

- Regular structs have named fields.
- Tuple structs have unnamed fields, accessed by index.
- Unit structs have no fields and are often used as markers.

## Implementations (impls)

Implementations allow you to define methods and associated functions for structs.

```rust
impl Person {
    // Associated function (constructor)
    fn new(name: &str, age: u32) -> Person {
        Person {
            name: String::from(name),
            age,
        }
    }

    // Method
    fn say_hello(&self) {
        println!("Hello, my name is {}", self.name);
    }

    // Mutable method
    fn have_birthday(&mut self) {
        self.age += 1;
    }
}

// Using the `new` constructor and methods
let mut alice = Person::new("Alice", 30);
alice.say_hello(); // Hello, my name is Alice
alice.have_birthday();
println!("Alice's new age: {}", alice.age); // Alice's new age: 31
```

- Associated functions (like `new`) are called on the struct itself.
- Methods take `&self` or `&mut self` as the first parameter and are called on instances.

## Enums

Enums allow you to define a type that can be one of several variants.

```rust
enum Direction {
    North,
    South,
    East,
    West,
}

let direction = Direction::East;

enum OptionalInt {
    Value(i32),
    Nothing,
}

let x = OptionalInt::Value(42);
let y = OptionalInt::Nothing;
```

- Enums can have variants with no data, like `Direction`.
- Enums can also have variants with associated data, like `OptionalInt`.

## Pattern Matching

Pattern matching is a powerful feature in Rust for destructuring and matching against patterns.

```rust
match direction {
    Direction::North => println!("Going North"),
    Direction::South => println!("Going South"),
    Direction::East => println!("Going East"),
    Direction::West => println!("Going West"),
}

match x {
    OptionalInt::Value(n) => println!("Value: {}", n),
    OptionalInt::Nothing => println!("Nothing"),
}
```

- `match` expressions must be exhaustive, covering all possible cases.
- Pattern matching can destructure enums, tuples, and structs.

## Vectors

Vectors are dynamic arrays that can grow or shrink in size.

```rust
let mut vec = Vec::new();
vec.push(1);
vec.push(2);
vec.push(3);
let first = vec[0]; // 1

// Slices
let slice = &vec[1..3]; // Creates a slice of the vector from index 1 to 2
println!("Slice: {:?}", slice); // Slice: [2, 3]

// Iterating over vectors
for x in &vec {
    println!("{}", x);
}
```

- Vectors can store elements of the same type.
- Slices are references to a contiguous sequence of elements in a collection.
- Vectors can be iterated over using a `for` loop.

## Ownership and Borrowing

Rust's ownership system is a key feature that ensures memory safety without a garbage collector.

```rust
let s1 = String::from("hello");
let s2 = s1; // s1 is invalid after this line
let s3 = s2.clone(); // Create a clone
let s4 = String::from("world");
let s5 = &s4; // Borrowing reference
println!("{}, {}", s5, s4); // world, world
```

- When a value is assigned to another variable, ownership is transferred (moved).
- To create a deep copy, use the `clone()` method.
- References allow you to refer to a value without taking ownership.

## Traits

Traits define shared behavior across types.

```rust
trait Summary {
    fn summarize(&self) -> String;
}

struct Article {
    headline: String,
    content: String,
}

impl Summary for Article {
    fn summarize(&self) -> String {
        format!("{} - {}", self.headline, self.content.split_whitespace().take(5).collect::<Vec<&str>>().join(" "))
    }
}

let article = Article {
    headline: String::from("Rust Cheatsheet"),
    content: String::from("This is a cheatsheet for the Rust programming language."),
};
println!("{}", article.summarize());
```

- Traits are similar to interfaces in other languages.
- Types can implement multiple traits.
- Trait methods can have default implementations.

## Error Handling

Rust uses the `Result` and `Option` types for error handling.

```rust
// Using Result
fn divide(x: f64, y: f64) -> Result<f64, String> {
    if y == 0.0 {
        Err(String::from("Division by zero"))
    } else {
        Ok(x / y)
    }
}

match divide(10.0, 2.0) {
    Ok(result) => println!("Result: {}", result),
    Err(e) => println!("Error: {}", e),
}

// Using Option
fn find_user(id: u32) -> Option<String> {
    if id == 1 {
        Some(String::from("Alice"))
    } else {
        None
    }
}

match find_user(1) {
    Some(name) => println!("User found: {}", name),
    None => println!("User not found"),
}
```

- `Result<T, E>` represents either success (`Ok(T)`) or failure (`Err(E)`).
- `Option<T>` represents either some value (`Some(T)`) or no value (`None`).

## Closures

Closures are anonymous functions that can capture their environment.

```rust
let add_one = |x: i32| x + 1;
println!("5 + 1 = {}", add_one(5));
```

- Closures can be used as arguments to higher-order functions.
- They can capture variables from their surrounding scope.

## Iterators

Iterators allow you to process sequences of elements.

```rust
let numbers = vec![1, 2, 3, 4, 5];
let doubled: Vec<i32> = numbers.iter().map(|&x| x * 2).collect();
println!("Doubled numbers: {:?}", doubled);
```

- Iterators are lazy and don't compute values until consumed.
- Many methods like `map`, `filter`, and `fold` are available on iterators.

## Generics

Generics allow you to write flexible, reusable code that works with multiple types.

```rust
fn largest<T: PartialOrd>(list: &[T]) -> &T {
    let mut largest = &list[0];
    for item in list.iter() {
        if item > largest {
            largest = item;
        }
    }
    largest
}

let numbers = vec![34, 50, 25, 100, 65];
println!("The largest number is {}", largest(&numbers));
```

- `<T: PartialOrd>` specifies that the type `T` must implement the `PartialOrd` trait.
- Generics can be used with functions, structs, and enums.

## Lifetimes

Lifetimes ensure that references are valid for as long as they're used.

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}

let result;
{
    let string1 = String::from("short");
    let string2 = String::from("longer");
    result = longest(string1.as_str(), string2.as_str());
}
println!("The longest string is {}", result);
```

- Lifetimes are denoted with an apostrophe, like `'a`.
- They ensure that references don't outlive the data they refer to.

## Smart Pointers

Smart pointers are data structures that act like pointers but have additional metadata and capabilities.

```rust
use std::rc::Rc;

let s = Rc::new(String::from("Hello, Rust!"));
let s1 = Rc::clone(&s);
let s2 = Rc::clone(&s);

println!("{} has {} strong references", s, Rc::strong_count(&s));
```

- `Rc<T>` (Reference Counted) allows multiple ownership of the same data.
- It keeps track of the number of references to a value.

## Concurrency

Rust provides tools for safe and efficient concurrent programming.

```rust
use std::thread;
use std::time::Duration;

let handle = thread::spawn(|| {
    for i in 1..10 {
        println!("hi number {} from the spawned thread!", i);
        thread::sleep(Duration::from_millis(1));
    }
});

handle.join().unwrap();
```

- `thread::spawn` creates a new thread.
- `handle.join()` waits for the spawned thread to finish.

## Modules

Modules help organize and encapsulate code.

```rust
mod math {
    pub fn add(a: i32, b: i32) -> i32 {
        a + b
    }
}

use math::add;
println!("5 + 3 = {}", add(5, 3));
```

- `mod` defines a module.
- `pub` makes items public and accessible outside the module.
- `use` brings items into scope.

This cheatsheet covers many of the fundamental concepts in Rust. Each section provides code examples and explanations to help you understand and use these features effectively in your Rust programs.
