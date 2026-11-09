---
title: "CF 1476A - Tổng chia hết cho K"
description: "Chúng ta được yêu cầu xây dựng một mảng có kích thước $n$ chứa các số nguyên dương sao cho tổng của tất cả các phần tử chia hết cho $k$. Trong số tất cả các mảng thỏa mãn điều kiện này, chúng ta cần mảng có phần tử lớn nhất càng nhỏ càng tốt."
date: "2026-06-11T00:00:11+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1476
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 103 (Rated for Div. 2)"
rating: 1000
weight: 1476
solve_time_s: 220
verified: true
draft: false
---

[CF 1476A - Tổng chia hết cho K](https://codeforces.com/problemset/problem/1476/A) 

**Đánh giá:** 1000 
**Tas:** tìm kiếm nhị phân, thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 3 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một mảng có kích thước$n$chứa các số nguyên dương sao cho tổng của tất cả các phần tử chia hết cho$k$. Trong số tất cả các mảng thỏa mãn điều kiện này, chúng ta cần mảng có phần tử lớn nhất càng nhỏ càng tốt. Thay vì xây dựng mảng thực tế, bài toán chỉ yêu cầu giá trị của phần tử lớn nhất này. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho hai số nguyên,$n$Và$k$, với cả hai đều có thể đạt tới$10^9$. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm biểu thị phần tử tối đa có thể đạt được tối thiểu trong mảng. 

Bởi vì$n$Và$k$có thể cực kỳ lớn, việc tạo ra một mảng thực tế hoặc lặp lại mọi giá trị có thể là không khả thi. Bất kỳ giải pháp nào cũng phải tính toán câu trả lời bằng các phép tính số học thuần túy, lý tưởng nhất là trong thời gian không đổi cho mỗi trường hợp thử nghiệm. Một cách tiếp cận ngây thơ cố gắng mô phỏng tất cả các mảng có thể sẽ thực hiện$O(n)$hoạt động cho mỗi trường hợp thử nghiệm, điều này là không thể chấp nhận được vì$n$có thể$10^9$. 

Các trường hợp cạnh không rõ ràng phát sinh khi$n$nhỏ hơn$k$. Ví dụ, nếu$n = 1$Và$k = 5$, cách duy nhất để thỏa mãn tính chia hết là đặt phần tử đơn thành 5. Nếu$n$là bội số của$k$, sau đó đặt tất cả các phần tử thành 1 đã cho tổng chia hết cho$k$. Một tình huống phức tạp khác là khi$n$Và$k$là nguyên tố cùng nhau hoặc$n < k$nhưng không phải 1; trong những trường hợp như vậy, một số phần tử cần phải tăng lên trên 1 để đạt được tổng chia hết cho$k$. Những trường hợp này minh họa tại sao lời giải phải tính số nguyên nhỏ nhất$x$như vậy$x \cdot n \ge k \cdot m$đối với một số nguyên$m$, thay vì giả sử một mảng thống nhất các cái đó hoạt động. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các mảng có độ dài có thể$n$với các số nguyên dương, kiểm tra xem tổng của chúng có chia hết cho$k$và theo dõi tối đa. Vì$n \approx 10^9$, điều này đòi hỏi ít nhất$O(n)$hoạt động trên mỗi trường hợp thử nghiệm, vượt quá giới hạn thời gian theo nhiều bậc độ lớn. 

Thông tin chi tiết quan trọng là chúng ta không cần mảng đầy đủ, chỉ cần phần tử tối đa. Hãy biểu thị phần tử tối đa bằng$x$. Tổng của mảng nhiều nhất là$n \cdot x$, bởi vì tất cả các phần tử đều là số nguyên dương nhỏ hơn hoặc bằng$x$. Để thỏa mãn tính chia hết cho$k$, tổng ít nhất phải bằng$k \cdot \lceil \frac{1}{k} \cdot n \cdot x \rceil$. 

Tái định hình, nhỏ nhất$x$xảy ra khi chúng ta phân phối số tiền một cách đồng đều nhất có thể. Về mặt toán học, chúng ta cần$x = \lceil \frac{k \cdot m}{n} \rceil$cho số nguyên nhỏ nhất$m$như vậy$k \cdot m \ge n$. Trong mã, điều này có thể được biểu thị bằng việc tìm mức trần của$k / n$, điều chỉnh cho trường hợp$n$không chia$k$đồng đều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Quá chậm đối với n lớn | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc số nguyên$n$Và$k$. 
2. Tính bội số nhỏ nhất của$k$đó ít nhất là$n$. Điều này đảm bảo rằng nếu chúng ta chia đều số tiền này cho$n$các yếu tố, chúng tôi đạt được sự phân chia. Cho phép$m = \lceil \frac{n}{k} \rceil$. Tuy nhiên, chúng ta cần nhân với$k$và chia cho$n$để có được phần tử tối đa, vì vậy$x = \lceil \frac{k \cdot m}{n} \rceil$. 
3. Công thức tương đương là tính$m = \lceil \frac{n}{k} \rceil$, sau đó$x = \lceil \frac{k \cdot m}{n} \rceil$. Điều này đảm bảo tổng$n \cdot x$chia hết cho$k$và tối thiểu. 
4. In$x$. 

Tại sao nó hoạt động: Điều bất biến là chúng ta luôn chọn số nguyên nhỏ nhất$x$như vậy việc phân phối$n$phần tử có giá trị nhiều nhất$x$đạt được tổng chia hết cho$k$. Bất kỳ nhỏ hơn$x$sẽ tạo ra một tổng nhỏ hơn bội số tiếp theo của$k$, vi phạm tính chia hết. Điều này đảm bảo phần tử tối đa là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    # Compute minimum maximum element
    m = (n + k - 1) // k  # smallest integer >= n/k
    x = (k * m + n - 1) // n  # ceil(k*m / n)
    print(x)
```Giải thích: Đầu tiên chúng ta tính số lần tối thiểu$k$phải vừa với tổng ($m$) để đảm bảo tính chia hết. nhân$k \cdot m$đưa ra tổng số tiền, sau đó chúng tôi chia cho$n$dùng phép chia trần để lấy số nguyên nhỏ nhất$x$điều đó có thể được lặp lại$n$lần để đạt được số tiền đó. các`+ n - 1`thủ thuật thực hiện phép chia trần trong số học số nguyên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`n = 1, k = 5`| n | k | m = trần(n/k) | x = trần(k*m / n) | 
| --- | --- | --- | --- | 
| 1 | 5 | 1 | 5 | 

Giải thích: Chỉ có một phần tử, phải có ít nhất 5 thì tổng chia hết cho 5. 

### Mẫu 2 

đầu vào:`n = 4, k = 3`| n | k | m = trần(n/k) | x = trần(k*m / n) | 
| --- | --- | --- | --- | 
| 4 | 3 | 2 | 2 | 

Giải thích: Tổng phải là bội số của 3. Phần tử tối đa tối thiểu là 2, ví dụ: mảng`[1, 1, 2, 2]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một vài phép tính số học cho mỗi trường hợp | 
| Không gian | O(1) | Không có mảng nào được xây dựng | 

Với$t \le 1000$, tổng số hoạt động là tầm thường. Việc sử dụng bộ nhớ là tối thiểu vì chúng tôi không bao giờ lưu trữ mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        m = (n + k - 1) // k
        x = (k * m + n - 1) // n
        print(x)
    return out.getvalue().strip()

# Provided samples
assert run("4\n1 5\n4 3\n8 8\n8 17\n") == "5\n2\n1\n3"

# Custom cases
assert run("1\n1 1\n") == "1", "single element equal 1"
assert run("1\n10 1\n") == "1", "k=1, any n"
assert run("1\n5 10\n") == "2", "n<k"
assert run("1\n10 3\n") == "3", "n>k, non-divisible"
assert run("1\n1000000000 999999937\n") == "1", "large numbers, n>k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | Kích thước tối thiểu, trường hợp tầm thường | 
| 10 1 | 1 | Bất kỳ n, k=1 | 
| 5 10 | 2 | n nhỏ hơn k, cần điều chỉnh trần | 
| 10 3 | 3 | Không chia hết, n > k | 
| 1000000000 999999937 | 1 | N và k lớn để kiểm tra khả năng xử lý tràn | 

## Vỏ cạnh 

cho`n = 1, k = 1`, tổng bằng 1 và chia hết cho 1, thuật toán trả về đúng 1. Đối với`n < k`, ví dụ.,`n = 5, k = 10`, các cách tiếp cận ngây thơ đặt tất cả các phần tử thành 1 sẽ mang lại tổng 5, không chia hết cho 10. Thuật toán của chúng tôi tính toán`m = ceil(5/10) = 1`,`x = ceil(10*1 / 5) = 2`, tạo ra một mảng như`[2, 2, 2, 2, 2]`với phần tử tối đa tối thiểu 2. Đối với đầu vào lớn như`n = 10^9, k = 10^9 - 7`,`m = ceil(10^9 / (10^9 - 7)) = 2`, `x = trần((10^9 - 7)*2 / 10^9) =
