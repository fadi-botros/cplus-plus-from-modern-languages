The main power of Swift, C# and C++, the power to make a *Value Type*.

 # What is a value type:
 
 A value type is a type that is put in place in memory.
 
 # e.g.
 
 ```cplusplus
 class a_value_type {
     double a;
     double b;
     double c;
 };
 ```
 
 ```
 [a 8 bytes][b 8 bytes][c 8 double]
 ```
 
 ```cplusplus
 class a_complex_value_type {
     double a;
     a_value_type b;
 };
 ```
 
 ```
 [a 8 bytes][b.a 8 bytes][b.b 8 bytes][b.c 8 double]
 ```
 
 **Notice:** You would see that a value of type: `a_value_type` is *embedded* in the `a_complex_value_type`.

 What happens if there is a reference type???
 
 If we imagine using a reference type inside `a_complex_value_type`:
 
 ```cplusplus
 class a_complex_value_type {
     double a;
     a_value_type &b;
 };
 ```
 
 The memory layout would be something like that:
 
 ```
 [a 8 bytes][b 16 bytes]
 ```
 
 `b` would be just a reference (i.e. an address in memory, that just points to a variable with type `a_value_type`)
 
 You can imagine it like:
 
 ```
 [a 8 bytes][b 16 bytes]

 ...
 
 [b.a 8 bytes][b.b 8 bytes][b.c 8 double]
 ```
 
 Where `b` points to the `[b.a 8 bytes][b.b 8 bytes][b.c 8 double]` structure.
 
 i.e. if the `[b.a 8 bytes][b.b 8 bytes][b.c 8 double]` is in memory address `0x12345678`, so the values would be:

 ```
 [a 8 bytes][b = 0x12345678]
 ```

 
# What if you need a reference type?

In C++, this is complicated. Because there is a lot of reference types, it is not like Java/C#/JS (in which all references are in GC).
  - A reference can be used (like the example above).
  - An r-value reference can be used.
  - A pointer can be used.
  - A smart pointer can be used.
  
This would be in more details in References and memory management section (WIP).

**********************************************************
***NOTE:*** The above techniques except the smart pointers are ***DANGEROUS***, especially the pointer, they should be used with caution.

WHY???

Imagine a thing happen like that:

```cplusplus
class a_value_type {
public:
    double a;
    double b;
    double c;
};
```

```cplusplus
// Returns an object by reference
a_value_type &return_a_reference() {
    a_value_type ret;
    ret.a = 1.0;
    ret.b = 1.0;
    ret.c = 1.0;
    return ret; // Returns a reference to ret
}

int main() {
    a_value_type &ref = return_a_reference();
    
    // using ref.a, or ref.b, or ref.c is undefined, it could be ANYTHING
}
```

You would notice this object is a temporary object, it is inside a function, this is destroyed when the function exits,
but still you refer to it !!! This is called a *dangling reference*

Imagine some person `a` with an address: `City 1, District 1, Street 1, Appartment 1`. And then this, person have moved to
another appartment, and `b` have bought his former appartment.

While still one knows that `a` dwells in `City 1, District 1, Street 1, Appartment 1`, and he wants to visit him, he would
find `b` instead, because `b` is now the new dweller in that appartment.

Sometimes the buying would be still in progress, so still `a` dwells in the same appartment, but he is in progress to move to
another home. So visiting the same address would still visit `a`, but this is "undefined", because you may visit `a` or anyone
else.
