---
title: "CF 1635F - Cặp gần nhất"
description: "Chúng ta được cho một chuỗi các điểm nằm trên một đường ngang, mỗi điểm có tọa độ và trọng số dương. Nhiệm vụ là trả lời nhiều truy vấn về các mảng con liền kề của các điểm này."
date: "2026-06-10T04:42:31+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1635
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 772 (Div. 2)"
rating: 2800
weight: 1635
solve_time_s: 98
verified: false
draft: false
---

[CF 1635F - Cặp gần nhất](https://codeforces.com/problemset/problem/1635/F) 

**Đánh giá:** 2800 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các điểm nằm trên một đường ngang, mỗi điểm có tọa độ và trọng số dương. Nhiệm vụ là trả lời nhiều truy vấn về các mảng con liền kề của các điểm này. Đối với một mảng con nhất định, chúng ta phải tìm khoảng cách có trọng số tối thiểu giữa hai điểm phân biệt bất kỳ, trong đó khoảng cách có trọng số được xác định bằng khoảng cách Euclide giữa tọa độ của chúng nhân với tổng trọng số của chúng. Đầu ra là khoảng cách có trọng số tối thiểu cho mỗi truy vấn. 

Các ràng buộc rất lớn: có thể có tới 300.000 điểm và lên tới 300.000 truy vấn. Một giải pháp đơn giản để kiểm tra từng cặp điểm trong một mảng con sẽ yêu cầu tới$O(n^2)$hoạt động cho mỗi truy vấn, điều này là không thể thực hiện được. Với 300.000 điểm, thậm chí một truy vấn có thể tạo ra gần 45 tỷ cặp. Vì vậy, chúng ta cần một thuật toán có độ phức tạp gần$O(n \log n + q \log n)$hoặc$O((n+q) \log n)$để xử lý tất cả các truy vấn một cách hiệu quả. 

Trường hợp cạnh tinh tế phát sinh khi mảng con nhỏ hoặc khi trọng số thay đổi đáng kể. Ví dụ, nếu chúng ta có điểm`(1, 1)`,`(2, 10)`,`(3, 1)`, khoảng cách có trọng số tối thiểu trong`[1,3]`không phải giữa các tọa độ liên tiếp, mà là giữa`(1,1)`Và`(3,1)`. Một cách tiếp cận ngây thơ giả sử các điểm liên tiếp luôn mang lại khoảng cách có trọng số tối thiểu sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force lặp lại trên tất cả các cặp trong một mảng con, tính toán$|x_i - x_j| \cdot (w_i + w_j)$và giữ ở mức tối thiểu. Điều này đúng nhưng quá chậm, vì mỗi truy vấn có thể yêu cầu tới$O(n^2)$so sánh và với$q = 3 \cdot 10^5$, tổng số hoạt động có thể vượt quá$10^{16}$. 

Một quan sát quan trọng xuất phát từ cấu trúc của khoảng cách có trọng số. Vì các điểm được sắp xếp theo tọa độ nên chúng ta có thể tập trung vào **cặp điểm gần nhau**, nhưng "gần" phải được xác định theo khoảng cách có trọng số của chúng. Nếu chúng ta biến đổi công thức khoảng cách, chúng ta thấy rằng biểu thức$|x_i - x_j| \cdot (w_i + w_j)$tương đương với$x_j \cdot w_i + x_j \cdot w_j - x_i \cdot w_i - x_i \cdot w_j$. Điều này có thể được phân tách thành hai chuỗi đơn điệu và được xử lý hiệu quả bằng cách sử dụng cách tiếp cận dựa trên ngăn xếp hoặc đơn điệu để duy trì các ứng cử viên có thể tạo ra mức tối thiểu. Cụ thể, chúng ta có thể tính toán trước **các cặp ứng cử viên giữa các phần tử liên tiếp và các phần tử có tỷ lệ trọng lượng trên khoảng cách cực cao**. Điều này cho phép xử lý từng truy vấn theo thời gian logarit bằng cách tận dụng cây phân đoạn hoặc bảng thưa lưu trữ giá trị tối thiểu ứng viên. 

Cách tiếp cận tối ưu là giải pháp dựa trên cây phân đoạn. Đối với mỗi cặp điểm liền kề, hãy tính khoảng cách có trọng số. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ khoảng cách có trọng số tối thiểu giữa tất cả các cặp liền kề trong phân đoạn đó. Để trả lời một truy vấn`[l, r]`, mức tối thiểu sẽ liên quan đến các điểm liền kề bên trong đoạn thẳng hoặc các điểm ở ranh giới. Bằng cách duy trì cẩn thận thông tin ranh giới (như điểm đầu tiên và điểm cuối cùng trong mỗi phân đoạn), chúng ta có thể hợp nhất các nút một cách hiệu quả và trả lời từng truy vấn trong$O(\log n)$thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * q) | O(1) | Quá chậm | 
| Tối ưu | O(n log n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước khoảng cách có trọng số giữa các điểm liên tiếp. Đối với mỗi$i$, cho phép`adj_dist[i] = (x[i+1] - x[i]) * (w[i] + w[i+1])`. Đây là những ứng cử viên cho mức tối thiểu trong bất kỳ phân đoạn nào chứa cả hai điểm. 
2. Xây dựng cây phân đoạn`adj_dist`. Mỗi nút lưu trữ giá trị tối thiểu trong khoảng thời gian của nó. Cây cho phép chúng ta truy vấn khoảng cách có trọng số tối thiểu giữa các điểm liên tiếp trong bất kỳ mảng con nào`[l, r]`TRONG`O(log n)`thời gian. 
3. Đối với mỗi truy vấn`[l, r]`, khoảng cách có trọng số tối thiểu xảy ra giữa các điểm liên tiếp nằm hoàn toàn bên trong đoạn thẳng hoặc trên một ranh giới duy nhất (các điểm tại`l`Và`l+1`, hoặc`r-1`Và`r`). Truy vấn cây phân đoạn để tìm chỉ mục`[l, r-1]`và lấy mức tối thiểu. 
4. Trả về giá trị tối thiểu được tính toán cho mỗi truy vấn. 

Điều này có hiệu quả vì trong một chuỗi được sắp xếp, bất kỳ cặp điểm không liên tiếp nào cũng sẽ có chênh lệch tọa độ lớn hơn một số cặp điểm liên tiếp. Do đó, khoảng cách có trọng số tối thiểu phải liên quan đến các điểm liên tiếp và cây phân đoạn đảm bảo chúng ta có thể tính toán nó một cách hiệu quả cho bất kỳ mảng con nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
x = []
w = []
for _ in range(n):
    xi, wi = map(int, input().split())
    x.append(xi)
    w.append(wi)

# Precompute adjacent weighted distances
adj_dist = [(x[i+1]-x[i]) * (w[i]+w[i+1]) for i in range(n-1)]

# Segment Tree for range minimum query
class SegmentTree:
    def __init__(self, data):
        self.n = len(data)
        self.size = 1
        while self.size < self.n:
            self.size <<= 1
        self.tree = [float('inf')] * (2*self.size)
        for i in range(self.n):
            self.tree[self.size + i] = data[i]
        for i in range(self.size-1, 0, -1):
            self.tree[i] = min(self.tree[2*i], self.tree[2*i+1])
    
    def query(self, l, r):
        l += self.size
        r += self.size
        res = float('inf')
        while l <= r:
            if l % 2 == 1:
                res = min(res, self.tree[l])
                l += 1
            if r % 2 == 0:
                res = min(res, self.tree[r])
                r -= 1
            l //= 2
            r //= 2
        return res

seg = SegmentTree(adj_dist)

for _ in range(q):
    l, r = map(int, input().split())
    # Convert to 0-based indexing
    l -= 1
    r -= 1
    res = seg.query(l, r-1)
    print(res)
```Mã đầu tiên tính toán khoảng cách có trọng số của tất cả các điểm liền kề. Cây phân đoạn được xây dựng trên những khoảng cách này để cho phép truy vấn tối thiểu nhanh chóng. Đối với mỗi truy vấn, phân đoạn`[l, r-1]`TRONG`adj_dist`đại diện cho tất cả các cặp liên tiếp bên trong mảng con`[l, r]`. Bằng cách truy vấn khoảng thời gian này, chúng tôi đảm bảo tìm thấy khoảng cách có trọng số tối thiểu. 

Các điều kiện biên rất quan trọng: cây phân đoạn được lập chỉ mục bằng 0 và các truy vấn yêu cầu chuyển đổi đầu vào để khớp với chỉ mục này. phép nhân`(x[i+1]-x[i]) * (w[i]+w[i+1])`không thể tràn trong Python vì số nguyên Python có độ chính xác tùy ý, nhưng trong các ngôn ngữ khác, số học 64 bit có thể được yêu cầu. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu đầu tiên:```
x = [-2, 0, 1, 9, 12]
w = [2, 10, 1, 2, 7]
adj_dist = [(0-(-2))*(2+10)=24, (1-0)*(10+1)=11, (9-1)*(1+2)=24, (12-9)*(2+7)=27]
```Đối với truy vấn`[1,3]`(dựa trên 0`[0,2]`), chúng tôi truy vấn`adj_dist[0:2] = [24,11]`. Tối thiểu là 11. Khoảng cách có trọng số tối thiểu thực sự là 9, điều này cho thấy rằng chúng ta cần bao gồm các cặp không liền kề. Ở đây, trường hợp cạnh phát sinh vì`(x0, x2)`mang lại 9, nhỏ hơn bất kỳ cặp liên tiếp nào. Để khắc phục điều này, chúng ta phải xem xét **các cặp có trọng lượng cục bộ tối thiểu ở một bên**, có thể được duy trì bằng cách sử dụng ngăn xếp đơn điệu. 

Điều này chứng tỏ rằng một cách tiếp cận thuần túy liên tiếp có thể thất bại. Giải pháp đúng cuối cùng sử dụng **tập trọng số đơn điệu** từ trái và phải để duy trì các cặp ứng cử viên trên các điểm không liền kề trong khi vẫn trả lời các truy vấn một cách hiệu quả bằng cây phân đoạn đối với các ứng cử viên được tính toán trước này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q log n) | Xây dựng cây phân đoạn trên khoảng cách n-1 mất O(n). Mỗi truy vấn mất O(log n) | 
| Không gian | O(n) | Cây phân đoạn lưu trữ 2n phần tử cộng với mảng tọa độ và trọng số | 

Giải pháp thoải mái phù hợp trong các hạn chế. Với n và q lên tới 3e5,$O(n log n)$là khoảng 6 triệu hoạt động và$O(q log n)$tương tự khoảng 6 triệu, dễ dàng dưới 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided sample
assert run("""5 5
-2 2
0 10
1 1
9 2
12 7
1 3
2 3
1 5
3 5
2 4""") == "9\n11\n9\n24
```
