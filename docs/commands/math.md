# math

Mathematical functions that generally only operate on a list of numbers (integers, decimals, bytes) and tables.
Currently the following functions are implemented:

* `math average`: Finds the average of a list of numbers or tables
* `math min`: Finds the minimum within a list of numbers or tables
* `math max`: Finds the maximum within a list of numbers or tables
* `math sum`: Finds the sum of a list of numbers or tables

However, the mathematical functions like `min` and `max` are more permissive and also work on `Dates`.

## Examples

To get the average of the file sizes in a directory, simply pipe the size column from the ls command to the average command.

### List of Numbers (Integers, Decimals, Bytes)

```shell
> ls
 #  │ name               │ type │ size     │ modified
────┼────────────────────┼──────┼──────────┼─────────────
  0 │ CODE_OF_CONDUCT.md │ File │   3.4 KB │ 4 days ago
  1 │ CONTRIBUTING.md    │ File │   1.3 KB │ 4 days ago
  2 │ Cargo.lock         │ File │ 106.3 KB │ 6 mins ago
  3 │ Cargo.toml         │ File │   4.6 KB │ 3 days ago
  4 │ LICENSE            │ File │   1.1 KB │ 4 days ago
  5 │ Makefile.toml      │ File │    449 B │ 4 days ago
  6 │ README.md          │ File │  16.0 KB │ 6 mins ago
  7 │ TODO.md            │ File │      0 B │ 6 mins ago
  8 │ assets             │ Dir  │    128 B │ 4 days ago
  9 │ build.rs           │ File │     78 B │ 4 days ago
 10 │ crates             │ Dir  │    672 B │ 3 days ago
 11 │ debian             │ Dir  │    352 B │ 4 days ago
 12 │ docker             │ Dir  │    288 B │ 4 days ago
 13 │ docs               │ Dir  │    160 B │ 4 days ago
 14 │ features.toml      │ File │    632 B │ 4 days ago
 15 │ images             │ Dir  │    160 B │ 4 days ago
 16 │ justfile           │ File │    234 B │ 3 days ago
 17 │ rustfmt.toml       │ File │     16 B │ 4 days ago
 18 │ src                │ Dir  │    128 B │ 4 days ago
 19 │ target             │ Dir  │    192 B │ 8 hours ago
 20 │ tests              │ Dir  │    192 B │ 4 days ago
```

```shell
> ls | get size | math average
───┬────────
 0 │ 7.2 KB
───┴────────
```

```shell
> ls | get size | math min
───┬─────
 0 │ 0 B
───┴─────
```

```shell
> ls | get size | math max
───┬──────────
 0 │ 113.5 KB
───┴──────────
```

```shell
> ls | get size | math sum
───┬──────────
 0 │ 143.4 KB
───┴──────────
```

### Dates

```shell
> ls | get modified | math min
2020-06-09 17:25:51.798743222 UTC
```

```shell
> ls | get modified | math max
2020-06-14 05:49:59.637449186 UT
```

### Operations on tables

```shell
>  pwd | split row / | size
───┬───────┬───────┬───────┬────────────
 # │ lines │ words │ chars │ max length
───┼───────┼───────┼───────┼────────────
 0 │     0 │     1 │     5 │          5
 1 │     0 │     1 │     7 │          7
 2 │     0 │     1 │     9 │          9
 3 │     0 │     1 │     7 │          7
───┴───────┴───────┴───────┴────────────
```

```shell
> pwd | split row / | size | math max
───────────┬───
 lines      │ 0
 words      │ 1
 chars      │ 9
 max length │ 9
────────────┴───
```

```shell
> pwd | split row / | size | math average
────────────┬────────
 lines      │ 0.0000
 words      │ 1.0000
 chars      │ 7.0000
 max length │ 7.0000
────────────┴────────
```

To get the sum of the characters that make up your present working directory.

```shell
> pwd | split row / | size | get chars | math sum
50
```

## Errors

`math` functions are aggregation functions so empty lists are invalid

```shell
> echo [] | math average
error: Error: Unexpected: Cannot perform aggregate math operation on empty data
```

Note `math` functions only work on list of numbers (integers, decimals, bytes) and tables of numbers, if any other types are piped into the function
then unexpected results can occur.

```shell
>  echo [1 2 a ] | math average
0
```
