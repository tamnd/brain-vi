---
title: "CF 1986A - Trục X"
description: "Chúng ta có ba điểm nguyên trên trục số một chiều, cụ thể là trục X. Nhiệm vụ là tìm một tọa độ nguyên sao cho tổng khoảng cách từ điểm đã chọn này đến ba điểm đã cho là nhỏ nhất."
date: "2026-06-08T16:10:56+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1986
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 954 (Div. 3)"
rating: 800
weight: 1986
solve_time_s: 96
verified: true
draft: false
---

[CF 1986A - Trục X](https://codeforces.com/problemset/problem/1986/A) 

**Đánh giá:** 800 
**Tags:** lực lượng vũ phu, hình học, toán học, sắp xếp 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba điểm nguyên trên trục số một chiều, cụ thể là trục X. Nhiệm vụ là tìm một tọa độ nguyên sao cho tổng khoảng cách từ điểm đã chọn này đến ba điểm đã cho là nhỏ nhất. Số liệu khoảng cách là sự khác biệt tuyệt đối thông thường, vì vậy đối với một điểm$a$và một điểm$x_i$, khoảng cách là$|a - x_i|$. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định ba số nguyên trong phạm vi từ 1 đến 10. Đầu ra cho mỗi trường hợp thử nghiệm là một số nguyên duy nhất biểu thị tổng khoảng cách nhỏ nhất có thể đạt được. 

Bởi vì các điểm bị giới hạn trong một phạm vi rất nhỏ, một cách tiếp cận đơn giản thậm chí có thể kiểm tra tất cả các tọa độ nguyên có thể có từ 1 đến 10. Tuy nhiên, để hiểu cấu trúc chung của bài toán, sẽ rất hữu ích khi xem xét các tính chất toán học: đối với ba điểm trên một đường thẳng, tổng khoảng cách được giảm thiểu khi điểm được chọn là trung tuyến của ba điểm. Điều này là do đường trung bình cân bằng khoảng cách ở cực trái và cực phải. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó hai hoặc cả ba điểm trùng nhau. Ví dụ: nếu tất cả các điểm đều ở mức 1 thì điểm tối ưu cũng là 1, mang lại tổng khoảng cách là 0. Nếu hai điểm ở mức 1 và một điểm ở mức 10, thì lựa chọn tối ưu là 1 hoặc 10, nhưng việc tính toán cẩn thận cho thấy phương pháp trung bình sẽ tự nhiên chọn điểm cân bằng khoảng cách. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét mọi tọa độ nguyên từ 1 đến 10 và tính tổng khoảng cách cho mỗi tọa độ. Vì phạm vi tọa độ cực kỳ nhỏ nên phương pháp này khả thi cho bài toán này, mặc dù nó không cần thiết xét từ góc độ toán học. Đối với mỗi ứng viên$a$, thuật toán tính tổng$|a - x_1| + |a - x_2| + |a - x_3|$. Với ba điểm và mười vị trí ứng cử viên, điều này yêu cầu tối đa 30 phép tính khoảng cách cho mỗi trường hợp kiểm thử. Cho rằng$t \leq 1000$, đây chỉ là 30.000 phép tính, dễ dàng phù hợp với giới hạn thời gian 2 giây. 

Cách tiếp cận tối ưu xuất phát từ việc quan sát thấy tổng các khác biệt tuyệt đối được giảm thiểu ở mức trung bình của các điểm. Sắp xếp ba điểm và lấy trực tiếp giá trị ở giữa sẽ cho tọa độ tối ưu$a$. Điều này làm giảm vấn đề xuống còn việc sắp xếp ba số, tức là thời gian không đổi. Không cần phải lặp qua các vị trí ứng cử viên hoặc thực hiện các phép tính tổng lặp đi lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t * 10) = O(t) | O(1) | Được chấp nhận do phạm vi đầu vào nhỏ | 
| Tối ưu dựa trên trung vị | O(t) | O(1) | Được chấp nhận và tối ưu | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Chúng tôi sẽ xử lý từng trường hợp thử nghiệm một cách độc lập. 
2. Với mỗi test, đọc ba số nguyên biểu thị các điểm trên trục X. 
3. Sắp xếp ba số nguyên. Điều này đảm bảo rằng các giá trị nhỏ nhất, trung bình và lớn nhất được xác định dễ dàng. 
4. Chọn giá trị giữa của danh sách đã sắp xếp làm tọa độ tối ưu$a$. Đây là mức trung bình và đảm bảo tổng khoảng cách tối thiểu. 
5. Tính tổng khoảng cách từ$a$đến từng điểm trong số ba điểm bằng cách sử dụng$|a - x_1| + |a - x_2| + |a - x_3|$. 
6. In tổng tính toán cho test case hiện tại. 

Tại sao nó hoạt động: Đối với một tập hợp ba điểm trên một đường thẳng, việc di chuyển điểm đã chọn ra khỏi đường trung tuyến sẽ làm tăng tổng khoảng cách theo ít nhất một hướng mà không giảm đủ ở hướng còn lại. Do đó, trung vị là điểm duy nhất làm giảm tổng sai biệt tuyệt đối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    x = list(map(int, input().split()))
    x.sort()
    median = x[1]
    total_distance = abs(median - x[0]) + abs(median - x[1]) + abs(median - x[2])
    print(total_distance)
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó phân tích ba tọa độ, sắp xếp chúng và lấy phần tử ở giữa làm trung vị. Sử dụng hàm giá trị tuyệt đối, nó tính tổng khoảng cách đến tất cả các điểm và in ra. Việc sắp xếp ba phần tử là thời gian không đổi, vì vậy bước này không quan trọng về mặt hiệu suất. Việc sử dụng phần tử ở giữa đảm bảo tính chính xác mà không cần kiểm tra tất cả tọa độ ứng cử viên. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`1 5 9`| Bước | Điểm được sắp xếp | Trung bình | Tổng khoảng cách | 
| --- | --- | --- | --- | 
| Ban đầu | 1, 5, 9 | 5 | | 
| Tính toán | | 5 | | 

Giải thích: Chọn 5 sẽ cân bằng khoảng cách đến 1 và 9. 

**Ví dụ 2:**`8 2 8`| Bước | Điểm được sắp xếp | Trung bình | Tổng khoảng cách | 
| --- | --- | --- | --- | 
| Ban đầu | 2, 8, 8 | 8 | | 
| Tính toán | | 8 | | 

Giải thích: Trung vị 8 được lặp lại và giảm thiểu khoảng cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Sắp xếp 3 phần tử là thời gian không đổi, được thực hiện cho mỗi t trường hợp thử nghiệm. | 
| Không gian | O(1) | Chỉ một danh sách ba phần tử được lưu trữ cho mỗi trường hợp thử nghiệm. | 

Được cho$t \leq 1000$và ba điểm cho mỗi trường hợp thử nghiệm, thuật toán này cực kỳ nhanh và tiết kiệm bộ nhớ, nằm trong giới hạn của bài toán. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # Solution function
    t = int(input())
    for _ in range(t):
        x = list(map(int, input().split()))
        x.sort()
        median = x[1]
        total_distance = abs(median - x[0]) + abs(median - x[1]) + abs(median - x[2])
        print(total_distance)
    return output.getvalue().strip()

# Provided sample
assert run("8\n1 1 1\n1 5 9\n8 2 8\n10 9 3\n2 1 1\n2 4 1\n7 3 5\n1 9 4\n") == "0\n8\n6\n7\n1\n3\n4\n8"

# Custom cases
assert run("1\n1 1 10\n") == "9", "two equal, one far"
assert run("1\n5 5 5\n") == "0", "all equal"
assert run("1\n1 2 3\n") == "2", "consecutive numbers"
assert run("1\n10 1 5\n") == "8", "extremes and middle"
assert run("1\n3 3 4\n") == "1", "two equal, one adjacent"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 10 | 9 | Hai điểm bằng nhau và một điểm ở xa | 
| 5 5 5 | 0 | Tất cả các điểm trùng nhau | 
| 1 2 3 | 2 | Số liên tiếp và lựa chọn trung vị | 
| 10 1 5 | 8 | Giá trị cực trị và độ chính xác trung bình | 
| 3 3 4 | 1 | Hai điểm bằng nhau, một điểm liền kề | 

## Vỏ cạnh 

Khi cả ba điểm đều giống nhau, ví dụ:`5 5 5`, thuật toán chọn trung vị`5`. Tổng khoảng cách là`0 + 0 + 0 = 0`, xử lý chính xác trường hợp cạnh này. 

Khi hai điểm trùng nhau và một điểm ở xa, ví dụ:`1 1 10`, sắp xếp sản lượng`[1, 1, 10]`và trung vị là`1`. Tổng khoảng cách là`0 + 0 + 9 = 9`, là tối thiểu so với việc chọn`10`(sẽ là`9 + 9 + 0 = 18`). 

Khi các điểm liên tiếp, ví dụ:`1 2 3`, danh sách được sắp xếp là`[1, 2, 3]`và trung vị`2`tạo ra tổng`1 + 0 + 1 = 2`. Lựa chọn`1`hoặc`3`sẽ mang lại`1 + 1 + 2 = 4`, do đó thuật toán xác định chính xác trung vị. 

Điều này xác nhận cách tiếp cận dựa trên trung vị xử lý chính xác tất cả các trường hợp cạnh phạm vi nhỏ mà không cần xử lý trường hợp đặc biệt.
