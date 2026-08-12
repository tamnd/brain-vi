---
title: "CF 102700M - Phép thuật"
description: "Chúng tôi có một chuỗi tham chiếu s. Mọi dãy con không trống của s đều được coi là một câu thần chú hợp lệ. For each input string a, some original spell has been followed by an arbitrary suffix, so the useful part of a is exactly its longest prefix that can still be embedded as a subsequence…"
date: "2026-08-12T19:13:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102700
codeforces_index: "M"
codeforces_contest_name: "2020 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102700
solve_time_s: 787
verified: true
draft: false
---

[CF 102700M - Phép thuật](https://codeforces.com/problemset/problem/102700/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 13m 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi tham chiếu`s`. Every non-empty subsequence of`s`được coi là một câu thần chú hợp lệ. Đối với mỗi chuỗi đầu vào`a`, một số câu thần chú ban đầu đã được theo sau bởi một hậu tố tùy ý, vì vậy phần hữu ích của`a`chính xác là tiền tố dài nhất của nó mà vẫn có thể được nhúng dưới dạng một dãy con của`s`. 

Do đó, nhiệm vụ này là một vấn đề về tiền tố-tiếp theo. Bắt đầu từ đầu của`a`, chúng tôi muốn tiếp tục lấy các ký tự miễn là chúng có thể xuất hiện trong`s`ở các vị trí tăng dần. Thời điểm một ký tự không thể được đặt sau vị trí khớp trước đó, thì tiền tố không thể hoạt động được nữa. Nếu ngay cả ký tự đầu tiên cũng không thể khớp thì câu trả lời là`IMPOSSIBLE`. 

Chuỗi tham chiếu có nhiều nhất`2 * 10^5`ký tự, có thể có tới`10^5`các truy vấn và tổng độ dài của tất cả các chuỗi được sửa đổi cũng tối đa`2 * 10^5`. Giới hạn cuối cùng là giới hạn giúp giải pháp tuyến tính hoặc gần tuyến tính trở nên khả thi: trên tất cả các truy vấn, chúng ta chỉ cần xử lý`2 * 10^5`nhân vật. Một cách tiếp cận dành`O(|s|)`hoạt động cho mọi ký tự truy vấn vẫn có thể tiếp cận được khoảng`2 * 10^5 * 2 * 10^5 = 4 * 10^10`kiểm tra nhân vật, vượt xa giới hạn hai giây. 

Trường hợp cạnh đầu tiên là khi ký tự đầu tiên không xuất hiện trong`s`. Ví dụ,```
abc
1
d
```có đầu ra```
IMPOSSIBLE
```Không có tiền tố nào trống mà là một dãy con. Việc triển khai bất cẩn có thể in ra một chuỗi trống, nhưng kết quả đầu ra được yêu cầu sử dụng rõ ràng`IMPOSSIBLE`khi không có câu thần chú nào trống rỗng có thể được hình thành. 

Trường hợp cạnh thứ hai xảy ra khi một ký tự tồn tại trong`s`, nhưng không phải sau vị trí được sử dụng cho ký tự trước đó. Ví dụ,```
abc
1
ca
```có đầu ra```
c
```các`c`có thể được khớp ở vị trí cuối cùng, nhưng không có`a`sau nó. Chỉ kiểm tra xem mọi ký tự có xuất hiện ở đâu đó trong`s`sẽ chấp nhận sai`ca`. 

Trường hợp cạnh thứ ba là khi toàn bộ chuỗi đầu vào là một chuỗi con hợp lệ. Ví dụ,```
abc
1
abc
```có đầu ra```
abc
```Thuật toán không được yêu cầu truy vấn chứa hậu tố được sửa đổi bổ sung. Một truy vấn có thể đã chính xác như chính tả ban đầu. 

Trường hợp cạnh thứ tư là các ký tự lặp lại. Ví dụ,```
aaa
2
aaaa
ba
```có đầu ra```
aaa
IMPOSSIBLE
```Truy vấn đầu tiên có thể sử dụng cả ba lần xuất hiện của`a`, nhưng cái thứ tư không thể đặt được. Truy vấn thứ hai thất bại ngay lập tức vì`b`không bao giờ xảy ra. Việc coi sự hiện diện của ký tự như một boolean là không đủ khi thứ tự và bội số quan trọng. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là xử lý mọi truy vấn một cách độc lập và mô phỏng định nghĩa của dãy con. Giữ một con trỏ vào`s`. Đối với mỗi ký tự của truy vấn, hãy quét tiếp theo`s`cho đến khi tìm thấy ký tự đó. Nếu tìm thấy, hãy di chuyển con trỏ qua nó và tiếp tục. Nếu sự kết thúc của`s`đạt được trước tiên, dừng lại và trả về tiền tố phù hợp cho đến nay. 

Phương pháp brute-force này đúng vì một dãy con được xác định chính xác bằng cách chọn các vị trí tăng dần trong`s`. Đối với mỗi ký tự truy vấn, việc chọn lần xuất hiện khả dụng đầu tiên của nó là tối ưu: việc chọn lần xuất hiện sau chỉ có thể để lại ít vị trí khả dụng hơn cho các ký tự còn lại. Vấn đề là thời gian chạy của nó. Một ký tự đơn có thể yêu cầu quét gần như toàn bộ`s`và điều này có thể xảy ra độc lập đối với nhiều truy vấn. Với`|s| = 2 * 10^5`và tổng chiều dài truy vấn`2 * 10^5`, trường hợp xấu nhất là theo thứ tự`4 * 10^10`so sánh nhân vật. 

Quan sát hữu ích là phần tốn kém của lực lượng vũ phu liên tục tìm kiếm trong cùng một chuỗi cố định`s`. Chuỗi không bao giờ thay đổi giữa các truy vấn, vì vậy tất cả thông tin về vị trí xuất hiện của các ký tự có thể được chuẩn bị một lần. 

Đối với mỗi ký tự chữ thường, hãy lưu trữ danh sách đã sắp xếp các vị trí xuất hiện trong`s`. Giả sử vị trí khớp trước đó là`p`và ký tự truy vấn tiếp theo là`c`. Chúng ta cần sự xuất hiện đầu tiên của`c`vị trí của nó lớn hơn`p`. Vì danh sách xuất hiện đã được sắp xếp nên đây chính xác là tìm kiếm nhị phân cho vị trí đầu tiên lớn hơn`p`. 

Điều đó thay đổi từng tra cứu ký tự từ việc quét`s`để tìm kiếm nhị phân trong số lần xuất hiện của một ký tự. Vì chỉ có`2 * 10^5`tổng số ký tự truy vấn, toàn bộ tính toán sẽ trở thành`O(|s| + L log |s|)`, Ở đâu`L`là tổng độ dài của tất cả các truy vấn. Với những giới hạn này, điều đó dễ dàng đủ nhanh. 

Ý tưởng tương tự cũng có thể được thực hiện với một bảng lần xuất hiện tiếp theo đầy đủ, đưa ra`O(|s| + L)`thời gian, nhưng trong Python một bảng như vậy có thể tiêu tốn nhiều bộ nhớ hơn vì nó lưu trữ thông tin cho 26 ký tự ở mọi vị trí. Danh sách sự cố đơn giản hơn và phù hợp với giới hạn một cách thoải mái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O( | s | * L)`trong trường hợp xấu nhất |`O(1)`bên cạnh đầu vào | Quá chậm | 
| Danh sách xuất hiện + tìm kiếm nhị phân |`O( | s | + L log | s | )`|`O( | s | )`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`s`và xây dựng 26 danh sách xuất hiện. Đối với mọi vị trí`i`TRONG`s`, nối thêm`i`vào danh sách thuộc về`s[i]`. Vì các vị trí được xử lý từ trái sang phải nên mọi danh sách đều được sắp xếp tự động. 
2. Xử lý từng phép thuật được sửa đổi`a`một cách độc lập. Bộ`pos = -1`, nghĩa là không có ký tự nào của`a`đã khớp chưa. Đồng thời tạo một danh sách trống cho câu trả lời. 
3. Đối với mỗi nhân vật`c`TRONG`a`, lấy danh sách xuất hiện thuộc về`c`và tìm kiếm nhị phân cho vị trí đầu tiên lớn hơn`pos`. Đây là địa điểm sớm nhất có thể`c`có thể được đặt trong khi vẫn giữ nguyên thứ tự tiếp theo. 
4. Nếu vị trí đó không tồn tại, hãy ngừng xử lý truy vấn này. Mỗi tiền tố dài hơn đều chứa cùng một ký tự không khớp, vì vậy nó cũng không thể là một chuỗi con. 
5. Nếu vị trí tồn tại, hãy nối thêm`c`để trả lời và cập nhật`pos`đến sự việc đó. Việc chọn sự xuất hiện sớm nhất có thể để lại hậu tố lớn nhất có thể có của`s`có sẵn cho các ký tự sau này. 
6. Sau khi xử lý truy vấn, hãy in tiền tố phù hợp nếu nó không trống. Nếu ký tự đầu tiên đã thất bại thì danh sách câu trả lời sẽ trống, vì vậy hãy in`IMPOSSIBLE`. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý lần đầu tiên`k`các ký tự của một truy vấn,`pos`là vị trí sớm nhất có thể trong`s`tại đó tiền tố đó có thể kết thúc. Điều này đúng ban đầu vì không có ký tự nào được khớp. Khi xử lý ký tự tiếp theo, tìm kiếm nhị phân chọn sự xuất hiện sớm nhất của nó sau`pos`, do đó vị trí mới lại là vị trí kết thúc sớm nhất có thể. 

Nếu sự xuất hiện đó không tồn tại thì không có cách nào để đặt ký tự tiếp theo sau bất kỳ vị trí hợp lệ nào của tiền tố trước đó. Vì chúng tôi đã giữ các ký tự trước đó càng sớm càng tốt nên việc chọn bất kỳ vị trí nào khác chỉ có thể di chuyển xa hơn về bên phải và không thể giúp ích gì. Do đó, tiền tố hiện tại là tiền tố hợp lệ dài nhất và không còn tiền tố nào có thể là một câu thần chú nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from bisect import bisect_right

def solve():
    s = input().strip()
    n = int(input())

    positions = [[] for _ in range(26)]

    for i, ch in enumerate(s):
        positions[ord(ch) - ord('a')].append(i)

    output = []

    for _ in range(n):
        a = input().strip()

        pos = -1
        answer = []

        for ch in a:
            occ = positions[ord(ch) - ord('a')]

            # First occurrence strictly after pos.
            idx = bisect_right(occ, pos)

            if idx == len(occ):
                break

            pos = occ[idx]
            answer.append(ch)

        if answer:
            output.append(''.join(answer))
        else:
            output.append("IMPOSSIBLE")

    sys.stdout.write('\n'.join(output))

if __name__ == "__main__":
    solve()
```các`positions`mảng chứa một danh sách được sắp xếp cho mỗi chữ cái viết thường. Ví dụ, nếu`s = "abracadabra"`, danh sách cho`a`chứa tất cả các chỉ số mà tại đó`a`xuất hiện theo thứ tự tăng dần. 

Đối với mỗi truy vấn,`pos`thể hiện vị trí trong`s`được sử dụng bởi ký tự khớp thành công cuối cùng. Nó bắt đầu lúc`-1`, vì vậy ký tự đầu tiên được phép sử dụng vị trí`0`. 

Hoạt động tinh tế là`bisect_right(occ, pos)`. Chúng tôi cần một vị trí lớn hơn`pos`, không lớn hơn hoặc bằng nó, vì hai ký tự tiếp theo phải sử dụng các vị trí khác nhau. Nếu như`pos`là`4`và ký tự tiếp theo xuất hiện ở vị trí`4, 7, 9`, lựa chọn đúng là`7`, Vì thế`bisect_right`cung cấp chính xác chỉ số cần thiết. 

Khi tìm kiếm nhị phân trả về`len(occ)`, không có sự xuất hiện hợp lệ sau trận đấu trước đó. Chúng tôi dừng ngay lập tức vì câu trả lời phải là tiền tố. Tiếp tục kiểm tra các ký tự sau này không thể tạo ra tiền tố dài hơn hợp lệ. 

Không có vấn đề tràn số nguyên trong Python và giá trị vị trí không bao giờ vượt quá`len(s) - 1`. Đầu ra được tích lũy thành một danh sách và được ghi một lần ở cuối, tránh việc xóa lặp đi lặp lại hoặc các thao tác đầu ra tốn kém. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp, chuỗi tham chiếu là`abracadabra`. 

Truy vấn`abra`có thể được kết hợp hoàn toàn. Truy vấn`cadabra`cũng có thể được kết hợp hoàn toàn, trong khi`dcba`chỉ có thể khớp với ký tự đầu tiên của nó vì sau ký tự được chọn`d`không có`c`. 

| Ký tự truy vấn | Các trường hợp được xem xét | Vị trí trước đó | Vị trí được chọn | Tiền tố phù hợp | 
| --- | --- | --- | --- | --- | 
|`a`|`a`vị trí |`-1`|`0`|`a`| 
|`b`|`b`vị trí |`0`|`1`|`ab`| 
|`r`|`r`vị trí |`1`|`2`|`abr`| 
|`a`|`a`vị trí |`2`|`3`|`abra`| 

Vì`dcba`, đầu tiên`d`được tìm thấy ở gần cuối`s`. Khi vị trí đó đã được chọn, việc tìm kiếm tiếp theo cho`c`không có vị trí hợp lệ nên thuật toán dừng và trả về`d`. 

| Ký tự truy vấn | Vị trí trước đó | Vị trí được chọn | Kết quả | 
| --- | --- | --- | --- | 
|`d`|`-1`|`6`|`d`| 
|`c`|`6`| không | dừng lại | 

Mẫu hoàn chỉnh tạo ra`abra`,`cadabra`,`abcd`,`d`, Và`IMPOSSIBLE`, phù hợp với đầu ra yêu cầu. 

Ví dụ thứ hai thể hiện các ký tự lặp lại và một ký tự chỉ xuất hiện trước kết quả khớp trước đó.```
abcba
4
abba
cba
bbbbb
ac
```Kết quả là:```
abba
cba
bb
ac
```Vì`abba`, các vị trí được chọn là`0, 1, 3, 4`, vì vậy toàn bộ chuỗi là hợp lệ. 

| Ký tự truy vấn | Vị trí trước đó | Vị trí được chọn | Tiền tố phù hợp | 
| --- | --- | --- | --- | 
|`a`|`-1`|`0`|`a`| 
|`b`|`0`|`1`|`ab`| 
|`b`|`1`|`3`|`abb`| 
|`a`|`3`|`4`|`abba`| 

Vì`bbbbb`, chỉ có hai`b`các ký tự có thể được khớp bởi vì`s`chỉ chứa hai lần xuất hiện của`b`. 

| Ký tự truy vấn | Vị trí trước đó | Vị trí được chọn | Tiền tố phù hợp | 
| --- | --- | --- | --- | 
|`b`|`-1`|`1`|`b`| 
|`b`|`1`|`3`|`bb`| 
|`b`|`3`| không | dừng lại | 

Dấu vết này chứng tỏ tại sao thuật toán phải tìm kiếm lần xuất hiện tiếp theo sau vị trí trước đó thay vì chỉ kiểm tra xem ký tự có xuất hiện ở đâu đó trong`s`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O( | s | + L log | s | )`| Xây dựng danh sách chi phí xảy ra`O( | s | )`, và mỗi trong số`L`ký tự truy vấn thực hiện một tìm kiếm nhị phân. | 
| Không gian |`O( | s | )`| Mỗi vị trí của`s`xuất hiện trong đúng một danh sách xuất hiện, bên cạnh bộ lưu trữ đầu vào và đầu ra. | 

Đây`L`là tổng chiều dài của tất cả các phép thuật được sửa đổi và vấn đề đảm bảo`L <= 2 * 10^5`Và`|s| <= 2 * 10^5`. Quá trình tiền xử lý là tuyến tính, trong khi tìm kiếm nhị phân chỉ được thực hiện đối với các ký tự thực sự xuất hiện trong truy vấn đầu vào. Điều này giúp giải pháp luôn thoải mái trong giới hạn hai giây và 512 MB đã nêu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from bisect import bisect_right

def solve():
    input = sys.stdin.readline

    s = input().strip()
    n = int(input())

    positions = [[] for _ in range(26)]

    for i, ch in enumerate(s):
        positions[ord(ch) - ord('a')].append(i)

    out = []

    for _ in range(n):
        a = input().strip()

        pos = -1
        answer = []

        for ch in a:
            occ = positions[ord(ch) - ord('a')]
            idx = bisect_right(occ, pos)

            if idx == len(occ):
                break

            pos = occ[idx]
            answer.append(ch)

        out.append(''.join(answer) if answer else "IMPOSSIBLE")

    sys.stdout.write('\n'.join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run(
    """abracadabra
5
abra
cadabra
abcd
dcba
magic
"""
) == """abra
cadabra
abcd
d
IMPOSSIBLE""", "sample 1"

# Minimum-size input
assert run(
    """a
1
a
"""
) == "a", "minimum-size valid spell"

# All equal characters, including one character too many
assert run(
    """aaaa
3
aaaaa
aa
b
"""
) == """aaaa
aa
IMPOSSIBLE""", "repeated characters"

# Boundary and ordering cases
assert run(
    """abc
5
abc
abca
c
ac
bc
"""
) == """abc
abc
c
ac
bc""", "boundary positions and subsequence order"

# Maximum n and maximum total query length.
# The reference string and all queries use the same character.
s = "a" * 100000
queries = "\n".join(["a"] * 100000)
large_input = s + "\n100000\n" + queries + "\n"
large_output = ("a\n" * 99999) + "a"

assert run(large_input) == large_output, "large input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a / 1 / a`|`a`| Chuỗi tham chiếu và truy vấn nhỏ nhất có thể. | 
|`aaaa / aaaaa, aa, b`|`aaaa`,`aa`,`IMPOSSIBLE`| Ký tự lặp lại, số lần xuất hiện mệt mỏi và thiếu ký tự đầu tiên. | 
|`abc / abc, abca, c, ac, bc`|`abc`,`abc`,`c`,`ac`,`bc`| Khớp chính xác, hậu tố không hợp lệ, vị trí đầu tiên/cuối cùng và các chuỗi tiếp theo được sắp xếp. | 
|`100000`bản sao của`a`ở cả hai`s`và các truy vấn |`100000`dòng chứa`a`| Số lượng truy vấn tối đa và tổng kích thước đầu vào lớn. | 

## Vỏ cạnh 

Khi ký tự đầu tiên vắng mặt, thuật toán nhìn vào danh sách xuất hiện của nó và ngay lập tức thấy nó trống. Vì```
abc
1
d
```danh sách cho`d`có độ dài bằng 0, vì vậy`bisect_right`trả về 0 và thuật toán in`IMPOSSIBLE`. Không có chuỗi trống nào được phát ra. 

Khi ký tự được yêu cầu chỉ tồn tại trước kết quả khớp trước đó, tìm kiếm nhị phân sẽ bỏ qua những lần xuất hiện đó một cách chính xác. Vì```
abc
1
ca
```

`c`được khớp ở vị trí`2`. Danh sách xuất hiện của`a`chỉ chứa vị trí`0`, và tìm kiếm một vị trí lớn hơn`2`thất bại. Do đó, câu trả lời được lưu trữ là`c`. 

Khi một truy vấn dài hơn số lượng ký tự lặp lại có sẵn, tìm kiếm nhị phân sẽ hết số lần xuất hiện. Vì```
aaa
1
aaaa
```ba tìm kiếm đầu tiên chọn vị trí`0`,`1`, Và`2`. Tìm kiếm thứ tư không tìm thấy vị trí nào lớn hơn`2`, vậy câu trả lời là`aaa`. 

Khi một truy vấn đã là một chuỗi con hợp lệ thì không cần xử lý đặc biệt. Vì```
abc
1
abc
```tìm kiếm chọn vị trí`0`,`1`, Và`2`và toàn bộ truy vấn được trả về. 

Điều kiện biên trong tìm kiếm nhị phân là nghiêm ngặt. Giả định`s = "ab"`và truy vấn là`bb`. đầu tiên`b`sử dụng vị trí`1`. Tìm kiếm thứ hai phải tìm một vị trí lớn hơn`1`, không lớn hơn hoặc bằng`1`. Vì không có vị trí như vậy tồn tại nên kết quả chỉ là`b`. Tái sử dụng vị trí`1`sẽ là một dãy con không hợp lệ và là lỗi thường gặp nhất trong giải pháp này. 

Cuối cùng, nếu một truy vấn chứa các ký tự tùy ý sau ký tự không thể đầu tiên thì các ký tự đó sẽ không bao giờ được xem xét. Ví dụ,```
abc
1
adzzzz
```có câu trả lời`a`. Một lần`d`không thành công, mọi tiền tố dài hơn cũng chứa giá trị không hợp lệ`d`, do đó việc xử lý`z`,`z`,`z`,`z`không thể thay đổi câu trả lời. Đây chính xác là lý do tại sao việc dừng ở ký tự bị lỗi đầu tiên sẽ mang lại tiền tố hợp lệ dài nhất.
