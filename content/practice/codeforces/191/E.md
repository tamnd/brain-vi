---
title: "CF 191E - Ngăn cản biểu tình"
description: "Chúng tôi có rất nhiều sự tin cậy của người lính. Mỗi phân mảng liền kề đại diện cho một nhóm cảnh sát có thể có. Tướng luôn chọn nhóm chưa sử dụng mạnh nhất trước tiên, nghĩa là tất cả các tổng của mảng con được sắp xếp theo thứ tự giảm dần và chọn từng cái một."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "trees"]
categories: ["algorithms"]
codeforces_contest: 191
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 121 (Div. 1)"
rating: 2200
weight: 191
solve_time_s: 106
verified: true
draft: false
---

[CF 191E - Ngăn cản các cuộc biểu tình](https://codeforces.com/problemset/problem/191/E) 

**Xếp hạng:** 2200 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, cây 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có rất nhiều sự tin cậy của người lính. Mỗi phân mảng liền kề đại diện cho một nhóm cảnh sát có thể có. Tướng luôn chọn nhóm chưa sử dụng mạnh nhất trước tiên, nghĩa là tất cả các tổng của mảng con được sắp xếp theo thứ tự giảm dần và chọn từng cái một. 

Chúng tôi cần độ tin cậy của nhóm được chọn vào ngày`k`. Nói cách khác, trong số tất cả`n(n+1)/2`tổng của mảng con, chúng ta phải tìm`k`-giá trị lớn nhất thứ 

Mảng có thể chứa số âm nên mảng con dài hơn không phải lúc nào cũng tốt hơn. Chỉ riêng kỹ thuật mảng con có tổng tối đa là không đủ vì chúng ta cần toàn bộ thứ hạng của các tổng mảng con, chứ không chỉ thứ hạng tốt nhất. 

Những hạn chế là nguyên nhân gây ra khó khăn thực sự ở đây. Số lượng mảng con là bậc hai, đại khái là`n^2 / 2`. Vì`n = 10^5`, điều này trở thành về`5 * 10^9`mảng con, quá nhiều để liệt kê một cách rõ ràng. Ngay cả việc lưu trữ tất cả các khoản tiền cũng cần tới hàng chục gigabyte bộ nhớ. Bất kỳ giải pháp xung quanh`O(n^2)`ngay lập tức là không thể. 

Giới hạn thời gian rất hào phóng ở mức 6 giây, điều này thường báo hiệu rằng`O(n log^2 n)`hoặc`O(n log n)`giải pháp với cấu trúc dữ liệu tiên tiến được mong đợi. 

Phần khó khăn là chúng tôi không được yêu cầu xây dựng phần trên cùng`k`tính tổng trực tiếp. Cách tiếp cận đúng là tìm kiếm nhị phân câu trả lời và liên tục đếm xem có bao nhiêu mảng con có tổng ít nhất một ngưỡng nào đó.`x`. 

Một số trường hợp phức tạp âm thầm phá vỡ việc triển khai ngây thơ. 

Hãy xem xét một mảng trong đó tất cả các số đều âm. 

đầu vào:```
3 2
-5 -2 -7
```Tất cả các tổng của mảng con là:`[-5, -7, -14, -2, -9, -7]`Sắp xếp giảm dần:`[-2, -5, -7, -7, -9, -14]`Câu trả lời là`-5`. 

Việc triển khai bất cẩn với giả định rằng các mảng con lớn hơn luôn mạnh hơn sẽ thất bại ngay lập tức. 

Một trường hợp tinh tế khác xuất hiện với số tiền trùng lặp. 

đầu vào:```
3 4
1 1 1
```Tổng của mảng con là:`[1,2,3,1,2,1]`Sắp xếp giảm dần:`[3,2,2,1,1,1]`Giá trị lớn thứ 4 là`1`. 

Nếu chúng ta tìm kiếm nhị phân giá trị đầu tiên có số lượng lớn hơn`k`, thay vì lớn hơn hoặc bằng`k`, các bản sao sẽ tạo ra từng lỗi một. 

Giá trị lớn cũng quan trọng. 

đầu vào:```
2 1
1000000000 1000000000
```Tổng mảng con tốt nhất là`2000000000`. 

Sử dụng số nguyên 32 bit tràn vào đây. Python xử lý việc này một cách tự động, nhưng ở các ngôn ngữ cấp thấp hơn, việc này yêu cầu số học 64-bit ở mọi nơi. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tạo mọi mảng con liền kề, tính tổng của nó, lưu trữ tất cả các tổng trong một danh sách, sắp xếp danh sách giảm dần và lấy`k`-phần tử thứ. 

Tổng tiền tố làm giảm tính toán tổng mảng con từ`O(length)`ĐẾN`O(1)`, do đó tạo ra tất cả các khoản chi phí`O(n^2)`. Sắp xếp rồi tính chi phí`O(n^2 log n)`. 

Điều này hiệu quả với các mảng nhỏ, nhưng đối với`n = 10^5`số lượng mảng con là khoảng năm tỷ. Thậm chí việc lặp lại chúng một lần là không thể. 

Quan sát quan trọng là chúng ta thực sự không cần thứ tự chính xác của tất cả các khoản tiền. Chúng ta chỉ cần một giá trị là`k`-lớn thứ Những bài toán dạng này thường trở nên dễ dàng hơn nhiều nếu chúng ta trả lời được câu hỏi: 

"Có bao nhiêu mảng con có tổng ít nhất`x`?" 

Giả sử chúng ta có thể tính toán số lượng này một cách hiệu quả. Sau đó, tìm kiếm nhị phân trở nên có thể. 

Nếu ít nhất`k`mảng con có tổng ít nhất`x`, thì câu trả lời ít nhất là`x`. 

Nếu ít hơn`k`mảng con tiếp cận`x`, sau đó`x`là quá lớn. 

Bây giờ nhiệm vụ sẽ trở thành đếm các mảng con có tổng ít nhất`x`. 

Đặt tổng tiền tố là:`pref[i] = a[1] + ... + a[i]`Một mảng con`(l, r)`có tổng:`pref[r] - pref[l-1]`Chúng tôi muốn:`pref[r] - pref[l-1] >= x`Sắp xếp lại:`pref[l-1] <= pref[r] - x`Vì vậy, trong khi quét các tiền tố từ trái sang phải, đối với mỗi`pref[r]`chúng ta cần số lượng tiền tố trước đó không vượt quá`pref[r] - x`. 

Đây là một vấn đề thống kê thứ tự cổ điển. 

Vì tổng tiền tố có thể rất lớn và âm nên chúng tôi phối hợp nén chúng và sử dụng cây Fenwick để duy trì tần số của tổng tiền tố trước đó. 

Mỗi lượt đếm có giá`O(n log n)`. Tìm kiếm nhị phân trên phạm vi câu trả lời sẽ thêm một yếu tố logarit khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² log n) | O(n²) | Quá chậm | 
| Tối ưu | O(n log2 n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng tổng tiền tố. 

Định nghĩa:`pref[0] = 0`Và:`pref[i] = pref[i-1] + a[i]`Bây giờ, mỗi tổng của mảng con có thể được viết dưới dạng hiệu của hai tổng tiền tố. 
2. Tìm kiếm câu trả lời nhị phân. 

Tổng mảng con nhỏ nhất có thể ít nhất là`-10^14`, và lớn nhất là nhiều nhất`10^14`, bởi vì`n ≤ 10^5`Và`|a[i]| ≤ 10^9`. 

Chúng tôi tìm kiếm nhị phân cho giá trị lớn nhất`x`ít nhất như vậy`k`mảng con có tổng ít nhất`x`. 
3. Đối với ngưỡng cố định`x`, đếm các mảng con hợp lệ. 

Đối với mỗi vị trí`r`, chúng ta cần số chỉ số trước đó`l-1`thỏa mãn:`pref[l-1] <= pref[r] - x`Điều này trở thành một truy vấn tần số tiền tố. 
4. Phối hợp nén tất cả các tổng tiền tố. 

Cây Fenwick hoạt động trên các chỉ số chứ không phải số nguyên lớn tùy ý. 

Thu thập tất cả các tổng tiền tố, sắp xếp chúng và loại bỏ các bản sao. 
5. Quét tiền tố từ trái sang phải. 

Ban đầu chèn`pref[0]`. 

Đối với mỗi`pref[r]`: 

Tìm xem có bao nhiêu tổng tiền tố được chèn vào`<= pref[r] - x`. 

Thêm số này vào câu trả lời. 

Sau đó chèn`pref[r]`vào cây Fenwick. 
6. Sử dụng kết quả đếm bên trong tìm kiếm nhị phân. 

Nếu số lượng ít nhất là`k`, sau đó`x`là khả thi và chúng tôi thử các giá trị lớn hơn. 

Nếu không thì,`x`quá lớn và chúng tôi tìm kiếm các giá trị nhỏ hơn. 
7. Sau khi tìm kiếm nhị phân kết thúc, xuất ra giá trị khả thi lớn nhất. 

### Tại sao nó hoạt động 

Đối với mọi điểm cuối phù hợp`r`, thuật toán sẽ đếm chính xác các điểm cuối bên trái tạo ra tổng mảng con ít nhất`x`. Mỗi mảng con hợp lệ tương ứng với một cặp tổng tiền tố thỏa mãn:`pref[r] - pref[l-1] >= x`Cây Fenwick duy trì tất cả các tổng tiền tố trước đó, do đó mỗi mảng con được tính chính xác một lần. 

Tìm kiếm nhị phân là hợp lệ vì tính khả thi là đơn điệu. Nếu ít nhất`k`mảng con có tổng ít nhất`x`, thì điều tương tự cũng đúng với mọi ngưỡng nhỏ hơn. Tính đơn điệu này đảm bảo rằng tìm kiếm nhị phân hội tụ tới giá trị chính xác`k`-tổng mảng con lớn thứ 

## Giải pháp Python```python
import sys
from bisect import bisect_right

input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, idx, val):
        while idx <= self.n:
            self.bit[idx] += val
            idx += idx & -idx

    def sum(self, idx):
        res = 0
        while idx > 0:
            res += self.bit[idx]
            idx -= idx & -idx
        return res

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    pref = [0]
    for x in a:
        pref.append(pref[-1] + x)

    coords = sorted(set(pref))

    def count_ge(x):
        fw = Fenwick(len(coords))
        total = 0

        pos0 = bisect_right(coords, pref[0])
        fw.add(pos0, 1)

        for i in range(1, n + 1):
            need = pref[i] - x

            cnt = bisect_right(coords, need)
            total += fw.sum(cnt)

            pos = bisect_right(coords, pref[i])
            fw.add(pos, 1)

        return total

    lo = -10**15
    hi = 10**15

    while lo < hi:
        mid = (lo + hi + 1) // 2

        if count_ge(mid) >= k:
            lo = mid
        else:
            hi = mid - 1

    print(lo)

solve()
```Phần đầu tiên tính tổng tiền tố để mỗi tổng của mảng con trở thành hiệu của hai giá trị. Đây là sự chuyển đổi cốt lõi giúp cho việc đếm có thể thực hiện được. 

Cây Fenwick lưu trữ tần số của các tổng tiền tố đã thấy trước đó. Vì tổng tiền tố có thể âm và cực kỳ lớn nên việc nén phối hợp sẽ ánh xạ chúng thành một phạm vi chỉ số nhỏ gọn. 

chức năng`count_ge(x)`thực hiện bước đếm cho tìm kiếm nhị phân. Đối với mỗi tổng tiền tố hiện tại`pref[i]`, nó đếm xem có bao nhiêu tổng tiền tố trước đó thỏa mãn:```
pref[j] <= pref[i] - x
```Cuộc gọi đến`bisect_right`là tinh tế nhưng quan trọng. Chúng ta cần các giá trị nhỏ hơn hoặc bằng ngưỡng chứ không phải ít hơn. sử dụng`bisect_left`ở đây tạo ra câu trả lời sai khi tồn tại tổng tiền tố trùng lặp. 

Tìm kiếm nhị phân tìm kiếm giá trị khả thi lớn nhất. Công thức trung điểm:```
mid = (lo + hi + 1) // 2
```tránh các vòng lặp vô hạn bằng cách thiên vị lên trên. 

Phạm vi tìm kiếm sử dụng`±10^15`, bao gồm mọi tổng mảng con có thể một cách an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
1 4 2
```Tất cả các tổng của mảng con là:`[1,5,7,4,6,2]`Sắp xếp giảm dần:`[7,6,5,4,2,1]`Câu trả lời là`4`. 

Giả sử kiểm tra tìm kiếm nhị phân`x = 4`. 

Tổng tiền tố:`[0,1,5,7]`| tôi | trước[i] | cần = pref[i] - 4 | Trước đó ≤ cần | Đếm thêm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | -3 | 0 | 0 | 
| 2 | 5 | 1 | 2 | 2 | 
| 3 | 7 | 3 | 2 | 2 | 

Tổng số mảng con hợp lệ:`4`. 

Vì ít nhất`4`mảng con có tổng ít nhất`4`, câu trả lời ít nhất là`4`. 

Dấu vết này thể hiện tính bất biến của lõi. Ở mỗi bước, cây Fenwick chứa chính xác các tổng tiền tố trước đó. 

### Ví dụ 2 

đầu vào:```
3 2
-5 -2 -7
```Tổng các mảng con được sắp xếp:`[-2,-5,-7,-7,-9,-14]`Câu trả lời là`-5`. 

Giả sử kiểm tra tìm kiếm nhị phân`x = -5`. 

Tổng tiền tố:`[0,-5,-7,-14]`| tôi | trước[i] | cần | Trước đó ≤ cần | Đếm thêm | 
| --- | --- | --- | --- | --- | 
| 1 | -5 | 0 | 1 | 1 | 
| 2 | -7 | -2 | 1 | 1 | 
| 3 | -14 | -9 | 0 | 0 | 

Tổng số mảng con hợp lệ:`2`. 

Chính xác hai mảng con có tổng ít nhất`-5`. 

Ví dụ này cho thấy tại sao các giá trị âm phải được xử lý cẩn thận. Thuật toán không bao giờ giả định tích cực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log2 n) | Tìm kiếm nhị phân thực hiện các lần lặp O(phạm vi log), mỗi lượt đếm có giá O(n log n) | 
| Không gian | O(n) | Tổng tiền tố, tọa độ nén và cây Fenwick | 

Logarit của dãy số là khoảng 50 vì tổng của mảng con nằm trong khoảng`[-10^14, 10^14]`. Với`n = 10^5`, tổng số hoạt động của Fenwick vẫn còn tốt trong thời hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from bisect import bisect_right

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, idx, val):
            while idx <= self.n:
                self.bit[idx] += val
                idx += idx & -idx

        def sum(self, idx):
            res = 0
            while idx > 0:
                res += self.bit[idx]
                idx -= idx & -idx
            return res

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    pref = [0]
    for x in a:
        pref.append(pref[-1] + x)

    coords = sorted(set(pref))

    def count_ge(x):
        fw = Fenwick(len(coords))
        total = 0

        fw.add(bisect_right(coords, pref[0]), 1)

        for i in range(1, n + 1):
            need = pref[i] - x
            total += fw.sum(bisect_right(coords, need))
            fw.add(bisect_right(coords, pref[i]), 1)

        return total

    lo = -10**15
    hi = 10**15

    while lo < hi:
        mid = (lo + hi + 1) // 2

        if count_ge(mid) >= k:
            lo = mid
        else:
            hi = mid - 1

    return str(lo) + "\n"

# provided sample
assert run(
"""3 4
1 4 2
"""
) == "4\n", "sample 1"

# minimum size
assert run(
"""1 1
5
"""
) == "5\n", "single element"

# all negative
assert run(
"""3 2
-5 -2 -7
"""
) == "-5\n", "all negative"

# duplicate sums
assert run(
"""3 4
1 1 1
"""
) == "1\n", "duplicate sums"

# off-by-one around ranking
assert run(
"""2 2
5 -1
"""
) == "4\n", "ranking boundary"

# large values
assert run(
"""2 1
1000000000 1000000000
"""
) == "2000000000\n", "64-bit range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 5`|`5`| Đầu vào kích thước tối thiểu | 
|`3 2 / -5 -2 -7`|`-5`| Xử lý hoàn toàn tiêu cực | 
|`3 4 / 1 1 1`|`1`| Tổng của mảng con trùng lặp | 
|`2 2 / 5 -1`|`4`| Xếp hạng thứ k đúng | 
|`2 1 / 1e9 1e9`|`2000000000`| Xử lý số nguyên lớn | 

## Vỏ cạnh 

Hãy xem xét lại trường hợp toàn âm. 

đầu vào:```
3 2
-5 -2 -7
```Tổng tiền tố trở thành:`[0,-5,-7,-14]`Khi kiểm tra`x = -5`, thuật toán đếm chính xác hai mảng con hợp lệ:`[-5]`Và`[-2]`. 

Logic cây Fenwick vẫn hoạt động vì nó chỉ so sánh các tổng tiền tố. Không có giả định nào về tính tích cực xuất hiện ở bất cứ đâu. 

Bây giờ hãy xem xét số tiền trùng lặp. 

đầu vào:```
3 4
1 1 1
```Tổng của mảng con được sắp xếp giảm dần:`[3,2,2,1,1,1]`Câu trả lời là`1`. 

Trong quá trình đếm, nhiều tổng tiền tố có thể ánh xạ tới cùng một tọa độ nén. sử dụng`bisect_right`đảm bảo rằng tất cả các giá trị bằng nhau đều được đưa vào truy vấn. Thay thế nó bằng`bisect_left`loại trừ không chính xác các tổng tiền tố bằng nhau và đếm thiếu số lượng mảng con hợp lệ. 

Cuối cùng, hãy xem xét trường hợp ranh giới xếp hạng. 

đầu vào:```
2 2
5 -1
```Tổng của mảng con là:`[5,4,-1]`Tổng lớn thứ hai là`4`. 

Tìm kiếm nhị phân kiểm tra xem ít nhất hai mảng con có đạt đến ngưỡng hay không. Tại`x = 4`, số đếm chính xác là hai:`[5]`Và`[5,-1]`. 

Tại`x = 5`, chỉ có một mảng con đủ điều kiện. 

Điều này chứng tỏ tại sao điều kiện tìm kiếm nhị phân phải là:```
count >= k
```sử dụng`count > k`sẽ di chuyển sai qua câu trả lời đúng.
