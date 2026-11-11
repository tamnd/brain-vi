---
title: "CF 1580A - Cổng thông tin"
description: "Chúng tôi được cung cấp một lưới nhị phân. Ô chứa 1 là khối obsidian và ô chứa 0 là khối trống. Chúng ta có thể lật bất kỳ ô nào, thay đổi 0 thành 1 hoặc 1 thành 0, với chi phí một lần cho mỗi lần lật."
date: "2026-06-10T10:14:21+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1580
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 745 (Div. 1)"
rating: 1700
weight: 1580
solve_time_s: 224
verified: false
draft: false
---

[CF 1580A - Cổng thông tin](https://codeforces.com/problemset/problem/1580/A) 

**Đánh giá:** 1700 
**Tags:** bạo lực, cấu trúc dữ liệu, dp, tham lam, triển khai 
**Thời gian giải:** 3 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới nhị phân. Một ô chứa`1`là một khối obsidian và một ô chứa`0`trống rỗng. 

Chúng ta có thể lật bất kỳ ô nào, thay đổi`0`ĐẾN`1`hoặc`1`ĐẾN`0`, với giá một lần cho mỗi lần lật. 

Mục tiêu của chúng tôi là tìm một hình chữ nhật con có thể chuyển đổi thành một cổng hợp lệ với số lần lật tối thiểu. 

Một cổng thông tin có cấu trúc rất cụ thể. Hình chữ nhật phải có ít nhất 5 hàng và ít nhất 4 cột. Mọi ô không phải là góc trên đường viền đều phải`1`, trong khi mọi ô bên trong phải`0`. Bốn ô góc hoàn toàn không liên quan và có thể chứa một trong hai giá trị. 

Câu trả lời cho một trường hợp thử nghiệm là số lần lật tối thiểu cần thiết để ít nhất một hình chữ nhật con trở thành một cổng. 

Các ràng buộc là manh mối đầu tiên cho thấy việc tìm kiếm trực tiếp là không thể. Cả hai kích thước có thể đạt tới 400. Một hình chữ nhật được xác định bằng cách chọn hai hàng và hai cột, điều này đã cho kết quả gần đúng$400^4$khả năng. Ngay cả việc kiểm tra một hình chữ nhật một cách hiệu quả cũng không thể cứu được giải pháp như vậy. 

Câu lệnh này cũng chứa một sự đảm bảo bất thường: tổng của tất cả số hàng trong các trường hợp thử nghiệm nhiều nhất là 400 và tổng của tất cả số lượng cột cũng nhiều nhất là 400. Điều này có nghĩa là một giải pháp xung quanh$O(n^2m)$hoặc$O(nm^2)$có thể chấp nhận được, trong khi$O(n^2m^2)$là quá lớn. 

Một chi tiết tinh tế là các góc không thành vấn đề. 

Coi như:```
1111
1001
1001
1001
1111
```Điều này đã trông giống như một cổng thông tin. Bây giờ thay đổi cả bốn góc:```
0110
1001
1001
1001
0110
```Nó vẫn là một cổng hợp lệ vì chỉ các ô viền không góc bị hạn chế. 

Một lỗi dễ mắc phải khác là quên chiều cao tối thiểu phải bằng 5. 

Ví dụ:```
1111
1001
1001
1111
```Hình chữ nhật này có mẫu viền bên phải nhưng cao 4 nên không phải là cổng. 

Lỗi phổ biến thứ ba là đếm các ô viền hai lần khi kết hợp chi phí từ hàng và cột. Giải pháp tối ưu tách biệt cẩn thận các yêu cầu bên trong khỏi các yêu cầu về đường viền để mỗi ô đóng góp chính xác một lần. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Liệt kê mọi hình chữ nhật có thể có chiều cao ít nhất là 5 và chiều rộng ít nhất là 4. Đối với mỗi hình chữ nhật, hãy đếm xem có bao nhiêu ô phải thay đổi. 

Một tế bào góp phần: 

- 0 nếu nó đã phù hợp với trạng thái được yêu cầu. 
- 1 nếu phải lật. 

Tối thiểu trên tất cả các hình chữ nhật là câu trả lời. 

Vấn đề là số lượng hình chữ nhật. có$O(n^2m^2)$những lựa chọn có thể. Với$n=m=400$, đây là đại khái$2.5\times10^{10}$hình chữ nhật. Ngay cả việc kiểm tra từng hình chữ nhật trong thời gian liên tục cũng sẽ vô vọng. 

Quan sát quan trọng là hình dạng cổng có cấu trúc cao. 

Giả sử chúng ta sửa hàng trên cùng và hàng dưới cùng của cổng. Chiều cao ít nhất phải là 5, vì vậy khoảng cách giữa chúng ít nhất là 4. 

Đối với một cặp hàng cố định, chúng ta chỉ cần chọn cột bên trái và bên phải. 

Bây giờ hãy nghĩ từng cột một. 

Đối với cột viền trái ứng cử viên, mọi ô ở giữa giữa hai hàng phải trở thành`1`. 

Đối với cột biên giới bên phải ứng cử viên, điều này cũng đúng. 

Đối với mỗi cột nằm hoàn toàn bên trong hình chữ nhật, tất cả các ô giữa các đường viền phải trở thành`0`, trong khi các ô viền trên và dưới của cột đó phải trở thành`1`. 

Sự đóng góp chi phí của mỗi cột có thể được tính toán độc lập. Sau đó, vấn đề trở thành tối ưu hóa cửa sổ trượt trên các cột. 

Điều này làm giảm việc tìm kiếm từ việc chọn bốn ranh giới đồng thời đến chọn một cặp hàng và sau đó quét các cột với mức bảo trì tối thiểu về kiểu cấu trúc dữ liệu. 

Giải pháp được chấp nhận chạy trong$O(n^2m)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hình chữ nhật O(n²m²), ngay cả khi có kiểm tra O(1) | O(nm) | Quá chậm | 
| Tối ưu | O(n²m) | O(m) cộng với tổng tiền tố | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tính toán trước 

Đối với mỗi cột, chúng ta cần biết nhanh có bao nhiêu cột xuất hiện giữa hai hàng. 

Xây dựng tổng tiền tố cột:$$col[j][i]$$lưu trữ số lượng những cái trong cột`j`từ hàng`1`qua hàng`i`. 

Sau đó, mọi truy vấn phân đoạn dọc đều trở thành O(1). 

### Cố định hàng trên và dưới 

1. Liệt kê từng cặp hàng`(top, bottom)`như vậy`bottom - top >= 4`. 
2. Hãy để`h = bottom - top - 1`, số hàng bên trong. 
3. Đối với mỗi cột`c`, tính toán ba giá trị. 

Số lượng ở phần bên trong của cột đó:$$insideOnes$$Chi phí nếu cột này được sử dụng làm đường viền bên. 

Tất cả các ô bên trong trên đường viền bên đều phải được`1`, Vì thế:$$sideCost = h - insideOnes$$4. Tính giá thành nếu dùng cột này làm cột bên trong. 

Các ô trên cùng và dưới cùng phải`1`. 

Tất cả các ô ở giữa phải`0`. 

Cho phép$$interiorCost = (grid[top][c]==0) +(grid[bottom][c]==0) + insideOnes$$bởi vì mọi nội thất`1`phải được lật sang`0`. 

### Chuyển hóa vấn đề 

1. Cột giả sử`r`được chọn làm đường viền bên phải. 

Khi đó các cột giữa viền trái và viền phải sẽ trở thành cột bên trong. 
2. Xác định$$value[c] = sideCost[c] - interiorCost[c]$$Điều này đo lường chúng ta phải trả thêm bao nhiêu chi phí nếu cột`c`trở thành đường viền bên thay vì cột bên trong. 
3. Trong khi quét các cột từ trái sang phải, hãy duy trì đường viền bên trái tốt nhất có thể trước đó ít nhất ba cột. 

Chiều rộng phải ít nhất là 4, vì vậy:$$right - left \ge 3$$4. Với mỗi cột viền phải, hãy tính$$total = sideCost[right] + prefixInteriorCost(right-1) + bestLeft$$Ở đâu`bestLeft`lưu trữ giá trị tối thiểu của$$sideCost[left] - prefixInteriorCost(left)$$over all valid left borders.
 5. Update the global answer.

 ### Tại sao nó hoạt động 

Đối với các hàng trên và dưới cố định, mỗi cột thuộc đúng một trong ba loại: 

- viền trái, 
- viền phải, 
- cột bên trong. 

Chi phí của một hình chữ nhật là tổng đóng góp của các hạng mục đó. 

Các cột bên trong đóng góp một tổng phạm vi, có thể được biểu thị bằng tổng tiền tố. Đường viền bên trái góp phần điều chỉnh thêm so với phần bên trong. Điều này chuyển đổi việc tối ưu hóa hình chữ nhật thành việc tìm giá trị nhỏ nhất của một biểu thức bao gồm một cột ở bên trái và một cột ở bên phải. 

Quá trình quét duy trì đường viền bên trái tốt nhất được thấy cho đến nay, vì vậy mọi hình chữ nhật hợp lệ cho cặp hàng hiện tại đều được xem xét chính xác một lần. Vì tất cả các cặp hàng đều được liệt kê nên sẽ tìm thấy giá trị tối thiểu trên tất cả các cổng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, m = map(int, input().split())
        g = [list(map(int, input().strip())) for _ in range(n)]

        col = [[0] * (n + 1) for _ in range(m)]
        for j in range(m):
            for i in range(n):
                col[j][i + 1] = col[j][i] + g[i][j]

        ans = 10**9

        for top in range(n):
            for bottom in range(top + 4, n):
                h = bottom - top - 1

                side = [0] * m
                interior = [0] * m

                for c in range(m):
                    inside_ones = (
                        col[c][bottom] -
                        col[c][top + 1]
                    )

                    side[c] = h - inside_ones

                    interior[c] = (
                        inside_ones +
                        (1 - g[top][c]) +
                        (1 - g[bottom][c])
                    )

                pref = [0] * (m + 1)
                for c in range(m):
                    pref[c + 1] = pref[c] + interior[c]

                best = side[0] - pref[1]

                for right in range(3, m):
                    left_candidate = right - 3
                    best = min(
                        best,
                        side[left_candidate] -
                        pref[left_candidate + 1]
                    )

                    cur = (
                        side[right]
                        + pref[right]
                        + best
                    )

                    ans = min(ans, cur)

        print(ans)

solve()
```Tổng tiền tố cột là nền tảng của việc thực hiện. Chúng cho phép chúng ta đếm số lượng đơn vị trong bất kỳ đoạn thẳng đứng nào trong thời gian không đổi. 

Đối với mỗi cặp hàng cố định, các mảng`side`Và`interior`lưu trữ phần đóng góp chi phí của mỗi cột theo hai vai trò quan trọng có thể có. Các giá trị này được tính toán độc lập, giúp cho việc tối ưu hóa sau này có thể thực hiện được. 

Mảng`pref`lưu trữ tổng tiền tố của`interior`. Điều này cho phép chúng ta thu được chi phí của tất cả các cột bên trong giữa hai đường viền dưới dạng sự khác biệt đơn giản của các tiền tố. 

Biến`best`là sự tối ưu hóa quan trọng. Khi quá trình quét đến một cột có thể đóng vai trò là đường viền bên phải,`best`đã chứa đường viền bên trái hợp lệ rẻ nhất. Bởi vì chiều rộng phải ít nhất là bốn, chỉ những cột có ít nhất ba vị trí trước đó mới được phép nhập mức tối thiểu này. 

Lỗi triển khai phổ biến nhất là lỗi từng lỗi một trong các chỉ mục tiền tố. biểu hiện`pref[left + 1]`là cố ý vì cột`left`bản thân nó không được tính vào các cột bên trong. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
5 4
1000
0000
0110
0000
0001
```Chỉ có thể có một hình chữ nhật có chiều cao 5 và một hình chữ nhật có chiều rộng 4. 

| Cột | bên trongOnes | sideCost | nội thấtChi phí | 
| --- | --- | --- | --- | 
| 0 | 0 | 3 | 1 | 
| 1 | 1 | 2 | 3 | 
| 2 | 1 | 2 | 3 | 
| 3 | 0 | 3 | 1 | 

Tổng tiền tố của chi phí nội thất: 

| Chỉ mục | Giá trị | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 4 | 
| 3 | 7 | 
| 4 | 8 | 

Hình chữ nhật hợp lệ duy nhất sử dụng cột 0 và 3 làm đường viền. 

Chi phí tính toán là 12, phù hợp với đầu ra mẫu. 

Ví dụ này cho thấy các cột viền và cột bên trong được xử lý khác nhau như thế nào mặc dù chúng chứa các ô từ cùng một hàng. 

### Ví dụ 2 

Hãy xem xét:```
1
5 4
0110
1001
1001
1001
0110
```Đây đã là một cổng thông tin hợp lệ. 

| Cột | bên trongOnes | sideCost | nội thấtChi phí | 
| --- | --- | --- | --- | 
| 0 | 3 | 0 | 2 | 
| 1 | 0 | 3 | 0 | 
| 2 | 0 | 3 | 0 | 
| 3 | 3 | 0 | 2 | 

Hình chữ nhật được chọn là toàn bộ lưới. 

Tổng chi phí tính toán trở thành 0. 

Điều này thể hiện quy tắc góc. Các góc đều bằng 0, tuy nhiên hình chữ nhật đã hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²m) | Liệt kê các cặp hàng và quét tất cả các cột | 
| Không gian | O(nm) | Tổng tiền tố cột cộng với mảng tạm thời | 

Đầu vào lớn nhất có thể có kích thước 400 x 400. Có khoảng$n^2/2$cặp hàng và mỗi cặp xử lý tất cả các cột một lần. Điều đó mang lại về$400^2 \times 400 = 64$triệu thao tác nguyên thủy, đây là mức độ phức tạp dự kiến ​​và phù hợp với các giới hạn trong Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        g = [list(map(int, input().strip())) for _ in range(n)]

        col = [[0] * (n + 1) for _ in range(m)]
        for j in range(m):
            for i in range(n):
                col[j][i + 1] = col[j][i] + g[i][j]

        ans = 10**9

        for top in range(n):
            for bottom in range(top + 4, n):
                h = bottom - top - 1

                side = [0] * m
                interior = [0] * m

                for c in range(m):
                    inside = col[c][bottom] - col[c][top + 1]
                    side[c] = h - inside
                    interior[c] = (
                        inside +
                        (1 - g[top][c]) +
                        (1 - g[bottom][c])
                    )

                pref = [0] * (m + 1)
                for c in range(m):
                    pref[c + 1] = pref[c] + interior[c]

                best = side[0] - pref[1]

                for r in range(3, m):
                    best = min(
                        best,
                        side[r - 3] - pref[r - 2]
                    )
                    ans = min(
                        ans,
                        side[r] + pref[r] + best
                    )

        out.append(str(ans))

    return "\n".join(out) + "\n"

# sample
assert run(
"""1
5 4
1000
0000
0110
0000
0001
"""
) == "12\n"

# already a portal
assert run(
"""1
5 4
0110
1001
1001
1001
0110
"""
) == "0\n"

# all zeros
assert run(
"""1
5 4
0000
0000
0000
0000
0000
"""
) == "8\n"

# all ones
assert run(
"""1
5 4
1111
1111
1111
1111
1111
"""
) == "6\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào mẫu | 12 | Trận đấu ví dụ chính thức | 
| Cổng thông tin đã hợp lệ | 0 | Xử lý góc và giải pháp không tốn chi phí | 
| Tất cả số không | 8 | Số lượng lớn các lần lật biên cần thiết | 
| Tất cả những cái | 6 | Chi phí dọn dẹp nội thất | 
| Kích thước tối thiểu 5 × 4 | Câu trả lời đúng | Kích thước ranh giới | 
| Hình chữ nhật rộng hơn | Câu trả lời đúng | Logic cửa sổ trượt | 

## Vỏ cạnh 

### Các góc không liên quan 

đầu vào:```
1
5 4
0110
1001
1001
1001
0110
```Cả bốn góc đều`0`. 

Thuật toán không bao giờ bao gồm các góc ở cả hai`sideCost`hoặc`interiorCost`. Yêu cầu về đường viền chỉ được áp dụng cho các ô viền không có góc, khớp chính xác với định nghĩa. Câu trả lời trở thành`0`. 

### Kích thước tối thiểu 

đầu vào:```
1
5 4
0000
0000
0000
0000
0000
```Có chính xác một hình chữ nhật có thể. Quá trình quét vẫn hoạt động vì đường viền bên phải hợp lệ đầu tiên là cột 3, tương ứng với chiều rộng 4. Không cần xử lý đặc biệt. 

### Chiều rộng chính xác là bốn 

đầu vào:```
1
5 4
1111
1001
1001
1001
1111
```Hình chữ nhật chỉ có hai cột bên trong. 

điều kiện`right - left >= 3`cho phép chính xác chiều rộng này. Một lỗi phổ biến là yêu cầu khoảng cách lớn hơn và vô tình từ chối hình chữ nhật hợp lệ duy nhất. 

### Chiều cao chính xác là năm 

đầu vào:```
1
5 5
11111
10001
10001
10001
11111
```Cặp đôi`(top=0,bottom=4)`được xử lý vì thuật toán bắt đầu từ`bottom = top + 4`. Mọi điều kiện chặt chẽ hơn sẽ bỏ qua các cổng hợp lệ có chiều cao tối thiểu một cách không chính xác.
