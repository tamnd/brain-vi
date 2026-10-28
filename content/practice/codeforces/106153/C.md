---
title: "CF 106153C - \u742a\u9732\u8bfa\u653e\u51b0\u5757"
description: "Chúng ta được cho một tấm bảng hình chữ nhật có kích thước $n nhân m$. Chúng ta cũng có các khối hình chữ nhật có kích thước $a nhân b$. Nhiệm vụ là xác định có bao nhiêu khối như vậy có thể được đặt trên bảng nếu chúng được căn chỉnh theo lưới và không thể chồng lên nhau, và điều quan trọng là chúng không thể xoay được."
date: "2026-06-19T19:20:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106153
codeforces_index: "C"
codeforces_contest_name: "HNNU Freshman Competition Round 2"
rating: 0
weight: 106153
solve_time_s: 54
verified: true
draft: false
---

[CF 106153C - \u742a\u9732\u8bfa\u653e\u51b0\u5757](https://codeforces.com/problemset/problem/106153/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tấm bảng hình chữ nhật có kích thước$n \times m$. Chúng tôi cũng có các khối hình chữ nhật có kích thước$a \times b$. Nhiệm vụ là xác định có bao nhiêu khối như vậy có thể được đặt trên bảng nếu chúng được căn chỉnh theo lưới và không thể chồng lên nhau, và điều quan trọng là chúng không thể xoay được. 

Cách giải thích rất đơn giản: chúng ta xếp bảng bắt đầu từ góc trên bên trái, đặt mỗi khối theo một hướng có chiều rộng cố định.$a$và chiều cao$b$. Câu hỏi đặt ra là có bao nhiêu khối đầy đủ vừa với chiều dọc và bao nhiêu khối vừa khít theo chiều ngang, sau đó kết hợp hai số lượng này. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp đưa ra bốn số nguyên biểu thị kích thước bảng và kích thước khối. Đối với mỗi trường hợp thử nghiệm, chúng tôi xuất ra một số nguyên duy nhất biểu thị số khối tối đa phù hợp. 

Các ràng buộc đủ nhỏ để mỗi trường hợp kiểm thử phải được xử lý trong thời gian không đổi. Cho dù có tới$10^5$các trường hợp thử nghiệm, bất kỳ giải pháp nào thực hiện nhiều hơn một vài phép tính số học cho mỗi trường hợp sẽ quá chậm. Điều này ngay lập tức loại trừ mọi mô phỏng trên lưới hoặc mọi nỗ lực đặt từng khối một. 

Một sai lầm nhỏ mà mọi người đôi khi mắc phải trong loại bài toán này là cho rằng phép quay được cho phép. Nếu cho phép xoay vòng, chúng ta sẽ cần xem xét cả hai$(a, b)$Và$(b, a)$vị trí và có kết quả tốt hơn. Một sai lầm khác là cố gắng giải thích các chiến lược chồng chéo một phần hoặc chiến lược đóng gói tham lam, những chiến lược này không cần thiết ở đây vì cấu trúc lưới khiến cho giải pháp chỉ thuần túy số học. 

Một trường hợp đáng chú ý là khi một trong hai$a > n$hoặc$b > m$. Trong trường hợp đó, không có khối nào phù hợp cả và câu trả lời sẽ bằng 0. Ví dụ, nếu$n = 3, m = 3, a = 5, b = 1$, thì không thể sắp xếp theo chiều dọc, do đó đầu ra phải là$0$, mặc dù không gian ngang tồn tại. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để suy nghĩ về vấn đề này là mô phỏng việc đặt các khối. Chúng tôi có thể lặp lại tất cả các vị trí trên cùng bên trái có thể có trong lưới và kiểm tra xem một khối có phù hợp hay không. Điều này sẽ liên quan đến việc kiểm tra tới$n \cdot m$vị trí cho mỗi trường hợp thử nghiệm và mỗi lần kiểm tra sẽ liên quan đến việc xác thực theo thời gian liên tục. Điều này dẫn đến$O(nm)$cho mỗi trường hợp thử nghiệm, vốn đã không cần thiết nhưng vẫn có thể quản lý được về mặt khái niệm. 

Tuy nhiên, mô phỏng này bỏ qua cấu trúc của vấn đề. Khi chúng tôi quan sát thấy các khối là các hình chữ nhật cố định được căn chỉnh theo lưới, chúng tôi thấy rằng vị trí đó độc lập dọc theo các hàng và cột. Dọc theo phương thẳng đứng, chúng ta chỉ có thể ghép$\lfloor n / a \rfloor$khối đầy đủ xếp chồng lên nhau. Dọc theo phương ngang, chúng ta chỉ có thể ghép$\lfloor m / b \rfloor$các khối cạnh nhau. Mỗi vị trí hợp lệ theo một hướng kết hợp độc lập với các vị trí theo hướng khác, do đó tổng số là tích của chúng. 

Điều này làm giảm vấn đề từ một nhiệm vụ đóng gói hình học thành hai phép chia số nguyên và phép nhân. Hiểu biết sâu sắc về cấu trúc quan trọng là do không được phép xoay và không được phép sắp xếp một phần nên lưới sẽ phân hủy thành các dải độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng vị trí Brute Force |$O(nm)$mỗi trường hợp thử nghiệm |$O(1)$| Quá chậm | 
| Phân rã số học trực tiếp |$O(1)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Mỗi trường hợp thử nghiệm đều độc lập nên chúng ta có thể xử lý chúng một cách riêng biệt mà không cần lưu trữ bất kỳ trạng thái nào. 
2. Với mỗi test, đọc 4 số nguyên$n, m, a, b$, đại diện cho kích thước bảng và kích thước khối. Mục đích là tính xem có bao nhiêu$a \times b$hình chữ nhật phù hợp với$n \times m$hình chữ nhật. 
3. Tính xem có bao nhiêu khối xếp theo chiều dọc bằng phép chia số nguyên$n // a$. Điều này thể hiện có bao nhiêu hàng đầy đủ chiều cao$a$có thể xếp chồng lên nhau mà không vượt quá chiều cao của bảng. 
4. Tính xem có bao nhiêu khối vừa theo chiều ngang bằng phép chia số nguyên$m // b$. Điều này thể hiện có bao nhiêu cột có chiều rộng đầy đủ$b$có thể được đặt trên bảng. 
5. Nhân hai giá trị này để có được tổng số khối. Điều này hoạt động vì mọi vị trí dọc có thể kết hợp độc lập với mọi vị trí ngang để tạo thành vùng vị trí hợp lệ. 
6. Xuất kết quả cho test case. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là khả năng phân tách các kích thước. Vì mỗi khối phải chiếm một vùng liền kề$a \times b$vùng và không thể xoay, mỗi vị trí hợp lệ sẽ tương ứng duy nhất với việc chọn chỉ mục dọc trong số$\lfloor n / a \rfloor$khả năng và chỉ số theo chiều ngang giữa$\lfloor m / b \rfloor$khả năng. Không có sự tương tác giữa các lựa chọn này và không có vùng còn sót lại nào có thể đóng góp một phần cho khối khác. Điều này tạo ra cấu trúc tích Descartes của các vị trí hợp lệ, đảm bảo tính chính xác của việc nhân hai thương số nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, a, b = map(int, input().split())
    print((n // a) * (m // b))

t = int(input())
for _ in range(t):
    solve()
```Giải pháp đọc từng trường hợp kiểm thử và áp dụng trực tiếp phép chia số nguyên để tính toán xem có bao nhiêu phân đoạn đầy đủ phù hợp với mỗi chiều. Bước nhân này an toàn vì các giá trị liên quan đủ nhỏ để vừa vặn thoải mái trong các số nguyên Python. 

Một cạm bẫy phổ biến là quên phép chia số nguyên và sử dụng phép chia dấu phẩy động, điều này sẽ gây ra các vấn đề về độ chính xác. Một sai lầm khác là cố làm tròn lên thay vì làm tròn xuống; chỉ đếm toàn bộ khối, vì vậy việc phân chia tầng là điều cần thiết. 

Thứ tự của các phép toán không quan trọng ở đây vì phép nhân có tính giao hoán, nhưng việc giữ cấu trúc như hai phép chia riêng biệt sẽ cải thiện sự rõ ràng và giảm nguy cơ trộn lẫn các chiều. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
6 8 2 3
```| Bước | Dọc (n//a) | Ngang (m//b) | Kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu | 6 | 8 | - | 
| Sau khi phân chia theo chiều dọc | 3 | 8 | - | 
| Sau khi chia ngang | 3 | 2 | - | 
| Cuối cùng | 3 | 2 | 6 | 

Ở đây, bảng có thể được chia thành 3 hàng đầy đủ chiều cao 2 và mỗi hàng chứa 2 cột đầy đủ chiều rộng 3. Sản phẩm cho 6 khối. 

Điều này chứng tỏ cách phân rã thành các thứ nguyên độc lập tạo thành một mạng lưới các vị trí hợp lệ một cách tự nhiên. 

### Ví dụ 2 

đầu vào:```
1
5 5 6 2
```| Bước | Dọc (n//a) | Ngang (m//b) | Kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu | 5 | 5 | - | 
| Sau khi phân chia theo chiều dọc | 0 | 5 | - | 
| Sau khi chia ngang | 0 | 2 | - | 
| Cuối cùng | 0 | 2 | 0 | 

Vì chiều cao khối vượt quá chiều cao bảng nên không thể đặt theo chiều dọc. Mặc dù không gian ngang có tồn tại nhưng điều đó không thành vấn đề và câu trả lời cuối cùng là số không. 

Điều này xác nhận việc xử lý chính xác các kích thước không khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm yêu cầu một số phép tính số học không đổi | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu bổ sung nào được sử dụng ngoài các biến đầu vào | 

Giải pháp này phù hợp một cách thoải mái trong các giới hạn điển hình cho lập trình cạnh tranh, ngay cả khi số lượng ca kiểm thử lớn, vì mỗi trường hợp đều được giải trong thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        n, m, a, b = map(int, input().split())
        return (n // a) * (m // b)

    t = int(input())
    out = []
    for _ in range(t):
        out.append(str(solve()))
    return "\n".join(out)

# provided sample-style tests
assert run("1\n6 8 2 3\n") == "6"
assert run("1\n5 5 6 2\n") == "0"

# custom cases
assert run("1\n1 1 1 1\n") == "1", "minimum valid case"
assert run("1\n10 10 3 3\n") == "9", "perfect tiling"
assert run("1\n9 9 10 2\n") == "0", "height too large"
assert run("1\n7 5 2 3\n") == "3", "mixed leftover space"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1×1 với khối 1×1 | 1 | ốp lát hợp lệ tối thiểu | 
| 10×10 với khối 3×3 | 9 | làm sạch toàn bộ lưới đóng gói | 
| 9×9 với khối 10×2 | 0 | khối lớn hơn kích thước bảng | 
| 7×5 với khối 2×3 | 3 | phần còn lại không được tính | 

## Vỏ cạnh 

Khi nào$a > n$, thương số dọc ngay lập tức trở thành 0 và thuật toán đưa ra kết quả bằng 0 bất kể chiều rộng. Ví dụ, đầu vào`n=4, m=10, a=5, b=2`sản xuất`(4//5)*(10//2) = 0*5 = 0`, phù hợp với thực tế là không thể đặt khối nào. 

Khi$b > m$, sự đối xứng tương tự được áp dụng theo chiều ngang. Ngay cả khi có thể xếp chồng theo chiều dọc, việc thiếu sự phù hợp theo chiều ngang sẽ buộc kết quả về 0. 

Khi cả hai chiều chia đều, chẳng hạn như`n=8, m=6, a=2, b=3`, việc tính toán trở thành`(8//2)*(6//3) = 4*2 = 8`, tương ứng chính xác với một phân vùng lưới đầy đủ không còn chỗ trống.
