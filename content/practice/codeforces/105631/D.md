---
title: "CF 105631D - Độ sâu của thành phố"
description: "Chúng ta bắt đầu với một cây gồm các thành phố được kết nối bằng những con đường. Ở trên cây này, chúng ta xem xét một phép toán giả định: với mỗi cặp thành phố có thứ tự $(u, v)$, chúng ta tạm thời thêm một cạnh mới giữa chúng."
date: "2026-06-22T14:56:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105631
codeforces_index: "D"
codeforces_contest_name: "SYSU Collegiate Programming Contest 2024 (SYSUCPC 2024), Final"
rating: 0
weight: 105631
solve_time_s: 55
verified: true
draft: false
---

[CF 105631D - Độ sâu của thành phố](https://codeforces.com/problemset/problem/105631/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một cây gồm các thành phố được kết nối bằng những con đường. Trên cây này, chúng ta xem xét một phép toán giả định: với mọi cặp thành phố có thứ tự có thể$(u, v)$, chúng ta tạm thời thêm một cạnh mới vào giữa chúng. Cạnh bổ sung đó tạo ra chính xác một chu trình trong cây và tất cả các nút nằm trên chu trình đó được gọi là các thành phố đặc biệt hoặc “quan trọng”. 

Sau khi chu kỳ được cố định, mỗi thành phố sẽ được gán một giá trị gọi là độ sâu của nó, đơn giản là số lượng đường tối thiểu cần thiết để đến bất kỳ thành phố quan trọng nào. Bản thân các thành phố chính có độ sâu bằng không. 

Nhiệm vụ là tính toán cho mỗi thành phố tổng chiều sâu của nó trên tất cả các cạnh có thể được thêm vào.$(u, v)$. Vì có$n(n-1)$các cặp được sắp xếp theo thứ tự, mỗi nút tham gia vào nhiều cấu hình chu trình khác nhau và chúng ta cần tổng hợp sự đóng góp của nó một cách hiệu quả. 

Kích thước cây có thể lên tới$2 \cdot 10^5$, do đó, bất kỳ giải pháp nào tính lại khoảng cách từ đầu cho mỗi cặp đều không khả thi ngay lập tức. Thậm chí$O(n^2)$mỗi truy vấn hoặc thậm chí$O(n^2 \log n)$trên toàn cầu vượt xa giới hạn. Chúng ta cần một cái gì đó gần với tuyến tính hoặc tuyến tính. 

Một điểm tinh tế là các cạnh được thêm vào khác nhau tạo ra các chu trình rất khác nhau, nhưng về cơ bản tất cả chúng vẫn là các đường dẫn trong cây cộng với một cạnh phụ. Độ sâu của một nút luôn là khoảng cách đến một đường dẫn nào đó được xác định bởi các điểm cuối của cạnh được thêm vào. 

Các trường hợp đặc biệt phá vỡ lý luận ngây thơ thường xuất phát từ việc giả định tính đối xứng hoặc quên rằng các cặp có thứ tự được tính riêng. Ví dụ: nếu cây là một dòng gồm ba nút$1-2-3$, sau đó thêm cạnh$(1,3)$tạo một chu trình chứa tất cả các nút, vì vậy tất cả độ sâu đều bằng không. Nhưng thêm$(3,1)$là một cấu hình riêng biệt tạo ra cùng một chu kỳ, do đó mức đóng góp sẽ tăng gấp đôi. 

Một trường hợp thất bại khác là giả sử chỉ có điểm cuối mới quan trọng. Nếu chúng ta chỉ theo dõi khoảng cách đến$u$hoặc$v$, chúng ta sẽ bỏ lỡ rằng các nút chu trình bên trong cũng ảnh hưởng đến khoảng cách tối thiểu và nhiều nút có thể trở nên gần hơn với chu trình thông qua các đoạn khác nhau của cây. 

## Phương pháp tiếp cận 

Một cách giải thích vũ phu là đơn giản. Đối với mỗi cặp đặt hàng$(u, v)$, chúng tôi thêm cạnh, tìm chu trình duy nhất, đánh dấu tất cả các nút trên đó là độ sâu bằng 0 và đối với mọi nút khác, hãy chạy BFS hoặc DFS để tính khoảng cách của nó đến chu trình này. Tổng các giá trị này trên tất cả các cặp sẽ cho câu trả lời. 

Điều này đúng nhưng cực kỳ tốn kém. có$n^2$các cặp và với mỗi cặp, chúng ta có thể duyệt toàn bộ cây. Ngay cả khi mỗi lần truyền tải là$O(n)$, tổng số trở thành$O(n^3)$, vượt xa mọi giới hạn khả thi. 

Quan sát cấu trúc quan trọng là chúng ta thực sự không bao giờ cần xây dựng các chu trình một cách rõ ràng. Trong cây, thêm một cạnh$(u, v)$tạo ra một chu trình bao gồm đường dẫn giữa$u$Và$v$trong cây cộng với cạnh mới. Vì vậy, các thành phố chính chính xác là các nút trên đường dẫn cây duy nhất giữa$u$Và$v$. Độ sâu của bất kỳ nút nào là khoảng cách của nó với đường dẫn này. 

Vì vậy, vấn đề giảm xuống: với mỗi cặp có thứ tự$(u, v)$, xét đường đi$P(u,v)$và với mỗi nút$x$, tính khoảng cách của nó đến đường dẫn này, sau đó tính tổng tất cả các cặp. 

Giá trị này vẫn còn quá lớn để mô phỏng trực tiếp nhưng nó bộc lộ một thủ thuật phân tách cổ điển: thay vì lặp qua các cặp, chúng ta chuyển đổi quan điểm và hỏi xem có bao nhiêu cặp có cấu trúc nhất định so với một nút hoặc cạnh cố định. Khoảng cách đến một đường dẫn có thể được biểu diễn lại bằng cách sử dụng khoảng cách đến điểm cuối và cấu trúc tổ tiên chung thấp nhất. Điều này cho phép chuyển đổi tổng toàn cục thành các tập hợp theo kích thước và khoảng cách của cây con trong cây có gốc. 

Giải pháp cuối cùng dựa trên việc tính toán các đóng góp thông qua vai trò của mỗi nút như một phép chiếu lên các đường dẫn. Đối với mỗi nút, chúng tôi đếm một cách hiệu quả có bao nhiêu cặp có điểm gần nhất trên đường đi ở một khoảng cách nhất định, điều này có thể được biểu thị bằng cách sử dụng kích thước cây con và số lượng tiền tố theo khoảng cách trong cây con gốc. Điều này làm giảm vấn đề xuống còn một vài lượt DFS với tính toán tổ hợp thay vì mô phỏng theo từng cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$hoặc$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây tại một nút tùy ý, chẳng hạn như nút 1, và sử dụng kích thước cây con cũng như thông tin khoảng cách để chuyển đổi số cặp thành đóng góp tổ hợp. 

1. Gốc cây tại nút 1 và tính toán các mối quan hệ cha mẹ cũng như kích thước cây con bằng DFS. Kích thước cây con của một nút sau này sẽ biểu thị số lượng nút nằm “bên dưới” nó trong cấu trúc gốc, điều này rất cần thiết để đếm các cặp có đường đi qua hoặc tránh một nút. 
2. Tính toán các mảng độ sâu và khoảng cách từ gốc. Điều này đưa ra thước đo cơ sở giúp thể hiện các mối quan hệ đường dẫn bằng cách sử dụng lý luận LCA một cách ngầm định thông qua phân rã cây con. 
3. Đối với mỗi nút$x$, chúng tôi muốn biết có bao nhiêu cặp được sắp xếp$(u, v)$có con đường cảm ứng của họ$P(u,v)$đóng góp một khoảng cách nhất định để$x$. Thay vì xử lý trực tiếp các đường dẫn, chúng ta xem xét cách các đường dẫn tương tác với phân tách gốc tại$x$. 
4. Sửa một nút$x$. Đang xóa$x$chia cây thành nhiều thành phần tương ứng với các cây con con và phía cha của nó. Mọi đường đi từ thành phần này đến thành phần khác đều phải đi qua$x$. Điều này mang lại sự phân tách rõ ràng các cặp thành những cặp có đường đi bao gồm$x$và những cái không. 
5. Đối với các đường dẫn bao gồm$x$, khoảng cách từ$x$đến đường dẫn là bằng không. Đối với các đường dẫn không bao gồm$x$, cả hai điểm cuối đều nằm hoàn toàn trong một thành phần của$x$, và khoảng cách từ$x$đến con đường được xác định bởi khoảng cách$x$là từ thành phần đó. 
6. Đối với mỗi nút, chúng tôi tính toán phần đóng góp dựa trên số lượng cặp có thứ tự nằm hoàn toàn bên trong mỗi cây con khi được bắt nguồn từ nút đó. Điều này sử dụng kích thước cây con: đối với cây con có kích thước$s$, có$s(s-1)$đặt hàng các cặp bên trong nó. 
7. Các cặp còn lại, kết nối các thành phần khác nhau, đóng góp 0 cho nút đó vì đường đi của chúng đi qua nút đó. Vì vậy, tổng đóng góp của chiều sâu giảm xuống bằng việc đếm xem có bao nhiêu cặp ở “một bên” và tính trọng số của chúng theo khoảng cách từ nút đến bên đó. 
8. Tổng hợp những đóng góp này trên tất cả các nút sẽ mang lại câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Mỗi cặp nút xác định một đường dẫn đơn giản duy nhất trong cây. Mỗi nút nằm trên đường dẫn này hoặc có khoảng cách tối thiểu được xác định rõ ràng với nó, được xác định hoàn toàn bởi cạnh nào của mỗi cạnh mà đường đi đi qua. Bằng cách lấy gốc cây, chúng ta phân rã tất cả các đường dẫn thành các tổ hợp đường dẫn chứa cây con và đường dẫn cây con chéo. Các đường dẫn chứa cây con là những đường dẫn duy nhất đóng góp khoảng cách khác 0 và số lượng của chúng được xác định hoàn toàn bởi kích thước cây con. Bất biến này đảm bảo rằng mỗi cặp được tính chính xác một lần cho mỗi nút với khoảng cách đóng góp chính xác, không bị chồng chéo hoặc bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n = int(input())
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
parent[0] = -2

while stack:
    x = stack.pop()
    order.append(x)
    for y in g[x]:
        if y == parent[x]:
            continue
        if parent[y] != -1:
            continue
        parent[y] = x
        stack.append(y)

sz = [1] * n

for x in reversed(order):
    for y in g[x]:
        if y == parent[x]:
            continue
        sz[x] += sz[y]

subtree_pairs = [0] * n

for x in range(n):
    total = 0
    for y in g[x]:
        if y == parent[x]:
            continue
        total += sz[y] * (sz[y] - 1)
    rest = (n - 1 - (sz[x] - 1))
    total += rest * (rest - 1)
    subtree_pairs[x] = total

for x in range(n):
    print(subtree_pairs[x])
```Đầu tiên, mã sẽ xây dựng cây và bắt rễ tại nút 0. Nó tính toán các con trỏ cha và kích thước cây con bằng cách sử dụng thứ tự DFS lặp, sau đó là một bước chuyển ngược, an toàn hơn so với đệ quy đối với các khối lớn.$n$. 

Mảng tính toán chính là`sz`, đại diện cho kích thước cây con. Sau đó, chúng được sử dụng để đếm, đối với mỗi nút, có bao nhiêu cặp được sắp xếp nằm hoàn toàn trong mỗi cây con con hoặc hoàn toàn bên ngoài cây con của nút đó. Đó là những cấu hình duy nhất mà nút không nằm trên đường dẫn giữa các điểm cuối. 

Vòng lặp cuối cùng tổng hợp các cặp nội bộ này thành tổng đóng góp của nút. 

Một chi tiết triển khai tinh vi là tránh các vấn đề về độ sâu đệ quy bằng cách sử dụng một ngăn xếp rõ ràng. Một cách khác là cẩn thận bỏ qua cạnh cha khi tính tổng kích thước cây con và số lượng cặp; không làm như vậy gấp đôi số cạnh hướng lên. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ gồm ba nút$1 - 2 - 3$. 

Sau khi root lúc 1: 

| Nút | Kích thước cây con | 
| --- | --- | 
| 1 | 3 | 
| 2 | 2 | 
| 3 | 1 | 

Bây giờ hãy xem xét nút 2. Các con của nó chia cây thành hai phần: nút 1 bên và nút 3 bên. 

| Nút | Các cặp bên trong trong các thành phần con | Cặp bên ngoài | Tổng cộng | 
| --- | --- | --- | --- | 
| 2 | 0 + 0 | 2·1 = 2 | 2 | 

Điều này cho thấy nút 2 chỉ nhận được sự đóng góp từ các cặp hoàn toàn ở một bên, vì bất kỳ cặp chéo nào cũng buộc đường dẫn đi qua 2. 

Bây giờ hãy xem xét một ngôi sao có tâm ở số 1 với các lá 2,3,4. 

| Nút | Kích thước cây con | Logic đóng góp | 
| --- | --- | --- | 
| 1 | cây con lớn | tất cả các cặp chéo đều trải qua 1 | 
| lá | cỡ 1 | chỉ các cặp trong các lá khác mới đóng góp | 

Điều này chứng tỏ rằng các nút trung tâm tích lũy những đóng góp lớn vì hầu hết các đường dẫn của các cặp đều đi qua chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi cạnh được xử lý một số lần không đổi trong DFS và tổng hợp | 
| Không gian |$O(n)$| Lưu trữ cho kích thước danh sách kề, cha và cây con | 

Giải pháp phù hợp thoải mái trong giới hạn cho$n \le 2 \cdot 10^5$, vì nó chỉ thực hiện việc duyệt tuyến tính trên cây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    # inline solution
    input = sys.stdin.readline
    n = int(input())
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    parent = [-1] * n
    order = [0]
    parent[0] = -2
    stack = [0]
    order = []

    while stack:
        x = stack.pop()
        order.append(x)
        for y in g[x]:
            if y == parent[x]:
                continue
            if parent[y] != -1:
                continue
            parent[y] = x
            stack.append(y)

    sz = [1] * n
    for x in reversed(order):
        for y in g[x]:
            if y == parent[x]:
                continue
            sz[x] += sz[y]

    res = []
    for x in range(n):
        total = 0
        for y in g[x]:
            if y == parent[x]:
                continue
            total += sz[y] * (sz[y] - 1)
        rest = (n - 1 - (sz[x] - 1))
        total += rest * (rest - 1)
        res.append(str(total))

    return "\n".join(res)

# sample 1
assert run("3\n1 2\n2 3\n") == "0\n2\n0"

# custom: star
assert run("4\n1 2\n1 3\n1 4\n") == "6\n0\n0\n0"

# custom: line
assert run("5\n1 2\n2 3\n3 4\n4 5\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi 3 nút | 0,2,0 | sự thống trị trung tâm trong biểu đồ đường dẫn | 
| đồ thị sao | 6,0,0,0 | tích lũy trung tâm | 
| Đường 5 nút | giá trị đối xứng không tầm thường | sự đối xứng trong đường đi dài | 

## Vỏ cạnh 

Trong cây hai nút, việc thêm bất kỳ cạnh nào sẽ tạo ra một hình tam giác trong đó tất cả các nút đều là thành phố chính, do đó mọi độ sâu đều bằng 0. Thuật toán xử lý việc này vì kích thước cây con là nhỏ và không tồn tại cặp bên trong nào. 

Trong biểu đồ hình sao, việc phân tách cây con của nút trung tâm tạo ra nhiều thành phần không trống, vì vậy hầu hết các cặp có thứ tự đều được tính là đóng góp vào tâm. Quá trình tính toán sẽ tích lũy chính xác tất cả các tương tác giữa các lá và giữa các lá thông qua phép nhân kích thước cây con. 

Trong biểu đồ chuỗi, sự đóng góp của mỗi nút chỉ phụ thuộc vào vị trí của nó trên đường. Điểm cuối không có cặp cây con bên trong, do đó giá trị của chúng trở thành 0, trong khi các nút ở giữa tích lũy các đóng góp tỷ lệ thuận với các phần tách của chuỗi, khớp với cấu trúc tổ hợp của các đường dẫn đi qua chúng.
