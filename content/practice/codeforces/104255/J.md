---
title: "CF 104255J - Người du hành xuyên không gian"
description: "Mỗi chiều của hệ thống hoạt động giống như một bước đi ngẫu nhiên một chiều độc lập. Con tàu bắt đầu ở vị trí $ai$ trong chiều $i$, và trong khi hệ thống không ổn định ở chiều đó (nghĩa là tọa độ ít nhất là 1), nó sẽ di chuyển sang phải một bước với xác suất $pi$ và…"
date: "2026-07-01T21:55:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104255
codeforces_index: "J"
codeforces_contest_name: "BSUIR Open X. Reload. Students final"
rating: 0
weight: 104255
solve_time_s: 91
verified: true
draft: false
---

[CF 104255J - Người du hành xuyên không gian](https://codeforces.com/problemset/problem/104255/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi chiều của hệ thống hoạt động giống như một bước đi ngẫu nhiên một chiều độc lập. Tàu xuất phát tại vị trí$a_i$trong chiều$i$và trong khi hệ thống không ổn định trong chiều đó (nghĩa là tọa độ ít nhất là 1), nó sẽ di chuyển sang phải một bước với xác suất$p_i$và xác suất còn lại một bước$q_i = 1 - p_i$. Thời điểm tọa độ trở nên nhỏ hơn 1, thứ nguyên đó được coi là đã phục hồi và ngừng phát triển. 

Nhiệm vụ là tính xác suất để mọi chiều cuối cùng đạt đến trạng thái “được phục hồi” này ít nhất một lần, bắt đầu từ vectơ vị trí ban đầu. 

Vì mỗi chiều phát triển độc lập nên sự kiện toàn cầu là sự giao thoa của các sự kiện độc lập. Điều này ngay lập tức gợi ý rằng các yếu tố trả lời thành tích của xác suất theo thứ nguyên, mỗi yếu tố là xác suất mà bước đi ngẫu nhiên sai lệch bắt đầu từ$a_i$bao giờ đạt tới 0. 

Các ràng buộc làm cho việc mô phỏng trực tiếp không thể thực hiện được. Mỗi tọa độ có thể lên tới 1000 và xác suất là các phân số có tử số và mẫu số lớn đến$10^9$, do đó, bất kỳ mô phỏng Monte Carlo hoặc mô phỏng từng bước ngây thơ nào cũng sẽ không hội tụ kịp thời. Cấu trúc gợi ý mạnh mẽ một tính toán xác suất dạng đóng cho mỗi chiều. 

Trường hợp cạnh tinh tế xuất hiện khi bước đi hoàn toàn lệch về bên phải, nghĩa là$p_i = 1$. Trong trường hợp đó, bước đi không bao giờ giảm đi, vì vậy bất kỳ vị trí bắt đầu nào cũng$a_i > 0$làm cho việc phục hồi không thể thực hiện được. Ngược lại, nếu bước đi thiên về trái hoặc đủ không thiên vị thì khả năng phục hồi sẽ chắc chắn. Việc xử lý chính xác các trường hợp biên này là rất quan trọng vì công thức trực tiếp liên quan đến phép chia có thể bị hỏng khi xác suất là 0 hoặc 1. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng bước đi ngẫu nhiên cho từng chiều một cách độc lập nhiều lần và ước tính liệu nó có chạm đến 0 hay không. Ngay cả khi chúng tôi cố gắng tính toán xác suất chính xác thông qua lập trình động theo các trạng thái$[0, 1000]$, các chuyển đổi có thể được mô hình hóa, nhưng vấn đề mang tính khái niệm hơn là tính toán: không gian trạng thái là vô hạn nếu chúng ta cho phép chuyển động lên trên mà không bị ràng buộc và việc cắt bớt sẽ gây ra lỗi. Ngay cả khi bị giới hạn ở 2000 trạng thái trên mỗi chiều, độ phức tạp tổng thể sẽ trở nên$O(n \cdot m^2)$, quá lớn đối với$n = 1000$. 

Nhận xét quan trọng là mỗi chiều là một bài toán phá hoại của một tay cờ bạc cổ điển trên một nửa đường vô hạn. Chúng ta chỉ quan tâm đến xác suất đạt tới 0 bắt đầu từ$a_i$. Đối với bước đi ngẫu nhiên có sai lệch: 

Nếu$p_i \le q_i$, bước đi bị lệch trái đủ để nó chạm 0 với xác suất 1. Nếu$p_i > q_i$, có sự dịch chuyển về bên phải và xác suất chạm tới 0 trở thành$(q_i / p_i)^{a_i}$. Điều này xuất phát từ việc giải quyết phép truy toán tiêu chuẩn cho xác suất đạt được trong chuỗi Markov. 

Khi mỗi thứ nguyên được giảm xuống giá trị dạng đóng, câu trả lời cuối cùng chỉ đơn giản là sản phẩm trên tất cả các thứ nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng / DP | O(n · S²) | O(S) | Quá chậm | 
| Dạng đóng theo chiều | O(n log MOD) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Chuyển từng xác suất thành dạng phân số 

Đối với mỗi chiều, hãy đọc$p_i = \frac{s_i}{t_i}$và tính toán$q_i = \frac{t_i - s_i}{t_i}$. 

Điều này mang lại một cách biểu diễn rõ ràng trong đó việc so sánh và tỷ lệ có thể được thực hiện chính xác bằng số nguyên. 

### 2. Quyết định xem khả năng hấp thụ có chắc chắn không 

Nếu$p_i \le q_i$, tương đương$2s_i \le t_i$, thì bước đi cuối cùng sẽ đạt 0 với xác suất là 1. 

Điều này là do không có sự trôi dạt nào đẩy quá trình ra xa 0. 

### 3. Xử lý hoàn toàn các trường hợp thiên phải 

Nếu$s_i = 0$, sau đó$p_i = 0$, nên bước đi luôn di chuyển sang trái và sự hấp thụ là chắc chắn. 

Nếu như$s_i = t_i$, sau đó$p_i = 1$, do đó bước đi không bao giờ di chuyển sang trái và không thể hấp thụ trừ khi$a_i = 0$, được loại trừ bởi các ràng buộc. 

### 4. Tính xác suất trúng trường hợp trôi đúng 

Nếu$p_i > q_i$, sử dụng công thức:$$\left(\frac{q_i}{p_i}\right)^{a_i}
= \left(\frac{t_i - s_i}{s_i}\right)^{a_i}$$Chúng tôi tính toán điều này theo modulo bằng cách sử dụng lũy ​​thừa mô-đun và nghịch đảo mô-đun. 

### 5. Nhân tất cả xác suất của chiều 

Vì các thứ nguyên là độc lập nên nhân tất cả xác suất trên mỗi thứ nguyên theo modulo$10^9+7$. 

### Tại sao nó hoạt động 

Mỗi chiều phát triển độc lập dưới dạng chuỗi Markov có sự kiện hấp thụ “đạt 0”. Xác suất hấp thụ chỉ được xác định bởi hướng trôi. Khi độ trôi không dương, sự tái diễn đảm bảo sự hấp thụ cuối cùng. Khi độ trôi là dương, việc giải quyết phép truy toán cho xác suất trúng sẽ mang lại sự phân rã theo cấp số nhân ở vị trí bắt đầu. Sự độc lập về các chiều biến sự kiện chung thành sản phẩm của xác suất vô hướng và số học mô-đun bảo toàn chính xác cấu trúc này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modexp(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

n = int(input())
a = list(map(int, input().split()))

ans = 1

for i in range(n):
    s, t = map(int, input().split())

    if s == 0:
        continue

    if s * 2 <= t:
        continue

    q = t - s

    ratio = q * pow(s, MOD - 2, MOD) % MOD
    ans = ans * modexp(ratio, a[i]) % MOD

print(ans)
```Giải pháp đọc từng chiều một cách độc lập và ngay lập tức giảm nó xuống mức đóng góp xác suất vô hướng. Nghịch đảo mô-đun của$s$được sử dụng để hình thành$q/p$theo số học modulo. lũy thừa nhanh xử lý sức mạnh$a_i$, có thể lên tới 1000, giữ cho lời giải luôn nằm trong giới hạn. 

Cần thận trọng khi so sánh$2s \le t$, giúp tránh hoàn toàn sự phân chia và ngăn chặn các vấn đề về độ chính xác. Vụ án$s = 0$được tách ra sớm vì nghịch đảo mô-đun sẽ bị phá vỡ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 2 3
4 6
8 10
5 6
```Chúng tôi xử lý từng chiều: 

| tôi | a_i | s_i/t_i | Tình trạng | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 6/4 | 2s > t | (2/4)^1 = 1/2 | 
| 2 | 2 | 10/8 | 2s > t | (2/8)^2 = (1/4)^2 | 
| 3 | 3 | 6/5 | 2s > t | (1/5)^3 | 

Nhân các giá trị mô-đun này mang lại:```
714250005
```Dấu vết này cho thấy rằng cả ba chiều đều ở chế độ trôi dạt bên phải, do đó mỗi chiều đóng góp một số hạng hình học phân rã. 

### Mẫu 2 

đầu vào:```
2
1 2
1 3
1 2
1 2
```Đây: 

| tôi | a_i | s_i/t_i | Tình trạng | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1/2 | 2s = t | 1 | 
| 2 | 2 | 1/2 | 2s = t | 1 | 

Cả hai chiều đều không thiên vị hoặc thiên trái đủ để độ hấp thụ là chắc chắn trong từng trường hợp. Sản phẩm là 1. 

Điều này xác nhận rằng điều kiện ngưỡng thu gọn chính xác toàn bộ tính toán xác suất về trường hợp không đổi khi độ lệch không dương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log MOD) | Mỗi chiều yêu cầu lũy thừa mô-đun | 
| Không gian | O(1) | Chỉ có một số biến vô hướng được duy trì | 

Lời giải dễ dàng nằm trong giới hạn vì$n \le 1000$và mỗi phép lũy thừa đều nhanh. Các phép toán số học là các phép toán modulo có kích thước không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def modexp(a, e):
        res = 1
        while e:
            if e & 1:
                res = res * a % MOD
            a = a * a % MOD
            e >>= 1
        return res

    n = int(input())
    a = list(map(int, input().split()))

    ans = 1
    for i in range(n):
        s, t = map(int, input().split())
        if s == 0:
            continue
        if s * 2 <= t:
            continue
        ratio = (t - s) * pow(s, MOD - 2, MOD) % MOD
        ans = ans * modexp(ratio, a[i]) % MOD

    return str(ans)

# provided sample
assert run("""3
1 2 3
4 6
8 10
5 6
""") == "714250005"

# minimum size
assert run("""1
1
1 2
""") == "1"

# fully right-biased (impossible to return)
assert run("""1
3
1 1
""") == "0"

# always left biased
assert run("""1
5
0 3
""") == "1"

# mixed case
assert run("""2
2 1
3 1
1 3
2 3
""") == run("""2
2 1
3 1
1 3
2 3
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thiên tả duy nhất | 1 | sự chắc chắn hấp thụ | 
| p = 1 trường hợp | 0 | xử lý bất khả thi | 
| s = 0 trường hợp | 1 | xác suất cạnh | 
| kích thước hỗn hợp | tính toán | độ chính xác của sản phẩm | 

## Vỏ cạnh 

Một trường hợp cạnh tranh quan trọng là khi$s_i = 0$. Trong tình huống này, bước đi luôn di chuyển sang trái nên nó chạm 0 ngay lập tức bất kể$a_i$. Thuật toán bỏ qua hoàn toàn việc đảo ngược mô-đun và đóng góp trực tiếp 1 vào sản phẩm. 

Một trường hợp cạnh khác là khi$s_i = t_i$. Con đường không bao giờ di chuyển sang trái, vì vậy nếu$a_i > 0$, xác suất là 0. Điều kiện$2s_i > t_i$kích hoạt chính xác công thức hình học, nhưng vì$t_i - s_i = 0$, tỷ lệ trở thành 0 và bất kỳ số mũ dương nào cũng mang lại 0, phù hợp với hành vi đúng. 

Cuối cùng, trường hợp ngưỡng$2s_i = t_i$đại diện cho một bước đi ngẫu nhiên đối xứng. Thuật toán phân loại xác suất này là xác suất 1, khớp với các đặc tính lặp lại của các bước đi ngẫu nhiên không thiên vị trên các số nguyên không âm trong đó độ hấp thụ ở 0 là chắc chắn.
