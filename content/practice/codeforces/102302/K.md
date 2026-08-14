---
title: "CF 102302K - Kẹo"
description: "Chúng ta có một mảng số nguyên C có độ dài N. Chuỗi kẹo là bất kỳ phần liền kề không trống nào của mảng này và giá trị của nó là tổng các phần tử của nó. Câu trả lời bắt buộc không phải là số lượng vị trí xuất hiện một chuỗi hợp lệ."
date: "2026-08-13T23:32:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102302
codeforces_index: "K"
codeforces_contest_name: "2019 USP-ICMC"
rating: 0
weight: 102302
solve_time_s: 191
verified: true
draft: false
---

[CF 102302K - Kẹo](https://codeforces.com/problemset/problem/102302/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 11s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng số nguyên`C`chiều dài`N`. Chuỗi kẹo là bất kỳ phần liền kề không trống nào của mảng này và giá trị của nó là tổng các phần tử của nó. Câu trả lời bắt buộc không phải là số lượng vị trí xuất hiện một chuỗi hợp lệ. Các chuỗi giá trị bằng nhau được hợp nhất, vì vậy`[4]`xảy ra năm lần vẫn được tính là một chuỗi riêng biệt, trong khi`[4]`Và`[4, 4]`khác nhau vì độ dài của chúng khác nhau. 

Dòng đầu tiên cho`N`, tiếp theo là tổng dưới và tổng trên cho phép`L`Và`R`. Dòng thứ hai chứa các giá trị kẹo. Chúng ta cần số mảng liền kề khác nhau có tổng nằm trong khoảng bao gồm`[L, R]`. 

Với`N`lớn như`5 * 10^5`, liệt kê tất cả`N(N+1)/2`khoảng thời gian đã cho biết về`1.25 * 10^11`ứng viên trong trường hợp xấu nhất. Thuật toán bậc hai vượt xa giới hạn 4 giây có thể hỗ trợ. Giá trị kẹo cũng có thể âm nên không áp dụng được các kỹ thuật dựa trên cửa sổ trượt đơn điệu. Giới hạn trên`L`Và`R`với tới`10^18`, trong khi các giá trị riêng lẻ đạt`10^9`, do đó, tổng tiền tố và so sánh phải được xử lý bằng số học 64 bit trong các ngôn ngữ có số nguyên có chiều rộng cố định. 

Có ba trường hợp thường gây ra giải pháp sai. 

Coi như```
2 2 2
2 2
```Có hai trường hợp xảy ra`[2]`, nhưng chúng đại diện cho cùng một trình tự riêng biệt. Câu trả lời đúng là`1`. Một giải pháp đếm các khoảng thời gian hợp lệ thay vì trả về các chuỗi riêng biệt`2`. 

Giá trị âm phá vỡ đối số hai con trỏ thông thường. Ví dụ,```
3 -1 1
1 -1 1
```Các chuỗi phân biệt hợp lệ là`[1]`,`[-1]`,`[1,-1]`,`[-1,1]`, Và`[1,-1,1]`, cho`5`. Một cửa sổ trượt giả định việc mở rộng điểm cuối bên phải luôn làm tăng tổng không thể suy luận chính xác về mảng này. 

Các ranh giới là bao gồm. Vì```
2 2 2
2 3
```chỉ một`[2]`có tổng chính xác`2`, vậy câu trả lời là`1`. Một giải pháp vô tình sử dụng bất đẳng thức nghiêm ngặt cho một trong hai ranh giới sẽ làm mất chuỗi này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp xem xét mọi cặp điểm cuối và duy trì tổng hiện tại bằng tổng tiền tố. Đối với mỗi khoảng, chúng ta có thể kiểm tra xem tổng của nó có nằm trong`[L,R]`và chèn chuỗi tương ứng vào một bộ. Điều này đúng vì mỗi chuỗi liền kề có chính xác một cặp điểm cuối và tập hợp này sẽ loại bỏ các chuỗi lặp lại. Vấn đề là số khoảng: với`N = 500000`, có`N(N+1)/2 = 125000250000`của họ. Ngay cả việc biểu thị mỗi khoảng bằng một hàm băm cũng sẽ để lại cho chúng ta khoảng`1.25 * 10^11`các ứng cử viên, điều này vốn là không thể, trước khi tính đến chi phí duy trì hoặc so sánh các trình tự thực tế. 

Quan sát quan trọng là một chuỗi liền kề là tiền tố của một hậu tố nào đó của mảng ban đầu. Giả sử chúng ta sửa một hậu tố bắt đầu ở vị trí`i`. Tiền tố của nó có độ dài`1, 2, ..., N-i`. Nếu chúng tôi xử lý các hậu tố theo thứ tự từ điển, thì các tiền tố đã được biểu thị bằng hậu tố trước đó chính xác là các tiền tố cho đến tiền tố chung dài nhất có hậu tố trước đó. 

Cho phép`lcp[i]`là độ dài LCP được liên kết với hậu tố bắt đầu từ`i`, trong đó hậu tố trước có nghĩa là hậu tố ngay trước nó theo thứ tự mảng hậu tố. Sau đó hậu tố`i`đóng góp chính xác độ dài tiền tố 

[ 
lcp[i]+1,\ lcp[i]+2,\ \ldots,\ N-i. 
] 

Đây là cách sử dụng mảng hậu tố thông thường để đếm các chuỗi con riêng biệt, nhưng ở đây chúng ta không thể đếm tất cả các độ dài này một cách đơn giản. Chúng ta chỉ được giữ lại những người có tổng nằm trong`[L,R]`. 

Tổng tiền tố biến điều kiện đó thành truy vấn phạm vi. Xác định 

[ 
P[0]=0,\qquad P[j]=C_0+C_1+\cdots+C_{j-1}. 
] 

Tiền tố có độ dài`k`của hậu tố bắt đầu tại`i`kết thúc ở vị trí tổng tiền tố`j=i+k`, và tổng của nó là 

[ 
P[j]-P[i]. 
] 

Do đó tiền tố có giá trị chính xác khi 

[ 
P[i]+L \le P[j] \le P[i]+R. 
] 

Do đó, đối với mỗi hậu tố, chúng ta cần đếm vị trí điểm cuối`j`bên trong phạm vi chỉ số 

[ 
i+lcp[i]+1 \le j \le N 
] 

có tổng tiền tố thuộc về khoảng giá trị`[P[i]+L, P[i]+R]`. 

Các truy vấn trở thành hai chiều: một tọa độ là vị trí điểm cuối`j`, và cái còn lại là tổng tiền tố của nó`P[j]`. Chúng tôi có thể xử lý chúng ngoại tuyến. Sắp xếp vị trí bắt đầu hậu tố theo`P[i]`. Sau đó cả hai ranh giới truy vấn`P[i]+L`Và`P[i]+R`di chuyển một cách đơn điệu. Duy trì trong cây Fenwick chính xác các vị trí điểm cuối có tổng tiền tố hiện nằm trong khoảng giá trị được yêu cầu. Sau đó, truy vấn tiền tố Fenwick sẽ loại bỏ các điểm cuối xảy ra trước điểm cuối bắt đầu được phép. 

Bản thân mảng hậu tố được xây dựng bằng thuật toán nhân đôi tiêu chuẩn và sắp xếp đếm, đưa ra`O(N log N)`thời gian xây dựng. Thuật toán của Kasai sau đó tính toán tất cả các giá trị LCP theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) hoặc tệ hơn | O(N²) trong biểu diễn tập hợp trực tiếp | Quá chậm | 
| Tối ưu | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng hậu tố của`C`. Mảng hậu tố lưu trữ tất cả các vị trí bắt đầu được sắp xếp theo thứ tự từ điển của các hậu tố bắt đầu từ đó. Một trọng điểm duy nhất nhỏ hơn mọi giá trị kẹo được thêm vào bên trong để có thể sử dụng thuật toán nhân đôi dịch chuyển theo chu kỳ tiêu chuẩn. 
2. Tính giá trị LCP cho mọi hậu tố bằng thuật toán Kasai. Đối với hậu tố ở vị trí`i`,`lcp[i]`là độ dài của tiền tố chung của nó với hậu tố ngay trước nó theo thứ tự mảng hậu tố. Nếu không có hậu tố trước đó thì giá trị của nó bằng 0. 
3. Xây dựng mảng tổng tiền tố`P`. Đối với điểm cuối`j`, tổng của mảng con bắt đầu tại`i`và kết thúc tại`j-1`là`P[j] - P[i]`. 
4. Đối với hậu tố bắt đầu tại`i`, đặt 

[ 
left_i=i+lcp[i]+1. 
] 

Chỉ điểm cuối`j >= left_i`đại diện cho các chuỗi con mới khi hậu tố này được xử lý. Điểm cuối trước`left_i`tương ứng với các tiền tố đã được đại diện bởi một hậu tố trước đó. 

1. Sắp xếp tất cả các vị trí bắt đầu hậu tố`i`qua`P[i]`và sắp xếp độc lập tất cả các vị trí điểm cuối`j`từ`1`bởi vì`N`qua`P[j]`. Điều này đưa ra một thứ tự chung trong đó các ngưỡng tổng tiền tố có thể được xử lý. 
2. Quét các hậu tố tăng dần`P[i]`. Duy trì cây Fenwick chứa các vị trí điểm cuối`j`thỏa mãn 

[ 
P[i]+L \le P[j] \le P[i]+R. 
] 

Bởi vì các hậu tố được xử lý tăng dần`P[i]`, cả hai giới hạn chỉ di chuyển sang bên phải trong danh sách điểm cuối đã sắp xếp. Thêm điểm cuối khi chúng vào giới hạn trên và xóa chúng khi chúng rơi xuống dưới giới hạn dưới. 

1. Đối với hậu tố hiện tại, hãy`active`là số điểm cuối hiện có trong cây Fenwick. Truy vấn tiền tố Fenwick tại`left_i-1`đếm các điểm cuối hoạt động xảy ra quá sớm, vì vậy 

[ 
active-\operatorname{prefixSum}(left_i-1) 
] 

chính xác là số tiền tố mới, riêng biệt của hậu tố này có tổng thuộc về`[L,R]`. 

1. Thêm phần đóng góp này vào câu trả lời cho mọi hậu tố. Tổng là số lượng yêu cầu của các chuỗi liên tiếp hợp lệ riêng biệt. 

**Tại sao nó hoạt động.** Mỗi chuỗi liền kề là tiền tố của chính xác một lần xuất hiện hậu tố, nhưng cùng một chuỗi có thể là tiền tố của nhiều hậu tố. Theo thứ tự mảng hậu tố, tất cả các hậu tố chia sẻ tiền tố sẽ tạo thành một nhóm liền kề. Do đó, đối với một hậu tố, mọi tiền tố có độ dài tối đa LCP của nó với hậu tố trước đó đã được biểu diễn, trong khi mọi tiền tố dài hơn đều mới. giá trị`left_i`nắm bắt chính xác sự khác biệt này. Quá trình quét Fenwick đếm chính xác những điểm cuối tiền tố mới có tổng thỏa mãn cả hai giới hạn bao gồm. Vì mỗi chuỗi hợp lệ riêng biệt đều mới ở chính xác một hậu tố và mọi tiền tố được đếm đều hợp lệ, nên câu trả lời tích lũy chứa mỗi chuỗi bắt buộc đúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_suffix_array(a):
    """Suffix array of an integer array using doubling + counting sort."""
    values = sorted(set(a))
    compress = {x: i + 1 for i, x in enumerate(values)}

    # 0 is a unique sentinel smaller than every real value.
    s = [compress[x] for x in a] + [0]
    m = len(s)

    # Initial counting sort by the first value.
    alphabet = len(values) + 1
    cnt = [0] * alphabet
    for x in s:
        cnt[x] += 1

    pos = [0] * alphabet
    total = 0
    for i in range(alphabet):
        pos[i] = total
        total += cnt[i]

    p = [0] * m
    for i, x in enumerate(s):
        p[pos[x]] = i
        pos[x] += 1

    # Initial equivalence classes.
    c = [0] * m
    classes = 1
    c[p[0]] = 0
    for i in range(1, m):
        if s[p[i]] != s[p[i - 1]]:
            classes += 1
        c[p[i]] = classes - 1

    length = 1

    while length < m:
        # Shift every suffix start left by 'length'.
        pn = [0] * m
        for i, x in enumerate(p):
            y = x - length
            if y < 0:
                y += m
            pn[i] = y

        # Counting sort by the first half's class.
        cnt = [0] * classes
        for x in pn:
            cnt[c[x]] += 1

        pos = [0] * classes
        total = 0
        for i in range(classes):
            pos[i] = total
            total += cnt[i]

        p_new = [0] * m
        for x in pn:
            cl = c[x]
            p_new[pos[cl]] = x
            pos[cl] += 1

        # Recompute classes for pairs of length 2 * length.
        c_new = [0] * m
        new_classes = 1
        c_new[p_new[0]] = 0

        for i in range(1, m):
            cur = p_new[i]
            prev = p_new[i - 1]

            cur_second = cur + length
            if cur_second >= m:
                cur_second -= m

            prev_second = prev + length
            if prev_second >= m:
                prev_second -= m

            if (
                c[cur] != c[prev]
                or c[cur_second] != c[prev_second]
            ):
                new_classes += 1

            c_new[cur] = new_classes - 1

        p = p_new
        c = c_new
        classes = new_classes
        length <<= 1

    # The sentinel suffix is always first.
    return p[1:]

def build_lcp(a, sa):
    """lcp[i] = LCP of suffix i with its previous suffix in SA order."""
    n = len(a)
    rank = [0] * n

    for r, pos in enumerate(sa):
        rank[pos] = r

    lcp = [0] * n
    h = 0

    for i in range(n):
        r = rank[i]

        if r == 0:
            h = 0
            continue

        j = sa[r - 1]

        while i + h < n and j + h < n and a[i + h] == a[j + h]:
            h += 1

        lcp[i] = h

        if h:
            h -= 1

    return lcp

def solve():
    n, L, R = map(int, input().split())
    a = list(map(int, input().split()))

    if n == 0:
        print(0)
        return

    sa = build_suffix_array(a)
    lcp = build_lcp(a, sa)

    # P[j] is the sum of a[0:j].
    pref = [0] * (n + 1)
    s = 0
    for i, x in enumerate(a):
        s += x
        pref[i + 1] = s

    # Query suffixes by P[i].
    query_order = sorted(range(n), key=pref.__getitem__)

    # Endpoint j is represented by prefix sum P[j], j in [1, n].
    endpoint_order = sorted(range(1, n + 1), key=pref.__getitem__)

    # Fenwick tree over endpoint positions.
    bit = [0] * (n + 1)

    def add(idx, delta):
        while idx <= n:
            bit[idx] += delta
            idx += idx & -idx

    def prefix_sum(idx):
        result = 0
        while idx > 0:
            result += bit[idx]
            idx -= idx & -idx
        return result

    hi = 0
    lo = 0
    active = 0
    answer = 0

    for i in query_order:
        low_value = pref[i] + L
        high_value = pref[i] + R

        # Add all endpoints that have entered the upper bound.
        while hi < n and pref[endpoint_order[hi]] <= high_value:
            j = endpoint_order[hi]
            add(j, 1)
            hi += 1
            active += 1

        # Remove endpoints that are below the lower bound.
        while lo < hi and pref[endpoint_order[lo]] < low_value:
            j = endpoint_order[lo]
            add(j, -1)
            lo += 1
            active -= 1

        # Only lengths greater than lcp[i] are new.
        left = i + lcp[i] + 1

        if left <= n:
            too_early = prefix_sum(left - 1)
            answer += active - too_early

    print(answer)

if __name__ == "__main__":
    solve()
```Cấu trúc mảng hậu tố trước tiên nén các giá trị kẹo để trọng điểm có thể được biểu thị một cách an toàn bằng 0 và tất cả các giá trị thực bằng số nguyên dương. Vòng lặp nhân đôi sắp xếp các dịch chuyển tuần hoàn theo cặp lớp tương đương. Bởi vì trọng điểm là duy nhất và nhỏ nhất nên việc loại bỏ hậu tố của nó ở cuối sẽ để lại mảng hậu tố thông thường của mảng ban đầu. 

Cấu trúc LCP sử dụng mảng hậu tố nghịch đảo để tìm hậu tố trước đó của từng vị trí bắt đầu. Việc Kasai sử dụng lại độ dài trận đấu trước đó làm cho tổng số so sánh ký tự trở nên tuyến tính, mặc dù các giá trị LCP riêng lẻ có thể lớn. 

Tổng tiền tố`pref[j]`đại diện cho điểm cuối của mảng ngay sau mảng con. Đây là lý do tại sao các chỉ số điểm cuối được cây Fenwick sử dụng là`1`bởi vì`N`, trong khi vị trí bắt đầu hậu tố là`0`bởi vì`N-1`. Việc trộn lẫn hai hệ tọa độ này là nguyên nhân phổ biến gây ra các lỗi riêng lẻ. 

Quá trình quét Fenwick duy trì một khoảng giá trị thay vì chỉ đơn thuần là giới hạn trên. Đối với một hậu tố bắt đầu tại`i`, một điểm cuối`j`có giá trị chính xác khi`pref[j]`nằm giữa`pref[i] + L`Và`pref[i] + R`. Vì các hậu tố được sắp xếp theo thứ tự`pref[i]`, cả hai ranh giới đều đơn điệu, do đó mỗi điểm cuối đi vào và rời khỏi cây Fenwick nhiều nhất một lần. 

các`left`biểu thức chứa ranh giới quan trọng khác. Tiền tố có độ dài bằng`lcp[i]`đã có trong hậu tố trước đó, vì vậy độ dài mới đầu tiên là`lcp[i] + 1`. Vì điểm cuối là`i + length`, điểm cuối mới đầu tiên là`i + lcp[i] + 1`. 

Số nguyên Python có độ chính xác tùy ý, do đó tổng của nó xấp xỉ`5 * 10^14`và câu trả lời cuối cùng xung quanh`1.25 * 10^11`không yêu cầu xử lý tràn đặc biệt. Trong ngôn ngữ có chiều rộng cố định, số nguyên 64 bit có dấu là đủ. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
5 5 10
1 2 3 4 5
```tổng tiền tố là`[0, 1, 3, 6, 10, 15]`. Mọi hậu tố đều khác với mọi hậu tố khác, vì vậy tất cả các giá trị LCP đều bằng 0. Bảng này thể hiện trực tiếp`[L,R]`quét. 

| tôi | lcp[i] | P[i] | Phạm vi P[j] hợp lệ | trái | hoạt động | quá sớm | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | [5, 10] | 1 | 2 | 0 | 2 | 
| 1 | 0 | 1 | [6, 11] | 2 | 2 | 0 | 2 | 
| 2 | 0 | 3 | [8, 13] | 3 | 1 | 0 | 1 | 
| 3 | 0 | 6 | [11, 16] | 4 | 1 | 0 | 1 | 
| 4 | 0 | 10 | [15, 20] | 5 | 1 | 0 | 1 | 

Năm đóng góp đó là`2 + 2 + 1 + 1 + 1 = 7`. Chúng tương ứng với`[3,4]`,`[1,2,3,4]`,`[2,3,4]`,`[4,5]`, Và`[5]`trong cách biểu diễn hậu tố-tiền tố thích hợp. Mỗi người có tổng giữa`5`Và`10`. 

Đối với mẫu 2,```
5 5 10
1 2 3 4 4
```tổng tiền tố là`[0, 1, 3, 6, 10, 14]`. Các hậu tố bắt đầu ở vị trí`3`Và`4`là`[4,4]`Và`[4]`. Theo thứ tự mảng hậu tố,`[4]`đến trước`[4,4]`, Vì thế`lcp[3] = 1`. Đây chính xác là những gì ngăn cản sự xuất hiện của`[4]`bên trong hậu tố dài hơn để được tính lần thứ hai. 

| tôi | lcp[i] | P[i] | Phạm vi P[j] hợp lệ | trái | hoạt động | quá sớm | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | [5, 10] | 1 | 2 | 0 | 2 | 
| 1 | 0 | 1 | [6, 11] | 2 | 2 | 0 | 2 | 
| 2 | 0 | 3 | [8, 13] | 3 | 1 | 0 | 1 | 
| 3 | 1 | 6 | [11, 16] | 5 | 1 | 0 | 1 | 
| 4 | 0 | 10 | [15, 20] | 5 | 0 | 0 | 0 | 

Tổng số tiền đóng góp là`2 + 2 + 1 + 1 = 6`. Giá trị lặp lại`4`minh họa tại sao các lần xuất hiện không thể được tính một cách đơn giản một cách độc lập. Trình tự kẹo đơn`[4]`đã được biểu thị bằng hậu tố bắt đầu ở vị trí cuối cùng, trong khi`[4,4]`là một chuỗi thực sự mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Việc xây dựng mảng hậu tố lấy O(N log N), cấu trúc LCP là O(N), sắp xếp tổng tiền tố lấy O(N log N) và quét Fenwick thực hiện các cập nhật và truy vấn O(N), mỗi lần trong O(log N). | 
| Không gian | O(N) | Mảng hậu tố, mảng LCP, tổng tiền tố, thứ tự sắp xếp, lớp tương đương và cây Fenwick đều sử dụng không gian tuyến tính. | 

Vì`N = 5 * 10^5`, phép liệt kê bậc hai sẽ yêu cầu khoảng`1.25 * 10^11`khoảng thời gian, trong khi giải pháp tối ưu thực hiện số lần truyền logarit trên các mảng có kích thước tuyến tính. Giới hạn bộ nhớ 1024 MB cao hơn một cách thoải mái so với tập hợp các mảng tuyến tính được sử dụng ở đây. Giới hạn 4 giây được thiết kế cho`O(N log N)`giải pháp thay vì bất kỳ cách tiếp cận nào liệt kê tất cả các mảng con. 

## Trường hợp thử nghiệm```python
# This test harness assumes the solution above has been placed in the
# same Python file and that solve() is available.

import sys
import io

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = input

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

# Provided samples
assert run("5 5 10\n1 2 3 4 5\n") == "7", "sample 1"
assert run("5 5 10\n1 2 3 4 4\n") == "6", "sample 2"

# Minimum-size input
assert run("1 1 1\n1\n") == "1", "single candy"

# Duplicate occurrences must count only once
assert run("2 2 2\n2 2\n") == "1", "duplicate sequence"

# Negative values, with inclusive lower and upper bounds
assert run("3 -1 1\n1 -1 1\n") == "5", "negative values"

# Maximum-size, all-equal input.
# Every distinct sequence is determined only by its length.
n = 500000
max_input = f"{n} 0 0\n" + " ".join(["0"] * n) + "\n"
assert run(max_input) == str(n), "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 1`|`1`| Kích thước mảng tối thiểu và một chuỗi hợp lệ duy nhất | 
|`2 2 2 / 2 2`|`1`| Lần xuất hiện trùng lặp không được tính hai lần | 
|`3 -1 1 / 1 -1 1`|`5`| Giá trị âm và ranh giới tổng bao gồm | 
|`500000 0 0 / 500000 zeros`|`500000`| Kích thước tối đa, giá trị hoàn toàn bằng nhau và hiệu suất | 

## Vỏ cạnh 

Đối với trường hợp xảy ra trùng lặp,```
2 2 2
2 2
```các hậu tố là`[2]`Và`[2,2]`. Theo thứ tự mảng hậu tố,`[2]`đến trước và`[2,2]`có LCP là`1`với nó. Hậu tố đầu tiên góp phần`[2]`, trong khi hậu tố thứ hai bị cấm đóng góp tiền tố có độ dài một và chỉ có thể đóng góp`[2,2]`. Từ`[2,2]`có tổng`4`, chỉ một`[2]`vẫn còn hiệu lực, cho`1`. 

Đối với các giá trị âm,```
3 -1 1
1 -1 1
```tổng tiền tố là`0,1,0,1`. Thuật toán không bao giờ giả định rằng tổng tiền tố tăng theo điểm cuối. Thay vào đó, nó sắp xếp tất cả các tổng tiền tố và thực hiện các truy vấn trong phạm vi giá trị. Phần mảng hậu tố xử lý việc sao chép độc lập với các tổng số, trong khi cây Fenwick xử lý thứ tự tùy ý của các tổng tiền tố. Năm chuỗi hợp lệ riêng biệt là`[1]`,`[-1]`,`[1,-1]`,`[-1,1]`, Và`[1,-1,1]`, vậy kết quả là`5`. 

Để có ranh giới chính xác,```
2 2 2
2 3
```tổng tiền tố là`0,2,5`. Đối với hậu tố bắt đầu ở vị trí đầu tiên, khoảng tổng tiền tố được yêu cầu là`[4,4]`, do đó không có điểm cuối nào được chấp nhận. Đối với hậu tố bắt đầu ở vị trí thứ hai, khoảng là`[4,4]`cũng vậy, và điểm cuối duy nhất của nó có tổng tiền tố`5`, vì vậy nó cũng nằm ngoài phạm vi. Ví dụ này thực sự không có chuỗi hợp lệ, vì vậy kết quả đầu ra là`0`. Nếu thay vào đó là mảng```
2 2 2
2 3
```phần tử đơn`2`có tổng chính xác`2`và phải được chấp nhận. Việc giải thích chính xác điều kiện điểm cuối là`P[i]+L <= P[j] <= P[i]+R`, không phải là bất đẳng thức nghiêm ngặt. 

Đối với trường hợp bằng nhau có kích thước tối đa,```
500000 0 0
0 0 0 ... 0
```mọi dãy không trống đều có tổng bằng 0. Tuy nhiên, các chuỗi khác biệt duy nhất là các chuỗi bao gồm một số 0, hai số 0, ba số 0, v.v.`500000`số không. Các giá trị LCP mảng hậu tố loại bỏ các tiền tố lặp lại, để lại chính xác một đóng góp cho mỗi độ dài có thể. Câu trả lời là do đó`500000`, trong khi thuật toán vẫn xử lý toàn bộ đầu vào trong`O(N log N)`thời gian.
