---
title: "CF 102215E - Phần mềm của bên thứ ba - 2"
description: "Chúng tôi có (n) phiên bản thư viện. Phiên bản (i) cung cấp mọi hàm có số nằm trong khoảng bao hàm ([ai,bi]). Pavel cần mọi hàm từ (1) đến (m), do đó các phiên bản được chọn phải bao trùm toàn bộ khoảng ([1,m]). Nhiệm vụ có hai phần."
date: "2026-08-20T02:46:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "E"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 384
verified: false
draft: false
---

[CF 102215E - Phần mềm của bên thứ ba - 2](https://codeforces.com/problemset/problem/102215/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có (n) phiên bản thư viện. Phiên bản (i) cung cấp mọi hàm có số nằm trong khoảng bao gồm ([a_i,b_i]). Pavel cần mọi hàm từ (1) đến (m), do đó các phiên bản được chọn phải bao trùm toàn bộ khoảng ([1,m]). 

Nhiệm vụ có hai phần. Đầu tiên, chúng ta phải xác định liệu tập hợp các phiên bản như vậy có tồn tại hay không. Nếu đúng như vậy, chúng ta phải tìm số lượng phiên bản nhỏ nhất có thể và xuất ra chỉ số của chúng. Các khoảng thời gian có thể trùng nhau và một phiên bản chỉ có thể được sử dụng một lần. 

Giá trị của (m) có thể lớn bằng (10^9), do đó việc xử lý mọi số hàm như một vị trí mảng riêng biệt là không khả thi. Số lượng khoảng thời gian tối đa là (2\cdot10^5), có nghĩa là thuật toán (O(n^2)) sẽ yêu cầu khoảng (4\cdot10^{10}) hoạt động khoảng thời gian trong trường hợp xấu nhất. Với giới hạn 2 giây, chúng ta cần giá trị nào đó gần với (O(n\log n)) hoặc (O(n)). Sắp xếp các khoảng một lần là có thể chấp nhận được, trong khi việc so sánh nhiều lần từng cặp thì không. 

Có một số trường hợp ranh giới có thể làm cho một giải pháp hợp lý khác thất bại. Nếu khoảng thời gian hữu ích đầu tiên không bắt đầu ở hàm (1), câu trả lời ngay lập tức là không thể. Ví dụ,```
2 5
2 5
3 5
```không có phiên bản nào chứa hàm (1) nên đáp án là`NO`. Một giải pháp chỉ kiểm tra xem điểm cuối bên phải lớn nhất đạt tới (m) có chấp nhận sai hay không. 

Trường hợp thứ hai là một khoảng trống ở giữa:```
3 8
1 3
4 5
7 8
```Các khoảng bao gồm (1) đến (5), sau đó không để lại hàm (6). Đầu ra đúng là`NO`. Chỉ kiểm tra điểm cuối tối thiểu và tối đa của liên kết là không đủ vì các khoảng thời gian bị ngắt kết nối không thể bù đắp được khoảng cách. 

Một trường hợp tinh tế khác là các khoảng chồng chéo trong đó việc lấy khoảng thời gian có sẵn đầu tiên là không tối ưu:```
3 8
1 3
1 5
5 8
```Câu trả lời tối ưu sử dụng phiên bản (2) và (3), bao gồm ([1,5]) và sau đó ([5,8]). Việc chọn phiên bản (1) trước tiên sẽ dẫn đến tiền tố ngắn hơn và yêu cầu phiên bản bổ sung. Thuật toán tham lam phải chọn khoảng kéo dài tiền tố được bao phủ xa nhất chứ không chỉ đơn giản là khoảng đầu tiên có thể tiếp tục nó. 

Cuối cùng, khi một khoảng đã bao gồm mọi thứ, câu trả lời phải chứa chính xác một phiên bản:```
1 10
1 10
```Kết quả đúng là`YES`, theo sau là`1`và phiên bản`1`. Một thuật toán nhất quyết tìm khoảng thứ hai sau khi đạt tới (m) sẽ đưa ra một lựa chọn không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là xem xét mọi tập hợp con của (n) phiên bản. Đối với mỗi tập hợp con, chúng ta có thể thu thập các khoảng của nó, xác định hợp và kiểm tra xem hợp đó có chứa mọi hàm từ (1) đến (m) hay không. Trong số tất cả các tập con thành công, chúng ta giữ lại một tập con có kích thước tối thiểu. Điều này đúng vì mọi quyết định mua hàng có thể đều tương ứng với một tập hợp con. 

Vấn đề là số lượng tập hợp con. Có (2^n) trong số chúng và thậm chí kiểm tra một tập hợp con bằng cách quét tất cả các khoảng (n) sẽ cho thời gian (O(n2^n)). Với (n=200000), điều này vượt xa mọi giới hạn thực tế. Ngay cả một thuật toán bằng cách nào đó kiểm tra từng tập hợp con trong thời gian không đổi vẫn có trạng thái không thể (2^{200000}). 

Cấu trúc hữu ích xuất phát từ thực tế là tất cả các hàm tạo thành một dòng có thứ tự, từ (1) đến (m). Giả sử chúng ta đã mua một số phiên bản và chúng bao gồm mọi chức năng thông qua (x). Để tiếp tục đưa tin mà không có khoảng trống, khoảng thời gian tiếp theo phải bắt đầu bằng hoặc trước (x+1). Trong số mọi khoảng thỏa mãn điều kiện này, việc chọn khoảng có điểm cuối bên phải lớn nhất ít nhất luôn tốt bằng việc chọn bất kỳ khoảng nào khác. Nó đạt ít nhất là xa trong khi sử dụng chính xác một phiên bản. 

Điều này đưa ra một chiến lược tham lam. Sắp xếp các khoảng theo điểm cuối bên trái của chúng. Bắt đầu không có gì được che đậy, hãy xem xét liên tục mọi khoảng có điểm cuối bên trái nhiều nhất là hàm chưa được che phủ đầu tiên. Trong số những khoảng đó, hãy lấy khoảng cách xa nhất về bên phải. Nếu không có khoảng thời gian nào như vậy mở rộng phạm vi phủ sóng hiện tại thì sẽ không thể tránh khỏi một khoảng trống và câu trả lời là`NO`. 

Lý do sự lựa chọn tham lam này là tối ưu là một đối số trao đổi. Giả sử tiền tố hiện được đề cập kết thúc tại (x) và để thuật toán tham lam chọn khoảng kết thúc tại (g). Bất kỳ nghiệm hợp lệ nào tiếp tục từ (x) đều phải chọn một khoảng nào đó có điểm cuối bên trái nhiều nhất là (x+1). Nếu khoảng đó kết thúc tại (r), thì (g\ge r). Việc thay thế nó bằng khoảng tham lam không thể làm cho phạm vi bao phủ còn lại trở nên khó khăn hơn, bởi vì lựa chọn tham lam ít nhất cũng đạt đến mức xa nhất. Việc lặp lại đối số này ở mỗi bước sẽ đưa ra giải pháp với số khoảng thời gian tối thiểu có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n2^n)) | (O(n)) | Quá chậm | 
| Tham lam sau khi phân loại | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các khoảng cùng với chỉ số phiên bản gốc của chúng, sau đó sắp xếp chúng theo điểm cuối bên trái. Việc sắp xếp cho phép chúng tôi xử lý các khoảng thời gian theo thứ tự chính xác mà chúng có thể sử dụng được. 
2. Đặt`covered = 0`. Điều này có nghĩa là các hàm (1) đến`covered`đã được đảm bảo sẵn có. Ban đầu không có chức năng nào được bảo hiểm. 
3. Duy trì một con trỏ`i`vào các khoảng đã sắp xếp. Tại mỗi lần lặp, hãy kiểm tra từng khoảng thời gian với`a[i] <= covered + 1`. Khoảng thời gian như vậy có thể được gắn vào tiền tố được bao phủ hiện tại mà không để lại khoảng trống. 
4. Trong số tất cả các khoảng hiện có thể sử dụng, hãy giữ khoảng có điểm cuối bên phải lớn nhất. Gọi điểm cuối này`best_end`và ghi nhớ chỉ mục phiên bản của nó. Chúng tôi không cam kết ngay về khoảng thời gian có thể sử dụng đầu tiên vì khoảng thời gian sau đó có thể mở rộng phạm vi bao phủ xa hơn nhiều. 
5. Sau tất cả các khoảng thời gian bắt đầu vào hoặc trước`covered + 1`đã được kiểm tra, kiểm tra xem`best_end`lớn hơn`covered`. Nếu không, không có khoảng nào có thể bao gồm hàm tiếp theo, do đó phạm vi hoàn chỉnh không thể được bao phủ và chúng tôi xuất ra`NO`. 
6. Nếu không, hãy chọn phiên bản đã nhớ, thêm chỉ mục gốc của nó vào câu trả lời và đặt`covered = best_end`. Lần lặp tiếp theo bây giờ sẽ cố gắng mở rộng tiền tố lớn hơn này. 
7. Dừng lại ngay khi`covered >= m`. Sau đó, mọi chức năng từ (1) đến (m) sẽ được đề cập và các phiên bản được chọn sẽ tạo thành một giải pháp hợp lệ. 
8. Bởi vì mỗi lần lặp lại chọn khoảng cách xa tiền tố hiện tại nhất, đối số trao đổi tham lam chứng minh rằng không thể tồn tại giải pháp nào có ít phiên bản hơn. Khoảng thời gian đã chọn có thể thay thế khoảng thời gian đầu tiên của bất kỳ sự tiếp tục tối ưu nào mà không làm giảm khả năng thực hiện các chức năng còn lại của nó. 

### Tại sao nó hoạt động 

Điều bất biến là trước mỗi phép chọn, tất cả các hàm từ (1) đến`covered`được bao phủ và các khoảng thời gian đã chọn sử dụng số lượng phiên bản tối thiểu có thể để đạt được ít nhất điều đó theo những lựa chọn tham lam. Mỗi khoảng có khả năng tiếp tục phủ sóng đều được xem xét trước lần lựa chọn tiếp theo và thuật toán sẽ chọn khoảng thời gian có điểm cuối bên phải tối đa. Bất kỳ lựa chọn tiếp theo hợp lệ thay thế nào cũng không thể tiến xa hơn, vì vậy việc thay thế lựa chọn đó bằng khoảng tham lam không thể làm tăng số khoảng cần thiết sau đó. Nếu không có khoảng thời gian sử dụng được kéo dài`covered`, hàm tiếp theo được phát hiện trong mỗi khoảng thời gian còn lại, do đó không tồn tại nghiệm hợp lệ. Khi`covered`đạt (m), các khoảng đã chọn sẽ bao phủ toàn bộ phạm vi yêu cầu với số lượng phiên bản tối thiểu có thể. 

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

    covered = 0
    i = 0
    answer = []

    while covered < m:
        best_end = covered
        best_idx = -1

        # Every interval starting at or before the next uncovered
        # function can extend the current prefix.
        while i < n and intervals[i][0] <= covered + 1:
            a, b, idx = intervals[i]

            if b > best_end:
                best_end = b
                best_idx = idx

            i += 1

        # No usable interval can extend the covered prefix.
        if best_idx == -1:
            print("NO")
            return

        answer.append(best_idx)
        covered = best_end

    print("YES")
    print(len(answer))
    print(*answer)

if __name__ == "__main__":
    solve()
```Bộ dữ liệu`(a, b, idx)`lưu trữ cả điểm cuối và số phiên bản gốc. Sắp xếp các bộ dữ liệu này chủ yếu sắp xếp theo`a`, đó chính xác là thứ tự cần thiết cho quá trình quét tham lam.`covered + 1`là chức năng đầu tiên chưa được đề cập. Một khoảng có thể sử dụng được khi điểm cuối bên trái của nó có giá trị tối đa bằng giá trị này. Điều kiện này cũng xử lý các khoảng thời gian chồng chéo một cách chính xác. Ví dụ, nếu`covered == 5`, một khoảng bắt đầu tại`5`có thể sử dụng được vì nó đã chồng lên vùng được che phủ, trong khi vùng bắt đầu từ`7`chức năng lá`6`chưa được khám phá. 

Vòng lặp bên trong tiến lên`i`vĩnh viễn. Khi một khoảng đã được coi là ứng cử viên cho tiền tố hiện tại, nó sẽ không bao giờ cần được kiểm tra lại. Nếu điểm cuối bên phải của nó không phải là phần mở rộng tốt nhất hiện tại, thì nó không thể trở thành ứng cử viên tốt hơn sau khi tiền tố được bảo hiểm di chuyển về phía trước, vì điểm cuối của nó là cố định và thuật toán chỉ quan tâm đến các khoảng mở rộng biên giới hiện tại. 

Séc`best_idx == -1`phát hiện cả khoảng trống ban đầu và khoảng trống xuất hiện sau đó. Ví dụ, nếu`covered == 3`và mọi khoảng thời gian còn lại bắt đầu tại`5`hoặc muộn hơn, không ai có thể che đậy được chức năng`4`, vì vậy việc tiếp tục sẽ là không thể. 

Thuật toán dừng ngay khi`covered >= m`, do đó khoảng vượt quá (m) là hoàn toàn có thể chấp nhận được. Không cần phải cắt điểm cuối của nó. Số nguyên Python cũng có độ chính xác tùy ý, do đó không có vấn đề tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các khoảng đã được sắp xếp theo thứ tự tăng dần của điểm cuối bên trái của chúng. Bảng hiển thị đường biên tham lam sau mỗi lần lựa chọn. 

| Lặp lại | Tiếp theo được khám phá | Khoảng thời gian có thể sử dụng | Phiên bản được chọn | Mới được bảo hiểm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1:[1,2] | 1 | 2 | 
| 2 | 3 | 2:[3,4] | 2 | 4 | 
| 3 | 5 | 3:[5,6] | 3 | 6 | 
| 4 | 7 | 4:[7,8] | 4 | 8 | 

Sau khi chọn phiên bản (1), hàm (3) sẽ trở thành hàm chưa được khám phá tiếp theo, vì vậy phiên bản (2) chính xác là loại khoảng mà quy tắc tham lam cần. Quá trình tương tự tiếp tục cho đến khi`covered = 8`, đưa ra bốn phiên bản được lựa chọn`1 2 3 4`. 

Ví dụ này cũng cho thấy tại sao các khoảng được diễn giải một cách toàn diện. Khoảng ([1,2]) theo sau là ([3,4]) không có khoảng cách vì hàm (2) và hàm (3) liên tiếp. 

### Mẫu 2 

Các khoảng là```
1: [1,5]
2: [2,7]
3: [3,4]
4: [6,8]
```Sau khi sắp xếp, chúng đã có sẵn theo thứ tự hiển thị. Quá trình quét hoạt động như sau. 

| Lặp lại | Tiếp theo được khám phá | Khoảng thời gian có thể sử dụng được kiểm tra | Phiên bản được chọn | Mới được bảo hiểm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1:[1,5] | 1 | 5 | 
| 2 | 6 | 2:[2,7], 3:[3,4], 4:[6,8] | 4 | 8 | 

Ở lần lặp đầu tiên, chỉ khoảng bắt đầu từ (1) mới có thể bắt đầu phủ sóng, do đó phiên bản (1) được chọn. Khi các chức năng (1) đến (5) được đề cập, cả phiên bản (2) và phiên bản (4) đều có thể tiếp tục phạm vi. Phiên bản (4) đạt (8), trong khi phiên bản (2) chỉ đạt (7), nên sự lựa chọn tham lam là phiên bản (4). 

Câu trả lời cuối cùng là`1 4`, sử dụng hai phiên bản. Không có phiên bản nào bao gồm cả chức năng (1) và chức năng (8), vì vậy hai là tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Sắp xếp chi phí (O(n\log n)) và mỗi khoảng thời gian sẽ được quét một lần sau đó. | 
| Không gian | (O(n)) | Các khoảng và chỉ số phiên bản đã chọn yêu cầu lưu trữ tuyến tính. | 

Với (n\le200000), các khoảng thời gian sắp xếp (200000) và sau đó thực hiện một lần tuyến tính nằm trong thang đo dự định cho giới hạn 2 giây trong Python. Giá trị của (m) không xuất hiện ở độ phức tạp vì thuật toán không bao giờ lặp qua các hàm riêng lẻ. Ngay cả khi (m=10^9), nó chỉ so sánh các điểm cuối khoảng. 

## Trường hợp thử nghiệm 

Đầu ra có thể chứa bất kỳ tập hợp chỉ số phiên bản tối ưu nào, do đó, việc khai thác thử nghiệm mạnh mẽ sẽ xác thực giải pháp được trả về thay vì yêu cầu một thứ tự chỉ số cụ thể. Các thử nghiệm sau đây thực hiện điều đó đồng thời kiểm tra xem số lượng phiên bản đã chọn có tối ưu cho các trường hợp được cung cấp hay không.```python
# This test harness reimplements the solution as a callable function.
import sys
import io

def solve_io(data: str) -> str:
    inp = io.StringIO(data)
    out = io.StringIO()

    n, m = map(int, inp.readline().split())
    intervals = []

    for idx in range(1, n + 1):
        a, b = map(int, inp.readline().split())
        intervals.append((a, b, idx))

    intervals.sort()

    covered = 0
    i = 0
    answer = []

    while covered < m:
        best_end = covered
        best_idx = -1

        while i < n and intervals[i][0] <= covered + 1:
            a, b, idx = intervals[i]
            if b > best_end:
                best_end = b
                best_idx = idx
            i += 1

        if best_idx == -1:
            out.write("NO\n")
            return out.getvalue()

        answer.append(best_idx)
        covered = best_end

    out.write("YES\n")
    out.write(str(len(answer)) + "\n")
    out.write(" ".join(map(str, answer)) + "\n")
    return out.getvalue()

def validate(data: str, output: str, expected_k=None):
    lines = output.strip().splitlines()
    assert lines, "empty output"

    if lines[0] == "NO":
        assert expected_k is None
        return

    assert lines[0] == "YES"
    assert len(lines) == 3

    n, m = map(int, data.splitlines()[0].split())
    intervals = [None]

    for line in data.splitlines()[1:]:
        a, b = map(int, line.split())
        intervals.append((a, b))

    k = int(lines[1])
    chosen = list(map(int, lines[2].split()))

    assert len(chosen) == k
    assert len(set(chosen)) == k
    assert all(1 <= x <= n for x in chosen)

    covered = [False] * (m + 1)
    for idx in chosen:
        a, b = intervals[idx]
        for x in range(a, b + 1):
            covered[x] = True

    assert all(covered[1:]), "selected intervals do not cover [1, m]"

    if expected_k is not None:
        assert k == expected_k

# Provided sample 1
sample1 = """\
4 8
1 2
3 4
5 6
7 8
"""
out = solve_io(sample1)
validate(sample1, out, expected_k=4)

# Provided sample 2
sample2 = """\
4 8
1 5
2 7
3 4
6 8
"""
out = solve_io(sample2)
validate(sample2, out, expected_k=2)

# Provided sample 3
sample3 = """\
3 8
1 3
4 5
6 7
"""
out = solve_io(sample3)
assert out.strip() == "NO"

# Minimum-size input: one version covers the only function.
case4 = """\
1 1
1 1
"""
out = solve_io(case4)
validate(case4, out, expected_k=1)

# All intervals equal. One copy is sufficient.
case5 = """\
5 10
1 10
1 10
1 10
1 10
1 10
"""
out = solve_io(case5)
validate(case5, out, expected_k=1)

# Greedy choice matters. Taking [1, 3] first would need more intervals.
case6 = """\
4 10
1 3
1 6
4 8
7 10
"""
out = solve_io(case6)
validate(case6, out, expected_k=2)

# Boundary gap at the beginning.
case7 = """\
3 5
2 5
3 5
1 1
"""
out = solve_io(case7)
validate(case7, out, expected_k=2)

# Maximum-size input pattern. Each interval covers one function.
n = 200000
case8 = str(n) + " " + str(n) + "\n"
case8 += "".join(f"{i} {i}\n" for i in range(1, n + 1))
out = solve_io(case8)
validate(case8, out, expected_k=n)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 1`|`YES`, (k=1) | Đầu vào kích thước tối thiểu và chấm dứt ngay lập tức | 
| Năm bản sao của`[1,10]`|`YES`, (k=1) | Khoảng thời gian trùng lặp và tránh các lựa chọn không cần thiết | 
|`[1,3], [1,6], [4,8], [7,10]`|`YES`, (k=2) | Chọn khoảng cách xa nhất | 
|`[2,5], [3,5], [1,1]`|`YES`, (k=2) | Ranh giới đầu và`covered + 1`xử lý | 
| (200000) khoảng đơn |`YES`, (k=200000) | Tối đa (n), quét tuyến tính sau khi sắp xếp và các ranh giới liên tiếp | 

Ngoài ra, các mẫu được cung cấp còn bao gồm các khoảng thời gian liên tiếp chính xác, các khoảng thời gian chồng chéo và khoảng trống không thể có. 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là một sự khởi đầu chưa được khám phá. Coi như```
2 5
2 5
3 5
```Giá trị ban đầu là`covered = 0`, vậy hàm cần thiết tiếp theo là`1`. Cả hai khoảng đều có điểm cuối lớn hơn`1`, nghĩa là vòng lặp bên trong kiểm tra không có khoảng thời gian có thể sử dụng được.`best_idx`còn lại`-1`và thuật toán in`NO`. Không có vấn đề gì khi một số khoảng đạt đến hàm (5), bởi vì hàm (1) đã không thể đạt được. 

Trường hợp khoảng cách giữa là```
3 8
1 3
4 5
7 8
```Lựa chọn đầu tiên thay đổi`covered`từ`0`ĐẾN`3`. Chức năng cần thiết tiếp theo là`4`, Vì thế`[4,5]`có thể sử dụng được và thay đổi`covered`ĐẾN`5`. Chức năng cần thiết tiếp theo là`6`, Nhưng`[7,8]`bắt đầu quá muộn. Không ứng cử viên nào có thể mở rộng tiền tố, do đó thuật toán sẽ in`NO`. Việc kiểm tra được thực hiện ở mọi biên giới, giúp ngăn chặn việc phạm vi phủ sóng bị ngắt kết nối bị nhầm lẫn với phạm vi phủ sóng hoàn chỉnh. 

Trường hợp lựa chọn tham lam là```
3 8
1 3
1 5
5 8
```Ban đầu cả hai`[1,3]`Và`[1,5]`có thể sử dụng được. Quá trình quét giữ điểm cuối lớn hơn và chọn phiên bản (2), đưa ra`covered = 5`. Chức năng cần thiết tiếp theo là`6`, Vì thế`[5,8]`có thể sử dụng được và mở rộng phạm vi phủ sóng tới`8`. Câu trả lời sử dụng hai phiên bản. Việc triển khai bất cẩn chọn khoảng thời gian có thể sử dụng đầu tiên sẽ chọn`[1,3]`, sau đó`[5,8]`không thể che được chức năng (4), buộc phải thực hiện thêm một bước không chính xác hoặc báo lỗi không chính xác. 

Trường hợp một phiên bản là```
1 10
1 10
```Lần lặp đầu tiên xem xét khoảng duy nhất và tập hợp`covered = 10`. Điều kiện vòng lặp bên ngoài`covered < m`bây giờ là sai, do đó thuật toán dừng ngay lập tức và đưa ra một phiên bản đã chọn. Điều này chứng tỏ tại sao điều kiện kết thúc phải được kiểm tra sau khi cập nhật biên thay vì tìm kiếm một khoảng khác một cách vô điều kiện. 

Trường hợp ranh giới kích thước tối đa sử dụng các khoảng đơn (200000)`[1,1], [2,2], ..., [200000,200000]`. Sau khi chọn`[i,i]`, hàm được yêu cầu tiếp theo chính xác là (i+1), vì vậy singleton tiếp theo có thể sử dụng được. Mỗi khoảng thời gian được xử lý một lần và thuật toán kết thúc sau (200000) lựa chọn. Điều này xác nhận rằng giải pháp không phụ thuộc vào (m) nhỏ và xử lý số lượng phiên bản lớn nhất được phép trong giới hạn (O(n\log n)) dự kiến.
