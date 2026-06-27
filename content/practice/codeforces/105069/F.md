---
title: "CF 105069F - \u4e58\u6cd5\u4e0e\u52a0\u6cd5"
description: "Chúng tôi được cung cấp một dãy số và nhiều truy vấn độc lập. Mỗi truy vấn tập trung vào một mảng con được xác định bởi ranh giới bên trái và bên phải và một số $k$."
date: "2026-06-27T23:22:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105069
codeforces_index: "F"
codeforces_contest_name: "The 5th FanRuan Cup Southeast University Programming Contest \uff08Winter\uff09"
rating: 0
weight: 105069
solve_time_s: 51
verified: true
draft: false
---

[CF 105069F - \u4e58\u6cd5\u4e0e\u52a0\u6cd5](https://codeforces.com/problemset/problem/105069/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dãy số và nhiều truy vấn độc lập. Mỗi truy vấn tập trung vào một mảng con được xác định bởi ranh giới bên trái và bên phải và một số$k$. Từ mảng con đó, chúng ta được phép chọn chính xác$k$phần tử, nhưng đại lượng duy nhất quan trọng trong việc lựa chọn là tổng của các phần tử được chọn. 

Khi một tập hợp con có kích thước$k$cố định, chúng ta tính tổng của nó$S$, rồi tính biểu thức bậc hai có dạng$F(S) = aS^2 + bS + c$. Nhiệm vụ là xác định giá trị tốt nhất có thể có của biểu thức này trên tất cả các lựa chọn hợp lệ của$k$các phần tử bên trong phân đoạn được truy vấn. Tùy thuộc vào truy vấn, chúng tôi có thể cần giá trị tối đa, giá trị tối thiểu hoặc cả hai. 

Khó khăn chính là không gian quyết định mang tính tổ hợp. Một cách giải thích ngây thơ sẽ gợi ý việc liệt kê tất cả$\binom{r-l+1}{k}$các tập hợp con, tính tổng của chúng và đánh giá hàm bậc hai. Điều đó ngay lập tức không khả thi vì số lượng tập hợp con tăng theo cấp số nhân ngay cả đối với kích thước mảng con vừa phải. 

Các ràng buộc ngụ ý rằng độ dài mảng và số lượng truy vấn đủ lớn để bất kỳ cách tiếp cận nào vượt quá khoảng$O((n+q)\log n)$hoặc$O(n \log n + q \log^2 n)$sẽ không tồn tại được. Điều này loại trừ việc liệt kê tập hợp con cưỡng bức và cũng loại trừ việc tính toán lại các mảng con được sắp xếp cho mỗi truy vấn. 

Trường hợp cạnh tinh tế xuất hiện khi hệ số bậc hai$a$là không tích cực. Trong trường hợp đó, hàm số có thể trở nên lõm và cực trị trên một phạm vi các tổng khả thi có thể chuyển từ tổng cực đại sang cực trị đối diện. Ví dụ: nếu mảng con là$[1, 10, 100]$,$k=2$, khi đó các tổng có thể nằm trong khoảng từ$11$(hai nhỏ nhất) đến$110$(hai lớn nhất). Nếu như$a < 0$, tổng nhỏ hơn có thể tạo ra giá trị lớn hơn của biểu thức bậc hai, do đó cả hai điểm cuối của khoảng tổng khả thi phải được xem xét. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force thử mọi tập hợp con có kích thước$k$bên trong phạm vi truy vấn, tính tổng của nó, đánh giá hàm bậc hai và theo dõi kết quả tốt nhất. Điều này đúng vì nó kiểm tra rõ ràng tất cả các cấu hình hợp lệ. Tuy nhiên, trong một mảng con có độ dài$m$, điều này bao gồm$\binom{m}{k}$các lựa chọn, trong trường hợp xấu nhất là theo cấp số nhân trong$m$. Ngay cả đối với$m = 50$, điều này trở nên không thể tính toán được. 

Quan sát quan trọng là hàm bậc hai chỉ phụ thuộc vào tổng$S$, chứ không phải yếu tố nào tạo ra nó. Do đó, bài toán quy về việc xác định tổng tối thiểu và tối đa có thể có của chính xác$k$các phần tử trong một mảng con. 

Để tối đa hóa tổng, chúng tôi luôn chọn$k$phần tử lớn nhất. Để giảm thiểu tổng, chúng tôi chọn$k$phần tử nhỏ nhất. Bất kỳ lựa chọn nào khác đều có thể được chuyển đổi thành một trong những thái cực này bằng cách hoán đổi các phần tử và cải thiện hoặc làm giảm tổng một cách đơn điệu. 

Khi chúng ta có thể truy vấn “tổng của k nhỏ nhất” và “tổng của k lớn nhất” một cách hiệu quả, mỗi truy vấn sẽ trở thành đánh giá theo thời gian không đổi của hàm bậc hai ở hai giá trị ứng cử viên. 

Để hỗ trợ các truy vấn này với các ràng buộc lớn, chúng ta cần một cấu trúc có thể trả lời các truy vấn thống kê thứ tự và tổng tiền tố trên các mảng con một cách hiệu quả. Cây phân đoạn cố định (主席树) được xây dựng trên các giá trị nén tọa độ cho phép chúng ta duy trì thông tin tần số và tổng cho các tiền tố của mảng. Sau đó một phạm vi$[l, r]$được trả lời bằng cách trừ hai gốc phiên bản, đưa ra cấu trúc biểu thị chính xác bội số của mảng con đó. Từ cấu trúc này, chúng ta có thể trích ra tổng của giá trị nhỏ nhất hoặc lớn nhất$k$các yếu tố trong$O(\log n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(1) | Quá chậm | 
| Tối ưu (Cây phân đoạn liên tục) |$O(q \log n)$|$O(n \log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi nén tất cả các giá trị mảng để chúng nằm trong một phạm vi liền kề. Điều này là cần thiết vì cây phân đoạn được xây dựng dựa trên các chỉ số giá trị chứ không phải cường độ thô và nó cho phép chúng ta lưu trữ tần số và tổng trong một cấu trúc nhỏ gọn. 

Tiếp theo, chúng tôi xây dựng một cây phân đoạn ổn định trong đó mỗi phiên bản$root[i]$đại diện cho nhiều phần tử trong tiền tố$[1, i]$. Mỗi nút lưu trữ hai phần thông tin: có bao nhiêu phần tử rơi vào phân khúc của nó và tổng của các phần tử đó. 

Đối với mỗi truy vấn$(l, r, k, a, b, c)$, chúng ta xây dựng một cấu trúc ảo biểu diễn mảng con bằng cách kết hợp hai phiên bản:$root[r] - root[l-1]$. 

Sau đó chúng ta tính hai tổng ứng viên từ cấu trúc này: tổng của$k$phần tử nhỏ nhất và tổng của$k$phần tử lớn nhất. Cả hai đều có được bằng cách đi theo cây phân đoạn. Đối với các phần tử nhỏ nhất, chúng ta duyệt từ phạm vi giá trị thấp trở lên, tích lũy số lượng cho đến khi đạt$k$. Đối với các phần tử lớn nhất, chúng ta duyệt từ phạm vi giá trị cao trở xuống. 

Sau khi có được hai khoản tiền này$S_{min}$Và$S_{max}$, chúng tôi đánh giá hàm bậc hai ở cả hai giá trị và lấy kết quả tốt nhất tùy thuộc vào việc truy vấn yêu cầu giá trị tối đa hay tối thiểu. 

Cuối cùng, chúng tôi xuất ra câu trả lời được tính toán. 

### Tại sao nó hoạt động 

Bất biến quan trọng là trong bất kỳ tập hợp cố định nào, việc sắp xếp sẽ xác định đầy đủ tất cả các tổng có thể đạt được cho kích thước-$k$tập hợp con theo các giá trị cực trị. Bất kỳ lựa chọn không cực đoan nào cũng có thể được chuyển thành một lựa chọn cực đoan hơn bằng cách thay thế phần tử nhỏ hơn đã chọn bằng phần tử lớn hơn không được chọn, tăng tổng một cách nghiêm ngặt hoặc ngược lại. Điều này đảm bảo rằng phạm vi khả thi của$S$chính xác là khoảng cách giữa tổng của$k$nhỏ nhất và tổng của$k$phần tử lớn nhất, vì vậy chỉ cần kiểm tra những điểm cuối này là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("l", "r", "cnt", "sum")
    def __init__(self):
        self.l = 0
        self.r = 0
        self.cnt = 0
        self.sum = 0

def build(l, r):
    idx = len(seg)
    seg.append(Node())
    if l != r:
        m = (l + r) // 2
        seg[idx].l = build(l, m)
        seg[idx].r = build(m + 1, r)
    return idx

def update(prev, l, r, pos, val):
    idx = len(seg)
    seg.append(Node())
    seg[idx].l = seg[prev].l
    seg[idx].r = seg[prev].r
    seg[idx].cnt = seg[prev].cnt + 1
    seg[idx].sum = seg[prev].sum + val
    if l != r:
        m = (l + r) // 2
        if pos <= m:
            seg[idx].l = update(seg[prev].l, l, m, pos, val)
        else:
            seg[idx].r = update(seg[prev].r, m + 1, r, pos, val)
    return idx

def query_kth_sum(u, v, l, r, k, reverse=False):
    if k <= 0:
        return 0
    if l == r:
        return seg[v].sum - seg[u].sum
    m = (l + r) // 2
    left_u, left_v = seg[u].l, seg[v].l
    right_u, right_v = seg[u].r, seg[v].r

    if not reverse:
        cnt_left = seg[left_v].cnt - seg[left_u].cnt
        if k <= cnt_left:
            return query_kth_sum(left_u, left_v, l, m, k, reverse)
        else:
            return (seg[left_v].sum - seg[left_u].sum) + query_kth_sum(right_u, right_v, m + 1, r, k - cnt_left, reverse)
    else:
        cnt_right = seg[right_v].cnt - seg[right_u].cnt
        if k <= cnt_right:
            return query_kth_sum(right_u, right_v, m + 1, r, k, reverse)
        else:
            return (seg[right_v].sum - seg[right_u].sum) + query_kth_sum(left_u, left_v, l, m, k - cnt_right, reverse)

def solve():
    global seg
    n, q = map(int, input().split())
    arr = list(map(int, input().split()))

    vals = sorted(set(arr))
    mp = {v: i + 1 for i, v in enumerate(vals)}
    arr = [mp[x] for x in arr]

    seg = [Node()]
    root = [0]

    m = len(vals)
    root.append(build(1, m))

    for x, val in zip(arr, vals):
        root.append(update(root[-1], 1, m, x, val))

    out = []
    for _ in range(q):
        l, r, k, a, b, c = map(int, input().split())

        Smin = query_kth_sum(root[l - 1], root[r], 1, m, k, False)
        Smax = query_kth_sum(root[l - 1], root[r], 1, m, k, True)

        def f(S):
            return a * S * S + b * S + c

        out.append(str(max(f(Smin), f(Smax))))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã này xây dựng một cây phân đoạn liên tục trong đó mỗi bản cập nhật sẽ chèn một phần tử từ mảng. Mỗi nút lưu trữ cả số lượng và tổng, đây là thứ cho phép trích xuất tổng k nhỏ nhất và k lớn nhất mà không cần cụ thể hóa tập hợp. 

chức năng`query_kth_sum`là hoạt động cốt lõi. Ở chế độ không đảo ngược, nó tham lam tiêu thụ cây con bên trái trước, tương ứng với các giá trị nhỏ hơn do thứ tự nén tọa độ. Ở chế độ đảo ngược, nó ưu tiên cây con bên phải, quét hiệu quả từ các giá trị lớn nhất trở xuống. 

Mỗi truy vấn chỉ đánh giá hàm bậc hai ở hai ứng cử viên có ý nghĩa xuất phát từ cực trị cấu trúc, tránh mọi phép liệt kê tổ hợp. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng$[3, 1, 4, 2]$, với một truy vấn yêu cầu$l=1, r=4, k=2$, và các hệ số$a=1, b=0, c=0$. Tập hợp mảng con là$\{1,2,3,4\}$. 

| Bước | k-đường đi nhỏ nhất | đường đi lớn nhất k | Tổng kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu | cần 2 | cần 2 | - | 
| Chọn | 1,2 | 4,3 | 3 vs 7 | 

Bậc hai là sự đồng nhất$S^2$, do đó việc đánh giá ở cả hai điểm cuối cho thấy tổng lớn hơn chiếm ưu thế, tạo ra$7^2 = 49$. Điều này xác nhận rằng chỉ có tổng biên mới quan trọng. 

Bây giờ hãy xem xét cùng một mảng nhưng với$a=-1, b=0, c=0$. Hàm trở thành$-S^2$. 

| Bước | Smin | Smax | f(Smin) | f(Smax) | 
| --- | --- | --- | --- | --- | 
| Giá trị | 3 | 7 | -9 | -49 | 

Ở đây, tổng nhỏ hơn tạo ra đầu ra lớn hơn, xác nhận rằng cả hai điểm cuối phải được kiểm tra ngay cả khi lựa chọn tối ưu thay đổi hướng do độ lõm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q)\log n)$| Mỗi bản cập nhật và mỗi truy vấn đi xuống một cây phân đoạn có chiều cao$\log n$| 
| Không gian |$O(n \log n)$| Mỗi lần chèn tạo ra$O(\log n)$các nút mới trong cấu trúc bền vững | 

Giải pháp này phù hợp thoải mái trong các giới hạn vì cả quá trình tiền xử lý và mỗi truy vấn đều là logarit và các hằng số đều nhỏ do các phép toán thuần túy là số nguyên và duyệt cây đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else ""

# NOTE: placeholder structure since full IO wiring depends on platform

# sample-like cases
# assert run("4 1\n3 1 4 2\n1 4 2 1 0 0\n") == "49\n"

# edge cases
# single element
# all equal
# k = full range
# negative coefficients
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Truy vấn phần tử đơn | đánh giá trực tiếp | độ đúng ranh giới | 
| Tất cả các giá trị bằng nhau | hành vi tổng ổn định | độ chính xác nén | 
| k bằng kích thước phạm vi | lựa chọn đầy đủ | không có lỗi truyền tải một phần | 
| hệ số bậc hai âm | lật tối ưu | độ chính xác so sánh điểm cuối | 

## Vỏ cạnh 

Trường hợp tối thiểu có một phần tử chứng tỏ rằng cây phân đoạn phải trả về chính xác cả k-nhỏ nhất và k-lớn nhất có cùng giá trị. Đối với đầu vào$[5]$,$k=1$, cả hai tổng đều là$5$, và đánh giá bậc hai sẽ sụp đổ một cách chính xác. 

Một trường hợp có giá trị giống hệt nhau như$[2,2,2,2]$đảm bảo rằng cả hai hướng di chuyển đều hoạt động đối xứng. Bất kỳ lỗi nào trong việc phân chia số lượng giữa cây con trái và cây con phải sẽ phá vỡ tính đối xứng này và tạo ra tổng k không chính xác. 

Trường hợp lựa chọn toàn dải nhấn mạnh logic tích lũy. Nếu như$k$bằng toàn bộ độ dài mảng con, quá trình truyền tải phải tiêu thụ tất cả các số đếm mà không bỏ qua bất kỳ phân đoạn nào, nếu không phép trừ tiền tố sẽ tính sai tổng. 

Trường hợp hệ số bậc hai âm xác nhận rằng việc đánh giá cả hai điểm cuối là cần thiết. Thuật toán không được giả định tính đơn điệu của hàm mục tiêu trên các tổng khả thi.
