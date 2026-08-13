---
title: "CF 102282I - \u041f\u0440\u043e\u0438\u0437\u0432\u0435\u0434\u0435\u043d\u0438\u044f"
description: "Chúng ta có một lưới (n lần n). Một số ô chứa số nguyên dương và phần còn lại chứa số 0. Mỗi hàng và mỗi cột phải chứa chính xác hai ô khác 0."
date: "2026-08-13T09:15:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102282
codeforces_index: "I"
codeforces_contest_name: "2011, \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u043a\u043e\u043d\u0442\u0435\u0441\u0442 \u0421\u0413\u0410\u0423 \u043d\u0430 \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b ACM ICPC"
rating: 0
weight: 102282
solve_time_s: 204
verified: true
draft: false
---

[CF 102282I - \u041f\u0440\u043e\u0438\u0437\u0432\u0435\u0434\u0435\u043d\u0438\u044f](https://codeforces.com/problemset/problem/102282/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới (n \times n). Một số ô chứa số nguyên dương và phần còn lại chứa số 0. Mỗi hàng và mỗi cột phải chứa chính xác hai ô khác 0. Tích của hai số ở hàng (i) phải bằng (y_i), còn tích của hai số ở cột (j) phải bằng (x_j). Mỗi số được viết vào lưới phải khác với mọi số được viết khác. 

Dữ liệu đầu vào bao gồm (n), (n) sản phẩm cột bắt buộc (x_0,\ldots,x_{n-1}) và (n) sản phẩm hàng bắt buộc (y_0,\ldots,y_{n-1}). Câu lệnh đảm bảo rằng tồn tại ít nhất một lưới hợp lệ, vì vậy chúng ta chỉ phải xây dựng một lưới. 

Một cách hữu ích để xem lưới là dưới dạng biểu đồ hai bên. Tạo một đỉnh cho mỗi hàng và một đỉnh cho mỗi cột. Một ô khác 0 sẽ trở thành cạnh giữa hàng và cột của nó và số trong ô trở thành nhãn cạnh. Vì mỗi hàng và mỗi cột chứa đúng hai số nên mọi đỉnh đều có bậc hai. Do đó, các ô được chọn tạo thành một tập hợp các chu kỳ chẵn. Sản phẩm của các nhãn liên quan đến từng hàng hoặc cột được quy định. 

Giới hạn (n \le 10) là tín hiệu chính về cách tiếp cận dự kiến. Chỉ có (2n \le 20) số để đặt, nhưng vị trí và giá trị của chúng tương tác qua cả hàng và cột. Lực tác động lên các ô tùy ý là quá lớn, trong khi tìm kiếm quay lui được cắt tỉa cẩn thận trên cấu trúc nhân bắt buộc là đủ nhỏ. 

Các sản phẩm nhiều nhất là (1000). Điều này đặc biệt hữu ích vì mỗi số được đặt trong bảng chia cho tích của hàng chứa nó và cũng chia cho tích của cột. Do đó, tất cả các giá trị ứng cử viên đều nằm trong số ước của các số từ (1) đến (1000) và mục tiêu có rất ít khả năng phân tích thành hai số nguyên dương riêng biệt. 

Có một số trường hợp nguy hiểm mà việc triển khai bất cẩn có thể bỏ sót. Số (1) được phép làm giá trị ô và nó có thể cần thiết. Ví dụ,```
2
2 12
3 8
```có đầu ra hợp lệ```
1 3
2 4
```Hàng đầu tiên sử dụng (1) và (3), do đó, việc loại bỏ (1) là thừa số vô ích sẽ loại bỏ nghiệm không chính xác. 

Một sản phẩm hàng cũng có thể là một hình vuông mà hai giá trị ô của nó không bằng nhau. Ví dụ,```
2
20 63
36 35
```có thể được giải quyết bằng```
4 9
5 7
```Hàng đầu tiên có tích (4 \cdot 9 = 36). Bộ giải chỉ xét cặp ((6,6)) của bình phương (36) sẽ không thành công, mặc dù các giá trị bằng nhau bị cấm. 

Thứ tự của hai yếu tố rất quan trọng vì chúng phải đi vào các cột khác nhau. Ví dụ,```
2
10 21
6 35
```có giải pháp```
2 3
5 7
```Các thừa số (2) và (3) đều thuộc tích hàng (6), nhưng chỉ (2) phù hợp với sản phẩm cột đầu tiên (10), trong khi (3) phù hợp với sản phẩm cột thứ hai (21). Việc xử lý một cặp yếu tố không có thứ tự như thể hướng của nó không quan trọng sẽ làm mất đi các vị trí hợp lệ. 

## Phương pháp tiếp cận 

Lực lượng vũ phu trực tiếp nhất chọn hai ô trong mỗi hàng, sau đó chọn hai giá trị có sản phẩm là sản phẩm hàng bắt buộc. Với (n=10), ngay cả việc chỉ chọn các vị trí cũng đã cho 

[ 
\binom{100}{2}^{10}=4950^{10} 
] 

các lựa chọn có thể có trước khi xem xét các giá trị, các ràng buộc về cột hoặc yêu cầu tất cả các giá trị phải khác biệt. Đó là khoảng (8,7\cdot10^{36}) khả năng, do đó việc liệt kê từng ô là hoàn toàn không thực tế. 

Quan sát hữu ích là tích hàng không cho phép các giá trị tùy ý. Nếu tích còn lại của một hàng là (p) thì hai giá trị của nó phải là cặp nhân tố (a,b) với (ab=p). Vì (p\le1000) nên có rất ít cặp như vậy. Quan sát tương tự áp dụng cho các cột. 

Chúng ta có thể tiến thêm một bước nữa và biểu diễn mỗi hàng và cột bằng tích còn lại và bậc còn lại của nó. Ban đầu mọi đỉnh đều có bậc hai và tích còn lại của nó là đích ban đầu. Bất cứ khi nào chúng ta đặt một giá trị (v) trên một cạnh, chúng ta chia tích còn lại của cả hai điểm cuối cho (v) và giảm cả hai độ điểm cuối đi một. 

Việc tìm kiếm luôn chọn hàng hoặc cột bị hạn chế nhất hiện nay. Nếu một đỉnh chỉ còn một cạnh còn lại thì giá trị tiếp theo của nó hoàn toàn được xác định bởi tích còn lại của nó. Chúng ta chỉ phải quyết định đỉnh đối diện nào nhận được nó. Nếu một đỉnh có hai cạnh còn lại, chúng ta liệt kê các cặp nhân tố của nó và hai đỉnh đối diện có thể có. 

Việc này nhỏ hơn nhiều so với việc tìm kiếm trên các ô. Quan trọng hơn, khi chúng ta chọn một đỉnh bậc hai và kết nối nó với hai đỉnh lân cận, những đỉnh lân cận đó sẽ trở thành đỉnh bậc một. Việc tìm kiếm sau đó sẽ truyền bá các giá trị bắt buộc trong suốt chu trình kết quả. Quá trình tương tự lặp lại đối với thành phần khác nếu giải pháp bao gồm nhiều chu kỳ. 

Đối với một tích không quá (1000) thì có nhiều nhất là 32 ước số, do đó có nhiều nhất là 16 cặp nhân tố không có thứ tự. Một đỉnh bậc hai có nhiều nhất (n(n-1)) các lựa chọn theo thứ tự của hai lân cận riêng biệt, đưa ra nhiều nhất (16\cdot10\cdot9=1440) ứng cử viên cục bộ trước khi phân chia và cắt tỉa tính duy nhất. Trong thực tế, số lượng ứng cử viên nhỏ hơn đáng kể vì mỗi ứng cử viên cũng phải chia tích còn lại của hai người hàng xóm được chọn của mình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O\left(\binom{n^2}{2}^n\right)) trước khi chọn giá trị | (O(n^2)) | Quá chậm | 
| Hạn chế quay lại | (O(B^{2n})) trường hợp xấu nhất, (B\le1440), với tính năng cắt tỉa phân chia mạnh mẽ | (O(n^2)) ngoài ngăn xếp tìm kiếm và ghi nhớ | Đã chấp nhận | 

Giới hạn trường hợp xấu nhất theo cấp số nhân được cố tình lỏng lẻo. Việc tìm kiếm thực tế được kiểm soát bởi (n\le10), số lượng ước số nhỏ lên tới (1000), cấu trúc bậc hai và thực tế là mọi phép gán một phần không thành công đều bị từ chối ngay khi một điểm cuối không thể hoàn thành được nữa. 

## Hướng dẫn thuật toán

1. Coi (n) hàng và (n) cột là (2n) đỉnh. Đối với mỗi đỉnh, lưu tích tích còn lại và bậc còn lại của nó. Ban đầu mỗi độ là hai và tích còn lại là giá trị đầu vào tương ứng. 
2. Duy trì tập hợp các giá trị ô đã được sử dụng. Một giá trị chỉ có thể được đặt nếu nó chưa xuất hiện ở bất kỳ nơi nào khác trong lưới. Đồng thời duy trì các ô cột hàng nào đã bị chiếm dụng, vì biểu đồ phải đơn giản và không thể chọn cùng một ô hai lần. 
3. Đối với một đỉnh có bậc còn lại là 1, giá trị cạnh tiếp theo của nó là bắt buộc. Nó phải bằng tích còn lại của đỉnh. Chúng tôi liệt kê các đỉnh đối diện trong đó giá trị đó chia cho tích còn lại đối diện và nơi ô kết nối chưa được sử dụng. 
4. Đối với một đỉnh có bậc còn lại là hai, hãy liệt kê mọi hệ số hóa (a\cdot b=p) của tích còn lại của nó thành hai giá trị riêng biệt. Đối với mỗi cặp thừa số, chọn hai đỉnh đối diện riêng biệt có sẵn. Cả hai hướng, (a) ở đỉnh đầu tiên và (b) ở đỉnh thứ hai và ngược lại, đều được xem xét vì các tích đối diện chỉ có thể chấp nhận một hướng. 
5. Trước khi chấp nhận một ứng viên, hãy kiểm tra những hậu quả tức thời trên cả hai điểm cuối. Nếu điểm cuối trở thành độ 0 thì tích còn lại của nó phải trở thành một. Nếu nó trở thành cấp một thì sản phẩm còn lại của nó phải là một giá trị vẫn có thể sử dụng được. Nếu vẫn giữ nguyên độ hai thì tích còn lại của nó vẫn phải được phân tích thành hai giá trị riêng biệt chưa sử dụng. Việc kiểm tra này loại bỏ các nhánh không thể thực hiện được trước khi đệ quy. 
6. Trong số tất cả các đỉnh vẫn còn các cạnh liên quan, hãy chọn đỉnh có ít ứng cử viên khả thi nhất hiện nay. Đây là ý tưởng về giá trị còn lại tối thiểu tiêu chuẩn. Đỉnh cấp một bắt buộc đặc biệt có giá trị vì giá trị của nó đã được biết trước nên thường chỉ có một vài đỉnh lân cận có thể có. 
7. Áp dụng ứng viên đã chọn bằng cách ghi giá trị của nó vào các ô tương ứng, chia các tích còn lại, giảm độ và đánh dấu các giá trị và ô được sử dụng. 
8. Lặp lại cho đến khi mọi đỉnh đều có bậc 0. Tại thời điểm đó, mỗi hàng và cột có chính xác hai giá trị, tất cả các sản phẩm còn lại là một và tất cả các giá trị ô đều khác nhau. Lưới được xây dựng là câu trả lời. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi phép gán một phần biểu thị một tập hợp hợp lệ các cạnh đã được chọn và đối với mỗi đỉnh, tích còn lại được lưu trữ của nó chính xác là tích vẫn được yêu cầu từ các cạnh phụ chưa được gán của nó. Một ứng cử viên chỉ được xem xét khi giá trị của nó chia cho cả hai sản phẩm điểm cuối, tôn trọng các mức còn lại, sử dụng một ô trống và không được sử dụng trên toàn cầu. Do đó mọi trạng thái đệ quy vẫn có thể tương ứng với một sự hoàn thành hợp lệ. Ngược lại, để hoàn thành hợp lệ một trạng thái, đỉnh được chọn tiếp theo phải sử dụng giá trị còn lại bắt buộc của nó khi bậc của nó là một hoặc một trong các cặp nhân tố hợp lệ khi bậc của nó là hai, được kết nối với hai lân cận thực tế của nó. Việc tìm kiếm liệt kê tất cả các khả năng như vậy nên không thể loại bỏ giải pháp hợp lệ duy nhất. Vì đầu vào đảm bảo rằng giải pháp tồn tại nên một số nhánh đạt đến trạng thái ở đó tất cả các độ đều bằng 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 1000

factor_pairs = [[] for _ in range(MAXV + 1)]
for p in range(1, MAXV + 1):
    d = 1
    while d * d <= p:
        if p % d == 0:
            e = p // d
            if d != e:
                factor_pairs[p].append((d, e))
        d += 1

def solve(data: str) -> str:
    it = iter(data.split())
    n = int(next(it))
    x = [int(next(it)) for _ in range(n)]
    y = [int(next(it)) for _ in range(n)]

    if prod(x) != prod(y):
        return ""

    m = 2 * n

    rem = x[:] + y[:]
    deg = [2] * m

    ans = [[0] * n for _ in range(n)]
    used_mask = 0
    edge_mask = 0

    def edge_bit(v, u):
        if v < n:
            r, c = v, u - n
        else:
            r, c = u, v - n
        return 1 << (r * n + c)

    def neighbors(v):
        if v < n:
            return range(n, 2 * n)
        return range(n)

    def cell_coords(v, u):
        if v < n:
            return v, u - n
        return u, v - n

    def value_unused(v, mask):
        return (mask >> v) & 1 == 0

    def future_possible(u, value, mask, emask):
        if deg[u] <= 0:
            return False

        if rem[u] % value != 0:
            return False

        nr = rem[u] // value
        nd = deg[u] - 1

        if nd == 0:
            return nr == 1

        if value_unused(value, mask):
            pass

        if nd == 1:
            if nr <= 0 or nr > MAXV:
                return False
            if not value_unused(nr, mask):
                return False

            for w in neighbors(u):
                if deg[w] == 0:
                    continue
                bit = edge_bit(u, w)
                if emask & bit:
                    continue
                if rem[w] % nr == 0:
                    return True
            return False

        if nr <= 0 or nr > MAXV:
            return False

        for a, b in factor_pairs[nr]:
            if not value_unused(a, mask) or not value_unused(b, mask):
                continue
            for w1 in neighbors(u):
                if deg[w1] == 0:
                    continue
                bit1 = edge_bit(u, w1)
                if emask & bit1:
                    continue
                if rem[w1] % a != 0:
                    continue
                for w2 in neighbors(u):
                    if w2 == w1 or deg[w2] == 0:
                        continue
                    bit2 = edge_bit(u, w2)
                    if emask & bit2:
                        continue
                    if rem[w2] % b == 0:
                        return True
        return False

    def candidates(v, mask, emask):
        result = []

        if deg[v] == 0:
            return result

        if deg[v] == 1:
            value = rem[v]

            if value <= 0 or value > MAXV:
                return result
            if not value_unused(value, mask):
                return result

            for u in neighbors(v):
                if deg[u] == 0:
                    continue

                bit = edge_bit(v, u)
                if emask & bit:
                    continue

                if rem[u] % value != 0:
                    continue

                new_mask = mask | (1 << value)
                if not future_possible(u, value, new_mask, emask | bit):
                    continue

                result.append((u, value))
            return result

        for a, b in factor_pairs[rem[v]]:
            if not value_unused(a, mask) or not value_unused(b, mask):
                continue

            for u in neighbors(v):
                if deg[u] == 0:
                    continue
                bit_u = edge_bit(v, u)
                if emask & bit_u:
                    continue
                if rem[u] % a != 0:
                    continue

                for w in neighbors(v):
                    if w == u or deg[w] == 0:
                        continue
                    bit_w = edge_bit(v, w)
                    if emask & bit_w:
                        continue
                    if rem[w] % b != 0:
                        continue

                    new_mask = mask | (1 << a) | (1 << b)
                    new_emask = emask | bit_u | bit_w

                    if not future_possible(u, a, new_mask, new_emask):
                        continue
                    if not future_possible(w, b, new_mask, new_emask):
                        continue

                    result.append((u, a, w, b))

        return result

    failed = set()

    def dfs(mask, emask):
        if all(d == 0 for d in deg):
            return True

        key = (
            tuple(rem),
            tuple(deg),
            mask,
            emask,
        )
        if key in failed:
            return False

        best_v = -1
        best_candidates = None

        for v in range(m):
            if deg[v] == 0:
                continue

            cand = candidates(v, mask, emask)

            if not cand:
                failed.add(key)
                return False

            if best_candidates is None or len(cand) < len(best_candidates):
                best_v = v
                best_candidates = cand

                if len(best_candidates) == 1:
                    break

        v = best_v

        for cand in best_candidates:
            if deg[v] == 1:
                u, value = cand

                r, c = cell_coords(v, u)
                ans[r][c] = value

                old_rem_v = rem[v]
                old_rem_u = rem[u]
                old_deg_v = deg[v]
                old_deg_u = deg[u]

                rem[v] //= value
                rem[u] //= value
                deg[v] -= 1
                deg[u] -= 1

                bit = edge_bit(v, u)
                if dfs(mask | (1 << value), emask | bit):
                    return True

                rem[v] = old_rem_v
                rem[u] = old_rem_u
                deg[v] = old_deg_v
                deg[u] = old_deg_u
                ans[r][c] = 0

            else:
                u, a, w, b = cand

                r1, c1 = cell_coords(v, u)
                r2, c2 = cell_coords(v, w)

                ans[r1][c1] = a
                ans[r2][c2] = b

                old = (
                    rem[v], rem[u], rem[w],
                    deg[v], deg[u], deg[w]
                )

                rem[v] //= a
                rem[u] //= a
                deg[v] -= 1
                deg[u] -= 1

                rem[v] //= b
                rem[w] //= b
                deg[v] -= 1
                deg[w] -= 1

                bit1 = edge_bit(v, u)
                bit2 = edge_bit(v, w)

                if dfs(
                    mask | (1 << a) | (1 << b),
                    emask | bit1 | bit2
                ):
                    return True

                rem[v], rem[u], rem[w] = old[:3]
                deg[v], deg[u], deg[w] = old[3:]
                ans[r1][c1] = 0
                ans[r2][c2] = 0

        failed.add(key)
        return False

    dfs(used_mask, edge_mask)

    return "\n".join(" ".join(map(str, row)) for row in ans)

def prod(a):
    result = 1
    for v in a:
        result *= v
    return result

if __name__ == "__main__":
    data = sys.stdin.read()
    sys.stdout.write(solve(data))
```Bảng cặp nhân tố được tính toán trước một lần. Đối với mỗi sản phẩm lên tới (1000), nó chỉ lưu trữ các cặp thừa số riêng biệt, vì sử dụng cùng một giá trị hai lần sẽ vi phạm điều kiện duy nhất toàn cục. 

Các mảng`rem`Và`deg`chứa cả hai vế của đồ thị lưỡng cực. Đỉnh`0`bởi vì`n-1`đại diện cho các hàng, trong khi các đỉnh`n`bởi vì`2*n-1`đại diện cho các cột. Chia`rem`và giảm dần`deg`sau khi đặt một cạnh phản ánh trực tiếp bất biến toán học từ thuật toán. 

các`edge_mask`là cần thiết ngoài thông tin về bằng cấp. Hai đỉnh có thể vẫn chưa được sử dụng trong khi ô kết nối của chúng đã bị chiếm bởi một quyết định trước đó. Nếu không ghi nhớ các ô bị chiếm giữ, việc tìm kiếm có thể vô tình tạo ra một cạnh song song giữa cùng một hàng và cột. 

các`used_mask`lưu trữ tất cả các giá trị được sử dụng dưới dạng bit. Vì mọi giá trị tối đa là (1000), số nguyên Python là một biểu diễn hiệu quả. Việc kiểm tra xem một giá trị đã xuất hiện hay chưa sẽ trở thành thao tác một bit.`future_possible`thực hiện kiểm tra chuyển tiếp cục bộ. Nó không chứng minh rằng trường hợp còn lại có thể giải quyết được, nhưng nó phát hiện ra một số điều không thể xảy ra ngay lập tức. Cụ thể, một đỉnh trở thành độ 0 phải có tích còn lại là một, và một đỉnh trở thành độ một phải có một giá trị còn lại mà thực tế có thể được kết nối ở đâu đó. 

Tìm kiếm đệ quy sử dụng các giá trị tối thiểu còn lại. Nó tạo ra các ứng cử viên cho mọi đỉnh chưa hoàn thành và chọn đỉnh có danh sách ứng cử viên nhỏ nhất. Đỉnh cấp một thường gần như bị ép buộc, do đó, sau quyết định cấp hai đầu tiên, việc tìm kiếm thường lan truyền trong toàn bộ chu trình với rất ít sự phân nhánh. 

Khóa ghi nhớ chứa các sản phẩm còn lại, độ còn lại, giá trị đã sử dụng và các ô bị chiếm dụng. Tất cả những điều này đều cần thiết vì hai trạng thái có sản phẩm giống hệt nhau vẫn có thể khác nhau ở chỗ ô nào đã được sử dụng hoặc giá trị nào đã được sử dụng. 

Số nguyên Python không bị tràn, vì vậy các sản phẩm và mặt nạ bit đều an toàn. Đầu vào chỉ có một ca kiểm thử, chính xác như được chỉ định bởi câu lệnh, do đó không có vòng lặp ca kiểm thử. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2
2 12
3 8
```Hàng đầu tiên phải chứa hai thừa số phân biệt của (3), nên khả năng duy nhất là (1) và (3). Cột đầu tiên có mục tiêu (2) nên (1) phải đến đó. Cột thứ hai nhận được (3). Hàng còn lại có mục tiêu (8) và vị trí duy nhất có thể có của nó là (2) trong cột đầu tiên và (4) trong cột thứ hai. 

| Bước | Đỉnh được chọn | Bài tập | Sản phẩm còn lại | 
| --- | --- | --- | --- | 
| 0 | Hàng 0 | không | Hàng: 3, 8; Cột: 2, 12 | 
| 1 | Hàng 0 | (r_0c_0=1,\ r_0c_1=3) | Hàng: 1, 8; Cột: 2, 4 | 
| 2 | Cột 0 | (r_1c_0=2) | Hàng: 1, 4; Cột: 1, 4 | 
| 3 | Cột 1 | (r_1c_1=4) | Hàng: 1, 1; Cột: 1, 1 | 

Lưới cuối cùng là```
1 3
2 4
```Mỗi hàng và cột có chính xác hai mục và bốn giá trị khác nhau. Bảng cũng minh họa tại sao các đỉnh bậc một lại mạnh: sau khi hàng đầu tiên được cố định, mỗi cột có đúng một cạnh còn lại, do đó giá trị của nó là bắt buộc. 

### Mẫu 2 

Đầu vào là```
3
5 8 18
2 30 12
```Đỉnh ban đầu bị ràng buộc nhất là cột 0. Mục tiêu của nó là (5) và hai thừa số của nó phải là (1) và (5). Các sản phẩm hàng tương thích duy nhất là hàng số 0 cho (1) và hàng một cho (5). 

| Bước | Đỉnh được chọn | Bài tập | Sản phẩm còn lại | 
| --- | --- | --- | --- | 
| 0 | Cột 0 | không | Hàng: 2, 30, 12; Cột: 5, 8, 18 | 
| 1 | Cột 0 | (r_0c_0=1,\ r_1c_0=5) | Hàng: 2, 6, 12; Cột: 1, 8, 18 | 
| 2 | Hàng 1 | (r_1c_2=6) | Hàng: 2, 1, 12; Cột: 1, 8, 3 | 
| 3 | Cột 2 | (r_2c_2=3) | Hàng: 2, 1, 4; Cột: 1, 8, 1 | 
| 4 | Hàng 0 | (r_0c_1=2) | Hàng: 1, 1, 4; Cột: 1, 6, 1 | 
| 5 | Hàng 2 | (r_2c_1=4) | Hàng: 1, 1, 1; Cột: 1, 1, 1 | 

Lưới kết quả là```
1 2 0
5 0 6
0 4 3
```Dấu vết thể hiện sự lan truyền theo chu kỳ. Khi cột 0 được cố định, hàng một chỉ còn lại một thừa số, cột này cố định cột hai, sau đó cố định hệ số của hàng hai trong cột đó. Hai vị trí cuối cùng trở nên bắt buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(B^{2n})) trong trường hợp xấu nhất, (B\le1440) | Mỗi quyết định đệ quy gán ít nhất một cạnh và mọi quyết định bậc hai chỉ xem xét các cặp nhân tố và cặp đỉnh đối diện | 
| Không gian | (O(n^2 + S)) | Lưới, mặt nạ, trạng thái đệ quy và trạng thái lỗi được ghi nhớ yêu cầu lưu trữ tỷ lệ thuận với tìm kiếm | 

Giới hạn hàm mũ lý thuyết là bảo thủ có chủ ý. Với (n\le10), chỉ có 20 đỉnh và 20 ô bị chiếm trong một nghiệm hoàn chỉnh. Giới hạn số chia làm cho mỗi lựa chọn yếu tố cục bộ trở nên nhỏ bé, trong khi việc kiểm tra chuyển tiếp và phỏng đoán ứng viên tối thiểu sẽ loại bỏ hầu hết các nhánh ngay lập tức. Đây là cấu trúc làm cho việc tìm kiếm trở nên thực tế trong giới hạn (1) giây đã cho. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm dưới đây giả định`solve`chức năng từ giải pháp có sẵn trong cùng một tệp hoặc được nhập từ nó. Vì bài toán cho phép bất kỳ câu trả lời hợp lệ nào nên các mẫu được kiểm tra chính xác, trong khi các trường hợp tùy chỉnh được kiểm tra bằng trình xác thực xác minh mọi yêu cầu của bài toán.```python
import io
import sys

def run(inp: str) -> str:
    return solve(inp).strip()

def validate(inp: str, out: str) -> bool:
    data = list(map(int, inp.split()))
    n = data[0]
    x = data[1:1 + n]
    y = data[1 + n:1 + 2 * n]

    lines = out.strip().splitlines()
    if len(lines) != n:
        return False

    a = []
    for line in lines:
        row = list(map(int, line.split()))
        if len(row) != n:
            return False
        a.append(row)

    used = set()

    for i in range(n):
        values = [a[i][j] for j in range(n) if a[i][j] != 0]
        if len(values) != 2:
            return False
        if values[0] in used or values[1] in used:
            return False
        if values[0] <= 0 or values[1] <= 0:
            return False
        if values[0] * values[1] != y[i]:
            return False
        used.update(values)

    for j in range(n):
        values = [a[i][j] for i in range(n) if a[i][j] != 0]
        if len(values) != 2:
            return False
        if values[0] * values[1] != x[j]:
            return False

    return True

sample1 = """\
2
2 12
3 8
"""

sample2 = """\
3
5 8 18
2 30 12
"""

assert run(sample1) == "1 3\n2 4", "sample 1"
assert run(sample2) == "1 2 0\n5 0 6\n0 4 3", "sample 2"

case_min = """\
2
10 21
6 35
"""
assert validate(case_min, run(case_min)), "minimum size and forced orientation"

case_boundary = """\
2
600 500
1000 300
"""
assert validate(case_boundary, run(case_boundary)), "product 1000 boundary"

case_equal_rows = """\
4
15 120 90 80
60 60 60 60
"""
assert validate(case_equal_rows, run(case_equal_rows)), "equal row products"

case_max = """\
10
11 40 57 72 85 96 105 112 117 120
20 38 54 68 80 90 98 104 108 110
"""
assert validate(case_max, run(case_max)), "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 10 21 / 6 35`| Bất kỳ lưới (2\times2) hợp lệ nào | Kích thước tối thiểu và định hướng yếu tố | 
|`2 / 600 500 / 1000 300`| Bất kỳ lưới (2\times2) hợp lệ nào | Sản phẩm biên (1000), cộng với các thừa số lớn | 
|`4 / 15 120 90 80 / 60 60 60 60`| Bất kỳ lưới (4\times4) hợp lệ nào | Nhiều sản phẩm mục tiêu bình đẳng | 
|`10 / 11 40 57 72 85 96 105 112 117 120 / 20 38 54 68 80 90 98 104 108 110`| Bất kỳ lưới (10\times10) hợp lệ nào | Tối đa (n) và tìm kiếm quay lui đầy đủ | 

Trường hợp kích thước tối đa xuất phát từ chu kỳ mười sử dụng các giá trị riêng biệt (1) đến (20). Các cặp hàng của nó là ((1,20),(2,19),\ldots,(10,11)) và các tích của cột được chọn từ các giá trị liền kề xung quanh chu trình. Mọi sản phẩm bắt buộc đều ở mức dưới (1000), trong khi tất cả 20 giá trị ô vẫn khác biệt. 

## Vỏ cạnh 

Giá trị (1) được xử lý chính xác như mọi số nguyên dương khác. Trong mẫu đầu tiên, hàng 0 có tích (3) nên cặp nhân tố là (1,3). Tích cột (2) và (12) buộc (1) vào cột đầu tiên và (3) vào cột thứ hai. Bộ giải không bao giờ coi (1) là đặc biệt và bit của nó được ghi vào`used_mask`, nên giây (1) không thể được giới thiệu sau. 

Đối với tích vuông chẳng hạn như (36), trình tạo cặp nhân tố sẽ liệt kê mọi ước số (d) cho đến (\sqrt{36}), sau đó lưu trữ cặp riêng biệt ((d,36/d)). Do đó ((4,9)) được coi là mặc dù (36) là hình vuông. Cặp ((6,6)) bị loại trừ một cách có chủ ý vì hai giá trị ô phải khác nhau. 

Đối với trường hợp nhạy cảm với định hướng```
2
10 21
6 35
```hàng 0 có thừa số (2) và (3). Cột đầu tiên chấp nhận (2), trong khi cột thứ hai chấp nhận (3). Sau khi đặt xong, hàng còn lại có sản phẩm (35), (5) và (7) được ép vào các ô còn lại. Hướng ngược lại sẽ để lại một cột có sản phẩm còn lại không tương thích, vì vậy việc kiểm tra tiến sẽ loại bỏ nó ngay lập tức hoặc ở bước đệ quy tiếp theo. 

Trường hợp ranh giới sản phẩm```
2
600 500
1000 300
```có một lưới hợp lệ```
20 50
30 10
```bởi vì tích của hàng là (20\cdot50=1000) và (30\cdot10=300), trong khi tích của cột là (20\cdot30=600) và (50\cdot10=500). Bộ giải sử dụng phép chia số nguyên thông thường cho các tích còn lại, do đó các giá trị ở giới hạn trên không gây ra trường hợp số học đặc biệt nào. 

Cuối cùng, các sản phẩm mục tiêu bằng nhau không bao hàm các giá trị ô bằng nhau. Trong trường hợp (4\times4) với tất cả các tích của hàng bằng (60), các hàng khác nhau có thể sử dụng các cặp thừa số khác nhau của (60). Kiểm tra tính duy nhất toàn cầu liên quan đến các số được đặt trong các ô chứ không phải các sản phẩm hàng và cột bắt buộc, do đó các giá trị mục tiêu lặp lại là hoàn toàn hợp lệ khi các cặp yếu tố tương ứng có thể vẫn khác biệt.
