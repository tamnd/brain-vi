---
title: "CF 142B - Trợ giúp chung"
description: "Chúng ta có một lưới giống như bàn cờ n × m và chúng ta muốn đặt càng nhiều quân càng tốt. Hai người lính xung đột nếu bình phương khoảng cách Euclide giữa các ô của họ bằng 5. Các cặp số nguyên duy nhất có bình phương khoảng cách bằng 5 là (1, 2) và (2, 1) cùng dấu."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 142
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 102 (Div. 1)"
rating: 1800
weight: 142
solve_time_s: 98
verified: true
draft: false
---

[CF 142B - Trợ giúp chung](https://codeforces.com/problemset/problem/142/B) 

**Đánh giá:** 1800 
**Tags:** thuật toán xây dựng, tham lam, triển khai 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một`n × m`lưới giống bàn cờ và chúng tôi muốn bố trí càng nhiều quân càng tốt. Hai người lính xung đột nếu bình phương khoảng cách Euclide giữa các ô của họ bằng chính xác`5`. 

Các cặp số nguyên duy nhất có khoảng cách bình phương bằng`5`là`(1, 2)`Và`(2, 1)`lên để ký. Nói cách khác, binh lính tấn công lẫn nhau giống hệt như các hiệp sĩ trong cờ vua. Nhiệm vụ thực sự là yêu cầu số lượng hiệp sĩ không tấn công tối đa trên một`n × m`Cái bảng. 

Kích thước bảng tối đa là`1000 × 1000`, nên tổng số ô có thể lên tới một triệu. Điều đó ngay lập tức loại trừ mọi tìm kiếm theo cấp số nhân hoặc quay lại. Ngay cả các thuật toán cố gắng khám phá các tập hợp con hoặc chạy các thuật toán đồ thị nặng trên tất cả các ô cũng sẽ gặp khó khăn. Chúng ta cần một quan sát mang tính xây dựng hoặc mang tính toán học trực tiếp. 

Một số trường hợp cạnh rất dễ bị xử lý sai nếu chúng ta chuyển thẳng sang mô hình bàn cờ. 

Hãy xem xét một hàng duy nhất: 

đầu vào:```
1 7
```Đầu ra đúng:```
7
```Các hiệp sĩ cần cả thế đứng ngang và dọc để tấn công. Trên một`1 × 7`bảng, không có hai ô nào tấn công lẫn nhau, vì vậy mọi ô vuông đều có thể bị chiếm giữ. Giải pháp bàn cờ bất cẩn sẽ trả về sai`4`. 

các`2 × m`trường hợp là cái bẫy thực sự. 

đầu vào:```
2 4
```Đầu ra đúng:```
4
```Một bàn cờ tiêu chuẩn chỉ cung cấp`4`, điều này đúng ở đây, nhưng đối với: 

đầu vào:```
2 5
```Đầu ra đúng:```
6
```Một màu xen kẽ đơn giản mang lại`5`, điều đó không tối ưu. Trên các bảng có một chiều bằng`2`, các hiệp sĩ có thể được xếp thành các khối lặp lại gồm bốn cột. 

Một ví dụ tinh tế khác: 

đầu vào:```
2 2
```Đầu ra đúng:```
4
```Không thể tấn công hiệp sĩ vì di chuyển bằng`(1,2)`hoặc`(2,1)`không vừa bên trong bảng. Mỗi ô có thể chứa một người lính. Bất kỳ công thức nào sử dụng một cách mù quáng một nửa ô sẽ thất bại. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Coi mỗi ô là một đỉnh trong biểu đồ, kết nối hai ô nếu các hiệp sĩ tấn công lẫn nhau và tìm kiếm tập hợp độc lập tối đa. Điều đó đảm bảo tính đúng đắn vì một tập hợp độc lập chính xác là một tập hợp những người lính không xung đột với nhau. 

Vấn đề là tập độc lập tối đa nói chung là theo cấp số nhân. Ngay cả khi chỉ có 40 ô, vũ lực trở nên không thực tế. Bảng của chúng tôi có thể chứa tới một triệu ô, vì vậy hướng đi này là vô vọng. 

Cấu trúc của phong trào hiệp sĩ thay đổi mọi thứ. Các hiệp sĩ luôn nhảy từ màu này sang màu khác của bàn cờ. Điều đó có nghĩa là biểu đồ tấn công là lưỡng cực. Trên hầu hết các bàn cờ, việc đặt quân mã trên tất cả các ô màu đen hoặc tất cả các ô màu trắng sẽ ngay lập tức mang lại khoảng một nửa số ô và trên thực tế, điều đó là tối ưu cho các bàn cờ có ít nhất cả hai chiều.`3`. 

Ngoại lệ duy nhất xảy ra khi một chiều rất nhỏ. 

Nếu một trong hai chiều bằng`1`, hiệp sĩ hoàn toàn không thể tấn công. Mỗi hình vuông có thể được sử dụng. 

Nếu một trong hai chiều bằng`2`, mô hình trở nên đặc biệt. Các hiệp sĩ chỉ có thể tương tác trong các nhóm gồm bốn cột liên tiếp. Trong mỗi khối bốn người như vậy, chúng ta có thể đặt bốn hiệp sĩ một cách an toàn. Bất kỳ cột còn sót lại nào đều đóng góp tối đa hai hiệp sĩ bổ sung cho mỗi cặp cột. 

Đối với tất cả các bảng lớn hơn, cách sắp xếp bàn cờ đơn giản là tối ưu, mang lại:```
ceil(n * m / 2)
```Toàn bộ vấn đề quy về việc xử lý ba trường hợp cấu trúc này một cách cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(nm)) | O(nm) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`. 
2. Chuẩn hóa kích thước sao cho`n <= m`. 

Điều này giúp đơn giản hóa việc xử lý các trường hợp đặc biệt vì chúng ta chỉ cần kiểm tra các giá trị nhỏ của`n`. 
3. Nếu`n == 1`, đầu ra`m`. 

Một hiệp sĩ cần di chuyển theo hai hướng. Trên một hàng duy nhất, không thể tấn công được. 
4. Nếu`n == 2`, xử lý bảng theo khối bốn cột. 

Trong mỗi khối gồm bốn cột, chúng ta có thể đặt bốn hiệp sĩ một cách an toàn bằng cách sử dụng mẫu lặp lại. 
5. Tính toán:```
blocks = m // 4
rem = m % 4
```6. Thêm`blocks * 4`hiệp sĩ cho các khối hoàn chỉnh. 
7. Đối với các cột còn lại: 

- nếu`rem == 0`, thêm vào`0`- nếu như`rem == 1`, thêm vào`2`- nếu như`rem >= 2`, thêm vào`4`Các cột còn sót lại có thể đóng góp tối đa bốn hiệp sĩ bổ sung. 
8. Ngược lại, khi`n >= 3`, đầu ra:```
(n * m + 1) // 2
```Đây là số ô có màu lớn hơn trong bảng tô màu bàn cờ. 

### Tại sao nó hoạt động 

Khi cả hai chiều ít nhất`3`, mỗi nước đi của hiệp sĩ sẽ thay đổi màu bàn cờ. Việc đặt các hiệp sĩ chỉ trên một màu đảm bảo không có xung đột và không có sự sắp xếp nào có thể vượt quá kích thước phân vùng lớn hơn của biểu đồ hai bên. 

các`1 × m`trường hợp không có nước đi hiệp sĩ hợp pháp nào cả, vì vậy mọi ô vuông đều an toàn. 

các`2 × m`trường hợp hoạt động khác vì các hiệp sĩ chỉ có thể tương tác trên một cấu trúc rất hạn chế. Sự sắp xếp tối ưu lặp lại cứ sau bốn cột. Bên trong mỗi đoạn bốn cột, cả bốn hiệp sĩ đều có thể được đặt an toàn. Bất kỳ phân đoạn chưa hoàn chỉnh nào đều đóng góp tối đa hai hiệp sĩ cho mỗi cặp cột còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

if n > m:
    n, m = m, n

if n == 1:
    print(m)

elif n == 2:
    blocks = m // 4
    rem = m % 4

    ans = blocks * 4

    if rem == 1:
        ans += 2
    elif rem > 1:
        ans += 4

    print(ans)

else:
    print((n * m + 1) // 2)
```Bước đầu tiên hoán đổi kích thước sao cho`n`luôn là số nhỏ hơn. Điều đó cho phép tất cả các hoạt động xử lý đặc biệt tập trung vào`n == 1`Và`n == 2`. 

các`n == 1`chi nhánh là trực tiếp. Các hiệp sĩ không thể tấn công vì mọi cuộc tấn công đều yêu cầu chuyển động qua hai chiều. 

các`n == 2`logic là phần khó khăn duy nhất. Mẫu lặp lại trải dài bốn cột. Mỗi nhóm hoàn chỉnh đóng góp chính xác bốn hiệp sĩ. Việc xử lý phần còn lại rất tinh tế: 

Đối với một cột phụ, chúng ta có thể đặt thêm hai hiệp sĩ. 

Đối với hai hoặc ba cột phụ, chúng ta có thể đặt thêm bốn hiệp sĩ. 

Một lỗi phổ biến là sử dụng:```
ans += rem * 2
```vượt quá khi nào`rem == 3`. 

Nhánh cuối cùng xử lý tất cả các bảng lớn hơn bằng công thức bàn cờ. Biểu thức:```
(n * m + 1) // 2
```tính toán trần của một nửa ô. 

Số nguyên Python dễ dàng xử lý tích tối đa ở đây, nhưng ngay cả trong các ngôn ngữ có số nguyên có chiều rộng cố định,`1000 × 1000`là hoàn toàn an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 4
```| Bước | Giá trị | 
| --- | --- | 
| Chuẩn hóa`(n, m)`|`(2, 4)`| 
|`blocks`|`1`| 
|`rem`|`0`| 
| Câu trả lời cơ bản |`4`| 
| Bổ sung từ phần còn lại |`0`| 
| Câu trả lời cuối cùng |`4`| 

Ví dụ này cho thấy một khối bốn cột hoàn hảo. Việc xây dựng lặp lại phù hợp chính xác một lần. 

### Ví dụ 2 

đầu vào:```
3 4
```| Bước | Giá trị | 
| --- | --- | 
| Chuẩn hóa`(n, m)`|`(3, 4)`| 
| Trường hợp đặc biệt được kích hoạt | Không | 
| Tổng số ô |`12`| 
| Công thức |`(12 + 1) // 2`| 
| Câu trả lời cuối cùng |`6`| 

Điều này thể hiện trường hợp bàn cờ tổng quát. Một nửa số ô có thể được sử dụng mà không có xung đột. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học và kiểm tra điều kiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp thực hiện công việc liên tục bất kể kích thước bảng. Thậm chí là tối đa`1000 × 1000`bảng được xử lý ngay lập tức, trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    if n > m:
        n, m = m, n

    if n == 1:
        print(m)

    elif n == 2:
        blocks = m // 4
        rem = m % 4

        ans = blocks * 4

        if rem == 1:
            ans += 2
        elif rem > 1:
            ans += 4

        print(ans)

    else:
        print((n * m + 1) // 2)

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
assert run("2 4\n") == "4", "sample 1"

# custom cases
assert run("1 1\n") == "1", "minimum board"
assert run("1 7\n") == "7", "single row"
assert run("2 5\n") == "6", "special 2-row pattern"
assert run("3 3\n") == "5", "checkerboard case"
assert run("1000 1000\n") == "500000", "maximum size"
assert run("2 2\n") == "4", "small special case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Bảng tối thiểu | 
|`1 7`|`7`| Không tấn công vào một hàng | 
|`2 5`|`6`| Xử lý phần còn lại đúng cho`n = 2`| 
|`3 3`|`5`| Công thức bàn cờ tổng quát | 
|`1000 1000`|`500000`| Hạn chế tối đa | 
|`2 2`|`4`| Bảng nhỏ nơi mọi hình vuông đều có thể sử dụng được | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1 7
```Thuật toán chuẩn hóa kích thước và nhập`n == 1`chi nhánh. Nó lập tức quay lại`7`. 

Không có nước đi hiệp sĩ nào có thể tồn tại vì không thể di chuyển theo chiều dọc. Thuật toán tránh được công thức bàn cờ một cách chính xác, điều này sẽ đưa ra sai`4`. 

Bây giờ hãy xem xét:```
2 5
```Việc tính toán trở thành: 

| Biến | Giá trị | 
| --- | --- | 
|`blocks`|`1`| 
|`rem`|`1`| 
| Căn cứ |`4`| 
| Thêm |`2`| 
| Trả lời |`6`| 

Cột đơn còn lại đóng góp chính xác hai hiệp sĩ bổ sung. Điều này nắm bắt các triển khai chỉ thêm một cách không chính xác. 

Cuối cùng:```
2 2
```Chúng tôi nhận được: 

| Biến | Giá trị | 
| --- | --- | 
|`blocks`|`0`| 
|`rem`|`2`| 
| Căn cứ |`0`| 
| Thêm |`4`| 
| Trả lời |`4`| 

Mặc dù bàn cờ rất nhỏ nhưng không có đòn tấn công hiệp sĩ nào có thể lọt vào bên trong nó. Việc xử lý đặc biệt một cách chính xác cho phép mọi ô vuông đều được chiếm giữ.
