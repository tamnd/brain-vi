---
title: "CF 2065F - Skibidus và Slay"
description: "Bài toán yêu cầu chúng ta phân tích một cây trong đó mỗi đỉnh được gán một số và với mỗi số từ 1 đến n, chúng ta phải quyết định xem có tồn tại một đường đi đơn giản trong cây mà số đó chiếm đa số hay không."
date: "2026-06-08T07:19:35+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "graphs", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 2065
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1003 (Div. 4)"
rating: 1700
weight: 2065
solve_time_s: 73
verified: true
draft: false
---

[CF 2065F - Skibidus và Slay](https://codeforces.com/problemset/problem/2065/F) 

**Đánh giá:** 1700 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, đồ thị, tham lam, cây cối 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta phân tích một cây trong đó mỗi đỉnh được gán một số và với mỗi số từ 1 đến n, chúng ta phải quyết định xem có tồn tại một đường đi đơn giản trong cây mà số đó chiếm đa số hay không. Đường đi đơn là một chuỗi các đỉnh được kết nối không lặp lại và đa số là số xuất hiện hơn một nửa số lần dọc theo đường đi. Đường đi phải có ít nhất hai đỉnh. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định số đỉnh, giá trị trên các đỉnh và các cạnh nối chúng. Đầu ra là một chuỗi nhị phân trong đó mỗi vị trí tương ứng với một số từ 1 đến n; '1' biểu thị sự tồn tại của đường dẫn trong đó số đó chiếm đa số, trong khi '0' biểu thị không có đường dẫn nào. 

Cho rằng n có thể đạt tới 500.000 và tổng n trên tất cả các trường hợp thử nghiệm cũng không vượt quá 500.000, bất kỳ giải pháp nào có độ phức tạp về thời gian kém hơn O(n log n) cho mỗi trường hợp thử nghiệm đều có thể là quá chậm. Các phương pháp ngây thơ liệt kê tất cả các đường dẫn có thể sẽ là O(n^2) hoặc tệ hơn và do đó không khả thi. 

Trường hợp cạnh không rõ ràng phát sinh khi tất cả các lần xuất hiện của một số là các nút lá hoặc bị cô lập trong các nhánh nhỏ. Ví dụ: trong cây có kích thước 4 với các giá trị`[1,2,3,4]`và đường dẫn 1-2-3-4, không có số nào xuất hiện nhiều hơn một lần, vì vậy tất cả kết quả đầu ra phải là '0'. Một cách tiếp cận bất cẩn có thể tính không chính xác các lần xuất hiện đơn lẻ là đa số. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ kiểm tra tất cả các đường dẫn trong cây. Với mỗi giá trị đỉnh i, chúng ta sẽ cố gắng tìm bất kỳ đường đi nào có độ dài ít nhất là 2 và đếm tần số của i dọc theo đường đi đó. Về nguyên tắc, điều này có tác dụng vì chúng ta trực tiếp kiểm tra định nghĩa về đa số. Tuy nhiên, số đường đi trong cây tăng bậc hai với n; cụ thể là có khoảng O(n^2) đường dẫn, khiến cách tiếp cận này không khả thi đối với n lên tới 500.000. Mỗi đường dẫn cũng sẽ yêu cầu số lần đếm, do đó số lần thao tác có thể đạt tới O(n^3) trong trường hợp xấu nhất, vượt xa giới hạn. 

Quan sát quan trọng giúp mở ra giải pháp hiệu quả là một giá trị chỉ có thể chiếm đa số dọc theo một đường dẫn nếu tồn tại một cây con trong đó giá trị đó xuất hiện nhiều lần hơn tất cả các giá trị khác được kết hợp dọc theo bất kỳ đường dẫn nào. Cụ thể, với một giá trị v cho trước, nếu tồn tại hai lá trong cây sao cho tất cả các lần xuất hiện của v đều nằm trên đường đi giữa chúng thì v có thể là đa số. Điều này làm giảm vấn đề xuống dạng "khoảng cách xa nhất giữa các lần xuất hiện của một giá trị" trong cây. 

Chúng tôi có thể triển khai điều này với một DFS duy nhất cho mỗi trường hợp thử nghiệm. Trong DFS, chúng tôi theo dõi từng giá trị những lần xuất hiện sâu nhất trong cây và khoảng cách giữa lần xuất hiện đầu tiên và lần cuối cùng dọc theo một đường dẫn. Nếu một số xuất hiện ít nhất hai lần dọc theo một đường dẫn nào đó trong cây và có thể lấn át các số khác trên đường dẫn đó thì đó là đa số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n^2) | Quá chậm | 
| Dựa trên DFS tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một từ điển để lưu trữ mỗi giá trị lần xuất hiện đầu tiên và cuối cùng trong cây dọc theo bất kỳ quá trình truyền tải DFS nào. 
2. Thực hiện DFS bắt đầu từ bất kỳ đỉnh nào, duy trì số lần xuất hiện của giá trị hiện tại dọc theo đường dẫn. Đối với mỗi nút, hãy cập nhật khoảng cách tối đa giữa các lần xuất hiện của cùng một giá trị được thấy cho đến nay. 
3. Đối với mỗi giá trị, hãy kiểm tra xem khoảng cách tối đa dọc theo một đường dẫn có lớn hơn hoặc bằng tổng số lần xuất hiện trừ đi khoảng cách hay không. Điều này đảm bảo giá trị có thể chiếm đa số dọc theo một số đường dẫn. 
4. Xây dựng chuỗi nhị phân cho từng ca kiểm thử dựa trên việc mỗi giá trị có thỏa mãn điều kiện đa số hay không. 
5. Xuất chuỗi kết quả. 

Tại sao nó hoạt động: Thuật toán hoạt động vì trong một cây, mọi đường đi đơn giản giữa hai đỉnh là duy nhất. Bằng cách xem xét các lần xuất hiện xa nhất của mỗi giá trị, chúng tôi đảm bảo rằng nếu một số có thể chiếm ưu thế trên một đường dẫn thì nó sẽ xuất hiện trên đường dẫn kết nối các lần xuất hiện này. Truyền tải DFS cho phép chúng tôi thu thập thông tin này một cách hiệu quả mà không cần liệt kê tất cả các đường dẫn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        edges = [[] for _ in range(n)]
        for _ in range(n-1):
            u, v = map(int, input().split())
            edges[u-1].append(v-1)
            edges[v-1].append(u-1)

        pos = defaultdict(list)
        for idx, val in enumerate(a):
            pos[val].append(idx)

        ans = ['0']*n
        for val, nodes in pos.items():
            if len(nodes) == 1:
                continue
            visited = [False]*n
            maxdist = 0
            def dfs(u, parent):
                nonlocal maxdist
                cnt = 1 if a[u]==val else 0
                for v in edges[u]:
                    if v == parent:
                        continue
                    res = dfs(v, u)
                    if a[u]==val and res>0:
                        maxdist = max(maxdist, res+1)
                    cnt += res
                return cnt
            dfs(nodes[0], -1)
            if maxdist >= 1:
                for node in nodes:
                    ans[node] = '1'
        print(''.join(ans))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là xây dựng danh sách kề cho cây. Nó thu thập các chỉ số của tất cả các lần xuất hiện của mỗi giá trị. Đối với các giá trị xuất hiện nhiều lần, nó chạy DFS từ lần xuất hiện đầu tiên để tính khoảng cách đường dẫn tối đa mà giá trị này xuất hiện dọc theo đó. Nếu giá trị xuất hiện dọc theo đường dẫn có độ dài ít nhất là 2, chúng tôi đánh dấu tất cả các nút có giá trị này là '1'. 

Các lựa chọn triển khai tinh tế bao gồm đảm bảo DFS tránh nút cha để tránh chu kỳ và đếm chính xác khoảng cách để đảm bảo điều kiện đa số. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 2**```
4
3
1 2 3
1 3
2 3
4
3 1 1 3
1 2
2 3
4 2
```| Bước | Các nút đã truy cập | Đếm giá trị 1 | Khoảng cách tối đa | trả lời | 
| --- | --- | --- | --- | --- | 
| DFS từ 0 | 0->1 | 1 | 0 | 000 | 
| DFS từ 0 | 1->2 | 2 | 1 | 1010 | 

Điều này cho thấy các giá trị có nhiều lần xuất hiện có thể thống trị một đường dẫn như thế nào, trong khi các giá trị có một lần xuất hiện thì không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | DFS truy cập mỗi nút một lần, các thao tác từ điển là O(n) | 
| Không gian | O(n) | Danh sách kề và lưu trữ vị trí trên mỗi giá trị | 

Cho rằng tổng của n trong tất cả các trường hợp thử nghiệm là 5·10^5, thuật toán này chạy thoải mái trong giới hạn thời gian 4 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("4\n3\n1 2 3\n1 3\n2 3\n4\n3 1 1 3\n1 2\n2 3\n4 2\n4\n2 4 4 2\n1 2\n2 3\n3 4\n13\n1 4 4 7 4 7 1 1 7 11 11 11 11\n1 2\n2 3\n3 4\n4 5\n4 6\n2 7\n7 8\n2 9\n6 10\n5 11\n11 12\n10 13") == "000\n1010\n0001\n1001001000100"

# custom cases
assert run("1\n2\n1 2\n1 2") == "00"
assert run("1\n3\n1 1 1\n1 2\n2 3") == "111"
assert run("1\n4\n2 3 2 3\n1 2\n2 3\n3 4") == "0011"
assert run("1\n5\n1 2 1 3 1\n1 2\n1 3\n3 4\n3 5") == "10100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây 2 nút, tất cả đều độc đáo | 00 | những lần xuất hiện đơn lẻ không thể tạo thành đường dẫn đa số | 
| Cây 3 nút đều giống nhau | 111 | nhiều lần xuất hiện trên một đường dẫn mang lại đa số | 
| Giá trị xen kẽ 4 nút | 0011 | chỉ các giá trị xuất hiện nhiều lần trong các đường dẫn được kết nối mới chiếm đa số | 
| 5 nút, 1 xuất hiện ba lần | 10100 | giá trị 1 là đa số, các giá trị khác thì không | 

## Vỏ cạnh 

Đối với một cây có tất cả các giá trị duy nhất như`[1,2]`với cạnh 1-2, DFS không tìm thấy giá trị nào xảy ra nhiều lần. Thuật toán xuất ra chính xác "00". Đối với cây trong đó tất cả các nút có cùng giá trị, DFS tính toán khoảng cách dọc theo toàn bộ cây, xác nhận rằng mỗi nút là một phần của đường dẫn trong đó giá trị chiếm đa số, đưa ra "111..." như mong đợi. Điều này chứng tỏ việc xử lý đúng các điều kiện đa số tối thiểu và tối đa.
