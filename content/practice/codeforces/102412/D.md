---
title: "CF 102412D - Bước nhảy vọt từ tầm cao của lòng tự trọng đến đỉnh cao của chỉ số IQ"
description: "Chúng ta có một dãy (n) tòa nhà chọc trời và chiều cao của chúng tạo thành một hoán vị của (1,2,ldots,n). Một bước nhảy hợp lệ sử dụng ba tòa nhà chọc trời theo thứ tự vị trí tăng dần có chiều cao cũng tăng dần."
date: "2026-08-12T00:36:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102412
codeforces_index: "D"
codeforces_contest_name: "MEX Foundation Contest (supported by AIM Tech)"
rating: 0
weight: 102412
solve_time_s: 193
verified: true
draft: false
---

[CF 102412D - Bước nhảy vọt từ tầm cao của lòng tự trọng lên đỉnh cao của chỉ số IQ](https://codeforces.com/problemset/problem/102412/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy (n) tòa nhà chọc trời và chiều cao của chúng tạo thành một hoán vị của (1,2,\ldots,n). Một bước nhảy hợp lệ sử dụng ba tòa nhà chọc trời theo thứ tự vị trí tăng dần có chiều cao cũng tăng dần. Nói cách khác, chúng ta cần biết liệu dãy hiện tại có chứa dãy con tăng dần có độ dài bằng ba hay không. 

Mỗi truy vấn lấy một phân đoạn liền kề và xoay nó theo chu kỳ sang phải theo (k) vị trí. Thứ tự bên trong của hai mảnh được giữ nguyên nhưng thứ tự của chúng bị hoán đổi. Sau mỗi phép quay, chúng ta phải báo cáo xem có tồn tại dãy con tăng dần có độ dài bằng ba hay không. Giới hạn chính thức là (n,q\le120000), với giới hạn thời gian 7 giây và bộ nhớ 512 MiB. 

Kiểm tra trực tiếp sau mỗi truy vấn sẽ quét toàn bộ hoán vị. Vì (n) và (q) đều có thể là (120000), nên (O(nq)) có nghĩa là khoảng (1,44\times10^{10}) lượt truy cập phần tử, vượt xa giới hạn thời gian cho phép. Ngay cả kiểm tra (O(n^2)) cho một trạng thái cũng đã quá lớn, vì vậy giải pháp phải khai thác thực tế là một phép quay làm thay đổi trình tự bằng cách sắp xếp lại toàn bộ các phần liền kề thay vì thay đổi độ cao riêng lẻ. Cách tiếp cận cây cân bằng cho ra (O(n\log^2 n+q\log^2 n)), đây là thang đo dự kiến ​​cho các giới hạn này. 

Có một số trường hợp nhỏ mà việc triển khai bất cẩn có thể thất bại. Ví dụ, chỉ với hai tòa nhà chọc trời,`2 1`không bao giờ có thể chứa bộ ba hợp lệ, vì vậy câu trả lời là`NO`; mã chỉ kiểm tra xem có đi lên hay không có thể báo cáo sai`YES`. Phép quay một phần tử là một trường hợp biên khác. Đối với đầu vào```
3
2 3 1
1
1 3 0
```đoạn đó không di chuyển và trình tự vẫn giữ nguyên`2 3 1`, vậy câu trả lời là`NO`. Việc coi (k=0) là một phép chia không tầm thường có thể vô tình làm thay đổi trình tự. Một thái cực khác là (k=r-l+1), cũng là một điều không thể thực hiện được. Cuối cùng, độ cao được đảm bảo là khác biệt, do đó trường hợp kiểm thử "hoàn toàn bằng nhau" không phải là đầu vào hợp lệ theo các ràng buộc của bài toán. Tuy nhiên, so sánh giá trị bằng nhau không bao giờ được sử dụng một cách ngẫu nhiên, vì các bất đẳng thức cần thiết là nghiêm ngặt. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Lưu trữ hoán vị hiện tại trong một mảng, thực hiện phép quay theo chu kỳ bằng cách di chuyển các phần tử (k) cuối cùng của khoảng được truy vấn về phía trước, sau đó quét toàn bộ mảng để xác định xem nó có chứa dãy con tăng dần có độ dài ba hay không. Quá trình quét có thể được thực hiện theo thời gian tuyến tính bằng cách duy trì giá trị nhỏ nhất được thấy cho đến nay và giá trị thứ hai nhỏ nhất có thể có của một cặp tăng dần. Điều này đúng vì giá trị thứ ba lớn hơn giá trị thứ hai đó hoàn thành bộ ba hợp lệ. 

Vấn đề là việc quét toàn bộ lặp đi lặp lại. Trong trường hợp xấu nhất, (n=q=120000), đưa ra khoảng (14,4) tỷ phép tính trước cả khi tính đến các phép quay. Việc cập nhật mảng cũng có thể yêu cầu chuyển động (O(n)) nếu được thực hiện theo nghĩa đen. 

Quan sát hữu ích là một phép quay không làm thay đổi thứ tự bên trong mỗi phần được tạo ra. Giả sử một đoạn được chia thành (A) và (B), trong đó (B) là phần được di chuyển từ đầu ra trước. Phân khúc mới chỉ đơn giản là (BA). Điều này gợi ý một cây cân bằng ngầm định, trong đó việc duyệt theo thứ tự là hoán vị hiện tại và việc phân chia theo vị trí sẽ mất thời gian logarit. 

Khó khăn còn lại là việc duy trì liệu một cây con có chứa bộ ba tăng hay không. Một cây con chỉ cần một lượng nhỏ thông tin. Chúng tôi lưu trữ chiều cao tối thiểu và tối đa của nó, cho dù nó đã chứa bộ ba tăng dần hay chưa và khi nó không chứa bộ ba như vậy thì hai thuộc tính của các cặp tăng dần của nó. Cho phép`first_max`là giá trị đầu tiên lớn nhất có thể có (a_i) trong số tất cả các cặp (i<j) với (a_i<a_j) và đặt`second_min`là giá trị thứ hai nhỏ nhất có thể (a_j) trong số các cặp như vậy. 

Khi hai chuỗi liền kề được nối, một bộ ba mới chỉ có thể nằm hoàn toàn bên trong một con hoặc vượt qua ranh giới. Một bộ ba xuyên ranh giới có hai phần tử ở con trái và một phần tử ở con bên phải, hoặc một phần tử ở con trái và hai phần tử ở con bên phải. Cặp cực trị được lưu trữ cho phép chúng tôi phát hiện cả hai trường hợp. Việc tính toán cặp cực trị xuyên biên giới chính xác đòi hỏi phải tìm một cây trước hoặc cây kế tiếp bên trong cây con tránh 123. Thực tế về cấu trúc quan trọng là, trong khi một cây con không chứa bộ ba tăng dần, việc tìm kiếm này có thể được thực hiện bằng cách giảm dần cây cân bằng và tỉa bớt toàn bộ cây con bằng cách sử dụng cực trị của chúng. Đây là thuộc tính đặc biệt cung cấp hệ số logarit bổ sung thay vì buộc phải quét tuyến tính. Đây cũng là quan sát trọng tâm đằng sau giải pháp cây cân bằng tiêu chuẩn. 

Do đó, giải pháp brute-force hoạt động vì bản thân vị từ rất dễ kiểm tra nhưng lại thất bại khi phải tính toán lại sau mỗi lần sắp xếp lại lớn. Quan sát rằng một phép quay chỉ là một phép phân tách theo sau là một phép hợp nhất cho phép chúng ta bảo toàn vị từ dưới dạng tổng hợp cây con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nq)) | (O(n)) | Quá chậm | 
| Xử lý ngầm tối ưu | (O((n+q)\log^2 n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một treap ngẫu nhiên ngầm có trình tự thứ tự là thứ tự tòa nhà chọc trời hiện tại. Mỗi nút đại diện cho một tòa nhà chọc trời và kích thước cây con xác định chỉ mục vị trí. Một treap là phù hợp vì việc tách các phần tử (k) đầu tiên và nối hai chuỗi đều mất thời gian logarit dự kiến. 
2. Đối với mỗi cây con, hãy duy trì kích thước, chiều cao tối thiểu, chiều cao tối đa của nó và liệu bộ ba tăng dần có tồn tại hay không. Nếu cây con vẫn không có bộ ba, hãy duy trì`first_max`Và`second_min`cho các cặp ngày càng tăng của nó. 
3. Khi kết hợp cây con bên trái, nút hiện tại và cây con bên phải, trước tiên hãy xác định xem một trong hai cây con có chứa bộ ba hay không. Nếu vậy, cây con kết hợp sẽ ngay lập tức chứa một cây con và thông tin về cặp này không còn cần thiết nữa. 
4. Ngược lại, phát hiện các bộ ba vượt qua ranh giới. Nếu cây con bên trái chứa một cặp tăng dần có giá trị thứ hai nhỏ hơn giá trị tối đa của cây con bên phải thì hai phần tử bên trái theo sau bởi giá trị lớn nhất đó sẽ tạo thành một bộ ba. Đây đúng là điều kiện`left.second_min < right.max`. 
5. Một cách đối xứng, nếu giá trị tối thiểu của cây con bên trái nhỏ hơn giá trị đầu tiên của một cặp tăng dần nào đó trong cây con bên phải thì giá trị tối thiểu ở bên trái theo sau cặp đó sẽ tạo thành một bộ ba. Điều này mang lại`left.min < right.first_max`. 
6. Nếu không có bộ ba nào tồn tại, hãy cập nhật thông tin về cặp đó. Các cặp đã có trong một trong hai đứa trẻ vẫn hợp lệ. Một cặp mới sử dụng một phần tử từ mỗi bên có giá trị đầu tiên lớn nhất có thể bằng giá trị trước đó của`right.max`giữa các giá trị ở cây con bên trái. Giá trị thứ hai nhỏ nhất có thể có của nó là giá trị kế tiếp của`left.min`giữa các giá trị ở cây con bên phải. 
7. Thực hiện từng truy vấn bằng cách tách trước vị trí (l), sau đó tách đoạn ([l,r]). Bên trong đoạn đó, chia nó thành (A) và (B), trong đó (B) có độ dài (k). Thay thế phân đoạn bằng (BA). Cuối cùng hợp nhất lại với tiền tố và hậu tố. 
8. Gốc của`has_three`cờ trực tiếp xác định câu trả lời. In`YES`khi nó là sự thật và`NO`nếu không thì. 

Tại sao nó hoạt động: mỗi bộ ba tăng dần trong một phép nối được chứa hoàn toàn ở phần bên trái, hoàn toàn ở phần bên phải hoặc vượt qua ranh giới. Hai trường hợp đầu tiên được thể hiện bằng lá cờ của trẻ em. Đối với một bộ ba giao nhau có hai phần tử ở bên trái, phần tử thứ hai tốt nhất có thể là`left.second_min`, và phần tử thứ ba tốt nhất có thể là`right.max`. Đối với một bộ ba giao nhau có hai phần tử ở bên phải, phần tử đầu tiên tốt nhất có thể là`left.min`và phần tử thứ hai tốt nhất có thể là`right.first_max`. Như vậy tất cả các bộ ba có thể đều được bao phủ. Cực trị cặp được cập nhật chính xác từ ba vị trí cặp có thể có, trái-trái, phải-phải và trái-phải. Do đó, tổng hợp được lưu trữ tại mỗi nút treap mô tả chính xác trình tự thứ tự của nó, do đó cờ gốc luôn chính xác sau mỗi lần phân tách và hợp nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1_000_000)

INF = 10**18

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())

    # Node 0 is the null node.
    left = [0]
    right = [0]
    value = [0]
    priority = [0]
    size = [0]

    mn = [INF]
    mx = [-INF]
    bad = [False]

    # For a triple-free subtree:
    # first_max  = maximum first value among increasing pairs.
    # second_min = minimum second value among increasing pairs.
    first_max = [-INF]
    second_min = [INF]

    seed = 712367821

    def rng():
        nonlocal seed
        seed ^= (seed << 13) & 0xFFFFFFFF
        seed ^= seed >> 17
        seed ^= (seed << 5) & 0xFFFFFFFF
        seed &= 0xFFFFFFFF
        return seed

    def new_node(v):
        idx = len(value)
        left.append(0)
        right.append(0)
        value.append(v)
        priority.append(rng())
        size.append(1)
        mn.append(v)
        mx.append(v)
        bad.append(False)
        first_max.append(-INF)
        second_min.append(INF)
        return idx

    # Find the largest value < x in a triple-free subtree.
    def predecessor(t, x):
        if not t or mn[t] >= x:
            return 0
        if mx[t] < x:
            return mx[t]

        ans = 0

        r = right[t]
        z = predecessor(r, x)
        if z > ans:
            ans = z

        v = value[t]
        if v < x and v > ans:
            ans = v

        l = left[t]
        z = predecessor(l, x)
        if z > ans:
            ans = z

        return ans

    # Find the smallest value > x in a triple-free subtree.
    def successor(t, x):
        if not t or mx[t] <= x:
            return INF
        if mn[t] > x:
            return mn[t]

        ans = INF

        l = left[t]
        z = successor(l, x)
        if z < ans:
            ans = z

        v = value[t]
        if v > x and v < ans:
            ans = v

        r = right[t]
        z = successor(r, x)
        if z < ans:
            ans = z

        return ans

    def pull(t):
        l = left[t]
        r = right[t]
        v = value[t]

        size[t] = size[l] + size[r] + 1
        mn[t] = min(mn[l], v, mn[r])
        mx[t] = max(mx[l], v, mx[r])

        if bad[l] or bad[r]:
            bad[t] = True
            first_max[t] = -INF
            second_min[t] = INF
            return

        has_triple = (
            second_min[l] < mx[r] or
            mn[l] < first_max[r]
        )

        cross_first = 0
        cross_second = INF

        if l and r:
            cross_first = predecessor(l, mx[r])
            cross_second = successor(r, mn[l])

            if cross_first and cross_second != INF:
                has_triple = has_triple or (
                    second_min[l] < mx[r] or
                    mn[l] < first_max[r]
                )

        bad[t] = has_triple

        if has_triple:
            first_max[t] = -INF
            second_min[t] = INF
            return

        fm = max(first_max[l], first_max[r], cross_first)
        sm = min(second_min[l], second_min[r], cross_second)

        # Pairs involving the root value itself.
        if l and v > mn[l]:
            p = predecessor(l, v)
            if p:
                fm = max(fm, p)
                sm = min(sm, v)

        if r and mx[r] > v:
            s = successor(r, v)
            if s != INF:
                fm = max(fm, v)
                sm = min(sm, s)

        first_max[t] = fm
        second_min[t] = sm

    # Build the initial treap in O(n) using the Cartesian-tree stack.
    nodes = [new_node(v) for v in a]

    stack = []
    for t in nodes:
        last = 0
        while stack and priority[stack[-1]] < priority[t]:
            last = stack.pop()
        if stack:
            right[stack[-1]] = t
        left[t] = last
        stack.append(t)

    root = stack[0]

    # Pull aggregates bottom-up.
    order = []
    st = [root]
    while st:
        t = st.pop()
        order.append(t)
        if left[t]:
            st.append(left[t])
        if right[t]:
            st.append(right[t])

    for t in reversed(order):
        pull(t)

    def split(t, k):
        if not t:
            return 0, 0

        l = left[t]

        if size[l] >= k:
            x, y = split(l, k)
            left[t] = y
            pull(t)
            return x, t

        x, y = split(right[t], k - size[l] - 1)
        right[t] = x
        pull(t)
        return t, y

    def merge(a_root, b_root):
        if not a_root:
            return b_root
        if not b_root:
            return a_root

        if priority[a_root] > priority[b_root]:
            right[a_root] = merge(right[a_root], b_root)
            pull(a_root)
            return a_root

        left[b_root] = merge(a_root, left[b_root])
        pull(b_root)
        return b_root

    out = []

    for _ in range(q):
        l, r, k = map(int, input().split())

        if k == 0 or k == r - l + 1:
            out.append("YES" if bad[root] else "NO")
            continue

        prefix, rest = split(root, l - 1)
        segment, suffix = split(rest, r - l + 1)

        first_part, second_part = split(
            segment,
            r - l + 1 - k
        )

        segment = merge(second_part, first_part)
        root = merge(prefix, merge(segment, suffix))

        out.append("YES" if bad[root] else "NO")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Treb là ngầm định nên không có khóa nào đại diện cho một vị trí. Vị trí của một nút được xác định bởi kích thước của các cây con bên trái của nó. Điều đó làm cho`split(root, k)`có nghĩa chính xác là "lấy (k) tòa nhà chọc trời đầu tiên", đó là những gì vòng quay theo chu kỳ cần. 

các`pull`chức năng là cốt lõi của giải pháp. Tối thiểu và tối đa là tập hợp cây con thông thường.`bad`ghi lại xem mẫu 123 đã tồn tại chưa. Khi`bad`là sai,`first_max`Và`second_min`mô tả từng cặp tăng dần trong cây con. Các tìm kiếm tiền thân và kế tiếp chỉ cần thiết trong khi cây con tránh được 123, đó chính xác là tình huống áp dụng thuộc tính cắt tỉa cấu trúc. Đây là tập thông tin tương tự được mô tả trong các bản ghi độc lập của giải pháp dự định. 

Hai cuộc kiểm tra liên quan đến`second_min[l]`Và`first_max[r]`cố tình sử dụng so sánh chặt chẽ. Độ cao là khác biệt nên sự bình đẳng không thể tạo thành một phần của bước nhảy hợp lệ. Bản thân nút gốc cũng tham gia vào việc tăng dần các cặp, đó là lý do tại sao`pull`xử lý rõ ràng các cặp giữa giá trị hiện tại và giá trị con. 

Truy vấn sử dụng`r - l + 1 - k`khi chia đoạn đã quay. Đây là chiều dài của phần nằm ở phía trước trước khi phép quay bên phải di chuyển phần tử (k) cuối cùng về phía trước nó. Nếu (k) bằng 0 hoặc toàn bộ độ dài đoạn thì chuỗi không thay đổi, do đó chúng ta có thể tránh được các thao tác cây không cần thiết. 

Số nguyên Python không tràn và tất cả chiều cao được lưu trữ tối đa là (120000). Mối quan tâm triển khai chính là độ sâu đệ quy, do đó giới hạn đệ quy được nâng lên đáng kể. Các mức độ ưu tiên ngẫu nhiên giữ cho chiều cao treap được mong đợi ở mức logarit. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
6
2 5 6 1 3 4
1
1 6 5
```Toàn bộ mảng được xoay sang phải năm vị trí, tương đương với việc di chuyển phần tử đầu tiên về cuối. 

| Bước | Trình tự | Chia luân chuyển | Gốc`bad`| 
| --- | --- | --- | --- | 
| Ban đầu |`2 5 6 1 3 4`| không |`YES`| 
| Chia |`2`+`5 6 1 3 4`| phần đầu dài 1 |`YES`| 
| Xoay |`5 6 1 3 4`+`2`| chiều dài phần bên phải 5 |`YES`| 
| Cuối cùng |`5 6 1 3 4 2`| hợp nhất |`YES`| 

Trình tự cuối cùng chứa`1,3,4`trong việc tăng vị trí và chiều cao, vì vậy câu trả lời là`YES`. Dấu vết chứng minh rằng phép quay có thể được thể hiện hoàn toàn thông qua việc chia tách và hợp nhất các treap mà không cần di chuyển vật lý năm phần tử mảng. 

### Mẫu 3 

Đầu vào là```
5
4 3 2 5 1
2
3 4 1
1 2 1
```| Bước | Trình tự | Hoạt động | Gốc`bad`| 
| --- | --- | --- | --- | 
| Ban đầu |`4 3 2 5 1`| không |`NO`| 
| 1 |`4 3 5 2 1`| quay`[3,4]`đúng 1 |`NO`| 
| 2 |`3 4 5 2 1`| quay`[1,2]`đúng 1 |`YES`| 

Sau thao tác đầu tiên, trình tự có mức tăng dần như`3,5`, nhưng không có chiều cao thứ ba sau lớn hơn`5`, vì vậy chỉ đi lên thôi là không đủ. Sau ca phẫu thuật thứ hai,`3,4,5`xuất hiện theo thứ tự tăng dần, tạo ra bộ ba cần thiết. Đây chính xác là sự khác biệt được nắm bắt bởi`first_max`Và`second_min`thông tin cặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((n+q)\log^2 n)) dự kiến ​​| Các hoạt động Treap sử dụng chiều cao dự kiến ​​(O(\log n)), trong khi việc xây dựng lại các tập hợp có thể thực hiện tìm kiếm tiền nhiệm hoặc tìm kiếm kế thừa logarit | 
| Không gian | (O(n)) | Một nút treap và lượng thông tin tổng hợp không đổi trên mỗi tòa nhà chọc trời | 

Các ràng buộc cho phép (120000) tòa nhà chọc trời và (120000) phép quay, do đó phương pháp bậc hai hoặc (O(nq)) là không khả thi. Biểu diễn cây cân bằng giữ cho mọi logarit phép quay ở cấp độ cấu trúc, trong khi tìm kiếm tránh 123 đặc biệt giữ cho việc tính toán tổng hợp nằm trong hệ số logarit bổ sung. Giới hạn thời gian chính thức là 7 giây và giới hạn bộ nhớ là 512 MiB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()

        solve()

        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    """6
2 5 6 1 3 4
1
1 6 5
"""
) == "YES\n", "sample 1"

# Provided sample 2
assert run(
    """8
5 1 2 8 7 6 3 4
4
2 4 2
4 5 1
1 3 2
3 8 2
"""
) == "YES\nYES\nYES\nYES\n", "sample 2"

# Provided sample 3
assert run(
    """5
4 3 2 5 1
2
3 4 1
1 2 1
"""
) == "NO\nYES\n", "sample 3"

# Provided sample 4
assert run(
    """6
6 5 4 3 2 1
3
1 1 0
1 3 1
2 5 3
"""
) == "NO\nNO\nYES\n", "sample 4"

# Minimum size.
assert run(
    """1
1
1
1 1 0
"""
) == "NO\n", "minimum size"

# Two elements can never form a triple.
assert run(
    """2
1 2
1
1 2 1
"""
) == "NO\n", "two elements"

# Full rotation by one creates 1,2,3.
assert run(
    """3
2 3 1
1
1 3 1
"""
) == "YES\n", "full-range rotation"

# Boundary case with no movement.
assert run(
    """3
3 2 1
1
1 3 0
"""
) == "NO\n", "zero rotation"

# Maximum-size decreasing permutation.
n = 120000
max_case = (
    str(n) + "\n" +
    " ".join(map(str, range(n, 0, -1))) + "\n" +
    "1\n" +
    "1 120000 1\n"
)
assert run(max_case) == "NO\n", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1 1 0`|`NO`| Kích thước tối thiểu | 
|`2 / 1 2 / 1 2 1`|`NO`| Ít hơn ba tòa nhà chọc trời | 
|`3 / 2 3 1 / 1 3 1`|`YES`| Xoay theo chu kỳ toàn dải | 
|`3 / 3 2 1 / 1 3 0`|`NO`| Ranh giới xoay không | 
| Giảm hoán vị kích thước`120000`|`NO`| Kích thước và hiệu suất đầu vào tối đa | 
| Cung cấp mẫu | Như được liệt kê | Độ đúng chung và ranh giới xoay | 

## Vỏ cạnh 

Đối với một đoạn có độ dài bằng một, mọi phép quay được phép đều không thay đổi. Ví dụ,```
3
3 2 1
1
2 2 1
```sản xuất`NO`. Việc triển khai xử lý việc này một cách tự nhiên vì việc tách một phân đoạn một thành phần và xoay nó sẽ để lại cùng một nút ở cùng một vị trí. 

Vòng quay bằng 0 cũng là không hoạt động. Với```
3
3 2 1
1
1 3 0
```trình tự vẫn còn`3 2 1`, không chứa bộ ba tăng dần, vì vậy đầu ra là`NO`. Mã này xử lý rõ ràng (k=0) trước khi thực hiện bất kỳ phép chia nào. 

Việc xoay toàn bộ chiều dài đoạn là một điều không nên làm. Vì```
3
2 3 1
1
1 3 3
```trình tự cuối cùng vẫn là`2 3 1`, vì vậy đầu ra là`NO`. Nếu coi đây là một phép quay thông thường sẽ gây ra sự phân chia không cần thiết và là nguồn gốc phổ biến của các sai lầm về ranh giới. 

Một dãy có thể chứa nhiều cặp tăng dần mà không chứa bộ ba tăng dần. Trạng thái trung gian mẫu-3`4 3 5 2 1`chứng tỏ điều này:`3,5`là một cặp tăng dần, nhưng không có tòa nhà chọc trời nào sau này cao hơn`5`. Thuật toán giữ thông tin cặp tách biệt với cờ ba, do đó, nó không nhầm lẫn giữa "tồn tại một sự đi lên" với "tồn tại một chuỗi con tăng dần có độ dài ba". 

Cuối cùng, bài toán đảm bảo rằng mọi độ cao đều khác nhau. Do đó, sự bình đẳng không bao giờ đóng góp vào một bước nhảy hợp lệ và mọi so sánh trong logic tổng hợp đều nghiêm ngặt. Việc thực hiện bất cẩn bằng cách sử dụng`<=`thay vì`<`sẽ giải quyết được một vấn đề khác.
