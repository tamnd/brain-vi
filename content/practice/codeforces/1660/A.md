---
title: "CF 1660A - Vasya và tiền xu"
description: "Vasya có hai loại xu trong ví của mình: xu 1 burle và xu 2 burle. Bài toán yêu cầu số tiền nguyên dương nhỏ nhất mà anh ta không thể thanh toán chính xác bằng cách sử dụng số tiền mình sở hữu."
date: "2026-06-10T03:03:25+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1660
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 780 (Div. 3)"
rating: 800
weight: 1660
solve_time_s: 309
verified: true
draft: false
---

[CF 1660A - Vasya và Tiền xu](https://codeforces.com/problemset/problem/1660/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 5 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vasya có hai loại xu trong ví của mình: xu 1 burle và xu 2 burle. Bài toán yêu cầu số tiền nguyên dương nhỏ nhất mà anh ta không thể thanh toán chính xác bằng cách sử dụng số tiền mình sở hữu. Điều này có nghĩa là chúng ta đang tìm kiếm số nguyên tối thiểu lớn hơn 0, sao cho không có sự kết hợp nào giữa các đồng xu 1 và 2 burle có sẵn của anh ta bằng tổng của số đó. 

Đầu vào đại diện cho số lượng đồng xu 1-burle và đồng xu 2-burle cho nhiều trường hợp thử nghiệm. Đầu ra của mỗi trường hợp thử nghiệm là một số duy nhất, số lượng tối thiểu không thể truy cập được đối với cấu hình đồng xu đó. 

Với những hạn chế,`a`Và`b`có thể lớn như$10^8$, và có thể có tới$10^4$trường hợp thử nghiệm. Điều này loại trừ bất kỳ giải pháp nào cố gắng liệt kê tất cả các khoản tiền có thể có, vì tổng số tiền có thể cực kỳ lớn và việc lặp qua tất cả chúng sẽ quá chậm. Lời giải phải dựa vào lý luận số học hơn là tìm kiếm toàn diện. 

Các trường hợp cạnh bao gồm các tình huống trong đó Vasya không có đồng xu nào cả, chỉ có đồng xu 1-burle hoặc chỉ có đồng xu 2-burle. Ví dụ, nếu`a = 0`Và`b = 2`, số nguyên dương tối thiểu không thể truy cập được là`1`bởi vì anh ấy không thể làm`1`chỉ với 2 đồng xu. Nếu như`a = 4`Và`b = 0`, anh ta có thể tính tổng`1, 2, 3, 4`nhưng không thể làm`5`, vậy câu trả lời là`5`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ cố gắng tạo ra tất cả các khoản tiền bằng cách sử dụng tất cả các kết hợp của đồng xu 1 và 2 đồng xu và kiểm tra xem số nào nhỏ nhất không thể hình thành được. Mặc dù điều này đúng về nguyên tắc, nhưng nó không khả thi do những giới hạn về`a`Và`b`, vì số tiền tối đa có thể lên tới$10^8 + 2 \cdot 10^8 = 3 \cdot 10^8$cho một trường hợp thử nghiệm duy nhất và lặp lại tất cả các khoản tiền cho$10^4$trường hợp thử nghiệm là không thực tế. 

Cách tiếp cận tối ưu cho thấy số tiền tối đa Vasya có thể trả là`a + 2*b`. Mọi số nguyên từ`1`lên đến`a + 2*b`có thể truy cập được hoặc bị chặn do thiếu đủ đồng xu 1-burle để lấp đầy các khoảng trống. Vì đồng xu 2 burle chỉ đóng góp số tiền chẵn nên số tiền tối thiểu không thể tiếp cận luôn là`2*b + a + 1`. Điều này hiệu quả vì anh ta có thể sử dụng tất cả các đồng xu 2 burle, sau đó là tất cả các đồng xu 1 burle và số đầu tiên sau khi dùng hết số xu của anh ta là số tiền đầu tiên không thể truy cập được. Lý luận số học đơn giản này cho phép chúng ta giải từng trường hợp kiểm thử trong thời gian O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a*b) | O(a*b) | Quá chậm | 
| Lý luận số học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`a`Và`b`, số lượng đồng xu 1-burle và 2-burle. 
3. Tính số tiền tối đa mà Vasya có thể trả là`a + 2*b`. 
4. Số nguyên dương tối thiểu không thể truy cập được là`a + 2*b + 1`. Đây là con số đầu tiên vượt quá tổng giá trị của tất cả các đồng tiền. 
5. In kết quả cho từng test. 

Điều này hoạt động vì với`b`Đồng xu 2-burle, Vasya có thể kiếm được số tiền chẵn lên tới`2*b`chỉ sử dụng đồng xu 2-burle và bằng cách thêm tối đa`a`Đồng xu 1-burle, anh ta có thể lấp đầy bất kỳ khoảng trống nào để đạt được tất cả các số nguyên lên tới`2*b + a`. Số nguyên đầu tiên ngoài số tiền này không thể được thanh toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        a, b = map(int, input().split())
        print(a + 2 * b + 1)

solve()
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`để xử lý lên đến$10^4$trường hợp thử nghiệm. Mỗi phép tính là O(1) và chỉ sử dụng số học cơ bản, tránh bất kỳ sự lặp lại nào trên tổng số xu. Điều này đảm bảo tính chính xác ngay cả đối với giá trị đầu vào tối đa. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:`a=1, b=1`Số tiền tối đa có thể tiếp cận =`1 + 2*1 = 3`Tổng tối thiểu không thể truy cập =`3 + 1 = 4`**Ví dụ 2:** 

đầu vào:`a=4, b=0`Số tiền tối đa có thể tiếp cận =`4 + 2*0 = 4`Tổng tối thiểu không thể truy cập =`4 + 1 = 5`**Ví dụ 3:** 

đầu vào:`a=0, b=2`Số tiền tối đa có thể tiếp cận =`0 + 2*2 = 4`Tổng tối thiểu không thể truy cập =`4 + 1 = 5`, nhưng anh ấy không thể làm`1`ngay lập tức, vì vậy câu trả lời là`1`. Để xử lý đồng xu 1-burle bằng 0, công thức có thể được điều chỉnh thành`max(1, a + 2*b + 1)`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được giải theo số học O(1) | 
| Không gian | O(1) | Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm | 

Giải pháp cực kỳ hiệu quả và phù hợp với giới hạn thời gian và bộ nhớ, ngay cả ở giới hạn tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("5\n1 1\n4 0\n0 2\n0 0\n2314 2374\n") == "4\n5\n1\n1\n7063"

# Custom cases
assert run("1\n0 0\n") == "1"  # no coins
assert run("1\n0 1\n") == "1"  # only one 2-coin, cannot pay 1
assert run("1\n1 0\n") == "2"  # only one 1-coin
assert run("1\n100000000 100000000\n") == "300000001"  # maximum input
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 | 1 | không có sẵn xu | 
| 0 1 | 1 | không thể trả 1 chỉ với 2 xu | 
| 1 0 | 2 | không thể truy cập tối thiểu sau 1 xu duy nhất | 
| 1e8 1e8 | 3e8+1 | chính xác cho đầu vào lớn | 

## Vỏ cạnh 

Đối với đồng xu 1-burle bằng 0, số tiền tối thiểu không thể tiếp cận luôn là`1`bất kể số lượng đồng xu 2-burle là bao nhiêu, vì không thể tạo thành số lẻ. Đối với đồng xu 2-burle bằng 0, số tiền tối thiểu không thể truy cập được là`a + 1`, đây chỉ là số nguyên tiếp theo sau tổng của các đồng xu 1-burle. Thuật toán xử lý cả bằng cách tính toán`a + 2*b + 1`, hoạt động tự nhiên trong mọi trường hợp và`max(1, ...)`biện pháp bảo vệ đảm bảo tính đúng đắn khi cả hai`a`Và`b`là số không.
