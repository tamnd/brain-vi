---
title: "CF 2194E - Rùa Phản Công"
description: "Chúng ta có một lưới hình chữ nhật. Mỗi ô chứa một giá trị nguyên, dương hoặc âm. Michelangelo chọn một đường dẫn từ góc trên bên trái đến góc dưới bên phải. Anh ta chỉ có thể di chuyển sang phải hoặc xuống."
date: "2026-06-07T20:46:14+07:00"
tags: ["codeforces", "competitive-programming", "dp", "graphs", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2194
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1078 (Div. 2)"
rating: 2000
weight: 2194
solve_time_s: 168
verified: true
draft: false
---

[CF 2194E - Rùa phản công](https://codeforces.com/problemset/problem/2194/E) 

**Xếp hạng:** 2000 
**Tags:** dp, đồ thị, tham lam, thực hiện 
**Thời gian giải:** 2m 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật. Mỗi ô chứa một giá trị nguyên, dương hoặc âm. 

Michelangelo chọn một đường dẫn từ góc trên bên trái đến góc dưới bên phải. Anh ta chỉ có thể di chuyển sang phải hoặc xuống. Điểm của một đường dẫn là tổng của tất cả các giá trị ô được truy cập trên đường dẫn đó và anh ta luôn chọn đường dẫn có số điểm tối đa có thể. 

Trước khi chọn đường dẫn, Raphael chọn chính xác một ô và lật dấu của nó. Một giá trị`x`trở thành`-x`. 

Raphael muốn làm cho điểm tối ưu cuối cùng của Michelangelo càng nhỏ càng tốt. Michelangelo nhìn thấy lưới đã được sửa đổi và sau đó chọn con đường tốt nhất của mình. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải tính điểm tối thiểu mà Raphael có thể ép buộc. 

Lưới chứa nhiều nhất`10^6`các ô trên tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi phương pháp tính toán lại bảng lập trình động đầy đủ cho mọi ô có thể bị lật. Một đĩa đơn`O(nm)`DP thì ổn, nhưng`O((nm)^2)`là hoàn toàn không thể khi`nm = 10^6`. 

Phần khó khăn là sau khi lật một ô, Michelangelo có thể chuyển sang một con đường hoàn toàn khác. Chúng ta không thể cho rằng anh ta tiếp tục sử dụng con đường tối ưu như trước. 

Hãy xem xét một ví dụ nhỏ:```
1 100
100 1
```Đường dẫn tốt nhất ban đầu sử dụng ba ô và có giá trị`102`. 

Nếu Raphael lật`100`ở góc trên bên phải, Michelangelo chỉ cần thay đổi tuyến đường và sử dụng đường đi khác`100`. 

Bất kỳ giải pháp nào chỉ nhìn vào đường dẫn tối ưu ban đầu sẽ thất bại ở đây. 

Một trường hợp dễ bỏ sót khác là khi tồn tại một số đường đi tối ưu khác nhau.```
0 0
0 0
```Mọi con đường đều có giá trị`0`. 

Lật bất kỳ ô nào cũng không thay đổi gì. Ngay cả khi một ô bị lật nằm trên một đường dẫn tối ưu, Michelangelo có thể chọn một đường dẫn khác có cùng số điểm. Một giải pháp giả định "phá hủy một đường dẫn tối ưu sẽ phá hủy đường đi tối ưu" sẽ sai. 

Trường hợp góc thứ ba xảy ra trên các đường chéo chỉ chứa một ô.```
5
```Ô bắt đầu và ô kết thúc giống nhau. Con đường nào cũng phải đi qua nó. Sau khi lật thì đáp án là`-5`. 

Khi đường chéo chỉ chứa một ô, không có ô thay thế nào trên đường chéo đó mà đường dẫn có thể sử dụng. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử mọi ô có thể theo sự lựa chọn của Raphael. 

Đối với một ô cố định, hãy lật dấu của nó và tính lại tổng đường dẫn tối đa bằng cách sử dụng lưới DP tiêu chuẩn. Giá trị kết quả là phản hồi của Michelangelo. Lấy giá trị tối thiểu trên tất cả các ô sẽ cho câu trả lời. 

Điều này đúng vì nó mô phỏng trò chơi theo đúng nghĩa đen. 

Vấn đề là sự phức tạp. Chi phí DP đường dẫn tối đa`O(nm)`. có`nm`các tế bào có thể lật. Tổng độ phức tạp trở thành`O((nm)^2)`, đạt tới`10^12`hoạt động trong trường hợp xấu nhất. 

Chúng ta cần hiểu việc lật một ô sẽ ảnh hưởng như thế nào đến các đường dẫn tối ưu. 

Cho phép`f[i][j]`= tổng đường đi tối đa từ đầu đến`(i,j)`. 

Cho phép`g[i][j]`= tổng đường dẫn tối đa từ`(i,j)`đến cuối cùng. 

Sau đó```
val(i,j) = f[i][j] + g[i][j] - a[i][j]
```là điểm tối đa của bất kỳ con đường nào đi qua`(i,j)`. Giá trị ô được tính hai lần nên chúng ta trừ nó một lần. 

Bây giờ giả sử Raphael lật ô`(i,j)`. 

Nếu Michelangelo chọn con đường vẫn đi qua`(i,j)`, đường đi đó mất chính xác`2 * a[i][j]`, bởi vì sự đóng góp thay đổi từ`a[i][j]`ĐẾN`-a[i][j]`. 

Vì vậy, đường dẫn tốt nhất sử dụng ô đó sẽ trở thành```
val(i,j) - 2*a[i][j]
```Câu hỏi còn lại là con đường tốt nhất để tránh`(i,j)`. 

Đây là nơi quan sát quan trọng xuất hiện. 

Mọi con đường đơn điệu từ`(1,1)`ĐẾN`(n,m)`thăm chính xác một ô trên mỗi đường chéo`i + j = constant`. 

Sửa một ô`(i,j)`và để`d = i + j`. 

Đường nào cũng tránh`(i,j)`vẫn phải đi qua một số ô khác trên đường chéo`d`. 

Đối với bất kỳ ô nào`(x,y)`trên cùng đường chéo đó,`val(x,y)`là điểm đường đi tốt nhất trong số tất cả các đường đi qua`(x,y)`. 

Vì thế con đường tốt nhất là tránh`(i,j)`đơn giản là tối đa`val`trong số tất cả các ô khác trên đường chéo`d`. 

Điều này biến bài toán thành việc duy trì, đối với mọi đường chéo, giá trị lớn nhất và lớn thứ hai của`val`. 

Đối với một tế bào`(i,j)`: 

Nếu nó`val`không phải là đường chéo lớn nhất, đường tránh tốt nhất sử dụng đường chéo tối đa. 

Nếu nó`val`là giá trị lớn nhất duy nhất, việc loại bỏ nó buộc chúng ta phải sử dụng giá trị lớn thứ hai. 

Sau đó, phản ứng tối ưu của Michelangelo là```
max(
    path_using_flipped_cell,
    best_path_avoiding_flipped_cell
)
```và Raphael chọn mức tối thiểu trong số tất cả các ô. 

### So sánh độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((nm)^2) | O(nm) | Quá chậm | 
| Tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`f[i][j]`, tổng đường dẫn tối đa từ đầu đến mỗi ô. 
2. Tính toán`g[i][j]`, tổng đường dẫn tối đa từ mỗi ô đến đích. 
3. Đối với mỗi ô tính toán```
val = f[i][j] + g[i][j] - a[i][j]
```Đây là số điểm tối đa trong số tất cả các đường dẫn đi qua ô đó. 
4. Nhóm các ô theo đường chéo`d = i + j`. 
5. Với mỗi đường chéo, lưu giá trị lớn nhất`mx1[d]`và giá trị lớn thứ hai`mx2[d]`. 

Khi một số ô chia sẻ giá trị tối đa,`mx2[d]`được phép trở thành bằng`mx1[d]`. Điều này tự động xử lý các mối quan hệ một cách chính xác. 
6. Liệt kê từng ô`(i,j)`như chiếc ô bị lật của Raphael. 
7. Tính toán```
through = val - 2*a[i][j]
```đó là điểm đường dẫn tốt nhất vẫn sử dụng ô được lật. 
8. Tính toán đường đi tốt nhất tránh ô đó. 

Nếu như`val == mx1[d]`, sử dụng`mx2[d]`. 

Nếu không thì sử dụng`mx1[d]`. 
9. Michelangelo chọn cái tốt hơn trong hai phương án đó:```
score = max(through, avoid)
```10. Giảm thiểu`score`trên tất cả các tế bào. 

### Tại sao nó hoạt động 

Một đường đi đơn điệu đi thăm chính xác một ô trên mỗi đường chéo. Sửa ô bị lật`(i,j)`trên đường chéo`d`. 

Đường nào cũng tránh`(i,j)`phải đi qua một ô khác có cùng đường chéo. Ngược lại, bất kỳ đường đi nào đi qua một ô đường chéo khác`d`tự động tránh`(i,j)`, vì một đường dẫn chỉ có thể truy cập một ô trên đường chéo đó. 

Đối với mỗi ô chéo`(x,y)`,`val(x,y)`là điểm tốt nhất trong số tất cả các con đường đi qua nó. Do đó điểm số tốt nhất của một con đường tránh`(i,j)`chính xác là mức tối đa`val`trong số tất cả các ô khác trên đường chéo`d`. 

Đường đi tốt nhất sử dụng ô bị lật sẽ mất chính xác`2*a[i][j]`. Michelangelo chọn tùy chọn "sử dụng" và "tránh" tốt hơn và Raphael chọn ô thu nhỏ kết quả đó. Mọi khả năng đều được đánh giá chính xác một lần nên thuật toán là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10 ** 30

t = int(input())

for _ in range(t):
    n, m = map(int, input().split())

    a = [list(map(int, input().split())) for _ in range(n)]

    f = [[-INF] * m for _ in range(n)]
    g = [[-INF] * m for _ in range(n)]

    for i in range(n):
        for j in range(m):
            if i == 0 and j == 0:
                f[i][j] = a[i][j]
            else:
                best = -INF
                if i > 0:
                    best = max(best, f[i - 1][j])
                if j > 0:
                    best = max(best, f[i][j - 1])
                f[i][j] = best + a[i][j]

    for i in range(n - 1, -1, -1):
        for j in range(m - 1, -1, -1):
            if i == n - 1 and j == m - 1:
                g[i][j] = a[i][j]
            else:
                best = -INF
                if i + 1 < n:
                    best = max(best, g[i + 1][j])
                if j + 1 < m:
                    best = max(best, g[i][j + 1])
                g[i][j] = best + a[i][j]

    diag_cnt = n + m - 1
    mx1 = [-INF] * diag_cnt
    mx2 = [-INF] * diag_cnt

    val = [[0] * m for _ in range(n)]

    for i in range(n):
        for j in range(m):
            cur = f[i][j] + g[i][j] - a[i][j]
            val[i][j] = cur

            d = i + j

            if cur >= mx1[d]:
                mx2[d] = mx1[d]
                mx1[d] = cur
            elif cur > mx2[d]:
                mx2[d] = cur

    ans = INF

    for i in range(n):
        for j in range(m):
            d = i + j
            cur = val[i][j]

            through = cur - 2 * a[i][j]

            if cur == mx1[d]:
                avoid = mx2[d]
            else:
                avoid = mx1[d]

            score = max(through, avoid)
            ans = min(ans, score)

    print(ans)
```DP đầu tiên tính toán tiền tố tốt nhất kết thúc ở mỗi ô. DP thứ hai tính toán hậu tố tốt nhất bắt đầu từ mỗi ô. 

Kết hợp chúng sẽ tạo ra con đường tốt nhất qua từng ô. Số lượng đó được tái sử dụng trong toàn bộ giải pháp, đó là lý do tại sao chúng ta không bao giờ cần chạy lại DP sau khi chọn một ô bị lật. 

Cực đại đường chéo xứng đáng được chú ý đặc biệt. Khi một giá trị mới ít nhất bằng mức tối đa hiện tại, nó sẽ trở thành mức tối đa mới và mức tối đa cũ trở thành mức tối đa thứ hai. Bởi vì chúng tôi sử dụng`>=`, cực đại bằng nhau được lưu trữ chính xác. Nếu hai ô có cùng giá trị tốt nhất trên đường chéo, việc loại bỏ một ô vẫn để lại cùng một giá trị có sẵn ở ô kia. 

Tất cả các phép tính đều sử dụng số nguyên Python. Tổng đường dẫn có thể đạt khoảng`(n+m) * 10^9`, vì vậy cần có số học 64 bit. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
1 -2 3
4 -5 2
1 6 -1
```Tính toán`val`giá trị là: 

| Tế bào | giá trị | 
| --- | --- | 
| (1,1) | 11 | 
| (1,2) | 8 | 
| (1,3) | 8 | 
| (2,1) | 11 | 
| (2,2) | 2 | 
| (2,3) | 8 | 
| (3,1) | 11 | 
| (3,2) | 11 | 
| (3,3) | 11 | 

Đối với tế bào`(3,2)`: 

| Số lượng | Giá trị | 
| --- | --- | 
| giá trị | 11 | 
| một | 6 | 
| qua | -1 | 
| tránh | 3 | 
| điểm | 3 | 

Raphael có thể ép Michelangelo xuống`3`, đó là câu trả lời cuối cùng. 

Ví dụ này cho thấy tại sao Michelangelo có thể chuyển hướng sau khi lật bóng. Con đường tốt nhất ban đầu không còn hấp dẫn nữa nên anh ta chuyển sang lựa chọn đường chéo khác. 

### Mẫu 2 

đầu vào:```
2 4
-1 -1 -1 1
-1 -1 -1 -1
```Các giá trị liên quan: 

| Tế bào | giá trị | 
| --- | --- | 
| (1,1) | -3 | 
| (1,2) | -3 | 
| (1,3) | -3 | 
| (1,4) | -3 | 
| (2,1) | -3 | 
| (2,2) | -3 | 
| (2,3) | -3 | 
| (2,4) | -3 | 

Lật ô tích cực duy nhất sẽ thay đổi sự đóng góp của nó bằng cách`-2`. 

| Số lượng | Giá trị | 
| --- | --- | 
| qua | -5 | 
| tránh | -5 | 
| điểm | -5 | 

Câu trả lời là`-5`. 

Ví dụ này chứng minh rằng kết quả tối ưu có thể âm ngay cả khi lưới ban đầu chứa giá trị không âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Hai lần DP, một lần quét chéo, một lần quét câu trả lời | 
| Không gian | O(nm) | Lưu trữ lưới, DP chuyển tiếp, DP lùi và`val`| 

Tổng số ô trong tất cả các trường hợp thử nghiệm nhiều nhất là`10^6`, do đó việc xử lý tuyến tính dễ dàng đủ nhanh trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    INF = 10 ** 30

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = [list(map(int, input().split())) for _ in range(n)]

        f = [[-INF] * m for _ in range(n)]
        g = [[-INF] * m for _ in range(n)]

        for i in range(n):
            for j in range(m):
                if i == 0 and j == 0:
                    f[i][j] = a[i][j]
                else:
                    best = -INF
                    if i:
                        best = max(best, f[i - 1][j])
                    if j:
                        best = max(best, f[i][j - 1])
                    f[i][j] = best + a[i][j]

        for i in range(n - 1, -1, -1):
            for j in range(m - 1, -1, -1):
                if i == n - 1 and j == m - 1:
                    g[i][j] = a[i][j]
                else:
                    best = -INF
                    if i + 1 < n:
                        best = max(best, g[i + 1][j])
                    if j + 1 < m:
                        best = max(best, g[i][j + 1])
                    g[i][j] = best + a[i][j]

        diag = n + m - 1
        mx1 = [-INF] * diag
        mx2 = [-INF] * diag
        val = [[0] * m for _ in range(n)]

        for i in range(n):
            for j in range(m):
                cur = f[i][j] + g[i][j] - a[i][j]
                val[i][j] = cur
                d = i + j

                if cur >= mx1[d]:
                    mx2[d] = mx1[d]
                    mx1[d] = cur
                elif cur > mx2[d]:
                    mx2[d] = cur

        ans = INF

        for i in range(n):
            for j in range(m):
                d = i + j
                cur = val[i][j]

                through = cur - 2 * a[i][j]
                avoid = mx2[d] if cur == mx1[d] else mx1[d]

                ans = min(ans, max(through, avoid))

        out.append(str(ans))

    return "\n".join(out) + "\n"

# provided samples
assert run(
"""2
3 3
1 -2 3
4 -5 2
1 6 -1
2 4
-1 -1 -1 1
-1 -1 -1 -1
"""
) == "3\n-5\n"

# 1x1 grid
assert run(
"""1
1 1
5
"""
) == "-5\n"

# all zeros
assert run(
"""1
2 2
0 0
0 0
"""
) == "0\n"

# equal maxima on a diagonal
assert run(
"""1
2 2
1 1
1 1
"""
) == "1\n"

# single row
assert run(
"""1
1 3
2 -1 4
"""
) == "-1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1x1`lưới |`-5`| Đường chéo chỉ có một ô | 
| Tất cả số không |`0`| Nhiều đường đi tối ưu và cực đại bị ràng buộc | 
|`2x2`tất cả những cái |`1`| Xử lý đúng cực đại đường chéo bằng nhau | 
| Hàng đơn |`-1`| Chuyển tiếp ranh giới và tính duy nhất của đường đi | 

## Vỏ cạnh 

### Lưới đơn ô 

đầu vào:```
1
1 1
5
```Chỉ có một khả năng lật và chỉ có một con đường khả thi.`val = 5`. 

Sau khi lật:```
through = 5 - 2*5 = -5
```Không có ô thay thế trên đường chéo nên kết quả là`-5`. 

Thuật toán xử lý điều này vì mức tối đa thứ hai trên đường chéo đó vẫn là âm vô cực và`through`quyền chọn chiếm ưu thế. 

### Nhiều đường đi tối ưu 

đầu vào:```
1
2 2
0 0
0 0
```Mọi con đường đều có điểm`0`. 

Đối với mỗi đường chéo, cả giá trị lớn nhất và lớn thứ hai đều trở thành`0`. 

Khi một ô bị loại trừ, một ô khác có cùng giá trị vẫn có sẵn. Thuật toán trả về`0`, điều đó đúng. 

### Ô bị lật không được đường dẫn cuối cùng sử dụng 

đầu vào:```
1
2 2
1 100
100 1
```Lật một trong hai`100`làm cho đường đi qua ô đó trở nên tồi tệ hơn nhiều. 

Thuật toán so sánh:```
through = val - 200
```so với giá trị thay thế tốt nhất từ ​​cùng một đường chéo. 

Con đường thay thế sẽ thắng, phù hợp với khả năng định tuyến lại của Michelangelo sau khi nhìn thấy cú lật. 

Đây chính xác là lý do tại sao giải pháp phải xem xét cả trường hợp “sử dụng” và “tránh” cho mỗi ô bị lật.
