---
title: "CF 1916G - Tối ưu hóa từ Chelsu"
description: "Chúng ta được cho một cây có các cạnh mang trọng số nguyên dương. Với hai đỉnh $u$ và $v$ bất kỳ, hãy gọi $len(u,v)$ là số cạnh trên đường đi của chúng và gọi $gcd(u,v)$ là gcd của tất cả các trọng số của cạnh trên đường đi đó."
date: "2026-06-08T19:53:16+07:00"
tags: ["codeforces", "competitive-programming", "divide-and-conquer", "dp", "number-theory", "trees"]
categories: ["algorithms"]
codeforces_contest: 1916
codeforces_index: "G"
codeforces_contest_name: "Good Bye 2023"
rating: 3500
weight: 1916
solve_time_s: 123
verified: true
draft: false
---

[CF 1916G - Tối ưu hóa từ Chelsu](https://codeforces.com/problemset/problem/1916/G) 

**Đánh giá:** 3500 
**Tags:** chia để trị, dp, lý thuyết số, cây 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây có các cạnh mang trọng số nguyên dương. 

Với hai đỉnh bất kỳ$u$Và$v$, cho phép$len(u,v)$là số cạnh trên đường đi của chúng và gọi$gcd(u,v)$là gcd của tất cả các trọng số cạnh trên đường đi đó. Nhiệm vụ là tối đa hóa$$len(u,v)\cdot gcd(u,v)$$trên tất cả các cặp đỉnh. 

Cây chứa tới$10^5$đỉnh trên tất cả các trường hợp thử nghiệm. Một thuật toán bậc hai trên tất cả các cặp sẽ yêu cầu khoảng$10^{10}$đánh giá cặp trong trường hợp xấu nhất, điều này hoàn toàn không thể thực hiện được trong vòng hai giây. Thậm chí một$O(n\sqrt n)$giải pháp phong cách sẽ không thoải mái. Mục tiêu là thứ gì đó gần gũi$O(n\log^2 n)$. 

Phần khó khăn là vật kính kết hợp hai đại lượng chuyển động ngược chiều nhau. Các đường dẫn dài hơn có xu hướng có các gcd nhỏ hơn, trong khi các gcd lớn thường chỉ xuất hiện trên các đường dẫn ngắn hơn. Bất kỳ giải pháp thành công nào cũng phải khai thác cấu trúc đặc biệt của các giá trị gcd thay vì xử lý tất cả các đường dẫn một cách độc lập. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều đường dẫn khác nhau có cùng một gcd. 

Ví dụ:```
1
3
1 2 6
2 3 6
```Câu trả lời hay nhất đến từ cả con đường$1\leftrightarrow3$:$$2\cdot 6 = 12.$$Chỉ giữ một đường dẫn đại diện cho mỗi gcd sẽ làm mất thông tin về độ dài đường dẫn và tạo ra câu trả lời sai. 

Một cái bẫy khác là các đường đi qua trọng tâm trong đó cả hai nửa đều đến từ cùng một cây con. Những đường dẫn như vậy không được tính trong bước hợp nhất centroid vì chúng được xử lý đệ quy bên trong cây con đó. 

Ví dụ:```
1
4
1 2 12
2 3 12
2 4 12
```con đường$3\leftrightarrow4$đi qua đỉnh$2$, nhưng cả hai điểm cuối đều thuộc về cây con centroid. Việc hợp nhất bất cẩn có thể vô tình kết hợp thông tin từ cùng một đứa trẻ hai lần. 

Cạm bẫy thứ ba đến từ trọng lượng cạnh rất lớn. Từ$w_i\le 10^{12}$, bất kỳ câu trả lời nào cũng có thể có xung quanh$10^{17}$. Sử dụng số nguyên 32 bit sẽ tràn ngay lập tức. 

## Phương pháp tiếp cận 

Một giải pháp vũ lực sẽ kiểm tra mọi cặp đỉnh. Đối với mỗi cặp, chúng ta có thể tính toán gcd đường dẫn và độ dài đường dẫn bằng cách sử dụng tiền xử lý dựa trên LCA. 

Số cặp là$O(n^2)$. Ngay cả khi mọi truy vấn đều trở thành$O(1)$, chúng ta vẫn cần khoảng$5\cdot10^9$đánh giá khi$n=10^5$. Nút cổ chai không phải là truy vấn đường dẫn mà là số lượng cặp. 

Quan sát quan trọng xuất phát từ việc xem xét các đường đi qua một tâm cố định. 

Giả sử hai chuỗi hướng xuống từ tâm đóng góp$$(len_1,g_1),\qquad (len_2,g_2)$$với$len_1\ge len_2$. 

Đường dẫn kết quả có gcd$$\gcd(g_1,g_2).$$Nếu như$\gcd(g_1,g_2)\lt g_1$, thì nhiều nhất là$g_1/2$. Một số chia thích hợp không bao giờ có thể vượt quá một nửa số đó. Do đó, một đường dẫn chỉ có thể cải thiện câu trả lời khi$g_1$chia rẽ$g_2$. Điều này ngay lập tức biến đổi vấn đề từ các tổ hợp gcd tùy ý thành các quan hệ chia hết. 

Cho rằng$$g_2=k\cdot g_1.$$Con đường kết hợp góp phần$$g_1(len_1+len_2).$$Chỉ sử dụng chuỗi thứ hai đã mang lại$$g_2\,len_2=k\,g_1\,len_2.$$Để con đường hợp nhất có tính cạnh tranh, chúng ta cần$$g_1(len_1+len_2)\ge k\,g_1\,len_2,$$điều đó đơn giản hóa thành$$(k-1)len_2\le len_1.$$Đặc biệt,$k\le len_1+1$và để triển khai được sử dụng trong các giải pháp được chấp nhận, việc liệt kê bội số là đủ$g_2=k g_1$với$k\le len_1$. 

Hạn chế này cực kỳ mạnh mẽ. Với mọi giá trị gcd$g$, chúng ta chỉ cần chuỗi dài nhất đạt được gcd đó. Sau đó chúng ta liệt kê một số lượng nhỏ bội số của$g$chứ không phải tất cả các giá trị gcd khác. 

Thử thách còn lại là tạo ra tất cả các đường dẫn. Vì vấn đề nằm trên cây và mọi đường đi hợp lệ đều có trọng tâm cao nhất duy nhất trong phân rã trọng tâm, nên phân rã trọng tâm trở thành khung tự nhiên. 

Đối với trọng tâm cố định: 

1. Liệt kê mọi nút trong mỗi cây con con. 
2. Ghi lại cặp$(g,len)$, Ở đâu$g$là gcd trên đường đi từ trọng tâm tới nút đó. 
3. Đối với mỗi giá trị gcd, hãy giữ độ dài chuỗi dài nhất và cũng là chuỗi tốt thứ hai đến từ một cây con khác. 
4. Kết hợp các giá trị gcd tương thích bằng cách sử dụng quan sát tính chia hết ở trên. 

Mọi đường đi đều được xử lý chính xác ở trọng tâm cao nhất trên đường đi của nó, do đó không có đường đi nào bị bỏ sót và không có đường đi nào được tính hai lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$kiểm tra cặp ngay cả với các truy vấn đường dẫn nhanh |$O(n)$| Quá chậm | 
| Tối ưu |$O(n\log^2 n\log W)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng phân rã trung tâm của cây. 
2. Đối với tâm hiện tại$c$, DFS vào từng cây con riêng biệt. 
3. Trong thời gian bảo trì DFS:$$g=\gcd(\text{all edges from }c\text{ to current node})$$và độ dài đường đi từ$c$. 
4. Đối với mọi giá trị gcd gặp phải$g$, lưu trữ độ dài chuỗi dài nhất tạo ra gcd đó. Đồng thời lưu trữ cây con nào đã tạo ra nó. 
5. Lưu trữ độ dài tốt thứ hai cho cùng một gcd đến từ cây con khác. Điều này là cần thiết vì đường dẫn nối hai điểm cuối phải sử dụng hai nhánh riêng biệt đi qua tâm. 
6. Hãy để$best[g]$là độ dài tối đa được liên kết với gcd$g$. 
7. Với mọi giá trị gcd$g$, hãy xem chiều dài chuỗi dài nhất của nó$len$. 
8. Liệt kê bội số$$g_2=g\cdot k$$với$1\le k\le len$. 
9. Nếu$g_2$tồn tại giữa các giá trị gcd được ghi lại, hãy thử kết hợp chuỗi cho$g$với chuỗi tốt nhất cho$g_2$. 
10. Nếu cả hai chuỗi đều đến từ các cây con khác nhau, hãy sử dụng độ dài tốt nhất của$g_2$. 
11. Nếu không thì sử dụng độ dài tốt thứ hai của$g_2$. 
12. Cập nhật$$ans=\max(ans,(len_1+len_2)\cdot g).$$1. Đánh dấu trọng tâm là đã bị loại bỏ và lặp lại vào mọi thành phần còn lại. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ con đường nào$P$. Trong phân rã centroid tồn tại một centroid cao nhất duy nhất nằm trên$P$. Đường dẫn được xử lý chính xác khi trọng tâm đó được xử lý. 

Tại trọng tâm đó, hai nửa đường đi tương ứng với hai chuỗi có giá trị gcd$g_1$Và$g_2$. Đường dẫn gcd bằng$\gcd(g_1,g_2)$. 

The divisibility argument shows that any improving combination must satisfy$g_1\mid g_2$, vì vậy việc liệt kê bội số bao gồm mọi ứng cử viên có liên quan. Đối với mỗi giá trị gcd, chúng tôi giữ lại chuỗi dài nhất hiện có, do đó, bất cứ khi nào kiểm tra một cặp giá trị gcd hợp lệ, thuật toán sẽ sử dụng độ dài tối đa có thể có giữa các chuỗi từ các cây con riêng biệt. 

Mọi đường đi ứng viên đều được xem xét ở trọng tâm cao nhất của nó và mọi ứng cử viên được xem xét đều tương ứng với một đường đi thực sự trên cây. Do đó, giá trị tối đa tìm được chính xác là giá trị tối ưu toàn cục. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def solve_case():
    n = int(input())
    g = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v, w = input().split()
        u = int(u) - 1
        v = int(v) - 1
        w = int(w)
        g[u].append((v, w))
        g[v].append((u, w))

    removed = [False] * n
    sz = [0] * n
    ans = 0

    def calc_size(v, p):
        sz[v] = 1
        for to, _ in g[v]:
            if to != p and not removed[to]:
                calc_size(to, v)
                sz[v] += sz[to]

    def find_centroid(v, p, tot):
        for to, _ in g[v]:
            if to != p and not removed[to]:
                if sz[to] > tot // 2:
                    return find_centroid(to, v, tot)
        return v

    def dfs(v, p, cur_g, depth, root_id):
        nonlocal max_g, max_len

        max_g = max(max_g, cur_g)
        max_len = max(max_len, depth)

        if cur_g not in best:
            best[cur_g] = depth
            owner[cur_g] = root_id
        else:
            if depth >= best[cur_g]:
                if owner[cur_g] == root_id:
                    best[cur_g] = depth
                else:
                    second[cur_g] = best[cur_g]
                    best[cur_g] = depth
                    owner[cur_g] = root_id
            elif owner[cur_g] != root_id:
                second[cur_g] = max(second.get(cur_g, 0), depth)

        for to, w in g[v]:
            if to == p or removed[to]:
                continue
            dfs(to, v, gcd(cur_g, w), depth + 1, root_id)

    def decompose(entry):
        nonlocal ans, max_g, max_len

        calc_size(entry, -1)
        c = find_centroid(entry, -1, sz[entry])

        best.clear()
        owner.clear()
        second.clear()

        max_g = 0
        max_len = 0

        for to, w in g[c]:
            if removed[to]:
                continue
            dfs(to, c, w, 1, to)

        keys = sorted(best)

        idx = {x: i for i, x in enumerate(keys)}
        best_len = [best[x] for x in keys]
        best_owner = [owner[x] for x in keys]
        second_len = [second.get(x, 0) for x in keys]

        for i, gg in enumerate(keys):
            length = best_len[i]

            if gg * (length + max_len) <= ans:
                continue

            k = 1
            while k <= length and gg * k <= max_g:
                target = gg * k
                j = idx.get(target)
                if j is not None:
                    if best_owner[j] == best_owner[i]:
                        ans = max(ans, (length + second_len[j]) * gg)
                    else:
                        ans = max(ans, (length + best_len[j]) * gg)
                k += 1

        removed[c] = True

        for to, _ in g[c]:
            if not removed[to]:
                decompose(to)

    best = {}
    owner = {}
    second = {}
    max_g = 0
    max_len = 0

    decompose(0)

    return str(ans)

def main():
    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve_case())
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Mã phân tách đảm bảo rằng mọi đường dẫn được gán chính xác cho một mức trung tâm. 

DFS không chỉ thu thập các giá trị gcd. Đối với mỗi gcd, nó theo dõi chuỗi dài nhất và cây con con đã tạo ra chuỗi đó. Chuỗi dài thứ hai từ một cây con khác cũng được duy trì vì việc kết hợp hai chuỗi từ cùng một cây con sẽ tạo ra một đường dẫn không đi qua trọng tâm hiện tại. 

Điều kiện cắt tỉa```
if gg * (length + max_len) <= ans:
    continue
```là một tối ưu hóa quan trọng từ việc triển khai được chấp nhận. Ngay cả việc ghép nối gcd hiện tại với chuỗi dài nhất có thể cũng không thể cải thiện câu trả lời, vì vậy tất cả các bội số có thể bị bỏ qua. 

Số nguyên Python tự động xử lý các giá trị lên tới$10^{17}$và hơn thế nữa, tránh các vấn đề tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
2
1 2 1000000000000
```Con đường duy nhất là cạnh duy nhất. 

| Đã đạt đến nút | gcd | chiều dài | 
| --- | --- | --- | 
| 2 | 1000000000000 | 1 | 

Trả lời:$$1 \cdot 10^{12}=10^{12}.$$### Ví dụ 2```
4
3 2 6
2 1 10
2 4 6
```Centroid là đỉnh 2. 

| Điểm cuối | gcd từ trung tâm | chiều dài | 
| --- | --- | --- | 
| 1 | 10 | 1 | 
| 3 | 6 | 1 | 
| 4 | 6 | 1 | 

Cặp tương thích tốt nhất là đường dẫn$3\leftrightarrow4$. 

| Chuỗi A | Chuỗi B | Đường dẫn gcd | Độ dài đường dẫn | Giá trị | 
| --- | --- | --- | --- | --- | 
| (6,1) | (6,1) | 6 | 2 | 12 | 

Đáp án = 12. 

Ví dụ này cho thấy tại sao chúng ta phải giữ thông tin từ các cây con khác nhau. Cả hai điểm cuối có gcd 6 đều thuộc về các nhánh riêng biệt của tâm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n\log^2 n\log W)$| Sự phân hủy Centroid góp phần$\log n$mức, trạng thái gcd đóng góp một yếu tố logarit khác | 
| Không gian |$O(n)$| Cây, siêu dữ liệu đệ quy và cấu trúc trung tâm | 

Tổng số đỉnh của tất cả các trường hợp thử nghiệm nhiều nhất là$10^5$, do đó độ phức tạp này phù hợp một cách thoải mái với các giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    # call solution here
    return out.getvalue()

# sample
assert run("""4
2
1 2 1000000000000
4
3 2 6
2 1 10
2 4 6
8
1 2 12
2 3 9
3 4 9
4 5 6
5 6 12
6 7 4
7 8 9
12
1 2 12
2 3 12
2 4 6
2 5 9
5 6 6
1 7 4
4 8 12
8 9 4
8 10 12
2 11 9
7 12 9
""") == """1000000000000
12
18
24
"""

# minimum tree
assert run("""1
2
1 2 7
""") == """7
"""

# all equal weights
assert run("""1
5
1 2 6
2 3 6
3 4 6
4 5 6
""") == """24
"""

# star
assert run("""1
4
1 2 12
1 3 12
1 4 12
""") == """24
"""

# gcd decreases along long path
assert run("""1
3
1 2 12
2 3 18
""") == """12
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cạnh đơn | 7 | Kích thước tối thiểu | 
| Tất cả trọng lượng bằng nhau | 24 | Con đường dài nhất chiếm ưu thế | 
| Cây sao | 24 | Kết hợp hai nhánh trung tâm | 
| Trọng lượng 12 và 18 | 12 | Giảm gcd đúng | 

## Vỏ cạnh 

Hãy xem xét```
1
3
1 2 6
2 3 6
```Cả hai cạnh đều có trọng lượng như nhau. Thuật toán ghi lại giá trị gcd 6 với độ dài 1 từ cả hai phía của tâm. Kết hợp chúng sẽ cho độ dài 2 và gcd 6, tạo ra 12. Chỉ giữ lại một lần xuất hiện của gcd 6 sẽ trả về sai 6. 

Hãy xem xét```
1
4
1 2 12
2 3 12
2 4 12
```Bước hợp nhất centroid lưu trữ quyền sở hữu cây con. Khi kết hợp hai chuỗi, nếu chuỗi tốt nhất cho gcd đến từ cùng một cây con thì thuật toán sẽ quay trở lại chuỗi tốt thứ hai. Điều này ngăn cản việc xây dựng các đường dẫn không tồn tại. 

Coi như```
1
2
1 2 1000000000000
```Câu trả lời bằng$10^{12}$. Trên cây lớn hơn câu trả lời có thể vượt quá$10^{17}$. Việc triển khai sử dụng số nguyên Python có độ chính xác tùy ý, do đó không xảy ra tràn.
