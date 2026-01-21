[![Github releases](https://img.shields.io/github/release/Neargye/nameof.svg)](https://github.com/Neargye/nameof/releases)
[![Conan package](https://img.shields.io/badge/Conan-package-blueviolet)](https://conan.io/center/recipes/nameof)
[![Vcpkg package](https://img.shields.io/badge/Vcpkg-package-blueviolet)](https://github.com/microsoft/vcpkg/tree/master/ports/nameof)
[![License](https://img.shields.io/github/license/Neargye/nameof.svg)](LICENSE)
[![Compiler explorer](https://img.shields.io/badge/compiler_explorer-online-blue.svg)](https://godbolt.org/z/s_ecko)
[![Stand With Ukraine](https://raw.githubusercontent.com/vshymanskyy/StandWithUkraine/main/badges/StandWithUkraine.svg)](https://stand-with-ukraine.pp.ua)

# Nameof C++

Header-only C++17 library provides nameof macros and functions to simply obtain the name of a variable, type, function, macro, and enum.

If you like this project, please consider donating to one of the funds that help victims of the war in Ukraine: https://u24.gov.ua.

## Documentation

* [Reference](https://github.com/Neargye/nameof/blob/master/doc/reference.md)
* [Limitations](https://github.com/Neargye/nameof/blob/master/doc/limitations.md)
* [Installation](#Installation)

## [Features & Examples](https://github.com/Neargye/nameof/blob/master/example/example.cpp)

* Nameof

  ```cpp
  // Name of variable.
  NAMEOF(somevar) -> "somevar"

  // Name of member variable.
  NAMEOF(person.address.zip_code) -> "zip_code"

  // Name of function.
  NAMEOF(foo<int, float>()) -> "foo"

  // Name of member function.
  NAMEOF(somevar.some_method()) -> "some_method"
  NAMEOF(somevar.some_method<int>()) -> "some_method"

  // Name of macro.
  NAMEOF(__LINE__) -> "__LINE__"
  NAMEOF(NAMEOF(structvar)) -> "NAMEOF"

  // Obtains full name of variable, function, macro.
  NAMEOF_FULL(somevar.some_method<int>()) -> "some_method<int>"

  // Obtains raw name of variable, function, macro.
  NAMEOF_RAW(somevar.some_method<int>()) -> "somevar.some_method<int>()"
  ```

* Nameof enum

  ```cpp
  enum class Color { RED = 1, BLUE = 2, GREEN = 4 };

  auto color = Color::RED;
  // Name of enum variable.
  NAMEOF_ENUM(color) -> "RED"
  nameof::nameof_enum(color) -> "RED"

  // Static storage enum variable to string.
  // This version is much lighter on the compile times and is not restricted to the enum_range limitation.
  NAMEOF_ENUM_CONST(Color::GREEN) -> "GREEN"
  nameof::nameof_enum<Color::GREEN>() -> "GREEN"

  // Enum flags variable to string.
  NAMEOF_ENUM_FLAG(Color::GREEN | Color::BLUE) -> "GREEN|BLUE"
  nameof::nameof_enum_flag<Color::GREEN | Color::BLUE>() -> "GREEN|BLUE"

  // Obtains name of enum variable or default value if enum variable out of range.
  NAMEOF_ENUM_OR(Color::GREEN) -> "GREEN"
  NAMEOF_ENUM_OR((Color)0, "none") -> "none"
  ```

* Nameof type

  ```cpp
  const my::detail::SomeClass<int>& var_ref = var;
  // Name of variable type.
  NAMEOF_TYPE_EXPR(var_ref) -> "my::detail::SomeClass<int>"
  nameof::nameof_type<decltype(var_ref)>() -> "my::detail::SomeClass<int>"
  NAMEOF_FULL_TYPE_EXPR(var_ref) -> "const my::detail::SomeClass<int>&"
  nameof::nameof_full_type<decltype(var_ref)>() -> "const my::detail::SomeClass<int>&"
  NAMEOF_SHORT_TYPE_EXPR(var_ref) -> "SomeClass"
  nameof::nameof_short_type<decltype(var_ref)>() -> "SomeClass"

  using T = const my::detail::SomeClass<int>&;
  // Name of type.
  NAMEOF_TYPE(T) ->"my::detail::SomeClass<int>"
  nameof::nameof_type<T>() -> "my::detail::SomeClass<int>"
  NAMEOF_FULL_TYPE(T) -> "const my::detail::SomeClass<int>&"
  nameof::nameof_full_type<T>() -> "const my::detail::SomeClass<int>&"
  NAMEOF_SHORT_TYPE(T) -> "SomeClass"
  nameof::nameof_short_type<T>() -> "SomeClass"

  my::detail::Base* ptr = new my::detail::Derived();
  // Name of type, using rtti.
  NAMEOF_TYPE_RTTI(*ptr) -> "my::detail::Derived"
  NAMEOF_FULL_TYPE_RTTI(*ptr) -> "volatile const my::detail::Derived&"
  NAMEOF_SHORT_TYPE_RTTI(*ptr) -> "Derived"

  struct A {
    int this_is_the_name;
  };
  // Obtains name of member.
  NAMEOF_MEMBER(&A::this_is_the_name) -> "this_is_the_name"
  nameof::nameof_member(&A::this_is_the_name) -> "this_is_the_name"

  int someglobalvariable = 0;
  // Obtains name of a function, a global or class static variable.
  NAMEOF_POINTER(&someglobalconstvariable) == "someglobalconstvariable"
  nameof::nameof_pointer(&someglobalconstvariable) == "someglobalconstvariable"

  constexpr auto global_ptr = &someglobalvariable;
  NAMEOF_POINTER(global_ptr) == "someglobalconstvariable"
  nameof::nameof_pointer(global_ptr) == "someglobalconstvariable"
  ```

## Remarks

* Before use, read the [limitations](https://github.com/Neargye/nameof/blob/master/doc/limitations.md) of functionality.

## Integration

Run:

```bash
$ npm i nameof.cxx
```

And then include `nameof.hpp` as follows:

```cxx
// main.cxx
#include <nameof.hpp>

int main() { /* ... */ }
```

Finally, compile while adding the path `node_modules/nameof.cxx` to your compiler's include paths.

```bash
$ clang++ -std=c++20 -I./node_modules/nameof.cxx main.cxx  # or, use g++
$ g++     -std=c++20 -I./node_modules/nameof.cxx main.cxx
```

You may also use a simpler approach with the [cpoach](https://www.npmjs.com/package/cpoach.sh) tool, which automatically adds the necessary include paths of all the installed dependencies for your project.

```bash
$ cpoach clang++ -std=c++20 main.cxx  # or, use g++
$ cpoach g++     -std=c++20 main.cxx
```

---

Alternatively, you should add required file [nameof.hpp](nameof.hpp).

If you are using [vcpkg](https://github.com/Microsoft/vcpkg/) on your project for external dependencies, then you can use the [nameof package](https://github.com/microsoft/vcpkg/tree/master/ports/nameof).

If you are using [Conan](https://www.conan.io/) to manage your dependencies, merely add `nameof/x.y.z` to your conan's requires, where `x.y.z` is the release version you want to use.

Archlinux users can install `nameof` by package manager AUR, using the following command: `yay -S nameof`.

Alternatively, you can use something like [CPM](https://github.com/TheLartians/CPM) which is based on CMake's `Fetch_Content` module.

```cmake
CPMAddPackage(
    NAME nameof
    GITHUB_REPOSITORY Neargye/nameof
    GIT_TAG x.y.z # Where `x.y.z` is the release version you want to use.
)
```

## Compiler compatibility

Check in the [reference](https://github.com/Neargye/nameof/blob/master/doc/reference.md) for each features.

## Licensed under the [MIT License](LICENSE)

<br>
<br>


[![](https://raw.githubusercontent.com/qb40/designs/gh-pages/0/image/11.png)](https://wolfram77.github.io)<br>
[![SRC](https://img.shields.io/badge/src-repo-green?logo=Org)](https://github.com/Neargye/nameof)
[![ORG](https://img.shields.io/badge/org-nodef-green?logo=Org)](https://nodef.github.io)
![](https://ga-beacon.deno.dev/G-RC63DPBH3P:SH3Eq-NoQ9mwgYeHWxu7cw/github.com/nodef/nameof.cxx)
