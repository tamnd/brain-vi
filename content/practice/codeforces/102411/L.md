---
title: "CF 102411L - Độ dài và chu kỳ"
description: "Chúng tôi có một chuỗi w có độ dài tối đa là 200000. Chúng tôi muốn tìm chuỗi con lặp lại nhiều nhất bên trong nó, trong đó sự lặp lại được phép dừng lại giữa chừng ở bản sao tiếp theo. Giả sử một chuỗi con có chu kỳ p và độ dài L. Số mũ của nó là L/p."
date: "2026-08-11T07:53:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102411
codeforces_index: "L"
codeforces_contest_name: "ICPC 2019-2020 North-Western Russia Regional Contest"
rating: 0
weight: 102411
solve_time_s: 722
verified: true
draft: false
---

[CF 102411L - Độ dài và Khoảng thời gian](https://codeforces.com/problemset/problem/102411/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 12m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi`w`có chiều dài tối đa`200000`. Chúng tôi muốn tìm chuỗi con lặp lại nhiều nhất bên trong nó, trong đó sự lặp lại được phép dừng lại giữa chừng ở bản sao tiếp theo. 

Giả sử một chuỗi con có dấu chấm`p`và chiều dài`L`. số mũ của nó là`L / p`. Ví dụ,`abababa`có thời gian`2`, bởi vì mọi ký tự đều đồng ý với ký tự đó hai vị trí sau đó ở bất kỳ vị trí nào tồn tại cả hai vị trí đó. số mũ của nó là`7 / 2`. Nhiệm vụ là tối đa hóa tỷ lệ này trên mọi chuỗi con và mọi khoảng thời gian hợp lệ. 

Câu trả lời không nhất thiết phải là số nguyên. Một chuỗi con có thể chứa một số bản sao hoàn chỉnh của một dấu chấm và sau đó là tiền tố của một bản sao khác. Đầu ra yêu cầu là tỷ lệ tối đa, được giảm xuống mức thấp nhất. 

Chiều dài giới hạn của`200000`loại trừ bất cứ điều gì kiểm tra rõ ràng tất cả các chuỗi con và sau đó so sánh các ký tự của chúng. Đã có khoảng`n²/2`chuỗi con, do đó, ngay cả công việc liên tục trên mỗi chuỗi con cũng là quá nhiều. Một thuật toán xung quanh`O(n log n)`phù hợp với giới hạn hai giây. Giải pháp cuối cùng sử dụng một`O(n log n)`xây dựng mảng hậu tố, xây dựng LCP thời gian tuyến tính và dự kiến`O(n log n)`tổng số công việc cho các công đoàn được thành lập. 

Có một số trường hợp nhỏ rất dễ xử lý sai. Vì`a`, không có cặp hậu tố riêng biệt và không có sự lặp lại, nhưng câu trả lời vẫn là`1/1`, bởi vì một ký tự có số mũ là một. Một giải pháp khởi tạo câu trả lời bằng 0 có thể vô tình tạo ra một phân số không hợp lệ. 

Vì`abc`, không có chuỗi con nào có chu kỳ ngắn hơn độ dài của chính nó, nên câu trả lời là`1/1`. Giải pháp chỉ tìm kiếm các cặp lặp lại có thể không tìm thấy gì và vẫn phải trả về một cặp. 

Vì`aba`, toàn bộ chuỗi có dấu chấm`2`: hai ký tự đầu tiên là`ab`, và ký tự còn lại là tiền tố`a`của thời kỳ đó. số mũ của nó là`3/2`. Một giải pháp chỉ xem xét sự lặp lại hoàn toàn sẽ bỏ lỡ câu trả lời phân số này và trả về sai`1/1`. 

Vì`aaaa`, toàn bộ chuỗi có dấu chấm`1`, cho số mũ`4/1`. Trường hợp này cũng cho thấy việc triển khai không hiệu quả vì mỗi cặp hậu tố đều có một tiền tố chung dài. So sánh từng ký tự trên tất cả các cặp sẽ trở thành hình khối. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp có thể liệt kê hai quan điểm`i < j`và quan tâm`j - i`như một giai đoạn ứng cử viên. Sau đó chúng tôi so sánh`w[i:]`Và`w[j:]`từng ký tự để tìm tiền tố chung dài nhất của chúng. Nếu LCP đó có độ dài`L`, chuỗi con bắt đầu tại`i`và kết thúc sau những điều đó`L`ký tự phù hợp có độ dài`L + (j-i)`và thời kỳ`j-i`, do đó nó cho số mũ 

[ 
\frac{L+(j-i)}{j-i}. 
] 

Điều này đúng, bởi vì mỗi lần lặp lại hợp lệ với dấu chấm`p`đưa ra hai hậu tố bắt đầu`p`các vị trí cách nhau có tiền tố chung chứa mọi thứ ngoại trừ bản sao đầu tiên của dấu chấm. 

Vấn đề là chi phí. có`Theta(n²)`cặp`(i,j)`và một so sánh LCP duy nhất có thể mất`Theta(n)`thời gian. Trên một chuỗi như`aaaa...a`, hầu hết mọi so sánh đều quét một số ký tự tuyến tính. Tổng công việc là`Theta(n³)`, xung quanh`8 * 10^15`so sánh nhân vật khi`n = 200000`, vượt xa giới hạn. 

Quan sát quan trọng là chúng ta không thực sự cần phải tính toán mọi LCP một cách độc lập. Một mảng hậu tố đặt tất cả các hậu tố theo thứ tự từ điển và LCP của bất kỳ hai hậu tố nào là giá trị LCP tối thiểu trong khoảng giữa các cấp bậc của chúng. Nếu như`height[k]`là LCP của các mục nhập mảng hậu tố`k-1`Và`k`, sau đó 

[ 
LCP(i,j)=\min(chiều cao[r_i+1],\ldots,chiều cao[r_j]). 
] 

Điều này biến vấn đề thành một vấn đề kết nối ngưỡng. 

Hãy tưởng tượng việc xử lý`height`giá trị từ lớn nhất đến nhỏ nhất. Khi chúng ta đạt đến một giá trị`h`, kết nối mọi cặp hậu tố liền kề có LCP ít nhất`h`. Một thành phần được kết nối hiện chứa chính xác các hậu tố chia sẻ tiền tố có độ dài ít nhất`h`. 

Bên trong một thành phần như vậy, chúng ta muốn có hai hậu tố có vị trí bắt đầu trong chuỗi gốc càng gần nhau càng tốt. Nếu vị trí của chúng khác nhau bởi`d`, LCP của họ ít nhất là`h`, vì vậy chúng tạo ra một chuỗi con có độ dài hợp lệ`h+d`với thời gian`d`. số mũ của nó là 

[ 
\frac{h+d}{d}. 
] 

Đối với một cố định`h`, việc tối đa hóa tỷ lệ này hoàn toàn giống với việc giảm thiểu`d`. 

Vấn đề cấu trúc dữ liệu còn lại là duy trì khoảng cách tối thiểu giữa hai vị trí chuỗi gốc trong mọi thành phần DSU. Vì vị trí là số nguyên nên tập hợp có thứ tự là đủ. Khi hai thành phần được hợp nhất, chúng ta cần khoảng cách tối thiểu bên trong liên kết của chúng. Chúng tôi duy trì từng thành phần dưới dạng một nhóm ngẫu nhiên được khóa theo vị trí chuỗi ban đầu. Mỗi nút treap lưu trữ vị trí đầu tiên, vị trí cuối cùng và khoảng cách tối thiểu giữa các vị trí liên tiếp trong cây con của nó. Một liên kết treap kết hợp hai tập hợp có thứ tự rời rạc một cách hiệu quả. 

Mảng hậu tố cung cấp thông tin LCP, quá trình quét giảm dần cung cấp các thành phần ngưỡng chính xác, DSU duy trì các thành phần đó và treap duy trì cặp vị trí ban đầu gần nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n³)`|`O(n)`| Quá chậm | 
| Tối ưu | Hy vọng`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng hậu tố của`w`. Mảng hậu tố chứa mọi vị trí bắt đầu của hậu tố theo thứ tự từ điển. Chúng tôi sử dụng nhân đôi tiền tố với sắp xếp đếm, vì vậy chi phí của mỗi vòng nhân đôi`O(n)`và có`O(log n)`vòng. 
2. Xây dựng mảng LCP bằng thuật toán Kasai. Đối với mọi thứ hạng mảng hậu tố`r > 0`,`height[r]`lưu trữ LCP của hậu tố`sa[r-1]`Và`sa[r]`. Việc Kasai sử dụng lại LCP trước đó làm cho toàn bộ công trình trở nên tuyến tính. 
3. Giải thích mọi`height[r]`như một cạnh giữa các vị trí mảng hậu tố`r-1`Và`r`. Một cạnh của giá trị`h`nói rằng hai hậu tố đó có chung một tiền tố có độ dài`h`. 
4. Sắp xếp các cạnh LCP dương theo chiều cao giảm dần. Ban đầu mỗi hậu tố là thành phần DSU của chính nó. Khi xử lý một cạnh có giá trị`h`, hợp nhất hai thành phần của nó. Tất cả các hậu tố trong thành phần kết quả đều có chung một tiền tố có độ dài ít nhất`h`. 
5. Liên kết một kho đã đặt hàng với mọi thành phần DSU. Các phím là vị trí bắt đầu của các hậu tố tương ứng trong chuỗi gốc. Mỗi treap lưu trữ sự khác biệt nhỏ nhất giữa hai khóa trong thành phần. 
6. Sau khi gộp một cạnh có chiều cao`h`, cho phép`d`là khoảng cách tối thiểu được lưu trữ bởi tre kết quả. Chọn hai hậu tố có vị trí bắt đầu khác nhau một khoảng`d`. Tiền tố chung của chúng có độ dài ít nhất`h`, do đó chuỗi con bao gồm chuỗi đầu tiên`d`các ký tự theo sau tiền tố chung đó có độ dài`d+h`và thời kỳ`d`. số mũ của nó là`(d+h)/d`. 
7. So sánh phân số đó với đáp án đúng nhất hiện tại bằng phép nhân chéo. Chúng tôi so sánh`a/b`Và`c/d`BẰNG`a*d`so với`c*b`, tránh hoàn toàn số học dấu phẩy động. 
8. Bỏ qua giá trị LCP bằng 0. Họ chỉ có thể tạo ra số mũ`1`, đó đã là câu trả lời ban đầu`1/1`. 
9. Giảm tử số và mẫu số cuối cùng bằng ước số chung lớn nhất của chúng trước khi in. Mẫu số luôn dương và tất cả các giá trị trung gian vừa khít với số nguyên Python. 

### Tại sao nó hoạt động 

Xem xét hai hậu tố bất kỳ bắt đầu từ vị trí`i < j`, và để`d = j-i`. Nếu LCP của họ là`L`, thì các ký tự ở vị trí`i+k`Và`j+k`đều bình đẳng với mọi`0 <= k < L`. Vì hậu tố thứ hai bắt đầu chính xác`d`ký tự sau, chuỗi con có độ dài`d+L`bắt đầu từ`i`có thời gian`d`. Do đó, mọi cặp hậu tố đều cho một số mũ hợp lệ`(d+L)/d`. 

Bây giờ hãy xem xét bất kỳ chuỗi con hợp lệ nào có dấu chấm`p`và chiều dài`T`. đầu tiên của nó`T-p`các ký tự bằng với chuỗi con bắt đầu`p`các vị trí muộn hơn, do đó các hậu tố tại hai vị trí giai đoạn đầu tiên của nó có ít nhất LCP`T-p`. Việc đảm nhận hai vị trí đó mang lại cho ứng viên số mũ ít nhất 

[ 
\frac{p+(T-p)}p=\frac Tp. 
] 

Do đó, một câu trả lời tối ưu được thể hiện bằng một số cặp hậu tố. 

Đối với ngưỡng LCP cố định`h`, các hậu tố nằm trong cùng một thành phần DSU một cách chính xác khi mọi cạnh LCP giữa các cấp bậc mảng hậu tố của chúng ít nhất`h`. Do đó, bất kỳ cặp nào trong thành phần đó có ít nhất LCP`h`. Khoảng cách vị trí ban đầu nhỏ nhất`d`trong thành phần mang lại giá trị lớn nhất có thể`(h+d)/d`trong số các cặp được biết là có LCP ít nhất`h`. 

Khi LCP thực tế của một cặp tối ưu là`L`, quá trình quét cuối cùng đạt đến độ cao`L`. Tại thời điểm đó, cặp thuộc về một thành phần nên khoảng cách của nó được xem xét. Do đó thuật toán không thể bỏ lỡ tối ưu. Mỗi ứng cử viên mà nó tạo ra đều tương ứng với một chuỗi con định kỳ thực tế, do đó nó cũng không thể tạo ra câu trả lời không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1_000_000)

INF = 10**18

def suffix_array(s):
    n = len(s)

    # Append a unique sentinel smaller than every real character.
    a = [c - 96 for c in s] + [0]
    m = n + 1

    # Initial sorting by character using counting sort.
    alphabet = 27
    cnt = [0] * alphabet
    for x in a:
        cnt[x] += 1

    for i in range(1, alphabet):
        cnt[i] += cnt[i - 1]

    p = [0] * m
    for i in range(m - 1, -1, -1):
        x = a[i]
        cnt[x] -= 1
        p[cnt[x]] = i

    c = [0] * m
    classes = 1
    c[p[0]] = 0

    for i in range(1, m):
        if a[p[i]] != a[p[i - 1]]:
            classes += 1
        c[p[i]] = classes - 1

    k = 1

    while k < m and classes < m:
        # Shift every cyclic suffix by k.
        shifted = [0] * m
        for i in range(m):
            x = p[i] - k
            if x < 0:
                x += m
            shifted[i] = x

        # Counting-sort shifted positions by their class.
        cnt = [0] * classes
        for x in shifted:
            cnt[c[x]] += 1

        total = 0
        for i in range(classes):
            v = cnt[i]
            cnt[i] = total
            total += v

        new_p = [0] * m
        for x in shifted:
            cls = c[x]
            new_p[cnt[cls]] = x
            cnt[cls] += 1

        p = new_p

        new_c = [0] * m
        new_classes = 1
        new_c[p[0]] = 0

        for i in range(1, m):
            cur = p[i]
            prev = p[i - 1]

            cur_pair = (c[cur], c[(cur + k) % m])
            prev_pair = (c[prev], c[(prev + k) % m])

            if cur_pair != prev_pair:
                new_classes += 1

            new_c[cur] = new_classes - 1

        c = new_c
        classes = new_classes
        k <<= 1

    # The sentinel itself is first and is not a suffix of the original string.
    return p[1:]

def build_lcp(s, sa):
    n = len(s)
    rank = [0] * n

    for i, pos in enumerate(sa):
        rank[pos] = i

    height = [0] * n
    h = 0

    for i in range(n):
        r = rank[i]

        if r == 0:
            continue

        j = sa[r - 1]

        while i + h < n and j + h < n and s[i + h] == s[j + h]:
            h += 1

        height[r] = h

        if h:
            h -= 1

    return height

def solve(s):
    n = len(s)

    if n == 1:
        return "1/1"

    sa = suffix_array(s)
    height = build_lcp(s, sa)

    # Treap arrays. Node i represents original string position i.
    left = [0] * n
    right = [0] * n
    priority = [0] * n

    first = list(range(n))
    last = list(range(n))
    min_gap = [INF] * n

    # Deterministic 32-bit pseudo-random priorities.
    seed = 0x12345678
    for i in range(n):
        seed = (seed * 1664525 + 1013904223) & 0xffffffff
        priority[i] = seed

    def pull(t):
        l = left[t]
        r = right[t]

        if l:
            first[t] = first[l]
        else:
            first[t] = t

        if r:
            last[t] = last[r]
        else:
            last[t] = t

        g = INF

        if l:
            if min_gap[l] < g:
                g = min_gap[l]
            d = t - last[l]
            if d < g:
                g = d

        if r:
            if min_gap[r] < g:
                g = min_gap[r]
            d = first[r] - t
            if d < g:
                g = d

        min_gap[t] = g

    def split(t, key):
        # All keys in the first result are < key.
        # All keys in the second result are > key.
        # key itself is guaranteed not to occur in t.
        if not t:
            return 0, 0

        if key < t:
            a, b = split(left[t], key)
            left[t] = b
            pull(t)
            return a, t
        else:
            a, b = split(right[t], key)
            right[t] = a
            pull(t)
            return t, b

    def unite(a, b):
        if not a:
            return b
        if not b:
            return a

        if priority[a] < priority[b]:
            a, b = b, a

        bl, br = split(b, a)

        left[a] = unite(left[a], bl)
        right[a] = unite(right[a], br)

        pull(a)
        return a

    # DSU over suffix-array ranks.
    parent = list(range(n))
    treap_root = list(sa)

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def merge_components(a, b):
        a = find(a)
        b = find(b)

        if a == b:
            return a

        parent[b] = a
        treap_root[a] = unite(treap_root[a], treap_root[b])
        treap_root[b] = 0

        return a

    # Each positive height is an edge between ranks idx-1 and idx.
    edges = [i for i in range(1, n) if height[i] > 0]
    edges.sort(key=height.__getitem__, reverse=True)

    best_num = 1
    best_den = 1

    for idx in edges:
        h = height[idx]

        root = merge_components(idx - 1, idx)
        d = min_gap[treap_root[root]]

        # The component contains at least two suffixes here,
        # so d is finite and positive.
        num = h + d
        den = d

        if num * best_den > best_num * den:
            best_num = num
            best_den = den

    g = __import__("math").gcd(best_num, best_den)
    return f"{best_num // g}/{best_den // g}"

def main():
    s = input().strip().encode()
    sys.stdout.write(solve(s) + "\n")

if __name__ == "__main__":
    main()
```Quy trình mảng hậu tố trước tiên sẽ thêm một trọng điểm được biểu thị bằng 0. Sắp xếp các dịch chuyển theo chu kỳ của`w + sentinel`tương đương với việc sắp xếp các hậu tố của`w`, bởi vì lính canh là duy nhất và nhỏ hơn mọi ký tự thực. Tiền tố nhân đôi sau đó thay thế tiền tố có độ dài`k`theo các lớp tương đương và sắp xếp các tiền tố có độ dài`2k`sử dụng hai giá trị lớp. 

Quy trình LCP là thuật toán của Kasai.`rank[i]`đưa ra vị trí mảng hậu tố của hậu tố bắt đầu từ`i`. Khi so sánh nó với hậu tố trước đó theo thứ tự từ điển, giá trị LCP trước đó đưa ra giới hạn dưới cho so sánh mới, do đó tổng số so sánh ký tự là tuyến tính. 

Treap có một nút trên mỗi vị trí chuỗi gốc. Khóa của nút chỉ đơn giản là chỉ mục của nó, do đó không cần mảng khóa riêng.`first`,`last`, Và`min_gap`tóm tắt tập hợp có thứ tự được biểu diễn bằng cây con. Khoảng cách tối thiểu chỉ có thể nằm bên trong cây con bên trái, bên trong cây con bên phải, giữa khóa hiện tại và khóa lớn nhất ở bên trái hoặc giữa khóa hiện tại và khóa nhỏ nhất ở bên phải.`split`ngăn cách một tre bằng một phím. Chìa khóa được sử dụng bởi`unite`luôn vắng mặt trong treap khác vì các thành phần DSU chứa các vị trí hậu tố rời rạc.`unite`giữ gốc có mức độ ưu tiên ngẫu nhiên lớn hơn và tách cây khác xung quanh khóa của gốc đó. Đây là hoạt động liên kết tập hợp treap ngẫu nhiên tiêu chuẩn. 

DSU được lập chỉ mục theo thứ hạng mảng hậu tố, không phải theo vị trí chuỗi gốc. Sự khác biệt đó là cần thiết. Các cạnh được kích hoạt nằm giữa các hậu tố liền kề theo thứ tự từ điển, trong khi khoảng cách được sử dụng trong công thức số mũ là giữa các vị trí bắt đầu ban đầu của chúng. 

Việc so sánh câu trả lời sử dụng phép nhân chứ không phải phép chia. Ví dụ, để so sánh`7/3`Và`2/1`, mã kiểm tra`7*1 > 2*3`. Số nguyên Python không bị tràn, nhưng việc sử dụng số học số nguyên chính xác cũng tránh được các vấn đề về độ chính xác mà việc so sánh dấu phẩy động sẽ gây ra. 

Mã cố tình khởi tạo câu trả lời cho`1/1`. Một chuỗi không có bất kỳ mẫu ký tự lặp lại nào sẽ không có cạnh LCP dương, nhưng số mũ quan trọng của nó vẫn là một. 

## Ví dụ đã hoạt động 

### Mẫu 1:`mississippi`Sử dụng các vị trí dựa trên số 0, mảng hậu tố là`[10, 7, 4, 1, 0, 9, 8, 6, 3, 5, 2]`. 

Mảng LCP tương ứng là`[0, 1, 1, 4, 0, 0, 1, 0, 2, 1, 3]`. 

Thuật toán xử lý các độ cao dương theo thứ tự giảm dần. 

| Chỉ số cạnh | Chiều cao`h`| Vị trí hậu tố mới được hợp nhất | Khoảng cách tối thiểu`d`| Ứng viên | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 4 |`{4, 1}`| 3 |`7/3`|`7/3`| 
| 10 | 3 |`{5, 2}`| 3 |`6/3 = 2`|`7/3`| 
| 8 | 2 |`{6, 3}`| 3 |`5/3`|`7/3`| 
| 1 | 1 |`{10, 7}`| 3 |`4/3`|`7/3`| 
| 2 | 1 |`{10, 7, 4, 1}`| 3 |`4/3`|`7/3`| 
| 6 | 1 |`{9, 8}`| 1 |`2/1`|`7/3`| 
| 9 | 1 |`{6, 3, 5}`| 1 |`2/1`|`7/3`| 

Ở độ cao`4`, hậu tố bắt đầu tại vị trí`4`Và`1`chia sẻ tiền tố`issi`. Khoảng cách của họ là`3`, do đó chuỗi con bắt đầu tại vị trí`1`có chiều dài`4+3=7`và thời kỳ`3`. Đó là`ississi`, cho số mũ`7/3`. 

Việc hợp nhất sau này chứa các vị trí`9`Và`8`tìm chuỗi con lặp lại`pp`, cho số mũ`2`. Đó là một ứng cử viên hợp lệ, nhưng nó không đánh bại`7/3`. 

### Mẫu 2:`abab`Các hậu tố được sắp xếp như sau`ab`,`abab`,`b`,`bab`, 

vì vậy mảng hậu tố là`[2, 0, 3, 1]`. Mảng LCP là`[0, 2, 0, 1]`. 

| Chỉ số cạnh | Chiều cao`h`| Vị trí mới được sáp nhập | Khoảng cách tối thiểu`d`| Ứng viên | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 |`{2, 0}`| 2 |`4/2 = 2`|`2/1`| 
| 3 | 1 |`{3, 1}`| 2 |`3/2`|`2/1`| 

Việc hợp nhất đầu tiên sử dụng các vị trí`2`Và`0`. Hậu tố của họ chia sẻ`ab`, vậy khoảng cách là`2`và chuỗi con kết quả có độ dài`4`. Điều này mang lại hình vuông chính xác`abab`, số mũ của nó là`2`. 

Sự hợp nhất thứ hai thể hiện sự lặp lại phân đoạn`bab`, có chu kì`2`và số mũ`3/2`. Nó nhỏ hơn hình vuông đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Hy vọng`O(n log n)`| Chi phí mảng hậu tố nhân đôi tiền tố`O(n log n)`, chi phí Kasai`O(n)`, sắp xếp chi phí cạnh LCP`O(n log n)`, và tập hợp treap được mong đợi`O(n log n)`tổng cộng | 
| Không gian |`O(n)`| Mảng hậu tố, dữ liệu LCP, mảng DSU và một nút treap cho mỗi vị trí chuỗi | 

Đầu vào chứa tối đa`200000`các ký tự, vì vậy phép liệt kê bậc hai đã quá lớn và việc so sánh bậc ba là hoàn toàn không khả thi. Giải pháp này chỉ thực hiện logarit nhiều lần truyền đầy đủ trong quá trình xây dựng mảng hậu tố và giữ cho mọi cấu trúc phụ trợ tuyến tính theo chiều dài chuỗi. Treap ngẫu nhiên tránh cần đến một thư viện được sắp xếp theo thứ tự không chuẩn trong Python. 

## Trường hợp thử nghiệm```
# Assume the submitted solution is saved as solution.py
from solution import solve

def run(inp: str) -> str:
    return solve(inp.strip().encode())

# Provided samples
assert run("mississippi") == "7/3", "sample 1"
assert run("abab") == "2/1", "sample 2"

# Minimum-size input
assert run("a") == "1/1", "single character"

# No repetition at all
assert run("abc") == "1/1", "all characters different"

# Fractional exponent
assert run("aba") == "3/2", "fractional repetition"

# Small repeated block, catches period and boundary handling
assert run("aab") == "2/1", "repeated pair at the beginning"

# All equal values
assert run("aaaaa") == "5/1", "all equal characters"

# Maximum-size input
assert run("a" * 200000) == "200000/1", "maximum-size all-equal string"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`1/1`| Đầu vào tối thiểu và trường hợp không có cạnh | 
|`abc`|`1/1`| Chuỗi không có mẫu lặp lại | 
|`aba`|`3/2`| Số mũ phân số và một phần giai đoạn cuối cùng | 
|`aab`|`2/1`| Sự lặp lại kết thúc chính xác tại một ranh giới | 
|`aaaaa`|`5/1`| Số mũ dài nhất có thể và nhiều giá trị LCP bằng nhau | 
|`a * 200000`|`200000/1`| Kích thước đầu vào tối đa và câu trả lời số nguyên lớn | 

## Vỏ cạnh 

Đối với đầu vào một ký tự`a`, mảng hậu tố chỉ chứa một hậu tố và mảng LCP chỉ chứa 0. Không có sự hợp nhất DSU nào được thực hiện. Câu trả lời ban đầu vẫn`1/1`, chính xác là số mũ của chuỗi con không trống duy nhất. 

Vì`abc`, mọi giá trị LCP dương đều không có. Không có cặp hậu tố nào có ký tự đầu tiên chung, vì vậy không có chuỗi con nào có thể có dấu chấm nhỏ hơn độ dài của chính nó. Lại là ban đầu`1/1`được bảo tồn. Một giải pháp giả định ít nhất một cạnh LCP dương sẽ thất bại ở đây. 

Vì`aba`, các hậu tố bắt đầu tại vị trí`0`Và`2`có LCP`1`. Khoảng cách của họ là`2`, do đó thuật toán cuối cùng sẽ kích hoạt ngưỡng LCP tương ứng và thu được 

[ 
\frac{1+2}{2}=\frac32. 
] 

Chuỗi con tương ứng là`aba`. Đây chính xác là lý do tại sao thuật toán phải sử dụng`h+d`, thay vì chỉ`2d`hoặc chỉ hoàn thành các khối lặp đi lặp lại. 

Vì`aab`, hậu tố bắt đầu tại vị trí`0`Và`1`có LCP`1`. Khoảng cách của họ là`1`, cho 

[ 
\frac{1+1}{1}=2. 
] 

Chuỗi con là`aa`. Điều này phát hiện các hoạt động triển khai vô tình yêu cầu chuỗi con lặp lại vượt ra ngoài hậu tố hiện tại hoặc xử lý sai vị trí LCP cuối cùng. 

Vì`aaaaa`, mỗi cặp hậu tố đều có một tiền tố chung dài. Ở ngưỡng LCP hữu ích lớn nhất, hai vị trí xuất phát gần nhất có khoảng cách`1`. Việc hợp nhất cuối cùng đạt đến một thành phần chứa tất cả năm vị trí và ứng cử viên là 

[ 
\frac{4+1}{1}=5. 
] 

Như vậy câu trả lời là`5/1`. Trường hợp này cũng chứng minh tại sao cấu trúc có thứ tự phải duy trì khoảng cách vị trí ban đầu tối thiểu một cách hiệu quả, bởi vì các thành phần mảng hậu tố có thể trở nên rất lớn. 

Đối với đầu vào tối đa bao gồm`200000`bản sao của`a`, lý do tương tự cho dấu chấm`1`và chiều dài`200000`, vậy câu trả lời là`200000/1`. Tử số được xử lý trực tiếp dưới dạng số nguyên và không có vấn đề về phép tính dấu phẩy động hoặc tràn chiều rộng cố định.
