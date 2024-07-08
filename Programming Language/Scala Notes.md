# Scala Notes

## Singleton Objects

An **object** is a class that has exactly one instance. It is created lazily when it is referenced, like a lazy val.

## Unit

`Unit` is a value type which carries no meaningful information. There is exactly one instance of `Unit` which can be declared literally like so: `()`. All functions must return something so sometimes `Unit` is a useful return type.

## Anonymous Function

```scala
// On the left of `=>` is a list of parameters. On the right is an expression involving the parameters.
(x: Int) => x + 1
```

## String Interpolation

Scala provides three string interpolation methods out of the box: `s`, `f` and `raw`.

1. The s String Interpolator

   Prepending `s` to any string literal allows the usage of variables directly in the string. You’ve already seen an example here:

   ```scala
   val name = "James"
   val age = 38
   println(s"$name is $age years old")  // Hello, James
   ```


2. The f String Interpolator

   Prepending `f` to any string literal allows the creation of simple formatted strings, similar to `printf` in other languages. When using the `f` interpolator, all variable references should be followed by a `printf`-style format string, like `%d`.

   ```scala
   val height = 1.9d
   val name = "James"
   println(f"$name%s is $height%2.2f meters tall")  // James is 1.90 meters tall
   ```


## Collection

*In Scala, a* buffer—such as `ArrayBuffer` *and* `ListBuffer`*—is a sequence that can grow and shrink.*

The recommended, general-purpose, “go to” sequential collections

| Type/Category         | Immutable | Mutable       |
| --------------------- | --------- | ------------- |
| Indexed               | `Vector`  | `ArrayBuffer` |
| Linear (Linked lists) | `List`    | `ListBuffer`  |

