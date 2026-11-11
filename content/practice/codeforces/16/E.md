---
title: "CF 16E - Cá"
description: "Chúng ta có n con cá trong một cái hồ. Mỗi ngày, có đúng một cặp cá còn sống không có thứ tự được chọn ngẫu nhiên một cách thống nhất. Khi cá tôi gặp cá j, cá tôi ăn cá j với xác suất a[i][j], và cá j ăn cá i với xác suất a[j][i] = 1 - a[i][j]."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 16
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 16 (Div. 2 Only)"
rating: 1900
weight: 16
solve_time_s: 94
verified: true
draft: false
---
[CF 16E - Cá](https://codeforces.com/problemset/problem/16/E) 

**Xếp hạng:** 1900 
**Thẻ:** bitmask, dp, xác suất 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`cá trong hồ. Mỗi ngày, có đúng một cặp cá còn sống không có thứ tự được chọn ngẫu nhiên một cách thống nhất. Khi cá`i`gặp cá`j`, cá`i`ăn cá`j`với xác suất`a[i][j]`, và cá`j`ăn cá`i`với xác suất`a[j][i] = 1 - a[i][j]`. 

Quá trình tiếp tục cho đến khi chỉ còn một con cá còn sống. Với mỗi con cá, chúng ta phải tính xác suất để nó trở thành con sống sót cuối cùng. 

Đầu vào là một ma trận xác suất. Lối vào`a[i][j]`cho chúng ta biết khả năng cá`i`đánh bại cá`j`trong cuộc gặp gỡ trực tiếp. Ma trận nói chung là bất đối xứng, nhưng mọi cặp đều thỏa mãn:```
a[i][j] + a[j][i] = 1
```Ràng buộc`n ≤ 18`hoàn toàn định hình giải pháp. Một mô phỏng bạo lực của tất cả các lệnh chiến đấu sẽ bùng nổ ngay lập tức vì số lượng trình tự loại bỏ có thể xảy ra là khoảng:```
n * (n - 1) * (n - 2) * ...
```đó là sự tăng trưởng giai thừa. Ngay cả đối với`n = 18`, đó là lớn về mặt thiên văn. 

số`2^18 = 262144`đủ nhỏ để lập trình động tập hợp con. Bất cứ khi nào`n`là khoảng 20, bitmask DP thường là thứ đầu tiên cần điều tra. Trạng thái của mọi tập hợp con cá còn sống trở nên hiện thực. 

Có một số trường hợp dễ bỏ sót là làm sai công thức. 

Hãy xem xét đầu vào này:```
2
0 1
0 0
```Cá`0`luôn đánh bại cá`1`. Câu trả lời đúng là:```
1.000000 0.000000
```Việc thực hiện bất cẩn phân phối xác suất một cách đối xứng giữa cả hai con cá sẽ tạo ra kết quả không chính xác.`0.5 0.5`. 

Một trường hợp tinh tế khác là khi có nhiều đường dẫn đến cùng một tập hợp sống động.```
3
0 0.5 0.5
0.5 0 0.5
0.5 0.5 0
```Tất cả các loài cá đều đối xứng nên mọi câu trả lời đều phải chính xác`1/3`. Nếu DP quên tích lũy đóng góp từ tất cả các trạng thái trước đó thì xác suất sẽ ngừng tổng thành`1`. 

Một sai lầm nguy hiểm hơn xuất hiện ở chính xác suất chuyển đổi. Giả sử cá còn sống`{0,1,2}`. Có chính xác:```
C(3,2) = 3
```các cặp có thể gặp nhau. Nếu cá`0`chết vì cá`1`, xác suất chuyển tiếp phải bao gồm phép chia cho`3`. Việc quên sự chuẩn hóa này làm cho tổng xác suất vượt quá`1`. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Ở mỗi bước, hãy chọn mọi cặp cá còn sống có thể, phân nhánh theo cả hai kết quả chiến đấu có thể xảy ra và tiếp tục đệ quy cho đến khi còn lại một con cá. 

Điều này hoạt động vì quá trình này không có bộ nhớ. Tương lai chỉ phụ thuộc vào tập sống hiện tại. Nếu chúng ta liệt kê đệ quy mọi chuỗi loại bỏ có thể và nhân các xác suất dọc theo đường đi, thì cuối cùng chúng ta sẽ có được câu trả lời chính xác. 

Vấn đề là số lượng trạng thái trong cây đệ quy. Lúc đầu có:```
C(n,2)
```những cuộc gặp gỡ có thể xảy ra. Sau một lần loại bỏ có:```
C(n-1,2)
```và vân vân. 

Tổng số chuỗi chiến đấu tăng lên gần giống như thời gian giai thừa. Vì`n = 18`, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là nhiều nhánh đệ quy tiếp cận cùng một tập hợp con cá sống. 

Ví dụ, giả sử cá sống trở thành`{0,2,5}`. Việc loại bỏ chính xác trình tự nào trước đó đã tạo ra bộ này không quan trọng. Từ thời điểm đó trở đi, xác suất trong tương lai là như nhau. 

Điều đó có nghĩa là trạng thái có thể được nén thành:```
mask = set of alive fish
```Điều này ngay lập tức gợi ý tập hợp con DP. 

Định nghĩa:```
dp[mask] = probability that exactly the fish in mask are alive
```Chúng tôi bắt đầu từ bộ đầy đủ, trong đó mọi con cá đều còn sống với xác suất`1`. 

Từ một tiểu bang`mask`, chúng tôi chọn một cặp trong số tất cả các con cá còn sống một cách đồng nhất. Một con cá chết, tạo ra chiếc mặt nạ nhỏ hơn. 

Vì mỗi lần chuyển đổi loại bỏ chính xác một con cá nên biểu đồ các trạng thái là không có chu kỳ. Chúng tôi có thể gia công mặt nạ từ lớn đến nhỏ. 

Số tập con là`2^n`và đối với mỗi tập hợp con, chúng ta có thể kiểm tra các cặp cá. Với`n ≤ 18`, điều này trở nên khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Tăng trưởng giai thừa theo cấp số nhân | Hàm mũ | Quá chậm | 
| DP mặt nạ bit tối ưu | O(2^n · n²) | O(2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn mọi tập sống động dưới dạng mặt nạ bit. 

Chút`i`là`1`nếu cá`i`vẫn còn sống. Với`n ≤ 18`, toàn bộ trạng thái vừa khít bên trong một số nguyên. 
2. Xác định trạng thái DP. 

Cho phép:```
dp[mask]
```là xác suất để số cá còn sống hiện tại chính xác là những con trong`mask`. 
3. Khởi tạo trạng thái bắt đầu. 

Ban đầu mọi con cá đều còn sống nên:```
full = (1 << n) - 1
dp[full] = 1
```4. Xử lý mặt nạ từ tập hợp con lớn hơn đến tập hợp con nhỏ hơn. 

Mỗi lần chuyển đổi sẽ loại bỏ một con cá, do đó xác suất luôn hướng tới mặt nạ có ít bit hơn. 
5. Đối với mặt nạ hiện tại, hãy đếm xem có bao nhiêu con cá còn sống. 

Giả định`k`cá còn sống. Khi đó số cặp có thể gặp nhau là:$\frac{k(k-1)}{2}$Mỗi cặp được chọn thống nhất. 
6. Lặp lại từng cặp có thứ tự`(i, j)`của cá sống ở đâu`i != j`. 

Nếu cá`i`ăn cá`j`, trạng thái tiếp theo trở thành:```
next_mask = mask ^ (1 << j)
```bởi vì cá`j`chết. 
7. Thêm xác suất chuyển tiếp. 

Xác suất đóng góp là:$dp[mask] \cdot \frac{a_{ij}}{\binom{k}{2}}$Chúng tôi chia cho số lần gặp có thể xảy ra vì mọi cặp không có thứ tự đều có khả năng được chọn như nhau. 
8. Tiếp tục cho đến khi tất cả mặt nạ được xử lý. 

Cuối cùng, mọi mặt nạ đơn đều nhận được tổng xác suất từ ​​tất cả các trạng thái lớn hơn. 
9. Trích xuất câu trả lời. 

Cá`i`tồn tại chính xác khi trạng thái cuối cùng là:```
mask = (1 << i)
```Tại sao nó hoạt động: 

Bất biến DP là:```
dp[mask] = probability that the process ever reaches exactly this alive set
```Trạng thái ban đầu là chính xác vì quá trình bắt đầu với tất cả cá còn sống. Mỗi quá trình chuyển đổi mô hình một sự kiện pháp lý của quy trình: chọn một cặp gặp gỡ thống nhất, sau đó áp dụng xác suất chiến đấu. Vì mọi sự kiện tiếp theo có thể xảy ra đều được tính chính xác một lần nên khối lượng xác suất được chuyển giao chính xác giữa các trạng thái. Bởi vì quá trình chuyển đổi chỉ loại bỏ cá, không có chu kỳ nào tồn tại và mọi trạng thái đều tích lũy đóng góp từ tất cả các trạng thái trước đó hợp lệ trước khi giá trị của nó được sử dụng thêm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = [list(map(float, input().split())) for _ in range(n)]

    size = 1 << n
    dp = [0.0] * size

    full = size - 1
    dp[full] = 1.0

    for mask in range(full, 0, -1):
        alive = []

        for i in range(n):
            if mask & (1 << i):
                alive.append(i)

        k = len(alive)

        if k <= 1:
            continue

        pairs = k * (k - 1) / 2.0

        for i in alive:
            for j in alive:
                if i == j:
                    continue

                next_mask = mask ^ (1 << j)
                dp[next_mask] += dp[mask] * a[i][j] / pairs

    ans = []

    for i in range(n):
        ans.append(f"{dp[1 << i]:.6f}")

    print(*ans)

solve()
```Mảng DP lưu trữ xác suất cho mọi tập hợp con cá còn sống. Mặt nạ đầy đủ chứa tất cả cá còn sống ban đầu, vì vậy xác suất của nó bắt đầu từ`1`. 

Vòng lặp bên ngoài lặp đi xuống thông qua các mặt nạ. Thứ tự này quan trọng vì quá trình chuyển đổi luôn loại bỏ chính xác một con cá. Một mặt nạ nhỏ hơn không bao giờ đóng góp trở lại thành một mặt nạ lớn hơn. 

Đối với mỗi mặt nạ, trước tiên chúng tôi thu thập cá còn sống. Điều này giúp đơn giản hóa việc lặp cặp và tránh việc kiểm tra bit lặp đi lặp lại. 

Biến`pairs`lưu trữ số lượng các cuộc họp không có thứ tự có thể có giữa các loài cá còn sống. Đây là yếu tố chuẩn hóa mà nhiều triển khai sai đã bỏ lỡ. Nếu không chia cho giá trị này, xác suất sẽ không tính tổng chính xác. 

Các vòng lặp lồng nhau lặp theo các cặp có thứ tự`(i, j)`. Đây là cố ý. Mỗi cặp có thứ tự tương ứng với một sự kiện loại bỏ cụ thể: cá`i`giết cá`j`. 

Sự chuyển tiếp:```
next_mask = mask ^ (1 << j)
```loại bỏ cá`j`từ tập sống. 

Độ chính xác của dấu phẩy động là đủ vì câu trả lời chỉ yêu cầu`1e-6`sự chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
0 0.5
0.5 0
```Các chuyển đổi trạng thái: 

| Mặt nạ hiện tại | Cá Sống | Số cặp | Chuyển tiếp | Đã thêm xác suất | 
| --- | --- | --- | --- | --- | 
| 11 | {0,1} | 1 | 0 giết 1 → 01 | 0,5 | 
| 11 | {0,1} | 1 | 1 giết 0 → 10 | 0,5 | 

DP cuối cùng: 

| Mặt nạ | Xác suất | 

|---|---|---| 

| 01 | 0,5 | 

| 10 | 0,5 | 

Cả hai con cá đều đối xứng hoàn hảo nên mỗi con sống sót với xác suất`1/2`. Điều này xác nhận việc chuẩn hóa chuyển tiếp là chính xác. 

### Ví dụ 2 

đầu vào:```
3
0 1 0
0 0 1
1 0 0
```Cá`0`luôn luôn đập`1`, cá`1`luôn luôn đập`2`, và cá`2`luôn luôn đập`0`. 

Trạng thái ban đầu: 

| Mặt nạ | Cá Sống | dp | 
| --- | --- | --- | 
| 111 | {0,1,2} | 1 | 

Chuyển đổi từ mặt nạ đầy đủ: 

| Cuộc họp | Mặt nạ kết quả | Xác suất | 
| --- | --- | --- | 
| 0 giết 1 | 101 | 1/3 | 
| 1 giết 2 | 011 | 1/3 | 
| 2 giết 0 | 110 | 1/3 | 

Bây giờ xử lý từng trạng thái 2 cá: 

| Mặt nạ hiện tại | Người chiến thắng | Mặt nạ cuối cùng | Đóng góp | 
| --- | --- | --- | --- | 
| 101 | 2 nhịp 0 | 100 | 1/3 | 
| 011 | 0 nhịp 1 | 001 | 1/3 | 
| 110 | 1 nhịp 2 | 010 | 1/3 | 

Xác suất cuối cùng: 

| Cá | Xác suất | 
| --- | --- | 
| 0 | 1/3 | 
| 1 | 1/3 | 
| 2 | 1/3 | 

Ví dụ này cho thấy rằng các kết quả theo cặp mang tính xác định không nhất thiết tạo ra người chiến thắng cuối cùng mang tính xác định. Thứ tự ngẫu nhiên của các cuộc họp rất quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^n · n²) | Đối với mỗi tập hợp con, chúng ta có thể kiểm tra từng cặp cá được đặt hàng | 
| Không gian | O(2^n) | Một giá trị DP cho mỗi tập hợp con | 

Vì`n = 18`, ta có nhiều nhất:```
2^18 = 262144
```tiểu bang. Hệ số bậc hai đối với các cặp cá vẫn có thể quản lý được trong giới hạn 3 giây trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n = int(input())
        a = [list(map(float, input().split())) for _ in range(n)]

        size = 1 << n
        dp = [0.0] * size

        full = size - 1
        dp[full] = 1.0

        for mask in range(full, 0, -1):
            alive = []

            for i in range(n):
                if mask & (1 << i):
                    alive.append(i)

            k = len(alive)

            if k <= 1:
                continue

            pairs = k * (k - 1) / 2.0

            for i in alive:
                for j in alive:
                    if i == j:
                        continue

                    nxt = mask ^ (1 << j)
                    dp[nxt] += dp[mask] * a[i][j] / pairs

        ans = []

        for i in range(n):
            ans.append(f"{dp[1 << i]:.6f}")

        return " ".join(ans)

    return solve()

# provided sample
assert run(
"""2
0 0.5
0.5 0
"""
) == "0.500000 0.500000"

# minimum size
assert run(
"""1
0
"""
) == "1.000000"

# deterministic dominance
assert run(
"""2
0 1
0 0
"""
) == "1.000000 0.000000"

# all equal probabilities
assert run(
"""3
0 0.5 0.5
0.5 0 0.5
0.5 0.5 0
"""
) == "0.333333 0.333333 0.333333"

# cyclic dominance
assert run(
"""3
0 1 0
0 0 1
1 0 0
"""
) == "0.333333 0.333333 0.333333"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cá đơn |`1.000000`| Trường hợp cơ bản không có chuyển tiếp | 
| Trận đấu 2 cá quyết định |`1.000000 0.000000`| Xử lý đúng xác suất 1 | 
| Hộp 3 cá đối xứng | phần ba bằng nhau | Bảo toàn xác suất | 
| Sự thống trị theo chu kỳ | phần ba bằng nhau | Thứ tự họp ngẫu nhiên ảnh hưởng đến kết quả | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
0
```Mặt nạ ban đầu đã chứa chính xác một con cá. DP bắt đầu bằng:```
dp[1] = 1
```Không có sự chuyển tiếp nào xảy ra vì`k <= 1`. Thuật toán xuất ra chính xác:```
1.000000
```Bây giờ hãy xem xét sự thống trị xác định:```
2
0 1
0 0
```Trạng thái ban đầu: 

| Mặt nạ | Còn sống | dp | 
| --- | --- | --- | 
| 11 | {0,1} | 1 | 

Chỉ có một cặp. Cá`0`đánh bại cá`1`với xác suất`1`. 

Chuyển tiếp: 

| Từ | Đến | Xác suất | 
| --- | --- | --- | 
| 11 | 01 | 1 | 

Kết quả cuối cùng:```
1.000000 0.000000
```Điều này xác minh rằng DP xử lý các xác suất cứng mà không có sự mất ổn định của dấu phẩy động. 

Cuối cùng, hãy xem xét trường hợp một số đường dẫn hợp nhất vào cùng một trạng thái:```
3
0 0.5 0.5
0.5 0 0.5
0.5 0.5 0
```Mặt nạ`{0,1}`có thể đến được bằng cá`0`giết chóc`2`hoặc cá`1`giết chóc`2`. DP thêm cả hai đóng góp vào cùng một giá trị trạng thái. 

Dấu vết: 

| Chuyển tiếp | Đóng góp cho Mặt nạ 011 | 
| --- | --- | 
| 0 giết 2 | 1/6 | 
| 1 giết 2 | 1/6 | 

Tổng cộng:```
dp[011] = 1/3
```Sự tích lũy này chính xác là lý do tại sao tập hợp con DP hoạt động ở đây. Lịch sử loại bỏ khác nhau sẽ rơi vào cùng một trạng thái tương lai.
