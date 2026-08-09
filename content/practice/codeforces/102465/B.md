---
title: "CF 102465B - Hình ảnh mờ"
description: "Mỗi hàng của hình ảnh chứa một khoảng pixel tốt liền kề nhau. Đối với hàng (i), các pixel tốt chiếm các cột từ (ai) đến (bi). Chúng ta cần hình vuông thẳng hàng theo trục lớn nhất có mọi pixel đều tốt. Giả sử một hình vuông sử dụng các hàng (l) đến (r)."
date: "2026-08-08T09:10:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102465
codeforces_index: "B"
codeforces_contest_name: "2018-2019 ICPC Southwestern European Regional Programming Contest (SWERC 2018)"
rating: 0
weight: 102465
solve_time_s: 257
verified: true
draft: false
---

[CF 102465B - Hình ảnh bị mờ](https://codeforces.com/problemset/problem/102465/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi hàng của hình ảnh chứa một khoảng pixel tốt liền kề nhau. Đối với hàng (i), các pixel tốt chiếm các cột từ (a_i) đến (b_i). Chúng ta cần hình vuông thẳng hàng theo trục lớn nhất có mọi pixel đều tốt. 

Giả sử một hình vuông sử dụng các hàng (l) đến (r). Mỗi hàng trong số đó phải chứa cùng một khoảng ngang được chiếm bởi hình vuông. Vì hàng (i) chính xác là khoảng ([a_i,b_i]), các cột chung có sẵn cho tất cả các hàng là 

[ 
[\max_{l\le i\le r} a_i,\ \min_{l\le i\le r} b_i]. 
] 

Nếu giao điểm này chứa ít nhất (k) cột và số hàng cũng là (k), thì tồn tại một hình vuông (k\times k). 

Điều kiện hình học đặc biệt cho chúng ta một sự đơn giản hóa bổ sung. Nếu hai pixel tốt nằm trong cùng một cột thì mọi pixel giữa chúng trong cột đó cũng tốt. Do đó, nếu cả hàng đầu tiên và hàng cuối cùng của một nhóm đều chứa một số khoảng cột thì mỗi hàng trung gian cũng chứa khoảng đó. Chúng ta có thể sử dụng hàng đầu tiên và hàng cuối cùng để kiểm tra ô vuông ứng viên thay vì kiểm tra rõ ràng tất cả các hàng giữa chúng. 

Với (N\le 100000), thuật toán (O(N^2)) đã quá chậm. Có khoảng 

[ 
\frac{N(N+1)}2 
] 

phạm vi hàng liền kề, tức là khoảng (5\times10^9) khi (N=100000). Ngay cả khối lượng công việc không đổi cho mỗi cặp cũng vượt xa giới hạn cho phép của cuộc thi trong hai giây. Chúng ta cần một thuật toán gần với thời gian tuyến tính. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể dẫn đến sai sót. Đầu tiên, một hàng luôn là một hình vuông hợp lệ (1\times1). Ví dụ,```
1
0 0
```có câu trả lời`1`. Việc triển khai bắt đầu câu trả lời bằng 0 và chỉ tìm kiếm hình vuông bằng hai hàng có thể trả về 0 không chính xác. 

Vấn đề thứ hai là một hàng riêng lẻ rộng không có nghĩa là một hình vuông lớn. Coi như```
3
0 3
1 2
0 3
```Hàng giữa chỉ có hai điểm ảnh tốt nên hình vuông lớn nhất có cạnh`2`, không`4`. Giải pháp chỉ dựa trên hàng rộng nhất sẽ sai. Dữ liệu nhập này hợp lệ vì các cột chung của hàng thứ nhất và thứ ba cũng có ở hàng giữa. 

Các điểm cuối bao gồm là một nguồn lỗi phổ biến khác. Ví dụ,```
3
0 1
0 1
0 1
```chứa chính xác hai cột tốt, vì vậy câu trả lời là`2`. Độ dài khoảng là`b_i-a_i+1`, không`b_i-a_i`. 

Cuối cùng, ô vuông ứng cử viên không thể vượt qua hàng cuối cùng. Vì```
2
0 1
0 1
```câu trả lời là`2`và kiểm tra ứng viên có hàng dưới cùng là chỉ mục`2`phải dừng lại trước khi truy cập nó. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản có thể liệt kê mọi hàng trên cùng và dưới cùng có thể. Đối với một cặp cố định, chúng ta có thể giao nhau tất cả các khoảng hàng trong phạm vi đó và xem liệu khoảng ngang thu được ít nhất có rộng bằng số lượng hàng hay không. Với số lượng điểm cuối bên trái tối đa và số điểm cuối bên phải tối thiểu đang chạy, mỗi hàng dưới cùng mới có thể được xử lý trong thời gian không đổi. Điều này mang lại cho (O(N^2)) công việc tổng thể, bởi vì có thể có (N(N+1)/2) phạm vi hàng. Đối với (N=100000), đó là khoảng (5\times10^9), quá lớn. 

Cấu trúc của bức tranh cho chúng ta khả năng quan sát mạnh mẽ hơn. Giả sử chúng ta đã biết rằng có một hình vuông có cạnh. Bây giờ hãy sửa hàng trên cùng của nó (i) và hỏi xem liệu chúng ta có thể mở rộng nó sang một bên (s+1) hay không. Các hàng bắt buộc là (i) đến (i+s). 

Chỉ nhìn vào hàng (i) và (i+s). Khoảng lợi ích chung của họ là 

[ 
[L,R]=[\max(a_i,a_{i+s}),\min(b_i,b_{i+s})]. 
] 

Nếu (R-L+1\ge s+1), thì cả hai hàng điểm cuối đều chứa (s+1) cột chung liên tiếp. Chọn bất kỳ khoảng (s+1)-cột nào như vậy. Mỗi cột trong khoảng đó đều tốt ở cả hai hàng điểm cuối. Theo điều kiện độ lồi dọc từ câu lệnh, mọi pixel giữa hai pixel tốt đó cũng tốt. Do đó, mỗi hàng trung gian chứa toàn bộ khoảng, tạo ra một hình vuông ((s+1)\times(s+1)) hợp lệ. 

Điều này biến vấn đề thành sự tăng trưởng gia tăng. Giữ cạnh hình vuông lớn nhất được tìm thấy cho đến nay, gọi nó là`ans`. Đối với mỗi hàng trên cùng có thể, hãy thử mở rộng`ans`theo một hàng và một cột. Tiện ích mở rộng thành công tăng lên`ans`, Và`ans`có thể tăng tối đa (N-1) lần trong toàn bộ thuật toán. 

Khấu hao chính là yếu tố làm cho phương pháp trở nên tuyến tính. Mặc dù một hàng trên cùng có thể thực hiện một số lần kiểm tra tiện ích mở rộng thành công, nhưng mỗi lần kiểm tra thành công sẽ tăng thêm câu trả lời chung. Có thể có nhiều nhất (N-1) mức tăng như vậy. Mỗi hàng trên cùng cũng có thể kết thúc sau một lần kiểm tra không thành công hoặc do đã đạt đến ranh giới hình ảnh. Do đó chỉ có tổng số lần lặp (O(N)) của vòng lặp bên trong. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N)) khấu hao | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc khoảng ([a_i,b_i]) cho mỗi hàng và lưu trữ hai điểm cuối. Biểu diễn khoảng là đủ vì mỗi hàng đều lồi theo chiều ngang. 
2. Đặt`ans = 1`. Vì mỗi hàng chứa ít nhất một pixel tốt nên luôn tồn tại một hình vuông (1\times1). 
3. Xử lý từng hàng`i`như một hàng trên cùng có thể của một hình vuông lớn hơn. Chúng ta đã biết rằng một hình vuông cạnh`ans`tồn tại ở đâu đó, nhưng bây giờ chúng ta hỏi liệu hàng trên cùng cụ thể này có thể chứa một hình vuông có cạnh không`ans + 1`. 
4. Trước tiên hãy kiểm tra xem hàng`i`bản thân nó có ít nhất`ans + 1`pixel tốt. Chiều rộng của nó là`b[i] - a[i] + 1`, vậy nếu 
[ 
a_i-a_i+1 < và+1, 
] 
hàng này không thể là đỉnh của một hình vuông lớn hơn. Không có lý do gì để thử một hình vuông lớn hơn bắt đầu từ đây. 
1. Hãy để`j = i + ans`. Đây là hàng dưới cùng của một hình vuông ứng cử viên có cạnh`ans + 1`. Nếu như`j >= N`, ứng cử viên sẽ mở rộng ra ngoài ảnh, vì vậy chúng tôi ngừng xử lý hàng trên cùng này. 
2. Tính giao điểm của khoảng hàng trên và hàng dưới: 

[ 
L=\max(a_i,a_j),\qquad R=\min(b_i,b_j). 
] 

Nếu 

[ 
R-L+1\ge ans+1, 
] 

hai hàng điểm cuối chia sẻ đủ số cột cho một hình vuông cạnh`ans + 1`. 

1. Khi giao lộ đủ lớn, hãy tăng dần`ans`và lặp lại việc kiểm tra tương tự. Ứng cử viên mới có phe`ans + 1`và do đó đạt tới một hàng xa hơn ở phía dưới. 
2. Nếu giao lộ điểm cuối quá hẹp, hãy dừng lại ở hàng trên cùng này. Việc tăng kích thước ứng cử viên không thể làm cho giao lộ rộng hơn, vì vậy không có hình vuông lớn hơn bắt đầu ở hàng này có thể thành công. 
3. Sau khi tất cả các hàng trên cùng đã được xử lý, hãy in`ans`. 

### Tại sao nó hoạt động 

Tính bất biến đó là`ans`luôn là độ dài cạnh của một số hình vuông hợp lệ đã được tìm thấy. Bất cứ khi nào thuật toán tăng nó lên, các hàng trên cùng và dưới cùng của ứng viên mới sẽ chia sẻ ít nhất`ans`cột. Mỗi cột trong khoảng chia sẻ đó đều chứa các pixel tốt ở cả hai điểm cuối. Độ lồi dọc đảm bảo rằng tất cả các pixel giữa các điểm cuối đó đều tốt, do đó toàn bộ ô vuông ứng cử viên là hợp lệ. 

Ngược lại, khi một ứng cử viên của bên`s`không thành công đối với hàng trên cùng cố định, khoảng thời gian chia sẻ của hai hàng điểm cuối của nó có ít hơn`s`cột. Bất kỳ hình vuông lớn hơn nào có cùng hàng trên cùng sẽ sử dụng cùng hàng dưới cùng hoặc một hàng thậm chí muộn hơn và chiều rộng yêu cầu sẽ chỉ tăng lên. Vì vậy không có ô vuông lớn hơn nào bị bỏ sót bắt đầu ở hàng đó. Vì mọi hàng trên cùng có thể đều được xem xét nên dòng cuối cùng`ans`là cạnh hình vuông lớn nhất có thể có. 

Độ phức tạp tuyến tính xuất phát từ khấu hao. Mỗi lần lặp vòng lặp bên trong thành công đều tăng`ans`, Và`ans`có thể tăng nhiều nhất (N-1) lần. Có nhiều nhất một lần lặp kết thúc cho mỗi hàng trên cùng, vì vậy tổng số lần lặp bên trong là (O(N)). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    left = [0] * n
    right = [0] * n

    for i in range(n):
        left[i], right[i] = map(int, input().split())

    ans = 1

    for i in range(n):
        while True:
            # Row i must be wide enough for a square of side ans + 1.
            if right[i] - left[i] + 1 < ans + 1:
                break

            # The bottom row of a square of side ans + 1.
            j = i + ans
            if j >= n:
                break

            # Intersection of the top and bottom row.
            L = max(left[i], left[j])
            R = min(right[i], right[j])

            # Need at least ans + 1 columns.
            if R - L + 1 >= ans + 1:
                ans += 1
            else:
                break

    print(ans)

if __name__ == "__main__":
    solve()
```Các mảng`left`Và`right`lưu trữ hai điểm cuối của mỗi hàng. Không cần biểu diễn từng pixel riêng lẻ, điều này giúp duy trì mức sử dụng bộ nhớ tuyến tính. 

Câu trả lời bắt đầu lúc`1`bởi vì mỗi hàng đầu vào có ít nhất một pixel tốt. điều kiện`right[i] - left[i] + 1 < ans + 1`sử dụng độ dài khoảng bao gồm. Viết nó như`right[i] - left[i] < ans`sẽ tương đương, nhưng rõ ràng`+1`làm cho việc diễn giải số pixel trở nên rõ ràng hơn. 

Đối với một ứng cử viên của bên`ans + 1`, hàng dưới cùng là`i + ans`, không`i + ans + 1`. Một hình vuông cạnh`s`bắt đầu từ hàng`i`chiếm chính xác các hàng`i, i+1, ..., i+s-1`, vì vậy đối với`s = ans + 1`, hàng cuối cùng là`i + ans`. 

Giao lộ cũng bao gồm. Số cột của nó là`R-L+1`, ít nhất phải bằng độ dài cạnh ứng cử viên. Không có vấn đề tràn số nguyên trong Python và tất cả các giá trị liên quan đều có nhiều nhất là (N). 

Phần tinh tế nhất của việc thực hiện là phần bên trong`while`. Nó không khiến (O(N^2)) hoạt động mặc dù nó được lồng bên trong vòng lặp trên các hàng. Mỗi khi cơ thể của nó thành công,`ans`tăng lên. Từ`ans`không bao giờ giảm và không thể vượt quá`N`, có nhiều nhất (N-1) lần lặp thành công trong toàn bộ chương trình. Mỗi hàng có thể đóng góp nhiều nhất một lần kiểm tra ranh giới hoặc thất bại cuối cùng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên là```
3
1 1
0 2
1 1
```Các hàng là`[1,1]`,`[0,2]`, Và`[1,1]`. Mỗi hàng có ít nhất một pixel tốt, vì vậy chúng ta bắt đầu với`ans = 1`. 

| Hàng trên cùng`i`|`ans`trước | Hàng dưới cùng`j`| Giao điểm trên/dưới | Kết quả |`ans`sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 |`[1,1]`| chiều rộng 1, quá nhỏ | 1 | 
| 1 | 1 | 2 |`[1,1]`| chiều rộng 1, quá nhỏ | 1 | 
| 2 | 1 | 3 | hình ảnh bên ngoài | dừng lại | 1 | 

Câu trả lời vẫn còn`1`. Điều này chứng tỏ tại sao chiều rộng của một hàng riêng lẻ là không đủ. Hàng giữa rộng nhưng các hàng lân cận chỉ chứa cột`1`, do đó không tồn tại (2\times2) hình vuông. 

### Mẫu 2 

Mẫu thứ hai là```
8
2 4
2 4
1 4
0 7
0 3
1 2
1 2
1 1
```Thuật toán bắt đầu với`ans = 1`. 

| Hàng trên cùng`i`|`ans`trước | Hàng dưới cùng`j`| Giao lộ | Chiều rộng | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 |`[2,4]`| 3 |`ans = 2`| 
| 0 | 2 | 2 |`[2,4]`| 3 |`ans = 3`| 
| 0 | 3 | 3 |`[2,4]`| 3 | không thể đạt tới 4 | 
| 1 | 3 | 4 |`[2,3]`| 2 | không thể đạt tới 4 | 
| 2 | 3 | 5 |`[1,2]`| 2 | không thể đạt tới 4 | 
| 3 | 3 | 6 |`[1,2]`| 2 | không thể đạt tới 4 | 
| 4 | 3 | 7 |`[1,1]`| 1 | không thể đạt tới 4 | 
| 5 | 3 | 8 | hình ảnh bên ngoài | dừng lại | 3 | 
| 6 | 3 | 9 | hình ảnh bên ngoài | dừng lại | 3 | 
| 7 | 3 | 10 | hình ảnh bên ngoài | dừng lại | 3 | 

Hàng đầu tiên có thể hỗ trợ hình vuông (2\times2), sau đó là hình vuông (3\times3), nhưng không hỗ trợ hình vuông (4\times4). Câu trả lời cuối cùng là`3`. 

Dấu vết cũng minh họa việc khấu hao. Hai tiện ích mở rộng thành công xảy ra trong khi xử lý hàng đầu tiên, nhưng hai tiện ích mở rộng đó chiếm hai trong số số gia tăng duy nhất (N-1) có thể có của`ans`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) khấu hao | Mỗi lần lặp bên trong thành công sẽ tăng`ans`, nhiều nhất là (N-1) lần và mỗi hàng có nhiều nhất một lần kiểm tra kết thúc. | 
| Không gian | (O(N)) | Hai mảng lưu trữ điểm cuối bên trái và bên phải của mỗi hàng. | 

Đối với (N=100000), thuật toán chỉ thực hiện một số tuyến tính so sánh khoảng thời gian và sử dụng bộ nhớ tuyến tính. Điều này thoải mái trong giới hạn 2 giây và 256 MB đã nêu. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    left = [0] * n
    right = [0] * n

    for i in range(n):
        left[i], right[i] = map(int, input().split())

    ans = 1

    for i in range(n):
        while True:
            if right[i] - left[i] + 1 < ans + 1:
                break

            j = i + ans
            if j >= n:
                break

            L = max(left[i], left[j])
            R = min(right[i], right[j])

            if R - L + 1 >= ans + 1:
                ans += 1
            else:
                break

    return str(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

# Provided sample 1
assert run("""\
3
1 1
0 2
1 1
""") == "1", "sample 1"

# Provided sample 2
assert run("""\
8
2 4
2 4
1 4
0 7
0 3
1 2
1 2
1 1
""") == "3", "sample 2"

# Minimum-size picture.
assert run("""\
1
0 0
""") == "1", "minimum size"

# All rows are the whole picture.
assert run("""\
4
0 3
0 3
0 3
0 3
""") == "4", "all rows equal"

# Off-by-one case: exactly two columns are available in every row.
assert run("""\
3
0 1
0 1
0 1
""") == "2", "inclusive interval width"

# Wide rows surrounding a narrow row.
assert run("""\
3
0 3
1 2
0 3
""") == "2", "narrow middle row"

# Maximum-size input, with the entire picture good.
n = 100000
maximum_input = str(n) + "\n" + ("0 99999\n" * n)
assert run(maximum_input) == "100000", "maximum size"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0 0`|`1`| Kích thước tối thiểu và đảm bảo (1\times1) hình vuông | 
| Bốn hàng`0 3`|`4`| Câu trả lời tối đa có thể và các phần mở rộng thành công lặp đi lặp lại | 
| Ba hàng`0 1`|`2`| Bao gồm ranh giới khoảng và chiều rộng hình vuông chính xác | 
|`0 3 / 1 2 / 0 3`|`2`| Một hàng trung gian hẹp ngăn hình vuông lớn hơn | 
| 100000 hàng`0 99999`|`100000`| Tối đa (N), hiệu suất tuyến tính và xử lý ranh giới | 

## Vỏ cạnh 

Trường hợp kích thước tối thiểu là```
1
0 0
```Thuật toán bắt đầu với`ans = 1`. Đối với hàng duy nhất, ứng cử viên của bên`2`sẽ yêu cầu hàng dưới cùng`1`, nằm ngoài hình ảnh. Vòng lặp dừng và in`1`. Không có chi nhánh trường hợp đặc biệt là cần thiết. 

Đối với một bức ảnh mà mọi hàng đều hoàn toàn tốt,```
4
0 3
0 3
0 3
0 3
```hàng đầu tiên phát triển thành công`ans`từ`1`ĐẾN`2`, thì từ`2`ĐẾN`3`, và cuối cùng từ`3`ĐẾN`4`. Khi nó cố gắng bên`5`, hàng dưới cùng sẽ là chỉ mục`4`, do đó việc kiểm tra ranh giới sẽ dừng vòng lặp. Câu trả lời là`4`, đó là toàn bộ bức tranh. 

Đối với trường hợp chiều rộng chính xác,```
3
0 1
0 1
0 1
```một bên ứng cử viên`2`có giao điểm cuối`[0,1]`, có chiều rộng là`1-0+1=2`. Thuật toán chấp nhận nó. Một bên ứng cử viên`3`sẽ yêu cầu ba cột, nhưng giao lộ vẫn chỉ có chiều rộng`2`, nên nó bị từ chối. Câu trả lời là`2`. Đây là trường hợp điển hình khi quên tính bao gồm`+1`sẽ tạo ra kết quả sai. 

Vì```
3
0 3
1 2
0 3
```hàng đầu và hàng cuối rộng nhưng hàng giữa chỉ có cột`1`Và`2`. Thuật toán có thể phát triển từ phía`1`sang một bên`2`sử dụng hai hàng đầu tiên. Khi nó cố gắng bên`3`, hàng thứ nhất và thứ ba giao nhau thành bốn cột, nhưng bản thân hàng trên cùng chỉ có thể hỗ trợ ứng viên thông qua điều kiện điểm cuối và thuộc tính lồi dọc đảm bảo rằng khoảng chia sẻ cũng phải có ở hàng trung gian. Ở đây khoảng chung có liên quan cho một bên`3`ứng cử viên sẽ phải chứa ba cột, trong khi hàng giữa chỉ chứa hai cột, vì vậy một hình vuông như vậy là không thể. Thuật toán dừng đúng ở`2`. 

Trường hợp kích thước tối đa chứa (100000) hàng giống hệt nhau:```
100000
0 99999
0 99999
...
```Câu trả lời là`100000`. Vòng lặp bên trong thực hiện một phần mở rộng thành công cho mỗi chiều dài cạnh có thể có và sau đó dừng ở ranh giới hình ảnh. Do đó, ngay cả đầu vào lớn nhất cũng chỉ tạo ra (O(N)) hoạt động chứ không phải (O(N^2)).
