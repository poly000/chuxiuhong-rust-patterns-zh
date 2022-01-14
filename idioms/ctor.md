# 构造器

## 说明

Rust 没有语言层面的构造器。
取而代之的是常用一个[关联函数][] `new` 创建对象：

## 示例

```rust
/// Time in seconds.
///
/// # Example
///
/// ```
/// let s = Second::new(42);
/// assert_eq!(42, s.value());
/// ```
pub struct Second {
    value: u64
}
impl Second {
    // Constructs a new instance of [`Second`].
    // Note this is an associated function - no self.
    pub fn new(value: u64) -> Self {
        Self { value }
    }
    /// Returns the value in seconds.
    pub fn value(&self) -> u64 {
        self.value
    }
}
```

## Default Constructors

Rust以 [`Default`][std-default] trait 支持默认构造器：

```rust,ignore
// A Rust vector, see liballoc/vec.rs
pub struct Vec<T> {
    buf: RawVec<T>,
    len: usize,
```rust
/// Time in seconds.
///
/// # Example
///
/// ```
/// let s = Second::default();
/// assert_eq!(0, s.value());
/// ```
pub struct Second {
    value: u64
}
impl Second {
    /// Returns the value in seconds.
    pub fn value(&self) -> u64 {
        self.value
    }
}
impl<T> Vec<T> {
    // 构造一个新的 `Vec<T>`.
    // 注意这是一个静态方法
    // 这个构造器不需要任何参数，但有些需要参数来初始化对象
    pub fn new() -> Vec<T> {
        // Create a new Vec with fields properly initialised.
        Vec {
            // 注意我们这里调用的是RawVec类型的构造器
            buf: RawVec::new(),
            len: 0,
        }
impl Default for Second {
    fn default() -> Self {
        Self { value: 0 }
    }
}
```

`Default` can also be derived if all types of all fields implement `Default`,
like they do with `Second`:

```rust
/// Time in seconds.
///
/// # Example
///
/// ```
/// let s = Second::default();
/// assert_eq!(0, s.value());
/// ```
#[derive(Default)]
pub struct Second {
    value: u64
}
impl Second {
    /// Returns the value in seconds.
    pub fn value(&self) -> u64 {
        self.value
    }
}
```

**Note:** When implementing `Default` for a type, it is neither required nor
recommended to also provide an associated function `new` without arguments.

**Hint:** The advantage of implementing or deriving `Default` is that your type
can now be used where a `Default` implementation is required, most prominently,
any of the [`*or_default` functions in the standard library][std-or-default].

## 参阅

- [default idiom](default.md)有对`Default` trait更深入的介绍。
- [生成器模式](../patterns/creational/builder.md)用于有多种构造对象方式的情况。

[关联函数]: https://doc.rust-lang.org/stable/book/ch05-03-method-syntax.html#associated-functions
[std-default]: https://doc.rust-lang.org/stable/std/default/trait.Default.html
[std-or-default]: https://doc.rust-lang.org/stable/std/?search=or_default
