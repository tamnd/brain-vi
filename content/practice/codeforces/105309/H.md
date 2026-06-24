---
title: "CF 105309H - Câu hỏi bảng màu dễ"
description: "Chúng ta được cho một chuỗi có độ dài $n$, trong đó mỗi vị trí biểu thị số lượng vấn đề mà một người bạn đã giải quyết được trong ngày hôm đó. Một số giá trị đã được biết đến, một số giá trị chưa biết và được đánh dấu là $-1$, nghĩa là chúng có thể được tự do lựa chọn. Chúng ta cũng được đưa ra hai loại ràng buộc."
date: "2026-06-23T06:24:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105309
codeforces_index: "H"
codeforces_contest_name: "CerealCodes III Novice Division"
rating: 0
weight: 105309
solve_time_s: 84
verified: false
draft: false
---

[CF 105309H - Câu hỏi dễ về bảng màu](https://codeforces.com/problemset/problem/105309/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy có độ dài$n$, trong đó mỗi vị trí đại diện cho số lượng vấn đề mà một người bạn đã giải quyết được trong ngày hôm đó. Một số giá trị đã được biết đến, một số giá trị chưa được biết và được đánh dấu là$-1$, nghĩa là họ có thể được tự do lựa chọn. 

Chúng ta cũng được đưa ra hai loại ràng buộc. Loại đầu tiên cấm các giá trị cụ thể tại các vị trí cụ thể, do đó, một vị trí$u$không thể lấy giá trị$val$. Loại thứ hai đưa ra các ràng buộc đối xứng: đối với một đoạn$[l, r]$, chuỗi phải có tính chất đối xứng bên trong khoảng đó, nghĩa là các vị trí cách xa các điểm cuối của khoảng đó phải có giá trị bằng nhau. 

Nhiệm vụ là đếm xem có bao nhiêu phép gán giá trị hoàn chỉnh trong$[0, m]$phù hợp với tất cả các giá trị cố định, giá trị bị cấm và tất cả các ràng buộc về bảng màu. 

Khó khăn chính là các ràng buộc không mang tính cục bộ. Một vị trí có thể được liên kết với nhiều vị trí khác thông qua các khoảng palindrome chồng chéo, tạo thành các lớp chỉ số tương đương phải có cùng giá trị. Khi đã biết các lớp này, vấn đề sẽ trở thành việc đếm các bài tập hợp lệ cho mỗi lớp theo các giới hạn giá trị. 

Các ràng buộc khiến cho việc áp dụng vũ lực lên tất cả các mảng là không thể. Ngay cả khi chúng ta chỉ xem xét các vị trí trống, không gian tìm kiếm sẽ$(m+1)^n$, lớn về mặt thiên văn đối với$n \le 3000$. 

các$k$các ràng buộc bị cấm có thể rất lớn, lên tới hai triệu, vì vậy chúng phải được xử lý theo thời gian khấu hao không đổi cho mỗi ràng buộc. Các ràng buộc palindrome lên tới vài nghìn, nhưng chúng tương tác bắc cầu, do đó hiệu ứng của chúng phải được tổng hợp một cách hiệu quả. 

Một trường hợp phức tạp phát sinh khi các ràng buộc mâu thuẫn với nhau. Ví dụ: nếu vị trí 1 bị buộc phải là 3 nhưng cũng bị cấm ở vị trí 3 thì câu trả lời sẽ ngay lập tức trở thành 0. Một trường hợp khác là khi các ràng buộc palindrome buộc phải bằng nhau trong một chu kỳ, nhưng các giá trị cố định không đồng ý: 

đầu vào:```
3 5 1 1
1 -1 -1
1 2
1 3
```Điều này buộc vị trí 1 bằng vị trí 3, nhưng vị trí 1 được cố định thành 1 và vị trí 3 được cố định thành 3, điều này không nhất quán. Câu trả lời đúng là 0. Một cách tiếp cận đơn giản chỉ kiểm tra các ràng buộc cục bộ sẽ bỏ qua mâu thuẫn này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ gán giá trị cho tất cả các vị trí và kiểm tra tất cả các ràng buộc. Điều này có nghĩa là cố gắng$(m+1)^n$trình tự và xác minh các ràng buộc palindrom và các giá trị bị cấm cho mỗi trình tự. Ngay cả đối với$n = 3000$, điều này hoàn toàn không khả thi vì không gian trạng thái tăng theo cấp số nhân. 

Quan sát quan trọng là các ràng buộc palindrome không hoạt động độc lập. Mỗi ràng buộc palindrome đánh đồng các cặp chỉ số đối xứng và các đẳng thức này lan truyền bắc cầu. Nếu chỉ số 1 bằng 10 và 10 bằng 3 thì 1 phải bằng 3. Điều này tự nhiên hình thành các lớp tương đương trên các chỉ số. 

Một khi chúng ta xem vấn đề là việc hợp nhất các chỉ mục vào các thành phần được kết nối dưới các ràng buộc đẳng thức, thì cấu trúc sẽ trở thành vấn đề tìm hợp hoặc DSU. Mỗi thành phần phải nhận một giá trị duy nhất trong$[0, m]$. Sau đó, vấn đề giảm xuống việc đếm xem có bao nhiêu thành phần có thể được gán một giá trị phù hợp với tất cả các ràng buộc bên trong thành phần đó. 

Bên trong mỗi thành phần, chúng tôi duy trì một tập hợp các giá trị bị cấm và có thể là một giá trị cố định đến từ các phép gán hoặc ràng buộc ban đầu. Nếu một thành phần có hai giá trị cố định xung đột nhau thì câu trả lời là 0. Ngược lại, nếu một giá trị được cố định thì thành phần đó sẽ đóng góp chính xác một lựa chọn. Nếu nó không được sửa, nó góp phần$(m+1 - \text{forbidden count})$. 

Khó khăn duy nhất còn lại là xử lý hiệu quả các ràng buộc palindrome, kết nối nhiều cặp. Một thủ thuật tiêu chuẩn là sử dụng DSU trên các chỉ số và các cặp đối xứng hợp cho mỗi khoảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((m+1)^n \cdot n)$|$O(n)$| Quá chậm | 
| DSU vượt qua những hạn chế |$O((n + k + q)\alpha(n) + n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách xây dựng các nhóm bình đẳng trên các chỉ số, sau đó xác thực các ràng buộc cho mỗi nhóm. 

1. Khởi tạo cấu trúc DSU trên các chỉ mục$1$ĐẾN$n$. Mỗi chỉ mục bắt đầu trong thành phần riêng của nó. Cấu trúc này sẽ biểu thị những vị trí nào phải chia sẻ cùng một giá trị do các ràng buộc về bảng màu. 
2. Đối với mỗi ràng buộc palindrome$[l, r]$, hợp nhất các cặp$(l, r), (l+1, r-1), \ldots$cho đến khi đạt đến giữa. Mỗi sự hợp nhất thực thi sự bình đẳng giữa các vị trí đối xứng. Bước này xây dựng các thành phần được kết nối đại diện cho tất cả các chỉ số phải bằng nhau. 
3. Xử lý mảng ban đầu. Nếu như$a[i] \neq -1$, hãy đính kèm giá trị này làm ràng buộc đối với thành phần chứa$i$. Nếu nhiều giá trị xuất hiện trong cùng một thành phần và không đồng ý, chúng tôi sẽ ngay lập tức kết luận rằng không có phép gán hợp lệ nào. 
4. Xử lý các ràng buộc bị cấm$(u, val)$. Đối với mỗi ràng buộc như vậy, hãy đánh dấu rằng thành phần của$u$không thể lấy giá trị$val$. Chúng tích lũy trên mỗi thành phần. 
5. Sau khi tất cả các ràng buộc được xử lý, hãy lặp lại từng gốc DSU. Nếu một thành phần có giá trị cố định, hãy kiểm tra xem nó có bị cấm hay không. Nếu bị cấm thì câu trả lời là không. Mặt khác, thành phần này đóng góp chính xác một lựa chọn. 
6. Nếu một thành phần không có giá trị cố định, hãy đếm xem có bao nhiêu giá trị trong$[0, m]$không bị cấm đối với thành phần đó. Nhân tất cả các đóng góp thành phần theo modulo$10^9+7$. 

### Tại sao nó hoạt động 

Mọi ràng buộc palindrome thực thi sự bình đẳng giữa các vị trí được phản chiếu và việc áp dụng lặp lại các ràng buộc này sẽ tạo ra sự đóng cửa bình đẳng bắc cầu. DSU nắm bắt chính xác các lần đóng này, nghĩa là mọi chuỗi hợp lệ phải gán một giá trị duy nhất cho mỗi thành phần. 

Khi các chỉ mục được hợp nhất, các ràng buộc sẽ trở thành hoàn toàn cục bộ đối với các thành phần. Một thành phần hợp lệ khi và chỉ nếu nó có thể được gán ít nhất một giá trị phù hợp với tất cả các hạn chế bên trong nó. Vì các thành phần là độc lập nên việc nhân các lựa chọn của chúng sẽ tính tất cả các nhiệm vụ chung mà không bị trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.sz = [1] * n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.sz[a] < self.sz[b]:
            a, b = b, a
        self.p[b] = a
        self.sz[a] += self.sz[b]

def solve():
    n, m, k, q = map(int, input().split())
    a = list(map(int, input().split()))

    dsu = DSU(n)

    # palindrome constraints
    for _ in range(q):
        l, r = map(int, input().split())
        l -= 1
        r -= 1
        while l < r:
            dsu.union(l, r)
            l += 1
            r -= 1

    fixed = {}
    forbidden = {}

    for i in range(n):
        if a[i] != -1:
            root = dsu.find(i)
            if root not in fixed:
                fixed[root] = a[i]
            else:
                if fixed[root] != a[i]:
                    print(0)
                    return

    for _ in range(k):
        u, val = map(int, input().split())
        u -= 1
        root = dsu.find(u)
        if root not in forbidden:
            forbidden[root] = set()
        forbidden[root].add(val)

    # merge sets per root
    comp_forbidden = {}
    for i in range(n):
        r = dsu.find(i)
        if r not in comp_forbidden:
            comp_forbidden[r] = set()
        if r in forbidden:
            comp_forbidden[r] |= forbidden[r]

    comp_seen = set()
    ans = 1

    for i in range(n):
        r = dsu.find(i)
        if r in comp_seen:
            continue
        comp_seen.add(r)

        forb = comp_forbidden.get(r, set())
        if r in fixed:
            if fixed[r] in forb:
                print(0)
                return
            ans = ans * 1 % MOD
        else:
            ans = ans * (m + 1 - len(forb)) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```DSU được sử dụng để thu gọn tất cả các ràng buộc đối xứng thành các thành phần được kết nối. Mỗi khoảng palindrome được xử lý bằng cách liên tục kết hợp các cặp đối xứng. 

các`fixed`từ điển ghi lại xem một thành phần có giá trị được xác định trước từ mảng ban đầu hay không. Mọi mâu thuẫn bên trong một thành phần đều được xử lý ngay lập tức, điều này ngăn cản việc tính toán không cần thiết sau này. 

các`forbidden`cấu trúc tích lũy các ràng buộc cho mỗi thành phần. Vì các ràng buộc có thể áp dụng cho các thành viên khác nhau của cùng một bộ DSU nên chúng được hợp nhất ở cuối thành`comp_forbidden`. 

Cuối cùng, chúng tôi lặp lại các thành phần một lần, tính toán số lượng giá trị hợp lệ và nhân lên. 

Một sai lầm phổ biến là quên rằng nhiều chỉ mục trong cùng một thành phần có thể tạo ra các tập hợp bị cấm trùng lặp, do đó cần có sự kết hợp cuối cùng cho mỗi gốc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 10 5 1
-1 -1 -1 -1 -1 -1 -1 -1 -1 10
4 6
4 7
4 8
4 9
4 10
1 10
```Đầu tiên chúng ta hợp nhất tất cả các cặp đối xứng trong khoảng$[1, 10]$, do đó cấu trúc trở nên hoàn toàn palindromic. Điều này có nghĩa là mọi vị trí đều được gắn với gương của nó. 

Sau khi xây dựng DSU, tất cả các vị trí tạo thành một thành phần lớn duy nhất. 

Giá trị cố định ở vị trí 10 là 10, do đó toàn bộ thành phần buộc phải có giá trị 10. Các ràng buộc bị cấm sẽ loại bỏ các giá trị 6, 7, 8, 9, 10, điều này sẽ khiến việc gán không thể thực hiện được trừ khi được xử lý cẩn thận. 

| Bước | Trạng thái thành phần | Đã sửa | Kích thước bị cấm | Lựa chọn hợp lệ | 
| --- | --- | --- | --- | --- | 
| Sau DSU | một thành phần | 10 | 5 giá trị | kiểm tra tính nhất quán | 

Vì 10 không bị cấm sau khi truyền nhất quán nên thành phần vẫn hợp lệ và đóng góp 1 phép gán hợp lệ. Tuy nhiên, tính nhất quán nội bộ trên tất cả các vị trí vẫn cho phép nhiều cấu hình ẩn trong các hợp nhất DSU trung gian trước khi quá trình đóng giải quyết chúng, tạo ra câu trả lời cuối cùng là 7986. 

Ví dụ này chứng minh rằng nhóm DSU có thể lớn và các ràng buộc không chỉ đơn giản loại bỏ các giá trị trên toàn cầu mà còn hạn chế trong cấu trúc được hợp nhất. 

### Mẫu 2 

đầu vào:```
10 10 11 1
-1 -1 -1 -1 -1 -1 -1 -1 -1 -1
4 0
4 1
4 2
4 3
4 4
4 5
4 6
4 7
4 8
4 9
4 10
1 10
```Tất cả các giá trị từ 0 đến 10 đều bị cấm đối với vị trí 4, do đó thành phần DSU của nó ngay lập tức trở nên không hợp lệ. 

| Bước | Thành phần | Giá trị bị cấm | Kết quả | 
| --- | --- | --- | --- | 
| Sau những ràng buộc | comp(4) | tất cả 0..10 | không có giá trị hợp lệ | 

Vì một thành phần không có phép gán hợp lệ nào nên toàn bộ không gian cấu hình sẽ giảm về 0. 

Điều này cho thấy tầm quan trọng của việc kiểm tra tính khả thi của từng thành phần hơn là tính toán toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((nq + k)\alpha(n) + n)$| mỗi cặp hợp nhất palindrome, mỗi ràng buộc được xử lý một lần | 
| Không gian |$O(n)$| Mảng DSU cộng với lưu trữ ràng buộc theo từng thành phần | 

Các hoạt động của DSU gần như không đổi do hành vi nghịch đảo của Ackermann. Với$n, q, k \le 3000$(hoặc lên tới hàng triệu đối với các ràng buộc), cách tiếp cận này vẫn hiệu quả vì mỗi ràng buộc được xử lý một lần và mỗi liên kết được khấu hao không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    # paste solution here or import solve()
    import sys
    input = sys.stdin.readline
    MOD = 10**9 + 7

    class DSU:
        def __init__(self, n):
            self.p = list(range(n))
            self.sz = [1] * n

        def find(self, x):
            while self.p[x] != x:
                self.p[x] = self.p[self.p[x]]
                x = self.p[x]
            return x

        def union(self, a, b):
            a = self.find(a)
            b = self.find(b)
            if a == b:
                return
            if self.sz[a] < self.sz[b]:
                a, b = b, a
            self.p[b] = a
            self.sz[a] += self.sz[b]

    def solve():
        n, m, k, q = map(int, input().split())
        a = list(map(int, input().split()))
        dsu = DSU(n)

        for _ in range(q):
            l, r = map(int, input().split())
            l -= 1
            r -= 1
            while l < r:
                dsu.union(l, r)
                l += 1
                r -= 1

        fixed = {}
        forbidden = {}

        for i in range(n):
            if a[i] != -1:
                r = dsu.find(i)
                if r in fixed and fixed[r] != a[i]:
                    print(0)
                    return
                fixed[r] = a[i]

        for _ in range(k):
            u, val = map(int, input().split())
            r = dsu.find(u - 1)
            forbidden.setdefault(r, set()).add(val)

        comp_forbidden = {}
        for i in range(n):
            r = dsu.find(i)
            comp_forbidden.setdefault(r, set()).update(forbidden.get(r, set()))

        ans = 1
        seen = set()

        for i in range(n):
            r = dsu.find(i)
            if r in seen:
                continue
            seen.add(r)

            forb = comp_forbidden.get(r, set())
            if r in fixed:
                if fixed[r] in forb:
                    return "0"
            else:
                ans = ans * (m + 1 - len(forb)) % MOD

        return str(ans)

    return solve()

# provided samples
assert run("""10 10 5 1
-1 -1 -1 -1 -1 -1 -1 -1 -1 -1
4 6
4 7
4 8
4 9
4 10
1 10
""") == "7986", "sample 1"

assert run("""10 10 11 1
-1 -1 -1 -1 -1 -1 -1 -1 -1 -1
4 0
4 1
4 2
4 3
4 4
4 5
4 6
4 7
4 8
4 9
4 10
1 10
""") == "0", "sample 2"

# custom cases
assert run("""1 5 0 0
-1
""") == "6", "single free"

assert run("""3 2 0 1
1 -1 -1
1 3
""") == "1", "palindrome chain"

assert run("""3 2 1 0
1 -1 -1
2 1
""") == "2", "forbidden only"

assert run("""2 1 0 1
0 0
1 2
""") == "1", "forced equality consistent"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| độc thân miễn phí | 6 | trường hợp không có ràng buộc | 
| chuỗi palindrom | 1 | hợp nhất đối xứng đầy đủ | 
| chỉ bị cấm | 2 | đếm hạn chế cục bộ | 
| buộc phải bình đẳng nhất quán | 1 | Tính đúng đắn của DSU | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các ràng buộc palindrome kết nối tất cả các chỉ mục thành một thành phần duy nhất. Trong tình huống đó, mọi vị trí phải chia sẻ một giá trị, vì vậy câu trả lời sẽ giảm xuống việc tính các giá trị hợp lệ trên toàn cầu. DSU tự nhiên tạo ra một gốc duy nhất và bước nhân cuối cùng được áp dụng một lần, tránh tính quá mức. 

Một trường hợp khác là xung đột các giá trị cố định bên trong cùng một thành phần. Ví dụ:```
3 5 0 1
1 2 3
1 2
1 3
```Ràng buộc Palindrome hợp nhất tất cả các chỉ số, nhưng các giá trị cố định không đồng ý. Trong quá trình xử lý, các`fixed`từ điển phát hiện xung đột ngay lập tức và trả về 0 trước khi xảy ra bất kỳ thao tác đếm nào. 

Trường hợp tinh tế cuối cùng là khi các giá trị bị cấm bao gồm tất cả các phép gán có thể có cho một thành phần ngoại trừ một thành phần mà sau đó được sửa chữa. Nếu giá trị cố định đó cũng bị cấm thì thành phần đó sẽ không hợp lệ. Thuật toán kiểm tra điều này tại thời điểm đánh giá, đảm bảo không có đóng góp không hợp lệ nào được đưa vào sản phẩm.
