---
title: "CF 1269B - Bình đẳng Modulo"
description: "Chúng tôi có hai mảng có độ dài bằng nhau. Chúng ta có thể chọn một giá trị x không âm, thêm nó vào mọi phần tử của mảng đầu tiên và lấy tất cả các kết quả theo modulo m. Sau sự chuyển đổi này, thứ tự của các phần tử không còn quan trọng nữa."
date: "2026-06-11T20:14:41+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1269
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 609 (Div. 2)"
rating: 1500
weight: 1269
solve_time_s: 118
verified: true
draft: false
---

[CF 1269B - Bình đẳng Modulo](https://codeforces.com/problemset/problem/1269/B) 

**Đánh giá:** 1500 
**Tags:** vũ lực, phân loại 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai mảng có độ dài bằng nhau. Chúng ta có thể chọn một giá trị không âm`x`, thêm nó vào mọi phần tử của mảng đầu tiên và lấy tất cả các kết quả theo modulo`m`. 

Sau sự chuyển đổi này, thứ tự của các phần tử không còn quan trọng nữa. Chúng ta được phép sắp xếp lại mảng đã biến đổi một cách tùy ý. Nhiệm vụ là tìm số nhỏ nhất`x`trong đó multiset được chuyển đổi trở nên giống hệt như multiset được biểu thị bằng mảng thứ hai. 

Chi tiết quan trọng là chúng tôi đang so sánh nhiều tập hợp chứ không phải vị trí. Nếu hai mảng chứa cùng một giá trị với cùng tần số thì chúng được coi là bằng nhau sau một số hoán vị. 

Những ràng buộc được bộc lộ một cách bất thường. Độ dài mảng tối đa là 2000, trong khi`m`có thể lớn như`10^9`. Một giải pháp lặp lại trên tất cả các giá trị có thể có của`x`là không thể vì không gian tìm kiếm quá lớn. Mặt khác,`n = 2000`đủ nhỏ để một`O(n² log n)`giải pháp hoàn toàn có thể chấp nhận được. Khoảng tám triệu hoạt động có thể được quản lý trong thời gian giới hạn. 

Một số trường hợp nguy hiểm có thể dễ dàng phá vỡ việc triển khai bất cẩn. 

Xem xét các giá trị trùng lặp:```
n = 3, m = 5
a = [0, 0, 0]
b = [2, 2, 2]
```Câu trả lời đúng là`2`. 

Một giải pháp giả định tất cả các phần tử đều khác biệt và cố gắng khớp các vị trí thay vì tần số sẽ không thành công. 

Số học modulo bao quanh là một nguồn sai lầm khác:```
n = 2, m = 10
a = [8, 9]
b = [0, 1]
```Câu trả lời đúng là`2`bởi vì:```
(8 + 2) mod 10 = 0
(9 + 2) mod 10 = 1
```Sử dụng phép trừ thông thường mà không xử lý modulo sẽ tạo ra sự dịch chuyển âm không chính xác. 

Một trường hợp tinh vi hơn xảy ra khi một số thay đổi có vẻ hợp lý:```
n = 3, m = 7
a = [0, 1, 2]
b = [1, 2, 3]
```Cả hai mảng đều đã được sắp xếp và ca hợp lệ là`1`. Chiến lược kết hợp tham lam không xác minh toàn bộ nhiều tập hợp có thể chấp nhận một ứng cử viên khác một cách không chính xác. 

Bài toán đảm bảo rằng có ít nhất một câu trả lời hợp lệ, nhưng chúng ta vẫn phải tìm ra câu trả lời nhỏ nhất. 

## Phương pháp tiếp cận 

Một ý tưởng đơn giản là thử mọi ca có thể`x`từ`0`ĐẾN`m - 1`. Đối với mỗi ứng cử viên, hãy biến đổi mọi yếu tố của`a`, sắp xếp mảng kết quả và so sánh nó với phiên bản đã sắp xếp của`b`. 

Điều này hoạt động vì điều kiện chỉ phụ thuộc vào nhiều giá trị. Nếu mảng biến đổi được sắp xếp bằng mảng mục tiêu được sắp xếp thì tồn tại một số hoán vị. 

Vấn đề là kích thước của`m`. Từ`m`có thể đạt được`10^9`, việc kiểm tra từng ca là hoàn toàn không khả thi. 

Quan sát quan trọng đến từ việc phân loại. 

Cho phép:```
A = sorted(a)
B = sorted(b)
```Giả sử có sự thay đổi nào đó`x`là hợp lệ. Sau khi nộp đơn`x`modulo`m`, phần tử nhỏ nhất của`B`phải bắt nguồn từ một yếu tố nào đó của`A`. 

Cho rằng`A[i]`trở thành phần tử nhỏ nhất`B[0]`. Sau đó:```
(A[i] + x) mod m = B[0]
```ngụ ý```
x = (B[0] - A[i] + m) mod m
```Đối với mọi vị trí`i`, công thức này tạo ra một sự dịch chuyển có thể xảy ra. chỉ có`n`những ứng viên như vậy. 

Tại sao thế là đủ? Bởi vì trong bất kỳ phép biến đổi hợp lệ nào, một số phần tử của`A`phải ánh xạ tới giá trị tối thiểu trong`B`. Sự thay đổi tương ứng sẽ xuất hiện trong số này`n`ứng viên. 

Bây giờ vấn đề trở nên nhỏ hơn nhiều. Tạo tất cả các ca ứng cử viên bắt nguồn từ việc khớp một phần tử của`A`ĐẾN`B[0]`. Với mỗi ứng viên, biến đổi mọi giá trị của`A`, sắp xếp kết quả và so sánh với`B`. 

Trong số tất cả các ứng cử viên hợp lệ, trả về mức tối thiểu. 

Vì chỉ có`n`ứng viên và mỗi chi phí xác minh`O(n log n)`, tổng độ phức tạp trở thành`O(n² log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m · n log n) | O(n) | Quá chậm | 
| Tối ưu | O(n² log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`, và hai mảng. 
2. Sắp xếp cả hai mảng và lưu trữ chúng dưới dạng`A`Và`B`. 
3. Đối với mọi chỉ số`i`TRONG`A`, tính độ dịch chuyển ứng viên```
x = (B[0] - A[i] + m) mod m
```Điều này tương ứng với việc buộc`A[i]`để trở thành giá trị nhỏ nhất trong multiset mục tiêu. 
4. Áp dụng sự thay đổi ứng cử viên này cho mọi phần tử của`A`:```
transformed[j] = (A[j] + x) mod m
```5. Sắp xếp mảng đã chuyển đổi. 
6. So sánh mảng đã biến đổi đã sắp xếp với`B`. 

Nếu chúng bằng nhau thì sự dịch chuyển này là hợp lệ. 
7. Giữ ca hợp lệ nhỏ nhất gặp phải. 
8. Xuất ra sự dịch chuyển tối thiểu đó. 

### Tại sao nó hoạt động 

Giả sử`x*`là câu trả lời đúng 

Sau khi nộp đơn`x*`, các giá trị được chuyển đổi tạo thành chính xác nhiều tập hợp được biểu thị bằng`B`. Giá trị nhỏ nhất của`B`, cụ thể là`B[0]`, phải đến từ một phần tử nào đó`A[i]`. 

Đối với phần tử đó,```
(A[i] + x*) mod m = B[0]
```sắp xếp lại để```
x* = (B[0] - A[i] + m) mod m
```Như vậy`x*`là một trong những ứng cử viên được tạo ra. 

Mọi ứng cử viên đều được kiểm tra bằng cách xây dựng rõ ràng multiset đã chuyển đổi và so sánh nó với`B`. Một ứng cử viên được chấp nhận khi và chỉ khi nó tạo ra chính xác nhiều tập hợp được yêu cầu. 

Vì mọi ca hợp lệ đều được kiểm tra và chúng tôi trả về giá trị hợp lệ nhỏ nhất nên thuật toán luôn tạo ra câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    a.sort()
    b.sort()

    answer = m

    for v in a:
        x = (b[0] - v) % m

        transformed = sorted((num + x) % m for num in a)

        if transformed == b:
            answer = min(answer, x)

    print(answer)

if __name__ == "__main__":
    solve()
```Bước đầu tiên là sắp xếp cả hai mảng vì vấn đề thực sự là so sánh nhiều mảng. Sau khi được sắp xếp, đẳng thức của nhiều tập hợp trở thành đẳng thức mảng thông thường. 

Vòng lặp kết thúc`a`tạo ra tất cả các ca ứng cử viên. Mỗi ứng cử viên xuất phát từ việc quan sát rằng một số phần tử của mảng nguồn được sắp xếp phải ánh xạ tới phần tử tối thiểu của mảng đích được sắp xếp. 

Đối với mỗi ứng cử viên, mã sẽ biến đổi mọi phần tử bằng cách sử dụng số học mô-đun. biểu hiện```
(num + x) % m
```phải được sử dụng chính xác trong hình thức này. Việc quên thao tác modulo sẽ thất bại trong các trường hợp bao quanh. 

Các giá trị chuyển đổi được sắp xếp trước khi so sánh. Ngay cả khi phép biến đổi đúng, số học modulo có thể thay đổi thứ tự của các phần tử, do đó việc so sánh mà không sắp xếp sẽ không chính xác. 

Biến`answer`lưu trữ ca hợp lệ tối thiểu được tìm thấy cho đến nay. Vấn đề yêu cầu sự thay đổi hợp lệ nhỏ nhất, không chỉ đơn thuần là bất kỳ sự thay đổi hợp lệ nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
0 0 2 1
2 0 1 1
```Sau khi sắp xếp:```
A = [0, 0, 1, 2]
B = [0, 1, 1, 2]
```| Giá trị nguồn ứng viên | x | Mảng được sắp xếp đã chuyển đổi | hợp lệ | 
| --- | --- | --- | --- | 
| 0 | 0 | [0,0,1,2] | Không | 
| 0 | 0 | [0,0,1,2] | Không | 
| 1 | 2 | [0,0,2,2] | Không | 
| 2 | 1 | [0,1,1,2] | Có | 

Sự thay đổi hợp lệ nhỏ nhất là`1`. 

Ví dụ này cho thấy tại sao chúng ta phải kiểm tra tất cả các ứng viên. Một vài ca đầu tiên có vẻ hợp lý nhưng không thành công khi kiểm tra toàn bộ nhiều tập hợp. 

### Ví dụ 2 

đầu vào:```
3 10
8 9 0
0 1 2
```Sau khi sắp xếp:```
A = [0, 8, 9]
B = [0, 1, 2]
```| Giá trị nguồn ứng viên | x | Mảng được sắp xếp đã chuyển đổi | hợp lệ | 
| --- | --- | --- | --- | 
| 0 | 0 | [0,8,9] | Không | 
| 8 | 2 | [0,1,2] | Có | 
| 9 | 1 | [0,0,9] | Không | 

Trả lời:```
2
```Ví dụ này thể hiện hành vi bao bọc của số học modulo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n² log n) | Lên đến`n`các ca ứng cử viên, mỗi ca yêu cầu một loại`n`giá trị chuyển đổi | 
| Không gian | O(n) | Mảng chuyển đổi tạm thời | 

Với`n ≤ 2000`, trường hợp xấu nhất là đại khái:```
2000 × 2000 × log2(2000)
```hoạt động phù hợp một cách thoải mái trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    a.sort()
    b.sort()

    ans = m

    for v in a:
        x = (b[0] - v) % m
        cur = sorted((y + x) % m for y in a)
        if cur == b:
            ans = min(ans, x)

    return str(ans)

# provided sample
assert run(
"""4 3
0 0 2 1
2 0 1 1
"""
) == "1", "sample 1"

# minimum size
assert run(
"""1 100
37
37
"""
) == "0", "single element"

# all equal values
assert run(
"""3 5
0 0 0
2 2 2
"""
) == "2", "duplicates"

# wrap-around modulo
assert run(
"""2 10
8 9
0 1
"""
) == "2", "modulo wrap"

# smallest valid answer among multiple candidates
assert run(
"""3 7
0 1 2
1 2 3
"""
) == "1", "minimum valid shift"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng phần tử đơn | 0 | Ranh giới kích thước tối thiểu | 
| Tất cả các giá trị bằng nhau | 2 | Xử lý đúng các bản sao | 
| Vỏ bọc xung quanh | 2 | Tính chính xác số học mô-đun | 
| Chuyển từng cái một | 1 | Lựa chọn câu trả lời hợp lệ nhỏ nhất | 

## Vỏ cạnh 

Xem xét các giá trị trùng lặp:```
3 5
0 0 0
2 2 2
```Sau khi sắp xếp:```
A = [0,0,0]
B = [2,2,2]
```Mỗi ứng cử viên tạo ra:```
x = (2 - 0) mod 5 = 2
```Việc chuyển đổi mang lại:```
[2,2,2]
```cái nào phù hợp`B`. Thuật toán so sánh nhiều tập hợp chứ không phải vị trí, do đó các bản sao được xử lý một cách tự nhiên. 

Hãy xem xét số học bao quanh:```
2 10
8 9
0 1
```Ứng viên xuất thân từ`8`là:```
x = (0 - 8 + 10) mod 10 = 2
```Áp dụng ca:```
(8 + 2) mod 10 = 0
(9 + 2) mod 10 = 1
```Multiset được chuyển đổi trở nên chính xác`[0,1]`. Sự khác biệt tiêu cực không bao giờ gây ra vấn đề vì tất cả các ca ứng cử viên đều được tính toán theo modulo`m`. 

Hãy xem xét trường hợp một ứng cử viên không chính xác có vẻ hợp lý:```
3 7
0 1 2
1 2 3
```Ứng viên được tạo từ`A[0]`là:```
x = 1
```thành công. 

Một ứng cử viên khác có thể căn chỉnh chính xác một phần tử nhưng tạo ra một tập hợp đa biến đổi khác với`B`. Vì mọi ứng cử viên đều được xác minh bằng cách so sánh toàn bộ các mảng đã được sắp xếp nên các kết quả khớp một phần không thể được chấp nhận một cách ngẫu nhiên.
