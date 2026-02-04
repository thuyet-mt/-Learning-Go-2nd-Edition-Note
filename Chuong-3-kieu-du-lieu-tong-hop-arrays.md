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

## Maps

Slices rất hữu ích khi bạn có dữ liệu tuần tự. Giống như hầu hết các ngôn ngữ khác, Go cung cấp một kiểu dữ liệu dựng sẵn cho những tình huống mà bạn muốn liên kết một giá trị với một giá trị khác. Kiểu map được viết dưới dạng `map[keyType]valueType`. Hãy cùng xem một vài cách khai báo map.

Trước tiên, bạn có thể dùng khai báo `var` để tạo một biến map được gán giá trị zero của nó:

```go
var nilMap map[string]int
```

Trong trường hợp này, `nilMap` được khai báo là một map với key kiểu `string` và value kiểu `int`. Giá trị zero của map là `nil`. Một map `nil` có độ dài bằng 0. Việc đọc từ một map `nil` luôn trả về giá trị zero của kiểu value của map. Tuy nhiên, nếu cố ghi dữ liệu vào một biến map `nil` thì chương trình sẽ panic.

Bạn có thể dùng khai báo `:=` để tạo một biến map bằng cách gán cho nó một map literal:

```go
totalWins := map[string]int{}
```

Ở đây, bạn đang dùng một map literal rỗng. Điều này **không giống** với một map `nil`. Nó có độ dài bằng 0, nhưng bạn có thể đọc và ghi dữ liệu vào một map được gán bằng map literal rỗng. Dưới đây là một ví dụ về map literal không rỗng:

```go
teams := map[string][]string{
    "Orcas":   []string{"Fred", "Ralph", "Bijou"},
    "Lions":   []string{"Sarah", "Peter", "Billie"},
    "Kittens": []string{"Waldo", "Raul", "Ze"},
}
```

Phần thân của một map literal được viết bằng key, theo sau là dấu hai chấm (`:`), rồi đến value. Mỗi cặp key–value được phân tách bằng dấu phẩy, kể cả dòng cuối cùng. Trong ví dụ này, value là một slice các chuỗi. Kiểu của value trong map có thể là bất cứ thứ gì. Tuy nhiên, kiểu của key có một số hạn chế, sẽ được nói đến sau.

Nếu bạn biết trước số lượng cặp key–value mà bạn sẽ đưa vào map nhưng chưa biết giá trị cụ thể, bạn có thể dùng `make` để tạo map với kích thước ban đầu:

```go
ages := make(map[int][]string, 10)
```

Các map được tạo bằng `make` vẫn có độ dài bằng 0, và chúng có thể tự động mở rộng vượt quá kích thước ban đầu đã chỉ định.

Maps giống slices ở một số điểm:

* Maps tự động mở rộng khi bạn thêm các cặp key–value.
* Nếu bạn biết số lượng cặp key–value sẽ chèn vào map, bạn có thể dùng `make` để tạo map với kích thước ban đầu cụ thể.
* Truyền một map vào hàm `len` sẽ cho bạn biết số lượng cặp key–value trong map.
* Giá trị zero của map là `nil`.
* Maps **không thể so sánh**. Bạn có thể kiểm tra xem chúng có bằng `nil` hay không, nhưng bạn không thể dùng `==` để kiểm tra hai map có cùng key và value hay dùng `!=` để kiểm tra sự khác nhau.

Key của map có thể là bất kỳ kiểu nào **có thể so sánh được**. Điều này có nghĩa là bạn **không thể** dùng slice hoặc map làm key của một map khác.

> **TIP**
> Khi nào nên dùng map, và khi nào nên dùng slice?
> Bạn nên dùng slice cho các danh sách dữ liệu khi dữ liệu cần được xử lý tuần tự hoặc khi thứ tự của các phần tử là quan trọng.
>
> Map hữu ích khi bạn cần tổ chức các giá trị dựa trên một thứ gì đó **không phải** là một số nguyên tăng dần, chẳng hạn như tên.

---

### Map băm (Hash Map) là gì?

Trong khoa học máy tính, map là một cấu trúc dữ liệu dùng để liên kết (hoặc ánh xạ) một giá trị với một giá trị khác. Map có thể được cài đặt theo nhiều cách khác nhau, mỗi cách có những đánh đổi riêng. Map được tích hợp sẵn trong Go là một **hash map** (hay **hash table**).

Nếu bạn chưa quen với khái niệm này, Chương 5 của cuốn *Grokking Algorithms* (Aditya Bhargava, Manning) mô tả hash table là gì và vì sao chúng lại hữu ích đến vậy.

Việc Go cung cấp sẵn một cài đặt hash map trong runtime là rất tuyệt vời, bởi vì tự xây dựng một hash map đúng đắn là việc khá khó. Nếu bạn muốn tìm hiểu sâu hơn về cách Go hiện thực map, hãy xem bài nói chuyện **“Inside the Map Implementation”** tại GopherCon 2016 của Keith Randall.

Go không yêu cầu (và thậm chí không cho phép) bạn tự định nghĩa thuật toán băm hay cách so sánh bằng nhau. Thay vào đó, Go runtime được biên dịch vào mỗi chương trình Go đã chứa sẵn mã cài đặt các thuật toán băm cho mọi kiểu được phép làm key.

---

### Đọc và ghi Map

Hãy xem một chương trình ngắn khai báo, ghi và đọc dữ liệu từ map. Bạn có thể chạy chương trình trong Ví dụ 3-10 trên Go Playground hoặc trong thư mục `sample_code/map_read_write` của Chapter 3.

**Ví dụ 3-10. Sử dụng map**

```go
totalWins := map[string]int{}
totalWins["Orcas"] = 1
totalWins["Lions"] = 2
fmt.Println(totalWins["Orcas"])
fmt.Println(totalWins["Kittens"])
totalWins["Kittens"]++
fmt.Println(totalWins["Kittens"])
totalWins["Lions"] = 3
fmt.Println(totalWins["Lions"])
```

Khi chạy chương trình, bạn sẽ thấy kết quả sau:

```
1
0
1
3
```

Bạn gán giá trị cho một key trong map bằng cách đặt key trong dấu ngoặc vuông và dùng `=` để chỉ định giá trị. Bạn đọc giá trị của một key cũng bằng cách đặt key trong dấu ngoặc vuông. Lưu ý rằng bạn **không thể** dùng `:=` để gán giá trị cho một key trong map.

Khi bạn đọc giá trị của một key chưa từng được gán, map sẽ trả về **giá trị zero** của kiểu value. Trong ví dụ này, kiểu value là `int`, nên bạn nhận được `0`. Bạn có thể dùng toán tử `++` để tăng giá trị số của một key trong map. Vì map mặc định trả về giá trị zero, nên điều này vẫn hoạt động ngay cả khi trước đó key chưa tồn tại.

---

### Idiom “comma ok”

Như bạn đã thấy, map trả về giá trị zero khi bạn truy cập một key không tồn tại. Điều này rất tiện khi triển khai các bộ đếm như `totalWins`. Tuy nhiên, đôi khi bạn cần biết **liệu một key có tồn tại trong map hay không**. Go cung cấp idiom *comma ok* để phân biệt giữa một key có giá trị zero và một key không tồn tại:

```go
m := map[string]int{
    "hello": 5,
    "world": 0,
}
v, ok := m["hello"]
fmt.Println(v, ok)

v, ok = m["world"]
fmt.Println(v, ok)

v, ok = m["goodbye"]
fmt.Println(v, ok)
```

Thay vì gán kết quả đọc map cho một biến duy nhất, với idiom *comma ok* bạn gán cho **hai biến**. Biến thứ nhất nhận giá trị gắn với key. Biến thứ hai là một giá trị `bool`, thường được đặt tên là `ok`. Nếu `ok` là `true`, key tồn tại trong map. Nếu `ok` là `false`, key không tồn tại. Trong ví dụ này, chương trình in ra: `5 true`, `0 true`, và `0 false`.

> **NOTE**
> Idiom *comma ok* được dùng trong Go khi bạn muốn phân biệt giữa việc đọc được giá trị thực sự và việc nhận về giá trị zero. Bạn sẽ gặp lại nó khi đọc từ channel trong Chapter 12 và khi dùng type assertion trong Chapter 7.

---

### Xóa phần tử khỏi Map

Các cặp key–value được xóa khỏi map bằng hàm dựng sẵn `delete`:

```go
m := map[string]int{
    "hello": 5,
    "world": 10,
}
delete(m, "hello")
```

Hàm `delete` nhận vào một map và một key, rồi xóa cặp key–value tương ứng. Nếu key không tồn tại trong map hoặc map là `nil`, thì không có gì xảy ra. Hàm `delete` không trả về giá trị nào.

---

### Làm rỗng Map

Hàm `clear` mà bạn đã thấy trong phần “Làm rỗng Slice” cũng hoạt động với map. Một map sau khi được clear sẽ có độ dài bằng 0 (khác với slice). Đoạn code sau:

```go
m := map[string]int{
    "hello": 5,
    "world": 10,
}
fmt.Println(m, len(m))
clear(m)
fmt.Println(m, len(m))
```

sẽ in ra:

```
map[hello:5 world:10] 2
map[] 0
```

---

### So sánh Map

Go 1.21 đã thêm một package mới vào thư viện chuẩn tên là `maps`, chứa các hàm hỗ trợ làm việc với map. Bạn sẽ tìm hiểu thêm về package này trong phần “Adding Generics to the Standard Library”. Hai hàm hữu ích để so sánh hai map có bằng nhau hay không là `maps.Equal` và `maps.EqualFunc`, tương tự như `slices.Equal` và `slices.EqualFunc`:

```go
m := map[string]int{
    "hello": 5,
    "world": 10,
}
n := map[string]int{
    "world": 10,
    "hello": 5,
}
fmt.Println(maps.Equal(m, n)) // in ra true
```

---

### Dùng Map như Set

Nhiều ngôn ngữ có kiểu dữ liệu `set` trong thư viện chuẩn. Set đảm bảo rằng mỗi giá trị chỉ xuất hiện tối đa một lần, nhưng không đảm bảo thứ tự. Việc kiểm tra một phần tử có nằm trong set hay không là rất nhanh, bất kể set có bao nhiêu phần tử. (Trong khi đó, kiểm tra trong slice sẽ chậm dần khi slice lớn lên.)

Go không có sẵn kiểu `set`, nhưng bạn có thể dùng map để mô phỏng một số tính năng của nó. Hãy dùng key của map làm kiểu dữ liệu bạn muốn đưa vào set và dùng `bool` làm value. Ví dụ 3-11 minh họa ý tưởng này.

**Ví dụ 3-11. Sử dụng map như một set**

```go
intSet := map[int]bool{}
vals := []int{5, 10, 2, 5, 8, 7, 3, 9, 1, 2, 10}
for _, v := range vals {
    intSet[v] = true
}
fmt.Println(len(vals), len(intSet))
fmt.Println(intSet[5])
fmt.Println(intSet[500])
if intSet[100] {
    fmt.Println("100 is in the set")
}
```

Bạn muốn một set các số nguyên, nên bạn tạo một map với key kiểu `int` và value kiểu `bool`. Bạn lặp qua các giá trị trong `vals` bằng vòng lặp `for-range` (sẽ được nói trong phần “The for-range Statement”) và đưa chúng vào `intSet`, gán mỗi số nguyên với giá trị `true`.

Bạn đã ghi 11 giá trị vào `intSet`, nhưng độ dài của `intSet` chỉ là 8, vì map không thể có key trùng lặp. Khi bạn kiểm tra `5` trong `intSet`, kết quả là `true` vì có key bằng 5. Tuy nhiên, khi kiểm tra `500` hoặc `100`, kết quả là `false`. Điều này xảy ra vì bạn chưa đưa các giá trị đó vào `intSet`, khiến map trả về giá trị zero của kiểu `bool`, mà giá trị zero của `bool` là `false`.

Nếu bạn cần các phép toán trên set như hợp (union), giao (intersection) hay hiệu (subtraction), bạn có thể tự cài đặt hoặc dùng một trong nhiều thư viện bên thứ ba có sẵn. (Bạn sẽ tìm hiểu thêm về việc dùng thư viện bên thứ ba trong Chapter 10.)

> **NOTE**
> Một số người thích dùng `struct{}` làm value khi map được dùng để cài đặt set. (Struct sẽ được nói trong phần tiếp theo.) Ưu điểm là một struct rỗng chiếm 0 byte bộ nhớ, trong khi một `bool` chiếm 1 byte.
>
> Nhược điểm là việc dùng `struct{}` làm code trở nên rườm rà hơn. Cách gán giá trị kém trực quan hơn, và bạn cần dùng idiom *comma ok* để kiểm tra một giá trị có nằm trong set hay không:
>
> ```go
> intSet := map[int]struct{}{}
> vals := []int{5, 10, 2, 5, 8, 7, 3, 9, 1, 2, 10}
> for _, v := range vals {
>     intSet[v] = struct{}{}
> }
> if _, ok := intSet[5]; ok {
>     fmt.Println("5 is in the set")
> }
> ```
>
> Trừ khi bạn có các set rất lớn, sự khác biệt về bộ nhớ thường không đủ đáng kể để bù đắp cho những nhược điểm này.

## Structs

Map là một cách tiện lợi để lưu trữ một số loại dữ liệu, nhưng chúng cũng có những hạn chế. Map không định nghĩa được một API, vì không có cách nào để giới hạn map chỉ cho phép một tập key nhất định. Ngoài ra, tất cả các value trong map phải có cùng một kiểu. Vì những lý do này, map không phải là cách lý tưởng để truyền dữ liệu từ hàm này sang hàm khác. Khi bạn có các dữ liệu liên quan và muốn gom chúng lại với nhau, bạn nên định nghĩa một struct.

> **NOTE**
> Nếu bạn đã quen với một ngôn ngữ hướng đối tượng, bạn có thể thắc mắc sự khác biệt giữa class và struct là gì. Câu trả lời rất đơn giản: Go không có class, vì Go không có cơ chế kế thừa. Điều này không có nghĩa là Go thiếu các đặc tính của lập trình hướng đối tượng, mà chỉ là Go tiếp cận chúng theo cách khác. Bạn sẽ tìm hiểu thêm về các đặc điểm hướng đối tượng của Go trong Chapter 7.

Hầu hết các ngôn ngữ đều có một khái niệm tương tự struct, và cú pháp mà Go dùng để đọc và ghi struct sẽ khá quen thuộc:

```go
type person struct {
    name string
    age  int
    pet  string
}
```

Một kiểu struct được định nghĩa bằng từ khóa `type`, tên kiểu struct, từ khóa `struct`, và một cặp dấu ngoặc nhọn (`{}`). Bên trong ngoặc, bạn liệt kê các field của struct. Tương tự như khai báo biến với `var`, bạn viết tên field trước và kiểu của field sau. Lưu ý rằng, khác với map literal, **không có dấu phẩy** ngăn cách các field trong phần khai báo struct.

Bạn có thể định nghĩa một kiểu struct bên trong hoặc bên ngoài một hàm. Một kiểu struct được định nghĩa bên trong hàm chỉ có thể được sử dụng trong phạm vi hàm đó. (Bạn sẽ tìm hiểu thêm về hàm trong Chapter 5.)

> **NOTE**
> Về mặt kỹ thuật, bạn có thể giới hạn phạm vi của định nghĩa struct ở bất kỳ mức block nào. Bạn sẽ tìm hiểu thêm về block trong Chapter 4.

Khi một kiểu struct đã được khai báo, bạn có thể tạo biến thuộc kiểu đó:

```go
var fred person
```

Ở đây ta dùng khai báo `var`. Vì không gán giá trị cho `fred`, nó sẽ nhận **giá trị zero** của kiểu struct `person`. Một struct ở giá trị zero có tất cả các field được gán giá trị zero tương ứng với kiểu của field đó.

Bạn cũng có thể gán một struct literal cho biến:

```go
bob := person{}
```

Khác với map, không có sự khác biệt giữa việc gán một struct literal rỗng và việc không gán giá trị gì cả. Cả hai cách đều khởi tạo tất cả các field của struct về giá trị zero. Có hai cách để khởi tạo struct literal không rỗng.

Cách thứ nhất là cung cấp một danh sách các giá trị, phân tách bằng dấu phẩy, tương ứng với các field trong struct:

```go
julia := person{
    "Julia",
    40,
    "cat",
}
```

Khi dùng kiểu struct literal này, bạn **bắt buộc** phải cung cấp giá trị cho tất cả các field trong struct, và các giá trị sẽ được gán theo đúng **thứ tự** các field được khai báo trong định nghĩa struct.

Cách thứ hai trông giống với map literal:

```go
beth := person{
    age:  30,
    name: "Beth",
}
```

Bạn sử dụng tên field để chỉ định giá trị. Cách này có một số ưu điểm: bạn có thể chỉ định field theo bất kỳ thứ tự nào, và bạn không cần phải cung cấp giá trị cho tất cả các field. Những field không được chỉ định sẽ nhận giá trị zero.

Bạn **không thể trộn lẫn** hai kiểu struct literal: hoặc là tất cả các field đều có tên, hoặc là không field nào có tên. Với các struct nhỏ, nơi mọi field luôn được khởi tạo, kiểu không dùng tên field là đủ đơn giản. Trong các trường hợp khác, bạn nên dùng kiểu có tên field. Dù dài dòng hơn, nhưng nó giúp code rõ ràng hơn: bạn biết chính xác giá trị nào được gán cho field nào mà không cần nhìn lại định nghĩa struct. Nó cũng dễ bảo trì hơn. Nếu bạn khởi tạo struct mà không dùng tên field, và sau này struct được bổ sung thêm field mới, code của bạn sẽ **không biên dịch được**.

Một field trong struct được truy cập bằng ký hiệu dấu chấm:

```go
bob.name = "Bob"
fmt.Println(bob.name)
```

Cũng giống như bạn dùng dấu ngoặc vuông để đọc và ghi map, bạn dùng ký hiệu dấu chấm để đọc và ghi các field của struct.

---

### Struct Ẩn Danh (Anonymous Struct)

Bạn cũng có thể khai báo một biến với một kiểu struct mà không cần đặt tên cho kiểu đó. Điều này được gọi là **anonymous struct**:

```go
var person struct {
    name string
    age  int
    pet  string
}

person.name = "bob"
person.age = 50
person.pet = "dog"
```

Bạn cũng có thể khởi tạo anonymous struct bằng struct literal:

```go
pet := struct {
    name string
    kind string
}{
    name: "Fido",
    kind: "dog",
}
```

Trong ví dụ này, kiểu của các biến `person` và `pet` là anonymous struct. Bạn gán và đọc các field của anonymous struct giống hệt như với struct có tên. Tương tự, bạn cũng có thể khởi tạo một instance của anonymous struct bằng struct literal.

Bạn có thể thắc mắc khi nào thì một kiểu dữ liệu chỉ gắn với duy nhất một instance lại hữu ích. Anonymous struct thường được dùng trong hai tình huống phổ biến.

Trường hợp thứ nhất là khi bạn chuyển đổi dữ liệu bên ngoài sang struct hoặc ngược lại (ví dụ như JSON hoặc Protocol Buffers). Quá trình này lần lượt được gọi là **unmarshaling** và **marshaling**. Bạn sẽ học cách làm điều này trong phần `encoding/json`.

Trường hợp thứ hai là khi viết test. Bạn sẽ sử dụng slice của các anonymous struct khi viết **table-driven tests** trong Chapter 15.

---

### So sánh và Chuyển đổi Struct

Một struct có thể so sánh được hay không phụ thuộc vào các field của nó. Struct mà tất cả các field đều là kiểu có thể so sánh thì bản thân struct đó cũng có thể so sánh. Ngược lại, struct có field là slice hoặc map thì không thể so sánh (như bạn sẽ thấy ở các chương sau, field kiểu function hoặc channel cũng khiến struct không thể so sánh).

Khác với Python hay Ruby, Go không có “phương thức ma thuật” nào cho phép bạn định nghĩa lại cách so sánh bằng nhau để `==` và `!=` hoạt động với các struct không thể so sánh. Dĩ nhiên, bạn vẫn có thể tự viết hàm để so sánh struct theo ý mình.

Tương tự như việc Go không cho phép so sánh giữa các kiểu nguyên thủy khác nhau, Go cũng không cho phép so sánh giữa các biến thuộc **hai kiểu struct khác nhau**. Tuy nhiên, Go cho phép bạn **chuyển kiểu (type conversion)** từ một kiểu struct sang kiểu struct khác nếu các field của chúng có **cùng tên, cùng thứ tự và cùng kiểu**.

Ví dụ, với struct sau:

```go
type firstPerson struct {
    name string
    age  int
}
```

bạn có thể chuyển một instance của `firstPerson` sang `secondPerson`, nhưng bạn **không thể** dùng `==` để so sánh một instance của `firstPerson` và một instance của `secondPerson`, vì chúng là hai kiểu khác nhau:

```go
type secondPerson struct {
    name string
    age  int
}
```

Bạn không thể chuyển một instance của `firstPerson` sang `thirdPerson`, vì thứ tự các field khác nhau:

```go
type thirdPerson struct {
    age  int
    name string
}
```

Bạn cũng không thể chuyển sang `fourthPerson`, vì tên field không khớp:

```go
type fourthPerson struct {
    firstName string
    age       int
}
```

Cuối cùng, bạn không thể chuyển sang `fifthPerson`, vì có thêm một field mới:

```go
type fifthPerson struct {
    name          string
    age           int
    favoriteColor string
}
```

Anonymous struct có một điểm đặc biệt: nếu hai biến struct được so sánh và **ít nhất một trong hai** là anonymous struct, bạn có thể so sánh chúng **mà không cần chuyển kiểu**, miễn là các field của cả hai struct có cùng tên, cùng thứ tự và cùng kiểu. Bạn cũng có thể gán giá trị qua lại giữa struct có tên và anonymous struct trong cùng điều kiện đó:

```go
type firstPerson struct {
    name string
    age  int
}
f := firstPerson{
    name: "Bob",
    age:  50,
}
var g struct {
    name string
    age  int
}

// biên dịch được — có thể dùng = và == giữa struct có tên và anonymous struct giống hệt nhau
g = f
fmt.Println(f == g)
```

---

### Bài tập

Các bài tập sau sẽ kiểm tra kiến thức của bạn về các kiểu dữ liệu composite trong Go. Bạn có thể tìm lời giải trong thư mục `exercise_solutions` của Chapter 3 Repository.

1. Viết một chương trình định nghĩa một biến tên `greetings` có kiểu là slice các chuỗi, với các giá trị sau:
   `"Hello"`, `"Hola"`, `"नमस्कार"`, `"こんにちは"`, và `"Привіт"`.
   Tạo:

   * một subslice chứa hai giá trị đầu tiên,
   * một subslice thứ hai chứa giá trị thứ hai, thứ ba và thứ tư,
   * một subslice thứ ba chứa giá trị thứ tư và thứ năm.
     In ra cả bốn slice.

2. Viết một chương trình định nghĩa một biến chuỗi tên `message` với giá trị `"Hi 🌍"` và in ra **rune thứ tư** trong chuỗi này dưới dạng **ký tự**, không phải số.

3. Viết một chương trình định nghĩa một struct tên `Employee` với ba field: `firstName`, `lastName`, và `id`.
   Hai field đầu có kiểu `string`, field cuối (`id`) có kiểu `int`.
   Tạo ba instance của struct này với các giá trị tùy ý:

   * Instance thứ nhất được khởi tạo bằng struct literal **không dùng tên field**
   * Instance thứ hai được khởi tạo bằng struct literal **có dùng tên field**
   * Instance thứ ba được khai báo bằng `var`, sau đó dùng ký hiệu dấu chấm để gán giá trị cho các field
     In ra cả ba struct.

---

### Tổng kết

Bạn đã học được rất nhiều về các kiểu dữ liệu composite trong Go. Ngoài việc hiểu rõ hơn về string, bạn còn biết cách sử dụng các kiểu container dựng sẵn có hỗ trợ generic: slice và map. Bạn cũng đã biết cách tự xây dựng các kiểu dữ liệu composite của riêng mình thông qua struct.

Trong chương tiếp theo, bạn sẽ tìm hiểu về các cấu trúc điều khiển trong Go: `for`, `if/else`, và `switch`. Bạn cũng sẽ học cách Go tổ chức code thành các block, và cách các mức block khác nhau có thể dẫn đến những hành vi bất ngờ.
