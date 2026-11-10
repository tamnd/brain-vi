---
title: "CF 153E - Khoảng cách Euclide"
description: "Chúng tôi được cho tới 50 điểm trên mặt phẳng 2D. Mỗi điểm có tọa độ nguyên và một số điểm có thể trùng nhau. Nhiệm vụ là tìm khoảng cách Euclide lớn nhất giữa bất kỳ cặp điểm nào."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 153
codeforces_index: "E"
codeforces_contest_name: "Surprise Language Round 5"
rating: 2100
weight: 153
solve_time_s: 93
verified: false
draft: false
---

[CF 153E - Khoảng cách Euclide](https://codeforces.com/problemset/problem/153/E) 

**Xếp hạng:** 2100 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cho tới 50 điểm trên mặt phẳng 2D. Mỗi điểm có tọa độ nguyên và một số điểm có thể trùng nhau. Nhiệm vụ là tìm khoảng cách Euclide lớn nhất giữa bất kỳ cặp điểm nào. 

Cho hai điểm$(x_1, y_1)$Và$(x_2, y_2)$, khoảng cách Euclide là:$d=\sqrt{(x_1-x_2)^2+(y_1-y_2)^2}$Kéo các điểm để cập nhật khoảng cách giữa hai điểm.-10-8-6-4-2246810-10-5510A(6.0, 6.0)B(-6.0, -6.0)d = 16.97Delta x = 12Delta y = 12 

Định dạng đầu vào hơi khác thường vì mỗi tọa độ được đưa ra trên một dòng riêng. Sau khi đọc`n`, chúng tôi đọc`2n`nhiều dòng hơn: một`x`, một`y`, lặp lại cho mọi điểm. 

Những hạn chế là cực kỳ nhỏ. Chỉ với 50 điểm, tổng số cặp nhiều nhất là:$\frac{50\cdot49}{2}=1225$Đó là rất nhỏ cho giới hạn 2 giây. Thậm chí một$O(n^2)$thuật toán chỉ thực hiện vài nghìn phép tính khoảng cách, điều này không đáng kể. 

Mối nguy hiểm chính không phải là hiệu suất mà là tính đúng đắn và chính xác. 

Một sai lầm dễ mắc phải là quên rằng khoảng cách Euclide sử dụng căn bậc hai. Việc triển khai bất cẩn có thể tối đa hóa khoảng cách bình phương và in trực tiếp. 

Ví dụ:```
2
0
0
3
4
```Câu trả lời đúng là:```
5
```bởi vì:$\sqrt{3^2+4^2}=5$In ấn`25`thay vào đó sẽ sai. 

Một trường hợp tế nhị khác là điểm trùng lặp.```
3
1
1
1
1
4
5
```Hai điểm giống nhau nên khoảng cách của chúng bằng không. Khoảng cách tối đa chính xác là giữa`(1,1)`Và`(4,5)`. 

Việc triển khai có lỗi giả định tất cả các điểm đều khác biệt có thể vô tình bỏ qua các so sánh hợp lệ hoặc xử lý sai việc lập chỉ mục. 

Định dạng đầu ra dấu phẩy động là một nguồn vấn đề phổ biến khác. Thẩm phán chấp nhận một dung sai lỗi nhỏ, do đó việc in với độ chính xác dấu phẩy động tiêu chuẩn là đủ, nhưng phép chia hoặc cắt bớt số nguyên sẽ không thành công. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. Chúng tôi kiểm tra từng cặp điểm, tính khoảng cách Euclide giữa chúng và giữ giá trị lớn nhất được thấy cho đến nay. 

Điều này có tác dụng vì câu trả lời được xác định theo nghĩa đen là giá trị lớn nhất trên tất cả các cặp. Nếu chúng ta kiểm tra từng cặp chính xác một lần, chúng ta không thể bỏ lỡ cặp tối ưu. 

Thuật toán brute-force thực hiện:$O(n^2)$các phép tính khoảng cách. Với`n = 50`, điều đó có nghĩa là nhiều nhất là 1225 cặp, không đáng kể. 

Đối với những ràng buộc lớn hơn, cách tiếp cận này sẽ trở thành vấn đề. Nếu như`n`đã từng$10^5$, việc kiểm tra theo cặp sẽ yêu cầu khoảng$10^{10}$hoạt động, đó là không thể trong thời gian giới hạn. Trong những cài đặt đó, các kỹ thuật hình học tính toán như bao lồi và thước cặp quay trở nên cần thiết vì cặp xa nhất phải nằm trên bao lồi. 

Vấn đề này không yêu cầu những tối ưu hóa đó. Các ràng buộc được cố ý đủ nhỏ để giải pháp cặp đôi đơn giản đã tối ưu trong thực tế. Quan sát quan trọng là việc cố gắng xây dựng một thuật toán hình học tiên tiến hơn sẽ chỉ làm tăng thêm độ phức tạp mà không mang lại bất kỳ lợi ích nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu hóa hình học | O(n log n) | O(n) | Không cần thiết | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. Đọc tất cả các điểm vào danh sách`(x, y)`cặp. 
3. Khởi tạo một biến`best = 0.0`. 
4. Lặp lại từng cặp chỉ số`(i, j)`với`i < j`. 
5. Với mỗi cặp, hãy tính hiệu tọa độ:$dx=x_i-x_j$

$dy=y_i-y_j$6. Tính khoảng cách Euclide:$d=\sqrt{dx^2+dy^2}$7. Cập nhật`best`nếu khoảng cách này lớn hơn. 
8. Sau khi tất cả các cặp được xử lý, hãy in`best`. 

### Tại sao nó hoạt động 

Thuật toán đánh giá rõ ràng khoảng cách cho mọi cặp điểm có thể. Vì định nghĩa của câu trả lời là khoảng cách tối đa giữa tất cả các cặp nên cặp tối ưu được đảm bảo xuất hiện trong quá trình lặp. Bất cứ khi nào một khoảng cách lớn hơn được tìm thấy,`best`được cập nhật. Sau khi vòng lặp kết thúc,`best`bằng khoảng cách tối đa trên toàn bộ các cặp, đây chính xác là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

# solution
def solve():
    n = int(input())
    
    points = []
    for _ in range(n):
        x = int(input())
        y = int(input())
        points.append((x, y))
    
    best = 0.0
    
    for i in range(n):
        x1, y1 = points[i]
        
        for j in range(i + 1, n):
            x2, y2 = points[j]
            
            dx = x1 - x2
            dy = y1 - y2
            
            dist = math.sqrt(dx * dx + dy * dy)
            best = max(best, dist)
    
    print(best)

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc tất cả các điểm vào một danh sách. Vì tọa độ được chia thành các dòng riêng biệt nên chúng ta đọc hai số nguyên cho mỗi điểm và lưu trữ chúng cùng nhau dưới dạng một bộ dữ liệu. 

Các vòng lặp lồng nhau liệt kê từng cặp không có thứ tự chính xác một lần. sử dụng`j`từ`i + 1`tránh so sánh trùng lặp như`(A,B)`Và`(B,A)`và cũng bỏ qua việc so sánh một điểm với chính nó. 

Tính toán khoảng cách sử dụng hiệu bình phương trước khi lấy căn bậc hai. Các tọa độ đủ nhỏ nên không thể tràn trong Python, nhưng việc sử dụng trực tiếp các số hạng bình phương vẫn tránh được các phép toán dấu phẩy động không cần thiết cho đến bước cuối cùng. 

Biến trả lời bắt đầu tại`0.0`vì khoảng cách không âm. Mỗi khoảng cách tính toán được so sánh với mức tối đa hiện tại. 

Câu lệnh in cuối cùng sử dụng định dạng dấu phẩy động mặc định của Python, định dạng này đã cung cấp đủ độ chính xác cho yêu cầu về dung sai của thẩm phán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0
1
2
3
4
5
```Các điểm là:```
(0,1), (2,3), (4,5)
```| tôi | j | Điểm A | Điểm B | Khoảng cách | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | (0,1) | (2,3) | 2.828427 | 2.828427 | 
| 0 | 2 | (0,1) | (4,5) | 5.656854 | 5.656854 | 
| 1 | 2 | (2,3) | (4,5) | 2.828427 | 5.656854 | 

Cặp xa nhất là`(0,1)`Và`(4,5)`. Dấu vết cho thấy rằng một khi đã tìm thấy mức tối đa, các phép so sánh sau này không thể ghi đè lên nó trừ khi chúng lớn hơn. 

### Ví dụ 2 

đầu vào:```
3
1
1
1
1
4
5
```Các điểm là:```
(1,1), (1,1), (4,5)
```| tôi | j | Điểm A | Điểm B | Khoảng cách | tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | (1,1) | (1,1) | 0,000000 | 0,000000 | 
| 0 | 2 | (1,1) | (4,5) | 5.000000 | 5.000000 | 
| 1 | 2 | (1,1) | (4,5) | 5.000000 | 5.000000 | 

Ví dụ này chứng minh rằng các điểm trùng lặp không gây ra bất kỳ vấn đề nào. Khoảng cách theo cặp của chúng đơn giản bằng 0 và thuật toán vẫn xác định chính xác cặp xa nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi cặp điểm được kiểm tra một lần | 
| Không gian | O(n) | Danh sách điểm được lưu trữ | 

Với`n ≤ 50`, thuật toán thực hiện tối đa 1225 phép tính khoảng cách. Đó là rất nhỏ so với các giới hạn có sẵn, vì vậy giải pháp phù hợp một cách thoải mái với cả những hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import math

def solve():
    input = sys.stdin.readline

    n = int(input())

    points = []
    for _ in range(n):
        x = int(input())
        y = int(input())
        points.append((x, y))

    best = 0.0

    for i in range(n):
        x1, y1 = points[i]

        for j in range(i + 1, n):
            x2, y2 = points[j]

            dx = x1 - x2
            dy = y1 - y2

            dist = math.sqrt(dx * dx + dy * dy)
            best = max(best, dist)

    print(best)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
ans = float(run(
"""3
0
1
2
3
4
5
"""))
assert abs(ans - 5.656854249) < 1e-6, "sample 1"

# minimum size
ans = float(run(
"""2
0
0
3
4
"""))
assert abs(ans - 5.0) < 1e-6, "minimum case"

# all points equal
ans = float(run(
"""4
2
2
2
2
2
2
2
2
"""))
assert abs(ans - 0.0) < 1e-6, "all equal"

# boundary coordinates
ans = float(run(
"""2
-50
-50
50
50
"""))
assert abs(ans - 141.421356237) < 1e-6, "boundary coordinates"

# off-by-one style check
ans = float(run(
"""3
0
0
1
1
10
10
"""))
assert abs(ans - 14.1421356237) < 1e-6, "largest pair at end"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai điểm`(0,0)`Và`(3,4)`|`5`| Kích thước đầu vào hợp lệ tối thiểu | 
| Tất cả các điểm`(2,2)`|`0`| Điểm trùng lặp và khoảng cách bằng 0 | 
|`(-50,-50)`Và`(50,50)`|`141.421356...`| Tọa độ ranh giới | 
| Cặp lớn nhất xuất hiện cuối cùng |`14.142135...`| Lặp lại cặp đúng | 

## Vỏ cạnh 

Xem xét các điểm trùng lặp:```
3
5
5
5
5
8
9
```Thuật toán so sánh hai điểm đầu tiên và tính khoảng cách`0`. Điều đó không ảnh hưởng đến tính đúng đắn vì`best`vẫn là mức tối đa được thấy cho đến nay. Khi`(5,5)`Và`(8,9)`được kiểm tra, khoảng cách trở thành:$\sqrt{3^2+4^2}=5$Câu trả lời cuối cùng là`5`. 

Bây giờ hãy xem xét tọa độ biên:```
2
-50
-50
50
50
```Thuật toán tính toán:$dx=-100$

$dy=-100$và sau đó:$\sqrt{100^2+100^2}=141.421356\ldots$Giới hạn tọa độ nhỏ nên việc bình phương các giá trị là hoàn toàn an toàn. 

Cuối cùng, hãy xem xét trường hợp cặp tối ưu xuất hiện muộn theo thứ tự lặp:```
3
0
0
1
1
10
10
```Sự so sánh đầu tiên cho khoảng cách về`1.414`. Việc triển khai có lỗi dừng sớm hoặc khởi tạo không chính xác có thể thất bại ở đây. Thuật toán tiếp tục đi qua tất cả các cặp và cuối cùng kiểm tra`(0,0)`với`(10,10)`, tạo ra mức tối đa chính xác`14.142135...`.
