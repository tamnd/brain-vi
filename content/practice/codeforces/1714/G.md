---
title: "CF 1714G - Tiền tố đường dẫn"
description: "Chúng ta được yêu cầu xử lý một cây có gốc với các giá trị trên mỗi cạnh. Mỗi cạnh có hai số nguyên $aj$ và $bj$. Đối với mỗi nút không phải là nút gốc $i$, chúng ta xem xét đường đi từ gốc đến $i$. Gọi $Ai$ là tổng của tất cả $aj$ dọc theo đường đi này."
date: "2026-06-09T20:09:15+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dfs-and-similar", "trees"]
categories: ["algorithms"]
codeforces_contest: 1714
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 811 (Div. 3)"
rating: 1700
weight: 1714
solve_time_s: 131
verified: false
draft: false
---

[CF 1714G - Tiền tố đường dẫn](https://codeforces.com/problemset/problem/1714/G) 

**Đánh giá:** 1700 
**Thẻ:** tìm kiếm nhị phân, cấu trúc dữ liệu, dfs và tương tự, cây 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xử lý một cây có gốc với các giá trị trên mỗi cạnh. Mỗi cạnh có hai số nguyên$a_j$Và$b_j$. Đối với mọi nút không phải gốc$i$, ta xét đường đi từ gốc đến$i$. Cho phép$A_i$là tổng của tất cả$a_j$dọc theo con đường này. Chúng tôi được yêu cầu tìm$r_i$, độ dài của tiền tố dài nhất của đường dẫn này sao cho tổng của$b_j$dọc theo tiền tố không vượt quá$A_i$. Độ dài tiền tố được đo bằng các cạnh, vì vậy$r_i = 0$có nghĩa là không có cạnh nào có thể được đưa vào tiền tố. 

Đầu vào đưa ra nhiều trường hợp kiểm thử, mỗi trường hợp mô tả một cây với$n$nút và$n-1$các cạnh, trong đó mỗi cạnh được xác định bởi cha mẹ$p_j$và hai chi phí$a_j$Và$b_j$. Đầu ra là một mảng$n-1$số nguyên, một cho mỗi nút không phải gốc. 

Các ràng buộc rất chặt chẽ:$n$có thể lên tới$2 \cdot 10^5$, và tổng của$n$trên tất cả các trường hợp thử nghiệm cũng bị giới hạn bởi$2 \cdot 10^5$. Một cách tiếp cận đơn giản tổng hợp rõ ràng tất cả các tiền tố cho mỗi nút sẽ là$O(n^2)$, quá chậm. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính về số lượng nút trên mỗi trường hợp thử nghiệm. Các trường hợp cạnh phát sinh khi tất cả$b_j$lớn hơn$A_i$đối với một nút, trong trường hợp đó độ dài tiền tố đúng bằng 0 và khi$A_i$khớp chính xác với một số tiền tố, phải được xử lý cẩn thận để tránh các lỗi riêng lẻ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là xây dựng lại đường dẫn từ gốc đến mỗi nút$i$và sau đó lặp lại tất cả các tiền tố để tính tổng của$b_j$, dừng khi tổng vượt quá$A_i$. Điều này đúng nhưng đòi hỏi$O(n^2)$hoạt động trong trường hợp xấu nhất, không đạt được các ràng buộc lớn nhất. 

Quan sát quan trọng để tối ưu hóa là chúng ta chỉ cần tổng tiền tố của$b_j$dọc theo con đường từ gốc. Nếu chúng ta duy trì một mảng lưu trữ tổng tích lũy của$b_j$dọc theo đường dẫn đến nút hiện tại, chúng ta có thể tìm kiếm nhị phân trên các tổng này để tìm tiền tố dài nhất có tổng không vượt quá$A_i$. Bởi vì cây đã có gốc, chúng ta có thể xây dựng các tổng tiền tố này bằng cách sử dụng tìm kiếm theo chiều sâu, truyền các tổng tích lũy từ cha đến con. Mỗi thao tác tìm kiếm nhị phân mất$O(\log n)$và việc truy cập từng nút sẽ mất$O(1)$, dẫn đến tổng thể$O(n \log n)$giải pháp. Việc sử dụng danh sách để lưu trữ tổng tích lũy trong DFS đảm bảo chúng ta có thể truy cập tổ tiên một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| DFS + Tìm kiếm nhị phân trên tổng tiền tố | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`prefix_b`để lưu trữ tổng tích lũy của$b_j$dọc theo con đường từ gốc. Nút gốc có tổng tích lũy bằng 0. Duy trì một mảng`A`để theo dõi tổng tích lũy của$a_j$từ gốc. 
2. Xây dựng cây dưới dạng danh sách kề, lưu trữ cho mỗi con các giá trị cạnh$a_j$Và$b_j$. Điều này cho phép truyền tải DFS dễ dàng từ thư mục gốc. 
3. Khởi động DFS từ thư mục gốc. Đối với mỗi nút$u$đã ghé thăm, tính toán`A[u] = A[parent] + a_edge`, đó là tổng của$a_j$dọc theo đường đi từ gốc tới$u$. 
4. Duy trì một`prefix_b`mảng đại diện cho tổng tích lũy của$b_j$dọc theo con đường. Khi đến thăm một đứa trẻ, hãy thêm vào`prefix_b[-1] + b_edge`vào mảng trước lệnh gọi DFS đệ quy. 
5. Để tính toán$r_i$đối với nút hiện tại, thực hiện tìm kiếm nhị phân trên`prefix_b`để tìm chỉ số ngoài cùng bên phải`idx`như vậy`prefix_b[idx] <= A[i]`. Chỉ số này chính xác là độ dài của tiền tố dài nhất. 
6. Sau khi hoàn thành DFS cho trẻ, hãy bật phần tử cuối cùng ra khỏi`prefix_b`để quay lại và khôi phục trạng thái cho anh chị em tiếp theo. 

Tại sao nó hoạt động: Tại mỗi nút,`prefix_b`mảng lưu trữ tổng tích lũy của$b_j$dọc theo con đường từ gốc. Bởi vì cây đã có gốc nên đường dẫn là duy nhất và mảng tăng dần. Tìm kiếm nhị phân trên mảng này đảm bảo chúng ta tìm thấy tiền tố dài nhất thỏa mãn ràng buộc về tổng. Việc truyền tải DFS đảm bảo mỗi nút được truy cập chính xác một lần và tổng tiền tố phản ánh đường dẫn chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import bisect

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        tree = [[] for _ in range(n + 1)]
        for j in range(2, n + 1):
            p, a, b = map(int, input().split())
            tree[p].append((j, a, b))

        A = [0] * (n + 1)
        res = [0] * (n + 1)
        prefix_b = [0]

        def dfs(u):
            for v, a_edge, b_edge in tree[u]:
                A[v] = A[u] + a_edge
                prefix_b.append(prefix_b[-1] + b_edge)
                # Binary search to find the longest prefix
                idx = bisect.bisect_right(prefix_b, A[v]) - 1
                res[v] = idx
                dfs(v)
                prefix_b.pop()

        dfs(1)
        print(" ".join(str(res[i]) for i in range(2, n + 1)))

if __name__ == "__main__":
    solve()
```DFS duy trì`prefix_b`mảng là tổng tích lũy của$b_j$dọc theo đường dẫn đến nút hiện tại. Tìm kiếm nhị phân được sử dụng để tìm hiệu quả độ dài tiền tố tối đa cho mỗi nút và bật lên sau khi truy cập nút con sẽ khôi phục trạng thái chính xác cho quá trình truyền tải anh chị em. sử dụng`bisect_right`đảm bảo chúng tôi xử lý trường hợp tổng tiền tố bằng`A_i`một cách chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

| Nút | Đường dẫn đến nút | Á_i | tiền tố_b | r_i | 
| --- | --- | --- | --- | --- | 
| 2 | 1→2 | 5 | [0,6] | 0 | 
| 3 | 1→2→3→4 | 19 | [0,6,16,17] | 3 | 
| 4 | 1→2→3 | 14 | [0,6,16] | 1 | 
| 5 | 1→2→3→5 | 16 | [0,6,16,17] | 2 | 

Bảng xác nhận rằng DFS có tổng tích lũy tạo ra độ dài tiền tố dài nhất chính xác cho mỗi nút. 

### Mẫu 2 

| Nút | Đường dẫn đến nút | Á_i | tiền tố_b | r_i | 
| --- | --- | --- | --- | --- | 
| 2 | 1→2 | 1 | [0,100] | 0 | 
| 3 | 1→2→3 | 2 | [0,100,101] | 0 | 
| 4 | 1→2→3→4 | 103 | [0,100,101,102] | 3 | 

Điều này cho thấy trường hợp cạnh trong đó cạnh đầu tiên$b_j > A_i$được xử lý chính xác, cho độ dài tiền tố 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | DFS truy cập mỗi nút một lần, tìm kiếm nhị phân trên mỗi nút lấy log(độ sâu), tổng O(n log n) | 
| Không gian | O(n) | Danh sách cây kề nhau, mảng tổng tích lũy và mảng kết quả | 

Thuật toán phù hợp tốt trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB, vì$n \le 2\cdot 10^5$và mỗi nút chỉ đóng góp các hoạt động log(n). 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""4
9
1 5 6
4 5 1
2 9 10
4 2 1
1 2 1
2 3 3
6 4 3
8 1 3
4
1 1 100
2 1 1
3 101 1
4
1 100 1
2 1 1
3 1 101
10
1 1 4
2 3 5
2 5 1
3 4 3
3 1 5
5 3 5
5 2 1
1 3 2
6 2 1
""") == "0 3 1 2 1 1 2 3\n0 0 3\n1 2 2\n0 1 2 1 1 2 2 1 1",
```
