---
title: "CF 44E - Khỉ Anfisa"
description: "Chúng ta được cung cấp một chuỗi các chữ cái viết thường không chứa dấu cách. Nhiệm vụ là chia chuỗi này thành đúng k phần liên tiếp. Mỗi mảnh phải có độ dài từ a đến b, bao gồm cả hai phần. Thứ tự các ký tự không thể thay đổi. Chúng tôi chỉ đang quyết định nơi để cắt dây."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 44
codeforces_index: "E"
codeforces_contest_name: "School Team Contest 2 (Winter Computer School 2010/11)"
rating: 1400
weight: 44
solve_time_s: 133
verified: false
draft: false
---

[CF 44E - Khỉ Anfisa](https://codeforces.com/problemset/problem/44/E) 

**Đánh giá:** 1400 
**Thẻ:** dp 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ cái viết thường không chứa dấu cách. Nhiệm vụ là chia chuỗi này thành chính xác`k`miếng liên tiếp. Mỗi mảnh phải có chiều dài giữa`a`Và`b`, bao gồm. 

Thứ tự các ký tự không thể thay đổi. Chúng tôi chỉ đang quyết định nơi để cắt dây. 

Giả sử đầu vào là:```
k = 3
a = 2
b = 5
s = "abrakadabra"
```Một phép chia hợp lệ là:```
ab
rakad
abra
```bởi vì độ dài là`2`,`5`, Và`4`, tất cả đều nằm trong phạm vi cho phép. 

Những hạn chế là nhỏ. Độ dài chuỗi tối đa là`200`, Và`k`,`a`, Và`b`nhiều nhất cũng có`200`. Điều này ngay lập tức cho chúng ta biết rằng ngay cả các giải pháp quy hoạch động bậc hai hoặc bậc ba cũng hoàn toàn an toàn. Việc tìm kiếm mạnh mẽ trên tất cả các phân vùng có thể vẫn sẽ nguy hiểm vì số cách đặt các vết cắt tăng theo cấp số nhân. Ví dụ, ngay cả một chuỗi có độ dài`200`có một số lượng lớn các phân đoạn có thể. 

Quan sát quan trọng là chúng ta chỉ quan tâm đến việc liệu tiền tố của chuỗi có thể được chia thành một số phần hợp lệ hay không. Cấu trúc đó tự nhiên dẫn đến lập trình động. 

Có một số trường hợp đặc biệt có thể âm thầm phá vỡ việc triển khai không chính xác. 

Một lỗi phổ biến là quên rằng phải sử dụng toàn bộ chuỗi. Coi như:```
2 2 3
abcde
```Độ dài chuỗi là`5`. Sự phân chia hợp lệ tồn tại:```
ab
cde
```Một cách tiếp cận tham lam bất cẩn có thể chiếm được phân khúc đầu tiên lớn nhất có thể`"abc"`rồi thất bại vì`"de"`có chiều dài`2`, thực tế vẫn hoạt động ở đây, nhưng các đầu vào tương tự có thể không thành công nếu phần còn lại quá ngắn. 

Một trường hợp phức tạp khác là khi không có nghiệm nào tồn tại vì tổng chiều dài không thể phân phối được.```
3 2 2
abcde
```Chúng ta cần chính xác ba đoạn có độ dài chính xác`2`, vậy tổng độ dài cần tìm là`6`, nhưng độ dài chuỗi là`5`. Đầu ra đúng là:```
No solution
```Việc triển khai chỉ kiểm tra độ dài phân đoạn cục bộ mà không xác minh tổng thể toàn cầu có thể in không chính xác một phần kết quả. 

Trường hợp tinh tế thứ ba xuất hiện khi`a = b`. Sau đó, mỗi đoạn có độ dài cố định.```
2 3 3
abcdef
```Câu trả lời hợp lệ duy nhất là:```
abc
def
```Điều này hoạt động giống như phân vùng chính xác hơn là phân chia linh hoạt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực cố gắng bằng mọi cách có thể để đặt`k - 1`cắt bên trong sợi dây. Đối với mỗi phân vùng, chúng tôi kiểm tra xem tất cả độ dài phân đoạn có nằm giữa`a`Và`b`. 

Điều này có tác dụng vì mọi câu trả lời hợp lệ đều tương ứng với một số vị trí cắt. Vấn đề là số lượng khả năng. Một chuỗi có độ dài`n`có`n - 1`vị trí cắt có thể, và chúng tôi đang chọn`k - 1`của họ. Số lượng phân vùng trở thành tổ hợp:$$\binom{n-1}{k-1}$$Với`n = 200`, điều này hoàn toàn không thể thực hiện được. 

Quan sát cấu trúc quan trọng là tính hợp lệ của phép phân tách chỉ phụ thuộc vào tiền tố. Nếu chúng ta đã biết liệu lần đầu tiên`i`ký tự có thể được chia thành`j`phân đoạn hợp lệ thì chúng ta có thể thử mở rộng trạng thái đó bằng một phân đoạn nữa. 

Điều này tự nhiên tạo thành một quá trình chuyển đổi lập trình năng động. 

Định nghĩa:$$dp[i][j]$$như thể là lần đầu tiên`i`các ký tự có thể được chia thành chính xác`j`các phần hợp lệ. 

Từ một tiểu bang`(i, j)`, chúng tôi thử lấy độ dài đoạn tiếp theo`len`giữa`a`Và`b`. Nếu như`i + len`không vượt quá độ dài chuỗi thì:$$dp[i + len][j + 1] = True$$Điều này làm giảm vấn đề từ tìm kiếm theo cấp số nhân xuống DP thời gian đa thức nhỏ. Vì mọi giới hạn đều lớn nhất`200`, ngay cả việc triển khai đơn giản cũng đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(k) | Quá chậm | 
| DP tối ưu | O(n × k × (b - a + 1)) | O(n × k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các giá trị`k`,`a`,`b`và chuỗi`s`. 
2. Hãy để`n`là độ dài của chuỗi. 
3. Tạo bảng DP trong đó`dp[i][j]`cho chúng ta biết liệu lần đầu tiên`i`các ký tự có thể được chia thành chính xác`j`các phân đoạn hợp lệ. 
4. Khởi tạo`dp[0][0] = True`. 

Điều này có nghĩa là một tiền tố trống có thể được chia thành các phần bằng 0. 
5. Đối với mọi vị trí`i`từ`0`ĐẾN`n`: 

Đối với mỗi số lượng phân đoạn`j`từ`0`ĐẾN`k - 1`:

Nếu như`dp[i][j]`là đúng, hãy thử từng độ dài đoạn`len`từ`a`ĐẾN`b`. 
6. Nếu`i + len <= n`, thì chúng ta có thể mở rộng phân vùng hiện tại thêm một phân đoạn hợp lệ. 

Đánh dấu:$$dp[i + len][j + 1] = True$$7. Để xây dựng lại câu trả lời, hãy lưu trữ trạng thái trước đó bất cứ khi nào quá trình chuyển đổi thành công. 

Ví dụ: lưu:```
parent[next_i][next_j] = (i, j)
```8. Sau khi điền vào bảng, kiểm tra`dp[n][k]`. 

Nếu sai thì in ra`"No solution"`. 
9. Ngược lại, quay lui bằng cách sử dụng con trỏ cha. 

Bắt đầu từ`(n, k)`, liên tục khôi phục trạng thái trước đó và trích xuất chuỗi con tương ứng. 
10. Đảo ngược các phân đoạn đã thu thập vì quá trình tái tạo diễn ra ngược. 
11. In từng đoạn một. 

### Tại sao nó hoạt động 

Bất biến DP là:`dp[i][j]`đúng khi và chỉ nếu điều đầu tiên`i`các ký tự có thể được phân chia thành chính xác`j`các phân đoạn hợp lệ. 

Trường hợp cơ bản là đúng vì một chuỗi trống yêu cầu các phân đoạn bằng 0. 

Mỗi quá trình chuyển đổi duy trì tính chính xác vì chúng tôi chỉ nối thêm các phân đoạn có độ dài nằm trong khoảng`a`Và`b`. Mọi trạng thái có thể truy cập đều tương ứng với một phân vùng hợp lệ của tiền tố. 

Việc xây dựng lại là hợp lệ vì mọi con trỏ cha được lưu trữ đều ghi lại quá trình chuyển đổi thực tế được sử dụng để xây dựng trạng thái DP. 

Vì mọi độ dài phân đoạn hợp lệ có thể được khám phá từ mọi trạng thái có thể truy cập nên không thể bỏ sót giải pháp hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k, a, b = map(int, input().split())
    s = input().strip()

    n = len(s)

    dp = [[False] * (k + 1) for _ in range(n + 1)]
    parent = [[None] * (k + 1) for _ in range(n + 1)]

    dp[0][0] = True

    for i in range(n + 1):
        for j in range(k):
            if not dp[i][j]:
                continue

            for length in range(a, b + 1):
                ni = i + length

                if ni > n:
                    continue

                if not dp[ni][j + 1]:
                    dp[ni][j + 1] = True
                    parent[ni][j + 1] = (i, j)

    if not dp[n][k]:
        print("No solution")
        return

    parts = []

    cur_i = n
    cur_j = k

    while cur_j > 0:
        prev_i, prev_j = parent[cur_i][cur_j]

        parts.append(s[prev_i:cur_i])

        cur_i = prev_i
        cur_j = prev_j

    parts.reverse()

    print("\n".join(parts))

solve()
```Giải pháp bắt đầu bằng cách tạo một bảng DP boolean. Các kích thước là`(n + 1) × (k + 1)`bởi vì chúng tôi theo dõi mọi độ dài tiền tố có thể có và mọi số lượng phân đoạn được xây dựng có thể có. 

Các vòng lặp lồng nhau lặp lại trên tất cả các trạng thái có thể truy cập. Bất cứ khi nào`dp[i][j]`là đúng, chúng tôi cố gắng nối thêm một đoạn khác có độ dài nằm giữa`a`Và`b`. 

Dòng:```
ni = i + length
```tính toán kích thước tiền tố mới sau khi lấy phân đoạn tiếp theo. 

Điều kiện:```
if ni > n:
```ngăn chặn việc truy cập các ký tự ngoài cuối chuỗi. Việc kiểm tra ranh giới này là cần thiết vì phân đoạn cuối cùng có thể tràn chuỗi. 

Bảng cha chỉ lưu trữ thông tin tái cấu trúc khi có thể truy cập được trạng thái lần đầu tiên. Mọi sự tái tạo hợp lệ đều được chấp nhận, vì vậy chúng ta không cần ghi đè lên cha mẹ trước đó. 

Trong quá trình xây dựng lại, chuỗi con:```
s[prev_i:cur_i]
```chính xác là phân khúc được thêm vào trong quá trình chuyển đổi từ`(prev_i, prev_j)`ĐẾN`(cur_i, cur_j)`. 

Các phân đoạn được thu thập xuất hiện theo thứ tự ngược lại vì quá trình quay lui bắt đầu từ chuỗi đầy đủ và đi lùi về phía tiền tố trống. Đảo ngược danh sách sẽ khôi phục lại thứ tự đúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2 5
abrakadabra
```Độ dài chuỗi là`11`. 

| Hiện tại tôi | Hiện tại j | Đã thử chiều dài | Trạng thái tiếp theo | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | (2,1) | Có | 
| 0 | 0 | 3 | (3,1) | Có | 
| 0 | 0 | 4 | (4,1) | Có | 
| 0 | 0 | 5 | (5,1) | Có | 
| 2 | 1 | 5 | (7,2) | Có | 
| 7 | 2 | 4 | (11,3) | Có | 

Một đường dẫn được xây dựng lại là:```
(0,0) -> (2,1) -> (7,2) -> (11,3)
```Các phân đoạn kết quả là:```
ab
rakad
abra
```Dấu vết này cho thấy cách DP khám phá nhiều độ dài phân đoạn có thể có trong khi vẫn giữ nguyên bất biến rằng mọi trạng thái có thể truy cập đều tương ứng với một phân vùng hợp lệ. 

### Ví dụ 2 

đầu vào:```
3 2 2
abcde
```Độ dài chuỗi là`5`. 

| Hiện tại tôi | Hiện tại j | Đã thử chiều dài | Trạng thái tiếp theo | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | (2,1) | Có | 
| 2 | 1 | 2 | (4,2) | Có | 
| 4 | 2 | 2 | (6,3) | Không | 

DP không bao giờ đạt được`(5,3)`. 

Đầu ra đúng là:```
No solution
```Ví dụ này cho thấy tại sao giá trị cục bộ là không đủ. Mặc dù mỗi độ dài phân đoạn riêng lẻ đều được phép nhưng tổng độ dài không thể được phân phối chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n × k × (b - a + 1)) | Mọi trạng thái DP đều thử tất cả độ dài phân đoạn | 
| Không gian | O(n × k) | Bảng DP và bảng cha | 

Vì tất cả các giá trị lớn nhất`200`, số lần chuyển tiếp tối đa là khoảng:$$200 \times 200 \times 200 = 8 \times 10^6$$dễ dàng phù hợp với giới hạn thời gian trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    k, a, b = map(int, input().split())
    s = input().strip()

    n = len(s)

    dp = [[False] * (k + 1) for _ in range(n + 1)]
    parent = [[None] * (k + 1) for _ in range(n + 1)]

    dp[0][0] = True

    for i in range(n + 1):
        for j in range(k):
            if not dp[i][j]:
                continue

            for length in range(a, b + 1):
                ni = i + length

                if ni > n:
                    continue

                if not dp[ni][j + 1]:
                    dp[ni][j + 1] = True
                    parent[ni][j + 1] = (i, j)

    if not dp[n][k]:
        print("No solution")
        return

    ans = []

    cur_i = n
    cur_j = k

    while cur_j > 0:
        prev_i, prev_j = parent[cur_i][cur_j]
        ans.append(s[prev_i:cur_i])

        cur_i = prev_i
        cur_j = prev_j

    ans.reverse()

    print("\n".join(ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup

    return out.getvalue().strip()

# provided sample
assert run("3 2 5\nabrakadabra\n") == "ab\nrakad\nabra", "sample 1"

# minimum size
assert run("1 1 1\na\n") == "a", "single character"

# impossible partition
assert run("3 2 2\nabcde\n") == "No solution", "impossible total length"

# exact fixed lengths
assert run("2 3 3\nabcdef\n") == "abc\ndef", "fixed partition sizes"

# boundary condition
assert run("2 1 4\nabcde\n") == "a\nbcde", "uses smallest and largest lengths"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / a`|`a`| Đầu vào nhỏ nhất có thể | 
|`3 2 2 / abcde`|`No solution`| Tổng chiều dài không thể | 
|`2 3 3 / abcdef`|`abc / def`| Phân khúc có kích thước cố định chính xác | 
|`2 1 4 / abcde`|`a / bcde`| độ dài ranh giới`a`Và`b`| 

## Vỏ cạnh 

Hãy xem xét trường hợp tổng chiều dài không thể:```
3 2 2
abcde
```Mỗi đoạn phải có độ dài chính xác`2`, vậy ba đoạn cần có độ dài`6`. DP đạt đến các tiểu bang`(2,1)`Và`(4,2)`, nhưng không thể mở rộng thêm vì việc thêm một đoạn khác sẽ vượt quá độ dài chuỗi. Từ`dp[5][3]`vẫn sai, thuật toán sẽ in chính xác:```
No solution
```Bây giờ hãy xem xét kịch bản có độ dài cố định:```
2 3 3
abcdef
```Kích thước phân khúc được phép duy nhất là`3`. Sự chuyển đổi trở nên quyết định:```
(0,0) -> (3,1) -> (6,2)
```Câu trả lời được xây dựng lại là:```
abc
def
```Thuật toán xử lý việc này một cách tự nhiên vì vòng lặp bên trong theo độ dài đoạn chỉ chứa một giá trị. 

Cuối cùng, hãy xem xét trường hợp các lựa chọn tham lam có thể thất bại:```
2 2 4
abcdefg
```Nếu chúng ta tham lam chiếm lấy đoạn đầu tiên lớn nhất`"abcd"`chiều dài`4`, phần còn lại`"efg"`có chiều dài`3`, vẫn hoạt động. Nhưng với những đầu vào tương tự, những quyết định tham lam có thể khiến chúng ta mắc bẫy. 

DP hoàn toàn tránh được vấn đề này vì nó khám phá mọi độ dài phân đoạn hợp lệ từ mọi trạng thái có thể truy cập. Nếu có bất kỳ phân vùng hợp lệ nào tồn tại, một số đường dẫn DP sẽ đạt tới`(n,k)`.
