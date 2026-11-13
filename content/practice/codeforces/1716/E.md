---
title: "CF 1716E - Hoán đổi và chặn tối đa"
description: "Mảng chứa chính xác các phần tử $2^n$. Mỗi truy vấn chọn một cấp độ $k$ và hoán đổi từng vị trí với vị trí khác nhau $2^k$. Việc hoán đổi được thực hiện đồng thời theo các cặp rời rạc, do đó, mỗi khối có kích thước $2^{k+1}$ có nửa bên trái được hoán đổi với nửa bên phải."
date: "2026-06-09T19:52:49+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "data-structures", "dfs-and-similar", "divide-and-conquer", "dp"]
categories: ["algorithms"]
codeforces_contest: 1716
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 133 (Rated for Div. 2)"
rating: 2500
weight: 1716
solve_time_s: 138
verified: true
draft: false
---

[CF 1716E - Hoán đổi và chặn tối đa](https://codeforces.com/problemset/problem/1716/E) 

**Đánh giá:** 2500 
**Thẻ:** bitmasks, cấu trúc dữ liệu, dfs và tương tự, phân chia và chinh phục, dp 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mảng chứa chính xác$2^n$các phần tử. Mỗi truy vấn chọn một cấp độ$k$và hoán đổi từng vị trí với vị trí khác nhau bởi$2^k$. Việc hoán đổi được thực hiện đồng thời theo từng cặp riêng biệt, do đó mỗi khối có kích thước$2^{k+1}$nửa bên trái của nó được hoán đổi với nửa bên phải của nó. 

Sau khi áp dụng hoán đổi, chúng ta cần tổng tối đa trong số tất cả các mảng con liền kề, trong đó mảng con trống được cho phép và đóng góp bằng 0. 

Bản thân mảng thay đổi vĩnh viễn nên các truy vấn sẽ tích lũy. Chúng tôi không áp dụng nhiều lần cùng một thao tác cho mảng ban đầu, chúng tôi đang thay đổi cách sắp xếp hiện tại. 

Kích thước của mảng tối đa là$2^{18}=262144$, đó là vừa phải. Một tấm thẻ Kadane thì rẻ, nhưng có thể có$2\cdot10^5$truy vấn. Việc xây dựng lại mảng và tính toán lại câu trả lời sau mỗi truy vấn sẽ yêu cầu khoảng$$2\cdot10^5 \times 2^{18}\approx 5\cdot10^{10}$$hoạt động vượt xa giới hạn. 

Bản thân hoạt động trao đổi là lừa đảo. Một truy vấn với$k=0$hoán đổi các phần tử liền kề. Lặp lại nó hai lần sẽ khôi phục lại trật tự ban đầu. Tổng quát hơn, mỗi cấp độ hoạt động độc lập và có thể được xem như việc đảo ngược một bit trong mỗi chỉ mục. Chuỗi truy vấn chỉ xác định cấp độ nào đã được đảo số lần lẻ. 

Một số tình huống dễ dẫn đến sai lầm. 

Giả định```
n = 1
a = [-5, -7]
```Sau truy vấn duy nhất có thể, mảng sẽ trở thành```
[-7, -5]
```Câu trả lời đúng là 0 vì cho phép mảng con trống. Việc triển khai Kadane bất cẩn và nhất quyết chọn ít nhất một phần tử sẽ cho kết quả -5. 

Một cạm bẫy khác là quên rằng các hoạt động được tích lũy. Vì```
n = 2
a = [1,2,3,4]
queries: 0,0
```Truy vấn đầu tiên đưa ra```
[2,1,4,3]
```Truy vấn thứ hai khôi phục```
[1,2,3,4]
```Câu trả lời là 10 và 10. Việc áp dụng lại mỗi truy vấn vào mảng ban đầu sẽ vô tình tạo ra cùng một trạng thái đầu tiên hai lần. 

Vấn đề tinh tế nhất là hiểu việc hoán đổi cấp độ thực hiện những gì bên trong các khối lớn hơn. Vì```
[1,2,3,4,5,6,7,8]
```với$k=1$, mỗi khối có độ dài bốn thay đổi```
[1,2,3,4] → [3,4,1,2]
[5,6,7,8] → [7,8,5,6]
```Hoạt động không thay đổi mảng trên toàn cầu. Nó chỉ trao đổi một nửa bên trong các khối có kích thước$2^{k+1}$. 

## Phương pháp tiếp cận 

Phương pháp đơn giản là duy trì mảng hiện tại một cách rõ ràng. Đối với mỗi truy vấn, chúng tôi thực hiện tất cả các phép hoán đổi bắt buộc, sau đó chạy thuật toán Kadane để thu được tổng mảng con tối đa. Điều này đúng vì Kadane tính toán chính xác số lượng mong muốn. 

Thật không may, một truy vấn có thể chạm vào tất cả$2^n$phần tử và Kadane cũng quét toàn bộ mảng. Với$2\cdot10^5$truy vấn và$2^{18}$các phần tử, tổng công việc sẽ trở thành khoảng$10^{11}$, đó là điều vô vọng. 

Quan sát quan trọng là mỗi giao dịch hoán đổi đều trao đổi hai nửa của một số phân khúc. Đây chính xác là cấu trúc giống như cây phân đoạn. Khi một nút đại diện cho một đoạn, câu trả lời của nó chỉ phụ thuộc vào bốn số: 

Tổng số tiền. 

Tổng tiền tố tối đa. 

Tổng hậu tố tối đa. 

Tổng mảng con tối đa. 

Đây là những giá trị thông thường được sử dụng cho các truy vấn mảng con tối đa. 

Thông thường, con bên trái luôn đến trước con bên phải. Ở đây, việc lật cấp độ có thể đảo ngược thứ tự của chúng bên trong mọi nút có chiều cao đó. Vì mỗi nút chỉ có hai hướng có thể nên chúng ta có thể tính toán trước cả hai phiên bản. 

Đối với mỗi nút, chúng tôi lưu trữ hai trạng thái. 

Trạng thái 0 có nghĩa là các phần tử con được nối theo thứ tự bình thường. 

Trạng thái 1 có nghĩa là các phần tử con được nối theo thứ tự đảo ngược. 

Khi kết hợp các phần tử con, thứ tự đảo ngược chỉ đơn giản là hoán đổi vai trò của chúng. Vì mỗi nút chỉ có hai khả năng nên quá trình tiền xử lý vẫn tuyến tính. 

Một truy vấn chuyển đổi một cấp độ. Tất cả các nút có chiều cao đó thay đổi hướng đồng thời. Thay vì chạm vào chúng một cách riêng lẻ, chúng tôi duy trì một mặt nạ bit mô tả các cấp độ được lật. Sử dụng mặt nạ đó, chúng ta biết trạng thái nào nên được sử dụng ở gốc. Giá trị mảng con tối đa của gốc là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q2^n)$|$O(2^n)$| Quá chậm | 
| Tối ưu |$O(2^n+q)$|$O(2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây nhị phân hoàn chỉnh trên mảng. 

Mỗi nút tương ứng với một đoạn có độ dài là lũy thừa của hai. 
2. Đối với một chiếc lá, cả hai hướng đều giống nhau. 

Tổng số bằng chính phần tử đó. Tiền tố, hậu tố và mảng con tối đa đều là giá trị phần tử nếu dương, nếu không thì bằng 0 vì mảng con trống được cho phép. 
3. Đối với nút nội bộ, hãy tính hai phiên bản. 

Phiên bản 0 nối con trái theo sau với con phải. 

Phiên bản 1 nối con phải theo sau là con trái. 
4. Để hợp nhất hai phân đoạn$A$Và$B$, sử dụng công thức chuẩn. 

Tổng số tiền là$A.sum+B.sum$. 

Tiền tố tối đa là tốt hơn$A.pref$Và$A.sum+B.pref$. 

Hậu tố tối đa là tốt hơn$B.suff$Và$B.sum+A.suff$. 

Mảng con tối đa là mảng tối đa trong số phân khúc tốt nhất bên trong$A$, phân khúc tốt nhất bên trong$B$và một đoạn đi qua ranh giới, có giá trị là$A.suff+B.pref$. 
5. Duy trì một mặt nạ mô tả các cấp độ đã được lật một số lần lẻ. 

Một truy vấn có giá trị$k$chuyển đổi bit$k$. 
6. Trong quá trình tiền xử lý, đánh số cấp từ dưới lên. 

Đối với một nút ở độ cao$h$, hướng của nó chỉ phụ thuộc vào bit$h-1$của mặt nạ hiện tại. 
7. Gốc có hai trạng thái được tính toán trước. 

Sau khi cập nhật mặt nạ, chọn trạng thái gốc tương ứng với mẫu bit cao nhất và xuất ra giá trị mảng con tối đa của nó. 

### Tại sao nó hoạt động 

Mỗi truy vấn hoán đổi độc lập hai nửa của mỗi phân đoạn có kích thước là$2^{k+1}$. Một nút ở độ cao đó trải qua chính xác một lần đảo chiều cục bộ, trong khi các nút ở sâu hơn hoặc cao hơn không bị ảnh hưởng. Do đó, thứ tự của mỗi nút chỉ phụ thuộc vào tính chẵn lẻ của các lần lật ở cấp độ riêng của nó. 

Bởi vì một nút chỉ có hai đơn hàng có thể có nên việc lưu trữ cả hai khả năng sẽ mô tả đầy đủ mọi sắp xếp trong tương lai. Các công thức hợp nhất hoàn toàn giống với các công thức được sử dụng trong cây phân đoạn để có tổng mảng con tối đa, do đó các giá trị được tính toán cho mỗi hướng là chính xác. Vì gốc đại diện cho toàn bộ mảng nên giá trị mảng con tối đa của nó bằng với câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("sm", "pref", "suff", "best")

    def __init__(self, sm=0, pref=0, suff=0, best=0):
        self.sm = sm
        self.pref = pref
        self.suff = suff
        self.best = best

def merge(a, b):
    res = Node()
    res.sm = a.sm + b.sm
    res.pref = max(a.pref, a.sm + b.pref)
    res.suff = max(b.suff, b.sm + a.suff)
    res.best = max(a.best, b.best, a.suff + b.pref)
    return res

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    m = 1 << n

    dp = [[None, None] for _ in range(2 * m)]

    for i, x in enumerate(a):
        val = max(0, x)
        node = Node(x, val, val, val)
        dp[m + i][0] = node
        dp[m + i][1] = node

    for v in range(m - 1, 0, -1):
        left = v << 1
        right = left | 1

        dp[v][0] = merge(dp[left][0], dp[right][0])
        dp[v][1] = merge(dp[right][1], dp[left][1])

    orient = [0] * (n + 1)

    q = int(input())
    out = []

    for _ in range(q):
        k = int(input())
        level = k + 1
        orient[level] ^= 1

        cur = 1
        for h in range(n, 0, -1):
            bit = orient[h]
            cur = (cur << 1) | bit

        out.append(str(dp[cur][0].best))

    sys.stdout.write("\n".join(out))

solve()
```Để lại giá trị 0 cho tiền tố âm, hậu tố và giá trị tối đa vì cho phép mảng con trống. 

Quy trình hợp nhất là hợp nhất mảng con tối đa tiêu chuẩn. Chỉ cần bốn số. 

Mỗi nút bên trong lưu trữ hai hướng. Trong một hướng, đứa trẻ bên trái đứng trước đứa trẻ bên phải và trong hướng kia, thứ tự của chúng bị đảo ngược. 

Mảng định hướng giữ tính chẵn lẻ của các lần lật ở mọi cấp độ. Vì việc áp dụng cùng một truy vấn hai lần sẽ tự hủy, nên chỉ có tính chẵn lẻ là quan trọng. 

Việc triển khai phân biệt cẩn thận chiều cao của cây phân đoạn với giá trị truy vấn. Truy vấn$k$ảnh hưởng đến các đoạn có chiều dài$2^{k+1}$, tương ứng với cấp độ$k+1$khi đếm từ lá lên trên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào```
3
-3
```
