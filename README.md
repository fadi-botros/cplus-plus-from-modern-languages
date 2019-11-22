# cplus-plus-from-modern-languages (WIP)
A guide for how to begin learning Modern C++ when coming from a modern language

# Why C++?
You may be coming from Java, C#, JavaScript, Elixir, Erlang, Haskell, GoLang, etc...

 - Java and C# have a VM and a GC, which are a performance hinderance, also they consume a lot of memory.
 - JavaScript is an extremely dynamic language, which doesn't have a lot of IDE help and also it is a scripting language, and so it is still performance hindered.
 - GoLang also has a GC, so it may consume a lot of memory (while it performs well, and in reality it doesn't consume a lot of memory), also GoLang is a "cryptic" language which is hard to read and write.
 - Functional languages like Elixir, Erlang, Haskell, etc... makes your code 99.9999999% reliable but inefficient because functional programming needs a lot of memory, also FP needs GC.
 - C, the root of most problems and vulnerabilities in the software world, is very efficient and performant, but it is an unproductive language and is very error prone because it is a fully manual language.
 
## C++ is not better than all of those, but:
 
 1. There is a "deterministic GC" (called **[RAII](https://en.cppreference.com/w/cpp/language/raii)**, which stands for *Resource Acquistion Is Initialization*) instead of a "stop-the-world" GC (like Java, C#, GoLang, JS, etc...). This is safer than C's fully manual memory and resource management, and more efficent than Java, C#, etc... (it is also sometimes safer than Java, C#, etc... when handling external resources, like files).
 2. Sometimes manual fine tuning is needed, this can't be done in other languages.
 3. There is no VM, so the total binary size would be smaller (in Java, you should count also the binary size of your JVM implementation itself).
 4. It is not owned by some company, so it is unlikely to move to propreitary at least for a long time.
 
# Enough introduction, let's start:

 - Starting from C# (you already know value types (which is `struct` in C#, and you know template specialization)):
   * [Compile-time vs. run-time](https://github.com/fadi-botros/cplus-plus-from-modern-languages/blob/master/compile-time-vs-runtime.md)
   * References and memory management (WIP)
   * RAII (WIP)
 
 - Starting from Java:
   * [Compile-time vs. run-time](https://github.com/fadi-botros/cplus-plus-from-modern-languages/blob/master/compile-time-vs-runtime.md)
   * [Value types](https://github.com/fadi-botros/cplus-plus-from-modern-languages/blob/master/compile-time-vs-runtime.md)
   * [RAII](https://en.cppreference.com/w/cpp/language/raii) (from the official documentation)
   * References and memory management (WIP)
   * Templates (WIP)
   * Functional Programming (WIP)
   
 - Starting from JavaScript:
   * [Compile-time vs. run-time](https://github.com/fadi-botros/cplus-plus-from-modern-languages/blob/master/compile-time-vs-runtime.md)
   * Static vs. dynamic types (WIP)
   * [RAII](https://en.cppreference.com/w/cpp/language/raii) (from the official documentation)
   * [Value types](https://github.com/fadi-botros/cplus-plus-from-modern-languages/blob/master/compile-time-vs-runtime.md)
   * References and memory management (WIP)
   * Templates (WIP)

 - Starting from Functional Programming languages:
   * OOP concepts (Encapsulation, Polymorphism) *Inheritance wouldn't be covered because it is undesirable and error prone* (WIP)
   * [RAII](https://en.cppreference.com/w/cpp/language/raii) (from the official documentation)
   * References and memory management (WIP)


 # Common OOP design patterns used in C++:
 
  - Factory:
    Sometimes making a constructor wouldn't be valid for your purpose, so a static factory method would be used, also sometimes it is done for convenience because a constructor with a big parameter list would be used.
    
  - Adapter:
    Sometimes you have a class which have a lot of functions that aren't overridable, and you want it to be overridable so that you don't know it at compile-time but you would call it at run-time.
    
