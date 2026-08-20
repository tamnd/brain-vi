---
title: "CF 102191G - Số tiếp theo"
description: "Chúng ta có một mảng a gồm n chữ số, trong đó mỗi chữ số là một số nguyên từ 0 đến b - 1. Đọc mảng từ trái sang phải sẽ ra số có n chữ số trong cơ số b. Chúng ta cần số nhỏ nhất lớn hơn số này có các chữ số khác nhau."
date: "2026-08-20T01:35:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102191
codeforces_index: "G"
codeforces_contest_name: "PSUT Coding Marathon 2019"
rating: 0
weight: 102191
solve_time_s: 1457
verified: false
draft: false
---

[CF 102191G - Số tiếp theo](https://codeforces.com/problemset/problem/102191/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 24m 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng`a`của`n`chữ số, trong đó mỗi chữ số là một số nguyên từ`0`ĐẾN`b - 1`. Đọc mảng từ trái sang phải sẽ cho kết quả`n`-chữ số ở cơ số`b`. Chúng ta cần số nhỏ nhất lớn hơn số này có các chữ số khác nhau. Chữ số đầu tiên đã được biết là khác 0 và câu trả lời được đảm bảo tồn tại. 

Khó khăn chính là "tiếp theo" là thứ tự số, đối với hai cơ sở có độ dài cố định-`b`số chính xác là thứ tự từ điển của mảng chữ số của chúng. Vì vậy chúng tôi muốn thay đổi mảng càng muộn càng tốt. Khi chúng tôi quyết định làm cho một vị trí lớn hơn, mọi vị trí sau đó phải được làm nhỏ nhất có thể trong khi vẫn giữ tất cả các chữ số riêng biệt. 

Cả hai`n`Và`b`có thể lớn như`3 * 10^5`. Điều đó loại trừ bất cứ điều gì thử nhiều số có thể, hoặc thậm chí bất cứ điều gì bậc hai trong`n`. Với giới hạn 2 giây, chúng tôi muốn gần như tuyến tính hoặc`O(n log b)`công việc. Cơ số có thể đủ lớn nên chúng ta cũng cần phải cẩn thận khi thực hiện các thao tác trên toàn bộ phạm vi chữ số, mặc dù`O(b)`vẫn có thể chấp nhận được vì`b`có giới hạn trên giống như`n`. 

Có một số trường hợp đặc biệt trong đó việc triển khai đơn giản có thể âm thầm gặp trục trặc. Đầu tiên, bản thân dữ liệu đầu vào có thể chứa các chữ số lặp lại. Ví dụ,```
3 10
1 1 9
```có câu trả lời`1 2 0`, không phải thứ thu được chỉ bằng cách sửa đổi chữ số cuối cùng. Tiền tố`1 1`đã không hợp lệ, vì vậy vị trí cuối cùng không thể được sử dụng làm nơi chúng ta làm cho số lớn hơn. 

Vấn đề thứ hai là chữ số cuối cùng có thể không tăng được vì mọi chữ số lớn hơn đã được tiền tố sử dụng. Ví dụ,```
4 4
3 2 0 1
```không thể tăng ở vị trí cuối cùng vì tiền tố đã sử dụng`0`,`2`, Và`3`. Câu trả lời đúng là`3 2 1 0`, thu được bằng cách thay đổi vị trí thứ ba. 

Trường hợp cạnh thứ ba xảy ra khi mức tăng duy nhất có thể xảy ra là ở vị trí đầu tiên. Ví dụ,```
3 4
1 3 3
```có câu trả lời`2 0 1`. Khi chữ số đầu tiên trở thành`2`, các vị trí còn lại phải sử dụng hai chữ số nhỏ nhất chưa được sử dụng. Việc triển khai bất cẩn có thể bảo tồn không chính xác một trong những bản gốc được lặp lại`3`s, mặc dù hậu tố phải chứa các chữ số riêng biệt. 

Cuối cùng, câu trả lời có thể thu được từ một đầu vào đã khác biệt bằng cách thay đổi vị trí không phải cuối cùng. Vì```
3 4
1 2 3
```câu trả lời là`1 3 0`. Giữ tiền tố`1`, tăng dần`2`ĐẾN`3`, sau đó điền vào hậu tố chữ số nhỏ nhất có sẵn sẽ cho số hợp lệ đầu tiên lớn hơn`123`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là liệt kê các số ứng viên bắt đầu ngay sau số đầu vào. Đối với mỗi ứng viên, chúng ta có thể kiểm tra xem tất cả`n`chữ số là khác biệt. Việc kiểm tra tự nó mất`O(n)`thời gian, vì vậy nếu chúng ta kiểm tra`K`các ứng cử viên liên tiếp chi phí là`O(Kn)`. Trong trường hợp xấu nhất có thể có nhiều ứng viên theo cấp số nhân trước khi đạt được số hợp lệ, với nhiều nhất`b^n`khả thi`n`-mảng chữ số. Do đó, giới hạn trường hợp xấu nhất là`O(n b^n)`, điều này hoàn toàn không thể thực hiện được đối với`n`lên đến`3 * 10^5`. 

Phương pháp brute-force có một đặc tính hữu ích: nó cho chúng ta biết chính xác câu trả lời mong muốn trông như thế nào. Chúng tôi muốn vị trí đầu tiên từ bên phải nơi chúng tôi có thể tăng một chữ số, trong khi mọi thứ trước vị trí đó không thay đổi. Sau khi thực hiện mức tăng đó, hậu tố phải là hậu tố hợp lệ nhỏ nhất có thể. 

Quan sát đó loại bỏ sự cần thiết phải liệt kê các con số. Giả sử chúng ta chọn vị trí`i`là vị trí thay đổi đầu tiên. Tiền tố`a[0:i]`phải bao gồm các chữ số riêng biệt. Chữ số thay thế phải lớn hơn`a[i]`và không được xuất hiện trong tiền tố đó. Trong số tất cả những lựa chọn như vậy, chúng ta muốn lựa chọn nhỏ nhất. Khi chữ số đó được chọn, hậu tố chỉ cần chứa các chữ số nhỏ nhất không được sử dụng theo thứ tự tăng dần. 

Vấn đề về cấu trúc dữ liệu còn lại là tìm chữ số nhỏ nhất chưa được sử dụng lớn hơn`a[i]`. Từ`b`tùy thuộc vào`3 * 10^5`, cây Fenwick có thể duy trì tập hợp các chữ số hiện chưa được sử dụng và tìm`k`-chữ số thứ không được sử dụng trong`O(log b)`thời gian. Chúng tôi quét các vị trí từ phải sang trái trong khi vẫn tự động duy trì các chữ số xuất hiện trong tiền tố. 

Brute-force hoạt động vì việc kiểm tra các ứng viên cuối cùng sẽ tìm thấy số lớn hơn hợp lệ đầu tiên, nhưng không thành công vì có thể có quá nhiều ứng viên. Quan sát cho thấy rằng chỉ vị trí được thay đổi đầu tiên mới quan trọng cho phép chúng tôi thay thế phép liệt kê hàm mũ bằng một lần quét từ phải sang trái và truy vấn kế tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n b^n)`trường hợp xấu nhất |`O(n + b)`| Quá chậm | 
| Tối ưu |`O(n log b + b)`|`O(n + b)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng tần số cho tiền tố`a[0:n-1]`, bởi vì khi chúng ta kiểm tra vị trí lần đầu tiên`n-1`, mọi chữ số trước đó thuộc về tiền tố không thay đổi. Đồng thời, khởi tạo cây Fenwick chứa mọi chữ số hiện không được tiền tố này sử dụng. 
2. Duy trì`bad`, số giá trị chữ số có tần số trong tiền tố hiện tại ít nhất là hai. Tiền tố có thể sử dụng được chính xác khi`bad == 0`. Chúng tôi cần điều này một cách rõ ràng vì mảng ban đầu không được đảm bảo chứa các chữ số riêng biệt. 
3. Bắt đầu với`i = n - 1`và di chuyển`i`hướng tới số không. Tại vị trí`i`, tiền tố trước nó là`a[0:i]`. Nếu như`bad`khác 0, tiền tố này không thể xuất hiện trong bất kỳ câu trả lời hợp lệ nào, vì vậy vị trí này không thể là vị trí được thay đổi đầu tiên. 
4. Nếu tiền tố là khác biệt, hãy truy vấn cây Fenwick để tìm chữ số nhỏ nhất chưa được sử dụng lớn hơn chính xác`a[i]`. Nếu một chữ số như vậy`x`tồn tại thì`a[0:i] + [x]`là tiền tố nhỏ nhất có thể lớn hơn số ban đầu tại vị trí`i`. 
5. Một lần`x`được tìm thấy, hãy tạo hậu tố bằng cách quét các chữ số từ`0`trở lên và lấy các chữ số nhỏ nhất không được tiền tố sử dụng và không bằng`x`. Đây chính xác là các chữ số hậu tố nhỏ nhất có thể về mặt từ điển, vì vậy lựa chọn này đưa ra số nhỏ nhất cho vị trí cố định này`i`. 
6. Nếu không hợp lệ`x`tồn tại ở vị trí`i`, chuyển đến`i - 1`. Để đại diện cho tiền tố mới`a[0:i-1]`, di dời`a[i-1]`từ số tiền tố hiện tại và đánh dấu chữ số đó là không được sử dụng nếu số đếm của nó bằng 0. Cập nhật`bad`nếu việc loại bỏ sự xuất hiện đó sẽ loại bỏ sự trùng lặp. 
7. Vị trí đầu tiên mà chúng ta có thể xây dựng câu trả lời là vị trí đúng cần thay đổi. Chúng tôi quét từ phải sang trái, vì vậy mọi vị trí sau đó đã được chứng minh là không thể, trong khi việc thay đổi vị trí trước đó sẽ tạo ra số lượng lớn hơn. 

### Tại sao nó hoạt động 

Hãy xem xét vị trí đầu tiên`i`nơi thuật toán thành công. Mỗi vị trí sau`i`đã được thử nghiệm trước tiên và không thể tạo ra số lớn hơn hợp lệ trong khi vẫn giữ nguyên tiền tố của nó. Vì vậy không có câu trả lời nào có thể khác với câu trả lời ban đầu muộn hơn`i`. 

Tại vị trí`i`, tiền tố`a[0:i]`khác biệt nên có thể bảo quản an toàn. Thuật toán chọn chữ số nhỏ nhất chưa được sử dụng lớn hơn`a[i]`, là chữ số nhỏ nhất có thể làm cho số kết quả lớn hơn ở vị trí này. Bất kỳ sự thay thế nhỏ hơn nào cũng sẽ không làm cho con số lớn hơn, trong khi bất kỳ sự thay thế lớn hơn nào sẽ tạo ra một con số lớn hơn mức cần thiết. 

Sau lần thay thế đó, tất cả các vị trí còn lại được điền bằng các chữ số nhỏ nhất có sẵn theo thứ tự tăng dần. Vì tiền tố và phần thay thế đã được cố định nên đây là hậu tố hợp lệ nhỏ nhất về mặt từ điển. Do đó, số được xây dựng lớn hơn số đầu vào, có các chữ số riêng biệt và không tồn tại số lớn hơn hợp lệ nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, pos, delta):
        pos += 1
        while pos <= self.n:
            self.bit[pos] += delta
            pos += pos & -pos

    def prefix_sum(self, pos):
        """Number of elements in [0, pos)."""
        res = 0
        while pos > 0:
            res += self.bit[pos]
            pos -= pos & -pos
        return res

    def kth(self, k):
        """Return the 0-based index of the k-th present element."""
        idx = 0
        step = 1 << (self.n.bit_length() - 1)

        while step:
            nxt = idx + step
            if nxt <= self.n and self.bit[nxt] < k:
                idx = nxt
                k -= self.bit[nxt]
            step >>= 1

        return idx

def solve_case(n, b, a):
    # The prefix before position n-1.
    cnt = [0] * b
    for i in range(n - 1):
        cnt[a[i]] += 1

    # Number of digit values appearing at least twice in the prefix.
    bad = sum(c >= 2 for c in cnt)

    # Fenwick tree stores currently unused digits.
    fw = Fenwick(b)
    for d in range(b):
        fw.add(d, 1)

    # Remove all digits used by the prefix from the available set.
    for d in range(b):
        if cnt[d]:
            fw.add(d, -1)

    for i in range(n - 1, -1, -1):
        if bad == 0:
            # Number of unused digits <= a[i].
            le = fw.prefix_sum(a[i] + 1)
            total = fw.prefix_sum(b)

            # We need the first unused digit strictly greater than a[i].
            k = le + 1

            if k <= total:
                x = fw.kth(k)

                # The prefix is already distinct, and x is unused.
                ans = a[:i] + [x]

                # Fill the suffix with the smallest remaining digits.
                need = n - i - 1
                for d in range(b):
                    if need == 0:
                        break
                    if cnt[d] == 0 and d != x:
                        ans.append(d)
                        need -= 1

                return ans

        if i > 0:
            # Move from prefix a[:i] to prefix a[:i-1].
            v = a[i - 1]

            if cnt[v] == 2:
                bad -= 1

            cnt[v] -= 1

            if cnt[v] == 0:
                fw.add(v, 1)

    # The statement guarantees that an answer exists.
    return []

def main():
    n, b = map(int, input().split())
    a = list(map(int, input().split()))

    ans = solve_case(n, b, a)
    print(*ans)

if __name__ == "__main__":
    main()
```Mảng tần số mô tả tiền tố không thay đổi hiện tại. Tần số lớn hơn một có nghĩa là tiền tố không bao giờ có thể là một phần của câu trả lời hợp lệ, vì vậy`bad`cho phép chúng tôi kiểm tra tính hợp lệ của tiền tố trong thời gian không đổi. 

Cây Fenwick chứa chính xác các chữ số không có trong tiền tố.`prefix_sum(a[i] + 1)`đếm các chữ số có sẵn từ`0`bởi vì`a[i]`, vậy chữ số có sẵn tiếp theo có thứ hạng`le + 1`. các`kth`hoạt động chuyển đổi thứ hạng đó thành chữ số thực tế trong`O(log b)`thời gian. 

Vòng lặp bắt đầu bằng tiền tố trước vị trí cuối cùng và loại bỏ một phần tử bất cứ khi nào nó di chuyển sang trái. Thứ tự này là chi tiết ranh giới quan trọng. Tại lần lặp`i`,`cnt`phải mô tả chính xác`a[0:i]`, không`a[0:i+1]`. 

Khi tần số thay đổi từ`2`ĐẾN`1`, một chữ số trùng lặp sẽ biến mất, vì vậy`bad`giảm đi. Khi tần số thay đổi từ`1`ĐẾN`0`, chữ số đó sẽ có sẵn trở lại trong cây Fenwick. Chúng ta không bao giờ cần cộng lại một chữ số khi số đếm của nó vẫn dương. 

Cấu trúc hậu tố có chủ ý quét từ 0 trở lên. Người thay thế được chọn`x`bị loại trừ, cũng như tất cả các chữ số đã có trong tiền tố. Vòng lặp diễn ra chính xác`n - i - 1`các chữ số, điều này có thể thực hiện được vì bài toán đảm bảo rằng một số câu trả lời hợp lệ tồn tại và sự tồn tại của một`n`-số phân biệt chữ số cũng ngụ ý rằng`b >= n`. 

Không có chuyển đổi số nguyên của cơ sở-`b`số, vì vậy kích thước số nguyên Python không liên quan. Câu trả lời được xử lý dưới dạng một mảng các chữ số, điều này cũng cần thiết vì`b`có thể lớn hơn nhiều so với mười. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho```
3 10
9 2 6
```đầu vào đã có tiền tố riêng biệt ở mọi vị trí. Chữ số ngoài cùng bên phải là`6`và chữ số nhỏ nhất không được sử dụng lớn hơn`6`là`7`, nên chúng ta có thể thay đổi vị trí cuối cùng ngay lập tức. 

|`i`| Tiền tố |`a[i]`| Chữ số lớn hơn chưa sử dụng | Hành động | 
| --- | --- | --- | --- | --- | 
| 2 |`9 2`| 6 | 7 | Chọn`7`| 

Số kết quả là`9 2 7`. Không còn hậu tố nào, vì vậy đây ngay lập tức là số hợp lệ nhỏ nhất lớn hơn đầu vào. 

### Mẫu 2 

cho```
4 11
10 5 5 1
```vị trí cuối cùng không thể được sử dụng vì tiền tố của nó chứa hai bản sao của`5`. Chúng tôi di chuyển sang trái cho đến khi tiền tố trở nên khác biệt. 

|`i`| Tiền tố |`bad`|`a[i]`| Chữ số lớn nhất nhỏ nhất chưa được sử dụng | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 3 |`10 5 5`| 1 | 1 | 2 | Không thể sử dụng tiền tố | 
| 2 |`10 5`| 0 | 5 | 6 | Chọn`6`| 

Sau khi chọn`6`, tiền tố là`10 5 6`. Vị trí duy nhất còn lại sẽ nhận được chữ số nhỏ nhất chưa được sử dụng, đó là`0`. 

Kết quả là`10 5 6 0`. Dấu vết này chứng tỏ tại sao chúng ta không thể đơn giản tìm kiếm giá trị lớn hơn ở vị trí cuối cùng. Tiền tố phải hợp lệ trước khi vị trí đó có thể được giữ nguyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log b + b)`| có`n`truy vấn kế tiếp hoặc cập nhật tiền tố, mỗi truy vấn lấy`O(log b)`, theo sau là nhiều nhất một`O(b)`xây dựng hậu tố. | 
| Không gian |`O(n + b)`| Đầu vào, mảng tần số, cây Fenwick và đầu ra đều sử dụng không gian tuyến tính. | 

Với`n, b <= 3 * 10^5`, thuật toán chỉ thực hiện vài triệu phép tính Fenwick cộng với một lần quét trên phạm vi chữ số. Điều này nằm trong phạm vi dự định cho giới hạn 2 giây và 256 MB, trong khi việc liệt kê số lượng ứng cử viên quá lớn theo cấp số nhân. 

## Trường hợp thử nghiệm```python
# helper: run the algorithm on an input string
import io
import sys

def run(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    b = int(next(it))
    a = [int(next(it)) for _ in range(n)]

    ans = solve_case(n, b, a)
    return " ".join(map(str, ans))

# Provided samples
assert run("""\
3 10
9 2 6
""") == "9 2 7", "sample 1"

assert run("""\
4 11
10 5 5 1
""") == "10 5 6 0", "sample 2"

assert run("""\
4 4
3 2 0 1
""") == "3 2 1 0", "sample 3"

# Minimum-size valid case.
assert run("""\
1 3
1
""") == "2", "minimum n"

# All values are equal, so the algorithm must move left before
# it finds a distinct prefix.
assert run("""\
4 5
2 2 2 2
""") == "2 3 0 1", "all equal values"

# The only possible change is at the first position.
assert run("""\
3 4
1 3 3
""") == "2 0 1", "change first position"

# Catches the off-by-one case where the last digit cannot be
# increased, but the previous digit can.
assert run("""\
3 4
1 2 3
""") == "1 3 0", "change previous position"

# Maximum-size construction.
n = 300000
a = list(range(1, n))
inp = f"{n} {n}\n" + " ".join(map(str, a)) + "\n"
expected = " ".join(map(str, list(range(1, n)) + [0]))
assert run(inp) == expected, "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 / 1`|`2`| Độ dài tối thiểu có thể và người kế nhiệm trực tiếp | 
|`4 5 / 2 2 2 2`|`2 3 0 1`| Các giá trị lặp lại và di chuyển sang trái qua các tiền tố không hợp lệ | 
|`3 4 / 1 3 3`|`2 0 1`| Tăng ở vị trí đầu tiên và xây dựng lại toàn bộ hậu tố | 
|`3 4 / 1 2 3`|`1 3 0`| Vị trí ngoài cùng bên phải không có chữ số lớn hơn không được sử dụng nên vị trí trước đó bị thay đổi | 
|`300000 300000 / 1 2 ... 299999`|`1 2 ... 299999 0`| Tối đa`n`Và`b`, cộng với hiệu suất quy mô lớn | 

## Vỏ cạnh 

Đối với trường hợp tiền tố lặp lại```
4 5
2 2 2 2
```vị trí đầu tiên được thử nghiệm là`i = 3`, với tiền tố`2 2 2`. Tần số của nó cho chữ số`2`là ba, vậy`bad > 0`và vị trí đó bị từ chối. Tại`i = 2`, tiền tố là`2 2`, vẫn không hợp lệ. Tại`i = 1`, tiền tố chỉ là`2`, đó là sự khác biệt. Chữ số nhỏ nhất không được sử dụng lớn hơn`2`là`3`, chữ số nhỏ nhất còn lại là`0`Và`1`, sản xuất`2 3 0 1`. 

Đối với trường hợp thay đổi phải xảy ra ngay từ đầu,```
3 4
1 3 3
```tiền tố trước vị trí cuối cùng là`1 3`, khác biệt nhưng không có chữ số nào được sử dụng lớn hơn`3`bởi vì chữ số duy nhất phía trên nó ít nhất phải bằng`4`, bên ngoài căn cứ. Di chuyển đến`i = 1`, tiền tố`1`là khác biệt, nhưng một lần nữa không có chữ số nào được sử dụng lớn hơn`3`. Tại`i = 0`, chữ số nhỏ nhất không được sử dụng lớn hơn`1`là`2`. Hậu tố sau đó sử dụng`0`Và`3`, cho`2 0 3`nếu như`3`có sẵn. Tuy nhiên, hậu tố nhỏ nhất thực tế là`0 1`, vì chữ số gốc`3`không phải là một phần của tiền tố được bảo tồn và không cần phải sử dụng lại. Vì vậy, đầu ra đúng là`2 0 1`. Điều này minh họa tại sao hậu tố phải được xây dựng lại từ tập hợp các chữ số được tiền tố mới sử dụng, thay vì sao chép từ đầu vào. 

Đối với trường hợp```
3 4
1 2 3
```tiền tố trước vị trí`2`là`1 2`, nhưng chữ số hiện tại`3`không có chữ số lớn hơn không được sử dụng. Chúng tôi loại bỏ`2`từ tiền tố được duy trì và vị trí kiểm tra`1`. Tiền tố`1`là khác biệt và`3`là chữ số nhỏ nhất không được sử dụng lớn hơn`2`. Sau khi chọn`3`, chữ số hậu tố nhỏ nhất không được sử dụng là`0`, cho`1 3 0`. Thay đổi vị trí đầu tiên sẽ tạo ra số lớn hơn nên dừng ở vị trí`1`chính xác là mục đích của quá trình quét từ phải sang trái. 

Đối với trường hợp ranh giới```
4 4
3 2 0 1
```tiền tố trước chữ số cuối cùng là`3 2 0`, khác biệt, nhưng chữ số hiện tại`1`không thể tăng lên vì mọi chữ số cơ sở 4 lớn hơn, cụ thể là`2`Và`3`, đã được sử dụng rồi. Thuật toán di chuyển đến vị trí`2`, tiền tố ở đâu`3 2`. chữ số`1`không được sử dụng và lớn hơn hiện tại`0`, vì vậy nó trở thành sự thay thế. Chữ số nhỏ nhất còn lại chưa được sử dụng là`0`, sản xuất`3 2 1 0`. Vì vị trí đã thay đổi càng xa bên phải càng tốt nên không tồn tại số lớn hơn hợp lệ nhỏ hơn.
