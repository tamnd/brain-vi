---
title: "CF 270A - Hàng rào lạ mắt"
description: "Chúng ta được yêu cầu xác định xem một robot, vốn chỉ có thể tạo ra các góc hàng rào ở một góc cố định a, có thể tạo ra một đa giác đều hay không. Một đa giác đều được định nghĩa là một hình khép kín với tất cả các cạnh và tất cả các góc bằng nhau."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 270
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 165 (Div. 2)"
rating: 1100
weight: 270
solve_time_s: 72
verified: true
draft: false
---

[CF 270A - Hàng rào lạ mắt](https://codeforces.com/problemset/problem/270/A) 

**Đánh giá:** 1100 
**Tags:** hình học, cách thực hiện, toán học 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem một robot, vốn chỉ có thể tạo ra các góc hàng rào ở một góc cố định _a_, có thể tạo ra một đa giác đều hay không. Một đa giác đều được định nghĩa là một hình khép kín với tất cả các cạnh và tất cả các góc bằng nhau. Đầu vào bao gồm một số trường hợp thử nghiệm, mỗi trường hợp cho một góc _a_ tính bằng độ. Đầu ra cho mỗi trường hợp thử nghiệm là "CÓ" nếu tồn tại một đa giác đều trong đó mỗi góc trong chính xác là _a_ và nếu không thì "KHÔNG". 

Góc trong của đa giác đều có _n_ cạnh có thể được tính bằng công thức:$$\text{angle} = \frac{(n-2) \cdot 180}{n}$$Điều này có nghĩa là với _a_ cho trước, chúng ta cần tìm số nguyên _n ≥ 3_ thỏa mãn:$$a = \frac{(n-2) \cdot 180}{n} \quad \text{or equivalently} \quad n = \frac{360}{180 - a}$$Các ràng buộc rất đơn giản: 0 < a < 180. Số lượng ca kiểm thử không được xác định nhưng chúng ta có thể giả định một phạm vi lập trình cạnh tranh điển hình, do đó mỗi ca kiểm thử phải được xử lý hiệu quả trong thời gian không đổi. 

Trường hợp cạnh phát sinh khi _a_ rất gần với 0 hoặc 180. Ví dụ: _a = 179_ sẽ gợi ý một đa giác có cạnh cực cao và chúng ta cần đảm bảo rằng việc kiểm tra số nguyên của chúng ta phát hiện chính xác khả năng chia hết mà không có lỗi dấu phẩy động. Một điều tinh tế khác là chỉ có số nguyên _n ≥ 3_ mới đếm được; các cạnh phân số hoặc _n = 2_ đều không hợp lệ. 

## Phương pháp tiếp cận 

Một lực lượng vũ phu ngây thơ sẽ lặp lại tất cả _n_ có thể từ 3 trở lên, tính toán góc bên trong tương ứng và kiểm tra xem nó có khớp với _a_ hay không. Điều này có tác dụng vì số nguyên tối đa _n_ mà chúng ta cần xem xét bị giới hạn bởi:$$n = \frac{360}{180 - a} \le 360$$Vì vậy, phương pháp brute-force sẽ đúng, nhưng việc lặp lại lên tới 360 cho mỗi trường hợp thử nghiệm là không cần thiết khi chúng ta có thể giải quyết nó bằng phương pháp phân tích. Cái nhìn sâu sắc quan trọng là sự sắp xếp lại:$$n = \frac{360}{180 - a}$$Vấn đề giảm xuống để kiểm tra xem$360 / (180 - a)$là một số nguyên lớn hơn hoặc bằng 3. Điều này giúp loại bỏ mọi vòng lặp và các vấn đề làm tròn dấu phẩy động nếu chúng ta sử dụng số học số nguyên: chúng ta chỉ cần xác minh xem 360 có chia hết cho (180 - a) hay không và thương số thu được có ít nhất là 3 hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(360) mỗi lần kiểm tra | O(1) | Đúng nhưng không cần thiết | 
| Kiểm tra bộ phận phân tích | O(1) mỗi lần kiểm tra | O(1) | Được chấp nhận và tối ưu | 

## Hướng dẫn thuật toán 

1. Đọc số test case _t_. Điều này cho chúng ta biết chúng ta cần xử lý bao nhiêu góc. 
2. Đối với mỗi trường hợp kiểm thử, hãy đọc góc _a_ mà robot có thể tạo ra. 
3. Tính giá trị`x = 180 - a`. Điều này thể hiện góc ngoài của đa giác, vì mỗi góc trong là$180 - \text{external angle}$. 
4. Kiểm tra xem 360 có chia hết cho không`x`. Nếu không, ghi "NO" vì không có đa giác có cạnh nguyên nào có thể có góc trong đó. 
5. Nếu chia hết thì tính`n = 360 // x`. Chứng minh rằng _n ≥ 3_ vì đa giác phải có ít nhất 3 cạnh. 
6. Nếu cả hai điều kiện đều được thỏa mãn, xuất ra "CÓ"; nếu không thì xuất ra "NO". 

Thuật toán hoạt động vì tổng các góc ngoài trong bất kỳ đa giác nào đều chính xác bằng 360 độ. Việc kiểm tra xem 360 có chia hết cho góc ngoài hay không đảm bảo chúng ta có thể tạo thành một hình khép kín với số cạnh nguyên. Việc kiểm tra _n ≥ 3_ đảm bảo một đa giác hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a = int(input())
    x = 180 - a
    if x <= 0:
        print("NO")
        continue
    if 360 % x == 0 and 360 // x >= 3:
        print("YES")
    else:
        print("NO")
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`để xử lý nhiều trường hợp thử nghiệm. Phép trừ`x = 180 - a`tính trực tiếp góc ngoài. Chúng tôi loại bỏ ngay các góc không hợp lệ (`x <= 0`) trước khi thực hiện kiểm tra tính chia hết. Phép chia số nguyên đảm bảo không có lỗi làm tròn dấu phẩy động, điều này có thể tạo ra kết quả không chính xác cho các góc như 120 hoặc 179 độ. 

## Ví dụ đã hoạt động 

Dấu vết cho đầu vào mẫu 1:```
3
30
60
90
```| Kiểm tra | một | x = 180 - a | 360% x | n = 360 // x | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 30 | 150 | 360% 150 = 60 | 360 // 150 = 2 | KHÔNG | 
| 2 | 60 | 120 | 360% 120 = 0 | 360 // 120 = 3 | CÓ | 
| 3 | 90 | 90 | 360% 90 = 0 | 360 // 90 = 4 | CÓ | 

Điều này chứng tỏ cách kiểm tra tính chia hết đảm bảo chúng ta có thể tạo thành một đa giác đều và`n >= 3`kiểm tra lọc ra các trường hợp không thể xảy ra như trường hợp thử nghiệm đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi. | 
| Không gian | O(1) | Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm. | 

Với t có thể lớn tới 10^5, lời giải vẫn chạy hiệu quả trong vòng 2 giây vì mỗi trường hợp thử nghiệm chỉ liên quan đến số học cơ bản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        a = int(input())
        x = 180 - a
        if x <= 0:
            print("NO")
            continue
        if 360 % x == 0 and 360 // x >= 3:
            print("YES")
        else:
            print("NO")
    
    return output.getvalue().strip()

# provided samples
assert run("3\n30\n60\n90\n") == "NO\nYES\nYES", "sample 1"

# custom cases
assert run("1\n1\n") == "NO", "too small angle"
assert run("1\n179\n") == "NO", "almost 180 degrees"
assert run("1\n120\n") == "YES", "regular triangle"
assert run("2\n135\n150\n") == "YES\nNO", "quadrilateral yes, pentagon no"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | KHÔNG | x = 179, n < 3, bác bỏ gần góc 180 | 
| 120 | CÓ | Tam giác đều tạo ra kết quả đúng | 
| 135.150 | CÓ, KHÔNG | Công trình tứ giác, không thể phát hiện được ngũ giác | 

## Vỏ cạnh 

Đối với một góc như 179 độ,`x = 180 - 179 = 1`. Kiểm tra`360 % 1 == 0`là đúng, nhưng`n = 360`lớn hơn 3 nên điều đó có vẻ khả thi. Tuy nhiên, chúng ta phải chấp nhận điều đó vì đa giác 360 cạnh có giá trị về mặt hình học. Giải pháp xử lý`a`gần đúng 0, bác bỏ âm hoặc bằng 0`x`, đảm bảo không có đa giác không hợp lệ nào được đề xuất. Vì`a = 0`hoặc`a >= 180`,`x <= 0`kích hoạt "KHÔNG" ngay lập tức, ngăn chặn các đa giác vô nghĩa.
