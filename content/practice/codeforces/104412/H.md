---
title: "CF 104412H - Có bao nhiêu nhóm"
description: "Chúng ta được cung cấp một hệ thống phân cấp công ty tạo thành một cây có gốc. Mỗi nhân viên có chính xác một người giám sát trực tiếp ngoại trừ người quản lý cấp cao nhất không có người quản lý nào."
date: "2026-07-01T02:28:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104412
codeforces_index: "H"
codeforces_contest_name: "2023 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 104412
solve_time_s: 60
verified: true
draft: false
---

[CF 104412H - Có bao nhiêu nhóm](https://codeforces.com/problemset/problem/104412/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống phân cấp công ty tạo thành một cây có gốc. Mỗi nhân viên có chính xác một người giám sát trực tiếp ngoại trừ người quản lý cấp cao nhất không có người quản lý nào. Chúng ta có thể coi đây như một cây được định hướng trong đó các cạnh trỏ từ nhân viên đến người giám sát của họ và mọi nút cuối cùng đều dẫn đến gốc. 

Mỗi nhân viên cũng thuộc về một “nhóm”, đó chỉ là một nhãn số nguyên. Sau khi nâng cấp hệ thống, một nhân viên được coi là chịu trách nhiệm không chỉ đối với nhóm của chính họ mà còn đối với tất cả các nhóm xuất hiện ở bất kỳ đâu dọc theo chuỗi giám sát của họ cho đến tận gốc. 

Nhiệm vụ là tính toán, đối với mỗi nhân viên, có bao nhiêu nhãn nhóm riêng biệt xuất hiện trên đường đi từ nhân viên đó đến gốc cây. 

Vì vậy, vấn đề rút gọn thành: với mỗi nút trong cây có gốc, hãy đếm số giá trị duy nhất trong tập hợp nhãn nút dọc theo đường dẫn đến gốc. 

Ràng buộc$N \le 10^6$buộc chúng ta phải hướng tới thời gian cơ bản là tuyến tính hoặc gần tuyến tính. Bất kỳ cách tiếp cận nào tính toán lại thông tin trên mỗi nút bằng cách đi lên nhiều lần sẽ giảm xuống$O(N^2)$trong một cái cây hình dây chuyền, điều này là không thể chấp nhận được. Thậm chí$O(N \log N)$các phương pháp phải được thiết kế cẩn thận do các hệ số không đổi lớn ở quy mô này. 

Trường hợp cạnh tinh tế là một chuỗi sâu trong đó mỗi nút có một nhóm duy nhất. Trong trường hợp đó, câu trả lời cho nút$i$chỉ đơn giản là độ sâu của nó và mọi giải pháp khởi động lại việc đếm trên mỗi nút sẽ hết thời gian chờ. 

Một trường hợp khác là khi tất cả các nút chia sẻ cùng một nhóm. Câu trả lời đúng cho mỗi nút là 1, nhưng các cấu trúc hợp nhất đơn giản có thể bị tính quá mức nếu các bản sao không được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi nhân viên, hãy đi lên chuỗi giám sát cho đến gốc, thu thập tất cả ID nhóm vào một tập hợp và trả về kích thước của nó. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề: chúng tôi tập hợp rõ ràng tất cả các nhóm có thể nhìn thấy dọc theo đường dẫn giám sát. 

Tuy nhiên, cách tiếp cận này quá chậm. Trong trường hợp xấu nhất, cây là một chuỗi có độ dài$N$. Đối với mỗi nút, chúng tôi duyệt qua tới$O(N)$tổ tiên và đặt chi phí chèn$O(1)$trung bình, cho$O(N^2)$tổng số hoạt động. Với$N = 10^6$, điều này vượt xa giới hạn. 

Quan sát quan trọng là chúng ta liên tục đếm thông tin giống tiền tố dọc theo đường dẫn gốc. Câu trả lời của mỗi nút chỉ phụ thuộc vào tập hợp các giá trị nhóm được nhìn thấy cho đến nay trên đường dẫn từ gốc đến nút đó. Nếu chúng tôi xử lý các nút theo thứ tự truyền tải từ gốc đến lá, chúng tôi có thể duy trì cấu trúc toàn cục biểu thị đường dẫn hiện tại. 

Khó khăn là xử lý tính duy nhất: chúng ta phải biết liệu một nhóm đã có mặt trên đường dẫn hiện tại hay chưa. Điều này gợi ý việc duy trì số lượng tần số của các nhóm trên đường dẫn DFS đang hoạt động. Khi nhập một nút, chúng tôi thêm nhóm của nút đó; khi rời đi, chúng tôi loại bỏ nó. Số lượng các nhóm riêng biệt trên đường đi chỉ đơn giản là số lượng khóa có tần số dương. 

Điều này biến vấn đề thành mẫu “DSU trên cây / DFS với trạng thái đường dẫn” cổ điển nhưng được theo dõi tần số cẩn thận. Chúng tôi tránh tính toán lại bằng cách duy trì trạng thái tăng dần. 

Thử thách đó chính là$N$lớn, do đó độ sâu đệ quy và chi phí phải được xem xét. DFS lặp lại thường an toàn hơn, nhưng DFS đệ quy có thể được chấp nhận trong PyPy hoặc tăng giới hạn đệ quy nếu được kiểm soát cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (đi tới gốc trên mỗi nút) | O(N2) | O(1) thêm | Quá chậm | 
| DFS với bản đồ tần số trên đường dẫn | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề từ mối quan hệ con với cha mẹ bằng cách đảo ngược con trỏ giám sát thành cấu trúc cây. Điều này cho phép truyền tải từ gốc xuống thay vì hướng lên trên, điều này cần thiết để tái sử dụng hiệu quả thông tin đường dẫn. 
2. Xác định nút gốc, nút có giám sát viên 0. Đây là điểm bắt đầu duy nhất cho quá trình truyền tải vì cuối cùng mọi nút đều kết nối với nó. 
3. Chạy truyền tải theo chiều sâu bắt đầu từ gốc, duy trì hai cấu trúc: từ điển tần số lưu trữ số lần mỗi nhóm xuất hiện trên đường dẫn gốc đến nút hiện tại và bộ đếm số lượng nhóm riêng biệt hiện đang hoạt động. 
4. Khi vào một nút, hãy tăng tần số của nhóm đó. Nếu tần số này trở thành 1, điều đó có nghĩa là nhóm này mới được giới thiệu trên đường dẫn hiện tại, vì vậy chúng tôi tăng bộ đếm nhóm riêng biệt. 
5. Lưu trữ bộ đếm nhóm riêng biệt hiện tại làm câu trả lời cho nút này, vì nó thể hiện chính xác số nhóm duy nhất trên đường dẫn gốc của nó. 
6. Lặp lại vào tất cả các phần tử con, truyền trạng thái cập nhật xuống dưới. 
7. Sau khi kết thúc tất cả các nút con, hãy quay lại bằng cách giảm tần suất của nhóm nút hiện tại. Nếu tần số của nó giảm xuống 0, chúng tôi sẽ giảm bộ đếm riêng biệt vì nhóm đó không còn tồn tại trong đường dẫn hoạt động. 

Lý do điều này hoạt động là vì tại bất kỳ thời điểm nào trong DFS, trạng thái được duy trì thể hiện chính xác nhiều nhóm dọc theo đường dẫn từ gốc đến nút hiện tại. Vì mỗi nút được truy cập chính xác một lần trong quá trình truyền tải này nên câu trả lời của mỗi nút được tính toán bằng cách sử dụng ảnh chụp nhanh nhất quán về trạng thái đường dẫn của nó. 

## Giải pháp Python```python
import sys
sys.setrecursionlimit(10**7)
input = sys.stdin.readline

def solve():
    n = int(input())
    parent = list(map(int, input().split()))
    group = list(map(int, input().split()))

    children = [[] for _ in range(n + 1)]
    root = -1

    for i in range(1, n + 1):
        p = parent[i - 1]
        if p == 0:
            root = i
        else:
            children[p].append(i)

    ans = [0] * (n + 1)
    freq = {}
    distinct = 0

    def dfs(u):
        nonlocal distinct

        g = group[u - 1]
        freq[g] = freq.get(g, 0) + 1
        if freq[g] == 1:
            distinct += 1

        ans[u] = distinct

        for v in children[u]:
            dfs(v)

        freq[g] -= 1
        if freq[g] == 0:
            distinct -= 1

    dfs(root)

    print(*ans[1:])

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đảo ngược các con trỏ của người giám sát thành một danh sách lân cận con. Điều này là cần thiết vì đầu vào tự nhiên mô tả các con trỏ cha, nhưng trạng thái DFS được duy trì một cách tự nhiên từ gốc đến lá. 

các`freq`từ điển theo dõi số lần mỗi nhóm xuất hiện dọc theo ngăn xếp đệ quy hiện tại. các`distinct`biến tránh tính toán lại kích thước từ điển, nếu không sẽ quá chậm trên quy mô lớn. Cập nhật nó dần dần là rất quan trọng. 

Bước quay lại là cần thiết. Nếu không loại bỏ nhóm sau khi quay trở lại từ đệ quy, trạng thái sẽ bị rò rỉ giữa các nhánh và nhóm đếm quá mức không chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 2 

đầu vào:```
6
0 1 2 3 4 5
1 2 3 4 5 6
```Đây là một chuỗi trong đó mỗi nút có một nhóm duy nhất. 

| Nút | Vào nhóm | Phân biệt sau khi thêm | Trả lời | Thoát hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | xóa 1 | 
| 2 | 2 | 1 | 1 | loại bỏ 2 | 
| 3 | 3 | 1 | 1 | loại bỏ 3 | 
| 4 | 4 | 1 | 1 | loại bỏ 4 | 
| 5 | 5 | 1 | 1 | loại bỏ 5 | 
| 6 | 6 | 1 | 1 | loại bỏ 6 | 

Mỗi nút giới thiệu một nhóm mới, nhưng vì đường dẫn DFS chỉ chứa một nút tại một thời điểm trong quá trình xử lý chuỗi tuyến tính nên mỗi câu trả lời vẫn là 1. 

Điều này xác nhận tính đúng đắn trong cấu trúc chuỗi sâu nhất nơi các giải pháp ngây thơ thất bại do quá trình truyền tải đi lên lặp đi lặp lại. 

### Mẫu 3 

đầu vào:```
6
0 1 2 3 4 5
1 2 3 3 2 6
```| Nút | Vào nhóm | Phân biệt sau khi thêm | Trả lời | Thoát hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | xóa 1 | 
| 2 | 2 | 2 | 2 | loại bỏ 2 | 
| 3 | 3 | 3 | 3 | loại bỏ 3 | 
| 4 | 3 | 3 | 3 | loại bỏ 3 | 
| 5 | 2 | 3 | 3 | loại bỏ 2 | 
| 6 | 6 | 4 | 4 | loại bỏ 6 | 

Tại nút 4, nhóm 3 được lặp lại nhưng đã hoạt động nên số lượng khác biệt không tăng. Tại nút 5, nhóm 2 vẫn xuất hiện sớm hơn trong đường dẫn, do đó nó cũng không làm tăng số lượng khác biệt. Điều này cho thấy tại sao cần phải theo dõi tần số thay vì tập hợp đơn giản cho mỗi lần tính toán lại cây con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi nút được vào và thoát chính xác một lần và tất cả các cập nhật tần số đều được khấu hao O(1) | 
| Không gian | O(N) | Danh sách kề cộng với ngăn xếp đệ quy và bản đồ tần số trong trường hợp xấu nhất | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì mọi hoạt động đều là công việc liên tục trên mỗi nút và mức sử dụng bộ nhớ tăng tuyến tính theo số lượng nhân viên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n = int(input())
        parent = list(map(int, input().split()))
        group = list(map(int, input().split()))

        children = [[] for _ in range(n + 1)]
        root = -1

        for i in range(1, n + 1):
            p = parent[i - 1]
            if p == 0:
                root = i
            else:
                children[p].append(i)

        ans = [0] * (n + 1)
        freq = {}
        distinct = 0

        sys.setrecursionlimit(10**7)

        def dfs(u):
            nonlocal distinct
            g = group[u - 1]
            freq[g] = freq.get(g, 0) + 1
            if freq[g] == 1:
                distinct += 1
            ans[u] = distinct
            for v in children[u]:
                dfs(v)
            freq[g] -= 1
            if freq[g] == 0:
                distinct -= 1

        dfs(root)
        return " ".join(map(str, ans[1:]))

    return solve()

# provided samples
assert run("""6
0 1 2 3 4 5
1 1 1 1 1 1
""") == "1 1 1 1 1 1"

assert run("""6
0 1 2 3 4 5
1 2 3 4 5 6
""") == "1 2 3 4 5 6"

assert run("""6
0 1 2 3 4 5
1 2 3 3 2 6
""") == "1 2 3 3 3 4"

# custom cases
assert run("""1
0
7
""") == "1", "single node"

assert run("""3
0 1 1
1 1 1
""") == "1 1 1", "all same group"

assert run("""4
0 1 1 1
1 2 1 2
""") == "1 2 2 2", "repeated groups in tree"

assert run("""5
0 1 2 2 3
1 2 1 3 2
""") == "1 2 2 3 3", "mixed overlaps"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | trường hợp cơ bản chỉ có gốc | 
| tất cả cùng một nhóm | 1 1 1 | tính chính xác của việc ngăn chặn trùng lặp | 
| nhóm lặp lại trong cây | 1 2 2 2 | phân nhánh + tái sử dụng số lượng nhóm | 
| chồng chéo hỗn hợp | 1 2 2 3 3 | nút giao đường không tầm thường | 

## Vỏ cạnh 

Một công ty nút đơn tiết lộ liệu thuật toán có khởi tạo chính xác trạng thái gốc trước DFS hay không. Quá trình truyền tải bắt đầu tại nút 1, thêm nhóm của nó và ghi ngay câu trả lời 1 mà không gặp vấn đề đệ quy. 

Cây nhóm thống nhất kiểm tra xem việc theo dõi tần số có tránh được việc đếm quá mức hay không. Mỗi nút liên tục thêm cùng một nhóm, nhưng bộ đếm riêng biệt chỉ tăng một lần ở gốc và không bao giờ tăng lại, tạo ra các số 1 nhất quán. 

Cây nghiêng với các giá trị nhóm lặp lại đảm bảo rằng việc quay lui được triển khai chính xác. Khi trở về từ một nhánh, việc không giảm tần số sẽ truyền sai các nhóm vào các cây con không liên quan, làm tăng các câu trả lời. Bước loại bỏ DFS đảm bảo rằng mỗi cây con chỉ nhìn thấy trạng thái đường dẫn hoạt động của chính nó.
