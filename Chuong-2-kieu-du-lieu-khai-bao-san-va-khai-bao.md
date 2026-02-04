# Chương 2. Các Kiểu Dữ Liệu Khai Báo Sẵn và Khai Báo Biến

Bây giờ khi bạn đã thiết lập xong môi trường phát triển, đã đến lúc bắt đầu tìm hiểu các đặc điểm của ngôn ngữ Go và cách sử dụng chúng một cách hiệu quả nhất. Khi cố gắng xác định thế nào là “tốt nhất”, có một nguyên tắc xuyên suốt: **hãy viết chương trình sao cho ý định của bạn được thể hiện rõ ràng**. Khi tôi lần lượt trình bày các tính năng và các lựa chọn khác nhau, tôi sẽ giải thích vì sao tôi cho rằng một cách tiếp cận cụ thể sẽ tạo ra mã nguồn rõ ràng hơn.

Tôi sẽ bắt đầu bằng việc xem xét các kiểu dữ liệu được xây dựng sẵn trong Go và cách khai báo biến với những kiểu đó. Mặc dù mọi lập trình viên đều đã quen với các khái niệm này, Go vẫn có những điểm khác biệt, và tồn tại nhiều khác biệt tinh tế giữa Go và các ngôn ngữ khác.

---

## Các Kiểu Dữ Liệu Khai Báo Sẵn (Predeclared Types)

Go có rất nhiều kiểu dữ liệu được tích hợp sẵn trong ngôn ngữ. Chúng được gọi là **predeclared types** (các kiểu khai báo sẵn). Chúng tương tự các kiểu dữ liệu trong nhiều ngôn ngữ khác: boolean, số nguyên, số thực và chuỗi. Việc sử dụng các kiểu dữ liệu này theo phong cách Go (idiomatic Go) đôi khi là một thử thách đối với các lập trình viên chuyển từ ngôn ngữ khác sang.

Trong phần này, bạn sẽ tìm hiểu các kiểu dữ liệu này và cách chúng hoạt động hiệu quả nhất trong Go. Trước khi đi vào từng kiểu cụ thể, hãy cùng tìm hiểu một số khái niệm áp dụng cho **tất cả** các kiểu dữ liệu.

---

## Giá Trị Zero (Zero Value)

Go, giống như hầu hết các ngôn ngữ hiện đại, gán một **giá trị zero mặc định** cho mọi biến được khai báo nhưng chưa được gán giá trị. Việc có một giá trị zero rõ ràng giúp mã nguồn dễ hiểu hơn và loại bỏ một nguồn lỗi phổ biến vốn tồn tại trong các chương trình C và C++.

Khi thảo luận về từng kiểu dữ liệu, tôi cũng sẽ đề cập đến giá trị zero tương ứng của kiểu đó. Bạn có thể xem chi tiết về giá trị zero trong *The Go Programming Language Specification*.

---

## Literal

Trong Go, **literal** là một giá trị số, ký tự hoặc chuỗi được chỉ định trực tiếp trong mã nguồn. Các chương trình Go thường sử dụng bốn loại literal phổ biến. (Một loại literal hiếm hơn sẽ được đề cập khi nói về số phức.)

### Literal Số Nguyên (Integer Literal)

Literal số nguyên là một chuỗi các chữ số. Mặc định, literal số nguyên ở hệ cơ số 10. Tuy nhiên, bạn có thể dùng các tiền tố khác để chỉ định hệ cơ số:

* `0b` hoặc `0B`: nhị phân (cơ số 2)
* `0o` hoặc `0O`: bát phân (cơ số 8)
* `0x` hoặc `0X`: thập lục phân (cơ số 16)

Một số nguyên bắt đầu bằng `0` nhưng không có chữ theo sau cũng biểu diễn bát phân — **không nên dùng**, vì rất dễ gây nhầm lẫn.

Để dễ đọc các số nguyên dài, Go cho phép chèn dấu gạch dưới (`_`) vào giữa literal. Ví dụ, bạn có thể nhóm theo hàng nghìn trong hệ cơ số 10 (`1_234`). Các dấu gạch dưới này **không ảnh hưởng** đến giá trị của số.

Quy tắc đối với dấu `_`:

* Không được ở đầu hoặc cuối số
* Không được đặt liên tiếp nhau

Về mặt kỹ thuật, bạn có thể viết `1_2_3_4`, nhưng **đừng làm vậy**. Hãy dùng `_` để cải thiện khả năng đọc: nhóm theo hàng nghìn ở hệ cơ số 10, hoặc theo ranh giới 1, 2 hoặc 4 byte đối với số nhị phân, bát phân hoặc thập lục phân.

---

### Literal Số Thực (Floating-Point Literal)

Literal số thực có dấu chấm thập phân để biểu thị phần lẻ. Chúng cũng có thể có số mũ, được chỉ định bằng chữ `e` kèm theo một số dương hoặc âm (ví dụ: `6.03e23`).

Bạn cũng có thể viết số thực ở dạng thập lục phân bằng cách dùng tiền tố `0x` và chữ `p` để chỉ định số mũ:

```text
0x12.34p5  // tương đương 582.5 ở hệ cơ số 10
```

Giống như literal số nguyên, bạn có thể dùng dấu `_` để định dạng literal số thực cho dễ đọc.

---

### Literal Rune

Literal rune biểu diễn một ký tự và được bao quanh bởi **dấu nháy đơn** (`'`). Không giống nhiều ngôn ngữ khác, trong Go, dấu nháy đơn và nháy kép **không thể hoán đổi cho nhau**.

Rune literal có thể được viết dưới nhiều dạng:

* Ký tự Unicode đơn: `'a'`
* Số bát phân 8-bit: `'\141'`
* Số thập lục phân 8-bit: `'\x61'`
* Số thập lục phân 16-bit: `'\u0061'`
* Số Unicode 32-bit: `'\U00000061'`

Ngoài ra còn có các rune literal escape phổ biến:

* `\n` – xuống dòng
* `\t` – tab
* `\'` – dấu nháy đơn
* `\\` – dấu gạch chéo ngược

Trong thực tế, hãy ưu tiên dùng hệ cơ số 10 cho literal số nguyên và số thực. Bát phân hiếm khi được dùng, chủ yếu để biểu diễn quyền POSIX (ví dụ: `0o777` cho `rwxrwxrwx`). Thập lục phân và nhị phân thường xuất hiện trong các thao tác bit, mạng hoặc hạ tầng. Tránh dùng các dạng escape số cho rune literal, trừ khi chúng giúp mã nguồn rõ ràng hơn trong ngữ cảnh cụ thể.

---

### Literal Chuỗi (String Literal)

Có hai cách để biểu diễn literal chuỗi trong Go.

#### Chuỗi Thông Dịch (Interpreted String Literal)

Cách phổ biến nhất là dùng **dấu nháy kép** (`"`). Đây là chuỗi thông dịch, chứa zero hoặc nhiều rune literal. Chúng được gọi là “thông dịch” vì các escape sequence và rune literal sẽ được chuyển thành ký tự tương ứng.

Ví dụ:

```go
"Greetings and Salutations"
```

> **LƯU Ý**
> Escape cho dấu nháy đơn (`\'`) **không hợp lệ** trong string literal. Thay vào đó, Go dùng escape cho nháy kép (`\"`).

Trong chuỗi thông dịch, các ký tự **không được phép xuất hiện nếu không escape** là:

* Dấu gạch chéo ngược (`\`)
* Xuống dòng
* Dấu nháy kép (`"`)

Ví dụ, nếu bạn muốn chuỗi hiển thị xuống dòng và có chữ trong ngoặc kép:

```go
"Greetings and\n\"Salutations\""
```

---

#### Chuỗi Thô (Raw String Literal)

Nếu bạn cần bao gồm dấu gạch chéo ngược, nháy kép hoặc xuống dòng, **raw string literal** sẽ đơn giản hơn. Chúng được bao quanh bởi **dấu backquote** (`` ` ``) và có thể chứa bất kỳ ký tự nào, ngoại trừ chính dấu backquote.

Raw string literal **không có escape character**; mọi ký tự được giữ nguyên như bạn viết.

Ví dụ:

```go
`Greetings and
"Salutations"`
```

---

## Literal Là Không Có Kiểu (Untyped)

Các literal trong Go được xem là **không có kiểu**. Tôi sẽ phân tích kỹ hơn khái niệm này trong phần *“Literals Are Untyped”*. Như bạn sẽ thấy trong phần *“var Versus :=”*, có những tình huống trong Go mà kiểu dữ liệu không được khai báo tường minh.

Trong các trường hợp đó, Go sẽ sử dụng **kiểu mặc định** cho literal. Nếu trong biểu thức không có yếu tố nào chỉ rõ kiểu của literal, Go sẽ tự gán kiểu mặc định. Tôi sẽ đề cập đến kiểu mặc định của các literal khi thảo luận về từng predeclared type cụ thể.

## Boolean

Kiểu `bool` biểu diễn các biến logic (Boolean). Biến kiểu `bool` chỉ có thể nhận một trong hai giá trị: `true` hoặc `false`. Giá trị zero của `bool` là `false`:

```go
var flag bool        // không gán giá trị, mặc định là false
var isAwesome = true
```

Rất khó để nói về kiểu dữ liệu mà không nhắc đến khai báo biến, và ngược lại. Tôi sẽ sử dụng khai báo biến trước và giải thích chi tiết hơn trong phần *“var Versus :=”*.

---

## Các Kiểu Số (Numeric Types)

Go có một số lượng lớn các kiểu số: **12 kiểu** (và một vài tên đặc biệt), được chia thành **ba nhóm**. Nếu bạn đến từ một ngôn ngữ như JavaScript – nơi chỉ có một kiểu số duy nhất – thì điều này có thể trông khá nhiều. Thực tế, một số kiểu được dùng rất thường xuyên, trong khi những kiểu khác khá hiếm gặp.

Tôi sẽ bắt đầu với các kiểu số nguyên, sau đó đến số thực và cuối cùng là kiểu số phức rất đặc biệt.

---

## Các Kiểu Số Nguyên (Integer Types)

Go cung cấp cả số nguyên **có dấu** và **không dấu**, với nhiều kích thước khác nhau, từ 1 đến 8 byte. Chúng được liệt kê trong Bảng 2-1.

### Bảng 2-1. Các kiểu số nguyên trong Go

| Tên kiểu | Phạm vi giá trị                              |
| -------- | -------------------------------------------- |
| int8     | –128 đến 127                                 |
| int16    | –32768 đến 32767                             |
| int32    | –2147483648 đến 2147483647                   |
| int64    | –9223372036854775808 đến 9223372036854775807 |
| uint8    | 0 đến 255                                    |
| uint16   | 0 đến 65535                                  |
| uint32   | 0 đến 4294967295                             |
| uint64   | 0 đến 18446744073709551615                   |

Giá trị zero của **tất cả** các kiểu số nguyên là `0`.

---

## Các Kiểu Số Nguyên Đặc Biệt

Go có một số tên đặc biệt dành cho kiểu số nguyên.

### `byte`

`byte` là một **alias** của `uint8`. Việc gán, so sánh hoặc thực hiện phép toán giữa `byte` và `uint8` là hoàn toàn hợp lệ. Tuy nhiên, trong mã Go thực tế, bạn hiếm khi thấy `uint8`; thay vào đó, hãy dùng `byte`.

### `int`

Kiểu `int` phụ thuộc vào kiến trúc CPU:

* Trên CPU 32-bit: `int` là số nguyên có dấu 32-bit (giống `int32`)
* Trên hầu hết CPU 64-bit: `int` là số nguyên có dấu 64-bit (giống `int64`)

Do `int` **không nhất quán giữa các nền tảng**, nên việc gán, so sánh hoặc thực hiện phép toán giữa `int` và `int32` hoặc `int64` **bắt buộc phải ép kiểu**, nếu không sẽ gây lỗi tại thời điểm biên dịch (xem *“Explicit Type Conversion”*).

Literal số nguyên mặc định có kiểu là `int`.

> **LƯU Ý**
> Một số kiến trúc CPU 64-bit hiếm gặp sử dụng `int` 32-bit. Go hỗ trợ ba kiến trúc này: `amd64p32`, `mips64p32`, và `mips64p32le`.

### `uint`

`uint` tuân theo cùng quy tắc với `int`, nhưng là số nguyên **không dấu** (giá trị luôn ≥ 0).

### `rune` và `uintptr`

Hai tên đặc biệt khác là `rune` và `uintptr`.

* `rune` là alias của `int32` và sẽ được bàn kỹ hơn trong phần *“A Taste of Strings and Runes”*
* `uintptr` sẽ được thảo luận trong Chương 16

---

## Chọn Kiểu Số Nguyên Phù Hợp

Với nhiều lựa chọn như vậy, bạn có thể tự hỏi khi nào nên dùng kiểu nào. Hãy tuân theo **ba quy tắc đơn giản** sau:

1. Nếu bạn làm việc với định dạng file nhị phân hoặc giao thức mạng có quy định rõ kích thước hoặc dấu của số nguyên, hãy dùng **kiểu tương ứng**.
2. Nếu bạn đang viết một hàm thư viện cần hoạt động với **mọi kiểu số nguyên**, hãy tận dụng **generics** của Go và dùng tham số kiểu tổng quát.
3. Trong **mọi trường hợp còn lại**, hãy dùng `int`.

> **LƯU Ý**
> Trong mã Go cũ, bạn có thể gặp hai hàm gần như giống nhau, một dùng `int64`, một dùng `uint64`. Điều này xuất phát từ thời điểm Go **chưa có generics**. Khi đó, để hỗ trợ nhiều kiểu, người ta buộc phải viết nhiều hàm khác nhau. Ví dụ điển hình là `strconv.FormatInt` và `strconv.FormatUint` trong thư viện chuẩn.

---

## Toán Tử Số Nguyên

Các số nguyên trong Go hỗ trợ các toán tử số học quen thuộc:

* `+`, `-`, `*`, `/`
* `%` (chia lấy dư)

Kết quả của phép chia số nguyên là **số nguyên**. Nếu bạn muốn kết quả là số thực, bạn phải **ép kiểu** sang số thực trước khi chia. Chia cho 0 sẽ gây ra **panic**.

> **LƯU Ý**
> Phép chia số nguyên trong Go luôn **cắt về 0** (truncation toward zero).

Bạn có thể kết hợp các toán tử với `=`: `+=`, `-=`, `*=`, `/=`, `%=`.

```go
var x int = 10
x *= 2 // x = 20
```

So sánh số nguyên bằng: `==`, `!=`, `>`, `>=`, `<`, `<=`.

Go cũng hỗ trợ các toán tử thao tác bit:

* Dịch bit: `<<`, `>>`
* Toán tử bit: `&` (AND), `|` (OR), `^` (XOR), `&^` (AND NOT)

Tất cả đều có thể kết hợp với `=`: `&=`, `|=`, `^=`, `&^=`, `<<=`, `>>=`.

---

## Các Kiểu Số Thực (Floating-Point Types)

Go có hai kiểu số thực, được liệt kê trong Bảng 2-2.

### Bảng 2-2. Các kiểu số thực trong Go

| Tên kiểu | Giá trị tuyệt đối lớn nhất | Giá trị tuyệt đối nhỏ nhất (khác 0) |
| -------- | -------------------------- | ----------------------------------- |
| float32  | 3.4028234663852886e+38     | 1.401298464324817e-45               |
| float64  | 1.7976931348623157e+308    | 4.940656458412465e-324              |

Giá trị zero của các kiểu số thực là `0`.

Go sử dụng chuẩn **IEEE 754**, giống hầu hết các ngôn ngữ khác. Việc chọn kiểu rất đơn giản: **trừ khi cần tương thích định dạng cũ, hãy dùng `float64`**. Literal số thực mặc định cũng có kiểu `float64`.

`float32` chỉ có độ chính xác khoảng 6–7 chữ số thập phân, trong khi `float64` chính xác hơn. Đừng lo về khác biệt bộ nhớ trừ khi profiling cho thấy đây là vấn đề thực sự.

### Có nên dùng số thực không?

Trong nhiều trường hợp, câu trả lời là **không**. Số thực **không biểu diễn chính xác** mọi giá trị; chúng chỉ lưu giá trị gần đúng. Vì vậy, chỉ nên dùng khi sự xấp xỉ là chấp nhận được, ví dụ:

* Đồ họa
* Thống kê
* Tính toán khoa học

> ⚠️ **CẢNH BÁO**
> Số thực **không thể biểu diễn chính xác giá trị thập phân**. **Không dùng chúng để biểu diễn tiền tệ** hoặc bất kỳ giá trị nào yêu cầu độ chính xác tuyệt đối.

---

## IEEE 754

Go (và hầu hết các ngôn ngữ khác) lưu trữ số thực theo chuẩn **IEEE 754**. Các quy tắc của chuẩn này khá phức tạp và nằm ngoài phạm vi cuốn sách.

Một vài điểm đáng chú ý:

* Chia một số thực khác 0 cho 0 → `+Inf` hoặc `-Inf`
* Chia `0.0 / 0.0` → `NaN` (Not a Number)

Mặc dù Go cho phép so sánh số thực bằng `==` và `!=`, **đừng làm vậy**. Hãy so sánh dựa trên một sai số cho phép (epsilon): kiểm tra xem hiệu tuyệt đối của hai số có nhỏ hơn epsilon hay không.

---

## Kiểu Số Phức (Complex Types)

Go có hỗ trợ **số phức** như một kiểu dữ liệu hạng nhất, dù rất ít người dùng.

* `complex64`: phần thực và ảo là `float32`
* `complex128`: phần thực và ảo là `float64`

Khai báo bằng hàm built-in `complex`:

```go
var complexNum = complex(20.3, 10.2)
```

Quy tắc suy luận kiểu:

* Hai literal không kiểu → `complex128`
* Hai `float32` → `complex64`
* Một `float32` + literal vừa `float32` → `complex64`
* Các trường hợp còn lại → `complex128`

Giá trị zero của số phức là `(0 + 0i)`.

Các toán tử số thực đều hoạt động với số phức. Có thể dùng `real()` và `imag()` để lấy phần thực và ảo. Package `math/cmplx` cung cấp thêm các hàm hỗ trợ.

---

### Ví dụ 2-1. Số phức

```go
func main() {
    x := complex(2.5, 3.1)
    y := complex(10.2, 2)
    fmt.Println(x + y)
    fmt.Println(x - y)
    fmt.Println(x * y)
    fmt.Println(x / y)
    fmt.Println(real(x))
    fmt.Println(imag(x))
    fmt.Println(cmplx.Abs(x))
}
```

Kết quả chạy:

```text
(12.7+5.1i)
(-7.699999999999999+1.1i)
(19.3+36.62i)
(0.2934098482043688+0.24639022584228065i)
2.5
3.1
3.982461550347975
```

Bạn có thể thấy rõ sự **không chính xác của số thực**.

Go còn hỗ trợ **imaginary literal**, trông giống số thực nhưng có hậu tố `i`.

Mặc dù có hỗ trợ số phức, Go không phổ biến trong tính toán số học nặng. Nếu cần, bạn có thể dùng thư viện bên thứ ba **Gonum**, nhưng nên cân nhắc các ngôn ngữ khác trước.

---

## Một Chút Về Chuỗi và Rune

Go có kiểu `string` được tích hợp sẵn. Giá trị zero của `string` là chuỗi rỗng (`""`). Go hỗ trợ Unicode; bạn có thể đặt bất kỳ ký tự Unicode nào vào chuỗi.

Chuỗi có thể:

* So sánh: `==`, `!=`, `>`, `>=`, `<`, `<=`
* Nối chuỗi bằng toán tử `+`

Chuỗi trong Go là **immutable**: bạn có thể gán lại biến chuỗi, nhưng không thể thay đổi nội dung chuỗi đã tồn tại.

Go cũng có kiểu `rune`, đại diện cho **một code point Unicode**. `rune` là alias của `int32`, giống như `byte` là alias của `uint8`.

Nếu bạn biểu diễn một ký tự, hãy dùng `rune`, không dùng `int32`, để thể hiện rõ ý định:

```go
var myFirstInitial rune = 'J' // tốt – rõ nghĩa
var myLastInitial int32 = 'B' // không nên – hợp lệ nhưng gây nhầm lẫn
```

Tôi sẽ nói nhiều hơn về chuỗi trong chương tiếp theo, bao gồm chi tiết cài đặt, mối quan hệ với `byte` và `rune`, cũng như các tính năng nâng cao và những cạm bẫy thường gặp.

---

## Chuyển đổi kiểu tường minh (Explicit Type Conversion)

Hầu hết các ngôn ngữ có nhiều kiểu số đều tự động chuyển đổi từ kiểu này sang kiểu khác khi cần. Cơ chế này được gọi là **tự động nâng kiểu** (automatic type promotion). Nghe có vẻ tiện lợi, nhưng trên thực tế, các quy tắc chuyển đổi có thể trở nên phức tạp và dẫn đến những kết quả không mong muốn.

Là một ngôn ngữ coi trọng **sự rõ ràng về ý định** và **khả năng đọc**, Go **không cho phép** tự động nâng kiểu giữa các biến. Khi kiểu của các biến không khớp nhau, bạn **bắt buộc phải chuyển đổi kiểu một cách tường minh**. Ngay cả các số nguyên hoặc số thực có kích thước khác nhau cũng phải được chuyển về cùng một kiểu thì mới có thể thao tác với nhau. Điều này giúp mã nguồn thể hiện rõ ràng chính xác kiểu dữ liệu bạn muốn dùng mà không cần phải ghi nhớ các quy tắc chuyển đổi ngầm (xem Ví dụ 2-2).

### Ví dụ 2-2. Chuyển đổi kiểu

```go
var x int = 10
var y float64 = 30.2
var sum1 float64 = float64(x) + y
var sum2 int = x + int(y)
fmt.Println(sum1, sum2)
```

Trong đoạn mã này, bạn khai báo bốn biến:

* `x` là `int` với giá trị 10
* `y` là `float64` với giá trị 30.2

Vì hai biến này có kiểu khác nhau, bạn cần chuyển đổi kiểu để có thể cộng chúng lại:

* Với `sum1`, bạn chuyển `x` sang `float64`
* Với `sum2`, bạn chuyển `y` sang `int`

Khi chạy chương trình, kết quả in ra sẽ là:

```cmd
40.2 40
```

Hành vi tương tự cũng áp dụng cho các kiểu số nguyên có kích thước khác nhau (xem Ví dụ 2-3).

### Ví dụ 2-3. Chuyển đổi kiểu số nguyên

```go
var x int = 10
var b byte = 100
var sum3 int = x + int(b)
var sum4 byte = byte(x) + b
fmt.Println(sum3, sum4)
```

Bạn có thể chạy các ví dụ này trên **The Go Playground** hoặc trong thư mục `sample_code/type_conversion` của repository Chapter 2.

Sự nghiêm ngặt này của Go còn kéo theo một hệ quả quan trọng khác: **không có khái niệm “truthy”**.

Trong nhiều ngôn ngữ, một số khác 0 hoặc một chuỗi không rỗng có thể được coi là `true`. Các quy tắc này khác nhau giữa các ngôn ngữ và rất dễ gây nhầm lẫn. Go hoàn toàn không cho phép điều đó. Không có kiểu nào khác có thể được chuyển thành `bool`, dù là ngầm định hay tường minh.

Nếu bạn muốn tạo ra một giá trị boolean từ kiểu khác, bạn **phải dùng toán tử so sánh** (`==`, `!=`, `>`, `<`, `<=`, `>=`). Ví dụ:

* Kiểm tra `x` có bằng 0 hay không: `x == 0`
* Kiểm tra chuỗi `s` có rỗng hay không: `s == ""`

> **Lưu ý**
> Chuyển đổi kiểu là một trong những nơi Go chấp nhận viết dài hơn một chút để đổi lấy sự đơn giản và rõ ràng. Go theo phong cách *idiomatic* luôn ưu tiên khả năng hiểu được hơn là sự ngắn gọn.

---

## Literal là không có kiểu (Literals Are Untyped)

Mặc dù bạn không thể cộng hai biến số nguyên có kiểu khác nhau, Go lại cho phép bạn dùng **literal số nguyên** trong biểu thức số thực, hoặc gán trực tiếp literal số nguyên cho biến `float64`:

```go
var x float64 = 10
var y float64 = 200.3 * 5
```

Điều này là vì **literal trong Go không có kiểu**. Go là một ngôn ngữ thực tế, và việc trì hoãn xác định kiểu cho đến khi lập trình viên chỉ rõ là hoàn toàn hợp lý. Nhờ đó, literal có thể được dùng với bất kỳ biến nào có kiểu tương thích.

Sau này, khi học về kiểu do người dùng định nghĩa (Chapter 7), bạn sẽ thấy literal thậm chí còn có thể dùng với các kiểu tự định nghĩa dựa trên kiểu dựng sẵn.

Tuy nhiên, “không có kiểu” cũng có giới hạn:

* Không thể gán literal chuỗi cho biến số
* Không thể gán literal số cho biến chuỗi
* Không thể gán literal số thực cho biến `int`

Tất cả các trường hợp trên đều bị trình biên dịch báo lỗi. Ngoài ra, còn có giới hạn về kích thước. Bạn có thể viết một literal số rất lớn, nhưng nếu cố gán nó cho một biến không chứa nổi giá trị đó (ví dụ gán `1000` cho `byte`), trình biên dịch sẽ báo lỗi tràn.

---

## var so với :=

Dù là một ngôn ngữ nhỏ, Go lại có khá nhiều cách khai báo biến. Lý do là vì **mỗi cách khai báo truyền tải một ý nghĩa khác nhau** về cách biến được sử dụng.

### Khai báo bằng `var`

Cách đầy đủ nhất là dùng `var`, chỉ rõ kiểu và giá trị:

```go
var x int = 10
```

Nếu biểu thức bên phải đã cho biết rõ kiểu, bạn có thể bỏ phần kiểu bên trái:

```go
var x = 10
```

Nếu bạn chỉ muốn khai báo biến và gán giá trị 0 mặc định:

```go
var x int
```

Bạn cũng có thể khai báo nhiều biến cùng lúc:

```go
var x, y int = 10, 20
var x, y int
var x, y = 10, "hello"
```

Hoặc dùng danh sách khai báo:

```go
var (
    x    int
    y        = 20
    z    int = 30
    d, e     = 40, "hello"
    f, g string
)
```

### Khai báo ngắn gọn với `:=`

Bên trong hàm, Go cho phép bạn dùng toán tử `:=` để vừa khai báo vừa gán giá trị:

```go
var x = 10
x := 10
```

Hai dòng trên là tương đương.

Bạn cũng có thể khai báo nhiều biến:

```go
var x, y = 10, "hello"
x, y := 10, "hello"
```

Một điểm đặc biệt của `:=` là nó cho phép **kết hợp biến mới và biến đã tồn tại**, miễn là bên trái có ít nhất một biến mới:

```go
x := 10
x, y := 30, "hello"
```

Hạn chế của `:=` là **không dùng được ở cấp package**; ngoài hàm, bạn bắt buộc phải dùng `var`.

### Khi nào nên dùng cách nào?

* Trong hàm: ưu tiên `:=`
* Ngoài hàm: dùng `var`
* Khi khởi tạo giá trị 0 có chủ đích: dùng `var x int`
* Khi muốn ép kiểu literal về một kiểu không phải mặc định: dùng `var x byte = 20`

Do `:=` có thể vô tình tạo biến mới khi bạn tưởng là đang dùng lại biến cũ (hiện tượng *shadowing*), trong các đoạn code phức tạp, nên dùng `var` để khai báo rõ ràng và sau đó gán bằng `=`.

Bạn nên hạn chế khai báo biến ở cấp package. Biến toàn cục dễ làm luồng dữ liệu khó theo dõi và dẫn tới bug tinh vi. Quy tắc chung: **chỉ khai báo biến cấp package nếu chúng gần như bất biến**.

> **Mẹo**
> Tránh khai báo biến ngoài hàm vì chúng làm phức tạp việc phân tích luồng dữ liệu.

---

## Sử dụng const

Trong Go, giá trị bất biến được khai báo bằng từ khóa `const`. Thoạt nhìn, nó khá giống các ngôn ngữ khác. Ví dụ 2-4 minh họa cách dùng `const`.

### Ví dụ 2-4. Khai báo hằng số

```go
package main

import "fmt"

const x int64 = 10

const (
    idKey   = "id"
    nameKey = "name"
)

const z = 20 * 10

func main() {
    const y = "hello"

    fmt.Println(x)
    fmt.Println(y)

    x = x + 1 // không biên dịch được!
    y = "bye" // không biên dịch được!
}
```

Khi biên dịch, bạn sẽ nhận được lỗi vì `const` không thể bị gán lại.

Trong Go, `const` **chỉ dùng để đặt tên cho literal**. Một hằng số chỉ có thể chứa giá trị mà trình biên dịch xác định được tại thời điểm biên dịch, bao gồm:

* Literal số
* `true` và `false`
* Chuỗi
* Rune
* Giá trị trả về từ các hàm dựng sẵn như `complex`, `real`, `imag`, `len`, `cap`
* Biểu thức được tạo từ các giá trị trên

Go **không cho phép** khai báo một giá trị được tính tại runtime là bất biến:

```go
x := 5
y := 10
const z = x + y // lỗi biên dịch
```

Go cũng không có mảng, slice, map hay struct bất biến, và không thể đánh dấu một field là immutable. Trên thực tế, điều này ít hạn chế hơn bạn nghĩ, vì trong phạm vi hàm, việc biến có bị thay đổi hay không là rất rõ ràng.

> **Mẹo**
> Trong Go, `const` chỉ là cách đặt tên cho literal. Không có cơ chế để khai báo một biến là bất biến.

## Typed and Untyped Constants

Trong Go, **hằng số (const)** có thể là **có kiểu (typed)** hoặc **không có kiểu (untyped)**.

* **Hằng số không có kiểu** hoạt động giống hệt literal: nó không có kiểu riêng, nhưng có *kiểu mặc định* được sử dụng khi không thể suy luận kiểu nào khác.
* **Hằng số có kiểu** chỉ có thể được gán trực tiếp cho biến có **đúng kiểu đó**.

Việc nên dùng hằng số có kiểu hay không phụ thuộc vào **mục đích khai báo hằng số**:

* Nếu bạn chỉ muốn đặt tên cho một **hằng số toán học** và có khả năng dùng với nhiều kiểu số khác nhau, hãy **để hằng số không có kiểu**.
* Nhìn chung, hằng số không có kiểu mang lại **tính linh hoạt cao hơn**.
* Trong một số trường hợp, bạn muốn hằng số **ép buộc kiểu**. Một ví dụ điển hình sẽ xuất hiện khi nói về **enumeration với iota** ở phần *“iota Is for Enumerations—Sometimes”*.

### Hằng số không có kiểu

```go
const x = 10
```

Tất cả các phép gán sau đều **hợp lệ**:

```go
var y int = x
var z float64 = x
var d byte = x
```

### Hằng số có kiểu

```go
const typedX int = 10
```

Hằng số này **chỉ** có thể gán trực tiếp cho biến kiểu `int`. Nếu gán sang kiểu khác, trình biên dịch sẽ báo lỗi, ví dụ:

```
cannot use typedX (type int) as type float64 in assignment
```

---

## Unused Variables

Một trong những mục tiêu của Go là giúp **các nhóm lớn** cộng tác hiệu quả khi phát triển phần mềm. Vì vậy, Go đưa ra một số quy tắc khá *nghiêm ngặt* so với nhiều ngôn ngữ khác.

Ở Chapter 1, bạn đã thấy Go yêu cầu format code bằng `go fmt`. Một yêu cầu khác là:

> **Mọi biến cục bộ (local variable) được khai báo đều phải được đọc (used).**

Nếu bạn khai báo một biến cục bộ nhưng không đọc giá trị của nó, chương trình sẽ **không biên dịch được**.

### Kiểm tra biến không dùng chưa hoàn toàn triệt để

Trình biên dịch Go **không kiểm tra toàn diện** tất cả các trường hợp. Chỉ cần một biến được đọc **ít nhất một lần**, compiler sẽ không phàn nàn, ngay cả khi có các phép gán khác không bao giờ được dùng.

Ví dụ sau là **chương trình Go hợp lệ**:

```go
func main() {
    x := 10 // giá trị này không bao giờ được dùng!
    x = 20
    fmt.Println(x)
    x = 30 // giá trị này cũng không được dùng!
}
```

Compiler và `go vet` sẽ **không phát hiện** hai phép gán không cần thiết là `10` và `30`. Tuy nhiên, các công cụ bên thứ ba có thể làm được điều này (sẽ nói ở phần *“Using Code-Quality Scanners”*).

> **Note**
> Trình biên dịch Go **không cấm** việc khai báo biến package-level mà không dùng. Đây là một lý do nữa để bạn **tránh khai báo biến ở package-level**.

---

## Unused Constants

Điều khá bất ngờ là Go **cho phép** khai báo hằng số mà không sử dụng.

Lý do là vì:

* Hằng số được tính **tại thời điểm biên dịch**
* Chúng **không có side effect**

Nếu một hằng số không được dùng, nó đơn giản là **không xuất hiện trong binary cuối cùng**.

---

## Naming Variables and Constants

Có sự khác biệt giữa:

* **Quy tắc đặt tên hợp lệ của Go**
* **Quy ước đặt tên theo phong cách Go (idiomatic Go)**

### Quy tắc ngôn ngữ

Tên định danh trong Go:

* Phải bắt đầu bằng **chữ cái hoặc dấu gạch dưới (_)**
* Có thể chứa chữ cái, chữ số và dấu gạch dưới
* Chữ cái và chữ số có thể là **Unicode**, không chỉ ASCII

Điều này khiến các ví dụ sau đều là **Go hợp lệ**:

```go
_0 := 0_0
_𝟙 := 20
π := 3
ａ := "hello" // Unicode U+FF41
__ := "double underscore"
fmt.Println(_0)
fmt.Println(_𝟙)
fmt.Println(π)
fmt.Println(ａ)
fmt.Println(__)
```

Code trên chạy được, nhưng **đừng bao giờ viết code như vậy**.

Những tên này **không idiomatic**, vì:

* Khó đọc
* Khó gõ trên bàn phím
* Dễ gây nhầm lẫn, đặc biệt với các ký tự Unicode trông giống nhau

### Unicode look-alike – cực kỳ nguy hiểm

```go
func main() {
    ａ := "hello"   // Unicode U+FF41
    a := "goodbye" // Unicode U+0061
    fmt.Println(ａ)
    fmt.Println(a)
}
```

Kết quả:

```
hello
goodbye
```

Dù nhìn gần như giống nhau, đây là **hai biến hoàn toàn khác**.

---

### Camel case thay vì snake case

Dù dấu gạch dưới `_` là ký tự hợp lệ, idiomatic Go **hiếm khi dùng snake_case** (`index_counter`).

Thay vào đó, Go dùng **camelCase**:

* `indexCounter`
* `numberTries`

> **Note**
> Dấu gạch dưới đơn lẻ (`_`) là một định danh đặc biệt trong Go. Nó sẽ được giải thích kỹ hơn ở Chapter 5.

---

### Đặt tên hằng số

Nhiều ngôn ngữ viết hằng số bằng **ALL_CAPS_WITH_UNDERSCORES**. Go **không làm vậy**.

Lý do là vì Go dùng **chữ cái đầu tiên viết hoa hay thường** để quyết định một định danh ở package-level có được export hay không. Điều này sẽ được nói lại khi học về package (Chapter 10).

---

### Độ dài tên theo phạm vi (scope)

* **Trong hàm**: ưu tiên tên ngắn
  
  * `i`, `j` cho vòng lặp
  * `k`, `v` cho `for range`

* **Scope càng nhỏ → tên càng ngắn**

Tên ngắn có hai lợi ích:

1. Ít phải gõ → code gọn hơn
2. Nếu bạn không theo dõi nổi biến tên ngắn → block code đang quá phức tạp

Với biến **package-level**, hãy dùng tên **mô tả rõ ràng hơn**, vì phạm vi sử dụng rộng hơn.

---

## Exercises

Các bài tập sau giúp củng cố kiến thức của chương. Lời giải nằm trong repository Chapter 2.

1. Viết chương trình khai báo biến số nguyên `i` có giá trị 20. Gán `i` cho biến số thực `f`. In ra `i` và `f`.

2. Viết chương trình khai báo hằng số `value` có thể gán cho cả biến số nguyên và số thực. Gán nó cho `i` (int) và `f` (float), rồi in ra.

3. Viết chương trình có ba biến:
   
   * `b` kiểu `byte`
   * `smallI` kiểu `int32`
   * `bigI` kiểu `uint64`
   
   Gán mỗi biến giá trị lớn nhất hợp lệ của kiểu tương ứng, sau đó cộng thêm 1 và in kết quả.

---

## Wrapping Up

Bạn đã học được rất nhiều nội dung quan trọng trong chương này:

* Cách dùng các kiểu dữ liệu có sẵn
* Khai báo biến và hằng số
* Gán giá trị và làm việc với toán tử

Trong chương tiếp theo, chúng ta sẽ tìm hiểu **các kiểu dữ liệu tổng hợp (composite types)** trong Go:

* Array
* Slice
* Map
* Struct

Đồng thời quay lại với **string và rune**, và cách chúng tương tác với **encoding ký tự**.
