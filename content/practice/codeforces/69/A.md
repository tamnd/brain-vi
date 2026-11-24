---
title: "CF 69A - Nhà vật lý trẻ"
description: "Chúng ta được cho một số vectơ lực tác dụng lên vật thể trong không gian ba chiều. Mỗi vectơ có ba thành phần: hiệu ứng của nó dọc theo trục x, trục y và trục z. Một vật chỉ ở trạng thái cân bằng khi tổng lực tác dụng lên nó bằng 0 theo mọi hướng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 69
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 63 (Div. 2)"
rating: 1000
weight: 69
solve_time_s: 103
verified: true
draft: false
---

[CF 69A - Nhà vật lý trẻ](https://codeforces.com/problemset/problem/69/A) 

**Đánh giá:** 1000 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số vectơ lực tác dụng lên vật thể trong không gian ba chiều. Mỗi vectơ có ba thành phần: hiệu ứng của nó dọc theo trục x, trục y và trục z. 

Một vật chỉ ở trạng thái cân bằng khi tổng lực tác dụng lên nó bằng 0 theo mọi hướng. Điều đó có nghĩa là: 

- tổng của tất cả các thành phần x phải bằng 0 
- tổng của tất cả các thành phần y phải bằng 0 
- tổng của tất cả các thành phần z phải bằng 0 

Nhiệm vụ đơn giản là đọc tất cả các vectơ, cộng tọa độ của chúng một cách độc lập và kiểm tra xem cả ba tổng có bằng không hay không. Nếu có, hãy in`"YES"`. Ngược lại, in`"NO"`. 

Những hạn chế là rất nhỏ. Tối đa 100 vectơ được đưa ra và mỗi tọa độ nằm trong khoảng từ -100 đến 100. Ngay cả một giải pháp kém hiệu quả cũng dễ dàng nằm gọn trong giới hạn. Tổng tuyệt đối lớn nhất có thể có dọc theo một trục chỉ là 100 × 100 = 10000, nên số học số nguyên thông thường là hoàn toàn an toàn. 

Thách thức chính không phải là hiệu suất mà là chuyển chính xác điều kiện vật lý thành mã. Một lỗi phổ biến là kiểm tra xem mỗi vectơ riêng lẻ có bằng nhau không`(0,0,0)`. Điều đó không bắt buộc. Các vectơ khác 0 có thể triệt tiêu lẫn nhau. 

Ví dụ:```
2
1 2 3
-1 -2 -3
```Đầu ra đúng là:```
YES
```Tổng lực là`(0,0,0)`mặc dù không có vectơ nào bằng 0. 

Một trường hợp khó phát hiện khác là khi chỉ có một trục cân bằng.```
3
1 0 0
-1 5 0
0 -3 0
```Trục x có tổng bằng 0, nhưng trục y có tổng bằng 2. Kết quả đúng là:```
NO
```Việc triển khai bất cẩn chỉ kiểm tra một tọa độ sẽ thất bại ở đây. 

Ngoài ra còn có trường hợp kích thước tối thiểu:```
1
0 0 0
```Đầu ra đúng là:```
YES
```Chỉ với một vectơ, trạng thái cân bằng chỉ xảy ra nếu bản thân vectơ đó bằng 0. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là vũ lực. Chúng ta có thể cố gắng tính tổng lực bằng cách kiểm tra từng vectơ và cộng tọa độ của nó vào tổng chạy. Sau khi xử lý tất cả các vectơ, chúng tôi kiểm tra xem tổng cuối cùng có bằng 0 hay không. 

Điều này đã hoạt động hiệu quả vì kích thước đầu vào rất nhỏ. Chúng tôi thực hiện chính xác ba phép cộng cho mỗi vectơ, vì vậy ngay cả trong trường hợp xấu nhất, chúng tôi chỉ thực hiện khoảng 300 phép tính số học. 

Quan sát quan trọng là trạng thái cân bằng chỉ phụ thuộc vào tác dụng tổng hợp của tất cả các lực chứ không phụ thuộc vào thứ tự hoặc độ lớn riêng lẻ của chúng. Phép cộng vectơ có tính kết hợp và giao hoán nên chúng ta không bao giờ cần lưu trữ các vectơ sau khi xử lý chúng. Chúng tôi chỉ cần tổng cộng ba lần. 

Điều đó làm giảm vấn đề để duy trì:```
sum_x
sum_y
sum_z
```Khi chúng ta đọc từng vectơ`(x, y, z)`, chúng tôi cập nhật những tổng số này. Cuối cùng: 

- nếu cả ba tổng bằng 0 thì cơ thể cân bằng 
- nếu không thì một lực nào đó vẫn còn tồn tại nên vật sẽ chuyển động 

Mặc dù các cách tiếp cận tối ưu và bạo lực trông giống nhau ở đây, nhưng việc tối ưu hóa quan trọng là nhận ra rằng chúng ta chỉ cần bộ nhớ bổ sung liên tục và một lần chuyển qua đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`, số lượng vectơ lực. 
2. Khởi tạo ba biến:`sum_x = 0`,`sum_y = 0`, Và`sum_z = 0`. 

Các biến này biểu thị tổng lực tích lũy dọc theo mỗi trục. 
3. Lặp lại`n`lần: 

- Đọc một vectơ`(x, y, z)`. 
- Thêm vào`x`ĐẾN`sum_x`. 
- Thêm vào`y`ĐẾN`sum_y`. 
- Thêm vào`z`ĐẾN`sum_z`. 

Mỗi vectơ đóng góp độc lập vào lực ròng dọc theo mọi trục. 
4. Sau khi xử lý tất cả các vectơ, hãy kiểm tra xem cả ba tổng có bằng 0 hay không. 

Vật chỉ ở trạng thái cân bằng khi không còn lực theo bất kỳ hướng nào. 
5. In`"YES"`nếu tất cả các tổng bằng 0. Nếu không thì in`"NO"`. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến sau khi xử lý dữ liệu đầu tiên`i`vectơ, các biến`sum_x`,`sum_y`, Và`sum_z`bằng tổng lực đóng góp của những người đó`i`vectơ dọc theo mỗi trục. 

Phép cộng vectơ hoạt động theo thành phần, do đó lực ròng tổng thể là:$$\left(\sum x_i,\ \sum y_i,\ \sum z_i\right)$$Một vật ở trạng thái cân bằng chính xác khi vectơ kết quả này bằng`(0,0,0)`. Vì thuật toán tính toán các khoản tiền này một cách chính xác nên lần kiểm tra cuối cùng luôn đưa ra câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

sum_x = 0
sum_y = 0
sum_z = 0

for _ in range(n):
    x, y, z = map(int, input().split())

    sum_x += x
    sum_y += y
    sum_z += z

if sum_x == 0 and sum_y == 0 and sum_z == 0:
    print("YES")
else:
    print("NO")
```Chương trình bắt đầu bằng cách đọc số lượng vectơ. Ba biến tích lũy được khởi tạo để theo dõi tổng lực dọc theo mỗi trục. 

Bên trong vòng lặp, mỗi vectơ được đọc và ngay lập tức được thêm vào tổng số đang chạy. Các vectơ không bao giờ cần phải được lưu trữ vì chỉ có tổng tích lũy mới quan trọng. 

Điều kiện cuối cùng kiểm tra đồng thời cả ba tọa độ. Điều này rất quan trọng vì trạng thái cân bằng đòi hỏi toàn bộ tổng vectơ phải bằng 0 chứ không chỉ một tọa độ. 

Tràn số nguyên không phải là vấn đề đáng lo ngại trong Python, nhưng ngay cả trong các ngôn ngữ có chiều rộng cố định, các giới hạn vẫn đủ nhỏ để vừa vặn thoải mái bên trong các kiểu số nguyên tiêu chuẩn. 

Việc thực hiện cũng tránh được các cấu trúc dữ liệu không cần thiết. Việc sử dụng bộ nhớ không đổi giúp giải pháp đơn giản và phù hợp trực tiếp với định nghĩa toán học về trạng thái cân bằng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
4 1 7
-2 4 -1
1 -5 -3
```| Bước | Vectơ | tổng_x | tổng_y | tổng_z | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | 0 | 0 | 0 | 
| 1 | (4, 1, 7) | 4 | 1 | 7 | 
| 2 | (-2, 4, -1) | 2 | 5 | 6 | 
| 3 | (1, -5, -3) | 3 | 0 | 3 | 

Tổng vectơ cuối cùng là`(3,0,3)`, không bằng không. Cơ thể vẫn chịu một lực ròng, vì vậy kết quả đầu ra chính xác là:```
NO
```Ví dụ này chứng minh rằng ngay cả khi một trục cân bằng hoàn hảo thì các trục khác cũng phải cân bằng. 

### Ví dụ 2 

đầu vào:```
3
3 -1 7
-5 2 -4
2 -1 -3
```| Bước | Vectơ | tổng_x | tổng_y | tổng_z | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | 0 | 0 | 0 | 
| 1 | (3, -1, 7) | 3 | -1 | 7 | 
| 2 | (-5, 2, -4) | -2 | 1 | 3 | 
| 3 | (2, -1, -3) | 0 | 0 | 0 | 

Cả ba tổng đều trở thành 0 sau khi xử lý mọi vectơ. 

Đầu ra đúng là:```
YES
```Dấu vết này xác nhận tính bất biến rằng tổng hoạt động luôn biểu thị lực ròng từ tất cả các vectơ được xử lý cho đến nay. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vectơ được xử lý một lần | 
| Không gian | O(1) | Chỉ có ba biến tích lũy được lưu trữ | 

Với tối đa 100 vectơ, chương trình sẽ chạy gần như ngay lập tức. Việc sử dụng bộ nhớ không đổi vì thuật toán không bao giờ lưu trữ danh sách đầy đủ các vectơ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())

    sx = sy = sz = 0

    for _ in range(n):
        x, y, z = map(int, input().split())
        sx += x
        sy += y
        sz += z

    if sx == 0 and sy == 0 and sz == 0:
        print("YES")
    else:
        print("NO")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run(
"""3
4 1 7
-2 4 -1
1 -5 -3
"""
) == "NO\n", "sample 1"

# minimum-size equilibrium
assert run(
"""1
0 0 0
"""
) == "YES\n", "single zero vector"

# minimum-size non-equilibrium
assert run(
"""1
5 -3 2
"""
) == "NO\n", "single nonzero vector"

# vectors cancel each other
assert run(
"""2
1 2 3
-1 -2 -3
"""
) == "YES\n", "perfect cancellation"

# only one axis unbalanced
assert run(
"""3
1 0 0
-1 5 0
0 -3 0
"""
) == "NO\n", "y-axis remains nonzero"

# boundary values
assert run(
"""2
100 100 100
-100 -100 -100
"""
) == "YES\n", "boundary coordinate values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đơn`(0,0,0)`vectơ | CÓ | Trường hợp cân bằng kích thước tối thiểu | 
| Vectơ đơn khác 0 | KHÔNG | Không cân bằng kích thước tối thiểu | 
| Hai vectơ đối diện | CÓ | Hủy bỏ trên tất cả các trục | 
| Một trục vẫn khác 0 | KHÔNG | Cả ba tọa độ đều phải cân bằng | 
| Giá trị tọa độ biên | CÓ | Xử lý đúng các đầu vào cực đoan | 

## Vỏ cạnh 

Hãy xem xét trường hợp các vectơ triệt tiêu lẫn nhau mặc dù không có vectơ nào bằng 0. 

đầu vào:```
2
1 2 3
-1 -2 -3
```Dấu vết thực hiện: 

- Sau vectơ đầu tiên:`(1,2,3)`- Sau vectơ thứ hai:`(0,0,0)`Thuật toán in`"YES"`vì tổng lực sẽ bằng không. Điều này khẳng định rằng trạng thái cân bằng phụ thuộc vào tổng các vectơ chứ không phụ thuộc vào từng vectơ riêng lẻ. 

Bây giờ hãy xem xét trường hợp chỉ có một tọa độ khác 0. 

đầu vào:```
3
1 0 0
-1 5 0
0 -3 0
```Dấu vết thực hiện: 

- Sau vectơ đầu tiên:`(1,0,0)`- Sau vectơ thứ hai:`(0,5,0)`- Sau vectơ thứ ba:`(0,2,0)`Trục x cân bằng, nhưng tổng trục y vẫn`2`. Thuật toán in chính xác`"NO"`bởi vì trạng thái cân bằng đòi hỏi mọi tổng tọa độ phải đồng thời bằng 0. 

Cuối cùng, hãy xem xét đầu vào hợp lệ nhỏ nhất. 

đầu vào:```
1
0 0 0
```Dấu vết thực hiện: 

- Sau khi xử lý vector duy nhất:`(0,0,0)`Thuật toán in`"YES"`ngay lập tức. Điều này xác nhận việc xử lý đúng ranh giới ràng buộc dưới.
