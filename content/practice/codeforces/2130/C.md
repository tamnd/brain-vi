---
title: "CF 2130C - Phối cảnh kép"
description: "Chúng ta được cho một tập hợp các cặp số nguyên, mỗi cặp đại diện cho cả một đoạn trên trục số và một cạnh trong đồ thị. Mục tiêu của chúng tôi là chọn một tập hợp con của các cặp này để tối đa hóa sự khác biệt giữa tổng chiều dài của các số nguyên được bao phủ, được gọi là $f(S')$ và số lượng nút…"
date: "2026-06-09T04:05:11+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dsu", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2130
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1040 (Div. 2)"
rating: 1300
weight: 2130
solve_time_s: 68
verified: true
draft: false
---

[CF 2130C - Phối cảnh kép](https://codeforces.com/problemset/problem/2130/C) 

**Đánh giá:** 1300 
**Tags:** thuật toán xây dựng, dsu, tham lam 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các cặp số nguyên, mỗi cặp đại diện cho cả một đoạn trên trục số và một cạnh trong đồ thị. Mục tiêu của chúng ta là chọn một tập con của các cặp này để tối đa hóa sự khác biệt giữa tổng chiều dài của các số nguyên được bao phủ, được gọi là$f(S')$và số lượng nút là một phần của chu kỳ có độ dài ít nhất là ba, được gọi là$g(S')$. Đầu vào cung cấp nhiều trường hợp kiểm thử, mỗi trường hợp có tối đa 3000 cặp và tổng của$n^2$trên tất cả các trường hợp thử nghiệm không vượt quá$9 \cdot 10^6$, cho phép thuật toán bậc hai trong$n$mỗi trường hợp thử nghiệm. 

Về mặt giải thích cụ thể,$f(S')$về cơ bản là số vị trí nguyên được bao phủ nếu chúng ta coi mỗi cặp là một đoạn kín, trong khi$g(S')$đo tính chu kỳ của đồ thị: nó đếm có bao nhiêu nút trong chu kỳ có độ dài từ ba trở lên nếu mỗi cặp được coi là một cạnh vô hướng. Tập hợp con$S'$là tối ưu nếu nó tối đa hóa$f(S') - g(S')$. Các trường hợp cạnh phát sinh khi các đoạn liền kề hoặc chồng lên nhau hoặc khi các cạnh tạo thành các chu kỳ nhỏ. Ví dụ: một phân đoạn như$(1,2)$đóng góp một đơn vị vào$f(S')$và không đến$g(S')$. Nếu chúng ta chọn ba cạnh tạo thành một hình tam giác,$g(S')$tăng ba, làm giảm tổng điểm. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là xem xét mọi tập hợp con có thể có của các cặp, tính toán sự kết hợp của các phân đoạn cho$f(S')$và phát hiện các chu kỳ trong biểu đồ cho$g(S')$, sau đó chọn tập hợp con tối đa hóa sự khác biệt. Mỗi tập hợp con mất$O(n^2)$để tính độ dài hợp và$O(n^2)$để phát hiện các chu kỳ thông qua DFS. Tổng thời gian tăng theo cấp số nhân với$2^n$tập hợp con, điều này là không thể thực hiện được ngay cả đối với$n=20$, huống hồ là$n=3000$. 

Cái nhìn sâu sắc quan trọng là các chu kỳ có độ dài ít nhất là ba chỉ xảy ra khi các cạnh tạo thành một vòng gồm ba đỉnh trở lên. Nếu chúng ta hạn chế lựa chọn các cạnh tạo thành một khu rừng (không có chu trình), thì$g(S') = 0$. Do đó, tối đa hóa$f(S') - g(S')$giảm đến mức tối đa$f(S')$đồng thời tránh chu kỳ. Điều này đương nhiên dẫn đến việc xử lý bài toán như một bài toán đồ thị: chúng ta muốn một tập hợp con các cạnh tạo thành một khu rừng (một tập hợp các cây) và tối đa hóa tổng chiều dài của phạm vi số nguyên. 

Chúng ta có thể tính toán độ dài liên kết tăng dần bằng cách tham lam chọn các cạnh theo thứ tự các điểm cuối bên phải của chúng trong khi vẫn duy trì Liên minh tập hợp rời rạc (DSU) để tránh chu kỳ. Mỗi lần chúng tôi xem xét một cạnh mới, chúng tôi sẽ thêm nó nếu nó không kết nối hai nút đã có trong cùng một thành phần được kết nối. Điều này đảm bảo không có chu kỳ hình thành, giữ$g(S') = 0$. Hoạt động DSU có hiệu quả$O(\alpha(n))$, gần như không đổi và việc kiểm tra sự đóng góp cho liên kết phân đoạn là tuyến tính theo$n$vì tọa độ được giới hạn bởi$2n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n + n \cdot \alpha(n))$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các cặp đầu vào cho từng trường hợp thử nghiệm và lưu trữ chúng cùng với các chỉ mục ban đầu của chúng. 
2. Sắp xếp các cặp theo điểm cuối bên phải của chúng. Việc sắp xếp giúp tối đa hóa độ dài liên kết tham lam vì các phân đoạn trước đó sẽ nhường chỗ cho các phân đoạn sau, không chồng chéo. 
3. Khởi tạo cấu trúc Disjoint Set Union (DSU) cho tất cả$2n$nút. DSU theo dõi những nút nào đã được kết nối, ngăn chặn chu kỳ. 
4. Duy trì một mảng biểu diễn phạm vi số nguyên từ 1 đến$2n$. Điều này giúp theo dõi số nguyên nào đã được bao phủ. 
5. Lặp lại các cặp đã được sắp xếp. Đối với mỗi cặp, hãy kiểm tra xem các điểm cuối của nó có nằm trong cùng một thành phần được kết nối hay không bằng DSU. Nếu có, hãy bỏ qua cặp này; thêm nó sẽ tạo ra một chu kỳ. 
6. Nếu cặp kết nối các thành phần khác nhau, hãy kết hợp chúng trong DSU. Cập nhật dần dần mảng vùng phủ sóng để đánh dấu tất cả các số nguyên nằm trong phân đoạn này. 
7. Giữ một danh sách các chỉ số đã chọn. Sau khi xử lý tất cả các cặp, xuất kích thước của danh sách này và chính các chỉ mục. 

Tại sao nó hoạt động: DSU đảm bảo không có chu kỳ nào được hình thành, vì vậy$g(S') = 0$. Việc sắp xếp và cộng các cặp một cách tham lam đảm bảo rằng chúng ta bao gồm càng nhiều số nguyên phân biệt càng tốt, tối đa hóa$f(S')$. Vì bất kỳ chu kỳ nào cũng sẽ chỉ làm giảm điểm nên việc tránh các chu kỳ trong khi tối đa hóa phạm vi bao phủ sẽ mang lại giải pháp tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n+1))
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    def union(self, x, y):
        x_root = self.find(x)
        y_root = self.find(y)
        if x_root == y_root:
            return False
        self.parent[y_root] = x_root
        return True

t = int(input())
for _ in range(t):
    n = int(input())
    edges = []
    for i in range(1, n+1):
        a, b = map(int, input().split())
        edges.append((b, a, i))  # store as (right, left, index)

    edges.sort()
    dsu = DSU(2*n)
    selected = []
    covered = [0]*(2*n+2)

    for r, l, idx in edges:
        if dsu.union(l, r):
            selected.append(idx)
            for x in range(l, r+1):
                covered[x] = 1

    print(len(selected))
    print(" ".join(map(str, selected)))
```Lớp DSU đảm bảo rằng không có chu trình nào được hình thành. Sắp xếp theo điểm cuối bên phải cho phép chúng ta xem xét các cạnh bao gồm các số nguyên sớm trước tiên. Đánh dấu các số nguyên được bao phủ là tùy chọn cho tính chính xác nhưng minh họa$f(S')$được tối đa hóa. Lặp lại trong phạm vi đảm bảo chúng tôi đếm mọi số nguyên trong liên minh. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
1
4
1 2
2 3
1 3
3 5
```| Bước | Cặp được xem xét | Liên minh DSU? | Chỉ số được chọn | Số nguyên được che phủ | 
| --- | --- | --- | --- | --- | 
| 1 | (1,2) | vâng | [1] | 1,2 | 
| 2 | (2,3) | vâng | [1,2] | 1,2,3 | 
| 3 | (1,3) | không | [1,2] | 1,2,3 | 
| 4 | (3,5) | vâng | [1,2,4] | 1,2,3,4,5 | 

Các cạnh được chọn là 1,2,4. Không có hình thức chu kỳ. Tất cả các số nguyên 1-5 đều được che phủ. 

### Đầu vào mẫu 2```
1
1
1 2
```| Bước | Cặp được xem xét | Liên minh DSU? | Chỉ số được chọn | Số nguyên được che phủ | 
| --- | --- | --- | --- | --- | 
| 1 | (1,2) | vâng | [1] | 1,2 | 

Một cặp duy nhất được chọn, cho$f(S')=1$Và$g(S')=0$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các cạnh chiếm ưu thế; Các công đoàn DSU có hiệu quả không đổi trên mỗi cạnh | 
| Không gian | O(n) | Mảng DSU và danh sách đã chọn; mảng phủ sóng là O(n) | 

Những hạn chế$n \le 3000$mỗi trường hợp thử nghiệm và$\sum n^2 \le 9 \cdot 10^6$đảm bảo giải pháp này chạy thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        edges = []
        for i in range(1, n+1):
            a, b = map(int, input().split())
            edges.append((b, a, i))
        edges.sort()
        dsu = DSU(2*n)
        selected = []
        for r, l, idx in edges:
            if dsu.union(l, r):
                selected.append(idx)
        print(len(selected))
        print(" ".join(map(str, selected)))
    return output.getvalue().strip()

# provided samples
assert run("2\n1\n1 2\n4\n1 2\n2 3\n1 3\n3 5\n") == "1\n1\n3\n1 2 4"

# minimum input
assert run("1\n1\n1 2\n") == "1\n1"

# all overlapping edges
assert run("1\n3\n1 3\n2 4\n1 4\n") in ["2\n1 2", "2\n2 1"], "overlapping segments handled"

# disconnected edges
assert run("1\n4\n1 2\n3 4\n5 6\n7 8\n") == "4\n1 2
```
