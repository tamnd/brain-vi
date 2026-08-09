---
title: "CF 102440F - Giải vô địch bóng đá"
description: "Chúng ta có những người bạn được đánh số từ (1) đến (n), và người bạn (i) đóng góp chính xác (i) sức mạnh cho đội nào trong ba đội nhận được họ. Mỗi người bạn phải thuộc đúng một đội và tổng số tiền của ba đội phải bằng nhau."
date: "2026-08-09T01:01:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102440
codeforces_index: "F"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Junior"
rating: 0
weight: 102440
solve_time_s: 593
verified: false
draft: false
---

[CF 102440F - Giải vô địch bóng đá](https://codeforces.com/problemset/problem/102440/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 53 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có những người bạn được đánh số từ (1) đến (n), và người bạn (i) đóng góp chính xác (i) sức mạnh cho đội nào trong ba đội nhận được họ. Mỗi người bạn phải thuộc đúng một đội và tổng số tiền của ba đội phải bằng nhau. Nhiệm vụ là in một phân vùng như vậy hoặc báo cáo rằng không có phân vùng nào tồn tại. 

Tổng sức mạnh là 

[ 
S = 1+2+\dots+n = \frac{n(n+1)}2. 
] 

Nếu có phân vùng hợp lệ thì mỗi đội phải có sức mạnh 

[ 
T = \frac{S}{3} = \frac{n(n+1)}6. 
] 

Vì vậy khả năng chia hết của (n(n+1)) cho (6) là điều kiện cần đầu tiên. Tuy nhiên, nó không đủ cho (n) nhỏ. Ví dụ: (n=2) cho tổng sức mạnh (3), nên mục tiêu là (1), nhưng chỉ người bạn (1) mới có thể tạo thành một đội có sức mạnh (1). Tương tự, (n=3) có mục tiêu (2), nhưng chỉ có người bạn duy nhất được đánh số (2) mới có sức mạnh đó. 

Ràng buộc (n\le 10^6) làm cho tính chất mang tính xây dựng của vấn đề trở nên mang tính quyết định. Chúng tôi có thể đủ khả năng vượt qua tất cả (n) người chơi, nhưng bất kỳ số mũ hoặc bậc hai nào đều hoàn toàn nằm ngoài phạm vi. Phiên bản chính thức của cuộc thi đưa ra giới hạn cho vấn đề này là một giây và 256 MB bộ nhớ, vì vậy giải pháp dự định phải gần với tuyến tính trong (n). 

Có một số trường hợp ranh giới nhỏ mà giải pháp chỉ dựa trên khả năng chia hết có thể xử lý sai. Với (n=1), tổng sức mạnh là (1), nên không thể có ba đội có sức mạnh tích cực. Với (n=2), tổng điểm chia hết cho 3 nhưng mục tiêu là (1) và chỉ có một người chơi có sức mạnh (1) nên đáp án vẫn là`Impossible`. Với (n=3), mục tiêu là (2) và một lần nữa chỉ có một tập hợp con có tổng đó, cụ thể là ({2}). Đầu vào (n=4) có tổng (10), không chia hết cho 3 nên điều này ngay lập tức không thể thực hiện được. Giá trị đầu tiên có thể là (n=5), trong đó các đội có thể là ({5}), ({4,1}) và ({3,2}), tất cả đều có sức mạnh (5). 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể chỉ định mọi người chơi một cách độc lập vào một trong ba đội. Có (3^n) bài tập. Sau khi chọn một bài tập, chúng ta sẽ tính ba tổng và kiểm tra xem chúng có bằng nhau hay không, lấy (O(n)) công cho mỗi bài tập. Do đó, số lượng hoạt động trong trường hợp xấu nhất là theo thứ tự (n3^n). Ngay cả đối với (n=30), con số này đã quá lớn, trong khi giới hạn thực tế đạt tới (10^6). 

Cách tiếp cận tìm kiếm tập hợp con chỉ tốt hơn một chút. Chúng ta có thể chọn một đội làm tập hợp con với số tiền cần thiết và sau đó cố gắng phân chia những người chơi còn lại, nhưng việc liệt kê các tập hợp con đã tốn (2^n). Thực tế là sức mạnh của người chơi chính xác là các số nguyên liên tiếp (1,2,\dots,n) cho chúng ta một cấu trúc mạnh hơn nhiều so với một thể hiện tổng tập hợp con chung. 

Quan sát quan trọng là nhìn vào sáu số liên tiếp. Hãy xem xét khối 

[ 
L,L+1,L+2,L+3,L+4,L+5. 
] 

Nó luôn có thể được chia thành ba cặp 

[ 
{L+5,L},\qquad 
{L+4,L+1},\qquad 
{L+3,L+2}. 
] 

Mỗi cặp có tổng 

[ 
2L+5. 
] 

Vì vậy, mỗi khối gồm sáu người chơi liên tiếp có thể được thêm vào một giải pháp đã hợp lệ mà không làm thay đổi sự bình đẳng giữa sức mạnh của ba đội. Điều này có nghĩa là chúng ta chỉ cần xây dựng một vài cấu hình ban đầu nhỏ. Sau đó, cứ sáu người chơi bổ sung sẽ được xử lý giống hệt nhau. 

Các cấu hình cơ sở hữu ích đặc biệt nhỏ: 

[ 
\bắt đầu{căn chỉnh} 
n=5 &: {5},{4,1},{3,2},\ 
n=6 &: {6,1},{5,2},{4,3},\ 
n=8 &: {8,4},{7,5},{6,3,2,1},\ 
n=9 &: {9,6},{8,7},{5,4,3,2,1}. 
\end{căn chỉnh} 
] 

Tổng của đội họ lần lượt là (5), (7), (12) và (15). 

Bốn cơ sở này bao gồm mọi loại dư lượng có thể dẫn đến dung dịch. Nếu (n\equiv 0\pmod 6), bắt đầu từ (6). Nếu (n\equiv2\pmod6), bắt đầu từ (8). Nếu (n\equiv3\pmod6), bắt đầu từ (9). Nếu (n\equiv5\pmod6), bắt đầu từ (5). Các giá trị còn lại của (n) có thặng dư (1) hoặc (4) nên tổng cường độ của chúng không chia hết cho ba. 

Có hai giá trị nhỏ đặc biệt, (n=2) và (n=3), trong đó khả năng chia hết được giữ nguyên nhưng không tồn tại phân vùng. Như vậy điều kiện đầy đủ là (n\ge5) và 

[ 
n\bmod6\in{0,2,3,5}. 
] 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n3^n)) | (O(n)) | Quá chậm | 
| Xây dựng tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán (n\bmod6). Nếu (n<5), các ứng cử viên duy nhất cần được chú ý đặc biệt là (1,2,3,4) và tất cả chúng đều không thể. Nếu dư lượng là (1) hoặc (4) thì tổng cường độ không chia hết cho ba nên in`Impossible`. 
2. Chọn một đế nhỏ theo cặn. Đối với phần dư (5), sử dụng phân vùng của (1,\dots,5) thành ({5}), ({4,1}) và ({3,2}). Đối với phần dư (0), sử dụng (1,\dots,6) với ({6,1}), ({5,2}) và ({4,3}). Đối với phần dư (2), sử dụng (1,\dots,8) với ({8,4}), ({7,5}) và ({6,3,2,1}). Đối với phần dư (3), sử dụng (1,\dots,9) với ({9,6}), ({8,7}) và ({5,4,3,2,1}). 
3. Hãy để`left`là số đầu tiên không có trong cơ số. Xử lý những người chơi còn lại theo khối sáu. Đối với khối bắt đầu tại (L), thêm (L+5) và (L) vào đội thứ nhất, (L+4) và (L+1) vào đội thứ hai, và (L+3) và (L+2) vào đội thứ ba. Mỗi đội nhận được chính xác (2L+5) sức mạnh bổ sung. 
4. Tăng (L) thêm sáu và tiếp tục cho đến khi tất cả người chơi đã được chỉ định. Bởi vì kích thước cơ sở có cùng số dư theo modulo 6 như (n), nên số người chơi còn lại luôn là bội số của 6. 
5. In`Possible`và ba đội được xây dựng. Tổng của chúng bằng nhau vì tổng cơ bản bằng nhau và mỗi khối sáu người chơi được thêm vào sẽ đóng góp số tiền như nhau cho mỗi đội. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý cơ sở và bất kỳ khối sáu người chơi hoàn chỉnh nào, cả ba đội đều có tổng sức mạnh như nhau và mỗi người chơi được xử lý đều thuộc về đúng một đội. Cơ sở thỏa mãn trực tiếp bất biến này. Đối với mỗi khối sau, ba cặp được thêm vào có cùng tổng (2L+5), do đó, việc thêm khối sẽ duy trì sự bằng nhau. Vì cơ sở cộng với tất cả các khối được xử lý cuối cùng chứa mọi người chơi từ (1) đến (n), các đội cuối cùng tạo thành một phân vùng hợp lệ hoàn chỉnh. 

Vì không thể, khi (n\equiv1) hoặc (4\pmod6), (n(n+1)/2) không chia hết cho ba, do đó tổng các đội bằng nhau không thể tồn tại. Các giá trị còn lại dưới 5 được kiểm tra riêng và không có giá trị nào thừa nhận ba đội có sức mạnh tích cực ngang nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    if n < 5 or n % 6 in (1, 4):
        print("Impossible")
        return

    r = n % 6

    if r == 5:
        teams = [
            [5],
            [4, 1],
            [3, 2],
        ]
        start = 6

    elif r == 0:
        teams = [
            [6, 1],
            [5, 2],
            [4, 3],
        ]
        start = 7

    elif r == 2:
        teams = [
            [8, 4],
            [7, 5],
            [6, 3, 2, 1],
        ]
        start = 9

    else:  # r == 3
        teams = [
            [9, 6],
            [8, 7],
            [5, 4, 3, 2, 1],
        ]
        start = 10

    left = start

    while left <= n:
        teams[0].extend([left + 5, left])
        teams[1].extend([left + 4, left + 1])
        teams[2].extend([left + 3, left + 2])
        left += 6

    out = ["Possible"]

    for team in teams:
        out.append(str(len(team)))
        out.append(" ".join(map(str, team)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Điều kiện đầu tiên xử lý cả lỗi chia hết và các giá trị ngoại lệ nhỏ. Kiểm tra`n < 5`trước khi phần dư thuận tiện vì (n=2) và (n=3) nếu không sẽ vượt qua phép thử chia hết. 

Bốn trường hợp cơ bản được mã hóa cứng vì chúng là phần thực sự đặc biệt duy nhất của cấu trúc. Kích thước của chúng là (5,6,8,9) và mỗi dư lượng tương ứng đảm bảo rằng`n - start + 1`chia hết cho sáu. 

Vòng lặp xử lý chính xác sáu người chơi mới cùng một lúc. Đội đầu tiên nhận được`left + 5`Và`left`, người thứ hai nhận được`left + 4`Và`left + 1`, và người thứ ba nhận được`left + 3`Và`left + 2`. Tổng của cả ba cặp đều có cùng giá trị nên không cần tính toán mục tiêu đang chạy. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn khi tính tổng theo khái niệm. Việc thực hiện thậm chí không cần tính toán tổng cường độ, điều này cũng giúp việc xây dựng trở nên đơn giản. 

Đầu ra lưu trữ tất cả số người chơi trong ba danh sách. Vì mỗi người chơi xuất hiện đúng một lần và (n\le10^6), nên tổng lượng dữ liệu được lưu trữ là (O(n)). Việc nối mỗi danh sách một lần sẽ tránh được chi phí in từng số riêng biệt. 

## Ví dụ đã hoạt động 

### Mẫu 1: (n=6) 

Phần dư là (0), do đó thuật toán sử dụng cơ sở sáu người chơi. 

| Bước | Đội 1 | Đội 2 | Đội 3 | 
| --- | --- | --- | --- | 
| Căn cứ | (6,1) | (5,2) | (4,3) | 
| Tổng hợp | (7) | (7) | (7) | 
| Người chơi còn lại | không | không | không | 

Ba tổng đều là (7) và mọi người chơi từ (1) đến (6) xuất hiện đúng một lần. Kết quả đầu ra có cấu trúc giống như mẫu đầu tiên. 

### Mẫu 2: (n=9) 

Phần dư là (3), do đó cơ sở chín người chơi được sử dụng trực tiếp. 

| Bước | Đội 1 | Đội 2 | Đội 3 | 
| --- | --- | --- | --- | 
| Căn cứ | (9,6) | (8,7) | (5,4,3,2,1) | 
| Tổng hợp | (15) | (15) | (15) | 
| Người chơi còn lại | không | không | không | 

Một lần nữa, mỗi người chơi được sử dụng một lần và cả ba số tiền đều bằng nhau (15). Đây chính xác là cấu trúc được thể hiện trong mẫu thứ hai. 

Hai mẫu này cũng chứng minh tại sao lời giải không chỉ dựa vào việc kiểm tra xem tổng có chia hết cho ba hay không. Việc xây dựng cần một phân vùng bắt đầu nhỏ hợp lệ, sau đó khối bất biến sáu người chơi sẽ tiếp quản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi người chơi được đưa vào đúng một đội và sau đó được viết một lần. | 
| Không gian | (O(n)) | Ba danh sách đầu ra cùng nhau chứa chính xác (n) số người chơi. | 

Với (n\le10^6), một lần tuyến tính duy nhất là phù hợp. Việc xây dựng chỉ thực hiện một lượng công việc không đổi cho mỗi nhóm sáu người chơi và bản thân đầu ra đã chứa (n) số, do đó (O(n)) thời gian là tối ưu tiệm cận cho một vấn đề yêu cầu in toàn bộ phân vùng. 

## Trường hợp thử nghiệm 

Trình trợ giúp kiểm tra bên dưới sử dụng cấu trúc tương tự như giải pháp đã gửi. Đối với các mẫu chính thức, kết quả đầu ra mang tính xác định và có thể so sánh chính xác. Đối với các trường hợp tùy chỉnh có thể xảy ra, bài kiểm tra sẽ kiểm tra các thuộc tính toán học thay vì yêu cầu một phân vùng hợp lệ cụ thể, vì bài toán chấp nhận bất kỳ cách xây dựng chính xác nào.```python
import io
import sys

def build(n: int) -> str:
    if n < 5 or n % 6 in (1, 4):
        return "Impossible\n"

    r = n % 6

    if r == 5:
        teams = [[5], [4, 1], [3, 2]]
        start = 6
    elif r == 0:
        teams = [[6, 1], [5, 2], [4, 3]]
        start = 7
    elif r == 2:
        teams = [[8, 4], [7, 5], [6, 3, 2, 1]]
        start = 9
    else:
        teams = [[9, 6], [8, 7], [5, 4, 3, 2, 1]]
        start = 10

    left = start
    while left <= n:
        teams[0].extend([left + 5, left])
        teams[1].extend([left + 4, left + 1])
        teams[2].extend([left + 3, left + 2])
        left += 6

    out = ["Possible"]
    for team in teams:
        out.append(str(len(team)))
        out.append(" ".join(map(str, team)))

    return "\n".join(out) + "\n"

def run(inp: str) -> str:
    n = int(inp.strip())
    return build(n)

def validate(inp: str):
    output = run(inp)
    lines = output.strip().splitlines()
    n = int(inp.strip())

    if n < 5 or n % 6 in (1, 4):
        assert lines == ["Impossible"], f"expected impossible for {n}"
        return

    assert lines[0] == "Possible"

    teams = []
    pos = 1

    for _ in range(3):
        k = int(lines[pos])
        pos += 1
        team = list(map(int, lines[pos].split()))
        pos += 1

        assert len(team) == k
        teams.append(team)

    flat = [x for team in teams for x in team]

    assert len(flat) == n
    assert sorted(flat) == list(range(1, n + 1))

    sums = [sum(team) for team in teams]
    assert sums[0] == sums[1] == sums[2]

# Provided samples
assert run("6") == (
    "Possible\n"
    "2\n"
    "6 1\n"
    "2\n"
    "5 2\n"
    "2\n"
    "4 3\n"
), "sample 1"

assert run("9") == (
    "Possible\n"
    "2\n"
    "9 6\n"
    "2\n"
    "8 7\n"
    "5\n"
    "5 4 3 2 1\n"
), "sample 2"

assert run("10") == "Impossible\n", "sample 3"

# Custom cases
validate("1")
validate("2")
validate("3")
validate("4")
validate("5")
validate("8")
validate("11")
validate("999999")
validate("1000000")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`Impossible`| Đầu vào tối thiểu và tổng không chia hết | 
|`2`|`Impossible`| Tổng số chia hết nhưng không thể phân vùng | 
|`3`|`Impossible`| Ngoại lệ chia nhỏ thứ hai | 
|`4`|`Impossible`| Ranh giới trước trường hợp hợp lệ đầu tiên | 
|`5`|`Possible`| Công trình hợp lệ nhỏ nhất | 
|`8`|`Possible`| Thùng cặn đặc biệt (2) | 
|`11`|`Possible`| Cơ sở năm người chơi theo sau là một khối sáu người chơi | 
|`999999`|`Possible`| Đầu vào hợp lệ lớn có dư lượng (3) | 
|`1000000`|`Impossible`| Ranh giới đầu vào tối đa có dư lượng (4) | 

## Vỏ cạnh 

Với (n=1), thuật toán sẽ in ngay`Impossible`bởi vì tổng của ba đội không âm không thể bằng nhau (1/3). Không cần phải xây dựng bất cứ điều gì. 

Với (n=2), tổng cường độ là (3), do đó cường độ cần thiết là (1). Đội duy nhất có thể có sức mạnh (1) là ({1}), không còn cách nào để tạo thêm hai đội có sức mạnh (1). các`n < 5`điều kiện nắm bắt được điều này trước khi việc kiểm tra dư lượng có thể chấp nhận nó một cách không chính xác. 

Với (n=3), tổng số là (6), cho ra sức mạnh mục tiêu (2). Tập con duy nhất có tổng (2) là ({2}), nên không thể thành lập được ba đội như vậy. Điều kiện giá trị nhỏ tương tự xử lý trường hợp này. 

Với (n=4), tổng số là (10), không chia hết cho ba. Số dư là (4) nên thuật toán in ra`Impossible`mà không cố gắng xây dựng. 

Với (n=5), cơ số đặc biệt là 

[ 
{5},\quad{4,1},\quad{3,2}. 
] 

Mỗi đội đều có sức mạnh (5). Đây là đầu vào hợp lệ nhỏ nhất có thể và cũng giải thích tại sao phần dư (5) phải được đưa vào danh sách các trường hợp hợp lệ. 

Với (n=8), cơ số là 

[ 
{8,4},\quad{7,5},\quad{6,3,2,1}. 
] 

Mỗi đội có sức mạnh (12). Trường hợp này phát hiện một triển khai chỉ biết cách xây dựng bội số của sáu. 

Đối với (n=11), thuật toán bắt đầu với việc xây dựng năm người chơi và xử lý khối (6,\dots,11). Khối đó trở thành 

[ 
{11,6},\quad{10,7},\quad{9,8}, 
] 

và mỗi cặp có tổng (17). Các đội ban đầu mỗi đội có tổng (5), nên cả ba đội cuối cùng đều có tổng (22). 

Với (n=10^6) thì số dư là (4) nên đáp án là ngay`Impossible`. Điều này rất hữu ích vì đầu vào tối đa không phải lúc nào cũng có nghĩa là đầu ra lớn. Thuật toán từ chối nó trong thời gian không đổi thay vì phân bổ cấu trúc triệu phần tử một cách không cần thiết.
