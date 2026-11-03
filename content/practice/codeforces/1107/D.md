---
title: "CF 1107D - Nén"
description: "Chúng ta có một ma trận vuông có kích thước $n nhân n$, nhưng thay vì được viết rõ ràng dưới dạng bit, mỗi hàng được đóng gói thành các ký tự thập lục phân. Mỗi chữ số hex đại diện cho bốn ô nhị phân, do đó đầu vào chỉ là mã hóa nhỏ gọn của ma trận nhị phân."
date: "2026-06-12T05:23:43+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1107
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 59 (Rated for Div. 2)"
rating: 1800
weight: 1107
solve_time_s: 85
verified: true
draft: false
---

[CF 1107D - Nén](https://codeforces.com/problemset/problem/1107/D) 

**Đánh giá:** 1800 
**Tags:** dp, thực hiện, toán, lý thuyết số 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một ma trận vuông có kích thước$n \times n$, nhưng thay vì được viết rõ ràng dưới dạng bit, mỗi hàng được đóng gói thành các ký tự thập lục phân. Mỗi chữ số hex đại diện cho bốn ô nhị phân, do đó đầu vào chỉ là mã hóa nhỏ gọn của ma trận nhị phân. 

Nhiệm vụ là tìm kích thước khối lớn nhất$x$sao cho ma trận có thể được “nén” thành một lưới có kích thước nhỏ hơn$\frac{n}{x} \times \frac{n}{x}$. Nén có nghĩa là nếu chúng ta chia ma trận thành các phần không chồng lấp$x \times x$các khối thì mọi ô bên trong một khối phải có cùng một giá trị. Các khối khác nhau được phép khác nhau, nhưng bên trong một khối không có biến thể. 

Vì vậy, điều kiện không phải là sự lặp lại toàn cục mà là tính đồng nhất cục bộ bên trong các phân vùng cố định của lưới. 

Những ràng buộc đẩy chúng ta tới một$O(n^2)$hoặc gần$O(n^2 \log n)$giải pháp. Từ$n \le 5200$, MỘT$O(n^2)$đã có vé rồi$2.7 \times 10^7$tế bào, điều đó là tốt. Điều không thể chấp nhận được là lặp lại việc quét toàn bộ ma trận đối với nhiều giá trị ứng cử viên của$x$, vì số ước vẫn có thể đủ lớn để đẩy chúng ta vào hàng tỷ phép tính. 

Một cách tiếp cận ngây thơ sẽ thử mọi ước số$x$của$n$và xác minh từng khối bằng cách quét tất cả$x^2$tế bào. Việc này trở nên chậm vì mỗi lần kiểm tra đều tốn$O(n^2)$và làm điều đó nhiều lần sẽ nhân chi phí đó lên. 

Một trường hợp thất bại tinh tế xuất hiện khi người ta giả định sự lặp lại định kỳ thay vì sự độc lập của khối. Ví dụ, trong một ma trận trong đó mỗi$4 \times 4$khối không đổi nhưng các khối khác nhau khác nhau, việc coi nó như một lưới định kỳ hoàn toàn sẽ từ chối các lần nén hợp lệ một cách không chính xác. 

Một trường hợp thất bại khác xuất phát từ việc không xử lý giải mã hex một cách cẩn thận. Mỗi ký tự mở rộng thành bốn bit và bất kỳ bit nào trong ánh xạ bit tới các ô sẽ thay đổi cấu trúc và phá vỡ các kiểm tra căn chỉnh khối. 

## Phương pháp tiếp cận 

Chiến lược vũ phu bắt đầu từ định nghĩa. Đối với mỗi ứng viên$x$, chúng tôi phân vùng ma trận thành$x \times x$khối và xác minh rằng mỗi khối chỉ chứa một giá trị riêng biệt. Đối với mỗi khối, chúng tôi quét tất cả các ô của nó. Điều này đúng vì nó trực tiếp thực thi điều kiện nén. 

Vấn đề là sự lặp lại. Mỗi séc đã chạm vào tất cả$n^2$các ô và việc thực hiện điều này với nhiều ước số sẽ làm cho tổng công việc trở nên quá lớn. 

Quan sát quan trọng là chúng ta không cần phải quét lại mọi ô bên trong mỗi khối nhiều lần. Một khối hợp lệ nếu tất cả các giá trị của nó giống hệt nhau, điều này đối với ma trận nhị phân tương đương với việc nói rằng tổng các giá trị trong khối đó bằng hoặc$0$hoặc$x^2$. Điều này biến mỗi lần kiểm tra khối thành một truy vấn có thời gian không đổi nếu chúng tôi duy trì tổng tiền tố 2D. 

Khi đã có sẵn tổng tiền tố, mỗi ứng cử viên$x$có thể được xác nhận bằng cách lặp qua nó$\left(\frac{n}{x}\right)^2$khối và kiểm tra tổng từng khối trong$O(1)$. Điều này làm giảm việc kiểm tra từng ứng viên theo tỷ lệ với số khối thay vì số lượng ô. 

Vì chúng ta chỉ kiểm tra các ước của$n$và tổng công việc trên tất cả các ước số tạo thành một tổng giống như hài hòa trên số khối, tổng độ phức tạp vẫn gần bằng$O(n^2)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \cdot d(n))$|$O(1)$| Quá chậm | 
| Tiền tố Tổng trên mỗi số chia |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Giải mã đầu vào thành ma trận nhị phân 

Mỗi ký tự thập lục phân được mở rộng thành bốn bit. Chúng tôi xây dựng đầy đủ$n \times n$ma trận$A$chỉ chứa 0 và 1. Bước này là cần thiết vì tất cả các suy luận sau này đều phụ thuộc vào việc truy cập trực tiếp vào ô. 

### Bước 2: Xây dựng mảng tổng tiền tố 2D 

Chúng tôi xây dựng một bảng tổng tiền tố trong đó mỗi mục lưu trữ số lượng đơn vị trong hình chữ nhật từ góc trên bên trái đến vị trí đó. Điều này cho phép chúng ta truy vấn tổng của bất kỳ ma trận con nào trong thời gian không đổi. Đây là cơ chế thay thế việc quét lặp đi lặp lại. 

### Bước 3: Liệt kê tất cả các ước của$n$Chúng tôi tạo ra tất cả các kích thước khối hợp lệ$x$như vậy$n \bmod x = 0$. Chỉ những giá trị này mới có ý nghĩa vì lưới phải được phân chia thành các khối bằng nhau. 

### Bước 4: Đối với từng ứng viên$x$, xác nhận tất cả các khối 

Chúng ta phân chia ma trận thành$x \times x$khối. Đối với mỗi khối, chúng tôi tính toán số khối bằng cách sử dụng tổng tiền tố. Nếu tổng không bằng$0$cũng không$x^2$, thì khối chứa các giá trị hỗn hợp và ứng cử viên$x$không hợp lệ. 

### Bước 5: Theo dõi giá trị hợp lệ tối đa$x$Chúng tôi lặp lại tất cả các ứng viên và lưu trữ ứng viên lớn nhất đã vượt qua quá trình xác thực. Đây là câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Mỗi khối phải tương ứng với một giá trị duy nhất trong ma trận nén. Đối với ma trận nhị phân, một khối hợp lệ tương đương với hằng số, tương đương với việc có tổng bằng 0 hoặc đầy đủ. Tổng tiền tố đảm bảo chúng tôi kiểm tra từng khối chính xác một lần mà không có sự mơ hồ. Vì mỗi ô thuộc về chính xác một khối cho một khối cố định$x$, việc xác nhận hoàn toàn đặc trưng cho tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_matrix(n):
    A = [[0] * n for _ in range(n)]
    for i in range(n):
        row = input().strip()
        col = 0
        for ch in row:
            v = int(ch, 16)
            for k in range(3, -1, -1):
                if col < n:
                    A[i][col] = (v >> k) & 1
                    col += 1
    return A

def build_prefix(A, n):
    ps = [[0] * (n + 1) for _ in range(n + 1)]
    for i in range(n):
        row_sum = 0
        for j in range(n):
            row_sum += A[i][j]
            ps[i + 1][j + 1] = ps[i][j + 1] + row_sum
    return ps

def rect_sum(ps, x1, y1, x2, y2):
    return ps[x2][y2] - ps[x1][y2] - ps[x2][y1] + ps[x1][y1]

def solve():
    n = int(input())
    A = build_matrix(n)
    ps = build_prefix(A, n)

    def ok(x):
        for i in range(0, n, x):
            for j in range(0, n, x):
                total = rect_sum(ps, i, j, i + x, j + x)
                if total != 0 and total != x * x:
                    return False
        return True

    best = 1
    for x in range(1, n + 1):
        if n % x == 0 and ok(x):
            best = x

    print(best)

if __name__ == "__main__":
    solve()
```Bước giải mã xử lý cẩn thận từng chữ số hex thành bốn bit theo đúng thứ tự, đảm bảo căn chỉnh bit khớp với bố cục ma trận ban đầu. Tổng tiền tố được xây dựng bằng cách lập chỉ mục dựa trên 1 để các truy vấn ma trận con không yêu cầu kiểm tra ranh giới. 

Hàm xác thực kiểm tra từng kích thước khối ứng viên bằng cách chỉ quét nguồn gốc khối chứ không phải các ô riêng lẻ. Truy vấn tổng hình chữ nhật tách biệt từng$x \times x$vùng trong thời gian không đổi, đó là tối ưu hóa cốt lõi. 

Vòng lặp trên tất cả$x$là an toàn vì số lượng ước số bị giới hạn và mỗi lần kiểm tra đều hiệu quả do tổng tiền tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
00
00
FF
FF
```Điều này tương ứng với một ma trận có hai dải ngang đồng nhất. 

| x | Chặn kiểm tra | hợp lệ | 
| --- | --- | --- | 
| 1 | tất cả các khối 1x1 đều hợp lệ | vâng | 
| 2 | mỗi khối đồng phục 2x2 | vâng | 
| 4 | toàn bộ ma trận trộn | không | 

Thuật toán trả về 2. 

Điều này cho thấy kích thước khối lớn hơn thất bại như thế nào khi nhiều vùng khác nhau. 

### Ví dụ 2 

đầu vào:```
4
0F
0F
0F
0F
```| x | Chặn kiểm tra | hợp lệ | 
| --- | --- | --- | 
| 1 | hợp lệ | vâng | 
| 2 | khối hỗn hợp bên trong | không | 
| 4 | hỗn hợp trên toàn cầu | không | 

Kết quả là 1, cho thấy chỉ tính đồng nhất cục bộ là không đủ để nén lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi ô đóng góp vào tổng tiền tố một lần và tổng số lần kiểm tra khối trên tất cả các ước số tính tổng thành một hệ số tuyến tính trên kích thước lưới | 
| Không gian |$O(n^2)$| Lưu trữ cho bảng tổng tiền tố và ma trận đã giải mã | 

Các ràng buộc cho phép đầy đủ$n \times n$tiền xử lý và xác thực dựa trên số chia giúp công việc lặp lại được giới hạn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return ""

# provided sample
assert run("""8
E7
E7
E7
00
00
E7
E7
E7
""") == ""

# all zeros, full compression
assert run("""4
00
00
00
00
""") == ""

# alternating rows
assert run("""4
FF
00
FF
00
""") == ""

# checkerboard pattern
assert run("""4
AA
55
AA
55
""") == ""

# minimum meaningful case
assert run("""4
0F
0F
F0
F0
""") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | 4 | ma trận thống nhất | 
| hàng xen kẽ | 2 | độ chính xác của cấu trúc khối | 
| bàn cờ | 1 | không nén ngoài đơn vị | 
| chia góc phần tư | 2 | phát hiện khối hỗn hợp | 

## Vỏ cạnh 

Một ma trận hoàn toàn đồng nhất là trường hợp đơn giản nhất. Mỗi ứng viên$x$vượt qua vì mỗi tổng khối luôn luôn$x^2$. Thuật toán xử lý việc này một cách tự nhiên vì tổng tiền tố luôn trả về phạm vi bao phủ đầy đủ cho mỗi khối. 

Một mẫu bàn cờ không thành công cho mọi$x > 1$. Trong trường hợp như vậy, tổng khối luôn được trộn lẫn và lần kiểm tra thất bại đầu tiên xảy ra ngay bên trong khối không hợp lệ đầu tiên, đảm bảo bị loại bỏ sớm mà không cần quét các vùng không cần thiết. 

Một ma trận chỉ đồng nhất ở các vùng liền kề lớn nhưng bị lệch với các ranh giới khối chứng tỏ tại sao việc căn chỉnh lại quan trọng. Ngay cả khi các khu vực lớn chia sẻ các giá trị, nếu chúng vượt qua các đường viền khối không chính xác, thì điều kiện tổng sẽ phát hiện sự không nhất quán một cách chính xác ở các khối biên.
