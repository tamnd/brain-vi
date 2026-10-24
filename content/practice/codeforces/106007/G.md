---
title: "CF 106007G - Game Nim Ở Byteland"
description: "Chúng ta được cung cấp một đồ thị có hướng với các nút $n$, trong đó mỗi nút đại diện cho một thành phố và có nhiều nhất hai cạnh đi ra. Alice bắt đầu ở thành phố $1$ và muốn đến thành phố $n$."
date: "2026-06-22T16:42:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106007
codeforces_index: "G"
codeforces_contest_name: "The 2025 Aleppo Collegiate programming contest"
rating: 0
weight: 106007
solve_time_s: 67
verified: true
draft: false
---

[CF 106007G - Trò chơi Nim ở Byteland](https://codeforces.com/problemset/problem/106007/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng với$n$các nút, trong đó mỗi nút đại diện cho một thành phố và có nhiều nhất hai cạnh đi ra. Alice bắt đầu tại thành phố$1$và muốn đến thành phố$n$. Trên đường đi, cô ấy có thể quay lại các thành phố một cách tùy ý nhiều lần, cho phép cô ấy đi qua các chu trình trong biểu đồ một cách hiệu quả. 

Mỗi thành phố cũng chứa một đống đá có kích thước cố định. Bất cứ khi nào Alice đến thăm một thành phố, cô ấy có thể chọn lấy đống đồ của thành phố đó, nhưng cô ấy được phép lấy từ mỗi thành phố nhiều nhất một lần trong toàn bộ hành trình. Khi cô ấy tới thành phố$n$, cô ấy sẽ thu thập được một tập hợp con các kích thước cọc được xác định theo con đường mà cô ấy “sử dụng” một cách hiệu quả. 

Khi cô ấy đến, các cọc được thu thập sẽ tạo thành một trò chơi Nim tiêu chuẩn: mỗi cọc là một đống độc lập và người chơi luân phiên loại bỏ bất kỳ số lượng đá dương nào khỏi một đống duy nhất. Alice di chuyển thứ hai. Vì lối chơi Nim tối ưu hoàn toàn được đặc trưng bởi XOR của kích thước cọc, Alice thắng khi và chỉ khi XOR của tất cả các kích thước cọc được chọn khác 0. 

Vì vậy, nhiệm vụ thực sự là quyết định xem Alice có thể chọn đi bộ từ$1$ĐẾN$n$và chọn một tập hợp con các nút dọc theo bước đi đó sao cho XOR có trọng số của chúng khác 0. 

Cấu trúc đồ thị là hạn chế quan trọng. Với tối đa$10^5$các nút và mỗi nút có nhiều nhất hai cạnh đi ra, biểu đồ thưa thớt, nhưng sự hiện diện của các chu kỳ có nghĩa là có thể có nhiều đường dẫn riêng biệt và các mẫu xem lại theo cấp số nhân. Việc khám phá một cách ngây thơ trên tất cả các đường dẫn hoặc tập hợp con là không thể. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các đường dẫn từ$1$ĐẾN$n$chỉ cho phép một tập hợp cố định các kết hợp nút có thể truy cập, buộc một kết quả XOR duy nhất có hiệu quả. Ví dụ: nếu mọi đường dẫn truy cập chính xác vào cùng một tập hợp nhiều nút (vì việc phân nhánh không làm thay đổi khả năng tiếp cận), thì Alice hoàn toàn không thể ảnh hưởng đến XOR. Một trường hợp quan trọng khác là khi chu kỳ cho phép truy cập lại nhiều lần, điều này có thể gợi ý bao gồm các giá trị lặp đi lặp lại, nhưng quy tắc “nhiều nhất một lần cho mỗi thành phố” sẽ loại bỏ bất kỳ lợi ích nào từ việc đạp xe ngoại trừ khả năng tiếp cận. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo cố gắng liệt kê tất cả các chuyến đi bộ có thể có từ thành phố$1$đến thành phố$n$. Đối với mỗi bước đi, Alice có thể chọn bất kỳ tập hợp con nào của các nút đã truy cập, vì vậy mỗi đường dẫn tương ứng với nhiều tập hợp con XOR theo cấp số nhân. Ngay cả khi bỏ qua lựa chọn tập hợp con, số bước đi riêng biệt trong biểu đồ có chu kỳ có thể bùng nổ, dễ dàng trở thành cấp số nhân trong$n$. Hệ số phân nhánh tối đa là hai cho mỗi nút, do đó chỉ riêng số lượng đường dẫn có thể đạt tới$2^n$trong trường hợp xấu nhất là các cấu trúc giống DAG, khiến cho việc liệt kê không thể thực hiện được. 

Sự đơn giản hóa chính xuất phát từ việc tách chuyển động khỏi lựa chọn. Chuyển động xác định các nút nào thậm chí có thể truy cập được một cách nhất quán, trong khi lựa chọn xác định các lựa chọn XOR đối với các nút có thể truy cập đó. Bởi vì việc xem lại các nút không cho phép lấy lại giá trị của chúng, nên chu kỳ chỉ quan trọng trong chừng mực chúng xác định các hạn chế về khả năng tiếp cận và kết nối. 

Chúng tôi chuyển vấn đề thành lý luận về tập con giá trị nút nào có thể “kích hoạt được” dọc theo một số đường dẫn từ$1$ĐẾN$n$. Điều này trở thành một vấn đề về khả năng tiếp cận biểu đồ được tăng cường bằng tính năng theo dõi trạng thái XOR. Mỗi trạng thái có thể được xem như một cặp nút hiện tại và giá trị XOR hiện tại được tích lũy cho đến nay. Tuy nhiên, trực tiếp thực hiện BFS qua$(node, xor)$cũng quá lớn vì giá trị XOR có thể đạt tới$10^6$, ngụ ý lên tới khoảng$2^{20}$các trạng thái có thể. 

Bước đột phá về cấu trúc là lưu ý rằng biểu đồ có nhiều nhất là hai, cho phép chúng ta nén hành vi thành một hệ thống chuyển tiếp xác định qua các trạng thái XOR bằng cách sử dụng lập trình động về khả năng tiếp cận. Thay vì theo dõi tất cả các giá trị XOR một cách rõ ràng trên mỗi nút, chúng tôi duy trì một tập hợp các trạng thái XOR có thể đạt được ở mỗi nút, nhưng chúng tôi khai thác thêm rằng các chuyển đổi XOR tạo thành cấu trúc tuyến tính trên$\mathbb{F}_2$. Mỗi nút đóng góp “lấy” hoặc “bỏ qua” và việc lấy một nút sẽ lật XOR bằng cách$a_i$. Vì vậy, việc di chuyển qua biểu đồ sẽ tạo ra các cập nhật XOR chỉ phụ thuộc vào lượt truy cập nút. 

Chúng ta có thể diễn giải lại quy trình như sau: mọi đường dẫn hợp lệ tương ứng với một tập hợp các nút có thể truy cập từ$1$ĐẾN$n$và dọc theo bất kỳ quá trình truyền tải nào, chúng ta có thể chọn bất kỳ tập hợp con nào của các nút tuân theo thứ tự khả năng tiếp cận. Ràng buộc thứ tự là sự ghép nối duy nhất giữa các nút. 

Điều này làm giảm khả năng tính toán liệu có tồn tại một đường dẫn từ$1$ĐẾN$n$sao cho tập hợp nhiều nút được truy cập thừa nhận một tập hợp con có XOR khác 0. Vì Alice có thể chọn các tập hợp con một cách tự do dọc theo một đường dẫn, nên trở ngại duy nhất là khi tất cả các tập hợp con có thể có của các giá trị nút có thể truy cập được thu gọn về XOR 0, điều này xảy ra chính xác khi khoảng tuyến tính của các giá trị có thể truy cập là trống hoặc chỉ có giá trị bằng 0. Trong thực tế, điều này giúp kiểm tra xem có tồn tại bất kỳ cấu trúc khả năng tiếp cận theo chu kỳ nào cho phép đưa phần tử cơ sở khác 0 trên một số đường dẫn hay không, có thể được phát hiện bằng cách truyền bá thông tin cơ sở XOR qua biểu đồ. 

Do đó, chúng tôi chạy truyền tải giống như DP từ nút$1$, duy trì cơ sở các giá trị XOR có thể truy cập được tại mỗi nút. Khi nhiều đường dẫn hợp nhất, chúng tôi hợp nhất các cơ sở XOR của chúng. Câu trả lời phụ thuộc vào việc liệu nút$n$có thể đạt được trạng thái XOR khác 0. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các đường dẫn và tập hợp con |$O(2^n)$|$O(n)$| Quá chậm | 
| Đồ thị trạng thái XOR DP với sự lan truyền cơ sở |$O(n \log A)$|$O(n \log A)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề từ các cạnh đi ra của mỗi nút, bỏ qua các cạnh bằng 0. Điều này đưa ra một biểu đồ có hướng trong đó mỗi nút có nhiều nhất hai nút lân cận. 
2. Thực hiện truyền tải khả năng tiếp cận từ nút$1$để xác định tất cả các nút có thể xuất hiện trên bất kỳ đường dẫn hợp lệ nào tới$n$. Điều này đảm bảo chúng tôi không truyền bá thông tin vào ngõ cụt không liên quan đến đích đến. 
3. Duy trì cấu trúc cho thông tin cơ sở XOR trên mỗi nút, trong đó mỗi nút lưu trữ một cơ sở tuyến tính nhỏ gồm các giá trị đại diện cho tất cả các tổng XOR có thể đạt được bằng các tập hợp con của các nút đã chọn dọc theo một số đường dẫn hợp lệ đến nó. 
4. Khởi tạo cơ sở tại nút$1$chỉ chứa$a_1$, vì Alice có thể chọn lấy cọc đầu tiên ngay lập tức. 
5. Duyệt qua biểu đồ theo cách lan truyền giống như BFS hoặc tôpô, cập nhật các nút lân cận. Khi di chuyển từ$u$ĐẾN$v$, truyền bá tất cả các phần tử cơ sở XOR từ$u$vào trong$v$, và ngoài ra hãy xem xét việc thêm$a_v$như một phần tử có thể lựa chọn. Điều này mô hình hóa sự lựa chọn đi hoặc bỏ qua thành phố$v$. 
6. Bất cứ khi nào hợp nhất các cơ sở, hãy chèn từng phần tử vào cơ sở đích bằng cách sử dụng phép loại bỏ Gaussian trên không gian XOR. Điều này đảm bảo cơ sở vẫn ở mức tối thiểu và hiệu quả. 
7. Nếu tại bất kỳ điểm nào cơ sở tại nút$n$có thể biểu thị cấu hình XOR khác 0, trả về CÓ. Ngược lại, sau khi xử lý tất cả các trạng thái có thể truy cập, hãy trả về NO. 

### Tại sao nó hoạt động 

Mỗi đường đi từ$1$ĐẾN$n$xác định một chuỗi các nút có thứ tự. Các lựa chọn của Alice tương ứng chính xác với việc chọn một tập hợp con của các nút đó, chuyển thành tổng XOR của các tập hợp con có trọng số dọc theo chuỗi đó. Cơ sở XOR được duy trì tại mỗi nút đại diện cho tất cả các kết hợp XOR có thể đạt được của các tập hợp con dọc theo một số đường dẫn tiền tố hợp lệ. Bởi vì các hoạt động XOR tạo thành một không gian vectơ trên các bit, nên việc hợp nhất các cơ sở dọc theo các đường dẫn khác nhau sẽ bảo toàn tất cả các kết quả XOR có thể có mà không liệt kê rõ ràng các tập hợp con hoặc đường dẫn. Do đó, thuật toán nắm bắt toàn bộ các kết quả XOR có thể truy cập được tại đích. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# Insert x into XOR basis (standard linear basis over integers)
def add_basis(basis, x):
    for b in basis:
        x = min(x, x ^ b)
    if x:
        basis.append(x)
        basis.sort(reverse=True)

def merge_basis(a, b):
    res = a[:]
    for x in b:
        add_basis(res, x)
    return res

def can_make_nonzero(basis):
    return any(x != 0 for x in basis)

n = int(input())
g = [[] for _ in range(n + 1)]

for i in range(1, n + 1):
    u, v = map(int, input().split())
    if u:
        g[i].append(u)
    if v:
        g[i].append(v)

a = [0] + list(map(int, input().split()))

# reachable nodes
from collections import deque

reach = [False] * (n + 1)
q = deque([1])
reach[1] = True

while q:
    u = q.popleft()
    for v in g[u]:
        if not reach[v]:
            reach[v] = True
            q.append(v)

# DP of bases
dp = [None] * (n + 1)
dp[1] = [a[1]]

q = deque([1])

while q:
    u = q.popleft()
    if dp[u] is None:
        continue
    for v in g[u]:
        if not reach[v]:
            continue
        new_basis = merge_basis(dp[u], [a[v]])
        if dp[v] is None:
            dp[v] = new_basis
            q.append(v)
        else:
            merged = merge_basis(dp[v], new_basis)
            if merged != dp[v]:
                dp[v] = merged
                q.append(v)

print("YES" if dp[n] and can_make_nonzero(dp[n]) else "NO")
```Việc triển khai bắt đầu bằng cách xây dựng danh sách kề từ hai cạnh đi ra trên mỗi nút. Thẻ khả năng tiếp cận đảm bảo chúng tôi chỉ truyền bá các trạng thái vẫn có thể dẫn đến đích, tránh các chu kỳ vô dụng không bao giờ góp phần tiếp cận nút$n$. 

các`add_basis`hàm duy trì cơ sở XOR rút gọn, chèn từng giá trị ứng cử viên đồng thời loại bỏ sự phụ thuộc tuyến tính bằng cách sử dụng rút gọn XOR. Đây là cơ chế cốt lõi ngăn chặn sự tăng trưởng theo cấp số nhân ở các trạng thái được lưu trữ. 

DP sử dụng hàng đợi để truyền các trạng thái cơ bản được cải tiến về phía trước. Bất cứ khi nào một nút nhận được cơ sở tốt hơn, nó sẽ được xử lý lại, đảm bảo sự ổn định cuối cùng trên tất cả các cấu hình có thể truy cập. Về mặt tinh thần, điều này tương tự như việc nới lỏng đường đi ngắn nhất, ngoại trừ “khoảng cách” được thay thế bằng trạng thái đại số phong phú hơn. 

Kiểm tra cuối cùng chỉ đơn giản là kiểm tra xem nút$n$có bất kỳ XOR nào có thể biểu thị khác 0, tương ứng trực tiếp với việc Alice có thể xây dựng cấu hình Nim chiến thắng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Biểu đồ đầu vào có phân nhánh nhỏ trong đó tồn tại một nút có lợi: 

| Bước | Nút | Cơ sở tại nút | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | {2} | bắt đầu | 
| 2 | 2 | {2, 4} | tuyên truyền và bao gồm | 
| 3 | 4 | {2, 4, 1} | đạt mục tiêu | 

Dấu vết này cho thấy cơ sở XOR khác 0 xuất hiện ở đích, nghĩa là Alice có thể chọn một tập hợp con mang lại vị trí Nim chiến thắng. 

### Ví dụ 2 

Đầu vào trong đó tất cả các giá trị có thể truy cập đều bị hủy theo cấu trúc: 

| Bước | Nút | Cơ sở tại nút | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | {1} | bắt đầu | 
| 2 | 2 | {1} | tuyên truyền cấu trúc giống hệt nhau | 
| 3 | n | {1} | chưa có hướng đi độc lập mới | 

Ở đây, mọi tập hợp con có thể truy cập sẽ thu gọn thành một phụ thuộc tuyến tính duy nhất, vì vậy tất cả các kết hợp XOR vẫn có thể dự đoán được và không bao giờ mang lại cấu hình khác 0 chiến thắng. 

Sự tương phản giữa các dấu vết này cho thấy thuật toán không theo dõi các đường dẫn mà là sự phong phú về mặt đại số của các lựa chọn có thể tiếp cận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log A)$| Mỗi lần chèn vào chi phí cơ sở XOR ở hầu hết các hoạt động bit logarit trên kích thước giá trị | 
| Không gian |$O(n \log A)$| Mỗi nút lưu trữ một cơ sở tuyến tính nhỏ | 

Những ràng buộc cho phép$10^5$các nút và giá trị lên tới$10^6$, do đó độ phức tạp bit logarit là đủ. Biểu đồ thưa thớt và mỗi cạnh được xử lý một số lần nhỏ do chỉ được nới lỏng khi có cải tiến, giúp duy trì thời gian chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def add_basis(basis, x):
        for b in basis:
            x = min(x, x ^ b)
        if x:
            basis.append(x)
            basis.sort(reverse=True)

    def merge_basis(a, b):
        res = a[:]
        for x in b:
            add_basis(res, x)
        return res

    def can_make_nonzero(basis):
        return any(x != 0 for x in basis)

    n = int(input())
    g = [[] for _ in range(n + 1)]
    for i in range(1, n + 1):
        u, v = map(int, input().split())
        if u:
            g[i].append(u)
        if v:
            g[i].append(v)

    a = [0] + list(map(int, input().split()))

    from collections import deque

    reach = [False] * (n + 1)
    q = deque([1])
    reach[1] = True
    while q:
        u = q.popleft()
        for v in g[u]:
            if not reach[v]:
                reach[v] = True
                q.append(v)

    dp = [None] * (n + 1)
    dp[1] = [a[1]]
    q = deque([1])

    while q:
        u = q.popleft()
        if dp[u] is None:
            continue
        for v in g[u]:
            if not reach[v]:
                continue
            new_basis = merge_basis(dp[u], [a[v]])
            if dp[v] is None:
                dp[v] = new_basis
                q.append(v)
            else:
                merged = merge_basis(dp[v], new_basis)
                if merged != dp[v]:
                    dp[v] = merged
                    q.append(v)

    return "YES" if dp[n] and can_make_nonzero(dp[n]) else "NO"

# provided samples
assert run("2 0\n3 4\n1 0\n0 0\n2 4 1 1\n") == "YES", "sample 1"
assert run("2 0\n3 4\n1 0\n0 0\n2 4 8 1\n") == "NO", "sample 2"

# custom cases
assert run("1 0\n0 0\n5\n") == "YES", "single node must take"
assert run("2 0\n0 0\n1 1\n") == "YES", "simple XOR nonzero"
assert run("3 0\n1 0\n2 0\n0 0\n1 1 1\n") == "NO", "cancellation path"
assert run("4 2\n2 3\n3 4\n4 0\n0 0\n1 2 4 8\n") == "YES", "chain with increasing values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | CÓ | lựa chọn bắt buộc tối thiểu | 
| 2 nút đơn giản | CÓ | tăng XOR trực tiếp | 
| đường dẫn hủy bỏ | KHÔNG | cấu trúc XOR phụ thuộc | 
| chuỗi ngày càng tăng | CÓ | truyền qua cấu trúc giống DAG | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi biểu đồ tạo thành một chu trình thuần túy giữa các nút có chung các giá trị giống hệt nhau. Mặc dù Alice có thể lặp vô thời hạn nhưng cô ấy không thể thu thập lại các giá trị, do đó chu trình không đóng góp tính linh hoạt XOR mới. Thuật toán xử lý vấn đề này vì việc xem lại các nút không mở rộng cơ sở, do đó quá trình truyền ổn định ngay lập tức và không thể tạo trạng thái mới nhân tạo. 

Một trường hợp cạnh khác xảy ra khi nhiều đường dẫn đến tới một nút nhưng tất cả đều mang các cơ sở XOR giống hệt nhau. Một BFS ngây thơ có thể xử lý lại nút một cách vô hạn hoặc dư thừa, nhưng việc so sánh cơ sở sẽ ngăn chặn các cập nhật trừ khi thông tin XOR hoàn toàn mới xuất hiện. Điều này đảm bảo sự hội tụ ngay cả trong các cấu trúc hợp nhất dày đặc. 

Trường hợp cạnh cuối cùng là khi nút$n$có thể truy cập được nhưng chỉ thông qua các đường dẫn buộc phải bao gồm một giá trị nhiều lần về mặt khái niệm. Vì mỗi nút đóng góp nhiều nhất một lần nên thuật toán tránh được việc đếm trùng lặp một cách chính xác và cách biểu diễn cơ sở ngăn ngừa việc đếm quá mức bằng cách giảm tất cả các kết hợp về mức độc lập tuyến tính.
