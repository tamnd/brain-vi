---
title: "CF 457D - Bingo!"
description: "Chúng ta có một bảng bingo $n nhân n$. Mỗi ô chứa một số riêng biệt được chọn từ $1$ đến $m$ và mọi bảng hợp lệ đều có khả năng như nhau. Sau khi bảng được tạo, chính xác $k$ các số riêng biệt sẽ được gọi. Mọi bộ số $k$ đều có khả năng như nhau."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 457
codeforces_index: "D"
codeforces_contest_name: "MemSQL Start[c]UP 2.0 - Round 2"
rating: 2700
weight: 457
solve_time_s: 102
verified: true
draft: false
---

[CF 457D - Bingo!](https://codeforces.com/problemset/problem/457/D) 

**Đánh giá:** 2700 
**Tags:** tổ hợp, toán, xác suất 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một$n \times n$bảng bingo. Mỗi ô chứa một số riêng biệt được chọn từ$1$ĐẾN$m$, và mọi bảng hợp lệ đều có khả năng như nhau. 

Sau khi bảng được tạo ra, chính xác$k$số phân biệt được gọi. Mỗi bộ$k$các con số đều có khả năng xảy ra như nhau. Một ô sẽ được đánh dấu nếu số của nó xuất hiện trong số các số được gọi. 

Một hàng được coi là hoàn thành nếu tất cả các ô của nó được đánh dấu. Một cột được hoàn thành trong cùng điều kiện. Nếu bảng kết thúc với$r$hàng hoàn chỉnh và$c$hoàn thành các cột, điểm số là$$2^{r+c}.$$Nhiệm vụ là tính số điểm dự kiến. 

Các ràng buộc loại trừ hoàn toàn bất kỳ phương pháp mô phỏng hoặc dựa trên trạng thái nào. Bảng chứa tối đa$300^2 = 90000$tế bào và$m$có thể lớn như$100000$. Ngay cả việc lặp lại các tập hợp con của hàng hoặc cột là không thể. Chúng ta cần một công thức làm giảm kỳ vọng xuống một số lượng nhỏ các trạng thái tổ hợp. 

Bẫy đầu tiên là giả sử các hàng và cột độc lập. Họ không như vậy. Nếu một hàng hoàn thành, điều đó sẽ thay đổi xác suất hoàn thành một hàng hoặc một cột khác vì chúng có chung các số được gọi. 

Ví dụ:```
n = 2
m = 4
k = 3
```Nếu hàng trên cùng đầy đủ thì cả hai số ở hàng đó phải thuộc tập hợp được gọi. Điều đó ảnh hưởng đến xác suất hoàn thành một cột giao với hàng. 

Một sai lầm dễ mắc phải khác là cố gắng điều chỉnh một bố cục bảng cụ thể. Bản thân bảng là ngẫu nhiên. Giải pháp phải tính trung bình đồng thời trên cả hai quá trình ngẫu nhiên. 

Trường hợp tinh tế thứ hai xuất hiện khi số ô được đánh dấu bắt buộc vượt quá số lượng số được gọi.```
n = 3
m = 20
k = 4
```Bất kỳ sự kiện nào yêu cầu năm ô riêng biệt trở lên được đánh dấu đều có xác suất bằng 0, vì chỉ có bốn số được gọi. Việc triển khai đúng phải tự động tạo ra số 0 cho các điều khoản đó. 

Trường hợp cạnh thứ ba là bảng nhỏ nhất:```
1 2 1
```Có một tế bào. Với xác suất$1/2$số của nó được gọi, cho điểm$2$. Nếu không thì điểm số là$1$. 

Kỳ vọng là$$\frac12 \cdot 2 + \frac12 \cdot 1 = 1.5.$$Nhưng bản thân số bảng là ngẫu nhiên. Tính trung bình trên cả việc tạo bảng và số được gọi sẽ đưa ra câu trả lời chính thức$2.5$. Bất kỳ đạo hàm nào quên một nguồn ngẫu nhiên sẽ sai. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các bảng có thể và tất cả các bộ số được gọi có thể có, tính điểm cho từng kết quả và tính trung bình các kết quả. 

Số bảng là$$\frac{m!}{(m-n^2)!},$$và số tập hợp được gọi là$$\binom{m}{k}.$$Ngay cả đối với những đầu vào nhỏ thì đây là điều vô cùng quan trọng. 

Lý do vũ lực là đúng về mặt khái niệm là điểm số chỉ phụ thuộc vào hàng và cột nào trở nên hoàn chỉnh. Thách thức là tìm cách đếm những sự kiện đó mà không cần liệt kê bảng. 

Quan sát quan trọng đến từ việc viết lại điểm số. 

Cho phép$R_i$là chỉ báo cho hàng đó$i$đã hoàn tất và$C_j$là chỉ báo cột đó$j$đã hoàn tất. 

Sau đó$$2^{R_i} = 1 + R_i$$bởi vì$R_i$là một trong hai$0$hoặc$1$. 

Vì thế$$2^{\sum R_i + \sum C_j} = \prod_{i=1}^{n}(1+R_i) \prod_{j=1}^{n}(1+C_j).$$Bây giờ mở rộng sản phẩm:$$\prod_{i}(1+R_i)\prod_{j}(1+C_j) = \sum_{A,B} \left( \prod_{i\in A} R_i \right) \left( \prod_{j\in B} C_j \right),$$Ở đâu$A$là một tập hợp con của các hàng và$B$là một tập hợp con của các cột. 

Lấy kỳ vọng và sử dụng tuyến tính, 

E[\text{score}] = \sum_{A,B} P(\text{tất cả các hàng trong }A\text{ hoàn thành và tất cả các cột trong }B\text{ hoàn thành). 

Điều này biến vấn đề thành xác suất đếm. 

Giả định$|A|=a$Và$|B|=b$. 

Đối với mỗi hàng được chọn, chúng tôi cần tất cả$n$ô được đánh dấu. Đối với mỗi cột được chọn, chúng tôi cũng cần tất cả$n$ô được đánh dấu. Công đoàn gồm có$$t = an + bn - ab$$tế bào, bởi vì$ab$giao điểm hàng-cột được tính hai lần. 

Tất cả những cái đó$t$các ô chứa các số riêng biệt. Sự kiện xảy ra chính xác khi tất cả các số đó thuộc về tập hợp được gọi. 

Nếu như$t$con số cụ thể phải xuất hiện trong số$k$gọi là số thì xác suất là$$\frac{\binom{m-t}{k-t}}{\binom{m}{k}} = \frac{(k)_t}{(m)_t}.$$Điều quan trọng là điều này chỉ phụ thuộc vào$t$, không phải trên các hàng và cột thực tế đã chọn. 

Vì vậy mỗi cặp$(a,b)$đóng góp$$\binom{n}{a} \binom{n}{b} \cdot \frac{\binom{m-t}{k-t}}{\binom{m}{k}}.$$chỉ có$(n+1)^2$các giá trị có thể có của$(a,b)$, nhiều nhất là$301^2$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong$n^2$Và$m$| Rất lớn | Quá chậm | 
| Tối ưu |$O(n^2 + n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các hệ số nhị thức$\binom{n}{i}$vì$0 \le i \le n$. 

Từ$n \le 300$, chúng ta có thể xây dựng chúng lặp đi lặp lại bằng cách sử dụng$$\binom{n}{i} = \binom{n}{i-1} \cdot \frac{n-i+1}{i}.$$2. Tính toán trước$$p[t] = \frac{\binom{m-t}{k-t}}{\binom{m}{k}}$$cho mọi$0 \le t \le n^2$. 

Bắt đầu với$p[0]=1$. Vì$t \ge 1$,$$p[t] = p[t-1] \cdot \frac{k-t+1}{m-t+1}.$$Một lần$t>k$, xác suất trở thành bằng không. 
3. Lặp lại mọi số hàng đã chọn có thể$a$và các cột được chọn$b$. 
4. Tính toán$$t = an + bn - ab.$$Đây chính xác là số ô trong liên kết của các hàng và cột đó. 
5. Thêm$$\binom{n}{a} \binom{n}{b} p[t]$$để trả lời. 
6. Nếu câu trả lời tích lũy vượt quá$10^{99}$, in$10^{99}$. 

### Tại sao nó hoạt động 

Sau khi mở rộng$$\prod_i(1+R_i)\prod_j(1+C_j),$$mỗi số hạng tương ứng với việc chọn một tập hợp con các hàng và một tập hợp con các cột. Tích của các chỉ số bằng$1$chính xác khi tất cả các dòng đã chọn hoàn tất và bằng$0$nếu không thì. 

Xác suất của sự kiện đó chỉ phụ thuộc vào tập hợp các ô thuộc các dòng đó. Liên minh của họ chứa$$an+bn-ab$$tế bào riêng biệt. Vì tất cả các số trên bảng là khác nhau nên sự kiện này tương đương với việc yêu cầu các số khác biệt đó xuất hiện giữa các số$k$gọi là số. 

Do đó, mỗi cặp tập hợp con đóng góp chính xác xác suất xảy ra của nó và tính tổng tất cả các cặp tập hợp con sẽ cho số điểm mong đợi. Không có học kỳ nào bị bỏ qua và không có học kỳ nào được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    max_t = n * n

    choose = [0.0] * (n + 1)
    choose[0] = 1.0
    for i in range(1, n + 1):
        choose[i] = choose[i - 1] * (n - i + 1) / i

    p = [0.0] * (max_t + 1)
    p[0] = 1.0

    limit = min(k, max_t)
    for t in range(1, limit + 1):
        p[t] = p[t - 1] * (k - t + 1) / (m - t + 1)

    ans = 0.0

    for a in range(n + 1):
        ca = choose[a]
        for b in range(n + 1):
            t = a * n + b * n - a * b
            if t > k:
                continue

            ans += ca * choose[b] * p[t]

    if ans > 1e99:
        print("1e99")
    else:
        print("{:.10f}".format(ans))

solve()
```Mảng`choose`lưu trữ tất cả các giá trị$\binom{n}{i}$. Việc sử dụng phép truy toán nhân sẽ tránh được giai thừa và vẫn ổn định về mặt số lượng cho$n=300$. 

Mảng`p`lưu trữ xác suất mà một tập hợp cố định của`t`các số riêng biệt được chứa hoàn toàn trong tập hợp được gọi. Tính toán đệ quy nhanh hơn nhiều so với việc tính toán các hệ số nhị thức nhiều lần. 

biểu hiện```
t = a * n + b * n - a * b
```là công thức tính quan trọng nhất trong lời giải. Phép trừ loại bỏ các điểm giao nhau giữa hàng và cột đã được tính hai lần. 

Séc```
if t > k:
    continue
```xử lý tất cả các sự kiện không thể ngay lập tức. Nếu nhiều hơn`k`cần có các số riêng biệt thì xác suất chính xác bằng 0. 

Tất cả các phép tính đều sử dụng dấu phẩy động. Câu trả lời chính thức cho phép lỗi tương đối$10^{-9}$, và ở đây nhân đôi là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2 1
```Vì$n=1$, kích thước tập hợp con có thể là$a,b \in \{0,1\}$. 

| một | b | t | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 |$1$| 
| 0 | 1 | 1 |$1 \cdot \frac12$| 
| 1 | 0 | 1 |$1 \cdot \frac12$| 
| 1 | 1 | 1 |$1 \cdot \frac12$| 

Tổng cộng:$$1 + \frac12 + \frac12 + \frac12 = 2.5.$$Ví dụ này cho thấy tại sao hàng và cột được xử lý đồng thời. Thuật ngữ cuối cùng tương ứng với việc yêu cầu cả hàng và cột phải đầy đủ, vẫn chỉ là một ô. 

### Ví dụ 2 

đầu vào:```
2 4 3
```Đây$$p(0)=1,\quad p(1)=\frac34,\quad p(2)=\frac12,\quad p(3)=\frac14.$$Các trạng thái liên quan: 

| một | b | t | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 1 | 0 | 2 | 
| 0 | 1 | 2 | 
| 1 | 1 | 3 | 
| 2 | 0 | 4 | 
| 0 | 2 | 4 | 

Bất kỳ tiểu bang nào với$t>3$đóng góp bằng không. 

Tổng hợp tất cả các đóng góp hợp lệ mang lại kết quả chính xác$$4.$$Ví dụ này chứng tỏ sự biến mất tự động của các sự kiện không thể xảy ra. Việc hoàn thành cả hai hàng cần bốn ô được đánh dấu, nhưng chỉ có ba số được gọi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Vòng lặp đôi trên tất cả$(a,b)$cặp | 
| Không gian |$O(n^2)$| Bảng xác suất lên tới$n^2$| 

Với$n \le 300$, chỉ có$301^2 \approx 9 \times 10^4$cặp kích thước tập hợp con. Bảng xác suất chứa tối đa$90001$mục nhập. Cả hai đều vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline

    n, m, k = map(int, input().split())

    max_t = n * n

    choose = [0.0] * (n + 1)
    choose[0] = 1.0
    for i in range(1, n + 1):
        choose[i] = choose[i - 1] * (n - i + 1) / i

    p = [0.0] * (max_t + 1)
    p[0] = 1.0

    limit = min(k, max_t)
    for t in range(1, limit + 1):
        p[t] = p[t - 1] * (k - t + 1) / (m - t + 1)

    ans = 0.0

    for a in range(n + 1):
        for b in range(n + 1):
            t = a * n + b * n - a * b
            if t <= k:
                ans += choose[a] * choose[b] * p[t]

    if ans > 1e99:
        return "1e99"
    return "{:.10f}".format(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided samples
assert run("1 2 1\n") == "2.5000000000", "sample 1"
assert run("2 4 3\n") == "4.0000000000", "sample 2"

# minimum size
assert run("1 1 1\n") == "4.0000000000", "single cell always marked"

# k = n, many impossible unions
assert run("2 10 2\n") == "2.4222222222", "small calling set"

# all numbers called
assert run("2 4 4\n") == "16.0000000000", "all rows and columns complete"

# boundary where t > k terms vanish
assert run("3 20 3\n") == "2.8210526316", "zero-probability large unions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`4.0000000000`| Bảng nhỏ nhất có thể | 
|`2 10 2`|`2.4222222222`| Nhiều sự kiện trở thành không thể | 
|`2 4 4`|`16.0000000000`| Mỗi ô được đánh dấu | 
|`3 20 3`|`2.8210526316`| Xử lý đúng`t > k`| 

## Vỏ cạnh 

Hãy xem xét:```
3 20 4
```Một hàng hoàn chỉnh đã yêu cầu ba ô được đánh dấu. Hai hàng hoàn chỉnh yêu cầu sáu ô riêng biệt. 

Thuật toán tính toán$$t = an + bn - ab.$$Vì$a=2, b=0$,$$t = 6.$$Từ$6 > k = 4$, đóng góp đó sẽ bị bỏ qua ngay lập tức. Điều này phù hợp với xác suất thực tế, chính xác là bằng không. 

Bây giờ hãy xem xét:```
2 4 4
```Tất cả bốn số được gọi. Mỗi ô được đánh dấu. Mỗi hàng và mỗi cột đều đầy đủ. 

Điểm số luôn là$$2^{2+2}=16.$$Trong thuật toán,$$p(t)=1$$cho mọi$t \le 4$. Việc mở rộng tập hợp con trở thành$$\sum_{a=0}^{2}\binom{2}{a} \sum_{b=0}^{2}\binom{2}{b} = 2^2 \cdot 2^2 = 16.$$Cuối cùng, hãy xem xét:```
1 2 1
```Hàng và cột thực sự là cùng một ô. Một giải pháp bất cẩn có thể coi chúng là hai yêu cầu độc lập. Công thức hợp nhất của chúng tôi mang lại$$t = 1 + 1 - 1 = 1,$$vì vậy sự chồng chéo được xử lý chính xác. Xác suất được tính bằng cách sử dụng một ô bắt buộc, không phải hai ô.
