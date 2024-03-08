# rustin

```rust
// Variable Declaration
let x = 5; // Immutable variable
let mut y = 10; // Mutable variable

// Basic Data Types
let a: u32 = 42; // Unsigned 32-bit integer
let b: i64 = -100; // Signed 64-bit integer
let c: f32 = 3.14; // 32-bit floating-point number
let d: bool = true; // Boolean
let e: char = 'x'; // Character
let f: &str = "Hello, World!"; // String slice

// Structs
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

// Implementations (impls)
impl Person {
    // Constructor
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
}

// Using the `new` constructor and `say_hello` method
let alice = Person::new("Alice", 30);
alice.say_hello(); // Hello, my name is Alice

// Enums
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

// Pattern Matching
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

// Vectors
let mut vec = Vec::new();
vec.push(1);
vec.push(2);
vec.push(3);

let first = vec[0]; // 1

// Iterating over vectors
for x in &vec {
    println!("{}", x);
}

// Ownership and Borrowing
let s1 = String::from("hello");
let s2 = s1; // s1 is invalid after this line
let s3 = s2.clone(); // Create a clone

let s4 = String::from("world");
let s5 = &s4; // Borrowing reference
println!("{}, {}", s5, s4); // world, world

// Traits
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
