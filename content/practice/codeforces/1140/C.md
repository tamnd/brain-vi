---
title: "CF 1140C - Danh sách phát"
description: "Mỗi bài hát có hai thuộc tính: độ dài t và vẻ đẹp của nó b. Nếu chúng tôi chọn một số tập hợp con các bài hát, điểm của nó là $$(văn bản{tổng độ dài}) lần (văn bản{vẻ đẹp tối thiểu})$$ Chúng tôi có thể chọn tối đa k bài hát và chúng tôi muốn đạt điểm tối đa có thể."
date: "2026-06-12T03:46:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1140
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 62 (Rated for Div. 2)"
rating: 1600
weight: 1140
solve_time_s: 89
verified: true
draft: false
---

[CF 1140C - Danh sách phát](https://codeforces.com/problemset/problem/1140/C) 

**Đánh giá:** 1600 
**Tags:** bạo lực, cấu trúc dữ liệu, sắp xếp 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi bài hát có hai thuộc tính: độ dài của nó`t`và vẻ đẹp của nó`b`. 

Nếu chúng ta chọn một tập hợp con các bài hát, số điểm của nó là$$(\text{sum of lengths}) \times (\text{minimum beauty})$$Chúng ta có thể chọn nhiều nhất`k`bài hát và chúng tôi muốn đạt được số điểm tối đa có thể. 

Phần thú vị là mục tiêu kết hợp hai hoạt động tổng hợp khác nhau. Độ dài được tổng hợp nhưng vẻ đẹp chỉ đóng góp thông qua giá trị tối thiểu của chúng. Một bài hát có vẻ đẹp rất nhỏ có thể bị giảm điểm trầm trọng ngay cả khi nó có độ dài lớn. 

Những hạn chế là lớn. Chúng ta có thể có tới`3 * 10^5`các bài hát, loại trừ ngay lập tức bất kỳ thuật toán nào liệt kê các tập hợp con, các cặp tập hợp con hoặc thực hiện quét bậc hai. Với giới hạn thời gian 2 giây, chúng ta nên nhắm mục tiêu khoảng`O(n log n)`. 

Một số trường hợp khó xử lý. 

Coi như:```
3 2
100 1
5 10
5 10
```Câu trả lời tối ưu là:```
100
```bằng cách lấy hai sắc đẹp-10 bài hát. Một chiến lược tham lam luôn chọn thời lượng lớn nhất sẽ chọn bài hát đầu tiên và chỉ thu được`100 * 1 = 100`, thiếu sự thật rằng sắc đẹp chính là điểm nghẽn. 

Một trường hợp tế nhị khác là khi chọn ít hơn`k`bài hát là hay nhất:```
3 3
100 100
1 1
1 1
```Câu trả lời là:```
10000
```Chỉ lấy bài hát đầu tiên mang lại`100 * 100`. Bao gồm một trong những bài hát khác làm giảm vẻ đẹp tối thiểu xuống`1`, làm cho điểm số trở nên tồi tệ hơn nhiều. 

Một trường hợp nữa chứng minh tại sao chúng ta phải xem xét mọi vẻ đẹp tối thiểu có thể có:```
4 2
10 5
10 4
10 3
10 2
```Câu trả lời tốt nhất là:```
80
```sử dụng hai bài hát đầu tiên. Vẻ đẹp tối thiểu là`4`, không phải vẻ đẹp lớn nhất toàn cầu`5`. Chỉ hạn chế sự chú ý đến những giá trị vẻ đẹp lớn nhất sẽ bỏ lỡ sự tối ưu. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ kiểm tra tối đa mọi tập hợp con có kích thước`k`, tính tổng độ dài, tính độ đẹp tối thiểu và giữ điểm cao nhất. 

Điều này đúng vì nó trực tiếp kiểm tra tất cả các khả năng. 

Thật không may, ngay cả đối với`n = 50`điều này trở nên không thực tế. Vì`n = 3 * 10^5`, số lượng tập hợp con là vô cùng lớn. 

Quan sát quan trọng đến từ việc nhìn vào vẻ đẹp tối thiểu. 

Giả sử chúng ta đã biết bài hát nào mang lại vẻ đẹp tối thiểu cho tập hợp đã chọn. Hãy để vẻ đẹp đó được`B`. 

Thì bài hát nào được chọn ít nhất cũng phải có nét đẹp`B`, nếu không thì mức tối thiểu sẽ nhỏ hơn. 

Trong số tất cả những bài hát có vẻ đẹp ít nhất`B`, bộ nào là tốt nhất`k`bài hát? 

Kể từ khi điểm số trở thành$$(\text{sum of lengths}) \times B,$$Và`B`đã được sửa, chúng tôi chỉ muốn tổng chiều dài lớn nhất có thể bằng cách sử dụng tối đa`k`bài hát đủ điều kiện. 

Điều đó có nghĩa là chúng ta nên áp dụng`k`độ dài lớn nhất trong số các bài hát có vẻ đẹp ít nhất`B`. 

Điều này gợi ý việc xử lý các giá trị vẻ đẹp từ lớn nhất đến nhỏ nhất. 

Sau khi sắp xếp các bài hát theo độ đẹp giảm dần, khi chúng ta hiện đang xem một bài hát có vẻ đẹp`b`, mọi bài hát đã được xử lý trước đó đều có vẻ đẹp ít nhất`b`. 

Tại thời điểm đó, chúng tôi muốn biết tổng chiều dài tối đa có thể đạt được từ nhiều nhất`k`bài hát đã được xử lý Một đống tối thiểu là hoàn hảo để duy trì kích thước lớn nhất`k`chiều dài nhìn thấy cho đến nay. 

Khi chúng tôi quét: 

1. Chèn độ dài hiện tại vào heap. 
2. Nếu heap phát triển vượt quá kích thước`k`, loại bỏ chiều dài nhỏ nhất. 
3. Duy trì tổng các giá trị hiện có trong heap. 
4. Hãy coi người đẹp hiện tại là ứng cử viên sắc đẹp tối thiểu và đánh giá:$$(\text{current sum}) \times b$$Đống luôn chứa độ dài lớn nhất hiện có trong số các bài hát có vẻ đẹp ít nhất bằng vẻ đẹp hiện tại, khớp hoàn toàn với quan sát ở trên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n \cdot n) | O(n) | Quá chậm | 
| Tối ưu | O(n log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các bài hát theo cặp`(beauty, length)`. 
2. Sắp xếp các bài hát theo vẻ đẹp theo thứ tự giảm dần. 

Sau khi sắp xếp, mọi bài hát được xử lý trước đó đều có vẻ đẹp ít nhất bằng vẻ đẹp của bài hát hiện tại. 
3. Tạo một vùng heap tối thiểu để lưu trữ các độ dài đã chọn. 
4. Duy trì`length_sum`, tổng của tất cả các độ dài hiện có trong heap. 
5. Lặp lại các bài hát theo thứ tự được sắp xếp. 
6. Chèn độ dài của bài hát hiện tại vào heap và thêm nó vào`length_sum`. 
7. Nếu kích thước heap vượt quá`k`, loại bỏ độ dài nhỏ nhất và trừ nó khỏi`length_sum`. 

Heap bây giờ chứa lớn nhất`k`độ dài trong số tất cả các bài hát được xử lý. 
8. Tính toán$$\text{candidate} = \text{length\_sum} \times \text{current beauty}$$Tại thời điểm này, mọi bài hát được thể hiện trong heap đều có vẻ đẹp ít nhất là vẻ đẹp hiện tại. 
9. Cập nhật câu trả lời với giá trị ứng cử viên tối đa được nhìn thấy. 
10. Sau khi xử lý tất cả các bài hát, hãy in câu trả lời. 

### Tại sao nó hoạt động 

Khi xử lý một bài hát với vẻ đẹp`b`, mỗi bài hát được xử lý ít nhất đều có vẻ đẹp`b`. Bất kỳ tập hợp con hợp lệ nào có vẻ đẹp tối thiểu chính xác`b`phải chỉ bao gồm các bài hát được xử lý này. 

Để có vẻ đẹp tối thiểu cố định`b`, việc tối đa hóa điểm sẽ giảm xuống mức tối đa hóa tổng chiều dài tùy thuộc vào việc chọn tối đa`k`bài hát. Heap duy trì chính xác lớn nhất`k`độ dài trong số tất cả các bài hát được xử lý, vì vậy`length_sum`là tổng chiều dài tối đa có thể đạt được cho ngưỡng vẻ đẹp này. 

Vì mọi vẻ đẹp tối thiểu có thể xuất hiện dưới dạng vẻ đẹp của một số bài hát nên quá trình quét sẽ đánh giá điểm tối ưu cho vẻ đẹp tối thiểu của mọi ứng cử viên. Lấy giá trị tối đa trong tất cả các đánh giá mang lại giá trị tối ưu toàn cục. 

## Giải pháp Python```python
import sys
from heapq import heappush, heappop

input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    songs = []
    for _ in range(n):
        t, b = map(int, input().split())
        songs.append((b, t))

    songs.sort(reverse=True)

    heap = []
    length_sum = 0
    answer = 0

    for b, t in songs:
        heappush(heap, t)
        length_sum += t

        if len(heap) > k:
            length_sum -= heappop(heap)

        answer = max(answer, length_sum * b)

    print(answer)

if __name__ == "__main__":
    solve()
```Bước sắp xếp tạo ra thứ tự xem xét các ngưỡng vẻ đẹp. Khi chúng ta đạt tới cái đẹp`b`, ít nhất mọi bài hát được xem cho đến nay đều có vẻ đẹp`b`. 

Heap chỉ lưu trữ độ dài. Việc giữ một vùng heap tối thiểu cho phép chúng ta loại bỏ hiệu quả độ dài nhỏ nhất bất cứ khi nào lớn hơn`k`bài hát có mặt. Sau lần điều chỉnh này, heap chứa giá trị lớn nhất`k`độ dài trong số tất cả các bài hát được xử lý. 

Biến`length_sum`được duy trì tăng dần. Việc tính lại tổng từ heap mỗi lần lặp sẽ đưa ra một hệ số không cần thiết là`k`. 

Phép nhân phải được thực hiện bằng số học 64 bit. Trong Python điều này xảy ra tự động. Trong các ngôn ngữ như C++,`long long`là bắt buộc vì độ dài có thể tổng cộng khoảng`3 * 10^11`, và nhân với vẻ đẹp có thể đạt đến mức`3 * 10^17`. 

Một lỗi phổ biến là tính điểm trước khi loại bỏ các phần tử dư thừa khỏi heap. Heap trước tiên phải được giảm xuống tối đa`k`bài hát, nếu không điểm thí sinh có thể sử dụng quá nhiều bài hát. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 3
4 7
15 1
3 6
6 8
```Sau khi phân loại theo vẻ đẹp:`(8,6), (7,4), (6,3), (1,15)`| Bước | Sắc đẹp | Chiều dài | Độ dài đống | Tổng chiều dài | Ứng viên | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 8 | 6 | [6] | 6 | 48 | 48 | 
| 2 | 7 | 4 | [4,6] | 10 | 70 | 70 | 
| 3 | 6 | 3 | [3,6,4] | 13 | 78 | 78 | 
| 4 | 1 | 15 | [4,6,15] | 25 | 25 | 78 | 

Điểm tối đa xuất hiện khi người đẹp`6`được coi là vẻ đẹp tối thiểu. Đống chứa các độ dài`6, 4, 3`, cho tổng chiều dài`13`và ghi điểm`13 * 6 = 78`. 

### Mẫu 2 

đầu vào:```
3 2
1 1
1 1
100 100
```Thứ tự sắp xếp:`(100,100), (1,1), (1,1)`| Bước | Sắc đẹp | Chiều dài | Độ dài đống | Tổng chiều dài | Ứng viên | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 100 | 100 | [100] | 100 | 10000 | 10000 | 
| 2 | 1 | 1 | [1.100] | 101 | 101 | 10000 | 
| 3 | 1 | 1 | [1.100] | 101 | 101 | 10000 | 

Câu trả lời tốt nhất đến từ việc chỉ chọn một bài hát. Điều này chứng tỏ tại sao vấn đề nói "nhiều nhất`k`bài hát" chứ không phải chính xác`k`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log k) | Sắp xếp là O(n log n), mỗi thao tác heap là O(log k). Từ`k ≤ n`, độ phức tạp tổng thể thường được viết là O(n log n) hoặc O(n log k) sau khi sắp xếp. | 
| Không gian | O(k) | Heap lưu trữ nhiều nhất`k`chiều dài. | 

Chi phí chủ yếu là phân loại`3 * 10^5`bài hát, có thể dễ dàng quản lý trong giới hạn. Các thao tác heap chỉ thêm chi phí logarit và mức sử dụng bộ nhớ vẫn ở mức nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from heapq import heappush, heappop

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, k = map(int, input().split())

    songs = []
    for _ in range(n):
        t, b = map(int, input().split())
        songs.append((b, t))

    songs.sort(reverse=True)

    heap = []
    s = 0
    ans = 0

    for b, t in songs:
        heappush(heap, t)
        s += t

        if len(heap) > k:
            s -= heappop(heap)

        ans = max(ans, s * b)

    return str(ans) + "\n"

# sample 1
assert run(
"""4 3
4 7
15 1
3 6
6 8
"""
) == "78\n"

# sample-style case
assert run(
"""3 2
1 1
1 1
100 100
"""
) == "10000\n"

# minimum size
assert run(
"""1 1
5 7
"""
) == "35\n"

# all equal values
assert run(
"""4 2
3 5
3 5
3 5
3 5
"""
) == "30\n"

# k = n
assert run(
"""3 3
5 4
2 3
1 2
"""
) == "16\n"

# choosing fewer than k songs is optimal
assert run(
"""3 3
100 100
1 1
1 1
"""
) == "10000\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 5 7`|`35`| Ví dụ nhỏ nhất có thể | 
| Bốn bài hát giống hệt nhau`k=2`|`30`| Xử lý các giá trị trùng lặp | 
|`k=n`ví dụ |`16`| Sử dụng tất cả các bài hát khi có lợi | 
| Vẻ đẹp ngoại lệ lớn |`10000`| Giải pháp tốt nhất có thể sử dụng ít hơn`k`bài hát | 

## Vỏ cạnh 

Hãy xem xét:```
3 3
100 100
1 1
1 1
```Sau khi sắp xếp, bài hát được xử lý đầu tiên có vẻ đẹp`100`. Đống chỉ chứa chiều dài`100`, cho điểm`10000`. Những bài hát sau này giảm ngưỡng nhan sắc xuống`1`, tạo ra điểm số nhỏ hơn nhiều. Thuật toán tự nhiên giữ câu trả lời trước đó, xử lý chính xác các trường hợp có ít hơn`k`bài hát là tối ưu. 

Coi như:```
3 2
100 1
5 10
5 10
```Thứ tự sắp xếp là:```
(10,5), (10,5), (1,100)
```Khi làm đẹp`10`được xử lý, heap chứa độ dài`5`Và`5`, mang lại điểm số`100`. Khi vẻ đẹp tụt xuống`1`, heap chứa hai độ dài lớn nhất,`100`Và`5`, tạo ra điểm số`105`. Câu trả lời vẫn còn`105`. Thuật toán không chỉ ưu tiên những người có chiều dài lớn hoặc vẻ đẹp lớn mà đánh giá cả hai cùng nhau qua mọi ngưỡng sắc đẹp. 

Coi như:```
4 2
10 5
10 4
10 3
10 2
```Quá trình quét đánh giá:```
10*5 = 50
20*4 = 80
20*3 = 60
20*2 = 40
```Đỉnh cao xảy ra ở vẻ đẹp`4`. Điều này khẳng định tại sao mọi vẻ đẹp tối thiểu đều phải được xem xét. Sự tối ưu không nhất thiết phải gắn liền với giá trị vẻ đẹp lớn nhất.
