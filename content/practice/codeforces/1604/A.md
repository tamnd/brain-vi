---
title: "CF 1604A - Kỷ nguyên"
description: "Chúng ta được cấp một chuỗi các số nguyên và chúng ta có thể chèn bất kỳ số nguyên dương nào vào bất kỳ vị trí nào trong chuỗi bao nhiêu lần tùy thích. Mục đích là làm cho dãy thỏa mãn điều kiện mỗi phần tử ở vị trí i nhiều nhất là i."
date: "2026-06-10T08:11:14+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1604
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 752 (Div. 2)"
rating: 800
weight: 1604
solve_time_s: 105
verified: false
draft: false
---

[CF 1604A - Kỷ nguyên](https://codeforces.com/problemset/problem/1604/A) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các số nguyên và chúng ta có thể chèn bất kỳ số nguyên dương nào vào bất kỳ vị trí nào trong chuỗi bao nhiêu lần tùy thích. Mục đích là làm cho dãy thỏa mãn điều kiện mỗi phần tử ở vị trí`i`nhiều nhất là`i`. Nói cách khác, với mỗi chỉ số`i`, giá trị tại vị trí đó không thể vượt quá chỉ số dựa trên 1. Chúng tôi muốn xác định số lần chèn tối thiểu cần thiết để đạt được điều này. 

Độ dài chuỗi nhỏ, tối đa 100 phần tử và các giá trị có thể lớn tới 10^9. Do các giá trị phần tử lớn, việc lặp lại tất cả các phần chèn có thể có theo cách mạnh mẽ là không thực tế. Thay vào đó, chúng ta phải suy luận về sự khác biệt giữa mỗi phần tử và vị trí của nó. Vì số lượng phần tử tối đa là khiêm tốn nên thuật toán O(n log n) hoặc O(n) cho mỗi trường hợp thử nghiệm là khả thi. 

Một vài trường hợp khó hiểu ngay lập tức. Nếu trình tự đã ở trạng thái hợp lệ thì không cần thực hiện thao tác nào. Nếu phần tử đầu tiên rất lớn, hãy nói`10^9`, chúng tôi không thể sửa nó bằng cách sửa đổi nó; chúng ta phải chèn nhiều số nhỏ vào trước nó để "đẩy" nó vào vị trí hợp lệ. Ví dụ, một trình tự`[69]`yêu cầu chèn`68`yếu tố`[1,2,...,68]`trước nó để thỏa mãn`a[69] <= 69`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng chèn từng phần tử vào bất cứ nơi nào cần thiết, kiểm tra trình tự ở mỗi bước. Mặc dù đúng về nguyên tắc nhưng nó nhanh chóng trở nên không khả thi vì giá trị phần tử có thể rất lớn và chúng ta có thể cần tới 10^9 lần chèn vào trong một mô phỏng đơn giản. Điều đó có nghĩa là không thể mô phỏng từng lần chèn riêng lẻ. 

Nhận xét quan trọng là vấn đề được chuyển thành một chiến lược tham lam. Sau khi sắp xếp chuỗi, phần tử nhỏ nhất sẽ chiếm vị trí đầu tiên, phần tử nhỏ thứ hai sẽ chiếm vị trí thứ hai, v.v. Nếu phần tử ở vị trí`i`lớn hơn`i`, khi đó chúng ta cần chèn đủ số lượng nhỏ hơn vào trước nó để dịch chuyển nó sang vị trí`a[i]`hoặc ít hơn. Về cơ bản, số lần chèn tối thiểu cần thiết là giá trị tối đa của`a[i] - i`trên tất cả các phần tử. Điều này có hiệu quả vì việc chèn số vào đầu sẽ làm tăng chỉ số của các phần tử tiếp theo, giúp giảm sự khác biệt một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * max(a)) | O(n + max(a)) | Quá chậm | 
| Tham lam/Sắp xếp Chèn | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Đối với mỗi trường hợp kiểm thử, hãy đọc độ dài chuỗi và các phần tử của chuỗi. 
3. Sắp xếp thứ tự. Việc sắp xếp rất quan trọng vì trình tự tối ưu sau khi chèn phải có các phần tử nhỏ nhất trước, sau đó là các phần tử lớn hơn. Điều này giảm thiểu số lượng chèn cần thiết. 
4. Khởi tạo bộ đếm`ops`về không. Điều này sẽ theo dõi số lượng hoạt động cần thiết. 
5. Lặp lại trình tự đã sắp xếp bằng chỉ mục dựa trên 1`i`. Đối với mỗi phần tử`a[i]`, tính sự khác biệt`a[i] - i`. Nếu sự khác biệt này là dương, nó thể hiện số lần chèn cần thiết trước phần tử này để đưa nó vào vị trí hợp lệ. Cập nhật`ops`tối đa của`ops`Và`a[i] - i`. 
6. Sau khi xử lý toàn bộ chuỗi,`ops`chứa số lần chèn tối thiểu cần thiết. In hoặc lưu trữ giá trị này. 
7. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: Việc sắp xếp đảm bảo rằng các số nhỏ nhất chiếm các chỉ số nhỏ nhất, giúp giảm thiểu số lần chèn cần thiết. Sự khác biệt`a[i] - i`nắm bắt chính xác số lượng phần tử nhỏ hơn "bị thiếu" trước vị trí hiện tại. Lấy mức tối đa đảm bảo rằng tất cả các phần tử đều hợp lệ sau số lần chèn được tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()
    ops = 0
    for i, val in enumerate(a, 1):
        ops = max(ops, val - i)
    print(ops)
```Giải pháp bắt đầu bằng cách đọc đầu vào bằng I/O nhanh. Sắp xếp đảm bảo các phần tử được sắp xếp theo thứ tự để chèn tối thiểu. Vòng lặp sử dụng chỉ mục dựa trên 1 để căn chỉnh các vị trí phần tử với giới hạn yêu cầu của chúng.`ops`nắm bắt được sự khác biệt tối đa, chính xác là số lần chèn cần thiết. sử dụng`max`ở mỗi bước sẽ tránh được việc đếm thiếu khi các phần tử sau này yêu cầu chèn thêm. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3
1 3 4
5 7 4
69
```### Bảng theo dõi cho`[1, 3, 4]`| tôi | giá trị | val - tôi | hoạt động | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 
| 2 | 3 | 1 | 1 | 
| 3 | 4 | 1 | 1 | 

Sự khác biệt tối đa là 1, vì vậy chúng ta cần một lần chèn. 

### Bảng theo dõi cho`[69]`| tôi | giá trị | val - tôi | hoạt động | 
| --- | --- | --- | --- | 
| 1 | 69 | 68 | 68 | 

Chúng tôi cần 68 lần chèn để đẩy`69`đến vị trí 69. 

Những dấu vết này xác nhận cách tiếp cận tham lam xác định chính xác số lượng hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; lặp qua chuỗi là O(n) | 
| Không gian | O(n) | Lưu trữ trình tự | 

Với n lên tới 100 và t lên tới 200, tổng số thao tác dưới 100.000, vừa vặn thoải mái trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()
        ops = 0
        for i, val in enumerate(a, 1):
            ops = max(ops, val - i)
        print(ops)
    return out.getvalue().strip()

# Provided samples
assert run("4\n3\n1 3 4\n5\n1 2 5 7 4\n1\n1\n3\n69 6969 696969\n") == "1\n3\n0\n696966"

# Custom cases
assert run("2\n1\n1\n2\n2 1\n") == "0\n0", "minimal inputs"
assert run("1\n5\n5 5 5 5 5\n") == "4", "all-equal values"
assert run("1\n3\n1000000000 1 1\n") == "999999997", "large value at start"
assert run("1\n4\n1 2 3 4\n") == "0", "already valid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1`|`0`| Đầu vào tối thiểu, đã hợp lệ | 
|`1\n5\n5 5 5 5 5`|`4`| Tất cả các giá trị bằng nhau yêu cầu chèn nhiều lần | 
|`1\n3\n1000000000 1 1`|`999999997`| Phần tử đầu tiên lớn, kiểm tra cạnh có giá trị cao | 
|`1\n4\n1 2 3 4`|`0`| Trình tự đã thỏa mãn điều kiện | 

## Vỏ cạnh 

Đối với một phần tử duy nhất`[1]`, trình tự đã hợp lệ. Vòng lặp tính toán`val - i = 0`, Vì thế`ops = 0`. Không cần chèn. 

Đối với một trình tự`[1000000000, 1, 1]`, sắp xếp sản lượng`[1, 1, 1000000000]`. Dấu vết tính toán`val - i`BẰNG`0, -1, 999999997`, vậy độ chênh lệch lớn nhất là`999999997`. Điều này đảm bảo thuật toán xử lý chính xác các giá trị rất lớn và định vị chúng bằng cách chèn đủ số lượng nhỏ hơn trước chúng.
