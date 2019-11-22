A big advantage of C++ is that you have a lot of control on your compile-time, i.e. you can automate a lot of writing code
which would be translated to copying and pasting a lot of code in other languages, or sometimes making a lot of `if` conditions
or other runtime tricks in other languages.

# e.g.

Java's `List` interface, and `ArrayList` class. Any addition, or getting from it, or any other operation would have *runtime*
type checking, which means something like that (happens at *runtime*):

```java

// pseudocode

public void add(Object obj) {
    if (!(obj instanceof T)) {
        throw ClassCastException("Not the required type");
    }
    // do the adding here
}

public Object get(int index) {
    Object obj = // do the getting here
    if (!(obj instanceof T)) {
        throw ClassCastException("Invalid data inside the array");
    }
}

```

Notes:
  - Runtime type checking is added, which causes an overhead
  - Exceptions are postponed to runtime, a lot of errors could be detected prior than that at compile-time
  - `Object` in Java is a reference type, this means there is an extra "dereference" operation
  
# What about C++ ??

C++ supports *template metaprogramming*, (metaprogramming means "programming the programming language itself"). This executes
some commands at compile-time, which *generates* code.

```cpp

template<typename T>
class vector {
    public:
        // An object "by reference"
        void add(T &obj) {
            // add the object here, without type checking, it is already compile-time checked
            // passing an object with another type would make a "compile-time error", you wouldn't wait
            // until run
        }

        T &at(int i) {
            // Directly return the reference to the object, no runtime type checking needed
        }
};

```

# What happens?

Any file that defines this, doesn't generate any compilable code (this doesn't generate any binary). But:

```cpp

vector<int> vec1;
vector<std::string> vec2;

```

Would generate ***two classes***, one is with *substituting* `T` for an `int`, and the other is with substituting it with a `std::string`.

In Java, `ArrayList<Integer>` and `ArrayList<String>` are compatible with each other at compile-time, they are both `ArrayList`.

In Java, you can *cast* an `ArrayList<Integer>` into an `ArrayList<String>` with just a warning.

In Java you maybe tempted into doing that:

```java
ArrayList<Integer> arr = new ArrayList<>();
// A warning here, but not an error
ArrayList unchecked = arr;
unchecked.add("A string here");
// A ClassCastException is thrown
arr.get(0);
```

In C++:

```cpp
vector<int> arr;
// INVALID: A compile-time error because a template requires an argument
// vector other;

// INVALID: A compile-time error because vector<int>, vector<std::string> are completely different classes!!
// vector<std::string> arr2 = arr;
```

This is safer and more performant, but, sometimes it is felt as a hinderance, but I think it worth it.

# Well, what if I *need* runtime types??

Sometimes, you may need runtime types, what if for example you want a `std::vector` that is *heterogenous*, i.e. contain more
than one type, for example a vector that may contain both strings and ints (unlikely to be needed in a real situation).

This has a lot of solutions, one of them would be *wrapping your object in a type that allows runtime type*, like [`std::variant`](https://en.cppreference.com/w/cpp/utility/variant),
or [std::any](https://en.cppreference.com/w/cpp/utility/any). More of this would be covered in the [Value types](https://github.com/fadi-botros/cplus-plus-from-modern-languages/blob/master/compile-time-vs-runtime.md) section.

# Example code:

(You would need to familiarize yourself to [Compiler explorer](https://godbolt.org/), it is a great tool when trying to optimize your code and want to see what your code compile into,
it is not only for C++, but also for C, GoLang, Swift, Rust and other languages)

 - [Example using `std::any`](https://godbolt.org/z/EE-CbY)
 - [Example using `std::variant`](https://godbolt.org/z/K5GKjc)

So, what is the difference between `std::any` and `std::variant`??

More would be covered in the [Value types](https://github.com/fadi-botros/cplus-plus-from-modern-languages/blob/master/compile-time-vs-runtime.md) section. But at least `std::variant` has known possible types, so its "size in memory"
could be known, so it could be wrapped in a value type, which is faster. While `std::any` can contain anything, so no size is known.

[Sizes](https://godbolt.org/z/4KWubZ)

Notice: The `std::variant`, has the size of the biggest type, plus a "type indicator" which is about 8 bytes.
While `std::any` has only 16 bytes (which is a "type indicator", and a reference/pointer), so `std::any` is a reference type.
