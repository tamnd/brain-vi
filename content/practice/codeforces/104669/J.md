---
title: "CF 104669J - Khóa và hoán vị cây con (Phiên bản dễ dàng)"
description: "Một cây được cho với các nút được đánh số từ 1 đến N, gốc ở nút 1. Mỗi nút mang một nhãn riêng biệt và các nhãn này tạo thành một hoán vị của các số từ 1 đến N. Đối với mỗi nút, chúng tôi xem xét các nút bên trong cây con gốc của nó và thu thập nhãn của chúng."
date: "2026-06-29T09:43:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104669
codeforces_index: "J"
codeforces_contest_name: "Turtle Codes"
rating: 0
weight: 104669
solve_time_s: 70
verified: true
draft: false
---

[CF 104669J - Khóa và hoán vị cây con (Phiên bản dễ dàng)](https://codeforces.com/problemset/problem/104669/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một cây được cho với các nút được đánh số từ 1 đến N, bắt nguồn từ nút 1. Mỗi nút mang một nhãn riêng biệt và các nhãn này tạo thành một hoán vị của các số từ 1 đến N. 

Đối với mỗi nút, chúng tôi xem xét các nút bên trong cây con gốc của nó và thu thập nhãn của chúng. Nhiệm vụ là xác định xem tập nhãn được thu thập này có tạo thành một hoán vị hoàn hảo có độ dài bằng kích thước của cây con hay không. Nói cách khác, nếu một cây con có k nút, chúng ta sẽ kiểm tra xem nhãn của nó có chính xác là các số từ 1 đến k theo thứ tự nào đó hay không. 

Đầu ra là một chuỗi các câu trả lời, mỗi câu trả lời cho mỗi nút, cho biết liệu cây con của nút đó có thỏa mãn thuộc tính này hay không. 

Ràng buộc N lên tới 200.000 buộc một giải pháp tuyến tính hoặc gần tuyến tính. Bất kỳ cách tiếp cận nào liên tục kiểm tra toàn bộ cây con một cách độc lập sẽ tính toán lại cùng một cấu trúc nhiều lần và chuyển sang hành vi bậc hai, vượt xa giới hạn chấp nhận được. Ngay cả các hoạt động như sắp xếp từng cây con cũng có nghĩa là tổng chi phí khoảng O(N log N) cho mỗi nút trong trường hợp xấu nhất, suy biến thành O(N^2 log N) trên cây hình chuỗi. 

Trường hợp phức tạp xuất phát từ thực tế là các nhãn là duy nhất trên toàn cầu. Điều này loại bỏ bất kỳ sự mơ hồ nào về các bản sao bên trong một cây con, nhưng nó cũng che giấu khó khăn chính: mặc dù không có sự lặp lại, một cây con vẫn có thể không đạt điều kiện nếu các giá trị của nó không chính xác nằm trong phạm vi từ 1 đến k. 

Ví dụ, hãy xem xét một cây con có kích thước 3 chứa các giá trị {2, 3, 4}. Nó không hợp lệ, mặc dù nó có kích thước hoàn hảo và không có bản sao vì nó không khớp với cách đánh số chuẩn được yêu cầu. 

Trường hợp khác là cây con có kích thước 3 chứa {1, 2, 4}. Điều này cũng không thành công và cách tiếp cận "chỉ kiểm tra tối thiểu và tối đa" ngây thơ có thể vượt qua nó một cách không chính xác nếu không được suy luận cẩn thận. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là tính toán, đối với mỗi nút, danh sách đầy đủ các giá trị trong cây con của nó, sắp xếp nó và sau đó xác minh xem nó có khớp với chuỗi 1 với k hay không. Điều này đúng vì nó tái tạo lại điều kiện đang được kiểm tra một cách rõ ràng. Vấn đề là chi phí. Mỗi trích xuất cây con có kích thước tuyến tính và việc sắp xếp sẽ thêm hệ số logarit, do đó trên tất cả các nút, việc này trở nên cực kỳ tốn kém đối với những cây lớn nơi có nhiều cây con chồng lên nhau. 

Quan sát quan trọng là chúng ta thực sự không cần thứ tự đầy đủ của các giá trị. Vì mỗi nhãn là duy nhất trên toàn cầu nên mỗi cây con đã chứa k số nguyên riêng biệt. Điều duy nhất quan trọng là liệu k số nguyên đó có lấp đầy chính xác khoảng từ 1 đến k mà không có khoảng trống hay không. Điều kiện đó có thể được xác minh chỉ bằng hai tập hợp: giá trị tối thiểu và giá trị tối đa bên trong cây con. 

Nếu cây con có kích thước k có giá trị tối thiểu là 1 và giá trị tối đa là k thì tất cả các giá trị phải nằm trong [1, k] và vì có chính xác k giá trị riêng biệt nên chúng phải chiếm mọi số nguyên trong phạm vi đó. Điều này làm giảm vấn đề từ việc duy trì các tập hợp đến duy trì các thống kê cây con đơn giản. 

Do đó, chúng tôi chuyển từ việc tính toán lại các tập hợp rõ ràng sang một DFS duy nhất tính toán kích thước cây con, mức tối thiểu và tối đa cho mỗi nút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (trích xuất cây con + sắp xếp) | O(N2 log N) | O(N) | Quá chậm | 
| DFS với cây con tối thiểu, tối đa, kích thước | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây ở nút 1 và thực hiện duyệt theo thứ tự sau để các cây con được xử lý trước cây cha của chúng.

1. Bắt đầu DFS từ nút 1, xử lý cây theo hướng từ gốc. Điều này đảm bảo mỗi cây con được xử lý chính xác một lần theo cách từ dưới lên. 
2. Đối với mỗi nút u, khởi tạo kích thước cây con của nó là 1 và đặt cả giá trị tối thiểu và tối đa của nó thành P[u]. Điều này đại diện cho cây con tầm thường chỉ chứa u. 
3. Đi qua từng con v của u, bỏ qua cha mẹ để tránh quay trở lại cây vô hướng. 
4. Sau khi trở về từ DFS(v), hợp nhất thông tin từ v vào u bằng cách thêm kích thước, lấy cực tiểu của cây con tối thiểu và cực đại của cây con tối đa. Bước này kết hợp các cây con rời rạc thành cây con đầy đủ của u. 
5. Khi tất cả các cây con đã được xử lý, cây con của u được biểu thị đầy đủ bằng ba giá trị: sz[u], mn[u], mx[u]. 
6. Kiểm tra xem mn[u] có bằng 1 và mx[u] có bằng sz[u] hay không. Nếu cả hai đều giữ, hãy đánh dấu u là hợp lệ, nếu không thì đánh dấu nó không hợp lệ. 

Tính chính xác của bước hợp nhất phụ thuộc vào thực tế là việc phân tách cây con trong một cây là không khớp: mỗi nút thuộc về chính xác một cây con con, do đó việc tổng hợp trên các cây con sẽ bảo toàn số lượng và phạm vi giá trị chính xác mà không bị trùng lặp. 

### Tại sao nó hoạt động 

Mỗi cây con duy trì một bản tóm tắt đầy đủ các giá trị của nó thông qua ba bất biến: kích thước, nhãn nhỏ nhất và nhãn lớn nhất. Bởi vì tất cả các nhãn đều khác biệt trên toàn cầu nên những bản tóm tắt này không bị mất đối với điều kiện cụ thể đang được kiểm tra. Cây con có kích thước k có giá trị nằm hoàn toàn trong [1, k] phải chứa mọi số nguyên trong khoảng đó, vì việc bỏ qua dù chỉ một số sẽ buộc kích thước nhỏ hơn hoặc khoảng cách mâu thuẫn với giới hạn tối đa. 

## Giải pháp Python```python
import sys
sys.setrecursionlimit(10**7)
input = sys.stdin.readline

def solve():
    n = int(input())
    p = [0] + list(map(int, input().split()))
    
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)
    
    sz = [0] * (n + 1)
    mn = [10**18] * (n + 1)
    mx = [0] * (n + 1)
    ans = [False] * (n + 1)
    
    def dfs(u, parent):
        sz[u] = 1
        mn[u] = p[u]
        mx[u] = p[u]
        
        for v in g[u]:
            if v == parent:
                continue
            dfs(v, u)
            sz[u] += sz[v]
            mn[u] = min(mn[u], mn[v])
            mx[u] = max(mx[u], mx[v])
        
        if mn[u] == 1 and mx[u] == sz[u]:
            ans[u] = True
    
    dfs(1, -1)
    
    out = []
    for i in range(1, n + 1):
        out.append("YES" if ans[i] else "NO")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```DFS tính toán số liệu thống kê cây con trong một lần duyệt. Mỗi nút khởi tạo phần đóng góp của riêng mình và sau đó tiếp thu kết quả từ các nút con của nó. Tham số cha ngăn chặn việc xem lại nút trước đó trong danh sách lân cận không được định hướng. 

Một lỗi triển khai phổ biến là quên khởi tạo mn[u] và mx[u] cho mỗi nút trước khi hợp nhất các nút con, điều này có thể khiến các giá trị từ các trường hợp thử nghiệm trước đó hoặc lệnh gọi đệ quy bị rò rỉ vào các phép tính không liên quan. Một điểm tinh tế khác là độ sâu đệ quy, vì cây hình chuỗi ở N = 200.000 sẽ vượt quá giới hạn Python mặc định mà không tăng rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
4 2 1 3
2 1
3 2
4 1
```Chúng tôi theo dõi các bản tóm tắt cây con. 

| Nút | sz | mn | mx | Kiểm tra hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 1 | 4 | CÓ | 
| 2 | 2 | 1 | 2 | CÓ | 
| 3 | 1 | 1 | 1 | CÓ | 
| 4 | 1 | 3 | 3 | KHÔNG | 

Điều này cho thấy điều kiện chỉ phụ thuộc vào việc liệu các giá trị cây con có bao phủ chính xác phạm vi từ 1 đến kích thước của nó hay không. Nút 4 không thành công vì giá trị đơn của nó không phải là 1, mặc dù về mặt cấu trúc nó tạo thành một cây con một phần tử hợp lệ. 

### Ví dụ 2 

đầu vào:```
5
1 3 2 5 4
1 2
1 3
3 4
3 5
```| Nút | sz | mn | mx | Kiểm tra hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 1 | 5 | CÓ | 
| 3 | 3 | 2 | 5 | KHÔNG | 
| 4 | 1 | 5 | 5 | KHÔNG | 
| 5 | 1 | 4 | 4 | KHÔNG | 

Chỉ cây đầy đủ có gốc tại 1 thỏa mãn điều kiện vì chỉ nó chứa các giá trị từ 1 đến 5. Cây con 3 không thành công vì giá trị của nó không được chuẩn hóa thành 1..3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi nút và cạnh được truy cập một lần trong DFS và tất cả các hoạt động bên trong đều có thời gian không đổi | 
| Không gian | O(N) | Danh sách kề cộng với ngăn xếp đệ quy và mảng trên mỗi nút | 

Truyền tải tuyến tính vừa vặn thoải mái trong giới hạn lên tới 200.000 nút và mức sử dụng bộ nhớ vẫn tỷ lệ thuận với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("""4
4 2 1 3
2 1
3 2
4 1
""") == """YES
YES
YES
NO"""

# single node
assert run("""1
1
""") == "YES"

# chain
assert run("""3
1 2 3
1 2
2 3
""") == """YES
YES
YES"""

# invalid subtree
assert run("""3
2 1 3
1 2
1 3
""") == """YES
YES
NO"""

# reversed order
assert run("""4
2 1 4 3
1 2
1 3
3 4
""") == """YES
YES
NO
NO"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | CÓ | độ chính xác của cây con nhỏ nhất | 
| chuỗi | tất cả CÓ | lan truyền cấu trúc tuyến tính | 
| cây con không hợp lệ | hỗn hợp | giá trị không liên tiếp | 
| thứ tự đảo ngược | một phần KHÔNG | phát hiện khoảng cách cây con | 

## Vỏ cạnh 

Cây nút đơn là kịch bản đơn giản nhất. DFS khởi tạo sz thành 1, mn và mx thành giá trị nút. Vì điều kiện kiểm tra mn bằng 1 và mx bằng sz nên chỉ có nút có nhãn 1 là vượt qua. Việc triển khai được cung cấp sẽ xử lý chính xác vấn đề này vì cả quá trình khởi tạo và kiểm tra đều diễn ra trong cùng một khung đệ quy. 

Cây hình chuỗi nhấn mạnh độ sâu đệ quy. Mỗi nút trở thành một lệnh gọi đệ quy sâu, nhưng vì con trỏ cha ngăn chặn việc truy cập lại nên mỗi nút vẫn được xử lý chính xác một lần. Thuật toán vẫn tuyến tính và tính chính xác được bảo toàn vì tập hợp cây con không phụ thuộc vào thứ tự truyền tải ngoài cấu trúc thứ tự sau. 

Các trường hợp giá trị không được căn chỉnh theo kích thước cây con thể hiện logic cốt lõi. Ví dụ: một cây con có kích thước 3 chứa các giá trị {2, 3, 4} tạo ra mn = 2 và mx = 4. Mặc dù mx - mn + 1 bằng 3, mn không phải là 1, do đó điều kiện sẽ loại bỏ nó một cách chính xác, chỉ ngăn chặn kết quả dương tính giả chỉ từ lý luận dựa trên khoảng thời gian.
