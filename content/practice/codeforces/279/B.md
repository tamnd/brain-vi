---
title: "CF 279B - Sách"
description: "Chúng ta có một hàng sách và biết được thời gian cần thiết để đọc mỗi cuốn sách. Valera có thể bắt đầu từ bất kỳ vị trí nào và sau đó đọc liên tiếp các cuốn sách ở bên phải. Anh ta không thể bỏ qua cuốn sách và anh ta chỉ bắt đầu đọc một cuốn sách nếu còn đủ thời gian để hoàn thành nó."
date: "2026-06-05T08:34:56+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 279
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 171 (Div. 2)"
rating: 1400
weight: 279
solve_time_s: 98
verified: true
draft: false
---

[CF 279B - Sách](https://codeforces.com/problemset/problem/279/B) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, vũ phu, triển khai, hai con trỏ 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng sách và biết được thời gian cần thiết để đọc mỗi cuốn sách. Valera có thể bắt đầu từ bất kỳ vị trí nào và sau đó đọc liên tiếp các cuốn sách ở bên phải. Anh ta không thể bỏ qua cuốn sách và anh ta chỉ bắt đầu đọc một cuốn sách nếu còn đủ thời gian để hoàn thành nó. 

Nhiệm vụ là tìm số lượng sách liên tiếp lớn nhất mà tổng thời gian đọc không vượt quá thời gian cho phép.`t`. 

The input gives the reading times of all books. Đầu ra là một số duy nhất, độ dài tối đa của một đoạn liền kề có tổng tối đa là`t`. 

Những hạn chế là những gì thúc đẩy giải pháp. Số lượng sách có thể đạt`100000`, do đó, bất kỳ thuật toán nào kiểm tra tất cả các phân đoạn có thể một cách rõ ràng sẽ quá chậm. Có khoảng`n² / 2`các phân đoạn liền kề, tức là khoảng năm tỷ khi`n = 100000`. Ngay cả việc tính toán tổng phân khúc một cách hiệu quả với tổng tiền tố vẫn sẽ để lại một`O(n²)`liệt kê, vượt xa những gì phù hợp với giới hạn 2 giây. 

Cần có một giải pháp tuyến tính hoặc gần tuyến tính. Vì tất cả thời gian đọc đều dương nên bài toán có cấu trúc đơn điệu hữu ích cho phép tiếp cận theo cửa sổ trượt. 

Một trường hợp dễ xảy ra là khi chỉ một cuốn sách cũng vượt quá thời gian có sẵn. 

đầu vào:```
3 2
3 4 5
```Đầu ra:```
0
```Không có phân đoạn hợp lệ tồn tại. Việc triển khai bất cẩn luôn cho rằng có thể đọc được ít nhất một cuốn sách sẽ trả về`1`. 

Một trường hợp cạnh khác xảy ra khi đoạn tối ưu bắt đầu ở giữa. 

đầu vào:```
5 5
10 1 1 1 1
```Đầu ra:```
4
```Bắt đầu từ cuốn thứ nhất là không thể, nhưng bắt đầu từ cuốn thứ hai thì có thể đọc được bốn cuốn. Những cách tiếp cận tham lam chỉ xem xét tiền tố sẽ thất bại. 

Trường hợp thứ ba là khi cửa sổ liên tục cần thu nhỏ lại. 

đầu vào:```
5 5
2 2 2 2 2
```Đầu ra:```
2
```Mọi đoạn có độ dài ba đều có tổng sáu. Việc triển khai không xóa sách ở phía bên trái một cách không chính xác khi số tiền quá lớn sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là thử mọi vị trí bắt đầu và kéo dài sang bên phải cho đến khi tổng thời gian đọc vượt quá`t`. Đối với mỗi cuốn sách bắt đầu, chúng tôi tiếp tục thêm từng cuốn sách một. 

Cách tiếp cận này đúng vì nó đánh giá rõ ràng mọi phân đoạn liền kề có thể. Vấn đề là thời gian chạy của nó. Trong trường hợp xấu nhất, chẳng hạn như khi tất cả thời gian đọc`1`, mỗi vị trí bắt đầu có thể kiểm tra hầu hết các cuốn sách còn lại. Tổng công việc trở thành:```
n + (n - 1) + (n - 2) + ... + 1 = O(n²)
```Với`n = 100000`, cái này quá lớn. 

Quan sát quan trọng là tất cả thời gian đọc đều tích cực. Giả sử chúng ta có một phân khúc có tổng đã vượt quá`t`. Mở rộng nó hơn nữa chỉ có thể làm tăng tổng chứ không bao giờ giảm được. Điều đó có nghĩa là ranh giới bên trái của đoạn chỉ di chuyển về phía trước, không bao giờ lùi lại. 

Thuộc tính này làm cho cửa sổ trượt có thể hoạt động được. Chúng tôi duy trì một cửa sổ`[left, right]`tổng của nó được theo dõi liên tục. Chúng tôi mở rộng cửa sổ bằng cách di chuyển`right`. Bất cứ khi nào số tiền trở nên quá lớn, chúng tôi di chuyển`left`chuyển tiếp cho đến khi tổng hợp lệ trở lại. 

Vì mỗi chỉ mục vào cửa sổ một lần và rời khỏi cửa sổ một lần nên tổng công việc là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Cửa Sổ Trượt Tối Ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`left = 0`,`current_sum = 0`, Và`answer = 0`. 
2. Lặp lại`right`từ`0`ĐẾN`n - 1`. 
3. Thêm thời gian đọc cuốn sách hiện tại vào`current_sum`. 
4. Nếu`current_sum`vượt quá`t`, liên tục di chuyển`left`chuyển tiếp và trừ những cuốn sách đó khỏi`current_sum`cho đến khi tổng đạt nhiều nhất`t`. 

Bước này khôi phục bất biến rằng cửa sổ hiện tại là một phân đoạn hợp lệ có tổng thời gian đọc phù hợp với thời gian sẵn có. 
5. Độ dài cửa sổ hiện tại là`right - left + 1`. 
6. Cập nhật`answer`với độ dài cửa sổ tối đa được thấy cho đến nay. 
7. Sau khi xử lý xong tất cả sách, hãy in`answer`. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, cửa sổ biểu thị phân đoạn hợp lệ dài nhất kết thúc tại vị trí`right`tổng của nó không vượt quá`t`. Khi tổng trở nên quá lớn, việc xóa sách ở bên trái là cách duy nhất để làm cho phân đoạn hợp lệ trở lại vì tất cả thời gian đọc đều dương. 

Từ`left`chỉ di chuyển về phía trước, mọi cửa sổ hợp lệ tối đa có thể được xem xét chính xác khi điểm cuối bên phải của nó được xử lý. Thuật toán không bao giờ bỏ lỡ phân đoạn ứng cử viên và`answer`ghi lại độ dài cửa sổ hợp lệ lớn nhất trong số chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, t = map(int, input().split())
    a = list(map(int, input().split()))

    left = 0
    current_sum = 0
    answer = 0

    for right in range(n):
        current_sum += a[right]

        while current_sum > t:
            current_sum -= a[left]
            left += 1

        answer = max(answer, right - left + 1)

    print(answer)

if __name__ == "__main__":
    solve()
```Biến`current_sum`lưu trữ tổng thời gian đọc của cửa sổ hiện tại. Mỗi khi ranh giới bên phải mở rộng, cuốn sách mới sẽ được thêm vào tổng. 

các`while`vòng lặp là phần quan trọng. Một lần loại bỏ không phải lúc nào cũng đủ. Ví dụ: nếu tổng cửa sổ lớn hơn nhiều so với`t`, có thể cần phải xóa một số sách trước khi phân đoạn trở lại hợp lệ. 

Độ dài cửa sổ được tính sau khi quá trình thu nhỏ kết thúc. Tại thời điểm đó, phân khúc được đảm bảo đáp ứng thời hạn. 

Số nguyên Python không bị giới hạn, do đó không có vấn đề tràn`t`có thể đạt được`10^9`và tổng cửa sổ có thể tạm thời trở nên lớn. 

Việc triển khai sử dụng các chỉ số dựa trên số 0 trong nội bộ, giữ nguyên công thức độ dài`right - left + 1`đơn giản. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5
3 1 2 1
```| đúng | giờ đặt sách | current_sum trước khi thu nhỏ | còn lại sau khi thu nhỏ | current_sum sau khi thu nhỏ | chiều dài cửa sổ | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 3 | 0 | 3 | 1 | 1 | 
| 1 | 1 | 4 | 0 | 4 | 2 | 2 | 
| 2 | 2 | 6 | 1 | 3 | 2 | 2 | 
| 3 | 1 | 4 | 1 | 4 | 3 | 3 | 

Câu trả lời cuối cùng:```
3
```Đoạn hợp lệ dài nhất là`[1, 2, 1]`, tổng thời gian đọc của nó là`4`. 

### Ví dụ 2 

đầu vào:```
5 5
2 2 2 2 2
```| đúng | giờ đặt sách | current_sum trước khi thu nhỏ | còn lại sau khi thu nhỏ | current_sum sau khi thu nhỏ | chiều dài cửa sổ | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | 0 | 2 | 1 | 1 | 
| 1 | 2 | 4 | 0 | 4 | 2 | 2 | 
| 2 | 2 | 6 | 1 | 4 | 2 | 2 | 
| 3 | 2 | 6 | 2 | 4 | 2 | 2 | 
| 4 | 2 | 6 | 3 | 4 | 2 | 2 | 

Câu trả lời cuối cùng:```
2
```Dấu vết này cho thấy tại sao việc thu nhỏ lặp đi lặp lại là cần thiết. Mỗi khi cuốn sách thứ ba vào cửa sổ, số tiền vượt quá giới hạn và cuốn sách ngoài cùng bên trái phải bị loại bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục vào và ra khỏi cửa sổ nhiều nhất một lần | 
| Không gian | O(1) | Chỉ có một số biến được duy trì bên cạnh mảng đầu vào | 

Thời gian chạy tuyến tính dễ dàng xử lý`100000`sách. Việc sử dụng bộ nhớ là tối thiểu và thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, t = map(int, input().split())
    a = list(map(int, input().split()))

    left = 0
    current_sum = 0
    answer = 0

    for right in range(n):
        current_sum += a[right]

        while current_sum > t:
            current_sum -= a[left]
            left += 1

        answer = max(answer, right - left + 1)

    print(answer)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return result

# provided sample
assert run("4 5\n3 1 2 1\n") == "3", "sample 1"

# minimum size
assert run("1 1\n1\n") == "1", "single readable book"

# no readable book
assert run("3 2\n3 4 5\n") == "0", "all books too large"

# all equal values
assert run("5 5\n2 2 2 2 2\n") == "2", "uniform values"

# optimal segment in the middle
assert run("5 5\n10 1 1 1 1\n") == "4", "skip large prefix"

# boundary exact sum
assert run("4 10\n1 2 3 4\n") == "4", "sum equals limit"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`1`| Kích thước đầu vào tối thiểu | 
|`3 2 / 3 4 5`|`0`| Không có phân đoạn hợp lệ tồn tại | 
|`5 5 / 2 2 2 2 2`|`2`| Thu nhỏ cửa sổ lặp đi lặp lại | 
|`5 5 / 10 1 1 1 1`|`4`| Phân đoạn hay nhất bắt đầu sau cuốn sách đầu tiên | 
|`4 10 / 1 2 3 4`|`4`| Bình đẳng chính xác với thời hạn | 

## Vỏ cạnh 

Hãy xem xét trường hợp mỗi cuốn sách riêng lẻ vượt quá thời gian có sẵn. 

đầu vào:```
3 2
3 4 5
```Khi cuốn sách đầu tiên vào cửa sổ, tổng sẽ trở thành`3`, vốn đã lớn hơn`2`. Thuật toán ngay lập tức loại bỏ nó, để lại một cửa sổ trống. Điều tương tự cũng xảy ra với các cuốn sách còn lại. Độ dài cửa sổ tối đa không bao giờ vượt quá`0`, vì vậy đầu ra là chính xác`0`. 

Hãy xem xét một tiền tố lớn không thể đọc được. 

đầu vào:```
5 5
10 1 1 1 1
```Sau khi xử lý cuốn sách đầu tiên, cửa sổ trở nên không hợp lệ và bị thu nhỏ về độ dài bằng 0. Thuật toán sau đó sẽ xây dựng một cửa sổ mới từ những cuốn sách sau này. Cuối cùng, cửa sổ chứa bốn cuốn sách cuối cùng có tổng`4`, đưa ra câu trả lời đúng`4`. Điều này cho thấy thuật toán không thiên về các đoạn bắt đầu gần đầu. 

Hãy xem xét một phân đoạn có tổng liên tục vượt quá giới hạn. 

đầu vào:```
5 5
2 2 2 2 2
```Bất cứ khi nào cuốn sách thứ ba được thêm vào, số tiền sẽ trở thành`6`. các`while`vòng lặp xóa sách ở bên trái cho đến khi tổng trở về`4`. Cửa sổ luôn hợp lệ sau khi thu nhỏ và độ dài hợp lệ lớn nhất được tìm thấy là`2`. Điều này chứng tỏ tại sao bước rút gọn phải là một vòng lặp chứ không phải một bước đơn lẻ.`if`tuyên bố.
