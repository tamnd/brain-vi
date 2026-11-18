---
title: "CF 203B - Trò chơi trên giấy"
description: "Chúng tôi có lưới n × n bắt đầu hoàn toàn màu trắng. Các ô được sơn đen lần lượt và mỗi lần di chuyển sẽ sơn một ô khác nhau. Sau mỗi lần di chuyển, chúng tôi muốn biết liệu bảng đã có hình vuông 3 × 3 hoàn toàn màu đen hay chưa."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 203
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 128 (Div. 2)"
rating: 1300
weight: 203
solve_time_s: 101
verified: true
draft: false
---

[CF 203B - Trò chơi trên giấy](https://codeforces.com/problemset/problem/203/B) 

**Đánh giá:** 1300 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một`n × n`lưới bắt đầu hoàn toàn màu trắng. Các ô được sơn đen lần lượt và mỗi lần di chuyển sẽ sơn một ô khác nhau. Sau mỗi lần di chuyển, chúng tôi muốn biết liệu bảng đã có toàn màu đen hay chưa`3 × 3`quảng trường. 

Nhiệm vụ là xuất ra chỉ số di chuyển sớm nhất nơi hình vuông như vậy xuất hiện lần đầu tiên. Nếu không`3 × 3`khối trở nên đen hoàn toàn, chúng tôi in`-1`. 

Kích thước lưới tối đa là`1000 × 1000`, có nghĩa là bảng chứa tối đa một triệu ô. Số lượng thao tác sơn nhiều nhất`10^5`. Điều đó ngay lập tức loại trừ mọi cách tiếp cận liên tục quét toàn bộ bảng sau mỗi lần di chuyển. Kiểm tra lại tất cả những gì có thể`3 × 3`hình vuông mỗi lần sẽ mất khoảng:$$10^5 \times 10^6 = 10^{11}$$trong trường hợp xấu nhất vượt xa giới hạn. 

Kích thước hình vuông cố định nhỏ là chi tiết quan trọng ở đây. Chúng tôi không tìm kiếm các hình chữ nhật tùy ý hoặc các mẫu lớn. Mọi cấu hình hợp lệ đều chính xác`3 × 3`, vì vậy mỗi ô mới được vẽ chỉ có thể ảnh hưởng đến một số lượng nhỏ các ô vuông ứng cử viên gần đó. 

Có một số trường hợp khó bỏ sót. 

Giả sử bảng quá nhỏ để có thể chứa một`3 × 3`quảng trường. 

đầu vào:```
2 4
1 1
1 2
2 1
2 2
```Đầu ra đúng:```
-1
```Việc triển khai bất cẩn vẫn có thể cố gắng kiểm tra các vị trí lân cận và vô tình truy cập vào các chỉ mục không hợp lệ. 

Một trường hợp tinh tế khác xuất hiện khi ô được vẽ cuối cùng không nằm ở góc trên bên trái của hình vuông đã hoàn thành. 

đầu vào:```
3 9
1 1
1 2
1 3
2 1
2 2
2 3
3 1
3 2
3 3
```Đầu ra đúng:```
9
```Nếu chúng ta chỉ kiểm tra các ô vuông có góc trên bên trái bằng ô mới được sơn thì chúng ta sẽ hoàn toàn bỏ sót điều này. Động thái cuối cùng là`(3,3)`, nhưng có liên quan`3 × 3`hình vuông bắt đầu lúc`(1,1)`. 

Ngoài ra còn có tình trạng một số`3 × 3`hình vuông chồng lên nhau. 

đầu vào:```
4 10
1 1
1 2
1 3
2 1
2 2
2 3
3 1
3 2
4 1
3 3
```Đầu ra đúng:```
10
```Thuật toán phải dừng ở thời điểm đầu tiên bất kỳ hình vuông hợp lệ nào tồn tại, không phải sau khi xử lý tất cả các nước đi. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. Sau mỗi lần di chuyển, chúng tôi quét mọi thứ có thể`3 × 3`lưới con và kiểm tra xem tất cả chín ô có màu đen hay không. 

có`(n - 2)^2`các ô vuông ứng cử viên và mỗi lần kiểm tra sẽ kiểm tra 9 ô. Sự phức tạp trở thành:$$O(m \cdot n^2)$$Vì`n = 1000`Và`m = 10^5`, điều này trở nên quá lớn. 

Quan sát quan trọng là việc tô một ô chỉ làm thay đổi các ô vuông chứa ô đó. Vì kích thước hình vuông mục tiêu được cố định ở`3 × 3`, một ô thuộc về nhiều nhất 9 ô khác nhau`3 × 3`hình vuông. 

Ví dụ, nếu chúng ta vẽ ô`(x, y)`, sau đó là góc trên bên trái của bất kỳ phần nào bị ảnh hưởng`3 × 3`hình vuông phải nằm trong:$$(x-2 \dots x,\ y-2 \dots y)$$Chỉ có 9 khả năng. Mọi ô vuông khác trên bảng vẫn không thay đổi và không cần phải kiểm tra lại. 

Điều này biến vấn đề từ việc quét liên tục toàn bộ lưới thành chỉ kiểm tra một số lượng cấu hình cục bộ không đổi sau mỗi lần di chuyển. 

Độ phức tạp thu được sẽ trở nên tuyến tính theo số bước di chuyển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m · n²) | O(n²) | Quá chậm | 
| Tối ưu | O(m) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một`n × n`lưới boolean được khởi tạo thành`False`. 

Mỗi ô lưu trữ xem nó đã được sơn màu đen hay chưa. 
2. Xử lý từng bước di chuyển theo thứ tự đầu vào. 

Động thái đầu tiên tạo ra một giá trị hợp lệ`3 × 3`hình vuông là câu trả lời, vì vậy chúng ta phải đánh giá các nước đi một cách tuần tự. 
3. Đánh dấu ô hiện tại`(x, y)`đen như vậy. 
4. Liệt kê tất cả các góc trên bên trái có thể có của`3 × 3`hình vuông có thể chứa`(x, y)`. 

Các góc này là:```
(x-2, y-2) ... (x, y)
```Có nhiều nhất 9 ứng viên. 
5. Góc trên bên trái dành cho mỗi ứng viên`(r, c)`, trước tiên hãy xác minh rằng hình vuông vẫn ở bên trong bảng. 

Chúng tôi cần:```
0 ≤ r ≤ n-3
0 ≤ c ≤ n-3
```6. Kiểm tra tất cả 9 ô bên trong đó`3 × 3`quảng trường. 

Nếu mọi ô đều màu đen, in ngay số nước đi hiện tại và kết thúc. 
7. Nếu tất cả các nước đi kết thúc mà không tìm được ô vuông hợp lệ, hãy in`-1`. 

### Tại sao nó hoạt động 

A`3 × 3`hình vuông chỉ có thể thay đổi trạng thái khi một trong các ô của nó được sơn. Trước khi sơn`(x, y)`, mọi ô vuông không bị ảnh hưởng đều có các ô giống hệt như trước nên giá trị của nó không thể thay đổi. 

Điều đó có nghĩa là sau khi di chuyển, ô vuông duy nhất đáng kiểm tra là những ô chứa ô mới được sơn. Vì mọi ô vuông như vậy đều được kiểm tra rõ ràng nên thuật toán không thể bỏ lỡ ô vuông hoàn thành đầu tiên`3 × 3`khối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())

    grid = [[False] * n for _ in range(n)]

    for move in range(1, m + 1):
        x, y = map(int, input().split())
        x -= 1
        y -= 1

        grid[x][y] = True

        for r in range(x - 2, x + 1):
            for c in range(y - 2, y + 1):

                if r < 0 or c < 0 or r + 2 >= n or c + 2 >= n:
                    continue

                ok = True

                for i in range(r, r + 3):
                    for j in range(c, c + 3):
                        if not grid[i][j]:
                            ok = False
                            break

                    if not ok:
                        break

                if ok:
                    print(move)
                    return

    print(-1)

if __name__ == "__main__":
    main()
```Giải pháp lưu trữ bảng dưới dạng ma trận boolean. Việc sử dụng boolean giúp việc triển khai trở nên đơn giản và hiệu quả vì mỗi lần tra cứu sẽ trở thành truy cập mảng trực tiếp. 

Tọa độ được chuyển đổi thành chỉ mục dựa trên 0 ngay sau khi đọc dữ liệu đầu vào. Điều này tránh lặp lại`-1`điều chỉnh sau này và làm cho các điều kiện biên dễ dàng giải thích hơn. 

Các vòng lặp:```
for r in range(x - 2, x + 1):
    for c in range(y - 2, y + 1):
```liệt kê mọi thứ có thể`3 × 3`hình vuông chứa`(x, y)`. Một lỗi phổ biến là chỉ kiểm tra`(x, y)`ở góc trên bên trái, thiếu nhiều cấu hình hợp lệ. 

Kiểm tra ranh giới:```
if r < 0 or c < 0 or r + 2 >= n or c + 2 >= n:
```ngăn chặn việc lập chỉ mục không hợp lệ gần các cạnh của bảng. 

Các vòng lặp lồng nhau trên`3 × 3`vùng dừng sớm ngay khi tìm thấy một tế bào trắng. Điều này giữ cho hệ số không đổi nhỏ. 

Khi một hình vuông hợp lệ xuất hiện, chương trình sẽ in chỉ mục di chuyển và thoát ngay lập tức. Vì các nước đi được xử lý theo thứ tự thời gian nên ô vuông được phát hiện đầu tiên được đảm bảo là ô vuông sớm nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 11
1 1
1 2
1 3
2 2
2 3
1 4
2 4
3 4
3 2
3 3
4 1
```Dấu vết: 

| Di chuyển | Tế Bào Sơn | Đã hoàn thành 3×3 Có tồn tại? | 
| --- | --- | --- | 
| 1 | (1,1) | Không | 
| 2 | (1,2) | Không | 
| 3 | (1,3) | Không | 
| 4 | (2,2) | Không | 
| 5 | (2,3) | Không | 
| 6 | (1,4) | Không | 
| 7 | (2,4) | Không | 
| 8 | (3,4) | Không | 
| 9 | (3,2) | Không | 
| 10 | (3,3) | Có | 
| 11 | chưa được xử lý | đã chấm dứt | 

Ở nước đi thứ 10, các ô:```
(1,2) (1,3) (1,4)
(2,2) (2,3) (2,4)
(3,2) (3,3) (3,4)
```tạo thành một hình thức hoàn chỉnh`3 × 3`quảng trường. 

Ví dụ này chứng minh tại sao chỉ kiểm tra xung quanh ô được vẽ mới nhất là đủ. Ô mới sơn`(3,3)`thuộc về hình vuông đã hoàn thành nên thuật toán sẽ phát hiện ra nó ngay lập tức. 

### Ví dụ 2 

đầu vào:```
3 8
1 1
1 2
1 3
2 1
2 2
2 3
3 1
3 2
```Dấu vết: 

| Di chuyển | Tế Bào Sơn | Đã hoàn thành 3×3 Có tồn tại? | 
| --- | --- | --- | 
| 1 | (1,1) | Không | 
| 2 | (1,2) | Không | 
| 3 | (1,3) | Không | 
| 4 | (2,1) | Không | 
| 5 | (2,2) | Không | 
| 6 | (2,3) | Không | 
| 7 | (3,1) | Không | 
| 8 | (3,2) | Không | 

Tế bào`(3,3)`không bao giờ được sơn, vì vậy cách duy nhất có thể`3 × 3`hình vuông vẫn chưa hoàn chỉnh. 

Đầu ra:```
-1
```Dấu vết này xác nhận rằng thuật toán không tạo ra kết quả dương tính giả khi một hình vuông gần như hoàn chỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi nước đi sẽ kiểm tra tối đa 9 ô ứng cử viên, mỗi ô chứa 9 ô | 
| Không gian | O(n²) | Trạng thái bảng được lưu trữ rõ ràng | 

Công tối đa cho mỗi lần di chuyển được giới hạn bởi một hằng số nhỏ. Ngay cả đối với`10^5`di chuyển, tổng số ô kiểm tra vẫn thoải mái trong thời hạn. Việc sử dụng bộ nhớ cũng an toàn vì`1000 × 1000`lưới boolean dễ dàng phù hợp với bên trong 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    grid = [[False] * n for _ in range(n)]

    for move in range(1, m + 1):
        x, y = map(int, input().split())
        x -= 1
        y -= 1

        grid[x][y] = True

        for r in range(x - 2, x + 1):
            for c in range(y - 2, y + 1):

                if r < 0 or c < 0 or r + 2 >= n or c + 2 >= n:
                    continue

                ok = True

                for i in range(r, r + 3):
                    for j in range(c, c + 3):
                        if not grid[i][j]:
                            ok = False
                            break

                    if not ok:
                        break

                if ok:
                    print(move)
                    return

    print(-1)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run(
"""4 11
1 1
1 2
1 3
2 2
2 3
1 4
2 4
3 4
3 2
3 3
4 1
"""
) == "10\n", "sample 1"

# minimum board size
assert run(
"""1 1
1 1
"""
) == "-1\n", "1x1 board cannot contain 3x3"

# exact 3x3 completion
assert run(
"""3 9
1 1
1 2
1 3
2 1
2 2
2 3
3 1
3 2
3 3
"""
) == "9\n", "entire board becomes black"

# incomplete square
assert run(
"""3 8
1 1
1 2
1 3
2 1
2 2
2 3
3 1
3 2
"""
) == "-1\n", "one missing cell"

# edge-aligned square
assert run(
"""5 9
3 3
3 4
3 5
4 3
4 4
4 5
5 3
5 4
5 5
"""
) == "9\n", "square touching lower-right boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1×1`bảng |`-1`| Không thể`3 × 3`vuông | 
| Đầy`3×3`bảng |`9`| Thời điểm phát hiện chính xác | 
| Một ô bị thiếu |`-1`| Không có kết quả dương tính giả | 
| Hình vuông căn chỉnh ranh giới |`9`| Xử lý cạnh đúng | 

## Vỏ cạnh 

Hãy xem xét bảng nhỏ nhất có thể. 

đầu vào:```
2 4
1 1
1 2
2 1
2 2
```MỘT`3 × 3`hình vuông không thể tồn tại trên một`2 × 2`Cái bảng. Trong mỗi lần di chuyển, các vị trí trên cùng bên trái của ứng viên không vượt qua được bước kiểm tra ranh giới:```
r + 2 >= n
```vì vậy không có hình vuông nào được kiểm tra. Thuật toán in chính xác:```
-1
```Bây giờ hãy xem xét trường hợp ô được vẽ cuối cùng không phải là góc trên cùng bên trái. 

đầu vào:```
3 9
1 1
1 2
1 3
2 1
2 2
2 3
3 1
3 2
3 3
```Ở nước đi thứ 9, ô được sơn là`(3,3)`. Thuật toán kiểm tra tất cả các góc trên bên trái ứng cử viên từ`(1,1)`ĐẾN`(3,3)`trong tọa độ dựa trên không. Hình vuông hợp lệ bắt đầu từ`(1,1)`được kiểm tra và phát hiện toàn bộ màu đen. 

Điều này xác nhận lý do tại sao việc kiểm tra tất cả các góc trên bên trái gần đó là cần thiết. 

Cuối cùng, hãy xem xét một hình vuông tiếp xúc với đường viền. 

đầu vào:```
5 9
3 3
3 4
3 5
4 3
4 4
4 5
5 3
5 4
5 5
```Hình vuông đã hoàn thành nằm ở góc dưới bên phải của bảng. Góc trên bên trái hợp lệ là`(3,3)`trong việc lập chỉ mục một cơ sở. Điều kiện biên cho phép hình vuông này vì:```
r + 2 = 4
c + 2 = 4
```vẫn nằm bên trong một`5 × 5`lưới sử dụng chỉ mục dựa trên số không. Thuật toán xuất ra chính xác:```
9
```
