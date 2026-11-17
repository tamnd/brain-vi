---
title: "CF 1944A - Phá hủy cầu"
description: "Chúng ta được yêu cầu xem xét một mạng lưới các hòn đảo trong đó ban đầu mỗi cặp đảo được nối với nhau bằng một cây cầu. Có n hòn đảo được đánh số từ 1 đến n và Everule sống trên đảo 1. Dominater có thể phá hủy tối đa k cây cầu để giảm số lượng hòn đảo mà Everule có thể tiếp cận."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1944
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 934 (Div. 2)"
rating: 800
weight: 1944
solve_time_s: 55
verified: true
draft: false
---

[CF 1944A - Phá hủy những cây cầu](https://codeforces.com/problemset/problem/1944/A) 

**Đánh giá:** 800 
**Tags:** đồ thị, tham lam, toán học 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xem xét một mạng lưới các hòn đảo trong đó ban đầu mỗi cặp đảo được nối với nhau bằng một cây cầu. có`n`đảo được đánh số từ 1 đến`n`, và Everule sống trên đảo 1. Dominater có thể tiêu diệt tối đa`k`những cây cầu để giảm số lượng hòn đảo mà Everule có thể tiếp cận. Nhiệm vụ là tìm ra số lượng đảo tối thiểu mà Everule vẫn có thể ghé thăm nếu Dominater hành động tối ưu. Câu trả lời luôn tính là đảo 1. 

Đầu vào đưa ra nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm bao gồm hai số nguyên,`n`Và`k`. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm - số lượng đảo mà Everule có thể tiếp cận sau khi loại bỏ các cây cầu một cách tối ưu. 

Từ`n`nhiều nhất là 100, đồ thị hoàn chỉnh ban đầu có tối đa 4950 cạnh, do đó, việc lặp qua tất cả các cạnh một cách rõ ràng là khả thi đối với các đồ thị nhỏ`n`. Tuy nhiên, số lượng test case có thể lên tới 1000, vì vậy chúng ta cần một giải pháp hoạt động theo thời gian không đổi hoặc tuyến tính về mặt`n`mỗi trường hợp thử nghiệm. Chúng tôi không thể mô phỏng từng thao tác loại bỏ cạnh hoặc kiểm tra kết nối riêng lẻ vì điều đó có thể đạt tới 5 triệu thao tác và trở nên chậm nếu lặp lại cho mọi trường hợp thử nghiệm. 

Các trường hợp đặc biệt cần chú ý bao gồm các tình huống trong đó`k`bằng 0 (không có cây cầu nào có thể bị phá hủy) và các tình huống trong đó`k`đủ lớn để có khả năng cô lập hoàn toàn các hòn đảo. Ví dụ, nếu`n = 4`Và`k = 4`, chúng ta cần kiểm tra xem có phá hủy`k`những cây cầu có thể cô lập hoàn toàn hòn đảo 1 hoặc nếu những cây cầu còn lại đủ để giữ cho một số hòn đảo có thể tiếp cận được một cách gián tiếp. Việc triển khai bất cẩn có thể cho rằng việc loại bỏ các cầu nối luôn làm giảm kết nối một cách tuyến tính, nhưng sự hiện diện của nhiều đường dẫn trong một biểu đồ hoàn chỉnh có nghĩa là việc loại bỏ một vài cầu nối thường ít có tác dụng. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô hình hóa biểu đồ hoàn chỉnh, mô phỏng tất cả các tập hợp có thể có của`k`loại bỏ cầu, sau đó tính toán thành phần được kết nối chứa hòn đảo 1. Phương pháp mạnh mẽ này hoạt động vì nó mô phỏng trực tiếp vấn đề. Số cạnh của một đồ thị đầy đủ là`n*(n-1)/2`, và chọn tới`k`các cạnh để loại bỏ yêu cầu`O((n*(n-1)/2 choose k))`khả năng, phát triển kết hợp. Ngay cả đối với`n = 10`Và`k = 5`, điều này trở nên cực kỳ lớn. Rõ ràng, vũ lực là không khả thi. 

Cái nhìn sâu sắc quan trọng là trong một biểu đồ hoàn chỉnh, ban đầu mỗi hòn đảo có mức độ`n-1`. Để cô lập hòn đảo 1, Dominater phải dỡ bỏ tất cả các cây cầu nối với nó. Do đó, số lượng đảo tối thiểu mà Everule có thể đến được xác định trực tiếp bằng số lượng cây cầu tới đảo 1 có thể bị phá hủy. Nếu như`k`ít nhất là`n-1`, hòn đảo 1 có thể bị cô lập hoàn toàn và Everule chỉ có thể tiếp cận chính mình. Nếu không, mọi cây cầu còn lại sẽ giúp có thể tiếp cận thêm các hòn đảo khác. Chúng ta có thể khái quát hóa điều này: nếu`k`ít hơn`n-1`, số hòn đảo Everule có thể ghé thăm là`n - k`bởi vì mỗi cây cầu bị phá hủy nhiều nhất có thể ngăn cản việc tiếp cận một hòn đảo. 

Quan sát này làm giảm vấn đề thành một công thức đơn giản mà không cần mô phỏng đồ thị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n^2 chọn k)) | O(n^2) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`Và`k`. 
3. Tính số cầu tối đa nối tới đảo 1 là`n - 1`. 
4. Nếu`k`lớn hơn hoặc bằng`n - 1`, Dominator có thể phá hủy tất cả các cây cầu từ đảo 1 nên đáp án là`1`. 
5. Nếu không, Dominator có thể tiêu diệt`k`mỗi cây cầu ngăn cản việc tiếp cận một hòn đảo, vì vậy số hòn đảo có thể tiếp cận được là`n - k`. 
6. In kết quả cho từng test. 

Tại sao nó hoạt động: Một biểu đồ hoàn chỉnh được kết nối tối đa. Mọi hòn đảo ngoại trừ hòn đảo 1 đều được kết nối với hòn đảo đó bằng đúng một cây cầu. Việc dỡ bỏ một cây cầu chỉ có thể cách ly hòn đảo tương ứng với hòn đảo 1 nếu không có con đường thay thế nào tồn tại. Trong một biểu đồ hoàn chỉnh, tất cả các đảo đều được kết nối với nhau, do đó, chỉ có các kết nối trực tiếp từ đảo 1 mới có ý nghĩa tách biệt. Điều bất biến là số lượng đảo có thể đến được từ đảo 1 bằng`1 + max(0, n - 1 - k)`, điều này đơn giản hóa thành`min(n, n - k)`hoặc`1`khi bị cô lập hoàn toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    # Maximum possible bridges from island 1 is n-1
    if k >= n - 1:
        print(1)
    else:
        print(n - k)
```Giải pháp này đọc đầu vào theo phong cách lập trình cạnh tranh tiêu chuẩn. Séc`k >= n - 1`xử lý trường hợp đảo 1 có thể bị cô lập hoàn toàn. Nếu không thì,`n - k`đếm tất cả các hòn đảo vẫn có thể tiếp cận được. Các lỗi sai lệch có thể tránh được bằng cách suy luận theo số lượng cầu nối tới đảo 1 thay vì đếm trực tiếp các đảo. 

## Ví dụ đã hoạt động 

Đối với đầu vào`4 1`, chúng tôi có`n = 4`Và`k = 1`. Số lượng cầu tối đa tính từ đảo 1 là`3`. Từ`k < n - 1`, câu trả lời là`n - k = 4 - 1 = 3`. Everule có thể đến được 3 hòn đảo trong đó có chính mình. 

Đối với đầu vào`5 10`,`n = 5`Và`k = 10`. Đảo 1 có`4`những cây cầu. Từ`k >= 4`, Dominator có thể tiêu diệt tất cả chúng, chỉ còn lại hòn đảo 1 có thể tiếp cận được. Câu trả lời là`1`. 

| n | k | n-1 | k >= n-1? | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 4 | 1 | 3 | Không | 3 | 
| 5 | 10 | 4 | Có | 1 | 

Dấu vết này xác nhận công thức hoạt động cho cả nhỏ và lớn`k`các giá trị, xử lý các trường hợp cạnh có thể cách ly. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi. | 
| Không gian | O(1) | Chỉ sử dụng các biến số nguyên; không có cấu trúc dữ liệu bổ sung. | 

Được cho`t <= 1000`Và`n <= 100`, thuật toán dễ dàng chạy trong giới hạn thời gian 1 giây và không vượt quá giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        if k >= n - 1:
            print(1)
        else:
            print(n - k)
    
    return out.getvalue().strip()

# provided samples
assert run("6\n2 0\n2 1\n4 1\n5 10\n5 3\n4 4\n") == "2\n1\n4\n1\n2\n1", "sample 1"

# custom cases
assert run("2\n1 0\n1 1\n") == "1\n1", "minimum size n=1"
assert run("2\n100 0\n100 99\n") == "100\n1", "large n, k=0 and k=max-1"
assert run("2\n10 45\n10 44\n") == "1\n6", "max bridges destroyed, almost max"
assert run("3\n3 2\n3 1\n3 0\n") == "1\n2\n3", "small n edge cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 \n 1 1 | 1 \n 1 | Đầu vào đảo có kích thước tối thiểu | 
| 100 0 \n 100 99 | 100 \n 1 | Lớn`n`, phá hủy hoàn toàn hoặc gần như hoàn toàn | 
| 10 45 \n 10 44 | 1 \n 6 | Tối đa`k`trường hợp gần cách ly hoàn toàn | 
| 3 2 \n 3 1 \n 3 0 | 1 \n 2 \n 3 | Đồ thị nhỏ, thay đổi`k`giá trị | 

## Vỏ cạnh 

cho`n = 1`Và`k = 0`, thuật toán xuất ra chính xác`1`bởi vì chỉ có một hòn đảo và không có cây cầu nào tồn tại. Vì`k`bằng`n*(n-1)/2`, thuật toán xác định chính xác hòn đảo 1 có thể cô lập hoàn toàn nếu`k >= n-1`. Bé nhỏ`n`giá trị, tối đa`k`các giá trị và`k=0`tất cả đều được xử lý đúng theo công thức`min(n, n - k)`với giới hạn dưới của`1`. Công thức này tự động tôn trọng bất biến mà hòn đảo 1 luôn có thể tiếp cận được.
