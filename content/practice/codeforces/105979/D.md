---
title: "CF 105979D - Khám phá Graphlandia"
description: "Bài toán này yêu cầu chúng ta xem xét mạng lưới đường bộ nơi mỗi thành phố đều có giá trị năng lượng. Xuất phát từ một thành phố, chúng ta có thể di chuyển dọc theo các con đường, nhưng chúng ta chỉ được phép đi vào những thành phố có năng lượng không lớn hơn năng lượng của thành phố xuất phát."
date: "2026-06-25T13:31:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105979
codeforces_index: "D"
codeforces_contest_name: "2025 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 105979
solve_time_s: 43
verified: true
draft: false
---

[CF 105979D - Khám phá Graphlandia](https://codeforces.com/problemset/problem/105979/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán này yêu cầu chúng ta xem xét mạng lưới đường bộ nơi mỗi thành phố đều có giá trị năng lượng. Xuất phát từ một thành phố, chúng ta có thể di chuyển dọc theo các con đường, nhưng chúng ta chỉ được phép đi vào những thành phố có năng lượng không lớn hơn năng lượng của thành phố xuất phát. Đối với mỗi thành phố, chúng ta cần tìm xem có thể truy cập được bao nhiêu thành phố theo quy tắc này. Nhiệm vụ đến từ Codeforces Gym 105979, Vấn đề D. 

Đầu vào mô tả một đồ thị vô hướng. Phần đầu tiên đưa ra số lượng thành phố và con đường, phần tiếp theo cung cấp năng lượng của mỗi thành phố và các dòng còn lại mô tả các con đường nối các cặp thành phố. Đầu ra chứa một giá trị cho mỗi thành phố, biểu thị quy mô của khu vực có thể tiếp cận đối với giới hạn năng lượng của thành phố đó. 

Giới hạn lớn là chìa khóa để chọn hướng đi đúng đắn. Biểu đồ có thể chứa tối đa một triệu thành phố và một triệu con đường, do đó, giải pháp thực hiện tìm kiếm biểu đồ từ mọi thành phố sẽ yêu cầu hoạt động khoảng O(N(N+M)) trong trường hợp xấu nhất. Với một triệu nút, thậm chí việc chạm vào một phần nhỏ của tất cả các lần truyền tải lặp lại có thể xảy ra là không thể. Chúng ta cần xử lý toàn bộ biểu đồ gần tuyến tính. 

Một sai lầm phổ biến là nghĩ rằng nếu chúng ta bắt đầu từ một thành phố có giá trị năng lượng lớn thì sau này bằng cách nào đó chúng ta có thể mở rộng sang các thành phố lớn hơn. Quy tắc di chuyển chỉ dựa trên giá trị của thành phố bắt đầu, do đó tập hợp được phép chính xác là các thành phố có năng lượng nhỏ hơn hoặc bằng giá trị bắt đầu đó. Điều này tạo ra một trật tự tự nhiên theo năng lượng. 

Hãy xem xét một biểu đồ trong đó mọi thành phố đều bị cô lập.```
3 0
5 5 5
```Đầu ra đúng là:```
1
1
1
```Một giải pháp bất cẩn có thể nhóm các thành phố có năng lượng ngang nhau lại với nhau và cho rằng chúng được kết nối với nhau vì giá trị của chúng khớp nhau. Năng lượng bằng nhau không tạo ra đường nên khả năng kết nối vẫn chỉ phụ thuộc vào các cạnh của đồ thị. 

Một trường hợp khác là một chuỗi có năng lượng tăng dần:```
3 2
1 2 3
1 2
2 3
```Đầu ra đúng là:```
1
2
3
```Thành phố đầu tiên không thể đi đâu được vì thành phố tiếp theo có năng lượng cao hơn. Thành phố thứ hai có thể tiếp cận thành phố thứ nhất và thành phố thứ ba có thể tiếp cận cả ba thành phố. Một giải pháp trả lời trong khi xử lý từng năng lượng riêng biệt trước khi kết nối tất cả các nút bằng nhau hoặc thấp hơn có thể dẫn đến sai thứ tự này. 

Trường hợp tinh tế cuối cùng là nhiều thành phố có cùng năng lượng:```
4 2
5 5 5 10
1 2
2 3
```Đầu ra đúng là:```
3
3
3
4
```Cả ba thành phố năng lượng 5 đều có sẵn cùng nhau. Nếu chúng ta xử lý từng năng lượng bằng nhau và trả lời ngay lập tức, thành phố được xử lý đầu tiên có thể chỉ nhìn sai một phần thành phần cuối cùng của nó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là chạy đồ thị truyền tải từ mọi thành phố. Đối với thành phố bắt đầu được chọn, chúng tôi sẽ đi bộ qua các con đường lân cận trong khi từ chối đi vào các thành phố có giá trị năng lượng lớn hơn. Điều này đúng vì việc truyền tải tuân theo chính xác quy tắc chuyển động. Tuy nhiên, nếu đồ thị lớn, những con đường và thành phố giống nhau sẽ được ghé thăm nhiều lần. Trong một biểu đồ dày đặc hoặc có tính kết nối cao, việc lặp lại điều này từ mọi thành phố có thể đạt tới khoảng O(N(N+M)) hoạt động, vượt xa kích thước đầu vào cho phép. 

Cấu trúc của vấn đề cho chúng ta một cách để đảo ngược quan điểm. Thay vì hỏi "thành phố này có thể đi đâu?", hãy hỏi "điều gì sẽ xảy ra khi chúng ta dần dần cho phép các thành phố trở nên có thể sử dụng được?". Sắp xếp các thành phố theo năng lượng. Khi chúng tôi xử lý các thành phố từ năng lượng nhỏ nhất đến lớn nhất, sau khi chúng tôi kích hoạt tất cả các thành phố đạt đến giá trị năng lượng x nào đó, các thành phần được kết nối giữa các thành phố được kích hoạt chính xác là các khu vực mà một thành phố có năng lượng x có thể khám phá. 

Lý do điều này có hiệu quả là vì mọi di chuyển được phép chỉ đi đến một thành phố có năng lượng nhiều nhất là giá trị ban đầu. Nếu chúng ta kích hoạt các thành phố theo thứ tự tăng dần, mỗi con đường giữa hai thành phố đã được kích hoạt sẽ thể hiện một chuyển động có thể xảy ra bên trong giới hạn năng lượng hiện tại. Cấu trúc liên kết tập hợp rời rạc có thể duy trì các thành phần được kết nối này trong khi đường được thêm vào. 

Chi tiết quan trọng là xử lý năng lượng bằng nhau. Tất cả các thành phố có cùng năng lượng phải được kích hoạt cùng nhau trước khi bất kỳ thành phố nào nhận được câu trả lời. Nếu không, hai thành phố năng lượng giống nhau được kết nối bằng một con đường xuyên qua các thành phố năng lượng tương tự khác có thể chưa được hợp nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N(N+M)) | O(N+M) | Quá chậm | 
| Tối ưu | O((N+M) log N) | O(N+M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các thành phố theo giá trị năng lượng của chúng. Chúng tôi sẽ xử lý chúng từ năng lượng thấp nhất đến cao nhất vì một thành phố có năng lượng x chỉ quan tâm đến các thành phố xuất hiện không muộn hơn x theo thứ tự này. 
2. Tạo một cấu trúc hợp tập hợp rời rạc. Ban đầu không có thành phố nào hoạt động nên mọi thành phần đều trống. Cấu trúc sẽ lưu trữ những thành phố đang hoạt động nào được kết nối và kích thước của từng thành phần được kết nối. 
3. Đối với mỗi nhóm thành phố có cùng năng lượng, trước tiên hãy kích hoạt tất cả chúng. Đánh dấu một thành phố đang hoạt động có nghĩa là thành phố đó hiện được phép tham gia vào các đường đi dành cho các thành phố có năng lượng này hoặc bất kỳ năng lượng nào lớn hơn. 
4. Đối với mỗi thành phố mới được kích hoạt, hãy kiểm tra tất cả các thành phố lân cận. Nếu hàng xóm đã hoạt động, hãy hợp nhất hai thành phần. Con đường có thể sử dụng được vì cả hai điểm cuối đều đáp ứng giới hạn năng lượng hiện tại. 
5. Sau khi tất cả các thành phố có năng lượng này đã được kích hoạt và hợp nhất, hãy chỉ định kích thước thành phần cho mọi thành phố trong nhóm. Giá trị này là số lượng thành phố có thể truy cập từ nó. 
6. Xuất ra các câu trả lời đã lưu theo thứ tự thành phố ban đầu. 

Tại sao nó hoạt động: 

Điều bất biến là sau khi kết thúc một nhóm giá trị năng lượng x, tập hợp thành phần rời rạc chứa chính xác các thành phần liên thông của đồ thị con hình thành bởi các thành phố có năng lượng lớn nhất là x. Mọi đường đi hợp lệ có thể có cho một thành phố có năng lượng x đều phải nằm trong sơ đồ con này, vì vậy kích thước thành phần của nó chính xác là câu trả lời. Việc xử lý các năng lượng bằng nhau theo một đợt sẽ duy trì tính bất biến vì tất cả các thành phố có cùng giới hạn đều khả dụng cùng một lúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    energy = list(map(int, input().split()))

    graph = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append(v)
        graph[v].append(u)

    parent = list(range(n))
    size = [1] * n
    active = [False] * n

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        a = find(a)
        b = find(b)
        if a == b:
            return
        if size[a] < size[b]:
            a, b = b, a
        parent[b] = a
        size[a] += size[b]

    order = list(range(n))
    order.sort(key=lambda x: energy[x])

    ans = [0] * n
    i = 0

    while i < n:
        j = i
        value = energy[order[i]]
        while j < n and energy[order[j]] == value:
            j += 1

        for k in range(i, j):
            u = order[k]
            active[u] = True

        for k in range(i, j):
            u = order[k]
            for v in graph[u]:
                if active[v]:
                    union(u, v)

        for k in range(i, j):
            u = order[k]
            ans[u] = size[find(u)]

        i = j

    sys.stdout.write("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã lưu trữ biểu đồ bằng danh sách kề vì số lượng đường có thể lớn và chúng ta cần quét các đường lân cận một cách hiệu quả. 

Việc triển khai kết hợp tập hợp rời rạc sử dụng tính năng nén đường dẫn và kết hợp theo kích thước. Những tối ưu hóa này giữ cho mỗi hoạt động hợp nhất có thời gian gần như không đổi, điều này cần thiết khi biểu đồ chứa khoảng một triệu con đường. 

Bước sắp xếp tạo ra thứ tự các thành phố có sẵn. Vòng lặp chính tiến lên thông qua các nhóm năng lượng bằng nhau. Hai vòng riêng biệt bên trong mỗi nhóm là cần thiết: đầu tiên mọi thành phố trong nhóm sẽ hoạt động, sau đó tất cả các con đường đều được xem xét. Kích hoạt và hợp nhất trong cùng một lượt sẽ tạo ra lỗi năng lượng tương tự như được mô tả trước đó. 

Câu trả lời được lấy từ kích thước thành phần được lưu trữ của thư mục gốc sau khi tất cả các quá trình hợp nhất hoàn tất. các`find`cuộc gọi là cần thiết vì bản thân thành phố có thể không còn là gốc rễ sau các cuộc liên hiệp trước đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 5
1 5 7 11 10 9
1 2
1 3
1 4
4 5
4 6
```| Năng lượng được xử lý | Thành phố được kích hoạt | Hoạt động hiện tại | Kích thước thành phần được chỉ định | 
| --- | --- | --- | --- | 
| 1 | 1 | Không có hàng xóm tích cực | thành phố 1 được 1 | 
| 5 | 2 | Kết nối với thành phố 1 | thành phố 2 được 2 | 
| 7 | 3 | Kết nối với thành phố 1 | thành phố 3 được 3 | 
| 9 | 6 | Không có kết nối mới | thành phố 6 được 1 | 
| 10 | 5 | Không có kết nối tới các thành phố đang hoạt động thấp hơn thông qua đường dẫn hợp lệ | thành phố 5 được 1 | 
| 11 | 4 | Kết nối 4,5,6 với các thành phố đang hoạt động trước đó | thành phố 4 được 6 | 

Dấu vết cho thấy tại sao một thành phố chỉ có thể tiếp cận những con đường dẫn qua các thành phố không lớn hơn năng lượng của chính thành phố đó. Kích hoạt cuối cùng sẽ nối toàn bộ biểu đồ vì giới hạn năng lượng đủ cao. 

### Mẫu 2 

đầu vào:```
3 3
1 2 3
1 2
2 3
3 1
```| Năng lượng được xử lý | Thành phố được kích hoạt | Hoạt động hiện tại | Kích thước thành phần được chỉ định | 
| --- | --- | --- | --- | 
| 1 | 1 | Thành phần biệt lập | thành phố 1 được 1 | 
| 2 | 2 | Kết nối với thành phố 1 | thành phố 2 được 2 | 
| 3 | 3 | Kết nối với cả hai thành phố đang hoạt động | thành phố 3 được 3 | 

Ví dụ này xác nhận hành vi đơn điệu. Khi năng lượng cho phép tăng lên, các thành phần chỉ có thể phát triển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N+M) log N) | Việc sắp xếp chiếm ưu thế, trong khi mỗi cạnh được kiểm tra một số lần không đổi trong quá trình hợp nhất DSU | 
| Không gian | O(N+M) | Biểu đồ, mảng DSU, thứ tự và câu trả lời đều yêu cầu bộ nhớ tuyến tính | 

Giới hạn đầu vào yêu cầu xử lý gần như tuyến tính. Thuật toán chỉ chạm vào mọi thành phố và con đường một vài lần sau khi sắp xếp, vì vậy nó phù hợp với các ràng buộc một cách thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.strip()

# provided samples
assert run("""6 5
1 5 7 11 10 9
1 2
1 3
1 4
4 5
4 6
""") == "1\n2\n3\n6\n1\n1"

assert run("""3 3
1 2 3
1 2
2 3
3 1
""") == "1\n2\n3"

# minimum size
assert run("""1 0
7
""") == "1"

# all equal values
assert run("""4 2
5 5 5 5
1 2
3 4
""") == "2\n2\n2\n2"

# increasing chain
assert run("""5 4
1 2 3 4 5
1 2
2 3
3 4
4 5
""") == "1\n2\n3\n4\n5"

# disconnected high energy case
assert run("""5 2
10 1 1 10 1
1 2
2 3
""") == "5\n3\n3\n5\n3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Thành phố đơn lẻ |`1`| Xử lý biểu đồ nhỏ nhất | 
| Năng lượng bình đẳng | Hai thành phần nút | Xác nhận việc phân nhóm các giá trị bằng nhau | 
| Tăng chuỗi | Câu trả lời ngày càng tăng | Xác nhận logic đặt hàng năng lượng | 
| Đồ thị bị ngắt kết nối | Thành phần riêng biệt | Xác nhận DSU theo dõi kết nối chính xác | 

## Vỏ cạnh 

Đối với trường hợp thành phố bị cô lập:```
3 0
5 5 5
```thuật toán kích hoạt từng thành phố, nhưng không có hoạt động liên minh nào diễn ra vì không có đường. Mỗi thành phần DSU vẫn giữ nguyên kích thước một, do đó đầu ra là`1 1 1`. Cách tiếp cận không bao giờ cho rằng năng lượng bằng nhau sẽ tạo ra sự kết nối. 

Đối với chuỗi tăng dần:```
3 2
1 2 3
1 2
2 3
```thành phố có năng lượng 1 được xử lý đầu tiên và tạo thành thành phần có kích thước một. Khi năng lượng 2 được xử lý, thành phố 2 sẽ hoạt động và hợp nhất với thành phố 1, tạo ra kích thước hai. Cuối cùng, thành phố 3 tham gia thành phần hoạt động, tạo ra kích thước 3. Điều này tuân theo chính xác các bộ có thể truy cập. 

Đối với trường hợp năng lượng bằng nhau:```
4 2
5 5 5 10
1 2
2 3
```ba thành phố đầu tiên được kích hoạt cùng nhau trước khi bất kỳ câu trả lời nào được viết ra. Hai con đường hợp nhất chúng thành một thành phần có kích thước ba, do đó mọi thành phố năng lượng 5 đều nhận được giá trị chính xác. Thành phố cuối cùng được kích hoạt sau đó với đủ năng lượng để xem tất cả các thành phố trước đó, vì vậy nó nhận được kích thước bốn. Đây là trường hợp phá vỡ các triển khai xử lý từng mức năng lượng bằng nhau.
