# Tập làm quen với borrow checker

- Đây là những nguyên tắc để giúp Rust giải phóng bộ nhớ mà không cần tới GC (Garbage Collector).
- Thấy khá hứng thú vì nó cũng ánh xạ những gì xảy ra trong thế giới thực, sỡ hữu và vay mượn.

## Move

- Khi quyền sỡ hữu được chuyển cho `b` từ `a` thì sau đó `a` không còn sử dụng được nữa.
- Điều này tránh việc tạo ra các con trỏ bị treo, không trỏ tới vùng nhớ nào (dangling pointers)

```rust
fn main() {
    // a sỡ hữu 5000 đồng
    let a = Box::new(5000);

    println!("a has {} VND", a);

    // a đưa 5000 cho b
    let b = a;

    println!("b has {} VND", b);

    // a không còn tờ 5000 nào => lỗi
    println!("a has {} VND", a);
}
```

- Nhưng hãy xem ví dụ sau để không bị nhầm lẫn:

```rust
fn main() {
    // a lại có 5000
    let a = 5000;

    // những kiểu dữ liệu implement trait Copy hoặc Clone thì sẽ không bị chuyển quyền sở hữu
    // cụ thể ở đây: https://doc.rust-lang.org/std/marker/trait.Copy.html
    // Rust in thêm một tờ 5000 giống của a rồi đưa cho b
    let b = a;

    // a và b, mỗi người có một tờ 5000
    println!("a has {} VND", a);
    println!("b has {} VND", b);
}
```

## Borrow

- Nhiều lúc thằng bạn có cuốn sách hay thì mình chỉ mượn để xem thôi chứ không muốn mua thì đây chính là tình huống đó.

```rust
fn main() {
    // a sỡ hữu 5000 đồng
    let a = Box::new(5000);
    println!("a has {} VND", a);

    // b mượn xem 5000 của a
    let b = &a;
    println!("b sees {} VND from a", b);

    // a vẫn giữ tờ 5000
    println!("a has {} VND", a);
}
```

- Cùng điểm qua một số ví dụ không hợp lệ

```rust
fn main() {
    let immutable = Box::new(5000);
    let mut mutable = Box::new(5000);

    // không thể borrow mutable nếu biến bị mượn là immutable
    let _b = &mut immutable;

    // chỉ có thể mượn mutable một lần
    let b = &mut mutable;
    let _c = &mut mutable;
    **b += 1;

    // đã mượn immutable thì không được thay đổi giá trị biến
    let b = &mutable;
    let c = &mut mutable;
    **c += 1;
    println!("b contains {}", b);
}
```
