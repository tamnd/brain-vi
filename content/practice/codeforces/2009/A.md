---
title: "CF 2009A - Giảm thiểu!"
description: "Chúng tôi được cung cấp nhiều truy vấn độc lập. Mỗi truy vấn cung cấp hai số nguyên a và b và chúng ta được phép chọn bất kỳ số nguyên c nào trong phạm vi bao gồm giữa chúng. Với mỗi lựa chọn c, chúng ta tính chi phí được xác định bằng khoảng cách từ a đến c cộng với khoảng cách từ c đến b."
date: "2026-06-08T13:16:34+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 2009
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 971 (Div. 4)"
rating: 800
weight: 2009
solve_time_s: 65
verified: true
draft: false
---

[CF 2009A - Giảm thiểu!](https://codeforces.com/problemset/problem/2009/A) 

**Đánh giá:** 800 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều truy vấn độc lập. Mỗi truy vấn cung cấp hai số nguyên`a`Và`b`, và chúng ta được phép chọn bất kỳ số nguyên nào`c`trong khoảng bao hàm giữa chúng. Đối với mỗi sự lựa chọn của`c`, chúng tôi tính toán chi phí được xác định là khoảng cách từ`a`ĐẾN`c`cộng với khoảng cách từ`c`ĐẾN`b`. 

Về mặt hình học, điều này giống như đặt một điểm`c`ở đâu đó trên trục số giữa`a`Và`b`, và đo khoảng cách chúng ta đi từ`a`ĐẾN`b`thông qua`c`. Nhiệm vụ là chọn`c`sao cho tổng chi phí đi lại này được giảm thiểu và tạo ra giá trị tối thiểu đó. 

Các ràng buộc là cực kỳ nhỏ: cả hai`a`Và`b`nhiều nhất là 10, vì vậy ngay cả việc kiểm tra vũ phu tất cả những gì có thể`c`các giá trị sẽ là tầm thường. Tuy nhiên, việc nhận ra cấu trúc quan trọng hơn việc tính toán ở đây. 

Một cạm bẫy tinh vi trong cách lý luận ngây thơ là cho rằng điều tốt nhất`c`phụ thuộc vào vị trí của nó hoặc yêu cầu tìm kiếm. Một cách khác là cố gắng “tối ưu hóa” bằng cách kiểm tra các điểm giữa hoặc các giá trị đặc biệt. Bởi vì biểu thức che giấu sự hủy bỏ, các phương pháp phỏng đoán như vậy có thể làm phức tạp quá mức một cái gì đó thực sự là hằng số. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ lặp đi lặp lại mọi khả năng có thể`c`giữa`a`Và`b`, tính toán`(c - a) + (b - c)`, và lấy giá trị nhỏ nhất Điều này hiệu quả vì phạm vi rất nhỏ nhưng nó che khuất quan sát chính. 

Mở rộng biểu thức cho thấy cấu trúc của nó:`(c - a) + (b - c) = c - a + b - c = b - a`Biến`c`biến mất hoàn toàn. Điều này có nghĩa là mọi lựa chọn hợp lệ của`c`tạo ra giá trị chính xác như nhau. Không có sự tối ưu hóa nào để thực hiện vì chi phí không phụ thuộc vào quyết định. 

Brute-force hoạt động vì nó đánh giá các trường hợp dư thừa, nhưng nó không phát hiện ra rằng tất cả các trường hợp đều giống hệt nhau. Nhận xét rằng số hạng giữa triệt tiêu cho phép chúng ta quy bài toán về một phép trừ trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(b - a + 1) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp tối ưu được rút ra trực tiếp từ việc đơn giản hóa công thức. 

1. Đọc số nguyên`a`Và`b`cho từng trường hợp thử nghiệm. Chúng xác định các điểm cuối của phân khúc. 
2. Tính chênh lệch`b - a`. Điều này thể hiện tổng khoảng cách giữa các điểm cuối. 
3. Xuất giá trị này ngay lập tức. Không phụ thuộc vào`c`còn lại sau khi đơn giản hóa. 

Ý tưởng chính là mọi sự lựa chọn có thể của`c`gây ra sự hủy bỏ đại số tương tự, do đó không có bước ra quyết định nào ngoài việc đọc dữ liệu đầu vào. 

### Tại sao nó hoạt động 

biểu hiện`(c - a) + (b - c)`luôn đơn giản hóa`b - a`bởi vì`+c`Và`-c`điều khoản hủy bỏ chính xác. Vì danh tính này đúng cho tất cả các số nguyên`c`, tối thiểu trên tất cả hợp lệ`c`cũng là`b - a`và không có cấu hình thay thế nào có thể cải thiện hoặc làm xấu đi kết quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b = map(int, input().split())
    print(b - a)
```Việc thực hiện phản ánh đạo hàm trực tiếp. Mỗi trường hợp thử nghiệm đều độc lập nên chúng tôi xử lý chúng theo vòng lặp. Phép trừ`b - a`là dạng đơn giản hóa hoàn toàn của hàm mục tiêu, do đó không cần tìm kiếm hoặc phân nhánh. Không có trường hợp đặc biệt nào ngoài trường hợp tầm thường trong đó`a == b`, mà chính xác tạo ra số không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = 1, b = 2
```| c | (c - a) | (b - c) | tổng cộng | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 
| 2 | 1 | 0 | 1 | 

Bảng cho thấy mọi lựa chọn hợp lệ đều tạo ra cùng một giá trị, xác nhận rằng câu trả lời được cố định ở mức 1, khớp với`b - a`. 

### Ví dụ 2 

đầu vào:```
a = 3, b = 10
```| c | (c - a) | (b - c) | tổng cộng | 
| --- | --- | --- | --- | 
| 3 | 0 | 7 | 7 | 
| 6 | 3 | 4 | 7 | 
| 10 | 7 | 0 | 7 | 

Một lần nữa, mọi thứ có thể`c`mang lại kết quả tương tự, xác nhận công thức trực tiếp`10 - 3 = 7`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Một phép tính thời gian không đổi cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Không có bộ nhớ phụ ngoài các biến đầu vào | 

Giải pháp dễ dàng nằm trong giới hạn vì nó chỉ thực hiện một phép trừ duy nhất cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        a, b = map(int, input().split())
        out.append(str(b - a))
    return "\n".join(out)

# provided samples
assert run("""3
1 2
3 10
5 5
""") == """1
7
0"""

# custom cases
assert run("""1
1 1
""") == "0", "single point"

assert run("""2
2 3
4 4
""") == """1
0""", "mixed small ranges"

assert run("""2
1 10
10 10
""") == """9
0""", "boundary ranges"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 0 | khoảng có độ dài bằng không | 
| 2 3, 4 4 | 1, 0 | trường hợp hỗn hợp nhỏ và thoái hóa | 
| 1 10, 10 10 | 9, 0 | hành vi điển hình và ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh không rõ ràng duy nhất là khi`a == b`. Trong trường hợp đó, phạm vi của`c`thu gọn về một giá trị duy nhất và biểu thức có giá trị bằng 0. Công thức`b - a`xử lý việc này một cách tự nhiên vì nó trở thành`0`, khớp với kết quả chính xác mà không có bất kỳ sự phân nhánh đặc biệt nào.
