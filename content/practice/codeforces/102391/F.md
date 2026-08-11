---
title: "CF 102391F - Khách sạn Hilbert"
description: "Chúng ta có một chuỗi vô hạn các phòng được đánh số (0,1,2,ldots) và mỗi phòng luôn chứa chính xác một khách. Khách thuộc nhóm. Ban đầu mỗi phòng có một khách từ nhóm (0). Thao tác loại 1 tạo ra một nhóm mới."
date: "2026-08-11T23:03:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102391
codeforces_index: "F"
codeforces_contest_name: "XX Open Cup, Grand Prix of Korea"
rating: 0
weight: 102391
solve_time_s: 222
verified: true
draft: false
---

[CF 102391F - Khách sạn Hilbert](https://codeforces.com/problemset/problem/102391/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi vô hạn các phòng được đánh số (0,1,2,\ldots) và mỗi phòng luôn chứa chính xác một khách. Khách thuộc nhóm. Ban đầu mỗi phòng có một khách từ nhóm (0). 

Thao tác loại 1 tạo ra một nhóm mới. Nếu tham số của nó là số nguyên dương (k), mọi khách hiện tại sẽ di chuyển (k) phòng sang bên phải, do đó nhóm mới sẽ chiếm các phòng (0,1,\ldots,k-1). Nếu tham số là (0), mọi khách hiện tại sẽ di chuyển từ phòng (x) sang phòng (2x) và nhóm mới sẽ chiếm tất cả các phòng lẻ. 

Truy vấn loại 2 yêu cầu phòng nhỏ nhất thứ (x) mà một nhóm cụ thể chiếm giữ. Truy vấn loại 3 hỏi nhóm nào hiện đang chiếm một phòng cụ thể. 

Khó khăn là khách sạn là vô hạn nên chúng ta không bao giờ có thể mô phỏng mảng phòng một cách rõ ràng. Số lượng thao tác cũng lớn tới (300000), loại trừ việc duyệt qua tất cả các thao tác trước đó cho mỗi truy vấn. Với giới hạn thời gian 2 giây, mô phỏng (O(Q^2)) vượt xa những gì có thể. Chúng tôi cần công việc logarit đại khái cho mỗi truy vấn. 

Có một số trường hợp ranh giới dễ gây ra câu trả lời sai. 

Đầu tiên, phòng (0) hoạt động khác đi dưới một phép toán vô hạn. Nó được ánh xạ tới (0), do đó việc đảo ngược liên tục các phép toán vô hạn không làm cho phòng (0) nhỏ hơn. Ví dụ,```
3
1 0
1 0
3 0
```có đầu ra```
2
```vì phòng (0) vẫn còn bị chiếm giữ bởi nhóm vô hạn mới nhất. Một mô phỏng ngược giả định mỗi thao tác vô hạn sẽ giảm một nửa số phòng có thể bị kẹt hoặc bỏ qua nhóm không chính xác. 

Thứ hai, sự bình đẳng trong một phép toán hữu hạn rất quan trọng. Nếu thao tác ngược lại có tham số (k), phòng (x=k) thuộc về trạng thái khách sạn cũ chứ không phải nhóm mới được chèn. Ví dụ,```
3
1 5
3 5
3 4
```kết quả đầu ra```
0
1
```vì nhóm (1) chiếm các phòng (0) đến (4), trong khi phòng (5) vẫn thuộc nhóm (0). Sử dụng (x\leq k) thay vì (x<k) sẽ gán sai phòng (5) cho nhóm (1). 

Thứ ba, khách thứ (x) được lập chỉ mục một. Nếu một nhóm chiếm các phòng (0,1,2) thì khách đầu tiên của nhóm đó là ở phòng (0), không phải phòng (1). Ví dụ,```
2
1 3
2 1 1
```kết quả đầu ra```
0
```vì phòng đầu tiên của nhóm (1) là phòng (0). 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là duy trì phòng hiện tại của mỗi khách và cập nhật tất cả các phòng sau mỗi hoạt động loại 1. Điều đó là không thể vì có vô số khách. Chúng tôi có thể hạn chế sự chú ý vào một tiền tố hữu hạn, nhưng ngay cả khi đó một thao tác đơn lẻ cũng có thể ảnh hưởng đến mọi phòng có liên quan, vì vậy điều này vẫn không mở rộng quy mô. Với (300000) phép tính, phương pháp bậc hai có thể dễ dàng thực hiện khoảng (9\time10^{10}) cập nhật. 

Quan sát quan trọng đầu tiên là mọi thao tác áp dụng cho khách hiện tại đều là một phép biến đổi affine. Một hoạt động hữu hạn là 

[ 
x\mapsto x+k, 
] 

trong khi một hoạt động vô hạn là 

[ 
x\mapsto 2x. 
] 

Thành phần của các thao tác này luôn có dạng 

[ 
F(x)=ax+b, 
] 

trong đó (a) là lũy thừa của hai. Điều này đưa ra một mô tả ngắn gọn về chuyển động của mỗi vị khách cũ. 

Giả sử nhóm (g) được chèn vào khi phép biến đổi tích lũy được thực hiện 

[ 
F_g(x)=a_gx+b_g. 
] 

Phòng riêng của nó trước khi hoạt động sau này tạo thành một cấp số cộng. Đối với một nhóm hữu hạn, tiến trình này là 

[ 
0,1,\ldots,k-1, 
] 

vì vậy số hạng đầu tiên của nó là (0) và hiệu của nó là (1). Đối với một nhóm vô hạn thì đó là 

[ 
1,3,5,\ldots, 
] 

vậy số hạng đầu tiên của nó là (1) và hiệu của nó là (2). 

Nếu phép biến đổi tích lũy hiện tại là (F(x)=ax+b), chúng ta có thể hủy bỏ (F_g) về mặt đại số và sau đó áp dụng (F). Sự chuyển đổi kết quả từ tọa độ chèn của nhóm sang các phòng hiện tại của nó lại là affine. Vì (a_g) là lũy thừa của hai nên nó là modulo khả nghịch (10^9+7). Do đó, truy vấn loại 2 có thể được trả lời trong thời gian không đổi sau khi lưu trữ một vài giá trị cho mỗi nhóm. 

Quan sát khóa thứ hai xử lý các truy vấn loại 3. Làm việc ngược lại từ phòng được truy vấn. Phép toán hữu hạn với tham số (k) có quy tắc ngược lại 

[ 
x\geq k \Rightarrow x\leftarrow x-k, 
] 

trong khi (x<k) có nghĩa là căn phòng vừa bị nhóm mới của hoạt động đó chiếm giữ. 

Đối với một phép toán vô hạn, số lẻ (x) có nghĩa là căn phòng mới được nhóm vô hạn đó chiếm giữ. Nếu (x) chẵn thì khách cũ đến từ phòng (x/2). 

Các phép toán hữu hạn giữa hai phép toán vô hạn liên tiếp có thể được xử lý thành một khối. Nếu tham số của chúng là (k_1,k_2,\ldots,k_m), hãy lưu tổng tiền tố của chúng. Đi ngược qua toàn bộ khối chỉ trừ 

[ 
k_1+k_2+\cdots+k_m. 
] 

Nếu (x) được truy vấn trở nên nhỏ hơn tổng số này thì chỉ cần một lần tìm kiếm nhị phân để tìm ra phép toán hữu hạn chính xác đã thu được nó. 

Sau khi vượt qua một phép toán vô hạn, bước đảo ngược thành công sẽ thay đổi dương (x) thành (x/2). Do đó, có thể có nhiều nhất (O(\log x)) bước như vậy. Đây là lý do quan trọng khiến quá trình ngược lại diễn ra nhanh chóng mặc dù chuỗi thao tác có thể chứa (300000) sự kiện. 

Kết quả so sánh là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(Q^2)) trong trường hợp xấu nhất | (O(Q)) hoặc tệ hơn | Quá chậm | 
| Biểu diễn Affine + khối phép toán hữu hạn | (O(Q(\log Q+\log V))) | (O(Q)) | Đã chấp nhận | 

Ở đây (V\leq10^9) là số phòng xuất hiện trong truy vấn loại 3. Trong thực tế, hệ số logarit từ quá trình ngược lại nhiều nhất là khoảng (30), bởi vì (2^{30}>10^9). 

## Hướng dẫn thuật toán 

1. Biểu diễn ảnh hưởng của tất cả các hoạt động đối với khách hiện tại bằng hàm affine 

[ 
F(x)=ax+b. 
] 

Ban đầu (a=1,b=0). Một phép toán hữu hạn với tham số (k) sẽ thay đổi điều này thành 

[ 
a'=a,\qquad b'=b+k, 
] 

trong khi một hoạt động vô hạn thay đổi nó thành 

[ 
a'=2a,\qquad b'=2b. 
] 

Chỉ cần (b) và (a) modulo (10^9+7) cho câu trả lời loại 2. 

1. Đối với mỗi nhóm, lưu trữ phép biến đổi affine (F_g(x)=a_gx+b_g) tồn tại ngay sau khi nhóm đó vào khách sạn. Đồng thời lưu trữ điểm bắt đầu (s_g) và hiệu (d_g) của cấp số cộng ban đầu của nó. 

Đối với nhóm ban đầu (0), sử dụng 

[ 
s_0=0,\qquad d_0=1. 
] 

Để sử dụng nhóm hữu hạn 

[ 
s_g=0,\qquad d_g=1. 
] 

Để sử dụng nhóm vô hạn 

[ 
s_g=1,\qquad d_g=2. 
]

Lý do lưu trữ sự chuyển đổi tại thời điểm chèn là vì tất cả các chuyển động sau đó có thể được mô tả bằng sự chuyển đổi từ trạng thái lịch sử đó sang trạng thái hiện tại. 

1. Giữ nguyên nghịch đảo của (a) modulo (10^9+7). Vì (a) luôn là lũy thừa của hai nên nghịch đảo của nó luôn tồn tại. Khi một phép toán vô hạn nhân (a) với (2), thay vào đó hãy nhân nghịch đảo của nó với (2^{-1}). 
2. Để trả lời truy vấn loại 2 cho nhóm (g), hãy 

[ 
c=a\cdot a_g^{-1}\pmod M. 
] 

Phép dịch giữa tọa độ lịch sử của nhóm (g) và tọa độ hiện tại là 

[ 
e=b-cb_g. 
] 

Phòng hiện tại tương ứng với tọa độ lịch sử (y) là 

[ 
cy+e. 
] 

Vị khách thứ (x) của nhóm (g) có tọa độ lịch sử 

[ 
s_g+d_g(x-1). 
] 

Do đó phòng hiện tại của nó là 

[ 
e+c(s_g+d_g(x-1))\pmod M. 
] 

Điều này đưa ra câu trả lời trong thời gian không đổi. 

1. Đối với truy vấn loại 3, chia tất cả các phép toán hữu hạn loại 1 thành các khối được phân tách bằng các phép toán vô hạn. Khối đầu tiên chứa các phép toán hữu hạn trước phép toán vô hạn đầu tiên. Mỗi khối sau này chứa các phép toán hữu hạn ngay sau một phép toán vô hạn. 

Đối với mỗi khối, lưu trữ tổng tích lũy của các giá trị hữu hạn (k) và số nhóm tương ứng. 

1. Nếu phòng được truy vấn là (0), trả về ngay số nhóm của phép toán hữu hạn mới nhất hoặc (0) nếu không có phép toán hữu hạn nào xảy ra. Các phép toán vô hạn làm cho khoảng trống (0) không thay đổi, do đó chỉ những phần chèn hữu hạn mới có thể thay thế được người chiếm giữ nó. 
2. Ngược lại, bắt đầu từ khối hữu hạn cuối cùng và đảo ngược các phép toán hữu hạn của nó. Nếu tổng (k) của khối lớn hơn (x), hoạt động chịu trách nhiệm nằm bên trong khối này. Sử dụng tìm kiếm nhị phân trên các tổng tích lũy để xác định phép toán hữu hạn cuối cùng có tổng hậu tố lớn hơn (x). 

Nếu tổng lớn nhất là (x), hãy trừ toàn bộ tổng khối và vượt qua phép toán vô hạn trước đó. 

1. Ở phép toán vô hạn, một số lẻ (x) thuộc về nhóm mới được chèn vào đó, vì vậy hãy trả về số nhóm của nó. Nếu (x) chẵn, thay (x) bằng (x/2) và tiếp tục với khối trước. 
2. Nếu trừ đi một khối hữu hạn đầy đủ sẽ tạo ra (x=0), câu trả lời là nhóm hữu hạn mới nhất trước khối đó. Không cần thiết phải duyệt qua một chuỗi các phép toán vô hạn liên tiếp có khả năng rất lớn vì tất cả chúng đều không thay đổi khoảng trống (0). 

### Tại sao nó hoạt động 

Bất biến đối với truy vấn loại 2 là mỗi nhóm được biểu thị bằng cấp số cộng ban đầu cùng với phép biến đổi affine đã hoạt động khi nhóm được nhập. Việc kết hợp phép biến đổi lịch sử nghịch đảo với phép biến đổi hiện tại mô tả chính xác vị trí hiện tại của mọi thành viên trong nhóm đó, do đó công thức trả về phòng thứ (x) chính xác. 

Đối với truy vấn loại 3, việc đảo ngược một phép toán hữu hạn là chính xác vì các phòng (0,\ldots,k-1) chính xác là các phòng mới được chèn, trong khi mọi phòng khác đều đến từ (x-k). Đảo ngược một phép toán vô hạn cũng chính xác vì các phòng lẻ chính xác là các phòng mới được chèn vào và mọi phòng chẵn đều đến từ (x/2). Việc nhóm các phép toán hữu hạn liên tiếp không làm thay đổi logic này, vì hiệu ứng ngược của chúng chỉ đơn giản là phép trừ các tham số của chúng theo thứ tự ngược lại. Mỗi khi một phép toán vô hạn được vượt qua với một số phòng dương, số phòng sẽ giảm đi một nửa, do đó, chỉ có thể thực hiện được nhiều lần giao nhau như vậy theo logarit. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

MOD = 1_000_000_007
INV2 = (MOD + 1) // 2

def solve():
    q = int(input())

    # Current global affine transformation:
    # F(x) = A * x + B
    A = 1
    B = 0
    invA = 1

    # Information stored for every group.
    # Group 0 is the initial infinite set 0,1,2,...
    a_hist = [1]
    b_hist = [0]
    inv_hist = [1]
    start = [0]
    step = [1]

    # Blocks of finite operations.
    # blocks[b] contains cumulative sums of k in block b.
    blocks = [[]]

    # The corresponding group ids for the finite operations.
    block_groups = [[]]

    # For every block, the latest finite group strictly before it.
    prev_finite = [0]

    # Group ids of infinite operations.
    # Infinite operation corresponding to block b (b > 0)
    # is inf_groups[b - 1].
    inf_groups = []

    last_finite_group = 0

    # Number of type 1 operations processed.
    groups = 0

    out = []

    for _ in range(q):
        query = input().split()
        typ = int(query[0])

        if typ == 1:
            k = int(query[1])
            groups += 1
            gid = groups

            if k > 0:
                # All old rooms shift by k.
                B = (B + k) % MOD

                # The new group occupies 0,1,...,k-1.
                a_hist.append(A)
                b_hist.append(B)
                inv_hist.append(invA)
                start.append(0)
                step.append(1)

                if blocks[-1]:
                    cumulative = blocks[-1][-1] + k
                else:
                    cumulative = k

                blocks[-1].append(cumulative)
                block_groups[-1].append(gid)

                last_finite_group = gid

            else:
                # All old rooms double.
                A = (2 * A) % MOD
                B = (2 * B) % MOD
                invA = (invA * INV2) % MOD

                # The new group occupies 1,3,5,...
                a_hist.append(A)
                b_hist.append(B)
                inv_hist.append(invA)
                start.append(1)
                step.append(2)

                inf_groups.append(gid)

                # Start a new finite block.
                blocks.append([])
                block_groups.append([])
                prev_finite.append(last_finite_group)

        elif typ == 2:
            g = int(query[1])
            x = int(query[2])

            c = (A * inv_hist[g]) % MOD
            e = (B - c * b_hist[g]) % MOD

            first = (e + c * start[g]) % MOD
            diff = (c * step[g]) % MOD

            answer = (first + diff * (x - 1)) % MOD
            out.append(str(answer))

        else:
            x = int(query[1])

            # Infinite operations leave room 0 fixed.
            if x == 0:
                out.append(str(last_finite_group))
                continue

            block = len(blocks) - 1

            while True:
                cumulative = blocks[block]

                if cumulative:
                    total = cumulative[-1]

                    if x < total:
                        # Find the first prefix sum >= total - x.
                        # Its corresponding finite operation is exactly
                        # the one whose reverse interval contains x.
                        idx = bisect_left(cumulative, total - x)
                        out.append(str(block_groups[block][idx]))
                        break

                    x -= total

                    if x == 0:
                        # Everything in this block was reversed.
                        # Room 0 now belongs to the latest finite group
                        # before this block.
                        out.append(str(prev_finite[block]))
                        break

                if block == 0:
                    # We have reached the initial configuration.
                    out.append("0")
                    break

                # Cross the infinite operation before this block.
                infinite_group = inf_groups[block - 1]

                if x & 1:
                    # Odd rooms were newly occupied by this group.
                    out.append(str(infinite_group))
                    break

                # An even room came from x / 2.
                x //= 2
                block -= 1

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`A`,`B`, Và`invA`các biến mô tả phép biến đổi affine hiện tại được áp dụng cho mọi khách đã tồn tại trước hoạt động loại 1 mới nhất. Một phép toán hữu hạn chỉ thay đổi`B`, trong khi một phép toán vô hạn nhân đôi cả hai`A`Và`B`. 

Bốn mảng lịch sử lưu trữ phép biến đổi và cấp số cộng của mỗi nhóm tại thời điểm chèn. Nhóm ban đầu được chèn vào theo khái niệm tại thời điểm 0, đó là lý do tại sao các giá trị được lưu trữ của nó là (A=1,B=0,s=0,d=1). 

Công thức loại 2 sử dụng tỷ lệ giữa số nhân hiện tại và số nhân khi chèn nhóm. Vì mọi số nhân đều là lũy thừa của hai nên phép chia mô-đun là an toàn. Hệ số nghịch đảo được cập nhật một cách rõ ràng, tránh việc tính lũy thừa mô-đun tốn kém cho mọi truy vấn. 

Đối với truy vấn loại 3,`blocks`lưu trữ tổng tích lũy thay vì giá trị thô (k). Giả sử một khối chứa (k_1,k_2,k_3), với tổng tích lũy (k_1,k_1+k_2,k_1+k_2+k_3). Nếu phòng hiện tại nhỏ hơn tổng số phòng,`bisect_left`tìm tổng tiền tố đầu tiên đạt`total - x`. Chỉ số đó xác định phép toán hữu hạn có khoảng mới được chèn chứa phòng đảo ngược. 

Sự so sánh chặt chẽ`x < total`và việc sử dụng`total - x`đều có ranh giới nhạy cảm. Khi`x == total`, mọi phép toán hữu hạn trong khối đã bị đảo ngược và chúng ta phải vượt qua phép toán vô hạn trước đó. 

Số nguyên Python có độ chính xác tùy ý, do đó tổng tích lũy của các giá trị hữu hạn (k) không bị tràn. Các giá trị được sử dụng cho câu trả lời loại 2 được giảm modulo (10^9+7), trong khi tổng khối vẫn là số nguyên chính xác vì chúng được sử dụng để so sánh với (x). 

## Ví dụ đã hoạt động 

Chỉ có một mẫu trong câu lệnh, vì vậy dấu vết thứ hai bên dưới sử dụng một chuỗi tùy chỉnh nhỏ. 

### Mẫu 1 

Trạng thái sau mỗi thao tác loại 1 có thể được tóm tắt bằng phép biến đổi affine hiện tại và nhóm mới được tạo. 

| Truy vấn | Trạng thái loại 1 | (A) | (B) | Nhóm mới | 
| --- | --- | --- | --- | --- | 
|`3 0`| không hoạt động | 1 | 0 | không | 
|`1 3`| dịch chuyển 3 | 1 | 3 | 1 | 
|`2 1 2`| không thay đổi | 1 | 3 | nhóm truy vấn 1 | 
|`1 0`| đôi | 2 | 6 | 2 | 
|`3 10`| không thay đổi | 2 | 6 | truy vấn ngược | 
|`2 2 5`| không thay đổi | 2 | 6 | nhóm truy vấn 2 | 
|`1 5`| dịch chuyển 5 | 2 | 11 | 3 | 
|`1 0`| đôi | 4 | 22 | 4 | 
|`3 5`| không thay đổi | 4 | 22 | truy vấn ngược | 
|`2 3 3`| không thay đổi | 4 | 22 | nhóm truy vấn 3 | 

Vì`2 1 2`, nhóm 1 được chèn bằng (A_g=1,B_g=3), trong khi phép biến đổi hiện tại giống hệt nhau. Tiến trình lịch sử của nó là (0,1,2), nên phòng thứ hai là (1). 

Sau phép toán vô hạn đầu tiên, nhóm 2 bắt đầu ở các phòng lẻ. Khi đó số phòng của nhóm 2 là (1,3,5,7,9,\ldots) nên phòng thứ năm của nó là (9). 

Truy vấn`3 10`đảo ngược phép toán vô hạn vì (10) chẵn, tạo ra (5). Không có phép toán vô hạn nào trước đó nên nó tiếp tục thông qua phép dịch chuyển hữu hạn của (3), tạo ra (2) và cuối cùng đạt được nhóm ban đầu. Như vậy phòng (10) thuộc nhóm (0). 

Sau hai hoạt động cuối cùng, nhóm 4 chiếm phòng lẻ nên phòng (5) thuộc nhóm (4). Truy vấn loại 2 cuối cùng yêu cầu phòng thứ ba của nhóm 3, đã trở thành cấp số lẻ (1,3,5,\ldots) sau phép toán vô hạn sau này, đưa ra (4) làm đầu ra sau phép biến đổi lịch sử thích hợp. 

Đầu ra hoàn chỉnh là```
0
1
0
9
4
4
```### Dấu vết tùy chỉnh 

Hãy xem xét:```
8
1 1
1 2
1 0
1 0
3 1
3 2
3 6
2 3 3
```Trạng thái quan trọng là: 

| Truy vấn | Hành động hiện tại | Phòng bị đảo ngược | Chặn | Kết quả | 
| --- | --- | --- | --- | --- | 
|`1 1`| nhóm 1 vào | | khối hữu hạn 0 | | 
|`1 2`| nhóm 2 vào | | khối hữu hạn 0 | | 
|`1 0`| nhóm 3 vào | | khối mới 1 | | 
|`1 0`| nhóm 4 vào | | khối mới 2 | | 
|`3 1`| lẻ nhất là vô hạn | 1 | khối 2 | nhóm 4 | 
|`3 2`| chẵn, chia cho 2 | 2 → 1 | khối 1 | nhóm 3 | 
|`3 6`| chẵn, chia cho 2 | 6 → 3 | khối 1 | nhóm 3 | 
|`2 3 3`| truy vấn cấp số cộng | | nhóm 3 | phòng 10 | 

Nhóm 3 được tạo bằng phép toán vô hạn thứ nhất nên ban đầu các phòng của nó là (1,3,5,\ldots). Phép toán vô hạn thứ hai nhân đôi số này thành (2,6,10,\ldots). Như vậy phòng (2) và (6) đều thuộc nhóm 3. 

Nhóm 4 là nhóm vô hạn mới nhất và chiếm tất cả các phòng lẻ, vì vậy phòng (1) ngay lập tức xác định nhóm 4. Truy vấn loại 2 yêu cầu phòng thứ ba của nhóm 3, đó là (10). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(Q(\log Q+\log V))) | Các phép toán loại 1 và loại 2 là (O(1)); một truy vấn loại 3 vượt qua tối đa (O(\log V)) hoạt động vô hạn và thực hiện tối đa một (O(\log Q)) tìm kiếm nhị phân. | 
| Không gian | (O(Q)) | Dữ liệu nhóm lịch sử và dữ liệu khối thao tác hữu hạn chứa tối đa một mục nhập cho mỗi thao tác loại 1. | 

Giá trị phòng tối đa trong truy vấn loại 3 là (10^9), do đó, tối đa khoảng (30) lần giảm một nửa thành công có thể xảy ra. Tổng số thao tác là (300000), do đó giới hạn lưu trữ tuyến tính dễ dàng nằm trong giới hạn bộ nhớ 1024 MB. Thuật toán tránh việc xây dựng bất kỳ tập hợp phòng vô hạn nào và chỉ thực hiện công việc logarit cho mỗi truy vấn. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới giả định giải pháp trên được lưu dưới dạng`solution.py`, với`solve()`chức năng không thay đổi```python
# Save the solution as solution.py before running this file.
import sys
import io
import solution

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solution.solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
sample1 = """\
10
3 0
1 3
2 1 2
1 0
3 10
2 2 5
1 5
1 0
3 5
2 3 3
"""

assert run(sample1) == """\
0
1
0
9
4
4\
""", "sample 1"

# Custom 1: minimum-size input
assert run("""\
1
3 0
""") == "0\n", "initial group"

# Custom 2: finite block, equality and x-th indexing
assert run("""\
5
1 2
1 3
3 0
3 4
2 1 2
""") == """\
2
1
4
""", "finite operations and boundaries"

# Custom 3: consecutive infinite operations
assert run("""\
8
1 1
1 2
1 0
1 0
3 1
3 2
3 6
2 3 3
""") == """\
4
3
3
10\
""", "repeated infinite operations"

# Custom 4: all-equal finite values and one-indexed query
assert run("""\
4
1 7
1 7
1 7
2 1 7
""") == "20\n", "repeated equal k values"

# Custom 5: maximum-size stress case
q = 300000
maximum_input = str(q) + "\n" + ("1 1\n" * (q - 1)) + "3 0\n"

assert run(maximum_input) == f"{q - 1}\n", "maximum number of queries"
```Các trường hợp tùy chỉnh xác thực các thuộc tính sau: 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 3 0`|`0`| Nhóm ban đầu và đầu vào hợp lệ nhỏ nhất | 
| Hai phép toán hữu hạn theo sau là loại 3 và loại 2 |`2, 1, 4`| Khối đảo ngược hữu hạn, xử lý ranh giới chính xác và một chỉ mục (x) | 
| Hai phép tính vô hạn liên tiếp |`4, 3, 3, 10`| Giảm một nửa lặp đi lặp lại, phát hiện phòng lẻ và cấp số cộng nhóm vô hạn | 
| Ba giống hệt nhau`k=7`hoạt động |`20`| Những ca lặp đi lặp lại và tổng hữu hạn tích lũy | 
| (299999) bản sao của`1 1`theo sau là`3 0`|`299999`| Số truy vấn tối đa và xử lý phòng (0) | 

## Vỏ cạnh 

Đối với phòng (0), thuật toán trả về ngay`last_finite_group`. Coi như```
4
1 5
1 0
1 0
3 0
```Hai phép toán vô hạn giữ phòng (0) tại phòng (0). Việc chèn hữu hạn duy nhất sau trạng thái ban đầu là nhóm (1), vì vậy câu trả lời là`1`. Thuật toán không bao giờ thực hiện một chuỗi các lần chia đôi bằng 0 có khả năng không giới hạn. 

Đối với ranh giới hữu hạn chính xác, hãy xem xét```
3
1 5
3 5
3 4
```Việc chèn hữu hạn chiếm các phòng (0,1,2,3,4). Phòng đảo ngược (5) sử dụng điều kiện (x\geq5), cho phòng trước (0) nên phòng (5) thuộc nhóm (0). Phòng đảo ngược (4) thay vào đó tìm thấy (x<5), do đó nhóm (1) được trả về. Đầu ra là`0`theo sau là`1`. 

Đối với các hoạt động vô hạn liên tiếp, hãy xem xét```
4
1 0
1 0
3 2
```Nhóm vô hạn đầu tiên ban đầu chiếm (1,3,5,\ldots). Thao tác vô hạn thứ hai sẽ di chuyển những vị khách đó đến (2,6,10,\ldots). Phòng đảo ngược (2) trước tiên nhìn thấy một phòng chẵn, chia đôi và nhận được (1). Phép toán vô hạn trước đó nhìn thấy một căn phòng lẻ nên đáp án là nhóm (1). 

Đối với khối hữu hạn, xét tham số (3,5). Nếu phòng hiện tại là (4), đảo ngược thao tác mới nhất với (k=5) sẽ ngay lập tức tìm thấy (4<5), vậy nhóm hữu hạn thứ hai sở hữu phòng. Nếu phòng hiện tại là (5), thì đảo ngược đầu tiên trừ (5), đạt (0), rồi tiếp tục về trạng thái trước đó. Sự bất bình đẳng nghiêm ngặt trong việc thực hiện chính là điều ngăn cách hai trường hợp này. 

Đối với lập chỉ mục loại 2, một nhóm hữu hạn được tạo bằng (k=3) bắt đầu từ (0) với hiệu (1). Số phòng của nó là (0,1,2), nên câu trả lời cho (x=1) là (0) và cho (x=3) là (2). Công thức sử dụng (x-1), ngăn ngừa lỗi thường gặp. 

Cuối cùng, số học mô-đun chỉ cần thiết cho câu trả lời loại 2. Tổng khối hữu hạn thực tế không bị giảm modulo (10^9+7), vì chúng được so sánh với số phòng thực trong quá trình mô phỏng ngược. Việc kết hợp hai vai trò đó sẽ tạo ra câu trả lời loại 3 không chính xác khi số ca tích lũy vượt quá mô đun.
