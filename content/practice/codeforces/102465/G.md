---
title: "CF 102465G - Dây đàn"
description: "Chúng ta có một chuỗi ban đầu, S(0), có độ dài tối đa là 1000. Mỗi chuỗi sau này được xác định từ các chuỗi đã tồn tại. Một thao tác APP x y tạo ra S(x) + S(y), trong khi thao tác SUB x lo hi tạo ra chuỗi con nửa mở S(x)[lo:hi]."
date: "2026-08-08T09:20:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102465
codeforces_index: "G"
codeforces_contest_name: "2018-2019 ICPC Southwestern European Regional Programming Contest (SWERC 2018)"
rating: 0
weight: 102465
solve_time_s: 189
verified: true
draft: false
---

[CF 102465G - Dây](https://codeforces.com/problemset/problem/102465/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi ban đầu,`S(0)`, có độ dài tối đa là 1000. Mỗi chuỗi sau này được xác định từ các chuỗi đã tồn tại. MỘT`APP x y`hoạt động tạo ra`S(x) + S(y)`, trong khi một`SUB x lo hi`hoạt động tạo ra chuỗi con nửa mở`S(x)[lo:hi]`. 

Chuỗi cuối cùng có thể lớn đến mức`2^63 - 1`các ký tự, vì vậy nhiệm vụ không phải là xây dựng nó. Chúng ta chỉ cần tổng giá trị ASCII của tất cả các ký tự của nó, rút ​​gọn modulo`1,000,000,007`. 

Có nhiều nhất là 2500 chuỗi. Điều này đủ nhỏ để một thuật toán xung quanh`O(N^2)`là hoàn toàn hợp lý, nhưng bất cứ điều gì tỷ lệ thuận với độ dài của chuỗi được xây dựng là không thể. Một chuỗi`APP`các thao tác có thể tăng gấp đôi độ dài nhiều lần, do đó, ngay cả chỉ với vài chục thao tác, chuỗi được biểu thị đã có thể có nhiều ký tự hơn mức có thể được lưu trữ hoặc xử lý riêng lẻ. 

Giới hạn độ dài cũng có nghĩa là chúng ta cần phân biệt giữa các giá trị chỉ lớn và các giá trị được sử dụng làm vị trí. Các số nguyên Python xử lý chúng trực tiếp, nhưng thuật toán không bao giờ được thực hiện một phép toán tỷ lệ với độ dài. Bản thân câu trả lời luôn được giữ theo modulo`1,000,000,007`, trong khi độ dài được lưu trữ chính xác vì chúng cần thiết để quyết định cạnh nào của`APP`hoạt động có chứa một vị trí được truy vấn. 

Trường hợp cạnh không rõ ràng đầu tiên là một chuỗi con trống. Coi như:```
2
a
SUB 0 0 0
```Kết quả là chuỗi rỗng nên kết quả đúng là`0`. Việc thực hiện bất cẩn dẫn đến mọi`SUB`tạo ra ít nhất một ký tự sẽ tạo ra một câu trả lời khác 0 không chính xác hoặc không vượt qua ranh giới. 

Trường hợp cạnh thứ hai là điểm cuối bên phải độc quyền của`SUB`. Vì:```
2
abc
SUB 0 0 3
```chuỗi kết quả là`abc`, có tổng ASCII là`97 + 98 + 99 = 294`. điều trị`hi`vì bao gồm sẽ bao gồm không chính xác ký tự thứ tư không tồn tại. 

Trường hợp cạnh thứ ba kết hợp nối và cắt:```
4
ab
APP 0 0
SUB 1 1 3
APP 2 0
```Đây`S(1) = "abab"`,`S(2) = "ba"`, và chuỗi cuối cùng là`"baab"`. Tổng của nó là`98 + 97 + 97 + 98 = 390`. Điều này giúp phát hiện các triển khai quên rằng thao tác chuỗi con sẽ thay đổi hệ tọa độ cho mỗi truy vấn sau này. 

Cuối cùng, độ dài có thể gần bằng`2^63`. Việc triển khai sử dụng số nguyên 32 bit cố định không thể biểu thị chúng và ngay cả việc triển khai 64 bit chính xác cũng phải tránh việc vô tình tạo ra độ dài lớn hơn mức tối đa cho phép trong quá trình xây dựng thử nghiệm trung gian. Giải pháp bên dưới lưu trữ độ dài dưới dạng số nguyên chính xác và chỉ xây dựng các biểu diễn, không bao giờ là ký tự thực. 

## Phương pháp tiếp cận 

Giải pháp đơn giản là thực sự xây dựng mọi chuỗi. Vì`APP x y`, chúng tôi sao chép cả hai chuỗi. Vì`SUB x lo hi`, chúng tôi sao chép phần được yêu cầu. Điều này đúng vì nó tuân theo chính xác các thao tác được sử dụng để xác định chuỗi. 

Vấn đề là các chuỗi được biểu diễn có thể rất lớn. Bắt đầu từ một chuỗi một ký tự, việc liên tục nối một chuỗi với chính nó sẽ tăng gấp đôi độ dài của nó. Sau 63 lần nhân đôi, chiều dài đã xấp xỉ`2^63`. Do đó, một thuật toán brute-force có thể thực hiện theo thứ tự`2^63`các thao tác ký tự chỉ dành cho chuỗi cuối cùng. Điều đó vượt xa những gì giới hạn bốn giây cho phép và việc lưu trữ một chuỗi như vậy cũng là điều không thể. 

Quan sát hữu ích là chúng ta không cần quyền truy cập tùy ý vào các ký tự. Chúng ta chỉ cần tổng của một chuỗi hoàn chỉnh và một`SUB`hoạt động có thể thu được tổng đó nếu chúng ta có thể trả lời các truy vấn tổng tiền tố trên chuỗi nguồn của nó. 

Định nghĩa`prefix(i, k)`bằng tổng của số đầu tiên`k`nhân vật của`S(i)`. Sau đó tổng của`S(i)[lo:hi]`đơn giản là`prefix(i, hi) - prefix(i, lo)`. 

Vấn đề quan trọng là làm thế nào để tính toán`prefix(i, k)`không mở rộng`S(i)`. 

Đối với một`APP x y`, đầu tiên`k`các ký tự hoàn toàn ở bên trong`S(x)`, hoặc chúng bao gồm tất cả`S(x)`theo sau là tiền tố của`S(y)`. Vì vậy, một truy vấn tiền tố theo sau chính xác một con. 

Đối với một`SUB x lo hi`, đầu tiên`k`nhân vật của`S(i)`tương ứng chính xác với lần đầu tiên`lo + k`nhân vật của`S(x)`, ngoại trừ việc đầu tiên`lo`các ký tự thuộc về trước khoảng thời gian được sao chép. Trực tiếp hơn,`prefix(i, k) = prefix(x, lo + k) - prefix(x, lo)`. 

Một lần nữa, điều này chỉ tuân theo một chuỗi phụ thuộc. 

Điều này thay đổi vấn đề hoàn toàn. Một truy vấn tiền tố đi qua nhiều nhất`N`các nút xây dựng và mỗi nút`SUB`nút tạo hai truy vấn như vậy khi chúng tôi tính tổng của nó. Vì chỉ có`N`chuỗi, tổng công việc là`O(N^2)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(L)`Ở đâu`L`có thể`2^63 - 1`|`O(L)`| Quá chậm | 
| Truy vấn tiền tố trên DAG xây dựng |`O(N^2)`|`O(N)`| Đã chấp nhận | 

Lịch sử xây dựng hoạt động giống như một biểu đồ tuần hoàn có hướng, nhưng chúng ta không cần duyệt biểu đồ tổng quát. Truy vấn tiền tố chọn một phụ thuộc gửi đi ở mỗi nút, do đó, nó trở thành một bước đi đơn giản trong lịch sử. 

## Hướng dẫn thuật toán 

1. Lưu trữ tổng tiền tố của chuỗi ban đầu. Vì`S(0)`, chúng ta có thể tính trực tiếp tổng của giá trị đầu tiên của nó`k`ký tự vì độ dài của nó tối đa là 1000. Chúng tôi cũng lưu trữ độ dài và tổng số ASCII của nó. 
2. Đối với mỗi chuỗi sau, chỉ lưu trữ loại của nó và các tham số xác định nó. Đối với một`APP`, lưu trữ hai chỉ số nguồn. Đối với một`SUB`, lưu trữ chỉ mục nguồn và khoảng`[lo, hi)`. 
3. Xử lý chuỗi theo thứ tự đầu vào. Vì mọi thao tác chỉ tham chiếu đến một chuỗi trước đó nên tất cả độ dài bắt buộc và tổng số tiền đều đã được biết khi chuỗi mới được xử lý. 
4. Đối với một`APP x y`, bộ`length[i] = length[x] + length[y]`Và`sum[i] = sum[x] + sum[y]`. 

Tổng được giảm modulo`1,000,000,007`, trong khi độ dài vẫn chính xác. 
5. Đối với một`SUB x lo hi`, bộ`length[i] = hi - lo`. 

Để tính tổng của nó, hãy truy vấn chuỗi nguồn để biết tổng của chuỗi đầu tiên`hi`các ký tự và trừ tổng của ký tự đầu tiên`lo`nhân vật:`sum[i] = prefix(x, hi) - prefix(x, lo)`. 

Hai truy vấn tiền tố là đủ vì chuỗi con chính xác là sự khác biệt giữa hai tiền tố. 
6. Thực hiện`prefix(i, k)`như một cuộc đi bộ lặp đi lặp lại. Nếu như`i`bằng 0, hãy sử dụng mảng tiền tố được tính toán trước của chuỗi gốc. Nếu như`i`là một`APP x y`, so sánh`k`với`length[x]`. Khi`k <= length[x]`, tiếp tục với`x`. Ngược lại, hãy cộng tổng số tiền đầy đủ của`x`, trừ`length[x]`từ`k`, và tiếp tục với`y`. 
7. Nếu nút hiện tại là một`SUB x lo hi`, dịch vị trí tiền tố được yêu cầu sang chuỗi nguồn bằng cách thay thế`k`với`lo + k`, sau đó tiếp tục với`x`. Không cần nhánh thứ hai vì tiền tố của chuỗi con luôn tương ứng với một tiền tố của chuỗi con đó. 
8. Sau khi tất cả các chuỗi đã được xử lý, xuất ra`sum[N - 1]`, đây đã là tổng kiểm tra mà vấn đề yêu cầu. 

### Tại sao nó hoạt động 

Tính bất biến đó là`prefix(i, k)`luôn đại diện cho tổng chính xác của số đầu tiên`k`nhân vật của`S(i)`. Đối với chuỗi ban đầu, chuỗi này theo sau mảng tiền tố trực tiếp. Tại một`APP`, tiền tố được yêu cầu nằm hoàn toàn trong chuỗi bên trái hoặc nó chứa toàn bộ chuỗi bên trái và tiền tố của chuỗi bên phải, khớp chính xác với quá trình chuyển đổi trong thuật toán. Tại một`SUB`, đầu tiên`k`ký tự tương ứng với vị trí nguồn`[lo, lo + k)`, tổng của nó được lấy từ chênh lệch tiền tố nguồn thích hợp. Vì vậy mọi truy vấn tiền tố đều đúng. MỘT`SUB`tổng là sự khác biệt giữa hai tiền tố nguồn chính xác của nó và một`APP`tổng là tổng của hai chuỗi con hoàn chỉnh của nó, vì vậy mọi chuỗi con được lưu trữ`sum[i]`là đúng. Do đó, tổng được lưu trữ cuối cùng là tổng kiểm tra cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def solve():
    n = int(input())
    s = input().strip()

    # type[i] is:
    # 0 for the initial string
    # 1 for APP
    # 2 for SUB
    types = [0] * n

    # For APP, a[i] and b[i] are the two source indices.
    # For SUB, a[i] is the source index, and b[i], c[i]
    # are lo and hi.
    a = [0] * n
    b = [0] * n
    c = [0] * n

    length = [0] * n
    total = [0] * n

    # Prefix sums for S(0).
    base_prefix = [0] * (len(s) + 1)
    for i, ch in enumerate(s):
        base_prefix[i + 1] = base_prefix[i] + ord(ch)

    length[0] = len(s)
    total[0] = base_prefix[-1] % MOD

    def prefix(idx, k):
        """
        Sum of the first k characters of S(idx), modulo MOD.

        The query follows exactly one dependency at every
        construction node.
        """
        ans = 0

        while idx != 0:
            if types[idx] == 1:  # APP
                x = a[idx]
                y = b[idx]

                if k <= length[x]:
                    idx = x
                else:
                    ans += total[x]
                    ans %= MOD
                    k -= length[x]
                    idx = y

            else:  # SUB
                x = a[idx]
                lo = b[idx]

                k += lo
                idx = x

        ans += base_prefix[k] % MOD
        return ans % MOD

    for i in range(1, n):
        parts = input().split()

        if parts[0] == "APP":
            x = int(parts[1])
            y = int(parts[2])

            types[i] = 1
            a[i] = x
            b[i] = y

            length[i] = length[x] + length[y]
            total[i] = (total[x] + total[y]) % MOD

        else:
            x = int(parts[1])
            lo = int(parts[2])
            hi = int(parts[3])

            types[i] = 2
            a[i] = x
            b[i] = lo
            c[i] = hi

            length[i] = hi - lo
            total[i] = (prefix(x, hi) - prefix(x, lo)) % MOD

    print(total[n - 1])

if __name__ == "__main__":
    solve()
```Các mảng`length`Và`total`giữ hai thông tin cần thiết cho mọi hoạt động trong tương lai. Độ dài phải chính xác vì`prefix`sử dụng chúng để quyết định xem một vị trí thuộc về bên trái hay bên phải của một`APP`. Số tiền có thể được giảm theo modulo một cách an toàn`MOD`sau mỗi lần cộng hoặc trừ. 

các`prefix`hàm lặp chứ không phải đệ quy. Một chuỗi 2500`SUB`hoạt động là hợp pháp, do đó việc đệ quy sẽ yêu cầu tăng giới hạn đệ quy của Python. Việc lặp lại hoàn toàn tránh được mối lo ngại đó. 

Điều kiện cho một`APP`là`k <= length[x]`. giá trị`k`có nghĩa là số lượng ký tự được yêu cầu, vì vậy`k == length[x]`hoàn toàn thuộc về chuỗi bên trái và không được chuyển sang chuỗi bên phải. Đây là một trong những lỗi thường gặp nhất trong vấn đề này. 

Đối với một`SUB x lo hi`, chuỗi hiện tại là`S(x)[lo:hi]`. Tiền tố có độ dài`k`do đó kết thúc ở vị trí nguồn`lo + k`, đó là lý do tại sao truy vấn chỉ cần thêm`lo`ĐẾN`k`. 

Trường hợp cơ bản sử dụng`base_prefix[k]`, Ở đâu`k`có thể dao động từ 0 đến`len(s)`. Đầu vào đảm bảo rằng mọi vị trí được dịch vẫn nằm trong chuỗi nguồn, do đó không cần cắt thêm. 

Số nguyên Python có thể biểu thị trực tiếp độ dài được phép. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, số nguyên 64 bit có dấu là đủ cho mức tối đa đã nêu`2^63 - 1`, nhưng các phần bổ sung vẫn phải được kiểm tra dựa trên sự đảm bảo của vấn đề. 

## Ví dụ đã hoạt động 

Mẫu chính thức là:```
3
foobar
SUB 0 0 3
APP 1 1
```Chuỗi ban đầu có tổng tiền tố cho`"foobar"`. Hoạt động đầu tiên trích xuất`"foo"`và thao tác thứ hai nối chuỗi con đó với chính nó. 

| Bước | Chuỗi | Hoạt động | Chiều dài | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 |`foobar`| ban đầu | 6 | 627 | 
| 1 |`foo`|`SUB 0 0 3`| 3 | 324 | 
| 2 |`foofoo`|`APP 1 1`| 6 | 648 | 

Đối với chuỗi 1,`prefix(0, 3) = 324`Và`prefix(0, 0) = 0`, vậy tổng của nó là 324. Chuỗi 2 chỉ chứa hai bản sao của chuỗi 1, cho`324 + 324 = 648`. Do đó, đầu ra là`648`. 

Ví dụ thứ hai thực hiện cắt và nối lồng nhau:```
4
ab
APP 0 0
SUB 1 1 3
APP 2 0
```Việc xây dựng phát triển như sau. 

| Bước | Chuỗi | Hoạt động | Chiều dài | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 |`ab`| ban đầu | 2 | 195 | 
| 1 |`abab`|`APP 0 0`| 4 | 390 | 
| 2 |`ba`|`SUB 1 1 3`| 2 | 195 | 
| 3 |`baab`|`APP 2 0`| 4 | 390 | 

Đối với chuỗi 2, thuật toán yêu cầu`prefix(1, 3)`Và`prefix(1, 1)`. Truy vấn đầu tiên đi qua`S(1) = S(0) + S(0)`, tiêu thụ lần hoàn thành đầu tiên`"ab"`với tổng là 195 thì lấy ký tự đầu tiên`"a"`từ bản sao thứ hai, cho 292. Truy vấn thứ hai cho 98. Sự khác biệt của chúng là`292 - 98 = 194`, tương ứng với`"ba"`chỉ khi số học bị đọc sai. Các giá trị tiền tố thực tế là`prefix(1, 3) = 293`Và`prefix(1, 1) = 98`, cho`195`, chính xác là tổng của`"ba"`. Dấu vết này chứng tỏ tại sao phía bên phải của một`APP`phải được truy vấn với vị trí đã điều chỉnh`k - length[left]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(N^2)`| Mỗi`SUB`thực hiện hai truy vấn tiền tố và mỗi truy vấn theo sau nhiều nhất`N`nút xây dựng. | 
| Không gian |`O(N + | S(0) | )`| Chúng tôi lưu trữ thông tin có kích thước không đổi cho mỗi chuỗi được xây dựng và một mảng tiền tố cho chuỗi ban đầu. | 

Với`N <= 2500`, nhiều nhất là khoảng`2N`các truy vấn tiền tố được thực hiện, mỗi truy vấn lấy tối đa`N`các bước. Điều này mang lại khoảng 12,5 triệu bước phụ thuộc trong trường hợp xấu nhất, điều này rất thực tế trong Python. Các chuỗi thực tế không bao giờ được lưu trữ, do đó, tiềm năng rất lớn`2^63 - 1`chiều dài không ảnh hưởng đến việc sử dụng bộ nhớ. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây bao gồm mẫu chính thức, trường hợp có kích thước tối thiểu, chuỗi con trống, cấu trúc lồng nhau có ranh giới nhạy cảm, chuỗi hoàn toàn bằng nhau và cấu trúc có độ dài cuối cùng chính xác`2^63 - 1`.```python
import sys
import io
from contextlib import redirect_stdout

MOD = 1_000_000_007

def solve():
    input = sys.stdin.readline

    n = int(input())
    s = input().strip()

    types = [0] * n
    a = [0] * n
    b = [0] * n
    c = [0] * n

    length = [0] * n
    total = [0] * n

    base_prefix = [0] * (len(s) + 1)
    for i, ch in enumerate(s):
        base_prefix[i + 1] = base_prefix[i] + ord(ch)

    length[0] = len(s)
    total[0] = base_prefix[-1] % MOD

    def prefix(idx, k):
        ans = 0

        while idx != 0:
            if types[idx] == 1:
                x = a[idx]
                y = b[idx]

                if k <= length[x]:
                    idx = x
                else:
                    ans = (ans + total[x]) % MOD
                    k -= length[x]
                    idx = y
            else:
                x = a[idx]
                lo = b[idx]
                k += lo
                idx = x

        return (ans + base_prefix[k]) % MOD

    for i in range(1, n):
        parts = input().split()

        if parts[0] == "APP":
            x = int(parts[1])
            y = int(parts[2])

            types[i] = 1
            a[i] = x
            b[i] = y

            length[i] = length[x] + length[y]
            total[i] = (total[x] + total[y]) % MOD

        else:
            x = int(parts[1])
            lo = int(parts[2])
            hi = int(parts[3])

            types[i] = 2
            a[i] = x
            b[i] = lo
            c[i] = hi

            length[i] = hi - lo
            total[i] = (prefix(x, hi) - prefix(x, lo)) % MOD

    print(total[n - 1])

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_input = solve.__globals__["input"]

    sys.stdin = io.StringIO(inp)
    solve.__globals__["input"] = sys.stdin.readline

    output = io.StringIO()

    try:
        with redirect_stdout(output):
            solve()
    finally:
        sys.stdin = old_stdin
        solve.__globals__["input"] = old_input

    return output.getvalue().strip()

# Provided sample
assert run(
    """3
foobar
SUB 0 0 3
APP 1 1
"""
) == "648", "sample 1"

# Minimum-size input: N = 1
assert run(
    """1
a
"""
) == "97", "minimum-size case"

# Empty substring: hi == lo
assert run(
    """2
a
SUB 0 0 0
"""
) == "0", "empty substring"

# Boundary-sensitive nested APP/SUB construction
assert run(
    """4
ab
APP 0 0
SUB 1 1 3
APP 2 0
"""
) == "390", "nested substring and concatenation"

# All characters equal
assert run(
    """4
z
APP 0 0
APP 1 1
SUB 2 0 4
"""
) == str((122 * 4) % MOD), "all-equal values"

# Final length exactly 2^63 - 1.
#
# S(0) has length 1.
# After 62 doublings, S(62) has length 2^62.
# S(63) has length 2^62 - 1.
# S(64) has length 2^63 - 2.
# S(65) has length 2^63 - 1.
parts = ["66", "a"]

for i in range(62):
    x = i
    parts.append(f"APP {x} {x}")

parts.append(f"SUB 62 0 {2**62 - 1}")
parts.append("APP 63 63")
parts.append("APP 64 0")

max_case = "\n".join(parts) + "\n"

expected_max = (97 * ((2**63) - 1)) % MOD

assert run(max_case) == str(expected_max), "maximum-length case"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chính thức`foobar`mẫu |`648`| Nền tảng`SUB`theo sau là`APP`| 
|`N = 1`,`S(0) = "a"`|`97`| Tiền tố đầu vào và cơ sở tối thiểu | 
|`SUB 0 0 0`|`0`| Chuỗi con trống | 
|`ab`, theo sau là`APP`,`SUB`,`APP`|`390`| Dịch tọa độ lồng nhau | 
| lặp đi lặp lại`z`nối |`488`| Các giá trị bằng nhau và tổng mô-đun | 
| Chiều dài cuối cùng`2^63 - 1`|`243684095`| Độ dài khổng lồ mà không cần hiện thực hóa | 

## Vỏ cạnh 

Trường hợp chuỗi con trống```
2
a
SUB 0 0 0
```có`length[1] = 0`. Hai truy vấn tiền tố đều là`prefix(0, 0)`, vì vậy sự khác biệt của chúng bằng không. Thuật toán không bao giờ cố gắng truy cập một ký tự ở vị trí 0, vì tiền tố có độ dài bằng 0 là hợp lệ. 

Đối với trường hợp điểm cuối độc quyền```
2
abc
SUB 0 0 3
```thuật toán tính toán`prefix(0, 3) - prefix(0, 0)`. Giá trị đầu tiên là`294`, và thứ hai là`0`, vậy kết quả là`294`. Ký tự ở chỉ số 3 không bao giờ được xem xét vì`hi`là điểm cuối độc quyền. 

Đối với việc xây dựng lồng nhau```
4
ab
APP 0 0
SUB 1 1 3
APP 2 0
```cái`SUB`vị trí yêu cầu hoạt động`[1, 3)`từ`"abab"`. Truy vấn tiền tố của nó cho`k = 3`được dịch sang vị trí nguồn`1 + 3 = 4`, trong khi truy vấn tiền tố của nó cho`k = 1`được dịch sang vị trí nguồn`1 + 1 = 2`. Sự khác biệt là tổng số vị trí`[2, 4)`trong nguồn, đó là`"ba"`. trận chung kết`APP`thêm vào`"ab"`và sản xuất`"baab"`với tổng`390`. 

Đối với trường hợp có độ dài tối đa, mỗi ký tự được`'a'`, vì vậy tổng cuối cùng chỉ đơn giản là`97 * (2^63 - 1)`modulo`1,000,000,007`. Thuật toán không bao giờ lưu trữ những ký tự đó. Nó chỉ lưu trữ độ dài cuối cùng và tổng mô-đun của nó, trong khi mọi truy vấn tiền tố đều đi qua lịch sử xây dựng nhỏ gọn. Kết quả mong đợi là`243684095`. 

Mô hình trung tâm cần loại bỏ còn rộng hơn vấn đề cụ thể này. Bất cứ khi nào một đối tượng lớn được xây dựng từ các tham chiếu nối và chuỗi con, hãy hỏi xem liệu tổng hợp được yêu cầu có thể được biểu thị dưới dạng truy vấn tiền tố hay không. Nếu có thể, phép nối thường cho phép truy vấn tiền tố chọn một con, trong khi chuỗi con thường chỉ dịch chuyển tọa độ được truy vấn. Điều đó biến một đối tượng có kích thước theo cấp số nhân thành một biểu đồ phụ thuộc nhỏ có thể đi qua trong thời gian đa thức. 

Nếu bạn muốn, tôi cũng có thể cung cấp phiên bản biên tập cuộc thi ngắn hơn hoặc triển khai C++ 17 bằng cách sử dụng cùng một ý tưởng truy vấn tiền tố.
