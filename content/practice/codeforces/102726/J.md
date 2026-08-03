---
title: "CF 102726J - Rủi ro"
description: "Bài toán yêu cầu chúng ta xem biểu đồ như một mô hình của các biến ngẫu nhiên tương quan. Mỗi đỉnh đại diện cho một biến ngẫu nhiên có giá trị trung bình bằng 0. Phương sai của một biến bằng bậc đỉnh của nó, các biến liền kề có hiệp phương sai -1 và các biến không liền kề có hiệp phương sai 0."
date: "2026-08-01T22:11:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102726
codeforces_index: "J"
codeforces_contest_name: "UTPC Contest 9-11-20 Div. 1"
rating: 0
weight: 102726
solve_time_s: 88
verified: true
draft: false
---

[CF 102726J - Rủi ro](https://codeforces.com/problemset/problem/102726/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Bài toán yêu cầu chúng ta xem biểu đồ như một mô hình của các biến ngẫu nhiên tương quan. Mỗi đỉnh đại diện cho một biến ngẫu nhiên có giá trị trung bình bằng 0. Phương sai của một biến bằng bậc đỉnh của nó, các biến liền kề có hiệp phương sai`-1`và các biến không liền kề có hiệp phương sai`0`. Chúng ta phải chọn các hệ số cho tổ hợp tuyến tính của các biến này, với tổng bình phương các hệ số được cố định bằng`1`và tối đa hóa độ lệch chuẩn của sự kết hợp đó. 

Đầu vào đưa ra một đồ thị không có trọng số vô hướng. Đầu ra là độ lệch chuẩn lớn nhất có thể có của bất kỳ tổ hợp tuyến tính chuẩn hóa nào của các biến liên quan đến các đỉnh. 

Đồ thị có nhiều nhất`100`đỉnh và`1600`các cạnh. Các giới hạn này đủ nhỏ để chúng ta có thể thực hiện được các thuật toán liên quan đến việc lặp đi lặp lại trên tất cả các cạnh hoặc tất cả các đỉnh. Tuy nhiên, chúng không đủ lớn để biện minh cho các thủ tục phân rã ma trận đa năng nặng nề trong môi trường cạnh tranh, vì vậy chúng ta cần khai thác cấu trúc của ma trận được tạo bởi biểu đồ. 

Quan sát quan trọng là ma trận hiệp phương sai chính xác là đồ thị Laplacian. Đối với mỗi đỉnh, mục nhập đường chéo là độ của nó. Đối với mỗi cạnh, hai mục ngoài đường chéo tương ứng là`-1`. Tất cả các mục khác đều bằng không. Phương sai lớn nhất của tổ hợp tuyến tính chuẩn hóa là giá trị riêng lớn nhất của ma trận đối xứng này và câu trả lời là căn bậc hai của nó. 

Một lỗi phổ biến là chỉ tìm kiếm giữa các biến riêng lẻ. Ví dụ: một đồ thị có hai đỉnh được kết nối có mỗi phương sai biến bằng`1`, nhưng kết hợp chúng với các dấu trái ngược nhau sẽ cho ra phương sai`4`, tạo ra độ lệch chuẩn`2`? Trên thực tế, đối với ma trận hiệp phương sai`[[1,-1],[-1,1]]`, giá trị riêng lớn nhất là`2`, vậy câu trả lời là`sqrt(2)`. Sự kết hợp tối đa hóa sử dụng cả hai biến cùng nhau, đó là lý do tại sao việc kiểm tra các đỉnh một cách độc lập lại thất bại. 

Một trường hợp cạnh khác là đồ thị bị ngắt kết nối. Coi như:```
3 1
0 1
```Đầu ra đúng là khoảng:```
1.4142135623730951
```Đỉnh bị cô lập đóng góp giá trị riêng bằng 0. Việc triển khai bất cẩn cho rằng đồ thị phải được kết nối hoặc loại bỏ các đỉnh bị cô lập sẽ làm thay đổi ma trận và có thể làm mất phổ chính xác. 

Trường hợp tinh tế cuối cùng là một biểu đồ trong đó tất cả các độ đều bằng nhau. Ví dụ:```
3 3
0 1
1 2
2 0
```Ma trận hiệp phương sai là đồ thị tam giác Laplacian. Giá trị riêng lớn nhất là`3`, vậy câu trả lời là xấp xỉ`1.7320508075688772`. Chỉ nhìn vào độ sẽ gợi ý không chính xác mọi hướng đều có phương sai`2`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xây dựng ma trận hiệp phương sai và thực hiện phân tách giá trị riêng tổng quát. Điều này đơn giản về mặt toán học vì câu trả lời là căn bậc hai của giá trị riêng lớn nhất. Tuy nhiên, việc triển khai một bộ giải riêng đầy đủ là không cần thiết và dễ xảy ra lỗi, đặc biệt khi chúng ta chỉ cần một giá trị riêng. 

Một ý tưởng đơn giản hơn là cố gắng xây dựng các vectơ hệ số ứng cử viên và tìm kiếm phương sai tối đa. Phương sai của vectơ hệ số`a`là`a^T L a`, Ở đâu`L`là người Laplacian. Vì không gian hệ số có vô số hướng khả thi nên việc liệt kê các ứng cử viên là không thể. Ngay cả việc giới hạn các hệ số trong một lưới cũng sẽ đòi hỏi số khả năng theo cấp số nhân. 

Cấu trúc quan trọng đó là`L`là đối xứng. Đối với ma trận đối xứng, việc nhân ma trận nhiều lần sẽ đẩy một vectơ về hướng có giá trị riêng lớn nhất. Đây là phương pháp lặp sức mạnh. Đồ thị Laplacian cũng cho phép nhân mà không lưu trữ đầy đủ ma trận. Đối với mọi cạnh`(u, v)`, phép nhân Laplacian đóng góp`x[u] - x[v]`đến đỉnh`u`Và`x[v] - x[u]`đến đỉnh`v`. 

Lực lượng vũ phu không thành công vì hướng tối ưu là một vectơ riêng liên tục ẩn bên trong không gian hệ số. Quan sát thấy rằng câu trả lời là một giá trị riêng cực trị của ma trận đối xứng cho phép chúng ta thay thế việc tìm kiếm bằng phép nhân vectơ ma trận thưa thớt lặp đi lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Theo cấp số nhân hoặc tệ hơn | Lớn | Quá chậm | 
| Phân hủy bản địa đầy đủ | Phụ thuộc vào việc triển khai, thường là O(n³) | O(n²) | Không cần thiết | 
| Lặp lại sức mạnh | O(lặp × (n + m)) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng biểu diễn đồ thị. Lưu trữ mọi cạnh vì nhân với Laplacian chỉ yêu cầu thăm các cạnh chứ không lưu trữ tất cả`n × n`mục nhập ma trận. 
2. Bắt đầu với một vectơ hệ số khác 0 và chuẩn hóa nó. Vectơ phải có một số thành phần theo hướng của vectơ riêng lớn nhất, nếu không phép lặp lũy thừa sẽ không phát hiện ra nó. 
3. Nhân vectơ hiện tại với Laplacian nhiều lần. Đối với một cạnh kết nối`u`Và`v`, thêm vào`x[u] - x[v]`đến kết quả tại`u`và thêm`x[v] - x[u]`đến kết quả tại`v`. 
4. Chuẩn hóa vectơ kết quả sau mỗi phép nhân. Nếu không chuẩn hóa, các giá trị sẽ tăng theo cấp số nhân với giá trị riêng và cuối cùng trở nên không ổn định về mặt số lượng. 
5. Ước tính giá trị riêng bằng thương số Rayleigh:$$\lambda = \frac{x^T Lx}{x^Tx}$$Đối với vectơ chuẩn hóa, điều này trở nên đơn giản`x · (Lx)`. 

1. Xuất căn bậc hai của giá trị riêng lớn nhất ước tính vì bài toán yêu cầu độ lệch chuẩn thay vì phương sai. 

Lý do điều này hoạt động là vì Laplacian là một ma trận đối xứng thực, do đó nó có cơ sở riêng trực giao. Phép nhân lặp lại chia tỷ lệ cho mỗi thành phần vectơ riêng theo giá trị riêng của nó. Thành phần thuộc giá trị riêng lớn nhất tăng nhanh nhất làm cho vectơ hội tụ về vectơ riêng đó. Thương số Rayleigh sau đó hội tụ về giá trị riêng lớn nhất. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    line = input().split()
    if not line:
        return
    n, m = map(int, line)

    edges = []
    for _ in range(m):
        u, v = map(int, input().split())
        edges.append((u, v))

    vec = [i + 1.0 for i in range(n)]
    norm = math.sqrt(sum(x * x for x in vec))
    vec = [x / norm for x in vec]

    for _ in range(10000):
        nxt = [0.0] * n
        for u, v in edges:
            d = vec[u] - vec[v]
            nxt[u] += d
            nxt[v] -= d

        norm = math.sqrt(sum(x * x for x in nxt))
        if norm == 0:
            print("0.0")
            return
        vec = [x / norm for x in nxt]

    lap_vec = [0.0] * n
    for u, v in edges:
        d = vec[u] - vec[v]
        lap_vec[u] += d
        lap_vec[v] -= d

    eigenvalue = sum(vec[i] * lap_vec[i] for i in range(n))
    print(math.sqrt(max(0.0, eigenvalue)))

if __name__ == "__main__":
    solve()
```Phần đầu vào đọc biểu đồ và chỉ lưu trữ các cạnh. Thuật toán không bao giờ tạo ra ma trận hiệp phương sai đầy đủ vì hầu hết tất cả các mục đều bằng 0 và phép nhân dựa trên cạnh nhanh hơn và sử dụng ít bộ nhớ hơn. 

Vectơ ban đầu sử dụng các giá trị khác nhau thay vì tất cả các giá trị. Vectơ toàn phần luôn là vectơ riêng của Laplacian với giá trị riêng bằng 0, vì vậy bắt đầu với nó sẽ ngay lập tức mất đi hướng hữu ích. 

Vòng lặp nhân thực hiện phép lặp lũy thừa. Phép trừ cho mỗi cạnh chính xác là phép toán Laplacian: mỗi đỉnh nhận được mức độ đóng góp của nó trừ đi các giá trị của các đỉnh lân cận. 

Sau khi hội tụ, mã sẽ tính thương số Rayleigh. các`max`call bảo vệ khỏi các lỗi dấu phẩy động âm nhỏ trước khi lấy căn bậc hai. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
2 1
0 1
```Laplacian là:$$\begin{bmatrix}
1 & -1\\
-1 & 1
\end{bmatrix}$$Giá trị riêng lớn nhất là`2`. 

| Lặp lại | Hướng vector | Giá trị riêng ước tính | 
| --- | --- | --- | 
| Bắt đầu |`(1, 2)`chuẩn hóa | không đo | 
| Nhân | phương hướng tiếp cận`(1, -1)`| gần với`2`| 
| Cuối cùng | vectơ riêng chiếm ưu thế |`2`| 

Đầu ra là:```
1.4142135623730951
```vì giá trị yêu cầu là độ lệch chuẩn, là căn bậc hai của phương sai. 

Đồ thị tam giác:```
3 3
0 1
1 2
2 0
```có bằng cấp`2`ở mọi đỉnh. 

| Lặp lại | Thuộc tính vector | Giá trị riêng ước tính | 
| --- | --- | --- | 
| Bắt đầu | vector khác 0 tùy ý | không ổn định | 
| Phép nhân lặp lại | loại bỏ thành phần có giá trị riêng bằng 0 | cách tiếp cận`3`| 
| Cuối cùng | hướng riêng lớn nhất |`3`| 

Câu trả lời là:```
1.7320508075688772
```Điều này chứng tỏ rằng lời giải phụ thuộc vào toàn bộ cấu trúc đồ thị chứ không chỉ các phương sai riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k(n + m)) |`k`phép lặp lũy thừa, mỗi lần thăm tất cả các đỉnh và cạnh | 
| Không gian | O(n + m) | Lưu trữ danh sách cạnh và một vài vectơ | 

Đây`k`là số lần lặp cố định. Chỉ với`100`đỉnh và`1600`các cạnh, phép nhân ma trận thưa thớt lặp đi lặp lại dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    data = sys.stdin.read().split()
    if not data:
        return ""

    it = iter(data)
    n = int(next(it))
    m = int(next(it))

    edges = []
    for _ in range(m):
        edges.append((int(next(it)), int(next(it))))

    vec = [i + 1.0 for i in range(n)]
    norm = math.sqrt(sum(x * x for x in vec))
    vec = [x / norm for x in vec]

    for _ in range(10000):
        nxt = [0.0] * n
        for u, v in edges:
            d = vec[u] - vec[v]
            nxt[u] += d
            nxt[v] -= d
        norm = math.sqrt(sum(x * x for x in nxt))
        if norm == 0:
            return "0.0"
        vec = [x / norm for x in nxt]

    lap = [0.0] * n
    for u, v in edges:
        d = vec[u] - vec[v]
        lap[u] += d
        lap[v] -= d

    ans = math.sqrt(max(0.0, sum(vec[i] * lap[i] for i in range(n))))
    sys.stdin = old
    return str(ans)

assert abs(float(run("""2 1
0 1
""")) - math.sqrt(2)) < 1e-4, "sample"

assert abs(float(run("""3 3
0 1
1 2
2 0
""")) - math.sqrt(3)) < 1e-4, "triangle"

assert abs(float(run("""1 0
""")) - 0.0) < 1e-4, "isolated vertex"

assert abs(float(run("""4 2
0 1
2 3
""")) - math.sqrt(2)) < 1e-4, "two components"

assert abs(float(run("""5 4
0 1
1 2
2 3
3 4
""")) - math.sqrt(3.6180339887)) < 1e-4, "path graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đỉnh kết nối |`sqrt(2)`| Trường hợp giá trị riêng Laplacian cơ bản | 
| Tam giác |`sqrt(3)`| Đồ thị mức độ bằng nhau | 
| Đỉnh đơn |`0`| Vỏ cạnh trống | 
| Hai cạnh bị ngắt kết nối |`sqrt(2)`| Đồ thị bị ngắt kết nối | 
| Đường dẫn năm đỉnh | Khoảng`1.9021`| Bằng cấp không đồng nhất | 

## Vỏ cạnh 

Đối với đồ thị có một đỉnh cô lập:```
1 0
```Laplacian là ma trận 0 từng cái một. Phép lặp lũy thừa phát hiện phép nhân tạo ra vectơ 0 và trả về`0`. Độ lệch chuẩn chính xác bằng 0 vì không có phương sai. 

Đối với các thành phần bị ngắt kết nối:```
4 2
0 1
2 3
```thuật toán không cần bất kỳ xử lý đặc biệt nào. Phép nhân Laplacian hoạt động độc lập trên từng thành phần. Phép lặp lũy thừa tìm thành phần chứa giá trị riêng lớn nhất, chính xác là giá trị tối đa toàn cục yêu cầu. 

Đối với biểu đồ thông thường:```
3 3
0 1
1 2
2 0
```tất cả các đỉnh đều có cùng phương sai, nhưng câu trả lời không chỉ đến từ mức độ chung đó. Các phép nhân lặp lại nắm bắt được sự tương tác giữa các số hạng hiệp phương sai, dẫn đến giá trị riêng lớn nhất chính xác`3`. 

Đối với các đồ thị nhỏ, độ chính xác của dấu phẩy động là rủi ro chính. Thuật toán tránh phép chia cho gần bằng 0 bằng cách kiểm tra chuẩn vectơ sau khi nhân và nó kẹp các ước tính giá trị riêng âm cực nhỏ trước khi áp dụng căn bậc hai.
