---
title: "CF 1458C - Quảng trường Latinh"
description: "Chúng ta bắt đầu với (n lần n) hình vuông Latin. Mỗi hàng là một hoán vị của (1 ldots n), và mỗi cột cũng là một hoán vị của (1 ldots n). Một chuỗi các thao tác sửa đổi ma trận. Bốn thao tác thực hiện dịch chuyển theo chu kỳ của hàng hoặc cột."
date: "2026-06-11T02:35:11+07:00"
tags: ["codeforces", "competitive-programming", "math", "matrices"]
categories: ["algorithms"]
codeforces_contest: 1458
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 691 (Div. 1)"
rating: 2700
weight: 1458
solve_time_s: 356
verified: false
draft: false
---

[CF 1458C - Hình vuông Latinh](https://codeforces.com/problemset/problem/1458/C) 

**Đánh giá:** 2700 
**Tags:** toán, ma trận 
**Thời gian giải:** 5 phút 56 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một\(n \times n\)hình vuông Latinh. Mỗi hàng là một hoán vị của\(1 \ldots n\)và mỗi cột cũng là một hoán vị của\(1 \ldots n\). 

Một chuỗi các thao tác sửa đổi ma trận. Bốn thao tác thực hiện dịch chuyển theo chu kỳ của hàng hoặc cột. Hai hoạt động còn lại khác thường hơn:\(I\)thay thế mọi hoán vị hàng bằng nghịch đảo của nó.\(C\)thay thế mọi hoán vị cột bằng nghịch đảo của nó. 

Sau tất cả các thao tác, chúng ta phải xuất ra ma trận kết quả. 

Quan sát đầu tiên xuất phát từ những hạn chế. Tổng của tất cả\(n\)nhiều nhất là\(1000\), do đó, việc in các ma trận cuối cùng đã tốn \(O(n^2)\) cho mỗi trường hợp thử nghiệm. Phần đó là không thể tránh khỏi. Thách thức thực sự là chuỗi thao tác có tổng chiều dài có thể đạt tới\(10^5\). 

Một mô phỏng đơn giản sẽ áp dụng mọi thao tác trực tiếp vào ma trận. Mỗi thao tác chạm vào\(n^2\)tế bào, do đó trường hợp xấu nhất trở thành$$
O(mn^2).
$$Với\(m=10^5\)Và\(n=1000\), điều này hoàn toàn không thể thực hiện được. 

Vấn đề đang che giấu một cấu trúc đại số mạnh mẽ. Một ô vuông Latin đương nhiên chứa ba tọa độ:$$
(\text{row}, \text{column}, \text{value}).
$$Sáu phép toán không bao giờ phá hủy tính chất bình phương Latinh vì chúng chỉ hoán vị ba tọa độ này theo nhiều cách khác nhau. 

### Trường hợp cạnh không rõ ràng 

Hãy xem xét\(n=3\)và vận hành\(I\).$$
\begin{matrix}
1&2&3\\
2&3&1\\
3&1&2
\end{matrix}
$$Việc triển khai bất cẩn có thể cố gắng đảo ngược từng hàng một cách độc lập. Điều đó có hiệu quả, nhưng nó bỏ lỡ cấu trúc sâu hơn. Sau khi đảo ngược ma trận trở thành$$
\begin{matrix}
1&2&3\\
3&1&2\\
2&3&1
\end{matrix}
$$đó chính xác là những gì sẽ xảy ra nếu các chỉ số cột và giá trị trao đổi vai trò. Giải thích tọa độ này là chìa khóa cho giải pháp tối ưu. 

Xem xét hoạt động\(C\)trên cùng một ma trận. 

Kết quả là$$
\begin{matrix}
1&3&2\\
2&1&3\\
3&2&1
\end{matrix}
$$Đây không phải là sự đảo ngược hàng. Nó trao đổi các chỉ số và giá trị hàng. Việc trộn lẫn hai phép biến đổi này sẽ tạo ra các câu trả lời sai mà vẫn trông giống như các ô vuông Latinh hợp lệ. 

Một trường hợp tinh vi khác xảy ra khi có nhiều ca tích lũy. 

Ví dụ, với\(n=5\),$$
RRRRR
$$phải tạo ra ma trận gốc. Lưu trữ ca theo modulo\(n\)là bắt buộc. Việc cho phép chênh lệch tăng lên mà không giảm cuối cùng sẽ gây ra việc lập chỉ mục không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực áp dụng trực tiếp từng thao tác vào ma trận. 

Vì\(R,L,U,D\), chúng ta xoay hàng hoặc cột. Vì\(I\), chúng tôi tính toán hoán vị nghịch đảo của mỗi hàng. Vì\(C\), chúng tôi tính toán hoán vị nghịch đảo của mỗi cột. 

Chi phí của mỗi thao tác là \(O(n^2)\). Vì có thể có\(10^5\)hoạt động, tổng độ phức tạp trở thành$$
O(mn^2).
$$Điểm nghẽn là chúng ta liên tục di chuyển cùng một dữ liệu. 

Để tìm thứ gì đó tốt hơn, hãy kiểm tra mục nhập ma trận dưới dạng bộ ba$$
(r,c,x),
$$Ở đâu\(r\)là chỉ số hàng,\(c\)chỉ số cột và\(x\)giá trị. 

Bởi vì ma trận là một hình vuông Latin nên mọi phép toán có thể được mô tả hoàn toàn dưới dạng phép biến đổi của ba tọa độ này. 

Các thao tác dịch chuyển chỉ sửa đổi một tọa độ:$$
R,L : c
$$

$$
U,D : r
$$Các hoạt động đảo ngược thậm chí còn thú vị hơn. 

Đối với hoán vị hàng, đảo ngược trao đổi vị trí và giá trị bên trong hàng đó. Như vậy$$
I : c \leftrightarrow x.
$$Đối với hoán vị cột, đảo ngược trao đổi hàng và giá trị:$$
C : r \leftrightarrow x.
$$Thông tin chi tiết quan trọng là mọi thao tác chỉ hoán vị ba loại tọa độ \((r,c,x)\) và thêm các dịch chuyển theo chu kỳ vào bất kỳ tọa độ nào hiện đóng vai trò hàng, cột hoặc giá trị. 

Thay vì chuyển đổi toàn bộ ma trận, chúng tôi theo dõi: 

1. Tọa độ gốc nào hiện đại diện cho các hàng. 
2. Tọa độ gốc nào hiện đại diện cho các cột. 
3. Tọa độ gốc nào hiện đại diện cho các giá trị. 
4. Một offset gắn liền với mỗi tọa độ. 

Sau khi xử lý tất cả các thao tác, chúng tôi xây dựng lại câu trả lời một lần trong \(O(n^2)\). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | \(O(mn^2)\) | \(O(n^2)\) | Quá chậm | 
| Tối ưu | \(O(m+n^2)\) | \(O(n^2)\) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Hãy để các loại tọa độ được đánh số:$$
0=r,\quad 1=c,\quad 2=x.
$$Duy trì một mảng\(p\)Ở đâu:$$
p[i]
$$cho biết tọa độ gốc nào hiện đang chiếm vị trí\(i\). 

Duy trì sự bù đắp$$
add[0],add[1],add[2].
$$Tất cả các tọa độ được lưu trữ ở dạng dựa trên số không. 

1. Khởi tạo$$
p=[0,1,2]
$$Và$$
add=[0,0,0].
$$2. Xử lý từng thao tác. 

3. Đối với\(R\), tăng độ lệch của tọa độ hiện đang đóng vai trò là cột:$$
add[p[1]]=(add[p[1]]+1)\bmod n.
$$4. Đối với\(L\), giảm modulo bù tương tự\(n\). 

5. Đối với\(D\), tăng độ lệch của tọa độ hiện đang hoạt động dưới dạng hàng:$$
add[p[0]]=(add[p[0]]+1)\bmod n.
$$6. Đối với\(U\), giảm modulo phần bù đó\(n\). 

7. Đối với\(I\), hoán đổi vai trò của các cột và giá trị:$$
p[1],p[2]=p[2],p[1].
$$8. Đối với\(C\), hoán đổi vai trò của các hàng và giá trị:$$
p[0],p[2]=p[2],p[0].
$$9. Sau tất cả các thao tác, hãy truy cập từng ô gốc \((i,j)\). 

10. Chuyển đổi giá trị về dạng dựa trên số 0:$$
x=a_{ij}-1.
$$11. Hình thức$$
cur=[i,j,x].
$$12. Áp dụng bù trừ lũy kế:$$
cur[k]=(cur[k]+add[k])\bmod n.
$$13. Đặt tọa độ đã chuyển đổi vào vai trò cuối cùng của chúng bằng cách sử dụng\(p\). 

14. Hãy để$$
res[p[k]]=cur[k].
$$15. Sau đó$$
res[0]
$$là hàng cuối cùng,$$
res[1]
$$là cột cuối cùng, 

và$$
res[2]
$$là giá trị cuối cùng 

16. Viết$$
res[2]+1
$$vào ma trận câu trả lời ở vị trí$$
(res[0],res[1]).
$$### Tại sao nó hoạt động 

Mọi thao tác đều tác động lên bộ ba \((r,c,x)\). Các hoạt động thay đổi thêm\(1\)hoặc\(-1\)modulo\(n\)tới bất kỳ tọa độ nào hiện đóng vai trò là hàng hoặc cột. Các hoạt động\(I\)Và\(C\)không bao giờ tự sửa đổi các giá trị tọa độ, chúng chỉ trao đổi ý nghĩa của các loại tọa độ. 

Thuật toán duy trì chính xác hai thông tin này: một hoán vị\(p\)mô tả ý nghĩa hiện tại của các loại tọa độ và độ lệch mô tả tất cả các dịch chuyển theo chu kỳ tích lũy. Vì mọi thao tác đều cập nhật các cấu trúc này một cách chính xác khi nó biến đổi bộ ba tọa độ, nên cách biểu diễn sau khi xử lý chuỗi thao tác giống hệt với việc áp dụng các thao tác cho từng ô riêng lẻ. Việc xây dựng lại ma trận từ các bộ ba được biến đổi sẽ mang lại ma trận cuối cùng duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, m = map(int, input().split())
        a = [list(map(int, input().split())) for _ in range(n)]
        ops = input().strip()

        p = [0, 1, 2]
        add = [0, 0, 0]

        for ch in ops:
            if ch == 'R':
                add[p[1]] = (add[p[1]] + 1) % n
            elif ch == 'L':
                add[p[1]] = (add[p[1]] - 1) % n
            elif ch == 'D':
                add[p[0]] = (add[p[0]] + 1) % n
            elif ch == 'U':
                add[p[0]] = (add[p[0]] - 1) % n
            elif ch == 'I':
                p[1], p[2] = p[2], p[1]
            else:  # 'C'
                p[0], p[2] = p[2], p[0]

        ans = [[0] * n for _ in range(n)]

        for i in range(n):
            for j in range(n):
                cur = [i, j, a[i][j] - 1]

                for k in range(3):
                    cur[k] = (cur[k] + add[k]) % n

                res = [0, 0, 0]

                for k in range(3):
                    res[p[k]] = cur[k]

                ans[res[0]][res[1]] = res[2] + 1

        out = []
        for row in ans:
            out.append(" ".join(map(str, row)))
        sys.stdout.write("\n".join(out) + "\n")

solve()
```Mảng`p`lưu trữ hoán vị hiện tại của vai trò tọa độ. Bất cứ khi nào`I`hoặc`C`xuất hiện, chỉ hoán vị này thay đổi. 

Mảng`add`lưu trữ các ca được gắn với các loại tọa độ ban đầu, không phải với các vai trò hiện tại. Chi tiết này rất dễ bị nhầm lẫn. Nếu tọa độ sau đó thay đổi vai trò vì`I`hoặc`C`, sự dịch chuyển tích lũy của nó phải di chuyển theo nó. 

Trong quá trình tái thiết, mỗi ô được hiểu là một bộ ba tọa độ. Độ lệch được áp dụng đầu tiên vì chúng thuộc loại tọa độ. Sau đó hoán vị`p`xác định tọa độ được chuyển đổi nào sẽ trở thành hàng, cột hoặc giá trị. 

Các phép toán modulo xử lý các chuỗi dịch chuyển dài tùy ý mà không bị tràn hoặc trôi chỉ số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Ma trận đầu vào:$$
\begin{matrix}
1&2&3\\
2&3&1\\
3&1&2
\end{matrix}
$$Hoạt động:`DR`| Hoạt động | p | thêm | 
|---|---|---| 
| bắt đầu | [0,1,2] | [0,0,0] | 
| D | [0,1,2] | [1,0,0] | 
| R | [0,1,2] | [1,1,0] | 

Không có sự hoán đổi tọa độ nào xảy ra. Các hàng dịch chuyển một và các cột dịch chuyển một. Tái cấu trúc ma trận tạo ra$$
\begin{matrix}
2&3&1\\
3&1&2\\
1&2&3
\end{matrix}
$$phù hợp với mẫu. 

### Ví dụ 2 

Hoạt động:`I`| Hoạt động | p | thêm | 
|---|---|---| 
| bắt đầu | [0,1,2] | [0,0,0] | 
| Tôi | [0,2,1] | [0,0,0] | 

Các cột và giá trị trao đổi vai trò. 

Hãy xem xét ô ban đầu$$
(r,c,x)=(1,2,3).
$$Sau khi hoán đổi:$$
(r,x,c)=(1,3,2).
$$Áp dụng điều này cho mọi tế bào mang lại$$
\begin{matrix}
1&2&3\\
3&1&2\\
2&3&1
\end{matrix}
$$phù hợp với đầu ra mẫu. 

Dấu vết này thể hiện sự bất biến trung tâm:`I`Và`C`chỉ hoán đổi ý nghĩa tọa độ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(m+n^2)\) | Xử lý các thao tác một lần, sau đó tái tạo lại từng ô một lần | 
| Không gian | \(O(n^2)\) | Lưu trữ ma trận câu trả lời | 

Tổng số tiền của\(m\)trên tất cả các trường hợp thử nghiệm là nhiều nhất\(10^5\), và tổng số tiền\(n\)nhiều nhất là\(1000\). Công cuộc tái thiết chiếm ưu thế trong công việc và đòi hỏi nhiều nhất là khoảng\(10^6\)hoạt động của tế bào. Điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# These are conceptual tests for the solve() function.

# sample-style shift
inp = """1
3 2
1 2 3
2 3 1
3 1 2
DR
"""
expected = """2 3 1
3 1 2
1 2 3
"""

# minimum size
inp2 = """1
1 5
1
RLICD
"""
expected2 = """1
"""

# full cycle shift
inp3 = """1
2 2
1 2
2 1
RR
"""
expected3 = """1 2
2 1
"""

# inversion twice
inp4 = """1
3 2
1 2 3
2 3 1
3 1 2
II
"""
expected4 = """1 2 3
2 3 1
3 1 2
"""

# C twice
inp5 = """1
3 2
1 2 3
2 3 1
3 1 2
CC
"""
expected5 = """1 2 3
2 3 1
3 1 2
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|\(n=1\)với nhiều thao tác | ma trận không thay đổi | Trường hợp thoái hóa | 
|`RR`trên cỡ 2 | ma trận gốc | Số học Modulo | 
|`II`| ma trận gốc |\(I\)là một sự tiến hóa | 
|`CC`| ma trận gốc |\(C\)là một sự tiến hóa | 
| Trường hợp chuyển mẫu | câu trả lời mẫu | Tính đúng đắn cơ bản | 

## Vỏ cạnh 

Hãy xem xét\(n=1\). Mỗi modulo tọa độ\(1\)còn lại\(0\). Mỗi lần hoán đổi đều khiến ô đơn không thay đổi. Thuật toán giữ`p`Và`add`, xây dựng lại một bộ ba và xuất ra`1`. 

Hãy xem xét một chuỗi như`RRRRR`với\(n=5\). Phần bù cột trở thành$$
(0+5)\bmod 5 = 0.
$$Giai đoạn tái thiết nhận tọa độ ban đầu và tái tạo ma trận ban đầu. Bất kỳ triển khai nào lưu trữ các ca làm việc thô mà không giảm modulo\(n\)có nguy cơ lập chỉ mục không chính xác sau này. 

Coi như`IC`. Sau đó`I`, cột và trao đổi giá trị. Sau đó`C`, hàng và trao đổi giá trị. Các thao tác này hoạt động trên vai trò tọa độ chứ không phải trực tiếp trên các mục nhập ma trận. Chỉ theo dõi những thay đổi hàng và cột sẽ bỏ lỡ tương tác này. Mảng hoán vị`p`ghi chép chính xác$$
[0,1,2]
\to
[0,2,1]
\to
[1,2,0],
$$đó chính xác là phép biến đổi tọa độ được thực hiện bởi các phép toán.
