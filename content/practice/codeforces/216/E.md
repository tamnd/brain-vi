---
title: "CF 216E - Sao Hỏa May Mắn"
description: "Chúng ta được cho một dãy các chữ số được viết bằng cơ số k. Mỗi chữ số nằm trong khoảng từ 0 đến k - 1 và một chữ số đặc biệt b được gọi là may mắn. Đối với mỗi chuỗi con của chuỗi chữ số, chúng tôi hiểu chuỗi con đó là số cơ sở-k, cho phép các số 0 đứng đầu."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 216
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 133 (Div. 2)"
rating: 2000
weight: 216
solve_time_s: 97
verified: true
draft: false
---

[CF 216E - Vận may của sao Hỏa](https://codeforces.com/problemset/problem/216/E) 

**Xếp hạng:** 2000 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các chữ số được viết dưới dạng cơ số`k`. Mỗi chữ số nằm giữa`0`Và`k - 1`, và một chữ số đặc biệt`b`được gọi là may mắn. 

Đối với mỗi chuỗi con của chuỗi chữ số, chúng tôi hiểu chuỗi con đó là chuỗi cơ sở-`k`số, cho phép số 0 đứng đầu. Một chuỗi con được coi là may mắn nếu gốc số của nó bằng`b`. 

Nhiệm vụ là đếm xem có bao nhiêu chuỗi con may mắn. 

Thử thách đầu tiên là hiểu gốc số có nghĩa gì trong một cơ số tùy ý. Trong số thập phân, việc cộng các chữ số liên tục cuối cùng sẽ cho một giá trị tương đương với số modulo`9`. Trong căn cứ`k`, hiện tượng tương tự xảy ra với mô đun`k - 1`. 

Giả sử một số được viết dưới dạng cơ số`k`:$$x = d_0 + d_1 k + d_2 k^2 + \dots$$Từ$$k \equiv 1 \pmod{k-1}$$chúng tôi cũng có$$x \equiv d_0 + d_1 + d_2 + \dots \pmod{k-1}$$Việc lặp lại quá trình tính tổng các chữ số sẽ giữ nguyên giá trị modulo`k - 1`, cho đến khi chỉ còn lại một chữ số. Chữ số cuối cùng đó chính xác là gốc kỹ thuật số. 

Điều này tạo ra một tình huống đặc biệt. Trong số học modulo thông thường, số dư`0`tương ứng với gốc kỹ thuật số`0`. Nhưng với số dương chia hết cho`k - 1`, gốc kỹ thuật số trở thành`k - 1`, không`0`. Điều này giống hệt với cách hoạt động của các gốc số thập phân:`18`có gốc kỹ thuật số`9`, không`0`. 

Các ràng buộc buộc chúng ta phải tránh phép tính bậc hai. Với`n = 100000`, có khoảng`5 * 10^9`chuỗi con nên việc liệt kê tất cả các chuỗi con là không thể. Ngay cả việc kiểm tra chuỗi con trong`O(1)`mỗi thứ vẫn còn quá chậm. Chúng ta cần một cái gì đó gần với thời gian tuyến tính. 

Có một số trường hợp khó bỏ sót. 

Coi như:```
2 0 1
0
```Chuỗi con duy nhất là`"0"`. Gốc kỹ thuật số của nó là`0`, vậy câu trả lời là`1`. 

Việc triển khai bất cẩn chỉ sử dụng công thức gốc kỹ thuật số tiêu chuẩn cho các số dương sẽ bác bỏ số 0 một cách không chính xác. 

Bây giờ hãy xem xét:```
10 9 2
1 8
```Chuỗi con`"18"`có tổng chữ số`9`, vậy gốc kỹ thuật số của nó là`9`, không`0`. 

Nếu chúng ta chỉ kiểm tra`sum % 9 == 0`, chúng tôi sẽ phân loại không chính xác nó là gốc`0`. 

Một trường hợp tế nhị khác xuất hiện khi`k = 2`. 

Sau đó`k - 1 = 1`, nên mọi số đều đồng dạng với`0 mod 1`. Số học mô-đun trở nên vô dụng trừ khi được xử lý riêng. 

Ví dụ:```
2 1 3
1 0 1
```Mọi số nhị phân khác 0 đều có gốc số`1`. Chuỗi con duy nhất có gốc kỹ thuật số`0`là chuỗi con bao gồm toàn bộ số 0. 

Giải pháp dựa trên modulo trực tiếp bị phá vỡ hoàn toàn ở đây vì tất cả các tổng tiền tố đều giống hệt modulo`1`. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Đối với mỗi chuỗi con, hãy tính tổng các chữ số của nó, liên tục giảm nó thành một cơ sở duy nhất-`k`chữ số và kiểm tra xem kết quả có bằng không`b`. 

có`O(n^2)`các chuỗi con. Ngay cả khi chúng ta tính toán trước các tổng tiền tố để mỗi tổng chuỗi con có sẵn trong`O(1)`, chúng ta vẫn phải kiểm tra tất cả các chuỗi con. Với`n = 100000`, điều này trở nên đại khái`5 * 10^9`chuỗi con, vượt xa giới hạn. 

Quan sát quan trọng là gốc kỹ thuật số trong cơ sở`k`chỉ phụ thuộc vào tổng các chữ số modulo`k - 1`. 

Đối với số dương:$$\text{digital root}(x) = \begin{cases} k - 1 & \text{if } x \equiv 0 \pmod{k-1} \\ x \bmod (k-1) & \text{otherwise} \end{cases}$$Một chuỗi con tương ứng với một dãy chữ số liền kề, do đó tổng các chữ số của nó có thể được lấy từ tổng tiền tố:$$S(l, r) = pref[r] - pref[l-1]$$Điều này biến bài toán thành việc đếm các cặp tổng tiền tố với modulo sai phân cụ thể.`k - 1`. 

Nếu như`b > 0`, thì chúng ta chỉ cần:$$S(l,r) \equiv b \pmod{k-1}$$Nếu như`b = 0`, only substrings whose total digit sum is exactly zero qualify. Since digits are nonnegative, that means every digit in the substring must be zero.

 Sự khác biệt này là toàn bộ vấn đề. 

Sau khi giảm về tổng tiền tố modulo`k - 1`, we can count valid substrings using a frequency map in linear time.

 | Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc căn cứ`k`, chữ số may mắn`b`, và dãy chữ số. 
2. Xử lý trường hợp đặc biệt`b = 0`. 

Một số có gốc kỹ thuật số`0`chỉ khi bản thân số đó bằng 0. Vì tất cả các chữ số đều không âm nên một chuỗi con biểu thị số 0 một cách chính xác khi mọi chữ số trong đó đều bằng 0. 

Chúng tôi quét mảng và đếm độ dài của các khối 0 liên tiếp. Khối có chiều dài`L`đóng góp:$$\frac{L(L+1)}{2}$$chuỗi con chỉ có 0. 
3. Xử lý trường hợp đặc biệt`k = 2`. 

Trong hệ nhị phân, các gốc số duy nhất có thể có là`0`Và`1`. 

Nếu như`b = 0`, bước 2 đã xử lý xong rồi. 

Nếu như`b = 1`, mọi chuỗi con khác 0 đều là chuỗi may mắn. Tổng số chuỗi con là:$$\frac{n(n+1)}{2}$$Chúng tôi trừ các chuỗi con chỉ có 0 được tính trước đó. 
4. Đối với trường hợp tổng quát`k > 2`Và`b > 0`, cho phép:$$m = k - 1$$5. Xây dựng tổng tiền tố modulo`m`. 

Định nghĩa:$$pref[i] = (a_1 + a_2 + \dots + a_i) \bmod m$$6. Đối với từng vị trí`r`, chúng tôi muốn các tiền tố trước đó thỏa mãn:$$pref[r] - pref[l-1] \equiv b \pmod m$$Sắp xếp lại:$$pref[l-1] \equiv pref[r] - b \pmod m$$7. Duy trì bản đồ tần số`cnt`của dư lượng tiền tố đã thấy trước đó. 

Ban đầu,`cnt[0] = 1`bởi vì tiền tố trống tồn tại. 
8. Đối với mỗi dư tiền tố`cur`: 

Tính toán:$$need = (cur - b) \bmod m$$Mỗi tiền tố trước có dư lượng`need`tạo thành một chuỗi con may mắn kết thúc ở đây. 
9. Thêm`cnt[need]`cho câu trả lời, sau đó tăng dần`cnt[cur]`. 

### Tại sao nó hoạt động 

Đối với bất kỳ chuỗi con nào, tổng chữ số của nó bằng hiệu của hai tổng tiền tố. Số học mô-đun bảo toàn sự khác biệt này theo mô-đun`k - 1`. 

Đối với các nghiệm số dương, giá trị được xác định duy nhất bởi lớp modulo đó. Mỗi chuỗi con được thuật toán đếm đều thỏa mãn điều kiện mô-đun chính xác tương ứng với gốc kỹ thuật số`b`và mọi chuỗi con hợp lệ sẽ tạo ra chính xác một cặp tiền tố phù hợp. 

các`b = 0`trường hợp được tách ra vì số học modulo không thể phân biệt gốc số`0`từ gốc kỹ thuật số`k - 1`. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

def solve():
    k, b, n = map(int, input().split())
    a = list(map(int, input().split()))

    # count zero-only substrings
    zero_substrings = 0
    cur = 0

    for x in a:
        if x == 0:
            cur += 1
        else:
            zero_substrings += cur * (cur + 1) // 2
            cur = 0

    zero_substrings += cur * (cur + 1) // 2

    # digital root 0
    if b == 0:
        print(zero_substrings)
        return

    # base 2 special case
    if k == 2:
        total = n * (n + 1) // 2
        print(total - zero_substrings)
        return

    mod = k - 1

    cnt = defaultdict(int)
    cnt[0] = 1

    ans = 0
    pref = 0

    for x in a:
        pref = (pref + x) % mod

        need = (pref - b) % mod
        ans += cnt[need]

        cnt[pref] += 1

    print(ans)

solve()
```Phần đầu tiên đếm tất cả các chuỗi con bao gồm toàn bộ số 0. Điều này là cần thiết cho cả`b = 0`trường hợp đặc biệt và nhị phân. 

Logic đếm 0 sử dụng các khối liên tiếp. Nếu một khối có chiều dài`L`, thì mọi lựa chọn của điểm cuối bên trái và bên phải bên trong khối đó tạo thành một chuỗi con bằng 0 hợp lệ, cho`L(L+1)/2`. 

Phần tiếp theo xử lý các trường hợp khó xử về mặt toán học một cách riêng biệt. 

Khi`b = 0`, chỉ những số có giá trị 0 mới đủ điều kiện, vì vậy chúng ta có thể in ngay số lượng chỉ có 0. 

Khi`k = 2`, mọi số khác 0 đều có nghiệm số`1`. Câu trả lời trở thành tất cả các chuỗi con trừ đi các chuỗi con chỉ có 0. 

Vòng lặp chính thực hiện phương thức tiền tố-modulo.`pref`lưu trữ modulo tổng tiền tố hiện tại`k - 1`. Đối với mỗi vị trí mới, chúng tôi tính toán phần dư nào trước đó sẽ làm cho phần dư của chuỗi con bằng`b`. Sơ đồ băm`cnt`lưu trữ số lần mỗi dư lượng đã xuất hiện cho đến nay. 

Thứ tự quan trọng. Trước tiên, chúng tôi truy vấn số lượng tiền tố phù hợp trước đó, sau đó chèn tiền tố hiện tại vào bản đồ. Đảo ngược thứ tự này sẽ đếm không chính xác các chuỗi con trống. 

Số nguyên Python tự động xử lý kích thước câu trả lời lớn một cách an toàn. Số lượng chuỗi con tối đa là khoảng`5 * 10^9`, dù sao cũng vừa vặn thoải mái với số nguyên 64 bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 5 6
3 2 0 5 6 1
```Đây:$$k - 1 = 9$$Chúng ta cần tổng chữ số chuỗi con phù hợp với`5 mod 9`. 

| Vị trí | Chữ số | Tiền tố Mod | Cần Mod | Đếm trước | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | - | 0 | - | cnt[0]=1 | 0 | 
| 1 | 3 | 3 | 7 | 0 | 0 | 
| 2 | 2 | 5 | 0 | 1 | 1 | 
| 3 | 0 | 5 | 0 | 1 | 2 | 
| 4 | 5 | 1 | 5 | 2 | 4 | 
| 5 | 6 | 7 | 2 | 0 | 4 | 
| 6 | 1 | 8 | 3 | 1 | 5 | 

Câu trả lời cuối cùng:`5`. 

Dấu vết này cho thấy cách các phần dư tiền tố lặp lại tự nhiên đếm nhiều chuỗi con hợp lệ kết thúc ở cùng một vị trí. 

### Ví dụ 2 

đầu vào:```
2 1 3
1 0 1
```Tất cả các chuỗi con:```
[1]
[1,0]
[1,0,1]
[0]
[0,1]
[1]
```Chỉ một`[0]`có gốc kỹ thuật số`0`. 

Tổng số chuỗi con:$$\frac{3 \cdot 4}{2} = 6$$Chuỗi con chỉ có 0:`1`. 

Trả lời:$$6 - 1 = 5$$| Số lượng | Giá trị | 
| --- | --- | 
| Tổng số chuỗi con | 6 | 
| Chuỗi con chỉ có 0 | 1 | 
| Chuỗi con may mắn | 5 | 

Ví dụ này chứng minh tại sao nhị phân yêu cầu xử lý riêng biệt. Modulo`1`số học không mang thông tin. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một đường chuyền cho khối 0 và một đường chuyền để đếm tiền tố | 
| Không gian | O(n) | Bản đồ băm của dư lượng tiền tố | 

Với`n = 100000`, thời gian tuyến tính dễ dàng phù hợp trong giới hạn. Hashmap lưu trữ tối đa một mục nhập cho mỗi dư lượng riêng biệt gặp phải, điều này cũng an toàn trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        k, b, n = map(int, input().split())
        a = list(map(int, input().split()))

        zero_substrings = 0
        cur = 0

        for x in a:
            if x == 0:
                cur += 1
            else:
                zero_substrings += cur * (cur + 1) // 2
                cur = 0

        zero_substrings += cur * (cur + 1) // 2

        if b == 0:
            return str(zero_substrings)

        if k == 2:
            total = n * (n + 1) // 2
            return str(total - zero_substrings)

        mod = k - 1

        cnt = defaultdict(int)
        cnt[0] = 1

        ans = 0
        pref = 0

        for x in a:
            pref = (pref + x) % mod

            need = (pref - b) % mod
            ans += cnt[need]

            cnt[pref] += 1

        return str(ans)

    return solve()

# provided sample
assert run(
"""10 5 6
3 2 0 5 6 1
"""
) == "5", "sample 1"

# single zero
assert run(
"""2 0 1
0
"""
) == "1", "single zero substring"

# binary case
assert run(
"""2 1 3
1 0 1
"""
) == "5", "all nonzero substrings"

# all zeros
assert run(
"""10 0 4
0 0 0 0
"""
) == "10", "all substrings are zero"

# divisibility edge case
assert run(
"""10 9 2
1 8
"""
) == "1", "digital root 9, not 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 0 1 / 0`|`1`| Xử lý đúng số 0 | 
|`2 1 3 / 1 0 1`|`5`| Trường hợp đặc biệt nhị phân | 
|`10 0 4 / 0 0 0 0`|`10`| Đếm chính xác các khối 0 | 
|`10 9 2 / 1 8`|`1`| bội số của`k-1`bản đồ tới`k-1`, không`0`| 

## Vỏ cạnh 

Hãy xem xét:```
2 0 1
0
```Thuật toán đầu tiên đếm số khối bằng 0. Có một khối chiều dài`1`, Vì thế:$$\frac{1 \cdot 2}{2} = 1$$Từ`b = 0`, thuật toán trả về ngay`1`. 

Điều này xử lý chính xác thực tế là số 0 có gốc số 0. 

Bây giờ hãy xem xét:```
10 9 2
1 8
```Tổng chuỗi con là:$$1 + 8 = 9$$Modulo`9`, đây là`0`. Một cách tiếp cận đơn giản theo mô-đun sẽ phân loại nó là gốc kỹ thuật số`0`. 

Thuật toán tránh được lỗi đó vì`b = 9`được xử lý theo logic nghiệm dương tổng quát, trong đó phần dư`0`tương ứng với gốc kỹ thuật số`9`. 

Cuối cùng, hãy xem xét trường hợp nhị phân:```
2 1 3
1 0 1
```Từ`k - 1 = 1`, tất cả các tổng tiền tố modulo`1`trở thành số không. Một giải pháp mô-đun thông thường sẽ đếm sai mọi chuỗi con cho mọi chuỗi dương`b`. 

Việc xử lý đặc biệt tính toán:```
total substrings = 6
zero-only substrings = 1
```và trả về`5`, điều này đúng vì mọi số nhị phân khác 0 đều có gốc số`1`.
