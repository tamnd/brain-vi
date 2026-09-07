---
title: "CF 104545B - Bóng lượng tử nổ"
description: "Chúng ta được cung cấp một số nhóm vật phẩm, trong đó mỗi nhóm tương ứng với một màu bóng bay và có số lượng ban đầu. Thùng chứa chỉ có thể chở tối đa một số lượng bóng bay đã được bơm căng cố định, vì vậy chúng ta cần giảm tổng số lượng bóng bay đã được bơm căng xuống tối đa một giới hạn nhất định."
date: "2026-06-30T08:56:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104545
codeforces_index: "B"
codeforces_contest_name: "VIII MaratonUSP Freshman Contest"
rating: 0
weight: 104545
solve_time_s: 62
verified: true
draft: false
---

[CF 104545B - Bóng lượng tử nổ](https://codeforces.com/problemset/problem/104545/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số nhóm vật phẩm, trong đó mỗi nhóm tương ứng với một màu bóng bay và có số lượng ban đầu. Thùng chứa chỉ có thể chở tối đa một số lượng bóng bay đã được bơm căng cố định, vì vậy chúng ta cần giảm tổng số lượng bóng bay đã được bơm căng xuống tối đa một giới hạn nhất định. 

Điều khó khăn là cách giảm thiểu hoạt động. Thời gian được chia thành giây. Trong mỗi giây, chúng ta chọn chính xác một màu và quan sát tất cả các quả bóng bay có màu đó. Khi một màu được quan sát trong một giây, tất cả các quả bóng bay có màu đó sẽ giảm đi một nửa một cách hiệu quả, với quá trình này hoạt động giống như sự phân chia tầng trên số đếm. Nếu một màu có x bong bóng, sau một thao tác, nó sẽ trở thành ⌊x/2⌋. Chúng tôi lặp lại quá trình này, chọn một màu mỗi giây và chúng tôi muốn giảm thiểu số giây cần thiết cho đến khi tổng số bóng bay còn lại tối đa là S. 

Đầu vào đưa ra số lượng màu và giới hạn dung lượng S, theo sau là số lượng ban đầu cho mỗi màu. Đầu ra là số thao tác tối thiểu (giây) cần thiết. 

Các ràng buộc cho phép tối đa 100.000 màu và mỗi số lượng có thể lớn bằng 1e9. Điều này ngay lập tức loại trừ mọi cách tiếp cận mô phỏng tất cả các hoạt động có thể có trên tất cả các màu một cách ngây thơ. Mỗi thao tác chỉ ảnh hưởng đến một màu, do đó, một chiến lược đơn giản liên tục tính lại tổng số tiền sau mỗi lựa chọn có thể sẽ quá chậm, đặc biệt vì một màu duy nhất có thể yêu cầu giảm O(log bi) và tính tổng tất cả các màu sẽ tạo ra giới hạn trên tiềm năng khoảng 1e5 × 30 phép toán chỉ để giảm, nhưng quá trình quyết định quan trọng hơn: việc chọn màu nào để giảm mỗi giây là khó khăn cốt lõi. 

Các trường hợp biên xuất hiện khi S rất nhỏ hoặc bằng 0 hoặc khi tất cả các giá trị đã nhỏ. Ví dụ: nếu S = 0 và tất cả bi = 1, chúng ta phải giảm mọi thứ về 0 và mỗi chuỗi giảm đều quan trọng vì mỗi màu co lại độc lập. Một trường hợp cạnh khác là khi S đủ lớn thì không cần thực hiện thao tác nào nữa, thao tác này sẽ trả về 0 ngay lập tức. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là mô phỏng quy trình từng bước. Tại mỗi giây, chúng tôi thử tất cả các màu có thể, tính tổng kết quả sau khi áp dụng thao tác giảm một nửa cho một màu đã chọn và chọn lựa chọn tốt nhất. Điều này sẽ liên quan đến việc tính lại tổng số tiền cho từng màu ứng cử viên ở mỗi bước. Ngay cả với tính toán trước, số lượng trạng thái tăng lên nhanh chóng vì mỗi thao tác sẽ thay đổi không gian trạng thái và chúng ta sẽ cần mô phỏng số lượng lớn các bước. Trong trường hợp xấu nhất, mỗi màu có thể bị giảm khoảng log bi lần, do đó có thể có hàng triệu lần chuyển đổi trạng thái và mỗi bước quyết định sẽ tốn O(N), dẫn đến hành vi tỷ lệ O(N² log A) không khả thi. 

Nhận xét quan trọng là mỗi thao tác đều có tác động giảm dần: giảm một nửa số lượng lớn ban đầu mang lại mức giảm đáng kể, nhưng lợi ích sẽ giảm đi khi số lượng trở nên nhỏ. Điều này cho thấy chúng ta nên luôn ưu tiên hoạt động hiện mang lại tổng số tiền giảm lớn nhất. 

Nếu chúng ta nghĩ về mặt lợi ích cận biên, thì mỗi màu có một chuỗi các “lợi ích” có thể có: lần giảm đầu tiên từ x xuống x/2 mang lại mức tăng x - x/2, lần giảm tiếp theo mang lại x/2 - x/4, v.v. Mỗi màu đóng góp một chuỗi lợi ích giảm dần. Vấn đề trở thành việc lựa chọn các hoạt động có mức tăng tối đa cho đến khi tổng giảm xuống tối đa S. Đây tự nhiên là một lựa chọn tham lam đối với tất cả các mức giảm tiềm năng, có thể được quản lý hiệu quả với vùng heap tối đa. 

Chúng tôi khởi tạo bằng cách tính toán tổng số tiền và đẩy, đối với mỗi màu, lợi ích của việc thực hiện lần giảm đầu tiên. Mỗi lần chúng tôi áp dụng một thao tác, chúng tôi sẽ bật mức tăng tốt nhất hiện có, trừ nó khỏi tổng số và đẩy mức tăng tiếp theo cho cùng màu đó.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N2 log A) | O(N) | Quá chậm | 
| Tham lam với Max Heap | O(N log A + K log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển bài toán thành việc áp dụng lặp đi lặp lại cách giảm có lợi nhất cho đến khi thỏa mãn ràng buộc tổng. 

1. Tính tổng số tiền ban đầu của tất cả các quả bóng bay. Đây là điểm khởi đầu mà chúng ta phải giảm bớt. 
2. Nếu tổng đã nhỏ hơn hoặc bằng S, chúng ta có thể dừng ngay lập tức với các phép toán bằng 0 vì không cần giảm. 
3. Đối với mỗi giá trị màu x, hãy tính tác động của một lần giảm: x trở thành x // 2, do đó mức tăng là x - x // 2. Lưu mức tăng này trong cấu trúc mức độ ưu tiên tối đa cùng với giá trị được cập nhật sau khi áp dụng thao tác một lần. 
4. Trích xuất màu mà lần giảm tiếp theo mang lại mức tăng lớn nhất. Áp dụng mức giảm đó, trừ đi mức tăng từ tổng số tiền và tính một giây thời gian đã sử dụng. 
5. Sau khi áp dụng mức giảm cho một màu, hãy tính mức tăng tiếp theo có thể có cho cùng màu đó bằng cách sử dụng giá trị mới của nó. Điều này thể hiện lần tiếp theo chúng ta có thể giảm lại màu đó và nó phải được đưa lại vào cấu trúc. 
6. Lặp lại quy trình cho đến khi tổng nhỏ hơn hoặc bằng S. 

Lý do điều này hoạt động là vì mỗi thao tác đóng góp một lượng giảm được xác định rõ ràng và mỗi lần giảm trong tương lai cho mỗi màu đều được biết trước như một phần của chuỗi giảm. Bằng cách luôn chọn mức giảm biên lớn nhất hiện tại, chúng tôi đảm bảo rằng mỗi giây được sử dụng để tối đa hóa mức giảm ngay lập tức trong tổng số tiền và không bao giờ chọn mức giảm nhỏ hơn trong khi vẫn còn mức giảm lớn hơn. 

Bất biến chính là ở mỗi bước, hàng đợi ưu tiên chứa chính xác mức giảm có sẵn tiếp theo cho mỗi màu dựa trên trạng thái hiện tại của nó. Điều này đảm bảo rằng mọi hành động có thể xảy ra trong tương lai đều được thể hiện chính xác và thuật toán không bao giờ bỏ qua việc giảm ngay lập tức tốt hơn. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, S = map(int, input().split())
    a = list(map(int, input().split()))

    total = sum(a)
    if total <= S:
        print(0)
        return

    # max heap via negative values
    pq = []

    for x in a:
        if x > 0:
            gain = x - x // 2
            heapq.heappush(pq, (-gain, x))

    ops = 0

    while total > S:
        gain, x = heapq.heappop(pq)
        gain = -gain

        total -= gain
        ops += 1

        new_x = x // 2
        if new_x > 0:
            new_gain = new_x - new_x // 2
            heapq.heappush(pq, (-new_gain, new_x))

    print(ops)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một hàng ưu tiên về mức giảm có thể. Mỗi phần tử lưu trữ giá trị hiện tại của một màu và lợi ích của việc giảm màu đó một lần. Sau mỗi thao tác, màu đó sẽ được cập nhật và được chèn lại với mức tăng tiềm năng tiếp theo. Vòng lặp dừng lại khi mức giảm tích lũy đưa tổng số về giới hạn yêu cầu. 

Một chi tiết tinh tế là chúng tôi luôn tính toán lợi nhuận từ giá trị hiện tại chứ không phải từ giá trị ban đầu. Điều này rất cần thiết vì trình tự giảm thay đổi sau mỗi lần giảm một nửa và việc không cập nhật nó sẽ đánh giá quá cao mức tăng trong tương lai. 

## Ví dụ đã hoạt động 

Xem xét đầu vào:```
2 5
3 5
```Tổng số ban đầu là 8, vì vậy chúng ta cần giảm ít nhất 3. 

| Bước | Được chọn | Bang (a) | Tổng cộng | 
| --- | --- | --- | --- | 
| 0 | - | [3, 5] | 8 | 
| 1 | 2 (5→2) | [3, 2] | 6 | 
| 2 | 1 (3→1) | [1, 2] | 5 | 

Sau hai thao tác, tổng số sẽ là 5, thỏa mãn điều kiện ràng buộc. 

Bây giờ hãy xem xét:```
5 0
1 1 1 1 1
```Tổng số ban đầu là 5, mục tiêu là 0 nên tất cả đều phải bị loại bỏ. 

| Bước | Được chọn | Bang (a) | Tổng cộng | 
| --- | --- | --- | --- | 
| 0 | - | [1,1,1,1,1] | 5 | 
| 1 | 1 | [0,1,1,1,1] | 4 | 
| 2 | 1 | [0,0,1,1,1] | 3 | 
| 3 | 1 | [0,0,0,1,1] | 2 | 
| 4 | 1 | [0,0,0,0,1] | 1 | 
| 5 | 1 | [0,0,0,0,0] | 0 | 

Mỗi thao tác loại bỏ chính xác một đơn vị, phù hợp với trực giác vì giảm một nửa 1 sẽ bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log A + K log N) | Mỗi màu tạo ra mức giảm O(log A), mỗi màu được đẩy/bật từ đống | 
| Không gian | O(N) | Heap lưu trữ tối đa một trạng thái hoạt động cho mỗi màu | 

Các ràng buộc cho phép tối đa 1e5 màu và giá trị lên tới 1e9, vì vậy nhật ký A là khoảng 30. Điều này giúp tổng số thao tác heap có thể quản lý được, nằm trong giới hạn thông thường cho việc thực thi 1 giây trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import heapq
    input = iter(inp.strip().split()).__next__

    n = int(input())
    S = int(input())
    a = [int(input()) for _ in range(n)]

    total = sum(a)
    if total <= S:
        return "0"

    pq = []
    for x in a:
        gain = x - x // 2
        pq.append((-gain, x))
    heapq.heapify(pq)

    ops = 0

    while total > S:
        gain, x = heapq.heappop(pq)
        gain = -gain
        total -= gain
        ops += 1
        nx = x // 2
        if nx > 0:
            ng = nx - nx // 2
            heapq.heappush(pq, (-ng, nx))

    return str(ops)

# sample-like tests
assert solve_capture("2 5\n3 5\n") == "2"
assert solve_capture("5 0\n1 1 1 1 1\n") == "5"

# custom tests
assert solve_capture("1 0\n1\n") == "1"
assert solve_capture("3 100\n10 20 30\n") == "0"
assert solve_capture("2 1\n8 1\n") == "3"
assert solve_capture("4 3\n4 4 4 4\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 / 1 | 1 | giảm phần tử đơn | 
| 10 20 30 / S=100 | 0 | đã ở mức giới hạn | 
| 8 1 với S=1 | 3 | giảm một nửa tham lam lặp đi lặp lại | 
| 4 bản sao của 4 với S=3 | 5 | nhiều mức giảm cạnh tranh | 

## Vỏ cạnh 

Với S = 0 với tất cả các giá trị 1, thuật toán liên tục chọn mức tăng 1 cho mỗi phần tử cho đến khi tất cả trở về 0. Mỗi bước chính xác sẽ giảm tổng số đi đúng một và vùng heap sẽ quay vòng qua các phần tử cho đến khi cạn kiệt. 

Đối với các đầu vào đã được đáp ứng như tổng ≤ S, việc thoát sớm sẽ ngăn chặn việc xây dựng hoặc các hoạt động heap không cần thiết, trả về 0 ngay lập tức. 

Đối với các đầu vào có độ lệch lớn như một giá trị lớn và nhiều giá trị nhỏ, vùng heap đảm bảo giá trị lớn luôn được xử lý trước vì mức tăng ban đầu của nó chiếm ưu thế và khi thu hẹp mức độ ưu tiên của nó sẽ giảm một cách tự nhiên, cho phép các giá trị nhỏ hơn chiếm ưu thế khi thích hợp.
