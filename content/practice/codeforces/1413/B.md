---
title: "CF 1413B - Một kỹ thuật mới"
description: "Chúng ta có tất cả các hàng của một ma trận chưa biết và tất cả các cột của cùng một ma trận. Điều hấp dẫn là cả hai bộ sưu tập đều bị xáo trộn. Các hàng được trình bày theo thứ tự tùy ý và các cột cũng được trình bày theo thứ tự tùy ý. Bên trong một hàng, thứ tự từ trái sang phải được giữ nguyên."
date: "2026-06-11T07:21:04+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1413
codeforces_index: "B"
codeforces_contest_name: "Technocup 2021 - Elimination Round 1"
rating: 1100
weight: 1413
solve_time_s: 143
verified: true
draft: false
---

[CF 1413B - Một kỹ thuật mới](https://codeforces.com/problemset/problem/1413/B) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có tất cả các hàng của một ma trận chưa biết và tất cả các cột của cùng một ma trận. 

Điều hấp dẫn là cả hai bộ sưu tập đều bị xáo trộn. Các hàng được trình bày theo thứ tự tùy ý và các cột cũng được trình bày theo thứ tự tùy ý. Bên trong một hàng, thứ tự từ trái sang phải được giữ nguyên. Bên trong một cột, thứ tự từ trên xuống dưới được giữ nguyên. 

Mọi giá trị từ`1`ĐẾN`n * m`xuất hiện đúng một lần trong ma trận. Vì tất cả các giá trị đều khác nhau nên mỗi số xác định duy nhất một ô. 

Nhiệm vụ là xây dựng lại ma trận ban đầu. 

Những hạn chế là đầu mối chính. Mặc dù`n`Và`m`mỗi người có thể tiếp cận được không`500`, tổng số ô ma trận trên tất cả các trường hợp thử nghiệm nhiều nhất là`250000`. Điều này có nghĩa là một giải pháp tỷ lệ thuận với số lượng ô là đủ nhanh. Một giải pháp liên tục so sánh từng hàng với từng cột và quét toàn bộ mảng bên trong các phép so sánh đó sẽ đạt được`O(n * m * min(n,m))`, điều này là không cần thiết và có nguy cơ trở nên chậm ở gần mức giới hạn. 

Cấu trúc của dữ liệu đầu vào ẩn chứa một thuộc tính rất hữu ích: mỗi số là duy nhất. Điều đó có nghĩa là nếu chúng ta biết một giá trị thuộc về vị trí nào trong thứ tự hàng cuối cùng, chúng ta sẽ biết ngay vị trí của mỗi hàng chứa giá trị đó. 

Một lỗi phổ biến là thử khớp các hàng với tất cả các cột và xây dựng lại ô ma trận theo từng ô. Coi như:```
n = 2, m = 3

Rows:
6 5 4
1 2 3

Columns:
1 6
2 5
3 4
```Nếu ta tùy ý chọn hàng đầu tiên làm hàng trên cùng thì ta có:```
6 5 4
1 2 3
```Ma trận này không khớp với thứ tự cột. Việc xây dựng lại đúng là:```
1 2 3
6 5 4
```Thứ tự hàng phải được xác định từ các cột, không được đoán. 

Một trường hợp tế nhị khác xảy ra khi`m = 1`.```
Rows:
3
1
2

Column:
3 1 2
```Chỉ có một cột nên cột đó xác định hoàn toàn thứ tự hàng:```
3
1
2
```Bất kỳ giải pháp nào giả định có nhiều cột tồn tại hoặc luôn sử dụng hàng đầu tiên làm điểm neo đều có thể thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xác định thứ tự hàng chính xác bằng cách so sánh mọi hàng với mọi cột. Vì mỗi giá trị là duy nhất nên người ta có thể tìm kiếm vị trí phần tử đầu tiên của hàng xuất hiện bên trong cột và sử dụng thông tin đó để đặt hàng. 

Tính năng này hoạt động nhưng được triển khai một cách đơn giản, nó liên tục quét các hàng và cột. Với tổng số lên tới 250.000 ô, các tìm kiếm lặp đi lặp lại không cần thiết sẽ tạo ra nhiều công việc hơn mức cần thiết. 

Quan sát quan trọng là mọi giá trị xuất hiện chính xác một lần trong toàn bộ ma trận. 

Giả định`n <= m`. Khi đó mỗi cột chứa`n`các giá trị. Nếu chúng ta kiểm tra bất kỳ hàng nào, phần tử đầu tiên của nó phải xuất hiện ở đúng một vị trí bên trong một trong các cột. Vị trí đó chính xác là chỉ số hàng của hàng đó trong ma trận cuối cùng. 

Thậm chí còn tốt hơn nữa, giải pháp dự định của Codeforces sử dụng thứ nguyên có ít mục nhập hơn trên mỗi chuỗi. 

Khi`n <= m`, chúng tôi đọc các hàng trước và xây dựng ánh xạ:```
first element of row -> row contents
```Sau đó chúng tôi kiểm tra các cột. Cột chứa một trong những phần tử đầu tiên đó hiển thị thứ tự hàng hoàn chỉnh vì thứ tự từ trên xuống dưới của nó chính xác là thứ tự của các hàng trong ma trận. 

Khi`n > m`, ý tưởng tương tự hoạt động một cách đối xứng bằng cách sử dụng các cột thay thế. 

Đối với vấn đề cụ thể này, một công thức đơn giản hơn nữa là đủ. 

Mỗi hàng được xác định duy nhất bởi phần tử đầu tiên của nó. Trong số các cột đã cho, có đúng một cột chứa các phần tử đầu tiên đó. Khi chúng tôi tìm thấy cột đó, thứ tự của nó sẽ ngay lập tức đưa ra thứ tự các hàng trong câu trả lời. 

Điều này làm giảm việc tái thiết thành một vài tra cứu bảng băm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²m) | O(nm) | Quá chậm | 
| Tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả`n`hàng và lưu trữ chúng. 
2. Tạo một từ điển ánh xạ phần tử đầu tiên của mỗi hàng tới toàn bộ hàng. 

Điều này cho phép chúng ta khôi phục một hàng ngay lập tức khi chúng ta biết giá trị đầu tiên của nó. 
3. Đọc tất cả`m`cột. 
4. Tìm cột chứa một trong các phần tử hàng đầu tiên được lưu trữ. 

Vì phần tử đầu tiên của mỗi hàng xuất hiện chính xác một lần trong ma trận nên có chính xác một cột có các mục nhập chính xác là phần tử đầu tiên của tất cả các hàng. 
5. Sử dụng thứ tự các giá trị trong cột đó để xác định thứ tự hàng. 

Nếu cột chứa:```
a
b
c
```sau đó các hàng bắt đầu bằng`a`,`b`, Và`c`phải xuất hiện theo thứ tự đó trong ma trận cuối cùng. 
6. Xuất ra các hàng tương ứng. 

### Tại sao nó hoạt động 

Mỗi hàng được xác định duy nhất bởi phần tử đầu tiên của nó vì tất cả các giá trị ma trận đều khác biệt. 

Hãy xem xét cột chứa phần tử đầu tiên của hàng trên cùng. Vì các cột giữ nguyên thứ tự từ trên xuống dưới nên cột đó cũng chứa các phần tử đầu tiên của mỗi hàng khác theo đúng thứ tự dọc của chúng. 

Khi cột đó được tìm thấy, chuỗi các phần tử đầu tiên sẽ xác định duy nhất chuỗi các hàng. Thay thế từng phần tử đầu tiên bằng hàng tương ứng của nó sẽ xây dựng lại toàn bộ ma trận. Vì bài toán đảm bảo một ma trận hợp lệ duy nhất nên không còn sự mơ hồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n, m = map(int, input().split())

        rows = [list(map(int, input().split())) for _ in range(n)]

        pos = {}
        for row in rows:
            pos[row[0]] = row

        answer_order = None

        for _ in range(m):
            col = list(map(int, input().split()))

            if answer_order is None:
                for x in col:
                    if x in pos:
                        answer_order = col
                        break

        for first_value in answer_order:
            out.append(" ".join(map(str, pos[first_value])))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp lưu trữ mọi hàng trong một từ điển được khóa bởi phần tử đầu tiên của nó. 

Trong khi đọc các cột, chúng tôi tìm kiếm cột đặc biệt chứa các giá trị dẫn đầu hàng. Vì tất cả các giá trị là duy nhất nên khi một cột chứa bất kỳ giá trị đầu hàng nào thì cột đó chính xác là cột được tạo bởi tất cả các giá trị đầu hàng. Thứ tự của nó là thứ tự hàng mong muốn. 

Sau khi khám phá cột đó, việc xây dựng lại sẽ trở thành một chuỗi tra cứu từ điển. 

Một lỗi triển khai phổ biến là dừng lại sau khi tìm thấy giá trị phù hợp mà không giữ lại toàn bộ cột. Toàn bộ cột là cần thiết vì nó cung cấp thứ tự của tất cả các hàng chứ không chỉ một hàng. 

Một sai lầm dễ mắc phải khác là cho rằng cột trùng khớp phải là cột đầu tiên được đọc. Các cột được xáo trộn, vì vậy mỗi cột phải được kiểm tra cho đến khi tìm thấy cột chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2, m = 3

Rows:
6 5 4
1 2 3

Columns:
1 6
2 5
3 4
```Xây dựng từ điển: 

| Yếu tố đầu tiên | Hàng được lưu trữ | 
| --- | --- | 
| 6 | [6,5,4] | 
| 1 | [1,2,3] | 

Quét cột: 

| Cột | Chứa phần tử hàng đầu tiên? | Hành động | 
| --- | --- | --- | 
| [1,6] | Có | Chọn cột này | 

Lệnh đã được khôi phục: 

| Giá trị trong cột đã chọn | Hàng đầu ra | 
| --- | --- | 
| 1 | [1,2,3] | 
| 6 | [6,5,4] | 

Ma trận cuối cùng:```
1 2 3
6 5 4
```Ví dụ này cho thấy ý tưởng trung tâm. cột`[1,6]`trực tiếp cho chúng ta biết thứ tự dọc của các hàng. 

### Ví dụ 2 

đầu vào:```
n = 3, m = 1

Rows:
3
1
2

Column:
3 1 2
```Xây dựng từ điển: 

| Yếu tố đầu tiên | Hàng được lưu trữ | 
| --- | --- | 
| 3 | [3] | 
| 1 | [1] | 
| 2 | [2] | 

Quét cột: 

| Cột | Chứa phần tử hàng đầu tiên? | Hành động | 
| --- | --- | --- | 
| [3,1,2] | Có | Chọn | 

Lệnh đã được khôi phục: 

| Giá trị | Hàng đầu ra | 
| --- | --- | 
| 3 | [3] | 
| 1 | [1] | 
| 2 | [2] | 

Ma trận cuối cùng:```
3
1
2
```Điều này chứng tỏ rằng phương thức này hoạt động ngay cả khi chỉ có một cột. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi giá trị ma trận được đọc một số lần không đổi | 
| Không gian | O(nm) | Lưu trữ tất cả các hàng và từ điển tra cứu | 

Tổng số ô trên tất cả các trường hợp thử nghiệm tối đa là 250.000. Việc truyền tuyến tính qua các ô đó diễn ra thoải mái trong giới hạn thời gian và việc lưu trữ ma trận chỉ cần vài megabyte bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())

        rows = [list(map(int, input().split())) for _ in range(n)]

        pos = {row[0]: row for row in rows}

        answer_order = None

        for _ in range(m):
            col = list(map(int, input().split()))

            if answer_order is None:
                for x in col:
                    if x in pos:
                        answer_order = col
                        break

        for x in answer_order:
            out.append(" ".join(map(str, pos[x])))

    return "\n".join(out)

# provided sample
assert run(
"""2
2 3
6 5 4
1 2 3
1 6
2 5
3 4
3 1
2
3
1
3 1 2
"""
) == (
"""1 2 3
6 5 4
3
1
2"""
)

# minimum size
assert run(
"""1
1 1
1
1
"""
) == "1"

# single column
assert run(
"""1
3 1
3
1
2
3 1 2
"""
) == (
"""3
1
2"""
)

# single row
assert run(
"""1
1 4
1 2 3 4
1
2
3
4
"""
) == "1 2 3 4"

# reordered rows and columns
assert run(
"""1
3 2
5 6
1 2
3 4
1 3 5
2 4 6
"""
) == (
"""1 2
3 4
5 6"""
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ma trận 1×1 | cùng một giá trị | Ranh giới tối thiểu | 
| Ma trận 3×1 | thứ tự cột được bảo toàn | Xử lý một cột | 
| Ma trận 1×4 | hàng gốc | Xử lý một hàng | 
| Hàng xáo trộn | ma trận được xây dựng lại | Phục hồi thứ tự hàng đúng | 

## Vỏ cạnh 

Hãy xem xét trường hợp một cột:```
1
3 1
3
1
2
3 1 2
```Từ điển trở thành:```
3 -> [3]
1 -> [1]
2 -> [2]
```Cột duy nhất là`[3,1,2]`. Cột đó được chọn làm cột đặt hàng. Tra cứu từng giá trị mang lại:```
[3]
[1]
[2]
```đó chính xác là ma trận chính xác. 

Hãy xem xét trường hợp hàng đầu tiên trong đầu vào không phải là hàng trên cùng:```
1
2 3
6 5 4
1 2 3
1 6
2 5
3 4
```Một giải pháp bất cẩn có thể xuất ra các hàng theo thứ tự đầu vào:```
6 5 4
1 2 3
```Thay vào đó, thuật toán tìm cột`[1,6]`, hiểu nó là thứ tự dọc của các đầu hàng và kết quả đầu ra:```
1 2 3
6 5 4
```phù hợp với tất cả các cột. 

Hãy xem xét một ma trận một hàng:```
1
1 4
1 2 3 4
1
2
3
4
```Từ điển hàng chỉ chứa một mục nhập. Mỗi cột bao gồm một giá trị duy nhất. Cột khớp đầu tiên xác định ngay hàng duy nhất và thuật toán đưa ra ma trận hợp lệ duy nhất không thay đổi.
