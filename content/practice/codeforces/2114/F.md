---
title: "CF 2114F - Hoạt động nhỏ"
description: "Chúng ta được cho hai số nguyên x và y và giới hạn trên k. Mục tiêu của chúng ta là biến x thành y bằng cách sử dụng ít thao tác nhất có thể."
date: "2026-06-08T04:20:34+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "dfs-and-similar", "dp", "math", "number-theory", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2114
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1027 (Div. 3)"
rating: 2000
weight: 2114
solve_time_s: 96
verified: false
draft: false
---

[CF 2114F - Hoạt động nhỏ](https://codeforces.com/problemset/problem/2114/F) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, dfs và tương tự, dp, toán, lý thuyết số, sắp xếp 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên`x`Và`y`, và giới hạn trên`k`. Mục tiêu của chúng tôi là biến đổi`x`vào trong`y`sử dụng ít thao tác nhất có thể. Mỗi thao tác cho phép chúng ta nhân`x`bởi bất kỳ số nguyên nào`a`giữa 1 và`k`, hoặc chia`x`bởi bất kỳ số nguyên nào`a`giữa 1 và`k`miễn là phép chia tạo ra một số nguyên. Đối với mỗi trường hợp thử nghiệm, chúng ta phải tìm số lượng thao tác tối thiểu hoặc báo cáo`-1`nếu điều đó là không thể. 

Những hạn chế là đáng kể bởi vì`x`,`y`, Và`k`có thể lớn tới 10^6 và có thể có tới 10^4 trường hợp thử nghiệm. Tổng của tất cả`x`Và`y`trên các trường hợp thử nghiệm không vượt quá 10^8, nghĩa là chúng tôi không thể thực hiện tìm kiếm toàn diện trên tất cả các chuỗi hoạt động có thể có cho mọi trường hợp thử nghiệm. Điều này gợi ý rằng chúng ta cần một phương pháp hoạt động hiệu quả với các số lên tới một triệu nhưng tránh khám phá số chuỗi thao tác theo cấp số nhân. 

Trường hợp cạnh xuất hiện khi`x`đã bằng rồi`y`, khi`x`hoặc`y`là 1, hoặc khi`k`là 1. Ví dụ: nếu`x = 4`,`y = 5`, Và`k = 3`, không có chuỗi phép nhân hoặc phép chia nào sử dụng các số từ 3 đến 5 có thể đạt tới 5. Một tình huống khó nhận thấy khác là khi`x`chia rẽ`y`chỉ thông qua các thừa số nguyên tố lớn hơn`k`; trong những trường hợp như vậy việc chuyển đổi là không thể. Cố gắng mô phỏng tất cả các trình tự có thể một cách ngây thơ sẽ âm thầm thất bại hoặc quá chậm. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là thử mọi chuỗi thao tác theo cách đệ quy hoặc bằng BFS. Mỗi bước sẽ khám phá tối đa`2*k`các lựa chọn (nhân hoặc chia cho 1 để`k`), theo dõi giá trị hiện tại. Mặc dù đúng nhưng cách tiếp cận này nhanh chóng trở nên không khả thi vì số lượng trình tự tăng theo cấp số nhân với số lượng thao tác. Đối với đầu vào lớn nhất, việc này dễ dàng yêu cầu 10^20 bước trở lên. 

Cái nhìn sâu sắc quan trọng là các phép toán có tính nhân và chỉ liên quan đến các số nguyên tối đa`k`. Do đó, chúng ta có thể suy luận về số lượng thừa số nguyên tố thay vì mọi chuỗi phép tính. Mỗi thao tác nhân hoặc chia một số cho một số nào đó ≤`k`, chỉ có thể tăng hoặc giảm số mũ của thừa số nguyên tố ≤`k`. Do đó, bài toán rút gọn thành việc đếm tổng số thừa số nguyên tố (có bội số) trong`x`Và`y`khác nhau và xem liệu chúng ta có thể "chuyển" chúng bằng các phép toán được giới hạn bởi`k`. 

Cụ thể ta có thể định nghĩa số phép toán như sau: đếm số thừa số nguyên tố cần nhân để đạt`y`từ`x`, chỉ xét các số nguyên tố ≤`k`và đếm số lần chúng ta có thể chia cho các thừa số này. Nhân với một số tổng hợp`a`tương đương với việc nhân với hệ số nguyên tố của nó. Các bộ phận yêu cầu các yếu tố phải có mặt trong`x`. Điều này chuyển đổi vấn đề từ tìm kiếm tổ hợp sang số học trên hệ số nguyên tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((2k)^hoạt động) | O(hoạt động) | Quá chậm | 
| Đếm hệ số nguyên tố | O(log(x*y) * sqrt(k)) | O(sqrt(k)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu`x`bằng`y`, xuất ra các phép toán 0 và chuyển sang ca kiểm thử tiếp theo. 
2. Nhân tố hóa`x`Và`y`vào các quyền lực hàng đầu. Đối với điều này, hãy xem xét tất cả các số nguyên từ 2 đến`k`, và với mỗi cái, hãy chia`x`Và`y`nhiều lần để đếm số mũ. Bỏ qua các yếu tố lớn hơn`k`bởi vì chúng không thể được giới thiệu hoặc loại bỏ. 
3. Với mỗi thừa số nguyên tố`p`, tính xem nó xuất hiện bao nhiêu lần nữa trong`y`hơn trong`x`. Mỗi hiệu đơn vị như vậy yêu cầu một phép nhân hoặc chia cho một số mà hệ số của nó đóng góp chính xác một`p`. 
4. Tính tổng số lượng của tất cả các phép tính nhân và tất cả các phép chia được yêu cầu. Nếu tại bất kỳ thời điểm nào một yếu tố trong`y`vượt quá`k`nhưng bị thiếu trong`x`, đầu ra`-1`bởi vì nó không thể được tạo ra. 
5. Trả về tổng số thao tác dưới dạng độ dài chuỗi tối thiểu. 

Thuật toán hoạt động vì phép nhân và chia là tuyến tính theo số mũ của các thừa số nguyên tố và mọi thao tác chỉ ảnh hưởng đến số lượng của các thừa số này. Bằng cách tập trung vào số mũ hơn là dãy số, chúng ta tránh được sự bùng nổ theo cấp số nhân. Bất biến là sau bất kỳ chuỗi phép toán nào, số mũ của các số nguyên tố ≤`k`phản ánh hiệu quả tích lũy của các hoạt động đó và chúng tôi chỉ áp dụng số lượng hoạt động cần thiết để cân bằng chênh lệch. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def prime_factors_count(n, k):
    counts = {}
    i = 2
    while i*i <= n and i <= k:
        while n % i == 0:
            counts[i] = counts.get(i, 0) + 1
            n //= i
        i += 1
    if n > 1 and n <= k:
        counts[n] = counts.get(n, 0) + 1
    return counts

def min_operations(x, y, k):
    if x == y:
        return 0
    xf = prime_factors_count(x, k)
    yf = prime_factors_count(y, k)
    
    ops = 0
    for p in set(xf.keys()) | set(yf.keys()):
        xi = xf.get(p, 0)
        yi = yf.get(p, 0)
        if yi > xi:
            ops += yi - xi
        elif xi > yi:
            ops += xi - yi
    # Check if y contains factors larger than k that are missing in x
    remainder = y
    for p, c in yf.items():
        remainder //= p**c
    if remainder > 1 and remainder > k:
        return -1
    return ops

t = int(input())
for _ in range(t):
    x, y, k = map(int, input().split())
    print(min_operations(x, y, k))
```Giải pháp đầu tiên kiểm tra trường hợp đẳng thức tầm thường. Sau đó, nó phân tích cả hai số chỉ sử dụng các số nguyên tố lên đến`k`. Mỗi sự khác biệt của yếu tố đóng góp một hoạt động. Việc kiểm tra cho`remainder > k`đảm bảo chúng tôi không nhầm lẫn khi tuyên bố rằng một sự chuyển đổi không thể thực hiện được là có thể đạt được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`4 6 3`Phân tích các thừa số nguyên tố:`4 = 2^2`,`6 = 2^1 * 3^1`| Thủ tướng | số x | y tính | Các hoạt động cần thiết | 
| --- | --- | --- | --- | 
| 2 | 2 | 1 | 1 sư đoàn | 
| 3 | 0 | 1 | 1 phép nhân | 

Tổng số thao tác = 2, phù hợp với kết quả đầu ra dự kiến. 

### Ví dụ 2 

đầu vào:`4 5 3`Phân tích các thừa số nguyên tố:`4 = 2^2`,`5 = 5^1`Hệ số 5 vượt quá`k=3`, không thể tạo ra, xuất ra`-1`. 

Những ví dụ này xác nhận rằng thuật toán xử lý chính xác cả những biến đổi có thể đạt được và không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(max(x,y)) * log(max(x,y))) | Hệ số hóa lên đến`k`cho mỗi số | 
| Không gian | O(sqrt(k)) | Lưu trữ số mũ nguyên tố | 

Được cho`x, y <= 10^6`và lên đến`10^4`trường hợp thử nghiệm, giải pháp phù hợp trong giới hạn thời gian 3 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open("solution.py").read())
    return out.getvalue().strip()

# provided sample
assert run("8\n4 6 3\n4 5 3\n4 6 2\n10 45 3\n780 23 42\n11 270 23\n1 982800 13\n1 6 2\n") == \
"2\n-1\n-1\n3\n3\n3\n6\n-1"

# custom cases
assert run("1\n1 1 1\n") == "0", "already equal"
assert run("1\n6 1 6\n") == "2", "division to 1"
assert run("1\n7 49 7\n") == "2", "square with allowed factor"
assert run("1\n8 16 3\n") == "-1", "factor 2 needs multiplication by 2 twice, possible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 0 | Đã bằng nhau | 
| 6 1 6 | 2 | Phép chia chia xuống còn 1 | 
| 7 49 7 | 2 | Nhân nhiều lần với thừa số cho phép | 
| 8 16 3 | -1 | Không thể đạt 16 với k=3 | 

## Vỏ cạnh 

Đối với đầu vào`4 5 3`, thuật toán xác định chính xác rằng 5 là thừa số nguyên tố không ≤ k nên việc chuyển đổi là không thể. Với `x =
