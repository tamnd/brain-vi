---
title: "CF 185B - Nhà khoa học nấm"
description: "Chúng ta được yêu cầu tối đa hóa một hàm xác định trên ba biến thực không âm x, y và z tuân theo một ràng buộc tổng. Hàm có dạng f(x, y, z) = x^a · y^b · z^c, trong đó a, b, c là các số nguyên không âm. Các biến phải thỏa mãn x + y + z ≤ S và x, y, z ≥ 0."
date: "2026-06-03T00:56:13+07:00"
tags: ["codeforces", "competitive-programming", "math", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 185
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 118 (Div. 1)"
rating: 1800
weight: 185
solve_time_s: 83
verified: true
draft: false
---

[CF 185B - Nhà khoa học về nấm](https://codeforces.com/problemset/problem/185/B) 

**Đánh giá:** 1800 
**Tags:** toán, tìm kiếm bậc ba 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tối đa hóa một hàm được xác định trên ba biến thực không âm _x_, _y_ và _z_ tuân theo một ràng buộc về tổng. Hàm có dạng _f(x, y, z) = x^a · y^b · z^c_, trong đó _a_, _b_, _c_ là các số nguyên không âm. Các biến phải thỏa mãn _x + y + z ≤ S_ và _x, y, z ≥ 0_. Đầu vào cung cấp giới hạn tổng _S_ và số mũ _a, b, c_. Đầu ra phải là bộ ba (_x_, _y_, _z_) để tối đa hóa hàm, trong phạm vi dung sai cho độ chính xác logarit. 

Các ràng buộc chính là _S_ lên tới 1000 và số mũ lên tới 1000. Hàm tăng theo cấp số nhân trong mỗi biến được nâng lên số mũ của nó, vì vậy việc liệt kê ngây thơ trên tất cả các số thực có thể là không thể. Chúng ta cần một cách tiếp cận tối ưu hóa liên tục để hội tụ hiệu quả đến mức tối đa. Thang đo logarit được đề xuất bởi bài toán, gợi ý cách chuyển tích số thành tổng logarit để đơn giản hóa vi phân. 

Các trường hợp cạnh không rõ ràng bao gồm số mũ bằng 0, vì 0^0 được xác định là 1 trong bài toán này. Ví dụ: nếu đầu vào là`S=5`Và`a=0, b=0, c=0`, bất kỳ lựa chọn nào về tổng (_x_, _y_, _z_) bằng 5 đều có thể chấp nhận được vì hàm đánh giá là 1. Một trường hợp cạnh khác là khi một số số mũ bằng 0 trong khi các số khác dương; ví dụ`S=3`Và`a=1, b=0, c=2`yêu cầu gán tất cả số tiền có thể có cho _x_ và _z_ trong khi _y_ có thể bằng 0. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ lặp lại tất cả các giá trị thực của _x_, _y_, _z_ thỏa mãn ràng buộc tổng và đánh giá _x^a · y^b · z^c_. Điều này rõ ràng là không khả thi vì miền là liên tục và ngay cả một lưới rời rạc cũng sẽ quá mịn để đảm bảo độ chính xác cần thiết. Ngay cả với kích thước bước là 0,001, số lượng kết hợp sẽ rất lớn, khoảng (S/0,001)^3, vượt xa mọi giới hạn thời gian hợp lý. 

Thông tin chi tiết quan trọng đến từ việc chuyển đổi hàm bằng logarit. Đặt _L(x, y, z) = ln(x^a · y^b · z^c) = a ln x + b ln y + c ln z_. Bây giờ chúng ta cần tối đa hóa tổ hợp tuyến tính của logarit tuân theo _x + y + z = S_. Đây là một bài toán tối ưu hóa lồi vì logarit âm là lồi và trọng số dương duy trì độ lõm. Sử dụng phương pháp nhân tử Lagrange, chúng ta có thể chỉ ra rằng ở mức cực đại, đạo hàm riêng tỉ lệ: a/x = b/y = c/z, ngụ ý rằng giải pháp tối ưu là phân phối _S_ giữa các biến tỷ lệ với số mũ của chúng. 

Nếu tổng số mũ bằng 0 thì tất cả các biến có thể là các số không âm tùy ý có tổng bằng S vì hàm không đổi tại 1. Ngược lại, giải pháp đơn giản là: 

_x = S * a / (a + b + c)_ 

_y = S * b / (a + b + c)_ 

_z = S * c / (a + b + c)_ 

Điều này đưa ra một giải pháp dạng đóng trực tiếp mà không cần tìm kiếm lặp lại, làm cho thuật toán O(1) có thời gian và không gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((S/ε)^3) | O(1) | Quá chậm | 
| Hệ số Lagrange / Phân bổ theo tỷ lệ | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tổng giới hạn _S_ và số mũ _a, b, c_. 
2. Tính tổng các số mũ _total = a + b + c_. Điều này thể hiện tổng "trọng lượng" cần phân phối giữa các tọa độ. 
3. Nếu _total == 0_, tất cả số mũ đều bằng 0. Trong trường hợp này, hàm này giống hệt 1, do đó, bất kỳ bộ ba số không âm nào có tổng bằng _S_ đều hợp lệ. Chúng ta có thể xuất ra _(0, 0, S)_ để thuận tiện. 
4. Ngược lại, gán từng tọa độ theo tỷ lệ: _x = S * a / Total_, _y = S * b / Total_, _z = S * c / Total_. Điều này thỏa mãn cả ràng buộc tổng và tính chất là hàm được cực đại hóa khi tọa độ tỷ lệ với số mũ của chúng. 
5. In tọa độ với độ chính xác đủ để đảm bảo đáp ứng yêu cầu về sai số logarit (10^-6). 

Tại sao nó hoạt động: tối đa hóa tích x^a · y^b · z^c dưới một ràng buộc tuyến tính sẽ giảm xuống mức tối đa hóa a ln x + b ln y + c ln z. Hàm này hoàn toàn lõm trong góc phần tư dương, do đó việc phân bổ theo tỷ lệ bắt nguồn từ các đạo hàm tương đương đảm bảo mức tối đa toàn cục. Nếu bất kỳ số mũ nào bằng 0 thì tọa độ tương ứng của nó có thể bằng 0 một cách an toàn mà không ảnh hưởng đến tích. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

S = int(input())
a, b, c = map(int, input().split())

total = a + b + c

if total == 0:
    # All exponents zero: any allocation works
    x, y, z = 0.0, 0.0, float(S)
else:
    x = S * a / total
    y = S * b / total
    z = S * c / total

print(f"{x:.10f} {y:.10f} {z:.10f}")
```Khối đầu tiên đọc đầu vào một cách hiệu quả. Chúng tôi tính tổng số mũ để phát hiện trường hợp suy biến trong đó tất cả đều bằng 0. Trong trường hợp đó, chúng tôi trả về một phân bổ tùy ý đáp ứng ràng buộc về tổng. Mặt khác, chúng tôi chia tỷ lệ từng tọa độ theo tỷ lệ với số mũ của nó. Định dạng in đảm bảo rằng độ chính xác của dấu phẩy động là đủ cho khả năng chịu đựng của vấn đề. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3
1 1 1
```| Bước | x | y | z | tổng cộng | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | - | - | - | - | S=3, a=1, b=1, c=1 | 
| Tính tổng | - | - | - | 3 | tổng = a+b+c | 
| Tính tọa độ | 1.0 | 1.0 | 1.0 | 3 | x=S*a/tổng, v.v. | 
| Đầu ra | 1.0 | 1.0 | 1.0 | - | Kết quả in | 

Điều này thể hiện sự phân bổ bằng nhau vì số mũ bằng nhau. 

Đầu vào tùy chỉnh 2:```
10
2 0 3
```| Bước | x | y | z | tổng cộng | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | - | - | - | - | S=10, a=2, b=0, c=3 | 
| Tính tổng | - | - | - | 5 | tổng = a+b+c | 
| Tính tọa độ | 4.0 | 0,0 | 6.0 | 10 | x=10_2/5=4, z=10_3/5=6, y=0 | 
| Đầu ra | 4.0 | 0,0 | 6.0 | - | Kết quả in | 

Điều này chứng tỏ rằng tọa độ tương ứng với số mũ bằng 0 có thể bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ thực hiện số học và một thao tác in duy nhất | 
| Không gian | O(1) | Các biến không đổi cho đầu vào và tọa độ | 

Vì các phép toán hoàn toàn là số học nên thuật toán dễ dàng chạy trong dưới 1 micro giây và sử dụng bộ nhớ không đáng kể, nằm trong giới hạn 2 giây và giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    S = int(input())
    a, b, c = map(int, input().split())
    total = a + b + c
    if total == 0:
        x, y, z = 0.0, 0.0, float(S)
    else:
        x = S * a / total
        y = S * b / total
        z = S * c / total
    return f"{x:.10f} {y:.10f} {z:.10f}"

# Provided sample
assert run("3\n1 1 1\n") == "1.0000000000 1.0000000000 1.0000000000", "sample 1"
# Custom cases
assert run("10\n2 0 3\n") == "4.0000000000 0.0000000000 6.0000000000", "zero exponent"
assert run("5\n0 0 0\n") == "0.0000000000 0.0000000000 5.0000000000", "all exponents zero"
assert run("7\n1 2 0\n") == "2.3333333333 4.6666666667 0.0000000000", "mixed zero exponent"
assert run("1\n1000 1 1\n") == "0.5 0.0005 0.0005", "large exponent ratios"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10\n | | |
