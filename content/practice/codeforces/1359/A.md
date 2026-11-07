---
title: "CF 1359A - Xì phé Berland"
description: "Chúng ta có một bộ bài chứa n lá bài, trong đó có m lá bài joker. Các lá bài được chia đều cho k người chơi nên mỗi người chơi nhận được chính xác n/k lá bài. Điểm số chỉ phụ thuộc vào cách phân phối các trò đùa. Giả sử một người chơi có số lượng người pha trò nhiều nhất."
date: "2026-06-11T13:06:05+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1359
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 88 (Rated for Div. 2)"
rating: 1000
weight: 1359
solve_time_s: 133
verified: true
draft: false
---

[CF 1359A - Berland Poker](https://codeforces.com/problemset/problem/1359/A) 

**Đánh giá:** 1000 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ bài chứa`n`thẻ, trong đó`m`là những người thích đùa. Các thẻ được chia đều cho`k`người chơi, vì vậy mọi người chơi đều nhận được chính xác`n / k`thẻ. 

Điểm số chỉ phụ thuộc vào cách phân phối các trò đùa. Giả sử một người chơi có số lượng người pha trò nhiều nhất. Cho phép`x`là số quân joker trong tay người chơi đó và lấy`y`trở thành người pha trò lớn nhất trong số tất cả những người chơi khác. Điểm của người chiến thắng là`x - y`. Nếu nhiều người chơi hòa nhau để có số lượng joker lớn nhất, điểm số sẽ trở thành`0`. 

Chúng ta có thể tự do tưởng tượng cách phân phối trò đùa nhằm tối đa hóa điểm số của người chiến thắng. Nhiệm vụ là tính số điểm tối đa có thể. 

Những hạn chế là rất nhỏ. Có tối đa 500 trường hợp thử nghiệm và`n ≤ 50`. Ngay cả những cách tiếp cận khá kém hiệu quả cũng sẽ hoạt động thoải mái trong giới hạn. Tuy nhiên, cấu trúc của bài toán dẫn đến một giải pháp toán học trực tiếp chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Trường hợp tinh tế đầu tiên xảy ra khi có ít người pha trò hơn khả năng bài của một người chơi. 

Ví dụ:```
n = 8, m = 3, k = 2
```Mỗi người chơi có thể giữ`4`thẻ. Một người chơi có thể lấy tất cả`3`những kẻ pha trò, không để lại gì cho người chơi khác. Câu trả lời là`3`, không phải thứ gì đó bắt nguồn từ việc phân bổ đều những người pha trò. 

Trường hợp tinh tế thứ hai xảy ra khi có nhiều người pha trò hơn mức mà một người chơi có thể nắm giữ. 

Ví dụ:```
n = 8, m = 7, k = 2
```Một người chơi có thể giữ tối đa`4`những kẻ pha trò vì bàn tay của họ chỉ chứa`4`thẻ. Sau khi cho một người chơi`4`những kẻ thích đùa, vẫn còn đó`3`những kẻ thích đùa đã rời đi. Những người pha trò còn lại phải chuyển sang người chơi khác nên tỷ số là`4 - 3 = 1`. 

Một giải pháp bất cẩn mà chỉ đơn giản là trả lại`min(m, n/k)`sẽ quay lại không chính xác`4`. 

Một trường hợp quan trọng khác là khi những người pha trò còn lại chia đều cho những người chơi khác. 

Ví dụ:```
n = 12, m = 9, k = 3
```Người chiến thắng được`4`những kẻ thích đùa. Còn lại năm gã hề. Để giảm thiểu đối thủ mạnh nhất, chúng tôi dàn trải chúng một cách đồng đều nhất có thể giữa hai người chơi. Một đối thủ phải nhận`ceil(5/2) = 3`những kẻ thích đùa. Điểm số là`4 - 3 = 1`. 

Sử dụng cách chia tầng ở đây sẽ tạo ra sai số`4 - 2 = 2`. 

Cuối cùng, khi không có người pha trò nào cả:```
n = 42, m = 0, k = 7
```Mọi người đều không có người pha trò, vì vậy mọi người hòa nhau và tỷ số là`0`. 

## Phương pháp tiếp cận 

Tư duy bạo lực bắt đầu bằng việc đặt câu hỏi làm thế nào để phân bổ những trò đùa giữa những người chơi. Chúng ta có thể liệt kê mọi phân phối có thể có của`m`những kẻ pha trò xuyên qua`k`người chơi đồng thời tôn trọng giới hạn kích thước bàn tay`n/k`. Đối với mỗi lần phân phối, chúng tôi có thể tính điểm của người chiến thắng và giữ mức tối đa. 

Điều này hoạt động về mặt khái niệm vì điểm số chỉ phụ thuộc vào số lượng người pha trò. Vấn đề là số lượng phân phối hợp lệ tăng lên theo kiểu tổ hợp. Mặc dù những ràng buộc thực tế là nhỏ, cách tiếp cận này không mở rộng quy mô và hoàn toàn bỏ qua cấu trúc của trò chơi. 

Quan sát quan trọng là số lượng người pha trò của người chiến thắng trước tiên phải được tối đa hóa. Vì mỗi người chơi chỉ giữ`n/k`thẻ, không người chơi nào có thể có nhiều hơn`n/k`những kẻ thích đùa. Do đó, người chiến thắng tốt nhất có thể nhận được```
x = min(m, n/k)
```những kẻ thích đùa. 

Nếu tất cả các trò đùa đều nằm gọn trong một tay, nghĩa là`m ≤ n/k`, thì những người chơi còn lại sẽ không nhận được trò đùa nào và câu trả lời chỉ đơn giản là`m`. 

Trường hợp thú vị là khi một số người pha trò vẫn ở lại sau khi đã xếp đầy ván bài của người chiến thắng. Cho phép```
remaining = m - x
```Những trò đùa này phải được phân phối cho những người khác`k - 1`người chơi. 

Để tối đa hóa điểm số, chúng tôi muốn đối thủ mạnh nhất có càng ít người pha trò càng tốt. Chiến lược tốt nhất là chia đều số joker còn lại cho những người chơi khác. 

Nếu như`remaining`những người pha trò được phân phát giữa`k - 1`người chơi, số lượng joker đối thủ lớn nhất là```
ceil(remaining / (k - 1))
```Sử dụng số học số nguyên:```
(remaining + k - 2) // (k - 1)
```Điểm cuối cùng là```
x - ceil(remaining / (k - 1))
```Điều này làm giảm toàn bộ vấn đề thành một vài phép tính số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong`m`Và`k`| Hàm mũ | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số lượng joker tối đa mà người chiến thắng có thể nhận được:```
x = min(m, n / k)
```Không người chơi nào có thể giữ nhiều hơn`n/k`bài, vì vậy không người chơi nào có thể giữ nhiều hơn số lá bài pha trò đó. 
2. Tính xem còn lại bao nhiêu người pha trò sau khi đưa cho người chiến thắng càng nhiều càng tốt:```
remaining = m - x
```3. Phân phối những người pha trò còn lại cho những người khác`k - 1`người chơi một cách đồng đều nhất có thể. 

Đối thủ mạnh nhất khi đó sẽ có```
y = ceil(remaining / (k - 1))
```những kẻ thích đùa. 
4. Trở về```
x - y
```vì đây chính xác là điểm của người chiến thắng. 

### Tại sao nó hoạt động 

Điểm của người chiến thắng tăng lên khi người chiến thắng nhận được nhiều người pha trò hơn và giảm khi đối thủ nhận được nhiều người pha trò hơn. 

Cho ít hơn`min(m, n/k)`những kẻ pha trò đối với người chiến thắng không bao giờ có thể giúp ích được gì, bởi vì mỗi kẻ pha trò bị loại khỏi người chiến thắng đều sẽ làm giảm giá trị`x`trực tiếp hoặc để nó không thay đổi trong khi vẫn tồn tại ở một nơi khác. Vì vậy, giải pháp tối ưu luôn tối đa hóa`x`. 

Một lần`x`đã được sửa, mục tiêu duy nhất còn lại là giảm thiểu số lượng joker của đối thủ lớn nhất. Trong số tất cả các bản phân phối của`remaining`những kẻ pha trò xuyên qua`k - 1`người chơi, tải tối đa nhỏ nhất có thể đạt được bằng cách phân phối chúng đồng đều nhất có thể. Tải tối đa đó chính xác là`ceil(remaining / (k - 1))`. 

Vì cả hai phần đều tối ưu độc lập nên điểm kết quả là điểm tối đa có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, m, k = map(int, input().split())

        max_jokers = min(m, n // k)
        remaining = m - max_jokers

        opponent = (remaining + k - 2) // (k - 1)

        ans.append(str(max_jokers - opponent))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Phép tính đầu tiên tìm ra số lượng joker lớn nhất mà người chiến thắng có thể nắm giữ. Sự hạn chế về kích thước bàn tay khiến`n // k`giới hạn tuyệt đối. 

Biến`remaining`theo dõi những người pha trò phải được chỉ định cho những người chơi khác. 

biểu hiện```
(remaining + k - 2) // (k - 1)
```tính toán phép chia trần mà không sử dụng số học dấu phẩy động. Đây là nơi thường xảy ra sai sót nhất. Sử dụng cách chia sàn thông thường sẽ đánh giá thấp đối thủ mạnh nhất khi số người pha trò còn lại không chia đều. 

Tất cả số học dễ dàng phù hợp với số nguyên Python. Các ràng buộc rất nhỏ nhưng công thức sẽ vẫn an toàn ngay cả đối với các giá trị lớn hơn nhiều. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8 3 2
```| Biến | Giá trị | 
| --- | --- | 
| hand_size = n/k | 4 | 
| max_jokers | 3 | 
| còn lại | 0 | 
| đối thủ | 0 | 
| trả lời | 3 | 

Người chiến thắng có thể lấy cả ba lá bài pha trò vì một ván bài có thể giữ bốn lá bài. Không còn kẻ pha trò nào cho bất kỳ ai khác, vì vậy tỷ số là`3`. 

### Ví dụ 2 

đầu vào:```
9 6 3
```| Biến | Giá trị | 
| --- | --- | 
| hand_size = n/k | 3 | 
| max_jokers | 3 | 
| còn lại | 3 | 
| đối thủ | 2 | 
| trả lời | 1 | 

Sau khi đưa cho người chiến thắng ba lá joker, chỉ còn lại ba lá. Chúng phải được phân phối cho hai người chơi. Sự phân phối cân bằng nhất là`2`Và`1`, vậy đối thủ mạnh nhất có`2`những kẻ thích đùa. Điểm số trở thành`3 - 2 = 1`. 

Ví dụ này chứng minh tại sao cần phải phân chia trần. Ba người pha trò còn lại không thể chia thành hai nhóm cỡ một. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một vài phép tính số học | 
| Không gian | O(1) | Không có cấu trúc dữ liệu phụ trợ | 

Ngay cả với 500 trường hợp thử nghiệm, tổng công việc cũng không đáng kể. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, m, k = map(int, input().split())

        x = min(m, n // k)
        rem = m - x
        y = (rem + k - 2) // (k - 1)

        out.append(str(x - y))

    return "\n".join(out) + "\n"

# provided sample
assert run(
"""4
8 3 2
4 2 4
9 6 3
42 0 7
"""
) == """3
0
1
0
"""

# minimum meaningful case
assert run(
"""1
2 0 2
"""
) == """0
"""

# all jokers fit in one hand
assert run(
"""1
8 4 2
"""
) == """4
"""

# ceiling division case
assert run(
"""1
12 9 3
"""
) == """1
"""

# near maximum values
assert run(
"""1
50 50 2
"""
) == """0
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 0 2`|`0`| Không có người pha trò trong bộ bài | 
|`8 4 2`|`4`| Tất cả những trò đùa đều nằm gọn trong một tay | 
|`12 9 3`|`1`| Phân chia trần đúng cách | 
|`50 50 2`|`0`| Buộc ở quy mô tối đa | 

## Vỏ cạnh 

Hãy xem xét:```
1
8 3 2
```Chúng tôi tính toán`x = min(3, 4) = 3`. Không còn kẻ pha trò nào nữa, vì vậy`remaining = 0`. Đối thủ mạnh nhất có`0`những kẻ thích đùa. Câu trả lời là`3 - 0 = 3`. 

Điều này xử lý trường hợp tất cả các trò đùa nằm gọn trong tay một người chơi. Thuật toán ngay lập tức nhận ra rằng không có đối thủ nào có thể nhận được một joker. 

Coi như:```
1
8 7 2
```Chúng tôi tính toán`x = min(7, 4) = 4`. Còn lại ba gã hề. Vì chỉ có một đối thủ nên đối thủ đó phải nhận đủ 3 quân joker còn lại. Thuật toán tính toán```
y = ceil(3 / 1) = 3
```và trả về`4 - 3 = 1`. 

Điều này xác nhận rằng giới hạn kích thước bàn tay được tôn trọng và những trò đùa còn sót lại vẫn quan trọng. 

Coi như:```
1
12 9 3
```Chúng tôi tính toán`x = 4`,`remaining = 5`, Và```
y = ceil(5 / 2) = 3
```Câu trả lời trở thành`1`. 

Đây là trường hợp chia trần cổ điển. Bất kỳ giải pháp nào sử dụng cách chia tầng sẽ tạo ra sai số`2`. 

Coi như:```
1
42 0 7
```Chúng tôi tính toán`x = 0`,`remaining = 0`, Và`y = 0`. Câu trả lời là`0`. 

Thuật toán xử lý tình huống một cách tự nhiên khi không có người pha trò nào cả, tạo ra tỷ số hòa như mong đợi.
