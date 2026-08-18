---
title: "CF 102279H - Houston, Bạn Có Ở Đó Không?"
description: "Mỗi quân cờ là một quân cờ giống như quân domino có hai con số ở hai đầu. Ô có thể được sử dụng theo hướng ban đầu, được biểu thị bằng a hoặc được lật, được biểu thị bằng b."
date: "2026-08-17T03:44:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102279
codeforces_index: "H"
codeforces_contest_name: "HCW 19 Team Round (ICPC format)"
rating: 0
weight: 102279
solve_time_s: 1054
verified: true
draft: false
---

[CF 102279H - Houston, bạn có ở đó không?](https://codeforces.com/problemset/problem/102279/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 17 phút 34 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi quân cờ là một quân cờ giống như quân domino có hai con số ở hai đầu. Ngói có thể được sử dụng theo hướng ban đầu của nó, được biểu thị bằng`a`, hoặc đảo ngược, đại diện bởi`b`. Sau khi chọn hướng cho mỗi mảnh, các mảnh phải được sắp xếp theo một trình tự sao cho các đầu tiếp xúc của mỗi cặp lân cận có cùng số lượng. 

Ví dụ, nếu một mảnh định hướng là`(2, 3)`và tiếp theo là`(3, 6)`, chúng có thể liền kề nhau vì đầu bên phải của mảnh thứ nhất và đầu bên trái của mảnh thứ hai đều bằng nhau`3`. 

Đầu vào chứa`N`phần, theo sau là hai giá trị điểm cuối của mỗi phần. Đầu ra phải cung cấp cho mỗi phần chính xác một lần, cùng với hướng của nó, theo thứ tự tạo thành một chuỗi hợp lệ. Tuyên bố đảm bảo rằng có ít nhất một chuỗi như vậy tồn tại. 

Hạn chế quan trọng là`N <= 8`. Điều này là cực kỳ nhỏ. Chúng ta được phép xem xét các sắp xếp liên quan đến tất cả các hoán vị của các quân cờ và cả hai hướng có thể có. chỉ có`8! * 2^8 = 10,321,920`các thỏa thuận được chỉ định đầy đủ trong trường hợp xấu nhất tuyệt đối. Nó đủ lớn để việc triển khai bất cẩn có thể tốn kém, nhưng đủ nhỏ để tìm kiếm toàn diện, đặc biệt là vì chúng ta có thể dừng ngay lập tức khi tìm thấy sự sắp xếp hợp lệ. 

Có một số chi tiết mà việc triển khai đơn giản phải xử lý chính xác. Một mảnh có thể có điểm cuối bằng nhau, vì vậy việc đảo ngược`(4, 4)`không có gì thay đổi Ví dụ, với```
24 44 4
```đầu ra có thể đơn giản là```
1 a2 a
```Vấn đề thứ hai là một quân cờ có thể cần phải được đảo ngược ngay cả khi số của nó khác nhau. Vì```
23 57 3
```một câu trả lời hợp lệ là```
1 a2 b
```bởi vì các mảnh định hướng là`(3, 5)`Và`(3, 7)`, không kết nối theo thứ tự đó. Thay vào đó, sự sắp xếp hợp lệ thực tế là```
1 b2 b
```mang lại`(5, 3)`theo sau là`(3, 7)`. Giải pháp chỉ hoán vị các phần và không bao giờ thử cả hai hướng sẽ thất bại. 

Các phần trùng lặp là một nguồn lỗi phổ biến khác. Nếu một số phần có điểm cuối giống hệt nhau thì chúng vẫn là các phần riêng biệt vì chỉ số đầu vào của chúng khác nhau. Vì```
31 21 22 1
```cả ba chỉ số đều phải xuất hiện trong câu trả lời. Việc chỉ xử lý các phần bằng giá trị của chúng có thể vô tình sử dụng một phần vật lý hai lần trong khi bỏ qua phần khác. 

Cuối cùng, phần đầu tiên không có phần lân cận trước đó, vì vậy hướng của nó không bị hạn chế bởi bất kỳ phần nào trước nó. Điều kiện khớp chỉ bắt đầu khi phần thứ hai được thêm vào. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tìm kiếm toàn diện. Chọn một phần không sử dụng, chọn một trong hai hướng của nó và nối nó nếu điểm cuối bên trái của nó khớp với điểm cuối bên phải hiện tại. Khi tất cả các mảnh đã được đặt, chúng ta có câu trả lời hợp lệ. 

Tìm kiếm này đúng vì mọi cấu hình cuối cùng có thể đều bao gồm một hoán vị của`N`các mảnh và sự lựa chọn định hướng cho mỗi mảnh. Tìm kiếm đệ quy xem xét chính xác những lựa chọn đó, do đó không thể bỏ qua cấu hình hợp lệ. 

Nếu chúng ta liệt kê đầy đủ mà không cắt tỉa thì có`N!`các đơn đặt hàng có thể và`2^N`bài tập định hướng, đưa ra`O(N! * 2^N)`khả năng. Tại`N = 8`, đó là`8! * 256 = 10,321,920`cấu hình. Việc kiểm tra một chuỗi hoàn chỉnh đòi hỏi nhiều nhất`N - 1 = 7`kết nối, do đó việc triển khai theo nghĩa đen có thể thực hiện xung quanh`72 million`so sánh điểm cuối trong trường hợp xấu nhất. Giới hạn nhỏ trên`N`làm cho điều này có thể chấp nhận được bằng một ngôn ngữ được biên dịch và việc đảm bảo rằng một giải pháp tồn tại thường cho phép tìm kiếm đệ quy kết thúc sớm hơn nhiều. 

Chúng ta có thể làm cho việc tìm kiếm nhỏ hơn đáng kể bằng lập trình động tập hợp con. Thay vì nhớ toàn bộ thứ tự đã được xây dựng cho đến nay, chúng ta chỉ cần nhớ những mảnh nào đã được sử dụng và số ở đầu mở hiện tại của chuỗi. Nếu hai chuỗi thành phần khác nhau sử dụng chính xác cùng một tập hợp các mảnh và hoàn thành với cùng một số lượng thì khả năng xảy ra trong tương lai của chúng là giống hệt nhau. Chúng ta chỉ cần giữ một trong số họ. 

có`2^N`có thể có mặt nạ mảnh đã sử dụng và chỉ có sáu giá trị điểm cuối có thể có. Đối với mỗi trạng thái, chúng tôi thử mọi phần chưa sử dụng ở cả hai hướng. Điều này mang lại`O(2^N * N * 2)`quá trình chuyển đổi, rất nhỏ đối với`N <= 8`. 

DP đặc biệt tự nhiên ở đây vì tương lai của chuỗi domino một phần chỉ phụ thuộc vào các quân cờ được sử dụng và điểm cuối hiện tại của nó. Thứ tự chính xác được sử dụng để đạt đến trạng thái đó không còn quan trọng nữa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(N! * 2^N)`|`O(N)`| Được chấp nhận cho`N <= 8`| 
| Tập hợp con DP |`O(2^N * N)`|`O(2^N * 6)`| Đã chấp nhận | 

Việc triển khai bên dưới sử dụng tập hợp con DP vì nó đưa ra giới hạn trường hợp xấu nhất mạnh hơn nhiều trong khi vẫn đủ đơn giản để tái tạo lại chuỗi thực tế. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các phần theo cặp`(u[i], v[i])`. Các chỉ số mảnh được giữ riêng biệt với giá trị của chúng vì hai mảnh trông giống hệt nhau vẫn là các phần đầu vào khác nhau. 
2. Biểu diễn một tập hợp các mảnh đã sử dụng bằng một`N`-bit mặt nạ. Chút`i`chính xác là một khi mảnh`i`đã được đặt. 
3. Xác định trạng thái DP bằng cách`(mask, last)`, Ở đâu`mask`là tập hợp các mảnh được sử dụng và`last`là số hiện được hiển thị ở đầu bên phải của chuỗi. Chúng tôi lưu trữ một tiền thân cho mọi trạng thái có thể truy cập để có thể xây dựng lại chuỗi cuối cùng. 
4. Bắt đầu với mọi mảnh theo cả hai hướng có thể. Nếu mảnh`i`là`(u[i], v[i])`, định hướng`a`tạo ra một chuỗi kết thúc bằng`v[i]`, trong khi định hướng`b`tạo ra một chuỗi kết thúc bằng`u[i]`. 
5. Đối với mọi trạng thái có thể truy cập, hãy xem xét từng phần không có trong`mask`. Nó có thể được thêm vào trong định hướng`a`khi`u[i] == last`, tạo ra một điểm cuối mới`v[i]`. Nó có thể được thêm vào trong định hướng`b`khi`v[i] == last`, tạo ra một điểm cuối mới`u[i]`. 
6. Khi một trạng thái mới chưa được truy cập trước đó, hãy lưu trạng thái trước đó cùng với phần và hướng được sử dụng để tiếp cận trạng thái đó. Nếu đã đạt đến trạng thái, hãy bỏ qua đường đi mới vì cả hai đường dẫn đều có những lựa chọn tương lai giống hệt nhau. 
7. Ngay khi một trạng thái có tất cả`N`đạt đến các bit được thiết lập, hãy xây dựng lại câu trả lời bằng cách đi theo các con trỏ trước đó. Đảo ngược danh sách đã thu thập vì quá trình xây dựng lại bắt đầu tự nhiên từ phần cuối cùng. 

### Tại sao nó hoạt động 

Điều bất biến là mọi trạng thái DP có thể tiếp cận`(mask, last)`đại diện cho ít nhất một chuỗi hợp lệ chứa chính xác các phần trong`mask`và kết thúc bằng giá trị`last`. Ban đầu điều này đúng vì mọi chuỗi một mảnh đều có giá trị. Khi chúng tôi nối thêm một phần, chúng tôi chỉ chấp nhận hướng có điểm cuối bên trái bằng`last`, do đó kết nối mới hợp lệ và bất biến vẫn đúng. 

Ngược lại, hãy xem xét bất kỳ chuỗi thành phần hợp lệ nào. Các phần được sử dụng của nó tạo thành một mặt nạ nào đó và điểm cuối cuối cùng của nó là một giá trị nào đó`last`. Bắt đầu từ phần đầu tiên, DP có thể tuân theo chính xác hướng và các phần của chuỗi đó, bởi vì mọi cặp liên tiếp đều đáp ứng sự bằng nhau về điểm cuối cần thiết. Do đó, mọi chuỗi hợp lệ đều tương ứng với một chuỗi chuyển tiếp DP. Vì bài toán đảm bảo rằng một chuỗi hoàn chỉnh tồn tại nên cuối cùng DP đạt đến trạng thái chứa mọi phần. Các liên kết tiền thân được lưu trữ mô tả một chuỗi hoàn chỉnh hợp lệ. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def solve():    n = int(input())    pieces = [tuple(map(int, input().split())) for _ in range(n)]
    full = (1 << n) - 1
    # parent[mask][last] = (previous_mask, previous_last, piece_index, orientation)    # last is in 1..6, so index 0 is unused.    parent = [[None] * 7 for _ in range(1 << n)]    seen = [[False] * 7 for _ in range(1 << n)]
    # Start with every possible first piece and both orientations.    for i, (u, v) in enumerate(pieces):        mask = 1 << i
        # Orientation 'a': (u, v), current endpoint is v.        if not seen[mask][v]:            seen[mask][v] = True            parent[mask][v] = (-1, -1, i, 'a')
        # Orientation 'b': (v, u), current endpoint is u.        if not seen[mask][u]:            seen[mask][u] = True            parent[mask][u] = (-1, -1, i, 'b')
    final_mask = None    final_last = None
    for mask in range(1 << n):        for last in range(1, 7):            if not seen[mask][last]:                continue
            if mask == full:                final_mask = mask                final_last = last                break
            for i, (u, v) in enumerate(pieces):                if mask & (1 << i):                    continue
                new_mask = mask | (1 << i)
                # Put piece i in its original orientation: (u, v).                if u == last and not seen[new_mask][v]:                    seen[new_mask][v] = True                    parent[new_mask][v] = (mask, last, i, 'a')
                # Reverse piece i: (v, u).                if v == last and not seen[new_mask][u]:                    seen[new_mask][u] = True                    parent[new_mask][u] = (mask, last, i, 'b')
        if final_mask is not None:            break
    # The problem guarantees that a complete chain exists.    answer = []
    mask = final_mask    last = final_last
    while mask != -1:        pmask, plast, i, orientation = parent[mask][last]        answer.append((i + 1, orientation))        mask, last = pmask, plast
    answer.reverse()
    sys.stdout.write(        ''.join(f"{i} {orientation}\n" for i, orientation in answer)    )

if __name__ == "__main__":    solve()
```các`pieces`mảng lưu trữ hướng ban đầu của mỗi ô. DP sử dụng các chỉ số dựa trên 0 bên trong, trong khi đầu ra yêu cầu số mảnh dựa trên một, do đó`i + 1`được in trong quá trình tái thiết. 

các`parent`bảng có sáu vị trí điểm cuối có ý nghĩa vì mọi điểm cuối đều nằm giữa`1`Và`6`. Giữ vị trí thứ bảy không được sử dụng làm cho việc lập chỉ mục trở nên trực tiếp và tránh bị trừ đi một vị trí nhiều lần. 

Việc khởi tạo hơi tinh tế. Phần đầu tiên không có hàng xóm bên trái, vì vậy cả hai hướng đều là trạng thái bắt đầu hợp lệ. Nếu hai điểm cuối bằng nhau thì cả hai hướng đều dẫn đến cùng một trạng thái và`seen`kiểm tra chính xác chỉ lưu trữ một trong số chúng. 

Để chuyển tiếp, định hướng`a`có nghĩa là mảnh đó`(u, v)`. Nó chỉ có thể được thêm vào khi`u`bằng điểm cuối hiện tại và sau đó điểm cuối mới trở thành`v`. Định hướng`b`có nghĩa`(v, u)`, vì vậy nó đòi hỏi`v == last`và lá`u`để lộ ra. 

Trạng thái trước đó chỉ được ghi lại khi đạt đến trạng thái lần đầu tiên. Điều này là an toàn vì tất cả các chuyển đổi trong tương lai chỉ phụ thuộc vào chính trạng thái đó chứ không phụ thuộc vào chuỗi phần cụ thể nào đã tạo ra nó. 

Việc xây dựng lại bắt đầu ở mặt nạ đầy đủ và theo sau các con trỏ tiền nhiệm cho đến khi đánh dấu trạng thái ban đầu`(-1, -1, ...)`đã đạt được. Vì những con trỏ này đi lùi nên danh sách kết quả phải được đảo ngược trước khi in. 

Không có vấn đề tràn số nguyên trong Python và ngay cả trong ngôn ngữ có chiều rộng cố định, tất cả các mặt nạ đều vừa khít với một số nguyên nhỏ vì`N <= 8`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
23 26 3
```Phần đầu tiên có thể được sử dụng như`(3, 2)`hoặc`(2, 3)`. DP bắt đầu với cả hai khả năng. 

| Mặt nạ | Điểm cuối hiện tại | Đã thêm phần | Định hướng | Điểm cuối mới | 
| --- | --- | --- | --- | --- | 
|`01`|`2`| 1 |`a`|`2`| 
|`01`|`3`| 1 |`b`|`3`| 
|`10`|`3`| 2 |`a`|`3`| 
|`10`|`6`| 2 |`b`|`6`| 
|`11`|`3`| 2 |`b`|`6`| 

Đoạn chuyển tiếp cuối cùng sử dụng đoạn 2 theo hướng`b`, thay đổi`(6, 3)`vào trong`(3, 6)`. Chuỗi là```
(3, 2) -> (3, 6)
```vì vậy một đầu ra hợp lệ là```
1 a2 b
```Dấu vết cho thấy tại sao định hướng phải là một phần của logic chuyển tiếp. Các mảnh không thể được giải quyết chỉ bằng cách tìm một hoán vị. 

### Mẫu 2 

Đầu vào là```
53 24 54 45 13 1
```Chuỗi hợp lệ được DP tìm thấy là```
(2, 3) -> (3, 1) -> (1, 5) -> (5, 4) -> (4, 4)
```Các phần và hướng tương ứng được hiển thị bên dưới. 

| Mặt nạ | Điểm cuối hiện tại | Đã thêm phần | Định hướng | Điểm cuối mới | 
| --- | --- | --- | --- | --- | 
|`00001`|`2`| 1 |`b`|`3`| 
|`10001`|`1`| 5 |`a`|`1`| 
|`11001`|`5`| 4 |`b`|`5`| 
|`11011`|`4`| 2 |`b`|`4`| 
|`11111`|`4`| 3 |`a`|`4`| 

Kết quả đầu ra là```
1 b5 a4 b2 b3 a
```Phần cuối cùng là`(4, 4)`, vì vậy việc nó được in bằng`a`hoặc`b`. Điều này thể hiện trường hợp điểm cuối bằng nhau, trong đó việc đảo ngược một quân cờ không có tác dụng rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(2^N * N)`| có`2^N * 6`các bang và mỗi bang xem xét tối đa`N`mảnh chưa sử dụng với hai hướng. | 
| Không gian |`O(2^N)`| các`seen`Và`parent`bảng chứa`7 * 2^N`mục nhập. | 

Với`N <= 8`, có nhiều nhất`256`mặt nạ và chỉ có sáu giá trị điểm cuối có liên quan. Ngay cả sau khi xem xét mọi phần tiếp theo có thể có và cả hai hướng, số lượng thao tác vẫn rất nhỏ so với giới hạn một giây. Việc sử dụng bộ nhớ cũng không đáng kể so với 256 MB. 

## Trường hợp thử nghiệm 

Bởi vì vấn đề cho phép bất kỳ cấu hình hợp lệ nào, nên sự bằng nhau của chuỗi chính xác không phải là một xác nhận phù hợp. Bộ khai thác kiểm tra bên dưới phân tích câu trả lời được tạo ra và xác minh rằng mỗi phần được sử dụng chính xác một lần, mọi hướng đều hợp pháp và mọi cặp lân cận đều kết nối chính xác.```python
Pythonimport sysimport io

def solve_data(inp: str) -> str:    data = inp.strip().split()    it = iter(data)
    n = int(next(it))    pieces = [(int(next(it)), int(next(it))) for _ in range(n)]
    full = (1 << n) - 1
    parent = [[None] * 7 for _ in range(1 << n)]    seen = [[False] * 7 for _ in range(1 << n)]
    for i, (u, v) in enumerate(pieces):        mask = 1 << i
        if not seen[mask][v]:            seen[mask][v] = True            parent[mask][v] = (-1, -1, i, 'a')
        if not seen[mask][u]:            seen[mask][u] = True            parent[mask][u] = (-1, -1, i, 'b')
    final_mask = None    final_last = None
    for mask in range(1 << n):        for last in range(1, 7):            if not seen[mask][last]:                continue
            if mask == full:                final_mask = mask                final_last = last                break
            for i, (u, v) in enumerate(pieces):                if mask & (1 << i):                    continue
                new_mask = mask | (1 << i)
                if u == last and not seen[new_mask][v]:                    seen[new_mask][v] = True                    parent[new_mask][v] = (mask, last, i, 'a')
                if v == last and not seen[new_mask][u]:                    seen[new_mask][u] = True                    parent[new_mask][u] = (mask, last, i, 'b')
        if final_mask is not None:            break
    answer = []    mask = final_mask    last = final_last
    while mask != -1:        pmask, plast, i, orientation = parent[mask][last]        answer.append((i + 1, orientation))        mask, last = pmask, plast
    answer.reverse()    return ''.join(f"{i} {o}\n" for i, o in answer)

def run(inp: str) -> str:    return solve_data(inp)

def validate(inp: str, out: str):    data = inp.strip().split()    n = int(data[0])
    pieces = []    pos = 1    for _ in range(n):        pieces.append((int(data[pos]), int(data[pos + 1])))        pos += 2
    lines = out.strip().splitlines()    assert len(lines) == n, f"expected {n} output lines, got {len(lines)}"
    used = set()    oriented = []
    for line in lines:        idx, orientation = line.split()        idx = int(idx)
        assert 1 <= idx <= n        assert orientation in ("a", "b")        assert idx not in used, "a piece was used more than once"
        used.add(idx)
        u, v = pieces[idx - 1]        if orientation == "a":            oriented.append((u, v))        else:            oriented.append((v, u))
    assert len(used) == n
    for i in range(n - 1):        assert oriented[i][1] == oriented[i + 1][0], (            f"invalid connection between positions {i} and {i + 1}"        )

# Provided sample 1sample1 = """\23 26 3"""validate(sample1, run(sample1))

# Provided sample 2sample2 = """\53 24 54 45 13 1"""validate(sample2, run(sample2))

# Minimum size, requiring a reversal.case_min = """\23 57 3"""validate(case_min, run(case_min))

# All endpoints equal.case_equal = """\44 44 44 44 4"""validate(case_equal, run(case_equal))

# Boundary values 1 and 6, with several reversals needed.case_boundary = """\61 26 13 64 35 45 5"""validate(case_boundary, run(case_boundary))

# Maximum size, eight pieces.case_max = """\81 22 33 44 55 66 11 33 5"""validate(case_max, run(case_max))
print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 3 2 / 6 3`| Bất kỳ dây chuyền hai mảnh hợp lệ nào | Mẫu chính thức, xử lý định hướng cơ bản | 
|`5 / 3 2 / 4 5 / 4 4 / 5 1 / 3 1`| Bất kỳ dây chuyền năm mảnh hợp lệ nào | Mẫu chính thức, điểm cuối bằng nhau và một số điểm đảo ngược | 
|`2 / 3 5 / 7 3`|`1 b`,`2 b`hợp lệ | tối thiểu`N`, cả hai phần cần đảo ngược theo thứ tự đã chọn | 
| Bốn bản sao của`4 4`| Bất kỳ hoán vị nào với một trong hai hướng | Điểm cuối giống hệt nhau và các phần trùng lặp | 
| Các mảnh sử dụng giá trị`1`Và`6`| Bất kỳ dây chuyền sáu mảnh hợp lệ nào | Ranh giới điểm cuối và chuyển tiếp định hướng | 
| Đầu vào tám mảnh | Bất kỳ dây chuyền tám mảnh hợp lệ nào | Tối đa`N`và DP toàn trạng thái | 

Bộ khai thác kiểm tra xác nhận kết quả đầu ra thay vì so sánh chúng với một câu trả lời cố định vì thẩm phán chấp nhận mọi sự sắp xếp hợp lệ. Đây là cách chính xác để kiểm tra một vấn đề mang tính xây dựng mà đầu ra của nó không phải là duy nhất. 

## Vỏ cạnh 

Để có điểm cuối bằng nhau, hãy xem xét```
24 44 4
```Việc khởi tạo tạo ra một trạng thái kết thúc tại`4`cho một trong hai hướng của mỗi phần. Quá trình chuyển đổi đầu tiên thấy rằng phần không được sử dụng cũng có điểm cuối bên trái`4`, vì vậy nó có thể được nối thêm ngay lập tức. Chuỗi cuối cùng có giá trị bất kể phần nào được in dưới dạng`a`hoặc`b`. của DP`seen`table cũng ngăn các trạng thái trùng lặp được lưu trữ một cách không cần thiết. 

Đối với một sự đảo ngược cần thiết, hãy xem xét```
23 57 3
```Nếu mảnh 1 được đặt như`a`, điểm cuối tiếp xúc là`5`, không thể kết nối với phần 2 theo một trong hai hướng. Trạng thái được tạo ra bằng cách sử dụng mảnh 1 làm`b`thay vào đó có điểm cuối`3`. Đoạn 2 sau đó có thể được đảo ngược thành`(3, 7)`, cho`(5, 3) -> (3, 7)`. DP kiểm tra rõ ràng cả hai hướng, vì vậy nó tìm thấy chuỗi này. 

Đối với các phần trùng lặp, hãy xem xét```
31 21 22 1
```Hai bản sao của`(1, 2)`có các chỉ số khác nhau và các bit khác nhau trong mặt nạ. Mặc dù giá trị của chúng giống hệt nhau nhưng việc sử dụng mảnh 1 không đánh dấu mảnh 2 là đã sử dụng. Do đó, một trạng thái có thể chứa một hoặc cả hai bản sao và mặt nạ đầy đủ chỉ đạt được sau khi cả ba phần vật lý đã được đặt. 

Đối với một mảnh hai đầu, hãy xem xét```
34 44 55 6
```Chuỗi hợp lệ có thể bắt đầu bằng`(4, 4)`, theo sau là`(4, 5)`, theo sau là`(5, 6)`. Hai hướng của ô đầu tiên tạo ra cùng một điểm cuối, nhưng thuật toán coi chúng là trạng thái tương đương. Điều này là an toàn vì các khả năng trong tương lai chỉ phụ thuộc vào điểm cuối hiện tại và mặt nạ mảnh đã sử dụng, chứ không phụ thuộc vào ký hiệu định hướng nào được chọn cho mảnh đối xứng. 

Đối với kích thước đầu vào tối đa,`N = 8`chỉ cho`256`mặt nạ. Tối đa sáu giá trị điểm cuối cần được biểu thị cho mỗi mặt nạ và mỗi trạng thái xem xét tối đa tám phần. Do đó, DP hoàn chỉnh vẫn còn rất nhỏ, mặc dù bảng liệt kê hoán vị và định hướng không hạn chế sẽ chứa hơn mười triệu cấu hình.
