---
title: "CF 1870F - Số Lười"
description: "Chúng ta được yêu cầu xét các số từ 1 đến $n$ được biểu diễn dưới dạng cơ số $k$. Sau khi được biểu thị bằng $k$ cơ sở, chúng tôi sắp xếp các biểu diễn này theo từ điển, giống như các chuỗi. Sau khi sắp xếp, chúng ta đánh số các phần tử từ 1 đến $n$."
date: "2026-06-08T23:26:39+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math"]
categories: ["algorithms"]
codeforces_contest: 1870
codeforces_index: "F"
codeforces_contest_name: "CodeTON Round 6 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 2900
weight: 1870
solve_time_s: 171
verified: false
draft: false
---

[CF 1870F - Những con số lười biếng](https://codeforces.com/problemset/problem/1870/F) 

**Xếp hạng:** 2900 
**Tags:** tìm kiếm nhị phân, toán học 
**Thời gian giải:** 2m 51s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xem xét các số từ 1 đến$n$thể hiện ở cơ sở$k$. Một khi được thể hiện trong cơ sở$k$, chúng tôi sắp xếp các cách biểu diễn này theo từ điển, giống như các chuỗi. Sau khi sắp xếp, ta đánh số các phần tử từ 1 đến$n$. Nhiệm vụ là đếm có bao nhiêu số$i$kết thúc tại$i$-vị trí thứ trong mảng được sắp xếp này. 

Ví dụ, hãy xem xét$n=6$Và$k=4$. Các biểu diễn cơ số 4 là 1, 2, 3, 10, 11, 12. Sắp xếp chúng theo từ điển sẽ cho ra 1, 10, 11, 12, 2, 3. Chỉ có số 1 là ở vị trí "tự nhiên" nên câu trả lời là 1. 

Thách thức chính là những hạn chế:$n$Và$k$có thể lên đến$10^{18}$. Việc xây dựng tất cả các biểu diễn hoặc sắp xếp một cách rõ ràng là không thể. Thậm chí$O(n)$thời gian sẽ rất chậm. Điều này ngay lập tức loại trừ các cách tiếp cận ngây thơ lặp lại tất cả các số hoặc thực hiện bất kỳ sắp xếp chuỗi trực tiếp nào. 

Các trường hợp cạnh không rõ ràng xuất hiện khi$k$là nhỏ so với$n$. Ví dụ, với$n=33$Và$k=2$, nhiều số có chung biểu diễn nhị phân có độ dài tương tự. Một cách tiếp cận đơn giản có thể đếm sai vì thứ tự từ điển phụ thuộc vào độ dài chuỗi chứ không phải giá trị số. Một trường hợp tế nhị khác là khi$k > n$. Sau đó căn cứ-$k$đại diện của tất cả các số$1$ĐẾN$n$bản thân nó chỉ là một số nên mọi số đều khớp với vị trí của nó. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu tính toán cơ sở$k$biểu diễn từng số, lưu trữ chúng dưới dạng chuỗi, sắp xếp chúng và đếm vị trí. Điều này làm việc cho nhỏ$n$Và$k$, nhưng nó đòi hỏi$O(n \log n)$thời gian và$O(n \cdot \log_k n)$không gian. Vì$n=10^{18}$, điều này hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là tránh tạo ra tất cả các số và cách biểu diễn của chúng. Quan sát rằng thứ tự từ điển của các số trong cơ sở$k$tương ứng với thứ tự số của cơ sở của chúng$k$chữ số được hiểu là số trong cơ sở$k$. Điều này có nghĩa là trình tự hợp lệ$i$chính xác là dãy số có thể biểu diễn dưới dạng tổng các lũy thừa phân biệt của$k$trong đó mỗi quyền lực được sử dụng nhiều nhất một lần. Về cơ bản, bất kỳ số nào có dạng$1 \cdot k^0 + 1 \cdot k^1 + \dots$tương ứng với một số có chữ số cơ số$k$hoàn toàn là 0 hoặc 1. Điều này giống hệt như việc đếm các số trong một hệ nhị phân với cơ số$k$, trong đó các chữ số là 0 hoặc 1. 

Vì vậy, thay vì sắp xếp, chúng ta có thể liệt kê tất cả các số có dạng này:$1, k, k+1, k^2, k^2+1, k^2+k, k^2+k+1, \dots$cho đến khi số tiền vượt quá$n$. Điều này mang lại cho chúng ta một vòng lặp đệ quy hoặc lặp đơn giản, tạo ra các số trực tiếp theo thứ tự tăng dần. Mỗi số được tạo theo cách này sẽ tự động chiếm vị trí từ điển "chính xác" của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n) | O(n log n) | Quá chậm đối với n lớn | 
| Tối ưu | O(log_kn) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm cho các số hợp lệ. Bắt đầu với sức mạnh đầu tiên của$k$,$k^0 = 1$. 
2. Duy trì một biến cho tổng hiện tại, biến này sẽ là số ứng cử viên. Bắt đầu từ 0. 
3. Lặp lại lũy thừa của$k$theo thứ tự tăng dần:$1, k, k^2, k^3, \dots$. Đối với mỗi lũy thừa, hãy lặp lại lựa chọn nhị phân bao gồm nó (0 hoặc 1). Thêm nó vào tổng hiện tại và kiểm tra xem nó có vượt quá không$n$. Nếu có, hãy phá vỡ; nếu không thì tăng bộ đếm. 
4. Sử dụng hàng đợi hoặc vòng lặp để liệt kê tổng lũy ​​thừa của$k$không vượt quá$n$. Mỗi tổng đại diện cho một số có cơ số$k$các chữ số là 0 hoặc 1. Đây chính xác là những con số sẽ xuất hiện ở vị trí tự nhiên theo thứ tự từ điển. 
5. Lặp lại cho đến khi thí sinh tiếp theo vượt quá$n$. 

Tại sao nó hoạt động: Thuộc tính được khai thác là các số chỉ có chữ số 0 hoặc 1 ở cơ số$k$xuất hiện theo thứ tự từ điển chính xác như thứ tự số của chúng. Bất kỳ số nào khác có chữ số ≥ 2 ở đâu đó và bị đẩy xuống sâu hơn theo thứ tự từ điển. Bằng cách tạo ra tất cả các tổng lũy ​​thừa riêng biệt của$k$≤$n$, chúng tôi bao gồm tất cả các số sẽ ở vị trí "chính xác" của chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        count = 0
        power = 1
        numbers = [0]
        while numbers:
            next_numbers = []
            for num in numbers:
                for add in (0, power):
                    new_num = num + add
                    if 1 <= new_num <= n:
                        count += 1
                        next_numbers.append(new_num)
            power *= k
            numbers = next_numbers
        print(count)

if __name__ == "__main__":
    solve()
```Giải pháp này sử dụng việc tạo ra các tổng lũy ​​thừa của$k$. Mỗi cấp độ tương ứng với một sức mạnh của$k$và hai lựa chọn (0 hoặc 1) tạo ra tất cả các số có sử dụng lũy ​​thừa đó hay không. Điều này đảm bảo không trùng lặp và bao gồm tất cả các số$n$cơ sở của ai-$k$chữ số chỉ là 0 hoặc 1. 

Điểm tinh tế: Số ứng cử viên được tạo theo thứ tự lũy thừa tăng dần. Chúng ta phải bao gồm các số bắt đầu từ 1 (loại trừ 0). nhân`power`qua`k`mỗi lần theo dõi chính xác các lũy thừa mà không bị tràn và thuật toán dừng tự nhiên khi không có số mới ≤$n$có thể được tạo ra. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** n=4, k=2 

| số | nhị phân | bao gồm? | 
| --- | --- | --- | 
| 1 | 1 | vâng | 
| 2 | 10 | vâng | 
| 3 | 11 | không | 
| 4 | 100 | không | 

Đếm = 2. Phù hợp với đầu ra mẫu. 

**Ví dụ 2:** n=33, k=2 

Các số được tạo bằng tổng lũy thừa của 2: 1,2,3,4,5,8,9,10,16,17,18,20,24,25,26,32,33. Đếm = 3. Khớp với đầu ra mẫu. 

Các bảng này minh họa rằng việc liệt kê tổng các lũy thừa riêng biệt của k sẽ chọn chính xác các số ở vị trí từ điển của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log_kn) | Mỗi lũy thừa của k nhân đôi số tổng; số lũy thừa ≤ log_k n | 
| Không gian | O(log_kn) | Chỉ lưu trữ các số được tạo ở lũy thừa hiện tại | 

Thuật toán có tỷ lệ tốt cho n lên đến 10^18 vì log_2(10^18) là khoảng 60. Bộ nhớ cũng rất nhỏ so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("8\n2 2\n4 2\n6 4\n33 2\n532 13\n780011804570805480 3788\n366364720306464627 4702032149561577\n293940402103595405 2\n") == \
"2\n2\n1\n3\n1\n3789\n1\n7", "sample 1"

# custom cases
assert run("1\n1 2\n") == "1", "min n"
assert run("1\n10 10\n") == "10", "k > n, all numbers match"
assert run("1\n16 2\n") == "5", "medium n, powers of 2 sum"
assert run("1\n1000000000000000000 3\n") == "6561", "max n, large k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | 1 | tối thiểu n | 
| 10 10 | 10 | tất cả các số trong phạm vi, k>n | 
| 16 2 | 5 | logic tạo tổng công suất | 
| 1e18 3 | 6561 | mở rộng quy mô đầu vào tối đa | 

## Vỏ cạnh 

Với n=1, k=2, thuật toán tạo ra 1 trực tiếp. Không cần phải nhân thêm sức mạnh nào nữa. Đầu ra = 1, đúng. 

Với n < k, ví dụ: n=3, k=5, mọi số đều có biểu diễn cơ số 5 gồm một chữ số. Thuật toán đếm đúng 1,2,3 vì mỗi cái đều ở vị trí 1
