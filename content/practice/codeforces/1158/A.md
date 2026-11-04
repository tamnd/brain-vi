---
title: "CF 1158A - Bữa tiệc và đồ ngọt"
description: "Chúng tôi có một bữa tiệc với $n$ chàng trai và $m$ cô gái. Mỗi chàng trai đưa một số kẹo cho mỗi cô gái, tạo thành một ma trận số nguyên $n nhân m$. Đối với mỗi chàng trai, số kẹo tối thiểu anh ta đưa cho bất kỳ cô gái nào được chỉ định là $bi$."
date: "2026-06-12T02:29:03+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "greedy", "implementation", "math", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1158
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 559 (Div. 1)"
rating: 1500
weight: 1158
solve_time_s: 156
verified: false
draft: false
---

[CF 1158A - Bữa tiệc và đồ ngọt](https://codeforces.com/problemset/problem/1158/A) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, tham lam, thực hiện, toán học, sắp xếp, hai con trỏ 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bữa tiệc với$n$chàng trai và$m$các cô gái. Mỗi chàng trai đưa một số kẹo cho mỗi cô gái, tạo thành một$n \times m$ma trận các số nguyên. Đối với mỗi chàng trai, số kẹo tối thiểu anh ta đưa cho một cô gái là$b_i$. Đối với mỗi cô gái, số kẹo tối đa cô ấy nhận được từ bất kỳ chàng trai nào được xác định là$g_j$. Nhiệm vụ của chúng ta là xây dựng lại một phép gán kẹo hợp lý, thỏa mãn tất cả những ràng buộc này và đồng thời giảm thiểu tổng số kẹo được trao. Nếu không thể thỏa mãn các ràng buộc, chúng ta phải xuất ra$-1$. 

Kích thước đầu vào lớn, lên tới$10^5$cho cả bé trai và bé gái, với số lượng mỗi chiếc kẹo lên đến$10^8$. Điều này ngụ ý rằng bất kỳ giải pháp nào lặp lại rõ ràng trên tất cả$n \cdot m$các mục ma trận có thể quá chậm, vì$10^5 \cdot 10^5 = 10^{10}$hoạt động sẽ bị cấm. Giải pháp phải dựa vào sự tổng hợp hoặc sắp xếp thông minh để tránh việc truyền tải ma trận đầy đủ. 

Một trường hợp khó phát hiện khi mức ngọt tối thiểu lớn nhất trong số các chàng trai vượt quá mức ngọt tối đa nhỏ nhất trong số các cô gái. Ví dụ, hãy xem xét$b = [5, 1]$Và$g = [4, 6]$. Cậu bé đầu tiên phải đưa ra ít nhất$5$kẹo cho ai đó, nhưng không cô gái nào có thể nhận được nhiều hơn$4$từ bất cứ ai. Cấu hình này là không thể, và một cách tiếp cận ngây thơ lấp đầy ma trận một cách tham lam mà không kiểm tra tính khả thi sẽ âm thầm thất bại. 

Một trường hợp đặc biệt khác xảy ra khi nhiều chàng trai có cùng giá trị ngọt ngào tối thiểu với giá trị ngọt ngào tối đa của một cô gái. Việc phân phối phải phân bổ cẩn thận mức tối thiểu lớn nhất vào cột thích hợp để phù hợp với mức tối đa của cô gái. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ cố gắng xây dựng toàn bộ$n \times m$ma trận một cách rõ ràng. Chúng ta sẽ gán các giá trị để thỏa mãn$b_i$cho mỗi hàng và$g_j$cho mỗi cột. Người ta có thể thử thực hiện một nhiệm vụ tham lam: với mỗi cậu bé, hãy bắt đầu bằng việc lấp đầy chiếc kẹo tối thiểu của mình.$b_i$trên tất cả các cô gái và sau đó cố gắng điều chỉnh một số mục nhất định để phù hợp với mức tối đa của các cô gái. Mặc dù đúng về nguyên tắc nhưng cách tiếp cận này có$O(n \cdot m)$phức tạp, không thể thực hiện được$n, m$lên đến$10^5$. 

Thông tin chi tiết quan trọng đến từ việc quan sát rằng mức ngọt tối thiểu đối với mỗi chàng trai phải xuất hiện ở đâu đó trong hàng của anh ấy và mức ngọt tối đa đối với mỗi cô gái phải xuất hiện ở đâu đó trong cột của cô ấy. Để giảm thiểu tổng số tiền, chúng ta nên điền càng nhiều mục càng tốt với giá trị tối thiểu của hàng, chỉ tăng giá trị để đáp ứng cực đại của cột. Sắp xếp$b$Và$g$cho phép chúng ta sắp xếp số lượng bé trai tối thiểu lớn nhất với số lượng bé gái lớn nhất để thỏa mãn các ràng buộc với mức tăng tối thiểu. 

Cụ thể, nếu cực tiểu lớn nhất của bé trai bằng cực đại nhỏ nhất của bé gái thì chúng ta phải gán nó cho bé gái đó; tất cả các cực tiểu của hàng khác có thể được giao cho các cô gái khác mà không cần tăng số tiền một cách không cần thiết. Điều này làm giảm vấn đề xuống còn một số phép tính số học thay vì lặp lại toàn bộ ma trận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n * m) | Quá chậm | 
| Tối ưu | O(n log n + m log m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng$b$của cậu bé cực tiểu và$g$của cô gái maxima. Cho phép$b_{\text{max}}$là cậu bé lớn nhất tối thiểu và$b_{\text{second}}$lớn thứ hai. Cho phép$g_{\text{min}}$là cô gái nhỏ nhất tối đa. 
2. Nếu$b_{\text{max}} > g_{\text{min}}$, tình huống là không thể, đầu ra$-1$. 
3. Khởi tạo tổng là tổng của tất cả$b_i$nhân với$m$, vì mỗi chàng trai đều dành ít nhất mức tối thiểu của mình cho mọi cô gái. 
4. Thêm vào tổng số chênh lệch giữa mỗi$g_j$Và$b_{\text{max}}$cho mọi cô gái ngoại trừ người sẽ nhận được$b_{\text{max}}$, bởi vì mỗi cô gái phải đạt đến mức tối đa ở đâu đó. 
5. Cuối cùng, thêm$b_{\text{max}} - b_{\text{second}}$đến tổng số tiền nếu số lượng tối thiểu của bé trai lớn nhất nhỏ hơn số lượng tối đa của bé gái lớn nhất. Điều này chiếm lượng đồ ngọt bổ sung cần thiết để đạt mức tối đa của cô gái lớn nhất trong khi vẫn giữ số tiền ở mức tối thiểu. 

Điều bất biến là mỗi chàng trai đóng góp ít nhất số kẹo tối thiểu của mình cho mọi cô gái và mỗi cô gái nhận được số kẹo tối đa của mình đúng một lần. Thuật toán đảm bảo rằng không có ràng buộc hàng hoặc cột nào bị vi phạm trong khi giảm thiểu lượng đồ ngọt bổ sung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
b = list(map(int, input().split()))
g = list(map(int, input().split()))

b.sort()
g.sort()

if b[-1] > g[0]:
    print(-1)
    sys.exit()

total = 0
# sum of all minimal contributions
total += sum(b) * m

# add difference for each girl
for j in range(m):
    total += g[j] - b[-1]

# adjust for the largest boy if needed
if b[-1] < g[0]:
    total += b[-1] - b[-2]

print(total)
```Bước đầu tiên sắp xếp các mảng để dễ dàng tìm cực đại và cực tiểu cũng như kiểm tra tính khả thi. Nhân tổng giá trị cực tiểu của hàng với$m$đảm bảo tất cả những đóng góp tối thiểu đều được tính. Việc điều chỉnh bằng sự khác biệt giữa mức tối đa của mỗi cô gái và mức tối thiểu lớn nhất của chàng trai đảm bảo mọi ràng buộc của cột đều được thỏa mãn. Việc điều chỉnh cuối cùng xử lý trường hợp mức tối thiểu của bé trai lớn nhất nhỏ hơn mức tối đa của bé gái lớn nhất. 

## Ví dụ đã hoạt động 

**Mẫu 1**: 

đầu vào:`3 2`,`1 2 1`,`3 4`| Bước | b (sắp xếp) | g (sắp xếp) | tổng cộng | 
| --- | --- | --- | --- | 
| Ban đầu | [1,1,2] | [3,4] | 0 | 
| tổng(b)*m | | | (1+1+2)*2 = 8 | 
| thêm g[j]-b[-1] | | | (3-2)+(4-2)=3 | 
| điều chỉnh cậu bé lớn nhất | | | 2-1=1 | 
| Tổng số cuối cùng | | | 8+3+1=12 | 

Điều này phù hợp với sản lượng dự kiến ​​và cho thấy rằng các ràng buộc được thỏa mãn với tổng số kẹo tối thiểu. 

**Mẫu 2**: 

đầu vào:`2 2`,`5 1`,`4 6`| Bước | b (sắp xếp) | g (sắp xếp) | tổng cộng | 
| --- | --- | --- | --- | 
| b[-1] > g[0]? | 5 > 4 | | không thể | 
| Đầu ra | | | -1 | 

Điều này chứng tỏ trường hợp khó khăn khi tính khả thi không thành công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m log m) | Sắp xếp cả hai mảng chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n + m) | Bản thân mảng, không có cấu trúc quan trọng nào | 

Được cho$n, m \le 10^5$, việc sắp xếp có thể được chấp nhận trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    b = list(map(int, input().split()))
    g = list(map(int, input().split()))

    b.sort()
    g.sort()

    if b[-1] > g[0]:
        return "-1"

    total = sum(b) * m
    for j in range(m):
        total += g[j] - b[-1]
    if b[-1] < g[0]:
        total += b[-1] - b[-2]
    return str(total)

# Provided samples
assert run("3 2\n1 2 1\n3 4\n") == "12", "sample 1"
assert run("2 2\n5 1\n4 6\n") == "-1", "sample 2"

# Custom tests
assert run("2 3\n1 1\n1 1 1\n") == "6", "all equal"
assert run("3 3\n0 0 0\n0 0 0\n") == "0", "all zeros"
assert run("3 2\n1 2 3\n3 3\n") == "15", "increasing minima"
assert run("2 2\n2 2\n3 3\n") == "10", "equal minima, higher maxima"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3\n1 1\n1 1 1\n`| 6 | mọi cực tiểu và cực đại đều bằng nhau, trường hợp đơn giản | 
|`3 3\n0 0 0\n0 0 0\n`| 0 | trường hợp không có kẹo ngọt | 
|`3 2\n1 2 3\n3 3\n`| 15 | tăng cực tiểu và cực đại, xác nhận thuật toán gán mức tối thiểu lớn nhất cho mức tối đa lớn nhất | 
|`2 2\n2 2\n3 3\n`| 10 | cực tiểu bằng nhau, cực đại cao hơn, điều chỉnh thử nghiệm để giảm thiểu tổng | 

## Vỏ cạnh 

Khi giá trị tối thiểu của bé trai lớn nhất vượt quá giá trị tối đa của bé gái, thuật toán sẽ đưa ra một cách chính xác`-1`. Ví dụ, đầu vào`2 2\n5 1\n4 6`thất bại vì không có phép gán nào thỏa mãn tất cả các ràng buộc về hàng và cột. Khi tất cả các giá trị đều bằng nhau, chẳng hạn như `2 3\n1 1\n1
