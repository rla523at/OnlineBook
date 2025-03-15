# inline

[stackoverlofw - what-happens-to-static-variables-in-inline-functions](https://stackoverflow.com/questions/185624/what-happens-to-static-variables-in-inline-functions)  

Specification

* [cppreference - inline](https://en.cppreference.com/w/cpp/language/inline)  
  * **It has the same address** in every translation unit.
  * The definition of an inline function or variable(since C++17) **must be reachable in the translation unit where it is accessed** (not necessarily before the point of access).
  * **Function-local static objects in all function definitions are shared across all translation units** (they all refer to the same object defined in one translation unit).
  * **function defined entirely inside a class/struct/union definition, whether it's a member function or a non-member friend function, is implicitly an inline function** unless it is attached to a named module(since C++20).
