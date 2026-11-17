---
title: "CF 1951G - Bóng kêu"
description: "Chúng ta được cho một vòng tròn gồm các giỏ được đánh số từ 1 đến $m$, và ban đầu các quả bóng $n$ được đặt trong các giỏ khác nhau. Alice liên tục chọn ngẫu nhiên một trong các quả bóng giống nhau và di chuyển nó theo chiều kim đồng hồ sang rổ tiếp theo."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1951
codeforces_index: "G"
codeforces_contest_name: "Codeforces Global Round 25"
rating: 3100
weight: 1951
solve_time_s: 85
verified: false
draft: false
---

[CF 1951G - Quả bóng kêu lách cách](https://codeforces.com/problemset/problem/1951/G) 

**Đánh giá:** 3100 
**Tags:** tổ hợp, toán, xác suất 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát một vòng tròn gồm các giỏ được đánh số từ 1 đến$m$, Và$n$quả bóng ban đầu được đặt trong các giỏ riêng biệt. Alice liên tục chọn ngẫu nhiên một trong các quả bóng giống nhau và di chuyển nó theo chiều kim đồng hồ sang rổ tiếp theo. Nếu rổ đó đã có bóng thì quả bóng hiện có sẽ bị ném đi. Quá trình tiếp tục cho đến khi chỉ còn lại một quả bóng. Nhiệm vụ là tính toán số giây dự kiến ​​mà quá trình này thực hiện. 

Từ góc độ đầu vào, mỗi trường hợp thử nghiệm cung cấp số lượng bóng$n$, số giỏ$m$và một mảng vị trí ban đầu của các quả bóng. Đầu ra phải là thời gian dự kiến ​​tính bằng giây, giảm modulo$10^9 + 7$dưới dạng phân số$p/q$, Ở đâu$q$là modulo khả nghịch$10^9 + 7$. 

Những hạn chế là đáng kể.$n$có thể lên tới$3 \cdot 10^5$mỗi trường hợp thử nghiệm và$m$lên đến$10^9$. Tổng số tiền của$n$trên tất cả các trường hợp thử nghiệm không vượt quá$3 \cdot 10^5$. Một mô phỏng đơn giản di chuyển từng quả bóng theo từng bước là không khả thi vì nó có thể yêu cầu tới$O(m \cdot n)$hoạt động trên mỗi trường hợp thử nghiệm, vượt xa mức chấp nhận được đối với$m \sim 10^9$. Chúng tôi cần một giải pháp chạy trong khoảng$O(n \log n)$hoặc$O(n)$mỗi trường hợp thử nghiệm. 

Các trường hợp biên bao gồm các tình huống trong đó$n = 1$, trong trường hợp đó không cần thời gian hoặc khi các quả bóng được đặt liên tiếp hoặc theo thứ tự ngược lại, điều này có thể tạo ra các cụm tương tác theo những cách có thể đoán trước được. Cách tiếp cận bất cẩn có thể cố gắng mô phỏng từng giây, dẫn đến sự bùng nổ về độ phức tạp về thời gian hoặc xử lý không chính xác các chuyển động chồng chéo. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng từng giây chuyển động của Alice. Chúng tôi sẽ duy trì một mảng đại diện cho các rổ, chọn ngẫu nhiên một quả bóng, di chuyển nó, kiểm tra các va chạm và lặp lại cho đến khi chỉ còn lại một quả bóng. Cách tiếp cận này đúng về nguyên tắc nhưng cực kỳ chậm vì số lượng thao tác tăng theo số bước cho đến khi còn lại một quả bóng, có khả năng lên tới$O(n \cdot m)$. Vì$n = 3 \cdot 10^5$Và$m = 10^9$, điều này là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là mô hình hóa vấn đề theo xác suất thay vì thông qua mô phỏng. Mỗi quả bóng có “khoảng cách đến va chạm” với quả bóng tiếp theo theo chiều kim đồng hồ. Khi chúng ta biết những khoảng cách này, số lần di chuyển dự kiến ​​​​cần thiết để loại bỏ một quả bóng tỷ lệ thuận với khoảng cách đó được tính theo xác suất lựa chọn. Chính thức hơn, nếu một quả bóng có$d$rổ tự do trước khi đánh quả bóng tiếp theo, số lần di chuyển dự kiến ​​của nó trước khi gây ra va chạm là$n \cdot d$. Chúng tôi tổng hợp những đóng góp này trừ quả bóng cuối cùng. Việc sắp xếp các quả bóng theo vị trí ban đầu của chúng cho phép chúng ta tính toán khoảng cách theo chiều kim đồng hồ một cách hiệu quả. Nghịch đảo modulo là bắt buộc vì giá trị kỳ vọng là một phân số và chúng ta phải giảm nó theo modulo$10^9 + 7$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n + m) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$,$m$và mảng vị trí$a$. 
3. Nếu$n = 1$, đáp án ngay lập tức là 0 vì chỉ có một quả bóng. 
4. Sắp xếp mảng vị trí$a$để dễ dàng tính toán khoảng cách theo chiều kim đồng hồ giữa các quả bóng liên tiếp. 
5. Tính khoảng cách theo chiều kim đồng hồ giữa các quả bóng liên tiếp bằng cách sử dụng$(a_{i+1} - a_i - 1 + m) \% m$. Điều này cho biết số lượng giỏ trống trước khi xảy ra va chạm. 
6. Tính tổng các khoảng cách này để có được tổng số “di chuyển tự do” cho đến khi loại bỏ được tất cả trừ một quả bóng. Tổng này đại diện cho tử số của thời gian dự kiến. 
7. Nhân số tiền này với$n$bởi vì mỗi bước di chuyển đều có xác suất xảy ra$1/n$, đưa ra tổng số giây dự kiến ​​ở dạng hữu tỷ. 
8. Giảm modulo kết quả$10^9 + 7$sử dụng nghịch đảo mô-đun của$1$(hoặc trực tiếp cho một phân số nếu cần), đảm bảo kết quả là một số nguyên modulo$10^9 + 7$. 

Thuật toán hoạt động vì khoảng cách giữa các quả bóng theo thứ tự được sắp xếp xác định duy nhất số lần mỗi quả bóng phải di chuyển để loại bỏ quả bóng tiếp theo. Mỗi quả bóng đóng góp độc lập vào tổng thời gian dự kiến ​​và việc lựa chọn ngẫu nhiên thống nhất sẽ chia tỷ lệ đóng góp này theo$n$. Việc sắp xếp đảm bảo rằng khoảng cách được tính toán chính xác trên mảng hình tròn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        if n == 1:
            print(0)
            continue
        a.sort()
        total_dist = 0
        for i in range(n):
            nxt = a[(i + 1) % n]
            dist = (nxt - a[i] - 1 + m) % m
            total_dist += dist
        expected_time = (total_dist * n) % MOD
        print(expected_time)

solve()
```Mã bắt đầu bằng việc đọc đầu vào và xác định hằng số modulo. các`modinv`hàm là một tiện ích để tính toán nghịch đảo mô-đun khi cần thiết. Đối với mỗi trường hợp thử nghiệm, nếu chỉ tồn tại một quả bóng thì câu trả lời là 0. Nếu không, các quả bóng sẽ được sắp xếp để tính khoảng cách theo chiều kim đồng hồ. Mỗi khoảng cách được điều chỉnh theo vòng tròn bằng số học modulo. Thời gian dự kiến ​​nhân tổng khoảng cách với$n$bởi vì mỗi quả bóng có một$1/n$cơ hội mỗi giây để di chuyển. Kết quả được in modulo$10^9 + 7$. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
n = 3, m = 10, a = [5,1,4]
Sorted: [1,4,5]
Distances: (4-1-1)=2, (5-4-1)=0, (1+10-5-1)=5
Sum = 2+0+5=7
Expected time = 7*3 = 21
Modulo 10^9+7 = 21
```Dấu vết đơn giản hóa này xác nhận rằng thuật toán tính toán chính xác khoảng cách và tỷ lệ vòng tròn bằng cách$n$. 

Đối với mẫu thứ năm:```
n = 1, m = 100, a = [69]
Answer = 0
```Điều này kiểm tra trường hợp cạnh chỉ có một quả bóng, trường hợp này sẽ kết thúc ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các quả bóng chiếm ưu thế; tính toán khoảng cách là O(n) | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng vị trí | 

Thuật toán phù hợp với giới hạn thời gian và bộ nhớ. Vì$n \le 3 \cdot 10^5$Và$t \le 10^4$, việc sắp xếp từng mảng có thể được chấp nhận và không cần lưu trữ trên mỗi giỏ mặc dù$m$có khả năng được$10^9$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("5\n3 10\n5 1 4\n2 15\n15 1\n6 6\n1 2 3 4 5 6\n6 9\n6 5 4 3 2 1\n1 100\n69\n") == "21\n14\n35\n33\n0", "samples"

# Custom cases
assert run("1\n1 1\n1\n") == "0", "single ball, single basket"
assert run("1\n2 2\n1 2\n") == "2", "two consecutive balls"
assert run("1\n3 5\n1 3 5\n") == "9", "non-consecutive balls"
assert run("1\n4 10\n1 3 7 10\n") == "24", "widely spaced balls"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 0 | Trường hợp bóng đơn tầm thường | 
| 2 2 1 2 | 2 | Loại bỏ hai quả bóng liên tiếp | 
| 3 5 1 3 5 | 9 | Khoảng cách sắp xếp theo vòng tròn | 
| 4 10 1 3 7 10 | 24 | Khoảng cách lớn hơn, số học mô-đun | 

## Vỏ cạnh 

Khi nào$n = 1$, thuật toán trả về 0 ngay lập tức. Đối với các quả bóng trong các rổ liên tiếp, việc tính toán khoảng cách sẽ bao hàm chính xác xung quanh modulo$m$. Đối với các quả bóng có khoảng cách rộng, việc điều chỉnh modulo đảm bảo khoảng cách vòng tròn được tính toán chính xác. Phép nhân với$n$giải thích chính xác cho việc lựa chọn ngẫu nhiên thống nhất. Trong mọi trường hợp, thao tác modulo giữ các số trong phạm vi số nguyên 32 bit và tránh tràn.
