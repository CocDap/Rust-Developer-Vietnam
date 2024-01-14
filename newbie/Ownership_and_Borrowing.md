## Hiểu khái niệm và cách thức hoạt động của ownership trong Rust

### Ownership là gì ? 

 "Ownership" trong Rust, một chủ đề rất hay và quan trọng! Hãy tưởng tượng bạn có một chiếc bánh pizza. Trong Rust, "Ownership" giống như quy tắc xác định ai có quyền ăn miếng pizza đó. Đây là một cách Rust giữ cho bộ nhớ của programs được sắp xếp gọn gàng và an toàn - giống như cách bạn không muốn ai khác cắn vào miếng pizza của mình 😁!

##### Các features:
 +   **Mỗi mảnh bộ nhớ có một chủ nhân**: Trong Rust, mỗi mảnh bộ nhớ được cấp phát (chẳng hạn như một biến) đều có một "owner" (chủ nhân). Điều này giúp Rust biết khi nào cần giải phóng bộ nhớ đó.
 +   **Chỉ có một chủ nhân tại một thời điểm**: Giống như việc chỉ có một người có thể cầm miếng pizza (không ai muốn một miếng pizza bị cắn chung đúng không 😔?), một mảnh bộ nhớ trong Rust chỉ có thể có một owner tại một thời điểm.
 +   **Khi chủ nhân ra đi, bộ nhớ được dọn dẹp**: Khi một owner (chẳng hạn như một biến) ra khỏi phạm vi hoạt động (hãy tưởng tượng họ đi khỏi bàn ăn), Rust tự động giải phóng bộ nhớ mà owner đó quản lý. Đây là cách Rust giải phóng bộ nhớ mà không cần đến garbage collector (trình dọn rác).

### Một số ưu điểm:

-    **An toàn và tiết kiệm cho bộ nhớ 🛡️**: giúp ngăn chặn lỗi rò rỉ bộ nhớ và đảm bảo rằng bộ nhớ không bị truy cập trái phép. Bằng cách quản lý bộ nhớ hiệu quả, "Ownership" giúp giảm bớt việc sử dụng tài nguyên không cần thiết. Điều này rất quan trọng đối với các ứng dụng yêu cầu hiệu năng cao hoặc chạy trên phần cứng có hạn chế. Nó giống như việc bạn sử dụng mỗi miếng pizza một cách triệt để.

-    **Tránh concurrency errors 😷**: Khi nhiều tiến trình cố gắng truy cập vào cùng một dữ liệu, "Ownership" giống như một quản lý hàng đợi thông minh. Nó giúp ngăn chặn lỗi liên quan đến sự đồng thời, đảm bảo mỗi tiến trình lần lượt "ăn" mà không "đụng hàng".

-    **Tối ưu hiệu năng (Không cần garbage collector) 🚀**: giúp chạy nhanh hơn, giống như là bạn chạy marathon mà không cần mang theo một cái ba lô nặng nề. "Ownership" giúp quản lý bộ nhớ một cách hiệu quả, làm tăng hiệu năng tổng thể của những đoạn code.

-    **Dễ dàng handle predict behavior 🧐**: khi bạn biết rõ ai là chủ nhân của từng mảnh bộ nhớ, việc dự đoán và hiểu hành vi của "programs" trở nên dễ dàng hơn. Nó giống như việc biết rõ ai sẽ ăn miếng pizza cuối cùng trước khi hộp pizza mở.

### Ví dụ ownership: 

###### Ví dụ đơn giản minh họa cách ownership giúp quản lý bộ nhớ:

```rust
    fn main() {
    let my_pizza = String::from("Bánh Pizza Pepperoni"); // Bạn có một chiếc pizza!
    eat_pizza(my_pizza); // Bạn đưa chiếc pizza của mình cho hàm `eat_pizza`.

    // Ôi không! Bạn không thể sử dụng `my_pizza` ở đây nữa. Nó đã được "ăn" (ownership  đã được chuyển giao).
    // println!("Tôi vẫn còn {}", my_pizza); // Compiler rust sẽ báo lỗi.
    }

    fn eat_pizza(pizza: String) {
        println!("Tôi đang ăn {}!", pizza); // Chiếc pizza đang được "ăn" ở đây.
        // Sau khi hàm này kết thúc, biến `pizza` sẽ hết phạm vi và bộ nhớ của nó được giải phóng.
    }
    /*
    Output: "Tôi đang ăn Bánh Pizza Pepperoni!"
    */
    /*
    Bạn tạo một String có tên my_pizza. Đó là chiếc pizza pepperoni ngon lành của bạn.
    Bạn gọi hàm eat_pizza và truyền my_pizza cho nó. Bây giờ, eat_pizza sở hữu chiếc pizza.
    Bên trong eat_pizza, chiếc pizza được sử dụng (trong trường hợp này, nó chỉ được in ra màn hình console).
    Một khi eat_pizza hoàn thành, biến pizza sẽ hết phạm vi, và bộ nhớ của nó được tự động giải phóng. Điều này giống như việc bạn đã ăn xong chiếc pizza – không còn gì để dùng nữa.
    Trở lại trong hàm main, nếu bạn cố gắng sử dụng my_pizza lần nữa, Rust sẽ ngăn bạn. Tại sao? Bởi vì bạn không còn sở hữu chiếc pizza nữa – bạn đã đưa nó đi mất rồi 😡!
    */
```

### Borrowing là gì ? 

 "Borrowing" trong Rust là một khái niệm quan trọng khác, liên quan chặt chẽ với "Ownership". Hãy tưởng tượng bạn có một cuốn sách hay và bạn muốn cho bạn bè mượn để đọc, nhưng vẫn đảm bảo cuốn sách đó cuối cùng sẽ quay trở lại của tay bạn. Đó chính là ý tưởng cơ bản của "Borrowing" trong Rust.

#### Giải Thích "Borrowing:
**Mượn tạm thời**: Khi bạn "mượn" một giá trị trong Rust, bạn tạm thời truy cập vào nó mà không lấy đi quyền sở hữu. Điều này được thực hiện thông qua tham chiếu.
 
**Tham chiếu không đổi và tham chiếu đổi**: Có hai loại "borring" trong Rust.

    + Tham chiếu không đổi Immutable (&): Cho phép bạn đọc dữ liệu mà không thay đổi nó. Bạn có thể có nhiều tham chiếu không đổi tới cùng một dữ liệu cùng một lúc. 

    + Tham chiếu đổi Mutable (&mut): Cho phép bạn thay đổi dữ liệu. Chỉ có thể có một tham chiếu đổi tới một dữ liệu tại một thời điểm.

**Quy tắc an toàn**: Rust đảm bảo rằng không bao giờ có tham chiếu đổi khi có tham chiếu không đổi khác đang tồn tại. Điều này ngăn chặn dữ liệu bị thay đổi khi đang được đọc, giúp tránh các lỗi liên quan đến sự đồng thời.

#### Ví Dụ về "Borrowing":

###### Giả sử bạn có một hàm đọc nội dung của một cuốn sách mà không thay đổi nó:

```rust
    fn main() {
        let book = String::from("The Rust Programming Language");

        read_book(&book); // Mượn `book` thông qua tham chiếu không đổi
        read_book(&book); // Bạn có thể mượn `book` nhiều lần

        // `book` vẫn có thể được sử dụng ở đây vì nó chưa bao giờ bị mất quyền sở hữu
        println!("I still own the book: {}", book);
    }

    fn read_book(book: &String) {
        println!("Reading: {}", book);
        // `book` không thể bị thay đổi ở đây vì đó là một tham chiếu không đổi.
    }

    /*
    Output:
    "Reading: The Rust Programming Language
    Reading: The Rust Programming Language
    I still own the book: The Rust Programming Language"
    */
    /* *Giải thích:
    read_book mượn book nhưng không sở hữu nó.
    Bạn có thể gọi read_book nhiều lần vì nó chỉ cần một tham chiếu không đổi.
    Quyền sở hữu của book vẫn ở trong main, và bạn có thể tiếp tục sử dụng book sau các lần gọi read_book.

    Bạn có thể "chia sẻ" dữ liệu mà không lo ngại về việc thay đổi không mong muốn hoặc lỗi do truy cập tạm thời. Đó giống như việc bạn cho bạn bè mượn cuốn sách của mình, nhưng vẫn biết chắc chắn rằng sách sẽ không bị viết bậy lên hoặc mất đi. 📚✨
    */
```
