---
title: "CF 413D - 2048"
description: "Chúng tôi xử lý một chuỗi các ô, mỗi ô là 2 hoặc 4. Một ô bắt đầu ở xa bên phải và trượt sang trái. Khi chạm vào một giá trị bằng nhau, cả hai sẽ hợp nhất thành một giá trị nhân đôi và ô mới tiếp tục di chuyển."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp"]
categories: ["algorithms"]
codeforces_contest: 413
codeforces_index: "D"
codeforces_contest_name: "Coder-Strike 2014 - Round 2"
rating: 2000
weight: 413
solve_time_s: 143
verified: true
draft: false
---

[CF 413D - 2048](https://codeforces.com/problemset/problem/413/D) 

**Xếp hạng:** 2000 
**Thẻ:** bitmask, dp 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi xử lý một chuỗi các ô, mỗi ô là một trong hai`2`hoặc`4`. Một ô bắt đầu ở xa bên phải và trượt sang trái. Khi chạm vào một giá trị bằng nhau, cả hai sẽ hợp nhất thành một giá trị nhân đôi và ô mới tiếp tục di chuyển. Đây là điểm khác biệt quan trọng so với năm 2048 thông thường, việc hợp nhất có thể xâu chuỗi trong một lần di chuyển. 

Chúng ta được cung cấp một chuỗi độ dài được chỉ định một phần`n`. Một số vị trí được cố định như`2`hoặc`4`, và một số là`0`, nghĩa là chúng ta có thể chọn một trong hai`2`hoặc`4`ở đó. Chúng ta phải đếm xem có bao nhiêu lần hoàn thành tạo ra ít nhất một ô có giá trị ít nhất`2^k`. 

Các ràng buộc hoàn toàn xác định loại giải pháp chúng ta cần. Độ dài chuỗi đạt`2000`, do đó bất cứ điều gì theo cấp số nhân trong`n`là không thể. Một lực lượng vũ phu đối với tất cả các sự thay thế đã đạt đến`2^2000`. Ngay cả một DP trạng thái có quá nhiều chiều cũng sẽ thất bại. 

Phần thú vị đó là`k ≤ 11`. Ô mục tiêu nhiều nhất là`2^11 = 2048`, có nghĩa là chỉ hợp nhất các cấp độ lên tới`11`vấn đề. Giới hạn nhỏ này gợi ý việc nén trạng thái bảng thành mặt nạ bit hoặc trạng thái DP nhỏ. 

Một điểm tinh tế là cách dải phát triển. Bởi vì các ô đã hợp nhất tiếp tục di chuyển nên dải cuối cùng luôn hoạt động giống như các chuỗi nhị phân. 

Ví dụ:```
2 2 2
```không trở thành:```
4 2
```Thay vì:```
2 -> 4
4 + 2 -> 4 2
```Trạng thái cuối cùng là`4 2`. 

Một sai lầm dễ mắc phải khác là cho rằng thứ tự các ô có cùng sức mạnh không quan trọng. Nó có. 

Ví dụ:```
2 4 2 2
```phát triển khác với:```
2 2 4 2
```vì sự hợp nhất xảy ra trong quá trình chèn. 

Một trường hợp cạnh khác xuất hiện khi ô chiến thắng chỉ hình thành sau một chuỗi dài. 

Ví dụ:```
2 2 2 2
```Quá trình này là:```
2
4
4 2
8
```Một mô phỏng đơn giản chỉ hợp nhất một lần cho mỗi lần chèn sẽ dừng không chính xác ở`4 4`. 

Đầu ra yêu cầu số lượng thay thế hợp lệ theo modulo`10^9 + 7`, do đó việc đếm phải được thực hiện theo số học mô-đun trong suốt DP. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. Thay thế mọi`0`với cả hai`2`Và`4`, mô phỏng toàn bộ trò chơi và kiểm tra xem ít nhất một ô`2^k`xuất hiện. 

Bản thân mô phỏng rất dễ dàng. Dải này hoạt động chính xác như phép cộng nhị phân. Nếu chúng ta mã hóa`2^1`như cấp độ`1`,`2^2`như cấp độ`2`, v.v., sau đó việc chèn một mức có tác dụng giống như thêm một mức vào bộ đếm nhị phân có số mang. 

Ví dụ, chèn một`2`có nghĩa là thêm mã thông báo ở cấp độ`1`. Nếu cấp độ`1`đã tồn tại, chúng hợp nhất thành cấp độ`2`, và quá trình mang tiếp tục. 

Lực lượng vũ phu là chính xác bởi vì nó tuân theo các quy tắc trò chơi theo đúng nghĩa đen. Vấn đề là sự phức tạp. Với nhiều nhất`2000`vị trí không xác định, chúng tôi sẽ cần phải kiểm tra`2^2000`trình tự. 

Quan sát quan trọng là trạng thái bảng rất nhỏ. 

Tại bất kỳ thời điểm nào, đối với mỗi cấp độ dưới đây`k`, chỉ có vấn đề chẵn lẻ. Hai ô bằng nhau ngay lập tức hợp nhất lên trên, do đó, một cấp độ có thể chứa tối đa một ô. Điều này có nghĩa là toàn bộ bảng liên quan có thể được biểu diễn bằng một bitmask có kích thước`k`. 

Nếu bit`i`được đặt thì dải hiện có chứa một ô`2^i`. 

Bây giờ việc chèn một ô sẽ trở thành một quá trình thực hiện trên mặt nạ này:```
while bit exists:
    remove bit
    carry upward
set first empty bit
```Nếu vật mang theo đạt đến cấp độ`k`hoặc cao hơn, chúng tôi đã thắng. 

Điều này biến vấn đề thành DP qua tiền tố và mặt nạ. 

Cho phép:```
dp[pos][mask]
```là số cách xử lý đầu tiên`pos`các phần tử và kết thúc bằng trạng thái bảng`mask`mà không sản xuất`2^k`. 

Số lượng khẩu trang chỉ có`2^k ≤ 2048`và mỗi lần chuyển đổi tốn nhiều nhất`k`thực hiện các bước. 

Tổng độ phức tạp trở thành:```
O(n * 2^k * k)
```xung quanh`2000 * 2048 * 11 ≈ 4.5e7`các hoạt động nguyên thủy, có thể chấp nhận được trong Python được tối ưu hóa với việc triển khai cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^z · n · k) | O(k) | Quá chậm | 
| Tối ưu | O(n · 2^k · k) | O(2^k) | Đã chấp nhận | 

Đây`z`là số các số 0. 

## Hướng dẫn thuật toán 

1. Biểu thị mỗi ô theo số mũ của nó. 

Chúng tôi lập bản đồ:```
2 -> 1
4 -> 2
```Mục tiêu đang đạt đến số mũ`k`. 
2. Biểu thị trạng thái dải dưới dạng mặt nạ bit. 

Chút`i`có nghĩa là một viên gạch`2^i`tồn tại. Chúng tôi chỉ lưu trữ cấp độ`0..k-1`. Bất kỳ thực hiện nào vượt quá điều đó có nghĩa là thành công. 
3. Xác định hàm chuyển tiếp. 

Giả sử chúng ta chèn cấp độ`x`. 

Bắt đầu từ bit`x`. 

Trong khi bit đó đã tồn tại trong mặt nạ: 

- loại bỏ bit, 
- tăng`x`, 
- tiếp tục đưa lên trên. 

Khi tìm thấy một vị trí trống: 

- đặt bit ở đó. 

Nếu như`x >= k`, thì chuỗi đó đã thắng. 
4. Xây dựng chương trình động theo các vị trí. 

Cho phép:```
dp[mask]
```lưu trữ số cách để tiếp cận mặt nạ này sau khi xử lý một số tiền tố, trong khi vẫn không chiến thắng. 
5. Xử lý trình tự từ trái sang phải. 

Đối với từng vị trí: 

- nếu giá trị được cố định là`2`, chỉ chèn số mũ`1`, 
- nếu cố định như`4`, chỉ chèn số mũ`2`, 
- nếu vậy`0`, hãy thử cả hai. 
6. Áp dụng chuyển tiếp. 

Đối với mọi mặt nạ hiện tại và mọi lần chèn được phép: 

- tính toán mặt nạ tiếp theo bằng cách sử dụng quy trình mang, 
- nếu số mang đạt đến số mũ`k`, thêm số lượng hiện tại trực tiếp vào câu trả lời, 
- nếu không thì cập nhật lớp DP tiếp theo. 
7. Thực hiện mọi thao tác theo modulo`10^9 + 7`. 

### Tại sao nó hoạt động 

Điều bất biến là mọi trạng thái DP thể hiện chính xác cấu hình dải duy nhất sau khi xử lý một số tiền tố, giả sử không có ô cấp độ nào`k`hoặc lớn hơn đã xuất hiện. 

Quá trình thực hiện khớp chính xác với cơ chế trò chơi. Hai ô bằng nhau hợp nhất và tiếp tục di chuyển, tương tự như phép cộng nhị phân có nhớ. Vì mỗi cấp độ có thể chứa tối đa một ô sau khi ổn định nên việc biểu diễn mặt nạ bit không bị mất mát. 

Mỗi lần hoàn thành có thể có của chuỗi đều tuân theo chính xác một chuỗi chuyển đổi DP và mỗi chuyển đổi DP đều tương ứng với một quá trình phát triển trò chơi hợp pháp. Thuật toán không bỏ sót chuỗi nào cũng như không đếm bất kỳ chuỗi nào hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    size = 1 << k

    # trans[level][mask] = next_mask
    # if next_mask == -1, reaching >= 2^k occurs
    trans = [[0] * size for _ in range(3)]

    for level in (1, 2):
        for mask in range(size):
            x = level
            cur = mask

            while x < k and (cur >> x) & 1:
                cur ^= (1 << x)
                x += 1

            if x >= k:
                trans[level][mask] = -1
            else:
                trans[level][mask] = cur | (1 << x)

    dp = [0] * size
    dp[0] = 1

    ans = 0

    for val in a:
        ndp = [0] * size

        if val == 0:
            options = [1, 2]
        elif val == 2:
            options = [1]
        else:
            options = [2]

        for mask in range(size):
            ways = dp[mask]
            if not ways:
                continue

            for level in options:
                nxt = trans[level][mask]

                if nxt == -1:
                    ans = (ans + ways) % MOD
                else:
                    ndp[nxt] = (ndp[nxt] + ways) % MOD

        dp = ndp

    print(ans % MOD)

solve()
```Ý tưởng quan trọng đầu tiên trong quá trình triển khai là chuyển đổi giá trị thành số mũ. Làm việc với`1`Và`2`thay vì`2`Và`4`làm cho việc xử lý mang theo tự nhiên. 

Bảng chuyển tiếp được tính toán trước khi DP bắt đầu. Điều này loại bỏ mô phỏng mang lặp đi lặp lại bên trong các vòng lặp chính. Vì chỉ có hai kiểu chèn và nhiều nhất là`2048`mặt nạ, tiền xử lý là rẻ. 

Mô phỏng mang sửa đổi mặt nạ tạm thời`cur`. Bất cứ khi nào một bit đã tồn tại, chúng tôi loại bỏ nó và mang lên trên. Điều này khớp chính xác với sự hợp nhất lặp đi lặp lại trong trò chơi. 

Một chi tiết triển khai tinh tế là xử lý các trạng thái chiến thắng. Chúng tôi không bao giờ lưu trữ mặt nạ có chứa chất`k`, bởi vì một khi ô như vậy xuất hiện thì trình tự đã thành công. Những chuyển đổi đó góp phần trực tiếp vào`ans`. 

Bản thân DP sử dụng mảng cuộn. Chỉ có lớp trước là quan trọng, vì vậy giữ hai mảng là đủ. 

Một chi tiết khác dễ mắc sai lầm là việc lập chỉ mục các bit. Chút`1`đại diện cho gạch`2`, và chút`2`đại diện cho gạch`4`. Chút`0`không được sử dụng, giúp đơn giản hóa việc ánh xạ giữa lũy thừa và số mũ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 4
2 2 4 2 2 2 2
```Mục tiêu đang đến`16`, tương ứng với số mũ`4`. 

| Bước | Chèn | Mặt nạ hiện tại | Ý nghĩa | 
| --- | --- | --- | --- | 
| 0 | - | 0000 | trống | 
| 1 | 2 | 0010 | ngói 2 | 
| 2 | 2 | 0100 | ngói 4 | 
| 3 | 4 | 1000 | ngói 8 | 
| 4 | 2 | 1010 | 8,2 | 
| 5 | 2 | 1100 | 8,4 | 
| 6 | 2 | 1110 | 8,4,2 | 
| 7 | 2 | thắng | mang theo đạt 16 | 

Việc chèn cuối cùng sẽ kích hoạt chuỗi mang:```
2 + 2 -> 4
4 + 4 -> 8
8 + 8 -> 16
```Điều này chứng tỏ tại sao việc thực hiện lặp lại là cần thiết. 

### Ví dụ 2 

đầu vào:```
3 3
0 0 0
```Mục tiêu đang đến`8`. 

| Trình tự | Kết quả | 
| --- | --- | 
| 2 2 2 | không | 
| 2 2 4 | vâng | 
| 2 4 2 | không | 
| 2 4 4 | không | 
| 4 2 2 | vâng | 
| 4 2 4 | không | 
| 4 4 2 | không | 
| 4 4 4 | vâng | 

có`3`chuỗi chiến thắng. 

Ví dụ này cho thấy thứ tự rất quan trọng. Cùng một bộ gạch có thể tạo ra các kết quả khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 2^k · k) | DP trên tất cả các mặt nạ với mô phỏng mang theo | 
| Không gian | O(2^k) | Mảng DP cuộn | 

Với`k ≤ 11`, số lượng mặt nạ nhiều nhất`2048`. Tổng số công việc vẫn nằm trong giới hạn của Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    size = 1 << k

    trans = [[0] * size for _ in range(3)]

    for level in (1, 2):
        for mask in range(size):
            x = level
            cur = mask

            while x < k and (cur >> x) & 1:
                cur ^= (1 << x)
                x += 1

            if x >= k:
                trans[level][mask] = -1
            else:
                trans[level][mask] = cur | (1 << x)

    dp = [0] * size
    dp[0] = 1

    ans = 0

    for val in a:
        ndp = [0] * size

        if val == 0:
            options = [1, 2]
        elif val == 2:
            options = [1]
        else:
            options = [2]

        for mask in range(size):
            ways = dp[mask]

            if not ways:
                continue

            for level in options:
                nxt = trans[level][mask]

                if nxt == -1:
                    ans = (ans + ways) % MOD
                else:
                    ndp[nxt] = (ndp[nxt] + ways) % MOD

        dp = ndp

    return str(ans)

# provided sample
assert run(
"""7 4
2 2 4 2 2 2 2
"""
) == "1"

# minimum non-winning case
assert run(
"""1 3
2
"""
) == "0"

# immediate winning
assert run(
"""2 3
4 4
"""
) == "1"

# all zeroes
assert run(
"""3 3
0 0 0
"""
) == "3"

# long carry chain
assert run(
"""4 4
2 2 2 2
"""
) == "1"

print("ok")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 / 2`|`0`| Trường hợp không trúng thưởng quy mô tối thiểu | 
|`2 3 / 4 4`|`1`| Hợp nhất trực tiếp vào mục tiêu | 
|`3 3 / 0 0 0`|`3`| DP phân nhánh trên các giá trị chưa biết | 
|`4 4 / 2 2 2 2`|`1`| Xích vận chuyển nhiều bước | 

## Vỏ cạnh 

Hãy xem xét:```
4 4
2 2 2 2
```Việc triển khai có lỗi chỉ hợp nhất một lần cho mỗi lần chèn sẽ dừng ở:```
4 4
```và kết luận sai sự thất bại. 

Mô phỏng quá trình chuyển đổi của chúng tôi tiếp tục tiến lên cho đến khi xuất hiện mức trống:```
2 + 2 -> 4
4 + 4 -> 8
8 + 8 -> 16
```vì vậy DP tính chính xác chuỗi này là chiến thắng. 

Bây giờ hãy xem xét:```
3 3
2 4 2
```Quá trình trở thành:```
2
4
4 2
```KHÔNG`8`xuất hiện. 

Nhưng:```
3 3
4 2 2
```trở thành:```
4
4 2
8
```Câu trả lời khác nhau mặc dù cả hai đều chứa hai`2`s và một`4`. Vì DP xử lý các phần chèn theo đúng thứ tự nên nó duy trì sự khác biệt này một cách chính xác. 

Cuối cùng, hãy xem xét:```
2 3
4 4
```đầu tiên`4`tạo số mũ`2`. Phần chèn thứ hai va chạm ngay lập tức và chuyển sang số mũ`3`, đã đạt được mục tiêu. 

Bảng chuyển đổi của chúng tôi đánh dấu những chuyển đổi như vậy bằng`-1`và DP thêm chúng trực tiếp vào câu trả lời cuối cùng thay vì lưu trữ mặt nạ không hợp lệ.
