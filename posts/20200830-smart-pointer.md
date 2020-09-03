# Smart pointer trong Rust

## Một chút lằng nhằng

- Khi lập trình thì ta đã quá quen thuộc với khái niệm `pass by value` và `pass by reference` rồi. Mỗi ngôn ngữ sẽ có cách xử lý hai vấn đề này riêng và Rust cũng vậy. Đó chính là mục đích bài viết này nhắm đến, giới thiệu một số ví dụ về việc sử dụng con trỏ thông minh (smart pointer) trong Rust (cái `pass by value` thì tạm không đề cập, mà có khi đề cập đến mà cũng không biết).
- Nếu các bạn đã từng học C hay C++ và đã từng sử dụng con trỏ thì mấy cái này chắc không làm khó bạn mấy đâu. Với mình thì khó lắm :).
- Lấy `Box` ra làm ví dụ đầu tiên. Khi bạn khai báo `let b = Box::new(5)` thì thực chất cái biến b kia chỉ đang lưu địa chỉ của nơi biến 5 kia được lưu mà thôi (heap đó). Và muốn lấy giá trị ra thì bạn phải dùng `*b` (nhìn quen nhể). Ở Rust thì nó được gọi là [Deref](https://doc.rust-lang.org/std/ops/trait.Deref.html). Chắc cứ tạm dịch là khử tham chiếu.

```rust
fn main() {
    let x = 5;
    // Giá trị 5 được khởi tạo trên heap và sau đó địa chỉ được gán cho biến y
    let y = Box::new(x);

    println!("{}", 5 == x);
    println!("{}", 5 == *y); // Deref thì mới lấy giá trị ra được để mà so sánh
}
```

## Các loại con trỏ thường gặp

- Khái niệm ownership giúp Rust giải phóng bộ nhớ mà không cần đến GC (Garbage Collector) nhưng kéo theo đó là việc sử dụng con trỏ cũng phức tạp hơn nhiều. Bạn phải lựa chọn loại con trỏ phù hợp với mục đích sử dụng của mình.
- Nói ngắn gọn thì ta có

  - Box<T> dùng cho một quyền sở hữu
  - Rc<T> dùng cho đa quyền sở hữu
  - Arc<T> dùng cho đa quyền sở hữu + threadsafe
  - Cell<T> cái này chưa hiểu lắm, quay lại viết sau.

- Một chút ví dụ

```rust
use std::rc::Rc;

fn main() {
    let x = Box::new(1);
    let y = x; // quyền sở hữu được được chuyển từ x sang cho y
    // từ đoạn code sau này không thể dùng x nữa

    let foo = Rc::new(vec![1.0, 2.0, 3.0]);
    // bạn có thể clone foo bằng 2 cách sau
    let a = foo.clone();
    let b = Rc::clone(&foo);
    // ta có thẻ dùng cả a và b và chúng đều refer đến foo
    // Rc chỉ cho phép bạn đọc chứ không cho phép thay đổi dữ liệu

    // vector sẽ được giải phóng nếu không còn Rc nào trỏ vào nó
}
```

- Rc là viết tắt của Reference Counted. Như tên gọi Rc sẽ đếm số lần một biến được clone.
- Nhưng như mọi ngôn ngữ khác, việc đếm hay có thể nói là increment có thể đúng với đơn luồng nhưng chưa chắc đã đúng
  với đa luồng, vì vậy Rc có một biến thể Arc (Atomically Rc) để có thể sử dụng với nhiều thread.
- Tất nhiên luôn có sự đánh đổi, Atomic tức là sẽ có sự block tài nguyên và làm giảm hiệu năng của chương trình xuống.
  Vì vậy hãy lựa chọn dùng một cách sáng suốt.
