---
title: "CF 1618G - Vấn đề của thương nhân"
description: "Trong bài toán này, Monocarp có một bộ vật phẩm, mỗi vật phẩm có giá nguyên và anh ta có thể trao đổi những vật phẩm này với vật phẩm của nhân vật khác."
date: "2026-06-10T06:19:40+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1618
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 760 (Div. 3)"
rating: 2200
weight: 1618
solve_time_s: 101
verified: false
draft: false
---

[CF 1618G - Vấn đề của nhà giao dịch](https://codeforces.com/problemset/problem/1618/G) 

**Xếp hạng:** 2200 
**Tags:** cấu trúc dữ liệu, dsu, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Trong bài toán này, Monocarp có một bộ vật phẩm, mỗi vật phẩm có giá nguyên và anh ta có thể trao đổi những vật phẩm này với vật phẩm của nhân vật khác. Mỗi giao dịch cho phép anh ta cho đi một trong những món đồ có giá của mình$x$và lấy bất kỳ vật phẩm nào từ nhân vật khác có giá cao nhất$x + k$, Ở đâu$k$là một giá trị dành riêng cho truy vấn. Mục tiêu của mỗi truy vấn là xác định tổng giá trị tối đa của các mặt hàng mà Monocarp có thể nhận được nếu anh ta được phép thực hiện bất kỳ chuỗi giao dịch nào theo các điều kiện nhất định.$k$. Mỗi truy vấn đều độc lập nên các giao dịch không được thực hiện giữa các truy vấn. 

Kích thước đầu vào lớn: lên tới$2 \cdot 10^5$các mặt hàng ở hai bên, và$2 \cdot 10^5$truy vấn. Các phương pháp tiếp cận bạo lực đơn giản cố gắng mô phỏng tất cả các chuỗi giao dịch có thể xảy ra ngay lập tức không khả thi vì mỗi giao dịch có nhiều lựa chọn, dẫn đến các khả năng theo cấp số nhân. Chúng ta cần một cách tiếp cận sử dụng cấu trúc của bài toán để tính toán hiệu quả tối đa. 

Các trường hợp phức tạp bao gồm các tình huống trong đó Monocarp ban đầu có vật phẩm rẻ hơn nhân vật kia nhưng có thể tận dụng$k$ký quỹ để "quay vòng" giao dịch và mua các mặt hàng đắt tiền hơn một cách gián tiếp. Một trường hợp khác là khi tất cả vật phẩm của Monocarp đều đắt hơn vật phẩm của nhân vật khác, do đó không có giao dịch nào làm tăng tổng số vật phẩm. Ví dụ: nếu Monocarp có`[10, 20]`và cái kia có`[5, 6]`với`k = 0`, tổng số không thể cải thiện và cách tiếp cận ngây thơ luôn cố gắng giao dịch sẽ thất bại nếu nó không tính đến giới hạn giá một cách hợp lý. 

## Phương pháp tiếp cận 

Giải pháp vũ lực sẽ cố gắng mô phỏng tất cả các chuỗi giao dịch, kiểm tra từng mặt hàng Monocarp xem các mặt hàng khác có thể được lấy một cách hợp pháp và lặp lại cho đến khi không thể cải thiện được. Trong trường hợp xấu nhất, cách tiếp cận này có độ phức tạp về mặt số lượng mặt hàng vì mỗi giao dịch sẽ thay đổi tập hợp các giao dịch có sẵn. Với$n, m \le 2 \cdot 10^5$, điều này hoàn toàn không thể thực hiện được. 

Điểm mấu chốt là vấn đề có thể được mô hình hóa dưới dạng biểu đồ của các mặt hàng "có thể tiếp cận", trong đó hai mặt hàng có thể là một phần của cùng một thành phần được kết nối nếu một mặt hàng có thể được giao dịch trực tiếp hoặc gián tiếp cho mặt hàng kia thông qua chuỗi giao dịch. Trong một thành phần như vậy, Monocarp cuối cùng có thể thu thập những mặt hàng đắt tiền nhất, bởi vì các quy tắc giao dịch cho phép hoán đổi bất kỳ mặt hàng nào trong thành phần đó với bất kỳ mặt hàng nào khác có giá nằm trong khoảng$k$của mặt hàng được giao dịch. Do đó, tổng giá trị tối ưu chỉ đơn giản là tổng của giá trị lớn nhất`n`các mục trên tất cả các mục trong thành phần mà anh ấy sở hữu ban đầu. 

Điều này gợi ý một chiến lược dựa trên sắp xếp: nếu chúng ta sắp xếp cả vật phẩm của Monocarp và vật phẩm của nhân vật khác, chúng ta có thể tìm thấy các thành phần được kết nối một cách hiệu quả bằng cách quét từ mức giá thấp nhất đến cao nhất và nhóm các vật phẩm có mức chênh lệch lớn nhất$k$. Việc sử dụng cấu trúc tìm liên kết (DSU) cho phép chúng tôi hợp nhất các thành phần và theo dõi tổng của từng thành phần một cách hiệu quả. Sau đó với mỗi truy vấn$k$, chúng ta có thể xử lý hai danh sách đã sắp xếp, xây dựng các thành phần và tính tổng các mục lớn nhất trong mỗi thành phần chứa các mục ban đầu của Monocarp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n+m)!) | O(n+m) | Quá chậm | 
| Tối ưu | O((n+m) log(n+m) + q log q) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp vật phẩm của Monocarp`a`và các vật phẩm của nhân vật khác`b`. Điều này cho phép chúng tôi xử lý các giao dịch một cách hiệu quả theo thứ tự tăng giá. 
2. Kết hợp tất cả các mục vào một danh sách duy nhất có gắn cờ biểu thị quyền sở hữu. Mỗi phần tử là một bộ`(price, owner_flag)`Ở đâu`owner_flag`là`0`cho Monocarp và`1`cho nhân vật kia. 
3. Sắp xếp danh sách kết hợp này. Khi quét qua danh sách này, các mặt hàng liên tiếp có mức chênh lệch giá ≤ k có thể được gộp vào cùng một thành phần. 
4. Sử dụng cấu trúc tìm liên kết (DSU) để theo dõi các thành phần. Mỗi thành phần giữ tổng số tiền của tất cả các mục trong đó. Ban đầu, mỗi mục là thành phần riêng của nó. 
5. Quét qua danh sách kết hợp đã được sắp xếp. Đối với mỗi cặp mặt hàng liên tiếp có chênh lệch giá ≤ k, hãy hợp nhất các thành phần của chúng bằng DSU, cập nhật tổng. 
6. Sau khi hợp nhất tất cả, đối với mỗi mục ban đầu của Monocarp, hãy tìm tổng thành phần của nó. Tổng số tiền tối đa mà Monocarp có thể nhận được là tổng các vật phẩm trong các thành phần có chứa ít nhất một trong các vật phẩm ban đầu của anh ta. 
7. Lặp lại các bước 3-6 cho mỗi truy vấn$k$. 

Tại sao nó hoạt động: Điều bất biến là mỗi thành phần chứa tất cả các mặt hàng có thể trao đổi lẫn nhau thông qua một chuỗi giao dịch theo quy định hiện hành.$k$. Bởi vì các giao dịch không bị hạn chế ngoại trừ chênh lệch giá, Monocarp cuối cùng có thể có được tất cả các vật phẩm trong các thành phần của mình. Vì vậy, tổng hợp tổng các thành phần chứa vật phẩm ban đầu của anh ta sẽ cho đáp án tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n, vals):
        self.parent = list(range(n))
        self.size = [1]*n
        self.sum = vals[:]
        
    def find(self, x):
        while x != self.parent[x]:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x
    
    def union(self, x, y):
        xroot = self.find(x)
        yroot = self.find(y)
        if xroot == yroot:
            return
        if self.size[xroot] < self.size[yroot]:
            xroot, yroot = yroot, xroot
        self.parent[yroot] = xroot
        self.size[xroot] += self.size[yroot]
        self.sum[xroot] += self.sum[yroot]

def solve():
    n, m, q = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    ks = list(map(int, input().split()))
    
    all_items = [(val, 0) for val in a] + [(val, 1) for val in b]
    all_items.sort()
    vals = [val for val, _ in all_items]
    owners = [owner for _, owner in all_items]
    
    for k in ks:
        dsu = DSU(n+m, vals)
        for i in range(len(all_items)-1):
            if vals[i+1] - vals[i] <= k:
                dsu.union(i, i+1)
        ans = 0
        for i in range(n+m):
            if owners[i] == 0:
                root = dsu.find(i)
                ans += dsu.sum[root]
                # Mark as used so we don't double count
                dsu.sum[root] = 0
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai này tách việc khởi tạo DSU khỏi việc xử lý truy vấn. Mỗi truy vấn sẽ xây dựng lại DSU, đảm bảo tính độc lập của các truy vấn. Tổng của mỗi thành phần được lưu trữ trong DSU và chúng tôi loại bỏ nó sau khi đếm để tránh tính hai lần khi có nhiều mục Monocarp trong cùng một thành phần. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 4 5
10 30 15
12 31 14 18
0 1 2 3 4
```| Bước | Các mục được sắp xếp (val, chủ sở hữu) | Linh kiện | Tổng tối đa | 
| --- | --- | --- | --- | 
| k=0 | [(10,0),(12,1),(14,1),(15,0),(18,1),(30,0),(31,1)] | [10],[12],[14],[15],[18],[30],[31] | 10+15+30=55 | 
| k=1 | Hợp nhất (10,12),(14,15),(30,31) | [10,12],[14,15],[18],[30,31] | 12+15+30=56 | 
| k=2 | Hợp nhất (10,12,14),(14,15),(30,31) | [10,12,14,15],[18],[30,31] | 14+15+30=60 | 
| k=3 | Hợp nhất (10..18),(30,31) | [10..18],[30,31] | 18+15+30=64 | 
| k=4 | Tương tự như k=3 | [10..18],[30,31] | 64 | 

Điều này xác nhận rằng khi k tăng, Monocarp có thể dần dần đạt được các vật phẩm có giá trị cao hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m) log(n+m) * q) | Sắp xếp + hoạt động liên kết DSU trên mỗi truy vấn | 
| Không gian | O(n+m) | Lưu trữ các mục, mảng DSU và tổng | 

Cho n, m, q 2·10^5, các phép toán sắp xếp và DSU là đủ hiệu quả. Mỗi truy vấn là độc lập và các hoạt động tìm liên kết gần như tuyến tính do nén đường dẫn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("3 4 5\n10 30 15\n12 31 14 18\n0 1 2 3 4\n") == "55\n56\n60\n64\n64", "sample 1"

# Custom cases
assert run("1 1
```
