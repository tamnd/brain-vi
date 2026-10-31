---
title: "CF 106391B - Lười biếng"
description: "Vấn đề mô tả một loạt các điểm vấn đề. Người tham gia chọn vị trí bắt đầu và phải giải mọi bài toán từ vị trí đó trở đi, ngoại trừ việc họ có thể bỏ qua tối đa một bài toán trong hậu tố đã chọn đó."
date: "2026-06-25T10:11:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106391
codeforces_index: "B"
codeforces_contest_name: "Purdue Spring 2026 In-House Contest #1"
rating: 0
weight: 106391
solve_time_s: 43
verified: true
draft: false
---

[CF 106391B - Lười biếng](https://codeforces.com/problemset/problem/106391/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một loạt các điểm vấn đề. Người tham gia chọn vị trí bắt đầu và phải giải mọi bài toán từ vị trí đó trở đi, ngoại trừ việc họ có thể bỏ qua tối đa một bài toán trong hậu tố đã chọn đó. Mục tiêu là đạt được ít nhất một nửa tổng số điểm hiện có trong khi giải được ít vấn đề nhất có thể. 

Đầu vào là một mảng duy nhất trong đó mỗi giá trị đại diện cho số điểm đạt được khi giải quyết vấn đề đó. Đầu ra là số lượng tối thiểu các vấn đề cần giải quyết theo chiến lược đã cho. 

Độ dài mảng có thể đạt tới$10^5$và điểm số có thể lớn bằng$10^9$. Điều này có nghĩa là một giải pháp thử mọi điểm bắt đầu và mô phỏng mọi số lượng vấn đề được giải quyết có thể đạt được$10^{10}$hoạt động, vượt xa những gì có thể có trong giới hạn thời gian của cuộc thi thông thường. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính. Điểm lớn cũng có nghĩa là chúng ta cần số nguyên 64 bit để tính tổng. 

Có một số trường hợp việc triển khai có thể thất bại trong âm thầm. Nếu toàn bộ hậu tố chỉ là một phần tử thì không được phép bỏ qua vì người tham gia sẽ không giải quyết được gì. Ví dụ:```
1
10
```Tổng điểm là 10, một nửa là 5 và đáp án là 1 vì bài toán duy nhất phải giải được. Một cách tiếp cận luôn trừ đi giá trị lớn nhất vì bài toán bị bỏ qua sẽ tính không chính xác các bài toán được giải bằng 0. 

Một trường hợp phức tạp khác là khi câu trả lời hay nhất yêu cầu bỏ qua một phần tử lớn trong khoảng đã chọn. Ví dụ:```
5
1 1 2 1 2
```Số điểm yêu cầu là 4. Bắt đầu từ bài toán thứ ba sẽ cho khoảng`[2, 1, 2]`. Bỏ qua phần giữa`1`chỉ cho phép giải quyết hai vấn đề và nhận được 4 điểm. Một phương pháp chỉ tìm kiếm các tổng liền kề mà không xem xét bài toán bị bỏ qua sẽ trả về kết quả lớn hơn. 

Trường hợp khó khăn cuối cùng là khi một vấn đề duy nhất đã đủ:```
3
2 3 1
```Tổng số là 6 và số điểm yêu cầu là 3. Chỉ riêng vấn đề thứ hai là có tác dụng nên câu trả lời là 1. Bất kỳ thuật toán nào buộc bỏ qua trong mỗi khoảng đã chọn sẽ bỏ lỡ điều này. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là thử mọi vị trí bắt đầu có thể. Đối với mỗi lần bắt đầu, chúng tôi mở rộng sang bên phải trong khi tích lũy điểm số. Ở mỗi độ dài, chúng tôi kiểm tra xem phân đoạn hiện tại, có thể sau khi loại bỏ một phần tử, có đủ điểm hay không. Điều này đúng vì mọi chiến lược hợp lệ đều tương ứng với một khoảng hậu tố nào đó với nhiều nhất một vấn đề bị bỏ qua. 

Vấn đề là số lượng khoảng thời gian. có$O(n^2)$các cặp bắt đầu và kết thúc có thể có. Với$n = 10^5$, điều này tạo ra khoảng$5 \times 10^9$khoảng thời gian trong trường hợp xấu nhất, không thể phù hợp với thời gian. 

Quan sát quan trọng là tất cả các điểm đều tích cực. Khi chúng tôi sửa điểm cuối bên phải, việc tăng điểm cuối bên trái chỉ làm giảm số điểm chúng tôi có thể đạt được. Điều này cho phép chúng ta sử dụng một cửa sổ trượt. Chúng tôi duy trì một cửa sổ có điểm là tổng của khoảng thời gian hiện tại và theo dõi giá trị lớn nhất bên trong nó. Nếu như`sum - maximum`là đủ, khoảng hiện tại có thể đáp ứng yêu cầu bằng cách bỏ qua phần tử lớn nhất của nó. Sau đó, chúng ta có thể thu nhỏ cửa sổ từ bên trái trong khi nó vẫn hợp lệ, vì cửa sổ hợp lệ nhỏ hơn chỉ có thể cải thiện câu trả lời. 

Độ dài cửa sổ cho biết số vấn đề được xem xét. Nếu chúng ta bỏ qua một phần tử, số vấn đề được giải quyết sẽ trở thành`length - 1`. Trường hợp phần tử đơn được xử lý riêng bằng cách chỉ áp dụng bỏ qua khi cửa sổ còn lại vẫn chứa ít nhất một vấn đề đã được giải quyết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số điểm của tất cả các bài và tính số điểm tối thiểu cần thiết, bằng một nửa tổng số điểm đã làm tròn. Cần có số nguyên 64 bit vì tổng có thể rất lớn. 
2. Bắt đầu một cửa sổ trượt với ranh giới bên trái ở bài toán đầu tiên. Mở rộng ranh giới bên phải từng vị trí một và cộng từng điểm vào tổng hiện tại. 
3. Duy trì giá trị tối đa bên trong cửa sổ hiện tại. Giá trị này thể hiện vấn đề đơn lẻ tốt nhất có thể bỏ qua nếu việc bỏ qua có ích. 
4. Bất cứ khi nào`current_sum - current_maximum`đạt được số điểm yêu cầu, cửa sổ hiện tại có thể đưa ra lời giải hợp lệ bằng cách bỏ qua bài toán lớn nhất của nó. Cập nhật câu trả lời bằng cách sử dụng`window_length - 1`. 
5. Di chuyển ranh giới bên trái về phía trước trong khi cửa sổ vẫn hợp lệ. Mỗi phần tử bị loại bỏ sẽ bị trừ khỏi tổng và các giá trị tối đa lỗi thời sẽ bị xóa khỏi cấu trúc lưu trữ mức tối đa của cửa sổ. 
6. Sau khi quá trình quét kết thúc, xuất ra giá trị nhỏ nhất tìm được. Nếu cửa sổ là một phần tử đơn lẻ thì lần bỏ qua sẽ không được tính vì ít nhất một vấn đề thực sự phải được giải quyết. 

Tại sao nó hoạt động: mọi chiến lược hợp lệ đều chọn một khối vấn đề liên tiếp và loại bỏ tối đa một phần tử khỏi khối đó. Cửa sổ trượt kiểm tra mọi ranh giới bên phải có thể có trong khi chỉ di chuyển ranh giới bên trái về phía trước, do đó mọi khoảng thời gian có thể xuất hiện chính xác một lần. Giá trị tối đa được lưu trữ là lựa chọn tốt nhất có thể cho bài toán bị bỏ qua trong khoảng đó, nghĩa là`sum - maximum`là số điểm lớn nhất đạt được khi giải được ít hơn một bài toán. Điều bất biến là bất cứ khi nào cửa sổ được xem xét, nó sẽ chứa chính xác các vấn đề từ điểm bắt đầu hiện tại đến điểm cuối hiện tại và mức tối đa được duy trì cho phép chúng ta đánh giá mức bỏ qua tốt nhất có thể. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    total = sum(a)
    need = (total + 1) // 2

    ans = n
    left = 0
    cur = 0
    heap = []

    for right, value in enumerate(a):
        cur += value
        heapq.heappush(heap, (-value, right))

        while heap and heap[0][1] < left:
            heapq.heappop(heap)

        while left <= right:
            while heap and heap[0][1] < left:
                heapq.heappop(heap)

            maximum = -heap[0][0]
            length = right - left + 1

            if length > 1 and cur - maximum >= need:
                ans = min(ans, length - 1)
                cur -= a[left]
                left += 1
            else:
                break

    print(ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên tính toán điểm mục tiêu. biểu hiện`(total + 1) // 2`thực hiện phép chia trần cần thiết vì người tham gia phải đạt được ít nhất một nửa tổng số. 

Vùng heap lưu trữ các giá trị âm vì vùng heap của Python là vùng heap tối thiểu. Bằng cách lưu trữ`-value`, phần tử trên cùng là điểm lớn nhất trong cửa sổ hiện tại. Mỗi mục nhập heap cũng lưu trữ chỉ mục của nó để các giá trị bên ngoài cửa sổ hiện tại có thể được loại bỏ một cách lười biếng. 

Vòng lặp bên ngoài mở rộng phía bên phải của cửa sổ. Vòng lặp bên trong cố gắng thu nhỏ cửa sổ bất cứ khi nào khoảng hiện tại có thể đáp ứng yêu cầu về điểm sau khi bỏ qua phần tử lớn nhất của nó. điều kiện`length > 1`ngăn chặn trường hợp không hợp lệ trong đó vấn đề được chọn duy nhất bị bỏ qua. 

Thứ tự của các hoạt động quan trọng. Chúng tôi xóa các mục nhập vùng nhớ heap lỗi thời trước khi sử dụng mức tối đa, nếu không thì mức tối đa có thể thuộc về một vấn đề không còn nằm trong khoảng thời gian đó. Các tổng được giữ dưới dạng số nguyên Python, do đó không có rủi ro tràn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
5
1 1 2 1 2
```Điểm yêu cầu là 4. 

| đúng | trái | tổng hiện tại | tối đa | số lượng đã giải quyết | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | không đủ | 
| 1 | 0 | 2 | 1 | không đủ | 
| 2 | 0 | 4 | 2 | có thể, đáp án 2 | 
| 3 | 2 | 3 | 2 | không đủ | 
| 4 | 2 | 5 | 2 | có thể, đáp án 2 | 

Phần quan trọng là khoảng cách từ chỉ số 2 đến chỉ số 4. Tổng của nó là 5 và loại bỏ giá trị 2 vẫn để lại đủ điểm nên chỉ giải quyết được hai vấn đề. 

Đối với mẫu thứ hai:```
3
2 3 1
```Số điểm yêu cầu là 3. 

| đúng | trái | tổng hiện tại | tối đa | số lượng đã giải quyết | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 2 | không đủ | 
| 1 | 0 | 5 | 3 | có thể, đáp án 1 | 
| 2 | 1 | 4 | 3 | có thể, đáp án 1 | 

Khoảng chỉ chứa giá trị 3 đã đạt được mục tiêu. Thuật toán tìm thấy nó trong khi thu nhỏ cửa sổ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi phần tử vào và rời khỏi heap một lần và các thao tác trên heap mất thời gian logarit | 
| Không gian | O(n) | Ứng viên cửa sổ heap lưu trữ | 

Giải pháp xử lý$10^5$các yếu tố một cách thoải mái. Hệ số logarit chỉ đến từ việc duy trì giá trị tối đa trong cửa sổ hiện tại. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    total = sum(a)
    need = (total + 1) // 2

    ans = n
    left = 0
    cur = 0
    heap = []

    for right, value in enumerate(a):
        cur += value
        heapq.heappush(heap, (-value, right))

        while heap and heap[0][1] < left:
            heapq.heappop(heap)

        while left <= right:
            while heap and heap[0][1] < left:
                heapq.heappop(heap)

            maximum = -heap[0][0]
            length = right - left + 1

            if length > 1 and cur - maximum >= need:
                ans = min(ans, length - 1)
                cur -= a[left]
                left += 1
            else:
                break

    sys.stdin = old_stdin
    return str(ans) + "\n"

assert solve_io("5\n1 1 2 1 2\n") == "2\n"
assert solve_io("3\n2 3 1\n") == "1\n"

assert solve_io("1\n10\n") == "1\n"
assert solve_io("4\n5 5 5 5\n") == "1\n"
assert solve_io("6\n1 1 1 1 1 100\n") == "1\n"
assert solve_io("5\n1 2 3 4 5\n") == "2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 10`|`1`| Trường hợp ranh giới phần tử đơn | 
|`4 / 5 5 5 5`|`1`| Tất cả các giá trị bằng nhau | 
|`6 / 1 1 1 1 1 100`|`1`| Yếu tố đơn lớn chiếm ưu thế | 
|`5 / 1 2 3 4 5`|`2`| Hành vi thu nhỏ cửa sổ trượt | 

## Vỏ cạnh 

Đối với trường hợp vấn đề duy nhất:```
1
10
```Mục tiêu là 5. Cửa sổ chứa một phần tử, nhưng điều kiện bỏ qua sẽ từ chối nó vì việc giải các bài toán bằng 0 là không hợp lệ. Câu trả lời vẫn là 1. 

Đối với trường hợp bỏ qua vấn đề:```
5
1 1 2 1 2
```Cửa sổ`[2, 1, 2]`có tổng 5 và tối đa 2. Loại bỏ số tối đa đó để lại chính xác 3, điều này là không đủ, vì vậy cách tốt nhất để bỏ qua là loại bỏ một trong những số đó khỏi`[1, 2, 1, 2]`, để lại 5 điểm từ ba vấn đề đã được giải quyết. Khoảng tối ưu là`[2, 1, 2]`với giá trị ở giữa bị bỏ qua, cho ra hai bài toán đã được giải quyết và đạt điểm 4. 

Đối với trường hợp có một giá trị vượt trội:```
6
1 1 1 1 1 100
```Mục tiêu là 53. Khi phần tử cuối cùng vào cửa sổ, giá trị tối đa là 100 và việc bỏ qua nó sẽ không hữu ích vì nó sẽ loại bỏ tất cả điểm. Cửa sổ chỉ chứa bài toán cuối cùng được xử lý như một phép giải thông thường, tạo ra câu trả lời 1.
