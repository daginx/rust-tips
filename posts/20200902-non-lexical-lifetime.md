# Non-lexical lifetime

## TL;DR;

https://doc.rust-lang.org/edition-guide/rust-2018/ownership-and-lifetimes/non-lexical-lifetimes.html

## Lằng nhằng

Đây là một tính năng cho phép borrow checker chấp nhận nhiều code an toàn hơn. Trước khi nó tồn tại, rất nhiều code đã bị từ chối mặc dù nó không gây nguy hiểm gì cả.

```rust
fn main() {
    let mut scores = vec![1, 2, 3];
    let score = &scores[0];
    scores.push(4);
    // ngày trước thì code như này đã bị từ chối rồi do scores đã bị immutable borrow
    // thì không được phép thay đổi giá trị bên trong

    // nhưng rõ ràng score chưa được sử dụng thì việc thay đổi scores cũng chẳng ảnh hưởng gì
    // cho nên kể từ sau này thì đoạn code trên vẫn được chấp nhận
    // cho đến khi ta sử dụng score thì mới bị báo lỗi
    println!("score is {}", score);
}
```
