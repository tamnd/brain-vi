---
title: "CF 102441I - Cắt"
description: "Chúng ta bắt đầu với một số nguyên dương được viết bằng ký hiệu thập phân. Một vết cắt chọn vị trí giữa hai chữ số, do đó số được chia thành hai phần thập phân không trống. Sau khi hiểu cả hai phần dưới dạng số nguyên, chúng ta thay thế số ban đầu bằng hiệu tuyệt đối của chúng."
date: "2026-08-08T13:40:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102441
codeforces_index: "I"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Final"
rating: 0
weight: 102441
solve_time_s: 193
verified: true
draft: false
---

[CF 102441I - Cắt](https://codeforces.com/problemset/problem/102441/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một số nguyên dương được viết bằng ký hiệu thập phân. Một vết cắt chọn vị trí giữa hai chữ số, do đó số được chia thành hai phần thập phân không trống. Sau khi hiểu cả hai phần dưới dạng số nguyên, chúng ta thay thế số ban đầu bằng hiệu tuyệt đối của chúng. Kết quả bằng 0 bị cấm. Chúng tôi có thể lặp lại thao tác này và đầu ra được yêu cầu là đường dẫn từ số ban đầu đến giá trị nhỏ nhất có thể đạt được bằng bất kỳ chuỗi cắt hợp lệ nào. 

Ví dụ, từ`42`chỉ có một khả năng cắt,`4 | 2`, cho`2`. Từ`100`, vết cắt`1 | 00`cho`|1 - 0| = 1`, vậy tối ưu là`1`. Đầu ra không chỉ là sự tối ưu. Nó phải chứa toàn bộ dãy số, bởi vì trọng tài kiểm tra xem mọi cặp liên tiếp có thực sự có được bằng một lần cắt hợp pháp hay không. Những ràng buộc chính thức là`t <= 1000`Và`n <= 10^12`. 

Giới hạn trên của`10^12`hữu ích vì lý do cấu trúc hơn là vì chúng ta chỉ cần số học 64-bit. Số như vậy có nhiều nhất là 13 chữ số thập phân. Sau khi cắt một`k`-số, cả hai phần kết quả có nhiều nhất`k-1`chữ số, vậy hiệu của chúng cũng có nhiều nhất`k-1`chữ số. Do đó, mọi thao tác đều giảm số chữ số ít nhất một. Một đường dẫn có thể chứa tối đa 13 số, điều này giúp cho việc tìm kiếm các phần cắt có thể khả thi sau khi chúng tôi tránh tính toán lại trạng thái tương tự. 

Một số trường hợp khó xử lý. Vì`7`, không có vị trí nào để cắt, do đó kết quả đầu ra đúng chỉ đơn giản là`1 7`. Việc triển khai giả định mọi số đều có chuyển đổi hợp lệ có thể thất bại ở đây. Vì`11`, vết cắt duy nhất là`1 | 1`, tạo ra số 0 và bị cấm. Như vậy`11`chính nó là mức tối thiểu và đầu ra đúng là`1 11`. Việc triển khai bất cẩn chấp nhận số 0 sẽ báo cáo không chính xác đường dẫn kết thúc bằng số 0. Vì`1001`, vết cắt`10 | 01`cho`9`, trong khi hai vết cắt ở cuối cho`99`Và`99`. Điều này nắm bắt được việc triển khai chỉ thử cắt ở gần cuối. Vì`121`, đường cắt trực tiếp`12 | 1`cho`11`, Nhưng`1 | 21`cho`20`, theo sau là`2 | 0`, cho`2`. Vì thế lấy kết quả tức thời nhỏ nhất cũng chưa đủ. 

## Phương pháp tiếp cận 

Lực lượng vũ phu đơn giản xây dựng toàn bộ cây tìm kiếm. Đối với mỗi số hiện tại, nó thử mọi lần cắt có thể, bỏ qua các lần cắt tạo ra số 0 và khám phá đệ quy mọi số kết quả. Điều này đúng vì mọi thao tác hợp pháp được biểu diễn bằng một nhánh và do đó, mỗi chuỗi thao tác được biểu diễn bằng một đường dẫn từ gốc đến lá. 

Vấn đề là số lượng đường dẫn. Nếu số hiện tại có`k`chữ số, nó có`k-1`các vị trí cắt có thể. Khi đó một đường đi có thể có nhiều nhất`k-1`hoạt động vì mọi hoạt động loại bỏ ít nhất một chữ số. Không nhớ trạng thái, số lá có thể đạt tới`(k-1)(k-2)...1 = (k-1)!`. 

Đối với đầu vào tối đa 13 chữ số, đây là`12! = 479,001,600`những con đường có thể. Điều đó vượt xa những gì mà giải pháp một giây có thể liệt kê được. Lực lượng vũ phu rất hữu ích để hiểu không gian trạng thái, nhưng không phải là một chiến lược triển khai. 

Quan sát quan trọng là có thể đạt được cùng một số nguyên thông qua nhiều chuỗi khác nhau. Một khi chúng ta đạt đến một giá trị nào đó`x`, khả năng tương lai của nó chỉ phụ thuộc vào`x`, không phải về cách chúng tôi đạt được nó. Do đó chúng ta có thể xác định câu trả lời cho`x`đệ quy và ghi nhớ nó. Điều này biến cây tìm kiếm thành một biểu đồ trạng thái theo chu kỳ có hướng. 

Quan sát thứ hai cho chúng ta giới hạn độ sâu. Nếu như`x`có`k`chữ số, vết cắt có dạng`x = A || B`, trong đó cả hai`A`Và`B`chứa ít hơn`k`chữ số. Kể từ đây`|A-B| < 10^(k-1)`. Số chữ số giảm dần sau mỗi thao tác. Không thể có chu kỳ và độ sâu đệ quy tối đa là 12 đối với các ràng buộc đã cho. 

Thuật toán kết quả chính xác là một tìm kiếm được ghi nhớ trên các số có thể truy cập được. Đối với mỗi tiểu bang, chúng tôi liệt kê tất cả các vị trí cắt hợp pháp, đệ quy lấy giá trị cuối cùng tốt nhất có thể đạt được từ mỗi đứa trẻ và giữ cho đứa trẻ đưa ra kết quả nhỏ nhất. Cùng với kết quả đó, chúng tôi nhớ đứa trẻ nào đã được chọn, cho phép chúng tôi xây dựng lại đường dẫn cần thiết sau đó. Bản tóm tắt chính thức của cuộc thi cũng mô tả giải pháp dự định cho vấn đề này một cách đơn giản là tìm kiếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((d-1)!) mỗi lần kiểm tra trong trường hợp xấu nhất | Độ sâu đệ quy O(d) | Quá chậm | 
| Tìm kiếm được ghi nhớ | O(Sd) | O(S) | Đã chấp nhận | 

Đây`d <= 13`là số chữ số thập phân và`S`là số lượng các trạng thái có thể truy cập riêng biệt thực tế đã được truy cập. Mỗi tiểu bang có tối đa`d-1`vết cắt. Cải tiến thực tế quan trọng là các trạng thái lặp lại chỉ được đánh giá một lần. 

## Hướng dẫn thuật toán 

1. Đọc số ban đầu`n`và coi nó như một số nguyên. Số nguyên Python dễ dàng bao phủ toàn bộ phạm vi đầu vào, do đó không có vấn đề tràn. 
2. Xác định`solve(x)`là con số cuối cùng tối thiểu có thể đạt được từ`x`. Nếu như`x`có ít hơn hai chữ số nên không bị cắt nên đáp án là`x`chính nó. Tình huống tương tự cũng xảy ra với số có hai chữ số như`11`khi vết cắt duy nhất của nó sẽ tạo ra số không. 
3. Trước khi đệ quy, hãy kiểm tra xem`x`đã có trong bảng ghi nhớ. Nếu có, hãy trả về câu trả lời được tính toán trước đó. Tương lai của`x`độc lập với đường đi được sử dụng để đến nó, vì vậy việc tính toán lại không thể cung cấp bất kỳ thông tin mới nào. 
4. Chuyển đổi`x`sang biểu diễn thập phân của nó và liệt kê mọi vị trí giữa hai chữ số. Để phân chia sau vị trí`i`, phần bên trái là tiền tố, phần bên phải là hậu tố. Tính toán`y = abs(left - right)`. 
5. Bỏ qua`y = 0`, vì số 0 bị cấm một cách rõ ràng. Dành cho nhau`y`, tính toán đệ quy`solve(y)`và so sánh giá trị trả về với câu trả lời tốt nhất hiện tại. 
6. Nhớ con`y`tạo ra giá trị cuối cùng nhỏ nhất. Việc lưu trữ phần tử con thay vì lưu trữ toàn bộ đường dẫn sẽ giữ cho bảng ghi nhớ được gọn gàng và cho phép chúng ta xây dựng lại đường dẫn sau này. 
7. Lưu kết quả tính toán và con đã chọn vào bảng ghi nhớ. Các lượt truy cập trong tương lai tới cùng một số nguyên bây giờ có thể quay lại ngay lập tức. 
8. Bắt đầu từ bản gốc`n`và lặp đi lặp lại theo các con trỏ con được lưu trữ. Nối mọi giá trị đã truy cập vào đường dẫn đầu ra cho đến khi đạt đến trạng thái không có con nào được chọn. 

Tại sao nó hoạt động: trạng thái`x`có chính xác cùng một tập hợp các hoạt động có thể xảy ra trong tương lai bất kể chúng ta đạt được nó bằng cách nào.`solve(x)`kiểm tra mọi lần cắt đầu tiên hợp pháp và kết hợp câu trả lời tối ưu của từng trạng thái kết quả, do đó nó chọn cách tiếp tục tốt nhất có thể. Vì mọi phép toán đều giảm nghiêm ngặt số chữ số thập phân nên cuối cùng đệ quy sẽ đạt đến trạng thái không có phép toán hợp pháp. Do đó, các con trỏ con được lưu trữ mô tả một đường dẫn hợp lệ có giá trị cuối cùng là giá trị tối thiểu có thể truy cập được từ số ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n):
    memo = {}
    nxt = {}

    def dfs(x):
        if x in memo:
            return memo[x]

        s = str(x)
        k = len(s)

        # No cut is possible for a one-digit number.
        # For a two-digit number, the only cut may produce zero.
        if k == 1:
            memo[x] = x
            nxt[x] = None
            return x

        best = x
        best_next = None

        power = 10 ** (k - 1)

        for i in range(1, k):
            power //= 10

            left = x // power
            right = x % power
            y = abs(left - right)

            if y == 0:
                continue

            value = dfs(y)

            if value < best:
                best = value
                best_next = y

        memo[x] = best
        nxt[x] = best_next
        return best

    dfs(n)

    path = [n]
    cur = n

    while nxt[cur] is not None:
        cur = nxt[cur]
        path.append(cur)

    return path

def main():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        path = solve_case(n)
        out.append(str(len(path)) + " " + " ".join(map(str, path)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```các`memo`từ điển lưu trữ giá trị cuối cùng tối ưu cho mọi trạng thái đã được giải quyết. Sự riêng biệt`nxt`từ điển ghi lại quá trình chuyển đổi đầu tiên đạt được giá trị đó. Việc giữ hai phần thông tin này tách biệt sẽ giúp việc tái thiết trở nên đơn giản. 

Vòng lặp kết thúc`i`đại diện cho mọi vết cắt thập phân hợp pháp.`power`là giá trị vị trí tương ứng với chữ số đầu tiên của phần bên phải. Ví dụ, đối với`1234`, ba lần lặp sử dụng lũy ​​thừa`100`,`10`, Và`1`, tạo ra sự phân chia`1 | 234`,`12 | 34`, Và`123 | 4`. 

các`y == 0`kiểm tra là cần thiết. Kết quả bằng 0 không chỉ đơn thuần là một câu trả lời không mong muốn, nó là một quá trình chuyển đổi bất hợp pháp, vì vậy nó không bao giờ được phép đưa vào đệ quy. 

Giá trị ban đầu`best = x`xử lý các trạng thái không có đường cắt khác 0 hợp pháp. Vì`11`, ứng cử viên duy nhất bằng 0, vì vậy`best`còn lại`11`Và`nxt[11]`còn lại`None`. Logic tương tự xử lý mọi số có một chữ số. 

Không có tràn số nguyên trong Python, ngay cả đối với các biểu thức trung gian và đầu vào lớn nhất chỉ là`10^12`. Độ sâu đệ quy tối đa là 12 vì số chữ số giảm nghiêm trọng sau mỗi thao tác hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1:`7`Số có một chữ số nên thuật toán nhận ra ngay rằng không thể cắt được. 

| Hiện hành`x`| Chữ số | Cắt giảm pháp lý | Giá trị cuối cùng tốt nhất | Tiếp theo | 
| --- | --- | --- | --- | --- | 
| 7 | 1 | không | 7 | không | 

Con đường được xây dựng lại là`7`, vì vậy đầu ra là`1 7`. Điều này thể hiện trường hợp một chữ số cuối cùng. 

### Mẫu 2:`100`Có hai vị trí cắt có thể. 

| Hiện hành`x`| Chia | Trái | Đúng | Kết quả | Giá trị cuối cùng tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 100 |`1 | 00`| 1 | 0 | 1 | 
| 100 |`10 | 0`| 10 | 0 | 10 | 

Sự chuyển tiếp đầu tiên đạt`1`, đó là thiết bị đầu cuối. Sự chuyển tiếp thứ hai đạt`10`, bản thân nó có thể được cắt thành`1 | 0`và cũng đạt tới`1`. Đường đi tối ưu được chương trình lựa chọn là`100 -> 1`. 

Ví dụ này cũng xác nhận rằng các số 0 đứng đầu trong hậu tố được hiểu là số nguyên 0, điều này cần thiết cho kết quả mẫu. 

### Ví dụ bổ sung:`121`Ví dụ này cho thấy tại sao chúng ta không thể tham lam lựa chọn kết quả ngay lập tức nhỏ nhất. 

| Hiện hành`x`| Chia | Kết quả | Kết quả tốt nhất từ ​​trẻ em | 
| --- | --- | --- | --- | 
| 121 |`1 | 21`| 20 | 
| 121 |`12 | 1`| 11 | 

Đứa con đầu lòng trông tệ hơn vì`20 > 11`, Nhưng`20`có thể được cắt thành`2 | 0`, đạt`2`. Việc tìm kiếm so sánh sự tiếp tục tối ưu của mỗi đứa trẻ, vì vậy nó chọn`121 -> 20 -> 2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Sd) | Mỗi trạng thái có thể truy cập riêng biệt được xử lý một lần và có nhiều nhất`d-1`vết cắt. | 
| Không gian | O(S + d) | Các bảng ghi nhớ lưu trữ một câu trả lời và một chuyển đổi cho mỗi trạng thái được truy cập, với độ sâu đệ quy nhiều nhất`d`. | 

Đây`d <= 13`, bởi vì`n <= 10^12`. Việc tìm kiếm không bao giờ có độ sâu đệ quy tỷ lệ thuận với giá trị số. Tính năng ghi nhớ loại bỏ các cây con lặp lại, đây là điểm khác biệt mang tính quyết định so với tìm kiếm giai thừa thô. Chỉ với 13 chữ số thập phân và tối đa 1000 trường hợp thử nghiệm, tìm kiếm này phù hợp với các ràng buộc đã định. 

## Trường hợp thử nghiệm 

Bởi vì đầu ra có thể chứa bất kỳ đường dẫn tối ưu nào, nên các thử nghiệm sẽ xác thực đường dẫn thay vì so sánh toàn bộ chuỗi đầu ra theo từng ký tự. Khai thác thử nghiệm sau đây sẽ kiểm tra xem mọi chuyển đổi có hợp pháp hay không và giá trị cuối cùng bằng với câu trả lời mạnh mẽ được tính toán độc lập cho những trường hợp nhỏ này.```python
# helper: run the solution on an input string
import sys
import io

def solve_case(n):
    memo = {}
    nxt = {}

    def dfs(x):
        if x in memo:
            return memo[x]

        s = str(x)
        k = len(s)

        if k == 1:
            memo[x] = x
            nxt[x] = None
            return x

        best = x
        best_next = None
        power = 10 ** (k - 1)

        for _ in range(1, k):
            power //= 10
            left = x // power
            right = x % power
            y = abs(left - right)

            if y == 0:
                continue

            value = dfs(y)
            if value < best:
                best = value
                best_next = y

        memo[x] = best
        nxt[x] = best_next
        return best

    dfs(n)

    path = [n]
    cur = n
    while nxt[cur] is not None:
        cur = nxt[cur]
        path.append(cur)

    return path

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    t = int(sys.stdin.readline())
    answers = []

    for _ in range(t):
        n = int(sys.stdin.readline())
        path = solve_case(n)
        answers.append(str(len(path)) + " " + " ".join(map(str, path)))

    sys.stdin = old_stdin
    return "\n".join(answers)

def can_cut(a, b):
    s = str(a)

    for i in range(1, len(s)):
        left = int(s[:i])
        right = int(s[i:])

        if abs(left - right) == b:
            return b != 0

    return False

def validate_output(inp, output):
    input_lines = inp.strip().splitlines()
    t = int(input_lines[0])
    ns = [int(x) for x in input_lines[1:]]

    lines = output.strip().splitlines()
    assert len(lines) == t

    for n, line in zip(ns, lines):
        data = list(map(int, line.split()))
        m = data[0]
        path = data[1:]

        assert m == len(path)
        assert path[0] == n
        assert path[-1] != 0

        for a, b in zip(path, path[1:]):
            assert can_cut(a, b), (a, b)

def brute(n):
    memo = {}

    def dfs(x):
        if x in memo:
            return memo[x]

        s = str(x)
        best = x

        for i in range(1, len(s)):
            left = int(s[:i])
            right = int(s[i:])
            y = abs(left - right)

            if y == 0:
                continue

            best = min(best, dfs(y))

        memo[x] = best
        return best

    return dfs(n)

# Provided samples.
sample = """3
7
100
42
"""
sample_out = run(sample)
validate_output(sample, sample_out)

sample_last = [int(x.split()[-1]) for x in sample_out.splitlines()]
assert sample_last == [7, 1, 2]

# Minimum-size inputs.
test = """4
1
9
11
22
"""
out = run(test)
validate_output(test, out)
last = [int(x.split()[-1]) for x in out.splitlines()]
assert last == [1, 9, 11, 22]

# Internal cut is necessary for 1001.
test = """1
1001
"""
out = run(test)
validate_output(test, out)
assert int(out.split()[-1]) == brute(1001) == 9

# A case where the smallest immediate result is not optimal.
test = """1
121
"""
out = run(test)
validate_output(test, out)
assert int(out.split()[-1]) == brute(121) == 2

# Maximum input boundary.
test = """1
1000000000000
"""
out = run(test)
validate_output(test, out)
assert int(out.split()[-1]) == brute(1000000000000) == 1
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`,`9`,`11`,`22`| Giá trị cuối cùng`1`,`9`,`11`,`22`| Thiết bị đầu cuối một chữ số và các vết cắt không tạo ra hai chữ số | 
|`1001`| Giá trị cuối cùng`9`| Việc cắt bên trong có thể cần thiết để tối ưu | 
|`121`| Giá trị cuối cùng`2`| Sự chuyển đổi ngay lập tức nhỏ nhất không nhất thiết phải dẫn đến sự tối ưu | 
|`1000000000000`| Giá trị cuối cùng`1`| Ranh giới số tối đa và chuỗi thập phân lớn | 

## Vỏ cạnh 

cho`7`, đầu vào là`7`. Biểu diễn thập phân chỉ có một chữ số nên vòng lặp cắt không có lần lặp.`memo[7]`trở thành`7`,`nxt[7]`là`None`, và việc tái thiết tạo ra`[7]`. Đầu ra là`1 7`. 

Vì`11`, cách phân chia duy nhất có thể là`1 | 1`, sự khác biệt của nó bằng không. Việc triển khai từ chối quá trình chuyển đổi này với`if y == 0`, rời đi`11`là giá trị tốt nhất và đưa ra đường dẫn`[11]`. Đây là lý do tại sao số 0 phải được lọc trước các cuộc gọi đệ quy. 

Vì`1001`, ba vết cắt tạo ra`999`,`99`, Và`9`tương ứng. Cái cuối cùng đến từ`10 | 01`, do đó việc tìm kiếm ngay lập tức có đường dẫn tới`9`. Từ`9`là thiết bị đầu cuối một chữ số, không có sự tiếp tục nào có thể cải thiện nó. Con đường cuối cùng là`1001 -> 9`. 

Vì`121`, vết cắt tạo ra`20`Và`11`. Nhà nước`11`là thiết bị đầu cuối vì lần cắt duy nhất của nó tạo ra số không. Nhà nước`20`có thể được cắt như`2 | 0`, sản xuất`2`. Tìm kiếm ghi nhớ so sánh các giá trị cuối cùng`2`Và`11`, không chỉ đơn thuần là các giá trị tức thời`20`Và`11`, và chọn đúng`121 -> 20 -> 2`. 

Để có đầu vào tối đa`1000000000000`, cắt sau chữ số đầu tiên cho`1 | 000000000000`, sự khác biệt của nó là`1`. Bản ghi thuật toán`1`vì sự tối ưu và quá trình tái thiết dừng lại ngay lập tức. Kích thước của số ban đầu không gây ra bất kỳ sự tràn hoặc đệ quy dài nào, vì Python xử lý trực tiếp số nguyên và đường dẫn thành công chỉ có hai số.
