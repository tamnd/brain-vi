---
title: "CF 452F - Hoán vị"
description: "Chúng ta được cho một hoán vị của các số 1...n. Đối với hai giá trị a và b phân biệt, giá trị trung bình của chúng là (a+b)/2. Vì hoán vị chứa mỗi số nguyên đúng một lần nên giá trị trung bình này cũng xuất hiện bất cứ khi nào a+b chẵn."
date: "2026-06-07T17:10:09+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "hashing"]
categories: ["algorithms"]
codeforces_contest: 452
codeforces_index: "F"
codeforces_contest_name: "MemSQL Start[c]UP 2.0 - Round 1"
rating: 2700
weight: 452
solve_time_s: 174
verified: true
draft: false
---

[CF 452F - Hoán vị](https://codeforces.com/problemset/problem/452/F) 

**Đánh giá:** 2700 
**Thẻ:** cấu trúc dữ liệu, phân chia để chinh phục, băm 
**Thời gian giải:** 2m 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị của các số`1...n`. Cho hai giá trị phân biệt`a`Và`b`, trung bình của họ là`(a+b)/2`. Vì hoán vị chứa mọi số nguyên đúng một lần nên giá trị trung bình này cũng xuất hiện bất cứ khi nào`a+b`là chẵn. 

Câu hỏi đặt ra là liệu có tồn tại một cặp`a, b`sao cho giá trị trung bình của chúng xuất hiện ở đâu đó giữa chúng theo thứ tự hoán vị. 

Một cách cải cách hữu ích hơn là xem xét cấp số cộng trong không gian giá trị. Cho phép$$a = x-k,\qquad b = x+k.$$Sau đó`x`chính xác là trung bình của`a`Và`b`. Nhiệm vụ trở thành: 

Liệu có tồn tại một giá trị`x`và một khoảng cách`k > 0`sao cho vị trí của`x`trong hoán vị nằm chặt chẽ giữa các vị trí của`x-k`Và`x+k`? 

Kích thước của hoán vị có thể đạt tới`300000`. Bất kỳ thuật toán nào kiểm tra rõ ràng tất cả các cặp hoặc tất cả cấp số cộng đều bị loại trừ ngay lập tức. Thậm chí một`O(n^2)`giải pháp sẽ yêu cầu khoảng`9·10^{10}`trong trường hợp xấu nhất vượt xa giới hạn. 

Phần khó khăn là điều kiện trộn lẫn hai thứ tự khác nhau. Các giá trị được so sánh theo thứ tự lũy tiến số học, trong khi "giữa" đề cập đến các vị trí bên trong hoán vị. 

Hãy xem xét hoán vị```
1 5 2 4 3
```giá trị`2`nằm giữa`1`Và`3`, vậy câu trả lời là`YES`. 

Bây giờ hãy xem xét```
1 3 4 2
```Đối với sự tiến triển duy nhất có thể`(1,2,3)`, giá trị`2`không ở giữa`1`Và`3`. Câu trả lời là`NO`. 

Một lỗi phổ biến là chỉ kiểm tra các giá trị liền kề hoặc chỉ cấp số có độ dài bằng ba theo thứ tự hoán vị. Cấp số cộng được xác định trên chính các giá trị chứ không phải trên các vị trí lân cận. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Với mọi giá trị trung tâm có thể`x`, thử mọi khoảng cách`k`như vậy`x-k`Và`x+k`vẫn ở bên trong`1...n`. Sử dụng vị trí được lưu trữ của tất cả các giá trị, chúng ta có thể kiểm tra xem vị trí của`x`nằm giữa vị trí của hai điểm cuối. 

Điều này đúng vì mọi câu trả lời hợp lệ đều tương ứng với đúng một trung tâm`x`và một khoảng cách`k`. Vấn đề là thời gian chạy. có`Θ(n^2)`cặp có thể`(x,k)`, quá lớn đối với`n = 300000`. 

Quan sát chính xuất hiện khi chúng ta xử lý hoán vị từ trái sang phải. 

Giả sử chúng ta hiện đang truy cập giá trị`x`. 

Mọi giá trị đã thấy đều nằm ở bên trái của`x`trong hoán vị. Mọi giá trị chưa được nhìn thấy đều nằm ở bên phải của`x`. 

Xác định một mảng nhị phân trên trục giá trị:```
0 = already processed
1 = not processed yet
```Khi chúng tôi đến`x`, chúng tôi thay đổi trạng thái của giá trị`x`từ`1`ĐẾN`0`. 

Bây giờ hãy xem một cặp giá trị đối xứng:```
x-k , x+k
```Nếu trạng thái của chúng khác nhau thì một trong số chúng đã được xử lý còn trạng thái còn lại thì không. Điều đó có nghĩa là một điểm cuối nằm ở bên trái của`x`và cái còn lại nằm bên phải`x`. Kể từ đây`x`nằm giữa chúng trong hoán vị, và chúng ta đã tìm thấy một cấp số cộng hợp lệ. 

Vì vậy với mỗi giá trị được xử lý`x`, chúng ta chỉ cần biết liệu chuỗi nhị phân xung quanh`x`

```
[x-t ... x+t]
```với```
t = min(x-1, n-x)
```đối xứng quanh tâm. 

Nếu dãy đối xứng hoàn toàn thì mỗi cặp`(x-k, x+k)`có trạng thái bằng nhau. 

Nếu chuỗi không đối xứng, một số cặp có trạng thái khác nhau, ngay lập tức đưa ra câu trả lời hợp lệ. 

Kiểm tra tính đối xứng của một phạm vi là một nhiệm vụ băm cuộn cổ điển. Chúng tôi duy trì mảng nhị phân trong cây phân đoạn lưu trữ cả giá trị băm thuận và ngược. Sau khi cập nhật một vị trí, chúng tôi truy vấn khoảng đối xứng lớn nhất xung quanh`x`và kiểm tra xem hàm băm tiến của nó có bằng hàm băm ngược hay không. 

Khoảng không palindromic đầu tiên chứng minh câu trả lời là`YES`. Nếu mọi khoảng đều có tính chất palindrome thì câu trả lời là`NO`. Đây là dự định`O(n log n)`giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu (cây phân đoạn + băm) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo mảng nhị phân trên các giá trị`1...n`. 

Ban đầu mỗi mục là`1`, nghĩa là giá trị đó chưa xuất hiện trong hoán vị. 
2. Xây dựng cây phân đoạn trên mảng này. 

Mỗi nút lưu trữ độ dài của phân đoạn, hàm băm cuộn tiến và hàm băm cuộn ngược của nó. 
3. Xử lý hoán vị từ trái sang phải. 

Khi giá trị`x`được truy cập, cập nhật vị trí`x`trong cây phân đoạn từ`1`ĐẾN`0`. 
4. Hãy để$$t=\min(x-1,n-x).$$Đây là khoảng cách lớn nhất mà cả hai`x-t`Và`x+t`hiện hữu. 
5. Truy vấn khoảng$$[x-t,\;x+t].$$Lấy hàm băm thuận và hàm băm ngược của nó. 
6. Nếu các giá trị băm khác nhau thì khoảng đó không phải là một bảng màu. 

Khi đó tồn tại một khoảng cách nào đó`k`sao cho các trạng thái của`x-k`Và`x+k`là khác nhau. 
7. Các trạng thái khác nhau có nghĩa là một điểm cuối đã được xử lý còn điểm cuối kia thì không. 

Một điểm cuối nằm ở phía bên trái của`x`trong hoán vị và cái còn lại ở bên phải. 
8. Giá trị`x`nằm giữa hai điểm cuối, do đó tồn tại một cặp hợp lệ. 

In`"YES"`và dừng lại. 
9. Nếu tất cả các giá trị được xử lý mà không tìm thấy khoảng như vậy, hãy in`"NO"`. 

### Tại sao nó hoạt động 

Tại thời điểm giá trị`x`được xử lý, mọi giá trị được đánh dấu`0`xuất hiện trước`x`trong hoán vị và mọi giá trị được đánh dấu`1`xuất hiện sau`x`. 

Cho bất kỳ khoảng cách`k`, cặp`(x-k, x+k)`đóng góp một tiến trình số học hợp lệ chính xác khi trạng thái của chúng khác nhau. Một điểm cuối nằm trước`x`và cái khác sau`x`, đặt`x`giữa họ. 

Khoảng đối xứng xung quanh`x`là một palindrome khi và chỉ nếu mọi cặp đối xứng đều có trạng thái bằng nhau. Một khoảng không palindromic tương đương với sự tồn tại của một cặp có các trạng thái khác nhau, tương đương với sự tồn tại của một cấp số cộng hợp lệ có tâm tại`x`. 

Thuật toán kiểm tra điều kiện này để tìm mọi giá trị trung tâm có thể có, do đó, thuật toán sẽ tìm thấy câu trả lời bất cứ khi nào có giá trị đó và không bao giờ báo cáo kết quả dương tính giả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MASK = (1 << 64) - 1
BASE = 911382323

def solve():
    n = int(input())
    p = list(map(int, input().split()))

    powb = [1] * (n + 1)
    for i in range(1, n + 1):
        powb[i] = (powb[i - 1] * BASE) & MASK

    size = 1
    while size < n:
        size <<= 1

    seg_len = [0] * (2 * size)
    seg_f = [0] * (2 * size)
    seg_r = [0] * (2 * size)

    for i in range(size):
        idx = size + i
        seg_len[idx] = 1
        if i < n:
            seg_f[idx] = 1
            seg_r[idx] = 1

    for v in range(size - 1, 0, -1):
        l = v * 2
        r = l + 1

        len_l = seg_len[l]
        len_r = seg_len[r]

        seg_len[v] = len_l + len_r

        seg_f[v] = (
            seg_f[l] + (seg_f[r] * powb[len_l])
        ) & MASK

        seg_r[v] = (
            seg_r[r] + (seg_r[l] * powb[len_r])
        ) & MASK

    def update(pos):
        v = size + pos - 1
        seg_f[v] = 0
        seg_r[v] = 0

        v //= 2
        while v:
            l = v * 2
            r = l + 1

            len_l = seg_len[l]
            len_r = seg_len[r]

            seg_f[v] = (
                seg_f[l] + (seg_f[r] * powb[len_l])
            ) & MASK

            seg_r[v] = (
                seg_r[r] + (seg_r[l] * powb[len_r])
            ) & MASK

            v //= 2

    def combine(a, b):
        len_a, f_a, r_a = a
        len_b, f_b, r_b = b

        return (
            len_a + len_b,
            (f_a + f_b * powb[len_a]) & MASK,
            (r_b + r_a * powb[len_b]) & MASK,
        )

    def query(l, r):
        l = l + size - 1
        r = r + size - 1

        left = (0, 0, 0)
        right = (0, 0, 0)

        while l <= r:
            if l & 1:
                left = combine(left, (seg_len[l], seg_f[l], seg_r[l]))
                l += 1

            if not (r & 1):
                right = combine((seg_len[r], seg_f[r], seg_r[r]), right)
                r -= 1

            l //= 2
            r //= 2

        return combine(left, right)

    for x in p:
        update(x)

        t = min(x - 1, n - x)
        if t == 0:
            continue

        length, fh, rh = query(x - t, x + t)

        if fh != rh:
            print("YES")
            return

    print("NO")

solve()
```Cây phân đoạn lưu trữ hàm băm luân phiên cho mỗi khoảng thời gian theo cả hai hướng. Truy vấn phạm vi trả về hàm băm thuận và hàm băm ngược của khoảng được yêu cầu. Bình đẳng có nghĩa là khoảng là một palindrome. 

Thao tác cập nhật thay đổi một giá trị từ`1`ĐẾN`0`. Điều này tương ứng chính xác với việc di chuyển một giá trị từ tập hợp "chưa nhìn thấy" sang tập hợp "đã được xử lý". 

Khoảng đối xứng lớn nhất xung quanh`x`được sử dụng vì mọi cặp có thể`(x-k, x+k)`phải được kiểm tra. Bất kỳ sự không khớp nào ở bất kỳ đâu trong khoảng đó đều tạo ra cấp số cộng cần thiết. 

Một điểm tinh tế là thứ tự thực hiện các thao tác. Chúng ta phải cập nhật`x`trước khi kiểm tra khoảng thời gian. Tại thời điểm kiểm tra,`x`chính nó thuộc về phía được xử lý, nó khớp với bất biến mà mọi`0`tương ứng với một giá trị xuất hiện không muộn hơn vị trí hiện tại. 

Việc sử dụng hàm băm 64 bit giúp quá trình triển khai trở nên nhỏ gọn và nhanh chóng. Về mặt lý thuyết có thể xảy ra va chạm nhưng trên thực tế không đáng kể đối với lập trình cạnh tranh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 3 4 2
```| Giá trị hiện tại x | Trạng thái nhị phân sau khi cập nhật (giá trị 1..4) | Khoảng thời gian truy vấn | Xuôi ngược đều giống nhau? | 
| --- | --- | --- | --- | 
| 1 | 0 1 1 1 | không | - | 
| 3 | 0 1 0 1 | [2,4] = 1 0 1 | Có | 
| 4 | 0 1 0 0 | không | - | 
| 2 | 0 0 0 0 | [1,3] = 0 0 0 | Có | 

Không có khoảng truy vấn nào là không palindromic, do đó không có giá trị trung tâm nào có điểm cuối ở các phía đối diện. Câu trả lời là`NO`. 

### Ví dụ 2 

đầu vào:```
5
1 5 2 4 3
```| Giá trị hiện tại x | Trạng thái nhị phân sau khi cập nhật (giá trị 1..5) | Khoảng thời gian truy vấn | Xuôi ngược đều giống nhau? | 
| --- | --- | --- | --- | 
| 1 | 0 1 1 1 1 | không | - | 
| 5 | 0 1 1 1 0 | không | - | 
| 2 | 0 0 1 1 0 | [1,3] = 0 0 1 | Không | 

Vì`x = 2`, cặp đối xứng`(1,3)`có tiểu bang`(0,1)`. Giá trị`1`còn lại của`2`trong hoán vị và giá trị`3`là đúng`2`. Như vậy`2`nằm giữa họ và câu trả lời là`YES`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Một bản cập nhật và một truy vấn phạm vi cho mỗi phần tử hoán vị | 
| Không gian | O(n) | Cây phân đoạn và mảng công suất | 

Với`n = 300000`, MỘT`O(n log n)`Thuật toán chỉ thực hiện vài triệu thao tác trên cây phân đoạn, dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    MASK = (1 << 64) - 1
    BASE = 911382323

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    p = list(map(int, input().split()))

    powb = [1] * (n + 1)
    for i in range(1, n + 1):
        powb[i] = (powb[i - 1] * BASE) & MASK

    size = 1
    while size < n:
        size <<= 1

    seg_len = [0] * (2 * size)
    seg_f = [0] * (2 * size)
    seg_r = [0] * (2 * size)

    for i in range(size):
        idx = size + i
        seg_len[idx] = 1
        if i < n:
            seg_f[idx] = 1
            seg_r[idx] = 1

    for v in range(size - 1, 0, -1):
        l = v * 2
        r = l + 1
        seg_len[v] = seg_len[l] + seg_len[r]
        seg_f[v] = (seg_f[l] + seg_f[r] * powb[seg_len[l]]) & MASK
        seg_r[v] = (seg_r[r] + seg_r[l] * powb[seg_len[r]]) & MASK

    def update(pos):
        v = size + pos - 1
        seg_f[v] = seg_r[v] = 0
        v //= 2
        while v:
            l = v * 2
            r = l + 1
            seg_f[v] = (seg_f[l] + seg_f[r] * powb[seg_len[l]]) & MASK
            seg_r[v] = (seg_r[r] + seg_r[l] * powb[seg_len[r]]) & MASK
            v //= 2

    def combine(a, b):
        la, fa, ra = a
        lb, fb, rb = b
        return (
            la + lb,
            (fa + fb * powb[la]) & MASK,
            (rb + ra * powb[lb]) & MASK,
        )

    def query(l, r):
        l = l + size - 1
        r = r + size - 1
        left = (0, 0, 0)
        right = (0, 0, 0)

        while l <= r:
            if l & 1:
                left = combine(left, (seg_len[l], seg_f[l], seg_r[l]))
                l += 1
            if not (r & 1):
                right = combine((seg_len[r], seg_f[r], seg_r[r]), right)
                r -= 1
            l //= 2
            r //= 2

        return combine(left, right)

    for x in p:
        update(x)
        t = min(x - 1, n - x)
        if t:
            _, fh, rh = query(x - t, x + t)
            if fh != rh:
                return "YES"

    return "NO"

# provided samples
assert run("4\n1 3 4 2\n") == "NO", "sample 1"
assert run("5\n1 5 2 4 3\n") == "YES", "sample 2"

# custom cases
assert run("1\n1\n") == "NO", "minimum size"
assert run("3\n1 2 3\n") == "YES", "simple progression"
assert run("3\n1 3 2\n") == "NO", "small negative case"
assert run("5\n5 4 3 2 1\n") == "YES", "reversed permutation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`NO`| Kích thước tối thiểu | 
|`3 / 1 2 3`|`YES`| Ví dụ tích cực nhỏ nhất | 
|`3 / 1 3 2`|`NO`| Ví dụ tiêu cực nhỏ nhất | 
|`5 / 5 4 3 2 1`|`YES`| Điểm cuối ở phía đối diện mặc dù thứ tự đảo ngược | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
1
```Không có cặp giá trị phân biệt nào tồn tại. Thuật toán thực hiện một lần cập nhật, tìm`t = 0`, không bao giờ truy vấn một phạm vi và in chính xác`NO`. 

Coi như```
3
1 3 2
```Cấp số cộng duy nhất về giá trị là`(1,2,3)`. Sau khi xử lý`2`, cả hai`1`Và`3`đã được nhìn thấy nên trạng thái của chúng bằng nhau. Khoảng thời gian`[1,3]`là palindromic và kết quả đầu ra của thuật toán`NO`. 

Coi như```
3
1 2 3
```Khi`2`được xử lý, giá trị`1`có trạng thái`0`và giá trị`3`có trạng thái`1`. Khoảng thời gian`[1,3]`trở nên không palindrome. Thuật toán báo cáo ngay lập tức`YES`, phù hợp với thực tế rằng`2`nằm giữa`1`Và`3`. 

Cuối cùng, hãy xem xét một trung tâm gần ranh giới, chẳng hạn như giá trị`1`hoặc giá trị`n`. Sau đó`t = 0`, do đó không có cặp đối xứng hợp lệ. Thuật toán bỏ qua hoàn toàn truy vấn, tránh từng lỗi một và xử lý chính xác các cạnh của phạm vi giá trị.
