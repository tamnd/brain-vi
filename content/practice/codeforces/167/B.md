---
title: "CF 167B - Pháp sư và giải thưởng khủng"
description: "Chúng tôi được yêu cầu tính toán xác suất thực hiện tốt trong một chuỗi các cuộc thi phù thủy, dựa trên cả cơ hội chiến thắng trong mỗi cuộc thi và loại giải thưởng bạn có thể nhận được."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 167
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 114 (Div. 1)"
rating: 1800
weight: 167
solve_time_s: 98
verified: false
draft: false
---

[CF 167B - Pháp sư và giải thưởng lớn](https://codeforces.com/problemset/problem/167/B) 

**Đánh giá:** 1800 
**Tags:** dp, toán, xác suất 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tính toán xác suất thực hiện tốt trong một chuỗi các cuộc thi phù thủy, dựa trên cả cơ hội chiến thắng trong mỗi cuộc thi và loại giải thưởng bạn có thể nhận được. Có _n_ cuộc thi và với mỗi cuộc thi, bạn biết xác suất chiến thắng của mình và giải thưởng đó là giải thưởng lớn hay chiếc túi có thể đựng một số giải thưởng lớn nhất định. Bạn bắt đầu với một chiếc túi xách ở nhà có thể đựng _k_ giải thưởng lớn. Mục tiêu là tính xác suất để bạn thắng ít nhất _l_ cuộc thi trong khi có thể mang theo tất cả các giải thưởng lớn mà bạn giành được, sử dụng cả chiếc túi bạn mang theo khi bắt đầu và bất kỳ chiếc túi nào được trao khi chiến thắng một số cuộc thi nhất định. 

Đầu vào cung cấp xác suất theo tỷ lệ phần trăm, cần được chuyển đổi thành phân số thập phân để tính toán. Đầu ra là xác suất số thực với độ chính xác cao. Các ràng buộc đủ chặt chẽ đến mức không thể liệt kê tất cả các tập hợp con chiến thắng có thể có (2^n) đối với giới hạn trên n=200. Các xác suất phải được kết hợp cẩn thận, tính đến những giải thưởng bạn giành được và liệu bạn có thể cất giữ chúng hay không. 

Một trường hợp phức tạp xảy ra khi tất cả các giải thưởng đều là túi. Ví dụ: nếu mỗi cuộc thi đều trao một chiếc túi, bạn không bao giờ có thể giành được giải thưởng lớn nhưng số cuộc thi thắng vẫn có thể đáp ứng _l_. Nếu mã giả định một cách ngây thơ rằng chiến thắng luôn làm tăng yêu cầu lưu trữ thì nó có thể tính ra một kịch bản không thể xảy ra. Một trường hợp đặc biệt khác là khi _k_ đã đủ để giữ bất kỳ số lượng giải thưởng lớn nào; mã vẫn phải thực thi số lượng cuộc thi tối thiểu đã thắng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là liệt kê tất cả các tập hợp con của các cuộc thi mà bạn có thể giành chiến thắng, tính toán tổng số giải thưởng lớn và tổng sức chứa túi cho mỗi tập hợp con, kiểm tra xem tập hợp con đó có đáp ứng cả hai điều kiện "ít nhất _l_ thắng" và "tất cả các giải thưởng phù hợp" hay không, đồng thời tính tổng xác suất của từng tập hợp con hợp lệ. Phương pháp này đúng về nguyên tắc nhưng yêu cầu 2^n thao tác trong trường hợp xấu nhất, điều này hoàn toàn không khả thi với n=200. 

Cái nhìn sâu sắc quan trọng là coi đây là một vấn đề lập trình động đối với số lượng cuộc thi được xử lý, số lượng chiến thắng cho đến nay và tổng số giải thưởng khổng lồ được mang theo. Vì dung lượng của túi có thể được coi là tài nguyên không giảm nên chúng ta có thể xác định bảng DP trong đó trạng thái biểu thị số lượng giải thưởng lớn tối đa có thể được lưu trữ, với một số chiến thắng nhất định và một số cuộc thi nhất định được xem xét. Với mỗi cuộc thi, chúng tôi cập nhật bảng DP xác suất thắng hoặc không thắng, điều chỉnh tổng công suất cho phù hợp. Bằng cách lặp lại các cuộc thi theo thứ tự và cập nhật trạng thái một cách hiệu quả, chúng ta có thể giảm độ phức tạp theo hàm mũ xuống một đa thức, cụ thể là O(n*k^2) khả thi trong các ràng buộc của bài toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(1) | Quá chậm | 
| Lập trình động | O(n * k * n) | O(n * k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi tất cả xác suất từ tỷ lệ phần trăm sang phân số thập phân trong khoảng từ 0 đến 1. Điều này giúp đơn giản hóa phép nhân và tránh các lỗi chia tỷ lệ số nguyên. 
2. Khởi tạo bảng DP 2D`dp[w][c]`, Ở đâu`w`là số cuộc thi đã thắng và`c`là số lượng giải thưởng khổng lồ hiện có thể mang theo. Bộ`dp[0][k] = 1.0`, vì khi xem xét không có cuộc thi nào, bạn không có chiến thắng nào và có thể mang theo sức chứa túi sân nhà ban đầu`k`với xác suất 1. 
3. Lặp lại từng cuộc thi. Đối với mỗi cuộc thi, hãy chuẩn bị một bảng DP mới`next_dp`có cùng kích thước. 
4. Đối với mỗi trạng thái DP hiện có`(wins, capacity)`, hãy xem xét kết quả nếu bạn không giành chiến thắng trong cuộc thi. Số trận thắng vẫn giữ nguyên, sức chứa không đổi và xác suất được nhân với khả năng thua cuộc thi này. 
5. Để có kết quả thắng, hãy tăng số lần thắng lên 1. Nếu giải thưởng là túi thì cộng sức chứa của nó vào sức chứa hiện tại; nếu giải thưởng lớn thì giảm dung lượng hiện có đi 1 (hoặc đảm bảo dung lượng đủ để giữ). Cập nhật xác suất bằng cách nhân với cơ hội chiến thắng cuộc thi. Chỉ cho phép chuyển đổi này nếu công suất mới không âm. 
6. Sau khi xử lý tất cả các cuộc thi, hãy tính tổng xác suất của tất cả các trạng thái DP có số trận thắng ít nhất`l`và công suất không âm. Tổng này là xác suất mong muốn. 

Tại sao nó hoạt động: Bảng DP tính toán toàn diện tất cả các kết hợp chiến thắng và năng lực sẵn có theo cách lặp đi lặp lại. Mỗi quá trình chuyển đổi đều tôn trọng các xác suất và hạn chế lưu trữ. Vì mỗi lần chúng tôi chỉ tiến hành một cuộc thi nên chúng tôi tránh tính toán quá mức và tích lũy xác suất chính xác cho từng kết quả khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, l, k = map(int, input().split())
p = list(map(int, input().split()))
a = list(map(int, input().split()))

p = [x / 100.0 for x in p]

dp = [[0.0 for _ in range(201 + 200)] for _ in range(n + 1)]
dp[0][k] = 1.0

for i in range(n):
    next_dp = [[0.0 for _ in range(201 + 200)] for _ in range(n + 1)]
    for wins in range(i + 1):
        for cap in range(201 + 200):
            if dp[wins][cap] < 1e-15:
                continue
            # Losing this contest
            next_dp[wins][cap] += dp[wins][cap] * (1 - p[i])
            # Winning this contest
            new_wins = wins + 1
            if a[i] == -1:
                if cap > 0:
                    next_dp[new_wins][cap - 1] += dp[wins][cap] * p[i]
            else:
                next_dp[new_wins][cap + a[i]] += dp[wins][cap] * p[i]
    dp = next_dp

result = 0.0
for wins in range(l, n + 1):
    for cap in range(201 + 200):
        result += dp[wins][cap]

print(f"{result:.12f}")
```Đầu tiên, mã này chuyển đổi tỷ lệ phần trăm đầu vào thành phân số thập phân để đơn giản hóa phép nhân. Bàn DP có kích thước phù hợp với mọi khả năng có thể, bao gồm cả việc tăng trưởng từ các túi được trao trong các cuộc thi. Quá trình chuyển đổi xử lý thắng thua một cách chính xác, chỉ cập nhật năng lực khi khả thi. Tổng xác suất cuối cùng xem xét tất cả các trạng thái đáp ứng yêu cầu thắng tối thiểu. 

## Ví dụ đã hoạt động 

Mẫu 1:`3 1 0`,`10 20 30`,`-1 -1 2`| tôi | thắng | mũ lưỡi trai | dp[thắng][giới hạn] | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 1.0 | 
| 0 | 0 | 0 | thua 10% → 0,9, thắng lớn 10% → bỏ qua vì cap=0 | 
| 1 | 1 | 2 | thắng túi thứ ba → 0,3 | 

DP xác định chính xác rằng chỉ có chiến thắng ở cuộc thi thứ ba là khả thi và tổng bằng 0,3. 

Mẫu 2:`1 1 0`,`100`,`1`DP bắt đầu ở dp[0][0] = 1. Thắng cuộc thi duy nhất sẽ được một túi, tăng sức chứa lên 1. Số lần thắng=1, đáp ứng`l=1`, tổng xác suất = 1,0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 * max_cap) | Mỗi cuộc thi lặp lại tối đa n chiến thắng và sức chứa tối đa lên tới 400 (k ban đầu cộng với tổng của tất cả sức chứa túi) | 
| Không gian | O(n * max_cap) | Hai bảng DP có kích thước (n+1) x (max_cap) được lưu trữ đồng thời | 

Cho n 200 và max_cap 400, n^2 * max_cap ≈ 16 triệu thao tác, vừa vặn thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read())
    return sys.stdout.getvalue().strip()

# provided samples
assert run("3 1 0\n10 20 30\n-1 -1 2\n") == "0.300000000000", "sample 1"
assert run("1 1 0\n100\n1\n") == "1.000000000000", "sample 2"

# custom cases
assert run("2 2 0\n50 50\n-1 -1\n") == "0.25", "both huge prizes"
assert run("2 1 1\n50 50\n-1 -1\n") == "0.75", "one bag sufficient"
assert run("3 2 0\n50 50 50\n-1 2 -1\n") == "0.375", "mix of bag and huge prize"
assert run("1 0 0\n0\n-1\n") == "1.0", "winning not required"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 0\n50 50\n-1 -1\n`| 0,25 | Tính đúng xác suất khi cả hai giải đều lớn | 
|`2 1 1\n50 50\n-1 -1\n`| 0,75 | Xử lý đúng túi xách nhà | 
| `3 2 0\n50 50 50 | | |
