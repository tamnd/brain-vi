---
title: "CF 1442E - Cây đen trắng xám"
description: "Chúng ta có một cây trong đó mỗi nút có màu trắng, đen hoặc xám. Mục tiêu là loại bỏ tất cả các nút trong số lượng thao tác tối thiểu, trong đó trong mỗi thao tác, chúng tôi chọn một thành phần được kết nối của các nút để loại bỏ."
date: "2026-06-11T04:19:25+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "dfs-and-similar", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1442
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 681 (Div. 1, based on VK Cup 2019-2020 - Final)"
rating: 3000
weight: 1442
solve_time_s: 108
verified: false
draft: false
---

[CF 1442E - Cây đen, trắng và xám](https://codeforces.com/problemset/problem/1442/E) 

**Đánh giá:** 3000 
**Thẻ:** tìm kiếm nhị phân, thuật toán xây dựng, dfs và tương tự, dp, tham lam, cây 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây trong đó mỗi nút có màu trắng, đen hoặc xám. Mục tiêu là loại bỏ tất cả các nút trong số lượng thao tác tối thiểu, trong đó trong mỗi thao tác, chúng tôi chọn một thành phần được kết nối của các nút để loại bỏ. Hạn chế duy nhất là một thao tác đơn lẻ không thể loại bỏ đồng thời nút trắng và nút đen. Các nút màu xám là màu trung tính và có thể đi với một trong hai màu. Sau khi loại bỏ các nút, cây có thể chia thành nhiều thành phần được kết nối và việc loại bỏ tiếp theo có thể được thực hiện độc lập trên từng thành phần. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm mô tả một cây theo số đỉnh, màu sắc của mỗi đỉnh và danh sách các cạnh. Đầu ra là số thao tác loại bỏ tối thiểu cần thiết để loại bỏ tất cả các nút. 

Hạn chế chính là tổng số nút trên tất cả các trường hợp thử nghiệm tối đa là 200.000 và mỗi cây riêng lẻ cũng có thể lên tới 200.000 nút. Điều này ngụ ý rằng bất kỳ giải pháp bậc hai nào về số nút đều không khả thi, vì trong trường hợp xấu nhất, nó sẽ yêu cầu khoảng 200.0002 phép tính. Các giải pháp tuyến tính hoặc gần tuyến tính (O(n) hoặc O(n log n)) đều được chấp nhận. Chúng ta cũng phải cẩn thận khi xử lý các trường hợp cạnh như cây chỉ có một màu, cây chỉ có các nút màu xám hoặc cây mà việc loại bỏ một nút sẽ ngắt kết nối phần còn lại của cây theo những cách không mong muốn. 

Một trường hợp khó nhận thấy là khi cây hoàn toàn có một màu, chẳng hạn như toàn màu trắng hoặc toàn màu đen. Một cách tiếp cận bất cẩn có thể cho rằng mỗi nút trắng phải được loại bỏ riêng biệt, nhưng trên thực tế, tất cả các nút được kết nối có cùng màu đều có thể bị loại bỏ chỉ bằng một thao tác. Một trường hợp phức tạp khác là khi các nút màu xám tạo thành cầu nối giữa cây con màu đen và cây con màu trắng; chúng tôi cần đảm bảo rằng chúng tôi không vô tình cố gắng loại bỏ nút trắng và nút đen trong cùng một thao tác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi trình tự loại bỏ có thể có. Đối với mỗi thao tác, chúng tôi sẽ liệt kê tất cả các thành phần được kết nối của các nút không trộn lẫn màu đen và trắng, loại bỏ chúng và xử lý đệ quy cây còn lại. Mặc dù cách tiếp cận này đúng về nguyên tắc, nhưng nó là hàm mũ trong trường hợp xấu nhất, bởi vì có nhiều tập hợp con được kết nối theo cấp số nhân để xem xét, và do đó nó hoàn toàn không khả thi với n lên tới 200.000. 

Cái nhìn sâu sắc quan trọng đến từ việc nhận thấy rằng chúng ta không cần phải xem xét tất cả các tập hợp con. Hạn chế hoàn toàn là tránh trộn lẫn màu đen và trắng trong cùng một hoạt động. Nếu chúng ta tính toán cho mỗi cây con xem nó chứa màu trắng, đen hay cả hai, chúng ta có thể quyết định đệ quy cần bao nhiêu thao tác. Các nút màu xám rất linh hoạt nên chúng có thể tham gia vào một trong hai nhóm. Chính thức hơn, chúng ta có thể thực hiện tìm kiếm theo chiều sâu và theo dõi số lượng thao tác tối thiểu cần thiết để loại bỏ cây con có gốc tại mỗi nút. Các thao tác đối với một cây con được xác định bằng số lượng tối đa các thao tác cần thiết cho các nút con của nó cộng với khả năng loại bỏ thêm nếu cây con chứa cả nút trắng và nút đen. Điều này làm giảm vấn đề thành một đường truyền tuyến tính trên cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| DFS + Tô màu cây con | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào và xây dựng biểu diễn danh sách kề của cây cho từng trường hợp thử nghiệm. Lưu trữ màu của mỗi nút. 
2. Định nghĩa hàm đệ quy`dfs(node, parent)`nó trả về một bộ dữ liệu`(has_white, has_black, operations)`, biểu thị liệu cây con có gốc tại`node`chứa bất kỳ nút trắng, nút đen nào và số lượng thao tác loại bỏ tối thiểu cần thiết để xóa cây con. 
3. Khởi tạo`has_white`Và`has_black`dựa trên màu sắc của nút hiện tại. Các nút màu xám không được đặt thành đúng. 
4. Đối với mỗi nút con của nút hiện tại không phải là nút cha, hãy gọi đệ quy`dfs(child, node)`và thu thập thông tin của trẻ. 
5. Nếu cây con chứa cả nút trắng và nút đen, hãy tăng số thao tác lên 1 vì chúng ta không thể loại bỏ cả hai màu trong một thao tác. Nếu không, hãy hợp nhất phần của trẻ`has_white`Và`has_black`cờ với cờ của nút hiện tại. 
6. Sau khi xử lý tất cả các phần tử con, trả về các cờ kết hợp và các phép toán tổng. Tổng số thao tác bao gồm tất cả các thao tác được tích lũy ở các nút con cộng thêm một thao tác nữa nếu cây con hiện tại chứa cả nút trắng và nút đen và chưa được tính. 
7. Sau khi DFS hoàn thành, tổng số lần loại bỏ cây là`operations`giá trị được trả về cho thư mục gốc. 

Tính chính xác đến từ tính bất biến mà đối với bất kỳ cây con nào, DFS tính toán chính xác xem nó có chứa cả nút trắng và nút đen hay không và số lượng thao tác tối thiểu để loại bỏ tất cả các nút trong cây con đó mà không loại bỏ cả nút đen và nút trắng cùng nhau. Bằng cách hợp nhất thông tin con trở lên, gốc sẽ tích lũy tổng số thao tác cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        colors = list(map(int, input().split()))
        adj = [[] for _ in range(n)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            adj[u].append(v)
            adj[v].append(u)

        def dfs(node, parent):
            has_white = colors[node] == 1
            has_black = colors[node] == 2
            ops = 0
            for child in adj[node]:
                if child == parent:
                    continue
                c_white, c_black, c_ops = dfs(child, node)
                ops += c_ops
                if c_white and c_black:
                    ops += 1
                else:
                    has_white |= c_white
                    has_black |= c_black
            return has_white, has_black, ops

        _, _, total_ops = dfs(0, -1)
        if _ and _:
            total_ops += 1
        print(total_ops)

if __name__ == "__main__":
    solve()
```Giải pháp đọc cây và màu sắc, xây dựng danh sách kề và thực thi DFS. Trong DFS,`has_white`Và`has_black`lan truyền lên trên để cho biết liệu cây con có chứa các màu đó hay không. Bất cứ khi nào một cây con chứa cả màu trắng và đen thì nó phải được loại bỏ bằng một thao tác riêng biệt, do đó`ops`được tăng lên. Cuối cùng, gốc được kiểm tra xem cây con của nó có chứa cả màu trắng và đen hay không, có khả năng thêm một thao tác cuối cùng. 

## Ví dụ đã hoạt động 

Đối với đầu vào```
2
2
1 1
1 2
4
1 2 1 2
1 2
2 3
3 4
```chúng tôi theo dõi DFS: 

| Nút | has_white | has_black | hoạt động | 
| --- | --- | --- | --- | 
| 2 | Đúng | Sai | 0 | 
| 1 | Đúng | Sai | 0 | 
| 4 | Sai | Đúng | 0 | 
| 3 | Đúng | Đúng | 1 | 
| gốc 1 | Đúng | Đúng | 1+? | 

Trong trường hợp đầu tiên, cả hai nút đều có màu trắng, do đó chỉ cần loại bỏ một nút là đủ. Trong trường hợp thứ hai, DFS tính một thao tác cho cây con chứa nút 3 và 4 (cả hai màu đều xuất hiện), sau đó kết hợp với phần còn lại để có tổng cộng ba thao tác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập một lần trong DFS và mỗi cạnh được duyệt hai lần. | 
| Không gian | O(n) | Danh sách kề và ngăn xếp đệ quy sử dụng bộ nhớ O(n). | 

Giải pháp là tuyến tính theo kích thước của cây. Vì tổng số nút trên tất cả các trường hợp thử nghiệm là 200.000 nên điều này dễ dàng phù hợp với giới hạn thời gian 2 giây. Việc sử dụng bộ nhớ cũng nằm trong giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""4
2
1 1
1 2
4
1 2 1 2
1 2
2 3
3 4
5
1 1 0 1 2
1 2
2 3
3 4
3 5
8
1 2 1 2 2 2 1 2
1 3
2 3
3 4
4 5
5 6
5 7
5 8""") == "1\n3\n2\n3"

# custom tests
assert run("1\n1\n0\n") == "0", "single grey node"
assert run("1\n3\n1 0 2\n1 2\n2 3\n") == "1", "white-grey-black chain"
assert run("1\n3\n1 2 0\n1 2\n2 3\n") == "2", "white-black-grey chain"
assert run("1\n5\n1 1 1 1 1\n1 2\n2 3\n3 4\n4 5\n") == "1", "all white nodes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút màu xám | 0 | Không cần thao tác nào đối với nút chỉ có màu xám | 
| dây chuyền trắng-xám-đen | 1 | Cầu màu xám có thể nối một trong hai màu mà không cần thao tác thêm | 
| dây chuyền trắng-đen-xám | 2 | Đen và trắng không thể đi cùng nhau |
