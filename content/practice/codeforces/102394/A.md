---
title: "CF 102394A - Tranh nghệ thuật"
description: "Chúng ta có một hàng (N) khối lập phương và mỗi khối được sơn hoặc để nguyên. Quy tắc loại 1 yêu cầu ít nhất (K) khối được sơn bên trong một khoảng cụ thể ([L,R]). Quy tắc loại 2 yêu cầu ít nhất (K) khối được sơn bên ngoài khoảng đó."
date: "2026-08-11T04:16:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102394
codeforces_index: "A"
codeforces_contest_name: "The 2019 China Collegiate Programming Contest Harbin Site"
rating: 0
weight: 102394
solve_time_s: 352
verified: true
draft: false
---

[CF 102394A - Những bức tranh nghệ thuật](https://codeforces.com/problemset/problem/102394/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 52 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng (N) khối lập phương và mỗi khối được sơn hoặc để nguyên. Quy tắc loại 1 yêu cầu ít nhất (K) khối được sơn bên trong một khoảng cụ thể ([L,R]). Quy tắc loại 2 yêu cầu ít nhất (K) khối được sơn bên ngoài khoảng đó. 

Mục đích không phải là xây dựng một bức tranh một cách rõ ràng. Chúng ta chỉ cần số khối được sơn tối thiểu có thể thỏa mãn mọi quy tắc. Bài toán chính thức có (N\le 3000), (M_1,M_2\le3000), và tổng các đại lượng này trong tất cả các ca kiểm thử cũng nhiều nhất là (3000). Sự cố ban đầu có giới hạn thời gian 1 giây và giới hạn bộ nhớ 512 MB. 

Biểu diễn tự nhiên là tổng tiền tố. Gọi (S_i) là số hình lập phương được sơn giữa các vị trí (1,\ldots,i). Khi đó vị trí vẽ (i) tương ứng với việc tăng tổng tiền tố lên 0 hoặc một: 

[ 
0\le S_i-S_{i-1}\le1. 
] 

Đối với quy tắc loại 1, số được vẽ trong ([L,R]) là (S_R-S_{L-1}), do đó quy tắc trở thành 

[ 
S_R-S_{L-1}\ge K. 
] 

Đối với quy tắc loại 2, số được sơn bên ngoài ([L,R]) là 

[ 
S_N-(S_R-S_{L-1}), 
] 

vì vậy quy tắc trở thành 

[ 
S_N-(S_R-S_{L-1})\ge K. 
] 

Phần rắc rối là (S_N), vì bản thân nó là đại lượng mà chúng ta muốn giảm thiểu. Khi (S_N) được cố định thành một giá trị (X nào đó), mọi ràng buộc sẽ trở thành ràng buộc sai phân chỉ liên quan đến hai tổng tiền tố. Đó là quan sát trung tâm. 

Kích thước (N=3000) loại trừ bất kỳ số mũ nào trong (N). Ngay cả việc liệt kê tất cả các bức tranh cũng cần có (2^{3000}) ứng cử viên. Ở một thái cực khác, một phương pháp (O(N^3)) sẽ bao gồm khoảng (27) tỷ thao tác cơ bản trong trường hợp đơn lẻ lớn nhất, do đó, giải pháp dự định cần khai thác cấu trúc khoảng thưa thớt. Phương pháp được chấp nhận sử dụng các kiểm tra tính khả thi (O(\log N)), mỗi kiểm tra dựa trên một biểu đồ thưa thớt có các cạnh (O(N+M_1+M_2)). 

Có một số trường hợp khó xử lý. Nếu không có quy tắc nào cả, câu trả lời là 0, vì bức tranh không có giá trị gì cả. Ví dụ,```
1
1 0 0
```có câu trả lời```
0
```Việc thực hiện bất cẩn luôn vẽ ít nhất một khối lập phương sẽ thất bại ngay lập tức. 

Một quy tắc có thể có (K=0), quy tắc này không đặt ra yêu cầu thực tế nào đối với bức tranh. Ví dụ,```
1
3 1 0
1 3 0
```có câu trả lời```
0
```Các bất đẳng thức tổng tiền tố phải cho phép sự bình đẳng trong trường hợp này. 

Các ranh giới (L=1) và (R=N) cũng quan trọng vì chúng liên quan đến (S_0) hoặc (S_N). Ví dụ,```
1
1 1 0
1 1 1
```yêu cầu khối 1 phải được sơn nên đáp án là (1). Việc quên vị trí tiền tố (S_0) sẽ làm cho công thức khoảng trở nên khó xử và thường gây ra lỗi từng lỗi một. 

Quy tắc loại 2 trên toàn bộ mảng là một trường hợp ranh giới hữu ích khác. Tập bên ngoài của nó trống nên yêu cầu hợp lệ duy nhất của nó là (K=0). Ví dụ,```
1
3 0 1
1 3 0
```có câu trả lời (0). Việc coi phần bù như một khoảng thông thường khác sẽ không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Hãy thử từng tập hợp con của các khối (N), đếm xem nó vẽ được bao nhiêu khối và kiểm tra mọi quy tắc. Vì có (2^N) tập hợp con và kiểm tra tập hợp con trực tiếp theo tất cả chi phí quy tắc (O(N+M_1+M_2)), công việc trong trường hợp xấu nhất là 

[ 
O\left(2^N(N+M_1+M_2)\right). 
] 

Đối với (N=3000), đây là khoảng (6000\cdot2^{3000}) kiểm tra cơ bản trong một trường hợp lớn, điều này hoàn toàn không khả thi. 

Lực lượng vũ phu có tác dụng vì mọi bức tranh có thể đều được thể hiện rõ ràng, vì vậy không có nghi ngờ gì về tính chính xác. Vấn đề là cấu trúc thú vị bị ẩn bởi phép liệt kê theo cấp số nhân. 

Quan sát hữu ích đầu tiên là thay thế các quyết định về khối được vẽ riêng lẻ bằng tổng tiền tố. Khi đã biết (S_i), số khối lập phương được sơn trong bất kỳ khoảng nào chỉ là hiệu của hai tổng tiền tố. Bản chất nhị phân của mỗi khối cũng được thể hiện bằng các bất đẳng thức đơn giản 

[ 
S_i-S_{i-1}\ge0 
] 

và 

[ 
S_i-S_{i-1}\le1. 
] 

Bây giờ giả sử số khối được sơn cuối cùng được cố định là (X=S_N). Quy tắc loại 1 trở thành 

[ 
S_{L-1}-S_R\le-K. 
] 

Quy tắc loại 2 trở thành 

[ 
S_R-S_{L-1}\le X-K. 
] 

Tổng số được cố định bởi 

[ 
S_N-S_0=X, 
] 

có thể biểu diễn bằng hai bất đẳng thức 

[ 
S_N-S_0\le X 
] 

và 

[ 
S_0-S_N\le-X. 
] 

Mọi ràng buộc kết quả đều có dạng chuẩn 

[ 
S_v\le S_u+w. 
] 

Những hệ thống như vậy được gọi là các ràng buộc khác biệt. Chúng có thể được biểu diễn bằng một cạnh có hướng (u\to v) với trọng số (w). Một hệ thống khả thi không có chu trình âm và ngược lại, nếu đồ thị ràng buộc không có chu trình âm, khoảng cách đường đi ngắn nhất sẽ cung cấp một phép gán khả thi. 

Điều này để lại một câu hỏi: làm thế nào chúng ta tìm được giá trị nhỏ nhất khả thi (X)? Tính khả thi là đơn điệu. Nếu một bức tranh có các khối được sơn (X) thỏa mãn mọi quy tắc, hãy sơn thêm một khối không sơn. Số lượng loại 1 chỉ có thể tăng lên. Đối với quy tắc loại 2, số khối được sơn bên ngoài khoảng của nó sẽ không thay đổi khi khối mới ở trong khoảng hoặc tăng lên khi khối mới ở bên ngoài. Vì vậy, mọi số lượng hình khối được sơn lớn hơn cũng có thể thực hiện được. 

Do đó, chúng ta có thể tìm kiếm nhị phân (X) từ (0) đến (N). Đối với mỗi ứng cử viên (X), chúng tôi xây dựng biểu đồ ràng buộc chênh lệch tương ứng và sử dụng SPFA để kiểm tra xem nó có chứa chu trình âm hay không. Bài xã luận chính thức đưa ra chính xác công thức tìm kiếm nhị phân cộng với SPFA này, với độ phức tạp (O(NM\log N)) khi (M) biểu thị số lượng ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^N(N+M_1+M_2))) | (O(N+M_1+M_2)) | Quá chậm | 
| Tìm kiếm nhị phân + SPFA | (O(N(N+M_1+M_2)\log N)) trường hợp xấu nhất | (O(N+M_1+M_2)) | Đã chấp nhận | 

Bản thân SPFA có cùng trường hợp xấu nhất (O(VE)) bị ràng buộc như Bellman-Ford, mặc dù việc triển khai dựa trên hàng đợi nhanh hơn nhiều trên các biểu đồ thưa thớt phát sinh ở đây. Bài xã luận chính thức rõ ràng dựa vào việc cắt tỉa thực tế của SPFA cho giải pháp (O(NM\log N)) được chấp nhận. 

## Hướng dẫn thuật toán

1. Xác định (S_i) là số hình lập phương được sơn giữa các vị trí (1) đến (i). Giá trị (S_0) bằng 0 và (S_N) chính xác là số khối được sơn. 
2. Sửa câu trả lời của thí sinh (X). Chúng tôi tạm thời yêu cầu (S_N=X). Lý do thực hiện điều này là vì mọi quy tắc chứa (S_N) khi đó sẽ trở thành một ràng buộc sai phân thông thường với vế phải không đổi. 
3. Thêm hai ràng buộc 
[ 
S_{i-1}-S_i\le0 
] 
và 
[ 
S_i-S_{i-1}\le1 
] 
cho mọi vị trí (i). Những điều này buộc mỗi hiệu (S_i-S_{i-1}) bằng 0 hoặc bằng một, khớp chính xác với một khối lập phương không sơn hoặc sơn. 
4. Chuyển mọi quy tắc loại 1 ((L,R,K)) thành 
[ 
S_{L-1}-S_R\le-K. 
] 
Đây chỉ là điều kiện ban đầu (S_R-S_{L-1}\ge K) với các số hạng được sắp xếp lại. 
5. Chuyển mọi quy tắc loại 2 ((L,R,K)) thành 
[ 
S_R-S_{L-1}\le X-K. 
] 
Bắt đầu từ (X-(S_R-S_{L-1})\ge K), điều này nói lên rằng số được sơn bên trong khoảng không thể vượt quá (X-K). 
6. Buộc tổng bằng (X) bằng cách thêm 
[ 
S_N-S_0\le X 
] 
và 
[ 
S_0-S_N\le-X. 
] 
Những thứ này cùng nhau cho ra (S_N-S_0=X) và (S_0=0). 
7. Biến mọi bất đẳng thức (S_v-S_u\le w) thành cạnh có hướng (u\to v) có trọng số (w). Khi đó đường thư giãn có dạng giống hệt như bất đẳng thức ban đầu: 
[ 
S_v\le S_u+w. 
] 
8. Chạy SPFA để tìm chu kỳ âm. Biểu đồ chứa tất cả các đỉnh có liên quan có thể tiếp cận được từ đỉnh (0) vì các cạnh tổng tiền tố bao gồm cạnh chuyển tiếp từ (i-1) đến (i). Nếu tồn tại một chu trình âm thì ứng viên (X) là không thể. Nếu không có chu trình âm thì hệ ràng buộc sai phân có nghiệm nên (X) là khả thi. 
9. Tìm kiếm nhị phân khả thi nhỏ nhất (X). Bắt đầu với phạm vi ([0,N]). Điểm cuối trên luôn khả thi vì việc vẽ mọi khối đều thỏa mãn mọi quy tắc được giới hạn đầu vào cho phép. Khi điểm giữa khả thi, hãy tìm kiếm nửa dưới. Nếu không hãy tìm kiếm nửa trên. 

### Tại sao nó hoạt động 

Đối với một (X) cố định, đồ thị biểu thị chính xác các tổng tiền tố có thể có của một bức tranh có các khối được sơn (X). Mọi bức tranh hợp lệ đều thỏa mãn mọi bất đẳng thức đồ thị nên không thể tạo ra chu trình âm. Ngược lại, nếu đồ thị không có chu kỳ âm, khoảng cách đường đi ngắn nhất thỏa mãn mọi ràng buộc sai phân và các ràng buộc (0\le S_i-S_{i-1}\le1) làm cho mỗi hiệu tiền tố trở thành một số nguyên trong ({0,1}). Do đó những khác biệt đó mô tả một bức tranh thực tế. Hai ràng buộc liên quan đến (S_N) buộc tổng số khối được sơn của nó phải chính xác (X). Vì tính khả thi là đơn điệu trong (X), tìm kiếm nhị phân trả về số nhỏ nhất có thể. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    input = sys.stdin.readline
    t = int(input())
    answers = []

    for _ in range(t):
        n, m1, m2 = map(int, input().split())

        type1 = [tuple(map(int, input().split())) for _ in range(m1)]
        type2 = [tuple(map(int, input().split())) for _ in range(m2)]

        def feasible(x):
            g = [[] for _ in range(n + 1)]

            # 0 <= S[i] - S[i-1] <= 1
            for i in range(1, n + 1):
                g[i].append((i - 1, 0))
                g[i - 1].append((i, 1))

            # S[L-1] - S[R] <= -K
            for l, r, k in type1:
                g[r].append((l - 1, -k))

            # S[R] - S[L-1] <= X - K
            for l, r, k in type2:
                g[l - 1].append((r, x - k))

            # S[N] - S[0] <= X
            # S[0] - S[N] <= -X
            g[0].append((n, x))
            g[n].append((0, -x))

            # SPFA, starting from 0.
            # The chain edges make every vertex reachable from 0.
            vcnt = n + 1
            dist = [0] * vcnt
            in_queue = [False] * vcnt
            relax_count = [0] * vcnt

            q = deque([0])
            in_queue[0] = True
            relax_count[0] = 1

            while q:
                u = q.popleft()
                in_queue[u] = False
                du = dist[u]

                for v, w in g[u]:
                    nd = du + w
                    if nd < dist[v]:
                        dist[v] = nd

                        if not in_queue[v]:
                            q.append(v)
                            in_queue[v] = True
                            relax_count[v] += 1

                            if relax_count[v] >= vcnt:
                                return False

            return True

        lo, hi = 0, n

        while lo < hi:
            mid = (lo + hi) // 2
            if feasible(mid):
                hi = mid
            else:
                lo = mid + 1

        answers.append(str(lo))

    return "\n".join(answers)

if __name__ == "__main__":
    sys.stdout.write(solve())
```Đầu vào được đọc theo từng trường hợp và hai loại ràng buộc này được lưu trữ riêng biệt vì các cạnh biểu đồ của chúng có sự phụ thuộc khác nhau vào giá trị tìm kiếm nhị phân (X). 

Bên trong`feasible`, đồ thị có (N+1) đỉnh được đánh số từ (0) đến (N), tương ứng trực tiếp với (S_0,\ldots,S_N). Đối với mỗi vị trí, cạnh (i\to i-1) có trọng số bằng 0 đại diện cho (S_{i-1}\le S_i), trong khi (i-1\to i) có trọng số 1 đại diện cho (S_i\le S_{i-1}+1). Họ cùng nhau thực thi mức tăng nhị phân. 

Cạnh loại 1 là`g[r].append((l - 1, -k))`. Điều kiện thư giãn của nó là (S_{L-1}\le S_R-K), chính xác là giới hạn dưới cần thiết của khoảng. 

Cạnh loại 2 bị đảo ngược so với loại 1. Trọng lượng của nó là`x - k`, bởi vì việc sửa (S_N=X) biến đổi điều kiện đếm bên ngoài thành (S_R\le S_{L-1}+X-K). 

Hai cạnh cuối cùng đều cần thiết. Chỉ sử dụng`0 -> n`sẽ cho (S_N\le X), trong khi chỉ sử dụng`n -> 0`sẽ cho (S_N\ge X). Sự kết hợp của họ sửa chữa tổng thể một cách chính xác. 

Mảng khoảng cách SPFA có thể bắt đầu bằng tất cả các số 0 nếu mọi đỉnh được coi là có thể truy cập được, nhưng việc triển khai này chỉ bắt đầu từ đỉnh 0. Điều đó là an toàn vì các cạnh tiền tố chuyển tiếp cung cấp một đường dẫn từ 0 đến mọi đỉnh. Do đó, một chu trình âm có thể đạt được từ bất kỳ đỉnh nào cũng có thể đạt được từ 0. 

Bộ đếm thư giãn phát hiện chu kỳ âm trước khi SPFA có thể tiếp tục vô thời hạn. Trong đồ thị có đỉnh (V) và không có chu trình âm, đường đi ngắn nhất không yêu cầu đường đi cải tiến liên tục chứa ít nhất (V) cạnh. Một đỉnh được thả lỏng nhiều lần báo hiệu một chu kỳ âm. 

Số nguyên Python loại bỏ mọi lo ngại về tràn. Các giá trị lớn nhất liên quan chỉ ở mức (N), mặc dù khoảng cách đường đi ngắn nhất tích lũy có thể âm. 

Việc tìm kiếm nhị phân sử dụng`hi = n`vì việc vẽ tất cả (N) hình khối luôn thỏa mãn mọi quy luật. Giới hạn đầu vào đảm bảo rằng yêu cầu loại 1 không bao giờ yêu cầu nhiều hơn độ dài khoảng và yêu cầu loại 2 không bao giờ yêu cầu nhiều hơn kích thước phần bù của nó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu thực tế chứa một trường hợp thử nghiệm:```
1
3 1 1
1 2 1
2 2 1
```Quy tắc đầu tiên yêu cầu ít nhất một khối được sơn trong số các vị trí 1 và 2. Quy tắc thứ hai yêu cầu ít nhất một khối được sơn bên ngoài vị trí 2, do đó vị trí 1 hoặc vị trí 3 phải được sơn. 

Tìm kiếm nhị phân hoạt động như sau. 

|`lo`|`hi`|`mid`| Phiên dịch ứng viên | Khả thi? | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 1 | Sơn chính xác 1 khối | Có | 
| 0 | 1 | 0 | Sơn chính xác 0 hình khối | Không | 

Với (X=1), khối sơn 1 có tác dụng. Nó thỏa mãn quy tắc thứ nhất vì khối 1 nằm trong ([1,2]) và nó thỏa mãn quy tắc thứ hai vì khối 1 nằm bên ngoài ([2,2]). Do đó, câu trả lời là 1. Tuyên bố chính thức đưa ra mẫu và kết quả tương tự. 

### Mẫu 2 

Hãy xem xét trường hợp sau:```
1
5 1 1
2 4 2
2 4 1
```Quy tắc loại 1 yêu cầu ít nhất hai khối được sơn ở các vị trí từ 2 đến 4. Quy tắc loại 2 yêu cầu ít nhất một khối được sơn bên ngoài các vị trí từ 2 đến 4, do đó, ít nhất một trong các vị trí 1 và 5 cũng phải được sơn. 

Tìm kiếm nhị phân là: 

|`lo`|`hi`|`mid`| Phiên dịch ứng viên | Khả thi? | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | 2 | Chính xác là 2 khối sơn | Không | 
| 3 | 5 | 4 | Chính xác là 4 khối sơn | Có | 
| 3 | 4 | 3 | Chính xác là 3 khối sơn | Có | 

Chỉ với hai khối được sơn, cả hai đều phải nằm ở vị trí từ 2 đến 4 để thỏa mãn quy tắc đầu tiên, không để khối nào được sơn ngoài khoảng đó. Do đó (X=2) là không thể. Với ba hình lập phương được sơn, các vị trí 1, 2 và 3 đều đúng nên đáp án là 3. 

Ví dụ này chứng minh tại sao chỉ lấy số khoảng yêu cầu lớn nhất là không đủ. Hai loại quy tắc tương tác thông qua tổng số khối được sơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N(N+M_1+M_2)\log N)) trường hợp xấu nhất | Có (O(\log N)) kiểm tra tính khả thi, mỗi kiểm tra SPFA có (O(VE)) độ phức tạp trong trường hợp xấu nhất với (V=O(N)) và (E=O(N+M_1+M_2)). | 
| Không gian | (O(N+M_1+M_2)) | Biểu đồ, khoảng cách, trạng thái hàng đợi và mảng ràng buộc đều có kích thước tuyến tính. | 

Bài xã luận chính thức đưa ra giới hạn tương tự (O(NM\log N)) cho phương pháp tìm kiếm nhị phân cộng với SPFA và nhận thấy rằng việc cắt tỉa SPFA là đủ cho các giới hạn của cuộc thi. Vì tổng của (N), (M_1) và (M_2) trên tất cả các trường hợp thử nghiệm đều được giới hạn bởi 3000, nên tổng đầu vào nhỏ hơn đáng kể so với việc xử lý mọi trường hợp thử nghiệm như một trường hợp xấu nhất độc lập. 

## Trường hợp thử nghiệm 

Đoạn trích trong câu hỏi bị thiếu phần dẫn đầu`1`từ đầu vào mẫu thực tế. Mẫu chính thức hoàn chỉnh là mẫu được sử dụng dưới đây. 

Các thử nghiệm sau đây giả định rằng giải pháp đã gửi được lưu dưới dạng`solution.py`.```python
from solution import solve
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve().strip()
    finally:
        sys.stdin = old_stdin

# Official sample
assert run(
    """1
3 1 1
1 2 1
2 2 1
"""
) == "1", "official sample"

# Minimum-size input, no constraints.
assert run(
    """1
1 0 0
"""
) == "0", "empty constraint set"

# Boundary condition L = R = 1, type 1 forces the only cube to be painted.
assert run(
    """1
1 1 0
1 1 1
"""
) == "1", "single-cube type 1"

# Type 2 complement at the boundary.
# Outside [1, 1] are positions 2, 3, 4, so two of them must be painted.
assert run(
    """1
4 0 1
1 1 2
"""
) == "2", "left boundary complement"

# Several identical constraints.
assert run(
    """1
4 3 0
1 4 2
1 4 2
1 4 2
"""
) == "2", "duplicate constraints"

# Interaction between type 1 and type 2 constraints.
assert run(
    """1
5 1 1
2 4 2
2 4 1
"""
) == "3", "inside and outside requirements"

# Maximum-size case: N = 3000 and 3000 constraints.
# Every identical type 1 rule requires all 3000 cubes.
n = 3000
max_case = "1\n{} 3000 0\n".format(n)
max_case += ("1 3000 3000\n" * 3000)
assert run(max_case) == "3000", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 0 0`|`0`| Trường hợp kích thước tối thiểu và không có ràng buộc | 
|`1 / 1 1 0 / 1 1 1`|`1`| (L=R=1) và (S_0) xử lý ranh giới | 
|`1 / 4 0 1 / 1 1 2`|`2`| Khoảng bù nhau tại ranh giới bên trái | 
|`1 / 4 3 0 / 1 4 2`lặp đi lặp lại |`2`| Ràng buộc trùng lặp và giống hệt nhau | 
|`1 / 5 1 1 / 2 4 2 / 2 4 1`|`3`| Tương tác giữa yêu cầu khoảng và yêu cầu bổ sung | 
|`N=3000`, 3000 quy tắc giống hệt nhau |`3000`| Số lượng hạn chế tối đa (N) và tối đa | 

## Vỏ cạnh 

Khi không có quy tắc nào, biểu đồ chỉ chứa các ràng buộc tiền tố-sai phân và các ràng buộc tổng cố định. Vì```
1
1 0 0
```ứng viên (X=0) tạo ra (S_0=S_1=0), do đó không có chu kỳ âm và tìm kiếm nhị phân trả về 0. Thuật toán không bao giờ giả định rằng ít nhất một khối lập phương phải được sơn. 

Khi một quy tắc có (K=0), cạnh đồ thị của nó có trọng số bằng 0 và không áp đặt hạn chế bổ sung nào ngoài hạn chế đã được cấu trúc tiền tố ngụ ý. Vì```
1
3 1 0
1 3 0
```ứng viên (X=0) là khả thi, đưa ra câu trả lời đúng là 0. 

Đối với một khối đơn có yêu cầu loại 1,```
1
1 1 0
1 1 1
```ràng buộc trở thành (S_0-S_1\le-1), trong khi ràng buộc tiền tố nhị phân cho (S_1-S_0\le1) và (S_0-S_1\le0). Giá trị duy nhất có thể là (S_1=1), vì vậy câu trả lời là một. Điều này thực hiện trực tiếp ranh giới (L-1=0). 

Đối với trường hợp ranh giới loại 2,```
1
4 0 1
1 1 2
```bên ngoài của ([1,1]) bao gồm các vị trí 2, 3 và 4. Quy tắc yêu cầu hai khối được sơn ở đó, vì vậy hai khối là đủ và cần thiết. Trong biểu đồ, quy tắc trở thành (S_1-S_0\le X-2), đây là phép chuyển đổi chính xác của điều kiện bù. 

Trường hợp khó phát hiện nhất là khi yêu cầu về khoảng và phần bù kéo theo các hướng ngược nhau. TRONG```
1
5 1 1
2 4 2
2 4 1
```hai khối lập phương phải được sơn bên trong các vị trí từ 2 đến 4, còn một khối lập phương khác phải được sơn ở bên ngoài. Do đó, mức tối thiểu là ba. Việc kiểm tra (X=2) tạo ra một hệ thống ràng buộc khác biệt không nhất quán, do đó SPFA tìm thấy một chu kỳ âm. Việc kiểm tra (X=3) loại bỏ mâu thuẫn đó và tìm kiếm nhị phân dừng ở đó. 

Cuối cùng, các giá trị (X) lớn hơn luôn an toàn khi một số (X) khả thi. Việc thêm một khối được sơn khác không thể ảnh hưởng đến điều kiện loại 1, vì số khoảng cách của nó chỉ có thể tăng lên. Đối với điều kiện loại 2, số lượng bên ngoài của nó không thay đổi hoặc tăng lên. Tính đơn điệu này chính là điều làm cho việc tìm kiếm nhị phân trở nên có giá trị hơn là chỉ mang tính tự nghiệm.
