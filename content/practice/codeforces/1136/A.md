---
title: "CF 1136A - Nastya đang đọc sách"
description: "Cuốn sách được chia thành các chương liên tiếp. Mỗi chương chiếm một phạm vi trang liên tục và mỗi trang thuộc về chính xác một chương. Nastya đã đọc từ trang 1 đến trang k - 1. Trang k là trang đầu tiên cô chưa đọc."
date: "2026-06-12T04:00:10+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1136
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 546 (Div. 2)"
rating: 800
weight: 1136
solve_time_s: 78
verified: true
draft: false
---

[CF 1136A - Nastya đang đọc sách](https://codeforces.com/problemset/problem/1136/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cuốn sách được chia thành các chương liên tiếp. Mỗi chương chiếm một phạm vi trang liên tục và mỗi trang thuộc về chính xác một chương. 

Nastya đã đọc trang`1`bởi vì`k - 1`. Trang`k`là trang đầu tiên cô chưa đọc. Chúng ta cần xác định có bao nhiêu chương chưa hoàn thành. 

Một chương được coi là chưa hoàn thành trong hai trường hợp. Hoặc Nastya chưa bắt đầu nó chút nào, hoặc cô ấy đã dừng lại ở đâu đó bên trong nó và không đọc đến trang cuối cùng. 

Các phạm vi chương được sắp xếp theo thứ tự và không trùng lặp. Vì các chương bao trùm cuốn sách một cách liên tiếp nên khi chúng ta tìm thấy chương có chứa trang`k`, mọi chương từ thời điểm đó trở đi đều chưa hoàn thành. Bất kỳ chương nào trước thời điểm đó đều đã được đọc đầy đủ. 

Những hạn chế là rất nhỏ. Có nhiều nhất là 100 chương nên ngay cả việc quét qua tất cả các chương cũng dễ dàng đủ nhanh. Chúng tôi không cần bất kỳ cấu trúc dữ liệu hoặc kỹ thuật tìm kiếm nâng cao nào. 

Một trường hợp tế nhị xảy ra khi`k`chính xác là trang đầu tiên của một chương. 

Ví dụ:```
3
1 3
4 7
8 10
4
```Trang`4`thuộc về chương thứ hai. Chương đầu tiên đã đọc xong, nhưng chương thứ hai vẫn chưa được bắt đầu. Câu trả lời là`2`, không`1`. 

Một trường hợp dễ bỏ sót khác là khi`k`là trang cuối cùng của một chương.```
3
1 3
4 7
8 10
7
```Chương thứ hai vẫn chưa hoàn thành vì trang`7`vẫn chưa được đọc. Câu trả lời là`2`. 

Một giải pháp bất cẩn có thể cho rằng đến trang cuối cùng có nghĩa là chương đó đã hoàn thành, nhưng trang`k`bản thân nó chưa được đọc. 

Đầu vào nhỏ nhất có thể cũng đáng được chú ý.```
1
1 1
1
```Chương duy nhất chứa trang`1`, chưa được đọc. Câu trả lời đúng là`1`. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là mô phỏng việc đọc từng trang. Chúng ta có thể xác định những trang nào đã được đọc, sau đó kiểm tra từng chương và kiểm tra xem tất cả các trang trong chương đó đã được đọc hay chưa. Vì mỗi chương có nhiều nhất là 100 trang và có nhiều nhất là 100 chương nên việc này yêu cầu tối đa khoảng 10.000 lần kiểm tra trang. Thậm chí điều đó sẽ được chấp nhận dưới những ràng buộc này. 

Cấu trúc của vấn đề cho phép một cái gì đó đơn giản hơn nhiều. 

Một chương được đọc hết nếu trang cuối cùng của nó nhỏ hơn trang cuối cùng`k`. Thời điểm chúng ta gặp một chương có phạm vi chứa trang`k`, hoặc trang kết thúc của nó ít nhất là`k`, chương đó chưa hoàn thành. Mỗi chương sau cũng chưa hoàn thành vì Nastya chưa bao giờ đọc xong. 

Điều này có nghĩa là chúng ta chỉ cần tìm chương đầu tiên có khoảng chứa trang`k`. Nếu chương đó có chỉ mục`i`(sử dụng chỉ mục dựa trên số 0), sau đó tất cả các chương từ`i`bởi vì`n - 1`vẫn chưa hoàn thành, đưa ra câu trả lời`n - i`. 

Một lần quét tuyến tính là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · trang_per_chapter) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số chương`n`. 
2. Lưu trữ tất cả các khoảng chương`(l, r)`. 
3. Đọc số trang`k`, đây là trang chưa đọc đầu tiên. 
4. Quét các chương từ trái sang phải. 
5. Đối với mỗi chương, hãy kiểm tra xem`k`nằm trong phạm vi trang của nó, nghĩa là`l ≤ k ≤ r`. 
6. Ngay khi tìm thấy một chương như vậy, hãy in ra số chương còn lại, đó là`n - current_index`. 
7. Hãy dừng lại ngay vì tất cả các chương sau cũng chưa hoàn thành. 

### Tại sao nó hoạt động 

Các chương tạo thành các phạm vi trang liên tiếp, không chồng chéo. Kể từ trang`k`là trang chưa đọc đầu tiên, mọi trang trước đó`k`đã được đọc rồi. 

Bất kỳ chương nào kết thúc trước`k`đã hoàn tất. Chương duy nhất chứa`k`chưa hoàn thành vì trang`k`bản thân nó chưa được đọc. Mỗi chương sau chương đó cũng chưa hoàn thành vì Nastya chưa bao giờ đọc đến những trang đó. 

Như vậy câu trả lời chính xác là số chương bắt đầu từ chương chứa`k`đến cuối cuốn sách. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

chapters = []
for _ in range(n):
    l, r = map(int, input().split())
    chapters.append((l, r))

k = int(input())

for i, (l, r) in enumerate(chapters):
    if l <= k <= r:
        print(n - i)
        break
```Phần đầu tiên đọc và lưu trữ các phạm vi chương. 

Sau khi đọc`k`, giải pháp thực hiện quét tuyến tính. Vì các khoảng liên tiếp và không chồng chéo nên có đúng một chương chứa trang`k`. 

Khi chương đó được tìm thấy ở vị trí`i`, câu trả lời là`n - i`. Điều này tính chương hiện tại cộng với tất cả các chương sau đó. 

Lỗi triển khai phổ biến nhất là lỗi từng lỗi một trên trang`k`. Hãy nhớ trang đó`k`chưa** được đọc. Một chương có chứa`k`vẫn chưa hoàn thành, ngay cả khi`k`tình cờ là trang cuối cùng của chương. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 3
4 7
8 11
2
```| Mục lục chương | Phạm vi | Nó có chứa k=2 không? | Trả lời nếu tìm thấy | 
| --- | --- | --- | --- | 
| 0 | [1, 3] | Có | 3 - 0 = 3 | 

Đầu ra:```
3
```Trang`2`nằm bên trong chương đầu tiên. Vì Nastya đã dừng ở chương đó nên mỗi chương vẫn chưa hoàn thành. 

### Mẫu 2 

đầu vào:```
3
1 3
4 8
9 12
6
```| Mục lục chương | Phạm vi | Nó có chứa k=6 không? | Trả lời nếu tìm thấy | 
| --- | --- | --- | --- | 
| 0 | [1, 3] | Không | - | 
| 1 | [4, 8] | Có | 3 - 1 = 2 | 

Đầu ra:```
2
```Chương đầu tiên kết thúc trước trang`6`, vì vậy nó được đọc đầy đủ. Chương thứ hai có trang`6`, khiến nó chưa hoàn thành và chương thứ ba vẫn chưa được bắt đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần quét qua danh sách chương | 
| Không gian | O(1) | Ngoài việc lưu trữ các khoảng đầu vào | 

Với tối đa 100 chương, quá trình quét tuyến tính thực hiện tối đa 100 lần kiểm tra. Giải pháp dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())

    chapters = []
    for _ in range(n):
        l, r = map(int, input().split())
        chapters.append((l, r))

    k = int(input())

    for i, (l, r) in enumerate(chapters):
        if l <= k <= r:
            return str(n - i) + "\n"

# provided sample
assert run(
"""3
1 3
4 7
8 11
2
"""
) == "3\n", "sample 1"

# minimum size
assert run(
"""1
1 1
1
"""
) == "1\n", "single chapter"

# k at first page of second chapter
assert run(
"""3
1 3
4 7
8 10
4
"""
) == "2\n", "start of chapter"

# k at last page of second chapter
assert run(
"""3
1 3
4 7
8 10
7
"""
) == "2\n", "end page still unread"

# k in final chapter
assert run(
"""4
1 2
3 4
5 6
7 8
8
"""
) == "1\n", "only last chapter remains"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một chương, trang 1 chưa đọc | 1 | Đầu vào kích thước tối thiểu | 
| k = trang đầu tiên của chương 2 | 2 | Chương chưa bắt đầu | 
| k = trang cuối chương 2 | 2 | Trang ranh giới chưa đọc | 
| k ở chương cuối | 1 | Chỉ còn một chương còn dang dở | 

## Vỏ cạnh 

Hãy xem xét trường hợp`k`chính xác là trang đầu tiên của một chương.```
3
1 3
4 7
8 10
4
```Quá trình quét kiểm tra`[1,3]`và tiếp tục. Khoảng thời gian tiếp theo`[4,7]`chứa`4`, vậy câu trả lời là`3 - 1 = 2`. Chương thứ hai còn chưa bắt đầu nên cả chương thứ hai và thứ ba đều chưa hoàn thành. 

Bây giờ hãy xem xét`k`là trang cuối cùng của một chương.```
3
1 3
4 7
8 10
7
```chương`[4,7]`chứa trang`7`. Kể từ trang`7`bản thân nó chưa được đọc, chương này vẫn chưa hoàn thành. Thuật toán trả về chính xác`2`. 

Cuối cùng, hãy xem xét cuốn sách nhỏ nhất có thể.```
1
1 1
1
```Chương duy nhất chứa trang`1`, đây là trang chưa đọc đầu tiên. Quá trình quét tìm thấy nó ngay lập tức và trả về`1`. Chương này không thể được coi là đã hoàn thành vì chưa có trang nào được đọc từ đó.
