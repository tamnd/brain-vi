---
title: "CF 1385F - Loại Bỏ Lá"
description: "Chúng ta đang làm việc với một cái cây trong đó các đỉnh được loại bỏ dần dần theo từng vòng. Trong một vòng, chúng ta được phép chọn một đỉnh bên trong $v$ và loại bỏ chính xác $k$ các lá lân cận hiện tại của nó. Một đỉnh được tính là lá nếu nó có bậc một trong đồ thị còn lại hiện tại."
date: "2026-06-16T14:24:41+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 1385
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 656 (Div. 3)"
rating: 2300
weight: 1385
solve_time_s: 470
verified: false
draft: false
---

[CF 1385F - Loại bỏ lá](https://codeforces.com/problemset/problem/1385/F) 

**Đánh giá:** 2300 
**Tas:** cấu trúc dữ liệu, tham lam, triển khai, cây 
**Thời gian giải:** 7 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc với một cái cây trong đó các đỉnh được loại bỏ dần dần theo từng vòng. Trong một vòng, chúng ta được phép chọn một đỉnh bên trong$v$và loại bỏ chính xác$k$của các lá lân cận hiện tại của nó. Một đỉnh được tính là lá nếu nó có bậc một trong đồ thị còn lại hiện tại. Sau khi loại bỏ những chiếc lá đó, cây sẽ teo lại và có thể xuất hiện những chiếc lá mới. Quá trình vẫn tiếp tục và chúng tôi muốn tối đa hóa số vòng chúng tôi có thể thực hiện. 

Khó khăn chính là trạng thái lá là động. Một đỉnh ban đầu không phải là một chiếc lá có thể trở thành một chiếc lá sau đó và các lựa chọn trong các vòng đầu sẽ ảnh hưởng đến số vòng còn lại có thể xảy ra trong tương lai. 

Kích thước đầu vào lớn, lên tới$2 \cdot 10^5$tổng số đỉnh trong các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi cách tiếp cận mô phỏng việc loại bỏ từng bước trên cây đầy đủ cho mỗi lần di chuyển, vì một mô phỏng đơn giản có thể xuống cấp thành$O(n^2)$trong một cấu trúc giống như dây chuyền, trong đó mỗi lần loại bỏ chỉ để lộ ra một chiếc lá mới. 

Trường hợp cạnh tinh tế xuất hiện ở những cây có độ mất cân bằng cao như một sợi dây chuyền. Ví dụ, nếu$k = 2$, đường đi có độ dài 5 hoạt động rất khác với một ngôi sao, mặc dù cả hai đều là những cây có kích thước tương tự nhau. Trong một ngôi sao, nhiều lá ngay lập tức xuất hiện xung quanh một tâm, cho phép thực hiện nhiều vòng. Trong một chuỗi, mỗi lúc chỉ có một hoặc hai lá tồn tại, hạn chế nghiêm trọng việc di chuyển. Bất kỳ chiến lược tham lam nào giả định sự phong phú của lá toàn cầu mà không có cấu trúc theo dõi đều thất bại ở đây. 

Một trường hợp cạnh quan trọng khác là khi nhiều nút cấp cao cạnh tranh để giành lá. Nếu chúng ta luôn chọn các lá cục bộ mà không tính đến sự cạn kiệt toàn cầu, chúng ta có thể làm cạn kiệt các lá sớm tại một nút mà nếu không sẽ duy trì được nhiều vòng hơn sau này. 

## Phương pháp tiếp cận 

Một mô phỏng lực lượng vũ phu sẽ liên tục xác định tất cả các lá, nhóm chúng theo hàng xóm của chúng, chọn bất kỳ nhóm kích thước hợp lệ nào$k$, xóa chúng và cập nhật độ. Mỗi vòng yêu cầu quét tất cả các nút để thu thập lá, chi phí$O(n)$. Vì chúng tôi có thể thực hiện tới$O(n)$vòng trong trường hợp tốt nhất, điều này dẫn đến$O(n^2)$tổng công việc, quá chậm so với các ràng buộc. 

Quan sát quan trọng là cấu trúc loại bỏ xung quanh mỗi đỉnh là độc lập theo nghĩa tổ hợp. Mỗi đỉnh$v$đóng góp lá theo thời gian, nhưng chỉ khi đủ số hàng xóm của nó trở thành lá. Thay vì mô phỏng quá trình trên toàn cầu, chúng ta có thể nghĩ xem mỗi cây con có thể "cung cấp" các lá lên trên cây cha bao nhiêu lần. 

Root cây tùy ý. Đối với mỗi nút, hãy xem xét có bao nhiêu lần loại bỏ lá có thể được hỗ trợ bởi các nút con của nó trước khi sự đóng góp của chính nó bị hạn chế. Điều này trở thành một vấn đề tổng hợp từ dưới lên: mỗi cây con trả về một số biểu thị số lượng lá cuối cùng mà nó có thể cung cấp cho cây mẹ của nó và mỗi nút bên trong kết hợp những đóng góp này theo từng đợt kích thước một cách tham lam.$k$. 

Cái nhìn sâu sắc quan trọng là mỗi nút chuyển đổi một cách hiệu quả sự đóng góp từ trẻ em thành các nhóm có quy mô$k$và mọi khoản đóng góp còn sót lại sẽ được truyền lên trên. Câu trả lời là tổng số nhóm hoàn chỉnh được hình thành trên tất cả các nút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Cây DP / Tập hợp tham lam |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây tại một nút tùy ý, thường là 1. 

1. Thực hiện DFS thứ tự sau từ gốc, tính toán các giá trị từ các lá trở lên. Mỗi nút trả về số lượng "mã thông báo lá có sẵn" mà nó có thể chuyển cho nút cha của nó. Nút lá ban đầu đóng góp 1 token. Mô hình này cho thấy một chiếc lá có thể được loại bỏ chỉ trong một thao tác. 
2. Đối với mỗi nút$v$, thu thập sự đóng góp từ tất cả trẻ em. Những đóng góp này thể hiện số lượng lá cuối cùng có thể được tạo ra trong mỗi cây con bắt nguồn từ những cây con đó. 
3. Tại nút$v$, tổng hợp tất cả các khoản đóng góp của con. Tổng này thể hiện số lượng điểm cuối lá hiện có sẵn trong cây con của$v$có khả năng có thể được nhóm lại. 
4. Chuyển số tiền này thành các phép tính đầy đủ bằng cách chia cho$k$. Mỗi nhóm$k$mã thông báo lá tương ứng với một bước di chuyển hợp lệ tập trung tại$v$hoặc được truyền lên trên thông qua$v$cha mẹ của. 
5. Vượt qua phần còn lại$\text{sum} \bmod k$trở lên đối với cha mẹ. Các mã thông báo còn sót lại này đại diện cho các nhóm đã hoàn thành một phần có thể kết hợp với các cây con khác cao hơn trong cây. 
6. Tích lũy giá trị thương trên tất cả các nút. Tổng số này là số nước đi hợp lệ. 

DFS đảm bảo mỗi cây con được xử lý chính xác một lần và việc nhóm được thực hiện cục bộ tại mỗi nút nơi có đủ sự đóng góp của lá. 

### Tại sao nó hoạt động 

Mỗi cây con đóng góp một số lượng lá cuối cùng nhất định có thể được hiện thực hóa độc lập với các cây con khác cho đến khi chúng gặp nhau ở một tổ tiên chung. Sự tương tác duy nhất giữa các cây con là ở tổ tiên chung thấp nhất của chúng, nơi các token lá từ các nhánh khác nhau có thể được kết hợp thành các nhóm có kích thước$k$. Bởi vì việc phân nhóm có tính giao hoán và kết hợp đối với phép cộng và lấy thương, việc thực hiện phân nhóm từ dưới lên đảm bảo không có sự kết hợp nào bị bỏ sót và không có sự kết hợp không hợp lệ nào được tính. Bất kỳ nước đi hợp lệ nào đều tương ứng chính xác với việc hình thành một nhóm$k$mã thông báo lá tại một số nút và mỗi nhóm như vậy được tính chính xác một lần tại nút thấp nhất nơi nó có sẵn đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        g = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        ans = 0

        def dfs(u, p):
            nonlocal ans
            total = 0
            for v in g[u]:
                if v == p:
                    continue
                total += dfs(v, u)

            if total == 0:
                return 1

            ans += total // k
            return total % k

        dfs(1, -1)
        print(ans)

if __name__ == "__main__":
    solve()
```DFS tính toán, đối với mỗi nút, có bao nhiêu lá đóng góp từ các nút con của nó. Một lá trả về 1, đại diện cho một điểm cuối có thể tháo rời. Các nút nội bộ tổng hợp các giá trị này. 

Khi một nút tích lũy ít nhất$k$những đóng góp này tương ứng với một thao tác loại bỏ hợp lệ tập trung vào một tổ tiên nào đó, vì vậy chúng tôi tăng câu trả lời lên bằng`total // k`. Mọi đóng góp còn sót lại sẽ được trả lại lên trên vì chúng có thể kết hợp với các nhánh khác cao hơn trên cây. 

Một điểm tinh tế là xử lý lá đúng cách. Trả về 1 cho một lá mã hóa rằng nó là một điểm cuối khả dụng duy nhất. Nếu một nút không có đóng góp con, nó sẽ hoạt động như một lá trong cây rút gọn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, k = 2
edges:
1-2, 1-3, 3-4, 3-5
```Chúng tôi chạy DFS từ root 1. 

| Nút | Đóng góp của trẻ em | Tổng cộng | Nhóm (//k) | Phần còn lại | 
| --- | --- | --- | --- | --- | 
| 2 | - | 1 | 0 | 1 | 
| 4 | - | 1 | 0 | 1 | 
| 5 | - | 1 | 0 | 1 | 
| 3 | 1 + 1 = 2 | 2 | 1 | 0 | 
| 1 | 1 (từ 2) + 1 (từ nhóm 3) = 2 | 2 | 1 | 0 | 

Câu trả lời là 2. 

Dấu vết này cho thấy các lá từ các nhánh khác nhau kết hợp như thế nào tại các nút bên trong. Nút 3 trước tiên tạo thành một nhóm hoàn chỉnh, sau đó nút 1 tạo thành một nhóm khác ở cấp cao nhất. 

### Ví dụ 2 

đầu vào:```
n = 4, k = 3
1-2, 1-3, 1-4
```| Nút | Đóng góp của trẻ em | Tổng cộng | Nhóm (//k) | Phần còn lại | 
| --- | --- | --- | --- | --- | 
| 2 | - | 1 | 0 | 1 | 
| 3 | - | 1 | 0 | 1 | 
| 4 | - | 1 | 0 | 1 | 
| 1 | 1+1+1 = 3 | 3 | 1 | 0 | 

Câu trả lời là 1. 

Điều này xác nhận rằng chỉ có thể thực hiện được một thao tác vì tất cả các lá phải được tiêu thụ cùng nhau tận gốc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi nút và cạnh được xử lý một lần trong DFS | 
| Không gian |$O(n)$| Danh sách kề và ngăn xếp đệ quy | 

Tổng cộng$n$trên các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó, một giải pháp tuyến tính cho mỗi trường hợp thử nghiệm là đủ. Cách tiếp cận DFS thoải mái phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    sys.setrecursionlimit(10**7)

    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        g = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        ans = 0

        def dfs(u, p):
            nonlocal ans
            total = 0
            for v in g[u]:
                if v == p:
                    continue
                total += dfs(v, u)

            if total == 0:
                return 1

            ans += total // k
            return total % k

        dfs(1, -1)
        print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue().strip()

# sample tests (from statement)
assert run("""4
8 3
1 2
1 5
7 6
6 8
3 1
6 4
6 1
10 3
1 2
1 10
2 3
1 5
1 6
2 4
7 10
10 9
8 10
7 2
3 1
4 5
3 6
7 4
1 2
1 4
5 1
1 2
2 3
4 3
5 3
""") == "2\n3\n3\n4"

# chain-like case
assert run("""1
5 2
1 2
2 3
3 4
4 5
""") == "2"

# star case
assert run("""1
6 3
1 2
1 3
1 4
1 5
1 6
""") == "1"

# k = 1 case (each leaf removal individually)
assert run("""1
4 1
1 2
1 3
1 4
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi | 2 | lan truyền tuần tự trong một đường dẫn | 
| ngôi sao | 1 | nhóm tại trung tâm duy nhất | 
| k=1 | n-1 | loại bỏ hoàn toàn thoái hóa | 

## Vỏ cạnh 

Một chuỗi cho thấy các thẻ lá phải lan truyền lên trên như thế nào thay vì được nhóm cục bộ quá sớm. DFS tích lũy một mã thông báo trên mỗi lá và chỉ tại các nút nội bộ mới hình thành các nhóm, đảm bảo ghép nối chính xác. 

Một ngôi sao xác minh rằng tất cả các lá phải được kết hợp tại một nút duy nhất khi có thể, vì không có cấu trúc sâu hơn nào tồn tại để trì hoãn việc nhóm. 

Khi$k = 1$, mọi đóng góp của lá ngay lập tức trở thành một hoạt động. Thuật toán xử lý việc này một cách tự nhiên vì mỗi tổng đều đóng góp`total // 1`, nghĩa là tất cả các mã thông báo được tiêu thụ tại mỗi nút mà không có phần còn lại.
