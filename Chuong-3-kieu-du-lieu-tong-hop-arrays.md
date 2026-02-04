# Chương 3. Kiểu Dữ Liệu Tổng Hợp (Composite Types)

Trong chương trước, bạn đã tìm hiểu về **literal** và các **kiểu dữ liệu được khai báo sẵn** trong Go: số, boolean và chuỗi. Trong chương này, bạn sẽ học về **các kiểu dữ liệu tổng hợp (composite types)** trong Go, các **hàm built-in** hỗ trợ chúng, cũng như **best practices** khi làm việc với các kiểu dữ liệu này.

---

## Arrays — Quá Cứng Nhắc Để Dùng Trực Tiếp

Giống như hầu hết các ngôn ngữ lập trình khác, Go có **array**. Tuy nhiên, trong thực tế, **array rất hiếm khi được dùng trực tiếp trong Go**. Chút nữa bạn sẽ thấy lý do vì sao, nhưng trước hết hãy điểm qua cú pháp khai báo và cách sử dụng array.

Tất cả các phần tử trong array **phải cùng một kiểu dữ liệu**. Có một vài cách khai báo array khác nhau. Cách đầu tiên là chỉ rõ **kích thước array** và **kiểu của phần tử**:

```go
var x [3]int
```

Dòng này tạo ra một array gồm **3 số nguyên (`int`)**. Vì chưa gán giá trị ban đầu, nên tất cả các phần tử (`x[0]`, `x[1]`, `x[2]`) đều được khởi tạo bằng **zero value** của `int`, tức là `0`.

Nếu bạn có sẵn giá trị ban đầu cho array, bạn có thể dùng **array literal**:

```go
var x = [3]int{10, 20, 30}
```

Nếu array là **sparse array** (đa số phần tử có giá trị zero), bạn có thể chỉ định **chỉ số** cho những phần tử khác zero:

```go
var x = [12]int{1, 5: 4, 6, 10: 100, 15}
```

Array này có 12 phần tử với giá trị:

```cmd
[1, 0, 0, 0, 0, 4, 6, 0, 0, 0, 100, 15]
```

Khi dùng array literal để khởi tạo array, bạn có thể thay kích thước bằng dấu `...` để Go **tự suy ra số phần tử**:

```go
var x = [...]int{10, 20, 30}
```

---

## So Sánh Array

Bạn có thể dùng toán tử `==` và `!=` để so sánh hai array. Hai array **bằng nhau** nếu:

* Có cùng độ dài
* Các phần tử tương ứng bằng nhau

```go
var x = [...]int{1, 2, 3}
var y = [3]int{1, 2, 3}
fmt.Println(x == y) // in ra true
```

---

## Array Nhiều Chiều

Go chỉ hỗ trợ **array một chiều**, nhưng bạn có thể mô phỏng array nhiều chiều bằng cách lồng array:

```go
var x [2][3]int
```

Câu lệnh này khai báo `x` là một array có độ dài 2, trong đó mỗi phần tử lại là một array `int` có độ dài 3.

Cách diễn đạt này nghe có vẻ hơi máy móc, nhưng là có chủ ý. Một số ngôn ngữ (như Fortran hay Julia) có hỗ trợ **ma trận thực sự**, còn Go thì không.

---

## Truy Cập Phần Tử Array

Giống như hầu hết các ngôn ngữ khác, bạn truy cập và gán giá trị cho array bằng **cú pháp dấu ngoặc vuông**:

```go
x[0] = 10
fmt.Println(x[2])
```

Bạn **không thể**:

* Truy cập chỉ số âm
* Truy cập vượt quá độ dài array

Nếu dùng chỉ số **hằng số hoặc literal**, lỗi sẽ được phát hiện **ngay khi compile**. Nếu dùng **biến** làm chỉ số, chương trình sẽ compile được nhưng **panic tại runtime** nếu vượt giới hạn (bạn sẽ tìm hiểu thêm về panic trong phần *panic and recover*).

---

## Độ Dài Array

Hàm built-in `len` trả về độ dài của array:

```go
fmt.Println(len(x))
```

---

## Vì Sao Array Ít Được Dùng Trong Go?

Như đã nói ở đầu phần, array hiếm khi được dùng trực tiếp trong Go. Nguyên nhân chính là vì **kích thước array là một phần của kiểu dữ liệu**.

Điều này có nghĩa là:

```go
[3]int
```

và

```go
[4]int
```

là **hai kiểu hoàn toàn khác nhau**.

Hệ quả:

* Bạn **không thể** dùng biến để xác định kích thước array (kiểu phải được xác định tại compile time)
* Bạn **không thể** convert trực tiếp array có kích thước khác nhau
* Không thể viết một hàm làm việc với array có kích thước bất kỳ
* Không thể gán array có kích thước khác nhau cho cùng một biến

> **Note**
> Bạn sẽ tìm hiểu cách array được tổ chức trong bộ nhớ khi học về **memory layout** ở Chương 6.

Do các hạn chế này, **đừng dùng array trừ khi bạn biết chính xác kích thước cần dùng ngay từ đầu**. Một ví dụ ngoại lệ là các hàm mật mã trong standard library — chúng trả về array vì kích thước checksum là một phần của thuật toán.

Ngoại lệ này là hiếm, không phải quy tắc chung.

---

## Vai Trò Thực Sự Của Array

Vậy tại sao Go vẫn có array dù chúng bị hạn chế như vậy?

Lý do chính là: **array tồn tại để làm backing store cho slice** — một trong những tính năng mạnh và quan trọng nhất của Go.

👉 Ngay phần tiếp theo, bạn sẽ học về **slice**, và lúc đó array mới thực sự phát huy vai trò của mình.

## Slices

Trong hầu hết các trường hợp, khi bạn cần một cấu trúc dữ liệu để chứa một dãy giá trị, **slice** là lựa chọn nên dùng. Điểm khiến slice trở nên cực kỳ hữu ích là bạn có thể **mở rộng slice khi cần**. Điều này là do **độ dài (length) của slice không phải là một phần của kiểu dữ liệu**. Nhờ đó, slice loại bỏ hạn chế lớn nhất của array và cho phép bạn viết một hàm duy nhất để xử lý slice với mọi kích thước khác nhau (phần viết hàm sẽ được trình bày ở Chương 5).

Sau khi nắm được các kiến thức cơ bản về slice trong Go, bạn sẽ thấy các best practice để sử dụng chúng hiệu quả.

---

### Khai báo và sử dụng slice

Làm việc với slice trông rất giống với array, nhưng có những khác biệt tinh tế. Điều đầu tiên cần chú ý là **bạn không chỉ định kích thước khi khai báo slice**:

```go
var x = []int{10, 20, 30}
```

> **TIP**
> Sử dụng `[...]` tạo **array**.
> Sử dụng `[]` tạo **slice**.

Dòng code trên tạo ra một slice gồm 3 số nguyên bằng slice literal. Tương tự array, bạn cũng có thể tạo slice thưa (sparse slice) bằng cách chỉ định các chỉ số có giá trị khác 0:

```go
var x = []int{1, 5: 4, 6, 10: 100, 15}
```

Slice này có độ dài 12 và giá trị:

```cmd
[1, 0, 0, 0, 0, 4, 6, 0, 0, 0, 100, 15]
```

Bạn có thể mô phỏng slice nhiều chiều bằng cách dùng slice của slice:

```go
var x [][]int
```

Việc đọc và ghi slice sử dụng cú pháp dấu ngoặc vuông giống array:

```go
x[0] = 10
fmt.Println(x[2])
```

Bạn **không thể truy cập vượt quá phạm vi** hoặc dùng chỉ số âm; nếu làm vậy, chương trình sẽ panic ở runtime.

---

### Slice nil

Bạn có thể khai báo slice mà không gán giá trị ban đầu:

```go
var x []int
```

Lúc này, `x` có **zero value là `nil`**. Đây là lần đầu bạn gặp một zero value khác `0` hay `""`.

`nil` đại diện cho việc **không có giá trị** đối với một số kiểu dữ liệu. Nó không có kiểu riêng, nên có thể so sánh với nhiều kiểu khác nhau.

Một slice `nil` không chứa bất kỳ phần tử nào.

---

### So sánh slice

Slice là kiểu dữ liệu **không thể so sánh trực tiếp**. Việc dùng `==` hoặc `!=` giữa hai slice sẽ gây lỗi biên dịch. Điều duy nhất bạn có thể so sánh slice bằng `==` là với `nil`:

```go
fmt.Println(x == nil) // true nếu x là nil slice
```

Kể từ Go 1.21, package `slices` cung cấp các hàm so sánh slice:

* `slices.Equal`: so sánh độ dài và từng phần tử (yêu cầu phần tử phải comparable)
* `slices.EqualFunc`: cho phép truyền vào hàm so sánh

```go
x := []int{1, 2, 3, 4, 5}
y := []int{1, 2, 3, 4, 5}
z := []int{1, 2, 3, 4, 5, 6}

fmt.Println(slices.Equal(x, y)) // true
fmt.Println(slices.Equal(x, z)) // false
```

> **WARNING**
> `reflect.DeepEqual` có thể so sánh slice nhưng là legacy, chậm và kém an toàn. Không nên dùng trong code mới.

---

### len

Hàm built-in `len` trả về độ dài của slice. Nếu truyền vào slice `nil`, `len` trả về `0`.

```go
fmt.Println(len(x))
```

> **NOTE**
> `len` là built-in vì nó hoạt động được với array, slice, string, map và channel – điều mà hàm do người dùng viết không thể làm được.

---

### append

Hàm `append` dùng để **mở rộng slice**:

```go
var x []int
x = append(x, 10)
```

`append` **luôn trả về một slice mới**, vì vậy bạn **bắt buộc phải gán lại kết quả**.

Bạn có thể append nhiều giá trị cùng lúc:

```go
x = append(x, 5, 6, 7)
```

Append một slice vào slice khác bằng toán tử `...`:

```go
y := []int{20, 30, 40}
x = append(x, y...)
```

Go là ngôn ngữ **call-by-value**. Khi truyền slice vào `append`, thực chất là truyền bản sao của slice, và `append` trả về slice mới sau khi mở rộng.

---

### Capacity

Ngoài `len`, slice còn có **capacity (dung lượng)** – số phần tử tối đa có thể chứa mà không cần cấp phát bộ nhớ mới.

* `len`: số phần tử đang có giá trị
* `cap`: số ô bộ nhớ được cấp phát liên tiếp

Khi `len` chạm `cap`, `append` sẽ yêu cầu Go runtime cấp phát mảng mới lớn hơn và copy dữ liệu cũ sang.

Quy tắc tăng dung lượng (Go 1.18+):

* `cap < 256`: nhân đôi
* `cap >= 256`: tăng dần, tiến tới ~25%

Ví dụ minh họa:

```go
var x []int
fmt.Println(x, len(x), cap(x))
```

> **TIP**
> Nếu biết trước số phần tử cần dùng, hãy cấp phát sẵn để tránh tốn chi phí copy.

---

### make

Hàm `make` dùng để tạo slice với độ dài và capacity xác định:

```go
x := make([]int, 5)
```

* `len = 5`, `cap = 5`
* Các phần tử được khởi tạo bằng zero value

Tạo slice có capacity lớn hơn length:

```go
x := make([]int, 5, 10)
```

Tạo slice length = 0 nhưng có capacity:

```go
x := make([]int, 0, 10)
x = append(x, 5, 6, 7, 8)
```

> **WARNING**
> Capacity không được nhỏ hơn length. Nếu làm vậy, chương trình sẽ lỗi biên dịch hoặc panic.

---

### clear

Go 1.21 bổ sung hàm `clear` để đặt tất cả phần tử của slice về zero value, **không thay đổi length**:

```go
s := []string{"first", "second", "third"}
clear(s)
```

---

### Cách khai báo slice hợp lý

* Slice có thể không dùng tới → dùng **nil slice**
* Có sẵn giá trị → dùng **slice literal**
* Biết trước dung lượng → dùng `make`
* Không chắc số lượng → `make` với `len = 0`, `cap > 0` rồi `append`

> **WARNING**
> `append` luôn tăng length. Hãy chắc chắn bạn thực sự muốn append.

---

### Slicing slices

Slice có thể được tạo từ slice khác:

```go
y := x[:2]
z := x[1:]
```

Slice **chia sẻ cùng vùng nhớ**. Thay đổi ở một slice sẽ ảnh hưởng slice còn lại.

Khi kết hợp slicing với `append`, tình huống càng phức tạp hơn. Để tránh ghi đè dữ liệu ngoài ý muốn, hãy dùng **full slice expression** với 3 tham số:

```go
y := x[:2:2]
z := x[2:4:4]
```

> **WARNING**
> Cẩn thận khi slice từ slice. Các slice có thể dùng chung bộ nhớ. Dùng full slice expression để tránh rắc rối khi `append`.

### copy: Tạo slice độc lập

Nếu bạn cần tạo một slice **độc lập hoàn toàn** với slice ban đầu (không chia sẻ vùng nhớ), hãy dùng hàm dựng sẵn `copy`.

Ví dụ đơn giản:

```go
x := []int{1, 2, 3, 4}
y := make([]int, 4)
num := copy(y, x)
fmt.Println(y, num)
```

Kết quả:

```cmd
[1 2 3 4] 4
```

Hàm `copy` nhận **hai tham số**:

1. Slice đích (destination)
2. Slice nguồn (source)

Hàm sẽ sao chép **tối đa số phần tử có thể**, bị giới hạn bởi **slice nào ngắn hơn**, và trả về **số phần tử đã được copy**. Lưu ý rằng **capacity không quan trọng**, chỉ **length** mới ảnh hưởng.

---

### Copy một phần của slice

Bạn có thể copy một phần của slice:

```go
x := []int{1, 2, 3, 4}
y := make([]int, 2)
num := copy(y, x)
```

Sau khi chạy:

* `y` = `[1 2]`
* `num` = `2`

Hoặc copy từ **giữa slice nguồn**:

```go
x := []int{1, 2, 3, 4}
y := make([]int, 2)
copy(y, x[2:])
```

Lúc này bạn đang copy phần tử thứ 3 và 4 của `x`.

Nếu bạn **không cần số phần tử được copy**, bạn không cần gán giá trị trả về của `copy`.

---

### Copy với slice chồng lấn vùng nhớ

Hàm `copy` cho phép copy giữa các slice **có vùng nhớ chồng lấn**:

```go
x := []int{1, 2, 3, 4}
num := copy(x[:3], x[1:])
fmt.Println(x, num)
```

Kết quả:

```cmd
[2 3 4 4] 3
```

Ở đây, ba phần tử cuối của `x` được copy đè lên ba phần tử đầu của chính nó.

---

### Copy giữa array và slice

Bạn có thể dùng `copy` với array bằng cách **lấy slice từ array**:

```go
x := []int{1, 2, 3, 4}
d := [4]int{5, 6, 7, 8}
y := make([]int, 2)

copy(y, d[:])
fmt.Println(y)

copy(d[:], x)
fmt.Println(d)
```

Kết quả:

```cmd
[5 6]
[1 2 3 4]
```

* Lần copy đầu: copy từ array `d` sang slice `y`
* Lần copy sau: copy từ slice `x` sang array `d`

---

### Chuyển Array → Slice

Bạn có thể lấy slice từ array bằng **slice expression**.

Chuyển toàn bộ array thành slice:

```go
xArray := [4]int{5, 6, 7, 8}
xSlice := xArray[:]
```

Hoặc chỉ lấy một phần:

```go
x := [4]int{5, 6, 7, 8}
y := x[:2]
z := x[2:]
```

⚠️ **Cảnh báo quan trọng**: Slice lấy từ array **chia sẻ cùng vùng nhớ**.

Ví dụ:

```go
x := [4]int{5, 6, 7, 8}
y := x[:2]
z := x[2:]

x[0] = 10
fmt.Println("x:", x)
fmt.Println("y:", y)
fmt.Println("z:", z)
```

Kết quả:

```cmd
x: [10 6 7 8]
y: [10 6]
z: [7 8]
```

---

### Chuyển Slice → Array

Bạn có thể dùng **type conversion** để tạo array từ slice:

```go
xSlice := []int{1, 2, 3, 4}
xArray := [4]int(xSlice)
smallArray := [2]int(xSlice)

xSlice[0] = 10
fmt.Println(xSlice)
fmt.Println(xArray)
fmt.Println(smallArray)
```

Kết quả:

```cmd
[10 2 3 4]
[1 2 3 4]
[1 2]
```

📌 Khi chuyển slice → array:

* **Dữ liệu được copy sang vùng nhớ mới**
* Thay đổi slice **không ảnh hưởng** đến array và ngược lại

---

### Lỗi runtime khi kích thước array không hợp lệ

* Kích thước array **phải biết tại compile time**
* Không được dùng `[...]` khi chuyển từ slice sang array

⚠️ Nếu kích thước array **lớn hơn length của slice**, chương trình sẽ **panic ở runtime**:

```go
panicArray := [5]int(xSlice)
fmt.Println(panicArray)
```

Lỗi:

```cmd
panic: runtime error: cannot convert slice with length 4 to array
     or pointer to array with length 5
```

---

### Slice → con trỏ array (array pointer)

> (Phần này dùng pointer, sẽ học chi tiết ở Chương 6)

Bạn có thể chuyển slice thành **con trỏ tới array**:

```go
xSlice := []int{1, 2, 3, 4}
xArrayPointer := (*[4]int)(xSlice)
```

Sau khi chuyển:

* Slice và array pointer **chia sẻ cùng vùng nhớ**

```go
xSlice[0] = 10
xArrayPointer[1] = 20
fmt.Println(xSlice)
fmt.Println(xArrayPointer)
```

Kết quả:

```cmd
[10 20 3 4]
&[10 20 3 4]
```

---

### Lời khuyên thiết kế API

Ở phần *Arrays—Too Rigid to Use Directly*, bạn đã thấy rằng array không phù hợp làm tham số hàm khi kích thước có thể thay đổi.

Về mặt kỹ thuật, bạn **có thể**:

1. Chuyển array → slice
2. Chuyển slice → array có kích thước nhỏ hơn
3. Truyền array đó vào hàm

Tuy nhiên:

* Rất dễ panic
* Code khó đọc
* Không idiomatic

👉 **Nếu bạn thấy mình làm điều này thường xuyên, hãy đổi API của hàm để nhận `slice` thay vì `array`.**

## Strings trong Go được lưu trữ như thế nào

Sau khi đã tìm hiểu về slice, giờ chúng ta quay lại với **string**. Nhiều người nghĩ rằng string trong Go được cấu thành từ các **rune**, nhưng thực tế **string trong Go là một chuỗi các byte**.

Bên dưới, Go chỉ lưu string như một dãy byte. Các byte này *không bắt buộc* phải thuộc một bảng mã cụ thể, nhưng phần lớn thư viện chuẩn của Go (và vòng lặp `for-range`) **giả định string là UTF-8**.

> **NOTE**
> Theo đặc tả ngôn ngữ, mã nguồn Go luôn được viết bằng UTF-8. Trừ khi bạn dùng escape hex, mọi string literal đều là UTF-8.

---

## Truy cập ký tự trong string bằng index

Tương tự array và slice, bạn có thể truy cập từng phần tử của string bằng index:

```go
var s string = "Hello there"
var b byte = s[6]
```

* Index bắt đầu từ 0
* `b` nhận giá trị **116**, là mã UTF-8 của ký tự `'t'`

⚠️ Giá trị bạn lấy được là **byte**, không phải rune.

---

## Cắt chuỗi (string slicing)

Cú pháp slice cũng hoạt động với string:

```go
var s string = "Hello there"
var s2 string = s[4:7]
var s3 string = s[:5]
var s4 string = s[6:]
```

Kết quả:

* `s2 = "o t"`
* `s3 = "Hello"`
* `s4 = "there"`

---

## Cảnh báo: UTF-8 và byte indexing

Vấn đề phát sinh khi string chứa **ký tự nhiều byte**, ví dụ emoji hoặc ngôn ngữ không phải tiếng Anh:

```go
var s string = "Hello ☀️"
var s2 string = s[4:7]
```

* `s3` vẫn là `"Hello"`
* `s4` là emoji mặt trời
* **`s2` bị lỗi nội dung**, vì bạn chỉ cắt được **một phần byte** của emoji

📌 UTF-8 có thể dùng **1–4 byte cho một code point**.

---

## len(string)

Hàm `len` trả về **số byte**, không phải số ký tự:

```go
var s string = "Hello ☀️"
fmt.Println(len(s))
```

Kết quả là `10`, không phải `7`.

⚠️ **Không dùng `len` để đếm số ký tự hiển thị**.

---

## Chuyển đổi giữa rune, byte và string

### Rune / Byte → String

```go
var a rune   = 'x'
var s string = string(a)

var b byte   = 'y'
var s2 string = string(b)
```

### ⚠️ Bug phổ biến với int → string

```go
var x int = 65
var y = string(x)
fmt.Println(y)
```

👉 Kết quả là **"A"**, không phải "65".

Từ Go 1.15, `go vet` **chặn** việc convert `int → string` (trừ `rune` và `byte`).

---

## String ↔ Slice

### String → []byte / []rune

```go
var s string = "Hello, ☀️"
var bs []byte = []byte(s)
var rs []rune = []rune(s)
```

Kết quả:

```text
[72 101 108 108 111 44 32 240 159 140 158]
[72 101 108 108 111 44 32 127774]
```

* `[]byte`: biểu diễn UTF-8
* `[]rune`: biểu diễn code point

📌 Trong thực tế, **[]byte được dùng nhiều hơn []rune**.

---

## UTF-8 là gì?

Unicode định nghĩa mỗi ký tự bằng **32 bit** (UTF-32) → rất tốn bộ nhớ.

Các dạng encoding:

* **UTF-32**: 4 byte / ký tự (rất lãng phí)
* **UTF-16**: 2 hoặc 4 byte
* **UTF-8**: 1–4 byte (phổ biến nhất)

### Vì sao UTF-8 tốt?

* ASCII (<128) chỉ cần **1 byte**
* Không phụ thuộc endian
* Có thể phát hiện byte đang ở đầu hay giữa ký tự
* Tối đa vẫn chỉ 4 byte

❌ Nhược điểm: **không thể random access theo ký tự**, phải duyệt từ đầu.

> Fun fact: UTF-8 được phát minh năm 1992 bởi **Ken Thompson** và **Rob Pike** – hai người tạo ra Go.

---

## Best practice khi làm việc với string

❌ Tránh dùng index/slice trực tiếp với string UTF-8

✅ Nên dùng:

* `strings` package
* `unicode/utf8` package
* `for-range` để duyệt rune

Trong chương tiếp theo, bạn sẽ học cách dùng `for-range` để duyệt code point một cách an toàn.

