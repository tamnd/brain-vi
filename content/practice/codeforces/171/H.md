---
title: "CF 171H - Một đường đa tuyến"
description: "Đầu vào chứa hai số. Số đầu tiên, a, xác định thứ tự của một đa tuyến được xây dựng đệ quy. Số thứ hai, b, là chỉ số dọc theo đường đa tuyến đó. Hình ảnh trong tuyên bố là chìa khóa."
date: "2026-06-02T08:52:57+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 171
codeforces_index: "H"
codeforces_contest_name: "April Fools Day Contest"
rating: 1700
weight: 171
solve_time_s: 99
verified: true
draft: false
---

[CF 171H - Một đường đa tuyến](https://codeforces.com/problemset/problem/171/H) 

**Đánh giá:** 1700 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào chứa hai số. Số đầu tiên,`a`, xác định thứ tự của một đa tuyến được xây dựng đệ quy. Số thứ hai,`b`, là một chỉ mục dọc theo đường đa tuyến đó. 

Hình ảnh trong tuyên bố là chìa khóa. Đó là đường cong Hilbert cổ điển, một đường cong lấp đầy không gian đi thăm từng ô của một`2^a × 2^a`lưới đúng một lần. Các ô được đánh số theo thứ tự đường cong đi qua chúng. Theo thứ tự`a`và một vị trí`b`trên đường cong, chúng ta phải khôi phục tọa độ của ô lưới tương ứng. 

Ràng buộc`a ≤ 10`là cực kỳ nhỏ. Chiều dài cạnh lưới tối đa là`2^10 = 1024`, vậy tổng số ô nhiều nhất là`2^20 = 1,048,576`. Ngay cả việc xây dựng đệ quy hoặc từng bit một cũng không đáng kể trong giới hạn. 

Khó khăn chính không phải là hiệu quả. Thách thức là nhận ra cấu trúc được mã hóa bằng hình ảnh và xử lý chính xác các phép quay và phản xạ xảy ra bên trong mỗi góc phần tư đệ quy của đường cong Hilbert. 

Một sai lầm phổ biến là coi việc đánh số là thứ tự hàng chính đơn giản hoặc thứ tự Morton. Ví dụ: 

đầu vào```
2 15
```Câu trả lời là```
3 0
```Một cách giải thích hàng chính sẽ tạo ra`(3,3)`, điều này không chính xác vì đường cong Hilbert thay đổi hướng giữa các góc phần tư. 

Một chỗ dễ mắc lỗi khác là việc xử lý định hướng đệ quy. Ví dụ: 

đầu vào```
4 160
```Câu trả lời là```
12 12
```Một giải pháp đi vào đúng góc phần tư nhưng quên phép quay cần thiết sẽ đến sai ô mặc dù tất cả các quyết định cấp cao hơn đều đúng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ tạo ra toàn bộ đường cong trật tự Hilbert một cách rõ ràng`a`, lưu trữ mọi tọa độ đã truy cập và sau đó xuất ra`b`-thứ 1 

Điều này có tác dụng vì đường cong lớn nhất chỉ chứa khoảng một triệu ô. Tạo ra tất cả chúng là khả thi. Phương pháp này cũng dễ chứng minh: nếu chúng ta xây dựng đường cong theo thứ tự truy cập theo đúng nghĩa đen thì câu trả lời đơn giản là`b`-điểm được tạo ra. 

Điểm yếu là chúng ta đang giải quyết vấn đề lập chỉ mục trực tiếp bằng cách xây dựng toàn bộ đối tượng. Chúng tôi chỉ cần một ô, nhưng chúng tôi tạo ra mọi ô. 

Cấu trúc đệ quy của đường cong Hilbert mang lại một giải pháp rõ ràng hơn nhiều. Đường cong trật tự Hilbert`a`bao gồm bốn đường cong Hilbert có trật tự`a-1`, mỗi cái chiếm một góc phần tư. chỉ số`b`ngay lập tức cho chúng ta biết góc phần tư nào chứa câu trả lời. Sau khi xác định góc phần tư, chúng tôi loại bỏ sự đóng góp của tất cả các góc phần tư trước đó và tiếp tục đệ quy bên trong góc phần tư đã chọn. 

Điều phức tạp duy nhất là mỗi góc phần tư có thể được xoay hoặc phản chiếu so với góc phần tư gốc. Khi các phép biến đổi đó được xử lý chính xác, mọi cấp độ đệ quy sẽ xử lý chính xác một cặp bit từ chỉ mục. 

Điều này dẫn đến thuật toán chuyển đổi chỉ số sang tọa độ Hilbert tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Thế hệ Brute Force | O(4^a) | O(4^a) | Được chấp nhận nhưng không cần thiết | 
| Giải mã Hilbert tối ưu | O(a) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`n = 2^a`. Chúng tôi muốn tọa độ của số ô`b`bên trong một`n × n`lưới. 
2. Xử lý chỉ số Hilbert từ mức đệ quy thấp nhất trở lên. 
3. Ở mức có kích thước hình vuông là`s × s`, xác định hai bit:`rx`Và`ry`. 

Các bit này xác định góc phần tư phụ nào chứa ô đích. 
4. Trước khi chuyển sang góc phần tư đã chọn, hãy áp dụng quy tắc định hướng Hilbert. 

Nếu như`ry = 0`, hình vuông hiện tại phải được xoay. Nếu thêm vào`rx = 1`, tọa độ được phản ánh trước khi quay. 
5. Sau khi chuyển đổi, thêm phần bù góc phần tư:`x += s * rx`Và`y += s * ry`. 
6. Loại bỏ hai bit đã xử lý khỏi chỉ mục bằng cách chia cho bốn. 
7. Gấp đôi`s`và tiếp tục cho đến khi tất cả các mức đệ quy đã được xử lý. 
8. Xuất kết quả cuối cùng`(x, y)`. 

### Tại sao nó hoạt động 

Đường cong Hilbert được xác định đệ quy. Ở mỗi thứ tự, hình vuông được chia thành bốn góc phần tư bằng nhau. Thứ tự di chuyển của các góc phần tư đó là cố định, nhưng mỗi góc phần tư chứa một phiên bản quay của đường cong Hilbert nhỏ hơn. 

Thuật toán duy trì tính bất biến sau khi xử lý một mức,`(x, y)`là vị trí chính xác bên trong phần đã được xây dựng lại của đường cong. Các quy tắc xoay và phản xạ khớp chính xác với cách đường cong Hilbert kết nối bốn đường cong con của nó. Vì mỗi lần lặp giải mã một mức đệ quy và bảo toàn bất biến này, nên xét cho cùng`a`các mức đã được xử lý, tọa độ kết quả chính xác là ô được truy cập tại chỉ mục`b`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b = map(int, input().split())

n = 1 << a

x = 0
y = 0
t = b
s = 1

while s < n:
    rx = (t // 2) & 1
    ry = (t ^ rx) & 1

    if ry == 0:
        if rx == 1:
            x = s - 1 - x
            y = s - 1 - y
        x, y = y, x

    x += s * rx
    y += s * ry

    t //= 4
    s <<= 1

print(x, y)
```Biến`t`lưu trữ phần còn lại của chỉ mục Hilbert chưa được giải mã. Mỗi lần lặp tiêu tốn hai bit vì mỗi cấp độ đệ quy chọn một trong bốn góc phần tư. 

Các giá trị`rx`Và`ry`xác định góc phần tư đã chọn. Việc điều chỉnh hướng phải diễn ra trước khi thêm phần bù góc phần tư. Đảo ngược thứ tự này sẽ tạo ra tọa độ không chính xác vì phép quay được xác định bên trong hệ tọa độ cục bộ của hình vuông hiện tại. 

Bước phản ánh sử dụng`s - 1 - x`Và`s - 1 - y`. các`-1`là điều cần thiết. Việc quên nó sẽ dịch chuyển mỗi tọa độ một ô và phá vỡ đệ quy. 

Không có mối lo ngại tràn vì tọa độ không bao giờ vượt quá`1023`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào```
1 0
```| s | t | rx | ry | x | y | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 0 | 0 | 

Đầu ra:```
0 0
```Đây là ô đầu tiên được đường cong Hilbert bậc 1 ghé thăm. 

### Ví dụ 2 

đầu vào```
2 15
```| s | t | rx | ry | x | y | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 15 | 1 | 0 | 1 | 0 | 
| 2 | 3 | 1 | 0 | 3 | 0 | 

Đầu ra:```
3 0
```Ví dụ này chứng minh tại sao việc định hướng lại quan trọng. Chỉ số Hilbert cuối cùng trong`4 × 4`lưới thì không`(3,3)`. Các phép quay đệ quy đặt nó tại`(3,0)`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(a) | Một lần lặp cho mỗi cấp độ đệ quy | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Từ`a ≤ 10`, vòng lặp chạy tối đa mười lần. Giải pháp này có hiệu quả tức thời và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    a, b = map(int, input().split())

    n = 1 << a
    x = y = 0
    t = b
    s = 1

    while s < n:
        rx = (t // 2) & 1
        ry = (t ^ rx) & 1

        if ry == 0:
            if rx == 1:
                x = s - 1 - x
                y = s - 1 - y
            x, y = y, x

        x += s * rx
        y += s * ry

        t //= 4
        s <<= 1

    print(x, y)

def run(inp: str) -> str:
    global input
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = old
    return out.getvalue().strip()

# provided samples
assert run("1 0\n") == "0 0", "sample 1"
assert run("2 15\n") == "3 0", "sample 2"
assert run("4 160\n") == "12 12", "sample 3"

# custom cases
assert run("1 1\n") == "0 1", "second cell of order 1"
assert run("1 3\n") == "1 0", "last cell of order 1"
assert run("10 0\n") == "0 0", "minimum index at maximum order"
assert run("10 1048575\n") == "1023 0", "last index of largest grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0 1`| Đúng vòng quay đầu tiên | 
|`1 3`|`1 0`| Cuối đường cong không tầm thường nhỏ nhất | 
|`10 0`|`0 0`| Thứ tự tối đa, chỉ số tối thiểu | 
|`10 1048575`|`1023 0`| Thứ tự tối đa, chỉ số tối đa | 

## Vỏ cạnh 

Hãy xem xét:```
1 0
```Vòng lặp thực hiện chính xác một cấp độ. Cả hai bit góc phần tư đều bằng 0, không thêm phần bù và kết quả vẫn giữ nguyên`(0,0)`. Điều này xác nhận việc xử lý chính xác chỉ số nhỏ nhất có thể. 

Coi như:```
2 15
```Ở cả hai cấp độ đệ quy, chúng ta đều bước vào góc phần tư Hilbert thứ tư. Thuật toán thực hiện phản xạ và xoay cần thiết trước khi thêm độ lệch. Bỏ qua sự chuyển đổi đó sẽ kết thúc không chính xác tại`(3,3)`. Đầu ra đúng là`(3,0)`. 

Coi như:```
10 1048575
```Đây là chỉ số hợp lệ lớn nhất. Mỗi cấp độ đệ quy đều chọn góc phần tư cuối cùng. Thuật toán vẫn chỉ thực hiện mười lần lặp và trả về`(1023,0)`, cho thấy việc triển khai xử lý giới hạn trên mà không có bất kỳ trường hợp đặc biệt nào.
