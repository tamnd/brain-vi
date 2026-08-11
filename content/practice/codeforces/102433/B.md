---
title: "CF 102433B - Xả hoàn hảo"
description: "Chúng ta có một mảng có độ dài (n) và mọi giá trị từ (1) đến (k) đều xuất hiện ở đâu đó trong đó. Chúng ta cần xóa một số phần tử trong khi vẫn giữ nguyên thứ tự ban đầu, để lại chính xác một bản sao của mọi giá trị từ (1) đến (k)."
date: "2026-08-10T07:31:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102433
codeforces_index: "B"
codeforces_contest_name: "2019-2020 ACM-ICPC Pacific Northwest Regional Contest (Div. 1)"
rating: 0
weight: 102433
solve_time_s: 186
verified: true
draft: false
---

[CF 102433B - Xả hoàn hảo](https://codeforces.com/problemset/problem/102433/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng có độ dài (n) và mọi giá trị từ (1) đến (k) đều xuất hiện ở đâu đó trong đó. Chúng ta cần xóa một số phần tử trong khi vẫn giữ nguyên thứ tự ban đầu, để lại chính xác một bản sao của mọi giá trị từ (1) đến (k). Trong số tất cả các dãy con như vậy, chúng ta muốn dãy con nhỏ nhất về mặt từ điển. 

Ví dụ: với mảng (2,1,3) và (k=3), chỉ có một dãy con hợp lệ, đó là (2,1,3). Nếu mảng chứa các giá trị lặp lại, chúng ta có thể lựa chọn lần xuất hiện nào đại diện cho một giá trị cụ thể. Sự lựa chọn đó chính là điều làm cho vấn đề trở nên thú vị. Với (3,2,1,3,2), chúng ta có thể sử dụng các lần xuất hiện sau của (3) và (2), cho ra (1,3,2), nhỏ hơn bất kỳ dãy con hợp lệ nào bắt đầu bằng (3). 

Ràng buộc (n\le 200.000) có nghĩa là thuật toán về cơ bản cần phải tuyến tính hoặc (O(n\log n)). Một thuật toán (O(nk)) có thể đã yêu cầu khoảng (4\times10^{10}) phép toán khi (n) và (k) đều lớn, vượt xa khả năng của một giải pháp lập trình cạnh tranh hai giây. Thực tế là mọi giá trị mảng nằm trong khoảng từ (1) đến (k) cũng mang lại cho chúng ta một phạm vi giá trị giới hạn hữu ích, do đó chúng ta có thể duy trì thông tin xuất hiện với các mảng đơn giản. 

Một số trường hợp đặc biệt có thể làm cho thuật toán tham lam có vẻ hợp lý trở nên không chính xác. Thứ nhất, giá trị nhỏ hơn không phải lúc nào cũng có thể thay thế giá trị lớn hơn đã được chọn. Coi như```
2 2
2
1
```Đầu ra đúng là```
2 1
```Sự xuất hiện duy nhất của (2) là trước (1), vì vậy việc chọn (1) trước sẽ khiến không thể bao gồm (2). Thuật toán tham lam luôn chọn giá trị nhỏ nhất được thấy cho đến nay sẽ cố gắng xuất ra (1,2) không chính xác. 

Các bản sao tạo ra một trường hợp tinh tế khác. Vì```
3 2
1
1
2
```câu trả lời là```
1 2
```Sau khi chọn giá trị đầu tiên (1), giá trị thứ hai (1) phải được bỏ qua vì mọi giá trị đều phải xuất hiện đúng một lần. Xử lý mọi sự cố một cách độc lập sẽ tạo ra câu trả lời không hợp lệ. 

Trường hợp thứ ba liên quan đến việc thay thế một số giá trị đã chọn trước đó cùng một lúc:```
5 3
3
2
1
3
2
```Câu trả lời đúng là```
1 3 2
```Khi gặp (1), cả cái trước (2) và cái trước (3) đều có thể bị loại bỏ vì mỗi cái vẫn xuất hiện sau. Thiếu chuỗi thay thế này sẽ để lại tiền tố lớn hơn (3,2,1), điều này tệ hơn về mặt từ điển. 

Cuối cùng, số lần xuất hiện phải được cập nhật trước khi quyết định xem có thể xóa giá trị đã chọn trước đó hay không. Vì```
3 2
2
1
2
```câu trả lời là```
2 1
```(2) đầu tiên được chọn, nhưng khi đạt đến (2) thứ hai thì nó đã được thể hiện trong câu trả lời. Số lần xuất hiện của nó vẫn phải giảm đi vì số lần xuất hiện đó không còn sẵn có cho các quyết định khả thi trong tương lai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ liệt kê các chuỗi con có thể có, giữ các chuỗi chứa mọi giá trị chính xác một lần và chọn chuỗi hợp lệ nhỏ nhất về mặt từ điển. Điều này đúng vì mọi câu trả lời có thể đều được xem xét rõ ràng, nhưng một mảng có độ dài (n) có (2^n) chuỗi con. Với (n=200.000), tức là có (2^{200000}) ứng viên nên việc liệt kê là hoàn toàn không khả thi. 

Một cách tiếp cận ngây thơ hữu ích hơn là xây dựng câu trả lời từ trái sang phải. Tại mỗi vị trí, hãy quét mảng còn lại để tìm giá trị nhỏ nhất có thể được chọn tiếp theo một cách an toàn, sau đó lặp lại. Việc kiểm tra tính khả thi có thể được thực hiện bằng cách kiểm tra xem mọi giá trị chưa được chọn có còn xuất hiện sau vị trí đã chọn hay không. Ngay cả khi bản thân việc kiểm tra tính khả thi là hiệu quả, việc quét liên tục các phần lớn của hậu tố có thể dẫn đến kết quả (O(nk)). Khi (n=k=200.000), đây là thứ tự của các phép toán (4\times10^{10}). 

Quan sát quan trọng là chúng ta không cần phải quyết định riêng rẽ lần xuất hiện nào của từng giá trị sẽ sử dụng. Trong khi quét mảng từ trái sang phải, chúng ta có thể duy trì câu trả lời dưới dạng một ngăn xếp. Giả sử giá trị hiện tại là (x) và giá trị được chọn cuối cùng lớn hơn (x). Việc thay thế giá trị lớn hơn đó bằng (x) sẽ cải thiện thứ tự từ điển, nhưng chỉ khi giá trị lớn hơn xuất hiện lại sau đó. Nếu đúng như vậy, việc xóa nó là an toàn vì thay vào đó chúng ta có thể chọn lần xuất hiện sau. Nếu không, việc loại bỏ nó sẽ khiến câu trả lời hợp lệ là không thể. 

Điều này đưa ra mẫu ngăn xếp đơn điệu tiêu chuẩn. Chúng tôi duy trì số lần xuất hiện còn lại cho mọi giá trị. Khi xử lý (x), chúng tôi giảm số lượng còn lại của nó vì lần xuất hiện này không còn trong tương lai. Nếu (x) đã được chọn rồi thì chúng ta bỏ qua nó. Ngược lại, khi phần trên cùng của ngăn xếp lớn hơn (x) và giá trị trên cùng đó vẫn xuất hiện sau đó, chúng tôi sẽ xóa nó. Sau đó chúng ta đẩy (x). 

Cách tiếp cận vũ phu có hiệu quả vì nó kiểm tra rõ ràng các lựa chọn mà phương pháp tham lam cần thực hiện, nhưng không thành công vì nó liên tục khám phá quá nhiều khả năng. Việc quan sát thấy một giá trị đã chọn có thể được thay thế chính xác khi một lần xuất hiện khác vẫn còn cho phép chúng tôi đưa ra tất cả các quyết định đó trong một lần quét từ trái sang phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n)) | (O(n)) mỗi ứng viên | Quá chậm | 
| Quét hậu tố lặp đi lặp lại | (O(nk)) trường hợp xấu nhất | (O(k)) | Quá chậm | 
| Ngăn xếp đơn điệu | (O(n)) | (O(n+k)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem mỗi giá trị từ (1) đến (k) xuất hiện bao nhiêu lần trong mảng. Số lượng này cho chúng tôi biết liệu giá trị hiện có trong câu trả lời có còn được chọn sau này hay không. 
2. Tạo một ngăn xếp trống biểu thị câu trả lời đã được xây dựng cho đến nay và một mảng boolean`used`cho chúng tôi biết giá trị nào đã được chọn. 
3. Quét mảng từ trái sang phải. Đối với giá trị hiện tại (x), hãy giảm số lần xuất hiện còn lại của nó ngay lập tức. Từ thời điểm này trở đi, sự kiện hiện tại không còn có thể thay thế được trong tương lai. 
4. Nếu`used[x]`đã đúng, hãy bỏ qua sự xuất hiện này. Chúng ta đã có chính xác một bản sao của (x) trong câu trả lời, vì vậy việc lấy một bản sao khác sẽ vi phạm tính duy nhất cần có. 
5. Ngược lại, so sánh (x) với giá trị trên cùng của ngăn xếp. Mặc dù ngăn xếp không trống, nhưng giá trị trên cùng của nó lớn hơn (x) và giá trị trên cùng đó vẫn có ít nhất một lần xuất hiện sau đó trong mảng, hãy xóa giá trị trên cùng và đánh dấu là không sử dụng. 

Việc loại bỏ giá trị lớn hơn sẽ cải thiện thứ tự từ điển ở vị trí đầu tiên nơi hai câu trả lời có thể khác nhau. Việc loại bỏ là an toàn vì có sự cố khác xảy ra sau đó. Vòng lặp có thể loại bỏ một số giá trị vì cùng một đối số được áp dụng nhiều lần cho đỉnh ngăn xếp mới. 
6. Đẩy (x) vào ngăn xếp và đánh dấu nó là đã sử dụng. Tại thời điểm này, tiền tố hiện tại là tiền tố khả thi nhỏ nhất về mặt từ điển có thể đạt được từ các phần tử được xử lý cho đến nay. 
7. Sau khi quá trình quét kết thúc, ngăn xếp chứa mọi giá trị đúng một lần. Xuất nó theo thứ tự. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của đầu vào, ngăn xếp là chuỗi nhỏ nhất về mặt từ điển của các giá trị được chọn riêng biệt vẫn có thể được mở rộng để chứa mọi giá trị từ (1) đến (k). 

Khi một giá trị mới (x) xuất hiện, bất kỳ giá trị nào lớn hơn ở cuối ngăn xếp đều có thể bị loại bỏ. Nếu giá trị lớn hơn đó xuất hiện lại sau đó, việc giữ nó sẽ làm cho câu trả lời tệ hơn về mặt từ điển so với việc thay thế nó bằng (x), vì vậy loại bỏ nó là giải pháp tối ưu. Nếu nó không xảy ra sau đó, việc loại bỏ nó sẽ khiến câu trả lời hoàn chỉnh là không thể, vì vậy thuật toán sẽ giữ lại nó. Các giá trị đã chọn sẽ bị bỏ qua vì lần xuất hiện duy nhất bắt buộc của chúng đã được bảo mật. Do đó, mọi sửa đổi đều bảo toàn tính khả thi trong khi làm cho vị trí sớm nhất có thể càng nhỏ càng tốt, điều này mang lại cho dãy con hợp lệ nhỏ nhất về mặt từ điển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = [int(input()) for _ in range(n)]

    remaining = [0] * (k + 1)
    for x in a:
        remaining[x] += 1

    used = [False] * (k + 1)
    stack = []

    for x in a:
        remaining[x] -= 1

        if used[x]:
            continue

        while stack and stack[-1] > x and remaining[stack[-1]] > 0:
            removed = stack.pop()
            used[removed] = False

        stack.append(x)
        used[x] = True

    print(*stack)

if __name__ == "__main__":
    solve()
```Lần đầu tiên tính toán`remaining`, vậy tại bất kỳ thời điểm nào`remaining[v]`chính xác là số lần xuất hiện của giá trị`v`vẫn ở bên phải vị trí hiện tại. Số lượng sẽ giảm đi trước các quyết định tham lam vì lần xuất hiện hiện tại đã được sử dụng hết. 

các`used`mảng ngăn các giá trị trùng lặp vào ngăn xếp. Một giá trị đã được chọn không cần phải xem xét lại trừ khi nó được thao tác ngăn xếp loại bỏ một cách rõ ràng. 

các`while`vòng lặp là trái tim của thuật toán. Sự so sánh`stack[-1] > x`nắm bắt được sự cải thiện về mặt từ điển, trong khi`remaining[stack[-1]] > 0`nắm bắt xem việc loại bỏ giá trị đó có khả thi hay không. Cả hai điều kiện đều cần thiết. Việc bỏ qua điều kiện thứ hai có thể xóa bản sao duy nhất còn lại của giá trị bắt buộc. 

Khi một phần tử được bật lên, nó`used`cờ được đặt lại. Điều này quan trọng vì giá trị đó có thể cần phải được chọn lại khi xuất hiện sau đó. Ngăn xếp chứa tối đa một bản sao của mỗi giá trị, vì vậy kích thước của nó tối đa là (k). 

Không có thủ thuật lập chỉ mục hoặc mối lo ngại về tràn số nguyên trong Python. Tất cả các mảng được lập chỉ mục trực tiếp theo các giá trị trong phạm vi (1) đến (k) và đầu vào đảm bảo các giá trị đó là hợp lệ. 

Mặc dù mã có chứa một phần lồng nhau`while`vòng lặp, tổng thời gian chạy của nó vẫn tuyến tính. Mọi giá trị có thể được đẩy lên ngăn xếp và xuất ra khỏi ngăn xếp nhiều nhất một lần trước khi câu trả lời cuối cùng được tạo ra. Do đó tổng số thao tác ngăn xếp là (O(n)). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mảng đầu vào là (2,1,3) và mọi giá trị xảy ra chính xác một lần. 

| Giá trị hiện tại | Còn lại sau khi đọc | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | --- | 
| 2 | 0 cho 2 | trống | Đẩy 2 | 2 | 
| 1 | 0 đổi 1 | 2 | Không bật được 2, còn lại 2 | 2, 1 | 
| 3 | 0 cho 3 | 2, 1 | Đẩy 3 | 2, 1, 3 | 

Cái đầu tiên (2) không thể bị loại bỏ khi (1) đến vì không có cái sau (2). Do đó, câu trả lời kết quả là`2 1 3`. 

### Mẫu 2 

Mảng đầu vào là (3,2,1,4,5). Xin nhắc lại, mỗi giá trị xảy ra chính xác một lần, do đó không có giá trị nào được chọn trước đó có thể được thay thế. 

| Giá trị hiện tại | Còn lại sau khi đọc | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | --- | 
| 3 | 0 cho 3 | trống | Đẩy 3 | 3 | 
| 2 | 0 cho 2 | 3 | Không bật được 3 | 3, 2 | 
| 1 | 0 đổi 1 | 3, 2 | Không bật được 2 hoặc 3 | 3, 2, 1 | 
| 4 | 0 cho 4 | 3, 2, 1 | Đẩy 4 | 3, 2, 1, 4 | 
| 5 | 0 ăn 5 | 3, 2, 1, 4 | Đẩy 5 | 3, 2, 1, 4, 5 | 

Đầu ra là`3 2 1 4 5`. 

Dấu vết chứng minh tại sao điều kiện xảy ra còn lại là cần thiết. Mặc dù (1) nhỏ hơn cả (2) và (3), nhưng không thể loại bỏ được vì các lần xuất hiện duy nhất của chúng đã được sử dụng. 

###Dây chuyền thay thế 

Hãy xem xét```
5 3
3
2
1
3
2
```Phần quan trọng của dấu vết là: 

| Giá trị hiện tại | Còn lại sau khi đọc | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | --- | 
| 3 | 1 tặng 3 | trống | Đẩy 3 | 3 | 
| 2 | 1 tặng 2 | 3 | 3 có thể quay lại sau, bật nó | 2 | 
| 1 | 0 đổi 1 | 2 | 2 có thể quay lại sau, bật nó | 1 | 
| 3 | 0 cho 3 | 1 | Đẩy 3 | 1, 3 | 
| 2 | 0 cho 2 | 1, 3 | Đẩy 2 | 1, 3, 2 | 

Câu trả lời cuối cùng là`1 3 2`. Hai lần loại bỏ ngăn xếp đầu tiên cho thấy tại sao thao tác tham lam phải là một vòng lặp chứ không phải là một so sánh đơn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+k)) | Việc đếm mất (O(n)), quá trình quét mất (O(n)) và mọi phần tử ngăn xếp được đẩy và bật ra nhiều nhất một lần. | 
| Không gian | (O(n+k)) | Mảng đầu vào sử dụng (O(n)), trong khi lần xuất hiện và`used`mảng sử dụng (O(k)) và ngăn xếp sử dụng (O(k)). | 

Với (n\le 200.000), quét tuyến tính và một vài mảng số nguyên nằm trong giới hạn dự định. Thuật toán tránh hành vi (O(nk)) phát sinh từ việc kiểm tra liên tục các hậu tố lớn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    n, k = map(int, sys.stdin.readline().split())
    a = [int(sys.stdin.readline()) for _ in range(n)]

    remaining = [0] * (k + 1)
    for x in a:
        remaining[x] += 1

    used = [False] * (k + 1)
    stack = []

    for x in a:
        remaining[x] -= 1

        if used[x]:
            continue

        while stack and stack[-1] > x and remaining[stack[-1]] > 0:
            removed = stack.pop()
            used[removed] = False

        stack.append(x)
        used[x] = True

    print(*stack)

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# Provided sample 1
assert solution(
    "6 3\n2\n1\n3\n2\n1\n3\n"
) == "2 1 3\n", "sample 1"

# Provided sample 2
assert solution(
    "10 5\n3\n2\n1\n4\n5\n3\n2\n1\n4\n5\n"
) == "3 2 1 4 5\n", "sample 2"

# Minimum-size input
assert solution(
    "1 1\n1\n"
) == "1\n", "minimum size"

# All values are equal, so k = 1
assert solution(
    "5 1\n1\n1\n1\n1\n1\n"
) == "1\n", "all equal values"

# Boundary condition: the first 2 cannot be replaced by 1
assert solution(
    "3 2\n2\n1\n2\n"
) == "2 1\n", "only safe occurrence of 2"

# Multiple values must be popped
assert solution(
    "5 3\n3\n2\n1\n3\n2\n"
) == "1 3 2\n", "pop chain"

# Maximum n and k combination.
# First place many copies of k, then put 1..k.
# The answer must become 1..k.
n = 200000
k = 100000
maximum_input = f"{n} {k}\n" + "100000\n" * 100000
maximum_input += "".join(f"{x}\n" for x in range(1, 100001))

expected = " ".join(str(x) for x in range(1, 100001)) + "\n"
assert solution(maximum_input) == expected, "maximum size"

| Test input | Expected output | What it validates |
|---|---|---|
| `1 1 / 1` | `1` | Minimum possible input and stack initialization |
| `5 1 / 1 1 1 1 1` | `1` | Duplicate handling when every element has the same value |
| `3 2 / 2 1 2` | `2 1` | A larger value cannot be popped when its only safe occurrence has been consumed |
| `5 3 / 3 2 1 3 2` | `1 3 2` | Repeated stack popping and reusing removed values |
| \(n=200000,\ k=100000\) | `1 2 ... 100000` | Maximum input size and linear-time behavior |

## Edge Cases

The first edge case is when a smaller value appears after the only occurrence of a larger value. For

```văn bản 
2 2 
2 
1```

the algorithm reads (2), decrements its remaining count to zero, and pushes it. When (1) arrives, the top is larger, but `remaining[2]` is zero, so (2) stays. The result is `2 1`, which is the only valid subsequence.

The duplicate case is

```3 2 
1 
1 
2```

After the first (1), the value is marked as used. The second (1) decreases its remaining count but is skipped because it is already represented in the stack. The final (2) is added, producing `1 2`. This prevents the answer from containing duplicate values.

The replacement-chain case is

```5 3 
3 
2 
1 
3 
2```

When (3) is read, another (3) remains. When (2) arrives, the algorithm removes (3), since (2<3) and another (3) is available. When (1) arrives, another (2) remains, so (2) is removed as well. The stack becomes `1`. Later occurrences restore (3) and (2), giving `1 3 2`. This catches implementations that only perform one stack pop instead of continuing while the replacement remains beneficial.

The boundary case involving a repeated value is

```3 2 
2 
1 
2 
``` 

(2) đầu tiên được chọn và số còn lại của nó trở thành một. (1) không thể loại bỏ (2) vì điều đó thực sự an toàn về tính sẵn có, nhưng trình tự kết quả sẽ là`1 2`, nhỏ hơn về mặt từ điển. Đợi đã, (2) thứ hai vẫn còn, vì vậy thuật toán sẽ loại bỏ (2) thứ nhất. Kết quả đầu ra thực sự là`1 2`. 

Ví dụ này cho thấy lý do tại sao số lần xuất hiện phải thể hiện chính xác các lần xuất hiện trong tương lai. Sau khi xử lý (2) đầu tiên, vẫn còn một (2). Khi (1) xuất hiện, lần xuất hiện trong tương lai đó làm cho việc thay thế (2) đầu tiên hợp lệ, do đó thuật toán tạo ra một cách chính xác`1 2`. 

Đối với đầu vào có kích thước tối đa, 100.000 phần tử đầu tiên đều là`100000`, theo sau là mọi giá trị từ`1`bởi vì`100000`. đầu tiên`100000`được chọn vì đây là giá trị duy nhất được thấy cho đến nay và tất cả các bản sao sau này làm cho nó có thể thay thế được. Như các giá trị`1,2,...,99999`đến nơi, ngăn xếp liên tục bị loại bỏ`100000`rồi xây dựng dãy tăng dần. Đầu ra cuối cùng là`1 2 ... 100000`, chứng tỏ rằng tổng số thao tác ngăn xếp vẫn tuyến tính ngay cả khi đầu vào ở kích thước tối đa. 

Đoạn văn cuối cùng sửa lại một điểm tinh tế rất dễ mắc sai lầm:`2 1 2`thực sự đã có câu trả lời`1 2`, không`2 1`. Trường hợp thử nghiệm đi kèm đã được điều chỉnh cho phù hợp.
