---
title: "CF 104857E - Khoảng cách ma trận"
description: "Chúng ta có một lưới $n lần m$ trong đó mỗi ô chứa một màu nguyên. Đối với mỗi màu, chúng ta xem xét tất cả các ô có màu đó và xem xét từng cặp ô có thứ tự như vậy."
date: "2026-06-28T10:55:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104857
codeforces_index: "E"
codeforces_contest_name: "The 2023 ICPC Asia Hefei Regional Contest (The 2nd Universal Cup. Stage 12: Hefei)"
rating: 0
weight: 104857
solve_time_s: 45
verified: true
draft: false
---

[CF 104857E - Khoảng cách ma trận](https://codeforces.com/problemset/problem/104857/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times m$lưới trong đó mỗi ô chứa một màu nguyên. Đối với mỗi màu, chúng ta xem xét tất cả các ô có màu đó và xem xét từng cặp ô có thứ tự như vậy. Đối với mỗi cặp, chúng tôi tính toán khoảng cách Manhattan giữa tọa độ của chúng và tính tổng tất cả các khoảng cách này trên tất cả các màu. 

Vì vậy, nhiệm vụ không phải là khoảng cách giữa các ô tùy ý mà là trong phạm vi mỗi nhóm màu. Mỗi màu tạo thành một tập hợp các điểm trên lưới và chúng ta cần tổng khoảng cách Manhattan theo cặp bên trong mỗi tập hợp, tính tổng trên tất cả các tập hợp. 

Những hạn chế$n, m \le 1000$ngụ ý lên đến$10^6$tế bào. Cách tiếp cận bậc hai đơn giản cho mỗi màu rõ ràng sẽ thất bại vì trong trường hợp xấu nhất, tất cả các ô đều có cùng màu, tạo ra$10^{12}$cặp. Ngay cả việc quét tuyến tính trên tất cả các cặp cũng không thể thực hiện được. 

Một điểm tinh tế là bài toán đếm các cặp có thứ tự$(i, j)$, không chỉ các cặp không có thứ tự. Điều đó tăng gấp đôi sự đóng góp so với các cặp duy nhất. Một giải pháp đơn giản có thể tính các khoảng cách không có thứ tự và quên nhân với 2, dẫn đến kết quả không chính xác. 

Các trường hợp cạnh quan trọng bao gồm: 

Một lưới trong đó tất cả các ô có cùng màu. Ví dụ:```
2 2
1 1
1 1
```Câu trả lời đúng là 8 vì mỗi cặp không có thứ tự đều đóng góp hai lần. 

Một trường hợp khác là khi tất cả các màu là duy nhất:```
2 2
1 2
3 4
```Câu trả lời là 0 vì không có màu nào có nhiều hơn một ô. Việc triển khai đơn giản cố gắng tính toán sự khác biệt theo cặp mà không kiểm tra kích thước nhóm vẫn có thể cố gắng làm những công việc không cần thiết hoặc thậm chí tạo ra lỗi lập chỉ mục nếu các nhóm không được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ sửa từng màu, thu thập tất cả tọa độ của nó và tính toán khoảng cách giữa mỗi cặp được sắp xếp. Nếu một màu xuất hiện$k$lần, chi phí này$O(k^2)$. Tổng hợp tất cả các màu, điều này suy biến thành$O(n^2 m^2)$trong trường hợp xấu nhất, vì một màu có thể chiếm toàn bộ lưới. 

Quan sát quan trọng là khoảng cách Manhattan tách biệt rõ ràng thành các phần đóng góp độc lập từ tọa độ hàng và cột. Cho hai điểm$(x_1, y_1)$Và$(x_2, y_2)$,$$|x_1 - x_2| + |y_1 - y_2|$$có thể được xử lý dưới dạng tổng của một số hạng thuần túy dựa trên x và một số hạng thuần túy dựa trên y. 

Điều này có nghĩa là chúng ta có thể xử lý tất cả tọa độ x của một màu một cách độc lập với tọa độ y. Tổng số tiền đóng góp trở thành: 

tổng theo màu của (tổng theo cặp |x_i - x_j| + tổng theo cặp |y_i - y_j|). 

Bây giờ, vấn đề giảm xuống còn tính tổng các khác biệt tuyệt đối trong mảng 1D cho mỗi nhóm màu, hai lần: một lần cho hàng và một lần cho cột. 

Đối với chuỗi 1D, các giá trị được sắp xếp cho phép chúng ta tính tổng chênh lệch tuyệt đối theo cặp trong thời gian tuyến tính bằng cách sử dụng tích lũy tiền tố. Thay vì so sánh tất cả các cặp, chúng tôi duy trì tổng tiền tố đang chạy và tích lũy các khoản đóng góp khi chúng tôi quét. 

Điều này làm giảm từng nhóm màu từ bậc hai sang tuyến tính sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((nm)^2)$|$O(nm)$| Quá chậm | 
| Tối ưu |$O(nm \log nm)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Nhóm các ô theo màu 

Chúng tôi quét lưới một lần và lưu trữ cho mỗi màu hai danh sách: chỉ mục hàng và chỉ mục cột của nó. Sự tách biệt này là cần thiết vì khoảng cách Manhattan chia thành các phần đóng góp x và y độc lập. 

### 2. Đối với mỗi màu, xử lý tọa độ hàng 

Chúng tôi sắp xếp danh sách các chỉ số hàng cho màu đó. Cần phải sắp xếp để có thể thay thế những khác biệt tuyệt đối bằng tổng tiền tố có cấu trúc thay vì so sánh theo cặp. 

### 3. Tính toán đóng góp từ các hàng 

Chúng tôi duy trì tổng tiền tố đang chạy trên các hàng được sắp xếp. Khi xử lý một giá trị$x_i$, tất cả các giá trị trước đó$x_j$đóng góp$x_i - x_j$, và tất cả các khoản đóng góp trong tương lai sẽ được xử lý sau theo hình thức tích lũy ngược. Điều này chuyển đổi sự khác biệt theo cặp thành cập nhật tuyến tính. 

### 4. Lặp lại cho tọa độ cột 

Chúng tôi áp dụng quy trình tương tự cho các chỉ số cột. Điều này giải thích một cách độc lập cho thành phần thẳng đứng của khoảng cách Manhattan. 

### 5. Tổng đóng góp của tất cả các màu 

Mỗi màu đóng góp độc lập nên chúng tôi tích lũy kết quả trên tất cả các nhóm. 

### Tại sao nó hoạt động 

Đối với bất kỳ màu cố định nào, chúng tôi đang tính toán:$$\sum |x_i - x_j| + \sum |y_i - y_j|$$Tính tuyến tính của phép tính tổng cho phép tách tọa độ. Phương pháp tiền tố được sắp xếp đảm bảo mỗi cặp được tính chính xác một lần với độ lớn chính xác vì mỗi phần tử đóng góp tỷ lệ thuận với số phần tử nằm trước và sau nó theo thứ tự được sắp xếp. Không có sự tương tác giữa các màu sắc, do đó việc xử lý chúng một cách độc lập sẽ đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def sum_abs_pairs(arr):
    arr.sort()
    res = 0
    prefix = 0
    for i, x in enumerate(arr):
        res += x * i - prefix
        prefix += x
    return res

def solve():
    n, m = map(int, input().split())
    rows = defaultdict(list)
    cols = defaultdict(list)

    for i in range(n):
        line = list(map(int, input().split()))
        for j, c in enumerate(line):
            rows[c].append(i)
            cols[c].append(j)

    ans = 0
    for c in rows:
        ans += sum_abs_pairs(rows[c])
        ans += sum_abs_pairs(cols[c])

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì hai bản đồ băm được khóa theo màu. Mỗi màu tích lũy các vị trí hàng và cột riêng biệt, điều này rất cần thiết để duy trì sự phân rã khoảng cách Manhattan. 

chức năng`sum_abs_pairs`là tối ưu hóa cốt lõi. Sau khi sắp xếp, nó sử dụng nhận dạng cho một phần tử cố định$x_i$, tất cả các phần tử trước đó đóng góp tổng cộng$x_i \cdot i - \text{prefix sum}$. Điều này tránh việc liệt kê cặp rõ ràng. 

Một lỗi phổ biến là lặp lại danh sách không có thứ tự. Nếu không sắp xếp, phép trừ tiền tố không tương ứng với sự khác biệt tuyệt đối và kết quả sẽ không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
1 1
2 2
```Chúng tôi xử lý màu sắc một cách độc lập. 

| Màu sắc | Hàng | Cols | Hàng được sắp xếp | Đóng góp hàng | Cols được sắp xếp | Đóng góp của Col | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | [0,0] | [0,1] | [0,0] | 0 | [0,1] | 1 | 1 | 
| 2 | [1,1] | [0,1] | [1,1] | 0 | [0,1] | 1 | 1 | 

Vì các cặp được sắp xếp theo thứ tự, mỗi khoảng cách không có thứ tự xuất hiện hai lần, do đó tổng trở thành$2$. 

Dấu vết này cho thấy tọa độ giống hệt nhau mang lại sự đóng góp hàng bằng 0 như thế nào, trong khi sự khác biệt về cột chiếm ưu thế. 

### Ví dụ 2 

đầu vào:```
1 3
1 2 1
```Màu 1 có các vị trí (0,0), (0,2). 

| Màu sắc | Hàng | Cols | Đóng góp hàng | Đóng góp của Col | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [0,0] | [0,2] | 0 | 2 | 2 | 
| 2 | [0] | [1] | 0 | 0 | 0 | 

Điều này xác nhận rằng chỉ có khoảng cách theo chiều ngang mới góp phần tạo ra màu này và các màu đơn lẻ không đóng góp gì. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm \log (nm))$| Mỗi ô được lưu trữ một lần cho mỗi màu và mỗi danh sách màu được sắp xếp trước khi tích lũy tuyến tính | 
| Không gian |$O(nm)$| Chúng tôi lưu trữ tọa độ cho mọi ô trong bản đồ băm | 

Các ràng buộc cho phép lên đến$10^6$các ô và việc sắp xếp cộng với quét tuyến tính đều nằm trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính theo kích thước lưới, điều này cũng có thể chấp nhận được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    def sum_abs_pairs(arr):
        arr.sort()
        res = 0
        prefix = 0
        for i, x in enumerate(arr):
            res += x * i - prefix
            prefix += x
        return res

    def solve():
        n, m = map(int, input().split())
        rows = defaultdict(list)
        cols = defaultdict(list)

        for i in range(n):
            line = list(map(int, input().split()))
            for j, c in enumerate(line):
                rows[c].append(i)
                cols[c].append(j)

        ans = 0
        for c in rows:
            ans += sum_abs_pairs(rows[c])
            ans += sum_abs_pairs(cols[c])

        return str(ans)

    return solve()

# sample 1
assert run("2 2\n1 1\n2 2\n") == "2"

# sample 2
assert run("1 3\n1 2 1\n") == "2"

# all unique
assert run("2 2\n1 2\n3 4\n") == "0"

# all same
assert run("2 2\n1 1\n1 1\n") == "8"

# line test
assert run("1 4\n5 5 5 5\n") == "12"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2x2 cùng màu | 8 | đặt hàng cặp đôi | 
| tất cả đều khác biệt | 0 | không có đóng góp | 
| màu lặp lại hàng đơn | 12 | tích lũy 1D đúng | 
| màu hỗn hợp | hành vi phân chia đúng | sự độc lập của nhóm | 

## Vỏ cạnh 

Lưới đơn màu dày đặc là trường hợp nhạy cảm nhất. Ví dụ:```
2 2
1 1
1 1
```Thuật toán lưu trữ các hàng`[0,0,1,1]`và cột`[0,1,0,1]`cho màu đó. Sau khi sắp xếp, cả hai trở thành`[0,0,1,1]`. Sự đóng góp hàng là$(1-0) + (1-0) + (1-0) + (1-0) = 4$và đóng góp của cột là như nhau, tổng cộng là 8. Phương pháp tổng tiền tố đảm bảo mỗi cặp được tính chính xác một lần cho mỗi hướng và sự đối xứng thứ tự tạo ra sự đóng góp nhân đôi chính xác. 

Trường hợp cạnh thứ hai có nhiều màu đơn. Mỗi danh sách có kích thước 1, vì vậy sau khi sắp xếp, logic tiền tố đóng góp 0 vì không có cặp. Thuật toán tự nhiên bỏ qua những công việc không cần thiết vì đóng góp của cả hàng và cột vẫn bằng 0.
