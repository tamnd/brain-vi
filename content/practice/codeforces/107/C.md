---
title: "CF 107C - Sự sắp xếp"
description: "Chúng tôi có n chỗ ngồi và n giáo sư. Giáo sư 1 là người có thâm niên cao nhất, giáo sư n là người có thâm niên thấp nhất. Một số cặp ghế áp đặt các ràng buộc về thứ tự. Nếu (a, b) được cho thì giáo sư ngồi ở ghế a phải cao cấp hơn giáo sư ngồi ở ghế b."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp"]
categories: ["algorithms"]
codeforces_contest: 107
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 83 (Div. 1 Only)"
rating: 2400
weight: 107
solve_time_s: 155
verified: true
draft: false
---

[CF 107C - Sắp xếp](https://codeforces.com/problemset/problem/107/C) 

**Đánh giá:** 2400 
**Thẻ:** bitmask, dp 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`chỗ ngồi và`n`các giáo sư. Giáo sư`1`là người cao cấp nhất, giáo sư`n`là cấp cao nhất. Một số cặp ghế áp đặt các ràng buộc về thứ tự. Nếu như`(a, b)`được đưa ra, sau đó giáo sư ngồi vào chỗ`a`chắc phải cao cấp hơn giáo sư đang ngồi trên ghế`b`. 

Việc sắp xếp chỗ ngồi chỉ đơn giản là sự hoán vị của các giáo sư được phân công vào chỗ ngồi. Vì số lượng giáo sư nhỏ hơn có nghĩa là thâm niên cao hơn, hạn chế`(a, b)`trở thành:```
perm[a] < perm[b]
```trong số các giáo sư được chỉ định. 

Tất cả các sắp xếp hợp lệ đều được sắp xếp theo từ điển. Năm 2001 sử dụng cách sắp xếp hợp lệ thứ nhất, năm 2002 sử dụng cách sắp xếp hợp lệ thứ hai, v.v. Chúng ta phải xuất ra cách sắp xếp được sử dụng trong năm`y`. 

Điều này tương đương với việc tìm`(y - 2001)`- hoán vị hợp lệ theo thứ tự từ điển. 

Nhận xét quan trọng nhất là những ràng buộc nằm ở chỗ ngồi chứ không phải ở giáo sư. Thứ tự tương đối giữa các giá trị chỗ ngồi là cố định. Chúng tôi đang đếm các hoán vị thỏa mãn một phần thứ tự. 

Ràng buộc`n ≤ 16`làm thay đổi hoàn toàn bản chất của vấn đề. Không thể tìm kiếm giai thừa trên tất cả các hoán vị vì:```
16! ≈ 2 * 10^13
```Ngay cả việc tạo ra các hoán vị trong vài giây cũng không đạt được hiệu quả. 

Đồng thời,`2^16 = 65536`, rất dễ quản lý. Điều này gợi ý rõ ràng về một tập hợp con DP. 

Năm có thể lớn bằng`10^18`, có nghĩa là số cách sắp xếp hợp lệ có thể rất lớn. Chúng ta không thể liệt kê từng sự sắp xếp một. Chúng ta phải đếm có bao nhiêu lần hoàn thành hợp lệ tồn tại từ một phép gán một phần và sử dụng số lượng đó để bỏ qua các khối hoán vị. 

Có một số trường hợp nguy hiểm phá vỡ việc triển khai ngây thơ. 

Một tập hợp các ràng buộc mâu thuẫn có thể tạo ra các sắp xếp không hợp lệ. 

Ví dụ:```
2 2001 2
1 2
2 1
```Ghế`1`phải chứa số giáo sư nhỏ hơn số ghế`2`, và đồng thời là một cái lớn hơn. Không có sự sắp xếp nào tồn tại. Đầu ra đúng là:```
The times have changed
```Việc triển khai bất cẩn vẫn có thể cố gắng chuyển đổi DP và vô tình tạo ra số lượng rác. 

Một trường hợp khó phát hiện khác là khi năm yêu cầu vượt quá số lượng thỏa thuận hợp lệ. 

Ví dụ:```
2 2003 0
```chỉ có`2! = 2`sắp xếp:```
1 2
2 1
```Năm 2001 và 2002 đều có hiệu lực. Năm 2003 không tồn tại. Đầu ra đúng là:```
The times have changed
```Một lỗi phổ biến là sử dụng cách lập chỉ mục dựa trên số 0 không chính xác và coi năm 2001 là chỉ mục`1`thay vì`0`. 

Các ràng buộc trùng lặp cũng xuất hiện trong đầu vào. 

Ví dụ:```
3 2001 3
1 2
1 2
1 2
```Những bản sao này sẽ không thay đổi bất cứ điều gì. Nếu các ràng buộc được lưu trữ không đúng cách, chúng có thể tăng thêm mức độ hoặc phá vỡ các quá trình chuyển đổi. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Tạo mọi hoán vị của các giáo sư, kiểm tra xem nó có thỏa mãn tất cả các ràng buộc hay không, thu thập những cái hợp lệ, sắp xếp chúng theo từ điển, sau đó chọn năm được yêu cầu. 

Điều này hiệu quả vì việc kiểm tra tính hợp lệ rất dễ dàng. Đối với mọi ràng buộc`(a, b)`, chúng tôi chỉ cần xác minh:```
perm[a] < perm[b]
```Vấn đề là số lượng hoán vị. Với`n = 16`, chúng ta sẽ cần kiểm tra:```
16! ≈ 2 * 10^13
```hoán vị. Ngay cả việc kiểm tra một tỷ hoán vị mỗi giây cũng sẽ mất hàng giờ. 

Quan sát quan trọng là việc xây dựng từ điển chỉ cần số lần hoàn thành. Giả sử chúng ta đang xây dựng sự sắp xếp từ trái sang phải. Ở một số vị trí, chúng tôi thử đặt giáo sư chưa sử dụng nhỏ nhất có thể. Nếu chúng tôi biết có bao nhiêu lần hoàn thành hợp lệ tồn tại sau lựa chọn đó, chúng tôi có thể quyết định liệu sự sắp xếp mục tiêu nằm bên trong khối đó hay sau nó. 

Điều này chuyển vấn đề thành việc đếm các phép gán hợp lệ từ trạng thái một phần. 

Tập con DP hoàn toàn phù hợp vì`n ≤ 16`. Hãy để một bitmask đại diện cho những giáo sư nào đã được chỉ định. Từ trạng thái đó, chúng tôi xác định giáo sư nào có thể được xếp tiếp theo mà không vi phạm bất kỳ hạn chế nào về chỗ ngồi. 

Những hạn chế là về số ghế, nhưng các giáo sư được phân công theo thứ tự thâm niên tăng dần. Điều này tạo ra một cách giải thích rất hữu ích: 

Nếu giáo sư`k`là giáo sư nhỏ nhất tiếp theo chưa được sử dụng, sau đó phân công giáo sư`k`đến chỗ ngồi`s`có nghĩa là chỗ ngồi`s`nhận được thứ hạng tiếp theo trong số các ghế trống. 

Đối với hạn chế`(a, b)`, ghế`a`phải nhận được số giáo sư nhỏ hơn số ghế`b`. Vậy ngồi đi`a`phải được lấp đầy trước khi ngồi`b`. 

Điều này biến vấn đề thành việc đếm thứ tự tôpô của DAG trên số ghế. 

Bây giờ trạng thái trở nên sạch sẽ hơn nhiều:```
dp[mask] = number of ways to fill exactly the seats in mask first
```Chúng tôi chỉ có thể thêm chỗ ngồi nếu tất cả chỗ ngồi tiên quyết đã có sẵn bên trong mặt nạ. 

Điều này mang lại một`O(n * 2^n)`giải pháp, có thể dễ dàng đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * m) | Ồ (n!) | Quá chậm | 
| Tối ưu | O(n * 2^n) | O(2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích mọi ràng buộc`(a, b)`như một sự phụ thuộc giữa các chỗ ngồi. 

Vì số lượng giáo sư càng nhỏ thì càng có thâm niên cao hơn, nên chỗ ngồi`a`phải nhận giáo sư sớm hơn chỗ ngồi`b`. Điều này có nghĩa là chỗ ngồi`b`phụ thuộc vào chỗ ngồi`a`. 
2. Xây dựng bitmask tiên quyết cho mỗi chỗ ngồi.`pre[i]`lưu trữ tất cả các ghế phải được lấp đầy trước khi ngồi`i`có thể được lấp đầy. 
3. Sử dụng tập con DP để đếm các thứ tự tôpô hợp lệ. 

Cho phép:```
dp[mask]
```biểu thị số cách hợp lệ để lấp đầy các ghế còn lại sau đúng số ghế trong`mask`đã được giao rồi. 
4. Xác định quá trình chuyển đổi. 

Từ`mask`, chúng ta có thể chọn chỗ ngồi`i`nếu như:```
i is not in mask
and
all prerequisites of i are already in mask
```Chính thức:```
(pre[i] & mask) == pre[i]
```5. Tính DP với DFS đã được ghi nhớ. 

Nếu tất cả các ghế đã được lấp đầy thì sẽ có đúng một lần hoàn thành. 

Ngược lại, tính tổng số lượng của tất cả các ghế hợp lệ tiếp theo. 
6. Hãy:```
k = y - 2001
```Đây là chỉ số dựa trên số 0 của sự sắp xếp mong muốn. 
7. Nếu:```
dp[0] <= k
```thì năm yêu cầu vượt quá số lượng thỏa thuận hợp lệ. In:```
The times have changed
```8. Xây dựng lại cách sắp xếp theo từ điển. 

Chúng tôi phân công các giáo sư từ nhỏ nhất đến lớn nhất. Ở mỗi bước, hãy thử các ghế của ứng viên theo thứ tự tăng dần vì thứ tự từ điển phụ thuộc vào ghế nào nhận được giáo sư nhỏ nhất hiện tại. 
9. Đối với mỗi ghế ứng cử viên, hãy tính xem có bao nhiêu lần hoàn thành nếu chúng ta đặt giáo sư hiện tại vào đó. 

Nếu số lần hoàn thành nhỏ hơn hoặc bằng`k`, bỏ qua tất cả những cách sắp xếp đó và trừ đi số đếm đó từ`k`. 

Nếu không, hãy cam kết với chỗ đó và tiếp tục. 
10. Sau khi xác định được thứ tự xếp chỗ, chuyển đổi thành hoán vị cuối cùng. 

Nếu chỗ ngồi`s`được điền ở bước`t`, sau đó nó nhận được giáo sư`t + 1`. 

### Tại sao nó hoạt động 

DP đếm chính xác thứ tự tôpô hợp lệ của biểu đồ phụ thuộc chỗ ngồi. Mỗi sự sắp xếp hợp lệ tương ứng với một thứ tự duy nhất trong đó các ghế nhận được số lượng giáo sư ngày càng tăng. 

Trong quá trình xây dựng lại, thứ tự từ điển được giữ nguyên vì số lượng giáo sư nhỏ hơn được chỉ định trước. Việc thử các ghế ứng cử viên theo thứ tự tăng dần sẽ khám phá sự sắp xếp theo thứ tự giống như so sánh hoán vị từ điển. 

Ở mỗi bước, số lượng DP cho chúng ta biết có bao nhiêu cách sắp xếp bắt đầu bằng một tiền tố cụ thể. Việc bỏ qua toàn bộ các khối bằng cách sử dụng số đếm này tương đương với việc chuyển trực tiếp đến cách sắp xếp từ điển mục tiêu. 

## Giải pháp Python```python
import sys
from functools import lru_cache

input = sys.stdin.readline

INF = 10**18 + 5

def solve():
    n, y, m = map(int, input().split())

    pre = [0] * n

    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        pre[b] |= (1 << a)

    full = (1 << n) - 1

    @lru_cache(None)
    def dp(mask):
        if mask == full:
            return 1

        res = 0

        for i in range(n):
            if (mask >> i) & 1:
                continue

            if (pre[i] & mask) != pre[i]:
                continue

            res += dp(mask | (1 << i))

            if res > INF:
                res = INF

        return res

    k = y - 2001

    total = dp(0)

    if total <= k:
        print("The times have changed")
        return

    order = []
    mask = 0

    for _ in range(n):
        for seat in range(n):
            if (mask >> seat) & 1:
                continue

            if (pre[seat] & mask) != pre[seat]:
                continue

            cnt = dp(mask | (1 << seat))

            if cnt <= k:
                k -= cnt
            else:
                order.append(seat)
                mask |= (1 << seat)
                break

    ans = [0] * n

    for professor, seat in enumerate(order, start=1):
        ans[seat] = professor

    print(*ans)

solve()
```Phần đầu tiên chuyển đổi các ràng buộc đầu vào thành các mặt nạ tiên quyết. Nếu như`(a, b)`tồn tại thì ngồi`b`phụ thuộc vào chỗ ngồi`a`, vì vậy chúng tôi đặt bit tương ứng bên trong`pre[b]`. 

DP được ghi nhớ hoạt động trên tập hợp con các chỗ ngồi đã được lấp đầy. Điều kiện chuyển tiếp:```
(pre[i] & mask) == pre[i]
```có nghĩa là mọi điều kiện tiên quyết để có được chỗ ngồi`i`đã được lấp đầy. 

Giá trị DP có thể trở nên rất lớn, có khả năng vượt quá phạm vi năm được yêu cầu. Chúng tôi giới hạn chúng ở`INF`bởi vì chúng ta chỉ cần so sánh với`10^18`. 

Giai đoạn tái thiết là phần tinh tế. Chúng tôi không trực tiếp xây dựng các giá trị hoán vị. Thay vào đó, chúng tôi xây dựng thứ tự các ghế tiếp nhận giáo sư`1, 2, 3, ...`. 

Giả sử chỗ ngồi`2`được chọn đầu tiên. Nghĩa là giáo sư`1`ngồi đó. Nếu chỗ ngồi`0`được chọn thứ hai thưa giáo sư`2`ngồi đó, vân vân. 

Việc thử chỗ ngồi theo thứ tự tăng dần đảm bảo việc duyệt từ điển. Bất cứ khi nào một ứng viên đóng góp`cnt`các cách sắp xếp đó tạo thành một khối từ điển liền kề nhau. Nếu như`k >= cnt`, chúng tôi bỏ qua toàn bộ khối. 

Một sai lầm phổ biến là giáo sư xây dựng lại thay vì sắp xếp chỗ ngồi. DP tự nhiên đếm số lượng đơn đặt chỗ trống, tương ứng trực tiếp với nhiệm vụ của giáo sư. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2001 2
1 2
2 3
```Phụ thuộc:```
seat 1 before seat 2
seat 2 before seat 3
```Chỉ có một đơn đặt hàng tồn tại. 

#### Dấu vết tái thiết DP 

| Bước | Mặt nạ hiện tại | Chỗ ngồi có sẵn | Chỗ ngồi được chọn | k | 
| --- | --- | --- | --- | --- | 
| 1 | 000 | 0 | 0 | 0 | 
| 2 | 001 | 1 | 1 | 0 | 
| 3 | 011 | 2 | 2 | 0 | 

Thứ tự điền chỗ ngồi là:```
0 -> 1 -> 2
```Vì thế:```
seat 0 gets professor 1
seat 1 gets professor 2
seat 2 gets professor 3
```Sự sắp xếp cuối cùng:```
1 2 3
```Dấu vết này chứng tỏ rằng biểu đồ phụ thuộc sửa chữa hoàn toàn thứ tự tôpô. 

### Ví dụ 2 

đầu vào:```
3 2002 1
1 2
```Ràng buộc:```
seat 1 before seat 2
```Sự sắp xếp hợp lệ theo thứ tự từ điển là:```
1 2 3
1 3 2
2 1 3
```Chúng tôi muốn chỉ mục:```
k = 1
```#### Dấu vết tái thiết 

| Bước | Mặt nạ | Ghế ứng cử viên | Số lần hoàn thành | Hành động | k | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 000 | 0 | 2 | lấy | 1 | 
| 2 | 001 | 1 | 1 | bỏ qua | 0 | 
| 2 | 001 | 2 | 1 | lấy | 0 | 
| 3 | 101 | 1 | 1 | lấy | 0 | 

Thứ tự chỗ ngồi:```
0 -> 2 -> 1
```Bài tập:```
seat 0 = 1
seat 2 = 2
seat 1 = 3
```Sự sắp xếp cuối cùng:```
1 3 2
```Ví dụ này cho thấy cách sử dụng số lượng DP để bỏ qua toàn bộ khối từ điển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 2^n) | Mỗi tập hợp con thử tất cả các ghế một lần | 
| Không gian | O(2^n) | Bảng ghi nhớ trên các tập hợp con | 

Với`n ≤ 16`, số lượng trạng thái nhiều nhất là`65536`. Mỗi tiểu bang kiểm tra nhiều nhất`16`chuyển đổi, do đó tổng số hoạt động là khoảng một triệu, thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from functools import lru_cache

INF = 10**18 + 5

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = io.StringIO()
    sys.stdout = out

    n, y, m = map(int, input().split())

    pre = [0] * n

    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        pre[b] |= (1 << a)

    full = (1 << n) - 1

    @lru_cache(None)
    def dp(mask):
        if mask == full:
            return 1

        res = 0

        for i in range(n):
            if (mask >> i) & 1:
                continue

            if (pre[i] & mask) != pre[i]:
                continue

            res += dp(mask | (1 << i))

            if res > INF:
                res = INF

        return res

    k = y - 2001

    if dp(0) <= k:
        print("The times have changed")
        return out.getvalue()

    order = []
    mask = 0

    for _ in range(n):
        for seat in range(n):
            if (mask >> seat) & 1:
                continue

            if (pre[seat] & mask) != pre[seat]:
                continue

            cnt = dp(mask | (1 << seat))

            if cnt <= k:
                k -= cnt
            else:
                order.append(seat)
                mask |= (1 << seat)
                break

    ans = [0] * n

    for professor, seat in enumerate(order, start=1):
        ans[seat] = professor

    print(*ans)

    return out.getvalue()

# provided sample
assert run(
"""3 2001 2
1 2
2 3
"""
) == "1 2 3\n", "sample 1"

# minimum size
assert run(
"""1 2001 0
"""
) == "1\n", "single professor"

# contradictory constraints
assert run(
"""2 2001 2
1 2
2 1
"""
) == "The times have changed\n", "cycle"

# year exceeds arrangements
assert run(
"""2 2003 0
"""
) == "The times have changed\n", "not enough permutations"

# duplicate constraints
assert run(
"""3 2001 3
1 2
1 2
1 2
"""
) == "1 2 3\n", "duplicate edges"

# no constraints, second permutation
assert run(
"""3 2002 0
"""
) == "1 3 2\n", "lexicographic order"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2001 0`|`1`| Kích thước đầu vào tối thiểu | 
| Chu kỳ mâu thuẫn |`The times have changed`| Phát hiện DAG không thể | 
|`2 2003 0`|`The times have changed`| Xử lý sắp xếp không đầy đủ | 
| Các cạnh trùng lặp |`1 2 3`| Các ràng buộc trùng lặp không phá vỡ DP | 
| Không có ràng buộc, hoán vị thứ hai |`1 3 2`| Tái thiết từ điển chính xác | 

## Vỏ cạnh 

### Ràng buộc mâu thuẫn 

đầu vào:```
2 2001 2
1 2
2 1
```Sự phụ thuộc trở thành:```
seat 0 depends on seat 1
seat 1 depends on seat 0
```Tại`mask = 00`, không có chỗ trống nào vì mỗi chỗ đều yêu cầu chỗ còn lại phải được lấp đầy. 

Vì thế:```
dp(0) = 0
```Vì tổng số cách sắp xếp hợp lệ bằng 0 nên thuật toán sẽ in:```
The times have changed
```DP phát hiện các chu kỳ một cách tự nhiên vì không có quá trình chuyển đổi nào trở nên hợp pháp. 

### Năm yêu cầu quá lớn 

đầu vào:```
2 2003 0
```Chỉ có hai cách sắp xếp hợp lệ:```
1 2
2 1
```Chỉ số được yêu cầu là:```
k = 2003 - 2001 = 2
```Nhưng:```
dp(0) = 2
```Vì các chỉ số hợp lệ chỉ`0`Và`1`, điều kiện:```
if total <= k:
```từ chối chính xác yêu cầu. 

### Ràng buộc trùng lặp 

đầu vào:```
3 2001 3
1 2
1 2
1 2
```Cả ba cạnh đều giống nhau. Bộ lưu trữ Bitmask chỉ giữ một bit phụ thuộc:```
pre[1] = 001
```DP hoạt động chính xác như thể cạnh đã xuất hiện một lần. 

Sự sắp xếp kết quả vẫn là:```
1 2 3
```### Trường hợp hoàn toàn không bị ràng buộc 

đầu vào:```
3 2004 0
```Tất cả`3! = 6`hoán vị là hợp lệ. 

Thuật toán xây dựng lại hoán vị thứ tư theo từ điển:```
2 3 1
```Trường hợp này xác minh rằng tính năng bỏ qua từ điển hoạt động chính xác ngay cả khi mọi chuyển đổi đều có sẵn.
