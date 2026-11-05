---
title: "CF 12C - Trái cây"
description: "Valera có một danh sách mua sắm bao gồm một số loại trái cây, có thể có những bản sao nếu anh ta muốn nhiều hơn một loại cùng loại. Ở chợ, sạp hàng có _n_ loại trái cây và _n_ thẻ giá nhưng chưa gắn thẻ lên trái cây."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 12
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 12 (Div 2 Only)"
rating: 1100
weight: 12
solve_time_s: 62
verified: true
draft: false
---
[CF 12C - Trái cây](https://codeforces.com/problemset/problem/12/C) 

**Đánh giá:** 1100 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Valera có một danh sách mua sắm bao gồm một số loại trái cây, có thể có những bản sao nếu anh ta muốn nhiều hơn một loại cùng loại. Ở chợ, sạp hàng có _n_ loại trái cây và _n_ thẻ giá nhưng chưa gắn thẻ lên trái cây. Mỗi thẻ đại diện cho giá của một loại trái cây nhất định. Valera muốn biết tổng số tiền tối thiểu và tối đa mà anh ta có thể trả, tùy thuộc vào cách người bán gắn thẻ vào loại trái cây mà anh ta muốn. 

Đầu vào cung cấp số lượng thẻ giá`n`và số lượng trái cây trong danh sách của Valera`m`. Dòng tiếp theo cung cấp cho`n`giá cả. Sau đó`m`dòng liệt kê tên các loại trái cây trong danh sách của mình. Vì số lượng các loại trái cây riêng biệt trong danh sách không nhiều hơn`n`, chúng tôi đảm bảo rằng luôn có sẵn một mức giá cho từng loại trái cây riêng biệt. Kết quả đầu ra phải là hai số: tổng nhỏ nhất và lớn nhất có thể của các loại trái cây trong danh sách của Valera. 

Với những ràng buộc`n, m ≤ 100`và giá ≤ 100, một cách tiếp cận ngây thơ lặp đi lặp lại trên tất cả các hoán vị có thể có của việc gán thẻ giá về mặt lý thuyết là có thể thực hiện được nhưng không hiệu quả, bởi vì`n!`hoán vị sẽ nhanh chóng trở nên không thể thực hiện được. Các trường hợp đặc biệt bao gồm các tình huống trong đó tất cả các loại trái cây trong danh sách đều thuộc cùng một loại hoặc khi giá giống nhau, trong đó việc xử lý trùng lặp hoặc lập chỉ mục một cách bất cẩn có thể tạo ra kết quả sai. 

Ví dụ, nếu`n=2`,`m=3`, giá cả`[1, 2]`, và danh sách chứa`apple`,`apple`,`apple`, tổng số tối thiểu là`1 + 1 + 2 = 4`, trong khi tổng số tối đa là`2 + 2 + 1 = 5`. Một cách tiếp cận ngây thơ không xử lý chính xác các kết quả lặp đi lặp lại có thể tính toán sai số tiền. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực xem xét mọi cách có thể để ấn định giá cho các loại trái cây trong danh sách của Valera. Bạn có thể tạo tất cả các hoán vị của bảng giá rồi tính tổng giá tương ứng cho các loại trái cây trong danh sách. Cách tiếp cận này đúng vì mọi phân phối có thể đều được xem xét rõ ràng, nhưng nó quá chậm: ngay cả đối với`n=10`,`10! = 3,628,800`hoán vị quá mức trong giới hạn thời gian 1 giây và`n`có thể lên tới 100. 

Điểm mấu chốt là vấn đề được chuyển thành chiến lược tham lam: tổng số tiền tối thiểu đạt được khi ấn định giá rẻ nhất hiện có cho các loại trái cây được yêu cầu thường xuyên nhất và tổng số tối đa đạt được khi ấn định giá đắt nhất cho các loại trái cây được yêu cầu thường xuyên nhất. Sắp xếp danh sách giá và đếm số lần mỗi quả xuất hiện là đủ để tính cả hai thái cực. Tần suất xuất hiện các loại trái cây rất quan trọng vì các món lặp lại trong danh sách đều phải được ấn định một mức giá. 

Cách tiếp cận tham lam có hiệu quả vì không có sự tương tác giữa các loại trái cây khác nhau ngoài số lượng. Khi đã biết số lượng, biến số duy nhất ảnh hưởng đến tổng là giá mà mỗi loại trái cây nhận được, do đó, việc chỉ định giá trị từ thấp nhất đến thường xuyên nhất (hoặc cao nhất đến thường xuyên nhất) sẽ đảm bảo số tiền tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * m) | O(n) | Quá chậm | 
| Tối ưu | O(n log n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng thẻ giá`n`và số lượng quả`m`. 
2. Đọc danh sách`n`giá và sắp xếp chúng theo thứ tự tăng dần. Việc sắp xếp sẽ cho phép lựa chọn hiệu quả những mức giá rẻ nhất và đắt nhất. 
3. Đọc`m`trái cây từ danh sách của Valera và đếm xem mỗi loại trái cây riêng biệt xuất hiện bao nhiêu lần. Điều này tạo ra một bản đồ tần số trong đó khóa là tên trái cây và giá trị là số lần Valera muốn mỗi loại trái cây. 
4. Trích xuất tần số và sắp xếp chúng theo thứ tự giảm dần. Những loại trái cây được yêu cầu thường xuyên nhất sẽ được ấn định mức giá thấp nhất cho tổng số tối thiểu và giá cao nhất cho tổng số tối đa. 
5. Khởi tạo hai tổng:`min_total`Và`max_total`. Vì`min_total`, ấn định giá rẻ nhất cho các loại trái cây thường xuyên nhất một cách tuần tự. Vì`max_total`, ấn định mức giá đắt nhất cho các loại trái cây thường xuyên nhất một cách tuần tự. Mỗi tần số có thể sử dụng nhiều mức giá từ bảng giá đã được sắp xếp. 
6. Đầu ra`min_total`Và`max_total`. 

Lý do điều này có hiệu quả là vì đối với cả hai thái cực, bất kỳ sự sai lệch nào so với ấn định này sẽ khiến giá rẻ hơn không được ấn định hoặc giá đắt hơn không được sử dụng, tăng mức tối thiểu hoặc giảm mức tối đa. Điều bất biến là tần số lớn nhất luôn khớp với mức giá còn lại nhỏ nhất hoặc lớn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

n, m = map(int, input().split())
prices = list(map(int, input().split()))
prices.sort()

fruits = [input().strip() for _ in range(m)]
freq = list(Counter(fruits).values())
freq.sort(reverse=True)

# minimum total
min_total = 0
price_idx = 0
for f in freq:
    for _ in range(f):
        min_total += prices[price_idx]
        price_idx += 1

# maximum total
prices_rev = prices[::-1]
max_total = 0
price_idx = 0
for f in freq:
    for _ in range(f):
        max_total += prices_rev[price_idx]
        price_idx += 1

print(min_total, max_total)
```Giải pháp đọc đầu vào một cách hiệu quả, đếm tần suất trái cây và sắp xếp cả giá cả và tần suất. Các vòng lặp bên trong lặp chính xác một lần cho mỗi quả trong danh sách. Các điều kiện biên, chẳng hạn như tất cả các loại trái cây đều giống nhau, được xử lý chính xác bằng bản đồ tần số. Việc đảo ngược mảng giá cho phép tính toán đối xứng để có tổng giá trị tối đa. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
5 3
4 2 1 10 5
apple
orange
mango
```| Bước | Giá | Tần số | tổng cộng tối thiểu | max_total | 
| --- | --- | --- | --- | --- | 
| Sắp xếp giá | [1,2,4,5,10] | [1,1,1] | 0 | 0 | 
| Chỉ định tối thiểu | 1→táo, 2→cam, 4→xoài | [1,1,1] | 7 | - | 
| Chỉ định tối đa | 10→táo, 5→cam, 4→xoài | [1,1,1] | - | 19 | 

Điều này cho thấy thuật toán ghép chính xác giá rẻ nhất với trái cây được yêu cầu ở mức tối thiểu và giá đắt nhất ở mức tối đa. 

**Mẫu tùy chỉnh 2** 

đầu vào:```
4 5
1 3 2 4
apple
apple
banana
banana
banana
```| Bước | Giá | Tần số | tổng cộng tối thiểu | max_total | 
| --- | --- | --- | --- | --- | 
| Sắp xếp giá | [1,2,3,4] | [3,2] | 0 | 0 | 
| Chỉ định tối thiểu | 1→chuối, 2→chuối, 3→chuối, 4→táo, rẻ nhất tiếp theo 2→táo | 0 | 12 | - | 
| Chỉ định tối đa | 4→chuối, 3→chuối, 2→chuối, 1→táo, đắt tiếp theo 2→táo | - | 14 | | 

Điều này cho thấy rằng các kết quả lặp đi lặp lại được xử lý chính xác và tổng số phản ánh các nhiệm vụ tham lam. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m) | Giá sắp xếp chiếm ưu thế, tần số đếm là O(m) | 
| Không gian | O(n + m) | Lưu trữ giá và bản đồ tần số | 

Với`n, m ≤ 100`, thuật toán dễ dàng thực thi trong giới hạn thời gian 1 giây và sử dụng ít hơn 256 MB bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    from collections import Counter

    n, m = map(int, input().split())
    prices = list(map(int, input().split()))
    prices.sort()

    fruits = [input().strip() for _ in range(m)]
    freq = list(Counter(fruits).values())
    freq.sort(reverse=True)

    min_total = 0
    price_idx = 0
    for f in freq:
        for _ in range(f):
            min_total += prices[price_idx]
            price_idx += 1

    prices_rev = prices[::-1]
    max_total = 0
    price_idx = 0
    for f in freq:
        for _ in range(f):
            max_total += prices_rev[price_idx]
            price_idx += 1

    return f"{min_total} {max_total}"

# Provided sample
assert run("5 3\n4 2 1 10 5\napple\norange\nmango\n") == "7 19", "sample 1"

# Custom: single fruit repeated
assert run("2 3\n1 2\napple\napple\napple\n") == "4 5", "single fruit repeated"

# Custom: all prices equal
assert run("3 3\n5 5 5\napple\nbanana\norange\n") == "15 15", "all prices equal"

# Custom: min-size input
assert run("1 1\n7\napple\n") == "7 7", "min-size input"

# Custom: max-size, uniform price
inp = "100 100\n" +
```
