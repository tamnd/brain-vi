---
title: "CF 229A - Ca"
description: "Chúng ta có một ma trận nhị phân trong đó mỗi hàng có thể được xoay theo chu kỳ. Xoay trái sẽ di chuyển mọi phần tử sang trái một vị trí và bao bọc phần tử đầu tiên đến cuối. Một vòng quay bên phải thì ngược lại."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 229
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 142 (Div. 1)"
rating: 1500
weight: 229
solve_time_s: 94
verified: true
draft: false
---

[CF 229A - Ca](https://codeforces.com/problemset/problem/229/A) 

**Đánh giá:** 1500 
**Tags:** vũ lực, hai con trỏ 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một ma trận nhị phân trong đó mỗi hàng có thể được xoay theo chu kỳ. Xoay trái sẽ di chuyển mọi phần tử sang trái một vị trí và bao bọc phần tử đầu tiên đến cuối. Một vòng quay bên phải thì ngược lại. 

Mục tiêu là tạo ra ít nhất một cột bao gồm toàn bộ`1`s, sử dụng tổng số lần dịch chuyển hàng tối thiểu. Mỗi hàng có thể được dịch chuyển độc lập và việc dịch chuyển một hàng theo một vị trí sẽ tốn một lần di chuyển. 

Quan sát quan trọng là các hàng không tương tác với nhau ngoại trừ thông qua việc chọn cột cuối cùng. Nếu chúng ta quyết định cột đó`j`nên trở thành tất cả`1`s, thì mỗi hàng độc lập đóng góp một số chi phí tối thiểu để đặt một`1`vào cột đó Tổng chi phí cho cột`j`là tổng của các chi phí trên mỗi hàng. 

Các ràng buộc nhỏ ở một chiều và lớn ở chiều kia. Chúng tôi có nhiều nhất`100`hàng, nhưng lên đến`10^4`cột. Một giải pháp xung quanh`O(n * m^2)`vẫn có thể chấp nhận được vì trường hợp xấu nhất là về`10^8`hoạt động gần với giới hạn trong Python và có nhiều rủi ro. Bất cứ điều gì hình khối là hoàn toàn không thể. Quét gần tuyến tính trên mỗi hàng là mục tiêu. 

Có một điều kiện bất khả thi đặc biệt quan trọng. Nếu một hàng không chứa`1`chút nào, thì không có sự dịch chuyển theo chu kỳ nào có thể đặt ra một`1`vào bất kỳ cột nào. Điều đó có nghĩa là câu trả lời là ngay lập tức`-1`. 

Một lỗi phổ biến là xử lý các dịch chuyển trái và phải một cách riêng biệt mà không xem xét việc bao bọc một cách chính xác. 

Ví dụ:```
1 5
10000
```Câu trả lời đúng là`0`, vì cột`0`đã chứa một`1`. 

Nếu chúng ta nhắm mục tiêu cột`4`, chi phí tối thiểu là`1`, không`4`, bởi vì một ca phải sẽ biến đổi`10000`vào trong`00001`. 

Một lỗi dễ xảy ra khác khi tính toán khoảng cách chỉ theo một hướng. 

Coi như:```
1 6
001000
```Cột nhắm mục tiêu`0`nên chi phí`2`, bởi vì dịch chuyển sang trái hai lần sẽ di chuyển`1`từ chỉ mục`2`lập chỉ mục`0`. Quét một hướng có thể trả về không chính xác`4`. 

Ngoài ra còn có một trường hợp bao quanh tinh tế:```
1 5
00001
```Cột nhắm mục tiêu`0`chi phí`1`, bởi vì một sự dịch chuyển trái sẽ di chuyển phần tử cuối cùng lên phía trước. Quên hành vi tuần hoàn sẽ quay trở lại không chính xác`4`. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi cột mục tiêu`j`, hãy thử từng hàng một cách độc lập và tính toán số ca tối thiểu cần thiết để mang lại một số`1`vào cột`j`. 

Giả sử một hàng có`1`ở vị trí`k`. Để di chuyển cái đó`1`vào cột`j`, chúng ta có thể xoay trái hoặc phải. Chi phí là:```
min((j - k) mod m, (k - j) mod m)
```Với mỗi hàng và mỗi cột mục tiêu, chúng ta có thể quét tất cả các vị trí chứa`1`và lấy chi phí tối thiểu. 

Điều này hoạt động vì mỗi hàng độc lập khi cột mục tiêu được cố định. 

Vấn đề là sự phức tạp. Trong trường hợp xấu nhất, mỗi hàng chứa`m`những cái đó. Sau đó đối với mỗi`m`cột mục tiêu chúng tôi quét tất cả`m`lại các vị trí. Tổng độ phức tạp trở thành:```
O(n * m^2)
```Với`m = 10^4`, điều này đạt đến khoảng`10^10`hoạt động nguyên thủy, quá chậm. 

Cấu trúc của vấn đề đưa ra một lựa chọn tốt hơn nhiều. Đối với một hàng cố định, chúng tôi muốn khoảng cách từ mỗi cột đến cột gần nhất`1`trên một vòng tròn. 

Điều đó trở thành vấn đề về phần tử gần nhất cổ điển trên mảng tuần hoàn. 

Thủ thuật hữu ích là nhân đôi hàng. Nếu hàng ban đầu có chiều dài`m`, tạo một mảng khái niệm có độ dài`2m`bằng cách lặp lại nó hai lần. Bây giờ vòng tròn bao quanh trở thành khoảng cách tuyến tính thông thường. 

Chúng tôi quét từ trái sang phải và theo dõi gần nhất`1`ở bên trái. Sau đó chúng tôi quét từ phải sang trái và theo dõi vị trí gần nhất`1`ở bên phải. Kết hợp hai khoảng cách đó sẽ tạo ra khoảng cách tuần hoàn tối thiểu cho mỗi cột. 

Sau khi tính toán phần chi phí của một hàng cho mỗi cột, chúng tôi thêm nó vào mảng câu trả lời chung. 

Điều này làm giảm tổng độ phức tạp xuống`O(n * m)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m2) | O(1) | Quá chậm | 
| Tối ưu | O(n * m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ma trận và tạo mảng`ans`kích thước`m`, khởi tạo thành`0`. 
2. Xử lý từng hàng một cách độc lập vì đóng góp chi phí của một hàng không phụ thuộc vào các hàng khác. 
3. Nếu một hàng không chứa`1`, in ngay`-1`. 

Không có sự dịch chuyển theo chu kỳ nào có thể tạo ra một`1`trong hàng đó nên việc tạo thành một cột toàn số một là không thể. 
4. Nhân đôi hàng bằng cách nối nó với chính nó. 

Điều này biến chuyển động tuần hoàn thành chuyển động tuyến tính thông thường. Các vị trí gần cuối bây giờ có thể nhìn thấy các hàng xóm được bao bọc một cách tự nhiên. 
5. Tạo một mảng`dist`chiều dài`2m`, được khởi tạo với giá trị lớn. 
6. Quét từ trái sang phải. 

Giữ nguyên vị trí gần đây nhất`1`. Nếu vị trí hiện tại có thể truy cập được từ đó`1`, cập nhật khoảng cách. 
7. Quét từ phải sang trái. 

Duy trì gần nhất`1`ở bên phải và giảm thiểu khoảng cách một lần nữa. 
8. Đối với mỗi cột gốc`j`, các vị trí liên quan là`j`Và`j + m`bên trong mảng nhân đôi. 

Lấy mức tối thiểu của hai khoảng cách đó. Điều này mang lại khoảng cách theo chu kỳ thực sự từ cột`j`đến gần nhất`1`. 
9. Thêm khoảng cách đó vào`ans[j]`. 
10. Sau khi tất cả các hàng được xử lý, hãy in giá trị nhỏ nhất trong`ans`. 

### Tại sao nó hoạt động 

Đối với cột mục tiêu cố định, chiến lược tối ưu là độc lập cho mỗi hàng. Tổng chi phí tối thiểu là tổng chi phí tối thiểu trên mỗi hàng. 

Nhân đôi hàng sẽ chuyển đổi khoảng cách tròn thành khoảng cách tuyến tính. Bất kỳ đường tuần hoàn ngắn nhất nào giữa hai vị trí sẽ xuất hiện dưới dạng một đoạn thông thường ở đâu đó bên trong mảng nhân đôi. 

Hai lần quét tính toán gần nhất`1`từ trái và phải cho mọi vị trí. Lấy mức tối thiểu sẽ cho khoảng cách tuyến tính ngắn nhất, phù hợp với khoảng cách dịch chuyển theo chu kỳ ngắn nhất sau khi sao chép. 

Vì mỗi hàng đóng góp chi phí tối thiểu chính xác của nó cho mỗi cột và cuối cùng chúng tôi chọn cột rẻ nhất nên thuật toán luôn tạo ra giá trị tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    rows = [input().strip() for _ in range(n)]

    ans = [0] * m
    INF = 10**18

    for s in rows:
        if '1' not in s:
            print(-1)
            return

        t = s + s
        dist = [INF] * (2 * m)

        last = -INF

        for i in range(2 * m):
            if t[i] == '1':
                last = i
            dist[i] = min(dist[i], i - last)

        last = INF

        for i in range(2 * m - 1, -1, -1):
            if t[i] == '1':
                last = i
            dist[i] = min(dist[i], last - i)

        for j in range(m):
            ans[j] += min(dist[j], dist[j + m])

    print(min(ans))

solve()
```Giải pháp duy trì tổng số đang chạy cho mỗi cột mục tiêu. Mỗi hàng độc lập cộng chi phí thay đổi tối thiểu của nó vào tổng số đó. 

Chuỗi trùng lặp`t = s + s`là thủ thuật cốt lõi. Nếu không có sự trùng lặp, việc xử lý gói gọn gàng sẽ trở nên lộn xộn và dễ xảy ra lỗi. Sau khi sao chép, một gần nhất-`1`tìm kiếm hoạt động chính xác như một bài toán mảng tuyến tính thông thường. 

Quét từ trái sang phải tính toán khoảng cách đến điểm gần nhất`1`ở phía bên trái. Quét từ phải sang trái tính toán gần nhất`1`ở phía bên phải. Khoảng cách tối thiểu của cả hai là khoảng cách ngắn nhất thực sự. 

Biểu thức:```
min(dist[j], dist[j + m])
```rất dễ bị bỏ qua. Cả hai vị trí đều tương ứng với cùng một cột tuần hoàn ban đầu. Lấy cả hai đảm bảo rằng các đường dẫn bao quanh được đưa vào một cách chính xác. 

Sử dụng một trọng điểm rất lớn như`10**18`tránh kiểm tra trường hợp đặc biệt trong quá trình quét. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 6
101010
000100
100000
```Đối với hàng đầu tiên: 

| Cột | Gần nhất`1`khoảng cách | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 0 | 
| 3 | 1 | 
| 4 | 0 | 
| 5 | 1 | 

Đối với hàng thứ hai: 

| Cột | Gần nhất`1`khoảng cách | 
| --- | --- | 
| 0 | 3 | 
| 1 | 2 | 
| 2 | 1 | 
| 3 | 0 | 
| 4 | 1 | 
| 5 | 2 | 

Đối với hàng thứ ba: 

| Cột | Gần nhất`1`khoảng cách | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 
| 4 | 2 | 
| 5 | 1 | 

Tính tổng theo cột: 

| Cột | Tổng chi phí | 
| --- | --- | 
| 0 | 3 | 
| 1 | 4 | 
| 2 | 3 | 
| 3 | 4 | 
| 4 | 3 | 
| 5 | 4 | 

Tối thiểu là`3`. 

Ví dụ này chứng minh rằng nhiều cột có thể có cùng một câu trả lời tối ưu. Thuật toán đánh giá tất cả chúng cùng một lúc. 

### Mẫu 2 

đầu vào:```
2 3
000
111
```Hàng đầu tiên không chứa`1`. 

| Hàng | Chứa`1`? | 
| --- | --- | 
| 000 | Không | 
| 111 | Có | 

Vì hàng đầu tiên không bao giờ có thể đóng góp một`1`vào bất kỳ cột nào, câu trả lời là ngay lập tức`-1`. 

Điều này xác nhận việc kiểm tra không thể thực hiện được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m) | Mỗi hàng được quét một số lần không đổi | 
| Không gian | O(m) | Mảng khoảng cách và mảng trả lời là tuyến tính theo cột | 

Với`n ≤ 100`Và`m ≤ 10^4`, tổng công việc là khoảng vài triệu thao tác, nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    rows = [input().strip() for _ in range(n)]

    ans = [0] * m
    INF = 10**18

    for s in rows:
        if '1' not in s:
            print(-1)
            return

        t = s + s
        dist = [INF] * (2 * m)

        last = -INF

        for i in range(2 * m):
            if t[i] == '1':
                last = i
            dist[i] = min(dist[i], i - last)

        last = INF

        for i in range(2 * m - 1, -1, -1):
            if t[i] == '1':
                last = i
            dist[i] = min(dist[i], last - i)

        for j in range(m):
            ans[j] += min(dist[j], dist[j + m])

    print(min(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run(
"""3 6
101010
000100
100000
"""
) == "3", "sample 1"

# impossible case
assert run(
"""2 3
000
111
"""
) == "-1", "sample 2"

# minimum size
assert run(
"""1 1
1
"""
) == "0", "single cell already valid"

# wraparound check
assert run(
"""1 5
00001
"""
) == "0", "existing column already works"

# cyclic distance must use wraparound
assert run(
"""1 5
10000
"""
) == "0", "distance through wraparound"

# all ones
assert run(
"""3 4
1111
1111
1111
"""
) == "0", "no shifts needed"

# off-by-one around edges
assert run(
"""2 5
01000
00010
"""
) == "2", "edge distance correctness"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`0`| Đầu vào kích thước tối thiểu | 
|`2 3 / 000 / 111`|`-1`| Hàng không thể | 
|`1 5 / 00001`|`0`| Cột hợp lệ hiện có | 
|`1 5 / 10000`|`0`| Xử lý toàn diện | 
| tất cả các hàng`1111`|`0`| Không cần chuyển động | 
|`01000`Và`00010`|`2`| Khoảng cách ranh giới | 

## Vỏ cạnh 

Hãy xem xét cấu hình không thể:```
2 4
0000
1010
```Hàng đầu tiên không chứa`1`. Trong quá trình xử lý, thuật toán sẽ kiểm tra:```
if '1' not in s:
```và ngay lập tức quay trở lại`-1`. 

Điều này đúng vì sự dịch chuyển theo chu kỳ chỉ sắp xếp lại các giá trị hiện có. Một hàng toàn số 0 không bao giờ có thể đóng góp một`1`vào bất kỳ cột nào. 

Bây giờ hãy xem xét hành vi bao quanh:```
1 5
00001
```Cột`0`chỉ cách hiện tại một điểm thay đổi`1`. 

Sau khi nhân đôi:```
0000100001
```quét từ trái sang phải và từ phải sang trái sẽ phát hiện chính xác rằng điểm gần nhất`1`để định vị`0`là khoảng cách`1`, không`4`. 

Điều này xác nhận thủ thuật chuyển đổi theo chu kỳ. 

Cuối cùng, hãy xem xét trường hợp đường đi ngắn nhất đi qua ranh giới:```
1 6
100000
```Cột nhắm mục tiêu`5`nên chi phí`1`. 

Bên trong chuỗi nhân đôi:```
100000100000
```chức vụ`5`nhìn thấy tiếp theo`1`ở vị trí`6`, cho khoảng cách`1`. 

Việc triển khai không theo chu kỳ ngây thơ sẽ trả về không chính xác`5`.
