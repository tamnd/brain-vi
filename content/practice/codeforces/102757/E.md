---
title: "CF 102757E - Chuỗi chữ tượng hình"
description: "Sự cố yêu cầu số lượng giá trị chữ tượng hình nhỏ nhất trong một chuỗi phải được thay đổi để XOR của toàn bộ chuỗi trở thành 0. Đầu vào là một dãy gồm n số nguyên dương, trong đó mỗi số nguyên đại diện cho một chữ tượng hình."
date: "2026-07-29T00:25:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102757
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 10-09-20 Div. 2"
rating: 0
weight: 102757
solve_time_s: 60
verified: true
draft: false
---

[CF 102757E - Chuỗi chữ tượng hình](https://codeforces.com/problemset/problem/102757/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Sự cố yêu cầu số lượng giá trị chữ tượng hình nhỏ nhất trong một chuỗi phải được thay đổi để XOR của toàn bộ chuỗi trở thành 0. Đầu vào là một chuỗi`n`số nguyên dương, trong đó mỗi số nguyên đại diện cho một chữ tượng hình. Chúng ta có thể thay thế một số số nguyên này bằng các giá trị khác và mục tiêu là làm cho XOR của tất cả các giá trị bằng`0`trong khi thay đổi ít vị trí nhất có thể. Vấn đề ban đầu có`2 ≤ n ≤ 10^4`và mỗi giá trị nhiều nhất là`10^9`. 

Các ràng buộc đủ nhỏ để hầu hết mọi lần quét tuyến tính đều có thể chấp nhận được. Một giải pháp thử nhiều tập hợp con của các vị trí sẽ ngay lập tức trở nên bất khả thi vì số lượng tập hợp con tăng theo cấp số nhân. Ngay cả việc thử mọi vị trí thay thế có thể cùng với nhiều giá trị ứng cử viên cũng sẽ vượt xa giới hạn một giây cho phép. Điều quan trọng cần lưu ý là XOR có thao tác nghịch đảo trực tiếp, vì vậy chúng ta chỉ cần kiểm tra toàn bộ chuỗi một lần. 

Có hai trường hợp quan trọng có thể phá vỡ việc triển khai không chính xác. 

Nếu chuỗi đã có XOR bằng 0 thì không cần thay thế. 

Ví dụ đầu vào:```
4
1 1 1 1
```Đầu ra là:```
0
```bởi vì`1 xor 1 xor 1 xor 1 = 0`. Một giải pháp bất cẩn luôn thay thế một giá trị sẽ tạo ra câu trả lời sai. 

Trường hợp thứ hai là khi XOR khác 0. Một sai lầm phổ biến là nghĩ rằng một số giá trị phải được thay đổi vì toàn bộ chuỗi không hợp lệ. Trong thực tế, một vị trí luôn là đủ. 

Ví dụ đầu vào:```
3
2 4 8
```Đầu ra là:```
1
```XOR của chuỗi là`2 xor 4 xor 8 = 14`. Nếu chúng ta thay thế giá trị đầu tiên bằng`14 xor 2 = 12`, trình tự trở thành`12 4 8`và XOR trở thành 0. Giải pháp chỉ tìm kiếm loại bỏ hoặc cố gắng cân bằng các cặp sẽ bỏ lỡ thuộc tính này. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là chọn vị trí cần thay thế và sau đó tìm kiếm các giá trị thay thế làm cho XOR bằng 0. Điều này đúng vì mọi chuỗi cuối cùng hợp lệ có thể được mô tả bằng tập hợp các vị trí đã thay đổi và các giá trị mới của chúng. Tuy nhiên, ngay cả việc lựa chọn các vị trí đã thay đổi cũng đòi hỏi phải khám phá nhiều khả năng. có`2^n`tập hợp con có thể có của các vị trí, điều này không thể thực hiện được khi`n`là lớn. 

Quan sát hữu ích đến từ hành vi đại số của XOR. Nếu XOR của chuỗi hiện tại là`x`, thì điều duy nhất ngăn chuỗi không hợp lệ là giá trị bổ sung này`x`trong kết quả XOR cuối cùng. Nếu chúng ta thay đổi một phần tử`a[i]`, XOR mới trở thành:```
x xor a[i] xor new_value
```Để làm cho nó bằng 0, chúng ta cần:```
new_value = x xor a[i]
```Vì bất kỳ phần tử nào cũng có thể được thay thế bằng giá trị được yêu cầu, nên một thay thế là đủ bất cứ khi nào XOR hiện tại chưa bằng 0. Vấn đề giảm từ việc tìm kiếm thông qua các thay đổi có thể có sang tính toán một XOR. 

Lực lượng vũ phu hoạt động vì nó khám phá mọi sự điều chỉnh có thể có, nhưng không thành công vì không gian tìm kiếm quá lớn. Thuộc tính nghịch đảo XOR cho phép chúng ta xây dựng phép sửa ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính XOR của mọi giá trị trong chuỗi. Giá trị này biểu thị mức độ chính xác mà chuỗi hiện tại khác với XOR yêu cầu bằng 0. 
2. Nếu XOR được tính toán bằng 0, xuất ra`0`bởi vì chuỗi đã thỏa mãn điều kiện và không cần thay thế. 
3. Nếu XOR được tính toán khác 0, xuất ra`1`bởi vì việc thay đổi bất kỳ phần tử nào cũng có thể loại bỏ giá trị XOR bổ sung. Đối với phần tử được chọn`a[i]`, thay thế nó bằng`xor_all xor a[i]`làm cho XOR hoàn chỉnh bằng 0. 

Tại sao nó hoạt động: 

XOR của tất cả các phần tử là thông tin duy nhất quan trọng. XOR một giá trị hai lần sẽ loại bỏ nó, vì vậy nếu tổng XOR là`x`, thay đổi một phần tử`a[i]`ĐẾN`a[i] xor x`thêm chính xác sự điều chỉnh còn thiếu. XOR cuối cùng trở thành:```
x xor a[i] xor (a[i] xor x)
```Hai bản sao của`a[i]`hủy bỏ, và hai bản sao của`x`hủy bỏ, để lại số không. Vì một lần thay thế luôn có hiệu quả đối với XOR khác 0 và việc thay thế bằng 0 là đủ khi XOR bằng 0, nên đây là hai câu trả lời khả thi duy nhất. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    x = 0
    for value in arr:
        x ^= value

    if x == 0:
        print(0)
    else:
        print(1)

if __name__ == "__main__":
    solve()
```Mã chỉ giữ một biến,`x`, bởi vì toàn bộ chuỗi có thể được tóm tắt bằng XOR của nó. Mỗi giá trị được xử lý một lần và việc áp dụng XOR nhiều lần sẽ tích lũy XOR cuối cùng của chuỗi. 

điều kiện`x == 0`xử lý trình tự đã hợp lệ. Nếu không thì câu trả lời là ngay lập tức`1`. Thực sự không cần thiết phải xây dựng trình tự đã sửa đổi vì bài toán chỉ yêu cầu số lượng thay thế tối thiểu. 

Không có vấn đề tràn trong Python vì số nguyên có độ chính xác tùy ý. Việc triển khai cũng tránh việc lưu trữ thông tin không cần thiết ngoài mảng đầu vào. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
2 4 8
```| Bước | Giá trị hiện tại | XOR trước | XOR sau | 
| --- | --- | --- | --- | 
| 1 | 2 | 0 | 2 | 
| 2 | 4 | 2 | 6 | 
| 3 | 8 | 6 | 14 | 

XOR cuối cùng là`14`, do đó trình tự không hợp lệ. Một sự thay thế là đủ, và câu trả lời là`1`. 

### Mẫu 2 

đầu vào:```
4
1 1 1 1
```| Bước | Giá trị hiện tại | XOR trước | XOR sau | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 
| 2 | 1 | 1 | 0 | 
| 3 | 1 | 0 | 1 | 
| 4 | 1 | 1 | 0 | 

XOR cuối cùng đã bằng 0 nên câu trả lời là`0`. Điều này xác nhận rằng thuật toán xử lý chính xác các chuỗi không cần thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi giá trị chữ tượng hình được xử lý chính xác một lần. | 
| Không gian | O(1) | Chỉ cần giá trị XOR đang chạy sau khi đọc đầu vào. | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì nó thực hiện một lần chuyển qua chuỗi. Ngay cả đầu vào được phép lớn nhất cũng chỉ yêu cầu một số lượng nhỏ thao tác XOR. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    arr = list(map(int, input().split()))

    x = 0
    for value in arr:
        x ^= value

    return str(0 if x == 0 else 1) + "\n"

# provided samples
assert solution("3\n2 4 8\n") == "1\n", "sample 1"
assert solution("4\n1 1 1 1\n") == "0\n", "sample 2"

# custom cases
assert solution("2\n5 5\n") == "0\n", "already balanced pair"
assert solution("2\n1 2\n") == "1\n", "two different values"
assert solution("5\n7 7 7 7 7\n") == "1\n", "odd count of equal values"
assert solution("2\n1000000000 1\n") == "1\n", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 5 5`|`0`| Các giá trị bằng nhau có thể bị hủy theo XOR. | 
|`2 / 1 2`|`1`| Bất kỳ XOR khác 0 nào cũng có thể được sửa bằng một lần thay thế. | 
|`5 / 7 7 7 7 7`|`1`| Số lẻ có giá trị giống hệt nhau không tự động hủy. | 
|`2 / 1000000000 1`|`1`| Xử lý số nguyên lớn. | 

## Vỏ cạnh 

Đối với một chuỗi đã hợp lệ, thuật toán dừng ở bước kiểm tra XOR. 

đầu vào:```
4
1 1 1 1
```XOR đang chạy kết thúc ở mức 0 sau khi xử lý tất cả các giá trị. Thuật toán trả về`0`, tránh sự thay thế không cần thiết. 

Đối với một chuỗi có XOR khác 0, thuật toán không cố gắng tìm một vị trí đặc biệt. Mọi vị trí đều hoạt động vì giá trị thay thế có thể hấp thụ toàn bộ chênh lệch XOR. 

đầu vào:```
3
2 4 8
```XOR là`14`. Thay thế`2`với`12`mang lại:```
12 xor 4 xor 8 = 0
```Thuật toán trả về`1`, giá trị này là tối thiểu vì các phép thay thế bằng 0 không thể hoạt động khi XOR hiện tại khác 0.
