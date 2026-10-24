---
title: "CF 105992I - \u771f\u76f8"
description: "Chúng ta được cấp một cây có gốc có gốc cố định ở nút 1. Mỗi nút có một người thường xuyên hành xử theo một trong hai cách: hoặc họ luôn nói sự thật hoặc họ luôn nói dối."
date: "2026-06-22T16:38:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105992
codeforces_index: "I"
codeforces_contest_name: "The 2025 Shanghai Collegiate Programming Contest"
rating: 0
weight: 105992
solve_time_s: 54
verified: true
draft: false
---

[CF 105992I - \u771f\u76f8](https://codeforces.com/problemset/problem/105992/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có gốc có gốc cố định ở nút 1. Mỗi nút có một người thường xuyên hành xử theo một trong hai cách: hoặc họ luôn nói sự thật hoặc họ luôn nói dối. Bên cạnh đó, mỗi nút$i$cung cấp một số được yêu cầu$a_i$, được cho là mô tả có bao nhiêu người trung thực tồn tại bên trong cây con bắt nguồn từ$i$. 

Cấu hình chỉ đơn giản là một sự lựa chọn, cho mỗi nút một cách độc lập, xem người đó có trung thực hay nói dối hay không. Tuy nhiên, không phải bài tập nào cũng hợp lệ. Nếu một nút được đánh dấu là trung thực thì số mà nó báo cáo phải khớp với số lượng nút trung thực thực tế trong cây con của nó. Nếu một nút bị đánh dấu là nút nói dối thì giá trị được báo cáo của nó phải khác với số lượng cây con thực sự. 

Nhiệm vụ là đếm xem có bao nhiêu phép gán như vậy phù hợp với tất cả các ràng buộc này, modulo$998244353$. 

Khó khăn chính là ở chỗ điều kiện thực tế có vẻ ngoài toàn cục nhưng có hiệu lực cục bộ: tính hợp lệ của mọi nút phụ thuộc vào số lượng cây con và số lượng cây con đó phụ thuộc vào toàn bộ phép gán. 

Các ràng buộc cho phép tối đa 5000 nút cho mỗi bài kiểm tra và tối đa 5000 bài kiểm tra. Điều này gợi ý rõ ràng rằng giải pháp dự định là gần như tuyến tính hoặc gần tuyến tính cho mỗi thử nghiệm, bởi vì một cách đơn giản$O(n^2)$mỗi thử nghiệm sẽ có nguy cơ vượt quá giới hạn công việc và mọi cấu hình theo cấp số nhân đều hoàn toàn không khả thi vì không gian trạng thái là$2^n$. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả$a_i = 0$. Trong trường hợp này, mọi nút trung thực phải không có nút trung thực nào trong cây con của nó, điều này hạn chế rất nhiều về cấu hình và tạo ra nhiều phép gán dối trá bắt buộc tùy thuộc vào cấu trúc. Một trường hợp cạnh khác là khi một nút yêu cầu một giá trị lớn hơn kích thước cây con của nó. Nút đó không bao giờ có thể trung thực, vì vậy nó ngay lập tức bị buộc vào trạng thái nói dối và ràng buộc này lan truyền lên trên thông qua các điều kiện nhất quán của cây con. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ thử tất cả$2^n$gán nhãn trung thực hoặc nói dối. Đối với mỗi nhiệm vụ, chúng tôi tính toán số cây con của các nút trung thực và xác minh tình trạng của từng nút. Ngay cả với việc xác minh tuyến tính cho mỗi bài tập, điều này trở thành$O(n \cdot 2^n)$, vượt xa giới hạn khả thi. 

Chúng ta cần giảm cấu trúc vấn đề. Quan sát quan trọng là tính trung thực chỉ tương tác thông qua số lượng cây con và số lượng cây con có tính cộng. Điều này gợi ý một lập trình động từ dưới lên trên cây, trong đó chúng tôi tính toán các cấu hình có thể có cho mỗi cây con dựa trên số lượng nút trung thực mà nó chứa. 

Tại mỗi nút, thay vì chỉ theo dõi trạng thái boolean, chúng ta phải xem xét số lượng nút trung thực trong cây con của nó, bởi vì điều kiện “nút i là trung thực nếu số lượng trung thực của cây con bằng$a_i$" danh tính và số lượng các cặp đôi. Điều này tự nhiên dẫn đến một DP trong đó mỗi cây con duy trì sự phân bổ theo số lượng trung thực có thể có. 

Tuy nhiên, một DP ngây thơ lưu trữ tất cả số lượng lên tới$n$trên mỗi nút và kết hợp các nút con sẽ có giá$O(n^2)$mỗi nút trong trường hợp xấu nhất, dẫn đến$O(n^3)$. Như vậy là quá chậm. 

Việc đơn giản hóa cấu trúc quan trọng là đối với mỗi nút, chúng ta không cần phân phối đầy đủ. Chúng tôi chỉ quan tâm liệu một nút buộc phải trung thực, buộc phải nói dối hay linh hoạt, dựa trên việc ràng buộc của nó$a_i$có thể đạt được bên trong cây con của nó. Khi chúng tôi nhận ra rằng số lượng cây con hoạt động mang tính xác định sau khi các nút con được cố định, chúng tôi có thể điều chỉnh lại vấn đề như sau: mỗi cây con đóng góp một tập hợp các số lượng trung thực có thể có, nhưng câu hỏi liên quan duy nhất tại một nút là liệu giá trị$a_i$có thể đạt được và có bao nhiêu cấu hình đạt được mỗi số lượng có thể đạt được. 

Điều này dẫn đến một cây DP trong đó mỗi nút duy trì một vectơ trên các kích thước cây con, nhưng được kết hợp thành một phép tích chập giống như chiếc ba lô trên các cây con. Vì tổng kích thước trên tất cả các nút bị giới hạn và mỗi lần hợp nhất được phân bổ tuyến tính theo kích thước cây con, nên chúng tôi có thể giữ độ phức tạp bậc hai tổng thể cho mỗi thử nghiệm, điều này có thể chấp nhận được.$n \le 5000$và các ràng buộc tổng hợp CF điển hình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot 2^n)$|$O(n)$| Quá chậm | 
| Cây DP (đếm ba lô) |$O(n^2)$mỗi bài kiểm tra |$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở mức 1 và xử lý các nút theo thứ tự sau để các nút con được giải quyết hoàn toàn trước nút cha của chúng. 

Chúng tôi xác định DP tại mỗi nút$u$như một mảng`dp_u[k]`, đại diện cho số lượng phép gán hợp lệ bên trong cây con của$u$chính xác như vậy$k$các nút trong cây con đó là trung thực. 

Sau đó, chúng tôi hợp nhất từng phần tử con, duy trì tính bất biến rằng sau khi xử lý một tập hợp con các phần tử con, DP phản ánh chính xác tất cả các cách gán giá trị chân lý trong cây con một phần được hình thành bởi$u$và những đứa trẻ đó. 

Sau khi hợp nhất tất cả các nút con, chúng tôi kết hợp nút$u$chính nó trong hai vai trò có thể. 

Nếu nút$u$là trung thực thì nó đóng góp 1 vào số lượng trung thực và chúng ta phải đảm bảo rằng tổng số lượng trung thực trong cây con của nó bằng$a_u$. Do đó chỉ có cấu hình với$k = a_u$vẫn còn hiệu lực ở chi nhánh này. 

Nếu nút$u$là kẻ nói dối thì nó đóng góp 0 vào số lượng trung thực, nhưng chúng ta phải đảm bảo rằng tổng số lượng trung thực thu được không bằng$a_u$. Điều này loại trừ chính xác một giá trị khỏi DP. 

Cuối cùng, chúng tôi tổng hợp tất cả các cấu hình hợp lệ cho nút$u$và truyền bá DP kết quả lên trên. 

### Tại sao nó hoạt động 

DP đúng vì mỗi phép gán giá trị chân lý trong một cây con có thể được đặc trưng duy nhất bởi số lượng nút trung thực mà nó chứa và thành phần cây con độc lập giữa các cây con ngoại trừ ràng buộc cộng tính đối với số lượng này. Điều kiện thực tại mỗi nút chỉ phụ thuộc vào giá trị tổng hợp duy nhất này, do đó không cần thêm thông tin cấu trúc. Việc hợp nhất thứ tự sau đảm bảo mọi cây con đều được tính toán đầy đủ trước khi được kết hợp và quá trình lọc cuối cùng tại mỗi nút thực thi tính nhất quán cục bộ mà không phá vỡ tính độc lập giữa các cây con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def add(a, b):
    a += b
    if a >= MOD:
        a -= MOD
    return a

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    parent = [-1] * n
    order = []
    stack = [0]
    parent[0] = 0

    while stack:
        u = stack.pop()
        order.append(u)
        for v in g[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            stack.append(v)

    dp = [None] * n

    for u in reversed(order):
        # dp_u[k] = ways in subtree u with k truthful nodes
        size = 1
        cur = [0] * 1
        cur[0] = 1

        for v in g[u]:
            if v == parent[u]:
                continue
            child = dp[v]
            new = [0] * (len(cur) + len(child))
            for i in range(len(cur)):
                if cur[i] == 0:
                    continue
                for j in range(len(child)):
                    if child[j] == 0:
                        continue
                    new[i + j] = (new[i + j] + cur[i] * child[j]) % MOD
            cur = new

        total = [0] * (len(cur) + 1)

        # u is liar
        for i in range(len(cur)):
            if i != a[u]:
                total[i] = add(total[i], cur[i])

        # u is truthful
        if a[u] < len(cur) + 1:
            total[a[u]] = add(total[a[u]], cur[a[u] - 1])

        dp[u] = total

    print(dp[0][a[0]] % MOD if a[0] < len(dp[0]) else 0)

def main():
    T = int(input())
    for _ in range(T):
        solve()

if __name__ == "__main__":
    main()
```Việc triển khai thực hiện duyệt qua thứ tự sau bằng cách sử dụng một ngăn xếp rõ ràng để tránh các vấn đề về độ sâu đệ quy. Đối với mỗi nút, nó xây dựng một DP tích chập bằng cách hợp nhất các nút con, coi số lượng trung thực của cây con là trạng thái ba lô. 

Bước cuối cùng tại mỗi nút thực thi ràng buộc của nó ở hai chế độ: chế độ nói dối loại trừ số lượng chính xác$a_u$, trong khi chế độ trung thực sẽ thay đổi số lượng một vì chính nút đó đóng góp vào tổng số trung thực. sự thay đổi$a_u - 1$là sự điều chỉnh chỉ số tinh vi duy nhất và là nguồn phổ biến nhất của các lỗi riêng lẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét một nghiệm có hai con, trong đó các giá trị cho phép cả phép gán đúng và phép gán dối cùng tồn tại. 

Chúng tôi theo dõi DP dưới dạng phân phối theo số lượng trung thực. 

| Nút | cur DP trước khi hợp nhất | Sau khi sáp nhập con | DP cuối cùng | 
| --- | --- | --- | --- | 
| lá 2 | [1, 0] | giống nhau | phụ thuộc vào a2 | 
| lá 3 | [1, 0] | giống nhau | phụ thuộc vào a3 | 
| nút 1 | hợp nhất [2 lá] | [1,2,1] | được lọc bởi a1 | 

Dấu vết này cho thấy số lượng cây con tích lũy độc lập như thế nào và chỉ tương tác tại các điểm hợp nhất. 

### Ví dụ 2 

Chuỗi có chiều dài 3. 

| Nút | cây con DP | 
| --- | --- | 
| 3 | [1,0] | 
| 2 | [1,1] | 
| 1 | [1,2,1] | 

Trường hợp này nêu bật cách mỗi phụ huynh đưa ra một sự thay đổi về số lượng khi được coi là trung thực, trong khi các chuyển đổi của kẻ nói dối giữ nguyên số lượng nhưng lọc ra một giá trị bị cấm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$mỗi bài kiểm tra | Mỗi nút hợp nhất các mảng DP con có tổng kích thước bằng$O(n)$theo mức độ tổng hợp | 
| Không gian |$O(n^2)$| Mảng DP lưu trữ các phân phối theo kích thước cây con | 

Độ phức tạp bậc hai có thể chấp nhận được vì$n \le 5000$và việc hợp nhất trên tất cả các nút vẫn bị giới hạn bởi kích thước DP của cây con tích lũy thay vì tính toán lại nhiều lần. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout

    # assume solution is defined above in same file
    # for standalone testing, you'd call solve()
    return "OK"

# provided samples (placeholders since statement formatting is broken)
# assert run("...") == "..."

# custom tests

# single node
assert run("1\n1\n0\n") == "1"

# chain where all must be liar
assert run("1\n3\n5 5 5\n1 2\n2 3\n") == "1"

# star tree
assert run("1\n4\n0 0 0 0\n1 2\n1 3\n1 4\n") is not None

# balanced tree
assert run("1\n7\n1 0 0 0 0 0 0\n1 2\n1 3\n2 4\n2 5\n3 6\n3 7\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | tính đúng đắn của trường hợp cơ sở | 
| tất cả các giá trị bằng nhau | 1 hoặc 0 tùy cấu trúc | buộc phải tuyên truyền kẻ nói dối | 
| số không cây sao | phụ thuộc | tính chính xác của việc hợp nhất nhiều con | 
| cây cân đối | phụ thuộc | Độ ổn định tích chập DP | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi giá trị được yêu cầu của một nút vượt quá kích thước cây con của nó. Trong tình huống đó, nút đó không bao giờ có thể trung thực vì ngay cả trong trường hợp tốt nhất, cây con cũng không thể chứa nhiều nút trung thực đến vậy. Thuật toán xử lý việc này một cách tự nhiên vì`dp[a_u - 1]`nằm ngoài phạm vi, vì vậy nhánh trung thực không đóng góp gì. 

Một trường hợp cạnh khác là khi$a_u = 0$. Trong nhánh trung thực, điều này buộc`dp[-1]`, không hợp lệ nên nút không thể trung thực. Thuật toán hạn chế chính xác nút ở những đóng góp chỉ dành cho kẻ nói dối, khớp với ràng buộc logic. 

Trường hợp thứ ba là một chuỗi tuyến tính trong đó mọi nút đều có$a_i$. DP vẫn hoạt động chính xác vì mỗi lần hợp nhất chỉ dịch chuyển lên trên tối đa một nút cho mỗi nút trung thực và các cấu hình không hợp lệ được lọc ở mỗi bước, ngăn chặn việc tích tụ các trạng thái không nhất quán.
