# Lecture 2: Java Review

## Language Performance Ranking

From higher to lower raw performance:

1. **Assembly** — direct machine-level instructions
2. **C** — native compiled code, low overhead
3. **C++** — native compiled code with powerful abstractions
4. **Java** — runs on the JVM with JIT optimization
5. **Python** — dynamic execution, usually more overhead

This is only a generalization. Algorithm choice, compiler/JIT, libraries, hardware, and code quality can all change the result.

## Java Data Types

### Primitive Types

Primitive types store the actual value directly in memory. Java has 8 of them.

| primitive | size | example |
|---|---|---|
| byte | 1 byte | `byte age = 25;` |
| short | 2 bytes | `short year = 2025;` |
| int | 4 bytes | `int count = 100;` |
| long | 8 bytes | `long population = 8000000000L;` |
| float | 4 bytes | `float price = 19.99f;` |
| double | 8 bytes | `double pi = 3.14159;` |
| char | 2 bytes | `char grade = 'A';` |
| boolean | 1 bit | `boolean passed = true;` |

### Non-Primitive (Reference) Types

Reference types store a reference (a memory address) that points to an object, rather than the value itself.

| type | example |
|---|---|
| String | `"Hello"` |
| Arrays | `int[] numbers` |
| Classes | `Student` |
| Objects | `new Student()` |
| Interfaces | `List<String>` |

### Wrapper Classes

A wrapper class wraps a primitive value inside an object, which lets primitives be used in places where an object is required.

| primitive | wrapper |
|---|---|
| byte | Byte |
| short | Short |
| int | Integer |
| long | Long |
| float | Float |
| double | Double |
| char | Character |
| boolean | Boolean |

## ADT, Interface, and Implementation

An ADT (abstract data type) describes **what** a data structure can do, without specifying how it is implemented internally. The ADT is the behavior and the operations, so it does not say whether a list uses an array or linked nodes.

**ADT = abstraction → Interface = contract → Class = implementation → Object = runtime instance**

| layer | question it answers |
|---|---|
| ADT | what behavior? (conceptual specification) |
| Interface | what methods exist? (the Java contract) |
| Class | how is it done? (data structure + algorithm) |
| Object | the actual instance in memory |

<img width="489" height="272" alt="Screenshot 2026-09-24 at 4 33 02 PM" src="https://github.com/user-attachments/assets/826f18af-34db-419a-bee6-c19b84c230b2" />

### The List ADT

`ArrayList<E>` and `LinkedList<E>` both implement the same `List<E>` interface. They offer the same public operations but use different internal data structures, which gives them different performance.

- **`ArrayList<E>`** uses a dynamic array, so `get(i)` is direct indexing.
- **`LinkedList<E>`** uses doubly linked nodes, so `get(i)` has to traverse the nodes.

## Java List Characteristics

- A list is an **ordered collection**, meaning elements are stored and iterated in insertion order.
- The list structure and its methods are **independent of the type** of objects stored (integers, strings, records, and so on).
- Lists **allow duplicate elements**.
- Lists support **index-based access**.
- Lists have a **dynamic size** and can grow and shrink.

**List capabilities:** create, retrieve, insert (by location), delete (by value or location), and size.

### Common List Methods

```java
list.add("Java");
list.get(0);
list.set(0, "Python");
list.remove(0);
list.contains("Java");
list.size();
list.clear();
```

### Insertion and Deletion by Value vs. Location

```java
List<String> fruits = new ArrayList<>();
fruits.add("Apple");         // insert at end
fruits.add(0, "Banana");     // insert by location/index
fruits.remove(0);            // delete by location/index
fruits.remove("Apple");      // delete by value
```

### Declaration vs. Creation

```java
List<String> fruits;
```

This line only **declares** a variable that can refer to a List of Strings. No List object exists yet, and the reference is currently `null`. The variable lives on the stack.

```java
List<String> fruits = new ArrayList<>();
```

Now an actual ArrayList object is created on the heap. Breaking down the pieces:

- `List` — the interface, the list abstraction
- `String` — the type of elements the list can contain
- `fruits` — the reference variable holding a reference to the object
- `new ArrayList<>()` — creates the ArrayList object

## ArrayList

```java
List<String> letters = new ArrayList<>();
letters.add("A");
letters.add("B");
letters.add("C");
letters.add("D");
System.out.println(letters.get(2));  // C
```

Elements are stored as references in an internal array and accessed by index.

- Maintains insertion order and allows duplicates.
- Fast random access: `get(i)` is o(1).
- Add at end is o(1).
- Insert or delete in the middle is o(n).
- Lower memory overhead than LinkedList.

### How ArrayList Grows

ArrayList uses a dynamic internal array that expands automatically by about 50% when more capacity is needed.

1. **Create:** `new ArrayList<>()` starts with capacity 0 and size 0.
2. **First add:** capacity jumps to 10, size becomes 1.
3. **Array becomes full:** more space is needed.
4. **Resize:** allocate a larger array and copy the existing elements over.

Typical capacity growth: 0 → 10 → 15 → 22 → 33 → 49 → 73

So when `letters.add("K")` runs on a full array of A–J, there is no free slot, so Java allocates a larger array, copies A–J into it, and then adds K. Capacity tells you how many references the internal array can currently hold, which is different from size.

Resizing is costly because every existing element has to be copied.

### Amortized o(1)

**Amortized analysis** means spreading the cost of occasional expensive operations over a long sequence of operations. It does **not** mean that every individual `add()` is o(1).

- Most `add()` operations are o(1).
- An occasional resize is o(n).
- Over many appends, the total work grows linearly with n, giving o(n) total work.
- o(n) total work divided by n appends = **amortized o(1) per append**.

So ArrayList append is **worst case o(n), amortized o(1)**.

## LinkedList

```java
import java.util.*;
List<String> fruits = new LinkedList<>();

fruits.add("Apple");
fruits.add("Orange");
fruits.add("Banana");
fruits.add("Orange");

System.out.println(fruits.get(1));  // [Orange]
System.out.println(fruits);         // [Apple, Orange, Banana, Orange]
```

A LinkedList stores nodes connected by references, in the pattern `prev ↔ data ↔ next`. Java's LinkedList is **doubly linked**.

- Each element is stored in its own node, and each node holds the data plus references to the previous and next nodes.
- The list keeps a reference to both the **head** and the **tail**.
- Nodes may be stored in random memory locations, unlike an array's contiguous block.
- Maintains insertion order and allows duplicates.
- Fast insert/delete at the beginning or end: o(1).
- Insert/delete in the middle: o(n), because it requires traversal.
- `get(i)` and `set(i)`: o(n), also traversal.
- Higher memory overhead, since every node stores next/prev references.

## Same Interface, Different Performance

The interface tells you which operations exist, but the implementation determines their cost.

| operation | ArrayList | LinkedList |
|---|---|---|
| `get(i)` | o(1) | o(n), requires traversal |
| `add()` at end | amortized o(1) | o(1), adds a tail node |
| `add()` at beginning | o(n), requires shifting n items | o(1), adds a head node |
| add/remove in middle | o(n), requires shifting n items | o(n), requires traversal |
| memory overhead | lower, no next/prev references | higher, stores next/prev per node |

**Choose ArrayList** for fast index-based access. **Choose LinkedList** for frequent insertions and deletions at the ends.

## Lists and Primitive Types

A Java List stores **objects**, not primitives, so these are not allowed:

```java
List<int> numbers = new ArrayList<>();      // not allowed
List<double> prices = new ArrayList<>();    // not allowed
List<char> letters = new ArrayList<>();     // not allowed
```

Use the wrapper classes instead:

```java
List<Integer> numbers = new ArrayList<>();
numbers.add(10);
```

### One Interface, Many Data Types

A List can store objects of any type, and the methods and behavior stay the same. Only the element type changes.

```java
List<Integer> numbers = new ArrayList<>();      // [10] [20] [30]
List<String> fruits = new ArrayList<>();        // ["Apple"] ["Orange"] ["Banana"]
List<Student> students = new LinkedList<>();    // [Alice] [Bob] [Carol]
List<Employee> employees = new LinkedList<>();  // [John] [Mary]
```

## Iterator

An `Iterator<E>` is a common way to walk through a collection one element at a time. It works like a bookmark or cursor that moves forward through the collection.

```java
List<String> fruits = new ArrayList<>();
Iterator<String> it = fruits.iterator();
while (it.hasNext()) {
    System.out.println(it.next());
}
```

**The three methods:**

- `hasNext()` asks whether there is another element and returns true or false.
- `next()` returns the next element and moves the iterator forward.
- `remove()` removes the last element returned by `next()`, which modifies the collection.

**Key points:**

- Iterator is an **interface**, so it cannot be instantiated directly. It has no instance variables or constructors.
- It preserves the abstraction of the Collection interface. Whether the collection is an ArrayList, LinkedList, or HashSet, the basic iterator code stays the same.
- It moves in one direction only: forward.

Iterators work across List (ArrayList, LinkedList), Set (HashSet, TreeSet), and Queue (PriorityQueue, ArrayDeque).

### Iterator vs. ListIterator

`Iterator` is for general collection traversal, while `ListIterator` adds List-specific capabilities.

| Iterator&lt;E&gt; | ListIterator&lt;E&gt; |
|---|---|
| `next()` | `next()` |
| `hasNext()` | `hasNext()` |
| `remove()` | `remove()` |
| | `previous()` |
| | `add()` |
| | `set()` |

`Iterator` is used with many collections. `ListIterator` is used specifically with Lists.
