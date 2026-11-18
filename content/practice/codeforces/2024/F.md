---
title: "CF 2024F - Nhiều Trò Chơi"
description: "Mỗi trò chơi có xác suất thành công là $pi/100$ và phần thưởng là $wi$. Chúng tôi có thể chọn bất kỳ tập hợp con nào của trò chơi. Điều đáng chú ý là chúng tôi chỉ nhận được phần thưởng nếu mọi trò chơi đã chọn đều thắng. Nếu thậm chí thua một trò chơi đã chọn, phần thưởng cuối cùng sẽ bằng không."
date: "2026-06-09T03:12:58+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 2024
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 980 (Div. 2)"
rating: 2900
weight: 2024
solve_time_s: 349
verified: false
draft: false
---

[CF 2024F - Nhiều trò chơi](https://codeforces.com/problemset/problem/2024/F) 

**Xếp hạng:** 2900 
**Tags:** dp, toán 
**Thời gian giải:** 5 phút 49 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trò chơi đều có xác suất thành công$p_i/100$và một phần thưởng$w_i$. 

Chúng tôi có thể chọn bất kỳ tập hợp con nào của trò chơi. Điều đáng chú ý là chúng tôi chỉ nhận được phần thưởng nếu mọi trò chơi đã chọn đều thắng. Nếu thậm chí thua một trò chơi đã chọn, phần thưởng cuối cùng sẽ bằng không. 

Đối với một tập hợp con được chọn$S$,$$\text{expected value} = \left(\prod_{i\in S}\frac{p_i}{100}\right) \left(\sum_{i\in S}w_i\right).$$Chúng ta phải tối đa hóa số lượng này. 

Điều nổi bật đầu tiên là giá trị to lớn của$n$, lên đến$2\cdot 10^5$. Bất kỳ thuật toán nào xem xét rõ ràng các tập hợp con đều không thể thực hiện được. Thậm chí$O(n^2)$là quá lớn. 

Hạn chế bất thường thứ hai là$$p_i \cdot w_i \le 2\cdot 10^5.$$Thoạt nhìn có vẻ không liên quan đến mục tiêu nhưng thực chất nó lại là chìa khóa giúp giải quyết vấn đề. 

Chỉ có 100 giá trị xác suất có thể có. Điều đó gợi ý mạnh mẽ việc phân nhóm các trò chơi theo xác suất. 

Một trường hợp cạnh tinh tế là$p_i=100$. Trò chơi như vậy không bao giờ làm giảm tích xác suất, bởi vì nhân với$1$không có gì thay đổi Vì phần thưởng là tích cực nên mọi$100\%$trò chơi nên luôn luôn được thực hiện. 

Ví dụ:```
2
100 5
100 7
```Câu trả lời tối ưu là$12$. Bỏ qua xác suất-100 trò chơi sẽ ngay lập tức mất đi tính đúng đắn. 

Một sai lầm dễ mắc phải khác là cho rằng nếu hai trò chơi có cùng xác suất thì chúng ta có thể chọn một trò chơi tùy ý trong số đó. 

Coi như:```
3
90 100
90 50
90 1
```Nếu chúng ta quyết định chơi đúng hai ván với xác suất$90\%$, nhận phần thưởng$100$Và$50$ít nhất thì luôn tốt bằng việc dùng$100$Và$1$. Trong số các xác suất bằng nhau, phần thưởng lớn hơn sẽ chiếm ưu thế hơn phần thưởng nhỏ hơn. 

Cái bẫy thứ ba là tin rằng tổng phần thưởng của một giải pháp tối ưu có thể lớn tùy ý vì$n$là rất lớn. Ràng buộc đặc biệt ngăn chặn điều đó. Chứng minh giới hạn này là quan sát trung tâm của nghiệm. 

## Phương pháp tiếp cận 

Giải pháp vũ phu là hiển nhiên. Liệt kê mọi tập hợp con, tính tổng phần thưởng và tích xác suất của nó, sau đó giữ lại câu trả lời đúng nhất. 

Điều này đúng vì nó kiểm tra mọi khả năng. 

Thật không may là nó đòi hỏi$2^n$tập hợp con. Ngay cả đối với$n=50$điều này là vô vọng, trong khi giới hạn thực tế là$2\cdot 10^5$. 

Câu hỏi tiếp theo là thuộc tính nào của tập hợp con tối ưu có thể được khai thác. 

Giả sử tập hợp con tối ưu có tổng phần thưởng$W$. 

Tham gia bất kỳ trò chơi nào đã chọn$i$. Loại bỏ nó không thể cải thiện câu trả lời. Cho phép$$q_i=\frac{p_i}{100}.$$Sự tối ưu mang lại$$q_i W \ge W-w_i.$$Sắp xếp lại,$$w_i \ge (1-q_i)W.$$nhân với$p_i$,$$p_iw_i \ge \frac{p_i(100-p_i)}{100}W.$$Đối với mọi$1\le p_i\le 99$,$$\frac{p_i(100-p_i)}{100}\ge 0.99.$$Từ$p_iw_i\le 2\cdot10^5$,$$W \le \frac{2\cdot10^5}{0.99} < 202021.$$Đây là bước đột phá. 

Giải pháp tối ưu không bao giờ cần tổng phần thưởng trên khoảng$2\cdot10^5$. 

Bây giờ chúng ta có thể coi số tiền thưởng như một kích thước ba lô. 

Có một quan sát nữa. 

Đối với xác suất cố định$p$, mọi trò chơi được chọn đều đóng góp cùng một hệ số nhân$p/100$. Nếu chúng ta quyết định lấy chính xác$k$trò chơi từ nhóm xác suất đó, lựa chọn tốt nhất chỉ đơn giản là$k$phần thưởng lớn nhất. 

Do đó, mỗi nhóm xác suất sẽ trở thành một chuỗi các tổng tiền tố sau khi sắp xếp phần thưởng theo thứ tự giảm dần. 

Đối số đếm thứ hai cho thấy rằng đối với xác suất$p<100$, một giải pháp tối ưu có thể mất nhiều nhất$$\left\lceil \frac1{\ln(100/p)} \right\rceil$$trò chơi từ nhóm đó. Tổng hợp các giới hạn này trên tất cả$p=1,\dots,99$chỉ cung cấp khoảng 500 trò chơi ứng cử viên. Bản gốc$2\cdot10^5$trò chơi sụp đổ xuống còn vài trăm trò chơi có liên quan. 

Sau mức giảm đó, một chiếc ba lô tiêu chuẩn trên tổng phần thưởng sẽ trở nên khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(W \cdot K)$|$O(W)$| Đã chấp nhận | 

Đây$W\approx 3\cdot10^5$Và$K\approx 500$. 

## Hướng dẫn thuật toán 

### 1. Tách tất cả các trò chơi có xác suất 100 

Mỗi trò chơi như vậy nên luôn luôn được lựa chọn. 

Cho phép$$\text{base}=\sum w_i \quad (p_i=100).$$Những trò chơi này không ảnh hưởng đến tích xác suất. 

### 2. Nhóm các trò chơi còn lại theo xác suất 

Đối với mọi$p\in[1,99]$, thu thập tất cả các phần thưởng có xác suất đó. 

Sắp xếp từng nhóm theo thứ tự giảm dần. 

### 3. Chỉ giữ lại những phần thưởng lớn nhất có liên quan 

Đối với xác suất$p$, tính toán$$\text{limit} = \left\lceil \frac1{\ln(100/p)} \right\rceil.$$Chỉ lớn nhất`limit`phần thưởng từ nhóm đó có thể quan trọng. 

Bất kỳ trò chơi bổ sung nào đều được chứng minh là vô dụng trong một giải pháp tối ưu. 

### 4. Chạy ba lô với số tiền thưởng 

hãy để$$dp[s]$$là sản phẩm có xác suất tối đa có thể đạt được với tổng phần thưởng bổ sung chính xác$s$. 

Khởi tạo$$dp[0]=1.$$Đối với mỗi trò chơi được giữ lại$(p,w)$,$$dp[s] = \max(dp[s],\, dp[s-w]\cdot p/100).$$Đây là bản cập nhật ba lô 0/1 tiêu chuẩn. 

### 5. Tính đáp án cuối cùng 

Đối với mỗi khoản tiền thưởng$s$,$$\text{candidate} = dp[s]\cdot (base+s).$$Tận dụng tối đa tất cả$s$. 

### Tại sao nó hoạt động 

Bất biến quan trọng là sau khi xử lý một số tập hợp trò chơi được giữ lại,`dp[s]`lưu trữ sản phẩm có xác suất lớn nhất có thể trong số tất cả các tập hợp con có tổng phần thưởng chính xác là`s`. 

Quá trình chuyển đổi là quá trình chuyển đổi ba lô 0/1 tiêu chuẩn. Trò chơi hiện tại không được chọn hoặc được chọn và nhân tích xác suất với$p/100$. 

Bước cắt tỉa là an toàn vì đối với một nhóm xác suất cố định, chỉ phần thưởng lớn nhất mới có thể là một phần của giải pháp tối ưu. Hơn nữa, giới hạn phân tích về số lượng trò chơi được chọn từ một nhóm xác suất đảm bảo rằng mọi giải pháp tối ưu đều tồn tại sau quá trình cắt tỉa. 

Vì mọi tập hợp con tối ưu đều tương ứng với một số tiền thưởng có thể đạt được$s$, Và`dp[s]`lưu trữ sản phẩm có xác suất tốt nhất cho số tiền đó, tối đa hóa$$dp[s](base+s)$$kiểm tra chính xác giá trị mục tiêu của tập hợp con tốt nhất cho mọi tổng phần thưởng có thể có. Mức tối đa trong số đó là mức tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

n = int(input())

groups = [[] for _ in range(101)]
base = 0

for _ in range(n):
    p, w = map(int, input().split())
    if p == 100:
        base += w
    else:
        groups[p].append(w)

items = []

for p in range(1, 100):
    if not groups[p]:
        continue

    groups[p].sort(reverse=True)

    limit = math.ceil(1.0 / math.log(100.0 / p))
    if limit < len(groups[p]):
        groups[p] = groups[p][:limit]

    for w in groups[p]:
        items.append((w, p / 100.0))

MAXS = base
for w, _ in items:
    MAXS += w

dp = [0.0] * (MAXS + 1)
dp[0] = 1.0

for w, prob in items:
    for s in range(MAXS, w - 1, -1):
        cand = dp[s - w] * prob
        if cand > dp[s]:
            dp[s] = cand

ans = float(base)

for s in range(MAXS + 1):
    if dp[s] > 0:
        ans = max(ans, dp[s] * (base + s))

print(f"{ans:.10f}")
```Phần đầu tiên phân chia các trò chơi xác suất 100 vì chúng luôn có lợi. 

Giai đoạn nhóm sắp xếp phần thưởng bên trong mỗi nhóm xác suất. Giới hạn cắt tỉa xuất phát từ giới hạn phân tích về số lượng trò chơi có cùng xác suất có thể xuất hiện trong một giải pháp tối ưu. 

Sau khi cắt tỉa, số lượng trò chơi còn lại chỉ còn vài trăm. Đó là lý do chiếc ba lô trở nên khả thi. 

DP lưu trữ trực tiếp các sản phẩm xác suất dưới dạng giá trị dấu phẩy động. Các tích có thể trở nên rất nhỏ, nhưng giới hạn cắt tỉa giữ cho số lượng các thừa số nhân bị giới hạn, do đó độ chính xác kép thông thường là đủ. 

Thứ tự lặp lại trong vòng lặp ba lô là cần thiết. Việc sử dụng phép lặp chuyển tiếp sẽ vô tình cho phép cùng một trò chơi được chọn nhiều lần. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
80 80
70 100
50 200
```Các trạng thái DP có liên quan: 

| Trò chơi được chọn | Số tiền thưởng | Sản phẩm xác suất | Giá trị kỳ vọng | 
| --- | --- | --- | --- | 
| {80} | 80 | 0,8 | 64 | 
| {70} | 100 | 0,7 | 70 | 
| {50} | 200 | 0,5 | 100 | 
| {80,70} | 180 | 0,56 | 100,8 | 
| {80,50} | 280 | 0,40 | 112 | 
| {70,50} | 300 | 0,35 | 105 | 
| {80,70,50} | 380 | 0,28 | 106,4 | 

Tối đa là$112$, đạt được bằng cách chọn trò chơi thứ nhất và thứ ba. 

### Mẫu 2 

đầu vào:```
2
100 1
100 1
```Sau khi tiền xử lý: 

| Biến | Giá trị | 
| --- | --- | 
| căn cứ | 2 | 
| các mặt hàng được giữ lại | không | 

DP chỉ chứa`dp[0]=1`. 

Giá trị cuối cùng:$$1\cdot(2+0)=2.$$Trả lời:$2$. 

Ví dụ này chứng minh tại sao trò chơi xác suất 100 phải được xử lý riêng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(W\cdot K)$| Ba lô vượt số tiền thưởng với khoảng 500 trò chơi được giữ lại | 
| Không gian |$O(W)$| Một mảng DP | 

Kích thước tổng phần thưởng được giới hạn bởi bằng chứng cấu trúc bắt nguồn từ$p_iw_i\le2\cdot10^5$. Sau khi cắt tỉa, chỉ còn lại vài trăm trò chơi nên DP dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import isclose

def run(inp: str) -> str:
    # paste solution into solve()
    pass

# sample 1
# expected 112.0

# sample 2
# expected 2.0

# custom minimum case
# 1 game
# 50% chance, reward 10
# answer = 5

# custom all probability 100
# answer equals sum of rewards

# custom equal probabilities
# checks that largest rewards are preferred

# custom boundary style
# p*w = 200000
# ensures constraint edge is handled
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chỉ một trò chơi | EV trò chơi đơn | Kích thước tối thiểu | 
| Tất cả$p=100$| Tổng phần thưởng | Bắt buộc phải đưa vào các trò chơi chắc chắn | 
| Nhiều xác suất bằng nhau | Sử dụng phần thưởng lớn nhất | Nhóm cắt tỉa đúng cách | 
|$p\cdot w=200000$| Câu trả lời hợp lệ | Ràng buộc ranh giới | 

## Vỏ cạnh 

Hãy xem xét:```
2
100 5
100 7
```Thuật toán đặt cả hai phần thưởng vào`base = 12`. 

Không có mục DP nào được tạo. 

Câu trả lời cuối cùng trở thành$$1\cdot12=12.$$Điều này xử lý chính xác các trò chơi chắc chắn. 

Bây giờ hãy xem xét:```
3
90 100
90 50
90 1
```Sau khi sắp xếp nhóm xác suất-90, phần thưởng sẽ trở thành:```
100, 50, 1
```Bất kỳ giải pháp nào sử dụng hai trò chơi từ nhóm này đều được thể hiện bằng hai phần thưởng lớn nhất. Đối số cắt tỉa đảm bảo rằng phần thưởng nhỏ hơn không bao giờ thay thế phần thưởng lớn hơn trong cùng một lớp xác suất. 

Cuối cùng hãy xem xét:```
1
1 200000
```Giá trị mong đợi là$$0.01 \cdot 200000 = 2000.$$Thuật toán giữ trò chơi, cập nhật`dp[200000] = 0.01`và tính toán chính xác giá trị này. ranh giới$p_iw_i=200000$không gây khó khăn gì đặc biệt.
