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