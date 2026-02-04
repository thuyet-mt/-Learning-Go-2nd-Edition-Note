# Chương 4. Khối lệnh (Blocks), Che khuất (Shadowing) và Cấu trúc điều khiển

Bây giờ tôi đã trình bày xong về biến, hằng số và các kiểu dựng sẵn (built-in types), bạn đã sẵn sàng để tìm hiểu về logic lập trình và cách tổ chức chương trình. Tôi sẽ bắt đầu bằng việc giải thích khối lệnh (block) là gì và cách chúng kiểm soát phạm vi tồn tại của một định danh (identifier). Sau đó, tôi sẽ giới thiệu các cấu trúc điều khiển của Go: `if`, `for` và `switch`. Cuối cùng, tôi sẽ nói về `goto` và trường hợp duy nhất mà bạn nên sử dụng nó.

---

## Khối lệnh (Blocks)

Go cho phép bạn khai báo biến ở rất nhiều nơi. Bạn có thể khai báo chúng bên ngoài hàm, làm tham số của hàm, hoặc là biến cục bộ bên trong hàm.

> **LƯU Ý**
> Cho tới lúc này, bạn mới chỉ viết hàm `main`, nhưng ở chương tiếp theo bạn sẽ viết các hàm có tham số.

Mỗi nơi mà một khai báo xuất hiện được gọi là một **khối lệnh (block)**. Các biến, hằng số, kiểu dữ liệu và hàm được khai báo bên ngoài mọi hàm đều nằm trong **package block**. Bạn đã sử dụng các câu lệnh `import` trong chương trình của mình để truy cập các hàm in ra màn hình và toán học (tôi sẽ nói chi tiết về chúng trong Chương 10). Các câu lệnh này định nghĩa tên cho các package khác và các tên đó hợp lệ trong file chứa câu lệnh `import`. Những tên này nằm trong **file block**.

Tất cả các biến được định nghĩa ở mức cao nhất của một hàm (bao gồm cả các tham số của hàm) đều nằm trong một block. Bên trong một hàm, mỗi cặp dấu ngoặc nhọn (`{}`) lại định nghĩa một block khác. Và chỉ một lát nữa thôi, bạn sẽ thấy rằng các cấu trúc điều khiển trong Go cũng tự tạo ra các block của riêng chúng.

Bạn có thể truy cập một định danh được định nghĩa ở bất kỳ block bên ngoài nào từ bên trong một block con. Điều này dẫn tới một câu hỏi: điều gì xảy ra khi bạn khai báo một định danh có cùng tên với một định danh ở block bao quanh nó? Khi đó, bạn đã **che khuất (shadow)** định danh được tạo ở block bên ngoài.

---

## Che khuất biến (Shadowing Variables)

Trước khi giải thích che khuất là gì, hãy xem qua một đoạn mã (xem Ví dụ 4-1). Bạn có thể chạy nó trên *The Go Playground* hoặc trong thư mục `sample_code/shadow_variables` của repository Chương 4.

### Ví dụ 4-1. Che khuất biến

```go
func main() {
    x := 10
    if x > 5 {
        fmt.Println(x)
        x := 5
        fmt.Println(x)
    }
    fmt.Println(x)
}
```

Trước khi chạy đoạn mã này, hãy thử đoán xem nó sẽ in ra gì:

* Không in ra gì cả; mã không biên dịch được
* 10 ở dòng đầu, 5 ở dòng thứ hai, 5 ở dòng thứ ba
* 10 ở dòng đầu, 5 ở dòng thứ hai, 10 ở dòng thứ ba

Kết quả thực tế là:

```cmd
10
5
10
```

Một **biến che khuất (shadowing variable)** là một biến có cùng tên với một biến trong block bao quanh nó. Trong suốt thời gian biến che khuất tồn tại, bạn không thể truy cập tới biến bị che khuất ở block ngoài.

Trong trường hợp này, gần như chắc chắn bạn không hề muốn tạo ra một biến `x` hoàn toàn mới bên trong câu lệnh `if`. Thay vào đó, có lẽ bạn chỉ muốn gán giá trị 5 cho biến `x` đã được khai báo ở mức cao nhất của block hàm. Ở câu lệnh `fmt.Println` đầu tiên bên trong `if`, bạn vẫn có thể truy cập biến `x` được khai báo ở đầu hàm. Tuy nhiên, ở dòng tiếp theo, bạn che khuất `x` bằng cách khai báo một biến mới cùng tên bên trong block được tạo bởi thân của câu lệnh `if`.

Tại câu lệnh `fmt.Println` thứ hai, khi bạn truy cập biến tên `x`, bạn nhận được biến che khuất, với giá trị là 5. Dấu ngoặc nhọn đóng của thân `if` kết thúc block nơi biến `x` che khuất tồn tại, và tại câu lệnh `fmt.Println` thứ ba, khi bạn truy cập biến tên `x`, bạn lại nhận được biến `x` được khai báo ở đầu hàm, với giá trị là 10.

Lưu ý rằng biến `x` này không hề biến mất hay bị gán lại giá trị; chỉ đơn giản là không có cách nào để truy cập nó trong lúc nó bị che khuất ở block bên trong.

---

Tôi đã đề cập ở chương trước rằng trong một số tình huống, tôi tránh sử dụng `:=` vì nó có thể làm cho việc hiểu biến nào đang được dùng trở nên không rõ ràng. Lý do là vì rất dễ vô tình che khuất một biến khi dùng `:=`.

Hãy nhớ rằng bạn có thể dùng `:=` để tạo và gán nhiều biến cùng một lúc. Ngoài ra, không phải tất cả các biến ở vế trái đều phải là biến mới thì `:=` mới hợp lệ. Bạn có thể dùng `:=` miễn là có **ít nhất một biến mới** ở vế trái. Hãy xem một chương trình khác (xem Ví dụ 4-2), bạn có thể tìm thấy nó trên *The Go Playground* hoặc trong thư mục `sample_code/shadow_multiple_assignment` của repository Chương 4.

### Ví dụ 4-2. Che khuất với phép gán nhiều biến

```go
func main() {
    x := 10
    if x > 5 {
        x, y := 5, 20
        fmt.Println(x, y)
    }
    fmt.Println(x)
}
```

Chạy đoạn mã này cho ra kết quả sau:

```cmd
5 20
10
```

Mặc dù đã có một định nghĩa `x` tồn tại ở block ngoài, `x` vẫn bị che khuất bên trong câu lệnh `if`. Đó là vì `:=` chỉ tái sử dụng các biến đã được khai báo trong **block hiện tại**. Khi dùng `:=`, hãy chắc chắn rằng bạn không có biến nào từ phạm vi ngoài ở vế trái, trừ khi bạn thực sự muốn che khuất chúng.

---

Bạn cũng cần cẩn thận để đảm bảo rằng mình không che khuất một package đã import. Tôi sẽ nói nhiều hơn về việc import package trong Chương 10, nhưng cho tới giờ bạn đã import package `fmt` để in ra kết quả chương trình. Hãy xem điều gì xảy ra khi bạn khai báo một biến tên là `fmt` bên trong hàm `main`, như trong Ví dụ 4-3. Bạn có thể thử chạy nó trên *The Go Playground* hoặc trong thư mục `sample_code/shadow_package_names` của repository Chương 4.

### Ví dụ 4-3. Che khuất tên package

```go
func main() {
    x := 10
    fmt.Println(x)
    fmt := "oops"
    fmt.Println(fmt)
}
```

Khi bạn chạy đoạn mã này, bạn sẽ gặp lỗi:

```cmd
fmt.Println undefined (type string has no field or method Println)
```

Lưu ý rằng vấn đề không nằm ở chỗ bạn đặt tên biến là `fmt`; mà là ở chỗ bạn đã cố gắng truy cập một thứ mà biến cục bộ `fmt` không hề có. Một khi biến cục bộ `fmt` được khai báo, nó che khuất package `fmt` trong file block, khiến bạn không thể sử dụng package `fmt` trong phần còn lại của hàm `main`.

---

## Universe Block

Còn một block nữa hơi đặc biệt: **universe block**. Hãy nhớ rằng Go là một ngôn ngữ nhỏ, chỉ có 25 từ khóa. Điều thú vị là các kiểu dựng sẵn (như `int` và `string`), các hằng số (như `true` và `false`), và các hàm (như `make` hay `close`) lại không nằm trong danh sách đó. `nil` cũng vậy. Vậy chúng ở đâu?

Thay vì coi chúng là từ khóa, Go xem chúng là các **định danh được khai báo sẵn (predeclared identifiers)** và định nghĩa chúng trong universe block, tức là block bao chứa tất cả các block khác.

Vì các tên này được khai báo trong universe block, chúng hoàn toàn có thể bị che khuất ở các phạm vi khác. Bạn có thể thấy điều này bằng cách chạy đoạn mã trong Ví dụ 4-4 trên *The Go Playground* hoặc trong thư mục `sample_code/shadow_true` của repository Chương 4.

### Ví dụ 4-4. Che khuất `true`

```go
fmt.Println(true)
true := 10
fmt.Println(true)
```

Khi chạy, bạn sẽ thấy:

```cmd
true
10
```

Bạn phải **cực kỳ cẩn thận** để không bao giờ định nghĩa lại bất kỳ định danh nào trong universe block. Nếu vô tình làm vậy, bạn sẽ gặp những hành vi rất kỳ lạ. Nếu may mắn, bạn sẽ gặp lỗi biên dịch. Nếu không, bạn sẽ rất khó để lần ra nguồn gốc của vấn đề.

Vì che khuất biến hữu ích trong một vài trường hợp (các chương sau sẽ chỉ ra những trường hợp đó), nên `go vet` không coi đây là một lỗi tiềm ẩn. Trong phần “Using Code-Quality Scanners”, bạn sẽ tìm hiểu về các công cụ bên thứ ba có thể phát hiện việc che khuất biến một cách vô tình trong mã của bạn.
