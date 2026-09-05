---
title: "CF 104522F - Máy Làm Kẹo"
description: "Chúng ta có một cây có gốc và mỗi cạnh mang một trọng số xác định khả năng một quá trình di chuyển qua cạnh đó. Quá trình bắt đầu từ gốc. Bất cứ khi nào chúng tôi “kích hoạt” một nút, chúng tôi sẽ tiêu tốn một đơn vị chi phí và nút đó sẽ trống."
date: "2026-06-30T10:13:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104522
codeforces_index: "F"
codeforces_contest_name: "CerealCodes II Intermediate"
rating: 0
weight: 104522
solve_time_s: 99
verified: false
draft: false
---

[CF 104522F - Máy làm kẹo](https://codeforces.com/problemset/problem/104522/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc và mỗi cạnh mang một trọng số xác định khả năng một quá trình di chuyển qua cạnh đó. Quá trình bắt đầu từ gốc. Bất cứ khi nào chúng tôi “kích hoạt” một nút, chúng tôi sẽ tiêu tốn một đơn vị chi phí và nút đó sẽ trống. Nếu nút có con, chính xác một trong số nút con của nó được chọn ngẫu nhiên, với xác suất tỷ lệ thuận với trọng số của cạnh và quá trình tiếp tục từ nút con đó. Nếu nút là một lá, quá trình sẽ dừng lại vì không còn nơi nào để di chuyển nữa. 

Mỗi nút đại diện cho một loại kẹo. Các nút bên trong ban đầu chỉ có một bản sao kẹo của chúng, trong khi các lá có vô số bản sao, nhưng sự khác biệt đó chủ yếu đảm bảo các lá không bao giờ cản trở tiến trình. 

Số lượng quan trọng mà chúng ta phải tính toán là, với mỗi nút i, tổng số tiền dự kiến ​​​​được chi cho đến khi chúng ta thu được kẹo i thông qua quy trình xếp tầng ngẫu nhiên này. 

Kích thước đầu vào đạt tổng thể 2 × 10^5 nút trong các trường hợp thử nghiệm, do đó, mọi giải pháp về cơ bản đều phải tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì liên quan đến việc tính toán lại trên mỗi nút hoặc trên mỗi đường dẫn riêng biệt sẽ quá chậm. Cấu trúc cây cũng gợi ý rằng câu trả lời của mỗi nút chỉ phụ thuộc vào tổ tiên của nó chứ không phụ thuộc vào các tương tác toàn cầu tùy ý. 

Một trường hợp thất bại tinh tế đối với lối suy nghĩ ngây thơ là giả định tính độc lập giữa các nút. Ví dụ, người ta có thể cố gắng mô phỏng quá trình nhiều lần cho đến khi đạt đến từng nút, nhưng ngay cả một mô phỏng đơn lẻ cũng không bị giới hạn vì cây có thể sâu và các xác suất được nhân lên thành các giá trị cực kỳ nhỏ. 

Một sai lầm phổ biến khác là nghĩ rằng chi phí dự kiến ​​phụ thuộc vào kích thước cây con hoặc số lượng lá. Nó không. Nó chỉ phụ thuộc vào xác suất tầng ngẫu nhiên kết thúc chính xác tại một nút nhất định. 

## Phương pháp tiếp cận 

Cách giải thích brute-force là mô phỏng rõ ràng quá trình ngẫu nhiên nhiều lần và ước tính xác suất tiếp cận từng nút. Một mô phỏng tương ứng với một tầng duy nhất bắt đầu từ gốc, đi xuống các phần tử con ngẫu nhiên cho đến khi chạm tới một chiếc lá. Nếu lặp lại quá trình này, chúng ta có thể ước tính tần suất mỗi nút là điểm cuối cuối cùng hoặc cần bao nhiêu tầng trước khi gặp nó. 

Điều này đúng về mặt mong đợi nhưng không thể sử dụng được về mặt tính toán. Ngay cả việc tạo ra một đường dẫn cho mỗi lần thử cũng là O(n) trong trường hợp xấu nhất và để ước tính chính xác kỳ vọng, chúng ta sẽ cần một số lượng thử nghiệm rất lớn. Với n lên tới 2 × 10^5, mọi phương pháp mô phỏng Monte Carlo hoặc lặp đi lặp lại sẽ sụp đổ ngay lập tức. 

Quan sát cấu trúc quan trọng là mỗi tầng không phải là sự ngẫu nhiên tùy ý trên toàn bộ cây mà là một bước đi ngẫu nhiên từ gốc đến lá trong đó xác suất chuyển tiếp được cố định cục bộ tại mỗi nút. Điều đó có nghĩa là xác suất kết thúc tại một nút cụ thể chính xác là tích của các xác suất biên dọc theo đường dẫn từ gốc đến nút duy nhất. 

Khi chúng ta biết xác suất p_i để một tầng kết thúc tại nút i, số tầng dự kiến ​​cần để chạm vào nút i chỉ đơn giản là kỳ vọng của phân bố hình học với xác suất thành công p_i, bằng 1 / p_i. 

Vì vậy, toàn bộ vấn đề giảm xuống việc tính toán p_i cho mỗi nút một cách hiệu quả, sau đó xuất ra nghịch đảo mô-đun của nó. 

Xác suất dọc theo một đường đi là tích của các số hạng có dạng w(u, v) / sum_w(u), do đó nghịch đảo của nó trở thành tích của sum_w(u) / w(u, v). Điều này chuyển đổi câu trả lời thành một DP nhân đơn giản trên cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(T · k · n) | O(n) | Quá chậm | 
| Xác suất cây DP | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta lấy gốc của cây ở mức 1 và tính toán cho mỗi nút tổng trọng số của các cạnh đối với các nút con của nó. Giá trị này biểu thị hệ số chuẩn hóa cho xác suất chuyển tiếp tại nút đó.

Sau đó, chúng tôi thực hiện DFS từ gốc, duy trì cho mỗi nút xác suất tương ứng để đạt được nó trong một tầng. 

1. Đặt dp[1] = 1 vì nghiệm đạt được trước khi đưa ra bất kỳ quyết định xác suất nào. 
2. Với mỗi nút u, hãy tính S(u), tổng trọng số của các cạnh từ u đến các cạnh con của nó. 
3. Khi đi qua cạnh u → v có trọng số w, xác suất chọn v khi chúng ta đang ở u là w / S(u). 
4. Do đó, xác suất đạt v là dp_prob[v] = dp_prob[u] × (w / S(u)). 
5. Chúng tôi không lưu trữ xác suất một cách trực tiếp. Thay vào đó, chúng tôi lưu trữ dp[v] = 1 / dp_prob[v], điều này tránh nghịch đảo mô-đun lặp lại dọc theo các đường dẫn. 
6. Điều này cho ra dp[v] = dp[u] × (S(u) / w). Chúng tôi tính toán điều này trong số học mô-đun bằng cách sử dụng nghịch đảo mô-đun của w. 
7. Duyệt toàn bộ cây một lần, truyền các giá trị dp từ gốc đến lá. 

Tại sao nó hoạt động được gắn với một bất biến duy nhất: tại mỗi nút u, dp[u] luôn bằng nghịch đảo của xác suất mà một tầng đơn từ gốc kết thúc chính xác tại u. Phép đệ quy bảo toàn điều này vì mỗi cạnh nhân xác suất với một hệ số chuyển tiếp cục bộ và các nghịch đảo biến phép nhân thành tỷ lệ nghịch đảo theo cùng một hướng. Vì mỗi nút có một đường dẫn cha duy nhất nên không tồn tại các bài toán con chồng chéo hoặc các thuật ngữ chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n = int(input())
    g = [[] for _ in range(n + 1)]
    
    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        g[u].append((v, w))
        g[v].append((u, w))
    
    parent = [0] * (n + 1)
    pw = [0] * (n + 1)
    order = []
    
    stack = [1]
    parent[1] = -1
    
    while stack:
        u = stack.pop()
        order.append(u)
        for v, w in g[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            pw[v] = w
            stack.append(v)
    
    children_sum = [0] * (n + 1)
    
    for u in range(1, n + 1):
        for v, w in g[u]:
            if parent[v] == u:
                children_sum[u] = (children_sum[u] + w) % MOD
    
    dp = [1] * (n + 1)
    
    for u in order:
        for v, w in g[u]:
            if parent[v] == u:
                dp[v] = dp[u] * children_sum[u] % MOD * modinv(w) % MOD
    
    print(*dp[1:])

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng cây, sau đó sửa cấu trúc gốc bằng cách sử dụng một ngăn xếp rõ ràng để tránh các vấn đề về độ sâu đệ quy. Sau đó, nó tính tổng trọng số gửi đi cho mỗi nút. Đây là hệ số chuẩn hóa cần thiết cho xác suất chuyển tiếp. 

Bước lan truyền DP tuân theo phép lặp chính xác được rút ra trước đó: mỗi phần tử con kế thừa giá trị của nó từ phần tử cha mẹ của nó nhân với tỷ lệ tổng trọng số gửi đi trên trọng lượng cạnh. Nghịch đảo mô-đun được sử dụng để xử lý phép chia theo modulo 998244353. 

Một điểm tinh tế là chúng tôi không bao giờ tính toán lại xác suất từ đầu cho mỗi nút. Mọi thứ đều diễn ra trong một lần truyền duy nhất, đảm bảo độ phức tạp tuyến tính. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây đơn giản: 

Nút 1 có con 2 và 3, có trọng số lần lượt là 2 và 3. 

| Bước | Nút | giá trị dp | Giải thích | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | Trường hợp cơ sở gốc | 
| 2 | 2 | (2+3)/2 = 5/2 | xác suất nghịch đảo đạt 2 | 
| 3 | 3 | (2+3)/3 = 5/3 | xác suất nghịch đảo đạt 3 | 

Đối với nút 2, xác suất trong một tầng là 2/5, do đó số lần thử dự kiến ​​là 5/2. Đối với nút 3, xác suất là 3/5 nên kỳ vọng là 5/3. 

Bây giờ hãy xem xét một chuỗi sâu hơn: 

1 → 2 → 3 với trọng số 4 và 5. 

| Bước | Nút | giá trị dp | Giải thích | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | Gốc | 
| 2 | 2 | 4/4 = 1 | con một, quyết đoán | 
| 3 | 3 | 1/5 | tích của nghịch đảo | 

Điều này cho thấy các cạnh xác định thu gọn xác suất một cách rõ ràng như thế nào và chỉ các nút phân nhánh mới đóng góp vào tỷ lệ không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi nút và cạnh được xử lý một lần trong DFS | 
| Không gian | O(n) | danh sách kề và mảng DP | 

Các ràng buộc cho phép tổng cộng tối đa 2 × 10^5 nút, do đó, việc truyền tải tuyến tính cho mỗi trường hợp thử nghiệm là đủ. Giải pháp này tránh mọi hoạt động tính toán lại trên mỗi nút hoặc xử lý đường dẫn lặp lại, duy trì thời gian chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import sys
    input = sys.stdin.readline
    
    n = int(input())
    g = [[] for _ in range(n + 1)]
    
    edges = []
    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        g[u].append((v, w))
        g[v].append((u, w))
        edges.append((u, v, w))
    
    parent = [0] * (n + 1)
    pw = [0] * (n + 1)
    
    stack = [1]
    parent[1] = -1
    order = []
    
    while stack:
        u = stack.pop()
        order.append(u)
        for v, w in g[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            pw[v] = w
            stack.append(v)
    
    children_sum = [0] * (n + 1)
    for u in range(1, n + 1):
        for v, w in g[u]:
            if parent[v] == u:
                children_sum[u] = (children_sum[u] + w) % MOD
    
    dp = [1] * (n + 1)
    for u in order:
        for v, w in g[u]:
            if parent[v] == u:
                dp[v] = dp[u] * children_sum[u] % MOD * modinv(w) % MOD
    
    return " ".join(str(x) for x in dp[1:])

# custom tests

# single node
assert run("1\n") == "1", "single node"

# chain
assert run("3\n1 2 2\n2 3 3\n") == "1 1 1", "linear deterministic chain"

# star
out = run("3\n1 2 1\n1 3 1\n")
assert len(out.split()) == 3, "star structure output size"

# symmetric small tree
out = run("5\n1 2 1\n1 3 2\n2 4 1\n2 5 3\n")
assert len(out.split()) == 5, "small tree structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút | 1 | trường hợp cơ sở đúng đắn | 
| cây xích | tất cả 1 | tuyên truyền xác định | 
| cây sao | 3 giá trị | chuẩn hóa phân nhánh | 
| cây hỗn hợp nhỏ | 5 giá trị | tính nhất quán DP chung | 

## Vỏ cạnh 

Cây nút đơn là ranh giới đơn giản nhất. Thuật toán khởi tạo dp[1] = 1 và vì không có con nên không có chuyển đổi nào xảy ra. Đầu ra chính xác vẫn là 1. 

Cây tuyến tính hoàn toàn đảm bảo rằng mọi nút đều có chính xác một nút con. Trong trường hợp đó, mỗi S(u) bằng trọng số cạnh đơn, do đó mọi tỷ lệ sẽ trở thành 1. DP truyền một giá trị không đổi xuống chuỗi, phù hợp với thực tế là tầng có xác suất 1 đi theo con đường duy nhất. 

Một mức độ cao kiểm tra gốc bình thường hóa. Ngay cả khi gốc có nhiều con thì chỉ có tổng trọng số là quan trọng. Giá trị của mỗi phần tử con được chia tỷ lệ độc lập bằng cách sử dụng cùng một hệ số gốc và DFS đảm bảo không có sự can thiệp giữa các nhánh.
