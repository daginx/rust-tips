# Smart pointer trong Rust

## Một chút lằng nhằng

- Khi lập trình thì ta đã quá quen thuộc với khái niệm `pass by value` và `pass by reference` rồi. Mỗi ngôn ngữ sẽ có cách xử lý hai vấn đề này riêng và Rust cũng vậy. Đó chính là chủ đề của bài này, một số ví dụ về sử dụng khái niệm con trỏ thông minh (smart pointer) trong Rust (cái `pass by value` thì không đề cập ở đây đâu nha).
- Nếu các bạn đã từng học c hay c++ căn bản và đã từng sử dụng con trỏ thì mấy cái này chắc không làm khó bạn mấy đâu. Với mình thì khó lắm :)
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

- Rust cung cấp cho ta một số abtraction hữu ích để sử dụng con trỏ tuỳ vào trường hợp và mục đích sử dụng.
- Nói ngắn gọn thì ta có

  - Box<T> dùng cho một quyền sở hữu
  - Rc<T> dùng cho đa quyền sở hữu
  - Arc<T> dùng cho đa quyền sở hữu + threadsafe
  - Cell<T> cái này chưa hiểu lắm, quay lại viết sau.

- Một chút ví dụ

```rust
fn main() {
    let x = Box::new(1);
    let y = x; // quyền sở hữu được được chuyển từ x sang cho y
    // từ đoạn code sau này không thể dùng x nữa


}
```
