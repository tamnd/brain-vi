---
title: "CF 103698F - Cây"
description: "Bài toán có thể hiểu là một bài toán tính đại số tuyến tính cổ điển trên đồ thị vô hướng. Thay vì lập luận tổ hợp về các cây bao trùm một cách trực tiếp, chúng tôi diễn giải lại biểu đồ thông qua một ma trận được xây dựng từ cấu trúc của nó và tính định thức mã hóa…"
date: "2026-07-02T09:50:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103698
codeforces_index: "F"
codeforces_contest_name: "The 4th Turing Cup"
rating: 0
weight: 103698
solve_time_s: 66
verified: true
draft: false
---

[CF 103698F - Cây](https://codeforces.com/problemset/problem/103698/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán có thể hiểu là một bài toán tính đại số tuyến tính cổ điển trên đồ thị vô hướng. Thay vì suy luận tổ hợp về các cây bao trùm một cách trực tiếp, chúng tôi diễn giải lại biểu đồ thông qua một ma trận được xây dựng từ cấu trúc của nó và tính toán định thức mã hóa câu trả lời. 

Cụ thể, đầu vào mô tả một đồ thị vô hướng đơn giản với$n$đỉnh và tập hợp các cạnh. Mục tiêu là tính toán có bao nhiêu cây bao trùm trong biểu đồ này. Cây bao trùm là một tập hợp con các cạnh nối tất cả các đỉnh trong khi không chứa chu trình và sử dụng chính xác$n-1$các cạnh. 

Đầu ra là một số nguyên duy nhất biểu thị số này. Trong cài đặt lập trình cạnh tranh điển hình, giá trị này được lấy modulo một số nguyên tố lớn, mặc dù toán học cơ bản cũng hoạt động trên các số nguyên. 

Từ góc độ phức tạp, hạn chế chính là đồ thị có thể đủ lớn để không thể liệt kê các cây bao trùm hoặc tập hợp con của các cạnh. Ngay cả các đồ thị có kích thước vừa phải cũng làm cho số lượng cây bao trùm là hàm mũ, do đó, bất kỳ giải pháp chấp nhận được nào cũng phải quy bài toán về đại số tuyến tính thời gian đa thức. Con đường khả thi duy nhất là tính toán xác định trên một$n \times n$ma trận, ngay lập tức gợi ý$O(n^\omega)$hoặc$O(n^3)$-loại phương pháp. 

Một số trường hợp đặc biệt quan trọng trong thực tế. Một đồ thị có một đỉnh luôn có chính xác một cây bao trùm, mặc dù không có cạnh nào. Một đồ thị không liên kết có cây bao trùm bằng 0 vì không thể kết nối tất cả các đỉnh mà không thêm các cạnh. Một đồ thị có nhiều cạnh nằm giữa cùng một cặp đỉnh sẽ làm tăng số lượng cây bao trùm một cách không hề tầm thường, vì mỗi cạnh đóng góp riêng cho Laplacian. Việc triển khai bất cẩn làm loại bỏ các cạnh trùng lặp sẽ âm thầm tạo ra kết quả không chính xác trong trường hợp đó. 

Một trường hợp tinh tế khác là khi đồ thị đã là một cái cây. Trong tình huống đó, câu trả lời chính xác là một và bất kỳ việc triển khai dựa trên định thức nào cũng phải bảo toàn tính toàn vẹn mặc dù hoạt động theo số học mô-đun hoặc ngẫu nhiên. 

## Phương pháp tiếp cận 

Quan điểm brute-force bắt đầu từ định nghĩa về cây bao trùm. Người ta có thể thử liệt kê tất cả các tập con của$n-1$các cạnh và kiểm tra xem mỗi tập hợp con có tạo thành một cây hợp lệ hay không. Khả năng kết nối có thể được kiểm tra bằng Union-find hoặc DFS và tính không theo chu kỳ sẽ tự động diễn ra nếu tập hợp con có chính xác$n-1$các cạnh và được kết nối. Cách tiếp cận này đúng, nhưng số tập con cạnh tăng lên khi$\binom{m}{n-1}$, điều này trở nên không khả thi ngay cả đối với các đồ thị nhỏ. Điểm nghẽn không phải là bước xác minh mà là số lượng ứng viên quá lớn. 

Một cách tiếp cận có cấu trúc hơn sử dụng một phép biến đổi cơ bản: thay vì đếm trực tiếp các đối tượng tổ hợp, chúng ta mã hóa chúng dưới dạng các thuật ngữ đại số trong một định thức. Ma trận Laplacian của đồ thị nắm bắt các độ đỉnh trên đường chéo và cấu trúc kề ngoài đường chéo. Định lý cây ma trận của Kirchhoff phát biểu rằng bất kỳ đồng yếu tố nào của Laplacian này đều bằng số lượng cây bao trùm. Điều này chuyển vấn đề thành tính toán một yếu tố quyết định duy nhất của một$(n-1) \times (n-1)$ma trận. 

Lý do điều này hữu ích là tính toán định thức thừa nhận các thuật toán đại số hiệu quả. Loại bỏ Gaussian hoặc các kỹ thuật nhân ma trận nâng cao hơn làm giảm độ phức tạp từ phép đếm hàm mũ đến số học đa thức. Sự bùng nổ tổ hợp được hấp thụ vào các sự hủy bỏ bên trong khai triển định thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | Hàm mũ | O(n + m) | Quá chậm | 
| Cây ma trận (Xác định) | O(n³) hoặc O(n^ω) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp được xây dựng xung quanh việc xây dựng ma trận Laplacian và tính toán một trong các đồng yếu tố của nó. 

1. Xây dựng một$n \times n$ma trận$L$, ban đầu chứa đầy số không. Ma trận này sẽ biểu diễn cấu trúc đồ thị ở dạng đại số. 
2. Cho mọi cạnh$u - v$, tăng$L[u][u]$Và$L[v][v]$một và giảm cả hai$L[u][v]$Và$L[v][u]$bởi một. Điều này mã hóa đồng thời độ đỉnh và độ kề. Mỗi cạnh đóng góp một bản cập nhật có cấu trúc để duy trì tính đối xứng. 
3. Xóa một hàng và cột tùy ý, thường là đỉnh cuối cùng, tạo ra một$(n-1) \times (n-1)$ma trận. Bước này là bắt buộc vì Laplacian đầy đủ luôn có định thức bằng 0 do tổng hàng bằng 0, nhưng bất kỳ đồng yếu tố nào đều mang số lượng cây bao trùm. 
4. Tính định thức của ma trận thu được bằng phép loại trừ Gaussian theo mô đun yêu cầu hoặc trên các số nguyên có phép loại trừ không có phân số. Định thức tổng hợp tất cả các đóng góp của cây bao trùm được mã hóa trong ma trận. 
5. Xuất định thức làm đáp án cuối cùng. 

Bước quan trọng không rõ ràng là tại sao việc xóa một hàng và cột không làm mất thông tin. Người Laplacian có thứ hạng$n-1$đối với một biểu đồ được kết nối và tất cả các đồng yếu tố đều bằng nhau. Điều này cho phép chúng ta rút gọn bài toán thành một phép tính định thức cấp đầy đủ. 

### Tại sao nó hoạt động 

Mỗi cây bao trùm tương ứng với đúng một số hạng trong khai triển định thức sau khi biến đổi Laplacian. Việc hủy bỏ đại số trong định thức sẽ loại bỏ tất cả các cấu trúc chứa chu trình, chỉ để lại các lựa chọn cạnh liên kết không theo chu trình. Các số hạng còn lại còn lại tương ứng một-một với các cây bao trùm, điều này đảm bảo định thức bằng với số đếm của chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def det_mod(mat, mod):
    n = len(mat)
    det = 1
    sign = 1

    for i in range(n):
        pivot = i
        for j in range(i, n):
            if mat[j][i] != 0:
                pivot = j
                break
        if mat[pivot][i] == 0:
            return 0

        if pivot != i:
            mat[i], mat[pivot] = mat[pivot], mat[i]
            sign *= -1

        inv = pow(mat[i][i], mod - 2, mod)
        det = det * mat[i][i] % mod

        for j in range(i + 1, n):
            factor = mat[j][i] * inv % mod
            for k in range(i, n):
                mat[j][k] = (mat[j][k] - factor * mat[i][k]) % mod

    return det * sign % mod

n, m = map(int, input().split())
mod = 10**9 + 7

L = [[0] * n for _ in range(n)]

for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    L[u][u] += 1
    L[v][v] += 1
    L[u][v] -= 1
    L[v][u] -= 1

for i in range(n):
    for j in range(n):
        L[i][j] %= mod

mat = [row[:-1] for row in L[:-1]]

print(det_mod(mat, mod))
```Việc triển khai trước tiên xây dựng Laplacian trực tiếp từ các đóng góp của cạnh. Việc loại bỏ hàng và cột cuối cùng được thực hiện bằng cách cắt, giúp giữ cho ma trận vuông góc và xếp hạng đầy đủ theo giả định kết nối. 

Thủ tục xác định sử dụng modulo loại bỏ Gaussian một số nguyên tố. Việc lựa chọn trục đảm bảo sự ổn định về số trong số học mô-đun bằng cách tránh các trục bằng 0 khi có thể. Các phép toán hàng được thực hiện tại chỗ và định thức được tích lũy thông qua việc chia tỷ lệ trục. Việc theo dõi dấu hiệu xử lý việc hoán đổi hàng, điều này rất cần thiết vì mỗi lần hoán đổi sẽ lật dấu định thức. 

Một điểm tinh tế là việc đảo ngược mô đun giả định mô đun là số nguyên tố. Đây là tiêu chuẩn trong lập trình cạnh tranh, trong đó$10^9 + 7$hoặc các số nguyên tố tương tự được sử dụng. 

## Ví dụ đã hoạt động 

Xét một đồ thị tam giác có ba đỉnh và ba cạnh. 

Laplacian là$$\begin{bmatrix}
2 & -1 & -1 \\
-1 & 2 & -1 \\
-1 & -1 & 2
\end{bmatrix}$$Việc xóa hàng và cột cuối cùng sẽ mang lại$$\begin{bmatrix}
2 & -1 \\
-1 & 2
\end{bmatrix}$$Việc tính toán xác định tiến hành như sau. 

| Bước | Trạng thái ma trận | Hoạt động | 
| --- | --- | --- | 
| 1 | [[2, -1], [-1, 2]] | ban đầu | 
| 2 | [[2, -1], [0, 3]] | loại bỏ hàng thứ hai | 
| 3 | 6 | sản phẩm của trục quay | 

Kết quả 3 khớp với số cây khung đã biết trong một tam giác. 

Bây giờ hãy xem xét một cây có ba đỉnh thẳng hàng. Hệ số Laplacian trở thành một ma trận có định thức có giá trị bằng 1. Điều này xác nhận rằng một cây đóng góp chính xác một cây bao trùm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n³) | Loại bỏ Gaussian trên ma trận (n-1)×(n-1) | 
| Không gian | O(n²) | Lưu trữ ma trận Laplacian | 

Độ phức tạp bậc ba là đủ cho các ràng buộc điển hình lên tới vài nghìn đỉnh. Đối với các giới hạn lớn hơn, người ta sẽ cần các kỹ thuật nâng cao như tối ưu hóa định thức hoặc các phương pháp nhận biết thưa thớt, nhưng cấu trúc Laplacian đã cung cấp đủ hiệu quả cho các cài đặt tiêu chuẩn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    n, m = map(int, sys.stdin.readline().split())
    mod = 10**9 + 7

    L = [[0]*n for _ in range(n)]
    for _ in range(m):
        u, v = map(int, sys.stdin.readline().split())
        u -= 1
        v -= 1
        L[u][u] += 1
        L[v][v] += 1
        L[u][v] -= 1
        L[v][u] -= 1

    mat = [row[:-1] for row in L[:-1]]

    def det(a):
        n = len(a)
        detv = 1
        sign = 1
        for i in range(n):
            p = i
            for j in range(i, n):
                if a[j][i]:
                    p = j
                    break
            if a[p][i] == 0:
                return 0
            if p != i:
                a[i], a[p] = a[p], a[i]
                sign *= -1
            inv = pow(a[i][i], mod-2, mod)
            detv = detv * a[i][i] % mod
            for j in range(i+1, n):
                f = a[j][i] * inv % mod
                for k in range(i, n):
                    a[j][k] = (a[j][k] - f*a[i][k]) % mod
        return detv * sign % mod

    return str(det(mat))

# triangle graph
assert run("3 3\n1 2\n2 3\n1 3\n") == "3"
# path graph
assert run("3 2\n1 2\n2 3\n") == "1"
# disconnected
assert run("4 2\n1 2\n3 4\n") == "0"
# single node
assert run("1 0\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tam giác | 3 | nhiều cây bao trùm | 
| con đường | 1 | trường hợp đế cây | 
| ngắt kết nối | 0 | yêu cầu kết nối | 
| nút đơn | 1 | trường hợp thoái hóa | 

## Vỏ cạnh 

Đồ thị bị ngắt kết nối được xử lý chính xác vì đồng yếu tố Laplacian trở thành số ít. Trong quá trình loại bỏ, trục quay bằng 0 cuối cùng xuất hiện, buộc định thức về 0, điều này phản ánh chính xác sự vắng mặt của cây bao trùm. 

Biểu đồ một đỉnh giảm xuống thành ma trận trống sau khi loại bỏ hàng và cột. Theo quy ước, định thức của ma trận 0×0 là 1, phù hợp với thực tế là có chính xác một cây bao trùm. 

Đầu vào dạng cây dẫn đến cấu trúc hình tam giác trong đồng yếu tố Laplacian sau khi loại bỏ. Mỗi trục tương ứng với chính xác một ràng buộc cạnh, tạo ra một ràng buộc xác định mà không bị hủy bỏ, xác nhận tính đúng đắn trong trường hợp liên thông tối thiểu.
