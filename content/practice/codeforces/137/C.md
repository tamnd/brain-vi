---
title: "CF 137C - Lịch sử"
description: "Mỗi sự kiện lịch sử được biểu thị bằng một khoảng thời gian [a, b], trong đó a là năm bắt đầu và b là năm kết thúc. Một sự kiện được coi là nằm trong một sự kiện khác nếu nó bắt đầu muộn hơn và kết thúc sớm hơn."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "sortings"]
categories: ["algorithms"]
codeforces_contest: 137
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 98 (Div. 2)"
rating: 1500
weight: 137
solve_time_s: 96
verified: true
draft: false
---

[CF 137C - Lịch sử](https://codeforces.com/problemset/problem/137/C) 

**Đánh giá:** 1500 
**Thẻ:** sắp xếp 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi sự kiện lịch sử được thể hiện bằng một khoảng thời gian`[a, b]`, Ở đâu`a`là năm bắt đầu và`b`là năm kết thúc. Một sự kiện được coi là nằm trong một sự kiện khác nếu nó bắt đầu muộn hơn và kết thúc sớm hơn. Chính thức, sự kiện`j`chứa sự kiện`i`khi:

-`a_j < a_i`-`b_i < b_j`Nhiệm vụ là đếm xem có bao nhiêu sự kiện được chứa trong ít nhất một sự kiện khác. 

Đầu vào cho đến`10^5`khoảng thời gian. Một giải pháp bậc hai so sánh mọi cặp sự kiện sẽ yêu cầu khoảng`10^10`so sánh trong trường hợp xấu nhất, vượt xa giới hạn 2 giây. Điều này ngay lập tức loại trừ bất kỳ`O(n^2)`tiếp cận. Chúng ta cần thứ gì đó xung quanh`O(n log n)`, gợi ý mạnh mẽ về việc sắp xếp. 

Một chi tiết tinh tế là việc ngăn chặn phụ thuộc vào cả hai tọa độ cùng một lúc. Chỉ nhìn vào những năm đầu hay chỉ nhìn vào những năm cuối là chưa đủ. 

Hãy xem xét ví dụ này:```
3
1 10
2 5
6 9
```Câu trả lời đúng là:```
2
```Cả hai`[2,5]`Và`[6,9]`đang ở bên trong`[1,10]`. 

Việc triển khai bất cẩn chỉ kiểm tra các khoảng lân cận sau khi sắp xếp có thể bỏ sót một trong số chúng. 

Một sai lầm dễ mắc phải khác là quên rằng bất đẳng thức nghiêm ngặt là cần thiết. Nếu các điểm cuối bằng nhau được cho phép, chúng ta sẽ cần phân biệt cẩn thận giữa ngăn chặn nghiêm ngặt và không nghiêm ngặt. Vấn đề này tránh được sự phức tạp đó vì tất cả phần bắt đầu và tất cả phần kết thúc đều khác biệt, nhưng thuật toán vẫn phải dựa vào thứ tự nghiêm ngặt một cách chính xác. 

Ví dụ này chứng minh tại sao việc theo dõi năm kết thúc tối đa cho đến nay lại có tác dụng:```
4
1 4
2 10
3 5
6 8
```Câu trả lời đúng là:```
1
```Chỉ một`[3,5]`được chứa đựng, bởi vì nó nằm bên trong`[2,10]`. 

Kiểm tra "khoảng thời gian hiện tại kết thúc sớm hơn khoảng thời gian trước đó" sẽ được tính không chính xác`[6,8]`, mặc dù điểm bắt đầu của nó đã ở bên ngoài`[2,10]`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi khoảng thời gian`i`, quét tất cả các khoảng khác`j`và kiểm tra xem:```
a_j < a_i and b_i < b_j
```Nếu có khoảng thời gian như vậy, hãy đánh dấu`i`như chứa đựng. 

Điều này hiệu quả vì định nghĩa có thể được kiểm tra trực tiếp trong thời gian không đổi cho mỗi cặp. Vấn đề là quy mô. Với`10^5`khoảng thời gian, số lượng so sánh trở thành:```
10^5 × 10^5 = 10^10
```Như vậy là quá chậm. 

Quan sát quan trọng là việc ngăn chặn trở nên dễ dàng hơn nhiều sau khi phân loại theo năm bắt đầu. 

Giả sử chúng ta sắp xếp các khoảng bằng cách tăng`a`. Khi xử lý một khoảng`[a_i, b_i]`, mỗi khoảng thời gian trước đó theo thứ tự đã sắp xếp đã có năm bắt đầu nhỏ hơn. Điều kiện ngăn chặn đầu tiên được tự động thỏa mãn. Chúng ta chỉ cần biết liệu có khoảng thời gian nào sớm hơn có năm kết thúc lớn hơn hay không. 

Điều đó biến vấn đề thành một truy vấn tối đa tiền tố. 

Khi chúng tôi quét từ trái sang phải, chúng tôi duy trì mức tối đa`b`thấy cho đến nay. Nếu khoảng thời gian hiện tại có:```
b_i < max_b
```thì một số khoảng thời gian trước đó kết thúc muộn hơn và bởi vì nó cũng bắt đầu sớm hơn do sắp xếp nên khoảng thời gian hiện tại sẽ được chứa. 

Điều này làm giảm toàn bộ vấn đề xuống còn một loại cộng với một lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các khoảng vào một danh sách. 

Chúng ta cần tất cả các khoảng cùng nhau vì giải pháp phụ thuộc vào việc sắp xếp chúng trên toàn cầu. 
2. Sắp xếp các khoảng thời gian theo năm bắt đầu theo thứ tự tăng dần. 

Sau khi sắp xếp, mỗi khoảng thời gian trước đó sẽ tự động thỏa mãn điều kiện`a_j < a_i`. 
3. Khởi tạo một biến`max_b`với giá trị rất nhỏ. 

Điều này lưu trữ năm kết thúc lớn nhất trong số tất cả các khoảng thời gian được xử lý trước đó. 
4. Quét các khoảng thời gian được sắp xếp từ trái sang phải. 

Đối với mỗi khoảng thời gian`[a, b]`, kiểm tra xem`b < max_b`. 
5. Nếu`b < max_b`, tăng câu trả lời. 

Một số khoảng thời gian trước đó có năm kết thúc lớn hơn và vì các khoảng thời gian trước đó cũng bắt đầu sớm hơn nên khoảng thời gian hiện tại được bao gồm. 
6. Cập nhật`max_b = max(max_b, b)`. 

Khoảng thời gian tương lai có thể được chứa bên trong khoảng thời gian hiện tại nếu nó có năm kết thúc lớn nhất cho đến nay. 
7. In số đếm cuối cùng. 

### Tại sao nó hoạt động 

Sau khi sắp xếp theo năm bắt đầu, mỗi khoảng thời gian được xử lý trước khoảng thời gian hiện tại có thời điểm bắt đầu nhỏ hơn rất nhiều. Yêu cầu duy nhất còn lại để ngăn chặn là tìm ra khoảng thời gian sớm hơn có năm kết thúc lớn hơn. 

Biến`max_b`luôn lưu trữ năm kết thúc lớn nhất trong số tất cả các khoảng thời gian trước đó. Nếu khoảng thời gian hiện tại kết thúc trước`max_b`, thì tồn tại một khoảng thời gian trước đó bắt đầu sớm hơn và kết thúc muộn hơn, điều đó có nghĩa là khoảng thời gian hiện tại được chứa. 

Nếu như`b >= max_b`, thì không có khoảng thời gian nào trước đó kết thúc muộn hơn khoảng thời gian hiện tại, do đó việc ngăn chặn là không thể. 

Vì mỗi khoảng được xử lý chính xác một lần theo bất biến này nên thuật toán sẽ tính chính xác các khoảng chứa trong một khoảng khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    intervals = []
    
    for _ in range(n):
        a, b = map(int, input().split())
        intervals.append((a, b))
    
    intervals.sort()
    
    max_b = -1
    ans = 0
    
    for a, b in intervals:
        if b < max_b:
            ans += 1
        
        max_b = max(max_b, b)
    
    print(ans)

solve()
```Giải pháp bắt đầu bằng cách đọc tất cả các khoảng thời gian và sắp xếp chúng theo năm bắt đầu. Python sắp xếp các bộ dữ liệu theo từ điển, vì vậy`intervals.sort()`tự nhiên sắp xếp theo`a`. 

Biến`max_b`theo dõi năm kết thúc lớn nhất trong số các khoảng thời gian đã được xử lý. Ban đầu nó được đặt thành`-1`, nhỏ hơn một cách an toàn so với bất kỳ năm kết thúc hợp lệ nào. 

Trong quá trình quét, tình trạng:```
if b < max_b:
```phát hiện sự ngăn chặn. Vì các khoảng thời gian được xử lý theo thứ tự bắt đầu tăng dần nên bất kỳ khoảng thời gian nào góp phần vào`max_b`đã bắt đầu sớm hơn. Khoảng thời gian hiện tại được chứa chính xác khi năm kết thúc của nó nhỏ hơn. 

Bản cập nhật:```
max_b = max(max_b, b)
```phải xảy ra sau khi kiểm tra ngăn chặn. Việc cập nhật trước tiên sẽ khiến mọi khoảng thời gian so sánh với chính nó, điều này sẽ không chính xác. 

Số nguyên Python xử lý phạm vi điểm cuối lên tới`10^9`một cách an toàn mà không có mối lo ngại tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 10
2 9
3 8
4 7
5 6
```Khoảng thời gian được sắp xếp vẫn giữ nguyên. 

| Khoảng thời gian hiện tại | max_b Trước | Chứa? | Trả lời | max_b Sau | 
| --- | --- | --- | --- | --- | 
| (1, 10) | -1 | Không | 0 | 10 | 
| (2, 9) | 10 | Có | 1 | 10 | 
| (3, 8) | 10 | Có | 2 | 10 | 
| (4, 7) | 10 | Có | 3 | 10 | 
| (5, 6) | 10 | Có | 4 | 10 | 

Câu trả lời cuối cùng:```
4
```Dấu vết này cho thấy rõ ràng bất biến trung tâm. Một khi khoảng thời gian`(1,10)`thiết lập`max_b = 10`, mỗi khoảng thời gian sau kết thúc trước đó sẽ tự động được chứa. 

### Ví dụ 2 

đầu vào:```
4
1 4
2 10
3 5
6 8
```Các khoảng được sắp xếp:```
(1,4), (2,10), (3,5), (6,8)
```| Khoảng thời gian hiện tại | max_b Trước | Chứa? | Trả lời | max_b Sau | 
| --- | --- | --- | --- | --- | 
| (1, 4) | -1 | Không | 0 | 4 | 
| (2, 10) | 4 | Không | 0 | 10 | 
| (3, 5) | 10 | Có | 1 | 10 | 
| (6, 8) | 10 | Có | 2 | 10 | 

Câu trả lời cuối cùng:```
2
```Ví dụ này chứng minh rằng việc ngăn chặn không yêu cầu các chuỗi lồng nhau. Một khoảng lớn có thể chứa nhiều khoảng không liên quan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp thống trị thời gian chạy | 
| Không gian | O(n) | Danh sách khoảng thời gian lưu trữ tất cả các sự kiện | 

Với`n = 10^5`, MỘT`O(n log n)`giải pháp dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ cũng nhỏ vì chỉ lưu trữ mảng khoảng thời gian. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    
    n = int(input())
    
    intervals = []
    
    for _ in range(n):
        a, b = map(int, input().split())
        intervals.append((a, b))
    
    intervals.sort()
    
    max_b = -1
    ans = 0
    
    for a, b in intervals:
        if b < max_b:
            ans += 1
        
        max_b = max(max_b, b)
    
    print(ans)

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
"""5
1 10
2 9
3 8
4 7
5 6
"""
) == "4", "sample 1"

# minimum size
assert run(
"""1
1 2
"""
) == "0", "single interval"

# no interval contained
assert run(
"""4
1 2
3 4
5 6
7 8
"""
) == "0", "disjoint intervals"

# one large interval contains all others
assert run(
"""5
1 100
2 10
20 30
40 50
60 70
"""
) == "4", "one container"

# mixed ordering
assert run(
"""4
6 8
1 4
3 5
2 10
"""
) == "2", "unsorted input"

# nested chain
assert run(
"""4
1 8
2 7
3 6
4 5
"""
) == "3", "deep nesting"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Quãng đơn | 0 | Kích thước đầu vào tối thiểu | 
| Khoảng rời rạc | 0 | Không có kết quả dương tính giả | 
| Một khoảng lớn | 4 | Nhiều khoảng thời gian chứa trong một sự kiện | 
| Đặt hàng hỗn hợp | 2 | Sắp xếp là cần thiết | 
| Chuỗi lồng nhau | 3 | Phát hiện ngăn chặn lặp đi lặp lại | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
1 2
```Sau khi sắp xếp, chỉ còn một khoảng.`max_b`bắt đầu lúc`-1`, Vì thế`2 < -1`là sai. Câu trả lời vẫn còn`0`, điều này đúng vì không tồn tại khoảng nào khác. 

Bây giờ hãy xem xét các khoảng hoàn toàn rời rạc:```
4
1 2
3 4
5 6
7 8
```Quá trình quét diễn ra như sau: 

-`(1,2)`bộ`max_b = 2`-`(3,4)`có`4 < 2`, SAI
-`(5,6)`có`6 < 4`, SAI
-`(7,8)`có`8 < 6`, sai 

Không có khoảng thời gian được tính. Mỗi khoảng thời gian kết thúc muộn hơn tất cả các khoảng thời gian trước đó, do đó việc ngăn chặn không bao giờ xảy ra. 

Cuối cùng, hãy xem xét trường hợp một khoảng chứa nhiều khoảng không liên quan:```
5
1 100
2 10
20 30
40 50
60 70
```Thứ tự sắp xếp vẫn giữ nguyên. Sau khi xử lý`(1,100)`,`max_b = 100`. Mỗi khoảng thời gian sau đó có năm kết thúc nhỏ hơn nên cả bốn đều được tính. 

Điều này xác nhận tính bất biến đằng sau thuật toán. Khi một khoảng thời gian có năm kết thúc rất lớn xuất hiện, mọi khoảng thời gian muộn hơn kết thúc sớm hơn đều được đảm bảo chứa đầy đủ vì thứ tự bắt đầu đã được xử lý bằng cách sắp xếp.
