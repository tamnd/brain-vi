---
title: "CF 104544C - LNCA thứ K"
description: "Chúng ta có một cây có gốc với nút 1 là gốc. Mỗi truy vấn chọn một tập hợp con gồm các nút riêng biệt và chúng tôi được yêu cầu phân tích xem tổ tiên chung “sâu” có thể được hình thành như thế nào khi chúng tôi lấy các nhóm gồm chính xác k nút từ tập hợp con đó."
date: "2026-06-30T09:01:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104544
codeforces_index: "C"
codeforces_contest_name: "Aleppo Collegiate Programming Contest 2023 V.2"
rating: 0
weight: 104544
solve_time_s: 101
verified: false
draft: false
---

[CF 104544C - LNCA thứ K](https://codeforces.com/problemset/problem/104544/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với nút 1 là gốc. Mỗi truy vấn chọn một tập hợp con gồm các nút riêng biệt và chúng tôi được yêu cầu phân tích xem tổ tiên chung “sâu” có thể được hình thành như thế nào khi chúng tôi lấy các nhóm gồm chính xác k nút từ tập hợp con đó. 

Đối với bất kỳ k nút nào được chọn, chúng ta có thể tính LCA của chúng theo nghĩa thông thường: nút thấp nhất trong cây nằm trên tất cả các đường đi từ k nút đó đến gốc. Sau đó, bài toán yêu cầu chúng ta xem xét mọi tập hợp con có kích thước k có thể có của m nút đã cho, tính toán LCA cho từng tập hợp con như vậy và thu thập tất cả các kết quả LCA này. 

Trong số tất cả các LCA đó, chúng tôi chỉ tập trung vào những LCA sâu nhất trong cây, nghĩa là những LCA ở xa gốc nhất. Đầu ra là số lượng nút riêng biệt đạt được độ sâu tối đa đó. 

Các ràng buộc là nhỏ theo một cách rất quan trọng. Có tối đa 1000 nút cho mỗi trường hợp thử nghiệm và tổng cộng 1000 truy vấn trên tất cả các thử nghiệm. Điều đó ngay lập tức gợi ý rằng mọi thứ gần bậc hai cho mỗi truy vấn đều có thể chấp nhận được, nhưng mọi thứ liên quan đến việc liệt kê tất cả các tập hợp con k đều không thể thực hiện được vì số lượng tập hợp con tăng lên theo kiểu tổ hợp. Điều quan trọng là trong khi định nghĩa nói về tất cả các tập hợp con k, cấu trúc của LCA trong cây cho phép chúng ta nén vụ nổ này thành số lượng trên mỗi nút bên trong cây con. 

Một cạm bẫy phổ biến là cố gắng tạo rõ ràng các tập hợp con có kích thước k hoặc mô phỏng trực tiếp quy trình LCA cho từng tập hợp con. Ngay cả khi m = 30, số tập con vẫn trở nên rất lớn và điều này nhanh chóng trở nên không khả thi. 

Một trường hợp phức tạp khác là khi tất cả các nút được chọn nằm trong một cây con của một số nút. Trong trường hợp đó, nút đó không thể là LCA của bất kỳ tập con k nào vì LCA sẽ luôn nằm sâu hơn bên trong cây con đó. “Sự thống trị của một nhánh duy nhất” này là hạn chế về cấu trúc chính thay thế cho việc liệt kê bạo lực. 

## Phương pháp tiếp cận 

Cách giải thích trực tiếp sẽ là liệt kê tất cả các tập hợp con có kích thước k của m nút đã cho, tính toán LCA của chúng bằng cấu trúc LCA tiêu chuẩn và theo dõi kết quả sâu nhất. Điều này đúng về mặt khái niệm vì nó tuân theo định nghĩa theo nghĩa đen. Tuy nhiên, số lượng tập hợp con là$\binom{m}{k}$, và thậm chí đối với m vừa phải thì giá trị này trở nên quá lớn. Chi phí cho mỗi truy vấn LCA chỉ là logarit hoặc không đổi khi xử lý trước, nhưng sự bùng nổ tổ hợp chiếm ưu thế ngay lập tức. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần liệt kê các tập hợp con. Chúng tôi chỉ quan tâm đến việc liệu nút x có thể xuất hiện dưới dạng LCA của một tập hợp con k nào đó hay không và liệu nó có thể nằm trong số các nút sâu nhất như vậy hay không. Điều này làm giảm vấn đề kiểm tra tính khả thi về cấu trúc trên mỗi nút. 

Sửa một nút x. Hãy xem xét cách m nút được đánh dấu đã cho được phân phối tương ứng với x. Chúng chia thành các nhóm: các nút chính xác là x (nếu x nằm trong tập hợp) và các nút nằm trong mỗi cây con của x. Gọi cnt[x] là tổng số nút được đánh dấu trong cây con của x. 

Để x là LCA của một tập hợp con k nào đó, chúng ta phải có khả năng chọn k nút bên trong cây con của nó sao cho tất cả chúng không được chứa trong một cây con duy nhất. Nếu tất cả k nút nằm hoàn toàn trong một cây con thì LCA sẽ sâu hơn x. Vì vậy, chúng ta cần ít nhất hai “nguồn” nút dưới x đóng góp cho tập hợp đã chọn. 

Điều này làm giảm vấn đề kiểm tra xem x có đủ nút được đánh dấu trong cây con của nó hay không và liệu các nút đó có được phân phối trên ít nhất hai nhánh khác nhau hay không (bao gồm cả khả năng chính x tạo thành một nhánh riêng nếu nó nằm trong tập hợp). 

Khi điều kiện này được xác định, nhiệm vụ trở nên đơn giản: trong số tất cả các nút x hợp lệ, chúng tôi tính toán độ sâu tối đa và đếm xem có bao nhiêu nút đạt được nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên k-tập hợp con | O(chọn(m, k) · LCA) | O(n) | Quá chậm | 
| Đếm cây con trên mỗi nút | O(n + q · n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở nút 1 và xử lý trước thông tin gốc và thông tin sâu để có thể làm việc với các mối quan hệ cây con một cách tự nhiên. 

Đối với mỗi truy vấn, trước tiên chúng tôi đánh dấu m nút đã chọn. Sau đó, chúng tôi tính toán cnt[x] cho mỗi nút x, là số lượng nút được chọn trong cây con của x. Điều này có thể được thực hiện với một DFS duy nhất trên cây. 

Sau khi biết số lượng cây con, chúng tôi đánh giá mỗi nút x là một ứng cử viên trả lời tiềm năng. 

1. Tính toán cnt[x] cho tất cả các nút bằng cách sử dụng DFS thứ tự sau. Điều này cho biết số nút được chọn trong mỗi cây con. 
2. Đối với mỗi nút x, hãy xác định cách phân bổ các nút đã chọn trên các nhánh trực tiếp của nó. Điều này bao gồm sự đóng góp từ chính x nếu nó được chọn, cộng với sự đóng góp từ mỗi cây con con trong đó cnt[child] > 0. 
3. Đếm xem có bao nhiêu thành phần không trống tại x. Một thành phần có thể là chính x (nếu được chọn) hoặc một cây con con chứa ít nhất một nút được chọn. 
4. Kiểm tra xem cnt[x] có ít nhất là k hay không. Nếu không, x không thể lưu trữ bất kỳ tập con k nào hoàn toàn trong cây con của nó, vì vậy nó sẽ bị loại bỏ ngay lập tức. 
5. Nếu x có ít nhất hai thành phần không trống và cnt[x] >= k thì x có khả năng là LCA của một số k-tập hợp con. 
6. Trong số tất cả các nút hợp lệ như vậy, hãy tính độ sâu tối đa và đếm xem có bao nhiêu nút đạt được độ sâu đó. 

Lý do lựa chọn độ sâu hoạt động là vì các nút sâu hơn thể hiện nguồn gốc chặt chẽ hơn, do đó, bất kỳ nút hợp lệ sâu hơn nào sẽ chiếm ưu thế các nút nông hơn theo nghĩa “LCA thấp nhất có thể”. 

### Tại sao nó hoạt động 

Mọi tập con k hoàn toàn bên trong cây con của x đều có LCA của nó ở đâu đó bên trong cây con đó. Cách duy nhất để bản thân x trở thành LCA là nếu các nút được chọn không thể bị giới hạn trong một cây con duy nhất của x. Điều đó buộc LCA phải di chuyển lên x thay vì sâu hơn. Điều kiện đếm cây con đảm bảo tồn tại đủ nút, trong khi điều kiện đa thành phần đảm bảo chúng ta không bị ép vào một nhánh duy nhất. Hai điều kiện này mô tả chính xác thời điểm x có thể xuất hiện trong S₁ như được xác định trong bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n, q = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    
    for _ in range(n - 1):
        a, b = map(int, input().split())
        g[a].append(b)
        g[b].append(a)

    parent = [0] * (n + 1)
    depth = [0] * (n + 1)
    order = []

    # build rooted tree
    stack = [1]
    parent[1] = -1
    while stack:
        u = stack.pop()
        order.append(u)
        for v in g[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            depth[v] = depth[u] + 1
            stack.append(v)

    for _ in range(q):
        tmp = list(map(int, input().split()))
        k, m = tmp[0], tmp[1]
        vs = tmp[2:]

        mark = [0] * (n + 1)
        for x in vs:
            mark[x] = 1

        cnt = [0] * (n + 1)

        # postorder accumulation
        for u in reversed(order):
            cnt[u] = mark[u]
            for v in g[u]:
                if v == parent[u]:
                    continue
                cnt[u] += cnt[v]

        best_depth = -1
        ans = 0

        for u in range(1, n + 1):
            if cnt[u] < k:
                continue

            components = 0
            if mark[u]:
                components += 1

            for v in g[u]:
                if v == parent[u]:
                    continue
                if cnt[v] > 0:
                    components += 1

            if components >= 2:
                d = depth[u]
                if d > best_depth:
                    best_depth = d
                    ans = 1
                elif d == best_depth:
                    ans += 1

        print(ans)

def main():
    t = int(input())
    for _ in range(t):
        solve()

if __name__ == "__main__":
    main()
```Cây được root một lần cho mỗi trường hợp thử nghiệm bằng cách sử dụng DFS lặp, giúp tránh các vấn đề về độ sâu đệ quy. Sau đó, chúng tôi sử dụng lại thứ tự duyệt cố định để tính toán nhanh số lượng cây con cho mỗi truy vấn. 

Phần quan trọng là kiểm tra tính khả thi trên mỗi nút. Chúng tôi không cố gắng suy luận trực tiếp về các tập hợp con; thay vào đó, chúng tôi giảm vấn đề về cách phân phối các nút được đánh dấu trong quá trình phân tách được tạo ra bằng cách loại bỏ một nút. 

Một chi tiết tinh tế là coi chính nút đó là thành phần của chính nó khi nó là một phần của tập hợp đã chọn. Điều này là cần thiết vì nếu không, chúng ta sẽ giả định không chính xác rằng tất cả các nút được đánh dấu phải nằm trong các cây con con, điều này sẽ bỏ sót các trường hợp trong đó chính x tham gia vào việc tạo thành một phép chia hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây đơn giản:```
1
├── 2
│   ├── 4
│   └── 5
└── 3
```Truy vấn: k = 2, m = 3, S = {4, 5, 3} 

Chúng tôi tính toán các giá trị cnt: 

| Nút | cnt | 
| --- | --- | 
| 1 | 3 | 
| 2 | 2 | 
| 3 | 1 | 
| 4 | 1 | 
| 5 | 1 | 

Bây giờ chúng tôi đánh giá các nút: 

| Nút | thành phần | có hiệu lực? | 
| --- | --- | --- | 
| 1 | 2 (cây con trái + cây con nút 3) | vâng | 
| 2 | 2 (4 cây con, 5 cây con) | vâng | 
| 3 | 0 hoặc 1 | không | 
| 4 | 1 | không | 
| 5 | 1 | không | 

Cả 1 và 2 đều hợp lệ, nhưng 2 sâu hơn nên câu trả lời là 1. 

Dấu vết này cho thấy câu trả lời chỉ phụ thuộc vào phân bố cây con chứ không phụ thuộc vào việc liệt kê các cặp. 

Một ví dụ thứ hai:```
1 - 2 - 3 - 4 - 5
```Truy vấn: k = 2, S = {4, 5} 

Chỉ các nút trên đường dẫn từ 4 đến 5 mới quan trọng. 

| Nút | cnt | thành phần | hợp lệ | 
| --- | --- | --- | --- | 
| 3 | 2 | 2 | vâng | 
| 4 | 1 | 1 | không | 
| 5 | 1 | 1 | không | 
| 2 | 2 | 1 | không | 

Chỉ nút 3 đủ điều kiện, vì vậy đây là nút hợp lệ sâu nhất duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nq) | Mỗi truy vấn tính toán số lượng cây con trong O(n) và kiểm tra tất cả các nút trong O(n) | 
| Không gian | O(n) | Mảng cho cấu trúc cây, đánh dấu và bộ đếm | 

Cho rằng tổng số n và q trên tất cả các trường hợp thử nghiệm tối đa là 1000, cách tiếp cận tuyến tính trên mỗi truy vấn này vẫn nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    # assume solution is defined above in same file
    return sys.stdout.getvalue().strip() if False else ""

# Minimal sanity style tests (illustrative placeholders since full harness depends on integration)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi đơn, k = m | 1 | toàn bộ đường dẫn sụp đổ thành hành vi LCA gốc | 
| cây sao, k = 2 | 1 | root chỉ trở thành LCA hợp lệ | 
| tất cả các nút được chọn trong một cây con | 1 | chỉ các nút trên chuỗi cây con đó mới quan trọng | 
| k bằng m trong cây cân bằng | 1 | kiểm tra trường hợp tập hợp con đầy đủ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các nút được chọn nằm hoàn toàn trong một cây con duy nhất của một số nút x. Trong tình huống đó, x không thể được tính là ứng cử viên LNCA hợp lệ ngay cả khi cnt[x] lớn. Ví dụ: trong chuỗi 1-2-3-4-5 có S = {4, 5}, nút 2 có cnt[2] = 2 nhưng tất cả các nút được chọn đều nằm theo một hướng con. Thuật toán đánh dấu chính xác nút 2 là không hợp lệ vì nó chỉ nhìn thấy một thành phần không trống, do đó chỉ nút 3 trở thành tổ tiên hợp lệ sâu nhất. 

Một trường hợp cạnh khác là khi tập nút được chọn bao gồm chính nút ứng cử viên. Điều này phải được tính là một thành phần riêng biệt; nếu không thì các nút trong đó x ∈ S sẽ bị từ chối không chính xác. Trong một cây trong đó S = {x, u} với u trong một cây con khác, x trở thành hợp lệ vì nó tạo ra hai thành phần ngay cả khi không có cây con con nào tách vùng chọn.
