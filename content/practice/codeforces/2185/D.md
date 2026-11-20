---
title: "CF 2185D - Lỗi hết bộ nhớ"
description: "Chúng ta bắt đầu với một mảng và một chuỗi các thao tác cập nhật. Mỗi thao tác thêm một số giá trị vào một vị trí. Phần bất thường là quy tắc sụp đổ. Sau mỗi lần cập nhật, nếu bất kỳ phần tử nào của mảng hiện tại lớn hơn h, máy tính sẽ gặp sự cố ngay lập tức."
date: "2026-06-07T21:31:04+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2185
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1074 (Div. 4)"
rating: 1100
weight: 2185
solve_time_s: 154
verified: true
draft: false
---

[CF 2185D - OutOfMemoryError](https://codeforces.com/problemset/problem/2185/D) 

**Đánh giá:** 1100 
**Tags:** cấu trúc dữ liệu, cách triển khai, toán học, hai con trỏ 
**Thời gian giải:** 2m 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một mảng và một chuỗi các thao tác cập nhật. Mỗi thao tác thêm một số giá trị vào một vị trí. 

Phần bất thường là quy tắc sụp đổ. Sau mỗi lần cập nhật, nếu bất kỳ phần tử nào của mảng hiện tại lớn hơn`h`, the computer immediately crashes. Khi điều đó xảy ra, toàn bộ mảng sẽ được đặt lại về trạng thái ban đầu chứ không phải về trạng thái trước thao tác cuối cùng. 

Chúng ta phải xử lý tất cả các thao tác theo thứ tự và xuất ra mảng cuối cùng sau mỗi lần áp dụng sự cố và thiết lập lại. 

Những hạn chế là điều đầu tiên hình thành nên giải pháp. Trên tất cả các trường hợp thử nghiệm, tổng số phần tử mảng và phép toán nhiều nhất là`2·10^5`. Điều đó có nghĩa là một`O(n + m)`hoặc`O((n+m) log n)`giải pháp dễ dàng đủ nhanh. Mặt khác, bất kỳ phương pháp nào quét toàn bộ mảng sau mỗi thao tác sẽ yêu cầu tới`O(nm)`công việc, trở thành về`4·10^10`hoạt động trong trường hợp xấu nhất và hoàn toàn không khả thi. 

Phần nguy hiểm nhất của vấn đề là hành vi thiết lập lại. Một sự cố không chỉ hoàn tác thao tác cuối cùng. Nó khôi phục mảng ban đầu từ đầu trường hợp thử nghiệm. 

Coi như:```
n = 2, h = 5
a = [1, 1]

operations:
(1, 3)
(1, 3)
```Sau lần cập nhật đầu tiên, chúng tôi nhận được`[4,1]`. 

Sau lần cập nhật thứ hai, chúng tôi nhận được`[7,1]`, gặp sự cố. 

Trạng thái đúng sẽ trở thành`[1,1]`, không`[4,1]`. 

Giải pháp chỉ hoàn nguyên thao tác cuối cùng sẽ tạo ra câu trả lời sai. 

Một trường hợp khó phát hiện khác là khi có một số bản cập nhật được tích lũy kể từ lần đặt lại gần đây nhất:```
a = [0]
h = 5

operations:
+2
+2
+2
```Các tiểu bang là:```
[2]
[4]
[6] -> crash -> [0]
```Câu trả lời cuối cùng là`[0]`. 

Việc kiểm tra tràn phải được thực hiện dựa trên giá trị tích lũy kể từ lần đặt lại cuối cùng, không phải đối với một thao tác đơn lẻ. 

Trường hợp cạnh thứ ba là các bản cập nhật với`c = 0`.```
a = [5]
h = 5

operation:
+0
```Giá trị vẫn chính xác`5`, điều này được cho phép vì sự cố chỉ xảy ra khi một giá trị lớn hơn rất nhiều`h`. 

Đầu ra chính xác vẫn còn`[5]`. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp nhất giữ mảng hiện tại một cách rõ ràng. Đối với mỗi thao tác, chúng tôi sửa đổi một vị trí và sau đó kiểm tra xem có phần tử nào vượt quá không`h`. Nếu xảy ra sự cố, chúng tôi sẽ khôi phục mảng ban đầu. 

Điều này đúng vì nó tuân theo câu nói theo nghĩa đen. 

Vấn đề là phát hiện sự cố. Sau mỗi thao tác chúng ta sẽ cần quét tất cả`n`các phần tử để xem liệu một số giá trị có vượt quá không`h`. Với tối đa`2·10^5`hoạt động và`2·10^5`các phần tử, điều này trở thành`O(nm)`trong trường hợp xấu nhất. 

Quan sát chính là chỉ có một vị trí thay đổi trong quá trình vận hành. 

Giả sử chúng ta lưu trữ, đối với mỗi chỉ mục, tổng số tiền được thêm vào kể từ lần đặt lại gần đây nhất. Cho phép```
delta[i] = additions applied to position i since the last crash
```Giá trị hiện tại tại vị trí`i`là```
a[i] + delta[i]
```Khi một thao tác thêm`c`để định vị`b`, chỉ có vị trí`b`những thay đổi. Mọi vị trí khác đều giữ nguyên giá trị như trước. 

Điều đó có nghĩa là sự cố chỉ có thể được kích hoạt bởi vị trí được cập nhật. Không cần phải kiểm tra toàn bộ mảng. 

Sau khi cập nhật`delta[b]`, chúng ta chỉ cần kiểm tra xem```
a[b] + delta[b] > h
```Nếu không, hoạt động thành công. 

Nếu nó lớn hơn`h`, xảy ra sự cố và mọi sửa đổi tích lũy kể từ lần đặt lại trước đó sẽ biến mất. Về mặt khái niệm tất cả`delta`các giá trị trở thành số không. 

Đặt lại toàn bộ chiều dài-`n`mảng mỗi lần sẽ lại đắt tiền. Chúng tôi cần một cách để xóa tất cả các bản cập nhật tích lũy trong thời gian liên tục. 

Bí quyết là coi các bản cập nhật giữa các sự cố là thuộc về một "thế hệ". Mỗi vị trí ghi nhớ thế hệ mà delta được lưu trữ của nó được cập nhật lần cuối. Khi xảy ra sự cố, chúng tôi sẽ tăng bộ đếm thế hệ. Bất kỳ đồng bằng cũ nào cũng tự động trở nên không phù hợp vì thế hệ của nó không còn phù hợp với thế hệ hiện tại. 

Đây là một kỹ thuật thiết lập lại lười biếng tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mảng ban đầu`a`. 
2. Tạo một mảng`delta`chiều dài`n`, ban đầu tất cả đều bằng không. 
3. Tạo một mảng`stamp`chiều dài`n`, ban đầu tất cả đều bằng không. 
4. Duy trì số thế hệ hiện tại`cur`, ban đầu`1`. 
5. Đối với một ca phẫu thuật`(b, c)`, chuyển thành`b`để lập chỉ mục dựa trên số không. 
6. Nếu`stamp[b] != cur`, thì vùng delta được lưu trữ thuộc về thế hệ cũ. Bộ`delta[b] = 0`Và`stamp[b] = cur`. 
7. Thêm`c`ĐẾN`delta[b]`. 
8. Kiểm tra xem`a[b] + delta[b] > h`. 
9. Nếu điều kiện sai, hãy tiếp tục thao tác tiếp theo. 
10. Nếu điều kiện đúng thì xảy ra sự cố. Tăng`cur`bởi một. Điều này sẽ xóa tất cả các vùng đồng bằng tích lũy một cách hợp lý vì mọi con tem hiện có giờ đây đều thuộc về thế hệ cũ hơn. 
11. Sau tất cả các thao tác, hãy xây dựng lại mảng cuối cùng. Đối với từng vị trí: 

- Nếu`stamp[i] == cur`, giá trị của nó là`a[i] + delta[i]`. 
- Ngược lại, giá trị của nó chỉ đơn giản là`a[i]`. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mảng hiện tại chỉ khác với mảng ban đầu ở những cập nhật được thực hiện kể từ sự cố gần đây nhất. Đối với mỗi chỉ số,`delta[i]`lưu trữ chính xác tổng số bản cập nhật còn sót lại. 

Bộ đếm thế hệ đảm bảo rằng tất cả các bản cập nhật từ các sự cố trước đó đều bị bỏ qua. Việc tăng thế hệ tương đương với việc đặt mọi delta về 0 nhưng không chạm vào toàn bộ mảng. 

Vì một thao tác chỉ sửa đổi một vị trí nên chỉ có vị trí đó mới có thể vượt quá`h`. Kiểm tra`a[b] + delta[b]`là đủ để xác định liệu một vụ tai nạn có xảy ra hay không. Thuật toán luôn duy trì trạng thái chính xác được mô tả trong câu lệnh bài toán, do đó mảng được xây dựng lại cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n, m, h = map(int, input().split())
        a = list(map(int, input().split()))

        delta = [0] * n
        stamp = [0] * n
        cur = 1

        for _ in range(m):
            b, c = map(int, input().split())
            b -= 1

            if stamp[b] != cur:
                delta[b] = 0
                stamp[b] = cur

            delta[b] += c

            if a[b] + delta[b] > h:
                cur += 1

        ans = []
        for i in range(n):
            if stamp[i] == cur:
                ans.append(str(a[i] + delta[i]))
            else:
                ans.append(str(a[i]))

        out.append(" ".join(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Mảng`a`không bao giờ thay đổi vì nó thể hiện trạng thái ban đầu mà mọi sự cố đều được đặt lại. 

Cặp đôi`(delta, stamp)`chỉ lưu trữ các bản cập nhật từ thế hệ hiện tại. Bất cứ khi nào một chỉ mục được chạm vào sau một sự cố, thông tin cũ của nó sẽ bị loại bỏ một cách lười biếng bằng cách kiểm tra tem. 

Phần tế nhị nhất là xử lý sự cố. Khi xảy ra sự cố, chúng tôi không sửa đổi bất kỳ yếu tố nào của`delta`hoặc`stamp`. Chúng tôi chỉ tăng`cur`. Điều này có tác dụng vì mọi bản cập nhật được lưu trữ ngay lập tức trở thành một phần của thế hệ cũ và bị bỏ qua kể từ thời điểm đó trở đi. 

Một điểm tinh tế khác là thứ tự thực hiện các thao tác. Trước tiên, chúng tôi áp dụng bản cập nhật, sau đó kiểm tra xem giá trị có vượt quá không`h`. Điều này khớp với câu lệnh, nơi xảy ra sự cố sau khi thao tác được thực hiện. 

Số nguyên Python có độ chính xác tùy ý, vì vậy các giá trị lên tới khoảng`2·10^14`không gây ra vấn đề tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, m = 4, h = 5
a = [1, 2, 1]

operations:
(1,4)
(2,4)
(3,3)
(2,0)
```| Bước | Hoạt động | đồng bằng | Thế hệ | Tai nạn? | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | [0,0,0] | 1 | Không | 
| 1 | (1,4) | [4,0,0] | 1 | Không | 
| 2 | (2,4) | [4,4,0] | 1 | Có, 2+4=6 | 
| Đặt lại | - | không thay đổi về mặt thể chất | 2 | - | 
| 3 | (3,3) | [4,4,3] | 2 | Không | 
| 4 | (2,0) | [4,0,3] | 2 | Không | 

Quá trình tái thiết cuối cùng chỉ sử dụng các bản cập nhật thế hệ 2:```
[1, 2, 4]
```Ví dụ này cho thấy tại sao các bản cập nhật cũ phải biến mất hoàn toàn sau một sự cố. Các bổ sung được lưu trữ trước thế hệ 2 sẽ bị bỏ qua. 

### Ví dụ 2 

đầu vào:```
n = 1, m = 3, h = 5
a = [0]

operations:
(1,2)
(1,2)
(1,2)
```| Bước | Giá trị hiện tại | Thế hệ | Tai nạn? | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | 1 | Không | 
| +2 | 2 | 1 | Không | 
| +2 | 4 | 1 | Không | 
| +2 | 6 | 1 | Có | 

Sau sự cố, thế hệ trở thành 2 và tất cả các bản cập nhật tích lũy đều biến mất. 

Câu trả lời cuối cùng:```
0
```Dấu vết này chứng tỏ rằng một sự cố sẽ đặt lại toàn bộ lịch sử kể từ lần đặt lại trước đó, chứ không chỉ hoạt động gây ra lỗi tràn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi thao tác và mỗi bước tái thiết cuối cùng đều có thời gian không đổi | 
| Không gian | O(n) | Mảng`delta`Và`stamp`được lưu trữ trên mỗi vị trí | 

Tổng cộng`n + m`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2·10^5`, do đó giải pháp tổng thể chỉ thực hiện công tuyến tính. Điều này thoải mái phù hợp trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, m, h = map(int, input().split())
        a = list(map(int, input().split()))

        delta = [0] * n
        stamp = [0] * n
        cur = 1

        for _ in range(m):
            b, c = map(int, input().split())
            b -= 1

            if stamp[b] != cur:
                delta[b] = 0
                stamp[b] = cur

            delta[b] += c

            if a[b] + delta[b] > h:
                cur += 1

        ans = []
        for i in range(n):
            if stamp[i] == cur:
                ans.append(str(a[i] + delta[i]))
            else:
                ans.append(str(a[i]))

        out.append(" ".join(ans))

    return "\n".join(out)

# provided sample
assert run(
"""3
3 4 5
1 2 1
1 4
2 4
3 3
2 0
5 3 1
1 1 1 1 1
1 1
1 1
2 1
4 4 1
1 0 0 0
1 1
4 4
3 3
4 4
"""
) == (
"""1 2 4
1 1 1 1 1
1 0 0 0"""
)

# minimum size
assert run(
"""1
1 1 5
0
1 3
"""
) == "3"

# exact boundary, no crash
assert run(
"""1
1 1 5
2
1 3
"""
) == "5"

# repeated crashes
assert run(
"""1
1 3 1
1
1 1
1 1
1 1
"""
) == "1"

# crash after accumulation
assert run(
"""1
1 3 5
0
1 2
1 2
1 2
"""
) == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố đơn lẻ, một bản cập nhật |`3`| Ví dụ kích thước tối thiểu | 
| Giá trị đạt chính xác`h`|`5`| Tràn là nghiêm ngặt`>`| 
| Sự cố lặp đi lặp lại |`1`| Thay đổi nhiều thế hệ | 
| Ba bản cập nhật tích lũy |`0`| Sự cố phụ thuộc vào giá trị tích lũy | 

## Vỏ cạnh 

### Crash phải reset về mảng ban đầu 

đầu vào:```
1
2 2 5
1 1
1 3
1 3
```Sau thao tác đầu tiên, mảng là`[4,1]`. 

Sau thao tác thứ hai, nó trở thành`[7,1]`, gặp sự cố. 

Thuật toán tăng bộ đếm thế hệ. Tất cả các bản cập nhật được lưu trữ đều trở nên lỗi thời, do đó mảng cuối cùng được xây dựng lại thành:```
[1,1]
```Điều này phù hợp với hành vi đặt lại về ban đầu được yêu cầu. 

### Cho phép giá trị bằng h 

đầu vào:```
1
1 1 5
2
1 3
```Giá trị cập nhật trở nên chính xác`5`. 

Thuật toán kiểm tra:```
2 + 3 > 5
```đó là sai. 

Không có sự cố nào xảy ra và câu trả lời là:```
5
```sử dụng`>= h`sẽ kích hoạt thiết lập lại không chính xác. 

### Cập nhật tích lũy gây tràn 

đầu vào:```
1
1 3 5
0
1 2
1 2
1 2
```Thứ tự các giá trị là:```
2
4
6
```Hoạt động thứ ba vượt quá`h`, do đó bộ đếm thế hệ tiến lên và tất cả các cập nhật tích lũy sẽ biến mất. 

Mảng được xây dựng lại là:```
0
```Điều này xác nhận rằng thuật toán theo dõi tổng mức đóng góp kể từ lần đặt lại gần đây nhất, không chỉ hoạt động gần đây nhất.
