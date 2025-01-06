# Generics in Java

Generics in Java provide a way to write code that can work with different types of objects while ensuring type safety. They were introduced in Java 5 to address issues of type casting and to allow code reusability for different types.

---

## **Key Concepts of Generics**

1. **Type Parameterization**: Generics allow you to define a class, interface, or method with type parameters, which can be replaced with specific types when the code is used.
2. **Type Safety**: Generics help catch type-related errors at compile time, reducing runtime errors.
3. **Code Reusability**: With generics, you can write a single method or class that works with different data types.

---

## **Basic Syntax**

The generic type is defined using angle brackets `<>`.

### **1. Generic Class**

```java
public class Box<T> { // T is a type parameter
    private T content;

    public void set(T content) {
        this.content = content;
    }

    public T get() {
        return content;
    }
}

public class Main {
    public static void main(String[] args) {
        Box<String> stringBox = new Box<>();
        stringBox.set("Hello");
        System.out.println(stringBox.get());

        Box<Integer> intBox = new Box<>();
        intBox.set(42);
        System.out.println(intBox.get());
    }
}
```

- **`<T>`** is a placeholder for a type, replaced with a real type (e.g., `String` or `Integer`) when you use the class.
- You can create multiple instances of `Box` with different types.

---

### **2. Generic Method**

```java
public class Main {
    public static <T> void printArray(T[] array) { // T is a type parameter
        for (T element : array) {
            System.out.println(element);
        }
    }

    public static void main(String[] args) {
        String[] stringArray = {"Hello", "World"};
        Integer[] intArray = {1, 2, 3};

        printArray(stringArray);
        printArray(intArray);
    }
}
```

- **`<T>`** before the return type indicates that this is a generic method.
- The method can accept arrays of any type.

---

### **3. Generic Interface**

```java
public interface Pair<K, V> { // K and V are type parameters
    K getKey();
    V getValue();
}

public class KeyValuePair<K, V> implements Pair<K, V> {
    private K key;
    private V value;

    public KeyValuePair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey() {
        return key;
    }

    public V getValue() {
        return value;
    }
}

public class Main {
    public static void main(String[] args) {
        Pair<String, Integer> pair = new KeyValuePair<>("Age", 30);
        System.out.println(pair.getKey() + ": " + pair.getValue());
    }
}
```

---

## **Common Use Cases**

### **1. Collections Framework**

Classes like `ArrayList`, `HashMap`, and `HashSet` use generics.

```java
ArrayList<String> list = new ArrayList<>();
list.add("Apple");
list.add("Banana");

for (String fruit : list) {
    System.out.println(fruit);
}
```

### **2. Avoiding Type Casting**

Without generics:

```java
ArrayList list = new ArrayList();
list.add("Hello");
String value = (String) list.get(0); // Manual casting
```

With generics:

```java
ArrayList<String> list = new ArrayList<>();
list.add("Hello");
String value = list.get(0); // No casting needed
```

---

## **Benefits of Generics**

1. **Type Safety**: Ensures that you don’t accidentally store the wrong type in a collection or variable.
2. **Eliminates Type Casting**: Reduces boilerplate code for type casting.
3. **Reusability**: Allows creating generic algorithms and data structures.

---

## **Wildcard in Generics**

The wildcard `?` is used when the exact type is unknown or irrelevant.

### **1. Unbounded Wildcard**: `<?>`

```java
public void printList(List<?> list) {
    for (Object obj : list) {
        System.out.println(obj);
    }
}
```

### **2. Bounded Wildcard**

- **Upper Bound**: `<? extends Number>` (accepts `Number` or any subclass like `Integer` or `Double`).
- **Lower Bound**: `<? super Integer>` (accepts `Integer` or any superclass like `Number` or `Object`).

---

# Comparing `List<T>` and `List<?>`

| Feature                | `List<T>`                          | `List<?>`                      |
| ---------------------- | ---------------------------------- | ------------------------------ |
| **Type**               | Fixed type (`T`) when declared     | Unknown type                   |
| **Can Add Elements?**  | Yes, elements of type `T`          | No, except `null`              |
| **Can Read Elements?** | Yes, as type `T`                   | Yes, but as `Object`           |
| **Use Case**           | When you know the type of the list | When you don’t care about type |

---
