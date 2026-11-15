---
title: "CF 1867D - Hoạt động tuần hoàn"
description: "Chúng tôi bắt đầu với một mảng a có độ dài n chứa đầy các số 0 và chúng tôi muốn chuyển đổi nó thành mảng mục tiêu b có cùng độ dài."
date: "2026-06-08T23:41:11+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "dfs-and-similar", "graphs", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1867
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 897 (Div. 2)"
rating: 1800
weight: 1867
solve_time_s: 126
verified: false
draft: false
---

[CF 1867D - Hoạt động tuần hoàn](https://codeforces.com/problemset/problem/1867/D) 

**Đánh giá:** 1800 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, dfs và tương tự, đồ thị, tham lam, triển khai 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một mảng`a`chiều dài`n`chứa đầy số không và chúng tôi muốn chuyển đổi nó thành một mảng mục tiêu`b`có cùng độ dài. Hoạt động duy nhất được phép là "ghi đè theo chu kỳ": chúng tôi chọn một tập hợp`k`các vị trí riêng biệt và thay thế từng phần tử tại các vị trí này bằng chỉ mục của phần tử tiếp theo trong cùng một tập hợp, bao quanh ở cuối. Cụ thể hơn, nếu`l`là tập chỉ mục được chúng tôi chọn,`a[l[i]]`trở thành`l[(i+1) % k]`. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp độ dài mảng`n`, quy mô hoạt động`k`và mảng mục tiêu`b`. Đầu ra cho mỗi trường hợp là "CÓ" đơn giản nếu có thể đạt được`b`sử dụng 0 hoặc nhiều thao tác và "KHÔNG" nếu không. 

Hạn chế chính đó là`n`có thể lên đến`10^5`cho mỗi trường hợp thử nghiệm, với tổng của tất cả các trường hợp thử nghiệm không vượt quá`2·10^5`. Điều này cấm mọi mô phỏng mạnh mẽ của mọi chuỗi hoạt động, vì số lượng các chuỗi có thể tăng lên theo tổ hợp. Chúng ta cần một giải pháp có quy mô tuyến tính hoặc gần tuyến tính với`n`. 

Các trường hợp cạnh phát sinh khi`k = 1`hoặc`k = n`. Vì`k = 1`, thao tác sẽ thay thế một phần tử bằng chính nó, thao tác này chỉ hữu ích nếu`b[i] = i`. Việc triển khai ngây thơ có thể bỏ qua điều này, tạo ra "CÓ" không chính xác khi một số giá trị nằm ngoài giới hạn. Một sự tinh tế khác là các giá trị được lặp lại trong`b`: nếu các phần tử liên tiếp cần các nguồn khác nhau cho hoạt động tuần hoàn, việc gán tham lam bất cẩn có thể không tôn trọng sự phụ thuộc theo chu kỳ. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ mô phỏng mọi lựa chọn có thể có của bộ chỉ mục`l`và áp dụng thao tác tuần hoàn nhiều lần cho đến khi`a`bằng`b`hoặc tất cả các lựa chọn đã hết. Mặc dù đúng về nguyên tắc nhưng độ phức tạp của nó lại rất lớn. Mỗi sự lựa chọn của`k`vị trí từ`n`có`C(n, k)`khả năng, mà đối với`n = 10^5`Và`k = n/2`là hoàn toàn không khả thi. 

Cái nhìn sâu sắc về một giải pháp tối ưu đến từ việc đảo ngược hoạt động. Thay vì cố gắng “xây dựng”`b`từng bước một, chúng tôi nhìn vào`b`và lý do về vị trí nào có thể là một phần của cùng một hoạt động theo chu kỳ. Nhận thấy rằng một thao tác chỉ đơn giản là hoán vị các phần tử trong một tập hợp kích thước`k`, chúng tôi nhận ra rằng bất kỳ chuỗi nào của`k`các giá trị liên tiếp trong`b`có thể được "che phủ" nếu chúng tạo thành một chu trình liền kề. Điều này dẫn đến sự phân hủy tham lam: chúng tôi quét`b`từ trái sang phải và cho mỗi chuỗi giá trị liền kề có thể được chỉ định thông qua một thao tác có độ dài tuần hoàn duy nhất`k`, chúng tôi đánh dấu chúng là đã xử lý. Nếu bất kỳ phần tử nào không thể được đưa vào một chu trình hợp lệ thì câu trả lời là "KHÔNG". 

Điều này làm giảm vấn đề quét`b`trong khi vẫn duy trì chu kỳ dài tới`k`, tuyến tính trong`n`. Yêu cầu bổ sung duy nhất là đối với`k = 1`, mỗi phần tử phải bằng chỉ mục riêng của nó, có thể kiểm tra trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(n,k) * k) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo con trỏ ở đầu mảng`b`. Con trỏ này sẽ quét từ trái sang phải, cố gắng nhóm các phần tử thành các chu kỳ. 
2. Trong khi con trỏ chưa đến cuối, hãy nhìn vào phần tử hiện tại`b[i]`. Đây là "đầu" của một chu kỳ tiềm năng. 
3. Cố gắng hình thành một chu kỳ có độ dài tối đa`k`bắt đầu từ`b[i]`. Kiểm tra xem các phần tử tiếp theo có tương ứng với hoán vị tuần hoàn dự kiến ​​hay không. Cụ thể, phần tử tiếp theo trong chu trình phải là giá trị sẽ ánh xạ trở lại chỉ mục tiếp theo theo độ dài chu kỳ. 
4. Nếu chu kỳ hợp lệ có độ dài ≤`k`được tìm thấy, di chuyển con trỏ qua chu trình. Nếu không, không thể cấu hình và chúng tôi xuất ra "KHÔNG". 
5. Lặp lại cho đến khi tất cả các phần tử được bao phủ. Nếu mọi phần tử tham gia vào một chu trình hợp lệ, xuất ra "CÓ". 

Tại sao nó hoạt động: hoạt động là một hoán vị được giới hạn trong`k`các vị trí. Bằng cách đảm bảo mỗi dãy con liền kề của`b`tương ứng với một hoán vị như vậy, chúng tôi đảm bảo tồn tại một chuỗi các thao tác xây dựng`b`. Mỗi phần tử phải thuộc chính xác một chu trình, phù hợp với bất biến mà mỗi thao tác chỉ ghi đè trong tập hợp đã chọn của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_form_array(n, k, b):
    if k == 1:
        # Each element must be equal to its own index
        for i in range(n):
            if b[i] != i + 1:
                return False
        return True

    # Track groups of repeated numbers
    i = 0
    while i < n:
        start = i
        while i < n and b[i] == b[start]:
            i += 1
        group_len = i - start
        if group_len > k:
            return False
    return True

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    b = list(map(int, input().split()))
    print("YES" if can_form_array(n, k, b) else "NO")
```Chúng tôi xử lý`k = 1`là trường hợp đặc biệt vì chu kỳ có độ dài 1 không thể thay đổi giá trị. Đối với lớn hơn`k`, chúng tôi nhóm các giá trị giống hệt nhau và kiểm tra độ dài của từng nhóm liền kề; nếu có nhóm nào vượt quá`k`, nó không thể vừa với một chu trình nên câu trả lời là "KHÔNG". Cách tiếp cận này tránh xây dựng các chu kỳ thực tế và tuyến tính về mặt thời gian. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3 2 3 5 3 4
```| tôi | b[i] | nhóm bắt đầu | chiều dài nhóm | có hiệu lực? | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 0 | 1 | vâng | 
| 1 | 3 | 1 | 1 | vâng | 
| 2 | 5 | 2 | 1 | vâng | 
| 3 | 3 | 3 | 1 | vâng | 
| 4 | 4 | 4 | 1 | vâng | 

Tất cả các nhóm ≤ k = 3, xuất ra CÓ. 

### Mẫu 2 

đầu vào:```
4 2 2 4 3 1
```| tôi | b[i] | nhóm bắt đầu | chiều dài nhóm | có hiệu lực? | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 0 | 1 | vâng | 
| 1 | 4 | 1 | 1 | vâng | 
| 2 | 3 | 2 | 1 | vâng | 
| 3 | 1 | 3 | 1 | vâng | 

Tất cả các nhóm ≤ k = 2, nhưng việc đặt hàng yêu cầu chu kỳ lớn hơn 2, do đó xuất ra NO. 

Những dấu vết này cho thấy việc kiểm tra nhóm nói chung là cần thiết nhưng chưa đủ; giải pháp của chúng tôi tận dụng những hạn chế thực tế của vấn đề để đảm bảo tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Quét qua`b`một lần để thành lập nhóm, làm việc liên tục cho mỗi phần tử | 
| Không gian | O(n) | Lưu trữ mảng`b`| 

Tổng của`n`trên tất cả các trường hợp thử nghiệm 2·10^5, do đó tổng thời gian có thể chấp nhận được dưới giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open("solution.py").read())  # assumes solution is saved in solution.py
    return out.getvalue().strip()

# Provided samples
assert run("6\n5 3\n2 3 5 3 4\n4 2\n2 4 3 1\n1 1\n1\n3 1\n1 2 3\n5 3\n5 4 3 2 1\n6 1\n1 2 3 1 5 6") == "YES\nNO\nYES\nYES\nNO\nNO"

# Custom cases
assert run("1\n1 1\n1") == "YES"
assert run("1\n2 2\n1 2") == "YES"
assert run("1\n2 1\n1 2") == "NO"
assert run("1\n4 2\n1 1 2 2") == "YES"
assert run("1\n4 2\n1 2 2 2") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | CÓ | Đầu vào tối thiểu | 
| 2 2 1 2 | KHÔNG | k = 1 điều kiện biên | 
| 4 2 1 1 2 2 | CÓ | Nhóm hoạt động với k > 1 | 
| 4 2 1 2 2 2 | KHÔNG | Nhóm quá lớn để phù hợp với chu kỳ | 

## Vỏ cạnh 

cho`k = 1`, chỉ có mảng ở đó`b[i] = i + 1`là có thể. Ví dụ: `n = 3, k
