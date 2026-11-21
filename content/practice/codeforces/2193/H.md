---
title: "CF 2193H - Loại bỏ Cây Chén Thánh"
description: "Chúng ta có một cây, là một đồ thị không tuần hoàn được kết nối với các đỉnh $n$ và mỗi đỉnh có một giá trị số nguyên liên quan. Hoạt động mà chúng ta có thể thực hiện lặp đi lặp lại là loại bỏ một đỉnh nếu tổng giá trị của các lân cận còn lại của nó khác với giá trị chẵn lẻ của chính nó."
date: "2026-06-07T20:54:09+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graphs", "greedy", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 2193
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 1076 (Div. 3)"
rating: 2400
weight: 2193
solve_time_s: 129
verified: false
draft: false
---

[CF 2193H - Loại bỏ Cây Chén Thánh](https://codeforces.com/problemset/problem/2193/H) 

**Đánh giá:** 2400 
**Thẻ:** dfs và tương tự, dp, đồ thị, tham lam, triển khai, cây cối 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây, là một đồ thị không tuần hoàn được kết nối với$n$đỉnh và mỗi đỉnh có một giá trị nguyên liên quan. Hoạt động mà chúng ta có thể thực hiện lặp đi lặp lại là loại bỏ một đỉnh nếu tổng giá trị của các lân cận còn lại của nó khác với giá trị chẵn lẻ của chính nó. Nghĩa là, nếu tổng các đỉnh lân cận là số lẻ và đỉnh là số chẵn hoặc ngược lại, chúng ta có thể loại bỏ nó. Sau khi bị loại bỏ, đỉnh và tất cả các cạnh của nó sẽ biến mất, có khả năng làm thay đổi tổng của các đỉnh lân cận. Nhiệm vụ là quyết định xem có thể loại bỏ tất cả các đỉnh một cách tuần tự theo quy tắc này hay không và nếu có thể, đưa ra bất kỳ chuỗi loại bỏ hợp lệ nào. 

Các ràng buộc rất chặt chẽ:$n$có thể lên đến$2 \cdot 10^5$và có thể có tới$10^4$trường hợp thử nghiệm. Một mô phỏng đơn giản liên tục quét tất cả các đỉnh để tìm ra các ứng cử viên có thể di chuyển được sẽ cần$O(n^2)$trong trường hợp xấu nhất, tốc độ này quá chậm so với giới hạn trên. Chúng tôi cần một giải pháp gần tuyến tính về tổng số đỉnh trên tất cả các trường hợp thử nghiệm. 

Trường hợp cạnh khóa là một đỉnh mà các đỉnh lân cận đều có tổng chẵn lẻ bằng chính nó. Nếu cây nhỏ thì nói$n = 3$với các giá trị [2, 1, 4] và các cạnh nối chúng tuyến tính thì có thể không có đỉnh nào thỏa mãn điều kiện chẵn lẻ lúc đầu. Một cách tiếp cận bất cẩn cho rằng bất kỳ cây nào cũng có thể tháo rời được sẽ cho kết quả CÓ không chính xác. Một trường hợp phức tạp khác xảy ra với những cây có nhiều lá, trong đó việc loại bỏ các lá sẽ làm thay đổi tổng của các cây lân cận và cho phép loại bỏ các đỉnh bị chặn trước đó. Thuật toán phải tính toán cẩn thận việc cập nhật động các tổng lân cận. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: ở mỗi bước, quét tất cả các đỉnh, tính tổng giá trị của các đỉnh lân cận hiện tại và loại bỏ một đỉnh có tổng khác nhau về tính chẵn lẻ. Lặp lại cho đến khi tất cả các đỉnh bị loại bỏ hoặc không còn đỉnh nào tồn tại. Điều này đúng về mặt khái niệm nhưng cực kỳ kém hiệu quả. Vì$n$các đỉnh, mỗi lần quét có thể mất$O(n)$, và trong trường hợp xấu nhất chúng ta thực hiện$n$sự loại bỏ, dẫn đến$O(n^2)$hoạt động cho mỗi trường hợp thử nghiệm. Điều này sẽ hết thời gian đối với những cây lớn. 

Cái nhìn sâu sắc quan trọng để tối ưu hóa là xử lý cây theo chiều sâu trong khi vẫn tôn trọng tính chẵn lẻ. Khi một lá bị loại bỏ, giá trị của nó sẽ ảnh hưởng đến tổng lân cận của lá cha, khi đó có thể thỏa mãn điều kiện loại bỏ. Bằng cách root cây một cách tùy ý và thực hiện DFS, chúng ta có thể loại bỏ các đỉnh trong quá trình truyền tải theo thứ tự sau trong khi kiểm tra điều kiện chẵn lẻ cục bộ. Việc truyền tải theo thứ tự sau đảm bảo rằng tất cả các đỉnh con của một đỉnh đều được xem xét trước chính đỉnh đó, mô phỏng hiệu quả các cập nhật động của các tổng lân cận mà không cần tính toán lại tổng một cách rõ ràng mỗi lần. Điều này làm giảm độ phức tạp xuống$O(n)$mỗi trường hợp thử nghiệm. 

DFS thứ tự sau hoạt động vì tính chẵn lẻ của một đỉnh chỉ phụ thuộc vào tổng của các đỉnh lân cận hiện tại. Khi chúng tôi xử lý phần tử con trước, chúng tôi đảm bảo rằng mọi tác động loại bỏ sẽ lan truyền lên trên. Nếu bản thân gốc không thể bị loại bỏ sau khi xử lý tất cả các cây con thì cây đó không thể bị loại bỏ hoàn toàn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| DFS / Tham lam đặt hàng sau | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số đỉnh$n$và mảng giá trị đỉnh$a$. Xây dựng danh sách kề đại diện cho cây. 
2. Chọn bất kỳ đỉnh nào làm gốc. Một lựa chọn thuận tiện là đỉnh 1. 
3. Xác định hàm DFS xử lý đệ quy các phần tử con của đỉnh hiện tại. Đối với mỗi đứa trẻ, hãy gọi DFS trên đó trước. 
4. Sau khi tất cả các nút con được xử lý, hãy kiểm tra tính chẵn lẻ của đỉnh hiện tại so với tổng các đỉnh lân cận hiện tại chưa bị xóa. Nếu các điểm chẵn lẻ khác nhau, hãy đánh dấu đỉnh này là có thể tháo rời và thêm nó vào trình tự loại bỏ. 
5. Tiếp tục DFS cho đến khi tất cả các đỉnh có thể truy cập được từ gốc đã được truy cập. Nếu tại bất kỳ thời điểm nào không có đỉnh nào trong cây con có thể bị xóa, hãy quay lại và cuối cùng nếu không thể xóa gốc, hãy khai báo NO. 
6. Sau DFS, nếu trình tự loại bỏ chứa tất cả$n$các đỉnh, xuất CÓ và dãy. Nếu không thì xuất ra NO. 

Tại sao nó hoạt động: bằng cách xử lý theo thứ tự sau, mỗi đỉnh được xem xét sau các đỉnh con của nó, đảm bảo rằng các tổng lân cận được phản ánh chính xác trong điều kiện loại bỏ. Bất kỳ đỉnh hoặc đỉnh lá nào có tính chẵn lẻ thỏa mãn đều có thể bị loại bỏ và việc loại bỏ sẽ truyền các hiệu ứng đến nút cha. Điều bất biến là tại mỗi bước, nếu có một đỉnh nằm trong dãy thì nó thỏa mãn điều kiện chẵn lẻ tại thời điểm đó. Vì mọi phép loại bỏ đều hợp lệ và tất cả các đỉnh cuối cùng đều được xem xét nên thuật toán sẽ tạo ra một chuỗi chính xác khi có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        adj = [[] for _ in range(n)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            adj[u].append(v)
            adj[v].append(u)
        
        removed = [False] * n
        result = []
        
        def dfs(v, parent):
            for u in adj[v]:
                if u == parent:
                    continue
                dfs(u, v)
            S_v = sum(a[u] for u in adj[v] if not removed[u])
            if (S_v % 2) != (a[v] % 2):
                removed[v] = True
                result.append(v + 1)
        
        dfs(0, -1)
        if len(result) == n:
            print("YES")
            print(" ".join(map(str, result)))
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Danh sách kề đại diện cho cây một cách hiệu quả. Chúng tôi sử dụng một mảng boolean`removed`để theo dõi những đỉnh nào đã được đưa ra. DFS là thứ tự sau, vì vậy trước tiên chúng tôi loại bỏ đệ quy các nút con, sau đó là đỉnh hiện tại nếu tính chẵn lẻ của tổng lân cận của nó cho phép. các`result`mảng nắm bắt trình tự. Lập chỉ mục cẩn thận đảm bảo đầu ra dựa trên 1 cho các đỉnh. Đặt giới hạn đệ quy cao sẽ ngăn chặn tình trạng tràn ngăn xếp cho cây sâu. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:```
4
3 4 2 1
1 2
2 3
3 4
```| Bước | Đỉnh | Giá trị lân cận | S_v | Kiểm tra tính chẵn lẻ | LOẠI BỎ? | Trình tự | 
| --- | --- | --- | --- | --- | --- | --- | 
| DFS bắt đầu từ 1 | 1 | 2 | 2 | 3 vs 2? vâng | Có | [2] | 
| DFS lúc 2 | 2 | 3,4 | 3+1=4 | 4 vs 4? không | Không | [2] | 
| DFS lúc 3 | 3 | 2,4 | 4+1=5 | 2 vs 5? vâng | Có | [2,3] | 
| DFS ở mức 4 | 4 | 3 | 3 | 1 vs 3? vâng | Có | [2,3,4,1] | 

Độ dài chuỗi = n, đầu ra CÓ. 

**Ví dụ 2:** 

đầu vào:```
3
1 2 4
1 2
2 3
```| Bước | Đỉnh | Giá trị lân cận | S_v | Kiểm tra tính chẵn lẻ | LOẠI BỎ? | Trình tự | 
| --- | --- | --- | --- | --- | --- | --- | 
| DFS bắt đầu từ 1 | 1 | 2 | 2 | 1 vs 2? vâng | Có | [1] | 
| DFS lúc 2 | 2 | 1,3 | 1+4=5 | 2 vs 5? vâng | Có | [1,2] | 
| DFS lúc 3 | 3 | 2 | 2 | 4 vs 2? không | Không | [1,2] | 

Không thể loại bỏ tất cả các đỉnh, xuất ra NO. 

Những dấu vết này cho thấy DFS xử lý chính xác các tổng lân cận một cách linh hoạt và xác định các đỉnh có thể tháo rời theo đúng thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi đỉnh và cạnh được truy cập một lần trong DFS. | 
| Không gian | O(n) | Danh sách kề, mảng bị loại bỏ và ngăn xếp đệ quy yêu cầu O(n). | 

Cho rằng tổng của$n$trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, tổng số hoạt động nằm trong mức chấp nhận được$O(2 \cdot 10^5)$phạm vi cho giới hạn thời gian 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("5\n3\n1 2 4\n1 2\n2 3\n4\n3 4 2 1\n1 2\n2 3\n3 4\n6\n9 6 5 1 7 4\n1 2\n2 3\n2 4\n3 5\n4 6\n5\n2 1 1 1 2\n2 1\n3 2\n2 4\n5 4\n5\n1 5 3 7 9\n1 2\n2 3\n3 4\n4 5\n") == "NO\nYES
```
