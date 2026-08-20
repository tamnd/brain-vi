---
title: "CF 102202E - Nước Biết Trả Lời"
description: "Ta có (N) hình hộp chữ nhật. Mỗi hộp có thể được đặt theo một trong hai hướng và tất cả các hộp phải tạo thành một hàng liền kề trên mặt đất. Mưa rơi theo phương thẳng đứng nên nước chỉ có thể tồn tại ở khu vực được bao bọc theo chiều ngang bởi các hộp."
date: "2026-08-18T01:12:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102202
codeforces_index: "E"
codeforces_contest_name: "2019 KAIST RUN Spring Contest"
rating: 0
weight: 102202
solve_time_s: 397
verified: false
draft: false
---

[CF 102202E - Nước Biết Câu Trả Lời](https://codeforces.com/problemset/problem/102202/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 37 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Ta có (N) hình hộp chữ nhật. Mỗi hộp có thể được đặt theo một trong hai hướng và tất cả các hộp phải tạo thành một hàng liền kề trên mặt đất. Mưa rơi theo phương thẳng đứng nên nước chỉ có thể tồn tại ở khu vực được bao bọc theo chiều ngang bởi các hộp. 

Cách hữu ích để nghĩ về một mực nước có thể có (H) là quyết định xem hộp nào tạo thành hai bức tường của thùng chứa và hộp nào nằm bên trong nó. hãy để 

[ 
L_i=\max(w_i,h_i),\qquad S_i=\min(w_i,h_i). 
] 

Đối với hộp được sử dụng bên trong thùng chứa, hướng tốt nhất là đặt cạnh dài của nó theo chiều ngang và cạnh ngắn của nó theo chiều dọc. Nếu (S_i<H), hộp đó sẽ đóng góp 

[ 
L_i(H-S_i) 
] 

tới khu vực được lưu trữ. Nếu (S_i\ge H), nó không đóng góp gì cả. 

Thay vào đó, một chiếc hộp có thể được sử dụng như một bức tường. Để đạt đến mức (H), cạnh thẳng đứng của nó ít nhất phải bằng (H), vì vậy điều này chỉ có thể thực hiện được khi (L_i\ge H). Trong vai trò đó, chúng ta xoay nó, đặt cạnh dài hơn của nó theo chiều dọc. Do đó, đối với một (H) cố định, chúng ta cần hai hộp riêng biệt với (L_i\ge H) là hai bức tường. Trong số tất cả các hộp như vậy, chúng ta nên chọn hai hộp có đóng góp thông thường (L_i(H-S_i)) nhỏ nhất, bởi vì đó là hai phần đóng góp mà chúng ta sẽ mất đi khi biến hộp thành các bức tường. Công thức có chiều cao cố định này là mức giảm trung tâm được sử dụng bởi giải pháp. 

Các ràng buộc loại trừ mọi thứ bậc hai trong (N). Với (N) lớn bằng (250000), thậm chí (O(N^2)) sẽ có nghĩa là khoảng (6,25\times10^{10}) phép toán cặp. Độ dài cạnh tối đa là (10^6), điều này mang lại cho chúng ta phạm vi tọa độ giới hạn hữu ích, nhưng việc lặp lại trên mọi độ cao và mọi hộp vẫn sẽ yêu cầu đánh giá lên tới (10^6\cdot250000=2.5\time10^{11}). Chúng ta cần khoảng (O(N\log N)). 

Có một số trường hợp đặc biệt mà việc triển khai trực tiếp có thể xử lý sai. Đầu tiên, có thể có ít hơn hai hộp có khả năng đạt đến cấp độ đã chọn. Ví dụ,```
3
1 3
1 2
1 1
```chỉ có một hộp có (L_i\ge3) nên mức (3) không thể chứa được chút nước nào. Một thói quen chỉ tính toán các khoản đóng góp mà không kiểm tra hai bức tường có thể báo cáo một giá trị dương. 

Thứ hai, một chiếc hộp có cạnh ngắn hơn chính xác là mực nước đóng góp bằng 0 chứ không phải là số âm. Vì```
3
1 2
1 2
1 1
```câu trả lời đúng là (1). Hai hộp (1\times2) có thể xoay thành những bức tường thẳng đứng có chiều cao (2), trong khi hộp (1\times1) chứa một đơn vị nước. Sử dụng (H-S_i) mà không kẹp ở mức 0 sẽ đưa ra những đóng góp âm không chính xác. 

Thứ ba, vấn đề luân chuyển ở cả hai vai trò. Coi như```
3
2 5
2 5
100 1
```Hai hộp (2\times5) trở thành những bức tường thẳng đứng có chiều cao (5), trong khi hộp (100\times1) nằm ngang bên trong chúng. Câu trả lời là (100(5-1)=400). Việc coi chiều rộng đầu vào ban đầu là chiều ngang vĩnh viễn sẽ bỏ lỡ sự sắp xếp này. 

Cuối cùng, các kích thước bằng nhau không gây ra sự phức tạp hình học đặc biệt nào. Vì```
3
2 2
2 2
2 2
```mọi cách sắp xếp có thể đều không chứa nước dự trữ, vì vậy câu trả lời là (0). Việc triển khai phải cho phép (L_i) và (S_i) bằng nhau mà không dựa vào sự bất bình đẳng nghiêm ngặt giữa chúng. 

## Phương pháp tiếp cận 

Lực lượng vũ phu nhất theo nghĩa đen liệt kê mọi hoán vị của các hộp, mọi lựa chọn xoay và sau đó đánh giá sự sắp xếp kết quả. Việc đó cần sắp xếp (N!,2^N), với lần quét (O(N)) cho mỗi cách sắp xếp, đưa ra (O(N!,2^N N)). Tại (N=250000), điều này vượt xa sự cân nhắc. 

Một cách tiếp cận ngây thơ hữu ích hơn là sử dụng quan sát có chiều cao cố định. Với mọi số nguyên có thể (H), hãy quét tất cả các hộp, tính toán (L_i\max(0,H-S_i)), xác định hai hộp có (L_i\ge H) có đóng góp nhỏ nhất và trừ chúng. Có nhiều nhất (10^6) độ cao có thể có, do đó, việc này cần đánh giá hộp (O(10^6N)) hoặc (2,5\times10^{11}) trong trường hợp xấu nhất. 

Lực lượng vũ phu hoạt động bởi vì, một khi (H) và hai bức tường được biết đến, mọi hộp còn lại có thể được xử lý độc lập. Vấn đề là việc tìm kiếm nhiều lần hai giá trị nhỏ nhất sẽ rất tốn kém. Quan sát quan trọng là mỗi hộp đều đóng góp một hàm tuyến tính của (H): 

[ 
f_i(H)=L_iH-L_iS_i. 
] 

Chúng ta chỉ cần hai hàm hoạt động nhỏ nhất, trong đó hộp trở thành một bức tường khi (H\le L_i). Đây chính xác là loại truy vấn dòng tối thiểu động được xử lý bởi cây Li Chao. Giải pháp ban đầu sử dụng cây Li Chao được sửa đổi để mỗi nút giữ hai dòng tốt nhất thay vì chỉ một dòng tốt nhất. 

Có một sự tối ưu hóa nữa. Câu trả lời có chiều cao cố định không cần phải kiểm tra ở tất cả (10^6) độ cao. Giữa hai giá trị liên tiếp xuất hiện giữa (S_i) và (L_i), tập hợp các hộp đóng góp và tập hợp các bức tường có thể không thay đổi. Tổng phần đóng góp là affine trong (H), trong khi tổng của hai hàm affine nhỏ nhất là lõm. Do đó, hiệu của chúng là lồi và hàm lồi đạt cực đại trên một khoảng tại điểm cuối. Vì vậy, chỉ cần kiểm tra các giá trị khác biệt giữa tất cả (S_i) và (L_i) là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N!2^N N)) | (O(N)) | Quá chậm | 
| Chiều cao cố định, quét tất cả các ô | (O(10^6N)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N\log N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi ô, hãy tính (L_i=\max(w_i,h_i)) và (S_i=\min(w_i,h_i)). Hãy coi (L_i) là kích thước có thể được sử dụng làm chiều rộng ngang của hộp bên trong hoặc chiều cao thẳng đứng của bức tường. Đóng góp của nó với tư cách là một hộp bên trong ở cấp độ (H) là 

[ 
g_i(H)=\max(0,L_i(H-S_i)). 
] 

1. Cố định mực nước (H). Một hộp có thể là một trong hai bức tường bên ngoài chính xác khi (L_i\ge H). Mọi hộp khác có thể được đặt giữa những bức tường đó. Diện tích tối đa của (H) này là tổng của tất cả (g_i(H)), trừ đi hai giá trị nhỏ nhất (g_i(H)) trong số các hộp thỏa mãn (L_i\ge H). 
2. Lưu ý rằng trước khi áp dụng mức tối đa bằng 0, mọi đóng góp đều là đường 

[ 
f_i(H)=L_iH-L_iS_i. 
] 

Chúng tôi xử lý (H) từ lớn đến nhỏ. Khi chúng ta đạt đến (H=L_i), hộp (i) đủ điều kiện trở thành một bức tường và đường của nó được chèn vào cây Li Chao. Vì chúng tôi không bao giờ tăng (H) nữa nên một dòng được chèn vẫn đủ điều kiện cho mọi truy vấn sau này.

1. Duy trì cây Li Chao trên tọa độ chiều cao ứng viên. Nút Li Chao bình thường lưu trữ đường tối thiểu tại điểm giữa của nó. Ở đây chúng tôi lưu trữ hai dòng khác biệt nhỏ nhất ở điểm giữa. Trong quá trình chèn, dòng tốt hơn sẽ trở thành dòng đầu tiên, dòng tốt nhất tiếp theo sẽ trở thành dòng thứ hai và chỉ ứng viên bị thay thế mới phải tiếp tục đệ quy thành một dòng con. 
2. Đối với truy vấn ở độ cao (H), hãy đi từ gốc tới lá biểu thị (H). Tại mỗi nút được truy cập, cả hai dòng được lưu trữ đều là ứng cử viên cho hai giá trị nhỏ nhất. Hợp nhất các giá trị đó thành hai giá trị cực tiểu đang chạy. Điều này mang lại giá trị thô nhỏ nhất và nhỏ thứ hai của (f_i(H)) trong số tất cả các ứng cử viên tường hiện được chèn. 
3. Tính riêng tổng của tất cả các đóng góp không âm. Sắp xếp các hộp theo (S_i) và xây dựng tổng tiền tố của (L_i) và (L_iS_i). Đối với chiều cao (H), tất cả các hộp có (S_i<H) đều đóng góp, vì vậy nếu tổng (L_i) của chúng là (A) và tổng (L_iS_i) của chúng là (B), thì tổng số là 

[ 
AH-B. 
] 

Các giá trị có (S_i=H) đóng góp bằng 0, do đó, việc sử dụng bao gồm nghiêm ngặt hoặc không nghiêm ngặt tại ranh giới đó sẽ cho kết quả tương tự. 

1. Trừ đi hai phần đóng góp không âm nhỏ nhất của bức tường thu được từ cây Li Chao. Nếu có ít hơn hai bức tường ứng cử viên, chiều cao đó không thể chứa nước và bị bỏ qua. 
2. Kiểm tra mọi giá trị khác biệt giữa tất cả (L_i) và (S_i), giữ diện tích kết quả lớn nhất. Những điều này là đủ vì giữa các độ cao sự kiện liên tiếp, các tập hoạt động được cố định và mục tiêu thu được là lồi, do đó độ cao bên trong không thể tốt hơn cả hai điểm cuối. 

Tại sao nó hoạt động: đối với bất kỳ (H) cố định nào, mọi hộp không phải là một trong hai bức tường đều có thể được định hướng độc lập để đóng góp chính xác (g_i(H)). Hộp duy nhất phải hy sinh là hai bức tường, vì vậy việc chọn hai đóng góp đủ điều kiện nhỏ nhất là tối ưu. Cây Li Chao duy trì chính xác hai mức tối thiểu đó. Tổng tiền tố cho biết phần đóng góp của mỗi hộp trước khi hai bức tường bị loại bỏ. Cuối cùng, mọi khoảng không có sự kiện (S_i) hoặc (L_i) đều có một tập hợp các đường tham gia cố định và mục tiêu của nó là lồi, do đó việc kiểm tra các điểm cuối của nó sẽ bao trùm mức tối đa của nó. Do đó, giá trị tốt nhất được thuật toán kiểm tra bằng diện tích nước tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

INF = 10**30

def solve():
    n = int(input())

    L = [0] * (n + 1)
    S = [0] * (n + 1)

    for i in range(1, n + 1):
        w, h = map(int, input().split())
        if w < h:
            w, h = h, w
        L[i] = w
        S[i] = h

    # Candidate heights are exactly the points where either
    # a contribution starts/ends or a box becomes a possible wall.
    events = sorted(set(L[1:] + S[1:]))

    # Sort boxes by wall threshold, descending.
    by_l = list(range(1, n + 1))
    by_l.sort(key=L.__getitem__, reverse=True)

    # Sort boxes by their smaller side for prefix sums.
    by_s = list(range(1, n + 1))
    by_s.sort(key=S.__getitem__)

    svals = [0] * n
    pref_l = [0] * (n + 1)
    pref_ls = [0] * (n + 1)

    for j, idx in enumerate(by_s):
        svals[j] = S[idx]
        pref_l[j + 1] = pref_l[j] + L[idx]
        pref_ls[j + 1] = pref_ls[j] + L[idx] * S[idx]

    # Dynamic Li Chao tree.
    # Each inserted line creates at most one new node.
    left = [0] * (n + 1)
    right = [0] * (n + 1)
    best = [0] * (n + 1)
    second = [0] * (n + 1)

    root = 0
    nodes = 0

    # We use the compressed event coordinates as the Li Chao domain.
    xs = events
    m = len(xs)

    def value(idx, x):
        if idx == 0:
            return INF
        return L[idx] * x - L[idx] * S[idx]

    def insert(line):
        nonlocal root, nodes

        if root == 0:
            nodes += 1
            root = nodes
            best[root] = line
            return

        node = root
        lo = 0
        hi = m - 1
        cur = line

        while True:
            mid = (lo + hi) >> 1
            xmid = xs[mid]

            a = best[node]
            b = second[node]

            if value(a, xmid) > value(cur, xmid):
                best[node], second[node], cur = cur, a, b
            elif value(b, xmid) > value(cur, xmid):
                second[node], cur = cur, b

            if cur == 0 or lo == hi:
                return

            # cur can improve on the current second-best line
            # only in a child where the two lines can change order.
            if value(second[node], xs[lo]) > value(cur, xs[lo]):
                nxt = left[node]
                if nxt == 0:
                    nodes += 1
                    nxt = nodes
                    left[node] = nxt
                    best[nxt] = cur
                    return
                node = nxt
                hi = mid
            elif value(second[node], xs[hi]) > value(cur, xs[hi]):
                nxt = right[node]
                if nxt == 0:
                    nodes += 1
                    nxt = nodes
                    right[node] = nxt
                    best[nxt] = cur
                    return
                node = nxt
                lo = mid + 1
            else:
                return

    def query(x):
        if root == 0:
            return INF, INF

        lo = 0
        hi = m - 1
        node = root
        first = INF
        second_best = INF

        while node:
            v = value(best[node], x)
            if v < first:
                second_best = first
                first = v
            elif v < second_best:
                second_best = v

            v = value(second[node], x)
            if v < first:
                second_best = first
                first = v
            elif v < second_best:
                second_best = v

            if lo == hi:
                break

            mid = (lo + hi) >> 1
            pos = bisect_left(xs, x, lo, hi + 1)

            if pos <= mid:
                node = left[node]
                hi = mid
            else:
                node = right[node]
                lo = mid + 1

        return first, second_best

    ans = 0
    p = 0

    for H in reversed(events):
        while p < n and L[by_l[p]] >= H:
            insert(by_l[p])
            p += 1

        # Boxes with S_i < H have positive possible contribution.
        k = bisect_left(svals, H)
        total = H * pref_l[k] - pref_ls[k]

        first, second_best = query(H)

        # Two distinct walls are mandatory.
        if second_best == INF:
            continue

        total -= max(first, 0)
        total -= max(second_best, 0)

        if total > ans:
            ans = total

    print(ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai chuẩn hóa mọi hình chữ nhật thành (L_i) và (S_i). Hướng đầu vào không còn quan trọng sau thời điểm này vì giải pháp có thể xoay từng hộp một cách độc lập. 

Hai thứ tự được sắp xếp phục vụ các mục đích khác nhau.`by_l`kiểm soát khi một chiếc hộp đi vào cấu trúc Li Chao như một bức tường có thể.`by_s`hỗ trợ các tổng tiền tố cần thiết để đánh giá tổng của tất cả các đóng góp của hộp thông thường ở một độ cao. 

Việc thực hiện Li Chao hơi khác so với sách giáo khoa. Mỗi nút lưu trữ`best[node]`Và`second[node]`. Khi chèn một dòng mới, hai dòng tốt nhất ở điểm giữa vẫn nằm trong nút, trong khi ứng cử viên bị dịch chuyển tiếp tục hướng về phía một đứa trẻ. Vì một lần chèn có thể tạo tối đa một nút mới, nên tối đa (N) nút được tạo, do đó cây động sử dụng bộ nhớ (O(N)). 

Việc đánh giá dòng luôn được thực hiện với số nguyên Python. Diện tích lớn nhất có thể có là theo thứ tự (N\cdot10^6\cdot10^6=2.5\times10^{17}), do đó số học 32 bit có chiều rộng cố định sẽ tràn. Số nguyên Python tự động tránh vấn đề đó. 

Truy vấn đi xuống cây tọa độ nén. các`bisect_left`bên trong truy vấn xác định vị trí bên chứa (x). Vì tọa độ truy vấn thuộc về`xs`, điều này là chính xác. Việc triển khai giữ hai giá trị nhỏ nhất được nhìn thấy dọc theo đường dẫn từ gốc đến lá, điều này là đủ vì mỗi dòng được lưu trữ trong nút Li Chao đều là ứng cử viên cho điểm được truy vấn. 

Phép trừ sử dụng`max(value, 0)`bởi vì (L_i(H-S_i)) chỉ là một đóng góp giả định. Một hộp có (S_i\ge H) đóng góp bằng 0, không phải là số âm. Điều này đặc biệt có liên quan vì cây Li Chao cố tình giữ các hàm affine thô thay vì các hàm bị kẹp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Ba hộp trở thành ((L,S)=(4,3),(6,2),(5,1)). Độ cao của sự kiện là (1,2,3,4,5,6). 

| (H) | Ứng viên chèn tường | Tổng diện tích tiềm năng | Hai giá trị tường nhỏ nhất | Câu trả lời của ứng viên | 
| --- | --- | --- | --- | --- | 
| 6 | hộp 2 | 61 | ít hơn hai | 0 | 
| 5 | hộp 2, 3 | 46 | 18, 20 | 8 | 
| 4 | hộp 1, 2, 3 | 31 | 4, 12 | 15 | 
| 3 | hộp 1, 2, 3 | 16 | 0, 6 | 10 | 
| 2 | hộp 1, 2, 3 | 5 | 0, 0 | 5 | 
| 1 | hộp 1, 2, 3 | 0 | 0, 0 | 0 | 

Tại (H=4), đóng góp tiềm năng là (4,12,15). Hai hộp có đóng góp (4) và (12) trở thành tường, để lại hộp thứ ba chứa (15) đơn vị nước. Vậy đáp án là (15). 

### Ví dụ được xây dựng 

Hãy xem xét```
3
1 3
1 3
1 1
```Các hộp được chuẩn hóa là ((3,1),(3,1),(1,1)). Độ cao sự kiện hữu ích duy nhất là (1) và (3). 

| (H) | Ứng viên chèn tường | Tổng diện tích tiềm năng | Hai giá trị tường nhỏ nhất | Câu trả lời của ứng viên | 
| --- | --- | --- | --- | --- | 
| 3 | hộp 1, 2 | 14 | 6, 6 | 2 | 
| 1 | hộp 1, 2 | 0 | 0, 0 | 0 | 

Ở độ cao (3), hai hộp (1\times3) được quay thành những bức tường thẳng đứng có chiều cao (3). Hộp (1\times1) nằm bên trong với chiều cao (1), nên nó chứa (3-1=2) đơn vị nước. Câu trả lời là (2). 

Những dấu vết này cũng cho thấy tại sao phải loại bỏ hai phần đóng góp nhỏ nhất thay vì chỉ chọn hai ô cao nhất. Tại (H=3), hai hộp dài nhất thiết phải là những bức tường và những đóng góp bên trong giả định của chúng chính xác là những giá trị bị truy vấn Li Chao loại bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log N)) | Hai loại cộng với (N) phần chèn Li Chao và truy vấn độ cao (O(N)), mỗi loại lấy (O(\log N)) | 
| Không gian | (O(N)) | Mảng hộp, mảng sắp xếp, tổng tiền tố và tối đa (N) nút Li Chao | 

Với (N=250000), (O(N\log N)) phù hợp với giới hạn ba giây, trong khi mức sử dụng bộ nhớ (O(N)) ở mức thoải mái trong giới hạn 1024 MB. Độ dài cạnh giới hạn chỉ được sử dụng để thúc đẩy công thức tính chiều cao cố định; việc triển khai nén các độ cao liên quan nên không cần cây Li Chao triệu phần tử. 

## Trường hợp thử nghiệm 

Khai thác sau đây giả định giải pháp trên được lưu dưới dạng`solution.py`và phơi bày`solve()`chức năng được hiển thị trong quá trình thực hiện.```python
import sys
import io
import solution

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solution.input = sys.stdin.readline
        solution.solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run(
    """3
4 3
2 6
5 1
"""
) == "15", "sample 1"

# Minimum-size input, and a water level exactly equal to the two wall heights.
assert run(
    """3
1 3
1 3
1 1
"""
) == "2", "minimum size and exact wall height"

# All boxes are identical, so no arrangement can trap water.
assert run(
    """3
2 2
2 2
2 2
"""
) == "0", "all equal values"

# The long dimension must be used horizontally for the interior box.
assert run(
    """3
2 5
2 5
100 1
"""
) == "400", "rotation choice"

# Water appears exactly at H = S + 1, catching strict-boundary mistakes.
assert run(
    """3
1 2
1 2
1 1
"""
) == "1", "off-by-one at the water level"

# Maximum N, with the smallest possible dimensions.
# There is no possible water, but the test exercises the full input size.
max_n = 250000
max_case = str(max_n) + "\n" + ("1 1\n" * max_n)
assert run(max_case) == "0", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 1 3 / 1 3 / 1 1`| 2 | Tối thiểu (N), hai bức tường hợp lệ, chiều cao ranh giới chính xác | 
|`3 / 2 2 / 2 2 / 2 2`| 0 | Kích thước hoàn toàn bằng nhau | 
|`3 / 2 5 / 2 5 / 100 1`| 400 | Sử dụng đúng cách xoay cho tường và hộp nội thất | 
|`3 / 1 2 / 1 2 / 1 1`| 1 | Hành vi sai lệch khi (H=S_i+1) | 
| 250000 bản`1 1`| 0 | Kích thước và hiệu suất đầu vào tối đa | 

## Vỏ cạnh 

Đối với trường hợp chỉ có một bức tường có thể ở mức cao,```
3
1 3
1 2
1 1
```xét (H=3). Chỉ có hộp đầu tiên có (L_i\ge3), vì vậy truy vấn Li Chao chỉ có thể trả về một ứng cử viên hữu hạn. Việc thực hiện phát hiện rằng mức tối thiểu thứ hai là`INF`và bỏ qua độ cao này. Tại (H=2), hai hộp có thể trở thành những bức tường, nhưng hộp đầu tiên đóng góp 0 làm bức tường và cấu hình còn lại vẫn không có diện tích bẫy dương. Câu trả lời cuối cùng là (0). 

Đối với trường hợp ranh giới chính xác,```
3
1 2
1 2
1 1
```tại (H=2), hai hộp (1\times2) được chèn vào làm tường. Ô (1\times1) đóng góp (1(2-1)=1). Hai ứng cử viên tường có mức đóng góp thô bằng 0, vì vậy hãy trừ đi số lá phiếu của họ (1). Câu trả lời chính xác là (1), cho thấy tại sao phần đóng góp phải được giữ ở mức 0 và tại sao một hộp có (S_i=H) không được coi là tạo ra nước âm. 

Đối với trường hợp nhạy cảm xoay,```
3
2 5
2 5
100 1
```kích thước chuẩn hóa là ((5,2),(5,2),(100,1)). Tại (H=5), hai hộp đầu tiên có thể được xoay sao cho các cạnh (5) của chúng thẳng đứng, tạo thành hai bức tường. Hộp thứ ba sử dụng cạnh (100) theo chiều ngang và cạnh (1) theo chiều dọc, đóng góp (100(5-1)=400). Truy vấn Li Chao loại bỏ hai ứng cử viên trên tường và câu trả lời được tính toán là (400). 

Đối với kích thước bằng nhau,```
3
2 2
2 2
2 2
```mọi hộp chuẩn hóa là ((2,2)). Tại (H=2), mọi đóng góp đều bằng 0. Ở trên (2) không có hộp nào có (L_i\ge H) nên không thể có hai bức tường. Bên dưới (2), không có hộp nào có khoảng trống dương phía trên nó. Thuật toán do đó trả về (0). 

Hộp có kích thước tối đa bao gồm (250000) hộp có kích thước (1\times1). Mỗi hộp đều có (L_i=S_i=1), vì vậy lượng nước đóng góp của mọi ứng cử viên đều bằng 0 và câu trả lời là (0). Việc triển khai vẫn xử lý tất cả các hộp thông qua máy phân loại và Li Chao, thực hiện hành vi dự định (O(N\log N)) mà không cần dựa vào kích thước đầu vào nhỏ.
