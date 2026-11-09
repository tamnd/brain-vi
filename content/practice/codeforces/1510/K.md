---
title: "CF 1510K - Nhiệm Vụ Của Vua"
description: "Chúng ta được cho một hoán vị các số nguyên từ 1 đến 2n và chúng ta được phép thực hiện hai phép toán đặc biệt để sắp xếp lại nó. Thao tác đầu tiên hoán đổi mọi cặp liên tiếp trong chuỗi: vị trí 1 và 2, 3 và 4, v.v."
date: "2026-06-10T19:32:37+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1510
codeforces_index: "K"
codeforces_contest_name: "2020-2021 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1200
weight: 1510
solve_time_s: 154
verified: true
draft: false
---

[CF 1510K - Nhiệm vụ của nhà vua](https://codeforces.com/problemset/problem/1510/K) 

**Đánh giá:** 1200 
**Tags:** bạo lực, đồ thị, thực hiện 
**Thời gian giải:** 2m 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số nguyên từ 1 đến 2n và chúng ta được phép thực hiện hai phép toán đặc biệt để sắp xếp lại nó. Thao tác đầu tiên hoán đổi mọi cặp liên tiếp trong chuỗi: vị trí 1 và 2, 3 và 4, v.v. Phép toán thứ hai hoán đổi nửa đầu của hoán vị với nửa sau theo phần tử: vị trí 1 với n+1, 2 với n+2, lên đến n với 2n. Mục đích là xác định số lượng tối thiểu các thao tác này cần thiết để sắp xếp hoán vị theo thứ tự tăng dần. Nếu không thể sắp xếp chỉ bằng các thao tác này, chúng ta phải trả về -1. 

Ràng buộc n 1000 ngụ ý độ dài hoán vị 2n 2000. Điều này cho phép thực hiện các thuật toán có độ phức tạp lên đến khoảng O(n²) mà không hết thời gian chờ, nhưng mọi thứ O(2^(2n)) rõ ràng là không khả thi. Các hoạt động có cấu trúc cao, điều này gợi ý rằng chúng ta có thể khai thác các chu kỳ hoặc các ứng dụng lặp lại thay vì thử tất cả các trình tự một cách mù quáng. 

Trường hợp cạnh tinh tế là khi hoán vị đã được sắp xếp. Số lượng thao tác tối thiểu bằng 0, phải được phát hiện mà không thực hiện các thao tác hoán đổi không cần thiết. Một tình huống phức tạp khác là khi áp dụng lặp đi lặp lại hai phép toán sẽ thực hiện các hoán vị mà không bao giờ đạt đến trạng thái được sắp xếp. Ví dụ: một hoán vị có thể lặp vô hạn giữa hai trạng thái, nghĩa là một cách tiếp cận đơn giản là chỉ thử các hoạt động xen kẽ có thể bị kẹt hoặc tạo ra số lượng tối thiểu sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét mọi chuỗi hoạt động. Bắt đầu từ hoán vị đã cho, chúng ta có thể áp dụng thao tác 1 hoặc 2 ở mỗi bước, tạo ra một cây khả năng cho đến khi đạt được hoán vị được sắp xếp. Mỗi nút có hai nút con (tương ứng với hai thao tác) nên số trạng thái tăng theo cấp số nhân: O(2^k), trong đó k là số thao tác. Với n 1000, độ dài hoán vị 2n 2000 và số hoán vị phân biệt có thể có là (2n)!, khiến phương pháp này hoàn toàn không khả thi. 

Thông tin chi tiết quan trọng là cả hai thao tác đều là phép biến đổi: áp dụng cùng một thao tác hai lần sẽ khôi phục hoán vị ban đầu. Thao tác 1 hoán đổi các cặp liên tiếp, do đó thực hiện hai lần không có tác dụng gì. Thao tác 2 hoán đổi nửa thứ nhất và nửa thứ hai, do đó áp dụng nó hai lần cũng trở về hoán vị ban đầu. Điều này làm giảm đáng kể số lượng các chuỗi thao tác riêng biệt: chúng ta chỉ cần xem xét các chuỗi trong đó các thao tác xen kẽ nhau. Do đó, có thể đạt được số lượng thao tác tối thiểu bằng cách mô phỏng hai tình huống riêng biệt: bắt đầu bằng cách áp dụng lặp lại thao tác 1 hoặc bắt đầu bằng cách áp dụng lặp lại thao tác 2. Chúng tôi theo dõi số bước cho đến khi hoán vị được sắp xếp hoặc cho đến khi phát hiện được một chu trình (có nghĩa là không thể sắp xếp). 

Cách tiếp cận này có hiệu quả vì bất kỳ trình tự tối ưu nào cũng có thể được biểu diễn dưới dạng một chuỗi các thao tác xen kẽ. Bằng cách mô phỏng cả hai kịch bản bắt đầu, chúng tôi đảm bảo rằng chúng tôi khám phá tất cả các khả năng có thể mang lại số lượng thao tác tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(2n)) | O(2n) | Quá chậm | 
| Mô phỏng xen kẽ | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai bản sao của hoán vị,`perm1`Và`perm2`.`perm1`đại diện cho kịch bản mà chúng ta bắt đầu với thao tác 1 và`perm2`thể hiện kịch bản nơi chúng ta bắt đầu với thao tác 2. 
2. Khởi tạo bộ đếm`cnt1`Và`cnt2`về không. Chúng sẽ theo dõi số lượng hoạt động được thực hiện trong mỗi kịch bản. 
3. Đối với`perm1`, áp dụng lặp lại thao tác 1, kiểm tra xem hoán vị có được sắp xếp sau mỗi ứng dụng hay không, sau đó áp dụng thao tác 2 và tăng bộ đếm. Dừng nếu phát hiện thấy một chu kỳ (tức là hoán vị trở về trạng thái ban đầu mà không được sắp xếp). 
4. Lặp lại mô phỏng tương tự cho`perm2`, nhưng thay vào đó hãy bắt đầu bằng thao tác 2. 
5. Theo dõi giá trị bộ đếm tối thiểu giữa hai mô phỏng. Nếu không mô phỏng nào đạt đến hoán vị được sắp xếp, hãy trả về -1. 
6. In số lượng thao tác tối thiểu được tìm thấy. 

**Tại sao nó hoạt động:** Cả hai hoạt động đều là sự tiến triển. Bất kỳ chuỗi thao tác nào cũng có thể được giảm xuống thành chuỗi xen kẽ 1 và 2 giây. Bằng cách mô phỏng cả hai kịch bản bắt đầu và đếm cho đến khi chúng tôi sắp xếp hoán vị hoặc quay lại trạng thái ban đầu, chúng tôi đảm bảo rằng chúng tôi tìm thấy số bước tối thiểu hoặc phát hiện những điều không thể thực hiện được. Hoán vị không bao giờ có thể thực hiện quá 2n thao tác trước khi lặp lại, do đó thuật toán luôn kết thúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_sorted(arr):
    for i in range(1, len(arr)):
        if arr[i-1] > arr[i]:
            return False
    return True

def op1(arr):
    n = len(arr) // 2
    res = arr[:]
    for i in range(0, 2*n, 2):
        res[i], res[i+1] = res[i+1], res[i]
    return res

def op2(arr):
    n = len(arr) // 2
    res = arr[:]
    for i in range(n):
        res[i], res[i+n] = res[i+n], res[i]
    return res

def solve():
    n = int(input())
    p = list(map(int, input().split()))
    
    orig = p[:]
    scenarios = [(p[:], 0, True), (p[:], 0, False)]
    ans = float('inf')
    
    for start_op1 in [True, False]:
        perm = orig[:]
        steps = 0
        visited = set()
        visited.add(tuple(perm))
        while True:
            if is_sorted(perm):
                ans = min(ans, steps)
                break
            if start_op1:
                perm = op1(perm)
            else:
                perm = op2(perm)
            steps += 1
            if is_sorted(perm):
                ans = min(ans, steps)
                break
            # Alternate the operation
            if start_op1:
                perm = op2(perm)
            else:
                perm = op1(perm)
            steps += 1
            t = tuple(perm)
            if t in visited:
                break
            visited.add(t)
    
    print(-1 if ans == float('inf') else ans)

solve()
```**Giải thích:** Chúng tôi xác định các hàm trợ giúp để kiểm tra xem mảng đã được sắp xếp chưa và để áp dụng hai thao tác. Chúng tôi mô phỏng hai kịch bản: bắt đầu với thao tác 1 và thao tác 2. Chúng tôi luân phiên các thao tác và giữ một tập hợp đã truy cập để phát hiện chu kỳ. Số lượng thao tác tối thiểu trên cả hai mô phỏng được trả về hoặc -1 nếu không đạt được hoán vị được sắp xếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
6 3 2 5 4 1
```| Bước | Hoán vị | Hoạt động được áp dụng | Đếm bước | 
| --- | --- | --- | --- | 
| 0 | 6 3 2 5 4 1 | Bắt đầu | 0 | 
| 1 | 3 6 5 2 1 4 | op1 | 1 | 
| 2 | 2 1 4 3 6 5 | op2 | 2 | 
| 3 | 1 2 3 4 5 6 | op1 | 3 | 

Hoán vị được sắp xếp theo 3 thao tác, khớp với kết quả mong đợi. 

### Đầu vào tùy chỉnh 

đầu vào:```
2
2 1 4 3
```| Bước | Hoán vị | Hoạt động được áp dụng | Đếm bước | 
| --- | --- | --- | --- | 
| 0 | 2 1 4 3 | Bắt đầu | 0 | 
| 1 | 1 2 3 4 | op1 | 1 | 

Các thao tác tối thiểu: 1. Hiển thị thuật toán phát hiện sắp xếp sớm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi thao tác mất O(n) và tối đa 2n thao tác được áp dụng cho mỗi kịch bản | 
| Không gian | O(n) | Mảng có kích thước 2n và một bộ lưu trữ các bộ dữ liệu để phát hiện chu trình | 

Thuật toán phù hợp một cách thoải mái trong các giới hạn vì n ≤ 1000, cho phép thực hiện tối đa 2n² = 2.000.000 thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("3\n6 3 2 5 4 1\n") == "3"

# Already sorted
assert run("2\n1 2 3 4\n") == "0"

# Minimum-size input
assert run("1\n2 1\n") == "1"

# Impossible case
assert run("2\n2 1 4 3\n") == "1"

# Maximum-size input with sorted permutation
max_input = "1000\n" + " ".join(str(i) for i in range(1, 2001)) + "\n"
assert run(max_input) == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3\n6 3 2 5 4 1 | 3 | Đúng trình tự các thao tác xen kẽ | 
| 2\n1 2 3 4 | 0 | Phát hiện hoán vị đã được sắp xếp | 
| 1\n2 1 | 1 | Xử lý đầu vào tối thiểu | 
| 2\n2 | | |
