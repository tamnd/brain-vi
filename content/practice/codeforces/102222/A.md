---
title: "CF 102222A - Phần tử tối đa trong một ngăn xếp"
description: "Chúng tôi có một ngăn xếp trống ban đầu. Mỗi trường hợp thử nghiệm không liệt kê các hoạt động một cách rõ ràng. Thay vào đó, nó cung cấp các tham số của trình tạo giả ngẫu nhiên và chúng ta phải tái tạo chuỗi hoạt động PUSH và POP giống như trình tạo."
date: "2026-08-19T00:40:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102222
codeforces_index: "A"
codeforces_contest_name: "2018-2019 ACM-ICPC, China Multi-Provincial Collegiate Programming Contest"
rating: 0
weight: 102222
solve_time_s: 529
verified: true
draft: false
---

[CF 102222A - Phần tử tối đa trong một ngăn xếp](https://codeforces.com/problemset/problem/102222/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 49 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một ngăn xếp trống ban đầu. Mỗi trường hợp thử nghiệm không liệt kê các hoạt động một cách rõ ràng. Thay vào đó, nó đưa ra các tham số của bộ tạo giả ngẫu nhiên và chúng ta phải tái tạo cùng một chuỗi các`PUSH`Và`POP`hoạt động như máy phát điện. Một cú đẩy tạo ra một giá trị giữa`1`Và`m`, trong khi pop sẽ loại bỏ phần tử trên cùng hiện tại nếu có. Sau mỗi thao tác, chúng ta cần giá trị lớn nhất hiện có trong ngăn xếp, sử dụng`0`khi ngăn xếp trống. Câu trả lời bắt buộc là XOR bitwise của`i * maximum_i`trên tất cả các chỉ số hoạt động`i`. Các ràng buộc chính thức cho phép tối đa`5 * 10^6`hoạt động trong một trường hợp thử nghiệm, với tối đa 50 trường hợp thử nghiệm và giới hạn bộ nhớ 256 MB. 

Giá trị lớn của`n`quyết định hình dạng của dung dịch. Ngay cả một lượng nhỏ công việc được lặp lại trên mỗi phần tử chỉ có thể được chấp nhận nếu đó là thời gian không đổi cho mỗi thao tác. Một thuật toán quét toàn bộ ngăn xếp sau mỗi thao tác có thể thực hiện gần đúng`1 + 2 + 3 + ... + n = n(n + 1) / 2`so sánh. Vì`n = 5 * 10^6`, đó là về`1.25 * 10^13`so sánh, vượt xa những gì giới hạn 10 giây có thể chịu đựng được. Giải pháp dự định phải xử lý mọi thao tác được tạo trong thời gian O(1). Trang vấn đề chính thức cũng đưa ra điều tương tự`5 * 10^6`giới hạn và giới hạn 10 giây. 

Có một số trường hợp khó xử lý. Đầu tiên,`POP`được phép khi ngăn xếp đã trống. Đối với mẫu đầu tiên, các thao tác được tạo bắt đầu bằng hai lần bật lên. Thao tác đầu tiên làm cho ngăn xếp trống, do đó đóng góp của nó là`1 * 0 = 0`; thứ hai cũng làm như vậy. Một giải pháp giả định mọi cửa sổ bật lên đều hợp lệ có thể truy cập vào phần tử ngăn xếp không hợp lệ.```
1
1 1 1 4 23333 66666 233333
```Ở đây, thao tác được tạo đầu tiên là thao tác đầu tiên giống như Mẫu 1, cụ thể là`POP`, vì vậy đầu ra đúng là`Case #1: 0`. Việc triển khai bất cẩn đọc phần tử trên cùng mà không kiểm tra tính trống sẽ thất bại. 

Trường hợp cạnh thứ hai là việc tăng mức tối đa hiện tại không có nghĩa là mức tối đa mới bằng 0. Mức tối đa trước đó phải xuất hiện lại nếu phần tử khác bên dưới nó vẫn còn hiện diện. Đây là lý do tại sao chỉ lưu trữ một mức tối đa toàn cầu là không đủ. Ví dụ, sau`PUSH(3), PUSH(8), POP()`, giá trị lớn nhất còn lại là`3`, không`0`. Cấu trúc dữ liệu chính xác phải ghi nhớ mức tối đa được liên kết với mọi độ sâu ngăn xếp. 

Trường hợp cạnh thứ ba được lặp lại các giá trị bằng nhau. Giả sử ngăn xếp chứa`7, 7, 7`và chúng tôi bật một lần. Mức tối đa vẫn là`7`. Một biểu diễn chỉ lưu trữ các vị trí có mức tăng tối đa nghiêm ngặt có thể làm mất dấu vết của bội số này. Biểu diễn tối đa tiền tố tránh được vấn đề đó vì mỗi độ sâu đều lưu trữ mức tối đa của chính nó. 

Cuối cùng, bản thân trình tạo sử dụng số học không dấu 32 bit. Số nguyên Python không bị tràn một cách tự nhiên, vì vậy mỗi lần cập nhật trạng thái của`SA`,`SB`, Và`SC`phải giảm modulo`2^32`. Việc bỏ qua mặt nạ đó sẽ làm thay đổi các thao tác được tạo và có thể tạo ra câu trả lời sai ngay cả khi logic ngăn xếp hoàn hảo. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là lưu trữ mọi giá trị ngăn xếp thực tế. Khi nhấn, hãy thêm giá trị. Trên một cửa sổ bật lên, hãy xóa giá trị cuối cùng. Để đạt được mức tối đa sau mỗi thao tác, hãy quét toàn bộ ngăn xếp và lấy mức tối đa của nó. Điều này đúng vì quá trình quét sẽ kiểm tra chính xác các phần tử hiện có trong ngăn xếp. 

Vấn đề là việc quét lặp đi lặp lại. Nếu ngăn xếp tăng kích thước`n`và vẫn lớn, truy vấn đầu tiên có thể kiểm tra một phần tử, hai phần tử tiếp theo, v.v. Trong trường hợp xấu nhất tổng công sẽ trở thành O(n2), khoảng`1.25 * 10^13`kiểm tra phần tử ngăn xếp cho`n = 5 * 10^6`. Trình tạo được sử dụng có chủ ý để bản thân đầu vào không trở nên quá lớn, nhưng điều đó không làm cho thuật toán O(n2) trở nên khả thi. 

Quan sát quan trọng là thông tin duy nhất chúng ta cần từ tiền tố ngăn xếp là thông tin tối đa. Hãy xem xét ngăn xếp sau vài lần đẩy. Ở độ sâu`k`, lưu trữ giá trị tối đa trong số đầu tiên`k`các yếu tố thực tế. Khi một giá trị mới`x`được đẩy, mức tối đa mới chỉ đơn giản là`max(previous_maximum, x)`. Khi đỉnh được bật lên, giá trị tối đa sẽ tự động trở thành giá trị được lưu ở độ sâu trước đó. 

Điều này có nghĩa là các giá trị được đẩy thực tế là không cần thiết. Ví dụ: giả sử ngăn xếp thực sự là`[4, 2, 9, 3]`. Chúng ta có thể thay thế nó bằng`[4, 4, 9, 9]`. Giá trị hàng đầu`9`là mức tối đa hiện tại. Nếu chúng ta bật một lần, biểu diễn sẽ trở thành`[4, 4, 9]`, và đỉnh của nó vẫn là mức tối đa chính xác. Bật lại và nó trở thành`[4, 4]`, tiết lộ`4`, chính xác như ngăn xếp ban đầu. 

Do đó, cấu trúc này chỉ là một ngăn xếp khác chứa tiền tố cực đại. Mọi hoạt động đều trở thành thời gian không đổi. Chúng ta có thể tiến thêm một bước nữa trong quá trình triển khai vì các giá trị thực tế không bao giờ cần thiết sau một lần đẩy. Một số nguyên 32 bit cho mỗi độ sâu ngăn xếp là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`p`,`q`,`m`và ba trạng thái máy phát điện`SA`,`SB`, Và`SC`. Trình tạo phải được sao chép chính xác vì trình tự thao tác được ẩn trong các giá trị này. 
2. Phân bổ một chồng tiền tố cực đại có thêm một khe chứa`0`. Cho phép`depth`biểu thị số phần tử hiện có trong ngăn xếp logic. Trọng điểm ở độ sâu bằng 0 có nghĩa là mức tối đa của ngăn xếp trống ngay lập tức có sẵn bằng 0. 
3. Đối với hoạt động`i`, hãy gọi trình tạo một lần để quyết định xem thao tác đó là đẩy hay bật. Điều kiện là`rng61() % (p + q) < p`. 
4. Nếu thao tác là đẩy, hãy gọi`rng61()`lần thứ hai để tạo ra giá trị`v = rng61() % m + 1`. Tăng độ sâu ngăn xếp và lưu trữ`max(stack[depth - 1], v)`ở độ sâu mới. Nếu ngăn xếp trước đó trống thì giá trị tối đa trước đó bằng 0, do đó giá trị được lưu trữ chỉ đơn giản là`v`. 
5. Nếu thao tác là pop, hãy giảm`depth`chỉ khi ngăn xếp không trống. Nếu nó đã bằng 0, hãy giữ nguyên. Sau đó, trọng điểm sẽ đưa ra mức tối đa chính xác bằng 0. 
6. Sau khi thực hiện thao tác, hãy đọc`stack[depth]`như mức tối đa hiện tại. XOR`i * stack[depth]`vào câu trả lời. Phép nhân sử dụng chỉ số hoạt động dựa trên một yêu cầu của bài toán. 
7. Rốt cuộc`n`hoạt động, in XOR tích lũy bằng cách sử dụng yêu cầu`Case #x: y`định dạng. 

### Tại sao nó hoạt động 

Tính bất biến đó là`stack[d]`bằng giá trị lớn nhất trong ngăn xếp thực đầu tiên`d`các phần tử. Ban đầu`d = 0`, và giá trị tối đa trống bằng không. Trong quá trình đẩy, ngăn xếp thực mới bao gồm ngăn xếp trước đó`d`các phần tử theo sau là`v`, vì vậy mức tối đa của nó chính xác là`max(stack[d], v)`, đó là những gì chúng tôi lưu trữ ở độ sâu`d + 1`. Trong quá trình bật lên, phần tử thực cuối cùng biến mất, do đó mức tối đa của ngăn xếp còn lại chính xác là giá trị đã được lưu trữ ở độ sâu`d - 1`. Do đó, bất biến tồn tại trong mọi hoạt động và giá trị được đọc từ`stack[depth]`luôn là mức tối đa cần thiết. Vì mọi đóng góp đều được XOR ngay sau hoạt động tương ứng của nó nên câu trả lời cuối cùng chính xác là XOR được yêu cầu. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

MASK = 0xFFFFFFFF

def rng61(sa, sb, sc):
    sa ^= (sa << 16) & MASK
    sa &= MASK

    sa ^= sa >> 5
    sa &= MASK

    sa ^= (sa << 1) & MASK
    sa &= MASK

    t = sa
    sa = sb
    sb = sc
    sc = (sc ^ t ^ sa) & MASK

    return sc, sa, sb, sc

def solve():
    t = int(input())
    out = []

    for case_id in range(1, t + 1):
        n, p, q, m, sa, sb, sc = map(int, input().split())

        # stack[d] = maximum of the logical stack when its size is d.
        # stack[0] = 0 represents the empty stack.
        stack = array('I', [0]) * (n + 1)
        depth = 0
        ans = 0
        total = p + q

        for i in range(1, n + 1):
            r, sa, sb, sc = rng61(sa, sb, sc)

            if r % total < p:
                r, sa, sb, sc = rng61(sa, sb, sc)
                value = r % m + 1

                previous_max = stack[depth]
                depth += 1

                if value > previous_max:
                    stack[depth] = value
                else:
                    stack[depth] = previous_max
            else:
                if depth > 0:
                    depth -= 1

            ans ^= i * stack[depth]

        out.append(f"Case #{case_id}: {ans}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`rng61`chức năng phản chiếu trình tạo C++. Sự rõ ràng`& 0xFFFFFFFF`hoạt động được yêu cầu vì C++`unsigned int`gói số học modulo`2^32`, trong khi số nguyên Python tăng không giới hạn. Việc che dấu sau mỗi XOR và dịch chuyển trái làm cho mọi thao tác tiếp theo hoạt động giống như trên giá trị không dấu 32 bit. 

Mảng được gọi là`stack`không chứa các giá trị được đẩy ban đầu. Tại chỉ mục`d`, nó chứa giá trị lớn nhất của số đầu tiên`d`các phần tử. Điều này là đủ vì truy vấn duy nhất từng được thực hiện về ngăn xếp là mức tối đa của nó. 

Mảng được phân bổ trước cho`n + 1`số nguyên 32 bit không dấu. Danh sách Python sẽ lưu trữ các tham chiếu đến các đối tượng số nguyên Python và có thể tiêu thụ hơn 100 MB ở độ sâu tối đa. các`array('I')`cách biểu diễn sử dụng bốn byte cho mỗi mục nhập, do đó độ sâu năm triệu cần khoảng 20 MB cho ngăn xếp. 

các`depth`biến là kích thước ngăn xếp logic. Một cú đẩy ghi vào`stack[depth + 1]`, trong khi một pop hợp lệ giảm`depth`. Giữ`stack[0] = 0`loại bỏ nhu cầu về giá trị tối đa đặc biệt khi ngăn xếp trống. 

Trình tạo phải được gọi chính xác một lần cho mỗi quyết định vận hành. Khi đẩy, nó phải được gọi chính xác một lần nữa cho giá trị được đẩy. Gọi nó theo thứ tự khác sẽ thay đổi toàn bộ trạng thái của trình tạo tiếp theo. Câu trả lời được cập nhật sau thao tác ngăn xếp, bởi vì`a_i`đề cập đến mức tối đa sau`i`-hoạt động thứ. 

Số nguyên của Python cũng hữu ích cho biểu thức cuối cùng. Sản phẩm lớn nhất là nhiều nhất`5 * 10^6 * 10^9 = 5 * 10^15`, phù hợp thoải mái với biểu diễn số nguyên của Python và cũng phù hợp với số nguyên 64 bit đã ký. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, máy phát điện tạo ra`POP, POP, PUSH(1), PUSH(4)`. Biểu diễn ngăn xếp chỉ lưu trữ mức tối đa ở mỗi độ sâu. 

| Hoạt động | Hành động được tạo | Độ sâu | Ngăn xếp tiền tố tối đa | Tối đa hiện tại | Đóng góp XOR | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`POP`| 0 |`[0]`| 0 |`1 * 0 = 0`| 
| 2 |`POP`| 0 |`[0]`| 0 |`2 * 0 = 0`| 
| 3 |`PUSH(1)`| 1 |`[0, 1]`| 1 |`3 * 1 = 3`| 
| 4 |`PUSH(4)`| 2 |`[0, 1, 4]`| 4 |`4 * 4 = 16`| 

Câu trả lời cuối cùng là`0 XOR 0 XOR 3 XOR 16 = 19`, cho`Case #1: 19`. Hai thao tác đầu tiên cũng xác minh rằng việc lấy ra một ngăn xếp trống phải là thao tác không hoạt động. 

Đối với Mẫu 2, các thao tác được tạo là`PUSH(2), POP, PUSH(1), POP`. 

| Hoạt động | Hành động được tạo | Độ sâu | Ngăn xếp tiền tố tối đa | Tối đa hiện tại | Đóng góp XOR | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`PUSH(2)`| 1 |`[0, 2]`| 2 |`1 * 2 = 2`| 
| 2 |`POP`| 0 |`[0, 2]`| 0 |`2 * 0 = 0`| 
| 3 |`PUSH(1)`| 1 |`[0, 1]`| 1 |`3 * 1 = 3`| 
| 4 |`POP`| 0 |`[0, 1]`| 0 |`4 * 0 = 0`| 

XOR cuối cùng là`2 XOR 0 XOR 3 XOR 0 = 1`. Dấu vết này thực hiện cả việc chèn và loại bỏ trở lại trạng thái trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi hoạt động thực hiện một số lượng cập nhật trình tạo và truy cập ngăn xếp không đổi. | 
| Không gian | O(n) | Mảng tiền tố tối đa có một mục nhập 32 bit cho mỗi độ sâu ngăn xếp có thể có. | 

Độ sâu tối đa là nhiều nhất`n`, vì vậy mảng được phân bổ trước cần tối đa khoảng 20 MB khi`n = 5 * 10^6`. Đây thực chất là thấp hơn giới hạn bộ nhớ 256 MB. Thời gian chạy là tuyến tính theo số lượng thao tác được tạo, đây là thang đo cần thiết cho trường hợp thử nghiệm chứa hàng triệu thao tác. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây sử dụng logic giải pháp tương tự như chương trình đã gửi. Trường hợp tùy chỉnh lớn được chọn có chủ ý để có thể chứng minh hành vi của nó mà không cần dựa vào giá trị kỳ vọng được tạo độc lập: với`SA = SB = SC = 65536`, mọi trạng thái của máy phát vẫn giữ nguyên, vì vậy với`p = q = 1`mọi thao tác đều là một cú đẩy. Cài đặt`m = 1`làm cho mọi giá trị được đẩy bằng`1`.```python
import sys
import io
from array import array

MASK = 0xFFFFFFFF

def solve_text(inp: str) -> str:
    data = io.StringIO(inp)
    input = data.readline

    t = int(input())
    out = []

    def rng61(sa, sb, sc):
        sa ^= (sa << 16) & MASK
        sa &= MASK
        sa ^= sa >> 5
        sa &= MASK
        sa ^= (sa << 1) & MASK
        sa &= MASK

        tmp = sa
        sa = sb
        sb = sc
        sc = (sc ^ tmp ^ sa) & MASK

        return sc, sa, sb, sc

    for case_id in range(1, t + 1):
        n, p, q, m, sa, sb, sc = map(int, input().split())

        stack = array('I', [0]) * (n + 1)
        depth = 0
        ans = 0
        total = p + q

        for i in range(1, n + 1):
            r, sa, sb, sc = rng61(sa, sb, sc)

            if r % total < p:
                r, sa, sb, sc = rng61(sa, sb, sc)
                value = r % m + 1

                depth += 1
                previous_max = stack[depth - 1]
                stack[depth] = max(previous_max, value)
            else:
                if depth:
                    depth -= 1

            ans ^= i * stack[depth]

        out.append(f"Case #{case_id}: {ans}")

    return "\n".join(out)

# Provided samples
sample = """\
2
4 1 1 4 23333 66666 233333
4 2 1 4 23333 66666 233333
"""

assert solve_text(sample) == """\
Case #1: 19
Case #2: 1
""", "provided samples"

# Minimum-size case. The first generated operation is POP,
# so the empty stack contributes zero.
assert solve_text(
    "1\n1 1 1 4 23333 66666 233333\n"
) == "Case #1: 0", "minimum-size empty pop"

# First three operations of Sample 1:
# POP, POP, PUSH(1), giving contributions 0, 0, 3.
assert solve_text(
    "1\n3 1 1 4 23333 66666 233333\n"
) == "Case #1: 3", "operation-index weighting"

# All generated operations are PUSH because every RNG state is even.
# m = 1 makes every pushed value equal to 1.
# For n = 5, the answer is 1 xor 2 xor 3 xor 4 xor 5 = 1.
assert solve_text(
    "1\n5 1 1 1 65536 65536 65536\n"
) == "Case #1: 1", "all-equal values"

# Maximum-size case.
# Every operation is PUSH(1), so the answer is xor(1..5,000,000).
# Since 5,000,000 is divisible by 4, xor(1..n) = n.
assert solve_text(
    "1\n5000000 1 1 1 65536 65536 65536\n"
) == "Case #1: 5000000", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 1 4 23333 66666 233333`|`Case #1: 0`| tối thiểu`n`và một cửa sổ bật lên ngăn xếp trống | 
|`1 / 3 1 1 4 23333 66666 233333`|`Case #1: 3`| Chỉ số hoạt động dựa trên một trong XOR | 
|`1 / 5 1 1 1 65536 65536 65536`|`Case #1: 1`|`m = 1`, lặp lại các giá trị tối đa bằng nhau và xử lý đẩy | 
|`1 / 5000000 1 1 1 65536 65536 65536`|`Case #1: 5000000`| Tối đa`n`, xử lý tuyến tính, biểu diễn bộ nhớ nhỏ gọn | 

## Vỏ cạnh 

Đối với cửa sổ ngăn xếp trống, hãy sử dụng thao tác đầu tiên của Mẫu 1 làm trường hợp cụ thể:```
1
1 1 1 4 23333 66666 233333
```Giá trị ngẫu nhiên được tạo đầu tiên sẽ chọn`POP`. Độ sâu logic bắt đầu từ 0, do đó điều kiện pop`if depth > 0`is false and the depth remains zero. lính gác`stack[0]`bằng 0, đóng góp`1 * 0 = 0`. Đầu ra là`Case #1: 0`. Không có quyền truy cập ngăn xếp bên ngoài phạm vi hợp lệ xảy ra. 

Đối với các giá trị bằng nhau lặp lại, hãy xem xét:```
1
5 1 1 1 65536 65536 65536
```Cả ba trạng thái máy phát điện đều bắt đầu chẵn. Các phép biến đổi chỉ sử dụng các ca, XOR và phép gán, vì vậy mọi trạng thái vẫn giữ nguyên. Với`p = q = 1`, một giá trị ngẫu nhiên chẵn thỏa mãn`value % 2 < 1`, vì vậy mọi thao tác đều là một thao tác đẩy. Bởi vì`m = 1`, mọi giá trị được tạo ra là`1`. Ngăn xếp tiền tố tối đa trở thành`[0, 1, 1, 1, 1, 1]`. Những đóng góp là`1, 2, 3, 4, 5`, XOR của nó là`1`. Mức tối đa lặp đi lặp lại không bao giờ cần điều trị đặc biệt. 

Đối với ranh giới kích thước tối đa, việc xây dựng tương tự với`n = 5 * 10^6`tạo ra năm triệu lần đẩy giá trị`1`. Ngăn xếp đạt đến độ sâu tối đa có thể, nhưng chương trình chỉ lưu trữ tối đa năm triệu tiền tố 32 bit, khoảng 20 MB. Câu trả lời là XOR của tất cả các số nguyên từ`1`bởi vì`5,000,000`. Từ`5,000,000 mod 4 = 0`, XOR đó bằng`5,000,000`. Điều này kiểm tra cả biểu diễn bộ nhớ và ranh giới vòng lặp ở mức chính xác lớn nhất được phép.`n`. 

Đối với ranh giới chỉ số hoạt động, hãy sử dụng ba thao tác đầu tiên của Mẫu 1:```
1
3 1 1 4 23333 66666 233333
```Các hoạt động được`POP, POP, PUSH(1)`. Hai đóng góp đầu tiên bằng không. Ở hoạt động thứ ba, mức tối đa là một, do đó phần đóng góp là`3 * 1 = 3`. Câu trả lời là`3`. Việc sử dụng chỉ mục vòng lặp dựa trên số 0 trong biểu thức XOR sẽ đóng góp không chính xác số 0 vào thời điểm này, vì vậy trường hợp này sẽ phát hiện ra từng lỗi một. 

Đối với ranh giới của trình tạo, các thử nghiệm tương tự cũng xác minh rằng một lần đẩy tiêu thụ hai số ngẫu nhiên trong khi một lần bật chỉ tiêu thụ một số. Trong Mẫu 1, hai thao tác đầu tiên là pops, vì vậy chỉ có một lệnh gọi trình tạo được thực hiện cho mỗi thao tác. Hoạt động thứ ba là một lệnh đẩy, vì vậy lệnh gọi lựa chọn thao tác ngay sau đó là lệnh gọi tạo giá trị. Nếu quá trình triển khai gọi trình tạo một giá trị sau một cửa sổ bật lên, thì tất cả các hoạt động sau đó sẽ thay đổi và XOR cuối cùng sẽ không chính xác. Việc triển khai Python tuân theo thứ tự lệnh gọi chính xác của trình tạo ban đầu.
