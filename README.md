# Math Lib (School 21)

`Math Lib` is a School 21 educational project that implements a custom version of part of the standard C `math.h` library.

The project does not use ready-made implementations from the standard library in its core code. The goal was to understand how basic mathematical functions work, implement them using numerical methods, build them into a static library, and check their behavior with unit tests by comparing the results with `math.h`.

The library's technical name in the code is `s21_math`: functions use the `s21_` prefix, the header file is `s21_math.h`, and the build output is `s21_math.a`.

## Project overview

The standard `math.h` library may seem familiar and ready to use, but functions such as `sin`, `log`, `sqrt`, and `pow` rely on numerical algorithms, special-value handling, and trade-offs between accuracy and speed. This project implements those fundamentals from scratch:

- implement mathematical functions with the `s21_` prefix;
- package them as the static library `s21_math.a`;
- expose the public interface in the `s21_math.h` header;
- match the behavior of the standard `math.h` as closely as possible;
- cover the functions with unit tests;
- prepare a coverage report using `gcov/lcov`.

The project helps build a better understanding of C and static-library builds, as well as applied mathematics: series expansions, iterative approximations, argument normalization, and handling `NaN`, infinities, and boundary values.

## Implemented functions

The library implements equivalents of the main `math.h` functions:

| Function | Purpose |
| --- | --- |
| `s21_abs` | absolute value of an integer |
| `s21_fabs` | absolute value of a floating-point number |
| `s21_ceil` | round up |
| `s21_floor` | round down |
| `s21_fmod` | remainder of floating-point division |
| `s21_sqrt` | square root |
| `s21_exp` | exponential function |
| `s21_log` | natural logarithm |
| `s21_pow` | raise a number to a power |
| `s21_sin` | sine |
| `s21_cos` | cosine |
| `s21_tan` | tangent |
| `s21_asin` | arcsine |
| `s21_acos` | arccosine |
| `s21_atan` | arctangent |

All public declarations are in [`src/s21_math.h`](src/s21_math.h).

## Project structure

```text
.
├── README.md
├── README_RUS.md
└── src
    ├── Makefile
    ├── s21_*.c
    ├── s21_math.h
    └── unit_tests
        ├── s21_*_test.c
        └── s21_math_test.*
```

The library's source code is in `src/s21_*.c`. Tests are in `src/unit_tests` and compare the results of the `s21_` functions with the behavior of the standard `math.h` library.

## Requirements

To build and check the project, you need:

- `gcc` or a compatible C compiler;
- `make`;
- the `check` library for unit tests;
- `lcov` and `genhtml` for an HTML coverage report.

Example dependency installation:

```bash
# macOS
brew install check lcov

# Ubuntu/Debian
sudo apt update
sudo apt install build-essential check lcov
```

## Building the project

Go to the source directory:

```bash
cd src
```

Build the static library:

```bash
make
```

Or specify the target explicitly:

```bash
make s21_math.a
```

The build produces:

- `s21_math.a` — the static library;
- `libs21_math.a` — a copy of the library with the `lib` prefix, suitable for linking with `-ls21_math`.

## Running the tests

From the `src` directory, run:

```bash
make test
```

This command:

1. builds the `s21_math.a` library;
2. compiles the unit tests from `unit_tests`;
3. runs the `unit_test` executable;
4. prints the test-suite results in the terminal.

If the tests fail to compile with an error such as `check.h file not found`, the `check` library may not be installed, or the compiler may not be able to find its header files.

## Checking coverage

To generate an HTML coverage report, run:

```bash
make gcov_report
```

The command builds the tests with coverage flags, runs them, collects data with `lcov`, and generates a report at:

```text
src/report/index.html
```

On macOS and Linux, the `Makefile` also attempts to open the report in a browser automatically.

## Using the library in your code

Example:

```c
#include "s21_math.h"
#include <stdio.h>

int main(void) {
    long double value = s21_sqrt(25.0);
    printf("%Lf\n", value);
    return 0;
}
```

To build from the project root:

```bash
gcc main.c src/s21_math.a -I src -o app
./app
```

## Main Makefile commands

| Command | Description |
| --- | --- |
| `make` / `make all` | builds the static library |
| `make s21_math.a` | builds `s21_math.a` |
| `make test` | builds and runs the unit tests |
| `make gcov_report` | generates an HTML coverage report |
| `make clean` | removes build, test, and coverage artifacts |

## Implementation goals

- Do not copy code from the standard `math.h` library.
- Use the C11 standard and the `-Wall -Werror -Wextra` flags.
- Check function accuracy across a broad set of ordinary and boundary values.
- Handle special cases: zero, negative arguments, infinities, `NaN`, and very large or small values.
- Keep a consistent public interface through `s21_math.h`.

## Ideas for further development

Possible additions to the project:

- a CI pipeline that automatically runs `make test` and `make gcov_report`;
- build and coverage status badges in the README;
- more boundary-case tests for `NaN`, `inf`, `-0.0`, and very large or small numbers;
- a separate section describing the algorithms used by each function;
- a benchmark comparing `s21_math` with the standard `math.h` library;
- style checks with `clang-format`;
- `AddressSanitizer` and `UndefinedBehaviorSanitizer`;
- usage examples in small C programs.

## Cleaning the project

To remove generated files, coverage reports, and test binaries:

```bash
cd src
make clean
```
