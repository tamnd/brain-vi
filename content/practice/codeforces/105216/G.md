---
title: "CF 105216G - Chuyến thăm làng của Graphoria"
description: "Chúng ta được cung cấp một cây có tối đa một triệu nút và mỗi cặp nút đều ngầm tạo ra một yêu cầu di chuyển: một khách du lịch bắt đầu từ nút được đánh số nhỏ hơn và đi đến nút được đánh số lớn hơn, đi theo con đường đơn giản duy nhất trong cây."
date: "2026-06-24T17:07:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105216
codeforces_index: "G"
codeforces_contest_name: "2024 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 105216
solve_time_s: 91
verified: false
draft: false
---

[CF 105216G - Chuyến thăm làng của Graphoria](https://codeforces.com/problemset/problem/105216/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cây có tối đa một triệu nút và mỗi cặp nút đều ngầm tạo ra một yêu cầu di chuyển: một khách du lịch bắt đầu từ nút được đánh số nhỏ hơn và đi đến nút được đánh số lớn hơn, đi theo con đường đơn giản duy nhất trong cây. Mỗi đường đi như vậy đóng góp một lần đếm đường đi cho mỗi cạnh mà nó sử dụng. 

Nhiệm vụ là xác định, trên tất cả các cạnh, cạnh nào được sử dụng nhiều nhất bởi các cặp định hướng này và có bao nhiêu cạnh đạt được số lượng sử dụng tối đa đó. 

Đầu vào là một cấu trúc cây. Vì một cây có chính xác một đường đi đơn giản giữa hai nút bất kỳ nên mỗi cặp nút sẽ đóng góp một cách xác định vào tải của mỗi cạnh dọc theo đường đi đó. Đầu ra là hai số: tải tối đa trên tất cả các cạnh và có bao nhiêu cạnh đạt mức tối đa đó. 

Ràng buộc lên tới một triệu nút sẽ loại trừ mọi phương pháp liệt kê các cặp nút hoặc mô phỏng các đường dẫn riêng lẻ. Ngay cả lý luận O(N^2) cũng hoàn toàn không thể xảy ra vì điều đó sẽ bao hàm khoảng 10^12 cặp. Ngay cả O(N log N) cũng chỉ khả thi nếu công việc trên mỗi nút cực kỳ nhỏ, thường là một DFS hoặc tập hợp tuyến tính. 

Một vấn đề tế nhị nảy sinh từ điều kiện có hướng u < v. Một cách tiếp cận đếm cặp vô hướng đơn giản sẽ bỏ qua thứ tự, nhưng ở đây hướng chỉ quan trọng ở cách chọn các cặp chứ không phải ở cách các đường đi qua. Mỗi cặp không có thứ tự {u, v} vẫn đóng góp đúng một lần, vì đúng một hướng thỏa mãn u < v. 

Một sai lầm phổ biến là giả sử tính đối xứng và cố gắng đếm kích thước cây con mà không xem xét sự đóng góp toàn cục mà mỗi cạnh nhận được. Một cạm bẫy tinh vi khác là cố gắng nhổ tận gốc cây và đếm số lần đóng góp bằng cách sử dụng kích thước cây con đơn giản mà không kết nối cẩn thận với số lượng cặp giao nhau trên một cạnh. 

## Phương pháp tiếp cận 

Giải thích bạo lực sẽ xem xét mọi cặp nút u và v có u < v, tính toán đường dẫn giữa chúng bằng cách sử dụng BFS hoặc LCA và tăng bộ đếm dọc theo các cạnh của đường dẫn đó. Điều này đúng về mặt khái niệm vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, có Θ(N^2) các cặp như vậy và mỗi đường dẫn có thể nhận O(N) trong trường hợp xấu nhất, dẫn đến hành vi O(N^3) trong cây chuỗi suy biến. Ngay cả với quá trình tiền xử lý cho LCA, mỗi cặp vẫn tốn O(1) để tính toán LCA nhưng vẫn yêu cầu suy luận về sự đóng góp cho các cạnh, không thể tích lũy trực tiếp trên mỗi cạnh nếu không có cấu trúc bổ sung. 

Quan sát quan trọng là chúng ta không cần các đường dẫn riêng lẻ. Mỗi cạnh chia cây thành hai thành phần sau khi bị loại bỏ. Bất kỳ cặp nút nào có điểm cuối nằm trong các thành phần khác nhau đều phải đi qua cạnh đó. Vì vậy, số lần một cạnh được sử dụng chỉ phụ thuộc vào kích thước hai cạnh của cạnh đó. Điều này chuyển đổi vấn đề từ việc liệt kê đường dẫn sang tính toán kích thước cây con. 

Khi chúng ta root cây một cách tùy ý, mỗi cạnh sẽ kết nối một nút với nút cha của nó. Việc loại bỏ cạnh như vậy sẽ tách cây thành một cây con có kích thước s và phần còn lại có kích thước N − s. Số cặp không có thứ tự đi qua cạnh đó là s × (N − s). Vì mỗi cặp tương ứng với chính xác một lần duyệt tập hợp cạnh trên đường đi của nó nên công thức này đưa ra số lượng sử dụng cạnh chính xác. 

Chúng tôi tính toán kích thước cây con bằng DFS, đánh giá giá trị này cho mọi cạnh và theo dõi mức tối đa cũng như số lượng cạnh đạt được giá trị đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng cặp Brute Force | O(N^3) trường hợp xấu nhất | O(N) | Quá chậm | 
| Đếm kích thước cây con | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Root cây tại nút 1 và xây dựng danh sách kề để truyền tải. Điều này đưa ra hướng xác định mối quan hệ cha-con mà không làm thay đổi cấu trúc cây. 
2. Chạy DFS từ gốc để tính toán kích thước của từng cây con. Đối với nút u, kích thước cây con của nó là 1 cộng với tổng kích thước cây con của tất cả các nút con. 
3. Trong DFS, đối với mỗi cạnh giữa nút u và nút con v của nó, hãy tính phần đóng góp của cạnh đó là sz[v] × (N − sz[v]). Điều này đại diện cho tất cả các cặp phải đi qua cạnh này vì một điểm cuối nằm bên trong cây con và điểm cuối kia nằm bên ngoài. 
4. Duy trì hai biến trong khi xử lý các cạnh: tải cạnh tối đa được thấy cho đến nay và có bao nhiêu cạnh đạt được mức tối đa đó. Cập nhật chúng sau khi tính toán sự đóng góp của mỗi cạnh. 
5. Sau khi DFS hoàn tất, xuất giá trị lớn nhất và tần số của nó. 

Điểm quan trọng là mỗi cạnh được đánh giá chính xác một lần khi chúng ta biết kích thước của cây con bên dưới nó, đảm bảo hoạt động tuyến tính. 

### Tại sao nó hoạt động 

Mỗi cặp nút đóng góp vào chính xác một đường dẫn và đường dẫn đó đi qua chính xác các cạnh phân tách hai điểm cuối thành các thành phần khác nhau. Đối với bất kỳ cạnh cố định nào, việc loại bỏ nó sẽ chia cây thành hai tập hợp nút riêng biệt. Một cặp đóng góp vào cạnh đó khi và chỉ khi điểm cuối của nó nằm trong các tập hợp khác nhau. Số lượng các cặp như vậy chính xác là tích của kích thước của hai thành phần, do đó, sự đóng góp của mọi cạnh hoàn toàn được xác định bởi kích thước cây con, không phụ thuộc vào cấu trúc tổng thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    N = int(input())
    g = [[] for _ in range(N + 1)]

    for _ in range(N - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    parent = [0] * (N + 1)
    sz = [0] * (N + 1)

    max_val = 0
    count = 0

    def dfs(u):
        nonlocal max_val, count
        sz[u] = 1
        for v in g[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            dfs(v)
            sz[u] += sz[v]

            contrib = sz[v] * (N - sz[v])
            if contrib > max_val:
                max_val = contrib
                count = 1
            elif contrib == max_val:
                count += 1

    dfs(1)

    print(max_val, count)

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng một lần truyền tải DFS duy nhất. Mảng cha ngăn chặn việc truy cập lại cha mẹ trong danh sách kề không được định hướng. Việc tích lũy kích thước cây con được thực hiện từ dưới lên. Mỗi khi chúng ta khám phá xong một thành phần con, chúng ta sẽ biết ngay kích thước của thành phần bên con tương ứng và có thể tính toán phần đóng góp của cạnh. 

Một chi tiết triển khai tinh tế là độ sâu đệ quy. Với N lên tới 10^6, giới hạn đệ quy mặc định của Python là không đủ nên phải tăng lên đáng kể. Một chi tiết quan trọng khác là chúng tôi tính toán các đóng góp của cạnh trong DFS thay vì lưu trữ kích thước cây con và lặp lại sau đó, điều này tránh được chi phí bổ sung về bộ nhớ và giữ mọi thứ ở mức O(N). 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
7
1 2
1 3
2 4
2 5
3 6
3 7
```Chúng tôi root ở mức 1. 

| Nút | Kích thước cây con | Đóng góp cạnh (nếu có) | 
| --- | --- | --- | 
| 4 | 1 | | 
| 5 | 1 | | 
| 2 | 3 | 1×6 cho cạnh 2-1 | 
| 6 | 1 | | 
| 7 | 1 | | 
| 3 | 3 | 3×4 cho cạnh 3-1 | 
| 1 | 7 | | 

Các cạnh: 

Cạnh 1-2: 3 × 4 = 12 

Cạnh 1-3: 3 × 4 = 12 

Cạnh 2-4: 1 × 6 = 6 

Cạnh 2-5: 1 × 6 = 6 

Cạnh 3-6: 1 × 6 = 6 

Cạnh 3-7: 1 × 6 = 6 

Tối đa là 12, xuất hiện ở hai cạnh. 

Dấu vết này cho thấy sự đối xứng kích thước của cây con ở gốc dẫn đến các cạnh nặng giống hệt nhau. 

### Mẫu 2 

đầu vào:```
5
1 2
2 3
3 4
4 5
```Đây là một chuỗi bắt nguồn từ 1. 

| Cạnh | Kích thước cây con | Đóng góp | 
| --- | --- | --- | 
| 1-2 | 4 | 4×1 = 4 | 
| 2-3 | 3 | 3×2 = 6 | 
| 3-4 | 2 | 2×3 = 6 | 
| 4-5 | 1 | 1×4 = 4 | 

Tối đa là 6, xuất hiện ở hai cạnh giữa. 

Điều này chứng tỏ rằng các cạnh trung tâm trong một đường dẫn tối đa hóa tích cây con chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi nút và cạnh được truy cập một lần trong DFS và mỗi đóng góp của cạnh được tính theo thời gian không đổi | 
| Không gian | O(N) | Danh sách kề, ngăn xếp đệ quy và mảng phụ lưu trữ thông tin tuyến tính | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì các phép toán 10^6 là khả thi trong DFS thời gian tuyến tính với số học đơn giản trên mỗi cạnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N = int(input())
    g = [[] for _ in range(N + 1)]
    for _ in range(N - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    sys.setrecursionlimit(10**7)
    parent = [0] * (N + 1)
    sz = [0] * (N + 1)

    max_val = 0
    count = 0

    def dfs(u):
        nonlocal max_val, count
        sz[u] = 1
        for v in g[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            dfs(v)
            sz[u] += sz[v]
            contrib = sz[v] * (N - sz[v])
            if contrib > max_val:
                max_val = contrib
                count = 1
            elif contrib == max_val:
                count += 1

    dfs(1)
    return f"{max_val} {count}"

# sample 1
assert run("""7
1 2
1 3
2 4
2 5
3 6
3 7
""") == "12 2"

# sample 2
assert run("""5
1 2
2 3
3 4
4 5
""") == "6 2"

# minimum chain
assert run("""2
1 2
""") == "1 1"

# star centered at 1
assert run("""5
1 2
1 3
1 4
1 5
""") == "4 4"

# balanced tree
assert run("""7
1 2
1 3
2 4
2 5
3 6
3 7
""") == "12 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây 2 nút | 1 1 | độ chính xác cấu trúc tối thiểu | 
| chuỗi | 6 2 | tối đa hóa cạnh giữa | 
| ngôi sao | 4 4 | đóng góp cạnh thống nhất | 

## Vỏ cạnh 

Trong cây hai nút, DFS ngay lập tức tính toán đóng góp của một cạnh là 1 × 1 = 1 và nó trở thành cả giá trị lớn nhất và duy nhất, xác nhận việc xử lý chính xác cây nhỏ nhất. 

Trong cây hình ngôi sao, mỗi cạnh lá có kích thước cây con là 1, cho kết quả đóng góp giống nhau là 1 × (N − 1). Thuật toán truy cập từng cạnh lá một lần trong DFS và tăng bộ đếm tần số bằng nhau một cách nhất quán, đảm bảo các mối quan hệ được tính chính xác. 

Trong một chuỗi dài, kích thước cây con giảm đều đặn dọc theo quá trình truyền tải. Đóng góp của mỗi cạnh được tính toán chính xác một lần khi trở về từ đệ quy và các cạnh trung tâm tự nhiên tạo ra sản phẩm cao nhất, xác thực rằng phân tách dựa trên DFS nắm bắt chính xác phân phối cặp toàn cầu mà không liệt kê rõ ràng các cặp.
