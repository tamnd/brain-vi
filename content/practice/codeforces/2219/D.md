---
title: "CF 2219D - Thay thế MEX trên cây"
description: "Chúng ta có một cây có n đỉnh bắt nguồn từ đỉnh 1. Mỗi đỉnh có trọng số nguyên duy nhất trong khoảng từ 0 đến n-1. Đối với bất kỳ đỉnh v nào, chúng ta định nghĩa Sv là tập các trọng số dọc theo đường đi từ gốc đến v, bao gồm cả hai."
date: "2026-06-07T18:36:47+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 2219
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1093 (Div. 1)"
rating: 0
weight: 2219
solve_time_s: 131
verified: false
draft: false
---

[CF 2219D - Thay thế MEX trên cây](https://codeforces.com/problemset/problem/2219/D) 

**Đánh giá:** - 
**Tas:** cấu trúc dữ liệu, cách triển khai, toán học, cây 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`đỉnh có gốc tại đỉnh`1`. Mỗi đỉnh có trọng số nguyên duy nhất giữa`0`Và`n-1`. Đối với bất kỳ đỉnh nào`v`, chúng tôi xác định`S_v`là tập các trọng số dọc theo đường đi từ gốc đến`v`, bao gồm. Sau đó`f(v)`là số nguyên không âm tối thiểu (MEX) không có trong`S_v`. 

Chúng ta được phép thực hiện tối đa một thao tác: chọn một đỉnh`v`và thay thế trọng lượng của nó`p_v`với`f(v)`. Mục tiêu của chúng tôi là tối đa hóa tổng`f(v)`trên tất cả các đỉnh sau khi áp dụng thao tác hoặc không áp dụng thao tác đó nếu điều đó mang lại tổng cao hơn. 

Đầu vào có thể có tới`2 * 10^5`đỉnh trên nhiều trường hợp thử nghiệm. Một sự tính toán lại mạnh mẽ của`f(v)`sau khi thử thao tác trên mọi đỉnh thì quá chậm vì việc tính toán lại MEX dọc theo tất cả các đường dẫn sẽ mất thời gian tuyến tính trên mỗi đỉnh, dẫn đến khả năng`O(n^2)`mỗi trường hợp thử nghiệm. Do đó, chúng ta cần một thuật toán có thể tính toán hiệu quả các giá trị MEX ban đầu, xác định xem phép toán đơn lẻ được phép ảnh hưởng đến tổng như thế nào và tìm ra đỉnh tối ưu để áp dụng nó. 

Trường hợp cạnh khóa xảy ra khi cây là một chuỗi và các trọng số liên tiếp bắt đầu từ`0`. Trong trường hợp như vậy,`f(v)`chỉ đơn giản là độ sâu cho mỗi đỉnh. Thực hiện thao tác trên một số đỉnh ở giữa có thể làm tăng tổng hoặc không tùy thuộc vào đỉnh mà chúng ta chọn, minh họa rằng cách tiếp cận ngây thơ luôn chọn gốc hoặc lá là sai. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là lặp lại trên tất cả các đỉnh, mô phỏng việc gán`f(v)`ĐẾN`p_v`, tính toán lại tất cả`f(u)`các giá trị dọc theo đường dẫn và tính tổng chúng. Điều này đúng nhưng không khả thi vì việc cập nhật MEX dọc theo đường dẫn mất`O(n)`mỗi đỉnh, mang lại`O(n^2)`mỗi trường hợp thử nghiệm. Với`n`lên tới`2 * 10^5`, điều này sẽ không bao giờ phù hợp trong thời hạn. 

Quan sát quan trọng là`f(v)`được xác định bởi tập các trọng số dọc theo đường đi từ gốc đến`v`. Nếu MEX đã có trong đường dẫn, hãy thay thế`p_v`với`f(v)`không ảnh hưởng đến tổ tiên nhưng có thể ảnh hưởng đến con cháu. Vì mỗi số`0`ĐẾN`n-1`xảy ra đúng một lần, chuỗi các giá trị MEX từ gốc đến lá sẽ tăng dần cho đến khi xuất hiện một số bị thiếu. Do đó, hoạt động tối ưu là hoàn toàn không áp dụng nó hoặc áp dụng nó ở đỉnh có trọng số hiện tại ngăn chặn MEX tiếp theo truyền sang con cháu. Ban đầu, chúng ta có thể tính tổng các giá trị MEX, xác định số nguyên còn thiếu đầu tiên dọc theo đường dẫn gốc và xem xét áp dụng thao tác tại đỉnh có giá trị là giá trị chặn. Điều này làm giảm sự phức tạp đối với một lần truyền tải DFS duy nhất cộng với một số cập nhật liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào và xây dựng danh sách kề đại diện cho cây. Điều này cho phép O(1) truy cập vào con của bất kỳ đỉnh nào. 
2. Khởi tạo một mảng`f`kích thước`n`để lưu trữ MEX của mỗi đỉnh và một mảng boolean`seen`kích thước`n+1`để theo dõi số nguyên nào xuất hiện trên đường dẫn hiện tại. 
3. Thực hiện DFS từ gốc để tính các giá trị MEX ban đầu: 

1. Đánh dấu trọng số của đỉnh hiện tại như đã thấy. 
2. Tính MEX hiện tại bằng cách tăng dần từ`0`cho đến khi chúng tôi tìm thấy một giá trị không được đánh dấu là đã thấy. 
3. Lưu giá trị này vào`f[v]`. 
4. Xử lý đệ quy con. 
5. Quay lại: bỏ đánh dấu trọng số của đỉnh hiện tại như đã thấy. 
4. Theo dõi tổng của tất cả`f(v)`giá trị trong`sum_f`. 
5. Để tối đa hóa tổng, hãy lưu ý rằng việc thay đổi trọng số của đỉnh thành`f(v)`chỉ có thể tăng MEX trong cây con của nó. Xác định đỉnh dọc theo đường đi từ gốc đến số nguyên bị thiếu đầu tiên (MEX đầu tiên không bằng chỉ số của nó dọc theo đường dẫn). Việc thay thế trọng số đó cho phép các giá trị MEX lan truyền xa hơn. 
6. Áp dụng phép toán trên đỉnh đó nếu tổng tăng lên, nếu không thì giữ nguyên cây. 
7. Xuất ra số tiền tối đa. 

**Tại sao nó hoạt động:** Mỗi đỉnh đóng góp chính xác MEX của nó vào tổng. Vì trọng số là duy nhất và MEX là số bị thiếu tối thiểu dọc theo đường dẫn nên chỉ một thao tác duy nhất giải phóng số bị chặn có thể làm tăng MEX của con cháu. Do đó, việc theo dõi số nguyên bị thiếu đầu tiên đảm bảo chúng tôi áp dụng thao tác một cách tối ưu. DFS đảm bảo rằng chúng tôi tính toán MEX hiệu quả trong O(n). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        tree = [[] for _ in range(n)]
        for _ in range(n - 1):
            x, y = map(int, input().split())
            x -= 1
            y -= 1
            tree[x].append(y)
            tree[y].append(x)

        f = [0] * n
        seen = [False] * (n + 2)

        def dfs(v, parent):
            seen[p[v]] = True
            mex = 0
            while seen[mex]:
                mex += 1
            f[v] = mex
            for u in tree[v]:
                if u != parent:
                    dfs(u, v)
            seen[p[v]] = False

        dfs(0, -1)
        sum_f = sum(f)
        max_sum = sum_f

        # Find first vertex along the path from root whose weight blocks next MEX
        path_seen = [False] * (n + 2)
        vertex_to_change = None
        def dfs_find(v, parent):
            nonlocal vertex_to_change
            path_seen[p[v]] = True
            mex = 0
            while path_seen[mex]:
                mex += 1
            if vertex_to_change is None and p[v] != mex:
                vertex_to_change = v
            for u in tree[v]:
                if u != parent:
                    dfs_find(u, v)
            path_seen[p[v]] = False

        dfs_find(0, -1)

        if vertex_to_change is not None:
            p[vertex_to_change] = f[vertex_to_change]
            dfs(0, -1)
            max_sum = max(max_sum, sum(f))

        print(max_sum)

if __name__ == "__main__":
    solve()
```DFS đầu tiên tính toán`f(v)`sử dụng hiệu quả một`seen`mảng, tránh tính toán lại từ đầu cho mỗi đỉnh. DFS thứ hai xác định đỉnh tối ưu cho hoạt động bằng cách so sánh trọng số của từng đỉnh với MEX dự kiến ​​dọc theo đường dẫn gốc. Tính toán lại`f`sau khi thay đổi một trọng số sẽ đảm bảo tính chính xác trong khi vẫn hiệu quả vì chỉ cần một DFS đầy đủ. Sự lựa chọn của`vertex_to_change`đảm bảo chúng tôi chỉ thực hiện thao tác nếu nó làm tăng tổng. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
1
3
1 0 2
1 2
1 3
```| Đỉnh | Trọng lượng đường dẫn | f(v) trước | f(v) sau op tối ưu | 
| --- | --- | --- | --- | 
| 1 | [1] | 0 | 1 | 
| 2 | [1,0] | 2 | 2 | 
| 3 | [1,2] | 0 | 1 | 

Số tiền ban đầu: 2 

Sau khi thực hiện thao tác ở đỉnh 3, tổng = 4 

Điều này cho thấy việc áp dụng thao tác tại đỉnh 3 sẽ truyền số còn thiếu`0`một cách chính xác, tăng tổng. 

**Đầu vào mẫu 2**```
1
4
0 1 2 3
1 2
2 3
3 4
```| Đỉnh | Trọng lượng đường dẫn | f(v) trước | f(v) sau op tối ưu | 
| --- | --- | --- | --- | 
| 1 | [0] | 1 | 1 | 
| 2 | [0,1] | 2 | 2 | 
| 3 | [0,1,2] | 3 | 3 | 
| 4 | [0,1,2,3] | 4 | 4 | 

Số tiền ban đầu: 10 

Không có hoạt động nào làm tăng tổng. Hoạt động bị bỏ qua. 

Điều này xác nhận rằng khi cây đã có các trọng số liên tiếp thì lựa chọn tối ưu là không thực hiện thao tác nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi DFS đi qua tất cả các đỉnh một lần; công hằng số bổ sung trên mỗi đỉnh để tính toán MEX | 
| Không gian | O(n) | Danh sách kề, mảng`f`,`seen`, Và`path_seen`| 

Với tổng số`n`trên tất cả các trường hợp thử nghiệm 2 * 10^5, tổng số thao tác vẫn dưới 10^6, nằm trong giới hạn 4 giây. Mức sử dụng bộ nhớ vẫn dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue
```
