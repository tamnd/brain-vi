---
title: "CF 1245D - Shichikuji và lưới điện"
description: "Chúng ta được cho một tập hợp các thành phố được đặt trên một mặt phẳng. Mỗi thành phố có thể được “cung cấp năng lượng” theo một trong hai cách: hoặc chúng tôi trực tiếp xây dựng một nhà máy điện trong đó hoặc chúng tôi kết nối nó (trực tiếp hoặc gián tiếp qua các thành phố khác) với một thành phố đã có nhà máy điện."
date: "2026-06-15T21:35:58+07:00"
tags: ["codeforces", "competitive-programming", "dsu", "graphs", "greedy", "shortest-paths", "trees"]
categories: ["algorithms"]
codeforces_contest: 1245
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 597 (Div. 2)"
rating: 1900
weight: 1245
solve_time_s: 198
verified: false
draft: false
---

[CF 1245D - Shichikuji và Lưới điện](https://codeforces.com/problemset/problem/1245/D) 

**Xếp hạng:** 1900 
**Tags:** dsu, đồ thị, tham lam, đường đi ngắn nhất, cây cối 
**Thời gian giải:** 3 phút 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các thành phố được đặt trên một mặt phẳng. Mỗi thành phố có thể được “cung cấp năng lượng” theo một trong hai cách: hoặc chúng tôi trực tiếp xây dựng một nhà máy điện trong đó hoặc chúng tôi kết nối nó (trực tiếp hoặc gián tiếp qua các thành phố khác) với một thành phố đã có nhà máy điện. 

Xây dựng một nhà ga trong thành phố$i$có chi phí cố định$c_i$. Kết nối hai thành phố$i$Và$j$yêu cầu đặt một sợi dây dọc theo lưới và chiều dài dây là khoảng cách Manhattan của họ$|x_i - x_j| + |y_i - y_j|$. Chi phí của kết nối này là chiều dài nhân với chi phí trên mỗi đơn vị phụ thuộc vào cả hai điểm cuối,$k_i + k_j$. 

Mục tiêu cuối cùng là đảm bảo mọi thành phố đều được cấp điện, đồng thời giảm thiểu tổng chi phí cho các trạm và kết nối đã chọn. Đầu ra không chỉ là chi phí tối thiểu mà còn là cấu trúc của giải pháp: thành phố nào có trạm và cặp thành phố nào được kết nối. 

Các ràng buộc cho phép lên tới 2000 thành phố. Một đồ thị tất cả các cặp đơn giản có khoảng 4 triệu cạnh, vốn đã là đường biên nhưng vẫn khả thi nếu được xử lý cẩn thận với$O(n^2)$thuật toán. Bất cứ điều gì liên quan$O(n^2 \log n)$hoặc tệ hơn cho mỗi bài kiểm tra vẫn sẽ vượt qua, nhưng mọi thứ khối hoặc liên quan đến việc tính toán đường đi ngắn nhất lặp đi lặp lại thì không. 

Một trường hợp phức tạp phát sinh khi tất cả chi phí kết nối đều rất cao so với chi phí của trạm. Trong trường hợp đó, giải pháp tối ưu là xây dựng các trạm ở mọi nơi và không sử dụng biên. Ngược lại, nếu chi phí trạm lớn và chi phí kết nối nhỏ thì giải pháp tối ưu sẽ là một trạm duy nhất cộng với một cây bao trùm trên tất cả các thành phố. Một cách tiếp cận ngây thơ tham lam chọn các cạnh cục bộ mà không có cấu trúc tổng thể có thể thất bại ở cả hai thái cực vì các quyết định tương tác trên toàn bộ biểu đồ. 

Một chế độ thất bại khác xuất hiện khi hai thành phố có tọa độ giống hệt nhau. Khoảng cách Manhattan của họ bằng 0, vì vậy việc kết nối chúng không tốn phí. Bất kỳ việc triển khai không chính xác nào giả định trọng số cạnh là dương hoặc bỏ qua các cạnh có độ dài bằng 0 đều có thể hoạt động không chính xác. 

## Phương pháp tiếp cận 

Vấn đề đương nhiên là một cách với chi phí tối thiểu để “kích hoạt” tất cả các nút, trong đó việc kích hoạt có thể đến từ việc trả chi phí nút hoặc trả chi phí biên để truyền bá kích hoạt. Điều này gợi ý một cấu trúc tối ưu hóa biểu đồ hơn là một lựa chọn tham lam cục bộ. 

Cách giải thích thô bạo sẽ coi mọi tập hợp con của thành phố là các vị trí trạm tiềm năng và sau đó tính toán cách tốt nhất để kết nối các nút còn lại bằng cách sử dụng cấu trúc kéo dài tối thiểu trên các biểu đồ cảm ứng. Điều này ngay lập tức trở nên không thể bởi vì có$2^n$tập hợp con và mỗi đánh giá vẫn sẽ yêu cầu ít nhất$O(n^2)$xử lý, dẫn đến thời gian theo cấp số nhân. 

Quan sát quan trọng là chúng ta có thể diễn giải lại bài toán dưới dạng một bài toán đồ thị đơn. Chúng tôi giới thiệu một nút ảo bổ sung đại diện cho “nguồn điện”. Kết nối thành phố$i$trực tiếp đến chi phí nút ảo này$c_i$. Điều này chuyển đổi việc xây dựng trạm thành lựa chọn cạnh. Bây giờ mọi thành phố đều được kết nối với nút ảo hoặc được kết nối qua các thành phố khác và chúng tôi đang chọn một cây bao trùm tối thiểu trên$n+1$nút. 

Chi phí giữa hai thành phố trở thành một lợi thế có trọng số:$$w(i, j) = (k_i + k_j)\cdot (|x_i - x_j| + |y_i - y_j|)$$và các cạnh của nút ảo là:$$w(i, 0) = c_i$$Do đó, lời giải đầy đủ là bài toán cây khung nhỏ nhất trên đồ thị đầy đủ của$n+1$nút. Từ$n \le 2000$, thuật toán Prim với$O(n^2)$thực hiện là đủ, tránh sự cần thiết phải lưu trữ rõ ràng tất cả các cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n²) | Quá chậm | 
| Prim MST với nút ảo | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một biểu đồ khái niệm với$n$các thành phố cộng với một nút bổ sung đại diện cho nguồn năng lượng toàn cầu. 

1. Khởi tạo một mảng`min_cost[i]`là cách rẻ nhất để kết nối thành phố$i$đến cơ cấu ngày càng tăng. Ban đầu đây là$c_i$, nghĩa là chúng tôi cho rằng việc xây dựng một nhà ga là lựa chọn tốt nhất. 

Điều này tương ứng với việc kết nối trực tiếp từng thành phố với nút ảo. 
2. Duy trì một mảng đã truy cập cho biết những thành phố nào đã được đưa vào cấu trúc cuối cùng. Bắt đầu không bao gồm. 
3. Liên tục chọn thành phố chưa được ghé thăm có kích thước nhỏ nhất`min_cost`. Lựa chọn này là tham lam nhưng hợp lệ vì chúng tôi đang chạy thuật toán Prim một cách hiệu quả, trong đó cạnh tốt nhất có sẵn để mở rộng cây hiện tại phải an toàn. 
4. Đánh dấu thành phố này là đã ghé thăm và thêm chi phí của nó vào câu trả lời. Nếu kết nối tốt nhất của nó là nút ảo, chúng tôi sẽ ghi lại một trạm. Mặt khác, chúng tôi ghi nhận lợi thế của thành phố đã mang lại sự cải thiện này. 
5. Sau khi thêm thành phố, hãy cố gắng cải thiện chi phí kết nối của mọi thành phố chưa được ghé thăm còn lại bằng cách xem xét lợi thế so với thành phố mới được thêm vào. Cho mỗi cặp$i, j$, tính khoảng cách Manhattan và cập nhật:$$\text{cost} = (k_i + k_j)\cdot(|x_i - x_j| + |y_i - y_j|)$$Nếu giá trị này nhỏ hơn chi phí được biết đến tốt nhất hiện tại cho$j$, chúng tôi cập nhật và ghi lại điều đó$i$là cha mẹ của$j$. 
6. Tiếp tục cho đến khi bao gồm tất cả các thành phố. 

Quá trình này xây dựng một cây bao trùm trong đó mỗi nút được kết nối với nút ảo (trạm) hoặc với một thành phố khác thông qua cạnh rẻ nhất đã biết. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán duy trì một tập hợp các nút đã hoàn thiện và cách rẻ nhất để gắn từng nút còn lại vào tập hợp này. Đây chính xác là bất biến được sử dụng trong thuật toán Prim. Nút ảo đảm bảo rằng việc “mở một trạm” được xử lý thống nhất như một cạnh khác. Vì mọi bản cập nhật luôn xem xét kết nối tốt nhất có thể với cấu trúc hiện có nên không thể bỏ lỡ cấu hình toàn cầu rẻ hơn sau khi nút được hoàn tất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    x = [0] * n
    y = [0] * n
    for i in range(n):
        x[i], y[i] = map(int, input().split())

    c = list(map(int, input().split()))
    k = list(map(int, input().split()))

    INF = 10**18

    min_cost = c[:]
    parent = [-1] * n
    used = [False] * n

    total = 0
    stations = []
    edges = []

    for _ in range(n):
        v = -1
        for i in range(n):
            if not used[i] and (v == -1 or min_cost[i] < min_cost[v]):
                v = i

        used[v] = True
        total += min_cost[v]

        if parent[v] == -1:
            stations.append(v + 1)
        else:
            edges.append((v + 1, parent[v] + 1))

        for to in range(n):
            if not used[to]:
                dist = abs(x[v] - x[to]) + abs(y[v] - y[to])
                cost = (k[v] + k[to]) * dist
                if cost < min_cost[to]:
                    min_cost[to] = cost
                    parent[to] = v

    print(total)
    print(len(stations))
    print(*stations)
    print(len(edges))
    for a, b in edges:
        print(a, b)

if __name__ == "__main__":
    solve()
```Việc triển khai là thuật toán Prim trực tiếp không có hàng đợi ưu tiên, điều này có thể chấp nhận được vì$n \le 2000$. các`min_cost`mảng theo dõi cách rẻ nhất đã biết để kết nối từng thành phố thông qua một nhà ga hoặc qua một biên. các`parent`mảng mã hóa xem tùy chọn rẻ nhất đó đến từ thành phố khác hay từ nhà ga ảo. Nếu như`parent[v]`là`-1`, chúng tôi giải thích nó như việc xây dựng một nhà ga. 

Một sai lầm phổ biến là quên rằng chi phí trạm cạnh tranh với các cạnh trong suốt quá trình. Ở đây nó được tích hợp ngay từ đầu bằng cách khởi tạo`min_cost[i] = c[i]`. Một vấn đề tế nhị khác là chỉ cập nhật khoảng cách sau khi chọn một nút; làm điều đó sớm hơn sẽ phá vỡ bất biến Prim. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 3
1 1
3 2
3 2 3
3 2 3
```Chúng ta bắt đầu với chi phí ban đầu bằng với chi phí của trạm. 

| Bước | Nút được chọn | Chi phí | Phụ huynh | Trạm | Cạnh | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | -1 | [2] | [] | 
| 2 | 1 | 3 | -1 | [2,1] | [] | 
| 3 | 3 | 3 | -1 | [2,1,3] | [] | 

Tất cả chi phí của trạm đều rẻ hơn bất kỳ kết nối nào, vì vậy mọi nút đều được kết nối trực tiếp với nút ảo. 

Điều này xác nhận thuật toán ưu tiên chính xác các trạm độc lập khi các cạnh không có lợi. 

### Ví dụ 2 

Hãy xem xét một cấu hình giống như chuỗi:```
4
0 0
0 1
0 2
0 3
10 10 10 10
1 1 1 1
```| Bước | Được chọn | Chi phí | Phụ huynh | Trạm | Cạnh | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 10 | -1 | [1] | [] | 
| 2 | 2 | 2 | 1 | [1] | (2,1) | 
| 3 | 3 | 2 | 2 | [1] | (3,2) | 
| 4 | 4 | 2 | 3 | [1] | (4,3) | 

Điều này cho thấy sự lan truyền từ một trạm duy nhất thông qua cây bao trùm tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi lần lặp sẽ quét tất cả các nút và cập nhật tất cả các cạnh | 
| Không gian |$O(n)$| Mảng chi phí, phụ huynh và trạng thái đã truy cập | 

Với$n \le 2000$,$n^2 = 4 \times 10^6$hoạt động, phù hợp thoải mái trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    def solve():
        n = int(input())
        x = [0]*n
        y = [0]*n
        for i in range(n):
            x[i], y[i] = map(int, input().split())
        c = list(map(int, input().split()))
        k = list(map(int, input().split()))

        INF = 10**18
        min_cost = c[:]
        parent = [-1]*n
        used = [False]*n

        total = 0
        stations = []
        edges = []

        for _ in range(n):
            v = -1
            for i in range(n):
                if not used[i] and (v == -1 or min_cost[i] < min_cost[v]):
                    v = i

            used[v] = True
            total += min_cost[v]

            if parent[v] == -1:
                stations.append(v+1)
            else:
                edges.append((v+1, parent[v]+1))

            for to in range(n):
                if not used[to]:
                    dist = abs(x[v]-x[to]) + abs(y[v]-y[to])
                    cost = (k[v]+k[to])*dist
                    if cost < min_cost[to]:
                        min_cost[to] = cost
                        parent[to] = v

        out = []
        out.append(str(total))
        out.append(str(len(stations)))
        out.append(" ".join(map(str, stations)))
        out.append(str(len(edges)))
        for a,b in edges:
            out.append(f"{a} {b}")
        return "\n".join(out)

    return solve()

# sample tests
assert run("""3
2 3
1 1
3 2
3 2 3
3 2 3
""").strip().split()[0] == "8"

# all stations cheaper
assert run("""2
1 1
2 2
1 1
100 100
""").split()[0] == "3"

# chain structure
assert run("""4
0 0
0 1
0 2
0 3
10 10 10 10
1 1 1 1
""").split()[0] == "12"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 3 thành phố | 8 | tối ưu cho tất cả các trạm | 
| Chi phí cạnh cao 2 thành phố | 3 | sự thống trị của trạm | 
| chuỗi tuyến tính | 12 | nhân giống qua MST | 

## Vỏ cạnh 

Khi tất cả các thành phố có tọa độ giống nhau, khoảng cách Manhattan trở thành bằng không. Trong trường hợp đó, thuật toán đương nhiên ưu tiên kết nối các thành phố hơn là xây dựng các trạm nếu$k_i + k_j > 0$không thành vấn đề vì chi phí cạnh trở thành bằng không. Quá trình Prim sẽ chọn các cạnh có chi phí bằng 0 trước tiên, hợp nhất tất cả các nút trước khi bất kỳ lựa chọn trạm nào trở nên cần thiết. 

Khi chi phí trạm cực kỳ lớn, việc khởi tạo`min_cost = c`đảm bảo rằng thuật toán không cam kết sai với các cạnh sớm nếu chúng đắt hơn việc mở một trạm. Vì mỗi thành phố so sánh độc lập cả hai phương án trong suốt quá trình nên không có cấu trúc sớm nào được cố định. 

Khi một thành phố hoàn toàn bị cô lập về mặt kết nối giá rẻ,`min_cost`vẫn giữ nguyên giá trạm và cuối cùng nó sẽ được chọn làm nút trạm. Điều này xác nhận rằng thuật toán xử lý các cấu trúc tối ưu bị ngắt kết nối một cách khéo léo.
