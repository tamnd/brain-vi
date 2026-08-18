---
title: "CF 102268D - Ngày tháng"
description: "Chúng ta có (t) ngày, trong đó ngày (d) có thể tổ chức nhiều nhất các ngày (quảng cáo). Mỗi cô gái được biểu thị bằng một khoảng ([li,ri]) và một giá trị khoái cảm (pi). Nếu chúng ta chọn cô ấy, chúng ta phải chỉ định cho cô ấy đúng một ngày trong khoảng thời gian đó và không ngày nào có thể nhận được nhiều hơn khả năng của nó."
date: "2026-08-17T18:40:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102268
codeforces_index: "D"
codeforces_contest_name: "300iq Contest 1"
rating: 0
weight: 102268
solve_time_s: 279
verified: false
draft: false
---

[CF 102268D - Ngày tháng](https://codeforces.com/problemset/problem/102268/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 39 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có (t) ngày, trong đó ngày (d) có thể lưu trữ nhiều nhất (a_d) ngày. Mỗi cô gái được biểu thị bằng một khoảng ([l_i,r_i]) và một giá trị khoái cảm (p_i). Nếu chúng ta chọn cô ấy, chúng ta phải chỉ định cho cô ấy đúng một ngày trong khoảng thời gian đó và không ngày nào có thể nhận được nhiều hơn khả năng của nó. 

Nhiệm vụ là chọn một tập hợp con các cô gái và ấn định mỗi cô gái được chọn vào một ngày hợp pháp sao cho tổng số niềm vui càng lớn càng tốt. Các khoảng đã được sắp xếp theo cả điểm cuối bên trái và bên phải của chúng, mặc dù giải pháp bên dưới thực sự không cần thuộc tính đặt hàng bổ sung đó. 

Giới hạn (n,t\le 300.000) ngay lập tức loại trừ bất cứ thứ gì được coi là tập hợp con của các bé gái, hoặc thậm chí bất cứ thứ gì bậc hai về số lượng bé gái. Có thể có (300.000) khoảng thời gian, do đó mục tiêu là khoảng (O((n+t)\log(n+t))). Giá trị khoái cảm có thể đạt tới (10^9) và có thể có (300.000) cô gái được chọn, vì vậy câu trả lời có thể nằm trong khoảng (3\cdot10^{14}). Số nguyên Python tự động xử lý việc này, trong khi việc triển khai C++ sẽ cần số nguyên 64 bit. 

Trường hợp cạnh đầu tiên có dung lượng bằng 0. Coi như```
1 2
0 0
1 2 10
```Câu trả lời là (0), vì cô gái có khoảng thời gian sẵn có nhưng không có ngày nào có thể sử dụng được. Việc triển khai chỉ kiểm tra xem mọi khoảng thời gian có trống hay không sẽ chọn sai cô ấy. 

Trường hợp thứ hai là một số cô gái cạnh tranh cho cùng một vị trí.```
2 1
1
1 1 10
1 1 9
```Chỉ có thể chọn một cô gái nên đáp án là (10). Một thuật toán bất cẩn xử lý từng cô gái một cách độc lập hoặc chỉ kiểm tra tổng công suất so với số lượng cô gái mà không tôn trọng khoảng cách của họ, có thể đếm sai cả hai cô gái. 

Trường hợp cạnh thứ ba liên quan đến các ranh giới bao hàm và các khoảng tiếp xúc với các điểm cuối của lịch trình.```
2 2
0 1
1 1 5
1 2 4
```Câu trả lời là (4). Cô gái đầu tiên chỉ có thể sử dụng ngày (1), sức chứa bằng 0. Cô gái thứ hai có thể sử dụng ngày (2), vì vậy cô ấy là cô gái duy nhất được chọn. Việc coi ([l,r]) là khoảng nửa mở hoặc vô tình dịch chuyển một điểm cuối trong quá trình kiểm tra tính khả thi có thể thay đổi câu trả lời này. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ liệt kê mọi tập hợp con của các cô gái, sau đó kiểm tra xem tập hợp con đó có thể được chỉ định theo ngày hay không. Bản thân việc kiểm tra có thể được thực hiện một cách tham lam bằng cách xử lý các ngày từ trái sang phải và chỉ định khoảng thời gian có sẵn với thời hạn sớm nhất. Điều đó mang lại một phương pháp vũ phu chính xác vì mọi lựa chọn có thể có của các cô gái đều được xem xét rõ ràng. 

Vấn đề là số lượng tập hợp con. Có (2^n) trong số chúng, do đó, ngay cả việc kiểm tra tính khả thi (O(n+t)) cũng sẽ dẫn đến (O(2^n(n+t))) hoạt động. Với (n=300.000), điều này khó có thể thực hiện được. 

Quan sát quan trọng là các tập hợp con khả thi có nhiều cấu trúc hơn một tập hợp các tập hợp con tùy ý. Hãy coi mỗi ngày là một số vị trí giống hệt nhau, với (a_d) bản sao của ngày (d). Một cô gái được kết nối với tất cả các ô thuộc ngày trong khoảng thời gian của cô ấy. Một tập hợp các cô gái sẽ khả thi khi những cô gái đó có thể được ghép vào các vị trí khác nhau. 

Họ các tập con có thể được so khớp trong đồ thị hai bên là một matroid ngang. Do đó, nếu chúng ta xử lý các cô gái theo thứ tự khoái cảm giảm dần và thêm một cô gái bất cứ khi nào tập kết quả vẫn khả thi thì tập được chấp nhận có tổng trọng số tối đa. Thuộc tính trao đổi đằng sau quy luật tham lam này là lý do chúng ta không cần lập trình động đối với các giá trị khoái cảm. Cách diễn giải matroid này và phép rút gọn điều kiện Hall tương ứng cũng là cốt lõi của lời giải đã biết cho bài toán này. 

Chúng ta còn lại vấn đề triển khai thực sự: sau khi chấp nhận một số cô gái có giá trị cao, làm thế nào chúng ta có thể kiểm tra xem liệu một khoảng khác có thể được chèn vào (O(\log t)) hay không? 

Định lý Hall đưa ra câu trả lời. Đối với bất kỳ khoảng thời gian ngày nào ([L,R]), tất cả các cô gái được chọn có toàn bộ khoảng thời gian khả dụng nằm trong ([L,R]) phải phù hợp với khả năng của những ngày đó. Do đó, một tập hợp được chọn (S) là khả thi chính xác khi 

[ 
#{[l_i,r_i]\in S\ge L,\ r_i\le R} 
\le 
\sum_{d=L}^{R}a_d 
] 

với mọi (1\le L\le R\le t). 

hãy để 

[ 
s_i=\sum_{d=1}^{i}a_d,\qquad s_0=0. 
] 

Thật thuận tiện khi dịch chuyển điểm cuối bên trái một. Xác định, cho (0\le i\le t), 

[ 
p_i=s_i-#{[l,r]\in S\le i} 
] 

và 

[ 
q_i=s_i-#{[l,r]\in S\le i}. 
] 

Điều kiện Hall trở thành 

[ 
p_i\le q_j 
] 

với mọi (0\le i<j\le t). Để thấy điều này, sự khác biệt (q_j-p_i) chính xác là công suất tính theo ngày (i+1,\ldots,j), trừ đi số khoảng được chọn hoàn toàn có trong đó. 

Bây giờ giả sử chúng ta muốn chèn ([l_0,r_0]). Việc thêm nó sẽ giảm mọi (p_i) với (i\ge l_0) đi một và mọi (q_i) với (i\ge r_0) đi một. Bất đẳng thức có khả năng bị vi phạm duy nhất có (i<l_0) và (j\ge r_0). Tất cả những bất đẳng thức khác hoặc không thay đổi hoặc trở nên dễ thỏa mãn hơn. 

Do đó việc chèn là khả thi chính xác khi 

[ 
\max_{0\le i<l_0}p_i 
< 
\min_{r_0\le j\le t}q_j. 
] 

Cây phân đoạn có thể duy trì mức tối đa (p), mức tối thiểu (q) và bổ sung hậu tố lười biếng. Mỗi khoảng được chấp nhận thực hiện hai lần giảm hậu tố, một trên (p) bắt đầu từ (l_0) và một trên (q) bắt đầu từ (r_0). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n(n+t))) | (O(n+t)) | Quá chậm | 
| Tham lam + Tình trạng hội trường + cây đoạn | (O(n\log n+n\log t+t)) | (O(n+t)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc công suất ngày và tính tổng tiền tố của chúng (s_0,s_1,\ldots,s_t). Tổng tiền tố cho phép chúng ta biểu thị dung lượng của bất kỳ khối liên tiếp nào dưới dạng một phép trừ. 
2. Sắp xếp tất cả các cô gái theo mức độ vui vẻ giảm dần. Đầu tiên chúng ta sẽ xem xét cô gái có giá trị nhất vì các tập hợp khả thi tạo thành một matroid. Định lý tham lam matroid có trọng số nói rằng việc chấp nhận phần tử khả thi có giá trị cao nhất ở mỗi bước sẽ tạo ra một tập hợp khả thi có trọng số tối đa. 
3. Ban đầu không có cô gái nào được chọn nên cả hai mảng đều thỏa mãn (p_i=q_i=s_i). Xây dựng cây phân đoạn có các lá biểu thị các chỉ số (0,\ldots,t). Mỗi nút lưu trữ giá trị tối đa (p_i) và tối thiểu (q_i) trong phạm vi của nó, cùng với các giá trị lười cho các phần bổ sung đang chờ xử lý cho hai mảng. 
4. Đối với một cô gái có khoảng ([l,r]), truy vấn giá trị lớn nhất (p_i) trên (0\le i<l) và giá trị nhỏ nhất (q_j) trên (r\le j\le t). Nếu giá trị đầu tiên nhỏ hơn giá trị thứ hai thì có thể thêm cô gái vào. Bất đẳng thức nghiêm ngặt là cần thiết vì việc thêm cô gái sẽ làm giảm các giá trị (q) liên quan đi đúng một. 
5. Nếu cô gái được chấp nhận, giảm (p_i) một cho mỗi (i\ge l), và giảm (q_i) một cho mỗi (i\ge r). Cả hai phép toán đều là phép cộng phạm vi hậu tố, do đó cây phân đoạn xử lý chúng theo thời gian logarit. 
6. Thêm niềm vui của cô gái vào câu trả lời bất cứ khi nào việc chèn thành công. Nếu kiểm tra tính khả thi không thành công, hãy giữ nguyên cây phân đoạn và tiếp tục với cô gái tiếp theo. 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của các cô gái được sắp xếp theo niềm vui, cây phân đoạn biểu thị chính xác các giá trị (p) và (q) cho các cô gái được chấp nhận và những cô gái được chấp nhận đó tạo thành một tập hợp khả thi. Đối với khoảng không được chọn ([l,r]), bất đẳng thức Hall duy nhất có thể trở thành sai sau khi chèn nó là các cặp có (i<l) và (j\ge r). Cây phân đoạn kiểm tra bất đẳng thức mạnh nhất bằng cách so sánh giá trị lớn nhất có thể (p_i) với giá trị nhỏ nhất có thể có (q_j). Do đó, mọi cô gái được chấp nhận đều duy trì tính khả thi, trong khi mọi cô gái bị từ chối không thể được thêm vào tập hiện tại. Vì các ứng cử viên được xử lý theo trọng lượng giảm dần và các tập khả thi tạo thành một matroid nên tập kết quả có tổng độ hài lòng tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from array import array

def solve():
    n, t = map(int, input().split())
    a = list(map(int, input().split()))

    girls = []
    for _ in range(n):
        l, r, p = map(int, input().split())
        girls.append((l, r, p))

    girls.sort(key=lambda x: x[2], reverse=True)

    # Prefix capacities s[0..t].
    pref = array('q', [0]) * (t + 1)
    cur = 0
    for i, x in enumerate(a, 1):
        cur += x
        pref[i] = cur

    # Segment tree over indices 0..t.
    size = 4 * (t + 1) + 5

    # Maximum p in a node.
    mxp = array('q', [0]) * size

    # Minimum q in a node.
    mnq = array('q', [0]) * size

    # Lazy suffix additions for p and q.
    lazy_p = array('i', [0]) * size
    lazy_q = array('i', [0]) * size

    def build(v, lo, hi):
        if lo == hi:
            x = pref[lo]
            mxp[v] = x
            mnq[v] = x
            return

        mid = (lo + hi) >> 1
        left = v << 1
        right = left | 1

        build(left, lo, mid)
        build(right, mid + 1, hi)

        mxp[v] = max(mxp[left], mxp[right])
        mnq[v] = min(mnq[left], mnq[right])

    def push(v):
        lp = lazy_p[v]
        lq = lazy_q[v]

        if lp:
            left = v << 1
            right = left | 1

            mxp[left] += lp
            mxp[right] += lp
            lazy_p[left] += lp
            lazy_p[right] += lp

            lazy_p[v] = 0

        if lq:
            left = v << 1
            right = left | 1

            mnq[left] += lq
            mnq[right] += lq
            lazy_q[left] += lq
            lazy_q[right] += lq

            lazy_q[v] = 0

    def max_prefix(v, lo, hi, qr):
        if hi <= qr:
            return mxp[v]

        push(v)
        mid = (lo + hi) >> 1
        left = v << 1
        right = left | 1

        if qr <= mid:
            return max_prefix(left, lo, mid, qr)

        x = maxp = mxp[left]
        y = max_prefix(right, mid + 1, hi, qr)
        return x if x > y else y

    def min_suffix(v, lo, hi, ql):
        if lo >= ql:
            return mnq[v]

        push(v)
        mid = (lo + hi) >> 1
        left = v << 1
        right = left | 1

        if ql > mid:
            return min_suffix(right, mid + 1, hi, ql)

        x = min_suffix(left, lo, mid, ql)
        y = mnq[right]
        return x if x < y else y

    def update(v, lo, hi, pl, ql):
        # No part of this segment belongs to either suffix.
        if hi < pl:
            return

        # Both suffixes fully cover this node.
        if lo >= ql:
            mxp[v] -= 1
            mnq[v] -= 1
            lazy_p[v] -= 1
            lazy_q[v] -= 1
            return

        # Only the p suffix fully covers this node.
        if lo >= pl and hi < ql:
            mxp[v] -= 1
            lazy_p[v] -= 1
            return

        if lo == hi:
            return

        # If p covers the whole node but q only covers part of it,
        # apply p here and descend for q.
        if lo >= pl:
            mxp[v] -= 1
            lazy_p[v] -= 1

        push(v)

        mid = (lo + hi) >> 1
        left = v << 1
        right = left | 1

        update(left, lo, mid, pl, ql)
        update(right, mid + 1, hi, pl, ql)

        mxp[v] = max(mxp[left], mxp[right])
        mnq[v] = min(mnq[left], mnq[right])

    build(1, 0, t)

    answer = 0

    for l, r, pleasure in girls:
        # The affected Hall inequalities have i < l and j >= r.
        left_max = max_prefix(1, 0, t, l - 1)
        right_min = min_suffix(1, 0, t, r)

        if left_max < right_min:
            answer += pleasure
            update(1, 0, t, l, r)

    return answer

if __name__ == "__main__":
    print(solve())
```các`pref`mảng lưu trữ (s_i), bao gồm (s_0=0). Việc sử dụng các chỉ số từ (0) đến (t) là điều làm cho điều kiện Hall trở thành (p_i\le q_j) cho (i<j). Do đó, truy vấn cho một cô gái bắt đầu tại (l) chính xác là tiền tố (0,\ldots,l-1), trong khi truy vấn cho cô ấy kết thúc tại (r) là hậu tố (r,\ldots,t). 

Cửa hàng cây phân đoạn`mxp`bởi vì thao tác chèn cần phía bên trái lớn nhất (p) và`mnq`vì thao tác chèn cần cạnh phải nhỏ nhất (q). Hai mảng lười này tách biệt nhau vì hai hậu tố thường bắt đầu ở các vị trí khác nhau. 

các`update`hàm xử lý cả hai phần giảm hậu tố trong một lần truyền tải. Vì (l\le r), hai ranh giới cập nhật xảy ra theo thứ tự được sắp xếp. Một nút có thể nằm hoàn toàn bên ngoài cả hai hậu tố, hoàn toàn bên trong cả hai, hoàn toàn bên trong chỉ hậu tố (p) hoặc nằm giữa một trong các ranh giới. Điều này giữ cho mỗi lần chèn logarit thay vì thực hiện (O(t)) mức giảm riêng lẻ. 

Việc so sánh sử dụng`<`, không`<=`. Giả sử trước khi chèn bất đẳng thức bị ảnh hưởng mạnh nhất là (p_i=q_j). Việc cộng khoảng sẽ giảm (q_j) đi một, do đó bất đẳng thức mới trở thành (p_i\le q_j-1), không thành công. Do đó, sự bình đẳng trước khi chèn có nghĩa là ứng viên phải bị loại. 

Số nguyên Python sẽ giữ mọi dung lượng tiền tố và câu trả lời cuối cùng một cách an toàn, nhưng cây phân đoạn sử dụng`array('q')`cho các trường số lớn của nó để tránh sử dụng quá nhiều bộ nhớ của hàng triệu đối tượng số nguyên Python. Các giá trị lười biếng chỉ nằm trong khoảng (-n) và (0), do đó, bộ nhớ 32 bit đã ký là đủ cho chúng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu chính thức là```
3 5
0 1 0 1 0
1 2 2
2 4 1
3 5 5
```Dung lượng tiền tố là 

[ 
s=[0,0,1,1,2,2]. 
] 

Các cô gái được xử lý theo thứ tự khoái cảm: ([3,5]) với khoái cảm (5), sau đó ([1,2]) với khoái cảm (2), sau đó ([2,4]) với khoái cảm (1). 

| Cô Gái | (l, r) | tối đa (p_i), (i<l) | phút (q_j), (j\ge r) | Quyết định | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| niềm vui 5 | 3, 5 | 1 | 2 | chấp nhận | 5 | 
| niềm vui 2 | 1, 2 | 0 | 1 | chấp nhận | 7 | 
| niềm vui 1 | 2, 4 | 0 | 0 | từ chối | 7 | 

Sau khi chấp nhận ([3,5]), giá trị (p) tại các chỉ số (3,4,5) giảm đi một, trong khi chỉ có (q_5) giảm. Chấp nhận ([1,2]) thì giảm (p_1,\ldots,p_5) và (q_2,\ldots,q_5). Đối với khoảng cuối cùng ([2,4]), vế trái tốt nhất (p) đã bằng vế phải nhỏ nhất (q), do đó việc chèn nó sẽ vi phạm điều kiện Hall. 

Các cô gái được chọn có thể được xếp lịch vào ngày (2) và ngày (4), mang lại niềm vui (2+5=7). Điều này cũng chứng tỏ tại sao ứng viên hài lòng (1) phải bị từ chối mặc dù vẫn còn một số năng lực ở nơi khác. 

### Ví dụ về tranh chấp tùy chỉnh 

Hãy xem xét```
2 1
1
1 1 10
1 1 9
```Chỉ có một khe ngày có sẵn. 

| Cô Gái | (l, r) | tối đa (p_i), (i<l) | phút (q_j), (j\ge r) | Quyết định | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| niềm vui 10 | 1, 1 | 0 | 1 | chấp nhận | 10 | 
| niềm vui 9 | 1, 1 | 0 | 0 | từ chối | 10 | 

Sau khi cô gái đầu tiên được chấp nhận, cả (p_1) và (q_1) đều trở thành 0. Lần chèn thứ hai sẽ yêu cầu (0<0), điều này là sai. Do đó, cấu trúc dữ liệu nắm bắt chính xác giới hạn dung lượng một khe mà không cần xây dựng một kết quả khớp một cách rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(t+n\log n+n\log t)) | Tổng tiền tố lấy (O(t)), sắp xếp mất (O(n\log n)) và mọi cô gái thực hiện công việc cây phân đoạn logarit | 
| Không gian | (O(n+t)) | Các cô gái, tổng tiền tố và mảng cây phân đoạn đều tuyến tính trong kích thước đầu vào | 

Với (n,t\le300.000), giải pháp chỉ thực hiện một lượng logarit công việc cấu trúc dữ liệu cho mỗi cô gái sau khi sắp xếp. Cây phân đoạn chứa các nút (O(t)) và các mảng số nhỏ gọn duy trì mức sử dụng bộ nhớ của nó trong giới hạn 256 MiB mà vấn đề chính thức nêu. 

## Trường hợp thử nghiệm 

Dây nịt sau đây giả định`solve()`hàm từ phần trước nằm trong cùng một tệp Python. Nó tạm thời thay thế`stdin`và toàn cầu`input`chức năng để mọi khẳng định đều thực hiện giải pháp thực tế.```python
# Place this harness in the same file as solve().

import sys
import io

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        return str(solve()).strip()
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided sample.
assert run(
    """3 5
0 1 0 1 0
1 2 2
2 4 1
3 5 5
"""
) == "7", "sample 1"

# Minimum-size case, with zero capacity.
assert run(
    """1 1
0
1 1 9
"""
) == "0", "minimum size and zero capacity"

# Two girls need the same single day.
assert run(
    """2 1
1
1 1 10
1 1 9
"""
) == "10", "single-slot contention"

# All capacities, intervals, and pleasures are equal.
assert run(
    """3 3
1 1 1
1 3 7
1 3 7
1 3 7
"""
) == "21", "equal values and tie handling"

# Boundary-heavy case.
assert run(
    """3 3
0 1 0
1 1 5
2 2 7
1 3 6
"""
) == "7", "inclusive interval boundaries"

# Maximum-size structural test.
N = 300_000
lines = [
    f"{N} {N}",
    " ".join(["1"] * N),
]
lines.extend(["1 300000 1"] * N)

assert run("\n".join(lines) + "\n") == "300000", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0 / 1 1 9`| 0 | Đầu vào tối thiểu và công suất bằng 0 | 
|`2 1 / 1 / 1 1 10 / 1 1 9`| 10 | Hai quãng cạnh tranh một vị trí | 
|`3 3 / 1 1 1 / 1 3 7`lặp đi lặp lại | 21 | Năng lực, khoảng thời gian, niềm vui và mối quan hệ phân loại ngang nhau | 
|`3 3 / 0 1 0 / 1 1 5 / 2 2 7 / 1 3 6`| 7 | Ranh giới bao gồm và ngày điểm cuối không có sẵn | 
| Ví dụ đã tạo (300000)-girl | 300000 | Tối đa (n,t), hiệu suất cây phân đoạn và câu trả lời lớn | 

## Vỏ cạnh 

### Không có dung lượng 

cho```
1 2
0 0
1 2 10
```dung lượng tiền tố ban đầu là (s=[0,0,0]), vì vậy (p=q=[0,0,0]). Đối với cô gái duy nhất, truy vấn bên trái cho kết quả (p_0=0), trong khi truy vấn bên phải cho kết quả (\min(q_2)=0). Bất đẳng thức nghiêm ngặt bắt buộc (0<0) không thành công, vì vậy cô gái bị từ chối và câu trả lời vẫn là (0). 

Thuật toán không cần trường hợp đặc biệt cho những ngày không có công suất. Chúng chỉ đơn giản xuất hiện dưới dạng các giá trị tiền tố bằng nhau và bất đẳng thức Hall phát hiện ra rằng không thể chỉ định ngày nào. 

### Nhiều cô gái tranh tài trong một ngày 

cho```
2 1
1
1 1 10
1 1 9
```mảng ban đầu là (p=q=[0,1]). Niềm vui (10) cô gái được chấp nhận vì (p_0=0<q_1=1). Cập nhật hậu tố làm cho cả hai giá trị tại chỉ mục (1) bằng 0. 

Khi xem xét mức độ hài lòng của cô gái (9), bài kiểm tra sẽ trở thành (p_0=0<q_1=0), thất bại. Thuật toán trả về (10), khớp chính xác với thực tế là một ngày chỉ có một vị trí. 

### Điểm cuối toàn diện và dung lượng bằng 0 ở ranh giới 

cho```
3 3
0 1 0
1 1 5
2 2 7
1 3 6
```cô gái có giá trị cao nhất là ([2,2]), vì vậy cô ấy được chấp nhận sử dụng ngày duy nhất có thể sử dụng được. Cô gái ([1,3]) được xem xét tiếp theo, nhưng cô ấy cạnh tranh để giành lấy năng lực còn lại đó và không thể được thêm vào. Cô gái ([1,1]) chỉ được sử dụng ngày (1), năng lực của cô ấy bằng 0 nên cũng bị từ chối. 

Câu trả lời là (7). Các chỉ số cây phân đoạn (0,\ldots,t) làm cho ranh giới bên trái (l=1) tương ứng với tiền tố chỉ chứa chỉ mục (0), trong khi ranh giới bên phải (r=3) bắt đầu hậu tố tại chỉ mục (3). Đây chính xác là sự thay đổi cần thiết trong công thức của Hall và tránh việc diễn giải sai lệch từng khoảng thời gian trong ngày ban đầu. 

### Một ứng cử viên có thể khả thi trên toàn cầu nhưng không thể ở địa phương 

Hãy xem xét```
3 3
1 0 1
1 1 100
2 3 60
1 3 70
```Cô gái đầu tiên có giá trị (100) và chỉ được sử dụng ngày (1) nên được chấp nhận. Cô gái thứ hai có giá trị (70) và có thể sử dụng ngày (1) đến (3), nhưng sau lần chấp nhận đầu tiên cô ấy vẫn có thể sử dụng ngày (3) nên cô ấy cũng được chấp nhận. Cô gái thứ ba có giá trị (60) và có thể sử dụng ngày (2) hoặc (3), nhưng ngày (2) không có năng lực và ngày (3) đã được cô gái có giá trị (70) linh hoạt cần đến. Bài kiểm tra Hall từ chối cô ấy. 

Điều này minh họa tại sao chỉ kiểm tra tổng dung lượng còn lại là không đủ. Tính khả thi phụ thuộc vào nơi khả năng đó xảy ra và phép biến đổi (p,q) nén tất cả các ràng buộc Hall khoảng vào hai cực trị được cây phân đoạn truy vấn.
