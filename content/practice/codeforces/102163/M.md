---
title: "CF 102163M - Lương NCD"
description: "Đối với mỗi trường hợp thử nghiệm, có hai mức lương được biểu diễn dưới dạng lũy ​​thừa. Mức lương ban đầu là (B1^{P1}), còn mức lương mới là (B2^{P2}). Chúng ta cần so sánh hai giá trị này mà không thực sự cần in ra mức lương."
date: "2026-08-20T00:28:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "M"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 1546
verified: false
draft: false
---

[CF 102163M - Mức lương NCD](https://codeforces.com/problemset/problem/102163/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 25 phút 46 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, có hai mức lương được biểu diễn dưới dạng lũy thừa. Mức lương ban đầu là (B_1^{P_1}), còn mức lương mới là (B_2^{P_2}). Chúng ta cần so sánh hai giá trị này mà không thực sự cần in ra mức lương. Nếu mức lương mới lớn hơn thì câu trả lời là`Congrats`; nếu nó nhỏ hơn thì câu trả lời là`HaHa`; nếu cả hai mức lương bằng nhau thì câu trả lời là`Lazy`. Bài toán chính thức có (B) và (P) nằm trong khoảng từ (0) đến (10^6). 

Khó khăn đầu tiên là số mũ có thể lên tới một triệu. Mặc dù bản thân các đầu vào vừa khít với các số nguyên thông thường, (10^6{}^{10^6}) có khoảng sáu triệu chữ số thập phân. Xây dựng những con số như vậy chỉ để so sánh chúng là lãng phí. Đầu vào chứa (T) các so sánh độc lập, do đó công việc trên mỗi trường hợp kiểm thử cần duy trì ở mức rất nhỏ. Một giải pháp dựa trên việc xây dựng rõ ràng các lũy thừa có thể nhanh chóng bị chi phối bởi phép nhân và sử dụng bộ nhớ có độ chính xác tùy ý, trong khi so sánh logarit chỉ cần một số phép toán dấu phẩy động cho mỗi trường hợp. 

Có một số trường hợp ranh giới có thể âm thầm phá vỡ quá trình triển khai đơn giản. Cơ số 0 không thể truyền trực tiếp tới`log`, bởi vì (\log(0)) không được xác định. Ví dụ,```
1
0 5 2 3
```đại diện cho (0^5) so với (2^3), do đó mức lương mới lớn hơn và câu trả lời là`Congrats`. Phép tính logarit trực tiếp sẽ cố gắng đánh giá`log(0)`. 

Số mũ bằng 0 là một trường hợp đặc biệt khác. Đối với cơ sở dương, (B^0=1). Ví dụ,```
1
7 0 3 2
```so sánh (1) với (9) nên đáp án là`Congrats`. Biểu thức logarit xử lý các cơ số dương một cách tự nhiên vì (0\cdot\log(B)=0). 

Sự bình đẳng cũng có thể là lừa dối. Các căn cứ không nhất thiết phải giống hệt nhau. Ví dụ,```
1
2 4 4 2
```so sánh (2^4) với (4^2), cả hai đều bằng (16), nên đáp án là`Lazy`. Chỉ so sánh các căn cứ sẽ báo cáo sai rằng mức lương thứ hai lớn hơn. 

Cách tiếp cận được chấp nhận của cuộc thi coi cơ sở bằng 0 là mức lương bằng 0, bao gồm cả mức lương thoái hóa.`0 0`biểu diễn và xử lý nó trước khi lấy logarit. Quy ước này phù hợp với các giải pháp được chấp nhận cho vấn đề. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là tính cả hai lũy thừa và so sánh các số nguyên thu được. Về mặt toán học, điều này hoàn toàn chính xác vì nó tính toán chính xác hai đại lượng mà chúng ta quan tâm. Ví dụ, chúng ta có thể tính toán`pow(B1, P1)`Và`pow(B2, P2)`và so sánh chúng. 

Vấn đề là kích thước của những số nguyên đó. Trong trường hợp xấu nhất, (B=10^6) và (P=10^6), do đó kết quả chứa khoảng (6\times10^6) chữ số thập phân hoặc khoảng (2\times10^7) bit. Do đó, một phép lũy thừa duy nhất hoạt động trên các số nguyên nhiều megabyte và các phép nhân có độ chính xác tùy ý lặp đi lặp lại làm cho phương pháp này trở nên quá đắt khi có nhiều trường hợp thử nghiệm. Số lượng chính xác của các phép toán máy cơ bản phụ thuộc vào việc triển khai số nguyên lớn, do đó, câu lệnh phức tạp hữu ích là chi phí số học tăng theo số bit trong kết quả khổng lồ thay vì với đầu vào có kích thước không đổi. 

Quan sát quan trọng là thứ tự của các số dương được bảo toàn bằng logarit. Đối với dương tính (B), 

[ 
B^P = e^{P\ln B}. 
] 

Vì hàm số mũ tăng nghiêm ngặt nên 

[ 
B_1^{P_1} < B_2^{P_2} 
] 

tương đương với 

[ 
P_1\ln B_1 < P_2\ln B_2. 
] 

Những thế lực to lớn đã biến mất. Chúng tôi chỉ tính tối đa hai tích liên quan đến số (10^6). Đây chính xác là lý do tại sao logarit phù hợp với cấu trúc của bài toán: số mũ làm cho số nguyên trở thành số lớn sẽ trở thành một phép nhân thông thường. 

Các trường hợp cơ số 0 phải được phân tách trước vì (\ln 0) không tồn tại. Sau đó, việc so sánh sử dụng logarit dấu phẩy động với dung sai nhỏ. Đây cũng là cách tiếp cận được các giải pháp được chấp nhận đã công bố sử dụng cho vấn đề này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Phụ thuộc vào kích thước số nguyên khổng lồ, lên tới hàng triệu chữ số thập phân trên mỗi lũy thừa | Phụ thuộc vào kích thước số nguyên lớn | Quá chậm | 
| Logarit | (O(1)) cho mỗi trường hợp thử nghiệm | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (B_1,P_1,B_2,P_2). Cặp đầu tiên đại diện cho mức lương cũ và cặp thứ hai đại diện cho mức lương mới. 
2. Kiểm tra xem một trong hai cơ số có bằng không hay không. Nếu cả hai cơ sở đều bằng 0 thì mức lương của cả hai đều được coi là bằng 0 theo quy ước cuộc thi của bài toán, vì vậy hãy in`Lazy`. Nếu chỉ có (B_1) bằng 0 thì lương cũ bằng 0 và lương mới dương nên in`Congrats`. Nếu chỉ có (B_2) bằng 0 thì lương mới bằng 0 và lương cũ dương nên in`HaHa`. Việc kiểm tra này cũng ngăn chặn một cuộc gọi không hợp lệ đến`log(0)`. 
3. Để có cơ số dương, hãy tính 

[ 
x_1=P_1\ln(B_1),\qquad x_2=P_2\ln(B_2). 
] 

Chúng tôi không tính lương gốc. Các giá trị (x_1) và (x_2) là logarit tự nhiên của chúng. 
4. So sánh (x_1) và (x_2). Nếu sự khác biệt của chúng cực kỳ nhỏ, hãy in`Lazy`, bởi vì các phép tính dấu phẩy động biểu thị hai logarit bằng nhau trong phạm vi độ chính xác số dự kiến. 
5. Nếu (x_1<x_2) thì mức lương mới lớn hơn nên in`Congrats`. Ngược lại (x_1>x_2) nghĩa là mức lương mới nhỏ hơn nên in`HaHa`. 

### Tại sao nó hoạt động 

Đối với cơ số dương, logarit tăng nghiêm ngặt nên việc áp dụng nó không thể làm thay đổi thứ tự của hai mức lương. chúng tôi có 

[ 
\ln(B_1^{P_1})=P_1\ln(B_1) 
] 

và 

[ 
\ln(B_2^{P_2})=P_2\ln(B_2). 
] 

Như vậy việc so sánh 2 sản phẩm cũng tương đương với việc so sánh mức lương ban đầu. Cơ số 0 được xử lý riêng trước phép chuyển đổi này, do đó thuật toán không bao giờ đánh giá logarit không xác định. Phép tính gần đúng duy nhất đến từ số học dấu phẩy động, được xử lý với sai số nhỏ như lời giải dự kiến ​​của bài toán mong đợi. Các giải pháp đã xuất bản sử dụng cùng một phép biến đổi logarit và một epsilon xung quanh (10^{-7}). 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        b1, p1, b2, p2 = map(int, input().split())

        if b1 == 0 or b2 == 0:
            if b1 == b2:
                out.append("Lazy")
            elif b1 < b2:
                out.append("Congrats")
            else:
                out.append("HaHa")
            continue

        x1 = p1 * math.log(b1)
        x2 = p2 * math.log(b2)

        if abs(x1 - x2) <= 1e-7:
            out.append("Lazy")
        elif x1 < x2:
            out.append("Congrats")
        else:
            out.append("HaHa")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Chương trình nhập đầu tiên`math`bởi vì toàn bộ sự tối ưu hóa đến từ việc thay thế một lũy thừa khổng lồ bằng logarit của nó. Sản lượng được tích lũy trong`out`và được viết một lần ở cuối, điều này tránh các lệnh gọi đầu ra lặp lại khi có nhiều trường hợp thử nghiệm. 

Nhánh không cơ sở xuất hiện trước mỗi lệnh gọi tới`math.log`. Điều này vừa cần thiết về mặt toán học vừa là một chi tiết triển khai dễ bị bỏ qua. Một cuộc gọi như`math.log(0)`đưa ra một ngoại lệ. 

Đối với các bazơ dương,`p1 * math.log(b1)`chính xác là dạng logarit (P_1\ln B_1). Phép nhân số nguyên của Python ở đây an toàn vì`p1`nhiều nhất là (10^6) và`math.log(b1)`là một giá trị dấu phẩy động thông thường. 

các`1e-7`dung sai tuân theo chiến lược giải pháp dự định được sử dụng trong các giải pháp đã công bố. Việc so sánh được thực hiện trên logarit chứ không phải trên lũy thừa ban đầu, do đó không có số nguyên lớn nào được xây dựng. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, hãy xem xét ba trường hợp thử nghiệm cùng một lúc. 

| (B_1) | (P_1) | (B_2) | (P_2) | (P_1\ln B_1) | (P_2\ln B_2) | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 3 | 4 | 2 | (3\ln2) | (2\ln4=4\ln2) | Xin chúc mừng | 
| 2 | 2 | 3 | 1 | (2\ln2) | (\ln3) | HaHa | 
| 2 | 4 | 4 | 2 | (4\ln2) | (2\ln4=4\ln2) | Lười biếng | 

Trong trường hợp đầu tiên là (3\ln2<4\ln2) nên (2^3<4^2) và mức lương mới lớn hơn. Trong trường hợp thứ hai, (2\ln2>\ln3), do đó (2^2>3). Trường hợp thứ ba chứng minh rằng các cặp số mũ và số mũ khác nhau có thể tạo ra mức lương giống hệt nhau. 

Một dấu vết bổ sung hữu ích là trường hợp cơ số 0 và trường hợp số mũ bằng 0. 

| (B_1) | (P_1) | (B_2) | (P_2) | Chi nhánh | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 5 | 2 | 3 | Không có cơ sở, chỉ có lương cũ bằng 0 | Xin chúc mừng | 
| 7 | 0 | 3 | 2 | Cơ số dương, logarit cho (0) và (2\ln3) | Xin chúc mừng | 
| 2 | 4 | 4 | 2 | Cơ số dương, giá trị logarit bằng nhau | Lười biếng | 

Hàng đầu tiên không bao giờ gọi`log(0)`. Hàng thứ hai cho thấy số mũ bằng 0 tự nhiên trở thành giá trị logarit bằng 0 vì (7^0=1). Hàng cuối cùng xác nhận điều kiện đẳng thức thông qua danh tính (2^4=4^2). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(T)) | Mỗi trường hợp thử nghiệm thực hiện một số phép tính số học và logarit không đổi | 
| Không gian | (O(T)) | Các chuỗi đầu ra được lưu trữ trước lần ghi cuối cùng | 

Phép toán logarit hoạt động trên các số dấu phẩy động thông thường, do đó giá trị của nó không đổi cho mục đích phân tích độ phức tạp của chương trình cạnh tranh. Với các giá trị đầu vào được giới hạn ở (10^6), thuật toán không bao giờ tạo ra các giá trị lương nhiều triệu chữ số khiến việc lũy thừa trực tiếp trở nên kém hấp dẫn. Giới hạn bộ nhớ 256 MB dễ dàng đủ cho trạng thái kích thước không đổi cho mỗi trường hợp kiểm tra và bộ đệm đầu ra. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        b1, p1, b2, p2 = map(int, input().split())

        if b1 == 0 or b2 == 0:
            if b1 == b2:
                out.append("Lazy")
            elif b1 < b2:
                out.append("Congrats")
            else:
                out.append("HaHa")
            continue

        x1 = p1 * math.log(b1)
        x2 = p2 * math.log(b2)

        if abs(x1 - x2) <= 1e-7:
            out.append("Lazy")
        elif x1 < x2:
            out.append("Congrats")
        else:
            out.append("HaHa")

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run(
    """3
2 3 4 2
2 2 3 1
2 4 4 2
"""
) == """Congrats
HaHa
Lazy""", "sample 1"

assert run(
    """1
1 0 1 0
"""
) == "Lazy", "minimum positive-base equality"

assert run(
    """1
0 5 1000000 1000000
"""
) == "Congrats", "zero old salary"

assert run(
    """1
1000000 1000000 999999 1000000
"""
) == "HaHa", "maximum-size values"

assert run(
    """4
2 4 4 2
3 1 3 1
10 0 2 1
2 1 1 1000000
"""
) == """Lazy
Lazy
HaHa
HaHa""", "equality and exponent boundaries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3 4 2`|`Congrats`| So sánh logarit cơ bản | 
|`1 0 1 0`|`Lazy`| Cả hai số mũ đều bằng 0 | 
|`0 5 1000000 1000000`|`Congrats`| Xử lý không có cơ sở | 
|`1000000 1000000 999999 1000000`|`HaHa`| Cường độ đầu vào tối đa mà không cần xây dựng công suất lớn | 
|`2 4 4 2`|`Lazy`| Bình đẳng với các căn cứ khác nhau | 
|`10 0 2 1`|`HaHa`| (10^0=1<2) | 
|`2 1 1 1000000`|`HaHa`| Số mũ lớn trên mức lương thứ hai | 

## Vỏ cạnh 

Cơ số 0 phải được xử lý trước logarit. Vì```
1
0 5 2 3
```mức lương đầu tiên là (0^5=0), trong khi mức lương thứ hai là (2^3=8). Thuật toán đi vào nhánh cơ số 0, thấy chỉ có (B_1) bằng 0 và in`Congrats`. Không có cuộc gọi đến`log(0)`xảy ra. 

Hai cơ sở bằng 0 tạo ra mức lương bằng nhau theo quy ước về cơ sở bằng 0 của cuộc thi. Vì```
1
0 7 0 3
```thuật toán nhìn thấy`b1 == b2 == 0`và in`Lazy`. Số mũ không còn quan trọng khi nhánh cơ số 0 được chọn. 

Số mũ bằng 0 không yêu cầu một nhánh riêng biệt khi cơ số dương. Vì```
1
10 0 2 1
```các giá trị logarit là (0) và (\ln2). Vì (0<\ln2), thuật toán in`Congrats`vì mức lương mới. Để so sánh ngược lại,```
1
2 1 1 1000000
```các giá trị là (\ln2) và (0), do đó thuật toán sẽ in`HaHa`. 

Cuối cùng, sự bình đẳng không thể được suy ra chỉ từ những cơ sở bình đẳng. Với```
1
2 4 4 2
```các giá trị logarit là (4\ln2) và (2\ln4). Vì (\ln4=2\ln2), cả hai biểu thức đều chính xác (4\ln2), do đó sự khác biệt nằm trong dung sai đẳng thức và câu trả lời là`Lazy`. Đây là lý do chính khiến thuật toán so sánh các biểu thức (P\ln B) hoàn chỉnh thay vì kiểm tra từng cặp đầu vào một cách độc lập.
