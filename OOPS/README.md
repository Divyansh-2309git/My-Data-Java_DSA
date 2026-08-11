# Object-Oriented Programming (OOP) in Java

## 1. What Is This?

Object-Oriented Programming (OOP) is a programming paradigm organized around "objects" rather than actions, and "data" rather than logic.

An **Object** is a real-world entity that holds state (attributes/fields) and behavior (methods/functions). A **Class** is a user-defined blueprint or template from which individual objects are instantiated.

OOP enables software engineers to model real-world domains cleanly, promote code reusability, enforce security through data encapsulation, and build scalable software architectures.

---

## 2. Core Idea: The Four Pillars of OOP

```text
                  ┌─────────────────────────────────────────┐
                  │    FOUR PILLARS OF OOP IN JAVA          │
                  └────────────────────┬────────────────────┘
                                       │
        ┌──────────────────┬───────────┴───────────┬──────────────────┐
        ▼                  ▼                       ▼                  ▼
┌───────────────┐  ┌───────────────┐       ┌───────────────┐  ┌───────────────┐
│ Encapsulation │  │  Abstraction  │       │  Inheritance  │  │ Polymorphism  │
│ Data hiding   │  │ Hiding complex│       │ Code reusability │ Compile-time  │
│ via private   │  │ details via   │       │ via parent    │  vs Runtime   │
│ & getters/    │  │ interfaces &  │       │ child classes │  dispatch     │
│ setters       │  │ abstract      │       │               │               │
└───────────────┘  └───────────────┘       └───────────────┘  └───────────────┘
```

1. **Encapsulation:** Wrapping data (variables) and code (methods) together into a single unit (Class) and restricting direct access using `private` access modifiers.
2. **Abstraction:** Hiding complex implementation details and displaying only essential features to the user.
3. **Inheritance:** The mechanism by which a child class acquires all properties and behaviors of a parent class (`extends`).
4. **Polymorphism:** The ability of a message, method, or object to take on multiple forms (Compile-time Method Overloading & Runtime Method Overriding).

---

## 3. Important Terminology

| Term | Meaning |
|---|---|
| **Class** | Logical blueprint defining fields and methods. |
| **Object / Instance** | Physical entity created in Heap memory from a class blueprint using `new`. |
| **Constructor** | Special block of code invoked automatically during object creation to initialize fields. |
| **Shallow Copy** | Copying object fields as-is; reference variables copy memory addresses, sharing inner arrays! |
| **Deep Copy** | Creating brand-new memory allocations for inner arrays/objects so copies are completely independent. |
| **Method Overloading** | Same method name with different parameter lists in the same class (Compile-time). |
| **Method Overriding** | Subclass provides a specific implementation of a method defined in parent class (Runtime). |

---

## 4. Visual / Mental Model

### Shallow Copy vs Deep Copy Memory Layout

Suppose object `s1` has an array `marks = [90, 85]`.

#### Shallow Copy (`s2` created from `s1`):
```text
  s1 ──► [ name="Alex", marks ──┐ ]
                                 ├──► [ 90 , 85 ]  (SHARED ARRAY IN HEAP!)
  s2 ──► [ name="Alex", marks ──┘ ]
```
*Modifying `s2.marks[0] = 100` WILL ACCIDENTALLY CHANGE `s1.marks[0]`!*

#### Deep Copy (`s2` created from `s1`):
```text
  s1 ──► [ name="Alex", marks ──► [ 90 , 85 ] ]
  s2 ──► [ name="Alex", marks ──► [ 90 , 85 ] ]  (INDEPENDENT ARRAY ALLOCATED!)
```
*Modifying `s2.marks[0]` leaves `s1.marks[0]` untouched!*

---

## 5. Access Modifiers Summary

| Access Modifier | Same Class | Same Package | Subclass (Diff Package) | World (Everywhere) |
|---|:---:|:---:|:---:|:---:|
| **`private`** | Yes | No | No | No |
| **Default** (no keyword) | Yes | Yes | No | No |
| **`protected`** | Yes | Yes | Yes | No |
| **`public`** | Yes | Yes | Yes | Yes |

---

## 6. Worked Examples

### Worked Example: Runtime Polymorphism (Dynamic Method Dispatch)

```java
class Animal {
    void eat() { System.out.println("Animal eats food"); }
}
class Deer extends Animal {
    @Override
    void eat() { System.out.println("Deer eats grass"); }
}
```

- **Execution Trace:**
  ```java
  Animal myPet = new Deer(); // Parent reference, Child object
  myPet.eat(); // Calls Deer's overridden method!
  ```
- **Output:** `"Deer eats grass"`
- **Why?** In Java, method calls on objects are resolved at **runtime** based on the actual object instance in Heap memory (`Deer`), not the reference variable type (`Animal`).

---

## 7. Java Implementation Concepts

- **Constructor Rules:**
  - Constructors have the EXACT same name as the class.
  - They have NO return type (not even `void`).
  - If no constructor is written, Java automatically inserts a default no-argument constructor.
- **`this` Keyword:** Reference variable referring to the current object instance (resolves field vs parameter name ambiguity `this.name = name`).
- **`super` Keyword:** Refers to the immediate parent class instance (invokes parent constructors `super()`, methods `super.eat()`).

---

## 8. Problem-Solving Patterns

### Pattern 1: Encapsulated Data Transfer Object (DTO)
- **When to think of it:** Building clean modular classes.
- **Mental Approach:** Mark fields `private`. Provide public getters and setters with validation logic (e.g. `setAge(int age)` throws error if `age < 0`).

### Pattern 2: Deep Copy Constructor for Mutable Fields
- **When to think of it:** Classes containing primitive arrays or mutable sub-objects.
- **Mental Approach:** Inside copy constructor, allocate new array space (`this.arr = new int[other.arr.length]`) and copy elements manually.

---

## 9. Algorithms / Structural Designs

### Deep Copy Constructor Template
```java
public Student(Student s) {
    this.name = s.name; // Primitives & Immutable Strings copy directly
    this.marks = new int[s.marks.length]; // Deep copy array allocation
    for (int i = 0; i < marks.length; i++) {
        this.marks[i] = s.marks[i];
    }
}
```

---

## 10. Complexity Reference

| Operation | Time Complexity | Space Complexity |
|---|---|---|
| Object Instantiation (`new`) | $O(1)$ | $O(\text{object size})$ Heap |
| Getter / Setter Call | $O(1)$ | $O(1)$ |
| Shallow Copy Constructor | $O(1)$ | $O(1)$ |
| Deep Copy Constructor | $O(N)$ ($N = \text{array size}$) | $O(N)$ Heap |
| Dynamic Method Dispatch | $O(1)$ (vtable lookup) | $O(1)$ |

---

## 11. Common Mistakes

- **Accidental Shallow Copy Bug:** Assigning arrays directly in constructors (`this.arr = arr`). Changes to external array modify object state!
- **NullPointerException:** Attempting to call methods on an uninitialized object variable (`Student s; s.getName();`).
- **Overriding Signature Mismatch:** Misspelling method name or parameter types when overriding parent method. ALWAYS use `@Override` annotation so compiler flags typos.

---

## 12. Edge Cases

- **Null References in Parameters.**
- **Circular Object References:** Object A holding reference to B, and B holding reference to A.
- **Destructors in Java:** Java does NOT have explicit destructors like C++. Garbage Collection manages heap memory automatically. `finalize()` is deprecated in modern Java.

---

## 13. Interview Questions

### Beginner
1. What are the four main pillars of Object-Oriented Programming?
2. Differentiate between a Class and an Object in Java.

### Intermediate
1. Explain the difference between Shallow Copy and Deep Copy with a code snippet.
2. Differentiate between Method Overloading (Compile-time) and Method Overriding (Runtime).

### Advanced
1. How does Java resolve method calls dynamically at runtime using Virtual Method Tables (Vtables)?
2. Why does Java not support Multiple Inheritance with classes, and how do Interfaces solve the Diamond Problem?

---

## 14. Real-World Applications

- **Enterprise Backend Architectures (Spring Boot):** Layered architecture using Services, Controllers, and DTO objects.
- **GUI Libraries (Swing / JavaFX):** UI component inheritance hierarchy (`Component` $\to$ `Button`).
- **Database ORM (Hibernate):** Mapping SQL database tables directly to Java OOP domain entities.

---

## 15. Repository Implementations

| File | Concept Demonstrated |
|---|---|
| [`AccessModifiersDemo.java`](AccessModifiersDemo.java) | Scope demonstration of `private`, `default`, `protected`, and `public` access modifiers. |
| [`ClassesAndObjectsDemo.java`](ClassesAndObjectsDemo.java) | Class definition, object creation using `new`, state, and behavior methods. |
| [`ConstructorsDemo.java`](ConstructorsDemo.java) | Constructor invocation during object instantiation. |
| [`ConstructorTypes.java`](ConstructorTypes.java) | Default, parameterized, and overloaded constructors. |
| [`CopyConstructorShallow.java`](CopyConstructorShallow.java) | Demonstration of Shallow Copy and unwanted shared reference mutations. |
| [`CopyConstructorDeep.java`](CopyConstructorDeep.java) | Deep Copy constructor allocating independent array memory. |
| [`EncapsulationDemo.java`](EncapsulationDemo.java) | Data hiding by restricting field access and providing controlled methods. |
| [`GarbageCollectionDestructors.java`](GarbageCollectionDestructors.java) | Automatic garbage collection concepts and memory deallocation. |
| [`GettersAndSetters.java`](GettersAndSetters.java) | Controlled access to private class variables using `get` and `set` methods. |
| [`HierarchicalInheritance.java`](HierarchicalInheritance.java) | Hierarchical class structure where multiple sub-classes inherit from one super-class. |
| [`InheritanceTypes.java`](InheritanceTypes.java) | Single, Multilevel, and Derived class inheritance patterns using `extends`. |
| [`MethodOverloadingDemo.java`](MethodOverloadingDemo.java) | Compile-time polymorphism overloading methods by parameter count/types. |
| [`MethodOverridingDemo.java`](MethodOverridingDemo.java) | Runtime polymorphism overriding parent class methods in child classes using `@Override`. |

---

## 16. Related Topics

### Prerequisites
- Java Basics, Functions, and Memory References.

### Related Topics
- Abstract Classes & Interfaces.
- Java Collections Framework.

### Next Topics
- Recursion & Call Stack Frames.
- Custom Data Structure Design (Linked List, Stacks, Queues).

---

# 🖼️ 17. Visual Notes / Personal Images

<!--
PERSONAL IMAGE:
Add diagram for OOP Inheritance Hierarchy or Shallow vs Deep Copy heap memory layout.
Example: Place personal image inside images/ directory when ready.
-->

*No visual notes uploaded yet. Place personal diagram files inside a local `images/` directory when ready.*
