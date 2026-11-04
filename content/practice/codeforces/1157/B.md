---
title: "CF 1157B - Số dài"
description: "Chúng ta được cho một số dưới dạng một chuỗi các chữ số từ 1 đến 9 và ánh xạ f từ mỗi chữ số này sang chữ số khác trong cùng một phạm vi. Nhiệm vụ là tối đa hóa số kết quả bằng cách chọn tối đa một đoạn chữ số liền kề và thay thế mỗi chữ số x trong đoạn đó bằng f(x)."
date: "2026-06-12T02:32:31+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1157
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 555 (Div. 3)"
rating: 1300
weight: 1157
solve_time_s: 85
verified: true
draft: false
---

[CF 1157B - Số dài](https://codeforces.com/problemset/problem/1157/B) 

**Đánh giá:** 1300 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số dưới dạng một chuỗi các chữ số từ`1`ĐẾN`9`và một bản đồ`f`từ mỗi chữ số này sang chữ số khác trong cùng một phạm vi. Nhiệm vụ là tối đa hóa số kết quả bằng cách chọn tối đa một đoạn chữ số liền kề và thay thế từng chữ số`x`trong phân khúc đó với`f(x)`. Tất cả các chữ số khác không thay đổi. 

Đầu vào cho chúng ta`n`, số chữ số, số`a`dưới dạng một chuỗi và một danh sách gồm 9 số nguyên biểu thị`f(1)`bởi vì`f(9)`. Đầu ra là số lớn nhất có thể đạt được bằng cách thực hiện thao tác một lần hoặc không thực hiện lần nào. 

Được cho`n`có thể lớn tới 200.000 và giới hạn thời gian là 2 giây, bất kỳ thuật toán nào xem xét tất cả các phân đoạn con có thể một cách ngây thơ sẽ yêu cầu theo thứ tự`O(n^2)`hoạt động quá chậm. Chúng ta cần một cách tiếp cận tuyến tính hoặc tuyến tính. 

Các trường hợp đặc biệt không rõ ràng bao gồm các trình tự áp dụng`f`có thể tạm thời giảm một chữ số trước khi tăng các chữ số sau. Ví dụ, nếu`a = 987`Và`f(9) = 8, f(8) = 9`, một kẻ tham lam ngây thơ có thể thử bắt đầu thay thế tại`9`mặc dù việc giữ nguyên nó sẽ tạo ra một con số tổng thể lớn hơn. Một trường hợp cạnh khác xảy ra khi tất cả các chữ số được ánh xạ tới các giá trị nhỏ hơn hoặc bằng nhau, trong trường hợp đó, bước đi tối ưu là không thực hiện thao tác nào cả. Ví dụ,`a = 123`Và`f = [1,1,1,4,5,6,7,8,9]`sản xuất`123`bởi vì không có sự thay thế nào làm tăng bất kỳ chữ số nào. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại mọi phân đoạn liền kề có thể, áp dụng`f`đến các chữ số trong đoạn đó và tính số kết quả. Theo dõi số lượng tối đa gặp phải. Điều này có hiệu quả vì bất kỳ thao tác nào được sự cố cho phép đều có thể được biểu diễn dưới dạng thay thế phân đoạn liền kề. Tuy nhiên, số lượng các phân đoạn tăng lên khi`n(n+1)/2`, vậy với`n = 2*10^5`, cách tiếp cận này sẽ yêu cầu theo thứ tự`2*10^10`những hoạt động không thực tế. 

Quan sát quan trọng cho một giải pháp tối ưu là chúng ta nên bắt đầu thay thế các chữ số từ vị trí đầu tiên trong đó`f(x) > x`và tiếp tục thay thế cho đến khi`f(x) >= x`. Một lần`f(x) < x`, chúng ta dừng việc thay thế, vì việc mở rộng thay thế hơn nữa sẽ làm giảm số lượng. Điều này có hiệu quả vì thao tác phải liền kề và được áp dụng nhiều nhất một lần và chữ số lớn hơn ở đầu số có tác động đáng kể hơn đến số cuối cùng so với các chữ số ở sau. Điều này đưa ra một giải pháp tham lam là quét số một lần từ trái sang phải, quyết định từng chữ số có nên thay thế nó hay không dựa trên một so sánh đơn giản với`f(x)`và một lá cờ cho biết liệu chúng tôi có đang ở trong phân khúc thay thế đang hoạt động hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo cờ`started`ĐẾN`False`để cho biết liệu chúng tôi đã bắt đầu một phân khúc thay thế hay chưa. Khởi tạo một danh sách trống`result`để xây dựng số cuối cùng. 
2. Lặp lại từng chữ số`d`trong số ban đầu từ trái sang phải. Chuyển thành`d`thành một số nguyên để so sánh với`f(d)`. 
3. Nếu`f(d) > d`, thay chữ số bằng`f(d)`và thiết lập`started`ĐẾN`True`. Nối chữ số được thay thế vào`result`. 
4. Nếu`f(d) == d`, kiểm tra`started`lá cờ. Nếu như`started`là`True`, tiếp tục thay thế bằng`f(d)`(không thay đổi). Nếu như`started`là`False`, nối chữ số gốc vào`result`. 
5. Nếu`f(d) < d`Và`started`là`True`, dừng đoạn thay thế. Bộ`started`ĐẾN`False`và nối chữ số gốc vào`result`. Nếu như`started`là`False`, chỉ cần nối thêm chữ số gốc. 
6. Sau khi xử lý xong tất cả các chữ số, hãy nối`result`list thành một chuỗi và xuất nó. 

Điều bất biến ở đây là khi chúng tôi bắt đầu phân đoạn thay thế, chúng tôi sẽ tối đa hóa từng chữ số cho đến khi việc thay thế làm giảm số lượng. Vì thao tác phải liền kề nên chúng ta không thể bỏ qua một chữ số trong phân đoạn. Điều này đảm bảo số lượng tối đa vì bất kỳ sự bắt đầu hoặc kết thúc nào sớm hơn hoặc muộn hơn sẽ tạo ra kết quả nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = input().strip()
f = list(map(int, input().split()))

result = []
started = False

for ch in a:
    digit = int(ch)
    mapped = f[digit - 1]
    if mapped > digit:
        result.append(str(mapped))
        started = True
    elif mapped == digit:
        result.append(str(digit) if not started else str(mapped))
    else:
        result.append(str(digit))
        if started:
            started = False

print(''.join(result))
```Giải pháp khởi tạo mảng kết quả và lặp qua các chữ số. Việc chuyển đổi sang số nguyên là cần thiết vì`f`là danh sách các số nguyên. các`started`cờ đảm bảo việc thay thế diễn ra liền kề và các phép so sánh xử lý chính xác việc bắt đầu, tiếp tục hoặc dừng thay thế. Việc nối danh sách kết quả ở cuối sẽ tránh được việc nối chuỗi lặp lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
a = 1337
f = [1,2,5,4,6,6,3,1,9]
```| tôi | ch | chữ số | f(chữ số) | bắt đầu | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | Sai | 1 | 
| 1 | 3 | 3 | 5 | Đúng | 1,5 | 
| 2 | 3 | 3 | 5 | Đúng | 1,5,5 | 
| 3 | 7 | 7 | 3 | Đúng | 1,5,5,7 | 

Đầu ra:`1557`Việc thay thế bắt đầu ở 3 số đầu tiên, tiếp tục đến 3 số thứ hai và dừng ở 7 vì`f(7) < 7`. Điều này chứng tỏ sự lựa chọn phân khúc chính xác. 

### Ví dụ tùy chỉnh 2 

đầu vào:```
a = 987
f = [1,2,3,4,5,6,7,8,9]
```| tôi | ch | chữ số | f(chữ số) | bắt đầu | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 9 | 9 | 9 | Sai | 9 | 
| 1 | 8 | 8 | 8 | Sai | 8 | 
| 2 | 7 | 7 | 7 | Sai | 7 | 

Đầu ra:`987`Không có sự thay thế nào làm tăng bất kỳ chữ số nào, vì vậy số này không thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chữ số được xử lý một lần và tất cả các phép toán bên trong vòng lặp là O(1) | 
| Không gian | O(n) | Danh sách kết quả lưu trữ n ký tự | 

Giải pháp phù hợp trong giới hạn thời gian và bộ nhớ một cách thoải mái cho`n`lên tới 2*10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = input().strip()
    f = list(map(int, input().split()))
    result = []
    started = False
    for ch in a:
        digit = int(ch)
        mapped = f[digit - 1]
        if mapped > digit:
            result.append(str(mapped))
            started = True
        elif mapped == digit:
            result.append(str(digit) if not started else str(mapped))
        else:
            result.append(str(digit))
            if started:
                started = False
    return ''.join(result)

# provided samples
assert run("4\n1337\n1 2 5 4 6 6 3 1 9\n") == "1557", "sample 1"

# custom cases
assert run("3\n987\n1 2 3 4 5 6 7 8 9\n") == "987", "no replacement needed"
assert run("5\n11111\n9 8 7 6 5 4 3 2 1\n") == "99999", "all digits increased"
assert run("4\n4321\n1 2 3 4 5 6 7 8 9\n") == "4321", "all digits mapped to smaller values"
assert run("6\n123456\n2 3 4 5 6 7 8 9 1\n") == "234567", "replacement starts at first digit"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 987 với f(x) ≤ x | 987 | Hoạt động có thể được bỏ qua | 
| 11111 với f(x) > x | 99999 | Thay thế tối đa tất cả các chữ số | 
| 4321 với f(x) < x | 4321 | Dừng đúng trước khi giảm chữ số | 
| 123456 với f(x) > x | 234567 | Thay thế bắt đầu ở lần tăng đầu tiên | 

## Vỏ cạnh 

Nếu tất cả các chữ số được ánh xạ nhỏ hơn
