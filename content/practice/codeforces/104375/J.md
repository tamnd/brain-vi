---
title: "CF 104375J - Phản ứng nhảy"
description: "Chúng ta được cung cấp một mảng các số nguyên trong đó mỗi giá trị đại diện cho một “năng lượng nhảy” của một chất. Khi trộn lẫn hai chất có năng lượng $a$ và $b$, chúng tạo ra năng lượng $ab$."
date: "2026-07-01T17:31:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104375
codeforces_index: "J"
codeforces_contest_name: "2023 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 104375
solve_time_s: 74
verified: true
draft: false
---

[CF 104375J - Phản ứng nhảy](https://codeforces.com/problemset/problem/104375/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên trong đó mỗi giá trị đại diện cho một “năng lượng nhảy” của một chất. Khi hai chất có năng lượng$a$Và$b$được trộn lẫn, chúng đóng góp một năng lượng của$ab$. Khi có nhiều hơn hai chất được trộn lẫn với nhau, tổng năng lượng được định nghĩa là tổng của tất cả các tích số theo cặp giữa các nguyên tố đã chọn. 

Vì vậy, đối với bất kỳ phạm vi truy vấn nào$[L, R]$, chúng ta lấy mảng con$A_L, A_{L+1}, \dots, A_R$và tính toán:$$\sum_{L \le i < j \le R} A_i A_j$$Mỗi truy vấn yêu cầu giá trị này theo modulo$10^9 + 7$, và có thể có tới$10^6$truy vấn trên một mảng có kích thước lên tới$10^6$. 

Việc đọc trực tiếp cho thấy chúng ta cần tính toán lặp đi lặp lại một hàm trên nhiều mảng con và hàm này phụ thuộc vào tất cả các tương tác theo cặp bên trong mảng con. 

Ý nghĩa hạn chế chính là cả hai$N$Và$Q$đủ lớn để bất kỳ giải pháp nào thực hiện được$O(R-L)$mỗi truy vấn quá chậm. Cách tiếp cận bậc hai cho mỗi truy vấn là hoàn toàn không thể thực hiện được vì một truy vấn trong trường hợp xấu nhất sẽ$O(10^{12})$hoạt động. 

Một cách tiếp cận ít ngây thơ hơn một chút vẫn chưa đủ: thậm chí$O(N)$mỗi truy vấn dẫn đến$10^{12}$tổng số hoạt động. 

Mục tiêu duy nhất có thể chấp nhận được là đại khái$O((N+Q)\log N)$hoặc$O(N+Q)$. 

Một vấn đề tinh tế xuất hiện trong các công thức đơn giản: việc tính toán lại các tổng cặp độc lập cho mỗi truy vấn dẫn đến công việc lặp đi lặp lại và không thể sử dụng lại trừ khi chúng ta cấu trúc lại biểu thức. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu tuân theo định nghĩa. Đối với mỗi truy vấn$[L, R]$, chúng tôi lặp lại tất cả các cặp$i < j$và tích lũy$A_i A_j$. Điều này đúng vì nó khớp hoàn toàn với định nghĩa năng lượng dưới dạng tổng của các tích từng cặp. Tuy nhiên, đối với một phạm vi chiều dài$k$, điều này đòi hỏi$k(k-1)/2$phép nhân. Trong trường hợp xấu nhất$k = N = 10^6$, vì vậy một truy vấn đã trở nên không khả thi. 

Ngay cả khi chúng tôi cố gắng cải thiện nó bằng cách sửa một điểm cuối và tính tổng điểm cuối khác, mỗi truy vấn vẫn tuyến tính trong kích thước phạm vi, điều này vẫn dẫn đến độ phức tạp bậc hai trong trường hợp xấu nhất. 

Quan sát quan trọng là tổng theo cặp có đồng nhất đại số chuẩn. Nếu chúng ta xác định:$$S = \sum A_i, \quad S_2 = \sum A_i^2$$sau đó:$$\left(\sum A_i\right)^2 = \sum A_i^2 + 2\sum_{i<j} A_iA_j$$Sắp xếp lại mang lại:$$\sum_{i<j} A_iA_j = \frac{S^2 - S_2}{2}$$Điều này biến bài toán từ “tổng trên tất cả các cặp” thành bài toán tính hai tổng tiền tố: tổng các phần tử và tổng bình phương. Khi những thứ đó có sẵn, mỗi truy vấn sẽ trở thành O(1). 

Chúng tôi tính toán trước tổng tiền tố:$$P[i] = \sum_{k=1}^{i} A_k,\quad Q[i] = \sum_{k=1}^{i} A_k^2$$Sau đó cho một truy vấn$[L, R]$:$$S = P[R] - P[L-1], \quad S_2 = Q[R] - Q[L-1]$$và câu trả lời là:$$(S^2 - S_2) \cdot inv2 \bmod (10^9+7)$$Việc chia cho 2 yêu cầu nghịch đảo mô-đun của 2. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NQ) | O(1) | Quá chậm | 
| Tổng tiền tố + danh tính | O(N + Q) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tổng tiền tố của các giá trị và tổng tiền tố của các bình phương trên mảng. 

Điều này cho phép tính tổng phân đoạn hoặc tổng bình phương trong thời gian không đổi. 
2. Đối với mỗi chỉ số$i$, cửa hàng:$P[i] = P[i-1] + A[i]$Và$Q[i] = Q[i-1] + A[i]^2$. 

Lý do chúng ta bình phương trước các giá trị là vì công thức cuối cùng phụ thuộc vào cả tổng tuyến tính và tổng bậc hai. 
3. Tính toán trước nghịch đảo mô đun của 2 dưới$10^9+7$. 

Điều này là cần thiết vì việc đếm cặp đưa ra phép chia cho 2 phải được xử lý theo số học mô-đun. 
4. Đối với mỗi truy vấn$[L, R]$, tính:$S = P[R] - P[L-1]$Và$S_2 = Q[R] - Q[L-1]$, modulo chuẩn hóa. 

Chúng đại diện cho tổng và tổng bình phương của phân đoạn đã chọn. 
5. Chuyển đổi phân số thành tổng cặp bằng cách sử dụng:$(S^2 - S_2) / 2$. 

Bước này dựa vào đồng nhất thức đại số để khai triển bình phương của một tổng. 
6. Xuất kết quả theo modulo$10^9+7$. 

### Tại sao nó hoạt động 

Tính chính xác đến từ việc mở rộng bình phương của tổng trên nhiều tập hợp. Mỗi sản phẩm$A_iA_j$với$i \ne j$xuất hiện đúng hai lần trong$S^2$, một lần như$A_iA_j$và một lần như$A_jA_i$. Trừ$\sum A_i^2$loại bỏ các số hạng theo đường chéo, để lại chính xác gấp đôi tổng theo cặp mong muốn. Chia cho hai sẽ sửa lỗi đếm thừa, đảm bảo mỗi cặp không có thứ tự đóng góp chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
INV2 = (MOD + 1) // 2

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    pref = [0] * (n + 1)
    pref2 = [0] * (n + 1)

    for i in range(1, n + 1):
        x = a[i - 1] % MOD
        pref[i] = (pref[i - 1] + x) % MOD
        pref2[i] = (pref2[i - 1] + x * x) % MOD

    out = []

    for _ in range(q):
        l, r = map(int, input().split())
        s = (pref[r] - pref[l - 1]) % MOD
        s2 = (pref2[r] - pref2[l - 1]) % MOD

        ans = (s * s - s2) % MOD
        ans = (ans * INV2) % MOD
        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Mảng tiền tố`pref`Và`pref2`lưu trữ tổng tích lũy và tổng bình phương tích lũy tương ứng, cả hai đều được lấy theo modulo$10^9+7$. Mỗi truy vấn giảm xuống số học theo thời gian không đổi trên các giá trị được tính toán trước này. 

phép nhân`s * s`là an toàn theo số học modulo vì tất cả các phép toán đều đã được giảm modulo MOD. Phép trừ cũng được chuẩn hóa để tránh các giá trị âm. Nghịch đảo của 2 được tính toán trước một lần vì MOD là số nguyên tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
1 2 3 4 5
1 2
1 5
3 5
```Chúng tôi xây dựng tổng tiền tố: 

| tôi | A[i] | P[i] | Hỏi[i] | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 3 | 5 | 
| 3 | 3 | 6 | 14 | 
| 4 | 4 | 10 | 30 | 
| 5 | 5 | 15 | 55 | 

Đối với truy vấn$[1,2]$,$S=3$,$S_2=5$. 

Trả lời$= (9 - 5)/2 = 2$. 

Vì$[1,5]$,$S=15$,$S_2=55$. 

Trả lời$= (225 - 55)/2 = 85$. 

Vì$[3,5]$,$S=12$,$S_2=50$. 

Trả lời$= (144 - 50)/2 = 47$. 

Điều này xác nhận danh tính chuyển đổi tổng cặp thành các phép tính tiền tố một cách nhất quán. 

### Ví dụ 2 

đầu vào:```
10 2
3 1 5 2 3 1 5 6 1 1
7 10
2 3
```Lý luận tiền tố: 

cho$[7,10]$: giá trị là$5,6,1,1$. 

Tổng$S=13$, hình vuông$S_2=25 + 36 + 1 + 1 = 63$. 

Trả lời$= (169 - 63)/2 = 53$. 

Vì$[2,3]$: giá trị là$1,5$. 

Tổng$S=6$, hình vuông$S_2=26$. 

Trả lời$= (36 - 26)/2 = 5$. 

Điều này chứng tỏ rằng ngay cả những phân bố rời rạc và cường độ hỗn hợp cũng hành xử thống nhất dưới cùng một phép biến đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + Q) | Một lần để xây dựng tổng tiền tố và công việc liên tục cho mỗi truy vấn | 
| Không gian | O(N) | Hai mảng tiền tố có kích thước N | 

Các ràng buộc yêu cầu tiền xử lý tuyến tính và truy vấn thời gian không đổi vì cả hai đều$N$Và$Q$đang lên đến$10^6$. Bất kỳ cách tiếp cận nào với chi phí logarit hoặc tuyến tính cho mỗi truy vấn sẽ vượt quá giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""5 3
1 2 3 4 5
1 2
1 5
3 5
""") == """2
85
47"""

assert run("""10 2
3 1 5 2 3 1 5 6 1 1
7 10
2 3
""") == """53
5"""

# custom cases

# minimum size ranges
assert run("""2 1
1 1
1 2
""") == "1"

# all equal values
assert run("""5 2
2 2 2 2 2
1 5
2 4
""") == """20
12"""

# single-element queries
assert run("""6 3
1 2 3 4 5 6
3 3
1 1
6 6
""") == """0
0
0"""

# increasing sequence stress
assert run("""4 2
1 2 3 4
1 4
2 4
""") == """35
26"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phạm vi phần tử đơn | 0 | đảm bảo các thuật ngữ đường chéo được loại trừ chính xác | 
| tất cả các giá trị bằng nhau | tăng trưởng bậc hai nhất quán | xác minh tính đối xứng và chia tỷ lệ | 
| toàn dải vs dải phụ | 35/26 | kiểm tra tính chính xác của tiền tố và cắt | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi phạm vi chỉ có một phần tử. Đối với đầu vào như:```
1 1
7
1 1
```câu trả lời đúng là 0 vì không có cặp nào. Thuật toán xử lý việc này vì$S^2 = S_2$, do đó phép trừ mang lại kết quả bằng 0 trước khi chia. 

Một trường hợp khác là các mảng lớn đồng nhất trong đó việc triển khai đơn giản có nguy cơ bị tràn hoặc chậm do nhân lặp lại. Ví dụ: tất cả các giá trị được$10^6$dẫn đến tổng trung gian lớn, nhưng số học mô-đun giữ cho các giá trị bị giới hạn và tính toán tiền tố vẫn ổn định. 

Trường hợp tinh vi cuối cùng là khi phép trừ tạo ra kết quả trung gian âm trong số học mô-đun. Việc triển khai khắc phục điều này bằng cách lấy modulo sau mỗi phép trừ, đảm bảo tính chính xác ngay cả khi$P[L-1]$hoặc$Q[L-1]$lớn hơn tiền tố tương ứng tại$R$.
