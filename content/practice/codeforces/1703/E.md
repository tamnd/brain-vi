---
title: "CF 1703E - Lưới gương"
description: "Chúng ta có một lưới $n lần n$ có các ô chỉ chứa 0 hoặc 1. Chúng ta có thể lật bất kỳ ô nào, thay đổi 0 thành 1 hoặc 1 thành 0 và mỗi lần lật tốn một thao tác. Mục tiêu là làm cho lưới trông giống hệt nhau sau khi xoay nó đi $90^circ$, $180^circ$ hoặc $270^circ$."
date: "2026-06-09T21:37:55+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1703
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 806 (Div. 4)"
rating: 1200
weight: 1703
solve_time_s: 156
verified: false
draft: false
---

[CF 1703E - Lưới gương](https://codeforces.com/problemset/problem/1703/E) 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới có các ô chỉ chứa`0`hoặc`1`. Chúng ta có thể lật bất kỳ ô nào, thay đổi`0`ĐẾN`1`hoặc`1`ĐẾN`0`và mỗi lần lật tốn một thao tác. 

Mục đích là làm cho lưới trông giống hệt nhau sau khi xoay nó bằng cách$90^\circ$,$180^\circ$, hoặc$270^\circ$. Nói cách khác, mỗi vòng quay phải tạo ra lưới giống hệt như lưới ban đầu. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được kích thước của lưới vuông và sau đó là chính lưới đó. Chúng ta phải xuất ra số lần lật ô tối thiểu cần thiết để đạt được sự đối xứng quay. 

Các ràng buộc đủ nhỏ để việc kiểm tra từng ô nhiều lần là hoàn toàn khả thi. Lưới lớn nhất có$100 \times 100 = 10{,}000$các ô và có tối đa 100 trường hợp thử nghiệm. MỘT$O(n^2)$giải pháp cho mỗi trường hợp thử nghiệm dễ dàng nằm trong giới hạn, trong khi mọi thứ liên quan đến tìm kiếm theo cấp số nhân trên các kết hợp lật đều là không thể. 

Phần khó khăn là hiểu được những ô nào được liên kết với nhau bằng phép đối xứng quay. Việc triển khai bất cẩn có thể xử lý các ô một cách độc lập ngay cả khi việc xoay lưới buộc một số vị trí luôn chứa cùng một giá trị. 

Hãy xem xét lưới này:```
2
10
00
```Bốn vị trí liên quan đến luân chuyển là:```
(0,0) -> (0,1) -> (1,1) -> (1,0)
```Giá trị của chúng là:```
1 0 0 0
```Để làm cho cả bốn đều bằng nhau, chỉ cần lật một lần là đủ. Lật cô đơn`1`cho bốn số không. Xử lý các quan điểm một cách độc lập sẽ bỏ lỡ mối quan hệ này. 

Một trường hợp tinh tế khác là ô trung tâm của lưới có kích thước lẻ:```
3
000
010
000
```Ô trung tâm ánh xạ tới chính nó dưới mỗi vòng quay. Không có hoạt động nào là cần thiết cho nó. Một giải pháp cố gắng buộc mọi nhóm phải có kích thước bốn có thể vô tình đếm thêm lần lật. 

Sai lầm phổ biến thứ ba là tính toán kép các nhóm luân phiên. Ví dụ:```
3
010
110
010
```Các ô ở góc tạo thành một nhóm quay và các ô ở cạnh giữa tạo thành một nhóm khác. Nếu chúng ta lặp lại qua tất cả các ô và xử lý lặp đi lặp lại từng nhóm thì câu trả lời sẽ trở nên quá lớn gấp bốn lần. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là xem xét mọi tập hợp ô được kết nối bằng phép quay. Đối với mỗi bộ, chúng ta có thể thử tất cả các bài tập cuối cùng có thể và chọn bài tập rẻ nhất. 

Một vòng quay bằng$90^\circ$gửi vị trí$(i,j)$ĐẾN:$$(j,\ n-1-i)$$Việc áp dụng điều này nhiều lần sẽ tạo ra tối đa bốn vị trí riêng biệt:$$(i,j) \rightarrow (j,n-1-i) \rightarrow (n-1-i,n-1-j) \rightarrow (n-1-j,i)$$Để lưới cuối cùng không thay đổi sau khi xoay, tất cả các ô trong quỹ đạo này phải chứa cùng một giá trị. 

Ý tưởng vũ lực có hiệu quả vì mỗi quỹ đạo là độc lập. Khi chúng ta biết các ô thuộc một quỹ đạo, chúng ta chỉ cần quyết định xem tất cả chúng có trở thành`0`hoặc tất cả đều trở thành`1`. Nếu một quỹ đạo chứa$k$những cái đó thì:```
cost to make all 0 = k
cost to make all 1 = size - k
```Chúng tôi lấy giá trị nhỏ hơn. 

Thử thách duy nhất còn lại là tránh việc xử lý lặp đi lặp lại cùng một quỹ đạo. Mỗi quỹ đạo có kích thước bốn xuất hiện bốn lần nếu chúng ta bắt đầu từ mọi ô. Thay vào đó, chúng tôi xử lý từng quỹ đạo chính xác một lần. 

Quan sát quan trọng là mọi quỹ đạo đều cắt góc phần tư phía trên bên trái đúng một lần. Nếu chúng ta chỉ lặp lại:```
0 <= i < n/2
0 <= j < (n+1)/2
```thì mỗi nhóm luân phiên được truy cập đúng một lần. Chúng tôi thu thập bốn ô của nó, đếm xem có bao nhiêu`1`, và cộng số lần lật tối thiểu cần thiết. 

Điều này biến vấn đề thành một vấn đề đơn giản$O(n^2)$quét. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với quá trình xử lý quỹ đạo lặp đi lặp lại |$O(n^2)$nhưng có xu hướng đếm gấp đôi |$O(1)$| Không đúng | 
| Xử lý dựa trên quỹ đạo tối ưu |$O(n^2)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới. 
2. Lặp lại qua các ô ở vùng đại diện phía trên bên trái:$$0 \le i < \frac{n}{2}$$

$$0 \le j < \frac{n+1}{2}$$Mỗi quỹ đạo quay xuất hiện đúng một lần trong vùng này. 
3. Đối với mỗi vị trí được chọn$(i,j)$, tính toán bốn ô tương đương xoay của nó:$$(i,j)$$

$$(j,n-1-i)$$

$$(n-1-i,n-1-j)$$

$$(n-1-j,i)$$4. Đếm xem bốn ô này chứa bao nhiêu`1`. 
5. Nếu số lượng là`ones`, thì số số 0 là`4 - ones`. 
6. Để làm cho cả bốn ô bằng nhau, hãy lật tất cả các số 1 về 0 hoặc tất cả các số 0 thành một. Thêm vào:$$\min(\text{ones},\ 4-\text{ones})$$để trả lời. 
7. Sau khi xử lý tất cả các ô đại diện, xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Tính đối xứng quay yêu cầu mỗi ô phải khớp với giá trị của tất cả các ô thu được bằng cách áp dụng nhiều lần một$90^\circ$xoay vòng. Những vị trí này tạo thành một quỹ đạo. Các quỹ đạo khác nhau không bao giờ tương tác, do đó giải pháp tối ưu có thể được tính toán độc lập cho từng quỹ đạo và sau đó tính tổng. 

Trong một quỹ đạo, giá trị cuối cùng phải là tất cả số 0 hoặc tất cả số 1. Lựa chọn rẻ nhất là lựa chọn nào cần ít lần lật hơn, cụ thể là$\min(\text{ones}, \text{zeros})$. Vì mỗi quỹ đạo được xử lý chính xác một lần nên mọi sửa đổi cần thiết đều được tính chính xác một lần, tạo ra tổng số thao tác tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        g = [input().strip() for _ in range(n)]

        ans = 0

        for i in range(n // 2):
            for j in range((n + 1) // 2):
                cells = [
                    g[i][j],
                    g[j][n - 1 - i],
                    g[n - 1 - i][n - 1 - j],
                    g[n - 1 - j][i]
                ]

                ones = sum(c == '1' for c in cells)
                ans += min(ones, 4 - ones)

        print(ans)

solve()
```Các vòng bên ngoài liệt kê chính xác một đại diện từ mỗi quỹ đạo quay. Các giới hạn là phần tế nhị nhất của việc thực hiện. sử dụng`n // 2`hàng và`(n + 1) // 2`các cột bao gồm tất cả các nhóm chính xác một lần và tự nhiên bỏ qua ô trung tâm khi`n`thật kỳ quặc. 

Đối với mỗi vị trí đại diện, mã sẽ tính toán bốn vị trí thu được bằng cách liên tiếp$90^\circ$các phép quay. Các tọa độ này được lấy trực tiếp từ công thức xoay. 

Số lượng cái xác định chi phí tối ưu cho quỹ đạo đó. Nếu một quỹ đạo chứa ba số một và một số 0, thì việc thay đổi một số 0 sẽ tốn một phép toán, trong khi việc thay đổi ba số một sẽ tốn ba phép tính. Lấy mức tối thiểu sẽ mang lại sự đóng góp tối ưu cho địa phương. 

Vì các quỹ đạo độc lập nên việc tính tổng các chi phí này sẽ mang lại kết quả tối ưu toàn cục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
010
110
010
```Thuật toán xử lý một quỹ đạo. 

| tôi | j | Giá trị quỹ đạo | Những cái | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0,0,0,1 | 1 | 1 | 

Tổng số câu trả lời:```
1
```Quỹ đạo góc chứa một`1`và ba`0`S. Làm cho tất cả chúng bằng 0 chỉ cần một lần lật. 

### Ví dụ 2 

đầu vào:```
1
0
```Không có quỹ đạo cỡ bốn. 

| n | Quỹ đạo đã xử lý | Trả lời | 
| --- | --- | --- | 
| 1 | 0 | 0 | 

Ô trung tâm ánh xạ tới chính nó theo mỗi vòng quay, do đó lưới đã đối xứng về mặt xoay. 

Ví dụ này chứng minh tại sao các lưới có kích thước lẻ không cần xử lý đặc biệt ngoài giới hạn vòng lặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi quỹ đạo được xử lý một lần, tỷ lệ thuận với số lượng ô | 
| Không gian |$O(1)$| Chỉ một số biến được lưu trữ bên cạnh lưới đầu vào | 

Trường hợp thử nghiệm lớn nhất chỉ chứa 10.000 ô. MỘT$O(n^2)$truyền tải cực kỳ nhanh và hệ số hằng số rất nhỏ vì mỗi quỹ đạo chỉ liên quan đến bốn lần tra cứu tọa độ. 

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
        n = int(input())
        g = [input().strip() for _ in range(n)]

        ans = 0

        for i in range(n // 2):
            for j in range((n + 1) // 2):
                cells = [
                    g[i][j],
                    g[j][n - 1 - i],
                    g[n - 1 - i][n - 1 - j],
                    g[n - 1 - j][i]
                ]
                ones = sum(c == '1' for c in cells)
                ans += min(ones, 4 - ones)

        out.append(str(ans))

    return "\n".join(out) + "\n"

# provided sample
assert run(
"""5
3
010
110
010
1
0
5
11100
11011
01011
10011
11000
5
01000
10101
01010
00010
01001
5
11001
00000
11111
10110
01111
"""
) == "1\n0\n9\n7\n6\n"

# minimum size
assert run(
"""1
1
1
"""
) == "0\n"

# already symmetric
assert run(
"""1
2
11
11
"""
) == "0\n"

# one differing corner
assert run(
"""1
2
10
00
"""
) == "1\n"

# odd size with center ignored
assert run(
"""1
3
000
010
000
"""
) == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 chứa`1`| 0 | Bản đồ ô trung tâm tới chính nó | 
| 2×2 tất cả những cái | 0 | Đã có lưới đối xứng | 
| 2×2 với một góc | 1 | Hiệu chỉnh quỹ đạo đơn | 
| 3×3 chỉ có tâm bằng`1`| 0 | Tế bào trung tâm không nên đóng góp | 

## Vỏ cạnh 

### Lưới có kích thước lẻ có ô trung tâm 

đầu vào:```
1
3
000
010
000
```Vị trí trung tâm là`(1,1)`. Xoay lưới sẽ giữ ô này ở cùng một vị trí. Thuật toán không bao giờ xử lý nó vì các vòng lặp đại diện chỉ bao gồm các quỹ đạo cỡ bốn. 

Tất cả các ô khác đã tạo thành các nhóm đối xứng, vì vậy câu trả lời vẫn là:```
0
```### Quỹ đạo chứa ba giá trị bằng nhau và một giá trị khác nhau 

đầu vào:```
1
2
10
00
```Quỹ đạo là:```
(0,0), (0,1), (1,1), (1,0)
```Giá trị:```
1 0 0 0
```Thuật toán tính toán:```
ones = 1
zeros = 3
min(1, 3) = 1
```Đầu ra:```
1
```Một nỗ lực tham lam để buộc mọi thứ phải`1`sẽ sử dụng ba lần lật thay vì một. 

### Tính nguy hiểm gấp đôi 

đầu vào:```
1
3
010
110
010
```Bốn ô góc tạo thành một quỹ đạo duy nhất:```
0 0 0 1
```Một lần quét đơn giản trên tất cả các ô sẽ gặp quỹ đạo này bốn lần. Việc lặp lại vùng đại diện ghé thăm nó một lần, tính toán chi phí`1`, và dừng lại. Câu trả lời đúng là:```
1
```Điều này xác nhận rằng mỗi nhóm luân phiên đóng góp chính xác một lần vào tổng cuối cùng.
