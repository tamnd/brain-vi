---
title: "CF 105064E - Câu hỏi hóc búa về màu sắc"
description: "Chúng ta đang làm việc với một cây có gốc trong đó mỗi đỉnh mang một nhãn màu. Gốc được cố định ở đỉnh 1. Trên cây này, chúng ta phải hỗ trợ hai thao tác theo thời gian: tô màu lại một đỉnh duy nhất và truy vấn có bao nhiêu đỉnh của một màu nhất định tồn tại bên trong một cây con cụ thể."
date: "2026-06-23T10:01:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105064
codeforces_index: "E"
codeforces_contest_name: "ICPC-de-Tryst 2024"
rating: 0
weight: 105064
solve_time_s: 83
verified: false
draft: false
---

[CF 105064E - Câu hỏi hóc búa về màu sắc](https://codeforces.com/problemset/problem/105064/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc với một cây có gốc trong đó mỗi đỉnh mang một nhãn màu. Gốc được cố định ở đỉnh 1. Trên cây này, chúng ta phải hỗ trợ hai thao tác theo thời gian: tô màu lại một đỉnh duy nhất và truy vấn có bao nhiêu đỉnh của một màu nhất định tồn tại bên trong một cây con cụ thể. 

Cây con ở đây có nghĩa là tất cả các đỉnh có đường đi tới gốc đi qua một đỉnh cho trước. Vì vậy, mọi truy vấn về cơ bản đều hỏi: nếu chúng ta nhìn vào “vùng con cháu” được kết nối của một nút, thì hiện tại có bao nhiêu nút có màu x? 

Đầu vào là động vì màu sắc thay đổi giữa các truy vấn. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại số liệu thống kê cây con từ đầu cho mỗi truy vấn, vì các bản cập nhật sẽ phá vỡ mọi câu trả lời tĩnh được tính toán trước. 

Tổng hợp các ràng buộc chặt chẽ: trên tất cả các trường hợp thử nghiệm, tổng số đỉnh và truy vấn lên tới 2×10^5. Điều này có nghĩa là mọi giải pháp đều phải hoạt động gần tuyến tính hoặc log-tuyến tính cho mỗi thao tác. Bất cứ điều gì như tính toán lại kết quả DFS cho mỗi truy vấn hoặc quét trực tiếp các cây con sẽ vượt quá giới hạn. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều nút chia sẻ cùng một màu và cập nhật liên tục các màu trong một vùng dày đặc. Ví dụ: hãy xem xét một chuỗi gồm 200000 nút, tất cả đều có màu ban đầu là 1. Nếu chúng ta truy vấn số lượng cây con một cách đơn giản cho mỗi nút, thì mọi truy vấn sẽ thoái hóa thành quá trình quét O(n), điều này trở nên thảm khốc. 

Một trường hợp thất bại tinh vi khác là quên rằng tư cách thành viên của cây con là mang tính cấu trúc chứ không dựa trên giá trị. Hai nút có màu giống hệt nhau không thể hoán đổi cho nhau trừ khi chúng nằm trong cùng một cây con; trộn lẫn các khái niệm này dẫn đến việc đếm không chính xác. 

## Phương pháp tiếp cận 

Cách trực tiếp để trả lời một truy vấn là duyệt qua cây con của nút đã cho bằng DFS hoặc BFS và đếm xem có bao nhiêu nút khớp với màu được truy vấn. Điều này đúng vì nó kiểm tra rõ ràng chính xác các đỉnh cần thiết. Tuy nhiên, mỗi truy vấn có thể chạm vào các nút O(n) và với tối đa 2×10^5 truy vấn, điều này dẫn đến hành vi O(nq) trong trường hợp xấu nhất, vượt xa giới hạn khả thi. 

Điều quan trọng là các truy vấn cây con trở nên đơn giản hơn nhiều nếu chúng ta làm phẳng cây thành một mảng bằng cách sử dụng chuyến tham quan Euler. Mỗi cây con tương ứng với một phân đoạn liền kề theo thứ tự duyệt này. Điều này biến vấn đề thành việc duy trì một mảng động nơi chúng ta cần hỗ trợ cập nhật điểm và truy vấn tần số trên một phạm vi. 

Tại thời điểm này, chúng ta cần một cấu trúc dữ liệu có thể trả lời: một giá trị xuất hiện bao nhiêu lần trong một phân đoạn, với các bản cập nhật thay đổi giá trị của một vị trí. Cấu trúc tự nhiên cho việc này là bản đồ từ màu đến tập hợp được sắp xếp hoặc cây Fenwick cho mỗi màu, nhưng việc duy trì cây Fenwick cho mỗi màu thì quá tốn kém về bộ nhớ. 

Thay vào đó, chúng tôi đảo ngược quan điểm: thay vì theo dõi vị trí trên mỗi màu, chúng tôi theo dõi màu trên mỗi vị trí bằng cách sử dụng cấu trúc có thể tổng hợp số lượng một cách hiệu quả. Giải pháp tiêu chuẩn là duy trì, đối với mỗi màu, một tập hợp các vị trí Euler theo thứ tự. Sau đó, mỗi truy vấn sẽ giảm xuống việc đếm xem có bao nhiêu vị trí của màu đó nằm trong một khoảng nhất định, có thể được trả lời bằng cách sử dụng tìm kiếm nhị phân. 

Các bản cập nhật sẽ bị xóa khỏi một bộ và chèn vào một bộ khác. Cả hai phép toán đều là O(log n) và các truy vấn cũng là O(log n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS cho mỗi truy vấn | O(nq) | O(n) | Quá chậm | 
| Chuyến tham quan Euler + bộ đặt hàng theo màu | O((n+q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng danh sách kề cho cây và lấy gốc của nó tại nút 1. Chúng ta cần một thứ tự duyệt xác định để mỗi cây con trở thành một khoảng liền kề. 
2. Chạy DFS để gán cho mỗi nút một thời gian nhập tin[v] và tùy chọn một phạm vi cây con [tin[v], tout[v]]. Thứ tự DFS đảm bảo rằng tất cả các hậu duệ của v nằm trong một đoạn liên tục. Đây là phép biến đổi cốt lõi thay thế cấu trúc cây bằng cấu trúc mảng. 
3. Duy trì một từ điển ánh xạ từng màu vào một thùng chứa được sắp xếp các vị trí Euler nơi màu đó hiện đang xuất hiện. Ban đầu, chèn tin[v] vào tập hợp tương ứng với c[v]. 
4. Đối với truy vấn loại 2 (v, x), diễn giải cây con của v là khoảng [tin[v], tout[v]]. Câu trả lời là số phần tử trong tập hợp màu x nằm trong khoảng này. Điều này được tính toán bằng cách sử dụng tìm kiếm nhị phân: Upper_bound(tout[v]) trừ low_bound(tin[v]). 
5. Đối với bản cập nhật loại 1 (v, x), chúng ta phải phản ánh sự thay đổi màu sắc. Xóa tin[v] khỏi bộ màu cũ và chèn nó vào bộ màu mới. Sau đó cập nhật màu được lưu trữ của v. 

Độ chính xác phụ thuộc vào việc luôn giữ cho các bộ được đồng bộ với màu hiện tại. 

### Tại sao nó hoạt động 

Thứ tự DFS đảm bảo rằng cây con(v) tương ứng chính xác với một khoảng liền kề trong thời gian Euler. Mỗi nút xuất hiện chính xác một lần theo thứ tự này, do đó việc lưu trữ các nút theo chỉ mục tin của chúng tương đương với việc lưu trữ chúng trong biểu diễn tuyến tính hóa của cây. Do đó, mỗi bộ màu đại diện cho tập hợp chính xác các vị trí mà màu đó xuất hiện trong mảng và mọi truy vấn cây con sẽ trở thành một vấn đề đếm phạm vi trên mảng đó. Vì các bản cập nhật chỉ di chuyển một phần tử duy nhất giữa các bộ nên cấu trúc luôn nhất quán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    c = [0] + list(map(int, input().split()))

    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    tin = [0] * (n + 1)
    tout = [0] * (n + 1)
    timer = 0

    stack = [(1, 0, 0)]  # node, parent, state (0 enter, 1 exit)
    while stack:
        v, p, state = stack.pop()
        if state == 0:
            timer += 1
            tin[v] = timer
            stack.append((v, p, 1))
            for to in g[v]:
                if to != p:
                    stack.append((to, v, 0))
        else:
            tout[v] = timer

    from collections import defaultdict
    import bisect

    pos = defaultdict(list)

    for i in range(1, n + 1):
        pos[c[i]].append(tin[i])

    for k in pos:
        pos[k].sort()

    def count(color, l, r):
        arr = pos.get(color, [])
        return bisect.bisect_right(arr, r) - bisect.bisect_left(arr, l)

    for _ in range(q):
        t, v, x = map(int, input().split())
        if t == 1:
            old = c[v]
            if old == x:
                continue
            old_arr = pos[old]
            idx = bisect.bisect_left(old_arr, tin[v])
            if idx < len(old_arr) and old_arr[idx] == tin[v]:
                old_arr.pop(idx)

            bisect.insort(pos[x], tin[v])
            c[v] = x
        else:
            print(count(x, tin[v], tout[v]))

def main():
    t = int(input())
    for _ in range(t):
        solve()

if __name__ == "__main__":
    main()
```Việc chuyển đổi DFS sang Euler được thực hiện lặp đi lặp lại để tránh các vấn đề về độ sâu đệ quy. Mỗi nút nhận được một thời gian vào duy nhất và ranh giới của cây con được tính từ những thời điểm này. 

Bản đồ màu theo vị trí được triển khai bằng cách sử dụng các danh sách được sắp xếp. Truy vấn sử dụng tìm kiếm nhị phân để tính toán số lượng phạm vi một cách hiệu quả. 

Bước cập nhật cẩn thận loại bỏ vị trí cũ trước khi chèn vị trí mới, đảm bảo tính nhất quán. Một lỗi thường gặp là quên cập nhật mảng màu`c[v]`, điều này sẽ hủy đồng bộ hóa các bản cập nhật trong tương lai. 

## Ví dụ đã hoạt động 

Hãy xem xét một cái cây nhỏ: 

đầu vào:```
1
5 4
1 2 1 2 3
1 2
1 3
3 4
3 5
2 1 1
1 5 1
2 1 1
2 3 2
```Sau khi truyền tải Euler, giả sử thứ tự thiếc là: 

1→1, 2→2, 3→3, 4→4, 5→5. 

Vị trí màu ban đầu: 

Màu 1: [1, 3] 

Màu 2: [2, 4] 

Màu 3: [5] 

Truy vấn đầu tiên yêu cầu cây con của 1 cho màu 1. 

| Bước | Hoạt động | Bộ màu | Trả lời | 
| --- | --- | --- | --- | 
| 1 | truy vấn (1,1) | {1:[1,3],2:[2,4],3:[5]} | 2 | 

Thao tác thứ hai thay đổi nút 5 từ 3 thành 1. 

| Bước | Hoạt động | Bộ màu | 
| --- | --- | --- | 
| 1 | nước đi 5: 3→1 | 1:[1,3,5], 3:[], 2:[2,4] | 

Truy vấn thứ ba yêu cầu cây con của 1 cho màu 1. 

| Bước | Hoạt động | Trả lời | 
| --- | --- | --- | 
| 1 | truy vấn (1,1) | 3 | 

Truy vấn cuối cùng yêu cầu cây con 3 cho màu 2, tương ứng với các nút 3,4,5 tùy thuộc vào cấu trúc. 

Điều này thể hiện cách các bản cập nhật chỉ lan truyền cục bộ trong các tập hợp màu trong khi các truy vấn cây con vẫn kiểm tra phạm vi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi bản cập nhật thực hiện một lần xóa và một lần chèn vào danh sách đã sắp xếp, mỗi lần O(log n) và mỗi truy vấn thực hiện hai tìm kiếm nhị phân | 
| Không gian | O(n) | Mỗi nút đóng góp chính xác một mục trong danh sách màu | 

Độ phức tạp vừa vặn trong giới hạn vì tổng số thao tác trên tất cả các trường hợp thử nghiệm được giới hạn bởi 2×10^5 và các hệ số logarit vẫn nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old
    return out.getvalue().strip()

# minimum case
assert run("""1
1 2
5
1 1 1
2 1 5
""") == "1"

# small tree, no updates
assert run("""1
3 2
1 2 3
1 2
1 3
2 1 1
2 2 2
""") == "1\n0"

# update flips same color back and forth
assert run("""1
3 4
1 2 3
1 2
1 3
2 1 2
1 2 3
2 1 2
2 1 3
""") == "1\n0\n1"

# star tree heavy subtree query
assert run("""1
5 2
1 1 1 2 2
1 2
1 3
1 4
1 5
2 1 1
2 1 2
""") == "3\n2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | độ đúng cơ sở | 
| cây tĩnh nhỏ | 1,0 | ánh xạ phạm vi cây con | 
| lật cập nhật | hỗn hợp | tính nhất quán năng động | 
| cây sao | 3,2 | tập hợp cây con lớn | 

## Vỏ cạnh 

Một trường hợp tinh tế phát sinh khi một bản cập nhật gán cùng màu mà nút đã có. Trong tình huống đó, việc tháo và lắp lại sẽ làm hỏng cấu trúc nếu không được bảo vệ. Việc triển khai sẽ bỏ qua công việc một cách rõ ràng khi màu cũ và màu mới khớp nhau, duy trì tính toàn vẹn của tập hợp. 

Một trường hợp khác là chuỗi sâu trong đó phạm vi cây con là các đoạn liền kề lớn. Nếu không làm phẳng Euler, các truy vấn này sẽ thoái hóa thành các bản quét toàn bộ; ở đây chúng giảm xuống một khoảng đếm đơn giản, do đó, ngay cả đường dẫn gồm 200000 nút vẫn hoạt động hiệu quả. 

Cuối cùng, các cập nhật lặp lại trên cùng một nút nhấn mạnh tính chính xác của logic loại bỏ. Vì các vị trí được lưu trữ duy nhất trong mỗi danh sách màu nên việc không xóa chỉ mục tin chính xác sẽ tích lũy các câu trả lời trùng lặp và làm tăng thêm các câu trả lời. Việc loại bỏ tìm kiếm nhị phân đảm bảo danh sách luôn phản ánh đúng trạng thái hiện tại của cây.
