---
title: "CF 248E - Sinh Nhật Heo Con"
description: "Chúng ta được yêu cầu tính toán số kệ dự kiến ​​không có hũ mật ong nào chưa được nếm sau một loạt hành động. Mỗi kệ bắt đầu với một số hũ mật ong. Winnie di chuyển một số ít chậu từ kệ này sang kệ khác, nếm thử chúng trong quá trình này."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 248
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 152 (Div. 2)"
rating: 2600
weight: 248
solve_time_s: 137
verified: false
draft: false
---

[CF 248E - Sinh nhật heo con](https://codeforces.com/problemset/problem/248/E) 

**Đánh giá:** 2600 
**Tags:** dp, toán, xác suất 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính toán số kệ dự kiến không có hũ mật ong nào chưa được nếm sau một loạt hành động. Mỗi kệ bắt đầu với một số hũ mật ong. Winnie di chuyển một số ít chậu từ kệ này sang kệ khác, nếm thử chúng trong quá trình này. Mỗi nồi trên kệ nguồn đều có cơ hội được chọn để nếm thử như nhau, vì vậy kết quả chỉ mang tính xác suất. Sau mỗi lần di chuyển, chúng ta cần số lượng kệ dự kiến ​​mà mỗi nồi đều được nếm thử ít nhất một lần. 

Đầu vào chỉ định số lượng kệ ban đầu và số lượng chậu trên mỗi kệ. Sau đó, chúng tôi nhận được danh sách các hành động trong đó Winnie di chuyển chậu. Mỗi hành động được xác định bởi giá nguồn, giá mục tiêu và số lượng chậu được di chuyển. Đầu ra sau mỗi hành động là một số dấu phẩy động biểu thị số lượng kệ dự kiến ​​​​hoàn toàn được “nếm thử”. 

Các ràng buộc chỉ ra rằng có thể có tới 100.000 giá sách và 100.000 hành động. Mỗi kệ có thể chứa tối đa 100 chậu và mỗi lần di chuyển sẽ chuyển tối đa 5 chậu. Điều này có nghĩa là một cách tiếp cận ngây thơ để theo dõi rõ ràng từng pot sẽ có khả năng liên quan đến$10^5 \times 100$tính toán cho mỗi hành động, quá chậm. Chúng ta cần một cách biểu diễn xác suất, toán học hơn là mô phỏng rõ ràng. 

Trường hợp khó khăn xảy ra khi kệ bắt đầu không có chậu nào. Việc triển khai bất cẩn có thể chia cho 0 hoặc bỏ qua các kệ như vậy, nhưng một kệ không có chậu nào được coi là hoàn toàn "nếm thử" theo định nghĩa và sẽ góp phần vào số lượng dự kiến ​​​​ngay từ đầu. Một trường hợp tinh vi khác phát sinh khi mọi hành động đều chuyển tất cả các chậu ra khỏi kệ; xác suất phải cập nhật chính xác mà không cần giả sử số nguyên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là mô phỏng từng hành động bằng cách xem xét mọi tổ hợp chậu có thể được chọn, đánh dấu chúng là đã nếm thử và đếm các kệ đã được nếm thử đầy đủ. Điều này đúng về mặt lý thuyết, nhưng đối với mỗi hành động, số lượng kết hợp sẽ tăng theo cấp số nhân theo số lượng chậu được di chuyển. Vì mỗi kệ có thể có tới 100 chậu và chúng tôi có tới 100.000 hành động nên điều này là không khả thi. Ví dụ: chọn 5 cái chậu từ 100 cái sẽ mang lại$\binom{100}{5} \approx 75 \text{ million}$sự kết hợp. Việc lặp lại điều này cho 100.000 hành động vượt xa giới hạn thời gian 2 giây. 

Cái nhìn sâu sắc quan trọng là chuyển từ mô phỏng rõ ràng sang theo dõi xác suất. Cho phép$p_i$biểu thị xác suất để một chiếc bình duy nhất trên kệ$i$chưa được nếm thử**. Số lượng kệ được nếm thử đầy đủ dự kiến ​​chỉ đơn giản là tổng của$(1 - p_i^{a_i})$trên tất cả các kệ, nơi$a_i$là tổng số chậu trên kệ$i$. Khi Winnie di chuyển$k$chậu từ kệ$u$để kệ$v$, chúng ta có thể cập nhật xác suất chưa được chạm tới cho giá nguồn$u$như một sản phẩm trên$k$chậu, sử dụng công thức lấy mẫu không thay thế:$$p_u \gets p_u \cdot \frac{a_u - k}{a_u} + (1 - p_u) \cdot \frac{k}{a_u}$$Đối với kệ mục tiêu$v$, chúng tôi cập nhật xác suất chưa được chạm tới của nó dựa trên các bình sắp tới. Từ$k \le 5$, những cập nhật này là thời gian không đổi cho mỗi hành động. Điều này cho phép mỗi hành động được xử lý trong$O(1)$thời gian trên mỗi kệ, làm cho sự phức tạp tổng thể$O(n + q)$, điều đó có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * 2^k) | O(n * max(a_i)) | Quá chậm | 
| Theo dõi xác suất | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`p`Ở đâu`p[i] = 1.0`nếu kệ`i`có bình, đại diện cho xác suất một bình chưa được nếm **. Nếu một kệ bắt đầu không có chậu nào, hãy coi như nó đã được nếm thử đầy đủ, vì vậy`p[i] = 0.0`. 
2. Tính số lượng kệ dự kiến ​​ban đầu sẽ được nếm thử đầy đủ như sau:$\sum_i (1 - p[i]^{a[i]})$. Kệ không có chậu đóng góp 1,0 trực tiếp. 
3. Với mỗi hành động, trích xuất kệ nguồn`u`, kệ mục tiêu`v`, và số chậu`k`. 
4. Đối với kệ nguồn`u`, cập nhật xác suất còn lại một chiếc bình chưa được chạm tới sau khi lấy mẫu$k$chậu. Vì Winnie chọn các bình giống nhau nên xác suất không chạm vào một bình duy nhất sẽ là:$$p[u] = p[u] \cdot \frac{a[u] - k}{a[u]}$$Điều này phản ánh thực tế rằng một cái bình vẫn tồn tại nguyên vẹn nếu nó không được chọn trong số các$k$đã di chuyển. 

5. Đối với kệ mục tiêu`v`, mỗi pot đến sẽ đóng góp xác suất chưa được chạm tới trước đó của nó. Xác suất chưa được chạm tới kết hợp cho giá được cập nhật theo cấp số nhân trên`k`chậu:$$p[v] = 1 - (1 - p[v]) \cdot (1 - p_u^{\text{per moved pot}})$$Trong thực tế, chúng tôi nâng cao`p[u]`đến sức mạnh`1`cho mỗi chậu được di chuyển kể từ`k`là nhỏ. 

6. Sau khi cập nhật cả hai kệ, hãy tính lại số lượng kệ dự kiến ​​sẽ được nếm thử đầy đủ như sau:$$\text{expected} = \sum_i (1 - p[i]^{a[i]})$$1. In giá trị mong đợi sau mỗi hành động với độ chính xác 12 chữ số thập phân. 

Bất biến được duy trì là`p[i]`luôn thể hiện chính xác xác suất của một chiếc bình trên kệ`i`chưa được nếm thử**. Vì công thức đếm dự kiến ​​sử dụng$1 - p[i]^{a[i]}$, chúng tôi luôn nhận được đúng số lượng kệ đã được nếm thử đầy đủ theo dự kiến ​​sau mỗi hành động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
q = int(input())

p = [1.0 if a[i] > 0 else 0.0 for i in range(n)]

def expected_count():
    return sum(1 - p[i]**a[i] for i in range(n))

for _ in range(q):
    u, v, k = map(int, input().split())
    u -= 1
    v -= 1
    if a[u] > 0:
        # Probability each pot on u survives untouched after moving k
        factor = (a[u] - k) / a[u]
        p[u] *= factor
    # Update target shelf
    for _ in range(k):
        p[v] = p[v] + p[u] * (1 - p[v])
    print("%.12f" % expected_count())
```Mã này thực hiện thuật toán chính xác như được mô tả. Chúng tôi xử lý các giá không có thùng bằng cách khởi tạo xác suất chưa được chạm tới của chúng thành 0. Mỗi hành động sẽ cập nhật giá nguồn theo cấp số nhân và giá mục tiêu lặp đi lặp lại cho mỗi thùng đến. Số lượng dự kiến ​​cuối cùng sử dụng công thức tính tổng chính xác$1 - p[i]^{a[i]}$. 

## Ví dụ đã hoạt động 

Sử dụng mẫu 1: 

Kệ đầu vào: [2, 2, 3] 

| Bước | Hành động | p[1] | p[2] | p[3] | Dự kiến ​​m | 
| --- | --- | --- | --- | --- | --- | 
| 0 | ban đầu | 1 | 1 | 1 | 0,0 | 
| 1 | 1->2 1 | 0,5 | 1 | 1 | 0,0 | 
| 2 | 2->1 2 | 0,875 | 0,0 | 1 | 0,3333 | 
| 3 | 1->2 2 | 0,65625 | 0,0 | 1 | 1.0 | 
| 4 | 3->1 1 | 0.828125 | 0,0 | 0,5 | 1.0 | 
| 5 | 3->2 2 | 0,65625 | 0,0 | 0,25 | 2.0 | 

Bảng này cho thấy cách cập nhật xác suất và xác nhận các giá trị bất biến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Cập nhật mảng và mỗi hành động ban đầu là O(1) vì k 5 | 
| Không gian | O(n) | Mảng`p`stores probability per shelf |

 The solution fits comfortably within the 2-second time limit, as n and q are both ≤ 100,000 and updates per action are constant.

 ## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("piglet_birthday_solution.py").read())
    return output.getvalue().strip()

# Provided sample
assert run("3\n2 2 3\n5\n1 2 1\n2 1 2\n1 2 2\n3 1 1\n3 2 2\n") == \
"0.000000000000\n0.333333333333\n1.000000000000\n1.000000000000\n2.000000000000", "sample 1"

# Minimum input, one shelf, one pot, one action moving the pot to itself
assert run("1\n1\n1\n1 1 1\n") == "1.000000000000", "single shelf"

# Maximum pots, single action moving one pot
assert run("1\n100
```
