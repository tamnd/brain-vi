---
title: "CF 1501B - Bánh Napoléon"
description: "Chúng tôi xây dựng từng lớp bánh. Sau khi đặt lớp thứ i, chúng ta đổ một [i] đơn vị kem lên trên. Lớp kem đó lan xuống dưới và bao phủ các lớp a[i] trên cùng hiện có. Nếu có ít hơn a[i] lớp thì mọi lớp hiện có sẽ bị che phủ."
date: "2026-06-10T21:02:10+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1501
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 707 (Div. 2, based on Moscow Open Olympiad in Informatics)"
rating: 900
weight: 1501
solve_time_s: 135
verified: false
draft: false
---

[CF 1501B - Bánh Napoleon](https://codeforces.com/problemset/problem/1501/B) 

**Xếp hạng:** 900 
**Tags:** dp, triển khai, sắp xếp 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi xây dựng từng lớp bánh. Sau khi đặt`i`-lớp thứ, chúng tôi đổ`a[i]`đơn vị kem lên trên. Chất kem đó lan xuống dưới và phủ lên trên`a[i]`các lớp hiện có. Nếu có ít hơn`a[i]`các lớp, mọi lớp hiện có sẽ bị che phủ. 

Đối với mỗi lớp, chúng ta phải quyết định xem nó có được bao phủ bởi ít nhất một thao tác kem hay không. Đầu ra là một mảng các số 0 và số 1, được sắp xếp từ dưới lên trên. Một giá trị của`1`có nghĩa là lớp đó bị ướt, trong khi`0`có nghĩa là nó vẫn khô. 

Tổng số lớp trên tất cả các trường hợp thử nghiệm nhiều nhất là`2·10^5`, đủ nhỏ cho một nghiệm tuyến tính nhưng quá lớn cho bất kỳ nghiệm bậc hai nào. Một phương pháp kiểm tra mọi lớp bị ảnh hưởng cho mỗi thao tác đổ có thể yêu cầu khoảng`n²`làm việc trong trường hợp xấu nhất. Với`n = 2·10^5`, điều đó có nghĩa là khoảng bốn mươi tỷ hoạt động, vượt xa giới hạn thời gian. 

Một số tình huống rất dễ xử lý sai. 

Một trường hợp là khi lượng kem vượt quá số lớp hiện có. 

đầu vào:```
1
4
5 0 0 0
```Thao tác đầu tiên đã bao gồm tất cả các lớp hiện có, vì vậy câu trả lời là```
1 0 0 0
```Việc triển khai bất cẩn cố gắng truy cập các lớp bên dưới đáy có thể tạo ra các chỉ mục không hợp lệ. 

Một trường hợp phức tạp khác đến từ các khoảng thời gian chồng chéo. 

đầu vào:```
1
6
0 3 0 0 1 3
```Câu trả lời đúng là```
1 1 0 1 1 1
```Thao tác nhiều lớp kem sẽ phủ liên tục một số lớp, nhưng một lớp chỉ cần phủ một lần. Việc mô phỏng mọi thao tác một cách độc lập sẽ gây lãng phí thời gian. 

Trường hợp thứ ba là không đổ kem. 

đầu vào:```
1
3
0 0 0
```Câu trả lời là```
0 0 0
```Nếu chúng ta truyền bá phạm vi phủ sóng trước đó không chính xác sau khi lẽ ra nó đã kết thúc, một số lớp có thể được đánh dấu là bị che phủ ngay cả khi không có thao tác nào tiếp cận được chúng. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là xử lý mọi thao tác đổ và đánh dấu rõ ràng các lớp mà nó ảnh hưởng. Khi chúng ta ở vị trí`i`, kem đạt từ lớp`i`hướng xuống cho`a[i]`các vị trí. Chúng ta có thể lặp qua các lớp đó và đánh dấu chúng là được che phủ. 

Cách tiếp cận này đúng vì mọi lớp bị ảnh hưởng đều được truy cập ít nhất một lần. Sự cố xuất hiện khi có nhiều giá trị lớn. Ví dụ, nếu mỗi`a[i] = n`, thao tác đầu tiên chạm vào một lớp, thao tác thứ hai chạm vào hai lớp, v.v., dẫn đến khoảng$$1 + 2 + \dots + n = O(n^2)$$hoạt động. 

Cấu trúc của vấn đề gợi ý một cái nhìn tốt hơn. Mỗi thao tác đổ sẽ tạo ra một đoạn lớp liền kề bị che phủ. Thay vì đánh dấu toàn bộ phân đoạn ngay lập tức, chúng ta có thể xử lý các lớp từ trên xuống dưới và nhớ xem còn bao nhiêu vị trí hướng xuống dưới vẫn nằm trong khoảng kem hoạt động nào đó. 

Giả sử chúng ta hiện đang ở lớp`i`và có`cur`các lớp còn lại từ các khoảng thời gian bắt đầu ở trên. Sau đó xếp lớp`i`được bảo hiểm bất cứ khi nào`cur > 0`. Nếu như`a[i]`bản thân nó lớn hơn`cur`, một khoảng mới bắt đầu tại`i`mở rộng phạm vi phủ sóng xuống dưới, vì vậy chúng tôi cập nhật```
cur = max(cur, a[i])
```Sau khi xử lý xong lớp hiện tại, chúng ta di chuyển xuống một lớp thấp hơn và giảm`cur`bởi một. 

Việc xử lý từ trên xuống dưới biến nhiều khoảng chồng chéo thành một bộ đếm duy nhất, tạo ra độ phức tạp tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ lớp trên cùng và di chuyển xuống phía dưới. 
2. Duy trì một biến`cur`, biểu thị có bao nhiêu lớp liên tiếp, kể cả lớp hiện tại, vẫn được bao phủ bởi lớp kem từ các lớp bên trên. 
3. Tại vị trí`i`, cập nhật`cur = max(cur, a[i])`. 

Nếu thao tác đổ mới có phạm vi hướng xuống xa hơn so với thao tác trước đó, hiệu ứng của thao tác này sẽ thay thế phạm vi cũ. Nếu không, phạm vi bảo hiểm trước đó đã đủ. 
4. Nếu`cur > 0`, đánh dấu lớp`i`như được che phủ. 

Giá trị dương có nghĩa là ít nhất một khoảng kem bao gồm lớp này. 
5. Sau khi xử lý lớp, giảm`cur`bằng một, nhưng không bao giờ dưới 0. 

Di chuyển một bước xuống dưới sẽ tiêu tốn một lớp che phủ còn lại. 
6. Tiếp tục cho đến khi chạm tới lớp dưới cùng. 

### Tại sao nó hoạt động 

Trong khi quét xuống dưới,`cur`luôn bằng số lớp liên tiếp bắt đầu từ lớp hiện tại được đảm bảo phủ kín bởi một số thao tác kem đã gặp phải. Mọi thao tác đổ đều bắt đầu tại vị trí riêng của nó và kéo dài xuống dưới một cách chính xác.`a[i]`các lớp. Lấy giá trị lớn nhất giữa giá trị hiện tại và`a[i]`giữ khoảng thời gian dài nhất hiện đang ảnh hưởng đến các lớp thấp hơn. Giảm dần`cur`sau mỗi bước sẽ loại bỏ chính xác một lớp khỏi phạm vi còn lại. Vì một lớp được đánh dấu là được che phủ chính xác khi đạt đến một khoảng nào đó nên thuật toán sẽ đưa ra câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
out = []

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    ans = [0] * n
    cur = 0

    for i in range(n - 1, -1, -1):
        cur = max(cur, a[i])
        if cur > 0:
            ans[i] = 1
        cur = max(0, cur - 1)

    out.append(" ".join(map(str, ans)))

print("\n".join(out))
```Quá trình quét chạy từ phải sang trái vì lớp trên cùng tương ứng với chỉ mục lớn nhất. Biến`cur`lưu trữ độ sâu bảo hiểm còn lại. Trước khi quyết định trạng thái của lớp`i`, chúng tôi kết hợp lượng kem đổ vào lớp đó bằng cách lấy tối đa`a[i]`. 

Thứ tự của các hoạt động quan trọng. Đầu tiên chúng tôi cập nhật`cur`, sau đó quyết định xem lớp hiện tại có bị che phủ hay không và chỉ sau đó mới giảm`cur`. Đảo ngược các bước này sẽ rút ngắn mỗi khoảng thời gian đi một bước và tạo ra câu trả lời sai. 

Cuộc gọi đến`max(0, cur - 1)`ngăn chặn các giá trị âm. Mặc dù số âm vẫn tạo điều kiện`cur > 0`sai, giữ`cur`không âm làm cho ý nghĩa của biến trở nên chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên. 

đầu vào:```
6
0 3 0 0 1 3
```| tôi | một [tôi] | cur trước khi cập nhật | cur sau khi cập nhật | trả lời[i] | cur sau khi giảm | 
| --- | --- | --- | --- | --- | --- | 
| 5 | 3 | 0 | 3 | 1 | 2 | 
| 4 | 1 | 2 | 2 | 1 | 1 | 
| 3 | 0 | 1 | 1 | 1 | 0 | 
| 2 | 0 | 0 | 0 | 0 | 0 | 
| 1 | 3 | 0 | 3 | 1 | 2 | 
| 0 | 0 | 2 | 2 | 1 | 1 | 

Câu trả lời cuối cùng:```
1 1 0 1 1 1
```Ví dụ này cho thấy một khoảng lớn bắt đầu ở chỉ số 5 tiếp tục ảnh hưởng đến các lớp thấp hơn như thế nào cho đến khi bộ đếm của nó đạt tới 0. 

Hãy xem xét một ví dụ khác. 

đầu vào:```
10
0 0 0 1 0 5 0 0 0 2
```| tôi | một [tôi] | cur trước khi cập nhật | cur sau khi cập nhật | trả lời[i] | cur sau khi giảm | 
| --- | --- | --- | --- | --- | --- | 
| 9 | 2 | 0 | 2 | 1 | 1 | 
| 8 | 0 | 1 | 1 | 1 | 0 | 
| 7 | 0 | 0 | 0 | 0 | 0 | 
| 6 | 0 | 0 | 0 | 0 | 0 | 
| 5 | 5 | 0 | 5 | 1 | 4 | 
| 4 | 0 | 4 | 4 | 1 | 3 | 
| 3 | 1 | 3 | 3 | 1 | 2 | 
| 2 | 0 | 2 | 2 | 1 | 1 | 
| 1 | 0 | 1 | 1 | 1 | 0 | 
| 0 | 0 | 0 | 0 | 0 | 0 | 

Câu trả lời cuối cùng:```
0 1 1 1 1 1 0 0 1 1
```Dấu vết này cho thấy các khoảng chồng chéo được hợp nhất một cách tự nhiên như thế nào bằng cách chỉ giữ lại phạm vi bao phủ lớn nhất còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi lớp được xử lý chính xác một lần | 
| Không gian | O(n) | Mảng câu trả lời lưu trữ một giá trị trên mỗi lớp | 

Vì tổng số lớp trên tất cả các trường hợp thử nghiệm nhiều nhất là`2·10^5`, một thuật toán tuyến tính chỉ thực hiện vài trăm nghìn lần lặp. Điều này thoải mái phù hợp trong giới hạn thời gian và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ans = [0] * n
        cur = 0

        for i in range(n - 1, -1, -1):
            cur = max(cur, a[i])
            if cur > 0:
                ans[i] = 1
            cur = max(0, cur - 1)

        out.append(" ".join(map(str, ans)))

    return "\n".join(out)

# provided samples
assert run(
"""3
6
0 3 0 0 1 3
10
0 0 0 1 0 5 0 0 0 2
3
0 0 0
"""
) == """1 1 0 1 1 1
0 1 1 1 1 1 0 0 1 1
0 0 0"""

# minimum size
assert run(
"""1
1
0
"""
) == "0"

# single layer covered
assert run(
"""1
1
1
"""
) == "1"

# all values equal
assert run(
"""1
5
5 5 5 5 5
"""
) == "1 1 1 1 1"

# off-by-one case
assert run(
"""1
5
0 0 0 0 1
"""
) == "0 0 0 0 1"

# large interval near the end
assert run(
"""1
5
0 0 0 3 0
"""
) == "0 1 1 1 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 0`|`0`| Kích thước tối thiểu | 
|`1 / 1 / 1`|`1`| Lớp phủ đơn | 
|`5 / 5 5 5 5 5`|`1 1 1 1 1`| Khoảng chồng chéo lớn | 
|`5 / 0 0 0 0 1`|`0 0 0 0 1`| Ranh giới ở lớp trên cùng | 
|`5 / 0 0 0 3 0`|`0 1 1 1 0`| Các lỗi có độ dài khoảng thời gian khác nhau | 

## Vỏ cạnh 

Hãy xem xét trường hợp lượng kem vượt quá số lớp hiện có. 

đầu vào:```
1
4
5 0 0 0
```Quét từ phải sang trái cho: 
| bạn | một[i] | cắt sau khi cập nhật | và [tôi] | 
| --- | --- | --- | --- | 
| 3 | 0 | 0 | 0 | 
| 2 | 0 | 0 | 0 | 
| 1 | 0 | 0 | 0 | 
| 0 | 5 | 5 | 1 | 
Câu trả lời trở thành```
1 0 0 0
```Thuật toán không bao giờ cần truy cập vào các lớp không tồn tại. Nó chỉ lưu trữ độ dài vùng phủ sóng còn lại. 

Bây giờ hãy xem xét các khoảng thời gian chồng chéo. 

đầu vào:```
1
5
0 2 0 3 0
```Tại chỉ số 3, khoảng thời gian dài ba bắt đầu. Khi đạt đến chỉ số 1, khoảng thời gian của nó ngắn hơn phạm vi bao phủ còn lại, do đó`max(cur, a[i])`giữ cái dài hơn. Câu trả lời là```
1 1 1 1 0
```Không có lớp nào được tính hai lần và không có công việc bổ sung nào được thực hiện. 

Cuối cùng, hãy xem xét trường hợp không có kem. 

đầu vào:```
1
3
0 0 0
```Biến`cur`vẫn bằng 0 trong suốt quá trình quét. 
| tôi | cắt sau khi cập nhật | và [tôi] | 
| --- | --- | --- | 
| 2 | 0 | 0 | 
| 1 | 0 | 0 | 
| 0 | 0 | 0 | 
Đầu ra là```
0 0 0
```Vì bộ đếm độ che phủ không bao giờ trở thành số dương nên mọi lớp đều khô ráo một cách chính xác.
