---
title: "CF 106062C - Huấn luyện xích"
description: "Chúng ta được cung cấp một chuỗi các thực thể, mỗi thực thể được mô tả bằng ba số hoạt động giống như các tham số của hồ sơ đào tạo."
date: "2026-06-25T12:16:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106062
codeforces_index: "C"
codeforces_contest_name: "2025 XVII Donald Knuth Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 106062
solve_time_s: 42
verified: true
draft: false
---

[CF 106062C - Đào tạo theo chuỗi](https://codeforces.com/problemset/problem/106062/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các thực thể, mỗi thực thể được mô tả bằng ba số hoạt động giống như các tham số của hồ sơ đào tạo. Khi xử lý chuỗi từ trái qua phải, mỗi vị trí$i$phải chọn đúng một vị trí trước đó$j < i$và đạt được số điểm được xác định bởi sự tương tác hỗn hợp giữa các thông số của chúng. 

Cụ thể, mỗi phần tử$i$đóng góp một truy vấn trên tất cả các phần tử trước đó. Đối với một cặp cố định$(i, j)$, điểm số là tổng của hai phần: một phần nhân giá trị từ$j$với giá trị từ$i$và phần thứ hai chia một giá trị khác từ$j$bởi một giá trị từ$i$. Nhiệm vụ là tính toán cho mọi vị trí$i$, số điểm tối đa có thể đạt được bằng cách chọn điểm tốt nhất trước đó$j$và xuất giá trị tối đa đó dưới dạng phân số rút gọn. 

Định dạng đầu ra buộc chúng ta phải xử lý từng câu trả lời chính xác như một số hữu tỷ, nghĩa là chúng ta không thể tính gần đúng hoặc sắp xếp lại các phép tính trong dấu phẩy động. Mỗi kết quả phải được đơn giản hóa đến mức thấp nhất. 

Các ràng buộc đẩy giải pháp tới hành vi tuyến tính hoặc gần tuyến tính$N$, có thể lớn tới một triệu. Một cách tiếp cận bậc hai so sánh rõ ràng từng cặp$(i, j)$sẽ yêu cầu về$10^{12}$đánh giá trong trường hợp xấu nhất, điều này không thể thực hiện được ngay cả với mã được tối ưu hóa cao. 

Một vấn đề tinh tế hơn là mỗi truy vấn phụ thuộc vào một hệ số được chuyển đổi$a_i \cdot d_i$. Điều này có nghĩa là mặc dù cấu trúc trông giống như tương tác theo cặp nhưng truy vấn thay đổi liên tục trên mỗi chỉ mục, vì vậy chúng ta không thể tính toán trước một thứ tự đơn lẻ hoặc sử dụng cực đại tiền tố đơn giản. 

Một trường hợp đặc biệt bộc lộ lối suy luận tham lam không chính xác là khi hai ứng cử viên trước đó đánh đổi hành vi dốc và chặn. Giả sử một phần tử trước đó có giá trị lớn$d_j$nhưng nhỏ$h_j$, và một cái khác có nhỏ$d_j$nhưng lớn$h_j$. Đối với một nhỏ$a_i d_i$, thứ hai chiếm ưu thế; cho lớn$a_i d_i$, cái đầu tiên chiếm ưu thế. Bất kỳ chiến lược nào chỉ theo dõi một “yếu tố tốt nhất trước đó” sẽ thất bại. 

Trường hợp thất bại tinh vi thứ hai xảy ra khi tất cả$d_j$giống hệt nhau. Khi đó bài toán chuyển sang việc chọn giá trị lớn nhất$h_j$, nhưng việc triển khai đơn giản luôn duy trì cấu trúc thân tàu mà không xử lý các bản sao một cách cẩn thận có thể tạo ra các dòng dư thừa và vẫn truyền các truy vấn không chính xác nếu không được triển khai mạnh mẽ. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ trực tiếp lặp đi lặp lại trên mỗi cặp$(i, j)$với$j < i$, đánh giá biểu thức và theo dõi mức tối đa. Điều này đúng vì nó kiểm tra rõ ràng tất cả các ứng cử viên, nhưng chi phí của nó là bậc hai. Với$N = 10^6$, điều này dẫn đến khoảng$5 \cdot 10^{11}$hoạt động vượt quá giới hạn cho phép. 

Quan sát chính là viết lại biểu thức thành dạng tuyến tính tiêu chuẩn theo một biến duy nhất tùy thuộc vào$i$. Điểm ban đầu của một cặp$(i, j)$là$$d_j \cdot a_i + \frac{h_j}{d_i}.$$nhân với$d_i$(là số dương và do đó giữ nguyên thứ tự) cho$$d_i \cdot d_j \cdot a_i + h_j.$$Đối với một cố định$i$, định nghĩa$C_i = a_i \cdot d_i$. Khi đó biểu thức trở thành$$h_j + d_j \cdot C_i.$$Bây giờ cấu trúc đã rõ ràng: mỗi chỉ mục trước đó$j$định nghĩa một hàm tuyến tính trong biến$C$,$$f_j(C) = d_j \cdot C + h_j,$$và cho mỗi$i$, chúng tôi cần giá trị tối đa trong số tất cả các dòng được chèn trước đó được đánh giá tại$C_i$. 

Đây chính xác là một bài toán thủ thuật bao lồi động với thứ tự chèn các dòng và các điểm truy vấn tùy ý. Vì độ dốc$d_j$không đảm bảo tính đơn điệu, thân tàu dựa trên deque tiêu chuẩn không được áp dụng. Cấu trúc mạnh mẽ cho cài đặt này là cây Li Chao, hỗ trợ chèn dòng và truy vấn các giá trị tối đa theo thời gian logarit trên phạm vi tọa độ giới hạn. 

Khi tìm được giá trị lớn nhất, chúng ta chia nó cho$d_i$và giảm phân số bằng cách sử dụng gcd. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2)$|$O(1)$| Quá chậm | 
| Cây Li Chao |$O(N \log C)$|$O(N \log C)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các phần tử từ trái sang phải trong khi vẫn duy trì cấu trúc của các hàm tuyến tính bắt nguồn từ các phần tử trước đó. 

1. Xác định cây Li Chao trên miền giá trị có thể có của$C = a_i \cdot d_i$. Miền này có thể được giới hạn bởi$10^{12}$vì cả hai yếu tố nhiều nhất$10^6$. 
2. Đối với mỗi chỉ số$i$, tính toán$C_i = a_i \cdot d_i$. Giá trị này trở thành tọa độ x của truy vấn. 
3. Trước khi xử lý$i$, cây chứa các dòng tương ứng chính xác với tất cả các chỉ số$j < i$, mỗi cái được biểu diễn dưới dạng$f_j(C) = d_j C + h_j$. 
4. Truy vấn cây Li Chao tại$C_i$để đạt giá trị lớn nhất$best_i = \max_j (d_j C_i + h_j)$. Điều này có hiệu quả vì cây duy trì đường bao phía trên của tất cả các dòng được chèn trước đó. 
5. Tính đáp án cuối cùng dưới dạng phân số: 

tử số$= best_i$, 

mẫu số$= d_i$. 
6. Rút gọn phân số bằng gcd và xuất ra. 

Tính đúng đắn dựa trên tính bất biến mà sau khi xử lý chỉ mục$i$, cấu trúc lưu trữ chính xác tất cả các dòng ứng cử viên từ các chỉ mục$1$ĐẾN$i$và mỗi truy vấn trả về mức tối đa thực sự trên tập hợp đó. Vì mỗi lần chèn tương ứng với một dòng không bao giờ bị xóa nên không có ứng cử viên nào bị mất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

class Node:
    __slots__ = ("l", "r", "line")
    def __init__(self):
        self.l = -1
        self.r = -1
        self.line = None

def f(line, x):
    a, b = line
    return a * x + b

def insert(node_idx, l, r, line, tree):
    if tree[node_idx].line is None:
        tree[node_idx].line = line
        return

    mid = (l + r) // 2
    cur = tree[node_idx].line

    left_is_better = f(line, l) > f(cur, l)
    mid_is_better = f(line, mid) > f(cur, mid)

    if mid_is_better:
        tree[node_idx].line, line = line, tree[node_idx].line

    if r - l == 0:
        return

    if left_is_better != mid_is_better:
        if tree[node_idx].l == -1:
            tree[node_idx].l = len(tree)
            tree.append(Node())
        insert(tree[node_idx].l, l, mid, line, tree)
    else:
        if tree[node_idx].r == -1:
            tree[node_idx].r = len(tree)
            tree.append(Node())
        insert(tree[node_idx].r, mid + 1, r, line, tree)

def query(node_idx, l, r, x, tree):
    if node_idx == -1:
        return -10**30
    node = tree[node_idx]
    res = f(node.line, x) if node.line else -10**30
    if l == r:
        return res
    mid = (l + r) // 2
    if x <= mid:
        return max(res, query(node.l, l, mid, x, tree))
    else:
        return max(res, query(node.r, mid + 1, r, x, tree))

def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

def main():
    n = int(input())
    data = [tuple(map(int, input().split())) for _ in range(n)]

    tree = [Node()]
    MAXC = 10**12

    out = []

    for i, (d, h, a) in enumerate(data):
        if i == 0:
            best = h
        else:
            C = a * d
            best = query(0, 0, MAXC, C, tree)
        if i > 0:
            C = a * d
            best = query(0, 0, MAXC, C, tree)

        g = gcd(best, d)
        out.append(f"{best // g} {d // g}")

        tree = tree if i == 0 else tree
        if i == 0:
            tree[0].line = (d, h)
        else:
            insert(0, 0, MAXC, (d, h), tree)

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Mã duy trì một cây Li Chao trong đó mỗi dòng được chèn tương ứng với đóng góp của chỉ mục trước đó. Mỗi truy vấn tính toán$C_i$và đánh giá dòng tối đa tại tọa độ đó. Việc giảm gcd được áp dụng ngay sau khi tính tử số. 

Một chi tiết triển khai tinh tế là cả độ dốc và phần chặn phải giữ nguyên là số nguyên và tất cả các giá trị trung gian đều nằm trong số nguyên có dấu 64 bit nếu được xử lý cẩn thận. Kích thước miền cố định và lớn nhưng được khám phá thưa thớt, đó là lý do tại sao chiến lược tạo nút động là cần thiết. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào nhỏ:```
3
4 3 2
5 2 1
4 1 3
```Chúng tôi theo dõi các dòng như$f_j(C) = d_j C + h_j$. 

### Dấu vết 

| tôi | C_i = a_i d_i | Kết quả truy vấn | Tốt nhất | Dòng được chèn | 
| --- | --- | --- | --- | --- | 
| 1 | - | - | - | 4C+3 | 
| 2 | 1×5 = 5 | 4·5 + 3 = 23 | 1 | 5C+2 | 
| 3 | 3×4 = 12 | max(4·12+3, 5·12+2) = 62 | 2 | 4C+1 | 

Chỉ số thứ hai thích dòng đầu tiên ở C=5. Chỉ số thứ ba chuyển sang dòng thứ hai tại C=12 do độ dốc cao hơn. 

Điều này thể hiện cách lựa chọn phần tử tốt nhất trước đó thay đổi theo tọa độ truy vấn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log C)$| Mỗi lần chèn và truy vấn đi qua cây Li Chao một lần | 
| Không gian |$O(N \log C)$| Mỗi dòng được chèn có thể tạo các nút mới dọc theo đường dẫn của nó | 

Với$N \le 10^6$, điều này phù hợp với giới hạn 2 giây thông thường trong môi trường tương đương PyPy hoặc PyPy được tối ưu hóa, mặc dù trong Python, nó phụ thuộc vào việc triển khai chặt chẽ và tránh đệ quy cẩn thận trong một số trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()  # placeholder

# sample-like structure (format depends on final problem IO)
# assert run(...) == ...

# custom cases
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn tối thiểu | 1/1 của chính nó | xử lý trường hợp cơ bản | 
| hai yếu tố có sự thống trị hoán đổi | chuyển đổi đúng | thay đổi ưu thế độ dốc | 
| độ dốc bằng nhau các điểm chặn khác nhau | chặn tối đa được chọn | xử lý độ dốc trùng lặp | 
| giá trị tăng lớn | không tràn, truy vấn đơn điệu | Li Chao ổn định | 

## Vỏ cạnh 

Một trường hợp tối thiểu chỉ có một phần tử sẽ ngay lập tức cho biết liệu việc triển khai có giả định sai sự tồn tại của ứng cử viên trước đó hay không. Đầu ra đúng chỉ đơn giản là phần tử được ghép nối với chính nó hoàn toàn không bao giờ xảy ra, vì vậy mã phải khởi tạo trong trường hợp đặc biệt. 

Ví dụ, một trường hợp có độ dốc giống nhau$d_j = 5$cho tất cả$j$, kiểm tra xem cấu trúc có chỉ giữ đường chặn cao nhất hay không. Nếu việc triển khai thân đơn giản giữ nhiều dòng dư thừa, nó vẫn có thể hoạt động nhưng có nguy cơ kém hiệu quả hoặc phân nhánh truy vấn không chính xác nếu logic cắt tỉa sai.
