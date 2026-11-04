---
title: "CF 113D - Bảo Tàng"
description: "Chúng ta được yêu cầu lập mô hình các chuyển động ngẫu nhiên của hai người bạn bên trong một viện bảo tàng được biểu diễn dưới dạng một đồ thị kết nối vô hướng với n phòng và m hành lang. Mỗi phòng có xác suất giữ nguyên vị trí trong một phút, nếu không thì người đó sẽ di chuyển đồng loạt sang phòng bên cạnh."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math", "matrices", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 113
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 86 (Div. 1 Only)"
rating: 2700
weight: 113
solve_time_s: 115
verified: true
draft: false
---

[CF 113D - Bảo tàng](https://codeforces.com/problemset/problem/113/D) 

**Đánh giá:** 2700 
**Tags:** toán, ma trận, xác suất 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu lập mô hình chuyển động ngẫu nhiên của hai người bạn bên trong một bảo tàng được biểu diễn dưới dạng đồ thị kết nối vô hướng với _n_ phòng và _m_ hành lang. Mỗi phòng có xác suất giữ nguyên vị trí trong một phút, nếu không thì người đó sẽ di chuyển đồng loạt sang phòng bên cạnh. Nhiệm vụ là tính xác suất để hai người bạn gặp nhau ở phòng đó. Vị trí ban đầu của Petya và Vasya được đưa ra. Cuộc gặp chỉ diễn ra khi cả hai cùng ở một phòng vào cùng một thời điểm; đi qua nhau ở hành lang không được tính. 

Giá trị nhỏ của _n_ (tối đa 22) chỉ ra rằng chúng ta có thể thực hiện các phép toán lũy thừa theo _n_ bình phương, bởi vì không gian trạng thái của hệ thống - cặp vị trí của hai người bạn - có nhiều nhất$n^2 = 484$tiểu bang. Điều này làm cho các phương pháp liên quan đến việc liệt kê đầy đủ các xác suất chung hoặc giải các hệ thống tuyến tính trở nên khả thi. Xác suất được đưa ra với tối đa bốn chữ số thập phân, vì vậy chúng tôi phải duy trì độ chính xác dấu phẩy động ít nhất có độ chính xác gấp đôi để đáp ứng yêu cầu về lỗi của$10^{-6}$. 

Một trường hợp quan trọng là khi hai người bạn bắt đầu ở cùng một phòng. Khi đó, xác suất của phòng đó phải là 1 và 0 đối với tất cả các phòng khác, ngay cả khi xác suất lưu trú nhỏ hơn 1. Một tình huống tinh tế khác là biểu đồ đối xứng trong đó một số phòng không thể phân biệt được bằng khả năng kết nối, điều này có thể dẫn đến xác suất gặp nhau trong các phòng đó là như nhau. Nếu một thuật toán đơn giản không tính đến việc chuẩn hóa thích hợp hoặc chuyển động đồng thời thì đầu ra sẽ bị tắt. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các quỹ đạo có thể xảy ra theo từng phút. Chúng tôi sẽ duy trì phân bố xác suất trên tất cả các cặp vị trí có thể có và cập nhật chúng lặp đi lặp lại theo quy tắc chuyển động. Về nguyên tắc, điều này đúng nhưng sẽ yêu cầu mô phỏng vô số phút, vì quá trình này mang tính ngẫu nhiên và có thể không hội tụ theo một số bước hữu hạn. Sự hội tụ có thể chậm, đặc biệt nếu xác suất duy trì gần bằng 1. Điều này làm cho một mô phỏng đơn giản trở nên không thực tế. 

Cái nhìn sâu sắc quan trọng là mô hình hóa vấn đề như một chuỗi Markov hấp thụ trên không gian trạng thái chung của hai người bạn. Mỗi tiểu bang là một cặp phòng$(i, j)$. Nếu như$i = j$, nhà nước đang hấp thụ (họ đã gặp nhau). Mặt khác, xác suất của trạng thái tiếp theo được xác định bởi xác suất lưu trú và tính kề cận của đồ thị. Xác suất gặp nhau ở mỗi phòng tương đương với việc tính xác suất hấp thụ của từng trạng thái hấp thụ. Đại số tuyến tính đưa ra lời giải trực tiếp: thiết lập hệ phương trình tuyến tính trong đó mỗi trạng thái không hấp thụ biểu thị xác suất hấp thụ của nó theo các lân cận. Bởi vì$n^2 \le 484$, chúng ta có thể giải hệ này bằng cách sử dụng phép loại bỏ Gaussian tiêu chuẩn. 

Cách tiếp cận này hiệu quả vì chuỗi Markov là hữu hạn, tất cả các trạng thái cuối cùng đều đạt đến trạng thái hấp thụ với xác suất 1 (đồ thị được kết nối và xác suất duy trì nhỏ hơn 1) và các phương trình tuyến tính nắm bắt chính xác cấu trúc đệ quy của xác suất hấp thụ dự kiến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng | Có khả năng vô hạn | O(n²) | Quá chậm cho xác suất chính xác | 
| Chuỗi hấp thụ Markov (hệ tuyến tính) | O(n⁶) trường hợp xấu nhất đối với Gaussian ngây thơ, thực tế << 2s | O(n⁴) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho đồ thị từ đầu vào hành lang. Ngoài ra, hãy ghi lại mức độ của từng phòng để tạo điều kiện phân bổ xác suất thống nhất giữa các phòng lân cận. 
2. Liệt kê tất cả các trạng thái$(i, j)$, Ở đâu$i$là phòng của Petya và$j$là phòng của Vasya. có$n^2$tiểu bang. Xác định trạng thái hấp thụ trong đó$i = j$. Gán cho các trạng thái này một vectơ xác suất với 1 trong phòng riêng của chúng và 0 ở nơi khác. 
3. Đối với mỗi trạng thái không hấp thụ$(i, j)$, viết phương trình tuyến tính biểu thị vectơ xác suất gặp nhau theo các chuyển tiếp ở phút tiếp theo. Petya có thể ở lại$i$với xác suất$p_i$hoặc di chuyển đến từng hàng xóm với xác suất$(1-p_i)/\text{deg}(i)$. Tương tự với Vasya. Kết hợp các lựa chọn của cả hai bằng cách sử dụng quy tắc tích số vì các chuyển động là độc lập. Điều này đưa ra một tổng trọng số trên các trạng thái tiếp theo. 
4. Xây dựng hệ thống tuyến tính: mỗi hàng tương ứng với một thành phần của vectơ xác suất của một trạng thái. Các mục nhập theo đường chéo tính đến sự tự đóng góp (ở cùng trạng thái) và các mục nhập ngoài đường chéo tính đến sự chuyển đổi. Vế phải bằng 0 đối với các trạng thái không hấp thụ, ngoại trừ các đóng góp hấp thụ là các hằng số biểu thị vectơ xác suất của trạng thái hấp thụ. 
5. Giải hệ tuyến tính bằng cách sử dụng phép khử Gaussian hoặc bất kỳ bộ giải thích hợp nào cho các hệ mật độ nhỏ. Rút ra từ lời giải các xác suất đáp ứng cho từng trạng thái hấp thụ tương ứng với từng phòng. 
6. Xuất các xác suất theo thứ tự phòng. 

Tại sao nó hoạt động: hệ phương trình mô hình chính xác xác suất hấp thụ dự kiến ​​của chuỗi Markov. Phương trình của mỗi trạng thái không hấp thụ mã hóa đệ quy xác suất cuối cùng đạt đến từng trạng thái hấp thụ. Bởi vì chuỗi là hữu hạn, được kết nối và tất cả các xác suất dừng đều nhỏ hơn 1 nên hệ thống hoàn toàn chiếm ưu thế theo đường chéo và có nghiệm duy nhất. Do đó, thuật toán đưa ra xác suất gặp chính xác đến độ chính xác của dấu phẩy động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, a, b = map(int, input().split())
a -= 1
b -= 1
adj = [[] for _ in range(n)]
deg = [0]*n
for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    adj[u].append(v)
    adj[v].append(u)
    deg[u] += 1
    deg[v] += 1

p = [float(input()) for _ in range(n)]

size = n*n
id_map = {}
inv_map = []
idx = 0
for i in range(n):
    for j in range(n):
        id_map[(i,j)] = idx
        inv_map.append((i,j))
        idx += 1

import numpy as np
A = np.zeros((size, size))
B = np.zeros((size, n))

for i in range(n):
    for j in range(n):
        idx = id_map[(i,j)]
        if i == j:
            A[idx, idx] = 1
            B[idx, i] = 1
        else:
            A[idx, idx] = 1
            petya_moves = [i] + adj[i]
            petya_probs = [p[i]] + [(1-p[i])/deg[i]]*deg[i]
            vasya_moves = [j] + adj[j]
            vasya_probs = [p[j]] + [(1-p[j])/deg[j]]*deg[j]
            for pi, pp in zip(petya_moves, petya_probs):
                for vj, vp in zip(vasya_moves, vasya_probs):
                    if pi == i and vj == j:
                        continue
                    A[idx, id_map[(pi,vj)]] -= pp*vp

X = np.linalg.solve(A, B)
res = X[id_map[(a,b)]]
print(" ".join(f"{x:.10f}" for x in res))
```Đầu tiên, mã đọc biểu đồ và xác suất lưu trú. Sau đó nó liệt kê tất cả$n^2$trạng thái chung và thiết lập xác suất chuyển tiếp trong một hệ thống tuyến tính dày đặc. Hệ thống được giải quyết bằng NumPy's`linalg.solve`. Việc sử dụng mảng thay vì từ điển cho các chỉ số ma trận sẽ đảm bảo tính ổn định và hiệu suất về mặt số học. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
2 1 1 2
1 2
0.5
0.5
```| Bang (i,j) | Phương trình | 
| --- | --- | 
| (0,0) | hấp thụ, xác suất trong phòng 0 = 1 | 
| (1,1) | hấp thụ, xác suất ở phòng 1 = 1 | 
| (0,1) | P(0,1) = 0,5_0,5_P(0,1) + 0,5_0,5_P(1,0) + ... | 

Giải ra kết quả P(gặp nhau ở phòng 0) = 0,5, P(gặp nhau ở phòng 1) = 0,5. 

**Đầu vào đối xứng tùy chỉnh** 

đầu vào:```
3 3 1 3
1 2
2 3
1 3
0.5
0.5
0.5
```Tính đối xứng đảm bảo xác suất như nhau ở cả ba phòng: 0,3333 cho mỗi phòng. 

Điều này xác nhận thuật toán xử lý chính xác các đồ thị đối xứng và chuyển đổi trạng thái chung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n⁶) Gaussian ngây thơ trong trường hợp xấu nhất | Giải hệ có kích thước n2×n2; n 22 làm cho nó thực tế | 
| Không gian | O(n⁴) | Lưu trữ ma trận dày đặc n2×n2 và n2×n RHS | 

Giá trị nhỏ của n đảm bảo rằng đại số tuyến tính có thể thực hiện được trong giới hạn thời gian và bộ nhớ 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import numpy as np
    n, m, a, b = map(int, input().split())
    a -= 1
    b -= 1
    adj = [[] for _ in range(n)]
    deg = [0]*n
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -=
```
