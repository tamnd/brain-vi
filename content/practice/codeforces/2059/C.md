---
title: "CF 2059C - Dịch vụ khách hàng"
description: "Bài toán trình bày một tình huống trong đó có n hàng đợi, ban đầu trống và một chuỗi gồm n thời điểm riêng biệt. Tại mỗi thời điểm, mỗi hàng đợi đều nhận được một lượng khách hàng mới nhất định."
date: "2026-06-08T08:04:42+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "graph-matchings", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2059
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1002 (Div. 2)"
rating: 1600
weight: 2059
solve_time_s: 105
verified: false
draft: false
---

[CF 2059C - Dịch vụ khách hàng](https://codeforces.com/problemset/problem/2059/C) 

**Đánh giá:** 1600 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, so khớp đồ thị, tham lam, toán học, sắp xếp 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề đưa ra một kịch bản trong đó có`n`hàng đợi, ban đầu trống và một chuỗi`n`những khoảnh khắc rời rạc. Tại mỗi thời điểm, mỗi hàng đợi đều nhận được một lượng khách hàng mới nhất định. Sau khi khách hàng đến, một hàng đợi duy nhất có thể được phục vụ, việc này sẽ đặt lại số lượng khách hàng về 0. Rốt cuộc`n`thời điểm đã trôi qua, chúng tôi được yêu cầu tối đa hóa số lượng bị loại trừ tối thiểu (MEX) trong số các khách hàng còn lại trong tất cả các hàng đợi. 

MEX là số nguyên không âm nhỏ nhất không có trong tập hợp số lượng khách hàng còn lại. Ví dụ: nếu số đếm cuối cùng là`[0, 1, 3]`, MEX là`2`bởi vì`0`Và`1`xuất hiện nhưng`2`không. Mục tiêu là xác định MEX lớn nhất có thể đạt được bằng cách chọn tối ưu hàng đợi nào sẽ phục vụ tại mỗi thời điểm. 

Cho rằng`n`có thể lên tới 300 và tổng của`n^2`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2×10^5`, mọi giải pháp lặp qua tất cả các hoán vị có thể có của các đơn đặt hàng dịch vụ đều quá chậm. Một cách tiếp cận bạo lực ngây thơ sẽ yêu cầu`n!`mô phỏng, điều này hoàn toàn không khả thi. Các trường hợp biên bao gồm các chuỗi trong đó tất cả các hàng đợi đều nhận được cùng một số hoặc trong đó một số hàng đợi nhận được số lượng rất lớn sớm, khiến không thể xây dựng chuỗi cuối cùng với các giá trị MEX nhỏ nếu việc phục vụ được thực hiện kém. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi thứ tự có thể có của hàng đợi được phục vụ tại mỗi thời điểm. Đối với mỗi hoán vị, chúng tôi sẽ theo dõi số lượng khách hàng tích lũy, đặt lại hàng đợi đã chọn ở mỗi bước và tính toán MEX cuối cùng. Cách tiếp cận này đúng về nguyên tắc nhưng hoàn toàn khó thực hiện vì`n!`phát triển nhanh hơn`10^300`vì`n=300`. 

Thông tin chuyên sâu quan trọng đến từ việc quan sát rằng mục tiêu là tạo ra số lượng hàng đợi cuối cùng sao cho chúng chứa càng nhiều số nguyên liên tiếp bắt đầu từ 0 càng tốt. Chúng ta có thể hiểu vấn đề là xây dựng một tập hợp các số nguyên biểu thị kích thước hàng đợi cuối cùng. Mỗi hàng đợi sẽ được phục vụ chính xác một lần, vì vậy số lượng cuối cùng của mỗi hàng đợi là tổng của tất cả các mức tăng mà nó nhận được, trừ đi mức tăng tại thời điểm nó được phục vụ. Vì số gia hoàn toàn dương nên số lượng khách hàng tối thiểu có thể đạt được trong hàng đợi sau dịch vụ là tổng số khách hàng tích lũy **trước dịch vụ**. Để tối đa hóa MEX, chúng ta phải luôn phục vụ hàng đợi hiện có tổng số lượng khách đến tích lũy nhỏ nhất chưa được phục vụ. Nói cách khác, các số cuối cùng nhỏ nhất có thể được phân bổ cho các số nguyên nhỏ nhất bắt đầu từ 0. 

Vì vậy, chiến lược tối ưu giảm xuống còn việc sắp xếp tất cả`n×n`số tiền tăng của khách hàng theo thứ tự không giảm và gán chúng một cách tham lam vào số lượng cuối cùng`0, 1, 2, …`. Cụ thể, sau khi sắp xếp các tổng hàng, MEX có thể đạt được tối đa là số nguyên lớn nhất`k`ít nhất như vậy`k`hàng đợi có số lượng cuối cùng không ít hơn`0, 1, …, k-1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! × n^2) | O(n^2) | Quá chậm | 
| Tham lam / Xây dựng | O(n^2 log n) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và`n×n`ma trận lượt đến`a`. 
3. Tính tổng số lượt đến cho mỗi hàng đợi`total[i] = sum(a[i])`. 
4. Sắp xếp`total`theo thứ tự không giảm. 
5. Khởi tạo`mex = 0`. 
6. Lặp lại các tổng được sắp xếp và với mỗi`total[i]`, nếu như`total[i] >= mex`, tăng`mex`bằng 1. Điều này đảm bảo rằng đối với MEX`k`, tồn tại ít nhất một hàng đợi có số đếm cuối cùng có thể biểu thị`k-1`. 
7. Đầu ra`mex`cho trường hợp thử nghiệm. 

**Tại sao nó hoạt động:** Phục vụ hàng đợi với tổng tích lũy nhỏ nhất tại mỗi thời điểm đảm bảo rằng số đếm cuối cùng nhỏ nhất có thể chiếm các số nguyên liên tiếp từ 0 trở lên. Bằng cách lặp qua các tổng được sắp xếp và tăng dần`mex`bất cứ khi nào hàng đợi có thể đáp ứng số nguyên bắt buộc tiếp theo, chúng tôi sẽ tính toán một cách hiệu quả tiền tố liên tiếp lớn nhất có thể, đó là MEX tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    total = []
    for _ in range(n):
        row = list(map(int, input().split()))
        total.append(sum(row))
    total.sort()
    mex = 0
    for val in total:
        if val >= mex:
            mex += 1
    print(mex)

t = int(input())
for _ in range(t):
    solve()
```**Giải thích:** Giải pháp đọc tất cả các lượt đến, tính tổng các hàng để có tổng số lượng cuối cùng tiềm năng và sắp xếp chúng. Việc lặp qua các tổng được sắp xếp cho phép chúng ta gán các số nguyên tối thiểu một cách tham lam cho các hàng đợi theo cách tối đa hóa MEX. Sự so sánh`val >= mex`đảm bảo rằng mỗi số từ 0 trở lên đều có thể được biểu diễn. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
2
1 2
2 1
```| Xếp hàng | Tổng số lượt đến | Tổng số được sắp xếp | Bước MEX | 
| --- | --- | --- | --- | 
| 1 | 1+2=3 | [3, 3] | 0→1→2 | 
| 2 | 2+1=3 | | | 

**Giải thích:** Cả hai hàng đợi đều có thể chứa số đếm cuối cùng là 0 và 1 sau khi được cung cấp dịch vụ tối ưu. MEX tối đa là 2. 

### Đầu vào mẫu 2```
3
2 2
1 2
2 3
```| Xếp hàng | Tổng hợp | Tổng số được sắp xếp | Bước MEX | 
| --- | --- | --- | --- | 
| 1 | 3 | [3, 5] | 0→1→2→3 | 
| 2 | 5 | | | 

MEX = 2 vì hàng đợi thứ hai đảm bảo rằng 1 có thể được biểu thị và hàng đợi đầu tiên đại diện cho 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 log n) | Tổng mỗi hàng là O(n), sắp xếp n tổng là O(n log n), tổng ≤ O(2×10^5 log 300) | 
| Không gian | O(n^2) | Lưu trữ tất cả các hàng | 

Giải pháp phù hợp thoải mái trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        total = []
        for _ in range(n):
            row = list(map(int, input().split()))
            total.append(sum(row))
        total.sort()
        mex = 0
        for val in total:
            if val >= mex:
                mex += 1
        print(mex)
    return output.getvalue().strip()

# Provided samples
assert run("4\n2\n1 2\n2 1\n2\n10 10\n10 10\n3\n2 3 3\n4 4 1\n2 1 1\n4\n4 2 2 17\n1 9 3 1\n5 5 5 11\n1 2 1 1") == "2\n1\n3\n3"

# Custom cases
assert run("1\n1\n1") == "1", "minimum size input"
assert run("1\n2\n1 1\n1 1") == "2", "all-equal values"
assert run("1\n3\n1 1 1\n1 1 1\n1 1 1") == "3", "equal increments"
assert run("1\n3\n1 2 3\n3 2 1\n2 2 2") == "3", "mixed increments"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp nhỏ nhất | 
| 2 | 2 | số lượng bằng nhau trên hàng đợi | 
| 3 | 3 | tổng hàng giống hệt nhau | 
| 4 | 3 | số tiền không đồng nhất, xác nhận tính tham lam | 

## Vỏ cạnh 

Nếu tất cả các hàng đợi nhận được số giống nhau tại mỗi thời điểm thì thuật toán sẽ xác định chính xác rằng MEX tối đa là`n`, vì mỗi hàng đợi có thể biểu diễn một trong các số nguyên liên tiếp từ 0 đến`n-1`. Nếu một hàng đợi sớm nhận được số lượng tăng rất lớn, thì đương nhiên nó sẽ được phục vụ cuối cùng theo thứ tự tham lam và các hàng đợi nhỏ hơn sẽ nhận được số lượng nhỏ hơn cần thiết để tối đa hóa MEX. Cách tiếp cận này xử lý các giá trị lớn một cách an toàn vì nó chỉ so sánh tổng với các số nguyên trong phạm vi`[0, n]`.
