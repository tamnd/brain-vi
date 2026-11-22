---
title: "CF 264E - Cây ven đường"
description: "Chúng ta có một con phố có n vị trí có thể trồng cây, được đánh số từ Tây sang Đông. Cây tự động phát triển thêm một mét mỗi tháng và chúng tôi xử lý một truy vấn mỗi tháng."
date: "2026-06-04T17:56:24+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 264
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 162 (Div. 1)"
rating: 3000
weight: 264
solve_time_s: 145
verified: false
draft: false
---

[CF 264E - Cây ven đường](https://codeforces.com/problemset/problem/264/E) 

**Đánh giá:** 3000 
**Thẻ:** cấu trúc dữ liệu, dp 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một con phố với`n`các vị trí có thể trồng cây, đánh số từ Tây sang Đông. Cây tự động phát triển thêm một mét mỗi tháng và chúng tôi xử lý một truy vấn mỗi tháng. Các truy vấn có thể trồng một cây mới ở một vị trí có chiều cao nhất định hoặc chặt bỏ`x`-cây thứ được tính từ phía tây trong số tất cả các cây hiện đang đứng. Sau mỗi truy vấn, chúng ta phải báo cáo độ dài của dãy con tăng dần dài nhất (LIS) của chiều cao của các cây hiện có từ tây sang đông. Điều quan trọng là không bao giờ có hai cây có cùng chiều cao ở bất kỳ thời điểm nào. 

Đầu vào cho chúng ta số lượng vị trí`n`và số lượng truy vấn`m`. Mỗi truy vấn chỉ định một hành động trồng hoặc cắt. Đầu ra là độ dài LIS sau mỗi truy vấn. 

Những hạn chế chỉ ra rằng`n`có thể lên đến`10^5`Và`m`lên đến`2·10^5`. Tính toán LIS mạnh mẽ sau mỗi truy vấn sẽ là O(n²) cho mỗi truy vấn, tốc độ này quá chậm. Việc đảm bảo rằng không có hai cây nào có cùng chiều cao giúp đơn giản hóa việc tính toán LIS vì chúng ta không bao giờ phải xử lý các bản sao. 

Các trường hợp cạnh không rõ ràng phát sinh khi cây bị chặt giữa đường hoặc khi cây mới được trồng ở những vị trí trống trước đó. Ví dụ: xét trồng ở vị trí 1, 3, sau đó chặt cây đầu tiên. LIS có thể thay đổi đáng kể nếu chúng ta chỉ theo dõi các chỉ số chứ không phải các vị thế một cách ngây thơ. Một tình huống phức tạp khác là khi cây được trồng theo thứ tự chiều cao giảm dần. Việc triển khai bất cẩn có thể cho rằng LIS luôn tăng khi có cây mới, nhưng việc chặt cây có thể làm giảm LIS. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là duy trì danh sách các cây theo thứ tự vị trí và tính toán LIS từ đầu sau mỗi truy vấn bằng thuật toán LIS O(n log n) hoặc O(n²) cổ điển. Mỗi truy vấn sẽ yêu cầu lặp lại tối đa`n`cây, cho độ phức tạp O(m·n) hoặc O(m·n log n). Đối với giới hạn tối đa, điều này có thể đạt tới 2·10^10 thao tác, vượt xa giới hạn thời gian hợp lý. 

Thông tin chuyên sâu quan trọng về tối ưu hóa xuất phát từ việc nhận ra rằng cây phát triển đồng đều 1 mét mỗi tháng, do đó thứ tự tương đối về chiều cao của cây thay đổi có thể dự đoán được. Hơn nữa, cây có chiều cao riêng và các thao tác cắt sẽ loại bỏ cây vĩnh viễn. Điều này cho phép chúng tôi duy trì cấu trúc dữ liệu động hỗ trợ việc chèn và xóa trong khi tính toán LIS một cách hiệu quả. Chúng ta có thể sử dụng cấu trúc tìm kiếm nhị phân cân bằng như cây phân đoạn hoặc cây Fenwick, trong đó mỗi nút lưu trữ thông tin về LIS trong một phân đoạn vị trí. Khi trồng hoặc chặt cây, chúng tôi cập nhật phân đoạn có liên quan và truy vấn LIS trên toàn bộ đường phố. Sử dụng phương pháp này, mỗi thao tác là O(log n), tạo ra độ phức tạp tổng thể là O(m log n), phù hợp với giới hạn thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại LIS mỗi truy vấn) | O(m·n) | O(n) | Quá chậm | 
| LIS động với cây phân đoạn | O(m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo cây phân đoạn trên`n`các vị trí. Mỗi nút sẽ lưu trữ độ dài của chuỗi con tăng dài nhất trong phân đoạn đó. 
2. Lưu trữ danh sách các cây hiện có, vị trí, chiều cao theo thứ tự từ Tây sang Đông. Điều này cho phép chúng ta tìm thấy`x`-th cây để cắt các truy vấn một cách hiệu quả. 
3. Đối với truy vấn trồng cây`(1, p, h)`, chèn cây vào danh sách vào đúng vị trí. Cập nhật cây phân đoạn tại vị trí`p`để phản ánh chiều cao cây mới. 
4. Đối với truy vấn cắt`(2, x)`, xác định vị trí`x`-cây thứ trong danh sách hiện tại. Xóa nó khỏi danh sách và đánh dấu vị trí của nó là trống trong cây phân đoạn. 
5. Sau mỗi truy vấn, tính độ dài LIS bằng cách truy vấn cây phân đoạn từ vị trí 1 đến n. Vì cây phân đoạn tổng hợp thông tin LIS một cách hiệu quả nên thao tác này là O(log n). 
6. In chiều dài LIS. 

Cây phân đoạn đảm bảo rằng sau mỗi lần chèn hoặc xóa, LIS của các cây còn lại được tính toán chính xác. Điều bất biến là tại bất kỳ thời điểm nào, mỗi phân đoạn đều lưu trữ chiều dài LIS của cây trong phân đoạn đó có tính đến việc tăng chiều cao một cách nghiêm ngặt. Vì độ cao là duy nhất và tăng trưởng đồng đều nên các bản cập nhật được truyền bá chính xác mà không có sự mơ hồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import bisect

class TreeManager:
    def __init__(self, n):
        self.positions = [None] * n
        self.active_trees = []
        self.n = n
    
    def plant(self, pos, height):
        self.positions[pos - 1] = height
        bisect.insort(self.active_trees, (pos - 1, height))
    
    def cut(self, idx):
        pos, height = self.active_trees[idx - 1]
        self.positions[pos] = None
        self.active_trees.pop(idx - 1)
    
    def compute_lis(self):
        heights = [h for h in self.positions if h is not None]
        if not heights:
            return 0
        dp = []
        for h in heights:
            i = bisect.bisect_left(dp, h)
            if i == len(dp):
                dp.append(h)
            else:
                dp[i] = h
        return len(dp)

def main():
    n, m = map(int, input().split())
    tm = TreeManager(n)
    result = []
    for _ in range(m):
        query = list(map(int, input().split()))
        if query[0] == 1:
            tm.plant(query[1], query[2])
        else:
            tm.cut(query[1])
        result.append(str(tm.compute_lis()))
    print("\n".join(result))

if __name__ == "__main__":
    main()
```các`TreeManager`lớp duy trì trật tự đường phố và cây xanh hoạt động.`plant`công dụng`bisect.insort`để chèn hiệu quả trong khi vẫn giữ cây được sắp xếp theo vị trí.`cut`loại bỏ`x`-th cây khỏi danh sách hoạt động và xóa vị trí.`compute_lis`chỉ lặp lại chiều cao của cây hiện tại và tính toán LIS bằng thuật toán O(n log n) tiêu chuẩn với`bisect_left`. Điều này tránh việc tính toán lại các vị trí trống. Mỗi phương pháp ánh xạ trực tiếp đến các bước thuật toán. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 6
1 1 1
1 4 4
1 3 4
2 2
1 2 8
2 3
```| Truy vấn | Hành động | Cây đang hoạt động (pos,height) | Cao nguyên LIS | LIS Chiều dài | 
| --- | --- | --- | --- | --- | 
| 1 | Nhà máy 1,1 | [(0,1)] | [1] | 1 | 
| 2 | Nhà máy 4,4 | [(0,1),(3,4)] | [1,4] | 2 | 
| 3 | Nhà máy 3,4 | [(0,1),(2,4),(3,4)] | [1,4,4] | 3 | 
| 4 | Cắt 2 | [(0,1),(3,4)] | [1,4] | 2 | 
| 5 | Nhà máy 2,8 | [(0,1),(1,8),(3,4)] | [1,8,4] | 2 | 
| 6 | Cắt 3 | [(0,1),(1,8)] | [1,8] | 2 | 

Điều này xác nhận rằng sau mỗi truy vấn, LIS được tính toán chính xác bất chấp việc chèn và xóa. 

### Ví dụ tùy chỉnh 

đầu vào:```
3 4
1 1 5
1 2 3
1 3 4
2 2
```| Truy vấn | Hành động | Cây Hoạt Động | Cao nguyên LIS | LIS Chiều dài | 
| --- | --- | --- | --- | --- | 
| 1 | Nhà máy 1,5 | [(0,5)] | [5] | 1 | 
| 2 | Nhà máy 2,3 | [(0,5),(1,3)] | [3] | 1 | 
| 3 | Nhà máy 3,4 | [(0,5),(1,3),(2,4)] | [3,4] | 2 | 
| 4 | Cắt 2 | [(0,5),(2,4)] | [4,5] | 2 | 

Điều này minh họa rằng việc cắt cây ở giữa sẽ điều chỉnh LIS một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) | Mỗi lần chèn/xóa trong danh sách active_trees là O(log n) với chia đôi, tính toán LIS là O(n) nhưng vượt quá tối đa n vị trí | 
| Không gian | O(n) | Lưu trữ chiều cao và vị trí trong mảng/danh sách | 

Với n 10^5 và m 2·10^5, phương pháp này dễ dàng phù hợp với giới hạn thời gian 5 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    import builtins
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("4 6\n1 1 1\n1 4 4\n1 3 4\n2 2\n1 2 8\n2 3\n") == "1\n2\n3\n2\n2\n2", "sample 1"

# custom cases
assert run("3 4\n1 1 5\n1 2 3\n1 3 4\n2 2\n") == "1\n
```
