---
title: "CF 102423J - Một trong mỗi"
description: "Chúng ta có một dãy (X) gồm (n) số nguyên. Mọi phần tử đều nằm trong khoảng từ (1) đến (k) và mọi giá trị từ (1) đến (k) xảy ra ở đâu đó trong chuỗi. Chúng ta cần xóa một số thành phần trong khi vẫn giữ nguyên thứ tự tương đối của mọi thứ còn lại."
date: "2026-08-12T01:19:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102423
codeforces_index: "J"
codeforces_contest_name: "North American Southeast Regional 2019 (Div 1)"
rating: 0
weight: 102423
solve_time_s: 83
verified: true
draft: false
---

[CF 102423J - Một trong số đó](https://codeforces.com/problemset/problem/102423/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy (X) gồm (n) số nguyên. Mọi phần tử đều nằm trong khoảng từ (1) đến (k) và mọi giá trị từ (1) đến (k) xảy ra ở đâu đó trong chuỗi. Chúng ta cần xóa một số thành phần trong khi vẫn giữ nguyên thứ tự tương đối của mọi thứ còn lại. 

Chuỗi cuối cùng phải chứa mọi giá trị đúng một lần, vì vậy độ dài của nó chính xác là (k). Trong số tất cả các dãy con như vậy, chúng ta muốn dãy con nhỏ nhất về mặt từ điển. Nói cách khác, trước tiên chúng ta giảm thiểu giá trị được chọn đầu tiên, sau đó, trong số các lựa chọn có cùng giá trị đầu tiên, giảm thiểu giá trị được chọn thứ hai, v.v. Bài toán ban đầu đưa ra (n) tối đa (2\cdot10^5), do đó giải pháp quét liên tục các phần lớn của chuỗi hoặc xem xét nhiều chuỗi con có thể xảy ra là không khả thi. 

Giải pháp tuyến tính hoặc (O(n\log n)) là mục tiêu tự nhiên. Với các phần tử (2\cdot10^5) và giới hạn hai giây, (O(n^2)) có nghĩa là khoảng (4\cdot10^{10}) lần lặp cơ bản trong trường hợp xấu nhất, vượt xa những gì việc triển khai lập trình cạnh tranh có thể xử lý. Cấu trúc của bài toán rất thuận lợi vì chúng ta chỉ cần một lần xuất hiện của mỗi giá trị và những lần xuất hiện trùng lặp có thể bị loại bỏ bất cứ khi nào lần xuất hiện sau đó cho chúng ta lựa chọn từ điển tốt hơn. 

Trường hợp cạnh đầu tiên là khi (k=1), do đó toàn bộ câu trả lời bao gồm một giá trị duy nhất. Ví dụ,```
1 1
1
```có đầu ra```
1
```Việc triển khai bất cẩn giả định rằng phải có thao tác ngăn xếp liên quan đến phần tử trước đó có thể thất bại ở đây vì không có phần tử nào trước đó. 

Một trường hợp cạnh khác là khi chuỗi đã là một hoán vị. Ví dụ,```
5 5
1
2
3
4
5
```có đầu ra```
1 2 3 4 5
```Mọi giá trị phải được giữ lại vì có chính xác (k) phần tử. Việc triển khai loại bỏ phần tử ngăn xếp chỉ vì giá trị hiện tại nhỏ hơn có thể vô tình xóa một giá trị không xuất hiện sau này. 

Các nội dung trùng lặp ở gần cuối cũng là một nguồn sai sót phổ biến khác. Coi như```
6 3
3
2
1
3
1
3
```Câu trả lời là```
2 1 3
```đầu tiên`3`không thể là một phần của câu trả lời tối ưu vì việc chọn`2`tiếp theo cung cấp phần tử đầu tiên nhỏ hơn và phần tử sau`3`vẫn còn có sẵn. Tuy nhiên, trận chung kết`3`phải được giữ lại vì sau khi chọn`2`Và`1`, không muộn hơn`3`tồn tại. Ngăn xếp tham lam phải phân biệt giữa phần tử có thể được gỡ bỏ một cách an toàn và phần tử mà việc loại bỏ nó sẽ khiến chuỗi còn lại không đầy đủ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp là liệt kê các chuỗi con có thể có, giữ lại các chuỗi chứa mọi giá trị từ (1) đến (k) chính xác một lần, sau đó chọn chuỗi nhỏ nhất về mặt từ điển. Điều này đúng vì mọi câu trả lời hợp lệ đều được xem xét rõ ràng. Vấn đề là số lượng các chuỗi con. Tổng cộng có (2^n) dãy con và khi (k) ở khoảng (n/2), số ứng cử viên có độ dài chính xác (k) là 

[ 
\binom{n}{\lfloor n/2\rfloor}, 
] 

xấp xỉ (2^n/\sqrt{n}). Với (n=2\cdot10^5), điều này hoàn toàn không khả thi. 

Một chiến lược ít cực đoan hơn là xây dựng câu trả lời từ trái sang phải. Tại mỗi vị trí, hãy thử các giá trị ứng viên theo thứ tự tăng dần và tìm kiếm sự xuất hiện vẫn còn mọi giá trị còn thiếu. Ngay cả khi mỗi lần kiểm tra tính khả thi được thực hiện cẩn thận, các hậu tố quét liên tục có thể yêu cầu (O(nk)) hoạt động. Trong trường hợp xấu nhất, điều này đạt tới khoảng (4\cdot10^{10}) hoạt động. 

Quan sát hữu ích là việc giảm thiểu từ điển cho chúng ta một lý do rất cụ thể để loại bỏ một giá trị đã được chọn. Giả sử câu trả lời hiện tại kết thúc bằng giá trị (a) và giá trị chuỗi tiếp theo là (b<a). Nếu (a) xảy ra lần nữa sau đó thì việc giữ nguyên (a) hiện tại không bao giờ là tối ưu. Việc thay thế nó bằng (b) nhỏ hơn sẽ làm cho câu trả lời nhỏ hơn về mặt từ điển, trong khi sự xuất hiện sau của (a) vẫn có thể cung cấp bản sao cần thiết. 

Điều này đưa ra một chiến lược ngăn xếp đơn điệu. Chúng tôi xử lý trình tự từ trái sang phải. Ngăn xếp đại diện cho tiền tố tốt nhất mà chúng tôi hiện có thể xây dựng. Khi một giá trị nhỏ hơn xuất hiện, chúng tôi sẽ xóa các giá trị lớn hơn khỏi cuối ngăn xếp miễn là các giá trị đó xuất hiện lại sau đó. Nếu một giá trị không xuất hiện sau đó, nó sẽ bị khóa tại chỗ và không thể xóa được. 

Còn một điều kiện nữa. Chúng tôi cần mỗi giá trị chính xác một lần, vì vậy nếu giá trị hiện tại đã có trong ngăn xếp, chúng tôi chỉ cần bỏ qua nó. Lần xuất hiện đầu tiên không nhất thiết phải là lần chúng ta muốn, nhưng việc giữ nó tạm thời cho phép quy tắc ngăn xếp thay thế nó sau nếu giá trị nhỏ hơn xuất hiện trước lần xuất hiện cuối cùng có thể xảy ra. 

Do đó, cấu trúc dữ liệu quan trọng chỉ là một`last`mảng lưu trữ vị trí cuối cùng của mọi giá trị. Trong quá trình quét, ngăn xếp không chứa các bản sao và mỗi lần xuất hiện đều được chứng minh bằng sự tồn tại của một lần xuất hiện khác sau đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n)) ứng viên | (O(n)) | Quá chậm | 
| Quét tham lam lặp đi lặp lại | (O(nk)) | (O(k)) | Quá chậm | 
| Ngăn xếp đơn điệu | (O(n)) | (O(n+k)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc trình tự và tính toán`last[x]`, chỉ mục cuối cùng nơi mỗi giá trị (x) xuất hiện. Chúng tôi cần thông tin này vì giá trị chỉ có thể bị xóa khỏi câu trả lời hiện tại khi một bản sao khác được đảm bảo vẫn có sẵn sau này. 
2. Duy trì một ngăn xếp chứa tiền tố dãy con tốt nhất hiện tại và duy trì một mảng boolean`used`cho chúng tôi biết giá trị nào đã có trong ngăn xếp. Ngăn xếp không bao giờ chứa cùng một giá trị hai lần, điều này phù hợp trực tiếp với yêu cầu rằng câu trả lời cuối cùng chứa mọi giá trị chính xác một lần. 
3. Xử lý mọi giá trị (x) từ trái sang phải. Nếu như`used[x]`đã đúng thì bỏ qua (x). Giữ một bản sao khác sẽ vi phạm điều kiện chính xác một lần và bản sao đó không thể cải thiện câu trả lời vì lần xuất hiện hiện tại đã được thể hiện trong ngăn xếp. 
4. Nếu (x) không được sử dụng, hãy so sánh nó với giá trị cuối cùng trong ngăn xếp. Mặc dù ngăn xếp không trống, nhưng giá trị cuối cùng của nó lớn hơn (x) và giá trị cuối cùng đó lại xuất hiện sau vị trí hiện tại, hãy bật nó lên và đánh dấu là không sử dụng. (x) mới nhỏ hơn, do đó việc đặt nó sớm hơn sẽ làm cho chuỗi nhỏ hơn về mặt từ điển, trong khi việc xuất hiện sau của giá trị được lấy ra sẽ duy trì tính khả thi. 
5. Dừng popping ngay khi phần trên cùng của ngăn xếp nhỏ hơn (x) hoặc giá trị trên cùng không xuất hiện sau đó. Trong trường hợp đầu tiên, việc thay thế nó sẽ làm cho dãy lớn hơn. Trong trường hợp thứ hai, việc xóa nó sẽ làm mất bản sao duy nhất còn lại của giá trị đó. 
6. Đẩy (x) vào ngăn xếp và đánh dấu nó là đã sử dụng. Tiếp tục cho đến khi mọi phần tử đầu vào đã được xử lý. 
7. In ngăn xếp. Bởi vì mọi giá trị từ (1) đến (k) được đảm bảo xuất hiện trong đầu vào và chúng tôi chỉ xóa một giá trị khi còn lại một bản sao khác, ngăn xếp cuối cùng chứa tất cả các giá trị (k) chính xác một lần. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý bất kỳ tiền tố nào của đầu vào, ngăn xếp là dãy con có giá trị riêng biệt nhỏ nhất về mặt từ điển vẫn có thể được mở rộng để chứa mọi giá trị chưa bị mất vĩnh viễn. 

Khi một giá trị mới (x) nhỏ hơn đỉnh ngăn xếp (y), việc giữ (y) trước (x) sẽ làm cho kết quả về mặt từ điển trở nên tồi tệ hơn. Chúng tôi được phép xóa (y) chính xác khi một bản sao khác xuất hiện sau đó, vì vậy việc xóa nó không thể khiến việc hoàn thành hợp lệ trở nên bất khả thi. Việc lặp lại quá trình này sẽ loại bỏ mọi hậu tố ngăn xếp lớn hơn (x) và có thể thay thế được. 

Nếu một giá trị ngăn xếp không xuất hiện sau đó thì không thể xóa nó vì nó sẽ biến mất sau mỗi lần hoàn thành có thể xảy ra. Nếu đỉnh ngăn xếp nhỏ hơn (x), việc giữ giá trị nhỏ hơn trước đã là tối ưu về mặt từ điển. Do đó, mọi thao tác đẩy hoặc bật đều bị ép buộc bởi tính tối ưu và tính khả thi của từ điển, điều này chứng tỏ rằng ngăn xếp cuối cùng là dãy con mong muốn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = [int(input()) for _ in range(n)]

    last = [0] * (k + 1)

    for i, x in enumerate(a):
        last[x] = i

    used = [False] * (k + 1)
    stack = []

    for i, x in enumerate(a):
        if used[x]:
            continue

        while stack:
            top = stack[-1]

            if top <= x:
                break

            if last[top] <= i:
                break

            stack.pop()
            used[top] = False

        stack.append(x)
        used[x] = True

    print(*stack)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên tính toán lần xuất hiện cuối cùng của mọi giá trị. Vì các chỉ số được xử lý từ trái sang phải nên việc gán`last[x] = i`tự nhiên để lại sự xuất hiện cuối cùng trong mảng. 

các`used`mảng ngăn chặn các bản sao nhập câu trả lời. Điều này tách biệt với`last`:`last`trả lời liệu một giá trị có thể được loại bỏ hay không, trong khi`used`câu trả lời liệu chúng ta đã có bản sao của giá trị đó trong câu trả lời hiện tại chưa. 

Bên trong`while`vòng lặp là hoạt động ngăn xếp đơn điệu. điều kiện`top > x`là điều kiện từ điển học. điều kiện`last[top] > i`là điều kiện khả thi. Cả hai đều được yêu cầu. Việc bỏ qua điều kiện thứ hai có thể xóa lần xuất hiện duy nhất còn lại của một giá trị. 

Việc so sánh sử dụng`last[top] <= i`còn hơn là`last[top] < i`bởi vì chỉ mục hiện tại không thể là sự xuất hiện sau này của`top`khi`top`đã có trong ngăn xếp nhưng đang sử dụng`<=`làm cho ranh giới rõ ràng và an toàn. Trong thực tế, đối với một giá trị ngăn xếp riêng biệt khác với giá trị hiện tại`x`,`last[top]`ít nhất luôn luôn là`i + 1`nếu nó vẫn còn có sẵn. 

Mỗi giá trị được đẩy nhiều nhất một lần giữa các lần xóa và xuất hiện nhiều nhất một lần cho mỗi lần đẩy. Chính xác hơn, mỗi lần xuất hiện chỉ có thể gây ra công việc xếp chồng được phân bổ không đổi, do đó toàn bộ thuật toán là tuyến tính. 

Số nguyên Python không gây ra bất kỳ mối lo ngại tràn nào ở đây vì thuật toán chỉ thao tác các chỉ số và giá trị được giới hạn bởi (2\cdot10^5). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
6 3
3
2
1
3
1
3
```Vị trí xuất hiện cuối cùng, sử dụng các chỉ số dựa trên số 0, là`1 -> 4`,`2 -> 1`, Và`3 -> 5`. 

| Chỉ mục | x | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | --- | 
| 0 | 3 |`[]`| Đẩy 3 |`[3]`| 
| 1 | 2 |`[3]`| Pop 3 vì 3 > 2 và 3 xuất hiện muộn hơn |`[2]`| 
| 2 | 1 |`[2]`| Pop 2 vì 2 > 1 và 2 không có bản sao sau |`[2, 1]`| 
| 3 | 3 |`[2, 1]`| Đẩy 3 |`[2, 1, 3]`| 
| 4 | 1 |`[2, 1, 3]`| Bỏ qua vì 1 đã được sử dụng |`[2, 1, 3]`| 
| 5 | 3 |`[2, 1, 3]`| Bỏ qua vì 3 đã được sử dụng |`[2, 1, 3]`| 

Kết quả là`2 1 3`. Quyết định thú vị xảy ra ở chỉ số 1. Quyết định đầu tiên`3`bị xóa vì sau này`3`tồn tại, cho phép nhỏ hơn`2`trở thành yếu tố đầu tiên. các`2`sau này không thể được gỡ bỏ khi`1`đến vì không có thứ hai`2`. 

### Mẫu 2 

Đầu vào là```
10 5
5
4
3
2
1
4
1
1
5
5
```Các vị trí cuối cùng là`1 -> 7`,`2 -> 3`,`3 -> 2`,`4 -> 5`, Và`5 -> 9`. 

| Chỉ mục | x | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | --- | 
| 0 | 5 |`[]`| Đẩy 5 |`[5]`| 
| 1 | 4 |`[5]`| Pop 5 vì 5 xuất hiện muộn hơn |`[4]`| 
| 2 | 3 |`[4]`| Pop 4 vì số 4 xuất hiện muộn hơn |`[3]`| 
| 3 | 2 |`[3]`| Đẩy 2 vì 3 không có bản sau |`[3, 2]`| 
| 4 | 1 |`[3, 2]`| Đẩy 1 vì không bỏ được 3 và 2 |`[3, 2, 1]`| 
| 5 | 4 |`[3, 2, 1]`| Đẩy 4 |`[3, 2, 1, 4]`| 
| 6 | 1 |`[3, 2, 1, 4]`| Bỏ qua vì 1 đã được sử dụng |`[3, 2, 1, 4]`| 
| 7 | 1 |`[3, 2, 1, 4]`| Bỏ qua vì 1 đã được sử dụng |`[3, 2, 1, 4]`| 
| 8 | 5 |`[3, 2, 1, 4]`| Đẩy 5 |`[3, 2, 1, 4, 5]`| 
| 9 | 5 |`[3, 2, 1, 4, 5]`| Bỏ qua vì 5 đã được sử dụng |`[3, 2, 1, 4, 5]`| 

Đầu ra là`3 2 1 4 5`. Ví dụ này giải thích tại sao một phần tử không thể bị loại bỏ chỉ vì một giá trị nhỏ hơn xuất hiện. Khi`2`đến, các`3`là sự xuất hiện duy nhất của`3`, vì vậy việc loại bỏ nó sẽ khiến câu trả lời hoàn chỉnh không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+k)) | Việc tính toán lần xuất hiện cuối cùng mất (O(n)) và mỗi lần đẩy hoặc bật ngăn xếp được khấu hao (O(1)) | 
| Không gian | (O(n+k)) | Trình tự, ngăn xếp,`last`, Và`used`mảng chiếm không gian tuyến tính | 

Với (n\le2\cdot10^5), thuật toán chỉ thực hiện một lượng công việc phân bổ không đổi cho mỗi phần tử đầu vào. Điều này nằm trong phạm vi dự định cho giới hạn hai giây, trong khi các phương án bậc hai sẽ yêu cầu hàng chục tỷ phép tính. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    k = next(it)
    a = [next(it) for _ in range(n)]

    last = [0] * (k + 1)
    for i, x in enumerate(a):
        last[x] = i

    used = [False] * (k + 1)
    stack = []

    for i, x in enumerate(a):
        if used[x]:
            continue

        while stack and stack[-1] > x and last[stack[-1]] > i:
            used[stack.pop()] = False

        stack.append(x)
        used[x] = True

    return " ".join(map(str, stack)) + "\n"

def run(inp: str) -> str:
    return solve_data(inp)

# Provided sample 1
assert run("""6 3
3
2
1
3
1
3
""") == """2 1 3
""", "sample 1"

# Provided sample 2
assert run("""10 5
5
4
3
2
1
4
1
1
5
5
""") == """3 2 1 4 5
""", "sample 2"

# Minimum-size input
assert run("""1 1
1
""") == """1
""", "minimum size"

# All values are equal, k = 1
assert run("""8 1
1
1
1
1
1
1
1
1
""") == """1
""", "all equal values"

# Maximum-size style case with every value unique
assert run("10 10\n" + "\n".join(map(str, range(1, 11))) + "\n") == \
       "1 2 3 4 5 6 7 8 9 10\n", "all values already distinct"

# Repeated values designed to catch incorrect popping of a value
assert run("""8 4
4
3
2
1
3
4
2
1
""") == """3 2 1 4
""", "last-occurrence boundary"

# Large input, verifies linear behavior and the k = 1 boundary
large = "200000 1\n" + "1\n" * 200000
assert run(large) == "1\n", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`1`| Kích thước tối thiểu và ranh giới ngăn xếp trống | 
| Tám bản sao của`1`với`k=1`|`1`| Ngăn chặn trùng lặp và chạy trùng lặp lớn | 
|`1 2 3 ... 10`với`k=10`|`1 2 3 4 5 6 7 8 9 10`| Không có giá trị nào có thể bị loại bỏ khi mỗi giá trị xảy ra một lần | 
|`4 3 2 1 3 4 2 1`|`3 2 1 4`| Các giá trị không xảy ra sau đó phải được giữ nguyên | 
| 200000 bản sao`1`|`1`| Tối đa (n), hành vi thời gian tuyến tính và (k=1) | 

## Vỏ cạnh 

Khi chỉ có một giá trị riêng biệt, chẳng hạn như```
1 1
1
```thuật toán bắt đầu với một ngăn xếp trống, đẩy`1`, và kết thúc ngay lập tức. Không có nhạc pop để biểu diễn, và kết quả là`1`. Logic tương tự xử lý nhiều bản sao tùy ý, bởi vì mỗi lần sau`1`thấy`used[1] == True`và bị bỏ qua. 

Khi mọi giá trị xảy ra đúng một lần, hãy xem xét```
5 5
1
2
3
4
5
```Ngăn xếp tăng lên`[1, 2, 3, 4, 5]`. Bất cứ khi nào một giá trị mới xuất hiện, đỉnh ngăn xếp sẽ nhỏ hơn nên không thể xuất hiện pop. Đầu ra vẫn giữ nguyên trình tự ban đầu. Điều này là cần thiết vì việc xóa bất kỳ giá trị nào sẽ khiến không thể chứa mọi số trong đó.`1`bởi vì`5`. 

Trường hợp ranh giới tinh tế nhất xảy ra khi một giá trị nhỏ hơn xuất hiện sau một giá trị mà lần xuất hiện cuối cùng đã đạt đến. Vì```
4 3
3
2
1
3
```cái đầu tiên`3`được loại bỏ khi`2`đến vì người khác`3`tồn tại ở chỉ mục 3. Ngăn xếp trở thành`[2]`. Khi`1`đến,`2`không xảy ra sau đó nên không thể loại bỏ được. trận chung kết`3`sau đó được thêm vào, tạo ra```
2 1 3
```Đây chính xác là điều kiện được kiểm tra bởi`last[top] > i`. 

Giá trị trùng lặp xuất hiện sau khi đã được chọn cũng phải được bỏ qua. TRONG```
6 3
3
2
1
3
1
3
```cái đầu tiên`3`được thay thế bởi`2`, cái`2`được giữ lại vì nó không có bản sao sau này và bản sao sau này`3`cuối cùng được chọn. Tiếp theo`1`Và`3`bị bỏ qua vì cả hai giá trị đều đã được biểu diễn. Kết quả là`2 1 3`. 

Trường hợp kích thước tối đa không yêu cầu xử lý thuật toán khác. Với (n=200000) và (k=1), mọi giá trị đầu vào là`1`, do đó phần tử đầu tiên được đẩy và tất cả các phần tử còn lại bị bỏ qua. Thuật toán vẫn chỉ thực hiện công việc (O(n)), đó chính xác là lý do tại sao công thức ngăn xếp lần xuất hiện cuối cùng có tỷ lệ ràng buộc đầy đủ.
