# Cấp phát biến trong heap và stack

- Nếu sử dụng các ngôn ngữ bậc cao thì ít khi bạn phải quan tâm đến vấn đề này. Còn Rust thì được thiết kế để làm một ngôn ngữ low level nên nó cung cấp tuỳ chọn cho phép bạn chọn nơi cấp phát biến.
- Ôn lại một chút, heap và stack là hai abtraction giúp bạn quản lý bộ nhớ của chương trình. Nói đơn giản thì stack sẽ là nơi cho phép truy cập với tốc độ cao hơn nhưng bị giới hạn về kích thước nên thường sẽ được dùng để lưu các function call hoặc các biến local trong function (nói đến function call thì bạn có thể nhớ đến các lần gọi hàm trong đệ quy, nếu gọi nhiều quá thì sẽ bị tràn stack). Còn heap thì lại cho tốc độ truy cập chậm hơn nhưng đổi lại nó lưu được nhiều dữ liệu hơn, thường được sử dụng cho cấp phát động (malloc trong C chẳng hạn). Thường thì các bạn theo lập trình thì toàn gặp các vấn đề phải trade-off như này, stack và heap làm mình liên tưởng đến thanh ghi và RAM ở mức hệ điều hành vậy.

```rust
fn main() {
    // đây là dùng heap nè
    let x = Box::new(5);

    // đây là dùng stack nè (mặc định)
    let y = 42;
}
```
