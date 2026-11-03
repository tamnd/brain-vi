---
title: "CF 111A - Petya và sự bất bình đẳng"
description: "Chúng ta cần xây dựng một mảng gồm n số nguyên dương. Mảng phải thỏa mãn hai điều kiện cùng một lúc. Tổng bình phương của tất cả các phần tử ít nhất phải bằng x, trong khi tổng bình phương của các phần tử không được vượt quá y. Nhiệm vụ không phải là tối ưu hóa bất cứ điều gì."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 111
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 85 (Div. 1 Only)"
rating: 1400
weight: 111
solve_time_s: 153
verified: false
draft: false
---

[CF 111A - Petya và sự bất bình đẳng](https://codeforces.com/problemset/problem/111/A) 

**Đánh giá:** 1400 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một mảng`n`số nguyên dương. Mảng phải thỏa mãn hai điều kiện cùng một lúc. 

Tổng bình phương của tất cả các phần tử ít nhất phải bằng`x`, trong khi tổng thông thường của các phần tử không được vượt quá`y`. 

Nhiệm vụ không phải là tối ưu hóa bất cứ điều gì. Chúng ta chỉ cần tìm một công trình hợp lệ hoặc xác định rằng không tồn tại công trình nào. 

Các ràng buộc ngay lập tức loại trừ bất kỳ tìm kiếm tốn kém nào. Giá trị của`n`có thể đạt được`10^5`, vì vậy mọi thứ bậc hai hoặc hàm mũ đều không thể xảy ra. Giá trị của`x`đặc biệt lớn, lên tới`10^12`, có nghĩa là việc ép buộc các mảng ứng viên một cách thô bạo là hoàn toàn không thực tế. Chúng ta cần một cấu trúc toán học trực tiếp có thể tính được trong thời gian tuyến tính. 

Chi tiết quan trọng là điều kiện bình phương tăng nhanh hơn nhiều so với tổng thông thường. Một số lớn đóng góp rất lớn vào tổng bình phương trong khi chỉ đóng góp tuyến tính vào tổng bình thường. Quan sát đó gợi ý mạnh mẽ việc tập trung hầu hết giá trị vào một phần tử thay vì trải đều. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
1 10 3
```Chúng ta cần một số nguyên dương có bình phương ít nhất là`10`, nhưng giá trị của nó lớn nhất`3`. Yếu tố duy nhất có thể cần phải đáp ứng cả hai:```
a^2 >= 10
a <= 3
```Không có số nguyên như vậy tồn tại, bởi vì`3^2 = 9`. Đầu ra đúng là:```
-1
```Việc thực hiện bất cẩn có thể chỉ kiểm tra điều kiện hình vuông và in`4`, quên rằng giới hạn tổng bị vi phạm. 

Một trường hợp tinh tế khác là khi số tiền tối thiểu có thể đã vượt quá`y`.```
5 100 4
```Tất cả các số phải là số dương nên tổng nhỏ nhất có thể đã là`5`. Từ`5 > 4`, câu trả lời là không thể dù`x`. Đầu ra đúng là:```
-1
```Việc triển khai chỉ tập trung vào yêu cầu hình vuông có thể bỏ lỡ tình huống không thể xảy ra ngay lập tức này. 

Thêm một ví dụ phức tạp nữa:```
3 11 5
```Một ý tưởng ngây thơ là phân phối các giá trị một cách đồng đều, ví dụ`[2,2,1]`. Tổng hợp lệ:```
2 + 2 + 1 = 5
```Nhưng tổng bình phương chỉ là:```
4 + 4 + 1 = 9
```cái đó quá nhỏ. Việc xây dựng hợp lệ là:```
3 1 1
```bởi vì:```
9 + 1 + 1 = 11
3 + 1 + 1 = 5
```Điều này chứng tỏ tại sao việc tập trung giá trị vào một phần tử lại mạnh hơn việc dàn trải nó ra. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra các mảng và kiểm tra xem chúng có thỏa mãn hai bất đẳng thức hay không. Ngay cả khi chúng tôi giới hạn tối đa mỗi phần tử`y`, số lượng mảng vẫn sẽ rất lớn. Ví dụ, với`n = 20`Và`y = 20`, đã có rồi`20^20`mảng có thể. Điều này là hoàn toàn không thể thực hiện được. 

Ý tưởng brute-force hoạt động về mặt khái niệm vì việc kiểm tra một mảng ứng cử viên rất dễ dàng. Chúng ta có thể tính cả hai tổng theo thời gian tuyến tính. Vấn đề là không gian tìm kiếm. 

Bước đột phá đến từ việc hiểu cách hoạt động của các hình vuông. Giả sử chúng ta đã ấn định tổng số tiền. Trong số tất cả các mảng có tổng đó, việc đặt càng nhiều giá trị càng tốt vào một phần tử sẽ tối đa hóa tổng bình phương. 

Ví dụ:```
5^2 = 25
```trong khi:```
3^2 + 2^2 = 13
```Việc chia một số thành những phần nhỏ hơn sẽ làm giảm phần đóng góp bình phương. 

Điều đó có nghĩa là chiến lược tốt nhất là cực kỳ đơn giản. Làm`n - 1`các phần tử bằng`1`, vì mọi số đều phải dương. Đặt mọi thứ khác vào một phần tử lớn. 

Hãy để phần tử lớn đó là`k`. 

Sau đó:```
sum = k + (n - 1)
square_sum = k^2 + (n - 1)
```Chúng tôi cần:```
k^2 + (n - 1) >= x
k + (n - 1) <= y
```Hợp lệ nhỏ nhất`k`với điều kiện bình phương là:```
k >= ceil(sqrt(x - (n - 1)))
```Nếu đó`k`cũng thỏa mãn ràng buộc tổng, chúng ta có một cách xây dựng hợp lệ. Nếu không thì không có giải pháp nào tồn tại. 

Điều này làm giảm toàn bộ vấn đề thành một vài phép tính số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) không bao gồm đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`x`, Và`y`. 
2. Dự trữ`n - 1`các yếu tố như`1`. 

Đây là đóng góp nhỏ nhất có thể có từ những vị trí đó, vì tất cả các số đều phải dương. 
3. Gọi phần tử còn lại là`k`. 

Tổng bình phương trở thành:```
k^2 + (n - 1)
```4. Tính giá trị tối thiểu`k`thỏa mãn điều kiện bình phương. 

Chúng tôi cần:```
k^2 >= x - (n - 1)
```Vì thế:```
k = ceil(sqrt(x - (n - 1)))
```Nếu như`x - (n - 1)`đã không dương rồi thì`k = 1`là đủ. 
5. Kiểm tra xem ràng buộc về tổng thông thường có được thỏa mãn hay không. 

Tổng số tiền là:```
k + (n - 1)
```Nếu điều này vượt quá`y`, in`-1`. 
6. Nếu không, hãy in`k`theo sau là`n - 1`những cái đó. 

### Tại sao nó hoạt động 

Việc xây dựng giảm thiểu số tiền thông thường cho bất kỳ đóng góp bình phương nào. 

Giả sử chúng ta muốn một tổng bình phương lớn. Việc chia giá trị cho nhiều số là không hiệu quả vì:```
(a + b)^2 > a^2 + b^2
```cho số nguyên dương`a`Và`b`. 

Vì vậy, phần đóng góp bình phương tối đa cho một tổng thông thường cố định đạt được bằng cách tập trung càng nhiều giá trị càng tốt vào một phần tử. Bằng cách đặt tất cả các phần tử khác thành`1`, chúng tôi để lại ngân sách lớn nhất có thể cho một số lượng lớn. 

Nếu ngay cả sự sắp xếp tốt nhất có thể này cũng không thể thỏa mãn đồng thời cả hai bất đẳng thức, thì không có sự sắp xếp nào khác có thể làm được. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

n, x, y = map(int, input().split())

need = x - (n - 1)

if need <= 1:
    k = 1
else:
    k = math.isqrt(need)
    if k * k < need:
        k += 1

if k + (n - 1) > y:
    print(-1)
else:
    print(k)
    for _ in range(n - 1):
        print(1)
```Việc thực hiện theo sau việc xây dựng trực tiếp. 

Đầu tiên, chúng tôi bảo lưu`n - 1`vị trí cho những người. Đóng góp bình phương của họ cũng là`n - 1`, do đó yêu cầu bình phương còn lại trở thành:```
x - (n - 1)
```Sau đó chúng tôi tính số nguyên nhỏ nhất`k`hình vuông của nó đủ lớn. sử dụng`math.isqrt`tránh các vấn đề về độ chính xác của dấu phẩy động.`isqrt(v)`trả về sàn của căn bậc hai, vì vậy chúng ta tăng thêm một nếu cần. 

Điều kiện:```
if need <= 1:
    k = 1
```xử lý các trường hợp trong đó các trường hợp hiện tại gần như đã thỏa mãn điều kiện bình phương. Vì tất cả các giá trị phải luôn dương,`k`không thể bằng không. 

Kiểm tra cuối cùng:```
if k + (n - 1) > y:
```là bài kiểm tra tính bất khả thi quan trọng. Ngay cả cách xây dựng hiệu quả bình phương nhất cũng vi phạm tổng giới hạn, do đó không có giải pháp nào tồn tại. 

Thứ tự đầu ra không quan trọng. Bất kỳ mảng hợp lệ đều được chấp nhận. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 15 15
```Chúng tôi tính toán:```
need = 15 - 4 = 11
```Số nguyên nhỏ nhất có hình vuông ít nhất`11`là`4`. 

| Bước | Giá trị | 
| --- | --- | 
| n - 1 người đóng góp | 4 | 
| Cần vuông còn lại | 11 | 
| Được chọn k | 4 | 
| Tổng số tiền | 8 | 
| Tổng bình phương | 20 | 

Mảng được tạo ra có thể là:```
4 1 1 1 1
```Tổng số tiền là:```
8 <= 15
```và tổng bình phương là:```
16 + 1 + 1 + 1 + 1 = 20 >= 15
```Dấu vết này thể hiện ý tưởng cốt lõi: một giá trị lớn chiếm ưu thế trong tổng bình phương. 

### Ví dụ 2 

đầu vào:```
3 11 5
```Chúng tôi tính toán:```
need = 11 - 2 = 9
```Hợp lệ nhỏ nhất`k`là`3`. 

| Bước | Giá trị | 
| --- | --- | 
| n - 1 người đóng góp | 2 | 
| Cần vuông còn lại | 9 | 
| Được chọn k | 3 | 
| Tổng số tiền | 5 | 
| Tổng bình phương | 11 | 

Đầu ra là:```
3 1 1
```Ví dụ này đạt đến giới hạn tổng một cách chính xác. Nó xác nhận rằng thuật toán xử lý đẳng thức một cách chính xác và không yêu cầu tổng phải nhỏ hơn hoàn toàn so với`y`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc in mảng chiếm ưu thế trong thời gian chạy | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Bản thân số học là thời gian không đổi. Công việc tuyến tính duy nhất là xuất ra`n`số nguyên, dễ dàng phù hợp với các ràng buộc cho`n <= 10^5`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import math

def solve():
    input = sys.stdin.readline

    n, x, y = map(int, input().split())

    need = x - (n - 1)

    if need <= 1:
        k = 1
    else:
        k = math.isqrt(need)
        if k * k < need:
            k += 1

    if k + (n - 1) > y:
        print(-1)
    else:
        print(k)
        for _ in range(n - 1):
            print(1)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("5 15 15\n") == "4\n1\n1\n1\n1\n"

# minimum size, impossible
assert run("1 10 3\n") == "-1\n"

# exact boundary
assert run("3 11 5\n") == "3\n1\n1\n"

# already satisfied by all ones
assert run("4 4 10\n") == "1\n1\n1\n1\n"

# impossible because minimum sum exceeds y
assert run("5 100 4\n") == "-1\n"

# large square requirement
out = run("2 1000000 2000\n")
vals = list(map(int, out.strip().split()))
assert len(vals) == 2
assert vals[0] + vals[1] <= 2000
assert vals[0] * vals[0] + vals[1] * vals[1] >= 1000000
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 10 3`|`-1`| Không thể có một yếu tố | 
|`3 11 5`|`3 1 1`| Bình đẳng chính xác trên cả hai điều kiện | 
|`4 4 10`|`1 1 1 1`| Tất cả đã đủ | 
|`5 100 4`|`-1`| Số tiền dương tối thiểu đã quá lớn | 
|`2 1000000 2000`| Bất kỳ mảng hợp lệ nào | Giá trị lớn và tính toán căn bậc hai | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1 10 3
```Thuật toán tính toán:```
need = 10
```Trần căn bậc hai hợp lệ nhỏ nhất là:```
k = 4
```Nhưng:```
4 > 3
```vì vậy điều kiện tổng không thành công ngay lập tức. Thuật toán in:```
-1
```Điều này xử lý chính xác các trường hợp không thể có một phần tử. 

Bây giờ hãy xem xét:```
5 100 4
```Ngay cả trước khi lo lắng về bình phương, năm số nguyên dương phải có tổng ít nhất bằng`5`. 

Thuật toán tính toán:```
need = 96
k = 10
```Sau đó:```
10 + 4 = 14
```vượt quá`y = 4`. 

Thuật toán in:```
-1
```Điều này nắm bắt chính xác thực tế rằng chỉ riêng tổng số bị ràng buộc đã khiến nhiệm vụ không thể thực hiện được. 

Cuối cùng, hãy xem xét:```
4 4 10
```Những người dành riêng đã đóng góp:```
3
```thành tổng bình phương, vì vậy:```
need = 1
```Thuật toán giữ:```
k = 1
```và kết quả đầu ra:```
1 1 1 1
```Tổng bình phương chính xác là`4`, thỏa mãn yêu cầu với giá trị nhỏ nhất có thể.
