---
title: "CF 302B - Danh sách Eugeny và Play"
description: "Danh sách phát được xây dựng từ các khối bài hát lặp đi lặp lại. Bài hát i có thời lượng t[i] và Eugeny nghe nó liên tục c[i] lần trước khi chuyển sang bài hát tiếp theo. Nếu một bài hát kéo dài 4 phút và được lặp lại 3 lần thì khối đó sẽ đóng góp 12 phút vào dòng thời gian của danh sách phát."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 302
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 182 (Div. 2)"
rating: 1200
weight: 302
solve_time_s: 205
verified: true
draft: false
---

[CF 302B - Danh sách Eugeny và Play](https://codeforces.com/problemset/problem/302/B) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, triển khai, hai con trỏ 
**Thời gian giải:** 3 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Danh sách phát được xây dựng từ các khối bài hát lặp đi lặp lại. bài hát`i`có thời lượng`t[i]`, và Eugeny lắng nghe nó`c[i]`lần liên tiếp trước khi chuyển sang bài hát tiếp theo. 

Nếu một bài hát kéo dài 4 phút và được lặp lại 3 lần thì khối đó sẽ đóng góp 12 phút vào dòng thời gian của danh sách phát. Truy vấn đặt những câu hỏi như: 

“Ở phút thứ 57 kể từ khi bắt đầu danh sách phát, bài hát nào được phát?” 

Chi tiết quan trọng là chúng tôi không được hỏi bài hát đang được phát lặp lại ở đâu mà chỉ được hỏi chỉ mục bài hát. 

Danh sách phát có thể chứa tối đa`10^5`khối bài hát và`10^5`truy vấn. Việc mô phỏng trực tiếp từng phút là không thể bởi vì mỗi`c[i] * t[i]`có thể rất lớn. Tổng thời lượng danh sách phát có thể đạt tới`10^9`, có nghĩa là việc xây dựng một mảng rõ ràng gồm tất cả số phút sẽ cần hàng tỷ phần tử. 

Với`10^5`truy vấn dưới giới hạn 2 giây, chúng ta sẽ mong đợi điều gì đó xung quanh`O(n log n)`hoặc`O(n + m)`vượt qua một cách thoải mái. Bất kỳ giải pháp nào quét toàn bộ danh sách phát cho mọi truy vấn sẽ trở nên quá chậm. 

Một điểm tinh tế là các truy vấn được lập chỉ mục 1 về mặt thời gian. Nếu một bài hát chiếm vài phút`[1, 16]`, sau đó truy vấn`16`vẫn thuộc về bài hát đó, trong khi truy vấn`17`thuộc về cái tiếp theo. Những sai lầm ngẫu nhiên rất phổ biến ở đây. 

Hãy xem xét ví dụ này:```
2 3
1 3
1 2
1 3 4
```Dòng thời gian của danh sách phát là:```
minutes 1..3 -> song 1
minutes 4..5 -> song 2
```Đầu ra đúng:```
1
1
2
```Việc thực hiện bất cẩn bằng cách sử dụng nghiêm ngặt`<`thay vì`<=`trong quá trình tìm kiếm nhị phân sẽ ánh xạ phút không chính xác`3`hát`2`. 

Một trường hợp phức tạp khác xảy ra khi một khối bài hát cực kỳ lớn:```
1 3
1000000000 1
1 500000000 1000000000
```Tất cả các truy vấn vẫn phải trả lại bài hát`1`. Điều này loại trừ mọi cách tiếp cận mở rộng danh sách phát từng phút. 

Chi tiết cuối cùng là các truy vấn đã được sắp xếp ngày càng nhiều. Một số giải pháp khai thác điều này bằng hai con trỏ, nhưng tìm kiếm nhị phân trên tổng tiền tố cũng đủ nhanh và đơn giản hơn để giải thích. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi có thể tạo dòng thời gian danh sách phát đầy đủ từng phút và lưu trữ bài hát nào phát ở mỗi phút. 

Ví dụ:```
song 1: duration 2, repeats 3 times
```sẽ đóng góp:```
[1, 1, 1, 1, 1, 1]
```vì bài hát chiếm tổng cộng 6 phút. 

Sau đó, mọi truy vấn sẽ trở thành tra cứu trực tiếp. 

Điều này hoạt động hợp lý vì mỗi phút đều được thể hiện rõ ràng. Vấn đề là kích thước. Tổng thời lượng có thể đạt tới`10^9`, vì vậy mảng này có thể chứa một tỷ mục. Cả bộ nhớ và thời gian chạy đều trở nên không thể. 

Cải tiến tiếp theo là quan sát rằng mỗi bài hát chiếm một khoảng thời gian liên tục trên dòng thời gian toàn cầu. 

Giả sử chúng ta tính toán thời lượng tích lũy:```
song 1 ends at minute 10
song 2 ends at minute 25
song 3 ends at minute 40
```Bây giờ một truy vấn`x`chỉ hỏi: 

“Bài hát đầu tiên có thời gian kết thúc ít nhất là`x`?” 

Đó chính xác là một vấn đề tìm kiếm nhị phân. 

Thời gian kết thúc tích lũy được sắp xếp một cách tự nhiên vì mỗi bài hát đều có thêm thời lượng tích cực. Đối với mỗi truy vấn, chúng tôi tìm kiếm nhị phân tổng tiền tố đầu tiên`>= x`. 

This reduces the work dramatically:

 Thay vì quét từng phút, chúng tôi chỉ lưu trữ một số cho mỗi khối bài hát. 

Thay vì tìm kiếm tuyến tính cho mọi truy vấn, chúng tôi sử dụng tìm kiếm nhị phân trong`O(log n)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng thời lượng + m) | O(tổng thời lượng) | Quá chậm | 
| Tối ưu | O(n + m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số khối bài hát`n`và số lượng truy vấn`m`. 
2. Xây dựng một mảng tổng tiền tố. 

Với mỗi khối bài hát, hãy tính:```
block_duration = c[i] * t[i]
```Sau đó nối thêm tổng thời lượng tích lũy cho đến nay. 

Nếu tổng tiền tố trở thành:```
[10, 25, 40]
```nó có nghĩa là: 

Bài hát 1 chiếm số phút`1..10`Bài hát 2 chiếm số phút`11..25`Bài hát 3 chiếm phút`26..40`3. Đọc từng phút truy vấn`x`. 
4. Tìm kiếm nhị phân trong mảng tổng tiền tố để tìm giá trị đầu tiên lớn hơn hoặc bằng`x`. 

Điều này mang lại cho khối bài hát chứa phút`x`. 
5. Xuất chỉ mục bài hát dưới dạng chỉ mục dựa trên 1. 

### Tại sao nó hoạt động 

Mảng tổng tiền tố lưu trữ phút kết thúc của mỗi khối bài hát. Vì mỗi thời lượng khối đều dương nên thời gian kết thúc này tăng lên rất nhiều. 

Đối với bất kỳ phút truy vấn nào`x`, bài hát đúng chính xác là khối đầu tiên có thời gian kết thúc ít nhất`x`. 

Nếu tổng tiền tố trước đó nhỏ hơn`x`, sau đó tất cả các bài hát trước đó kết thúc trước phút`x`. Nếu tổng tiền tố hiện tại ít nhất là`x`, thì bài hát hiện tại đã bắt đầu và chưa kết thúc. 

Bởi vì tìm kiếm nhị phân tìm thấy chính xác ranh giới này nên thuật toán luôn trả về đúng bài hát. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    prefix = []
    total = 0

    for _ in range(n):
        c, t = map(int, input().split())
        total += c * t
        prefix.append(total)

    queries = list(map(int, input().split()))

    ans = []

    for x in queries:
        idx = bisect_left(prefix, x)
        ans.append(str(idx + 1))

    sys.stdout.write("\n".join(ans))

solve()
```Cốt lõi của giải pháp là`prefix`mảng. Mỗi mục lưu trữ phút cuối cùng mà khối bài hát đó chiếm giữ. 

Ví dụ: nếu:```
prefix = [6, 15, 21]
```sau đó:```
song 1 -> minutes 1..6
song 2 -> minutes 7..15
song 3 -> minutes 16..21
```

`bisect_left(prefix, x)`trả về chỉ mục đầu tiên có giá trị ít nhất là`x`. Đó chính xác là bài hát chứa phút`x`. 

các`+1`là cần thiết vì danh sách Python sử dụng chỉ mục dựa trên 0 trong khi số bài hát dựa trên 1. 

Một lỗi phổ biến là sử dụng`bisect_right`. Điều đó sẽ thất bại trong những phút giới hạn khi bài hát kết thúc chính xác vào lúc`x`. 

Một lỗi dễ mắc phải khác là quên rằng dòng thời gian bắt đầu từ phút`1`, không phải phút`0`. Sử dụng thời gian kết thúc tích lũy sẽ tránh được vấn đề này một cách tự nhiên. 

Số nguyên Python xử lý tổng số tối đa có thể một cách an toàn ở đây, do đó không cần xử lý tràn đặc biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2
2 8
1 16
```Danh sách phát gồm một bài hát được lặp lại hai lần, mỗi lần lặp lại kéo dài 8 phút. 

Tổng khoảng thời gian chiếm dụng:```
minutes 1..16 -> song 1
```Tổng tiền tố: 

| Bài hát | c | t | Thời lượng chặn | Tiền tố Tổng | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 8 | 16 | 16 | 

Truy vấn: 

| Phút truy vấn | Tiền tố đầu tiên ≥ Truy vấn | Bài hát | 
| --- | --- | --- | 
| 1 | 16 | 1 | 
| 16 | 16 | 1 | 

Đầu ra:```
1
1
```Ví dụ này xác nhận rằng số phút giới hạn thuộc về cùng một khối bài hát. 

### Ví dụ 2 

đầu vào:```
3 5
1 2
2 3
1 4
1 2 5 8 12
```Xây dựng dòng thời gian. 

Bài hát 1 đóng góp`1 * 2 = 2`phút. 

Bài hát 2 góp phần`2 * 3 = 6`phút. 

Bài hát 3 góp phần`1 * 4 = 4`phút. 

Tổng tiền tố: 

| Bài hát | Thời lượng chặn | Tiền tố Tổng | 
| --- | --- | --- | 
| 1 | 2 | 2 | 
| 2 | 6 | 8 | 
| 3 | 4 | 12 | 

Truy vấn: 

| Phút truy vấn | Kết quả tìm kiếm nhị phân | Bài hát | 
| --- | --- | --- | 
| 1 | 2 | 1 | 
| 2 | 2 | 1 | 
| 5 | 8 | 2 | 
| 8 | 8 | 2 | 
| 12 | 12 | 3 | 

Đầu ra:```
1
1
2
2
3
```Dấu vết này cho thấy các vị trí truy vấn khác nhau ánh xạ như thế nào vào các khoảng liền kề được xác định bởi tổng tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m log n) | Xây dựng tổng tiền tố là tuyến tính, mỗi truy vấn sử dụng tìm kiếm nhị phân | 
| Không gian | O(n) | Mảng tổng tiền tố lưu trữ một giá trị cho mỗi khối bài hát | 

Với`n, m ≤ 10^5`, điều này dễ dàng phù hợp trong giới hạn. Xung quanh`10^5`tìm kiếm nhị phân trên một mảng có kích thước`10^5`nằm trong thời gian chạy cho phép trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from bisect import bisect_left

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    prefix = []
    total = 0

    for _ in range(n):
        c, t = map(int, input().split())
        total += c * t
        prefix.append(total)

    queries = list(map(int, input().split()))

    ans = []

    for x in queries:
        ans.append(str(bisect_left(prefix, x) + 1))

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run(
    "1 2\n"
    "2 8\n"
    "1 16\n"
) == "1\n1"

# minimum-size input
assert run(
    "1 1\n"
    "1 1\n"
    "1\n"
) == "1"

# boundary transition between songs
assert run(
    "2 4\n"
    "1 3\n"
    "1 2\n"
    "1 3 4 5\n"
) == "1\n1\n2\n2"

# all queries inside same large block
assert run(
    "1 3\n"
    "1000000000 1\n"
    "1 500000000 1000000000\n"
) == "1\n1\n1"

# mixed intervals
assert run(
    "3 5\n"
    "1 2\n"
    "2 3\n"
    "1 4\n"
    "1 2 5 8 12\n"
) == "1\n1\n2\n2\n3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bài hát đơn với một truy vấn |`1`| Đầu vào hợp lệ tối thiểu | 
| Truy vấn chính xác tại ranh giới bài hát | Chuyển tiếp đúng | Độ chính xác từng cái một | 
| Khối lặp đi lặp lại lớn | Tất cả`1`| Không mở rộng từng phút | 
| Khoảng hỗn hợp |`1 1 2 2 3`| Tính đúng đắn chung | 

## Vỏ cạnh 

Hãy xem xét trường hợp ranh giới trong đó truy vấn đến chính xác vào phút cuối cùng của khối bài hát. 

đầu vào:```
2 3
1 3
1 2
3 4 5
```Tổng tiền tố trở thành:```
[3, 5]
```Đối với truy vấn`3`, tìm kiếm nhị phân tìm tổng tiền tố đầu tiên`>= 3`, đó là chỉ số`0`. Thuật toán trả về chính xác bài hát`1`. 

Đối với truy vấn`4`, tổng tiền tố đầu tiên`>= 4`là`5`, tương ứng với bài hát`2`. 

Đầu ra:```
1
2
2
```Điều này xác nhận rằng các khoảng được xử lý toàn diện trên ranh giới bên phải. 

Bây giờ hãy xem xét một khoảng thời gian rất lớn. 

đầu vào:```
1 2
1000000000 1
1 1000000000
```Mảng tổng tiền tố chỉ chứa:```
[1000000000]
```Cả hai truy vấn tìm kiếm nhị phân trực tiếp vào khoảng thời gian duy nhất này và trả về bài hát`1`. 

Thuật toán không bao giờ xây dựng mảng hàng tỷ phút, vì vậy nó vẫn nhanh và tiết kiệm bộ nhớ. 

Cuối cùng, hãy xem xét nhiều khối bài hát nhỏ. 

đầu vào:```
4 4
1 1
1 1
1 1
1 1
1 2 3 4
```Tổng tiền tố:```
[1, 2, 3, 4]
```Mỗi truy vấn ánh xạ chính xác tới một bài hát riêng biệt. 

Đầu ra:```
1
2
3
4
```Điều này xác nhận rằng các ranh giới liên tiếp được xử lý chính xác mà không bỏ qua hoặc chồng chéo các khoảng thời gian.
