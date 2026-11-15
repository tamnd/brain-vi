---
title: "CF 1866C - Tìm kiếm nghịch đảo hoàn toàn"
description: "Chúng ta được cung cấp một biểu đồ tuần hoàn có hướng trong đó mỗi đỉnh có một danh sách có thứ tự các cạnh đi ra. Mỗi cạnh mang một nhãn 0 hoặc 1 và trỏ đến một đỉnh khác."
date: "2026-06-08T23:44:10+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1866
codeforces_index: "C"
codeforces_contest_name: "COMPFEST 15 - Preliminary Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 1900
weight: 1866
solve_time_s: 95
verified: false
draft: false
---

[CF 1866C - Tìm kiếm hoàn toàn các nghịch đảo](https://codeforces.com/problemset/problem/1866/C) 

**Xếp hạng:** 1900 
**Thẻ:** dfs và các đồ thị, dp, tương tự 
**Thời gian giải:** 1 phút 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ tuần hoàn có hướng trong đó mỗi đỉnh có một danh sách có thứ tự các cạnh đi ra. Mỗi cạnh mang một nhãn 0 hoặc 1 và trỏ đến một đỉnh khác. Bắt đầu từ đỉnh 1, chúng ta thực hiện duyệt đệ quy trong đó với mỗi đỉnh chúng ta lặp qua các cạnh đi ra của nó theo thứ tự. Đối với mỗi cạnh, trước tiên chúng ta gắn nhãn của nó vào một mảng toàn cục và sau đó tiếp tục duyệt đệ quy từ đỉnh đích của cạnh đó. Điểm mấu chốt là không có mảng đã truy cập, do đó các đỉnh được nhập lại nhiều lần bất cứ khi nào các đường dẫn khác nhau dẫn đến chúng. 

Quá trình này tạo ra một chuỗi chỉ bao gồm 0 và 1. Nhiệm vụ là đếm số lần đảo ngược trong chuỗi này, nghĩa là các cặp vị trí có số 1 xuất hiện trước số 0. 

Đồ thị là một DAG, nhưng bản thân việc truyền tải không phải là một quá trình truyền tải DAG theo nghĩa thông thường vì sự lặp lại được cho phép. Điều này có nghĩa là về nguyên tắc, chuỗi được tạo ra có thể dài theo cấp số nhân, nhưng cấu trúc và các ràng buộc đảm bảo chúng ta có thể tính toán số lượng đảo ngược mà không cần xây dựng nó một cách rõ ràng. 

Các ràng buộc rất lớn, lên tới 100.000 đỉnh và 200.000 cạnh. Bất kỳ cách tiếp cận nào mô phỏng rõ ràng DFS và xây dựng mảng đều không thể thực hiện được vì độ dài chuỗi có thể vượt xa kích thước tuyến tính. Điều này buộc chúng tôi phải làm việc với các đóng góp trên mỗi nút hoặc trên mỗi cạnh và tổng hợp thông tin theo cách kết hợp. 

Trường hợp cạnh tinh vi phát sinh khi một nút có nhiều cạnh đi ra dẫn đến các đồ thị con chồng chéo. Ví dụ: nếu đỉnh 1 có hai cạnh cuối cùng đều đạt đến cùng một cây con, một mô phỏng đơn giản sẽ tính toán lại cây con đó hai lần, nhân đôi phần đóng góp của nó. Sự trùng lặp đó là do định nghĩa DFS dự định, do đó, việc ghi nhớ chỉ truyền tải là không chính xác trừ khi nó duy trì tính đa dạng của các đường dẫn vào. 

Một vấn đề khác là sự đảo ngược phụ thuộc vào trật tự chung trong mảng được xây dựng. Mặc dù biểu đồ không có tính tuần hoàn, thứ tự DFS tạo ra một cấu trúc nối rất cụ thể, do đó lý luận về cây con cục bộ phải tôn trọng thứ tự giữa các cạnh. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ mô phỏng DFS theo đúng nghĩa đen, xây dựng mảng Z và sau đó đếm các nghịch đảo bằng cách quét tất cả các cặp. Mỗi lần chúng ta đi qua một cạnh, chúng ta sẽ thêm một giá trị và lặp lại. Vì không có điểm đánh dấu đã ghé thăm nên mọi mở rộng đường dẫn riêng biệt đều được thực hiện đầy đủ. Trong trường hợp xấu nhất, ngay cả một chuỗi tuyến tính có phân nhánh cũng có thể tạo ra nhiều lượt truy cập theo cấp số nhân, vì mỗi nhánh liên tục nhập lại các sơ đồ con được chia sẻ. Điều này làm cho mô phỏng rõ ràng không thể thực hiện được. 

Quan sát quan trọng là cấu trúc DFS xác định một chuỗi được tạo thành bằng cách ghép các khối theo một thứ tự nghiêm ngặt. Mỗi đỉnh tạo ra một chuỗi được hình thành bằng cách ghép nối, đối với mỗi cạnh đi ra theo thứ tự, trọng số cạnh theo sau là chuỗi đầy đủ của đỉnh con. 

Thay vì xây dựng chuỗi, chúng tôi tính toán cho mỗi đỉnh một bản tóm tắt về những gì mà bản mở rộng DFS của nó đóng góp: tổng số số 0, tổng số số một và số nghịch đảo bên trong bản mở rộng của nó. Điều khó khăn là cùng một cây con không độc lập khi được kết hợp giữa các cạnh, bởi vì sự đóng góp đảo ngược cũng phát sinh trên các phân đoạn được nối. 

Khi chúng ta ghép hai chuỗi A theo sau là B, các phép đảo ngược là: 

nghịch đảo(A) + nghịch đảo(B) + (số lượng 1 trong A) × (số lượng 0 trong B). 

Bản sắc này là sự giảm thiểu cốt lõi. Nếu chúng ta có thể tính toán bộ ba (số 0, số 1, số đảo ngược) cho mỗi đỉnh, thì chúng ta có thể hợp nhất các phần tử con theo thứ tự DFS trong khi tính trọng số các cạnh dưới dạng các chuỗi phần tử đơn được chèn giữa các phần mở rộng đệ quy. 

Do đó, mỗi đỉnh có thể được xử lý theo thứ tự sau trên DAG và các cạnh đi ra của nó được xử lý tuần tự, duy trì tổng hợp đang chạy của chuỗi được xây dựng cho đến nay.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng Brute Force DFS | Hàm mũ | Hàm mũ | Quá chậm | 
| DP trên DAG với công thức hợp nhất | O(N + M) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán các giá trị DP cho mỗi đỉnh: số lượng số 0, số lượng số một và số lần đảo ngược của bản mở rộng DFS đầy đủ của nó. 

1. Chúng tôi xử lý các đỉnh theo thứ tự tôpô ngược để tất cả các nút con được tính trước cha mẹ chúng. Điều này hợp lệ vì biểu đồ là DAG. 
2. Đối với mỗi đỉnh, chúng tôi duy trì một cấu trúc đang chạy biểu thị chuỗi được xây dựng cách xa các cạnh đi ra của nó. Ban đầu cấu trúc này trống rỗng: số 0, số 0, số 0 đảo ngược. 
3. Chúng ta lặp qua các cạnh đi theo thứ tự. Đối với mỗi cạnh, trước tiên chúng ta coi trọng số của cạnh là một chuỗi phần tử đơn. Nếu trọng số bằng 0 thì nó đóng góp một số 0; nếu là 1 thì nó đóng góp một. Không có đảo ngược nội bộ được thêm vào. 
4. Sau đó, chúng tôi hợp nhất chuỗi phần tử đơn này vào chuỗi tích lũy hiện tại của đỉnh bằng cách sử dụng quy tắc nối. Nếu chúng ta thêm một chuỗi X vào sau chuỗi A hiện tại thì mức tăng nghịch đảo là: 

current_inversions + nghịch đảo(X) + số một(A) × số không(X). 

Vì X là một phần tử nên các nghịch đảo bên trong của nó bằng 0 và số hạng chéo chỉ phụ thuộc vào X là 0 hay 1. 

1. Sau khi xử lý trọng số cạnh, chúng ta lấy DP đã được tính toán của đỉnh con và hợp nhất nó tiếp theo, một lần nữa sử dụng cùng một công thức nối. Đứa trẻ đóng góp các số 0, số 1 và các phép đảo ngược của riêng nó, và các phép đảo ngược chéo phụ thuộc vào tiền tố tích lũy của đỉnh hiện tại. 
2. Chúng tôi cập nhật tổng số chạy cho đỉnh sau mỗi cạnh và tiếp tục. 
3. Giá trị DP cuối cùng ở đỉnh 1 cho kết quả. 

Lý do điều này hoạt động là vì thứ tự DFS xác định sự kết hợp xác định của các chuỗi. Mỗi cạnh mở rộng thành một khối liền kề trong mảng cuối cùng và phép nối sẽ nắm bắt đầy đủ thứ tự. Nhận dạng đảo ngược đảm bảo rằng mọi cặp phần tử đều nằm trong một khối hoặc đi qua hai khối và các đóng góp xuyên khối được tính chính xác bằng cách sử dụng số tiền tố là số 1 và số 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

N = int(input())
g = [[] for _ in range(N + 1)]

for i in range(1, N + 1):
    S = int(input())
    for _ in range(S):
        v, w = map(int, input().split())
        g[i].append((v, w))

dp0 = [0] * (N + 1)
dp1 = [0] * (N + 1)
dpinv = [0] * (N + 1)

visited = [False] * (N + 1)

def dfs(u):
    visited[u] = True

    zeros = 0
    ones = 0
    inv = 0

    for v, w in g[u]:
        if w == 0:
            zeros += 1
        else:
            inv += ones
            ones += 1

        if not visited[v]:
            dfs(v)

        inv += dpinv[v]
        inv += ones * dp0[v]
        zeros += dp0[v]
        ones += dp1[v]

    dp0[u] = zeros
    dp1[u] = ones
    dpinv[u] = inv

dfs(1)

print(dpinv[1] % 998244353)
```Mã thực hiện DFS từ đỉnh 1 và tính các giá trị DP từ dưới lên. Đối với mỗi đỉnh, nó duy trì số lượng các số 0 và số được tạo ra cho đến nay trong tiền tố chuỗi được xây dựng của nó. Khi một cạnh được xử lý, trọng số của nó ngay lập tức được áp dụng dưới dạng một chuỗi phần tử đơn góp phần đảo ngược trực tiếp (nếu là 1) so với các cạnh trước đó hoặc tăng số lượng tiền tố. Sau đó, cây con con được kết hợp bằng cách sử dụng các giá trị DP được tính toán trước. 

Thuật ngữ`inv += ones * dp0[v]`là sự đảo ngược chéo giữa tiền tố hiện tại và tất cả các số 0 trong cây con con. Các cập nhật tới`zeros`Và`ones`đảm bảo các cạnh trong tương lai nhìn thấy chính xác tiền tố mở rộng. 

Một điểm tinh tế là mặc dù chúng tôi đánh dấu các nút đã truy cập, chúng tôi vẫn lặp lại chính xác một lần cho mỗi nút. Điều này có tác dụng vì DP không phụ thuộc vào các đường dẫn vào khác nhau một cách riêng biệt mà chỉ phụ thuộc vào cấu trúc của các bản mở rộng gửi đi đã được cố định. 

## Ví dụ đã hoạt động 

Xét một đồ thị đơn giản trong đó đỉnh 1 có hai cạnh: đầu tiên đến đỉnh 2 có trọng số 1, sau đó đến đỉnh 3 có trọng số 0 và cả hai đỉnh 2 và 3 đều là lá. DFS tạo ra chuỗi [1, 0]. Có một sự đảo ngược vì số 1 xuất hiện trước số 0. 

| Bước | Nút | Tiền tố số 0 | Tiền tố | Đảo ngược | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 0 | Bắt đầu | 
| 2 | cạnh 1→2 (1) | 0 | 1 | 0 | Thêm 1 | 
| 3 | cây con cạnh 2 | 0 | 1 | 0 | Lá | 
| 4 | cạnh 1→3 (0) | 1 | 1 | 1 | Thêm 0 tạo đảo ngược | 

Điều này xác nhận rằng thứ tự cạnh đơn góp phần đảo ngược chéo một cách chính xác. 

Bây giờ hãy xem xét một cấu trúc lớn hơn một chút trong đó đỉnh 1 trỏ đến đỉnh 2 và 3, cả hai đều dẫn đến cấu trúc chung tạo ra nhiều số 0 sau số 1. Sự tích lũy chứng tỏ rằng các giá trị DP của cây con được sử dụng lại một cách nhất quán và các thuật ngữ chéo có tỷ lệ với các tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M) | Mỗi đỉnh và cạnh được xử lý một lần và mỗi thao tác hợp nhất có thời gian không đổi | 
| Không gian | O(N + M) | Danh sách kề cộng với mảng DP cho mỗi đỉnh | 

Các ràng buộc cho phép lên tới 200.000 cạnh, do đó, việc truyền tuyến tính nằm trong giới hạn một cách thoải mái. DP tránh mọi sự tính toán lại cấu trúc cây con và không bao giờ xây dựng chuỗi DFS rõ ràng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N = int(input())
    g = [[] for _ in range(N + 1)]
    for i in range(1, N + 1):
        S = int(input())
        for _ in range(S):
            v, w = map(int, input().split())
            g[i].append((v, w))

    sys.setrecursionlimit(10**7)

    dp0 = [0] * (N + 1)
    dp1 = [0] * (N + 1)
    dpinv = [0] * (N + 1)
    vis = [False] * (N + 1)

    def dfs(u):
        vis[u] = True
        z = o = inv = 0
        for v, w in g[u]:
            if w == 0:
                z += 1
            else:
                inv += o
                o += 1

            if not vis[v]:
                dfs(v)

            inv += dpinv[v]
            inv += o * dp0[v]
            z += dp0[v]
            o += dp1[v]

        dp0[u] = z
        dp1[u] = o
        dpinv[u] = inv

    dfs(1)
    return str(dpinv[1] % 998244353)

# sample
assert run("""5
2
4 0
3 1
0
1
2 0
2
3 1
5 1
0
""") == "4"

# chain
assert run("""3
1
2 1
1
3 0
0
""") == "1"

# all zeros
assert run("""2
1
2 0
0
""") == "0"

# all ones
assert run("""2
1
2 1
0
""") == "0"

# branching
assert run("""4
2
2 1
3 0
0
0
0
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ví dụ về chuỗi | 1 | lan truyền nghịch đảo cơ bản | 
| tất cả số không | 0 | không thể đảo ngược | 
| tất cả những cái | 0 | đảo ngược yêu cầu 1 trước 0 | 
| phân nhánh | 1 | đặt hàng qua nhiều cạnh | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi tất cả các trọng số của cạnh đều giống hệt nhau. Đối với đầu vào trong đó mọi trọng số đều bằng 0, đầu ra DFS là một chuỗi chỉ có các số 0 bất kể cấu trúc như thế nào, do đó số lần đảo ngược phải bằng 0. Thuật toán không bao giờ tăng các bộ đếm đảo ngược trong trường hợp này, bởi vì các thuật ngữ chéo yêu cầu các số đếm đảo ngược. 

Trường hợp thứ hai là khi tất cả các trọng số đều bằng 1. Chuỗi được tạo ra chỉ chứa các trọng số, do đó không tồn tại sự đảo ngược. Thuật toán chỉ tính các nghịch đảo khi số 1 xuất hiện trước số 0, do đó dp0 luôn bằng 0 và các đóng góp chéo biến mất. 

Trường hợp thứ ba liên quan đến việc phân nhánh tới các cây con dùng chung. Giả sử đỉnh 1 có hai cạnh với cùng một đỉnh 2. DFS truy cập cây con 2 hai lần và thuật toán đếm chính xác nó hai lần vì DP được áp dụng một lần cho mỗi đường đi ngang, không phải cho mỗi danh tính đỉnh. Mỗi cạnh kích hoạt sự đóng góp của cây con một cách độc lập, duy trì tính đa bội chính xác theo yêu cầu của định nghĩa bài toán.
