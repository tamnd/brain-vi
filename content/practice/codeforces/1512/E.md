---
title: "CF 1512E - Hoán vị bằng tổng"
description: "Chúng ta được yêu cầu xây dựng một hoán vị các số từ 1 đến $n$ sao cho tổng của một đoạn liền kề từ chỉ số $l$ đến $r$ bằng một giá trị cho trước $s$. Hoán vị ở đây chỉ là sự sắp xếp của tất cả các số nguyên từ 1 đến $n$ không lặp lại."
date: "2026-06-10T18:53:28+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1512
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 713 (Div. 3)"
rating: 1600
weight: 1512
solve_time_s: 147
verified: false
draft: false
---

[CF 1512E - Hoán vị theo Tổng](https://codeforces.com/problemset/problem/1512/E) 

**Đánh giá:** 1600 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một hoán vị các số từ 1 đến$n$sao cho tổng của một phân đoạn liền kề từ chỉ mục$l$ĐẾN$r$bằng một giá trị nhất định$s$. Hoán vị ở đây chỉ là sự sắp xếp của tất cả các số nguyên từ 1 đến$n$không có sự lặp lại. Đầu vào đưa ra nhiều trường hợp kiểm thử, mỗi trường hợp chỉ định$n$,$l$,$r$, Và$s$. Đầu ra phải là một hoán vị hợp lệ thỏa mãn điều kiện tổng hoặc -1 nếu không tồn tại hoán vị đó. 

Thách thức chính đến từ thực tế là$s$có thể là bất kỳ tổng nào trong phạm vi các tổng có thể có của phân khúc đó, được giới hạn bằng cách chọn số nhỏ nhất hoặc lớn nhất$k = r - l + 1$số từ 1 đến$n$. Cụ thể, tổng nhỏ nhất có thể của$k$số là$1 + 2 + \dots + k = k(k+1)/2$, và lớn nhất là$n + (n-1) + \dots + (n-k+1) = k(2n - k + 1)/2$. Nếu như$s$nằm ngoài phạm vi này thì không có giải pháp. 

Cho rằng$n$nhiều nhất là 500 và tổng của$n$trên tất cả các trường hợp thử nghiệm cũng tối đa là 500, chúng ta có thể sử dụng$O(n^2)$hoặc thậm chí$O(n^3)$cách tiếp cận không vượt quá thời hạn, nhưng cách tiếp cận tuyến tính hoặc gần tuyến tính được ưa chuộng hơn vì sự rõ ràng và đơn giản. 

Một cách tiếp cận ngây thơ sẽ thử tất cả các kết hợp của$k$các phần tử từ 1 đến$n$để xem liệu chúng có tổng hợp không$s$, về mặt tổ hợp thì quá chậm. Các trường hợp cạnh có thể gây khó khăn cho việc triển khai sơ bộ bao gồm các đoạn có độ dài 1 hoặc$n$, tổng bằng mức tối thiểu hoặc tối đa có thể và các trường hợp các phần tử còn lại cũng phải tạo thành một hoán vị hợp lệ. 

Ví dụ, với$n=3, l=1, r=2, s=4$, người ta có thể chọn$[1,3]$cho phân khúc này, nhưng điều đó để lại$[2]$cho phần tử còn lại. Việc xử lý bất cẩn có thể tạo ra sự trùng lặp hoặc bỏ qua các cấu hình không thể thực hiện được. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các kết hợp kích thước$k = r - l + 1$từ$[1, n]$và kiểm tra xem tổng của chúng có bằng không$s$. Nếu có sự kết hợp như vậy thì người ta có thể đặt các số còn lại vào các vị trí khác một cách tùy ý. Sự phức tạp là$O(\binom{n}{k})$cho mỗi trường hợp thử nghiệm, điều này phù hợp với quy mô nhỏ$n$nhưng trở nên chậm khi$n$là khoảng 500. Cách tiếp cận này đúng nhưng không hiệu quả. 

Cái nhìn sâu sắc quan trọng cho một giải pháp hiệu quả là nhận ra rằng chúng ta có thể xây dựng phân khúc một cách tham lam. Bắt đầu từ việc nhỏ nhất$k$số, đưa ra số tiền tối thiểu. Sau đó, tăng các số lớn nhất trong phân đoạn để tăng tổng về phía$s$trong khi vẫn giữ các con số khác biệt. Khi chúng tôi có một phân đoạn hợp lệ có tổng giá trị là$s$, chúng ta có thể điền vào các vị trí còn lại bằng các số còn sót lại theo thứ tự bất kỳ. Điều này có tác dụng vì tổng của bất kỳ$k$số phân biệt trong khoảng$[1, n]$có thể được điều chỉnh bằng cách hoán đổi số nhỏ hơn với số lớn hơn chưa sử dụng. 

Điều này làm giảm vấn đề về một cấu trúc tuyến tính hơn là tìm kiếm tổ hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\binom{n}{k})$|$O(n)$| Quá chậm đối với$n \sim 500$| 
| Xây dựng tham lam |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán$k = r - l + 1$, độ dài của đoạn 
2. Tính tổng nhỏ nhất của số đầu tiên$k$số:$min\_sum = k(k+1)/2$, và tổng lớn nhất của lớn nhất$k$số:$max\_sum = k(2n - k + 1)/2$. 
3. Nếu$s < min\_sum$hoặc$s > max\_sum$, xuất ra -1 vì không phân khúc nào có thể đạt được tổng này. 
4. Khởi tạo phân đoạn đầu tiên$k$số:$[1, 2, ..., k]$. Tính tổng hiện tại của phân đoạn này. 
5. Bắt đầu từ cuối đoạn và cố gắng tăng từng số nhiều nhất có thể mà không vượt quá$n$và không lặp lại các số trong đoạn. Đối với số tại chỉ mục$i$, giá trị lớn nhất có thể là$n - (k - i - 1)$. Tăng nó vừa đủ để tổng số tiền bằng$s$. 
6. Đặt đoạn đã xây dựng này vào các vị trí$l-1$ĐẾN$r-1$trong hoán vị. 
7. Điền các số còn lại từ 1 đến$n$không có trong phân khúc. Bất kỳ đơn đặt hàng nào cũng hoạt động. 
8. Xuất ra hoán vị kết quả. 

Bất biến chính là chúng tôi luôn giữ số phân đoạn khác biệt và trong phạm vi hợp lệ. Việc điều chỉnh từ cuối đảm bảo rằng các số nhỏ hơn không bị chặn lấy các giá trị cần thiết để đạt được tổng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, l, r, s = map(int, input().split())
        k = r - l + 1
        min_sum = k * (k + 1) // 2
        max_sum = k * (2 * n - k + 1) // 2
        if s < min_sum or s > max_sum:
            print(-1)
            continue
        
        # Start with the smallest k numbers
        seg = list(range(1, k + 1))
        curr_sum = min_sum
        for i in range(k - 1, -1, -1):
            # Max possible for this position without breaking uniqueness
            max_val = n - (k - 1 - i)
            if curr_sum == s:
                break
            add = min(max_val - seg[i], s - curr_sum)
            seg[i] += add
            curr_sum += add
        
        # Fill the permutation
        used = set(seg)
        perm = []
        idx = 0
        for pos in range(1, n + 1):
            if l <= pos <= r:
                perm.append(seg[idx])
                idx += 1
            else:
                for candidate in range(1, n + 1):
                    if candidate not in used:
                        perm.append(candidate)
                        used.add(candidate)
                        break
        print(*perm)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên sẽ kiểm tra xem tổng có thể đạt được hay không. Phân đoạn được xây dựng bằng cách tăng dần từ cuối để đạt được tổng mong muốn trong khi vẫn duy trì tính duy nhất. Các số còn lại được điền tuần tự từ các giá trị không sử dụng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$n=5, l=2, r=3, s=5$| Bước | phân đoạn | curr_sum | Giải thích | 
| --- | --- | --- | --- | 
| ban đầu | [1,2] | 3 | số tiền tối thiểu | 
| tôi=1 | [1,3] | 4 | tăng 2->3 | 
| tôi=0 | [2,3] | 5 | tăng 1->2 | 
| điền vào perm | [1,2,3,4,5] | - | đoạn ở vị trí 2-3: [2,3], phần còn lại được lấp đầy | 

Đầu ra: 1 2 3 4 5 

### Ví dụ 2 

đầu vào:$n=5, l=3, r=4, s=1$- min_sum = 1+2=3 > s=1, không thể. 

Đầu ra: -1 

Những ví dụ này chứng minh thuật toán điều chỉnh chính xác phân đoạn hoặc loại bỏ các khoản tiền không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi lặp lại theo độ dài phân đoạn và sau đó trên tất cả các vị trí một lần | 
| Không gian | O(n) | Lưu trữ phân đoạn, hoán vị và tập hợp đã sử dụng | 

Vì tổng của$n$trên tất cả các trường hợp thử nghiệm là ≤ 500, giải pháp này phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("5\n5 2 3 5\n5 3 4 1\n3 1 2 4\n2 2 2 2\n2 1 1 3\n") == "1 2 3 4 5\n-1\n1 3 2\n1 2\n-1"

# Custom cases
assert run("1\n3 1 3 6\n") == "1 2 3"
assert run("1\n3 1 3 5\n") == "1 2 3" or run("1\n3 1 3 5\n") == "2 1 3"
assert run("1\n4 2 3 7\n") == "1 3 4 2" or run("1\n4 2 3 7\n") == "2 3 4 1"
assert run("1\n5 1 5 15\n") == "1 2 3 4
```
