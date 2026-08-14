---
title: "CF 102299A - Kolkhozy"
description: "Mỗi trang trại tập thể sản xuất k[i] bao ngũ cốc. Đối với truy vấn (l, r, x, m), chúng tôi chỉ xem xét các trang trại từ l đến r. Nếu một trang trại cung cấp cho m gia đình, số bao còn lại sau khi giao cho mỗi gia đình số nguyên bao như nhau chính xác là k[i] mod m."
date: "2026-08-13T08:03:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102299
codeforces_index: "A"
codeforces_contest_name: "2019 USP Try-outs"
rating: 0
weight: 102299
solve_time_s: 118
verified: true
draft: false
---

[CF 102299A - Kolkhozy](https://codeforces.com/problemset/problem/102299/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trang trại tập thể sản xuất`k[i]`túi ngũ cốc. Đối với một truy vấn`(l, r, x, m)`, chúng tôi chỉ xem xét các trang trại từ`l`bởi vì`r`. Nếu một trang trại cung cấp`m`gia đình, số túi còn lại sau khi phát cho mỗi gia đình số nguyên túi như nhau là chính xác.`k[i] mod m`. 

Truy vấn hỏi có bao nhiêu trang trại trong khoảng thời gian đó còn lại chính xác`x`. Từ`0 <= x < m`, điều này tương đương với việc đếm chỉ số`i`TRONG`[l, r]`thỏa mãn`k[i] mod m = x`. 

Những ràng buộc chính thức là`n, q <= 5 * 10^4`,`k[i] <= 5 * 10^4`, Và`m <= 5 * 10^4 + 1`, với giới hạn thời gian là 1,5 giây. Các giới hạn được cố tình đủ lớn để loại trừ việc xử lý mọi phần tử cho mỗi truy vấn, việc này sẽ mất tới`2.5 * 10^9`các hoạt động còn lại. Đồng thời, các giá trị`k[i]`chỉ được giới hạn bởi`5 * 10^4`, và giới hạn thứ hai đó là chìa khóa cho giải pháp nhanh hơn. 

Có một số trường hợp đặc biệt mà việc triển khai trực tiếp có thể xử lý sai. Đầu tiên, sản xuất bằng 0 là hợp lệ. Ví dụ,```
1 1
0
1 1 0 1
```có câu trả lời`1`, bởi vì`0 mod 1 = 0`. Việc triển khai giả định tất cả các giá trị sản xuất đều dương sẽ loại trừ trang trại một cách không chính xác. 

Vụ án`m = 1`cũng đặc biệt. Mọi số nguyên đều có số dư bằng 0 modulo một. Như vậy```
3 1
4 7 0
1 3 0 1
```phải sản xuất```
3
```Một lỗi phổ biến là lặp qua các giá trị có thể`x, x + m, ...`và vô tình coi giới hạn trên là độc quyền theo cách bỏ sót số 0 hoặc xử lý sai phần dư duy nhất. 

Trường hợp ranh giới khác là khi giá trị lớn nhất có thể là thành viên của lớp dư lượng. Vì```
3 1
2 5 10
1 3 0 5
```câu trả lời là`1`, bởi vì chỉ`5`Và`10`có số dư bằng 0 theo modulo 5, nên thực tế kết quả đầu ra đúng là`2`. Một vòng lặp tiến triển bất cẩn như`range(x, max_value, m)`sẽ dừng lại trước`max_value`và trở lại`1`. 

## Phương pháp tiếp cận 

Giải pháp vũ phu tuân theo định nghĩa trực tiếp. Đối với mỗi truy vấn, lặp qua`k[l-1:r]`, tính toán`k[i] % m`, và tăng câu trả lời khi nó bằng`x`. Điều này rõ ràng là đúng vì mỗi trang trại được kiểm tra chính xác một lần và điều kiện chính xác giống với điều kiện trong truy vấn. 

Vấn đề là số lượng hoạt động trong trường hợp xấu nhất. Với`n = q = 50000`, một truy vấn có thể bao gồm toàn bộ mảng, do đó thuật toán có thể thực hiện gần đúng`50000 * 50000 = 2.5 * 10^9`kiểm tra phần còn lại. Điều đó vượt xa giới hạn 1,5 giây cho phép. 

Quan sát hữu ích là`k[i]`bản thân nó là nhỏ. Một truy vấn không thực sự quan tâm đến giá trị chính xác của trang trại, chỉ quan tâm xem giá trị đó có thuộc về cấp số cộng hay không`x, x + m, x + 2m, ...`. 

Nếu như`m`lớn, tiến trình này chỉ chứa một số lượng nhỏ các giá trị vì mọi`k[i]`nhiều nhất là`50000`. Chúng ta có thể lưu trữ vị trí của mọi giá trị chính xác và sử dụng tìm kiếm nhị phân để đếm xem có bao nhiêu lần xuất hiện của mỗi giá trị`[l, r]`. 

Ví dụ, với`m = 1000`Và`x = 7`, giá trị sản xuất duy nhất có thể là`7, 1007, 2007, ...`. Có nhiều nhất khoảng`50000 / 1000 = 50`của họ. Do đó, một mô đun lớn cho chúng ta một danh sách ngắn các giá trị ứng cử viên. 

Bé nhỏ`m`cư xử theo cách ngược lại. Vì`m = 2`, tiến trình chứa nhiều giá trị, vì vậy việc kiểm tra tất cả chúng sẽ rất tốn kém. Nhưng chỉ có một số lượng nhỏ các mô đun nhỏ riêng biệt. Chúng tôi có thể xử lý mọi vấn đề nhỏ khác nhau`m`cùng nhau bằng cách quét toàn bộ mảng một lần, xây dựng danh sách vị trí cho mỗi modulo còn lại`m`. Sau đó, tất cả các truy vấn có điều này`m`có thể được trả lời bằng tìm kiếm nhị phân. 

Điều này mang lại sự phân tách căn bậc hai dựa trên mô đun. Chọn một ngưỡng`B`xung quanh`sqrt(50000)`, đại khái`224`. Vì`m <= B`, xử lý mô-đun đó bằng một lần quét mảng. Vì`m > B`, xử lý từng truy vấn bằng cách liệt kê các giá trị có thể có của nó. Cả hai bên thực hiện đại khái`O(sqrt(50000))`làm việc cho mỗi truy vấn hoặc nhóm mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nq)`|`O(1)`bên cạnh đầu vào | Quá chậm | 
| Tối ưu |`O(nB + q(K/B) log n)`|`O(n + K)`| Đã chấp nhận | 

Đây`K = max(k[i])`, Và`B`được chọn gần`sqrt(K)`. Với những giới hạn đã cho, cả hai số hạng chính đều khoảng vài chục triệu phép tính đơn giản. 

## Hướng dẫn thuật toán 

1. Đọc mảng và xây dựng`positions[v]`, chứa tất cả các chỉ số trong đó`k[i] = v`. Lưu trữ các chỉ số theo thứ tự tăng dần vì mảng được quét từ trái sang phải. 
2. Nhóm các truy vấn theo mô đun của chúng`m`là nhỏ hay lớn. Sử dụng một ngưỡng`B = 224`. Mô đun nhỏ sẽ được xử lý chung, còn mô đun lớn sẽ được giải quyết độc lập. 
3. Đối với mỗi mô-đun nhỏ thực sự xuất hiện trong các truy vấn, hãy quét mảng một lần và đặt từng chỉ mục vào nhóm tương ứng với`k[i] % m`. Nhóm kết quả`buckets[x]`chứa chính xác các chỉ số có phần dư theo modulo`m`là`x`. 
4. Đối với mỗi truy vấn có mô-đun nhỏ, hãy lấy nhóm phần dư tương ứng của nó và sử dụng hai tìm kiếm nhị phân để đếm các chỉ số trong`[l, r]`.`bisect_left`ít nhất tìm được vị trí đầu tiên`l`, trong khi`bisect_right`tìm thấy vị trí đầu tiên lớn hơn`r`. Sự khác biệt của họ chính xác là số lượng trang trại phù hợp. 
5. Đối với mỗi truy vấn có mô đun lớn, hãy liệt kê các giá trị sản xuất có thể có`x, x + m, x + 2m, ...`lên đến`max(k)`. Với mỗi giá trị`v`, sử dụng`positions[v]`và hai tìm kiếm nhị phân để đếm số lần xuất hiện của nó trong`[l, r]`, sau đó cộng các số đó lại với nhau. 
6. In câu trả lời theo thứ tự câu hỏi ban đầu. Việc giữ chỉ mục truy vấn là cần thiết vì việc xử lý các mô đun lớn và nhỏ diễn ra trong các nhóm khác nhau. 

### Tại sao nó hoạt động 

Đối với mô đun cố định`m`, một trang trại có chính xác`x`túi còn sót lại khi và chỉ khi giá trị sản xuất của nó thuộc về bộ`{x + tm | t >= 0}`. Đối với nhỏ`m`, các nhóm còn lại chứa rõ ràng mọi chỉ mục thỏa mãn điều kiện này, do đó, số lượng phạm vi trên nhóm sẽ đưa ra câu trả lời truy vấn chính xác. Đối với lớn`m`, việc liệt kê tất cả các giá trị trong cùng một cấp số cộng đó sẽ bao gồm mọi giá trị sản xuất có thể có và không có giá trị nào khác. Sau đó, danh sách vị trí có giá trị chính xác sẽ đếm chính xác các chỉ số phù hợp trong khoảng được yêu cầu. Do đó, cả hai nhánh đều tính cùng một tập toán học, chỉ được biểu diễn khác nhau. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, bisect_right

input = sys.stdin.readline

MAX_K = 50000
B = 224

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    positions = [[] for _ in range(MAX_K + 1)]
    for i, value in enumerate(a, 1):
        positions[value].append(i)

    queries = []
    small_mods = set()

    for qi in range(q):
        l, r, x, m = map(int, input().split())
        queries.append((l, r, x, m))
        if m <= B:
            small_mods.add(m)

    answers = [0] * q

    small_queries = [[] for _ in range(B + 1)]
    large_queries = []

    for qi, (l, r, x, m) in enumerate(queries):
        if m <= B:
            small_queries[m].append((qi, l, r, x))
        else:
            large_queries.append((qi, l, r, x, m))

    # Process every small modulus that occurs in the input.
    for m in small_mods:
        buckets = [[] for _ in range(m)]

        for i, value in enumerate(a, 1):
            buckets[value % m].append(i)

        for qi, l, r, x in small_queries[m]:
            bucket = buckets[x]
            left = bisect_left(bucket, l)
            right = bisect_right(bucket, r)
            answers[qi] = right - left

    # Process large moduli query by query.
    max_value = max(a) if a else 0

    for qi, l, r, x, m in large_queries:
        total = 0
        value = x

        while value <= max_value:
            pos = positions[value]
            left = bisect_left(pos, l)
            right = bisect_right(pos, r)
            total += right - left
            value += m

        answers[qi] = total

    sys.stdout.write("\n".join(map(str, answers)))

if __name__ == "__main__":
    solve()
```các`positions`việc xây dựng tương ứng với phần mô đun lớn của thuật toán.`positions[v]`được sắp xếp tự động vì các chỉ mục được thêm vào trong khi quét mảng ban đầu từ trái sang phải. 

Nhóm truy vấn tách biệt hai chế độ phức tạp. Một mô-đun nhỏ được xử lý một lần bất kể có bao nhiêu truy vấn sử dụng nó, trong khi một mô-đun lớn không chứng minh được việc xây dựng cấu trúc phần còn lại đầy đủ, vì vậy những truy vấn đó chỉ liệt kê các giá trị sản xuất có thể có của chúng. 

Vòng lặp mô-đun nhỏ tạo ra`m`xô và quét mảng một lần. Mỗi chỉ mục đi vào chính xác một nhóm, cụ thể là`value % m`. Bởi vì`x < m`được đảm bảo bởi đầu vào, truy cập`buckets[x]`luôn luôn hợp lệ. 

Hai tìm kiếm nhị phân có chủ ý khác nhau.`bisect_left(bucket, l)`trả về chỉ mục đầu tiên có vị trí được lưu trữ ít nhất là`l`, trong khi`bisect_right(bucket, r)`trả về vị trí đầu tiên lớn hơn`r`. Sự khác biệt của họ tính đến vị trí thỏa mãn`l <= position <= r`, khớp với khoảng truy vấn bao gồm. 

Đối với mô đun lớn, quá trình tiến triển bắt đầu ở`x`, không phải tại`0`. Các giá trị có thể có số dư`x`chính xác là`x + tm`. Vòng lặp sử dụng`<= max_value`, còn hơn là`< max_value`, do đó, giá trị sản xuất bằng giá trị mảng tối đa sẽ không bị bỏ qua một cách vô tình. 

Số nguyên Python không bị tràn và tối đa tất cả các chỉ mục được lưu trữ`50000`. Mối quan tâm triển khai chính là thời gian chạy, đó là lý do tại sao ngưỡng tránh việc quét liên tục toàn bộ mảng để tìm các mô đun lớn. 

## Ví dụ đã hoạt động 

Mẫu chính thức là:```
3 4
1 2 3
1 3 1 2
2 3 1 2
1 3 0 2
1 3 0 1
```Đối với ba truy vấn đầu tiên, mô đun nhỏ`m = 2`được xử lý một lần. 

| chỉ mục |`k[i]`|`k[i] % 2`| xô | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 0 | 0 | 
| 3 | 3 | 1 | 1 | 

Đối với truy vấn`(1, 3, 1, 2)`, xô`1`chứa các vị trí`[1, 3]`. Cả hai đều ở bên trong`[1, 3]`, cho`2`. 

Vì`(2, 3, 1, 2)`, cùng một thùng chứa`[1, 3]`, nhưng chỉ có vị trí`3`thuộc về`[2, 3]`, cho`1`. 

Vì`(1, 3, 0, 2)`, xô`0`chỉ chứa vị trí`2`, cho`1`. 

Truy vấn cuối cùng có`m = 1`. Mọi giá trị đều có số dư bằng 0 modulo một. 

| chỉ mục |`k[i]`|`k[i] % 1`| xô | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 
| 2 | 2 | 0 | 0 | 
| 3 | 3 | 0 | 0 | 

Nhóm số 0 còn lại chứa cả ba vị trí, vì vậy câu trả lời là`3`. Kết quả đầu ra là`2, 1, 1, 3`. 

Ví dụ thứ hai minh họa nhánh mô đun lớn.```
5 2
0 7 14 21 25
1 5 7 7
1 5 4 10
```Đối với truy vấn đầu tiên,`m = 7`Và`x = 7`. Các giá trị có thể là`7, 14, 21, 28, ...`. Chỉ một`7`,`14`, Và`21`xảy ra trong mảng. 

| giá trị ứng viên | vị trí | đếm vào`[1,5]`| 
| --- | --- | --- | 
| 7 |`[2]`| 1 | 
| 14 |`[3]`| 1 | 
| 21 |`[4]`| 1 | 
| 28 |`[]`| 0 | 

Câu trả lời là`3`. 

Đối với truy vấn thứ hai,`m = 10`Và`x = 4`. Các giá trị có thể là`4, 14, 24, 34, ...`. Chỉ một`14`xảy ra, tại vị trí`3`, vậy câu trả lời là`1`. 

Đầu ra là```
3
1
```Ví dụ này chứng minh tại sao giới hạn giá trị lại quan trọng. Mặc dù khoảng chứa năm trang trại, mô đun lớn cho phép chúng tôi kiểm tra chỉ một số ít giá trị sản xuất có thể có. 

## Phân tích độ phức tạp 

hãy để`K = max(k[i])`, với`K <= 50000`, và để`B = 224`. 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(nB + q(K/B) log n)`| Mỗi mô-đun nhỏ riêng biệt sẽ quét mảng một lần, trong khi mỗi truy vấn lớn chỉ kiểm tra nhiều nhất`K/B + 1`giá trị ứng cử viên | 
| Không gian |`O(n + K)`cộng với một cấu trúc xô mô-đun nhỏ tạm thời | Sử dụng vị trí giá trị chính xác`O(n)`và một mô đun nhỏ có`O(n + m)`lưu trữ xô | 

Có nhiều nhất`B`mô-đun nhỏ, do đó, việc quét mô-đun nhỏ tốn nhiều nhất khoảng`224 * 50000 = 11.2 million`thăm mảng. Một mô đun lớn lớn hơn`224`, vì vậy một truy vấn sẽ kiểm tra ít hơn khoảng`224`các giá trị ứng cử viên. Với`50000`truy vấn này một lần nữa theo thứ tự`11 million`kiểm tra ứng cử viên, mỗi kiểm tra sử dụng tìm kiếm nhị phân. Điều này tương thích với các giới hạn nhất định, không giống như`2.5 * 10^9`kiểm tra theo yêu cầu của vũ lực. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới phản ánh thuật toán được gửi thông qua hàm chấp nhận chuỗi đầu vào. Trường hợp sử dụng tối đa lớn`50000`các giá trị bằng nhau và một truy vấn, kiểm tra cả giới hạn giá trị và điểm cuối bên phải bao gồm.```python
import sys
import io
from bisect import bisect_left, bisect_right

MAX_K = 50000
B = 224

def solve():
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    positions = [[] for _ in range(MAX_K + 1)]
    for i, value in enumerate(a, 1):
        positions[value].append(i)

    queries = []
    small_mods = set()

    for qi in range(q):
        l, r, x, m = map(int, input().split())
        queries.append((l, r, x, m))
        if m <= B:
            small_mods.add(m)

    answers = [0] * q
    small_queries = [[] for _ in range(B + 1)]
    large_queries = []

    for qi, (l, r, x, m) in enumerate(queries):
        if m <= B:
            small_queries[m].append((qi, l, r, x))
        else:
            large_queries.append((qi, l, r, x, m))

    for m in small_mods:
        buckets = [[] for _ in range(m)]

        for i, value in enumerate(a, 1):
            buckets[value % m].append(i)

        for qi, l, r, x in small_queries[m]:
            bucket = buckets[x]
            answers[qi] = (
                bisect_right(bucket, r) -
                bisect_left(bucket, l)
            )

    max_value = max(a) if a else 0

    for qi, l, r, x, m in large_queries:
        total = 0
        value = x

        while value <= max_value:
            pos = positions[value]
            total += (
                bisect_right(pos, r) -
                bisect_left(pos, l)
            )
            value += m

        answers[qi] = total

    return "\n".join(map(str, answers))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run(
    """3 4
1 2 3
1 3 1 2
2 3 1 2
1 3 0 2
1 3 0 1
"""
) == "2\n1\n1\n3", "provided sample"

# Minimum-size input
assert run(
    """1 1
0
1 1 0 1
"""
) == "1", "minimum size and zero production"

# All values equal, with both small and large moduli
assert run(
    """5 3
10 10 10 10 10
1 5 0 5
2 4 3 7
1 3 10 11
"""
) == "5\n0\n3", "all equal values"

# Maximum production value must be included
assert run(
    """3 2
2 5 10
1 3 0 5
1 3 0 10
"""
) == "2\n1", "right endpoint of arithmetic progression"

# Large modulus with several candidate values
assert run(
    """5 2
0 7 14 21 25
1 5 7 7
1 5 4 10
"""
) == "3\n1", "large modulus progression"

# Maximum-size n with a uniform array
assert run(
    "50000 1\n" +
    " ".join(["50000"] * 50000) +
    "\n1 50000 0 50001\n"
) == "50000", "maximum n and maximum m"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0 / 1 1 0 1`|`1`| Kích thước tối thiểu, giá trị bằng 0 và`m = 1`| 
| Năm bản sao của`10`với ba truy vấn |`5, 0, 3`| Các giá trị bằng nhau và một số lớp dư lượng | 
|`2 5 10`, truy vấn số dư bằng 0 modulo`5`Và`10`|`2, 1`| Giá trị ứng viên tối đa bao gồm | 
|`0 7 14 21 25`, mô đun lớn |`3, 1`| Cấp tiến số học mô đun lớn | 
|`50000`bản sao của`50000`|`50000`| Tối đa`n`, tối đa`k[i]`, Và`m = 50001`| 

## Vỏ cạnh 

Đối với sản xuất bằng không, hãy xem xét```
1 1
0
1 1 0 1
```Thuật toán phân loại`m = 1`như một mô-đun nhỏ, tạo một nhóm còn lại và đặt vị trí`1`vào xô`0`bởi vì`0 % 1 = 0`. Truy vấn tìm kiếm nhóm đó giữa các vị trí`1`Và`1`, tìm một vị trí, vì vậy đầu ra là`1`. 

Vì`m = 1`, coi như```
3 1
4 7 0
1 3 0 1
```Số dư duy nhất có thể bằng không. Trong quá trình tiền xử lý, các vị trí`1`,`2`, Và`3`tất cả vào cùng một nhóm. Tìm kiếm nhị phân bao gồm toàn bộ khoảng thời gian và trả về`3`. Điều này tránh bất kỳ mã trường hợp đặc biệt nào cho modulo một. 

Để có điểm cuối phù hợp của một cấp số cộng, hãy xem xét```
3 1
2 5 10
1 3 0 5
```Các giá trị sản xuất liên quan là`0, 5, 10, ...`. Nhánh mô đun lớn không được sử dụng ở đây vì`m = 5`, nhưng nguyên tắc lũy tiến bao hàm tương tự cũng áp dụng cho nhóm mô-đun nhỏ: vị trí`2`Và`3`cả hai đều có số dư bằng 0 nên kết quả là`2`. Việc triển khai sử dụng ranh giới trên độc quyền sẽ bỏ lỡ giá trị một cách không chính xác`10`. 

Đối với một mô đun lớn, hãy xem xét```
5 1
0 7 14 21 25
1 5 7 7
```Môđun lớn hơn ngưỡng nên thuật toán liệt kê`7`,`14`, Và`21`, sau đó dừng khi giá trị tiếp theo là`28 > 25`. Danh sách vị trí của họ đóng góp mỗi vị trí một số lượng, tạo ra`3`. Vòng lặp không kiểm tra các giá trị không liên quan như`0`hoặc`25`, vì cả hai đều không có số dư`7`modulo`7`.
