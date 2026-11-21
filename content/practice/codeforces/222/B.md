---
title: "CF 222B - Bàn vũ trụ"
description: "Chúng ta có một ma trận các số nguyên và ba loại phép toán được áp dụng lặp đi lặp lại cho nó. Một thao tác hoán đổi hai hàng. Một hoán đổi khác hai cột. Thứ ba yêu cầu giá trị hiện hiển thị ở một hàng và cột cụ thể."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 222
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 137 (Div. 2)"
rating: 1300
weight: 222
solve_time_s: 93
verified: true
draft: false
---

[CF 222B - Bàn vũ trụ](https://codeforces.com/problemset/problem/222/B) 

**Đánh giá:** 1300 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một ma trận các số nguyên và ba loại phép toán được áp dụng lặp đi lặp lại cho nó. 

Một thao tác hoán đổi hai hàng. Một hoán đổi khác hai cột. Thứ ba yêu cầu giá trị hiện hiển thị ở một hàng và cột cụ thể. 

Phần khó khăn là bản thân ma trận có thể lớn tới một triệu ô và số lượng truy vấn có thể lên tới năm trăm nghìn. Một giải pháp sắp xếp lại toàn bộ hàng hoặc cột về mặt vật lý mỗi lần sẽ tốn quá nhiều thời gian để di chuyển dữ liệu xung quanh. 

Đầu vào đầu tiên đưa ra kích thước của bảng và số lượng thao tác. Sau đó đến ma trận ban đầu. Sau đó, mỗi truy vấn là một trong những truy vấn sau:`r x y`hàng hoán đổi`x`với hàng`y`.`c x y`cột hoán đổi`x`với cột`y`.`g x y`yêu cầu giá trị hiện tại ở hàng`x`, cột`y`. 

Đối với mọi`g`truy vấn, chúng tôi in số được yêu cầu. 

Các ràng buộc định hình mạnh mẽ giải pháp. Kích thước ma trận tối đa là`1000 × 1000`, có thể quản lý được trong bộ nhớ. Áp lực thực sự đến từ`500000`hoạt động. Thậm chí một`O(n)`hoặc`O(m)`chi phí cho mỗi lần trao đổi trở nên nguy hiểm ở đây. 

Giả sử chúng ta trao đổi vật lý các hàng. Một lần chạm trao đổi hàng đơn`m`các phần tử. Với`m = 1000`Và`500000`hoạt động, điều đó trở nên đại khái`5 × 10^8`nhiệm vụ trong trường hợp xấu nhất. Python sẽ không tồn tại được trong thời gian giới hạn. 

Chúng ta cần mỗi thao tác chạy trong thời gian không đổi. 

Phần không rõ ràng là các chỉ mục hàng và chỉ mục cột ngừng khớp với ma trận ban đầu sau khi bắt đầu hoán đổi. Việc triển khai bất cẩn thường nhầm lẫn “vị trí hàng hiện tại” với “hàng gốc được lưu trong bộ nhớ”. 

Hãy xem xét ví dụ này:```
2 2 3
1 2
3 4
r 1 2
g 1 1
g 2 1
```Sau khi hoán đổi hàng 1 và 2, ma trận hiển thị sẽ trở thành:```
3 4
1 2
```Đầu ra đúng là:```
3
1
```Việc triển khai có lỗi mà quên ánh xạ lại các hàng sẽ vẫn đọc từ ma trận gốc và in`1`sau đó`3`. 

Hoán đổi cột tạo ra vấn đề tương tự từ hướng khác.```
2 3 2
1 2 3
4 5 6
c 1 3
g 1 1
```Hàng đầu tiên hiển thị trở thành:```
3 2 1
```Câu trả lời là`3`, không`1`. 

Một trường hợp tinh vi khác là việc hoán đổi lặp đi lặp lại các hàng hoặc cột giống nhau.```
2 2 3
1 2
3 4
r 1 2
r 1 2
g 1 1
```Hai giao dịch hoán đổi hủy bỏ lẫn nhau. Câu trả lời phải quay lại`1`. Một giải pháp làm thay đổi dữ liệu không chính xác có thể bị lệch khỏi trạng thái thực theo thời gian. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Lưu trữ ma trận chính xác như nó xuất hiện. Để hoán đổi hàng, hãy trao đổi toàn bộ hai mảng hàng. Để hoán đổi cột, hãy lặp qua từng hàng và trao đổi giá trị của hai cột. Đối với một truy vấn, hãy đọc trực tiếp ô được yêu cầu. 

Điều này hoạt động vì mỗi thao tác mô phỏng trực tiếp trạng thái bảng. 

Vấn đề là chi phí. Một trao đổi hàng mất`O(m)`, bởi vì chúng tôi di chuyển tất cả các mục trong các hàng đó. Việc hoán đổi cột mất`O(n)`. Trong trường hợp xấu nhất, với`500000`hoán đổi và kích thước gần`1000`, tổng công việc lên tới hàng trăm triệu thao tác. 

Quan sát quan trọng là các giao dịch hoán đổi không thực sự tự thay đổi các giá trị. Chúng chỉ thay đổi hàng hoặc cột ban đầu hiện xuất hiện ở vị trí hiển thị nhất định. 

Thay vì di chuyển dữ liệu, chúng ta có thể giữ hai mảng ánh xạ:`row_pos[i]`cho chúng ta biết hàng gốc nào hiện đang chiếm hàng hiển thị`i`.`col_pos[j]`cho chúng tôi biết cột gốc nào hiện đang chiếm giữ cột hiển thị`j`. 

Ban đầu:```
row_pos[i] = i
col_pos[j] = j
```Khi chúng ta hoán đổi các hàng, chúng ta chỉ hoán đổi các mục bên trong`row_pos`. 

Khi hoán đổi các cột, chúng ta chỉ hoán đổi các mục bên trong`col_pos`. 

Đối với một truy vấn`(x, y)`, giá trị thực tế đến từ:```
matrix[row_pos[x]][col_pos[y]]
```Bây giờ mọi hoạt động đều trở thành thời gian không đổi. 

Giải pháp brute-force hoạt động vì nó duy trì trực tiếp ma trận hiển thị chính xác. Giải pháp tối ưu hóa hoạt động vì nội dung ma trận không bao giờ di chuyển một cách hợp lý, chỉ có cách giải thích các chỉ số thay đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k × max(n, m)) | O(nm) | Quá chậm | 
| Tối ưu | O(k + nm) | O(nm + n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ma trận vào bộ nhớ. 

Chúng tôi giữ nguyên ma trận ban đầu trong suốt chương trình. 
2. Tạo một mảng ánh xạ hàng.`rows[i]`lưu trữ hàng gốc hiện đang xuất hiện ở hàng hiển thị`i`. 

Ban đầu, mỗi hàng ánh xạ tới chính nó. 
3. Tạo một mảng ánh xạ cột.`cols[j]`lưu trữ cột gốc hiện đang xuất hiện ở cột hiển thị`j`. 

Ban đầu, mỗi cột ánh xạ tới chính nó. 
4. Xử lý từng truy vấn một. 

Mỗi truy vấn sửa đổi ánh xạ hoặc yêu cầu một giá trị. 
5. Đối với truy vấn hoán đổi hàng`r x y`, tráo đổi`rows[x]`Và`rows[y]`. 

Điều này cập nhật thứ tự hiển thị ngay lập tức mà không cần chạm vào ma trận. 
6. Đối với truy vấn hoán đổi cột`c x y`, tráo đổi`cols[x]`Và`cols[y]`. 

Một lần nữa, chỉ có cách giải thích vị trí cột thay đổi. 
7. Đối với truy vấn nhận`g x y`, tính:```
original_row = rows[x]
original_col = cols[y]
```Sau đó xuất ra:```
matrix[original_row][original_col]
```8. Tiếp tục cho đến khi tất cả các truy vấn được xử lý. 

### Tại sao nó hoạt động 

Bất biến là:`rows[i]`luôn bằng hàng ban đầu hiện được hiển thị ở hàng hiển thị`i`, Và`cols[j]`luôn bằng cột gốc hiện được hiển thị ở cột hiển thị`j`. 

Ban đầu điều này đúng vì không có gì được hoán đổi. 

Hoán đổi hàng trao đổi hai vị trí hiển thị, do đó hoán đổi các mục tương ứng trong`rows`bảo toàn chính xác bất biến. Lập luận tương tự cũng áp dụng cho việc hoán đổi cột. 

Một truy vấn yêu cầu giá trị hiện được hiển thị tại`(x, y)`. Theo bất biến, giá trị được lưu trữ thực tế đến từ hàng gốc`rows[x]`và cột ban đầu`cols[y]`. Đọc ô đó từ ma trận không thay đổi luôn trả về câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    table = [list(map(int, input().split())) for _ in range(n)]

    rows = list(range(n))
    cols = list(range(m))

    ans = []

    for _ in range(k):
        t, x, y = input().split()
        x = int(x) - 1
        y = int(y) - 1

        if t == 'r':
            rows[x], rows[y] = rows[y], rows[x]

        elif t == 'c':
            cols[x], cols[y] = cols[y], cols[x]

        else:
            ans.append(str(table[rows[x]][cols[y]]))

    sys.stdout.write("\n".join(ans))

solve()
```Ma trận được lưu trữ một lần và không bao giờ được sửa đổi sau đó. Đây là ý tưởng thực hiện trung tâm. Mọi thay đổi đều xảy ra thông qua`rows`Và`cols`mảng ánh xạ. 

Các ánh xạ sử dụng lập chỉ mục dựa trên 0 vì danh sách Python dựa trên 0. Mỗi truy vấn sẽ trừ một chỉ số đầu vào ngay lập tức. Quên chuyển đổi này là lỗi thường gặp nhất trong vấn đề này. 

Để hoán đổi hàng, mã chỉ hoán đổi hai số nguyên bên trong`rows`. Điều tương tự cũng áp dụng cho các cột. Không có phần tử ma trận nào di chuyển vật lý. 

Hoạt động truy vấn thoạt nhìn có vẻ hơi bất đối xứng:```
table[rows[x]][cols[y]]
```Điều này hoạt động vì`rows[x]`đã cho chúng tôi biết hàng gốc nào hiện đang chiếm hàng hiển thị`x`. Bản thân ma trận vẫn sử dụng tọa độ ban đầu. 

Đầu ra được tích lũy thành một danh sách và được in một lần ở cuối. Việc in từng dòng lên tới nửa triệu lần có thể trở nên chậm hơn trong Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3 5
1 2 3
4 5 6
7 8 9
g 3 2
r 3 2
c 2 3
g 2 2
g 3 2
```Trạng thái ban đầu:```
rows = [0, 1, 2]
cols = [0, 1, 2]
```| Truy vấn | hàng | cols | Đầu ra | 
| --- | --- | --- | --- | 
| g 3 2 | [0,1,2] | [0,1,2] | 8 | 
| r 3 2 | [0,2,1] | [0,1,2] | | 
| c 2 3 | [0,2,1] | [0,2,1] | | 
| g 2 2 | [0,2,1] | [0,2,1] | 9 | 
| g 3 2 | [0,2,1] | [0,2,1] | 6 | 

Dấu vết cho thấy ma trận không bao giờ thay đổi về mặt vật lý. Chỉ có mảng giải thích phát triển. Sau khi hoán đổi hàng 2 và 3, hàng hiển thị 2 hiện trỏ tới hàng 3 ban đầu. Sau khi hoán đổi cột 2 và 3, cột hiển thị 2 trỏ đến cột 3 ban đầu. 

### Ví dụ tùy chỉnh 

đầu vào:```
2 3 4
1 2 3
4 5 6
c 1 3
r 1 2
g 1 1
g 2 3
```Trạng thái ban đầu:```
rows = [0, 1]
cols = [0, 1, 2]
```| Truy vấn | hàng | cols | Đầu ra | 
| --- | --- | --- | --- | 
| c 1 3 | [0,1] | [2,1,0] | | 
| r 1 2 | [1,0] | [2,1,0] | | 
| g 1 1 | [1,0] | [2,1,0] | 6 | 
| g 2 3 | [1,0] | [2,1,0] | 1 | 

Sau khi hoán đổi cột, cột hiển thị 1 tương ứng với cột 3 ban đầu. Sau khi hoán đổi hàng, hàng hiển thị 1 tương ứng với hàng 2 ban đầu. Truy vấn`(1,1)`truy cập chính xác vào ô gốc`(2,3)`, trong đó có chứa`6`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm + k) | Đọc ma trận tốn O(nm), mỗi truy vấn tốn O(1) | 
| Không gian | O(nm + n + m) | Lưu trữ ma trận cộng với ánh xạ hàng và cột | 

Ma trận chứa tối đa một triệu số nguyên, vừa vặn trong giới hạn bộ nhớ. Mỗi truy vấn chỉ thực hiện một vài truy cập hoặc hoán đổi mảng, do đó, ngay cả năm trăm nghìn thao tác cũng có thể chạy hiệu quả trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n, m, k = map(int, input().split())

        table = [list(map(int, input().split())) for _ in range(n)]

        rows = list(range(n))
        cols = list(range(m))

        ans = []

        for _ in range(k):
            t, x, y = input().split()
            x = int(x) - 1
            y = int(y) - 1

            if t == 'r':
                rows[x], rows[y] = rows[y], rows[x]

            elif t == 'c':
                cols[x], cols[y] = cols[y], cols[x]

            else:
                ans.append(str(table[rows[x]][cols[y]]))

        return "\n".join(ans)

    return solve()

# provided sample
assert run(
"""3 3 5
1 2 3
4 5 6
7 8 9
g 3 2
r 3 2
c 2 3
g 2 2
g 3 2
"""
) == "8\n9\n6", "sample 1"

# minimum size
assert run(
"""1 1 1
42
g 1 1
"""
) == "42", "minimum matrix"

# repeated swaps cancel out
assert run(
"""2 2 3
1 2
3 4
r 1 2
r 1 2
g 1 1
"""
) == "1", "double row swap"

# column remapping correctness
assert run(
"""2 3 2
1 2 3
4 5 6
c 1 3
g 1 1
"""
) == "3", "column swap"

# combined row and column swaps
assert run(
"""2 3 4
1 2 3
4 5 6
c 1 3
r 1 2
g 1 1
g 2 3
"""
) == "6\n1", "combined mappings"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ma trận 1×1 | 42 | Đầu vào hợp lệ nhỏ nhất | 
| Hai lần hoán đổi hàng giống hệt nhau | 1 | Hoán đổi ngược lại một cách chính xác | 
| Hoán đổi cột đơn | 3 | Cột ánh xạ lại chính xác | 
| Hoán đổi hàng và cột kết hợp | 6, 1 | Tương tác của cả hai ánh xạ | 

## Vỏ cạnh 

Xem xét việc hoán đổi lặp lại các hàng giống nhau. 

đầu vào:```
2 2 3
1 2
3 4
r 1 2
r 1 2
g 1 1
```Thực thi: 

Sau lần hoán đổi đầu tiên:```
rows = [1, 0]
```Sau lần hoán đổi thứ hai:```
rows = [0, 1]
```Ánh xạ trở về trạng thái ban đầu. Truy vấn`(1,1)`truy cập:```
table[0][0] = 1
```Thuật toán xử lý việc này một cách tự nhiên vì các giao dịch hoán đổi chỉ trao đổi các mục ánh xạ. 

Bây giờ hãy xem xét việc ánh xạ lại hàng và cột độc lập. 

đầu vào:```
2 3 3
1 2 3
4 5 6
r 1 2
c 1 3
g 1 1
```Sau khi trao đổi hàng:```
rows = [1, 0]
```Sau khi hoán đổi cột:```
cols = [2, 1, 0]
```Truy vấn`(1,1)`bản đồ tới:```
table[1][2] = 6
```Ô trên cùng bên trái có thể nhìn thấy thực sự trở thành`6`, vì hàng hiển thị 1 là hàng gốc 2 và cột hiển thị 1 là cột gốc 3. 

Cuối cùng, hãy xem xét ma trận nhỏ nhất có thể. 

đầu vào:```
1 1 1
99
g 1 1
```Không có giao dịch hoán đổi nào tồn tại vì chỉ có một hàng và một cột. Các mảng ánh xạ vẫn còn:```
rows = [0]
cols = [0]
```Truy vấn có nội dung:```
table[0][0] = 99
```Điều này xác nhận việc triển khai xử lý các kích thước ranh giới một cách chính xác mà không có trường hợp đặc biệt.
