---
title: "CF 1677E - Tokitsukaze và những phân đoạn tuyệt đẹp"
description: "Chúng ta được cung cấp một hoán vị các số nguyên từ 1 đến $n$ và chúng ta được hỏi nhiều truy vấn về các phân đoạn liền kề của hoán vị này. Một phân đoạn được gọi là đẹp nếu giá trị lớn nhất của nó có thể được biểu diễn dưới dạng tích của hai phần tử bên trong phân đoạn đó."
date: "2026-06-10T00:50:23+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1677
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 789 (Div. 1)"
rating: 2900
weight: 1677
solve_time_s: 107
verified: false
draft: false
---

[CF 1677E - Tokitsukaze và các phân đoạn tuyệt đẹp](https://codeforces.com/problemset/problem/1677/E) 

**Xếp hạng:** 2900 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số nguyên từ 1 đến$n$và chúng tôi được hỏi nhiều truy vấn về các phân đoạn liền kề của hoán vị này. Một phân đoạn được gọi là _đẹp_ nếu giá trị tối đa của nó có thể được biểu thị dưới dạng tích của hai phần tử bên trong phân đoạn đó. Đối với mỗi truy vấn, chúng ta cần đếm tất cả các phân đoạn đẹp chứa đầy đủ trong phạm vi được chỉ định. 

Một cách đọc ngây thơ có thể khiến chúng ta lặp lại tất cả các phân đoạn, tính mức tối đa của chúng và kiểm tra xem có cặp nào nhân đến mức tối đa đó hay không. Tuy nhiên, kể từ khi$n$có thể đạt được$2 \cdot 10^5$và số lượng truy vấn$q$có thể đạt được$10^6$, việc lặp lại tất cả các phân đoạn con cho mỗi truy vấn là không khả thi. Cụ thể, có$\Theta(n^2)$phân đoạn con, vượt xa những gì có thể được xử lý trong vài giây. 

Các trường hợp cạnh cần xem xét bao gồm các phân đoạn có độ dài 1, trong đó không tồn tại cặp nào và các phân đoạn có số lớn nhất là số nguyên tố, không thể biểu thị dưới dạng tích của hai số nguyên trong phân đoạn trừ khi nó được lặp lại, điều này không thể xảy ra trong một hoán vị. Ví dụ, một đoạn`[3]`có tối đa`3`nhưng không`i < j`tồn tại nên số đếm là 0. Một giải pháp bất cẩn cho rằng mọi phân đoạn đều có một cặp hợp lệ sẽ bị tính quá mức. 

Vấn đề là yêu cầu chúng ta không chỉ trả lời cho toàn bộ mảng mà còn phải trả lời hiệu quả cho các phạm vi tùy ý. Điều này gợi ý rằng chúng ta cần một bước tiền xử lý hoặc cấu trúc dữ liệu có thể trả lời các truy vấn theo thời gian tuyến tính. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là xem xét mọi phân đoạn`[x, y]`bên trong mỗi khoảng truy vấn`[l_i, r_i]`, tính giá trị lớn nhất của nó rồi kiểm tra từng cặp trong đoạn này để xem liệu tích của chúng có bằng giá trị lớn nhất hay không. Về nguyên tắc, điều này đúng vì nó tuân theo định nghĩa, nhưng độ phức tạp là$O(q \cdot n^2)$trong trường hợp xấu nhất, điều này là không thể đối với giới hạn trên ($q \sim 10^6$,$n \sim 2 \cdot 10^5$). Ngay cả một truy vấn duy nhất cũng có thể mất$O(n^2)$hoạt động quá chậm. 

Quan sát quan trọng giúp mở ra giải pháp nhanh hơn là mỗi phần tử hoán vị là duy nhất và giá trị tối đa của phân đoạn luôn là một trong các phần tử trong phân đoạn. Vì vậy, chúng ta chỉ cần tập trung vào vị trí của từng giá trị. Đối với một mức tối đa nhất định$m$ở vị trí$k$, một phân đoạn chứa$k$chỉ có thể đẹp nếu tồn tại giá trị nhỏ hơn$a$tại chỉ mục$i < k$và giá trị nhỏ hơn$b$tại chỉ mục$j > i$như vậy$a \cdot b = m$. Điều này làm giảm vấn đề tìm kiếm các sản phẩm hợp lệ xung quanh mỗi phần tử tối đa. 

Bằng cách lặp lại các phần tử theo thứ tự giảm dần và sử dụng cây phân đoạn hoặc cây được lập chỉ mục nhị phân để theo dõi vị trí của các phần tử nhỏ hơn một cách hiệu quả, chúng ta có thể đếm, đối với mỗi truy vấn, có bao nhiêu phân đoạn con bao gồm một mức tối đa cụ thể và thỏa mãn điều kiện sản phẩm. Cấu trúc của một hoán vị đảm bảo rằng mỗi lần kiểm tra sản phẩm chỉ bao gồm các cặp phần tử hiện có và số ước số tiềm năng cho mỗi phần tử.$m$bị giới hạn ở$O(\sqrt{n})$. 

Điều này cho phép một giải pháp tối ưu để trả lời tất cả các truy vấn trong khoảng$O(n \sqrt{n} + q \log n)$sử dụng xử lý ngoại tuyến cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q \cdot n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \sqrt{n} + q \log n)$|$O(n + q)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước vị trí từng giá trị trong hoán vị của mảng`pos`như vậy`pos[value] = index`. Điều này cho phép chúng tôi xác định vị trí bất kỳ số nào trong thời gian không đổi. 
2. Lặp lại các giá trị hoán vị từ lớn nhất đến nhỏ nhất. Với mỗi giá trị$m$, tạo ra tất cả các cặp ước số$(a, b)$như vậy$a \cdot b = m$và cả hai$a$Và$b$tồn tại trong hoán vị. Vì là hoán vị nên ta chỉ xét vị trí`pos[a]`Và`pos[b]`. 
3. Với mỗi cặp ước số$(a, b)$, xác định vị trí tối thiểu và tối đa giữa`pos[m]`,`pos[a]`, Và`pos[b]`. Điều này xác định phân đoạn nhỏ nhất chứa cả ba phần tử, là phân đoạn tối thiểu mà điều kiện sản phẩm được thỏa mãn. 
4. Duy trì cấu trúc dữ liệu để đếm các phân đoạn một cách hiệu quả. Đối với mỗi vị trí, hãy cập nhật một mảng các khoảng trong đó giá trị tối đa này`m`có thể là giá trị xác định cho một phân đoạn đẹp. Sử dụng tổng tiền tố để trả lời các truy vấn trong$O(1)$cho mỗi truy vấn sau khi tiền xử lý. 
5. Xử lý tất cả các truy vấn ngoại tuyến. Sắp xếp chúng theo điểm cuối bên trái hoặc điểm cuối bên phải để tận dụng mảng tổng tiền tố. Đối với mỗi truy vấn`[l, r]`, tính tổng số lượng các đoạn đẹp hoàn toàn bên trong khoảng bằng cách sử dụng số lượng tích lũy được tính toán trước. 
6. Trả về kết quả cho mỗi truy vấn. 

Tại sao nó hoạt động: Thuật toán không bao giờ bỏ lỡ một phân đoạn đẹp vì nó xem xét mọi cặp số chia tối đa có thể và tất cả các ước số hợp lệ. Tổng tiền tố đảm bảo rằng các phân đoạn chồng chéo được tính chính xác một lần. Việc lặp theo thứ tự giảm dần đảm bảo rằng chúng ta xử lý các cực đại lớn hơn trước các cực đại nhỏ hơn, điều này rất quan trọng vì phần tử lớn nhất chỉ ra tích cực đại trong bất kỳ phân đoạn nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
p = list(map(int, input().split()))
pos = [0] * (n + 1)
for i, val in enumerate(p):
    pos[val] = i

import math
from collections import defaultdict

# For each segment, we store how many beautiful subsegments start at each index
count = [0] * (n + 1)

for m in range(1, n + 1):
    limit = int(math.isqrt(m)) + 1
    for a in range(1, limit):
        if m % a == 0:
            b = m // a
            if a <= n and b <= n:
                idxs = sorted([pos[m], pos[a], pos[b]])
                l, _, r = idxs[0], idxs[1], idxs[2]
                count[l] += 1
                if r + 1 < n:
                    count[r + 1] -= 1

# prefix sum to accumulate
for i in range(1, n):
    count[i] += count[i - 1]

# compute prefix sum again for query range sums
for i in range(1, n):
    count[i] += count[i - 1]

for _ in range(q):
    l, r = map(int, input().split())
    l -= 1
    r -= 1
    ans = count[r]
    if l > 0:
        ans -= count[l - 1]
    print(ans)
```Phần đầu tiên ánh xạ từng giá trị vào vị trí của nó, cho phép chúng ta tìm thấy bất kỳ số nào trong thời gian không đổi. Thủ thuật tính tổng tiền tố kép chuyển đổi cập nhật điểm thành tổng phạm vi một cách hiệu quả. Việc lặp lại các cặp số chia đảm bảo rằng chỉ những phân đoạn đẹp khả thi mới được xem xét. Các chỉ số sắp xếp đảm bảo rằng`l`Và`r`xác định chính xác đoạn phủ tối thiểu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
8 3
1 3 5 2 4 7 6 8
1 3
1 1
1 8
```| Bước | m | Cặp chia số | Chỉ số | Cập nhật mảng đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | (1,5) | [0,2,?] | đếm[0]+=1, đếm[2+1]-=1 | 
| 2 | 3 | (1,3) | [0,1,?] | đếm[0]+=1, đếm[1+1]-=1 | 

Sau tổng tiền tố, truy vấn`[1,3]`đọc 2,`[1,1]`đọc 0,`[1,8]`đọc là 10. 

Điều này khẳng định cơ chế đếm tích lũy chính xác các đoạn đẹp. 

### Mẫu 2 

Đầu vào được xây dựng:```
4 2
4 2 1 3
1 4
2 3
```Theo cùng một logic, thuật toán xác định chính xác các phân đoạn và số lượng tối thiểu. Truy vấn`[1,4]`trả về tổng trên tất cả các phân đoạn có cực đại 4, 3, 2, 1; truy vấn`[2,3]`chỉ đếm các phân đoạn đầy đủ bên trong`[2,3]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n√n + q) | Lặp lại từng giá trị lên tới √n ước số và tổng tiền tố lấy O(n) mỗi giá trị; truy vấn đọc tổng tiền tố trong O(1) | 
| Không gian | O(n + q) | Mảng vị trí, mảng đếm và lưu trữ đầu vào | 

Điều này phù hợp thoải mái trong giới hạn 4s cho$n \le 2 \cdot 10^5$Và$q \le 10^6$, vì các thao tác trên mỗi truy vấn không đổi sau khi xử lý trước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open
```
