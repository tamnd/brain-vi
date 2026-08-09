---
title: "CF 102448G - Người Bạn Tuyệt Vời Của Peter"
description: "Chúng ta cần tính điểm của từng ứng viên từ luồng bài nộp toàn cầu. Peter đã chọn một tập hợp các bài toán và mỗi bài toán được chọn đều có điểm cố định. Một thí sinh đạt được điểm của bài toán đó một cách chính xác khi bài nộp của họ nhận được phán quyết AC."
date: "2026-08-08T12:19:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102448
codeforces_index: "G"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2020"
rating: 0
weight: 102448
solve_time_s: 742
verified: true
draft: false
---

[CF 102448G - Người bạn tuyệt vời của Peter](https://codeforces.com/problemset/problem/102448/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 12m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần tính điểm của từng ứng viên từ luồng bài nộp toàn cầu. Peter đã chọn một tập hợp các bài toán và mỗi bài toán được chọn đều có điểm cố định. Một thí sinh đạt được điểm của bài toán đó một cách chính xác khi bài nộp của họ nhận được phán quyết`AC`. 

Đầu vào đầu tiên cung cấp cho các tay cầm ứng viên. Sau đó, nó đưa ra các ID vấn đề đã chọn cùng với điểm số của chúng. Cuối cùng, nó đưa ra các bài nộp từ những người dùng tùy ý. Nội dung gửi chứa thông tin xử lý của người dùng, ID sự cố và phán quyết. Một số bài gửi có thể thuộc về những người dùng không phải là ứng cử viên và một số có thể liên quan đến các vấn đề mà Peter không chọn. Không nên ảnh hưởng đến điểm số của thí sinh. 

Đối với mỗi ứng cử viên, kết quả đầu ra phải giữ nguyên thứ tự ứng viên ban đầu và chứa phần xử lý của họ, theo sau là tổng điểm của tất cả các vấn đề đã chọn mà họ đã giải quyết. 

Hạn chế chính là có thể có tới 50.000 ứng viên, 50.000 bài toán được chọn và 50.000 bài nộp. Một giải pháp kiểm tra mọi bài nộp của mọi ứng viên sẽ thực hiện được nhiều nhất 

[ 
50.000 \times 50.000 = 2,5 \times 10^9 
] 

séc. Điều đó vượt xa những gì giới hạn thời gian 1 giây có thể đáp ứng. Chúng ta cần một cách tiếp cận gần với kích thước đầu vào tuyến tính. Vì tất cả các thẻ điều khiển và ID vấn đề có tối đa 20 ký tự nên việc sử dụng bảng băm để tra cứu trực tiếp cũng rất thiết thực. 

Có một số trường hợp việc thực hiện bất cẩn có thể âm thầm đưa ra câu trả lời sai. Đầu tiên, một`AC`của một người không phải là ứng cử viên không được đóng góp vào điểm số của bất kỳ ai. Ví dụ:```
1 1 1
alice
p1 100
bob p1 AC
```Đầu ra đúng là:```
alice 0
```Một giải pháp tích lũy điểm chỉ bằng bài toán sẽ cho Alice 100 điểm một cách không chính xác. 

Thứ hai, một bài toán không được chọn phải được bỏ qua ngay cả khi thí sinh giải được:```
1 1 1
alice
p1 100
alice p2 AC
```Đầu ra đúng là:```
alice 0
```Việc thực hiện bất cẩn có thể chấm điểm cho mọi`AC`gửi thay vì kiểm tra xem vấn đề có thuộc về tập hợp đã chọn hay không. 

Thứ ba, việc nộp bài sai không được ảnh hưởng đến điểm số. Coi như:```
1 1 2
alice
p1 100
alice p1 WA
alice p1 AC
```Đầu ra đúng là:```
alice 100
```Chỉ có`AC`vấn đề nộp hồ sơ. Càng sớm`WA`sẽ không có tác dụng. 

Cuối cùng, một ứng viên có thể nộp bài sai nhiều lần trước khi giải quyết một vấn đề. Chúng ta không được thêm điểm vấn đề khi xử lý những lần thử đó. Sự đảm bảo rằng người dùng sẽ không bao giờ gửi lại vấn đề tương tự sau khi nhận được`AC`có nghĩa là một khi`AC`xuất hiện, cặp vấn đề người dùng đó sẽ không tạo ra lần gửi khác sau đó. Do đó, chúng ta có thể cộng điểm khi`AC`gặp phải mà không cần cấu trúc ngăn ngừa trùng lặp riêng biệt. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là xử lý từng ứng viên một cách độc lập. Đối với một ứng cử viên, hãy quét tất cả các bài nộp và tìm kiếm các hồ sơ mà người dùng là ứng cử viên đó, phán quyết là`AC`, và bài toán đó là một trong những bài toán được chọn. Bất cứ khi nào tìm thấy bài nộp như vậy, hãy cộng điểm bài toán tương ứng. 

Cách tiếp cận này đúng vì mọi cách có thể để ứng viên kiếm điểm đều được xem xét rõ ràng. Tuy nhiên, nó lặp lại quá trình quét bài nộp tương tự cho mọi ứng viên. Với 50.000 ứng viên và 50.000 bài nộp, trường hợp xấu nhất là 2,5 tỷ so sánh bài nộp của ứng viên. Ngay cả khi mỗi lần so sánh rất rẻ thì khối lượng công việc đó vẫn quá lớn so với thời hạn. 

Quan điểm tốt hơn là xử lý mỗi lần gửi chính xác một lần. Việc gửi đã cho chúng tôi biết người dùng, vấn đề và phán quyết, vì vậy không có lý do gì để liên tục tìm kiếm ứng viên có liên quan. Chúng ta có thể xây dựng một bảng băm ánh xạ mọi mã xử lý ứng viên tới vị trí của nó trong đầu ra và một bảng băm khác ánh xạ mọi ID vấn đề đã chọn với điểm của nó. 

Sau đó, một bản đệ trình có phán quyết khác với`AC`có thể bỏ qua ngay. Đối với một`AC`, chúng ta tra cứu người dùng của nó trong bảng ứng viên và bài toán của nó trong bảng bài toán đã chọn. Nếu cả hai đều tồn tại, bài nộp duy nhất đó sẽ trực tiếp xác định ứng viên có số điểm cần tăng và số tiền cần bổ sung. 

Phương pháp vũ lực hoạt động vì cuối cùng nó sẽ kiểm tra mọi bài nộp có liên quan của mọi ứng viên, nhưng nó không thành công vì nó lặp lại công việc. Quan sát cho thấy mỗi lần gửi đều xác định độc lập nhiều nhất một ứng cử viên và một vấn đề được chọn cho phép chúng tôi biến vấn đề đó thành tra cứu bảng băm theo thời gian không đổi cho mỗi lần gửi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(CS) | O(P + C) | Quá chậm | 
| Tối ưu | O(C + P + S) dự kiến ​​| O(C + P) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng thí sinh, các bài toán được chọn và bài nộp. Lưu trữ mọi thẻ điều khiển ứng viên trong một từ điển cùng với vị trí của nó trong mảng đầu ra. Vị trí này rất hữu ích vì đầu ra cuối cùng phải sử dụng cùng thứ tự với đầu vào, bất kể thứ tự gửi được xử lý như thế nào. 
2. Đọc từng vấn đề đã chọn và điểm của nó. Lưu trữ cặp này trong một từ điển trong đó ID vấn đề là khóa và điểm là giá trị. Điều này chuyển đổi việc tìm kiếm điểm của vấn đề từ việc quét qua tất cả các vấn đề đã chọn thành tra cứu liên tục theo thời gian dự kiến. 
3. Tạo một mảng`C`số không. Lối vào`i`thể hiện số điểm tích lũy của thí sinh có tay cầm xuất hiện ở vị trí`i`. 
4. Xử lý mỗi lần gửi một lần. Nếu phán quyết của nó không`AC`, bỏ qua nó vì nó không thể tăng bất kỳ điểm nào. 
5. Đối với một`AC`gửi, tra cứu tay cầm của người dùng đã gửi trong từ điển ứng viên. Nếu không có phần xử lý thì bài dự thi thuộc về người không phải là thí sinh nên không ảnh hưởng đến câu trả lời. 
6. Nếu người dùng là thí sinh, hãy tra cứu bài toán đã gửi trong từ điển bài toán đã chọn. Nếu nó vắng mặt, vấn đề không được chọn và do đó không đóng góp được gì. 
7. Nếu cả hai lần tra cứu đều thành công, hãy cộng điểm của bài toán đã chọn vào điểm tích lũy của thí sinh. Sự đảm bảo về việc nộp bài sau một`AC`có nghĩa là cùng một người dùng sau đó không thể nhận được bài gửi khác cho vấn đề đó, do đó điểm được cộng đúng một lần. 
8. Sau khi tất cả bài nộp đã được xử lý, lặp lại danh sách ứng viên ban đầu và in từng tay cầm cùng với điểm tích lũy của nó. Việc giữ các điểm điều khiển theo thứ tự đầu vào đảm bảo rằng thứ tự đầu ra là chính xác. 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của danh sách bài nộp, điểm được lưu trữ của mỗi thí sinh bằng tổng số điểm của mọi bài toán được chọn mà thí sinh đó đã nhận được điểm`AC`trong tiền tố đó. Một không-`AC`sự phục tùng không thể thay đổi sự bất biến. MỘT`AC`từ một vấn đề không phải ứng cử viên hoặc không được chọn cũng không thể đóng góp. Đối với một`AC`từ một thí sinh về một bài toán đã chọn, điểm của thí sinh đó sẽ tăng đúng bằng số điểm của bài toán đó. Do đó, bất biến vẫn đúng sau mỗi lần gửi và sau lần gửi cuối cùng, nó đưa ra chính xác các câu trả lời được yêu cầu. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    C, P, S = map(int, input().split())

    candidates = []
    candidate_index = {}

    for i in range(C):
        handle = input().strip()
        candidates.append(handle)
        candidate_index[handle] = i

    problem_score = {}

    for _ in range(P):
        problem, score = input().split()
        problem_score[problem] = int(score)

    answer = [0] * C

    for _ in range(S):
        user, problem, verdict = input().split()

        if verdict != "AC":
            continue

        idx = candidate_index.get(user)
        if idx is None:
            continue

        score = problem_score.get(problem)
        if score is None:
            continue

        answer[idx] += score

    output = []
    for i in range(C):
        output.append(f"{candidates[i]} {answer[i]}")

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```các`candidates`list giữ nguyên thứ tự đầu vào chính xác, trong khi`candidate_index`cung cấp quyền truy cập nhanh vào vị trí điểm số tương ứng. Ví dụ, nếu`beza`là ứng cử viên thứ hai,`candidate_index["beza"]`là`1`, vì vậy mỗi bài nộp đủ điều kiện cho`beza`cập nhật`answer[1]`. 

các`problem_score`từ điển đóng vai trò tương tự cho các vấn đề. Chúng ta không cần lưu trữ riêng các vấn đề đã chọn vì sự tồn tại của khóa đã cho chúng ta biết rằng vấn đề đã được chọn, trong khi giá trị của nó cho điểm. 

Phán quyết được kiểm tra trước khi tra cứu từ điển. Điều này không cần thiết đối với độ phức tạp tiệm cận, nhưng nó tránh được những công việc không cần thiết đối với số lượng lớn các bài nộp không có triệu chứng.`AC`. 

sử dụng`.get()`cho phép chúng tôi phân biệt người dùng hoặc sự cố bị thiếu mà không đưa ra ngoại lệ. Điểm số hoàn toàn tích cực, vì vậy`None`không thể nhầm lẫn với điểm hợp lệ. 

Số nguyên Python tự động xử lý các giá trị lớn tùy ý. Mặc dù mỗi điểm riêng lẻ nhiều nhất là 20.000, nhưng tổng điểm có thể đạt xấp xỉ 1 tỷ khi nhiều vấn đề được chọn được giải quyết, vẫn được xử lý an toàn bằng số nguyên Python. 

Không có phép tính riêng lẻ nào vì các vị trí ứng viên được lưu trữ trực tiếp bằng cách sử dụng các chỉ số dựa trên số 0. Vòng lặp đầu ra sau đó truy cập chính xác các vị trí tương tự. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các ứng cử viên là`GabrielPessoa`Và`beza`. Các vấn đề được lựa chọn là`metebronca`, có giá trị 100, và`geometry`, trị giá 200. 

| Đệ trình | Phán quyết | Tra cứu ứng viên | Tra cứu vấn đề | Điểm sau khi nộp | 
| --- | --- | --- | --- | --- | 
|`beza metebronca AC`| AC |`beza -> 1`|`metebronca -> 100`| GabrielPessoa = 0, beza = 100 | 
|`ffern numbertheory AC`| AC | vắng mặt | không cần thiết | GabrielPessoa = 0, beza = 100 | 
|`GabrielPessoa geometry WA`| WA | không cần thiết | không cần thiết | GabrielPessoa = 0, beza = 100 | 
|`beza geometry AC`| AC |`beza -> 1`|`geometry -> 200`| GabrielPessoa = 0, beza = 300 | 

Lần nộp thứ hai là một`AC`, Nhưng`ffern`không phải là ứng cử viên nên bị loại bỏ. Đệ trình thứ ba là của một ứng cử viên và liên quan đến một vấn đề đã chọn, nhưng phán quyết của nó là`WA`, nên nó cũng bị loại bỏ. Hai vấn đề được lựa chọn thành công được giải quyết bằng`beza`đóng góp 100 và 200, tặng 300. 

Đầu ra cuối cùng là:```
GabrielPessoa 0
beza 300
```### Xây dựng ví dụ 2 

Hãy xem xét:```
3 2 5
alice
bob
carol
p1 50
p2 100
alice p1 WA
bob p3 AC
carol p2 AC
alice p1 AC
bob p1 AC
```Dấu vết là: 

| Đệ trình | Phán quyết | Ứng viên | Vấn đề đã chọn | Điểm sau khi nộp | 
| --- | --- | --- | --- | --- | 
|`alice p1 WA`| WA | chưa được xử lý | chưa được xử lý | alice = 0, bob = 0, carol = 0 | 
|`bob p3 AC`| AC |`bob`tìm thấy |`p3`vắng mặt | alice = 0, bob = 0, carol = 0 | 
|`carol p2 AC`| AC |`carol`tìm thấy |`p2 -> 100`| alice = 0, bob = 0, carol = 100 | 
|`alice p1 AC`| AC |`alice`tìm thấy |`p1 -> 50`| alice = 50, bob = 0, carol = 100 | 
|`bob p1 AC`| AC |`bob`tìm thấy |`p1 -> 50`| Alice = 50, Bob = 50, Carol = 100 | 

Đầu ra cuối cùng là:```
alice 50
bob 50
carol 100
```Ví dụ này thực hiện ba bộ lọc khác nhau. các`WA`bị bỏ qua,`AC`không được chọn`p3`bị bỏ qua và việc gửi thành công các bài toán đã chọn sẽ cập nhật chính xác ứng viên tương ứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(C + P + S) dự kiến ​​| Mỗi ứng cử viên, bài toán và bài nộp được xử lý một lần, với các thao tác bảng băm dự kiến ​​là O(1) | 
| Không gian | O(C + P) | Từ điển ứng viên, từ điển vấn đề, danh sách ứng viên và mảng câu trả lời theo từng thang đo tuyến tính | 

Đầu vào tối đa chỉ chứa 150.000 bản ghi trên ba phần chính. Thuật toán thực hiện một số lượng thao tác từ điển không đổi cho mỗi lần gửi, do đó thời gian chạy dự kiến ​​của nó dễ dàng tương thích với giới hạn 1 giây so với 2,5 tỷ thao tác mà phương pháp brute-force yêu cầu. Từ điển và mảng lưu trữ thông tin tỷ lệ thuận với tối đa 100.000 ứng viên và các vấn đề được chọn, vừa vặn trong phạm vi 256 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây thực hiện tương tự`solve`cấu trúc hàm và chạy giải pháp dựa trên mẫu được cung cấp cùng với một số trường hợp tùy chỉnh. Trường hợp kích thước tối đa được tạo theo chương trình để bản thân bài kiểm tra vẫn có thể đọc được trong khi vẫn xử lý 50.000 ứng viên, 50.000 vấn đề và 50.000 bài nộp.```python
import sys
import io

def solve():
    input = sys.stdin.readline

    C, P, S = map(int, input().split())

    candidates = []
    candidate_index = {}

    for i in range(C):
        handle = input().strip()
        candidates.append(handle)
        candidate_index[handle] = i

    problem_score = {}

    for _ in range(P):
        problem, score = input().split()
        problem_score[problem] = int(score)

    answer = [0] * C

    for _ in range(S):
        user, problem, verdict = input().split()

        if verdict != "AC":
            continue

        idx = candidate_index.get(user)
        if idx is None:
            continue

        score = problem_score.get(problem)
        if score is None:
            continue

        answer[idx] += score

    output = []
    for i in range(C):
        output.append(f"{candidates[i]} {answer[i]}")

    return "\n".join(output)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

# Provided sample.
sample1 = """\
2 2 4
GabrielPessoa
beza
metebronca 100
geometry 200
beza metebronca AC
ffern numbertheory AC
GabrielPessoa geometry WA
beza geometry AC
"""

assert run(sample1) == """\
GabrielPessoa 0
beza 300
""", "sample 1"

# Minimum-size case.
minimum = """\
1 1 1
a
p 1
a p AC
"""

assert run(minimum) == """\
a 1
""", "minimum-size case"

# All submissions are relevant, with several candidates solving
# the same selected problems.
all_equal = """\
3 2 4
a
b
c
p1 7
p2 7
a p1 AC
b p1 AC
b p2 AC
c p2 AC
"""

assert run(all_equal) == """\
a 7
b 14
c 7
""", "all-equal scores"

# Boundary behavior: WA, unknown user, and unselected problem
# must all be ignored.
filters = """\
2 1 5
alice
bob
selected 100
alice selected WA
alice other AC
unknown selected AC
bob selected AC
bob selected WA
"""

assert run(filters) == """\
alice 0
bob 100
""", "filtering irrelevant submissions"

# A candidate can have several wrong submissions before AC.
# The selected problem score must be added only for AC.
retries = """\
2 2 10
alice
bob
p1 10
p2 20
alice p1 WA
alice p1 CE
alice p1 AC
bob p1 WA
bob p2 AC
alice p2 AC
bob p3 AC
alice p2 WA
bob p1 AC
alice p1 WA
"""

assert run(retries) == """\
alice 30
bob 30
""", "multiple attempts and irrelevant problems"

# Maximum-size generated case.
C = 50000
P = 50000
S = 50000

parts = [f"{C} {P} {S}"]

for i in range(C):
    parts.append(f"u{i}")

for i in range(P):
    parts.append(f"p{i} 1")

# Each submission is a valid AC for a corresponding candidate
# and problem. Every candidate receives exactly one point.
for i in range(S):
    parts.append(f"u{i} p{i} AC")

maximum = "\n".join(parts) + "\n"

expected = "\n".join(f"u{i} 1" for i in range(C))

assert run(maximum) == expected, "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vỏ kích thước tối thiểu |`a 1`| Đầu vào hợp lệ nhỏ nhất và tra cứu thành công trực tiếp | 
| Điểm số bằng nhau |`a 7`,`b 14`,`c 7`| Nhiều thí sinh giải quyết cùng một vấn đề và giá trị điểm lặp lại | 
| Vỏ lọc |`alice 0`,`bob 100`| Người dùng không xác định, sự cố không được chọn và không-`AC`phán quyết | 
| Trường hợp thử nhiều lần |`alice 30`,`bob 30`| Một số lần gửi không thành công trước khi gửi thành công | 
| Trường hợp được tạo kích thước tối đa | Mỗi ứng viên đều có điểm`1`| Giá trị tối đa của cả ba kích thước đầu vào chính và hiệu suất | 

## Vỏ cạnh 

Một`AC`từ người dùng không nằm trong số ứng viên sẽ bị bỏ qua ở giai đoạn tra cứu ứng viên. Ví dụ:```
1 1 1
alice
p1 100
bob p1 AC
```Từ điển ứng viên chỉ chứa`alice`. Khi`bob`được xử lý,`candidate_index.get("bob")`trả lại`None`, do đó không có thay đổi về điểm số. Đầu ra là:```
alice 0
```Việc gửi thành công một vấn đề không được chọn sẽ được xử lý tương tự. Coi như:```
1 1 1
alice
p1 100
alice p2 AC
```Việc tra cứu ứng viên thành công, nhưng`problem_score.get("p2")`trả lại`None`bởi vì chỉ`p1`đã được chọn. Thuật toán loại bỏ việc gửi và in:```
alice 0
```Việc gửi không thành công không được đóng góp bất cứ điều gì, ngay cả khi cả người dùng và vấn đề đều hợp lệ. Với:```
1 1 2
alice
p1 100
alice p1 WA
alice p1 AC
```lần nộp đầu tiên bị từ chối ngay lập tức bởi việc kiểm tra bản án. Cái thứ hai vượt qua tất cả các kiểm tra và cộng 100. Kết quả là:```
alice 100
```Một số lần thử thất bại không gây ra bất kỳ xử lý đặc biệt nào. Vì:```
1 1 3
alice
p1 50
alice p1 WA
alice p1 CE
alice p1 AC
```hai bản ghi đầu tiên để lại số điểm bằng 0, trong khi bản ghi cuối cùng`AC`thay đổi nó thành 50. Đầu ra là:```
alice 50
```Thứ tự nộp bài không nhất thiết phải trùng với thứ tự của ứng viên. Giả sử các ứng viên là:```
2 1 2
alice
bob
p1 25
bob p1 AC
alice p1 AC
```Lần gửi được xử lý đầu tiên cập nhật mục nhập của Bob, là chỉ mục 1. Lần gửi thứ hai cập nhật mục nhập của Alice, là chỉ mục 0. Bởi vì mảng câu trả lời được lập chỉ mục theo các vị trí ứng viên ban đầu nên kết quả cuối cùng vẫn là:```
alice 25
bob 25
```Một bài toán có thể được giải bởi nhiều thí sinh khác nhau và mỗi thí sinh phải nhận điểm độc lập. Ví dụ:```
2 1 2
alice
bob
p1 100
alice p1 AC
bob p1 AC
```Sau lần gửi đầu tiên, mảng câu trả lời là`[100, 0]`. Sau lần thứ hai nó trở thành`[100, 100]`. Thực tế là vấn đề đã được Alice giải quyết không có nghĩa là Bob không có khả năng giải quyết vấn đề đó, bởi vì sự đảm bảo liên quan đến việc gửi đi lặp lại bởi cùng một người dùng chứ không phải do những người dùng khác nhau gửi. 

Tổng điểm tối đa cũng không tạo ra vấn đề tràn số nguyên trong Python. Ngay cả khi một ứng viên giải được nhiều bài toán đã chọn, kiểu số nguyên của Python vẫn tăng lên khi cần thiết. Thuật toán không bao giờ dựa vào biểu diễn số nguyên có chiều rộng cố định.
