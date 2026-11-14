---
title: "CF 1804A - Vua què"
description: "Vua bắt đầu từ điểm gốc của lưới và muốn đến ô đích (a, b). Không giống như một vị vua bình thường, nó chỉ có thể di chuyển một bước theo chiều dọc hoặc chiều ngang hoặc giữ nguyên vị trí. Hạn chế bất thường là không thể sử dụng cùng một hành động hai lần liên tiếp."
date: "2026-06-09T09:22:32+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1804
codeforces_index: "A"
codeforces_contest_name: "Nebius Welcome Round (Div. 1 + Div. 2)"
rating: 800
weight: 1804
solve_time_s: 220
verified: true
draft: false
---

[CF 1804A - Vua què](https://codeforces.com/problemset/problem/1804/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 3 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vua bắt đầu từ điểm gốc của lưới và muốn tiếp cận ô mục tiêu`(a, b)`. Không giống như một vị vua bình thường, nó chỉ có thể di chuyển một bước theo chiều dọc hoặc chiều ngang hoặc giữ nguyên vị trí. Hạn chế bất thường là không thể sử dụng cùng một hành động hai lần liên tiếp. 

Nếu vua di chuyển ngay giây này thì nó không thể di chuyển ngay giây tiếp theo. Nếu nó bỏ qua giây này thì nó không thể bỏ qua giây tiếp theo. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi cần số giây tối thiểu cần thiết để đạt được mục tiêu. 

Tọa độ rất nhỏ, giá trị tuyệt đối nhiều nhất là 100 và có tới 10.000 trường hợp thử nghiệm. Điều này ngay lập tức gợi ý rằng lời giải phải là một công thức toán học đơn giản. Thậm chí một`O(|a|+|b|)`mô phỏng cho mỗi bài kiểm tra sẽ hoạt động, nhưng có câu trả lời trực tiếp về thời gian không đổi. 

Phần khó khăn là hiểu được hạn chế ảnh hưởng như thế nào đến việc di chuyển. Một giải pháp bất cẩn có thể cho rằng câu trả lời chỉ đơn giản là khoảng cách Manhattan`|a|+|b|`, nhưng điều đó không thành công khi chỉ cần chuyển động theo một hướng. 

Coi như:```text
a = 0, b = -6
```Nhà vua cần sáu nước đi trái. Vì hai bước di chuyển trái liên tiếp đều bị cấm nên phải chèn một cái gì đó vào giữa chúng:```text
L S L S L S L S L S L
```Quá trình này mất 11 giây chứ không phải 6. 

Một trường hợp tinh tế khác là khi cả hai tọa độ đều yêu cầu cùng một lượng chuyển động:```text
a = 4, b = 4
```Chúng ta có thể luân phiên:```text
R U R U R U R U
```Không cần thêm giây. Câu trả lời là 8. 

Trường hợp quan trọng thứ ba là khi một tọa độ chiếm ưu thế:```text
a = 7, b = -8
```Sau khi xen kẽ 7 nước đi ngang và 7 nước đi dọc, còn lại một nước đi dọc. Vì nước đi cuối cùng đã theo chiều dọc nên chúng ta phải chèn thêm một hành động trước nước đi dọc cuối cùng. Câu trả lời sẽ là 15 thay vì 14. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô hình hóa trạng thái như sau: 

- vị trí hiện tại 
- hành động trước đó 

Sau đó thực hiện BFS cho đến khi đạt được mục tiêu. 

Bảng chỉ chứa 201 × 201 ô và có 5 hành động có thể thực hiện trước đó nên không gian trạng thái nhỏ. Điều này thực sự sẽ giải quyết vấn đề trong giới hạn nhất định. 

Tuy nhiên, BFS đang giải quyết một vấn đề lớn hơn nhiều so với mức cần thiết. Tọa độ rất nhỏ nhưng hạn chế chuyển động có cấu trúc rất đều đặn. Chúng ta có thể rút ra câu trả lời trực tiếp. 

Cho phép:```text
x = |a|
y = |b|
```Giả định`x ≥ y`. 

Chúng ta có thể xen kẽ các chuyển động ngang và dọc cho`y`vòng:```text
H V H V ... H V
```Điều này tiêu tốn tất cả`y`chuyển động thẳng đứng và`y`chuyển động ngang. 

Bây giờ chỉ:```text
x - y
```chuyển động ngang vẫn còn. 

Nước đi ngang đầu tiên còn lại có thể được thực hiện ngay lập tức nếu nước đi cuối cùng là nước đi dọc. Sau đó, mỗi lần di chuyển theo chiều ngang bổ sung đều cần có dấu phân cách vì các bước di chuyển liên tiếp giống hệt nhau đều bị cấm. 

Đối với các bước di chuyển ngang còn lại:```text
k = x - y
```chi phí của họ là:```text
1 + 2(k - 1) = 2k - 1
```khi`k > 0`. 

Kết hợp mọi thứ:```text
2y + (2k - 1)
= 2y + 2(x-y) - 1
= 2x - 1
```bất cứ khi nào`x > y`. 

Nếu như`x = y`, sự thay thế hoàn hảo là có thể và câu trả lời đơn giản là:```text
2x
```Điều này mang lại một công thức rất nhỏ gọn:```text
if x == y:
    answer = 2x
else:
    answer = 2 * max(x, y) - 1
```### So sánh phương pháp tiếp cận 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| BFS về vị trí và bước di chuyển trước đó | O(Kích thước bảng) | O(Kích thước bảng) | Được chấp nhận nhưng không cần thiết | 
| Công thức toán học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tọa độ mục tiêu`(a, b)`. 

2. Tính toán:```text
   x = |a|
   y = |b|
   ```Chỉ có số lần di chuyển theo chiều ngang và chiều dọc cần thiết mới quan trọng. 

3. Hãy:```text
   m = max(x, y)
   n = min(x, y)
   ```4. Nếu`m == n`, đầu ra:```text
   2m
   ```Chúng ta có thể luân phiên hoàn hảo giữa hai kiểu di chuyển. 

5. Nếu không thì xuất ra:```text
   2m - 1
   ```Sau khi đã cạn kiệt hướng nhỏ, hướng lớn hơn vẫn cần di chuyển thêm. Mỗi bước đi bổ sung sau bước đầu tiên đều yêu cầu một hành động phân cách. 

### Tại sao nó hoạt động 

Chiến lược tối ưu luôn thay đổi các hướng di chuyển bất cứ khi nào cả hai đều có sẵn. Điều này sử dụng một lần di chuyển theo chiều ngang và một lần di chuyển theo chiều dọc trong hai giây mà không có hành động lãng phí nào. 

Sau khi tất cả các bước di chuyển theo hướng nhỏ hơn được tiêu thụ, chỉ còn lại một loại nước đi. Nước đi còn lại đầu tiên có thể được thực hiện ngay lập tức vì nước đi trước đó thuộc loại ngược lại. Mỗi nước đi sau cùng loại đó yêu cầu chính xác một hành động phân tách. Sử dụng bỏ qua là đủ nên không cần chi phí lớn hơn. 

Do đó, công thức dẫn xuất khớp chính xác với thời gian tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        a, b = map(int, input().split())

        x = abs(a)
        y = abs(b)

        if x == y:
            ans.append(str(2 * x))
        else:
            ans.append(str(2 * max(x, y) - 1))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo đạo hàm toán học trực tiếp. 

Lấy giá trị tuyệt đối sẽ loại bỏ mọi sự phụ thuộc vào hướng. Di chuyển sang trái hoạt động giống hệt như di chuyển sang phải từ góc độ hạn chế. Điều tương tự cũng đúng cho lên và xuống. 

Nhánh duy nhất là liệu hai số lần di chuyển bắt buộc có bằng nhau hay không. Khi chúng bằng nhau thì có thể thay thế hoàn hảo. Nếu không thì số lượng lớn hơn sẽ xác định câu trả lời. 

Không tồn tại mối lo ngại tràn vì tọa độ được giới hạn bởi 100. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```text
-4 1
```| Biến | Giá trị | 
|---|---| 
| x | 4 | 
| y | 1 | 
| tối đa(x,y) | 4 | 
| x == y | Không | 
| Trả lời | 2×4−1 = 7 | 

Một chuỗi hợp lệ là:```text
D R D R D L D
```Dấu vết cho thấy trường hợp tọa độ không bằng nhau. Số lượng chuyển động lớn hơn chiếm ưu thế trong câu trả lời. 

### Ví dụ 2 

đầu vào:```text
4 4
```| Biến | Giá trị | 
|---|---| 
| x | 4 | 
| y | 4 | 
| tối đa(x,y) | 4 | 
| x == y | Có | 
| Trả lời | 8 | 

Một chuỗi hợp lệ là:```text
R U R U R U R U
```Điều này thể hiện sự luân phiên hoàn hảo. Không cần di chuyển bỏ qua. 

### Ví dụ 3 

đầu vào:```text
0 -6
```| Biến | Giá trị | 
|---|---| 
| x | 0 | 
| y | 6 | 
| tối đa(x,y) | 6 | 
| x == y | Không | 
| Trả lời | 11 | 

Một chuỗi hợp lệ là:```text
L S L S L S L S L S L
```Đây là trường hợp cực đoan khi mọi chuyển động đều theo một hướng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một vài phép tính số học | 
| Không gian | O(1) | Bộ nhớ bổ sung liên tục | 

Ngay cả với 10.000 trường hợp thử nghiệm, giải pháp chỉ thực hiện một số thao tác cho mỗi thử nghiệm. Nó dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        a, b = map(int, input().split())

        x = abs(a)
        y = abs(b)

        if x == y:
            ans.append(str(2 * x))
        else:
            ans.append(str(2 * max(x, y) - 1)

)

    return "\n".join(ans)

# provided sample
assert run(
"""5
-4 1
4 4
0 -6
-5 -4
7 -8
"""
) == """7
8
11
9
15"""

# minimum movement
assert run(
"""1
1 0
"""
) == "1"

# equal coordinates
assert run(
"""1
3 3
"""
) == "6"

# only one direction needed
assert run(
"""1
0 5
"""
) == "9"

# largest coordinates
assert run(
"""1
100 -100
"""
) == "200"
```### Tóm tắt thử nghiệm tùy chỉnh 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|`(1,0)`|`1`| Mục tiêu khác 0 nhỏ nhất | 
|`(3,3)`|`6`| Sự thay thế hoàn hảo | 
|`(0,5)`|`9`| Sử dụng lặp đi lặp lại một hướng | 
|`(100,-100)`|`200`| Chuyển động cân bằng tối đa | 

## Vỏ cạnh 

Hãy xem xét:```text
1
0 1
```Vua thực hiện một nước đi trái hoặc phải và đến ngay lập tức. Thuật toán tính toán:```text
max = 1
min = 0
answer = 2×1−1 = 1
```cái nào đúng. 

Coi như:```text
1
5 5
```Mỗi nước đi có thể được xen kẽ với một nước đi thuộc loại khác:```text
R U R U R U R U R U
```Thuật toán trả về:```text
2×5 = 10
```phù hợp với độ dài chuỗi tối ưu. 

Coi như:```text
1
0 6
```Chỉ có một loại di chuyển tồn tại. Cần có năm dấu phân cách:```text
R S R S R S R S R S R
```Thuật toán trả về:```text
2×6−1 = 11
```đó chính xác là số lần di chuyển trong công trình tối ưu.
