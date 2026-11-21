---
title: "CF 257A - Ổ cắm"
description: "Chúng tôi có sẵn k ổ cắm điện trong căn hộ. Có m thiết bị điện cuối cùng cũng cần nguồn điện. Chúng tôi cũng sở hữu n dải nguồn, trong đó dải thứ i cung cấp ổ cắm [i]. Một dải nguồn không được sử dụng miễn phí."
date: "2026-06-04T17:05:10+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 257
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 159 (Div. 2)"
rating: 1100
weight: 257
solve_time_s: 130
verified: true
draft: false
---

[CF 257A - Ổ cắm](https://codeforces.com/problemset/problem/257/A) 

**Đánh giá:** 1100 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`k`ổ cắm điện có sẵn trong căn hộ. có`m`các thiết bị điện cuối cùng cần nguồn điện. Chúng tôi cũng sở hữu`n`dải điện, nơi`i`-th dải cung cấp`a[i]`ổ cắm. 

Một dải nguồn không được sử dụng miễn phí. Để kích hoạt nó, chúng ta phải cắm nó vào một số ổ cắm điện hiện có. Điều đó tiêu thụ một ổ cắm và tạo ra`a[i]`ổ cắm mới. Lợi ích ròng từ việc sử dụng dải này là:`a[i] - 1`vì một ổ cắm được dùng để cắm chính dải đó. 

Nhiệm vụ là xác định số lượng dải nguồn tối thiểu phải được sử dụng để cuối cùng chúng ta có đủ ổ cắm điện cho tất cả mọi người.`m`thiết bị. Nếu ngay cả việc sử dụng mọi dải có sẵn vẫn không đủ, chúng tôi phải xuất`-1`. 

Những hạn chế là rất nhỏ. Có tối đa 50 dải nguồn và mỗi dải có tối đa 50 ổ cắm. Ngay cả các thuật toán hàm mũ cũng sẽ không thảm khốc ở quy mô này, nhưng vấn đề có cấu trúc tham lam đơn giản hơn nhiều dẫn đến lời giải cực kỳ ngắn. 

Một điểm tinh tế là một dải nguồn chỉ có một ổ cắm không hề tăng công suất chút nào. Việc cắm một dải như vậy sẽ tiêu tốn một ổ cắm và trả lại một ổ cắm, do đó mức tăng ròng bằng không. 

Một sai lầm dễ mắc phải khác là quên rằng ký tự ban đầu`k`ổ cắm đã được tính vào số ổ cắm có sẵn cho thiết bị. Coi như:```
1 3 3
10
```Câu trả lời đúng là`0`vì ba ổ cắm hiện có đã cấp nguồn cho cả ba thiết bị. Giải pháp luôn cố gắng sử dụng ít nhất một dải sẽ là sai. 

Trường hợp cạnh thứ hai xảy ra khi ngay cả tất cả các dải kết hợp cũng không đủ:```
2 10 1
2 2
```Bắt đầu với một ổ cắm, mỗi dải chỉ đóng góp một mức tăng ròng. Dung lượng tối đa trở thành ba ổ cắm, thấp hơn nhiều so với mười thiết bị. Câu trả lời đúng là`-1`. 

Trường hợp thứ ba liên quan đến các dải chỉ có một ổ cắm:```
3 4 1
1 1 10
```Hai dải đầu tiên không đóng góp gì. Dải hữu ích duy nhất là dải cuối cùng. Câu trả lời đúng là`1`. Việc coi mỗi dải là số lượng ổ cắm đóng góp của nó sẽ đánh giá quá cao dung lượng sẵn có. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi tập hợp con của dải nguồn. Đối với mỗi tập hợp con, chúng tôi có thể kiểm tra xem các dải đó có thể cung cấp đủ ổ cắm và giữ kích thước tập hợp con hợp lệ nhỏ nhất hay không. 

Điều này hoạt động vì`n`chỉ là 50 trong phát biểu bài toán thực, nhưng số tập con thì`2^50`, đại khái là`1.1 × 10^15`. Điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là thuộc tính liên quan duy nhất của dải điện là sự đóng góp ròng của nó,`a[i] - 1`. 

Giả sử chúng ta hiện có sẵn một số ổ cắm điện. Nếu chúng ta quyết định sử dụng dải khác, việc chọn dải có mức tăng ròng lớn hơn không bao giờ có thể tệ hơn việc chọn dải có mức tăng ròng nhỏ hơn. Cả hai đều sử dụng chính xác một ổ cắm để kết nối, nhưng dải lớn hơn sẽ trả về nhiều ổ cắm hơn sau đó. 

Điều này có nghĩa là bất cứ khi nào chúng ta cần thêm dung lượng, chúng ta phải luôn lấy dải có số lượng ổ cắm lớn nhất trước tiên. Sau khi sắp xếp các dải theo thứ tự giảm dần, chúng tôi liên tục thêm chúng cho đến khi tổng dung lượng đạt ít nhất`m`. 

Lựa chọn tham lam là tối ưu vì mỗi dải được chọn có giá đúng bằng một đơn vị trong câu trả lời. Nếu chúng ta muốn tiếp cận mục tiêu bằng cách sử dụng càng ít dải càng tốt, chúng ta nên tối đa hóa công suất đạt được của mỗi dải đã chọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tham lam tối ưu | O(n log n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`, Và`k`. 
2. Nếu`k >= m`, đầu ra`0`. 

Các ổ cắm trên tường hiện có đã cấp nguồn cho tất cả các thiết bị nên không cần dây nguồn. 
3. Sắp xếp tất cả các ổ cắm theo thứ tự giảm dần về số lượng ổ cắm. 
4. Hãy để`available = k`. 

Con số này thể hiện tổng số ổ cắm điện hiện có sẵn cho các thiết bị và dải kết nối trong tương lai. 
5. Đi qua các dải được sắp xếp từ lớn nhất đến nhỏ nhất. 
6. Đối với mỗi dải có`a`ổ cắm, cập nhật:```
available += a - 1
```Một ổ cắm được sử dụng để cắm dải và`a`ổ cắm mới xuất hiện. 
7. Đếm xem đã sử dụng bao nhiêu dải. 
8. Sau mỗi lần thêm, kiểm tra xem`available >= m`. 

Ngay khi điều này trở thành sự thật, hãy xuất ra số lượng dải được sử dụng. 
9. Nếu tất cả các dải đã được xử lý và`available < m`, đầu ra`-1`. 

### Tại sao nó hoạt động 

Mỗi dải nguồn được sử dụng sẽ tăng công suất một cách chính xác`a[i] - 1`. Mỗi dải đóng góp vào câu trả lời với cùng một chi phí, tức là sử dụng thêm một dải. 

Để giảm thiểu số lượng dải, chúng tôi muốn tăng công suất lớn nhất có thể từ mỗi dải đã chọn. Nếu giải pháp sử dụng dải có độ lợi`x`trong khi lợi nhuận chưa sử dụng lớn hơn`y > x`tồn tại, thay thế`x`với`y`không thể giảm tổng công suất và có thể tăng nó. Việc áp dụng nhiều lần lập luận trao đổi này sẽ biến bất kỳ giải pháp tối ưu nào thành giải pháp sử dụng lợi ích lớn nhất trước tiên. 

Thuật toán tham lam xử lý các dải theo đúng thứ tự đó nên ngay lần đầu tiên nó đạt công suất`m`, nó đã sử dụng số lượng dải tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))

    if k >= m:
        print(0)
        return

    a.sort(reverse=True)

    available = k

    for i, sockets in enumerate(a, 1):
        available += sockets - 1
        if available >= m:
            print(i)
            return

    print(-1)

solve()
```Kiểm tra đầu tiên xử lý trường hợp ổ cắm trên tường hiện có đã đủ. 

Bước phân loại là trọng tâm của chiến lược tham lam. Dải nguồn lớn hơn mang lại lợi ích ròng lớn hơn, vì vậy chúng phải được xem xét trước tiên. 

Biến`available`theo dõi tổng công suất ổ cắm hiện tại. Khi một dải với`sockets`ổ cắm được kết nối, một ổ cắm được tiêu thụ và`sockets`được tạo ra, mang lại mức tăng ròng của`sockets - 1`. 

Vòng lặp sử dụng`enumerate(..., 1)`sao cho chỉ số vòng lặp trực tiếp bằng số dải được sử dụng cho đến nay. Lần đầu tiên công suất đạt ít nhất`m`, số đó chính là câu trả lời. 

Một lỗi triển khai phổ biến là thêm`sockets`thay vì`sockets - 1`. Làm như vậy sẽ quên mất ổ cắm cần thiết để cắm dải và tạo ra dung lượng quá lớn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 5 3
3 1 2
```Sau khi sắp xếp:```
[3, 2, 1]
```| Bước | Dải được sử dụng | Có sẵn trước | Lợi nhuận ròng | Có sẵn sau | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | Không có | 3 | - | 3 | 
| 1 | 3 | 3 | 2 | 5 | 

Tại thời điểm này`available = 5`, phù hợp`m = 5`. Câu trả lời là`1`. 

Ví dụ này cho thấy rằng một dải lớn có thể là đủ vì mức tăng ròng của nó mới là vấn đề chứ không phải số lượng ổ cắm thô. 

### Mẫu 2 

Hãy xem xét:```
4 7 1
1 3 1 5
```Sau khi sắp xếp:```
[5, 3, 1, 1]
```| Bước | Dải được sử dụng | Có sẵn trước | Lợi nhuận ròng | Có sẵn sau | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | Không có | 1 | - | 1 | 
| 1 | 5 | 1 | 4 | 5 | 
| 2 | 3 | 5 | 2 | 7 | 

Sau hai dải, dung lượng đạt tới bảy, vì vậy câu trả lời là`2`. 

Dấu vết này minh họa tại sao việc chọn dải lớn nhất trước tiên là tối ưu. Việc sử dụng dải một ổ cắm trước đó sẽ lãng phí một bước mà không tăng dung lượng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Thống trị bằng cách phân loại các dải điện | 
| Không gian | O(1) thêm | Ngoài mảng đầu vào và sắp xếp nội bộ | 

Với`n ≤ 50`, thời gian chạy rất nhỏ. Ngay cả việc sắp xếp cũng có hiệu quả ngay lập tức và giải pháp phù hợp một cách thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))

    if k >= m:
        return "0"

    a.sort(reverse=True)

    available = k

    for i, sockets in enumerate(a, 1):
        available += sockets - 1
        if available >= m:
            return str(i)

    return "-1"

# sample
assert run("3 5 3\n3 1 2\n") == "1", "sample 1"

# minimum size
assert run("1 1 1\n1\n") == "0", "already enough sockets"

# impossible case
assert run("2 10 1\n2 2\n") == "-1", "cannot reach target"

# all equal values
assert run("4 8 2\n3 3 3 3\n") == "3", "equal strips"

# single useful strip
assert run("3 4 1\n1 1 10\n") == "1", "ignore zero-gain strips"

# boundary where exact capacity is reached
assert run("2 5 2\n3 2\n") == "2", "exactly reaches target"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 1`|`0`| Không cần dải | 
|`2 10 1 / 2 2`|`-1`| Cấu hình không thể | 
|`4 8 2 / 3 3 3 3`|`3`| Lợi ích bằng nhau lặp đi lặp lại | 
|`3 4 1 / 1 1 10`|`1`| Dải không tăng được xử lý chính xác | 
|`2 5 2 / 3 2`|`2`| Phạm vi ranh giới chính xác | 

## Vỏ cạnh 

Hãy xem xét:```
1 3 3
10
```Thuật toán kiểm tra ngay lập tức`k >= m`. Từ`3 >= 3`, nó xuất ra`0`. Không cần phân loại hoặc lựa chọn tham lam. Điều này tránh việc sử dụng dải không đúng cách khi các ổ cắm hiện có đã đủ. 

Coi như:```
2 10 1
2 2
```Sau khi phân loại, các dải vẫn còn`[2, 2]`. 

Sự tiến triển là:```
available = 1
available = 2
available = 3
```Tất cả các dải đã cạn kiệt trong khi dung lượng vẫn dưới mười. Đầu ra của thuật toán`-1`, phát hiện chính xác những điều không thể. 

Coi như:```
3 4 1
1 1 10
```Sau khi sắp xếp:```
[10, 1, 1]
```Dải đầu tiên thêm`10 - 1 = 9`, do đó công suất trở thành:```
1 + 9 = 10
```Mục tiêu đã đạt được và câu trả lời là`1`. Dải một ổ cắm không bao giờ cần phải được sử dụng. Điều này chứng tỏ tại sao thứ tự tham lam lại quan trọng và tại sao mức tăng phải được tính là`a[i] - 1`còn hơn là`a[i]`.
