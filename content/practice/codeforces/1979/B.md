---
title: "CF 1979B - Chuỗi XOR"
description: "Cho ta hai số nguyên x và y. Bằng cách sử dụng chúng, chúng tôi xác định hai chuỗi vô hạn: $$an=noplus x$$ và $$bn=noplus y.$$ Các chỉ số bắt đầu từ 1. Mọi phần tử đều có được bằng cách XOR chỉ mục với một giá trị cố định."
date: "2026-06-08T17:00:54+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1979
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 951 (Div. 2)"
rating: 1000
weight: 1979
solve_time_s: 123
verified: true
draft: false
---

[CF 1979B - Trình tự XOR](https://codeforces.com/problemset/problem/1979/B) 

**Đánh giá:** 1000 
**Tags:** bitmasks, tham lam 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên`x`Và`y`. Sử dụng chúng, chúng tôi xác định hai chuỗi vô hạn:$$a_n=n\oplus x$$Và$$b_n=n\oplus y.$$Các chỉ số bắt đầu từ`1`. Mọi phần tử có được bằng cách XOR chỉ mục với một giá trị cố định. 

Nhiệm vụ là tìm độ dài tối đa của khối liền kề xuất hiện trong cả hai chuỗi. Khối có thể bắt đầu ở các vị trí khác nhau trong hai chuỗi, nhưng khi vị trí bắt đầu được chọn, các phần tử phải khớp từng phần tử một trong toàn bộ chiều dài. 

Các ràng buộc nhỏ về số lượng kiểm tra nhưng các giá trị của`x`Và`y`có thể lớn như$10^9$. Bản thân các chuỗi là vô hạn, vì vậy bất kỳ giải pháp nào cố gắng tạo ra các số hạng một cách rõ ràng đều không thể thực hiện được. Ngay cả việc kiểm tra vài triệu phần tử đầu tiên cũng không đủ vì câu trả lời có thể lớn bằng$2^{25}$, như được thể hiện bởi mẫu. 

Vấn đề thực sự là hiểu cách XOR biến đổi các số nguyên liên tiếp. Khi chúng tôi tìm thấy mẫu bit cơ bản, câu trả lời có thể được tính toán theo thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi`x`Và`y`chỉ khác nhau ở bit ít quan trọng nhất. 

Ví dụ:```
x = 0
y = 1
```Sau đó:```
d = x xor y = 1
```Câu trả lời là`1`. Một sai lầm phổ biến là nghĩ rằng câu trả lời nên là`2`vì XOR chỉ thay đổi một bit. Hành vi mang của các số nguyên liên tiếp sẽ ngay lập tức phá vỡ bất kỳ phân đoạn nào phù hợp dài hơn. 

Một sai lầm dễ xảy ra khác khi`x xor y`là sức mạnh của hai. 

Ví dụ:```
x = 12
y = 4
```Sau đó:```
x xor y = 8
```Câu trả lời là`8`, không`4`. Độ dài phụ thuộc vào vị trí của bit khác nhau thấp nhất chứ không phụ thuộc vào số lượng bit khác nhau. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là tạo ra các tiền tố lớn của cả hai chuỗi và tìm kiếm mảng con chung dài nhất. Điều này đúng vì định nghĩa theo nghĩa đen là yêu cầu một phân đoạn liền kề chung. 

Vấn đề là trình tự là vô hạn. Ngay cả khi chúng tôi tạo ra hàng triệu phần tử đầu tiên thì cũng không có gì đảm bảo rằng phân khúc tối ưu sẽ xuất hiện ở đó. Bản thân câu trả lời có thể vượt quá ba mươi triệu, nên mọi mô phỏng trực tiếp đều vô vọng. 

Quan sát chính xuất phát từ việc viết lại điều kiện đẳng thức. 

Giả sử một đoạn chung bắt đầu ở vị trí`i`Và`j`. Sau đó với mọi phần bù`t`bên trong phân khúc,$$(i+t)\oplus x=(j+t)\oplus y.$$Áp dụng XOR với`x`cả hai bên đều đưa ra$$(i+t) \oplus (j+t)=x\oplus y.$$Cho phép$$d=x\oplus y.$$Vấn đề trở thành: 

Tìm khoảng dài nhất của các giá trị liên tiếp của`t`như vậy$$(i+t)\oplus(j+t)=d.$$Bây giờ chúng ta chỉ cần nghiên cứu cách XOR hoạt động khi hai số tăng đồng thời. 

Thực tế quan trọng là độ dài tối đa có thể chỉ phụ thuộc vào bit được đặt thấp nhất của`d`. 

Nếu bit được đặt thấp nhất của`d`đang ở vị trí`k`, thì câu trả lời là$$2^k.$$Tương đương,$$\text{answer}=d\ \&\ (-d).$$Đại lượng này là giá trị của bit thiết lập có ý nghĩa nhỏ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không giới hạn | Không giới hạn | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán$$d=x\oplus y.$$Từ`x ≠ y`, chúng tôi biết`d > 0`. 
2. Tìm bit tập hợp nhỏ nhất của`d`. 

Trong số học nhị phân, đây là$$d\ \&\ (-d).$$3. Xuất giá trị đó. 

Toàn bộ giải pháp bao gồm ba bước này. 

### Tại sao nó hoạt động 

Đặt bit thấp nhất của`d`ở vị trí`k`. 

Tất cả các bit ở dưới vị trí`k`bằng 0 trong`d`. Điều này có nghĩa là với bất kỳ cặp số trùng khớp nào`u`Và`v`thỏa mãn$$u\oplus v=d,$$thấp hơn của họ`k`bit phải giống hệt nhau. 

Khi chúng ta tăng đồng thời cả hai số, những số thấp hơn`k`bit phát triển giống hệt nhau cho chính xác`2^k`các giá trị liên tiếp trước khi mang đạt tới bit`k`. 

Khi việc mang đó xảy ra, bit`k`thay đổi ở một bên khác với bên kia bởi vì`d`chứa một`1`ở vị trí đó. Sự bình đẳng$$(u+t)\oplus(v+t)=d$$không thể tiếp tục được nữa. 

Vì vậy không có đoạn nào có thể dài hơn`2^k`. 

Giới hạn này có thể đạt được bằng cách chọn các giá trị bắt đầu phù hợp có giá trị thấp hơn`k`bit bắt đầu từ số 0. Khi đó XOR vẫn bằng`d`chính xác`2^k`bước liên tiếp. 

Do đó đoạn chung dài nhất có thể có độ dài$$2^k=d\&(-d).$$## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        x, y = map(int, input().split())
        d = x ^ y
        ans.append(str(d & -d))

    sys.stdout.write("\n".join(ans))

solve()
```Việc thực hiện tuân theo kết quả toán học trực tiếp. 

biểu hiện`x ^ y`tính toán mặt nạ của các bit khác nhau. Vì các số là khác nhau nên giá trị này luôn dương. 

biểu hiện`d & -d`cô lập bit được đặt thấp nhất. Trong số học bù hai,`-d`lật tất cả các bit trên bit được đặt thấp nhất và bảo toàn chính xác bit đó khi được AND với`d`. 

Không có lo ngại về tràn vì số nguyên Python có độ chính xác tùy ý. Các giá trị lớn nhất là khoảng`2^30`, do đó, ngay cả số nguyên 64 bit có chiều rộng cố định cũng đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
x = 12
y = 4
```| Số lượng | Giá trị | 
| --- | --- | 
| x | 12 | 
| y | 4 | 
| d = x xor y | 8 | 
| Nhị phân d | 1000 | 
| Bit đặt thấp nhất | 8 | 
| Trả lời | 8 | 

Bit được đặt thấp nhất đã là bit được đặt cao nhất và duy nhất. Đoạn chung dài nhất có độ dài`8`. 

### Ví dụ 2 

đầu vào:```
x = 57
y = 37
```| Số lượng | Giá trị | 
| --- | --- | 
| x | 57 | 
| y | 37 | 
| d = x xor y | 28 | 
| Nhị phân d | 11100 | 
| Bit đặt thấp nhất | 4 | 
| Trả lời | 4 | 

Bit thiết lập ít quan trọng nhất là`4`, vậy câu trả lời là`4`, phù hợp với mẫu 

Ví dụ này chứng minh rằng chỉ có bit khác nhau thấp nhất mới quan trọng. Các bit khác nhau cao hơn không ảnh hưởng đến độ dài tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Hoạt động một XOR và một bit | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Mỗi trường hợp thử nghiệm yêu cầu một số phép tính số học không đổi. Với tối đa`10^4`trường hợp thử nghiệm, thời gian chạy không đáng kể so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        x, y = map(int, input().split())
        d = x ^ y
        out.append(str(d & -d))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    res = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return res.strip()

# provided sample
assert run(
"""4
0 1
12 4
57 37
316560849 14570961
"""
) == "\n".join([
    "1",
    "8",
    "4",
    "33554432"
])

# adjacent values
assert run(
"""1
10 11
"""
) == "1"

# power-of-two difference
assert run(
"""1
0 16
"""
) == "16"

# single low differing bit among many high bits
assert run(
"""1
123456 123460
"""
) == "4"

# large values
assert run(
"""1
1000000000 0
"""
) == "512"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`10 11`|`1`| Chỉ chênh lệch bit thấp nhất | 
|`0 16`|`16`| Sức mạnh thuần khiết của hai | 
|`123456 123460`|`4`| Nhiều bit khác nhau, bit thấp nhất chiếm ưu thế | 
|`1000000000 0`|`512`| Giá trị lớn gần giới hạn | 

## Vỏ cạnh 

Hãy xem xét:```
x = 7
y = 6
```Sau đó$$x\oplus y = 1.$$Bit được đặt thấp nhất là`1`, vậy câu trả lời là`1`. 

Một trực giác sai lầm là những con số rất giống nhau sẽ cho phép một đoạn chung dài. Phần mang được tạo sau một lần tăng ngay lập tức đạt đến bit khác nhau, do đó phân đoạn không thể mở rộng ra ngoài một phần tử. 

Bây giờ hãy xem xét:```
x = 0
y = 32
```Sau đó$$x\oplus y=32.$$Bit được đặt thấp nhất là`32`, và câu trả lời là`32`. Tất cả năm bit thấp hơn đều giống hệt nhau trong mặt nạ XOR, cho phép chính xác`32`các vị trí liên tiếp trước khi mang đến bit khác nhau đầu tiên. 

Cuối cùng hãy xem xét:```
x = 13
y = 9
```Sau đó$$x\oplus y=4.$$Mặc dù một số bit cao hơn có thể khác nhau nhưng vị trí khác biệt đầu tiên so với đáy là bit`2`, đưa ra câu trả lời`4`. Thuật toán bỏ qua mọi bit cao hơn vì chúng không ảnh hưởng khi lần mang đầu tiên không thể tránh khỏi phá vỡ đẳng thức.
