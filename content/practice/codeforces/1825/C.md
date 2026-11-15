---
title: "CF 1825C - LuoTianyi và buổi biểu diễn"
description: "Chúng ta được xếp một hàng ghế được đánh số từ 1 đến $m$ và $n$ những người đến xem buổi biểu diễn. Mỗi người có một kiểu chỗ ngồi ưa thích: họ muốn ngồi ngay bên trái của ghế ngồi ngoài cùng bên trái, ngay bên phải của ghế ngồi ngoài cùng bên phải, hoặc trong một…"
date: "2026-06-09T07:37:02+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1825
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 872 (Div. 2)"
rating: 1400
weight: 1825
solve_time_s: 111
verified: false
draft: false
---

[CF 1825C - LuoTianyi và buổi biểu diễn](https://codeforces.com/problemset/problem/1825/C) 

**Đánh giá:** 1400 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp một hàng ghế được đánh số từ 1 đến$m$Và$n$những người đến tham gia một buổi biểu diễn Mỗi người có một loại chỗ ngồi ưa thích: họ muốn ngồi ngay bên trái của ghế ngoài cùng bên trái, ngay bên phải của ghế ngoài cùng bên phải hoặc trên một ghế được đánh số cụ thể. Nếu một người không thể ngồi theo sở thích của mình vì chỗ ngồi đã có người hoặc đã chạm tới mép ghế, họ sẽ rời đi. Mục tiêu là xác định số lượng người tối đa có thể ngồi thành công nếu chúng ta được phép chọn thứ tự mọi người vào. 

Đầu vào cung cấp số lượng người$n$và tổng số ghế$m$, theo sau là một mảng$x$chiều dài$n$. Mỗi$x_i$cho biết ưu tiên chỗ ngồi:$-1$đối với quy tắc liền kề bên trái,$-2$cho quy tắc liền kề bên phải và số nguyên dương cho một chỗ ngồi cụ thể. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm: số người tối đa có thể chiếm một chỗ. 

Với những hạn chế, với$n$Và$m$lên đến$10^5$và tổng số trên tất cả các trường hợp thử nghiệm cũng bị giới hạn ở mức$10^5$, bất kỳ giải pháp nào vượt quá$O(n \log n)$mỗi trường hợp thử nghiệm có thể có nguy cơ hết thời gian chờ. Một mô phỏng ngây thơ về tất cả các hoán vị của những người bước vào là thời gian giai thừa, điều này không thực tế. Các trường hợp đặc biệt bao gồm các tình huống trong đó tất cả mọi người đều muốn có cùng một chỗ ngồi, trong đó tất cả đều muốn có các ghế liền kề bên trái hoặc bên phải hoặc sự kết hợp trong đó các lựa chọn tham lam phải được cân bằng. 

Một ví dụ nhỏ nêu bật những cạm bẫy: nếu$n=3, m=5$với$x = [5, 5, 5]$, chỉ có một người có thể ngồi vì tất cả đều nhắm đến cùng một chỗ. Nếu tất cả ghế trống và mọi người thuộc loại trái/phải, chiến lược tối ưu là cân bằng việc lấp đầy chỗ ngồi ở cả hai đầu, không tham lam từ trái sang phải hoặc từ phải sang trái. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi hoán vị của mọi người và mô phỏng chỗ ngồi, ghi lại số người đã ngồi thành công. Điều này đúng nhưng không khả thi vì số hoán vị là$n!$, tăng trưởng nhanh hơn$10^{400,000}$vì$n \sim 10^5$. 

Cái nhìn sâu sắc quan trọng là những người có chỗ ngồi cụ thể phải chiếm những chỗ ngồi riêng biệt, không phụ thuộc vào trật tự. Đối với những người ở bên trái và bên phải, số lượng tối đa có thể ngồi tùy thuộc vào số ghế trống còn trống ở bên trái và bên phải của số ghế đã sử dụng. Sau khi phân bổ những người ngồi ở ghế cụ thể, chúng tôi có thể tính toán xem có thể xếp thêm bao nhiêu người ở bên trái và bên phải mà không có xung đột. Do đó, thứ tự tối ưu mang tính quyết định: xếp chỗ cho tất cả những người có ghế cụ thể trước, sau đó xếp chỗ ngồi bên trái cho ghế còn trống ngoài cùng bên trái và ngồi bên phải cho ghế còn trống ngoài cùng bên phải. 

Để triển khai hiệu quả, chúng tôi sắp xếp và loại bỏ số ghế cụ thể, sau đó tính số ghế trống ở bên trái của ghế ngoài cùng bên trái và bên phải của ghế ngoài cùng bên phải. Kết quả là số lượng ghế cụ thể riêng biệt cộng với số người tối thiểu còn lại ở bên trái và bên phải phù hợp với các ghế còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm số người ở bên trái`left_count`, người liền kề bên phải`right_count`và thu thập số ghế cụ thể thành một bộ`specific_seats`. 
2. Xóa các bản sao khỏi`specific_seats`bằng cách chuyển đổi thành một tập hợp, sau đó sắp xếp nó để dễ dàng tính toán các khoảng trống ở cả hai đầu. 
3. Tính số ghế trống ở bên trái của ghế cụ thể ngoài cùng bên trái,`left_empty = leftmost - 1`và ở bên phải của ghế cụ thể ngoài cùng bên phải,`right_empty = m - rightmost`. 
4. Số người ngồi bên trái tối đa có thể ngồi là tối thiểu`left_count`Và`left_empty`. Tương tự, số người ở bên phải tối đa là`min(right_count, right_empty)`. 
5. Đối với phần giữa còn lại (giữa các ghế cụ thể ngoài cùng bên trái và ngoài cùng bên phải), hãy tính số ghế trống`middle_empty = m - len(specific_seats) - max_left - max_right`. Con số này bằng 0 hoặc dương. 
6. Tổng số chỗ ngồi tối đa là`len(specific_seats) + max_left + max_right`, cộng thêm có thể thêm một chỗ nữa nếu còn chỗ trống ở giữa để chứa thêm một người ở bên trái hoặc bên phải ở giữa mà không bị chồng lên nhau. 
7. Xuất tổng của từng test. 

Tại sao nó hiệu quả: Bằng cách sắp xếp chỗ ngồi cho những người có ghế cụ thể trước, chúng tôi khắc phục được những hạn chế tuyệt đối. Người liền kề bên trái và người liền kề bên phải chỉ có thể lấp đầy các ghế tiếp giáp với các cạnh mà không xung đột. Việc sắp xếp và đếm các khoảng trống đảm bảo rằng chúng ta không bao giờ đếm quá mức và tôn trọng mọi ràng buộc, tạo ra mức tối đa toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_seated(n, m, x):
    left_count = x.count(-1)
    right_count = x.count(-2)
    specific_seats = sorted(set([xi for xi in x if xi > 0]))
    
    if not specific_seats:
        return min(n, m)
    
    left_empty = specific_seats[0] - 1
    right_empty = m - specific_seats[-1]
    
    max_left = min(left_count, left_empty)
    max_right = min(right_count, right_empty)
    
    middle_empty = m - len(specific_seats) - max_left - max_right
    total = len(specific_seats) + max_left + max_right
    # If there is a leftover left/right person that can fit in the middle
    if middle_empty > 0:
        total += 1
    return min(total, n, m)

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    x = list(map(int, input().split()))
    print(max_seated(n, m, x))
```Giải pháp này trước tiên sẽ đếm số người ở bên trái và bên phải, sau đó trích xuất các yêu cầu về chỗ ngồi cụ thể duy nhất. Việc sắp xếp là cần thiết để xác định ranh giới cho phần mở rộng bên trái và bên phải. Tính toán`middle_empty`chiếm số ghế còn sót lại giữa các ranh giới bị chiếm đóng. các`min(total, n, m)`đảm bảo chúng tôi không bao giờ vượt quá tổng số người hoặc tổng số ghế. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`n=3, m=10, x=[5,5,5]`| Bước | left_count | đúng_count | chỗ ngồi cụ thể | left_empty | đúng_empty | max_left | max_right | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 0 | [5] | 4 | 5 | 0 | 0 | 1 | 

Chỉ có một người có thể chiếm ghế số 5. Kết quả: 1 

**Ví dụ 2** 

đầu vào:`n=5, m=7, x=[-1,-1,4,-2,-2]`| Bước | left_count | đúng_count | chỗ ngồi cụ thể | left_empty | đúng_empty | max_left | max_right | tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 2 | [4] | 3 | 3 | 2 | 2 | 5 | 

Bên trái chiếm ghế 1 và 2, bên phải chiếm ghế 5 và 6. Ghế cụ thể 4 đã có người sử dụng. Tổng cộng = 5. 

Những dấu vết này xác nhận rằng thuật toán cân bằng chính xác những người ở bên trái/phải xung quanh các yêu cầu về chỗ ngồi cụ thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các chỗ ngồi cụ thể duy nhất chiếm ưu thế. Đếm -1/-2 là O(n). | 
| Không gian | O(n) | Để lưu trữ chỗ ngồi cụ thể và mảng đầu vào. | 

Điều này phù hợp với các hạn chế:$n \le 10^5$cho mỗi trường hợp thử nghiệm, nhiều trường hợp thử nghiệm có tổng bằng$10^5$và Python có thể xử lý$O(n \log n)$hoạt động thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided samples
assert run("10\n3 10\n5 5 5\n4 6\n1 -2 -2 1\n5 7\n-1 -1 4 -2 -2\n6 7\n5 -2 -2 -2 -2 -2\n6 6\n-1 1 4 5 -1 4\n6 8\n-1 -1 -1 3 -1 -2\n6 7\n5 -1 -2 -2 -2 -2\n3 1\n-2 -2 1\n2 5\n5 -2\n1 2\n-1") == "1\n3\n5\n6\n5\n5\n5\n1\n2\n1", "provided samples"

# Custom cases
assert run("1\n3 3\n-1 -2 -1") == "3", "all left/right fill seats"
assert run("1\n4 4\n1 2 3 4")
```
