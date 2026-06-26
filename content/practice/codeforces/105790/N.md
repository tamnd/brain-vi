---
title: "CF 105790N - Tấm chắn điều hướng"
description: "Chúng ta có lưới $N nhân M$. Mỗi khi một lá chắn được xây dựng ở vị trí $(x,y)$, nó sẽ bảo vệ mọi ô trong hàng $x$ và mọi ô trong cột $y$. Một ô có thể sử dụng được nếu nó được bảo vệ bởi ít nhất một lá chắn. Có hai loại hoạt động. Hoạt động loại 1 xây dựng một lá chắn mới."
date: "2026-06-26T03:51:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105790
codeforces_index: "N"
codeforces_contest_name: "UDESC Selection Contest 2024-1"
rating: 0
weight: 105790
solve_time_s: 51
verified: true
draft: false
---

[CF 105790N - Điều hướng tấm chắn](https://codeforces.com/problemset/problem/105790/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một$N \times M$lưới. Mỗi khi một lá chắn được xây dựng ở vị trí$(x,y)$, nó bảo vệ mọi ô trong hàng$x$và mọi ô trong cột$y$. Một ô có thể sử dụng được nếu nó được bảo vệ bởi ít nhất một lá chắn. 

Có hai loại hoạt động. 

Hoạt động loại 1 xây dựng một lá chắn mới. 

Thao tác loại 2 hỏi liệu Almeiding có bắt đầu từ$(x_i,y_i)$có thể đạt được$(x_f,y_f)$chỉ sử dụng các ô được bảo vệ và di chuyển từng bước một theo bốn hướng chính. 

Bản thân lưới có thể chứa tới$10^6$các ô, trong khi số lượng truy vấn lớn bằng$2 \cdot 10^5$. Bất kỳ giải pháp nào cố gắng duy trì trạng thái của mọi ô được bảo vệ hoặc thực hiện tìm kiếm biểu đồ cho từng truy vấn đều quá chậm. Ngay cả một BFS duy nhất trên một lưới kích thước$10^6$sẽ rất tốn kém và việc làm điều đó nhiều lần là không thể. Chúng tôi cần một$O(1)$hoặc$O(\log N)$câu trả lời cho mỗi truy vấn. 

Mối nguy hiểm chính là hiểu sai cấu trúc kết nối do các tấm chắn tạo ra. 

Hãy xem xét đầu vào này:```
3 3 2
1 2 2
2 2 1 2 3
```Khiên kích hoạt hàng 2 và cột 2. Cả hai ô$(2,1)$Và$(2,3)$nằm trên hàng được bảo vệ nên đáp án là:```
SIM
```Việc triển khai đơn giản chỉ kiểm tra xem các điểm cuối có nằm trên cùng hàng hoặc cột được bảo vệ hay không sẽ không thành công trong các trường hợp chung hơn. 

Một trường hợp thú vị khác là:```
3 3 2
1 2 2
2 1 2 3 2
```Ô bắt đầu và ô kết thúc đều được bảo vệ vì chúng thuộc cột 2. Câu trả lời đúng là:```
SIM
```Mặc dù các ô nằm ở các hàng khác nhau nhưng toàn bộ cột vẫn được bảo vệ. 

Cuối cùng:```
3 3 2
1 2 2
2 1 1 3 3
```Không có điểm cuối nào được bảo vệ. Câu trả lời đúng là:```
NAO
```Một đường dẫn thậm chí không thể bắt đầu hoặc kết thúc trên một ô không được bảo vệ. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ đánh dấu rõ ràng mọi ô được bảo vệ và đối với mỗi truy vấn loại 2, hãy chạy BFS hoặc DFS giữa hai điểm cuối. Điều này đúng vì nó mô phỏng trực tiếp chuyển động trong vùng được bảo vệ. 

Vấn đề là quy mô. Một lá chắn duy nhất có thể ảnh hưởng đến toàn bộ một hàng và toàn bộ cột. Với tối đa$10^6$tế bào và$2 \cdot 10^5$truy vấn, khám phá liên tục lưới vượt xa giới hạn. 

Quan sát quan trọng xuất phát từ việc hiểu những gì một tấm khiên thực sự tạo ra. 

Khi một lá chắn được xây dựng tại$(x,y)$, hàng ngang$x$trở nên được bảo vệ hoàn toàn và cột$y$trở nên được bảo vệ hoàn toàn. Ô giao nhau của chúng$(x,y)$nối hàng và cột với nhau. 

Vì mỗi tấm chắn luôn kích hoạt cả một hàng và một cột nên mọi cấu trúc được bảo vệ đều chứa ít nhất một giao điểm hàng-cột. Bất kỳ ô được bảo vệ nào đều thuộc về một hàng được kích hoạt hoặc một cột được kích hoạt. Từ một ô như vậy, chúng ta có thể di chuyển đến một giao lộ và từ đó đến bất kỳ hàng hoặc cột được bảo vệ nào khác được kích hoạt bằng các tấm chắn. Kết quả là tất cả các ô được bảo vệ đều thuộc về một thành phần được kết nối duy nhất. 

Điều đó hoàn toàn thay đổi vấn đề. 

Truy vấn loại 2 hoàn toàn không yêu cầu kiểm tra kết nối. Chúng tôi chỉ cần xác minh rằng cả hai điểm cuối đều được bảo vệ. Nếu cả hai đều được bảo vệ, chúng sẽ tự động thuộc về thành phần được bảo vệ duy nhất và có thể truy cập được từ nhau. Nếu một trong hai điểm cuối không được bảo vệ thì câu trả lời là không thể. 

Chúng ta có thể duy trì hai mảng boolean: 

Một mảng ghi lại những hàng nào đã từng xuất hiện dưới dạng tấm chắn ở giữa. 

Các ghi chép khác về cột nào đã từng xuất hiện dưới dạng tấm chắn ở giữa. 

Một tế bào$(r,c)$được bảo vệ chính xác khi hàng$r$đang hoạt động hoặc cột$c$đang hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS Brute Force cho mỗi truy vấn | O(Q · N · M) trường hợp xấu nhất | O(N · M) | Quá chậm | 
| Theo dõi hàng/cột tối ưu | O(Q) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng boolean`row`kích thước`N`và một mảng boolean`col`kích thước`M`. 
2. Đối với truy vấn loại 1`(x, y)`, đánh dấu`row[x] = True`Và`col[y] = True`. 
3. Đối với truy vấn loại 2, hãy xác định xem ô bắt đầu có được bảo vệ hay không. 

Một tế bào`(r, c)`được bảo vệ nếu`row[r]`đang hoạt động hoặc`col[c]`đang hoạt động. 
4. Xác định xem ô đích có được bảo vệ bằng quy tắc tương tự hay không. 
5. Nếu cả hai ô đều được bảo vệ, hãy in`"SIM"`. 

Mọi ô được bảo vệ đều thuộc cùng một vùng được bảo vệ được kết nối. 
6. Nếu không thì in`"NAO"`. 

### Tại sao nó hoạt động 

Mỗi lá chắn kích hoạt toàn bộ một hàng và toàn bộ cột. Hai cấu trúc đó giao nhau ở tâm tấm chắn, tạo ra sự kết nối giữa chúng. 

Bất kỳ ô được bảo vệ nào đều nằm trên một hàng hiện hoạt hoặc trên một cột hiện hoạt. Từ ô đó, chúng ta có thể di chuyển dọc theo hàng hoặc cột của nó đến giao điểm hình khiên. Thông qua các nút giao, mỗi hàng và cột được bảo vệ sẽ trở thành một phần của một cấu trúc được kết nối. Vì vậy, tất cả các ô được bảo vệ đều có thể truy cập được lẫn nhau. 

Lý do duy nhất khiến truy vấn có thể thất bại là ô bắt đầu hoặc ô đích không được bảo vệ. Đó chính xác là những gì thuật toán kiểm tra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, q = map(int, input().split())

rows = [False] * n
cols = [False] * m

ans = []

for _ in range(q):
    data = list(map(int, input().split()))

    if data[0] == 1:
        _, x, y = data
        rows[x - 1] = True
        cols[y - 1] = True
    else:
        _, xi, yi, xf, yf = data

        start_ok = rows[xi - 1] or cols[yi - 1]
        finish_ok = rows[xf - 1] or cols[yf - 1]

        ans.append("SIM" if start_ok and finish_ok else "NAO")

sys.stdout.write("\n".join(ans))
```Hai mảng là toàn bộ trạng thái của vấn đề. 

Khi tạo một tấm chắn, chúng tôi không đánh dấu tất cả các ô trong hàng hoặc cột của nó. Làm như vậy sẽ rất tốn kém. Thay vào đó, chúng ta chỉ nhớ rằng hàng và cột đang hoạt động. 

Đối với một ô truy vấn`(r,c)`, điều kiện bảo vệ chính xác là:```
rows[r] or cols[c]
```Điều này trực tiếp phù hợp với định nghĩa về bảo vệ. 

Việc triển khai sử dụng tính năng lập chỉ mục dựa trên 0 trong nội bộ, do đó, mỗi tọa độ đọc từ đầu vào sẽ giảm đi một trước khi truy cập vào các mảng. 

Không cần xây dựng biểu đồ, BFS, DFS hoặc cấu trúc tìm liên kết vì khả năng kết nối đã được ngụ ý bởi hình dạng của các tấm chắn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3 3
1 2 2
2 2 1 2 3
2 1 1 3 3
```Sự phát triển của trạng thái: 

| Bước | Hoạt động | Hàng hoạt động | Col hoạt động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | Xây dựng (2,2) | {2} | {2} | - | 
| 2 | Truy vấn (2,1) → (2,3) | {2} | {2} | SIM | 
| 3 | Truy vấn (1,1) → (3,3) | {2} | {2} | NAO | 

Đối với truy vấn đầu tiên, cả hai ô đều nằm trên hàng 2, đang hoạt động. Đối với truy vấn thứ hai, cả điểm cuối đều không được bảo vệ. 

### Ví dụ 2 

đầu vào:```
3 3 4
2 1 1 3 3
1 1 1
2 1 1 3 3
2 3 1 1 3
```Sự phát triển của trạng thái: 

| Bước | Hoạt động | Hàng hoạt động | Col hoạt động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | Truy vấn (1,1) → (3,3) | {} | {} | NAO | 
| 2 | Xây dựng (1,1) | {1} | {1} | - | 
| 3 | Truy vấn (1,1) → (3,3) | {1} | {1} | NAO | 
| 4 | Truy vấn (3,1) → (1,3) | {1} | {1} | SIM | 

Truy vấn cuối cùng thành công vì cả hai ô đều thuộc cột hoạt động 1 hoặc hàng hoạt động 1, đặt chúng bên trong thành phần được bảo vệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q) | Mỗi truy vấn chỉ thực hiện một vài truy cập mảng | 
| Không gian | O(N + M) | Một mảng boolean cho hàng và một mảng cho cột | 

Từ$Q \le 2 \cdot 10^5$Và$N \cdot M \le 10^6$, việc xử lý tuyến tính các truy vấn đủ nhanh. Không có hoạt động nào phụ thuộc vào kích thước của lưới ngoài hai mảng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m, q = map(int, input().split())
    rows = [False] * n
    cols = [False] * m

    out = []

    for _ in range(q):
        a = list(map(int, input().split()))

        if a[0] == 1:
            rows[a[1] - 1] = True
            cols[a[2] - 1] = True
        else:
            s = rows[a[1] - 1] or cols[a[2] - 1]
            t = rows[a[3] - 1] or cols[a[4] - 1]
            out.append("SIM" if s and t else "NAO")

    return "\n".join(out)

# sample 1
assert run(
"""3 3 3
1 2 2
2 2 1 2 3
2 1 1 3 3
"""
) == "SIM\nNAO"

# sample 2
assert run(
"""3 3 4
2 1 1 3 3
1 1 1
2 1 1 3 3
2 3 1 1 3
"""
) == "NAO\nNAO\nSIM"

# minimum grid, no shield
assert run(
"""1 1 1
2 1 1 1 1
"""
) == "NAO"

# single shield, same cell
assert run(
"""1 1 2
1 1 1
2 1 1 1 1
"""
) == "SIM"

# protected through row
assert run(
"""4 4 2
1 3 2
2 3 1 3 4
"""
) == "SIM"

# one endpoint protected, one not
assert run(
"""4 4 2
1 2 2
2 2 1 4 4
"""
) == "NAO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1, không có tấm chắn | NAO | Bắt đầu/kết thúc không được bảo vệ | 
| Lưới 1×1, có tấm chắn | SIM | Trường hợp tích cực nhỏ nhất | 
| Cùng một hàng hoạt động | SIM | Logic bảo vệ hàng | 
| Một điểm cuối được bảo vệ | NAO | Cả hai điểm cuối đều phải được bảo vệ | 
| Mẫu 1 | SIM, NAO | Hành vi chính thức | 
| Mẫu 2 | NAO, NAO, SIM | Kết nối sau khi kích hoạt | 

## Vỏ cạnh 

Hãy xem xét:```
1 1 1
2 1 1 1 1
```Không có lá chắn nào được xây dựng. Hàng không hoạt động và cột không hoạt động. Thuật toán đánh giá:```
rows[1] OR cols[1] = false
```cho cả hai điểm cuối, tạo ra:```
NAO
```điều này đúng vì ô duy nhất không được bảo vệ. 

Bây giờ hãy xem xét:```
3 3 2
1 2 2
2 1 2 3 2
```Sau khi xây dựng lá chắn, hàng 2 và cột 2 sẽ hoạt động. Cả hai điểm cuối đều nằm trên cột 2 nên cả hai đều được bảo vệ. Thuật toán trả về:```
SIM
```Mặc dù các hàng khác nhau nhưng vẫn có thể di chuyển dọc theo cột đang hoạt động. 

Cuối cùng:```
4 4 2
1 2 2
2 2 1 4 4
```Ô bắt đầu`(2,1)`được bảo vệ vì hàng 2 đang hoạt động. Điểm đến`(4,4)`không được bảo vệ vì cả hàng 4 và cột 4 đều không hoạt động. Thuật toán phát hiện điều này ngay lập tức và in:```
NAO
```Không có đường dẫn nào có thể kết thúc ở ô không được bảo vệ, vì vậy câu trả lời là đúng.
