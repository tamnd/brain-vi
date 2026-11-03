---
title: "CF 1118A - Mua nước"
description: "Polycarp cần mua chính xác $n$ lít nước bằng cách sử dụng các chai có hai kích cỡ cố định: chai 1 lít giá $a$ và chai 2 lít giá $b$. Đối với mỗi truy vấn, chúng ta được yêu cầu xác định tổng chi phí tối thiểu để thu được chính xác $n$ lít."
date: "2026-06-12T04:36:42+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1118
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 540 (Div. 3)"
rating: 800
weight: 1118
solve_time_s: 323
verified: false
draft: false
---

[CF 1118A - Mua nước](https://codeforces.com/problemset/problem/1118/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 5 phút 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Polycarp cần mua chính xác$n$lít nước sử dụng chai có hai kích cỡ cố định: chai 1 lít theo giá$a$và chai 2 lít có giá$b$. Đối với mỗi truy vấn, chúng tôi được yêu cầu xác định tổng chi phí tối thiểu để có được chính xác$n$lít. Không giới hạn số lượng mua được và quán luôn có sẵn cả 2 loại. 

Các đầu vào được cấu trúc dưới dạng nhiều truy vấn độc lập. Mỗi truy vấn chỉ rõ số lít Polycarp cần, giá chai 1 lít và giá chai 2 lít. Đầu ra là một số nguyên cho mỗi truy vấn, chi phí tối thiểu. 

Những hạn chế cho thấy$n$có thể lớn như$10^{12}$, trong khi$a$Và$b$nhỏ hơn nhiều (lên tới 1000). Điều này ngụ ý rằng bất kỳ giải pháp nào lặp lại trên tất cả các tổ hợp chai có thể có đều không khả thi. Chúng tôi cần một giải pháp tính toán chi phí tối thiểu bằng cách sử dụng công thức hoặc quyết định theo thời gian không đổi cho mỗi truy vấn. 

Xuất hiện trường hợp cạnh tranh khi mua hai chai 1 lít sẽ rẻ hơn mua chai 2 lít. Ví dụ, nếu$a = 1$Và$b = 3$, mua 2 chai 1 lít giá 2 chai, nhỏ hơn chai 2 lít giá 3. Tương tự, nếu$n$Thật kỳ lạ, không thể đổ đầy toàn bộ chai 2 lít, vì vậy chúng ta phải thêm đúng một chai 1 lít. Các giải pháp bất cẩn có thể cho rằng chúng ta luôn muốn có càng nhiều chai 2 lít càng tốt, điều này sẽ thất bại khi hai chai 1 lít rẻ hơn một chai 2 lít. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là lặp lại tất cả số lượng có thể có của chai 2 lít từ 0 đến$n/2$, tính số lít còn lại để đổ vào chai 1 lít, tính tổng chi phí cho mỗi cách kết hợp và lấy giá trị nhỏ nhất. Cách tiếp cận này đúng vì nó kiểm tra tất cả các kết hợp hợp lệ, nhưng nó trở nên không khả thi khi$n$là lớn. Vì$n = 10^{12}$, thậm chí lặp đi lặp lại$n/2$thời gian là không thể chậm. 

Quan sát quan trọng để có giải pháp nhanh hơn là mỗi chai 2 lít có thể được thay thế bằng hai chai 1 lít. Vì vậy, chi phí hiệu quả của chai 2 lít nên được so sánh với chi phí hiệu quả của chai 1 lít. Nếu mua hai chai 1 lít rẻ hơn chai 2 lít thì mua chai 1 lít vẫn tốt hơn. Ngược lại, chúng ta có thể tham lam dùng càng nhiều chai 2 lít càng tốt, chỉ dùng chai 1 lít nếu$n$thật kỳ quặc. 

Giải pháp tối ưu có hiệu quả vì việc lựa chọn cho mỗi chai 2 lít là độc lập và có thể rút gọn thành một phép so sánh đơn giản: nếu$b < 2a$, hãy sử dụng chai 2 lít nếu có thể; nếu không thì thay toàn bộ chai 2 lít bằng 2 chai 1 lít. Điều này làm giảm vấn đề thành tính toán theo thời gian không đổi cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm đối với n lớn | 
| Tối ưu | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi truy vấn, hãy đọc$n$,$a$, Và$b$. 
2. Tính chi phí hiệu quả khi sử dụng chai 2 lít: nếu$2a < b$, bộ$b = 2a$. Điều này đảm bảo rằng việc mua hai chai 1 lít luôn được cân nhắc nếu rẻ hơn chai 2 lít. 
3. Tính số chai 2 lít cần mua là$n // 2$, số lượng tối đa phù hợp$n$lít. 
4. Tính số chai 1 lít là$n \% 2$, lít còn lại sau khi dùng bình 2 lít. 
5. Tính tổng chi phí như sau$(n // 2) \times b + (n \% 2) \times a$. Công thức này dùng cho cả bình 2 lít và bình 1 lít. 
6. In tổng chi phí cho truy vấn. 

Tại sao hiệu quả: Bằng cách điều chỉnh chi phí 2 lít không quá gấp đôi chi phí 1 lít, chúng tôi đảm bảo rằng việc sử dụng chai 2 lít không bao giờ tệ hơn việc sử dụng hai chai 1 lít. Tham lam sử dụng càng nhiều chai 2 lít càng tốt sẽ giảm thiểu số lượng chai, đồng thời xử lý một lít còn sót lại bằng chai 1 lít đảm bảo chính xác$n$lít. Không có sự kết hợp nào khác có thể mang lại chi phí thấp hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

q = int(input())
for _ in range(q):
    n, a, b = map(int, input().split())
    b = min(b, 2 * a)  # cheaper to buy two 1-liter bottles than one 2-liter bottle
    total_cost = (n // 2) * b + (n % 2) * a
    print(total_cost)
```Giải pháp này đọc từng truy vấn, điều chỉnh giá thành chai 2 lít nếu mua 2 chai 1 lít rẻ hơn, tính xem cần mua bao nhiêu chai 2 lít và cần bao nhiêu chai 1 lít còn sót lại, sau đó in ra tổng chi phí. Nó xử lý rất lớn$n$bởi vì nó không bao giờ lặp lại trên từng lít. Các trường hợp cạnh như$n = 1$hoặc$b > 2a$được quản lý chính xác bởi`min(b, 2 * a)`đường kẻ. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
10 1 3
```| n | một | b | b điều chỉnh | n//2 | n%2 | Tổng chi phí | 
| --- | --- | --- | --- | --- | --- | --- | 
| 10 | 1 | 3 | 2 | 5 | 0 | 10 | 

Giải thích: Vì hai chai 1 lít có giá 2 < 3 nên chúng ta thay giá chai 2 lít bằng 2. Chúng ta có thể mua 5 chai 2 lít để đựng 10 lít mà không cần chai 1 lít. Tổng chi phí là 5*2 = 10. 

Đầu vào thứ hai:```
7 3 2
```| n | một | b | b điều chỉnh | n//2 | n%2 | Tổng chi phí | 
| --- | --- | --- | --- | --- | --- | --- | 
| 7 | 3 | 2 | 2 | 3 | 1 | 3_2 + 1_3 = 9 | 

Giải thích: Ở đây, chai 2 lít đã rẻ hơn hai chai 1 lít (2 < 6). Ta mua 3 chai 2 lít (6 lít) và 1 chai 1 lít (1 lít) để pha được 7 lít. Tổng chi phí là 9. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi truy vấn được tính toán theo thời gian không đổi; có q truy vấn. | 
| Không gian | O(1) | Chỉ có một vài biến cho mỗi truy vấn; không có bộ nhớ bổ sung phụ thuộc vào n. | 

Với q lên tới 500 và n lên tới 10^12, việc tính toán theo thời gian không đổi cho mỗi truy vấn sẽ hiệu quả và phù hợp với giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    q = int(input())
    for _ in range(q):
        n, a, b = map(int, input().split())
        b = min(b, 2 * a)
        print((n // 2) * b + (n % 2) * a)
    return output.getvalue().strip()

# Provided samples
assert run("4\n10 1 3\n7 3 2\n1 1000 1\n1000000000000 42 88\n") == "10\n9\n1000\n42000000000000"

# Custom cases
assert run("1\n1 1 100\n") == "1", "minimum n, large b"
assert run("1\n2 5 8\n") == "10", "cheaper to use two 1-liter bottles"
assert run("1\n3 3 4\n") == "7", "odd n, 2-liter cheaper than 2*1-liter"
assert run("1\n1000000000000 1 3\n") == "2000000000000", "maximum n, 2*1 < 2-liter cost"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 100 | 1 | Chai một lít, chai 2 lít cực đắt | 
| 2 5 8 | 10 | Hai chai 1 lít rẻ hơn 2 lít | 
| 3 3 4 | 7 | Lẻ n có 2 lít rẻ hơn 2*1 lít | 
| 1000000000000 1 3 | 2000000000000 | N tối đa, rất lớn, thuật toán xử lý số lớn | 

## Vỏ cạnh 

cho$n = 1$,$a = 1000$,$b = 1$, bộ thuật toán$b = min(b, 2a) = 1$nhưng chỉ cần 1 lít. Nó tính toán`n//2 = 0`chai hai lít và`n%2 = 1`chai một lít. Tổng chi phí là`1 * a = 1000`, đúng là bỏ qua chai 2 lít rẻ tiền vì chúng ta chỉ cần một lít
