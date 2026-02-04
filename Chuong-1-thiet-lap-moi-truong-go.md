# Chương 1. Thiết Lập Môi Trường Go

Mỗi ngôn ngữ lập trình đều cần một môi trường phát triển, và Go cũng không ngoại lệ. Nếu bạn đã từng viết một hoặc hai chương trình Go, thì bạn đã có một môi trường hoạt động được, nhưng có thể bạn đã bỏ lỡ một số kỹ thuật và công cụ mới hơn. Nếu đây là lần đầu tiên bạn cài đặt Go trên máy tính, đừng lo lắng; việc cài đặt Go và các công cụ hỗ trợ của nó khá đơn giản. Sau khi thiết lập và xác minh môi trường, bạn sẽ xây dựng một chương trình đơn giản, tìm hiểu các cách khác nhau để build và chạy mã Go, rồi khám phá một số công cụ và kỹ thuật giúp việc phát triển Go trở nên dễ dàng hơn.

---

## Cài Đặt Các Công Cụ Go

Để build mã Go, bạn cần tải và cài đặt bộ công cụ phát triển Go. Bạn có thể tìm phiên bản mới nhất trên trang tải về của website Go. Chọn bản phù hợp với nền tảng của bạn và cài đặt nó. Trình cài đặt `.pkg` cho macOS và `.msi` cho Windows sẽ tự động cài Go vào đúng vị trí, gỡ bỏ các bản cũ và thêm file thực thi `go` vào đường dẫn mặc định.

> **MẸO**
> 
> * macOS (Homebrew): `brew install go`
> * Windows (Chocolatey): `choco install golang`

### Linux / BSD

Các bản cài đặt cho Linux và BSD là file TAR nén gzip:

```bash
$ tar -C /usr/local -xzf go1.20.5.linux-amd64.tar.gz
$ echo 'export PATH=$PATH:/usr/local/go/bin' >> $HOME/.bash_profile
$ source $HOME/.bash_profile
```

Nếu cần quyền root:

```bash
sudo tar -C /usr/local -xzf go1.20.5.linux-amd64.tar.gz
```

> **LƯU Ý**
> Chương trình Go được biên dịch thành một file nhị phân native duy nhất, không cần cài thêm runtime. Điều này giúp việc phân phối dễ dàng hơn so với Java, Python hay JavaScript.

---

## Kiểm Tra Cài Đặt Go

```bash
$ go version
```

Ví dụ:

```text
go version go1.20.5 darwin/arm64
go version go1.20.5 linux/amd64
```

---

## Xử Lý Sự Cố Khi Cài Đặt

* Kiểm tra `PATH`:

```bash
which go
```

* Đảm bảo đúng kiến trúc CPU (32-bit / 64-bit)

---

## Hệ Thống Công Cụ Go

Các công cụ phổ biến:

* `go build` – biên dịch
* `go fmt` – định dạng mã
* `go mod` – quản lý dependency
* `go test` – chạy test
* `go vet` – phát hiện lỗi tiềm ẩn

> **LƯU Ý**
> Với Go hiện đại, bạn có thể tổ chức project ở bất kỳ đâu; không cần quan tâm `GOPATH`, `GOROOT`.

---

## Chương Trình Go Đầu Tiên

### Tạo Go Module

```bash
$ mkdir ch1
$ cd ch1
$ go mod init hello_world
```

File `go.mod`:

```go
module hello_world

go 1.20
```

---

## Viết Chương Trình Hello World

`hello.go`:

```go
package main

import "fmt"

func main() {
fmt.Println("Hello, world!")
}
```

Build và chạy:

```bash
$ go build
$ ./hello_world
```

Đổi tên file nhị phân:

```bash
$ go build -o hello
```

---

## go fmt

```bash
$ go fmt ./...
```

> **MẸO**
> Luôn chạy `go fmt` trước khi commit.

---

## Quy Tắc Tự Chèn Dấu Chấm Phẩy

Go tự động chèn dấu `;` theo quy tắc lexer. Vì vậy, dấu `{` **phải nằm cùng dòng** với khai báo hàm hoặc lệnh mở khối.

Ví dụ sai:

```go
func main()
{
    fmt.Println("Hello")
}
```

---

## go vet

Ví dụ lỗi:

```go
fmt.Printf("Hello, %s!\n")
```

Chạy:

```bash
$ go vet ./...
```

Sửa lỗi:

```go
fmt.Printf("Hello, %s!\n", "world")
```

---

## Chọn Công Cụ Phát Triển

### Visual Studio Code

* Miễn phí
* Cài extension Go
* Sử dụng `gopls`, Delve

### GoLand

* IDE chuyên biệt cho Go của JetBrains
* Hỗ trợ refactor, debug, test, database
* Có bản dùng thử 30 ngày

---

## The Go Playground

* Chạy code Go online
* Hỗ trợ format, share link
* Có giới hạn về network, thời gian, bộ nhớ

> **CẢNH BÁO**
> Không đưa thông tin nhạy cảm lên Playground.

---

## Makefile

`Makefile`:

```makefile
.DEFAULT_GOAL := build

.PHONY: fmt vet build
fmt:
    go fmt ./...

vet: fmt
    go vet ./...

build: vet
    go build
```

Chạy:

```bash
$ make
```

---

## Cam Kết Tương Thích Của Go

* Không phá vỡ tương thích ngược trong Go 1.x
* Không áp dụng cho các lệnh `go`

---

## Cập Nhật Go

Linux / BSD:

```bash
$ mv /usr/local/go /usr/local/old-go
$ tar -C /usr/local -xzf go1.20.6.linux-amd64.tar.gz
$ rm -rf /usr/local/old-go
```

---

## Bài Tập

1. Chạy chương trình Hello World trên Go Playground và chia sẻ link.
2. Thêm target `clean` vào Makefile.
3. Thử thay đổi định dạng và chạy `go fmt`.

---

## Tổng Kết

Bạn đã thiết lập xong môi trường Go, làm quen với các công cụ build, format, kiểm tra chất lượng mã. Chương tiếp theo sẽ đi sâu vào kiểu dữ liệu và biến trong Go.
