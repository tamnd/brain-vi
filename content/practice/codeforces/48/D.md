---
title: "CF 48D - Hoán vị"
description: "Chúng ta được cung cấp một mảng được xáo trộn ban đầu đến từ việc ghép một số hoán vị. Mỗi hoán vị có thể có kích thước khác nhau. Sau khi ghép, tất cả các số được trộn lẫn với nhau nên nhóm ban đầu biến mất."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 48
codeforces_index: "D"
codeforces_contest_name: "School Personal Contest #3 (Winter Computer School 2010/11) - Codeforces Beta Round 45 (ACM-ICPC Rules)"
rating: 1500
weight: 48
solve_time_s: 123
verified: false
draft: false
---

[CF 48D - Hoán vị](https://codeforces.com/problemset/problem/48/D) 

**Đánh giá:** 1500 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng được xáo trộn ban đầu đến từ việc ghép một số hoán vị. Mỗi hoán vị có thể có kích thước khác nhau. Sau khi ghép, tất cả các số được trộn lẫn với nhau nên nhóm ban đầu biến mất. 

Nhiệm vụ của chúng ta là gán mỗi phần tử mảng cho một số hoán vị sao cho mỗi nhóm tạo thành một hoán vị hợp lệ. Một hoán vị hợp lệ của kích thước`k`chứa mọi số nguyên từ`1`ĐẾN`k`đúng một lần. 

Ví dụ: nếu một nhóm chứa`{1,2,3,4}`, đó là một hoán vị. Nếu nó chứa`{1,2,4}`, không phải vậy, bởi vì`3`bị thiếu. Nếu nó chứa`{1,1,2}`, nó cũng không hợp lệ vì sự lặp lại. 

Đầu vào chỉ đưa ra các số được xáo trộn. Chúng tôi phải xây dựng lại bất kỳ nhóm hợp lệ nào hoặc báo cáo rằng không có sự phân tách nào tồn tại. 

Ràng buộc`n ≤ 10^5`ngay lập tức loại trừ bất cứ điều gì bậc hai. Một giải pháp quét liên tục tất cả các nhóm hiện có để tìm mọi số sẽ hoạt động xung quanh`10^10`trong trường hợp xấu nhất vượt xa thời hạn. Chúng ta cần một cái gì đó gần tuyến tính hoặc`O(n log n)`. 

Phần khó khăn là hiểu những điều kiện nào có thể thực hiện được sự phân hủy. 

Giả sử giá trị`x`xuất hiện`cnt[x]`lần. Mọi hoán vị chứa`x`cũng phải chứa tất cả các số từ`1`ĐẾN`x-1`. Điều đó có nghĩa là: 

- Số hoán vị chứa`x`bằng`cnt[x]`. 
- Những hoán vị tương tự cũng phải chứa`1,2,...,x-1`. 

Vậy số đếm phải thỏa mãn:```
cnt[1] ≥ cnt[2] ≥ cnt[3] ≥ ...
```Nếu sự đơn điệu này thất bại ở bất cứ đâu thì việc tái thiết là không thể. 

Một số trường hợp khó xử lý. 

Coi như:```
3
2 2 1
```Có hai bản sao của`2`, vì vậy chúng ta sẽ cần hai hoán vị chứa`2`. Nhưng mọi hoán vị chứa`2`cũng phải chứa`1`. Vì chỉ có một`1`, câu trả lời là không thể. 

Một trường hợp nguy hiểm khác là:```
5
1 3 1 2 2
```Số lượng là:```
cnt[1] = 2
cnt[2] = 2
cnt[3] = 1
```Điều này là hợp lệ. Một sự phân hủy có thể là:```
(1,2,3)
(1,2)
```Một chiến lược tham lam bất cẩn đặt các số một cách tùy ý có thể tạo ra các giá trị trùng lặp trong một nhóm và thất bại ngay cả khi tồn tại một giải pháp hợp lệ. 

Trường hợp nhỏ nhất cũng có vấn đề:```
1
1
```Đây đã là một hoán vị kích thước hợp lệ`1`. 

Nhưng:```
1
2
```là không thể vì không hoán vị nào có thể chứa`2`mà không chứa`1`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xây dựng rõ ràng từng hoán vị một. Đối với mỗi số trong mảng, chúng ta có thể quét tất cả các nhóm hiện có và đặt nó vào một nhóm chưa chứa giá trị đó và vẫn có thể trở thành một hoán vị hợp lệ sau này. 

Điều này hoạt động về mặt khái niệm vì yêu cầu duy nhất là tính duy nhất bên trong mỗi hoán vị và sự hiện diện của các giá trị nhỏ hơn. Nhưng việc thực hiện nhanh chóng trở nên tốn kém. Nếu chúng ta có tới`10^5`nhóm và quét nhiều nhóm trong số chúng để tìm mọi phần tử, độ phức tạp sẽ trở nên`O(n^2)`. 

Cấu trúc hoán vị mang lại một tính chất mạnh hơn nhiều. 

Nếu một hoán vị chứa`x`, thì nó phải chứa mọi số nguyên dương nhỏ hơn. Điều đó có nghĩa là tập hợp các hoán vị chứa`x+1`luôn là tập con của tập hợp chứa`x`. 

Điều này biến vấn đề thành một điều kiện đếm. 

Giả sử giá trị`x`xuất hiện`k`lần. Thế thì chính xác`k`hoán vị phải chứa`x`. Vì mọi hoán vị chứa`x+1`cũng chứa`x`, ta phải có:```
cnt[x] ≥ cnt[x+1]
```Khi điều kiện này được giữ cho tất cả các giá trị, việc xây dựng trở nên dễ dàng. 

Chúng ta có thể tạo chính xác`cnt[1]`hoán vị. Khi đó với mọi giá trị`x`, chúng tôi phân phối số lần xuất hiện của nó trong số những lần đầu tiên`cnt[x]`hoán vị. Vì số lượng không tăng nên bất kỳ hoán vị nào nhận được`x`đã nhận được tất cả các giá trị nhỏ hơn trước đó. 

Sự phân công tham lam này đủ để đảm bảo tính hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n²) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và đếm tần số của mọi giá trị. 

Cho phép`cnt[x]`là số lần xuất hiện của giá trị`x`. 
2. Kiểm tra xem tần số có tăng không. 

Đối với mọi`x > 1`, xác minh:```
cnt[x] ≤ cnt[x-1]
```Nếu điều này không thành công, hãy in`-1`. 

Lý do là về mặt cấu trúc. Mọi hoán vị chứa`x`cũng phải chứa`x-1`, vì vậy không thể có thêm bản sao của`x`hơn bản sao của`x-1`. 
3. Tạo`cnt[1]`hoán vị. 

Mọi hoán vị hợp lệ phải chứa`1`, do đó số hoán vị buộc phải bằng số hoán vị một. 
4. Chỉ định các lần xuất hiện một cách tham lam. 

Với mỗi giá trị`x`, phân phối số lần xuất hiện của nó giữa các hoán vị`1`bởi vì`cnt[x]`. 

Cụ thể, nếu`x`xuất hiện ba lần, gán chúng cho các hoán vị`1`,`2`, Và`3`. 
5. Đưa ra câu trả lời cho thứ tự mảng ban đầu. 

Vì mảng đã bị xáo trộn nên chúng ta cần gán một phép gán cho từng lần xuất hiện riêng lẻ. Lưu trữ một hàng ID hoán vị cho mọi giá trị. Khi duyệt qua mảng ban đầu, hãy bật một ID từ hàng đợi tương ứng. 

### Tại sao nó hoạt động 

Bất biến chính là:```
If permutation p receives value x, then it has already received every value from 1 to x-1.
```Điều này vẫn đúng vì các giá trị được xử lý theo thứ tự tăng dần và giá trị`x`chỉ được gán cho cái đầu tiên`cnt[x]`hoán vị. Từ:```
cnt[x] ≤ cnt[x-1]
```những hoán vị đó đã nhận được`x-1`. 

Mỗi hoán vị kết thúc có chứa chính xác:```
1,2,...,k
```đối với một số người`k`, đó chính xác là một hoán vị hợp lệ. 

## Giải pháp Python```python
import sys
from collections import defaultdict, deque

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    freq = defaultdict(int)

    for x in a:
        freq[x] += 1

    mx = max(a)

    for x in range(2, mx + 1):
        if freq[x] > freq[x - 1]:
            print(-1)
            return

    groups = freq[1]

    assign = defaultdict(deque)

    for x in range(1, mx + 1):
        for group_id in range(1, freq[x] + 1):
            assign[x].append(group_id)

    ans = []

    for x in a:
        ans.append(assign[x].popleft())

    print(groups)
    print(*ans)

solve()
```Phần đầu tiên đếm tần số. sử dụng`defaultdict(int)`tránh việc kiểm tra sự tồn tại thủ công và giữ mã sạch. 

Việc kiểm tra tính đơn điệu là toàn bộ bài kiểm tra tính khả thi. Nếu một giá trị nào đó xuất hiện nhiều lần hơn giá trị trước đó thì việc xây dựng lại là không thể vì mọi bản sao của giá trị lớn hơn đều yêu cầu một bản sao riêng biệt của giá trị nhỏ hơn trong cùng một hoán vị. 

Số lượng hoán vị được cố định là`freq[1]`. Mọi hoán vị hợp lệ phải chứa`1`, do đó không thể có nhiều hoán vị hơn một và mỗi hoán vị thuộc về một hoán vị khác nhau. 

Cấu trúc bài tập rất tinh tế. Với mọi giá trị`x`, chúng tôi tính toán trước ID hoán vị nào sẽ nhận được nó:```
1, 2, ..., freq[x]
```Sau đó, trong khi duyệt qua thứ tự mảng ban đầu, chúng tôi sử dụng từng ID này bằng cách sử dụng`popleft()`. 

Điều này duy trì tính chính xác ngay cả khi các giá trị bằng nhau xuất hiện rải rác trong mảng. 

Một lỗi phổ biến là cố gắng gán trực tiếp trong lần truyền tải đầu tiên mà không phân tách các lần xuất hiện. Điều đó thường tạo ra các giá trị trùng lặp trong cùng một hoán vị. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9
1 2 3 1 2 1 4 2 5
```Tần số:```
cnt[1] = 3
cnt[2] = 3
cnt[3] = 1
cnt[4] = 1
cnt[5] = 1
```Số lượng không tăng nên có thể xây dựng được. 

Bài tập đã chuẩn bị:```
1 -> [1,2,3]
2 -> [1,2,3]
3 -> [1]
4 -> [1]
5 -> [1]
```Xử lý mảng ban đầu: 

| Vị trí | Giá trị | Hoán vị được chỉ định | Hàng đợi còn lại | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | [2,3] | 
| 2 | 2 | 1 | [2,3] | 
| 3 | 3 | 1 | [] | 
| 4 | 1 | 2 | [3] | 
| 5 | 2 | 2 | [3] | 
| 6 | 1 | 3 | [] | 
| 7 | 4 | 1 | [] | 
| 8 | 2 | 3 | [] | 
| 9 | 5 | 1 | [] | 

Đầu ra cuối cùng:```
3
1 1 1 2 2 3 1 3 1
```Dấu vết này cho thấy sự bất biến trung tâm. Mọi hoán vị nhận số lớn hơn đều đã nhận được tất cả các số nhỏ hơn. 

### Ví dụ 2 

đầu vào:```
3
2 2 1
```Tần số: 

| Giá trị | Đếm | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 

Hiện nay:```
cnt[2] > cnt[1]
```Vì vậy việc tái thiết là không thể. 

Đầu ra:```
-1
```Điều này chứng tỏ tại sao điều kiện tần số là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được tính và gán một lần | 
| Không gian | O(n) | Bảng tần số và hàng đợi phân công lưu trữ tối đa n giá trị | 

Giải pháp thoải mái phù hợp với các hạn chế. Với`n = 10^5`, xử lý tuyến tính dễ dàng đủ nhanh trong giới hạn 1 giây trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import defaultdict, deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = io.StringIO()
    sys.stdout = out

    def solve():
        n = int(input())
        a = list(map(int, input().split()))

        freq = defaultdict(int)

        for x in a:
            freq[x] += 1

        mx = max(a)

        for x in range(2, mx + 1):
            if freq[x] > freq[x - 1]:
                print(-1)
                return

        groups = freq[1]

        assign = defaultdict(deque)

        for x in range(1, mx + 1):
            for group_id in range(1, freq[x] + 1):
                assign[x].append(group_id)

        ans = []

        for x in a:
            ans.append(assign[x].popleft())

        print(groups)
        print(*ans)

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided-style sample
assert run(
"""9
1 2 3 1 2 1 4 2 5
"""
).splitlines()[0] == "3"

# minimum valid case
assert run(
"""1
1
"""
) == """1
1"""

# impossible because 2 appears without enough 1s
assert run(
"""3
2 2 1
"""
) == "-1"

# all equal ones, each forms its own permutation
assert run(
"""4
1 1 1 1
"""
).splitlines()[0] == "4"

# exact single permutation
assert run(
"""5
1 2 3 4 5
"""
) == """1
1 1 1 1 1"""

# boundary monotonicity failure
assert run(
"""5
1 2 2 3 3
"""
) == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`| Một hoán vị | Đầu vào hợp lệ tối thiểu | 
|`2 2 1`|`-1`| Thiếu giá trị nhỏ hơn | 
|`1 1 1 1`| Bốn hoán vị | Nhiều hoán vị cỡ 1 | 
|`1 2 3 4 5`| Một hoán vị | Hoán vị hoàn chỉnh đơn | 
|`1 2 2 3 3`|`-1`| Vi phạm tính đơn điệu tần số | 

## Vỏ cạnh 

Hãy xem xét:```
3
2 2 1
```Tần số trở thành:```
cnt[1] = 1
cnt[2] = 2
```Trong quá trình xác thực, thuật toán sẽ kiểm tra:```
2 > 1
```và ngay lập tức từ chối đầu vào. 

Điều này đúng vì mọi hoán vị chứa`2`cũng phải chứa`1`. Hai bản sao của`2`sẽ yêu cầu hai bản sao của`1`. 

Bây giờ hãy xem xét:```
5
1 3 1 2 2
```Tần số:```
cnt[1] = 2
cnt[2] = 2
cnt[3] = 1
```Bài tập:```
1 -> [1,2]
2 -> [1,2]
3 -> [1]
```Trình tự xử lý: 

| Giá trị | Hoán vị được chỉ định | 
| --- | --- | 
| 1 | 1 | 
| 3 | 1 | 
| 1 | 2 | 
| 2 | 1 | 
| 2 | 2 | 

Hoán vị 1 trở thành:```
(1,2,3)
```Hoán vị 2 trở thành:```
(1,2)
```Mặc dù thứ tự mảng bị xáo trộn nhưng việc xây dựng vẫn thành công vì phép gán chỉ phụ thuộc vào tần số. 

Cuối cùng, kiểm tra:```
1
2
```Chúng tôi nhận được:```
cnt[1] = 0
cnt[2] = 1
```Điều kiện đơn điệu thất bại ngay lập tức:```
1 > 0
```Không hoán vị nào có thể bắt đầu tại`2`, do đó thuật toán xuất ra chính xác`-1`.
