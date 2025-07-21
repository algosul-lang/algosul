# AAU

## Start

```rust
#[start]
fn start_function() {}
```

```rust
use std::prelude::*;
#[start]
fn start_function_return_result() -> Result<(), ()> { Ok(()) }
```

```rust
use std::prelude::*;
#[start]
fn start_function_return_int() -> c_int { 0 }
```

```rust
use std::prelude::*;
#[start]
fn start_function_return_result_int() -> Result<c_int, ()> { Ok(0) }
```

## Basic Type

### Buildin Type

|    type \ bit    |   8    |  16   |   32   |  64   |  128   | bits(32\|64) |
|:----------------:|:------:|:-----:|:------:|:-----:|:------:|:------------:|
|     integer      |  `i8`  | `i16` | `i32`  | `i64` | `i128` |   `isize`    |
| unsigned integer |  `u8`  | `u16` | `u32`  | `u64` | `u128` |   `usize`    |
|   float number   |   \    | `f16` | `f32`  | `f64` | `f128` |      \       |
|       bool       | `bool` |   \   |   \    |   \   |   \    |      \       |
|    character     |   \    |   \   | `char` |   \   |   \    |      \       |
|     pointer      |   \    |   \   |   \    |   \   |   \    |  `*const T`  |
| mutable pointer  |   \    |   \   |   \    |   \   |   \    |   `*mut T`   |
|       ref        |   \    |   \   |   \    |   \   |   \    |     `&T`     |
|   mutable ref    |   \    |   \   |   \    |   \   |   \    |   `&mut T`   |

|     type      |           define           |                            size                             |
|:-------------:|:--------------------------:|:-----------------------------------------------------------:|
| dynamic array |         `arr: [T]`         |                `size_of::<T>() * arr.len()`                 |
| static array  |      `arr: [T; len]`       |                   `size_of::<T>() * len`                    |
|  empty tuple  |        `tuple: ()`         |                             `0`                             |
|     tuple     | `tuple: (T1, T2, T3, ...)` | `size_of::<T1>() + size_of::<T2>() + size_of::<T3>() + ...` |
|   no return   |       `no_return: !`       |                             `0`                             |

### C Type

| type \ size |    0     |   byte    |   wide    | short / 16 bits | int / 32 bits |   long    |   long long    |
|:-----------:|:--------:|:---------:|:---------:|:---------------:|:-------------:|:---------:|:--------------:|
|   signed    |          | `c_schar` |           |    `c_short`    |    `c_int`    | `c_long`  | `c_long_long`  |
|  unsigned   |          | `c_uchar` |           |   `c_ushort`    |   `c_uint`    | `c_ulong` | `c_ulong_long` |
|    char     |          | `c_char`  | `c_wchar` |   `c_char16`    |  `c_char32`   |           |                |
|    void     | `c_void` |           |           |                 |               |           |                |

|     type     |   float   |   double   |   long double   |
|:------------:|:---------:|:----------:|:---------------:|
| float number | `c_float` | `c_double` | `c_long_double` |

## Module

### File&Module

> `module_exsample.aau`
>
> ```rust
> pub mod module;
> mod pri_module;
> ```
>
> `module_exsample/`
>
> > `module.aau`
> >
> >`pri_module.aau`

### In File Module

```rust
mod module_exsample {
    pub mod module {
        todo!();
    }
    mod pri_module {
        todo!();
    }
}
```

## Function

### Define

```rust
fn function(arg1: T1, arg2: T2, ...) -> ResultType {
    let result: ResultType;
    // ...
    result
}
```

### Call

```no_run
let result = function(param1, param2,...);
```

## Export

`export.aau`

```rust
#[export(
    C(version: 98, str_ref: `char*`),
    CPP(
        versions: [11, 20], 
        include[>=11]: "string",
        str[>=11]: `std::string`,
        include[>=17]: "string_view",
        str[>=17]: `std::string_view`,
        file_extension[>=20]: ".ixx",
    ), 
    Rust(edition: 2024), 
    CSharp(version: 12.0)),
]
fn println(message: &str) {
    std::println("{}", message);
}
```

> ```shell
> $ aau build export.aau
> ```
>
> |       Description       |        Output        |
> | :---------------------: | :------------------: |
> | static library (C\|C++) |   `./lib/export.a`   |
> |  dynamic library (C#)   |   `./cs/export.so`   |
> |   rust library (Rust)   | `./rust/export.rlib` |
> |        C header         |    `./c/export.h`    |
> |       C++ header        | `./cpp11/export.hpp` |
> |      C++20 module       | `./cpp20/export.ixx` |
>
> `./c/export.h`
>
> ```c
> #ifdef EXPORT_H
> #define EXPORT_H
> typedef const char* export_str_ref;
> void export_println(export_str_ref message);
> #endif
> ```
>
> `./cpp11/export.hpp`
>
> ```c++
> #ifdef EXPORT_H
> #define EXPORT_H
> #include <string>
> namespace export {
>     using str = const std::string;
>     void println(str& message);
> }
> #endif
> ```
>
> `./cpp20/export.ixx`
>
> ```c++
> module;
> #include <string_view>
> export module export;
> export namespace export {
>     using str = const std::string_view;
> 	export auto println(str& message) -> void {
>         
>     }
> }
> ```

## Other Language Support

### C

```C
// ISO C 98
#include <aau/std.h>
int main(void) {
    aau_println("Hello AAU!");
    return 0;
}
```

### C++

```c++
// ISO C++ 11
#include <aau/std.hpp>
using namespace ::aau::std;
int main() {
    std::println("Hello AAU!");
    return 0;
}
```

### C++ 20

```c++
// ISO C++ 20
export module main;
import aau.std;
export auto main() -> int {
    std::println("Hello AAU!");
    return 0;
}
```

### Rust

```rust
// rust edition 2024
#![no_std]
use ::algosul_lang::std::prelude::*;
fn main() {
    println("Hello AAU!");
}
```

### C#

```c#
// .net 9.0
namespace CallAAU;
using AlgosulLang.std as AAU;
public class Program {
    static void Main(string[] args) {
        AAU.println("Hello AAU!");
    }
}
```
