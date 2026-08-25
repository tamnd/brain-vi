---
title: "CF 102215E - Phần mềm của bên thứ ba - 2"
description: "Chúng tôi có n phiên bản thư viện. Phiên bản i cung cấp mọi hàm có số nằm trong khoảng bao gồm [ai, bi]. Pavel cần mọi hàm từ 1 đến m, do đó các khoảng được mua phải bao trùm toàn bộ phạm vi số nguyên [1, m]. Nhiệm vụ có hai phần."
date: "2026-08-24T16:52:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "E"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 2403
verified: true
draft: false
---

[CF 102215E - Phần mềm của bên thứ ba - 2](https://codeforces.com/problemset/problem/102215/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`các phiên bản thư viện Phiên bản`i`cung cấp mọi hàm có số nằm trong khoảng bao gồm`[a_i, b_i]`. Pavel cần mọi chức năng từ`1`bởi vì`m`, do đó các khoảng được mua phải bao trùm toàn bộ phạm vi số nguyên`[1, m]`. 

Nhiệm vụ có hai phần. Đầu tiên, chúng ta phải quyết định xem liệu tập hợp các phiên bản như vậy có tồn tại hay không. Nếu đúng như vậy, chúng ta phải tìm một bộ sưu tập có số lượng phiên bản nhỏ nhất có thể và in chỉ mục gốc của chúng. 

Giá trị lớn của`m`, lên đến`10^9`, ngay lập tức loại trừ các thuật toán lặp qua mọi số hàm. Tham số kích thước hữu ích là`n`, nhiều nhất là`200000`, vì vậy một`O(n log n)`thuật toán phù hợp với giới hạn hai giây. MỘT`O(n^2)`tìm kiếm theo cặp hoặc kết hợp lớn hơn sẽ cần tới khoảng`4 * 10^10`so sánh khoảng thời gian, vượt xa những gì giới hạn cho phép. 

Có một số trường hợp ranh giới có thể phá vỡ việc triển khai bất cẩn. Nếu khoảng đầu tiên không bắt đầu ở hàm`1`, phạm vi bảo hiểm là không thể. Ví dụ,```
2 5
2 5
1 1
```thực sự có thể thực hiện được vì phiên bản thứ hai bao gồm chức năng`1`và những trang bìa đầu tiên`2`bởi vì`5`. Một thuật toán tham lam chỉ cần chọn khoảng có điểm cuối bên phải lớn nhất trên toàn cầu sẽ chọn`[2,5]`đầu tiên và có thể kết luận sai chức năng đó`1`bị thiếu. Thuật toán phải tôn trọng ranh giới bên trái trước khi tối đa hóa điểm cuối bên phải. 

Một sự cố trực tiếp hơn xảy ra khi có một khoảng cách thực sự:```
2 5
1 2
4 5
```Câu trả lời đúng là`NO`, vì hàm`3`không có sẵn. Việc chọn các khoảng chỉ theo đúng điểm cuối của chúng sẽ không phát hiện chính xác điều này trừ khi khoảng tiếp theo được yêu cầu bắt đầu ở nhiều nhất một vị trí sau tiền tố hiện được bao phủ. 

Một trường hợp cạnh khác là khoảng bắt đầu chính xác ở hàm chưa được khám phá đầu tiên:```
3 6
1 2
3 4
5 6
```Câu trả lời là`YES`với ba phiên bản. Vì các hàm là số nguyên nên sau khi duyệt qua`2`, một khoảng bắt đầu tại`3`là hoàn toàn hợp lệ. Điều kiện là`a_i <= covered + 1`, không`a_i <= covered`. 

Cuối cùng, các khoảng chồng chéo có thể làm cho một lựa chọn có vẻ ngắn cục bộ trở nên không tối ưu:```
3 8
1 3
2 7
6 8
```Giải pháp tối ưu sử dụng`[1,3]`Và`[2,7]`chỉ khi một khoảng khác đạt tới`8`, nhưng không phải vậy, vì vậy trong trường hợp đầu vào cụ thể này, câu trả lời là`NO`. Nếu chúng ta thay đổi khoảng thời gian cuối cùng thành`[6,8]`, lý do tương tự cho thấy tại sao mọi khoảng được chọn phải mở rộng tiền tố hiện được bao phủ. Sự lựa chọn tham lam phải luôn tối đa hóa điểm cuối bên phải mới trong số tất cả các khoảng thời gian có thể tiếp tục phạm vi phủ sóng hiện tại. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu được rút ra trực tiếp từ định nghĩa. Chúng ta có thể thử mọi tập hợp con của các phiên bản thư viện, kiểm tra xem liệu tập hợp có bao gồm`[1,m]`và giữ tập hợp con hợp lệ nhỏ nhất. Điều này đúng vì mọi quyết định mua hàng đều được xem xét rõ ràng, nhưng có`2^n`tập hợp con. Với`n = 200000`, nó lớn đến mức vô vọng. 

Ngay cả việc hạn chế lực lượng vũ phu theo cặp, bộ ba hoặc các tổ hợp nhỏ khác cũng không giải quyết được vấn đề chung. Ví dụ, việc kiểm tra từng cặp đã mất`O(n^2)`, đạt tới khoảng`2 * 10^10`cặp khi`n = 200000`. Vấn đề cần một cách để đưa ra lựa chọn một cách tham lam hơn là liệt kê các kết hợp. 

Quan sát quan trọng là các hàm chúng ta đã đề cập luôn tạo thành tiền tố`[1, x]`. Giả định`x`là chức năng lớn nhất hiện nay được bảo hiểm. Bất kỳ khoảng nào có thể mở rộng tiền tố này đều phải thỏa mãn`a_i <= x + 1`. Trong số tất cả các khoảng như vậy, việc chọn khoảng có giá trị lớn nhất`b_i`không bao giờ tệ hơn việc chọn một điểm có điểm cuối bên phải nhỏ hơn. Cả hai lựa chọn đều có thể bắt đầu phần tiếp theo của phạm vi phủ sóng, nhưng khoảng thời gian tiến xa hơn chỉ có thể giải quyết được ít nhất phần lớn vấn đề còn lại. 

Điều này đưa ra chiến lược tham lam tiêu chuẩn để bao phủ khoảng thời gian tối thiểu. Sắp xếp các khoảng theo điểm cuối bên trái của chúng. Bắt đầu với`covered = 0`, quét mọi khoảng có điểm cuối bên trái nhiều nhất`covered + 1`và ghi nhớ cái có điểm cuối bên phải lớn nhất. Khi quá trình quét đạt đến khoảng thời gian bắt đầu quá xa về bên phải, chúng tôi phải cam kết khoảng thời gian tốt nhất được tìm thấy cho đến nay vì mọi khoảng thời gian hiện có thể sử dụng đều đã được xem xét. Nếu khoảng thời gian tốt nhất đó không kéo dài`covered`, có một khoảng trống và câu trả lời là không thể. 

Brute-force hoạt động vì nó tìm kiếm rõ ràng tất cả các cách để mở rộng vùng được bao phủ, nhưng không thành công vì có nhiều lựa chọn theo cấp số nhân. Nhận xét rằng chỉ khoảng thời gian có thể sử dụng được ở phạm vi xa nhất mới quan trọng làm giảm việc tìm kiếm thành một lần quét được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^n n)`|`O(n)`| Quá chậm | 
| Tham lam tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mỗi khoảng cùng với số phiên bản gốc của nó, sau đó sắp xếp các khoảng theo điểm cuối bên trái của chúng. Việc sắp xếp cho phép chúng tôi xử lý tất cả các khoảng thời gian hiện có thể tiếp tục tiền tố được đề cập trong một lần quét chuyển tiếp. 
2. Đặt`covered = 0`. Điều này có nghĩa là chưa có chức năng nào được đề cập, vì vậy chức năng đầu tiên chúng ta cần là`1`. 
3. Quét các khoảng thời gian đã sắp xếp. Trong khi một khoảng có`a_i <= covered + 1`, nó có thể kết nối trực tiếp với tiền tố đã được bảo vệ. Trong số tất cả các khoảng thời gian như vậy, hãy giữ khoảng thời gian có giá trị lớn nhất`b_i`. 
4. Khi khoảng thời gian tiếp theo bắt đầu sau`covered + 1`, ngừng xem xét nhóm hiện tại và mua khoảng thời gian đã ghi nhớ. Đó là sự lựa chọn tốt nhất có thể trong số mọi khoảng thời gian có thể mở rộng tiền tố hiện tại. 
5. Cập nhật`covered`đến điểm cuối bên phải của khoảng đã chọn và thêm chỉ mục ban đầu của nó vào câu trả lời. Sau đó tiếp tục quét từ khoảng đầu tiên chưa được xem xét. 
6. Nếu không tìm thấy khoảng thời gian sử dụng được thì chức năng tiếp theo sẽ không được thực hiện. Có một khoảng cách giữa`covered`và các khoảng còn lại, vậy câu trả lời đúng là`NO`. 
7. Nếu`covered >= m`, toàn bộ phạm vi yêu cầu`[1,m]`được che phủ. Các khoảng được chọn tạo thành một giải pháp hợp lệ. 

Lý do lựa chọn tham lam là tối ưu được nắm bắt bởi một đối số trao đổi. Giả sử tiền tố hiện tại kết thúc tại`covered`. Mọi giải pháp hợp lệ phải chọn một khoảng nào đó với`a_i <= covered + 1`để tiếp tục đưa tin. Hãy để thuật toán tham lam chọn khoảng đủ điều kiện kết thúc tại`G`, trong khi giải pháp tối ưu chọn khoảng thời gian đủ điều kiện kết thúc tại`O`. Vì sự lựa chọn tham lam tối đa hóa điểm cuối đúng,`G >= O`. Việc thay thế khoảng thời gian được chọn đầu tiên của giải pháp tối ưu bằng khoảng tham lam không thể làm cho bất kỳ phạm vi bao phủ nào sau đó trở nên khó khăn hơn, bởi vì khoảng tham lam ít nhất cũng đạt đến mức xa nhất. Việc lặp lại đối số này ở mọi phần mở rộng cho thấy thuật toán tham lam sử dụng số khoảng tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    intervals = []
    for idx in range(1, n + 1):
        a, b = map(int, input().split())
        intervals.append((a, b, idx))

    intervals.sort()

    ans = []
    covered = 0
    i = 0

    while covered < m:
        best_right = covered
        best_idx = -1

        while i < n and intervals[i][0] <= covered + 1:
            a, b, idx = intervals[i]

            if b > best_right:
                best_right = b
                best_idx = idx

            i += 1

        if best_idx == -1:
            print("NO")
            return

        ans.append(best_idx)
        covered = best_right

    print("YES")
    print(len(ans))
    print(*ans)

if __name__ == "__main__":
    solve()
```Đầu vào đầu tiên được chuyển đổi thành`(left, right, original_index)`gấp ba lần. Việc giữ lại chỉ mục gốc là cần thiết vì việc sắp xếp sẽ thay đổi thứ tự các khoảng được lưu trữ, trong khi đầu ra phải tham chiếu đến các phiên bản khi chúng xuất hiện trong đầu vào. 

Việc quét được sắp xếp sử dụng`covered + 1`còn hơn là`covered`. Vì các hàm được đánh số bằng số nguyên, nên một khoảng bắt đầu ở chính xác hàm chưa được khám phá tiếp theo sẽ kết nối hoàn hảo với tiền tố hiện tại. Ví dụ, bảo hiểm thông qua`5`có thể được mở rộng bởi`[6,8]`. 

Bên trong vòng lặp bên trong,`best_right`ghi lại điểm cuối xa nhất trong số mỗi khoảng thời gian hiện có thể sử dụng được. Chúng tôi tiến lên`i`ngay sau khi một khoảng được kiểm tra, do đó mỗi khoảng sẽ đi vào vòng lặp bên trong đúng một lần. Khi khoảng thời gian tiếp theo có`a_i > covered + 1`, không thể sử dụng khoảng chưa được kiểm tra ở bước hiện tại vì các khoảng được sắp xếp theo điểm cuối bên trái của chúng. 

các`best_idx == -1`kiểm tra xử lý cả tập hợp có thể sử dụng trống và tình huống trong đó mọi khoảng thời gian có thể sử dụng đều kết thúc không xa hơn`covered`. Vì mỗi khoảng có`b_i >= a_i`, một khoảng thỏa mãn`a_i <= covered + 1`vẫn có thể không mở rộng tiền tố chỉ khi nó kết thúc tại hoặc trước`covered`. Khoảng thời gian như vậy không thể giúp ích được gì, vì vậy hãy coi nó như một lựa chọn không kéo dài là đúng. 

Số nguyên Python có độ chính xác tùy ý, do đó giới hạn trên`m <= 10^9`không gây ra vấn đề tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các khoảng đã được sắp xếp theo điểm cuối bên trái của chúng. Ban đầu không có gì được đề cập nên chỉ có một khoảng thời gian bắt đầu từ`1`có thể được chọn. Sau khi xem xét nó, mức độ bao phủ đạt đến`2`. Khoảng thời gian có thể sử dụng tiếp theo bắt đầu lúc`3`, vân vân. 

|`covered`trước bước | Khoảng thời gian có thể sử dụng | Điểm cuối bên phải tốt nhất | Phiên bản được chọn |`covered`sau bước | 
| --- | --- | --- | --- | --- | 
| 0 |`[1,2]`| 2 | 1 | 2 | 
| 2 |`[3,4]`| 4 | 2 | 4 | 
| 4 |`[5,6]`| 6 | 3 | 6 | 
| 6 |`[7,8]`| 8 | 4 | 8 | 

Thuật toán đạt`m = 8`sau bốn lựa chọn, tạo ra`YES`,`4`và các chỉ số phiên bản`1 2 3 4`. Mỗi khoảng đã chọn buộc phải tiếp tục trực tiếp từ tiền tố trước đó, do đó, bất biến vẫn có hiệu lực sau mỗi bước. 

### Mẫu 2 

Các khoảng là`[1,5]`,`[2,7]`,`[3,4]`, Và`[6,8]`. Lúc đầu, ba khoảng đầu tiên có thể sử dụng được vì điểm cuối bên trái của chúng nhiều nhất là`1`chỉ trong khoảng thời gian đầu tiên, vì vậy phiên bản`1`được chọn và phạm vi phủ sóng đạt tới`5`. Bây giờ mọi khoảng thời gian bắt đầu nhiều nhất`6`có thể sử dụng được, bao gồm cả phiên bản`4`, đạt tới`8`. 

|`covered`trước bước | Khoảng thời gian được xem xét trong bước này | Điểm cuối bên phải tốt nhất | Phiên bản được chọn |`covered`sau bước | 
| --- | --- | --- | --- | --- | 
| 0 |`[1,5]`| 5 | 1 | 5 | 
| 5 |`[2,7]`,`[3,4]`,`[6,8]`| 8 | 4 | 8 | 

Kết quả là có hai phiên bản,`1`Và`4`. Phiên bản`2`chỉ đạt tới`7`, vì vậy việc chọn nó thay vào đó sẽ yêu cầu một phiên bản khác để tiếp cận`8`. Sự lựa chọn tham lam sẽ tránh việc mua thêm đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Phân loại chi phí`O(n log n)`và lần quét tiếp theo sẽ kiểm tra từng khoảng thời gian một lần | 
| Không gian |`O(n)`| Mảng khoảng và các chỉ số phiên bản được chọn chứa tối đa`n`yếu tố | 

Với`n <= 200000`, việc sắp xếp chiếm ưu thế trong thời gian chạy và nằm trong phạm vi dự định cho giải pháp hai giây. Giá trị của`m`có thể lớn như`10^9`, nhưng thuật toán không bao giờ lặp qua các hàm riêng lẻ, do đó giới hạn lớn đó không ảnh hưởng đến thời gian chạy. Việc sử dụng bộ nhớ là tuyến tính trong`n`, thoải mái dưới 256 MB. 

## Trường hợp thử nghiệm 

Trình trợ giúp kiểm tra bên dưới chạy logic tham lam tương tự trên đầu vào trong bộ nhớ và xác thực bộ phiên bản được trả về thay vì yêu cầu một bộ hợp lệ cụ thể. Điều này quan trọng vì bài toán cho phép bất kỳ giải pháp tối ưu nào.```python
import sys
import io

def solve_io():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    intervals = []

    for idx in range(1, n + 1):
        a, b = map(int, input().split())
        intervals.append((a, b, idx))

    intervals.sort()

    ans = []
    covered = 0
    i = 0

    while covered < m:
        best_right = covered
        best_idx = -1

        while i < n and intervals[i][0] <= covered + 1:
            a, b, idx = intervals[i]
            if b > best_right:
                best_right = b
                best_idx = idx
            i += 1

        if best_idx == -1:
            print("NO")
            return

        ans.append(best_idx)
        covered = best_right

    print("YES")
    print(len(ans))
    print(*ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve_io()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def check(inp: str, out: str) -> bool:
    lines = out.strip().splitlines()
    first = lines[0]

    data = inp.strip().split()
    it = iter(data)
    n = int(next(it))
    m = int(next(it))

    intervals = []
    for idx in range(1, n + 1):
        a = int(next(it))
        b = int(next(it))
        intervals.append((a, b))

    # A small independent greedy check tells us whether coverage is possible
    intervals_sorted = sorted(intervals)
    covered = 0
    i = 0
    possible = True

    while covered < m:
        best = covered

        while i < n and intervals_sorted[i][0] <= covered + 1:
            best = max(best, intervals_sorted[i][1])
            i += 1

        if best == covered:
            possible = False
            break

        covered = best

    if not possible:
        return first == "NO"

    if first != "YES":
        return False

    k = int(lines[1])
    chosen = list(map(int, lines[2].split()))

    if k != len(chosen) or k == 0:
        return False

    if len(set(chosen)) != k:
        return False

    if any(x < 1 or x > n for x in chosen):
        return False

    chosen_intervals = [intervals[x - 1] for x in chosen]
    chosen_intervals.sort()

    covered = 0
    for a, b in chosen_intervals:
        if a > covered + 1:
            return False
        covered = max(covered, b)

    return covered >= m

# Provided samples
sample1 = """\
4 8
1 2
3 4
5 6
7 8
"""
assert check(sample1, run(sample1)), "sample 1"

sample2 = """\
4 8
1 5
2 7
3 4
6 8
"""
assert check(sample2, run(sample2)), "sample 2"

sample3 = """\
3 8
1 3
4 5
6 7
"""
assert check(sample3, run(sample3)), "sample 3"

# Minimum-size input
case4 = """\
1 1
1 1
"""
assert check(case4, run(case4)), "minimum-size case"

# All intervals equal
case5 = """\
5 10
1 10
1 10
1 10
1 10
1 10
"""
assert check(case5, run(case5)), "all-equal intervals"

# Exact boundary connection: [1,2] followed by [3,5]
case6 = """\
2 5
1 2
3 5
"""
assert check(case6, run(case6)), "exact covered+1 boundary"

# Gap at function 3
case7 = """\
3 5
1 2
4 5
1 1
"""
assert check(case7, run(case7)), "gap case"

# Large n with a single interval covering everything
case8 = "200000 1000000000\n" + "1 1000000000\n" * 200000
assert check(case8, run(case8)), "maximum-n case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 1`|`YES`, một phiên bản | Giá trị tối thiểu và phạm vi bao phủ nhỏ nhất có thể | 
| Năm bản sao của`[1,10]`|`YES`, một phiên bản | Khoảng thời gian trùng lặp và hoàn toàn bằng nhau | 
|`[1,2]`,`[3,5]`|`YES`, hai phiên bản | Việc sử dụng đúng các`covered + 1`ranh giới | 
|`[1,2]`,`[4,5]`,`[1,1]`|`NO`| Phát hiện chức năng chưa được phát hiện chính hãng | 
|`200000`bản sao của`[1,10^9]`|`YES`, một phiên bản | Tối đa`n`và lớn`m`mà không cần lặp lại các hàm | 

## Vỏ cạnh 

### Khoảng thời gian sử dụng đầu tiên phải đạt chức năng 1 

Hãy xem xét```
2 5
2 5
1 1
```Ban đầu`covered = 0`, vậy điều kiện là`a_i <= 1`. Phiên bản`2`,`[1,1]`, là khoảng thời gian duy nhất có thể sử dụng được và mở rộng phạm vi phủ sóng tới`1`. Khoảng thời gian có thể sử dụng tiếp theo là`[2,5]`, bắt đầu lúc`covered + 1 = 2`, do đó mức độ bao phủ đạt đến`5`. Đầu ra của thuật toán`YES`với hai phiên bản. Chiến lược chọn khoảng có điểm cuối bên phải lớn nhất mà không kiểm tra kết nối trước sẽ thất bại trong trường hợp này. 

### Khoảng cách thực sự phải tạo ra KHÔNG 

cho```
2 5
1 2
4 5
```Bước đầu tiên chọn`[1,2]`, cho`covered = 2`. Khoảng thời gian tiếp theo bắt đầu lúc`4`, trong khi chức năng cần thiết tiếp theo là`3`. Từ`4 > 2 + 1`, không có khoảng nào có thể bao hàm hàm`3`, Và`best_idx`vẫn chưa được đặt. Thuật toán xuất ra ngay lập tức`NO`. 

### Bắt đầu chính xác ở hàm tiếp theo là hợp lệ 

cho```
2 5
1 2
3 5
```khoảng thời gian đầu tiên tạo ra`covered = 2`. Khoảng thứ hai có`a = 3`, thỏa mãn`a <= covered + 1`. Nó được chọn và mở rộng phạm vi phủ sóng tới`5`. Câu trả lời là`YES`với hai phiên bản. sử dụng`a <= covered`thay vào đó sẽ từ chối trường hợp hợp lệ này một cách không chính xác. 

### Không nên chọn khoảng thời gian không mở rộng phạm vi phủ sóng 

Giả sử```
3 6
1 3
2 3
4 6
```Sau khi chọn`[1,3]`, khoảng tiếp theo`[2,3]`đủ điều kiện về mặt kỹ thuật vì`2 <= 4`, nhưng nó không làm tăng tiền tố được bảo hiểm. Thuật toán ghi lại nó nhưng để lại`best_right = 3`vì điểm cuối của nó không lớn hơn. Sau đó`[4,6]`cũng đủ điều kiện và trở thành sự lựa chọn tốt nhất, mở rộng phạm vi bảo hiểm tới`6`. Sự dư thừa`[2,3]`không bao giờ được mua. 

Chi tiết này rất hữu ích vì tính đủ điều kiện và tính hữu ích là những khái niệm khác nhau. Một khoảng thời gian có thể chồng lấp tiền tố hiện tại mà không mở rộng nó và khoảng thời gian như vậy không được tính là tiến trình. 

### Các khoảng chồng chéo yêu cầu điểm cuối xa nhất 

cho```
4 8
1 3
2 5
4 7
6 8
```bước đầu tiên chỉ xem xét`[1,3]`, Vì thế`covered`trở thành`3`. Bước tiếp theo có thể sử dụng`[2,5]`, đạt`5`. Từ đó`[4,7]`có thể sử dụng được và đạt được`7`, theo sau là`[6,8]`. Thuật toán chọn bốn khoảng. 

Nếu một đầu vào thay thế chứa`[2,7]`thay vì`[2,5]`, thuật toán tham lam ngay lập tức ưu tiên khoảng đó hơn vì nó vươn xa hơn. Sự lựa chọn đó có thể loại bỏ việc mua hàng sau này. Đối số trao đổi đảm bảo rằng việc chọn điểm cuối có thể tiếp cận xa nhất không bao giờ làm tăng số khoảng thời gian tối thiểu được yêu cầu sau đó.
