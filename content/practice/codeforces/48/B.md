---
title: "CF 48B - Lô đất"
description: "Khu vườn được biểu diễn dưới dạng lưới n × m. Mỗi ô chứa 0 hoặc 1. Số 1 có nghĩa là có một cây trong hình vuông đó, trong khi 0 có nghĩa là hình vuông trống."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 48
codeforces_index: "B"
codeforces_contest_name: "School Personal Contest #3 (Winter Computer School 2010/11) - Codeforces Beta Round 45 (ACM-ICPC Rules)"
rating: 1200
weight: 48
solve_time_s: 107
verified: true
draft: false
---
[CF 48B - Lô đất](https://codeforces.com/problemset/problem/48/B) 

**Đánh giá:** 1200 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Khu vườn được thể hiện như một`n × m`lưới. Mỗi ô chứa một trong hai`0`hoặc`1`. MỘT`1`nghĩa là có một cái cây trong hình vuông đó, trong khi`0`có nghĩa là hình vuông trống. 

Chúng tôi muốn đặt một lô đất hình chữ nhật ở đâu đó bên trong lưới. Hình chữ nhật phải có kích thước`a × b`, nhưng được phép xoay, vì vậy a`b × a`hình chữ nhật cũng hợp lệ. Mọi cây bên trong hình chữ nhật đã chọn sẽ bị xóa. Nhiệm vụ là tìm ra vị trí tiêu diệt được ít cây nhất. 

Đây thực sự là một vấn đề truy vấn hình chữ nhật. Đối với mỗi vị trí hình chữ nhật hợp lệ, chúng ta cần đếm có bao nhiêu`1`s nằm bên trong nó, sau đó lấy mức tối thiểu trên tất cả các vị trí. 

Các giới hạn rất nhỏ: cả hai chiều đều tối đa`50`. Một lưới chứa nhiều nhất`2500`tế bào. Thậm chí một`O(n^2 m^2)`giải pháp hoàn toàn ổn ở đây vì`50^4 = 6.25 million`, thoải mái trong giới hạn thời gian trong Python. 

Phần nguy hiểm không phải là hiệu suất, mà là tính chính xác trong việc sắp xếp và xoay hình chữ nhật. 

Một sai lầm dễ mắc phải là quên rằng ngôi nhà có thể bị xoay. 

Ví dụ:```
2 3
1 1 1
0 0 0
3 2
```Hình chữ nhật`3 × 2`không phù hợp trực tiếp bởi vì`3 > n`, nhưng đã quay`2 × 3`hình chữ nhật phù hợp hoàn hảo. Câu trả lời đúng là:```
3
```Việc triển khai bất cẩn chỉ kiểm tra một hướng sẽ từ chối vị trí một cách không chính xác. 

Một lỗi phổ biến khác là lỗi từng cái một khi liệt kê các góc trên cùng bên trái. 

Coi như:```
3 3
0 0 0
0 1 0
0 0 0
2 2
```Các góc trên bên trái hợp lệ là`(0,0)`,`(0,1)`,`(1,0)`, Và`(1,1)`. Việc thiếu hàng hoặc cột cuối cùng trong quá trình lặp có thể bỏ qua hình chữ nhật tối ưu hợp lệ. 

Câu trả lời đúng là:```
0
```bởi vì chúng ta có thể đặt hình chữ nhật ở bất kỳ góc nào cách xa cây trung tâm. 

Ngoài ra còn có trường hợp đặc biệt khi kích thước hình chữ nhật bằng toàn bộ lưới. 

Ví dụ:```
2 2
1 0
1 1
2 2
```Chỉ có một vị trí tồn tại nên câu trả lời phải bằng tổng số cây:```
3
```Việc triển khai giả định luôn có chỗ để trượt hình chữ nhật sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. 

Đối với mỗi vị trí hợp lệ của hình chữ nhật, chúng tôi quét từng ô bên trong hình chữ nhật đó và đếm xem nó chứa bao nhiêu cây. Vì được phép xoay nên chúng tôi lặp lại điều này cho cả hai`(a, b)`Và`(b, a)`. 

Giả sử kích thước hình chữ nhật là`h × w`. Có khoảng`(n - h + 1)(m - w + 1)`các vị trí. Đối với mỗi vị trí, chúng tôi kiểm tra`h × w`tế bào. 

Tổng độ phức tạp trở thành:```
O((n - h + 1)(m - w + 1)hw)
```Trong trường hợp xấu nhất, mọi số lượng đều ở xung quanh`50`, do đó số lượng hoạt động là khoảng:```
50 × 50 × 50 × 50 = 6.25 million
```Ngay cả giải pháp mạnh mẽ này cũng thực sự vượt qua được những ràng buộc nhất định. 

Tuy nhiên, vẫn có một cách rõ ràng hơn để suy nghĩ về vấn đề. Mọi truy vấn đều yêu cầu tổng các giá trị bên trong một hình chữ nhật con. Việc tính toán lại ô tổng đó bằng cách lặp lại ô sẽ hoạt động không cần thiết. 

Quan sát quan trọng là tổng hình chữ nhật có thể được trả lời ngay lập tức bằng cách sử dụng tổng tiền tố 2D. 

Chúng tôi xây dựng một ma trận tiền tố trong đó:```
pref[i][j]
```lưu trữ số lượng cây bên trong hình chữ nhật từ`(1,1)`ĐẾN`(i,j)`. 

Sau đó, bất kỳ tổng hình chữ nhật nào cũng có thể được tính bằng cách loại trừ bao gồm trong thời gian không đổi. 

Ý tưởng brute-force vẫn giữ nguyên: liệt kê tất cả các vị trí có thể. Sự cải tiến đến từ việc thay thế việc quét hình chữ nhật bên trong bằng một`O(1)`truy vấn. 

Điều đó làm giảm sự phức tạp xuống:```
O(number of placements)
```nhiều nhất là về`2500`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nmab) | O(1) | Đã chấp nhận | 
| Tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và lưu nó dưới dạng số nguyên. 
2. Xây dựng mảng tổng tiền tố 2D`pref`.`pref[i][j]`lưu trữ tổng số cây bên trong hình chữ nhật từ góc trên bên trái`(1,1)`ĐẾN`(i,j)`. 
3. Xác định hàm trợ giúp trả về số lượng cây bên trong bất kỳ hình chữ nhật nào bằng cách sử dụng loại trừ bao gồm. 

Đối với hình chữ nhật có các góc`(r1,c1)`Và`(r2,c2)`:```
sum =
pref[r2][c2]
- pref[r1-1][c2]
- pref[r2][c1-1]
+ pref[r1-1][c1-1]
```4. Hãy thử cả hai hướng:`(a,b)`Và`(b,a)`. 

Cho phép xoay nên cả hai kích thước phải được kiểm tra độc lập. 
5. Đối với mỗi hướng, hãy liệt kê mọi góc trên cùng bên trái hợp lệ. 

Nếu kích thước hình chữ nhật là`h × w`, sau đó:```
1 ≤ row ≤ n-h+1
1 ≤ col ≤ m-w+1
```6. Đối với mỗi vị trí, hãy tính số cây bên trong hình chữ nhật bằng cách sử dụng tổng tiền tố. 
7. Giữ giá trị tối thiểu trên tất cả các vị trí và in nó. 

### Tại sao nó hoạt động 

Ma trận tổng tiền tố đảm bảo rằng mỗi tổng hình chữ nhật được tính chính xác một lần thông qua loại trừ bao gồm. Mọi vị trí hình chữ nhật hợp lệ đều được kiểm tra, bao gồm cả các hướng được phép. Vì thuật toán kiểm tra tất cả các ứng viên và tính toán chính xác số lượng cây bên trong mỗi cây nên giá trị tối thiểu tìm được nhất thiết phải là câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

grid = [list(map(int, input().split())) for _ in range(n)]

a, b = map(int, input().split())

pref = [[0] * (m + 1) for _ in range(n + 1)]

for i in range(1, n + 1):
    for j in range(1, m + 1):
        pref[i][j] = (
            grid[i - 1][j - 1]
            + pref[i - 1][j]
            + pref[i][j - 1]
            - pref[i - 1][j - 1]
        )

def rect_sum(r1, c1, r2, c2):
    return (
        pref[r2][c2]
        - pref[r1 - 1][c2]
        - pref[r2][c1 - 1]
        + pref[r1 - 1][c1 - 1]
    )

ans = float('inf')

for h, w in [(a, b), (b, a)]:
    if h > n or w > m:
        continue

    for r in range(1, n - h + 2):
        for c in range(1, m - w + 2):
            trees = rect_sum(r, c, r + h - 1, c + w - 1)
            ans = min(ans, trees)

print(ans)
```Phần đầu tiên đọc lưới và xây dựng ma trận tổng tiền tố. Việc triển khai sử dụng lập chỉ mục dựa trên 1 cho`pref`, loại bỏ nhiều kiểm tra ranh giới. Mỗi mục nhập tiền tố bao gồm ô hiện tại, hình chữ nhật ở trên, hình chữ nhật ở bên trái và trừ phần chồng lấp một lần. 

các`rect_sum`chức năng là tiện ích cốt lõi. Nó tính tổng hình chữ nhật bất kỳ trong thời gian không đổi bằng cách sử dụng loại trừ bao gồm. Các tọa độ đã bao gồm, đó là lý do tại sao góc dưới bên phải được chuyển thành`r + h - 1`Và`c + w - 1`. 

Vòng lặp bên ngoài kiểm tra cả hai hướng của hình chữ nhật. Nếu một hướng không vừa với lưới, nó sẽ bị bỏ qua ngay lập tức. 

Các phạm vi:```
range(1, n - h + 2)
```Và```
range(1, m - w + 2)
```là những nơi dễ mắc sai lầm. Phạm vi Python loại trừ điểm cuối, vì vậy`+2`là cần thiết để bao gồm vị trí bắt đầu hợp lệ cuối cùng. 

Câu trả lời được cập nhật với tổng hình chữ nhật tối thiểu gặp phải. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
1 0
1 1
1 1
```Kích thước hình chữ nhật là`1 × 1`. 

Lưới: 

| Hàng | Giá trị | 
| --- | --- | 
| 1 | 1 0 | 
| 2 | 1 1 | 

Các vị trí có thể: 

| Trên cùng bên trái | Ô hình chữ nhật | Cây | 
| --- | --- | --- | 
| (1,1) | 1 | 1 | 
| (1,2) | 0 | 0 | 
| (2,1) | 1 | 1 | 
| (2,2) | 1 | 1 | 

Giá trị tối thiểu là`0`. 

Đầu ra:```
0
```Dấu vết này cho thấy thuật toán kiểm tra chính xác mọi vị trí bắt đầu có thể và giữ tổng hình chữ nhật nhỏ nhất. 

### Ví dụ 2 

đầu vào:```
3 3
0 1 0
1 1 1
0 1 0
2 2
```Tổng tiền tố: 

| tôi | j | pref[i][j] | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 1 | 2 | 1 | 
| 1 | 3 | 1 | 
| 2 | 1 | 1 | 
| 2 | 2 | 3 | 
| 2 | 3 | 4 | 
| 3 | 1 | 1 | 
| 3 | 2 | 4 | 
| 3 | 3 | 5 | 

Kiểm tra hình chữ nhật: 

| Trên cùng bên trái | Dưới cùng bên phải | Cây | 
| --- | --- | --- | 
| (1,1) | (2,2) | 3 | 
| (1,2) | (2,3) | 3 | 
| (2,1) | (3,2) | 3 | 
| (2,2) | (3,3) | 3 | 

Trả lời:```
3
```Ví dụ này minh họa cách tổng tiền tố cho phép mọi truy vấn hình chữ nhật được trả lời ngay lập tức, bất kể kích thước hình chữ nhật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Tổng tiền tố xây dựng lấy O(nm) và tất cả các vị trí hình chữ nhật cùng nhau cũng là O(nm) | 
| Không gian | O(nm) | Ma trận tổng tiền tố lưu trữ một giá trị trên mỗi ô lưới | 

Với`n, m ≤ 50`, tổng công việc là rất nhỏ. Ngay cả cách tiếp cận bạo lực cũng được thực hiện một cách thoải mái, trong khi giải pháp tổng tiền tố về cơ bản chạy ngay lập tức và có quy mô tốt hơn nhiều về mặt khái niệm. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    grid = [list(map(int, input().split())) for _ in range(n)]
    a, b = map(int, input().split())

    pref = [[0] * (m + 1) for _ in range(n + 1)]

    for i in range(1, n + 1):
        for j in range(1, m + 1):
            pref[i][j] = (
                grid[i - 1][j - 1]
                + pref[i - 1][j]
                + pref[i][j - 1]
                - pref[i - 1][j - 1]
            )

    def rect_sum(r1, c1, r2, c2):
        return (
            pref[r2][c2]
            - pref[r1 - 1][c2]
            - pref[r2][c1 - 1]
            + pref[r1 - 1][c1 - 1]
        )

    ans = float('inf')

    for h, w in [(a, b), (b, a)]:
        if h > n or w > m:
            continue

        for r in range(1, n - h + 2):
            for c in range(1, m - w + 2):
                ans = min(ans, rect_sum(r, c, r + h - 1, c + w - 1))

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run(
"""2 2
1 0
1 1
1 1
"""
) == "0", "sample 1"

# minimum size grid
assert run(
"""1 1
1
1 1
"""
) == "1", "single cell"

# rotation required
assert run(
"""2 3
1 1 1
0 0 0
3 2
"""
) == "3", "rotation"

# all zeros
assert run(
"""4 4
0 0 0 0
0 0 0 0
0 0 0 0
0 0 0 0
2 2
"""
) == "0", "empty garden"

# entire grid selected
assert run(
"""2 2
1 0
1 1
2 2
"""
) == "3", "whole grid"

# off-by-one boundary case
assert run(
"""3 3
0 0 0
0 1 0
0 0 0
2 2
"""
) == "0", "last valid position"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1×1`lưới một cây |`1`| Xử lý kích thước tối thiểu | 
| Vị trí hợp lệ chỉ xoay vòng |`3`| Xử lý đúng các kích thước được hoán đổi | 
| Tất cả số không |`0`| Tính toán tối thiểu đúng | 
| Hình chữ nhật bằng lưới |`3`| Logic vị trí ranh giới | 
| Cây trung tâm trong`3×3`lưới |`0`| Độ chính xác từng cái một trong lần lặp | 

## Vỏ cạnh 

Hãy xem xét trường hợp chỉ xoay:```
2 3
1 1 1
0 0 0
3 2
```sự định hướng`(3,2)`không vừa vì chiều cao`3`vượt quá`n=2`. Thuật toán bỏ qua nó. Sau đó nó cố gắng`(2,3)`, phù hợp chính xác một lần. Tổng hình chữ nhật bằng`3`, do đó đầu ra trở thành:```
3
```Điều này xác nhận rằng việc kiểm tra cả hai hướng là bắt buộc. 

Bây giờ hãy xem xét từng trường hợp một:```
3 3
0 0 0
0 1 0
0 0 0
2 2
```Các vị trí trên cùng bên trái hợp lệ là:```
(1,1), (1,2), (2,1), (2,2)
```Vị trí bắt đầu từ`(2,2)`đặc biệt quan trọng vì nó chạm vào ranh giới phía dưới bên phải. Thuật toán bao gồm nó vì các vòng lặp chạy cho đến khi:```
range(1, n - h + 2)
```Vì`n=3`Và`h=2`, điều này trở thành:```
range(1, 4 - 2)
= range(1, 3)
```trong đó bao gồm một cách chính xác`2`. 

Hình chữ nhật tại`(2,2)`chỉ chứa một cây, trong khi các vị trí ở góc không chứa cây nào. Câu trả lời cuối cùng trở thành:```
0
```Cuối cùng, hãy xem xét trường hợp toàn lưới:```
2 2
1 0
1 1
2 2
```Chỉ có một vị trí tồn tại. Truy vấn tổng hình chữ nhật bao gồm toàn bộ ma trận tiền tố:```
pref[2][2] = 3
```Không xảy ra trượt và thuật toán xuất ra chính xác:```
3
```
