---
title: "CF 106188D - Đồng hồ điểm 12 giờ"
description: "Bob muốn mua một gói thẻ giao dịch với mức giá chính xác. Cửa hàng chỉ chấp nhận tiền xu đã được làm sạch và mỗi loại tiền xu có giá trị tính bằng xu và thời gian làm sạch. Anh ta có thể sử dụng bất kỳ số lượng xu nào của từng loại, nhưng mỗi đồng xu anh ta sử dụng phải hoàn thành việc dọn dẹp trước nửa đêm."
date: "2026-06-25T10:46:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106188
codeforces_index: "D"
codeforces_contest_name: "UTPC x WiCS Contest 11-12-2025"
rating: 0
weight: 106188
solve_time_s: 35
verified: true
draft: false
---

[CF 106188D - Đồng hồ điểm 12](https://codeforces.com/problemset/problem/106188/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bob muốn mua một gói thẻ giao dịch với mức giá chính xác. Cửa hàng chỉ chấp nhận tiền xu đã được làm sạch và mỗi loại tiền xu có giá trị tính bằng xu và thời gian làm sạch. Anh ta có thể sử dụng bất kỳ số lượng xu nào của từng loại, nhưng mỗi đồng xu anh ta sử dụng phải hoàn thành việc dọn dẹp trước nửa đêm. Mục tiêu là chọn số lượng xu nhỏ nhất có thể có giá trị cộng lại chính xác với giá gói trong khi tổng thời gian dọn dẹp vẫn nằm trong thời gian còn lại. 

Đầu vào mô tả các loại tiền có sẵn, giá mục tiêu và thời hạn. Đầu ra là số lượng xu được làm sạch tối thiểu cần thiết, hoặc`-1`nếu không có cách nào để đạt được mức giá chính xác kịp thời. 

Các ràng buộc đủ nhỏ để cho phép lập trình động. Số tiền mục tiêu`m`tối đa là 500 và thời hạn`k`cũng nhiều nhất là 500. Một nghiệm phụ thuộc vào mọi lượng có thể và mọi thời gian trôi qua có thể có thể phù hợp vì không gian trạng thái nhiều nhất là khoảng 250.000 trạng thái. Các phương pháp thử mọi sự kết hợp của các đồng tiền sẽ phát triển theo cấp số nhân và không thể thực hiện được ngay cả đối với những giới hạn nhỏ này. 

Các trường hợp chính xuất phát từ sự tương tác giữa giá trị, thời gian và số lượng xu. Một phương pháp chỉ tìm thấy ít đồng tiền nhất mà không tính đến thời gian có thể thất bại. Ví dụ:```
3 10 9
1 2 5
2 1 5
```Rẻ nhất tính theo số xu sẽ là hai đồng xu 5 xu, nhưng chúng mất 10 giây và trễ nửa đêm. Câu trả lời đúng là`4`, sử dụng một đồng xu 5 xu, hai đồng xu 2 xu và một đồng xu 1 xu. Một giải pháp kiếm tiền ngắn nhất bất cẩn sẽ tạo ra`2`. 

Một trường hợp khác là khi có thể có được giá trị chính xác nhưng chỉ sau khi vượt quá giới hạn thời gian.```
2 6 3
3 6
2 4
```Một đồng xu 6 cent đạt đến mức giá đó nhưng phải mất 4 giây. Đầu ra đúng là`-1`. Việc triển khai chỉ kiểm tra thứ nguyên giá trị sẽ chấp nhận nó một cách không chính xác. 

Trường hợp thứ ba là khi một đồng tiền có giá trị tương đương với một đồng tiền khác nhưng chậm hơn.```
2 10 5
5 5
1 6
```Đầu ra đúng là`2`, sử dụng hai đồng xu 5 xu nhanh. Một quá trình chuyển đổi chỉ lưu trữ thời gian tốt nhất hoặc chỉ số lượng xu tốt nhất cho mỗi giá trị có thể loại bỏ các trạng thái hữu ích nếu nó quên rằng cả hai thứ nguyên đều quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử mọi số lượng xu có thể có từ mọi loại và giữ các kết hợp có tổng giá trị chính xác là mục tiêu. Điều này đúng vì mọi giao dịch mua có thể đều được xem xét, nhưng số lượng kết hợp tăng lên nhanh chóng. Với số tiền không giới hạn, ngay cả mục tiêu 500 xu cũng có thể yêu cầu khám phá một số lượng lớn các chuỗi, khiến phương pháp này không thể sử dụng được. 

Một nỗ lực có cấu trúc hơn là sử dụng ý tưởng lập trình động thay đổi đồng xu cổ điển và lưu trữ số lượng xu tối thiểu cần thiết cho mỗi số tiền có thể tiếp cận. Điều này áp dụng cho việc đổi tiền thông thường, nhưng ở đây nó thiếu một thứ nguyên. Đạt được số lượng nhanh chóng là không đủ nếu thời gian làm sạch quá lớn. 

Quan sát quan trọng là cả số tiền và thời gian dọn dẹp đã trôi qua đều bị giới hạn bởi 500. Thay vì chỉ hỏi "số xu tối thiểu cho giá trị này là bao nhiêu?", chúng ta có thể hỏi "số xu tối thiểu cần thiết để đạt được giá trị này sau khi dành chính xác khoảng thời gian này là bao nhiêu?". Mỗi tiểu bang mô tả trực tiếp mọi thứ quan trọng. 

Nguồn cung tiền xu không giới hạn có nghĩa là mọi tiểu bang đều có thể chuyển đổi sang chính mình với thêm một loại tiền xu bất kỳ. Vì giá trị và thời gian chỉ tăng lên nên các trạng thái thư giãn lặp đi lặp lại theo thứ tự số lượng và thời gian tăng dần sẽ phát hiện ra tất cả các giao dịch mua hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Lập trình động tối ưu | O(nmk) | O(mk) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo bảng lập trình động trong đó`dp[value][time]`lưu trữ số lượng xu tối thiểu cần thiết để tạo chính xác`value`xu trong khi chi tiêu chính xác`time`giây. Khởi tạo mọi trạng thái là không thể truy cập được ngoại trừ`dp[0][0] = 0`, bởi vì việc thực hiện giao dịch mua hàng không tốn phí không cần phải có xu. 
2. Xử lý mọi trạng thái có thể truy cập và thử thêm từng đồng xu có sẵn. Nếu một đồng tiền có giá trị`w`và thời gian dọn dẹp`t`, sự chuyển tiếp từ`(value, time)`đạt tới`(value + w, time + t)`và tăng số lượng xu lên một. 
3. Bỏ qua các chuyển tiếp khi giá trị mới vượt quá`m`hoặc thời gian mới vượt quá`k`. Những trạng thái này không bao giờ có thể trở thành một phần của câu trả lời hợp lệ vì tiền xu trong tương lai chỉ có thể tăng cả hai giá trị. 
4. Sau khi tất cả quá trình chuyển đổi hoàn tất, hãy kiểm tra mọi trạng thái`dp[m][time]`vì`0 <= time <= k`. Số lượng xu được lưu trữ nhỏ nhất trong số đó là câu trả lời vì đây chính xác là những giao dịch mua đạt mức giá yêu cầu trước nửa đêm. 

Tại sao nó hoạt động: Mỗi giao dịch mua hợp lệ là một chuỗi bổ sung xu. Bảng lập trình động chứa trạng thái sau mỗi tiền tố có thể có của chuỗi đó. Khi đồng xu cuối cùng được thêm vào, quá trình chuyển đổi sẽ đạt đến trạng thái chính xác đại diện cho toàn bộ giao dịch mua. Vì mỗi lần chuyển đổi sẽ thêm một xu và lấy mức tối thiểu trong số tất cả các cách để đạt đến trạng thái, nên mỗi trạng thái sẽ lưu trữ ít xu nhất trong số tất cả các giao dịch mua với giá trị và thời gian chính xác đó. Kiểm tra tất cả thời gian hoàn thành được phép sau đó đưa ra ít xu nhất trong số tất cả các giao dịch mua hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    w = list(map(int, input().split()))
    t = list(map(int, input().split()))

    INF = 10**9
    dp = [[INF] * (k + 1) for _ in range(m + 1)]
    dp[0][0] = 0

    for value in range(m + 1):
        for time in range(k + 1):
            if dp[value][time] == INF:
                continue
            current = dp[value][time]
            for coin_value, coin_time in zip(w, t):
                nv = value + coin_value
                nt = time + coin_time
                if nv <= m and nt <= k:
                    if dp[nv][nt] > current + 1:
                        dp[nv][nt] = current + 1

    answer = min(dp[m])
    print(-1 if answer == INF else answer)

if __name__ == "__main__":
    solve()
```Bảng được xây dựng với các hàng thể hiện số tiền đã thu được và các cột thể hiện thời gian dọn dẹp đã sử dụng. Trạng thái ban đầu`(0, 0)`là trạng thái duy nhất tồn tại trước khi làm sạch bất kỳ đồng xu nào. 

Các vòng lặp lồng nhau quét mọi trạng thái có thể truy cập. Từ mỗi tiểu bang, mọi loại tiền đều được coi là đồng tiền tiếp theo cần được làm sạch. Bởi vì số tiền là không giới hạn nên việc chuyển đổi tương tự được phép lặp đi lặp lại. Việc kiểm tra giới hạn ngăn chặn việc lưu trữ các trạng thái không thể dẫn đến câu trả lời. 

trận chung kết`min(dp[m])`tìm kiếm toàn bộ hàng giá mục tiêu. Các thời điểm khác nhau có thể tạo ra cùng một mức giá và chỉ những giá trong thời hạn mới được lưu trữ, do đó không cần lọc bổ sung. 

Số nguyên Python không tràn ở đây vì câu trả lời tối đa bị giới hạn bởi số tiền mục tiêu và giá trị vô cực được chọn lớn hơn nhiều so với bất kỳ số xu nào có thể có. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 10 10
1 2 5
2 4 5
```Các trạng thái quan trọng phát triển như sau. 

| Bước | Đã thêm tiền | Giá trị | Thời gian | Tiền xu được sử dụng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | không | 0 | 0 | 0 | 
| 1 | Đồng xu 5 xu | 5 | 5 | 1 | 
| 2 | Đồng xu 5 xu | 10 | 10 | 2 | 

Câu trả lời là`2`. Dấu vết cho thấy có thể tìm thấy số lượng xu tối thiểu trong khi vẫn tôn trọng thời hạn. 

Đối với mẫu thứ hai:```
3 10 9
1 2 5
2 1 5
```| Bước | Đã thêm tiền | Giá trị | Thời gian | Tiền xu được sử dụng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | không | 0 | 0 | 0 | 
| 1 | Đồng xu 5 xu | 5 | 5 | 1 | 
| 2 | Đồng xu 5 xu | 10 | 10 | 2 | 
| Từ chối | hai đồng xu 5 xu | 10 | 10 | 2 | 
| 2 | đồng xu 2 xu | 7 | 6 | 2 | 
| 3 | đồng xu 2 xu | 9 | 7 | 3 | 
| 4 | Đồng xu 1 xu | 10 | 9 | 4 | 

Giải pháp hai xu 5 xu đạt giá nhưng nằm ngoài thời hạn. Bảng không đưa ra câu trả lời vì các trạng thái có thời gian lớn hơn`k`bị loại bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nmk) | có`(m + 1) * (k + 1)`tiểu bang và mọi tiểu bang đều thử tất cả`n`các loại tiền xu. | 
| Không gian | O(mk) | Bảng lập trình động lưu trữ một giá trị cho mỗi cặp số lượng và thời gian. | 

Bảng lớn nhất có thể có 501 x 501 mục, tức là khoảng 250.000 số nguyên. Con số này đủ nhỏ cho giới hạn bộ nhớ và số lần chuyển đổi nhiều nhất là khoảng 125 triệu thao tác đơn giản. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# provided samples
assert run("""3 10 10
1 2 5
2 4 5
""") == "2\n", "sample 1"

assert run("""3 10 9
1 2 5
2 1 5
""") == "4\n", "sample 2"

# minimum size
assert run("""1 1 1
1
1
""") == "1\n", "single coin"

# impossible target
assert run("""2 7 10
2 4
1 1
""") == "-1\n", "unreachable value"

# all equal values with different times
assert run("""3 10 5
5 5 5
3 2 4
""") == "2\n", "choose fastest equal coins"

# boundary deadline
assert run("""2 6 3
3 6
2 4
""") == "-1\n", "coin finishes after midnight"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 1 / 1`|`1`| Đầu vào kích thước tối thiểu và chuyển đổi trực tiếp | 
|`2 7 10 / 2 4 / 1 1`|`-1`| Giá trị chính xác không thể | 
|`3 10 5 / 5 5 5 / 3 2 4`|`2`| Giá trị bằng nhau với thời gian làm sạch khác nhau | 
|`2 6 3 / 3 6 / 2 4`|`-1`| Xử lý ranh giới thời hạn | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là sự khác biệt giữa giảm thiểu tiền xu và giảm thiểu tiền xu theo thời hạn.```
3 10 9
1 2 5
2 1 5
```Thuật toán tạo trạng thái cho hai đồng xu 5 xu có giá trị`10`và thời gian`10`, nhưng trạng thái này không bao giờ được lưu trữ vì`10 > k`. Các trạng thái hợp lệ còn lại bao gồm giải pháp bốn xu tại thời điểm đó`9`, vì vậy câu trả lời trở thành`4`. 

Trường hợp cạnh thứ hai là một giá trị chính xác không thể đạt được trước nửa đêm.```
2 6 3
3 6
2 4
```Đồng xu 6 xu duy nhất tạo ra giá trị`6`, nhưng quá trình chuyển đổi cần có thời gian`4`. Vì thời hạn là`3`, trạng thái bị từ chối. Không có sự kết hợp nào khác đạt được giá trị`6`, do đó hàng cuối cùng không có trạng thái có thể truy cập được và kết quả đầu ra của thuật toán`-1`. 

Trường hợp thứ ba là lựa chọn giữa các đồng tiền có giá trị giống hệt nhau.```
3 10 5
5 5 5
3 2 4
```Các trạng thái lập trình động giữ nguyên chiều thời gian, do đó hai đồng xu 5 xu nhanh tạo ra`(10, 4)`trong khi những lựa chọn chậm hơn tạo ra những trạng thái có thời gian lớn hơn. Lần quét cuối cùng của hàng mục tiêu sẽ chọn số lượng xu tối thiểu chỉ trong số những lần hợp lệ và trả về`2`.
