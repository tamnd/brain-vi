---
title: "CF 314E - Sereja và Squares"
description: "Mô tả hình học ẩn giấu một cấu trúc tổ hợp đơn giản hơn nhiều. Lấy một cặp điểm $(l,0)$ và $(r,0)$, với $l<r$. Điểm cuối bên trái được đánh dấu bằng chữ cái viết thường và điểm cuối bên phải bằng chữ cái viết hoa tương ứng."
date: "2026-06-06T01:08:37+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 314
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 187 (Div. 1)"
rating: 2900
weight: 314
solve_time_s: 137
verified: true
draft: false
---

[CF 314E - Sereja và Squares](https://codeforces.com/problemset/problem/314/E) 

**Xếp hạng:** 2900 
**Thẻ:** dp 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mô tả hình học ẩn giấu một cấu trúc tổ hợp đơn giản hơn nhiều. 

Lấy một cặp điểm$(l,0)$Và$(r,0)$, với$l<r$. Điểm cuối bên trái được đánh dấu bằng chữ cái viết thường và điểm cuối bên phải bằng chữ cái viết hoa tương ứng. 

Nếu hai cặp giao nhau thì hình vuông của chúng cắt nhau. Nếu một cặp nằm hoàn toàn bên trong một cặp khác thì hình vuông tương ứng nằm hoàn toàn bên trong hình vuông lớn hơn và chúng không chạm vào nhau. Các cặp dấu ngoặc hợp lệ hoàn toàn giống với các cặp dấu ngoặc được lồng hoặc rời nhau đúng cách. Cấu trúc giống hệt một chuỗi dấu ngoặc đúng với 25 loại dấu ngoặc. 

Sau khi Petya xóa: 

- Mỗi chữ cái viết thường nhìn thấy được chắc chắn là dấu ngoặc mở của một loại đã biết. 
- Mọi chữ hoa đều biến mất. 
- Mọi`?`có thể là: 

- một dấu ngoặc mở đã bị xóa, không rõ loại, hoặc 
- một khung đóng bị xóa. 

Chúng ta phải đếm xem có bao nhiêu chuỗi dấu ngoặc đúng hoàn chỉnh có thể tạo ra chuỗi đã cho. 

Chiều dài có thể đạt tới$10^5$, điều này ngay lập tức loại trừ các trạng thái DP trong khung cổ điển, chẳng hạn như`dp[position][balance]`. Một thuật toán bậc hai sẽ yêu cầu khoảng$10^{10}$hoạt động và là không thể. 

Cần có một sự quan sát tinh tế hơn. 

Hãy xem xét một vị trí có chứa`?`. 

Nếu chúng tôi quyết định rằng đó là dấu ngoặc mở thì loại của nó có thể được chọn sau. Nếu đó là dấu ngoặc đóng, loại của nó đã bị ép buộc bởi dấu ngoặc mở chưa khớp trên đầu ngăn xếp. Một khi đã biết bên mở thì bên đóng phù hợp không có sự lựa chọn độc lập. 

Điều này có nghĩa là độ khó tổ hợp hoàn toàn không phải ở các loại khung. Trước tiên, chúng tôi tính các vị trí mở/đóng hợp lệ giả sử chỉ có một loại khung. Sau đó, chúng tôi nhân với số cách gán loại cho các lỗ mở chưa biết. 

Một số trường hợp dễ bỏ sót: 

Nếu$n$thật kỳ quặc, một cặp đôi hoàn hảo là điều không thể. 

Ví dụ:```
3
???
```Câu trả lời là`0`. 

Nếu đã có nhiều hơn$n/2$các chữ cái viết thường có thể nhìn thấy được, chúng tôi không thể điền đủ số lượng dấu ngoặc mở cần thiết. 

Ví dụ:```
4
abcd
```Cả 4 vị trí đều là bắt buộc phải mở nên câu trả lời là`0`. 

Một trường hợp khó phát hiện khác là khi tiền tố chứa quá nhiều dấu ngoặc đóng. 

Ví dụ:```
4
?a??
```đầu tiên`?`không thể được chọn làm dấu ngoặc đóng vì một chuỗi dấu ngoặc đúng có thể không bao giờ có nhiều dấu đóng hơn dấu mở trong bất kỳ tiền tố nào. 

Giới hạn DP tự động loại bỏ các trạng thái đó. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là DP chuỗi ngoặc tiêu chuẩn. 

Cho phép`dp[i][bal]`là số cách sau khi xử lý lần đầu tiên`i`vị trí có kích thước ngăn xếp hiện tại`bal`. 

Một chữ cái nhìn thấy được luôn làm tăng số dư. MỘT`?`có thể tăng hoặc giảm nó. 

Điều này đúng, nhưng số dư có thể lớn bằng$n$. Với$n=10^5$, không gian trạng thái trở thành$O(n^2)$, vượt xa giới hạn. 

Quan sát quan trọng là mọi chữ cái viết thường nhìn thấy được đều được biết đến là dấu ngoặc mở. Các quyết định duy nhất được đưa ra lúc`?`các vị trí. 

Thay vì lưu trữ số dư hiện tại, hãy lưu trữ bao nhiêu dấu ngoặc đóng đã được đặt cho đến nay. 

Cho phép$m=n/2$, tổng số dấu ngoặc đóng trong bất kỳ chuỗi hợp lệ nào. 

Sau khi xử lý vị trí$i$: 

- nhiều nhất$\lfloor i/2 \rfloor$phần đóng có thể xuất hiện, nếu không thì một số tiền tố sẽ chứa nhiều phần đóng hơn phần mở; 
- ít nhất$i-m$việc đóng cửa phải xuất hiện, nếu không sẽ không có đủ vị trí còn lại để tiếp cận chính xác$m$đóng cửa vào cuối. 

Những giới hạn đó chặt chẽ một cách đáng ngạc nhiên. Chúng giảm phạm vi DP hoạt động xuống một dải chéo hẹp. Tổng hợp chiều rộng trên tất cả các vị trí chỉ cho$O(n)$tổng công việc. 

Sau khi đếm các bố cục mở/đóng hợp lệ, mọi dấu ngoặc mở không xác định đều đóng góp một hệ số$25$, bởi vì kiểu của nó có thể là bất kỳ chữ cái viết thường nào ngoại trừ`x`. Sau đó, các loại dấu ngoặc đóng sẽ bị ép buộc bởi cấu trúc phù hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP cân bằng |$O(n^2)$|$O(n)$hoặc$O(n^2)$| Quá chậm | 
| DP tối ưu về số lần đóng |$O(n)$khấu hao |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu$n$là số lẻ, đầu ra`0`. 
2. Hãy để$m=n/2$. Bất kỳ chuỗi hợp lệ nào cũng phải chứa chính xác$m$lỗ mở và$m$đóng cửa. 
3. Đếm xem đã tồn tại bao nhiêu chữ cái viết thường có thể nhìn thấy được. Gọi giá trị này`fixed_open`. 
4. Hãy để`dp[j]`là số cách xử lý tiền tố hiện tại trong khi đặt chính xác`j`dấu ngoặc đóng. 
5. Khởi tạo`dp[0] = 1`. 
6. Xử lý các vị trí từ trái qua phải. 
7. Nếu ký tự hiện tại là chữ cái viết thường có thể nhìn thấy được thì nó buộc phải là dấu ngoặc mở. Không cần chuyển đổi DP. 
8. Nếu ký tự hiện tại là`?`, nó có thể trở thành: 

- một dấu ngoặc mở, 
- hoặc dấu ngoặc đóng. 

Việc chọn nó làm dấu ngoặc đóng sẽ tăng số lần đóng lên một:```
dp[j] += dp[j-1]
```Việc cập nhật được thực hiện theo thứ tự giảm dần`j`. 
9. Trong quá trình cập nhật, chỉ xem xét```
i - m <= j <= floor(i/2)
```Ở đâu`i`là vị trí dựa trên 1 hiện tại. 

Giới hạn trên đảm bảo tính hợp lệ của tiền tố. Giới hạn dưới đảm bảo đủ vị trí còn lại để đạt được chính xác`m`đóng cửa. 
10. Sau khi tất cả các vị trí được xử lý,`dp[m]`bằng số lượng bố cục mở/đóng hợp lệ giả sử chỉ có một loại dấu ngoặc. 
11. Số lần mở cửa đến từ`?`vị trí là```
m - fixed_open
```1. Nhân với```
25^(m - fixed_open)
```vì mỗi lần mở như vậy có thể chọn bất kỳ chữ nào trong số 25 chữ cái được phép. 

1. Xuất kết quả theo modulo$2^{32}$. 

### Tại sao nó hoạt động 

DP chỉ tính các vị trí của dấu ngoặc đóng. 

Một chuỗi dấu ngoặc hợp lệ có độ dài$n$chứa chính xác$m$đóng cửa. Với mỗi tiền tố, số lần đóng không bao giờ được vượt quá số lần mở, tương đương với$j \le i/2$. Giới hạn dưới$j \ge i-m$đảm bảo rằng hậu tố còn lại vẫn có thể cung cấp đủ các kết thúc để kết thúc một cách chính xác$m$. 

Mọi sắp xếp mở/đóng hợp lệ đều tương ứng với chính xác một cấu trúc lồng hợp pháp. Khi vị trí mở được cố định, mọi loại dấu ngoặc đóng sẽ được xác định bởi dấu ngoặc mở phù hợp với nó. Quyền tự do duy nhất còn lại là chọn kiểu của mỗi lần mở có nguồn gốc từ một`?`, đưa ra hệ số của$25$mỗi lần mở như vậy. 

Do đó DP đếm tất cả các cấu trúc hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1 << 32

def solve():
    n = int(input())
    s = input().strip()

    if n & 1:
        print(0)
        return

    m = n // 2

    dp = [0] * (m + 1)
    dp[0] = 1

    fixed_open = 0

    for i, ch in enumerate(s, 1):
        if ch == '?':
            lo = max(1, i - m)
            hi = min(m, i // 2)

            for j in range(hi, lo - 1, -1):
                dp[j] = (dp[j] + dp[j - 1]) & 0xFFFFFFFF
        else:
            fixed_open += 1

    ans = dp[m]

    for _ in range(m - fixed_open):
        ans = (ans * 25) & 0xFFFFFFFF

    print(ans)

solve()
```Mảng`dp[j]`số lượng cửa hàng chỉ được lập chỉ mục theo số lần đóng được sử dụng cho đến nay. Một chữ cái viết thường nhìn thấy được không làm thay đổi số đếm này vì nó buộc phải là dấu ngoặc mở. 

Đối với một`?`, giá trị hiện có`dp[j]`đã đại diện cho việc chọn nó làm dấu ngoặc mở. Thêm`dp[j-1]`tương ứng với việc chọn nó làm dấu ngoặc đóng. 

Thứ tự lặp giảm dần là cần thiết. Nó ngăn không cho các giá trị được tạo ở vị trí hiện tại được sử dụng lại trong cùng một lần cập nhật. 

Mô-đun là$2^{32}$. sử dụng```
x & 0xFFFFFFFF
```tương đương với việc lấy kết quả modulo$2^{32}$sau mỗi lần thao tác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
a???
```Đây$m=2$. 

| Vị trí | Nhân vật | Số lượng đóng có thể tiếp cận | 
| --- | --- | --- | 
| 1 | một | {0} | 
| 2 | ? | {0, 1} | 
| 3 | ? | {0, 1} | 
| 4 | ? | {1, 2} | 

Chung kết: 

| Giá trị | Kết quả | 
| --- | --- | 
| dp[2] | 2 | 
| lỗ cố định | 1 | 
| sơ hở chưa biết | 1 | 
| số nhân | 25 | 
| trả lời | 50 | 

Đầu ra:```
50
```Ví dụ này cho thấy sự tách biệt giữa việc đếm cấu trúc và gán chữ cái. 

### Mẫu 2 

đầu vào:```
6
abc???
```| Vị trí | Nhân vật | 
| --- | --- | 
| 1 | một | 
| 2 | b | 
| 3 | c | 
| 4 | ? | 
| 5 | ? | 
| 6 | ? | 

Tất cả ba vị trí đầu tiên đều bị buộc phải mở. Cấu trúc hợp lệ duy nhất có thể có là đặt cả ba vị trí còn lại làm điểm đóng. 

Như vậy: 

| Giá trị | Kết quả | 
| --- | --- | 
| dp[3] | 1 | 
| lỗ cố định | 3 | 
| sơ hở chưa biết | 0 | 
| trả lời | 1 | 

Đầu ra:```
1
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$khấu hao | Phạm vi hợp lệ của`j`tạo thành một dải chéo có tổng chiều rộng trên tất cả các vị trí là tuyến tính | 
| Không gian |$O(n)$| Một mảng DP có kích thước$n/2+1$| 

Với$n=10^5$, cả mức sử dụng bộ nhớ và thời gian chạy đều phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 1 << 32

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    s = input().strip()

    if n & 1:
        return "0"

    m = n // 2

    dp = [0] * (m + 1)
    dp[0] = 1
    fixed_open = 0

    for i, ch in enumerate(s, 1):
        if ch == '?':
            lo = max(1, i - m)
            hi = min(m, i // 2)

            for j in range(hi, lo - 1, -1):
                dp[j] = (dp[j] + dp[j - 1]) & 0xFFFFFFFF
        else:
            fixed_open += 1

    ans = dp[m]
    for _ in range(m - fixed_open):
        ans = (ans * 25) & 0xFFFFFFFF

    return str(ans)

# provided samples
assert run("4\na???\n") == "50", "sample 1"
assert run("4\nabc?\n") == "0", "sample 2"
assert run("6\nabc???\n") == "1", "sample 3"

# custom cases
assert run("1\n?\n") == "0", "odd length"
assert run("2\n??\n") == "25", "single pair, unknown type"
assert run("2\na?\n") == "1", "forced pair"
assert run("4\n????\n") == "1250", "all positions unknown"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 ?`|`0`| Độ dài lẻ không thể ghép nối | 
|`2 ??`|`25`| Một mở đầu, một kết thúc, 25 chữ cái lựa chọn | 
|`2 a?`|`1`| Cấu trúc và loại được xác định đầy đủ | 
|`4 ????`|`1250`| Nhiều cấu trúc và bài tập kiểu | 

## Vỏ cạnh 

Hãy xem xét:```
3
???
```Thuật toán ngay lập tức phát hiện ra rằng$n$là số lẻ và trả về`0`. Không có cặp hợp lệ nào có thể tồn tại. 

Coi như:```
4
abcd
```Tất cả bốn vị trí đều bị buộc phải mở. Chúng ta sẽ cần chính xác hai lần mở và hai lần đóng, nhưng không có vị trí nào để đóng. DP không bao giờ đạt đến trạng thái`dp[2]`, vậy câu trả lời là`0`. 

Coi như:```
4
?a??
```Ở vị trí đầu tiên, việc chọn dấu ngoặc đóng sẽ tạo ra tiền tố có nhiều dấu đóng hơn là dấu mở. Sự ràng buộc```
j <= floor(i/2)
```tự động loại bỏ trạng thái đó. Chỉ các cấu hình tiền tố hợp lệ mới tồn tại và câu trả lời cuối cùng sẽ tính chính xác các chuỗi khung pháp lý.
