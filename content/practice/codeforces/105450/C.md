---
title: "CF 105450C - Ống Hút Chua"
description: "Chúng ta được cấp một tập hợp các độ dài nguyên, mỗi độ dài đại diện cho một ống hút chua. Từ những điều này, chúng ta muốn chọn một tập hợp con sao cho khi chúng ta sắp xếp độ dài đã chọn theo thứ tự không giảm, mọi phần tử nhỏ hơn sẽ chia cho mọi phần tử lớn hơn theo sau nó."
date: "2026-06-23T17:32:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105450
codeforces_index: "C"
codeforces_contest_name: "UTPC x WiCS Contest 10-25-24 (UT Internal)"
rating: 0
weight: 105450
solve_time_s: 82
verified: false
draft: false
---

[CF 105450C - Ống hút chua](https://codeforces.com/problemset/problem/105450/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các độ dài nguyên, mỗi độ dài đại diện cho một ống hút chua. Từ những điều này, chúng ta muốn chọn một tập hợp con sao cho khi chúng ta sắp xếp độ dài đã chọn theo thứ tự không giảm, mọi phần tử nhỏ hơn sẽ chia cho mọi phần tử lớn hơn theo sau nó. 

Nói cách khác, bên trong tập hợp con đã chọn, nếu chúng ta lấy bất kỳ hai phần tử nào thì phần tử nhỏ hơn phải là ước của phần tử lớn hơn. Nhiệm vụ là tìm kích thước tối đa có thể có của tập con như vậy. 

Kích thước đầu vào lên tới 1000 phần tử và mỗi giá trị có thể lớn tới 2 × 10^9. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào có hành vi bậc ba hoặc bất kỳ điều gì liên quan đến việc phân tích nhân tử lặp đi lặp lại trên mỗi cặp đều có thể là đường biên nhưng vẫn có khả năng chấp nhận được, trong khi mọi thứ theo cấp số nhân trên các tập hợp con là không thể. 

Trường hợp cạnh tinh tế xuất phát từ các giá trị lặp lại. Các giá trị bằng nhau luôn có giá trị cùng nhau vì bất kỳ số nào cũng chia hết cho chính nó. Một trường hợp khác là khi không có hai số nào chia hết cho nhau, trong trường hợp đó câu trả lời chỉ là 1. 

Một sai lầm ngây thơ là nghĩ rằng đây là một vấn đề về dãy con tăng dài nhất cổ điển và cố gắng chỉ thực thi thứ tự mà bỏ qua khả năng chia hết. Điều đó sẽ thất bại trong các trường hợp như [2, 3, 6], trong đó chỉ đặt hàng là không đủ và cấu trúc phân chia là quan trọng. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là thử mọi tập hợp con và kiểm tra xem nó có thỏa mãn điều kiện chia hết hay không. Đối với tập hợp con có kích thước k, việc kiểm tra tính hợp lệ yêu cầu kiểm tra tất cả các cặp hoặc ít nhất là tất cả các cặp liên tiếp sau khi sắp xếp, đó là O(k^2). Vì có 2^n tập hợp con nên cách tiếp cận này hoàn toàn không khả thi, tăng theo thứ tự 2^1000. 

Một cách tiếp cận có cấu trúc hơn là nhận ra rằng điều kiện chia hết trở nên dễ dàng hơn nhiều sau khi sắp xếp mảng. Sau khi được sắp xếp, ràng buộc sẽ giảm xuống việc xây dựng chuỗi dài nhất trong đó mỗi phần tử chia phần tử tiếp theo. Điều này biến bài toán thành một biến thể của đường đi dài nhất trong đồ thị chu kỳ có hướng trong đó các cạnh đi từ i đến j nếu i < j và l[j] chia hết cho l[i]. 

Từ đây, lập trình động trở nên tự nhiên. Đối với mỗi phần tử, chúng tôi tính toán chuỗi hợp lệ dài nhất kết thúc ở phần tử đó. Để tính toán các chuyển đổi, chúng ta chỉ cần xem xét các phần tử trước đó theo thứ tự sắp xếp để phân chia nó. 

Quan sát quan trọng là khả năng chia hết có tính bắc cầu theo cách hỗ trợ cho chuỗi: nếu a chia hết b và b chia hết c thì a chia hết c. Việc sắp xếp đảm bảo chúng tôi chỉ mở rộng từ các giá trị nhỏ hơn hoặc bằng giá trị lớn hơn, điều này ngăn cản việc chuyển đổi ngược không hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(2^n · n) | O(n) | Quá chậm | 
| DP trên mảng được sắp xếp | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp danh sách độ dài theo thứ tự không giảm. Điều này đảm bảo rằng bất kỳ tập hợp con hợp lệ nào cũng có thể được sắp xếp sao cho chỉ cần kiểm tra tính chia hết về sau. 
2. Tạo một mảng DP trong đó dp[i] biểu thị kích thước tối đa của một tập hợp con hợp lệ có phần tử lớn nhất nằm ở vị trí i trong mảng được sắp xếp. 
3. Khởi tạo mọi dp[i] thành 1, vì mỗi phần tử riêng lẻ đã tạo thành một tập hợp con hợp lệ. 
4. Với mỗi chỉ số i từ trái sang phải, xét tất cả các chỉ số trước đó j < i. Nếu l[i] chia hết cho l[j] thì chúng ta có thể mở rộng bất kỳ tập hợp con hợp lệ nào kết thúc tại j bằng cách thêm l[i]. Cập nhật dp[i] = max(dp[i], dp[j] + 1). Bước này xây dựng tất cả các chuỗi hợp lệ kết thúc tại i. 
5. Câu trả lời là giá trị lớn nhất trên tất cả dp[i], vì tập con tốt nhất có thể kết thúc ở bất kỳ vị trí nào. 

Tại sao nó hoạt động xuất phát từ thực tế là việc sắp xếp loại bỏ nhu cầu xem xét các hoán vị của tập hợp con. Mọi tập hợp con hợp lệ đều có dạng chuẩn theo thứ tự được sắp xếp và DP đảm bảo rằng mọi phần tử cuối cùng có thể đều được xem xét trong khi vẫn duy trì các ràng buộc về khả năng chia hết trong quá trình chuyển đổi. Trạng thái DP nắm bắt đầy đủ tất cả các chuỗi hợp lệ kết thúc ở mỗi chỉ mục, do đó không bỏ sót giải pháp tối ưu nào.

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

a.sort()

dp = [1] * n

for i in range(n):
    for j in range(i):
        if a[i] % a[j] == 0:
            dp[i] = max(dp[i], dp[j] + 1)

print(max(dp))
```Giải pháp bắt đầu bằng cách sắp xếp mảng sao cho bất kỳ tập hợp con tiềm năng nào cũng có thể được hiểu là một chuỗi tăng dần trong đó chỉ cần kiểm tra khả năng chia hết về phía trước. Mảng dp lưu trữ chuỗi kết thúc tốt nhất ở mỗi chỉ mục. Mỗi lần chuyển đổi sẽ kiểm tra xem giá trị hiện tại có thể mở rộng chuỗi hợp lệ trước đó hay không và giá trị tối đa trên tất cả các giá trị dp sẽ đưa ra câu trả lời cuối cùng. 

Một sự tinh tế phổ biến là xử lý các bản sao. điều kiện`a[i] % a[j] == 0`một cách tự nhiên cho phép các giá trị bằng nhau vì một số tự chia, do đó nhân đôi chuỗi chính xác với nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 7 5 4 8
```Mảng được sắp xếp trở thành`[2, 4, 5, 7, 8]`. 

| tôi | một [tôi] | j xem xét | tiện ích mở rộng hợp lệ | dp[i] | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | - | - | 1 | 
| 1 | 4 | 0 | 2 → 4 | 2 | 
| 2 | 5 | 0,1 | không | 1 | 
| 3 | 7 | 0,1,2 | không | 1 | 
| 4 | 8 | 0,1,2,3 | 2 → 8, 4 → 8 | 3 | 

Chuỗi tốt nhất là`[2, 4, 8]`, xác nhận câu trả lời 3. 

Dấu vết này cho thấy cách DP xây dựng chuỗi một cách tự nhiên mà không cần phải theo dõi rõ ràng các tập hợp con. 

### Ví dụ 2 

đầu vào:```
5
10 10 10 10 10
```Mảng được sắp xếp vẫn còn`[10, 10, 10, 10, 10]`. 

| tôi | một [tôi] | j xem xét | tiện ích mở rộng hợp lệ | dp[i] | 
| --- | --- | --- | --- | --- | 
| 0 | 10 | - | - | 1 | 
| 1 | 10 | 0 | 10 → 10 | 2 | 
| 2 | 10 | 0,1 | tất cả đều hợp lệ | 3 | 
| 3 | 10 | 0,1,2 | tất cả đều hợp lệ | 4 | 
| 4 | 10 | 0,1,2,3 | tất cả đều hợp lệ | 5 | 

Mọi phần tử đều có thể mở rộng mọi phần tử trước đó vì đẳng thức thỏa mãn tính chia hết. DP tích lũy chính xác toàn bộ chuỗi có độ dài 5. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Đối với mỗi i, chúng tôi quét tất cả j < i và thực hiện kiểm tra tính chia hết theo thời gian không đổi | 
| Không gian | O(n) | Chỉ mảng dp được lưu trữ | 

Với n lên tới 1000, giải pháp O(n^2) thực hiện khoảng 10^6 thao tác, nằm trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    dp = [1] * n
    for i in range(n):
        for j in range(i):
            if a[i] % a[j] == 0:
                dp[i] = max(dp[i], dp[j] + 1)

    return str(max(dp))

# provided samples
assert run("5\n2 7 5 4 8\n") == "3"
assert run("5\n10 10 10 10 10\n") == "5"

# custom cases
assert run("1\n42\n") == "1", "single element"
assert run("4\n2 3 5 7\n") == "1", "no divisibility"
assert run("4\n1 2 4 8\n") == "4", "perfect chain"
assert run("6\n3 6 7 14 28 2\n") == "4", "mixed chain structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | xử lý kích thước tối thiểu | 
| số nguyên tố | 1 | không có tiện ích mở rộng hợp lệ | 
| sức mạnh của hai | 4 | tăng trưởng toàn chuỗi | 
| hỗn hợp | 4 | chuỗi chính xác trên nhiều nhánh | 

## Vỏ cạnh 

Đối với một yếu tố đầu vào như`[42]`, mảng DP là`[1]`và kết quả là 1 vì không có chuyển tiếp nào tồn tại. 

Vì`[2, 3, 5, 7]`, việc sắp xếp không tạo ra bất kỳ quan hệ chia hết nào. Mọi dp[i] vẫn là 1 vì không có cặp nào thỏa mãn`a[i] % a[j] == 0`. Đầu ra chính xác vẫn là 1. 

Đối với một trường hợp hoàn toàn có thể xâu chuỗi như`[1, 2, 4, 8]`, việc sắp xếp sẽ giữ nguyên thứ tự và mọi phần tử đều mở rộng tất cả các phần tử trước đó vì mỗi phần tử chia phần tiếp theo. Bản dựng DP`[1, 2, 3, 4]`, đưa ra đáp án cuối cùng 4.
