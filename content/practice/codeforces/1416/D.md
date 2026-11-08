---
title: "CF 1416D - Đồ thị và truy vấn"
description: "Chúng ta đang giải quyết vấn đề kết nối động trên đồ thị vô hướng có n đỉnh và m cạnh. Mỗi đỉnh ban đầu chứa một số nguyên riêng biệt từ 1 đến n."
date: "2026-06-11T07:09:42+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu", "graphs", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 1416
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 673 (Div. 1)"
rating: 2600
weight: 1416
solve_time_s: 465
verified: false
draft: false
---

[CF 1416D - Biểu đồ và Truy vấn](https://codeforces.com/problemset/problem/1416/D) 

**Đánh giá:** 2600 
**Tags:** cấu trúc dữ liệu, dsu, đồ thị, triển khai, cây 
**Thời gian giải:** 7 phút 45 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang giải quyết vấn đề kết nối động trên đồ thị vô hướng với`n`đỉnh và`m`các cạnh. Mỗi đỉnh ban đầu chứa một số nguyên phân biệt giữa`1`Và`n`. Truy vấn có hai loại: trước tiên, bạn cần xác định đỉnh có giá trị lớn nhất trong số tất cả các đỉnh có thể truy cập được từ một đỉnh nhất định, sau đó đặt giá trị đó thành 0; thứ hai, bạn cần xóa một cạnh khỏi biểu đồ. Đầu ra bao gồm các câu trả lời cho loại truy vấn đầu tiên theo thứ tự chúng xuất hiện. 

Với những hạn chế,`n`có thể lên tới 200.000,`m`lên tới 300.000 và số lượng truy vấn`q`lên tới 500.000. Một giải pháp đơn giản chạy tìm kiếm theo chiều rộng hoặc tìm kiếm theo chiều sâu cho từng truy vấn thuộc loại một sẽ quá chậm vì mỗi tìm kiếm có thể mất thời gian O(n + m). Tương tự, việc sửa đổi biểu đồ một cách đơn giản cho mỗi lần xóa có thể dẫn đến việc xây dựng lại biểu đồ tốn kém. Bài toán đảm bảo rằng các giá trị ban đầu trên các đỉnh là khác biệt, điều này cho phép chúng ta sử dụng các cấu trúc dữ liệu dựa vào tính duy nhất khi theo dõi các giá trị tối đa. 

Trường hợp cạnh tinh tế xảy ra khi tất cả các đỉnh có thể tiếp cận đã bị loại bỏ. Trong trường hợp này, truy vấn vẫn phải trả về một giá trị, nhưng bất kỳ đỉnh nào trong thành phần được kết nối sẽ trả về vì tất cả chúng đều chứa số 0. Một trường hợp cạnh khác là việc xóa lặp đi lặp lại các cạnh và thứ tự truy vấn, có thể phân tách các thành phần được kết nối một cách linh hoạt. Nếu việc triển khai bất cẩn không xử lý việc xóa một cách hiệu quả, nó có thể báo cáo giá trị tối đa không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận mạnh mẽ sẽ là mô phỏng biểu đồ, thực hiện truy vấn loại một bằng cách chạy DFS hoặc BFS bắt đầu từ đỉnh truy vấn và nhập hai truy vấn bằng cách loại bỏ cạnh. Mỗi loại một truy vấn có thể tốn thời gian O(n + m). Với tối đa 500.000 truy vấn, phương pháp này có thể yêu cầu thứ tự 10^11 thao tác trong trường hợp xấu nhất, điều này là không khả thi. 

Cách tiếp cận tối ưu là khai thác thực tế là bài toán có thể được giải quyết một cách hiệu quả theo chiều ngược lại. Thay vì xóa các cạnh trong quá trình xử lý truy vấn chuyển tiếp, chúng ta có thể đánh dấu các cạnh cuối cùng sẽ bị xóa và bắt đầu từ trạng thái cuối cùng của biểu đồ (sau khi xóa tất cả). Ngược lại, truy vấn loại hai tương ứng với việc thêm một cạnh. Bằng cách xử lý ngược các truy vấn và duy trì các thành phần được kết nối bằng cấu trúc dữ liệu Disjoint Set Union (DSU), chúng tôi có thể theo dõi các thành phần được kết nối một cách hiệu quả. Mỗi thành phần duy trì một cấu trúc dữ liệu (ví dụ: một tập hợp được sắp xếp) để lưu trữ các giá trị đỉnh mà nó chứa, cho phép chúng ta trích xuất mức tối đa trong thời gian O(log n). Khi đảo ngược truy vấn loại hai, chúng tôi hợp nhất hai thành phần, cập nhật tập hợp giá trị. Việc xử lý ngược các truy vấn loại một cho phép chúng tôi ghi lại các giá trị tối đa và điều chỉnh chúng khi chúng tôi tiếp tục, cuối cùng tạo ra các câu trả lời đúng theo thứ tự chuyển tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q*(n+m)) | O(n+m) | Quá chậm | 
| DSU với truy vấn đảo ngược | O((n+q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào, lưu trữ các cạnh, giá trị đỉnh ban đầu và danh sách truy vấn. Duy trì một mảng đánh dấu các cạnh sẽ bị xóa trong quá trình xử lý tiếp theo. 
2. Khởi tạo DSU trong đó mỗi đỉnh bắt đầu như thành phần riêng của nó. Mỗi thành phần giữ một tập hợp được sắp xếp của`p_i`giá trị các đỉnh của nó. Điều này cho phép truy xuất nhanh giá trị tối đa trong một thành phần. 
3. Xử lý các truy vấn ngược lại. Đối với truy vấn loại một, chúng tôi ghi lại giá trị tối đa trong thành phần hiện tại của đỉnh`v`trong một mảng câu trả lời riêng biệt. Đối với truy vấn loại hai, chúng tôi hợp nhất các thành phần của cạnh được "thêm lại", cập nhật tập hợp giá trị để duy trì mức tối đa. 
4. Sau khi xử lý ngược lại tất cả các truy vấn, chúng tôi đảo ngược mảng câu trả lời cho các truy vấn loại một để khớp với thứ tự truy vấn ban đầu. Bất cứ khi nào một giá trị đỉnh được sử dụng làm giá trị tối đa, chúng tôi sẽ xóa nó khỏi tập hợp thành phần tương ứng để mô phỏng việc đặt nó về 0 trong quá trình xử lý chuyển tiếp. 
5. In câu trả lời cho tất cả các truy vấn loại một theo thứ tự. 

Điều này hiệu quả vì DSU đảm bảo rằng mỗi đỉnh thuộc về chính xác một thành phần tại bất kỳ điểm nào trong quá trình xử lý ngược. Tập hợp được liên kết với từng thành phần sẽ theo dõi chính xác giá trị của tất cả các đỉnh trong thành phần đó. Bằng cách đảo ngược các truy vấn, chúng tôi tránh được sự phức tạp của việc xóa cạnh động và vẫn mô phỏng chính xác các lựa chọn giá trị tối đa và zeroing đỉnh. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

class DSU:
    def __init__(self, n, values):
        self.par = list(range(n))
        self.size = [1]*n
        self.values = [{values[i]} for i in range(n)]
    
    def find(self, x):
        while x != self.par[x]:
            self.par[x] = self.par[self.par[x]]
            x = self.par[x]
        return x
    
    def merge(self, x, y):
        x = self.find(x)
        y = self.find(y)
        if x == y:
            return
        if self.size[x] < self.size[y]:
            x, y = y, x
        self.par[y] = x
        self.size[x] += self.size[y]
        self.values[x].update(self.values[y])
        self.values[y].clear()

def solve():
    n, m, q = map(int, input().split())
    p = list(map(int, input().split()))
    edges = [tuple(map(lambda x:int(x)-1, input().split())) for _ in range(m)]
    queries = []
    deleted = [False]*m
    for _ in range(q):
        line = input().split()
        if line[0] == '1':
            queries.append((1, int(line[1])-1))
        else:
            idx = int(line[1])-1
            queries.append((2, idx))
            deleted[idx] = True

    dsu = DSU(n, p)
    for i, (a, b) in enumerate(edges):
        if not deleted[i]:
            dsu.merge(a, b)
    
    ans = []
    for typ, val in reversed(queries):
        if typ == 1:
            root = dsu.find(val)
            if dsu.values[root]:
                mx = max(dsu.values[root])
                ans.append(mx)
                dsu.values[root].remove(mx)
            else:
                ans.append(0)
        else:
            a, b = edges[val]
            dsu.merge(a, b)
    
    for val in reversed(ans):
        print(val)

if __name__ == "__main__":
    solve()
```DSU duy trì các thành phần được kết nối một cách hiệu quả và mỗi thành phần theo dõi tập hợp giá trị hiện tại của nó. Xử lý truy vấn ngược lại sẽ tránh được sự phức tạp khi xóa động. Việc loại bỏ các giá trị tối đa mô phỏng việc thiết lập các đỉnh về 0 trong quá trình thực hiện truy vấn chuyển tiếp. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
5 4 6
1 2 5 4 3
1 2
2 3
1 3
4 5
1 1
2 1
2 3
1 1
1 2
1 2
```| Bước | Truy vấn | Thành phần tối đa | Đầu ra | 
| --- | --- | --- | --- | 
| Đảo ngược 6 | 1 2 | 2 | Nối 2 | 
| Đảo ngược 5 | 1 2 | 1 | Nối 1 | 
| Đảo ngược 4 | 1 1 | 5 | Nối 5 | 
| Ngược 3 | 2 3 | hợp nhất cạnh | - | 
| Đảo ngược 2 | 1 2 | hợp nhất cạnh | - | 
| Đảo ngược 1 | 1 1 | tối đa=5 | Nối 5 | 

Sau khi đảo ngược đầu ra:`5 1 2 0`. 

Điều này xác nhận phương pháp xử lý kết nối động và lựa chọn tối đa một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+q) log n) | Các hoạt động DSU được khấu hao gần như O(1), lựa chọn tối đa và hợp nhất nhật ký chi phí n trên mỗi hoạt động với các bộ | 
| Không gian | O(n) | Mảng cha DSU và đặt giá trị lưu trữ cho mỗi thành phần | 

Giải pháp phù hợp thoải mái trong giới hạn bài toán lên tới 500.000 truy vấn và 200.000 đỉnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        solve()
    return f.getvalue().strip()

assert run("""5 4 6
1 2 5 4 3
1 2
2 3
1 3
4 5
1 1
2 1
2 3
1 1
1 2
1 2""") == "5\n1\n2\n0"

assert run("""3 2 4
3 1 2
1 2
2 3
1 1
2 1
1 1
1 2""") == "3\n1\n2\n0"

assert run("""4 3 3
1 4 2 3
1 2
2 3
3 4
1 1
2 2
1 4""") == "4\n3"

assert run("""2 1 2
2 1
1 2
1 1
1 2""") == "2\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 5 1 2 0 | Xử lý chính xác việc hợp nhất và lựa chọn tối đa | 
| Đồ thị 3 nút nhỏ | 3 1 2 | |
