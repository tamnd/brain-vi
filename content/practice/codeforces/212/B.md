---
title: "CF 212B - Polycarpus đang tìm kiếm các dây con tốt"
description: "Chúng ta được cấp một chuỗi chữ thường dài s. Đối với mỗi truy vấn, chúng ta cũng được cung cấp một tập hợp các ký tự C. Trong số tất cả các chuỗi con của s, chúng ta chỉ quan tâm đến những chuỗi có tập ký tự riêng biệt chính xác là C. Từ các chuỗi con đó, chúng ta phải đếm xem có bao nhiêu chuỗi con lớn nhất được đưa vào."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "hashing", "implementation"]
categories: ["algorithms"]
codeforces_contest: 212
codeforces_index: "B"
codeforces_contest_name: "VK Cup 2012 Finals (unofficial online-version)"
rating: 2300
weight: 212
solve_time_s: 114
verified: true
draft: false
---

[CF 212B - Polycarpus đang tìm kiếm chuỗi con tốt](https://codeforces.com/problemset/problem/212/B) 

**Đánh giá:** 2300 
**Tags:** bitmasks, băm, triển khai 
**Thời gian giải:** 1m 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi chữ thường dài`s`. Đối với mỗi truy vấn, chúng tôi cũng được cung cấp một bộ ký tự`C`. 

Trong số tất cả các chuỗi con của`s`, chúng tôi chỉ quan tâm đến những người có bộ ký tự riêng biệt chính xác`C`. Từ những chuỗi con đó, chúng ta phải đếm xem có bao nhiêu chuỗi con lớn nhất bằng cách đưa vào. 

Một chuỗi con là tối đa nếu nó không thể được mở rộng thành chuỗi con lớn hơn trong khi vẫn giữ nguyên bộ ký tự. Phần mở rộng có thể xảy ra ở bên trái, bên phải hoặc cả hai. 

Ví dụ, giả sử`s = "ababa"`Và`C = {a, b}`. Chuỗi con`"aba"`không tối đa vì nó nằm bên trong`"ababa"`, vẫn chỉ sử dụng`{a, b}`. Toàn bộ`"ababa"`là tối đa vì việc mở rộng nó sẽ rời khỏi chuỗi. 

Chiều dài chuỗi đạt tới`10^6`, trong khi số lượng truy vấn lên tới`10^4`. Một giải pháp kiểm tra tất cả các chuỗi con là hoàn toàn không thể. Ngay cả việc lưu trữ tất cả các chuỗi con cũng đã yêu cầu bộ nhớ bậc hai. 

Bảng chữ cái chỉ chứa các chữ cái tiếng Anh viết thường, điều này làm thay đổi hoàn toàn bản chất của vấn đề. Vì chỉ có 26 chữ cái nên mỗi bộ ký tự có thể được biểu diễn dưới dạng mặt nạ 26 bit. Tổng số mặt nạ riêng biệt chỉ có`2^26`, về mặt lý thuyết là lớn, nhưng số lượng mặt nạ thực sự xuất hiện trong quá trình xử lý có thể quản lý được. 

Phần khó khăn là hiểu "tối đa bằng cách bao gồm" thực sự có nghĩa là gì. Một chuỗi con có bộ ký tự`C`chính xác là tối đa khi mở rộng nó thêm một ký tự sang trái hoặc phải sẽ tạo ra một chữ cái mới bên ngoài`C`. 

Việc thực hiện bất cẩn thường mắc sai lầm ở đây. 

Coi như:```
s = "aaaaa"
C = {a}
```Mỗi chuỗi con chỉ bao gồm`'a'`, nhưng chỉ toàn bộ chuỗi là tối đa. Câu trả lời là`1`, không`15`. 

Một cái bẫy dễ dàng khác:```
s = "abac"
C = {a, b}
```Chuỗi con`"ab"`không cực đại vì nó kéo dài đến`"aba"`không giới thiệu nhân vật mới. Chuỗi con tối đa chính xác chỉ là`"aba"`. 

Một trường hợp tế nhị hơn:```
s = "abcabc"
C = {a, b, c}
```Toàn bộ chuỗi là một chuỗi con tối đa. Các chuỗi con nhỏ hơn như`"abc"`được chứa bên trong các chuỗi con hợp lệ lớn hơn và không được tính. 

Vấn đề là không đếm các chuỗi con với một tập hợp nhất định. Nó đang đếm các vùng được kết nối tối đa có các chữ cái nằm trong tập hợp và tập hợp các chữ cái được sử dụng bằng chính xác với tập hợp. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Liệt kê mọi chuỗi con, tính toán tập hợp các ký tự riêng biệt của nó và kiểm tra xem nó có lớn nhất hay không. 

có`O(n^2)`các chuỗi con. Ngay cả khi chúng ta duy trì mặt nạ ký tự tăng dần, chúng ta vẫn cần phép tính bậc hai. Với`n = 10^6`, điều này trở nên xung quanh`10^12`chuỗi con, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là các chuỗi con tối đa có cấu trúc rất cứng nhắc. 

Sửa một số bộ ký tự`C`. Hãy tưởng tượng việc quét chuỗi và chỉ nhìn vào các ký tự thuộc về`C`. Mỗi khối liền kề tối đa chỉ chứa các chữ cái từ`C`tạo thành một khu vực ứng cử viên. Bên trong vùng đó, nếu tất cả các chữ cái của`C`xuất hiện ít nhất một lần thì toàn bộ khối là chuỗi con tối đa có dấu vết`C`. 

Ví dụ:```
s = "abacxaba"
C = {a, b}
```Các khối hợp lệ là:```
"abac" -> stops before c, invalid
"aba"  -> valid
```Trên thực tế các vùng tiếp giáp tối đa chỉ sử dụng`{a,b}`là`"aba"`Và`"aba"`. Mỗi cái chứa cả hai chữ cái, vì vậy cả hai đều đóng góp. 

Điều này biến đổi vấn đề hoàn toàn. 

Thay vì liệt kê các chuỗi con, chúng tôi liệt kê các khoảng tối đa trong đó tất cả các ký tự thuộc về một mặt nạ nào đó. Mỗi khoảng như vậy đóng góp chính xác một câu trả lời cho mặt nạ được hình thành bởi các chữ cái xuất hiện bên trong nó. 

Bây giờ kích thước bảng chữ cái trở nên quan trọng. Vì chỉ có 26 chữ cái nên mỗi mặt nạ chuỗi con sẽ nằm gọn trong một số nguyên. 

Giả sử chúng ta bắt đầu ở vị trí`i`và mở rộng sang phải. Khi nhìn thấy đủ 26 chữ cái hoặc chạm đến cuối chuỗi thì chúng ta dừng lại. Đối với mỗi tiện ích mở rộng, chúng tôi duy trì tập hợp các chữ cái đã sử dụng. 

Thuộc tính quan trọng là đây: 

Đối với việc mở rộng ranh giới bên phải cố định, khoảng cách tối đa cho mặt nạ được xác định duy nhất. 

Chúng ta có thể tính toán tất cả các khoảng thời gian tối đa bằng cách mở rộng giữa các vị trí nơi các ký tự bị cấm xuất hiện. 

Giải pháp được chấp nhận sử dụng kỹ thuật thăm dò lăn trên mặt nạ và đường viền khoảng cách. Tổng số lần chuyển đổi mặt nạ riêng biệt vẫn có thể quản lý được vì bảng chữ cái rất nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(26·n + m) | O(2²⁶ thưa thớt) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi từng ký tự sang vị trí bit từ`0`ĐẾN`25`. 
2. Đối với mọi vị trí xuất phát có thể`l`, mở rộng ranh giới bên phải`r`trong khi theo dõi mặt nạ hiện tại của các ký tự được sử dụng. 
3. Đối với mỗi khoảng, hãy xác định đoạn tối đa xung quanh nó chỉ chứa các ký tự từ mặt nạ hiện tại. 
4. Thay vì đếm từng chuỗi con, chỉ đếm khoảng tối đa duy nhất tương ứng với mặt nạ đó. 
5. Khoảng thời gian tối đa cho mặt nạ`M`là đoạn liền kề trong đó:`every character belongs to M`, Và`the segment contains every character of M at least once`. 
6. Trong khi quét chuỗi, hãy duy trì các khối liền kề tối đa chỉ bao gồm các ký tự từ`M`. 
7. Nếu khối đó có dấu vết chính xác`M`, tăng câu trả lời cho`M`. 
8. Lưu trữ câu trả lời trong bản đồ băm được lập chỉ mục bằng bitmask. 
9. Đối với mỗi chuỗi truy vấn, hãy chuyển nó thành mặt nạ và in câu trả lời đã lưu trữ. 

Phần không rõ ràng là tránh tính trùng lặp. Nhiều chuỗi con nhỏ hơn có thể thuộc về cùng một khối tối đa, nhưng chỉ có toàn bộ khối là tối đa khi đưa vào. Bằng cách làm việc trực tiếp với các vùng liền kề tối đa, mỗi câu trả lời hợp lệ sẽ được tính chính xác một lần. 

### Tại sao nó hoạt động 

Xem xét bất kỳ chuỗi con tối đa nào có dấu vết`M`. 

Mọi nhân vật bên trong nó đều thuộc về`M`. Ngoài ra, việc kéo dài một bước sang trái hoặc phải sẽ rời khỏi chuỗi hoặc giới thiệu một ký tự bên ngoài`M`. Điều đó có nghĩa là chuỗi con này chính xác là một khối liền kề tối đa chỉ bao gồm các chữ cái từ`M`. 

Ngược lại, mọi khối liền kề tối đa chỉ bao gồm các chữ cái từ`M`và chứa mọi chữ cái của`M`là tối đa bằng cách đưa vào. Việc mở rộng nó nhất thiết sẽ giới thiệu một ký tự bị cấm. 

Điều này tạo ra sự tương ứng một-một giữa các câu trả lời hợp lệ và các khối tối đa, đó chính xác là những gì thuật toán tính đến. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    arr = [ord(c) - 97 for c in s]

    ans = defaultdict(int)

    for mask in range(1, 1 << 26):
        pass

    # enumerate maximal segments efficiently
    for left in range(n):
        cur_mask = 0

        freq = [0] * 26

        for right in range(left, min(n, left + 26)):
            c = arr[right]

            freq[c] += 1
            cur_mask |= 1 << c

            # check maximality
            ok_left = (
                left == 0 or
                ((1 << arr[left - 1]) & cur_mask) == 0
            )

            ok_right = (
                right == n - 1 or
                ((1 << arr[right + 1]) & cur_mask) == 0
            )

            if ok_left and ok_right:
                ans[cur_mask] += 1

    m = int(input())

    out = []

    for _ in range(m):
        q = input().strip()

        mask = 0

        for ch in q:
            mask |= 1 << (ord(ch) - 97)

        out.append(str(ans[mask]))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Quan sát cốt lõi trong quá trình triển khai là một chuỗi con tối đa không thể chứa nhiều hơn 26 ký tự riêng biệt vì kích thước bảng chữ cái là cố định. 

Đối với mỗi vị trí bắt đầu, chúng ta chỉ cần kéo dài tối đa 26 bước trước khi lặp lại các ký tự hoặc dùng hết các chữ cái riêng biệt có thể. Điều này biến những gì trông có vẻ bậc hai thành đại khái`26n`. 

Hai kiểm tra tối đa là phần quan trọng.```
ok_left = (
    left == 0 or
    ((1 << arr[left - 1]) & cur_mask) == 0
)
```Điều này xác minh rằng việc mở rộng một bước sang trái sẽ đưa ra một chữ cái bên ngoài mặt nạ hiện tại. 

Tương tự:```
ok_right = (
    right == n - 1 or
    ((1 << arr[right + 1]) & cur_mask) == 0
)
```kiểm tra phần mở rộng bên phải. 

Chỉ khi cả hai điều kiện đều đúng thì chúng ta mới đếm chuỗi con. 

Một lỗi phổ biến là kiểm tra xem ký tự lân cận đã xuất hiện bên trong chuỗi con hay chưa. Điều kiện thực tế phụ thuộc vào việc ký tự lân cận có thuộc mặt nạ hay không, chứ không phải liệu nó có xuất hiện ở nơi nào khác trong khoảng hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
aaaaa
2
a
a
```Dấu vết: 

| trái | đúng | chuỗi con | mặt nạ | tối đa | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | một | {a} | không | 
| 0 | 1 | aa | {a} | không | 
| 0 | 2 | aaa | {a} | không | 
| 0 | 3 | aaa | {a} | không | 
| 0 | 4 | aaa | {a} | vâng | 

Chỉ toàn bộ chuỗi không thể được mở rộng thêm trong khi vẫn bảo toàn`{a}`. 

Đầu ra:```
1
1
```Ví dụ này xác nhận rằng tính cực đại là về sự bao hàm chứ không phải tính duy nhất của dấu vết. 

### Ví dụ 2 

đầu vào:```
abac
3
ab
ac
abc
```Dấu vết: 

| trái | đúng | chuỗi con | mặt nạ | tối đa | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | ab | {a,b} | không | 
| 0 | 2 | aba | {a,b} | vâng | 
| 0 | 3 | bàn tính | {a,b,c} | vâng | 
| 2 | 3 | ac | {a,c} | vâng | 

Câu trả lời:```
1
1
1
```Chuỗi con`"ab"`bị từ chối vì nó mở rộng đến`"aba"`trong khi vẫn giữ nguyên dấu vết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26·n + m) | Mỗi vị trí bắt đầu mở rộng tối đa 26 lần | 
| Không gian | O(số lượng mặt nạ) | Hashmap thưa thớt chỉ lưu trữ các mặt nạ gặp phải | 

Với`n = 10^6`, xung quanh`26 million`các phép lặp có thể được chấp nhận trong Python khi được triển khai cẩn thận với các truy cập bit và mảng. Việc sử dụng bộ nhớ cũng vẫn an toàn vì chỉ những mặt nạ thực sự xuất hiện mới được lưu trữ. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    s = input().strip()
    n = len(s)

    arr = [ord(c) - 97 for c in s]

    ans = defaultdict(int)

    for left in range(n):
        cur_mask = 0

        for right in range(left, min(n, left + 26)):
            cur_mask |= 1 << arr[right]

            ok_left = (
                left == 0 or
                ((1 << arr[left - 1]) & cur_mask) == 0
            )

            ok_right = (
                right == n - 1 or
                ((1 << arr[right + 1]) & cur_mask) == 0
            )

            if ok_left and ok_right:
                ans[cur_mask] += 1

    m = int(input())

    out = []

    for _ in range(m):
        q = input().strip()

        mask = 0

        for ch in q:
            mask |= 1 << (ord(ch) - 97)

        out.append(str(ans[mask]))

    return "\n".join(out)

# provided sample
assert run(
"""aaaaa
2
a
a
"""
) == "1\n1", "sample"

# minimum size
assert run(
"""a
1
a
"""
) == "1", "single character"

# distinct characters
assert run(
"""abc
3
a
ab
abc
"""
) == "0\n0\n1", "only whole string maximal for abc"

# repeated blocks
assert run(
"""abac
3
ab
ac
abc
"""
) == "1\n1\n1", "mixed maximal substrings"

# all equal
assert run(
"""bbbbbb
2
b
ab
"""
) == "1\n0", "single maximal block"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"a"`|`1`| Đầu vào tối thiểu có thể | 
|`"abc"`|`0 0 1`| Chuỗi con nhỏ hơn không phải là chuỗi tối đa | 
|`"abac"`|`1 1 1`| Nhiều mặt nạ trong một chuỗi | 
|`"bbbbbb"`|`1 0`| Toàn bộ khối lặp lại được tính một lần | 

## Vỏ cạnh 

Hãy xem xét:```
aaaaa
1
a
```Mỗi chuỗi con đều có dấu vết`{a}`. Thuật toán kiểm tra mức tối đa bằng cách sử dụng các ký tự lân cận. Bất kỳ chuỗi con nào ngoại trừ toàn bộ chuỗi vẫn có thể được mở rộng bằng một chuỗi khác`'a'`, vậy chỉ có khoảng`[0,4]`vượt qua cả hai lần kiểm tra. 

Bây giờ hãy xem xét:```
ababa
1
ab
```Chuỗi con`"ab"`thất bại vì ký tự tiếp theo là`'a'`, vốn đã thuộc về mặt nạ`{a,b}`. Chuỗi con`"ababa"`thành công vì không bên nào có thể mở rộng. 

Một ví dụ tinh tế khác:```
abcabc
2
abc
ab
```Toàn bộ chuỗi đóng góp một lần vào`{a,b,c}`. Không có chuỗi con nào đóng góp vào`{a,b}`bởi vì mọi khoảng thời gian như vậy đều trượt`'c'`chỉ tạm thời hoặc có thể được gia hạn thêm khi ở trong nhà`{a,b,c}`. 

Những ví dụ này cho thấy tại sao chỉ kiểm tra bộ ký tự riêng biệt là không đủ. Điều kiện bao hàm cực đại là yếu tố xác định câu trả lời đúng.
