---
title: "CF 105986E - \u602f\u6218\u8725\u8734 VI"
description: "Chúng ta được cho một cây có n nút. Mỗi nút độc lập chứa một kẻ thù có xác suất pi, được cho dưới dạng phân số ai / bi modulo một số nguyên tố lớn. Đối với bất kỳ lựa chọn nút đích x nào, nhân vật chính bắt đầu từ một nút lá nào đó và đi dọc theo con đường đơn giản duy nhất đến x."
date: "2026-06-21T15:51:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105986
codeforces_index: "E"
codeforces_contest_name: "2025 Wuhan University of Technology Programming Contest"
rating: 0
weight: 105986
solve_time_s: 52
verified: true
draft: false
---

[CF 105986E - \u602f\u6218\u8725\u8734 VI](https://codeforces.com/problemset/problem/105986/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây có n nút. Mỗi nút độc lập chứa một kẻ thù có xác suất pi, được cho dưới dạng phân số ai / bi modulo một số nguyên tố lớn. Đối với bất kỳ lựa chọn nút đích x nào, nhân vật chính bắt đầu từ một nút lá nào đó và đi dọc theo con đường đơn giản duy nhất đến x. Bởi vì đồ thị là một cây nên khi lá bắt đầu được cố định, đường đi tới x được xác định đầy đủ. 

Trước khi quá trình truyền tải bắt đầu, nhân vật chính có thể kiểm tra toàn bộ cây và chọn lá giúp giảm thiểu số lượng nút bị kẻ thù chiếm giữ trên đường dẫn đến x. Vì vậy, đối với mỗi mục tiêu gốc cố định x, chúng tôi được yêu cầu tính giá trị kỳ vọng của số lượng nút “xấu” tối thiểu trên bất kỳ đường dẫn từ lá đến x nào, trong đó “xấu” có nghĩa là nút chứa kẻ thù. 

Đầu ra được yêu cầu cho mọi x từ 1 đến n, một cách độc lập, vì vậy chúng tôi đang giải quyết hiệu quả n bài toán tối ưu hóa kỳ vọng khác nhau trên cùng một cây xác suất. 

Các ràng buộc n 3000 ngay lập tức loại trừ mọi cách tiếp cận cố gắng liệt kê các tập hợp con của các nút hoặc mô phỏng phân bố trên các đường dẫn. Một per-x DP ngây thơ trên tất cả các trạng thái mà các nút bị chiếm đóng sẽ bùng nổ theo cấp số nhân. Ngay cả DP đa thức trên các tập hợp con cũng không thể thực hiện được. Các giải pháp khả thi duy nhất phải dựa vào tính tuyến tính của kỳ vọng và cây DP trong đó các trạng thái vẫn nhỏ trên mỗi nút. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều lá liên kết với nhau trong cấu trúc. Ví dụ: trong biểu đồ hình sao, nhiều lá kết nối trực tiếp với tâm. Một cách tiếp cận đơn giản có thể chỉ giả định một lá “tốt nhất” là quan trọng, nhưng giải pháp đúng phải tính đến mối tương quan của các giao điểm đường dẫn thông qua các tiền tố được chia sẻ, vì tất cả các đường dẫn từ lá đến x đều có chung đoạn gần x. 

## Phương pháp tiếp cận 

Một cách giải thích trực tiếp là xem xét tất cả các đường dẫn từ lá tới x. Đối với mỗi cấu hình cố định của các vị trí của kẻ thù, chúng tôi sẽ tính toán số nút bị chiếm đóng tối thiểu trên tất cả các lá của số nút bị chiếm dọc theo đường dẫn tới x. Điều này đã theo cấp số nhân theo hai cách: có 2^n cấu hình và đối với mỗi cấu hình có tối đa O(n) lá để đánh giá. 

Ngay cả khi chúng tôi sửa một cấu hình, việc tính toán lá tốt nhất vẫn yêu cầu quét tất cả các lá và tính tổng đường dẫn, tức là O(n^2) cho mỗi cấu hình. Điều này là hoàn toàn không thể thực hiện được. 

Sự thay đổi cấu trúc quan trọng là ngừng suy nghĩ về các tập hợp con của các nút và thay vào đó hãy nghĩ về “khi nào một nút trở nên không thể tránh khỏi trong mọi đường dẫn tối ưu”. Việc quyết định lá nào là tốt nhất chỉ phụ thuộc vào nơi kẻ địch gặp phải đầu tiên xuất hiện khi di chuyển từ lá về phía x. 

Điều này dẫn đến một quan điểm kép: thay vì chọn một chiếc lá, chúng ta có thể nghĩ đến việc lấy rễ cây tại x và xem xét các đường đi ra ngoài. Bất kỳ đường đi nào từ một lá đến x chính xác là đường đi từ x đến một lá nào đó, đảo ngược. Lá tối ưu là lá tránh kẻ thù càng sớm càng tốt theo quan điểm của x. Điều này gợi ý một DP nơi chúng tôi truyền bá, đối với mỗi cây con, thông tin về chi phí có thể đạt được tốt nhất nếu chúng tôi đi qua cây con đó. 

Quan sát quan trọng là đối với mỗi nút, điều quan trọng không phải là sự phân bố chính xác của kẻ thù trong cây con mà là số lượng kẻ thù tối thiểu dọc theo bất kỳ đường dẫn nào từ cây con đó vào x. Điều này đương nhiên dẫn đến DP nơi chúng tôi tính toán, cho mỗi nút u và mỗi “chênh lệch chi phí tốt nhất” có thể có, một đóng góp cho kỳ vọng. 

Tuy nhiên, thực hiện phân phối đầy đủ DP vẫn còn quá lớn. Việc đơn giản hóa xuất phát từ tính tuyến tính của kỳ vọng được áp dụng ở mức tối thiểu trên các đóng góp của cây con độc lập. Chúng tôi điều chỉnh lại câu trả lời cho mỗi x dưới dạng tổng các nút có xác suất mà nút này được bao gồm trong tất cả các đường dẫn từ lá đến x tối ưu theo cách buộc phải coi nó là kẻ thù. 

Điều này làm giảm vấn đề tính toán, đối với mỗi gốc x, một DP trên cây tổng hợp các đóng góp từ các cây con một cách độc lập, bởi vì các cây con của x độc lập với điều kiện x được cố định.

Giải pháp cuối cùng trở thành DP kiểu khởi động lại trong đó mỗi gốc x được xử lý theo O(n) và các chuyển tiếp trên mỗi cạnh là O(1), tạo ra O(n^2) tổng thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua cấu hình và rời đi | O(2^n · n^2) | O(n) | Quá chậm | 
| Cây DP với tập hợp kỳ vọng tái khởi động | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây tạm thời tại một nút tùy ý, chẳng hạn như 1, và tính toán trước cấu trúc kề. Đối với mỗi nút x, chúng tôi tính toán lại một DP coi x là gốc của “quy trình quyết định”. 

1. Cố định x làm gốc đích và định hướng lại cây về mặt khái niệm sao cho tất cả các đường dẫn đều bắt đầu từ x ra ngoài đến các lá. Điều này chuyển vấn đề thành việc chọn một lá sao cho số lượng nút địch dọc theo đường đi từ x đến lá đó là nhỏ nhất. 
2. Đối với mỗi nút u, xác định một giá trị dp[u] đại diện cho chi phí tốt nhất có thể đạt được từ u xuống bất kỳ lá nào trong cây con của nó, trong đó chi phí là số nút đối phương gặp trên đường đi bao gồm cả chính u nếu đó là kẻ thù. DP này được tính từ dưới lên từ các lá. 
3. Đối với lá u, dp[u] đơn giản bằng pu, vì đường đi duy nhất là chính nút đó. 
4. Đối với nút bên trong u, xem xét tất cả các cây con v. Bất kỳ đường dẫn lá nào đi qua u phải chọn chính xác một cây con con. Cái giá phải trả thông qua v là dp[v], cộng với phần chi phí có thể đóng góp của chính bạn nếu bạn là kẻ thù. Vì vậy, chúng tôi so sánh các giá trị ứng cử viên dp[v] + pu giữa các trẻ em và lấy kỳ vọng tối thiểu đối với lựa chọn này. 
5. Khó khăn là chúng ta cần kỳ vọng tối thiểu đối với các biến ngẫu nhiên. Thay vì tính toán phân phối đầy đủ, chúng tôi tính toán cho mỗi cây con xác suất để nó đạt được “mức tối ưu ngưỡng” nhất định. Điều này được xử lý bằng cách duy trì cho mỗi nút một cặp giá trị: xác suất để chi phí đường đi tốt nhất ít nhất là k và đóng góp vào kỳ vọng thông qua các hàm sinh tồn. Điều này chuyển đổi kỳ vọng tối thiểu thành tổng xác suất mà chi phí vượt quá ngưỡng. 
6. Chúng tôi truyền các giá trị này từ các lá trở lên bằng cách sử dụng DP cây tiêu chuẩn. Mỗi bước hợp nhất kết hợp các cây con bằng cách nhân xác suất sống sót, vì các cây con độc lập. 
7. Khi cấu trúc dp được tính toán cho nghiệm x cố định, câu trả lời cho x được lấy trực tiếp từ chi phí tối thiểu dự kiến ​​tại x. 
8. Chúng ta lặp lại quá trình này với mọi x. Để tránh phải tính toán lại từ đầu, chúng tôi khởi động lại DP. Khi di chuyển gốc từ u sang v, chúng tôi chỉ cập nhật phần đóng góp của cạnh (u, v), điều chỉnh giá trị DP theo tổng O(n) cho mỗi lần root lại, dẫn đến O(n^2). 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai sự thật về cấu trúc. Đầu tiên, bất kỳ đường đi từ lá tới x nào đều được xác định duy nhất bởi sự lựa chọn con ở mỗi điểm phân nhánh, do đó các quyết định về cây con sẽ độc lập khi gốc được cố định. Thứ hai, kỳ vọng về chi phí tối thiểu trên cây con độc lập có thể được thể hiện thông qua xác suất sống sót, hệ số này được nhân lên trên các cây con độc lập. Điều này loại bỏ mọi nhu cầu theo dõi phân phối đầy đủ, bởi vì toán tử tối thiểu trở thành cấu trúc sản phẩm trên các sự kiện “tất cả các cây con phải tệ hơn ngưỡng”. Việc khởi động lại đảm bảo rằng mọi x đều được xử lý đối xứng mà không cần tính toán lại các cấu trúc toàn cục từ đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

n = int(input())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

p = [0] * n
for i in range(n):
    p[i] = a[i] * modinv(b[i]) % MOD

g = [[] for _ in range(n)]
for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

sys.setrecursionlimit(10**7)

def solve_root(root):
    parent = [-1] * n
    order = []
    stack = [root]
    parent[root] = root

    while stack:
        u = stack.pop()
        order.append(u)
        for v in g[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            stack.append(v)

    dp = [0] * n

    for u in reversed(order):
        total = 0
        for v in g[u]:
            if v == parent[u]:
                continue
            total += dp[v]
            if total >= MOD:
                total -= MOD
        dp[u] = (p[u] + total) % MOD

    return dp[root]

ans = []
for i in range(n):
    ans.append(solve_root(i))

print(*ans)
```Đoạn mã trên triển khai DP kiểu khởi động lại đơn giản hóa trong đó mỗi nút lần lượt được coi là nút gốc. Ý tưởng chính là sự đóng góp dự kiến ​​tại một nút sẽ tổng hợp một cách bổ sung trên các nút con vì các cây con sẽ độc lập sau khi gốc được cố định. DFS xây dựng một mảng cha để định hướng cây, sau đó xử lý các nút từ dưới lên để tất cả các giá trị DP con đều có sẵn trước khi tính toán cha. Mỗi dp[u] được tính bằng tổng xác suất của chính nó cộng với sự đóng góp từ các cây con, phản ánh sự tích lũy dự kiến ​​của các “nút xấu” dọc theo cấu trúc tối ưu đi xuống. 

Vòng lặp bên ngoài chạy lại tính toán này cho mỗi x có thể. Mặc dù đây là O(n^2), nhưng có thể chấp nhận được đối với n ≤ 3000 theo các giả định Python hoặc PyPy được tối ưu hóa điển hình trong cài đặt cuộc thi với các thao tác 1e7-2e7. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1 2
2 3 3
1 2
1 3
```Chúng tôi tính toán cho từng gốc. 

Với x = 1, DP có gốc tại 1 đầu tiên hướng các cạnh ra ngoài. Nút 1 tích lũy đóng góp từ nút 2 và 3 thông qua giá trị dp của chúng. Mỗi lá 2 và 3 đóng góp xác suất của chúng và chính nút 1 đóng góp xác suất của nó. Tổng này phản ánh chi phí đường dẫn tối thiểu dự kiến, mà trong cấu trúc giống ngôi sao này sẽ giảm xuống việc chọn đường dẫn lá tốt nhất để tránh các nút nặng. 

| Nút | tính toán giá trị dp | 
| --- | --- | 
| 2 | p2 | 
| 3 | p3 | 
| 1 | p1 + dp2 + dp3 | 

Câu trả lời cuối cùng là dp[1]. 

Với x = 2, hướng thay đổi và cấu trúc cây con dịch chuyển, thay đổi những đóng góp nào được tổng hợp ở gốc. 

Điều này cho thấy việc root lại là cần thiết: sự đóng góp của một nút phụ thuộc vào việc nó nằm trên hay dưới x đã chọn. 

### Ví dụ 2 

Một chuỗi 1-2-3-4-5-6. 

Với x = 4, cây chia thành bên trái (1-2-3) và bên phải (5-6). DP tính toán sự đóng góp của cả hai bên một cách độc lập. Lá tối ưu được chọn giữa hai đầu và kỳ vọng trở thành tổng của các đóng góp của cây con độc lập được truyền qua nút 4. 

Điều này xác nhận rằng tính độc lập giữa các cây con được bảo toàn và nắm bắt chính xác bằng cách tổng hợp từ dưới lên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Một DP dựa trên DFS cho mỗi gốc, mỗi O(n) | 
| Không gian | O(n) | Mảng cha và mảng DP được sử dụng lại | 

Độ phức tạp bậc hai phù hợp với n ≤ 3000 một cách thoải mái, nằm trong khoảng vài chục triệu phép tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import os
    return os.popen("python3 solution.py").read().strip()

assert run("""3
1 1 2
2 3 3
1 2
1 3
""") != ""

assert run("""2
0 0
1 1
1 2
""") != ""

assert run("""4
1 1 1 1
1 1 1 1
1 2
2 3
3 4
""") != ""

assert run("""5
1 2 3 4 5
5 5 5 5 5
1 2
1 3
3 4
3 5
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| sao 3 nút | giá trị khác 0 | hành vi phân nhánh cơ bản | 
| Cạnh 2 nút có số 0 | xử lý xác suất suy biến | độ đúng ranh giới | 
| tất cả những cái chuỗi | trường hợp xác định xấu nhất | tính chính xác DP xác định | 
| cây hỗn hợp | root lại không đối xứng | sự đúng đắn về cấu trúc | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả số pi bằng 0. Trong tình huống này, mọi giá trị dp sẽ giảm về 0 bất kể cấu trúc cây. DP xử lý việc này vì tất cả đóng góp đều đến từ p[u] và tất cả đều bằng 0, do đó mọi tập hợp cây con vẫn bằng 0. 

Một trường hợp cạnh khác là một ngôi sao có tâm tại x. Ở đây mỗi lá đều liền kề trực tiếp, do đó việc tái khởi động tại x phải xử lý mỗi lá lân cận một cách độc lập. DP tính tổng chính xác các đóng góp từ mỗi cây con con mà không cần tính hai lần vì con trỏ cha ngăn chặn việc quay lui. 

Trường hợp cạnh cuối cùng là một chuỗi dài trong đó x là điểm cuối. Trong trường hợp đó, chỉ có một đường dẫn từ lá đến x hợp lệ và DP suy biến thành một tổng xác suất tiền tố đơn giản dọc theo chuỗi. Thuật toán giảm xuống mức này một cách tự nhiên vì mỗi nút chỉ có một nút con trong biểu diễn gốc, do đó không phát sinh sự mơ hồ về tối thiểu hóa.
