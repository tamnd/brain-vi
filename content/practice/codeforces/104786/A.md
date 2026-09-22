---
title: "CF 104786A - John và M\u00f6bius Convolution"
description: "Chúng ta có hai mảng số nguyên, mỗi mảng có độ dài $n$. Mọi giá trị trong cả hai mảng đều nhỏ, nhiều nhất là $10^3$, nhưng bản thân các mảng có thể lớn, lên tới $2 cdot 10^5$ phần tử."
date: "2026-06-28T14:29:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104786
codeforces_index: "A"
codeforces_contest_name: "FIICode2023Round1"
rating: 0
weight: 104786
solve_time_s: 59
verified: true
draft: false
---

[CF 104786A - Sự chuyển đổi của John và M\u00f6bius](https://codeforces.com/problemset/problem/104786/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai mảng số nguyên, mỗi mảng có độ dài$n$. Mọi giá trị trong cả hai mảng đều nhỏ, nhiều nhất là$10^3$, nhưng bản thân các mảng có thể lớn, lên tới$2 \cdot 10^5$các phần tử. 

Nhiệm vụ là xem xét từng cặp được hình thành bằng cách chọn một phần tử từ mảng đầu tiên và một phần tử từ mảng thứ hai, tính toán$\gcd(x, y) \cdot \mathrm{lcm}(x, y)$, và tính tổng số này trên tất cả các cặp. 

Sự đơn giản hóa cấu trúc quan trọng là với hai số nguyên bất kỳ$x$Và$y$, tích của gcd và lcm của họ chính xác là$x \cdot y$. Sự đồng nhất hóa này biến một vấn đề có vẻ như lý thuyết số thành một vấn đề đếm và tổng hợp thuần túy trên các sản phẩm. 

Vì vậy, vấn đề giảm xuống việc tính tổng của$a_i \cdot b_j$trên tất cả các cặp. 

Ràng buộc$n \le 2 \cdot 10^5$loại trừ bất kỳ chiến lược ghép đôi bậc hai nào. Mặc dù mỗi giá trị đều nhỏ nhưng việc lặp lại tất cả$n^2$các cặp sẽ liên quan đến tối đa$4 \cdot 10^{10}$hoạt động vượt xa thời hạn. 

Việc tính toán trực tiếp theo từng cặp cũng không cần thiết vì biểu thức phân tích nhân tử một cách rõ ràng. 

Các trường hợp cạnh chủ yếu là về sự đa dạng cực độ. Ví dụ: nếu tất cả các giá trị là 1 thì mỗi cặp đóng góp 1 và câu trả lời trở thành$n^2$. Một cách tiếp cận ngây thơ vẫn có thể đúng nhưng quá chậm. Một trường hợp khác là khi tất cả các giá trị đều$10^3$, trong đó phép nhân đơn giản có rủi ro là tổng trung gian lớn nhưng vẫn phù hợp với phạm vi 64 bit vì tích tối đa là$10^6$và tổng số cặp là$4 \cdot 10^{10}$, yêu cầu độ an toàn 128-bit ở một số ngôn ngữ, mặc dù Python xử lý nó một cách tự nhiên. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại mọi$a_i$Và$b_j$, tính toán$\gcd(a_i, b_j)$, tính toán$\mathrm{lcm}(a_i, b_j)$, nhân chúng và thêm vào câu trả lời. Điều này đúng vì nó tuân theo định nghĩa trực tiếp. Chi phí đến từ vòng lặp kép trên tất cả các cặp, điều này đã mang lại$O(n^2)$và mỗi cặp cũng thực hiện tính toán gcd. Ngay cả với gcd được tối ưu hóa, điều này hoàn toàn không khả thi ở$n = 2 \cdot 10^5$. 

Quan sát quan trọng là biểu thức bên trong tổng được đơn giản hóa về mặt đại số. Sử dụng danh tính$\gcd(x,y) \cdot \mathrm{lcm}(x,y) = x \cdot y$, mỗi cặp đóng góp đơn giản$a_i \cdot b_j$. Tổng kép trở nên có thể tách rời:$\sum_i \sum_j a_i b_j = \left(\sum_i a_i\right)\left(\sum_j b_j\right)$. Điều này biến vấn đề từ tương tác theo cặp thành tập hợp độc lập trên mỗi mảng. 

Chúng ta không cần phải xem xét sự tương tác giữa các cặp riêng lẻ nữa. Chỉ có tổng số tiền của mỗi mảng là quan trọng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$, sau đó đọc mảng$a$Và$b$. Chúng xác định hai tập hợp giá trị mà chúng ta phải tổng hợp các tương tác theo cặp. 
2. Tính tổng các phần tử trong$a$, gọi nó$S_a$. Điều này nắm bắt tổng mức đóng góp từ mảng đầu tiên trên tất cả các cặp, vì mỗi phần tử sẽ được sử dụng lại trên tất cả các phần tử của mảng thứ hai. 
3. Tính tổng các phần tử trong$b$, gọi nó$S_b$. Điều này đóng vai trò đối xứng cho mảng thứ hai. 
4. Nhân$S_a$Và$S_b$. Điều này có tác dụng vì mọi$a_i$cặp với mọi$b_j$, vậy mỗi$a_i$được lặp lại một cách hiệu quả$n$lần, một lần cho mỗi phần tử trong$b$, và ngược lại. 
5. Xuất sản phẩm$S_a \cdot S_b$. 

Tại sao nó hoạt động: sự chuyển đổi$\gcd(x,y)\cdot \mathrm{lcm}(x,y) = x \cdot y$loại bỏ tất cả sự phức tạp tương tác giữa các phần tử. Sau khi thay thế, tổng trở thành song tuyến tính và có thể phân tách hoàn toàn. Thuật toán đang tính toán một cách hiệu quả tích số chấm giữa hai vectơ có trọng số theo mảng ban đầu và tính song tuyến đảm bảo rằng các thuật ngữ tập hợp lại không làm thay đổi kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

sa = sum(a)
sb = sum(b)

print(sa * sb)
```Việc thực hiện dựa hoàn toàn vào việc giảm đại số. Không cần phải tính gcd hoặc lcm một cách rõ ràng. Điều tinh tế duy nhất là đảm bảo đầu vào được đọc hiệu quả và tổng được tích lũy dưới dạng số nguyên Python tiêu chuẩn, xử lý các giá trị lớn một cách tự nhiên mà không bị tràn. 

Phép nhân được thực hiện một lần ở cuối, sau khi cả hai tổng được tính toán đầy đủ, đảm bảo không có sự tăng trưởng bậc hai trung gian của các phép toán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
2 6
3 12
```Chúng tôi tính tổng của từng mảng rồi nhân lên. 

| Bước | Sa | Sb | Trạng thái hiện tại | Ghi chú | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | a, b nạp | khởi tạo | 
| Đọc một | 8 | 0 | [2, 6] | tổng của a là 8 | 
| Đọc b | 8 | 15 | [3, 12] | tổng của b là 15 | 
| Cuối cùng | 8 | 15 | trả lời | 8 × 15 = 120 | 

Điều này xác nhận rằng mặc dù bài toán ban đầu liên quan đến gcd và lcm, nhưng cấu trúc vẫn sụp đổ thành một tích của các tổng. 

### Ví dụ 2 

đầu vào:```
3
1 1 1
5 10 15
```| Bước | Sa | Sb | Trạng thái hiện tại | Ghi chú | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | mảng được nạp | khởi tạo | 
| Đọc một | 3 | 0 | [1,1,1] | mảng thống nhất | 
| Đọc b | 3 | 30 | [5,10,15] | tính tổng | 
| Cuối cùng | 3 | 30 | trả lời | 90 | 

Điều này cho thấy các giá trị lặp lại không yêu cầu xử lý đặc biệt; bội số đã được mã hóa trong tổng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| truyền một lần qua từng mảng để tính tổng | 
| Không gian |$O(1)$| chỉ lưu trữ tổng số đang chạy | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì$n = 2 \cdot 10^5$chỉ yêu cầu quét tuyến tính và một phép nhân duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    return str(sum(a) * sum(b))

# provided sample
assert run("2\n2 6\n3 12\n") == "120"

# minimum size
assert run("1\n1\n1\n") == "1"

# all equal values
assert run("3\n2 2 2\n3 3 3\n") == "36"

# mixed small values
assert run("4\n1 2 3 4\n4 3 2 1\n") == "100"

# larger skew
assert run("5\n10 0 0 0 0\n1 2 3 4 5\n") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 / 1 1 | 1 | trường hợp cạnh tối thiểu | 
| tất cả 2 và 3 | 36 | giá trị lặp lại | 
| trình tự đảo ngược | 100 | tính đúng đắn đối xứng | 
| thưa thớt khác không | 10 | số không và phân phối | 

## Vỏ cạnh 

### Mảng phần tử đơn 

đầu vào:```
1
7
9
```Thuật toán tính toán$S_a = 7$,$S_b = 9$, và trả về 63. Cặp duy nhất là (7, 9) và$\gcd(7,9)\cdot\mathrm{lcm}(7,9) = 63$, khớp với kết quả một cách chính xác. Điều này xác nhận tính đúng đắn trong trường hợp suy biến trong đó không cần tổng hợp. 

### Sự hiện diện của số không 

Mặc dù không xuất hiện trong các ràng buộc ở đây, nhưng nếu cho phép số 0 thì danh tính vẫn giữ nguyên vì$0 \cdot x = 0$. Thuật toán sẽ tạo ra số 0 một cách chính xác bất cứ khi nào tổng bằng 0, phù hợp với đánh giá theo cặp. 

### Mảng đồng nhất lớn 

Đầu vào chứa tất cả các giá trị$10^3$. Thuật toán tính tổng tuyến tính và nhân chúng một lần. Mỗi cặp đóng góp$10^6$, và có$n^2$cặp, phù hợp$(n \cdot 10^3)(n \cdot 10^3)$. Việc giảm thiểu tránh lặp đi lặp lại các cặp này một cách rõ ràng trong khi vẫn duy trì chính xác tổng đóng góp.
