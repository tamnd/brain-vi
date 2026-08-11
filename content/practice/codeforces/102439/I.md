---
title: "CF 102439I - Phân đoạn Mod bằng nhau"
description: "Đối với mỗi đoạn liền kề [L, R], có hai cách để đánh giá nó. Bắt đầu từ L, liên tục lấy phần còn lại theo phần tử mảng tiếp theo: a[L] % a[L+1] % ... % a[R]. Bắt đầu từ R, thực hiện tương tự theo hướng ngược lại: a[R] % a[R-1] % ... % a[L]."
date: "2026-08-10T07:01:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102439
codeforces_index: "I"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Semifinal"
rating: 0
weight: 102439
solve_time_s: 386
verified: true
draft: false
---

[CF 102439I - Phân đoạn Mod bằng nhau](https://codeforces.com/problemset/problem/102439/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi đoạn liền kề`[L, R]`, có hai cách để đánh giá nó. Bắt đầu lúc`L`, liên tục lấy phần còn lại theo phần tử mảng tiếp theo:`a[L] % a[L+1] % ... % a[R]`. 

Bắt đầu lúc`R`, thực hiện tương tự theo hướng ngược lại:`a[R] % a[R-1] % ... % a[L]`. 

Chúng ta phải đếm xem có bao nhiêu phân đoạn tạo ra cùng một giá trị cuối cùng theo cả hai hướng. Một đoạn có độ dài luôn đủ điều kiện vì cả hai biểu thức chỉ chứa cùng một phần tử. 

Mảng chứa tối đa`10^5`các phần tử, trong khi mọi giá trị tối đa`3 * 10^5`. Một thuật toán bậc hai đã có khoảng`10^10`các phân đoạn, do đó, ngay cả việc xử lý thời gian không đổi cho mỗi phân đoạn cũng sẽ quá chậm. Thực tế hơn, việc đánh giá chuỗi modulo tự nó tiêu tốn thời gian tuyến tính theo chiều dài đoạn, điều này đẩy giải pháp trực tiếp thành thời gian khối. Cấu trúc hữu ích phải xuất phát từ thực tế là các phép toán modulo nhanh chóng giảm đối số của chúng. 

Có một số trường hợp khó xử lý. Vì`n = 1`, phân đoạn duy nhất phải được tính, vì vậy đầu vào```
1
7
```có câu trả lời`1`. Một giải pháp chỉ xem xét các đoạn có độ dài ít nhất là hai sẽ bỏ lỡ nó. 

Các giá trị bằng nhau có thể làm cho phép toán modulo tạo ra số không. Vì```
2
5 5
```hai người độc thân đủ điều kiện, và`[1,2]`cũng đủ điều kiện vì cả hai hướng đều đánh giá`5 % 5 = 0`, vậy câu trả lời là`3`. Xử lý các giá trị bằng nhau như thể giá trị hiện tại không thay đổi sẽ trả về sai`2`. Đây cũng là mẫu đầu tiên chính thức. 

Các điểm cuối không cần phải bằng nhau để một phân khúc đủ điều kiện và bản thân các điểm cuối bằng nhau không đảm bảo bất kỳ điều gì. Vì```
2
6 3
```hai người độc thân đủ điều kiện, nhưng`[1,2]`không: kết quả từ trái sang phải là`6 % 3 = 0`, trong khi kết quả từ phải sang trái là`3 % 6 = 3`. Câu trả lời đúng là`2`. 

Cuối cùng, câu trả lời có thể vượt quá số nguyên có dấu 32 bit. Nếu tất cả`100000`các phần tử đều bằng nhau, mỗi phần tử`n(n+1)/2 = 5000050000`phân đoạn đủ điều kiện. Các số nguyên Python xử lý việc này một cách tự nhiên, trong khi việc triển khai C++ sẽ cần`long long`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp rất đơn giản. Đối với mỗi cặp`(L,R)`, đánh giá chuỗi modulo từ bên trái và đánh giá lại chuỗi đó từ bên phải. Nếu hai kết quả khớp nhau, hãy tăng câu trả lời. 

Điều này đúng vì nó kiểm tra chính xác định nghĩa của một phân đoạn hợp lệ. Vấn đề là khối lượng công việc. có`n(n+1)/2`các đoạn và tổng chiều dài của tất cả các đoạn là`n(n+1)(n+2)/6`. 

Vì`n = 100000`, đó là về`1.67 * 10^14`lượt truy cập phần tử cho một hướng và về`3.33 * 10^14`cho cả hai hướng. Cách tiếp cận không ở gần giới hạn 1,5 giây. 

Quan sát quan trọng là chuỗi modulo không thực sự thay đổi ở mọi vị trí. Giả sử giá trị hiện tại là`x`và giá trị mảng tiếp theo là`y`. Nếu như`y > x`, sau đó`x % y = x`, nên kết quả không thay đổi chút nào. Do đó, vị trí đầu tiên có thể thay đổi kết quả là vị trí đầu tiên có giá trị lớn nhất`x`. 

Khi vị trí như vậy được tìm thấy, giá trị mới sẽ trở thành`x % y`. Nếu như`y <= x`, giá trị mới này hoàn toàn nhỏ hơn`x/2`. Để thấy điều này, nếu`y <= x/2`, số dư nhỏ hơn`y`, do đó nhỏ hơn`x/2`. Nếu như`y > x/2`, thì thương đúng bằng 1 và số dư là`x-y`, lại nhỏ hơn`x/2`. 

Do đó, mọi thay đổi thực tế đều làm giảm giá trị hiện tại ít nhất một nửa. Vì mỗi giá trị mảng nhiều nhất là`3 * 10^5`, một vị trí bắt đầu cố định chỉ có thể có`O(log a[L])`, do đó nhiều nhất là khoảng hai mươi trạng thái riêng biệt. 

Đối với điểm cuối bên trái cố định`L`, do đó chúng ta có thể biểu diễn toàn bộ chuỗi kết quả từ trái sang phải dưới dạng một số khoảng nhỏ. Mỗi khoảng nói rằng kết quả là một giá trị cố định nào đó`v`cho mọi điểm cuối phù hợp trong`[p,q]`. Cấu trúc tương tự, được áp dụng từ bên phải, đưa ra một số lượng nhỏ các khoảng cho mỗi điểm cuối bên phải cố định. 

Đây là mức giảm trung tâm. Thay vì xem xét mọi`(L,R)`riêng biệt, chúng tôi chỉ nhận được`O(n log A)`khoảng ngang và dọc, trong đó`A = max(a_i)`. Một phân đoạn hợp lệ chính xác là giao điểm của khoảng trạng thái bên trái và khoảng trạng thái bên phải có cùng giá trị. 

Nhiệm vụ còn lại là đếm hình học. Đối với một giá trị`v`, một khoảng trạng thái trái có dạng`L = fixed, R in [p,q]`, 

trong khi khoảng trạng thái phải có dạng`R = fixed, L in [p,q]`. 

Giao điểm của chúng là một cặp hợp lệ`(L,R)`chính xác khi tọa độ cố định nằm bên trong các khoảng tương ứng. 

Chúng tôi xử lý một giá trị tại một thời điểm bằng một đường quét phía trên`R`. Mỗi khoảng trạng thái bên trái trở thành một điểm hoạt động tại tọa độ`L`trong khi phạm vi điểm cuối bên phải của nó đang hoạt động. Mỗi khoảng trạng thái phải sẽ hỏi có bao nhiêu điểm hoạt động`L`bên trong khoảng của nó. Cây Fenwick xử lý các cập nhật điểm và truy vấn phạm vi đó. 

Ý tưởng tương tự xuất hiện trong công thức đường quét dự định của bài toán: các trạng thái modulo chỉ hình thành`O(n log A)`các khoảng thời gian, sau đó giao điểm của chúng có thể được tính là bài toán truy vấn ngoại tuyến hai chiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^3)`|`O(1)`| Quá chậm | 
| Tối ưu |`O(n log A log n)`|`O(n log A + A)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn lưu trữ giá trị mảng tối thiểu trong mỗi phân đoạn. Chúng ta cần thực hiện một thao tác lặp đi lặp lại: cho trước một vị trí bắt đầu`p`và giá trị hiện tại`x`, tìm vị trí đầu tiên tại hoặc sau`p`có giá trị mảng nhiều nhất`x`. Cây phân đoạn tối thiểu có thể trả lời trực tiếp điều này trong`O(log n)`bằng cách bỏ qua các nút có mức tối thiểu lớn hơn`x`. 
2. Sửa điểm cuối bên trái`L`và bắt đầu với`x = a[L]`. Điểm cuối bên phải hiện tại là`L`. Tìm vị trí đầu tiên`p > L`với`a[p] <= x`. Mọi vị trí từ điểm cuối bên phải hiện tại cho đến`p-1`để lại giá trị bằng`x`, bởi vì tất cả các giá trị của chúng đều lớn hơn`x`. Lưu trữ một khoảng ngang biểu thị các điểm cuối phù hợp. 
3. Nếu như vậy`p`tồn tại, thay thế`x`với`x % a[p]`và tiếp tục từ`p`. Nếu không có vị trí nào như vậy tồn tại thì giá trị hiện tại không thay đổi cho đến hết mảng, do đó trạng thái cho điểm cuối bên trái này kết thúc. 
4. Lặp lại điều này cho mọi điểm cuối bên trái. Mọi trạng thái được lưu trữ trong nhóm thuộc về giá trị của nó. Bản ghi trạng thái bên trái chứa khoảng thời gian điểm cuối bên phải của nó`[p,q]`và điểm cuối bên trái cố định của nó`L`. 
5. Đảo ngược mảng và thực hiện chính xác quy trình tương tự. Trạng thái trong mảng đảo ngược tương ứng với điểm cuối bên phải cố định trong mảng ban đầu. Chuyển đổi khoảng thời gian đảo ngược về tọa độ ban đầu và lưu nó dưới dạng bản ghi dọc chứa tọa độ cố định`R`và phạm vi cho phép`L`. 
6. Đối với mỗi giá trị modulo`v`, sắp xếp tất cả các bản ghi của nó theo tọa độ quét của chúng. Bản ghi ngang bắt đầu hoạt động ở mức nhỏ nhất cho phép`R`. Lưu trữ cố định của nó`L`trong cây Fenwick và vị trí hết hạn của nó trong một đống tối thiểu. 
7. Khi một bản ghi dọc có điểm cuối bên phải cố định`R`đạt đến, trước tiên hãy xóa mọi bản ghi ngang đang hoạt động có mức tối đa cho phép`R`nhỏ hơn`R`. Cây Fenwick còn lại chứa chính xác các trạng thái nằm ngang có các khoảng chứa trạng thái này`R`. 
8. Truy vấn cây Fenwick trong phạm vi cho phép của bản ghi dọc`[L1,L2]`. Mỗi điểm tìm thấy đại diện cho một cặp`(L,R)`mà cả hai hướng đều có cùng giá trị`v`, vì vậy hãy thêm số đó vào câu trả lời. 
9. Xử lý mọi nhóm giá trị và in câu trả lời tích lũy. Các phân đoạn đơn đã có sẵn trong cả hai biểu diễn trạng thái, vì vậy chúng không yêu cầu trường hợp đặc biệt riêng. 

### Tại sao nó hoạt động 

Đối với mỗi cố định`L`, các khoảng ngang được lưu trữ tạo thành một phân vùng của tất cả các điểm cuối bên phải có thể có. Trong một khoảng như vậy, giá trị modulo từ trái sang phải không đổi vì không có ước số nào gặp nhiều nhất là giá trị hiện tại. Khi khoảng kết thúc, số chia tiếp theo sẽ thay đổi giá trị và giá trị mới hoàn toàn nhỏ hơn một nửa giá trị cũ. Do đó, các khoảng được lưu trữ chứa chính xác mọi trạng thái có thể từ trái sang phải. 

Cấu trúc đảo ngược cung cấp sự phân vùng chính xác tương tự cho các trạng thái từ phải sang trái. Hãy xem xét bất kỳ phân khúc nào`[L,R]`. Nó thuộc về đúng một trạng thái nằm ngang có giá trị`v`và chính xác một trạng thái thẳng đứng với một giá trị nào đó`w`. Phân đoạn này hợp lệ chính xác khi`v = w`. Đối với một giá trị cố định, đường quét tính chính xác các giao điểm trong đó khoảng bên phải của trạng thái ngang chứa`R`và khoảng bên trái của trạng thái thẳng đứng chứa`L`. Do đó, mỗi phân đoạn hợp lệ được tính một lần và không có phân đoạn không hợp lệ nào được tính. 

Thuộc tính giảm một nửa giới hạn số lượng trạng thái được tạo từ mỗi vị trí bắt đầu. Mỗi khi giá trị hiện tại thay đổi, nó sẽ nhỏ hơn một nửa giá trị trước đó và khi về 0, nó sẽ không bao giờ thay đổi nữa vì tất cả các giá trị mảng đều dương. Điều này mang lại`O(log A)`trạng thái cho mỗi điểm cuối. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**9
SHIFT = 17
MASK = (1 << SHIFT) - 1

def build_min_tree(a):
    n = len(a)
    size = 1 << (n - 1).bit_length()
    tree = [INF] * (2 * size)
    tree[size:size + n] = a

    for i in range(size - 1, 0, -1):
        left = tree[i << 1]
        right = tree[i << 1 | 1]
        tree[i] = left if left <= right else right

    return tree, size

def first_leq(tree, size, n, start, x):
    """First index >= start with a[index] <= x, or n if none exists."""
    if start >= n:
        return n

    p = start + size

    while p:
        if p & 1:
            if tree[p] <= x:
                while p < size:
                    left = p << 1
                    if tree[left] <= x:
                        p = left
                    else:
                        p = left | 1
                pos = p - size
                return pos if pos < n else n
            p += 1
        p >>= 1

    return n

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    max_a = max(a)

    # buckets[v] contains packed horizontal and vertical records
    # having modulo-state value v.
    buckets = [[] for _ in range(max_a + 1)]

    # ------------------------------------------------------------
    # Left-to-right states.
    #
    # Record:
    #   type = 0
    #   coord = first R for which the state is active
    #   field1 = last R for which the state is active
    #   field2 = fixed L
    #
    # Packed as:
    #   coord << 35 | field1 << 17 | field2
    # ------------------------------------------------------------
    tree, size = build_min_tree(a)

    for L in range(n):
        now = a[L]
        j = L

        while True:
            p = first_leq(tree, size, n, j + 1, now)

            if p == n:
                end = n - 1
                buckets[now].append((j << 35) | (end << SHIFT) | L)
                break

            end = p - 1
            buckets[now].append((j << 35) | (end << SHIFT) | L)

            now %= a[p]
            j = p

    # ------------------------------------------------------------
    # Right-to-left states.
    #
    # Generate them as left-to-right states on the reversed array.
    #
    # Record:
    #   type = 1
    #   coord = fixed R
    #   field1 = smallest allowed L
    #   field2 = largest allowed L
    #
    # The type bit is bit 34.
    # ------------------------------------------------------------
    rev = a[::-1]
    tree, size = build_min_tree(rev)

    for s in range(n):
        now = rev[s]
        j = s
        original_r = n - 1 - s

        while True:
            p = first_leq(tree, size, n, j + 1, now)

            if p == n:
                end = n - 1
                lo = n - 1 - end
                hi = n - 1 - j

                record = (
                    (original_r << 35)
                    | (1 << 34)
                    | (lo << SHIFT)
                    | hi
                )
                buckets[now].append(record)
                break

            end = p - 1
            lo = n - 1 - end
            hi = n - 1 - j

            record = (
                (original_r << 35)
                | (1 << 34)
                | (lo << SHIFT)
                | hi
            )
            buckets[now].append(record)

            now %= rev[p]
            j = p

    # ------------------------------------------------------------
    # For each value, count intersections between horizontal
    # and vertical state rectangles.
    # ------------------------------------------------------------
    bit = [0] * (n + 1)
    tag = [0] * (n + 1)
    stamp = 0
    answer = 0

    for bucket in buckets:
        if not bucket:
            continue

        bucket.sort()
        stamp += 1

        heap = []

        for rec in bucket:
            typ = (rec >> 34) & 1
            coord = rec >> 35
            x1 = (rec >> SHIFT) & MASK
            x2 = rec & MASK

            if typ == 0:
                # Horizontal state:
                # active for R in [coord, x1],
                # fixed L = x2.
                end = x1
                point = x2

                idx = point + 1
                while idx <= n:
                    if tag[idx] != stamp:
                        tag[idx] = stamp
                        bit[idx] = 1
                    else:
                        bit[idx] += 1
                    idx += idx & -idx

                heapq.heappush(heap, (end << SHIFT) | point)

            else:
                # Vertical state:
                # fixed R = coord,
                # allowed L in [x1, x2].
                while heap and (heap[0] >> SHIFT) < coord:
                    item = heapq.heappop(heap)
                    point = item & MASK

                    idx = point + 1
                    while idx <= n:
                        bit[idx] -= 1
                        idx += idx & -idx

                # Fenwick range sum [x1, x2].
                idx = x2 + 1
                right_sum = 0
                while idx:
                    if tag[idx] == stamp:
                        right_sum += bit[idx]
                    idx -= idx & -idx

                idx = x1
                left_sum = 0
                while idx:
                    if tag[idx] == stamp:
                        left_sum += bit[idx]
                    idx -= idx & -idx

                answer += right_sum - left_sum

        bucket.clear()

    print(answer)

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ giá trị cực tiểu thay vì kết quả modulo thực tế. Thế là đủ vì vị trí tiếp theo có thể thay đổi giá trị hiện tại`x`chính xác là vị trí đầu tiên có giá trị mảng nhiều nhất`x`. các`first_leq`thường trình tìm kiếm trực tiếp hậu tố, tránh tìm kiếm nhị phân riêng biệt xung quanh truy vấn RMQ. 

Các bản ghi vòng lặp từ trái sang phải`[j,end]`bởi vì mọi điểm cuối bên phải trong khoảng đó đều thấy giá trị tích lũy giống hệt nhau. Trạng thái chỉ thay đổi tại`p`, Ở đâu`a[p] <= now`và thao tác modulo được thực hiện trước khi tiếp tục từ`p`. 

Đường chuyền ngược đáng được chú ý cẩn thận đến tọa độ. Chỉ số đảo ngược`q`tương ứng với chỉ số gốc`n-1-q`. Do đó một khoảng đảo ngược`[j,end]`trở thành khoảng điểm cuối bên trái ban đầu`[n-1-end,n-1-j]`, trong khi điểm cuối bên phải ban đầu được cố định tại`n-1-s`. 

Các số nguyên đóng gói được sử dụng thay cho các bộ dữ liệu Python vì có thể có`O(n log A)`hồ sơ. Việc đóng gói các trường làm giảm đáng kể mức tiêu thụ bộ nhớ và cũng cung cấp cho các bản ghi thứ tự sắp xếp tự nhiên theo tọa độ quét và loại bản ghi. Kiểu`0`được sử dụng cho các bản ghi ngang và loại`1`đối với các bản ghi theo chiều dọc, do đó, các bản ghi theo chiều ngang bắt đầu tại cùng một tọa độ sẽ được xử lý trước truy vấn theo chiều dọc tại tọa độ đó. 

Cây Fenwick lưu trữ một số đếm cho mỗi điểm cuối cố định bên trái đang hoạt động. Heap theo dõi khi các khoảng ngang đó ngừng hoạt động. Trước khi xử lý một truy vấn dọc tại`R`, mọi khoảng ngang kết thúc trước`R`được gỡ bỏ. Khoảng thời gian kết thúc chính xác tại`R`vẫn hoạt động, đó là điều kiện biên bao hàm bắt buộc. 

Mảng dấu thời gian ngăn chúng ta xóa toàn bộ cây Fenwick sau khi xử lý mọi giá trị. Chỉ các vị trí được chạm vào trong nhóm giá trị hiện tại mới được coi là khởi tạo. Điều này quan trọng vì có thể có tới`3 * 10^5`các giá trị modulo khác nhau có thể. 

Các số nguyên có độ chính xác tùy ý của Python cũng loại bỏ vấn đề tràn trong câu trả lời. Câu trả lời tối đa có thể là`5000050000`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên chính thức là```
2
5 5
```và câu trả lời của nó là`3`. 

Các trạng thái bên trái và trạng thái bên phải có thể được tóm tắt như sau. 

| Hướng | Điểm cuối cố định | Giá trị trạng thái | Khoảng thời gian điểm cuối thay đổi | 
| --- | --- | --- | --- | 
| Trái |`L=1`|`5`|`R=1..1`| 
| Trái |`L=1`|`0`|`R=2..2`| 
| Trái |`L=2`|`5`|`R=2..2`| 
| Đúng |`R=1`|`5`|`L=1..1`| 
| Đúng |`R=2`|`5`|`L=2..2`| 
| Đúng |`R=2`|`0`|`L=1..1`| 

Đối với giá trị`5`, quá trình quét tìm thấy`(1,1)`Và`(2,2)`. Đối với giá trị`0`, nó tìm thấy`(1,2)`. Tổng cộng là`3`. 

Ví dụ này chứng minh tại sao các giá trị liền kề bằng nhau phải tạo ra một trạng thái mới. hoạt động`5 % 5`thay đổi giá trị tích lũy từ`5`ĐẾN`0`, do đó, coi các ước bằng nhau là vô hại sẽ làm mất đoạn có độ dài hai. 

### Mẫu 2 

Mẫu thứ hai là```
3
8 3 5
```với câu trả lời`4`. 

Các trạng thái từ trái sang phải là 

| Đã sửa`L`| Giá trị trạng thái hiện tại | Điểm cuối bên phải | 
| --- | --- | --- | 
|`1`|`8`|`1..1`| 
|`1`|`2`|`2..3`| 
|`2`|`3`|`2..3`| 
|`3`|`5`|`3..3`| 

Ví dụ, bắt đầu từ`L=1`, giá trị đầu tiên nhiều nhất`8`là`3`, Vì thế`8 % 3 = 2`. Vì giá trị còn lại`5`lớn hơn`2`, kết quả vẫn giữ nguyên`2`bởi vì`R=3`. 

Các trạng thái từ phải sang trái là 

| Đã sửa`R`| Giá trị trạng thái hiện tại | Điểm cuối bên trái | 
| --- | --- | --- | 
|`1`|`8`|`1..1`| 
|`2`|`3`|`1..2`| 
|`3`|`5`|`3..3`| 
|`3`|`2`|`1..2`| 

Các giao lộ phù hợp là 

| Giá trị | Phân đoạn | 
| --- | --- | 
|`8`|`[1,1]`| 
|`3`|`[2,2]`| 
|`5`|`[3,3]`| 
|`2`|`[1,3]`| 

Vậy câu trả lời là`4`. 

Bộ ba`[1,3]`là trường hợp thú vị Từ bên trái giá trị của nó là`8 % 3 % 5 = 2`, trong khi từ bên phải là`5 % 3 % 8 = 2`. Đường quét tìm thấy nó là giao điểm của trạng thái nằm ngang`L=1, R in [2,3]`với trạng thái thẳng đứng`R=3, L in [1,2]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log A log n)`| Mỗi điểm cuối có`O(log A)`tiểu bang, mỗi tiểu bang sử dụng tìm kiếm cây phân đoạn và tất cả các bản ghi trạng thái được xử lý bằng các phép toán phân loại và Fenwick | 
| Không gian |`O(n log A + A)`| có`O(n log A)`đóng gói hồ sơ nhà nước và`O(A)`thùng giá trị | 

Đây`A = max(a_i) <= 3 * 10^5`, do đó mỗi điểm cuối chỉ tạo ra một số lượng nhỏ trạng thái. Với`n = 10^5`, hệ số logarit từ việc giảm modulo là khoảng hai mươi. Quét ngoại tuyến tránh mọi phép liệt kê bậc hai của các phân đoạn và phù hợp với mục đích dự định`O(n log n log A)`cách tiếp cận được mô tả cho vấn đề này. 

## Trường hợp thử nghiệm 

Khai thác sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`solve()`hoạt động từ giải pháp trên.```python
# Test harness for solution.py
import sys
import io

from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("2\n5 5\n") == "3", "sample 1"
assert run("3\n8 3 5\n") == "4", "sample 2"

# Minimum-size input
assert run("1\n7\n") == "1", "single element"

# All values equal.
# Every segment evaluates to 5 for a singleton and 0 for length >= 2.
# Both directions are identical for every segment.
assert run("3\n5 5 5\n") == "6", "all equal"

# Equal modulo can produce zero, and the boundaries are inclusive.
assert run("4\n4 2 3 2\n") == "6", "zero and boundary handling"

# A length-two segment can fail even when one direction becomes zero.
assert run("2\n6 3\n") == "2", "different two-element results"

# Maximum n and maximum answer.
n = 100000
inp = str(n) + "\n" + " ".join(["300000"] * n) + "\n"
assert run(inp) == "5000050000", "maximum size and 64-bit answer"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`1`| Kích thước tối thiểu và xử lý đơn lẻ | 
|`3 / 5 5 5`|`6`| Giá trị bằng nhau và kết quả modulo trở thành 0 | 
|`4 / 4 2 3 2`|`6`| Nhiều thay đổi trạng thái và ranh giới khoảng bao gồm | 
|`2 / 6 3`|`2`| Kết quả modulo trái và phải khác nhau | 
|`100000 / 300000 ... 300000`|`5000050000`| Kích thước tối đa và câu trả lời lớn | 

## Vỏ cạnh 

Trường hợp phần tử đơn`1 / 7`tạo ra một trạng thái bên trái và một trạng thái bên phải, cả hai đều có giá trị`7`và cả hai đều cố định ở vị trí`1`. Cuộc quét giao nhau với họ một lần, đưa ra câu trả lời`1`. 

Vì`2 / 5 5`, trạng thái đầu tiên bên trái là giá trị`5`chỉ tại`R=1`. Tại`R=2`, số chia bằng giá trị hiện tại, do đó trạng thái thay đổi thành 0. Việc xây dựng từ phải sang trái hoạt động đối xứng. giá trị`5`đóng góp cả hai phân khúc đơn lẻ, trong khi giá trị`0`đóng góp`[1,2]`, cho`3`. 

Vì`2 / 6 3`, trạng thái bên trái cho`L=1`thay đổi từ`6`ĐẾN`0`ở phần tử thứ hai. Trạng thái phù hợp cho`R=2`còn lại`3`trên cả hai điểm cuối bên trái có thể vì`3 % 6 = 3`. Vì các giá trị trạng thái không bao giờ khớp với đoạn có độ dài hai nên chỉ còn lại hai giao điểm đơn, tạo ra`2`. 

Vì`4 / 4 2 3 2`, đoạn`[2,4]`đặc biệt hữu ích để kiểm tra ranh giới khoảng. Kết quả bên trái của nó là`2 % 3 % 2 = 0`, trong khi kết quả đúng của nó là`2 % 3 % 2 = 0`, nên phải tính. Thuật toán biểu thị kết quả bên trái`0`như một khoảng kết thúc tại`R=4`và kết quả đúng`0`như một khoảng bắt đầu tại`L=2`. Bởi vì cả hai ranh giới đường quét đều được bao gồm nên giao điểm của chúng được giữ lại. 

Đối với đầu vào tối đa bao gồm toàn bộ`300000`, mỗi cặp`(L,R)`có kết quả như nhau ở cả hai hướng. có`100000 * 100001 / 2 = 5000050000`những cặp như vậy. Việc nén trạng thái đặc biệt hiệu quả ở đây: mỗi điểm cuối chỉ có một vài trạng thái vì giá trị bằng nhau đầu tiên ngay lập tức thay đổi giá trị tích lũy thành 0, sau đó nó vẫn bằng 0. 

Nếu bạn muốn, tôi cũng có thể cung cấp **phiên bản cô đọng mang phong cách cuộc thi hơn** của bài xã luận này, giữ nguyên bằng chứng và cách triển khai Python nhưng giảm đáng kể phần trình bày.
