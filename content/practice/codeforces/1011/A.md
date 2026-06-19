---
title: "CF 1011A - Giai đoạn"
description: "Chúng ta có nhiều bộ ký tự, mỗi ký tự đại diện cho một giai đoạn tên lửa với giá trị nội tại bằng với vị trí của nó trong bảng chữ cái."
date: "2026-06-16T22:42:24+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1011
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 499 (Div. 2)"
rating: 900
weight: 1011
solve_time_s: 103
verified: true
draft: false
---

[CF 1011A - Các giai đoạn](https://codeforces.com/problemset/problem/1011/A) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có nhiều bộ ký tự, mỗi ký tự đại diện cho một giai đoạn tên lửa với giá trị nội tại bằng với vị trí của nó trong bảng chữ cái. Từ nhiều tập hợp này, chúng ta phải chọn chính xác k giai đoạn riêng biệt và sắp xếp chúng theo thứ tự tôn trọng quy tắc tương thích nghiêm ngặt: khi giai đoạn có chữ x được theo sau bởi giai đoạn có chữ y, chữ y phải ở sau ít nhất hai vị trí trong bảng chữ cái so với x. Nói cách khác, nếu x là chữ cái thứ i thì y ít nhất phải bằng i+2 hoặc lớn hơn. 

Nhiệm vụ là chọn k chữ cái từ chuỗi đã cho và sắp xếp chúng sao cho ràng buộc này đúng với mọi cặp liền kề, đồng thời giảm thiểu tổng bảng chữ cái của các chữ cái đã chọn. Nếu không thể chọn được k chữ cái thỏa mãn ràng buộc về thứ tự thì đáp án là -1. 

Về nguyên tắc, các ràng buộc n ≤ 50 khiến cho việc áp đặt mạnh mẽ lên các tập hợp con trở nên hợp lý. Có tối đa 2^50 tập hợp con, vốn đã quá lớn nhưng hạn chế về thứ tự bổ sung khiến hầu hết các tập hợp con không hợp lệ. Tuy nhiên, ngay cả việc lọc các tập hợp con vẫn sẽ quá tốn kém nếu được thực hiện một cách ngây thơ. Một quan sát quan trọng hơn là cấu trúc của ràng buộc chỉ phụ thuộc vào thứ tự sắp xếp của các chữ cái đã chọn, điều này làm giảm vấn đề chọn một dãy con hợp lệ thay vì một hoán vị tùy ý. 

Trường hợp cạnh phím là khi tất cả các chữ cái được chọn nằm gần trong bảng chữ cái. Ví dụ: nếu chúng ta buộc phải chọn các chữ cái như 'b' và 'c', điều kiện không thành công vì chúng chỉ khác nhau một vị trí. Một trường hợp khác là khi đầu vào có nhiều chữ cái nhỏ nhưng k lớn, khiến không thể duy trì quy tắc khoảng cách mặc dù có đủ ký tự. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi tập con có kích thước k, sắp xếp nó và kiểm tra xem nó có thỏa mãn quy tắc giãn cách hay không. Điều này đúng vì mọi giải pháp hợp lệ đều tương ứng với một tập hợp con nào đó và mọi thứ tự của tập hợp con đó tuân theo ràng buộc cũng phải tăng nghiêm ngặt theo thứ tự bảng chữ cái. Tuy nhiên, số lượng tập hợp con là C(n, k), trong trường hợp xấu nhất là khoảng 2^50, vượt xa giới hạn khả thi. 

Quan sát quan trọng là khi chúng ta sắp xếp các ký tự đã chọn, thứ tự của chúng sẽ được cố định. Ràng buộc sau đó trở thành cục bộ: đối với bất kỳ chữ cái được chọn liền kề nào, giá trị số của chúng phải khác nhau ít nhất là 2. Điều này biến vấn đề thành việc chọn k phần tử từ một mảng được sắp xếp sao cho các phần tử được chọn liên tiếp có khoảng cách vừa đủ, đồng thời giảm thiểu tổng. 

Cấu trúc này gợi ý một chiến lược tham lam đối với các ký tự được sắp xếp: chúng ta muốn chọn các chữ cái nhỏ nhất có thể, nhưng chúng ta cũng phải tôn trọng giới hạn khoảng cách. Vì việc chọn một chữ cái nhỏ hơn không bao giờ ảnh hưởng đến tính khả thi trừ khi nó chặn các lần chọn trong tương lai, nên chúng ta có thể xử lý các chữ cái theo thứ tự được sắp xếp và quyết định xem nên lấy từng chữ cái hay bỏ qua nó tùy thuộc vào việc liệu nó có thể mở rộng một chuỗi có độ dài k hợp lệ hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(2^n · k) | O(k) | Quá chậm | 
| Quét tham lam trên các chữ cái được sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chuyển đổi mỗi ký tự thành trọng số số từ 1 đến 26, vì việc tối ưu hóa chỉ phụ thuộc vào các giá trị này. 
2. Sắp xếp mảng có trọng số theo thứ tự tăng dần. Điều này là hợp lý vì bất kỳ giải pháp tối ưu nào cũng có thể được sắp xếp lại theo thứ tự được sắp xếp mà không vi phạm ràng buộc và ràng buộc là đơn điệu đối với thứ tự. 
3. Duy trì trạng thái lập trình động trong đó chúng tôi theo dõi giá trị được chọn cuối cùng tối thiểu có thể có cho các chuỗi có độ dài khác nhau. Cụ thể, dp[j] đại diện cho giá trị tối thiểu có thể được chọn lần cuối khi chúng ta đã chọn j phần tử. 
4. Khởi tạo dp[0] = -infinity và tất cả các giá trị dp khác là vô cùng. Trường hợp cơ bản tương ứng với việc chưa chọn gì cả. 
5. Lặp lại từng trọng số đã được sắp xếp. Đối với mỗi trọng số x, hãy cập nhật mảng dp ngược từ k xuống 1. Với mỗi j, chúng tôi kiểm tra xem x có thể mở rộng một chuỗi có độ dài hợp lệ j-1 hay không, nghĩa là x ít nhất phải bằng dp[j-1] + 2. Nếu vậy, chúng tôi cập nhật dp[j] ở mức tối thiểu giữa giá trị hiện tại của nó và x. 
6. Sau khi xử lý tất cả các phần tử, nếu dp[k] vẫn là vô cùng thì không có lựa chọn hợp lệ nào tồn tại. Mặt khác, dp[k] là giá trị phần tử cuối cùng tối thiểu có thể có, nhưng điều chúng ta thực sự muốn là tổng của các phần tử được chọn. Để theo dõi tổng chính xác, chúng tôi duy trì dp là tổng tối thiểu thay vì giá trị cuối cùng. 
7. Do đó, hãy xác định lại dp[j] là tổng tối thiểu có thể đạt được bằng cách sử dụng j phần tử hợp lệ và cập nhật nó cho phù hợp: nếu x có thể mở rộng dp[j-1], hãy đặt dp[j] = min(dp[j], dp[j-1] + x). 

### Tại sao nó hoạt động 

Bất biến DP là dp[j] lưu trữ tổng tối thiểu có thể có của bất kỳ lựa chọn hợp lệ nào của j phần tử được xử lý cho đến nay, trong đó tính hợp lệ bao gồm giới hạn khoảng cách so với phần tử được chọn cuối cùng. Bởi vì chúng tôi xử lý các phần tử theo thứ tự tăng dần, bất kỳ chuỗi hợp lệ nào kết thúc tại x chỉ có thể được cải thiện bởi các ứng cử viên sớm hơn hoặc bằng nhau. Quá trình chuyển đổi xem xét tất cả các cách để nối x vào một chuỗi có độ dài (j-1) hợp lệ và lấy mức tối ưu duy trì tối thiểu. Vì mọi giải pháp hợp lệ đều có thể được xây dựng tăng dần theo thứ tự được sắp xếp nên không có giải pháp tối ưu nào bị loại trừ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    s = input().strip()

    a = sorted(ord(c) - ord('a') + 1 for c in s)

    INF = 10**18
    dp = [INF] * (k + 1)
    dp[0] = 0

    for x in a:
        for j in range(k, 0, -1):
            if dp[j - 1] != INF:
                # enforce spacing constraint
                if j == 1 or True:
                    # for j > 1 we still need to ensure gap constraint
                    # but since we are not tracking last value, we must encode differently
                    pass
        # corrected DP below
        pass

    # correct solution: need state with last value tracking
    dp = [[INF] * 27 for _ in range(k + 1)]
    for i in range(27):
        dp[0][i] = 0

    for x in a:
        for j in range(k, 0, -1):
            for last in range(27):
                if dp[j - 1][last] != INF:
                    if j == 1 or x >= last + 2:
                        dp[j][x] = min(dp[j][x], dp[j - 1][last] + x)

    ans = min(dp[k])
    print(-1 if ans == INF else ans)

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng DP hai chiều trong đó chiều thứ hai theo dõi giá trị ký tự được chọn cuối cùng. Điều này là cần thiết vì giới hạn khoảng cách phụ thuộc vào độ kề nhau chứ không chỉ vào kích thước vùng chọn. Mỗi dp[j][last] đại diện cho tổng tối thiểu của việc chọn j phần tử có giá trị được chọn cuối cùng bằng giá trị cuối cùng. Khi xử lý một ký tự x mới, chúng ta bắt đầu một chuỗi mới hoặc chỉ mở rộng chuỗi hiện có nếu x ít nhất là cuối cùng + 2. 

Việc lặp lại ngược lại trên j không cần thiết trong phiên bản cuối cùng vì các chuyển đổi chỉ phụ thuộc vào lớp dp[j-1] trước đó. Cấu trúc thực thi việc tăng độ dài chuỗi một cách tự nhiên trong khi vẫn duy trì các ràng buộc về tính khả thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
xyabd
```Trọng số được sắp xếp: a=1, b=2, d=4, x=24, y=25 

Chúng tôi theo dõi các chuyển đổi dp[j][last] theo khái niệm: 

| Bước | Đã chọn x | j=1 tốt nhất | j=2 tốt nhất | j=3 tốt nhất | 
| --- | --- | --- | --- | --- | 
| bắt đầu | - | (1,2,4,24,25) | thông tin | thông tin | 
| một | 1 | 1 | thông tin | thông tin | 
| b | 2 | 1 | 3 (a,b bị bỏ qua không hợp lệ) | thông tin | 
| d | 4 | 1 | 3 | 7 (đường dẫn a,d,x bắt đầu hình thành) | 
| x | 24 | 1 | 3 | 7 | 
| y | 25 | 1 | 3 | 7 | 

Bộ ba hợp lệ nhất là a-d-x có tổng bằng 29, xác nhận dp tìm thấy 29. 

### Ví dụ 2 (đã xây dựng) 

đầu vào:```
4 2
abcd
```Trọng lượng: 1,2,3,4 

Chúng ta cần hai phần tử có khoảng cách ≥ 2. 

| Bước | x | Các cặp hợp lệ được hình thành | Tổng tốt nhất | 
| --- | --- | --- | --- | 
| một | 1 | - | 1 | 
| b | 2 | - | 1 | 
| c | 3 | (a,c) | 4 | 
| d | 4 | (a,d),(b,d) không hợp lệ ngoại trừ (a,d) | 5 | 

Câu trả lời là 4 sử dụng (a,c). 

Dấu vết này cho thấy ràng buộc kề sẽ cắt bỏ các lượt chọn liên tiếp như thế nào nhưng cho phép bỏ qua một vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · k · 26) | DP trên k trạng thái và thứ nguyên giá trị cuối cùng lên tới 26 | 
| Không gian | O(k · 26) | lưu trữ bảng DP cho mọi độ dài và giá trị cuối cùng | 

Với n ≤ 50 và k ≤ 50, điều này diễn ra thoải mái trong giới hạn, vì tổng số thao tác nhiều nhất là vài chục nghìn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n, k = map(int, sys.stdin.readline().split())
    s = sys.stdin.readline().strip()
    a = sorted(ord(c) - 97 + 1 for c in s)

    INF = 10**18
    dp = [[INF] * 27 for _ in range(k + 1)]
    for i in range(27):
        dp[0][i] = 0

    for x in a:
        for j in range(k, 0, -1):
            for last in range(27):
                if dp[j - 1][last] != INF:
                    if j == 1 or x >= last + 2:
                        dp[j][x] = min(dp[j][x], dp[j - 1][last] + x)

    ans = min(dp[k])
    return str(-1 if ans == INF else ans)

# provided sample
assert run("5 3\nxyabd\n") == "29"

# minimum size impossible
assert run("2 2\nab\n") == "-1"

# simple valid spaced case
assert run("3 2\nace\n") == "4"

# all equal letters
assert run("5 3\naaaaa\n") == "-1"

# large spacing easy
assert run("4 2\nazzz\n") == "27"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 3, xyabd | 29 | tiêu chuẩn lựa chọn tối ưu | 
| 2 2, ab | -1 | không thể do liền kề | 
| 3 2, át | 4 | cặp cách nhau tốt nhất | 
| aaa | -1 | trùng lặp không thể tạo thành chuỗi hợp lệ | 
| a, z, z, z | 27 | tham lam bỏ qua xử lý | 

## Vỏ cạnh 

Một đoạn bảng chữ cái được xếp chặt chẽ như "ab" cho k=2 ngay lập tức vi phạm quy tắc giãn cách. DP từ chối chính xác mọi chuyển đổi từ 'a' sang 'b' vì b < a + 2, do đó dp[2] không bao giờ trở nên hữu hạn. 

Trường hợp có nhiều chữ cái nhỏ và một chữ cái lớn như "aaaaaz" với k=2 vẫn hoạt động vì lựa chọn tối ưu là (a, z). DP bắt đầu bằng a tại dp[1] và sau đó cho phép z mở rộng nó vì z ≥ a + 2 giữ nguyên. 

Khi tất cả các ký tự giống hệt nhau, mọi nỗ lực tạo thành một chuỗi có độ dài lớn hơn 1 đều không đạt được ràng buộc vì không có lựa chọn thứ hai nào có thể khác nhau ít nhất 2. DP chỉ giữ dp[1] hợp lệ và để lại các trạng thái cao hơn là vô hạn, tạo ra -1 theo yêu cầu.
