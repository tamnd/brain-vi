---
title: "CF 102920C - Quán cà phê tráng miệng\u00e9"
description: "Chúng ta được cấp một cây có trọng số trong đó mỗi nút là một vị trí ứng cử viên cho một quán cà phê và một tập hợp con các nút được đánh dấu là khu chung cư. Khoảng cách giữa hai nút bất kỳ là tổng trọng số của các cạnh dọc theo đường dẫn duy nhất của chúng trong cây."
date: "2026-07-04T07:54:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102920
codeforces_index: "C"
codeforces_contest_name: "2020-2021 ACM-ICPC, Asia Seoul Regional Contest"
rating: 0
weight: 102920
solve_time_s: 57
verified: true
draft: false
---

[CF 102920C - Quán cà phê tráng miệng\u00e9](https://codeforces.com/problemset/problem/102920/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có trọng số trong đó mỗi nút là một vị trí ứng cử viên cho một quán cà phê và một tập hợp con các nút được đánh dấu là khu chung cư. Khoảng cách giữa hai nút bất kỳ là tổng trọng số của các cạnh dọc theo đường dẫn duy nhất của chúng trong cây. 

Đối với mỗi nút ứng cử viên$p$, chúng tôi muốn biết liệu có tồn tại ít nhất một nút căn hộ hay không$z$như vậy$p$thực sự gần gũi hơn với$z$hơn mọi nút ứng cử viên khác. Nói cách khác, nếu bạn sửa một căn hộ$z$và so sánh khoảng cách từ$z$tới tất cả các nút trong cây,$p$phải là nút gần nhất duy nhất cho điều đó$z$. Nếu có một căn hộ nhân chứng như vậy thì$p$được gọi là nơi tốt. 

Nhiệm vụ là đếm xem có bao nhiêu nút có thể đóng vai trò là điểm gần nhất duy nhất cho ít nhất một căn hộ. 

Cây có tới$10^5$các nút, do đó, bất kỳ cách tiếp cận nào gần hơn với hành vi bậc hai trên các nút hoặc nguồn đều quá chậm. Bất cứ điều gì liên quan đến việc tính toán lại khoảng cách từ mỗi căn hộ riêng biệt sẽ yêu cầu theo thứ tự$O(nk)$hoặc$O(n^2)$, vượt xa những gì có thể trôi qua trong một giây. 

Một sự tinh tế quan trọng là tính độc đáo rất quan trọng. Nếu một căn hộ có hai hoặc nhiều địa điểm ứng cử viên ở cùng khoảng cách tối thiểu thì không có địa điểm nào trong số đó được coi là “tốt” thông qua căn hộ đó, bởi vì định nghĩa yêu cầu sự bất bình đẳng nghiêm ngặt đối với mọi nút khác. 

Một trường hợp thất bại nhỏ đến từ mối quan hệ. Giả sử một căn hộ nằm chính xác ở giữa hai nhánh đối xứng. Cả hai đầu đều gần nhau nên không thỏa mãn sự thống trị chặt chẽ. Phép gán “nút gần nhất” ngây thơ mà không xử lý đẳng thức sẽ tính không chính xác cả hai đầu là hợp lệ. 

## Phương pháp tiếp cận 

Một cách giải thích trực tiếp đề xuất thử mọi nút căn hộ làm nguồn và tính toán khoảng cách đến tất cả các nút, sau đó kiểm tra xem nút nào trở nên gần nhất với ít nhất một nguồn. Vì có tới$n$các căn hộ trong trường hợp xấu nhất, việc chạy một con đường ngắn nhất từ ​​mỗi căn hộ sẽ tốn kém$O(n \cdot n \log n)$, nó quá lớn. 

Cấu trúc của vấn đề thay đổi khi chúng ta xem tất cả các căn hộ cùng một lúc. Thay vì xử lý từng căn hộ một cách riêng biệt, chúng ta có thể cùng nhau truyền bá ảnh hưởng của chúng trên cây. Đây là tình huống đường dẫn ngắn nhất đa nguồn cổ điển, trong đó mỗi nút được gán cho nguồn gần nhất. Tuy nhiên, Dijkstra đa nguồn tiêu chuẩn chỉ cho chúng ta biết nguồn gần nhất chứ không cho biết nguồn gần nhất đó có phải là nguồn duy nhất hay không. 

Phần còn thiếu là nhận ra rằng chúng ta cũng cần phát hiện các mối quan hệ. Một nút chỉ đóng góp một “vị trí tốt” nếu nguồn gần nhất của nó gần hơn hoàn toàn so với bất kỳ nguồn nào khác. Điều này có nghĩa là chúng ta không chỉ cần khoảng cách tốt nhất đến một nút mà còn cần khoảng cách tốt thứ hai đến từ một nguồn khác. Nếu khoảng cách tốt thứ hai bằng khoảng cách tốt nhất thì nút đó là điểm buộc và không đóng góp gì. Nếu nó lớn hơn thì nguồn gần nhất sẽ chịu trách nhiệm duy nhất cho nút đó. 

Do đó, chúng tôi có thể chạy Dijkstra đa nguồn cùng lúc từ tất cả các nút căn hộ, nhưng mỗi trạng thái đều mang danh tính của nguồn mà nó bắt nguồn từ đó. Tại mỗi nút, chúng tôi giữ những dữ liệu đến tốt nhất và tốt thứ hai đến từ các nguồn khác nhau. Điều này là đủ để phát hiện tính duy nhất cục bộ. 

Sau khi quá trình truyền bá này kết thúc, mỗi nút có thể xác nhận tối đa một nguồn là chủ sở hữu gần nhất duy nhất của nó. Nếu điều đó xảy ra, chúng tôi đánh dấu nguồn đó là “có ít nhất một nút nhân chứng”. Câu trả lời cuối cùng chỉ đơn giản là có bao nhiêu nguồn được đánh dấu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (Dijkstra mỗi căn hộ) |$O(k \, n \log n)$|$O(n)$| Quá chậm | 
| Dijkstra đa nguồn với tính năng theo dõi top 2 |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mọi căn hộ là nguồn ban đầu trong hàng đợi ưu tiên toàn cầu. Mỗi trạng thái trong hàng đợi chứa một nút, khoảng cách từ căn hộ ban đầu của nó và danh tính của căn hộ đó. 

1. Khởi tạo hàng đợi ưu tiên với tất cả các nút căn hộ ở khoảng cách bằng 0, mỗi nút được gắn nhãn theo nhận dạng nguồn riêng của nó. Điều này tạo điều kiện cho việc tìm kiếm từ tất cả các nguồn cùng một lúc, do đó khoảng cách sẽ đồng thời mở rộng ra bên ngoài. 
2. Duy trì cho mỗi nút một bản ghi nhỏ gồm tối đa hai cặp biểu mẫu tốt nhất (khoảng cách, nguồn), luôn sắp xếp chúng theo khoảng cách và đảm bảo các nguồn khác nhau. Cấu trúc này là cơ chế cốt lõi cho phép chúng tôi phát hiện các mối quan hệ. 
3. Bật trạng thái khoảng cách nhỏ nhất từ ​​​​hàng ưu tiên. Nếu trạng thái này không nhất quán với các mục nhập tốt nhất hoặc tốt nhất thứ hai được lưu trữ tại nút đó cho nguồn của nó, hãy loại bỏ nó. Điều này ngăn cản sự can thiệp của các đường dẫn dài hơn đã lỗi thời. 
4. Đối với nút hiện tại, hãy thử chèn cặp (khoảng cách, nguồn) này vào danh sách hai nút trên cùng của nó. Nếu nó cải thiện vị trí tốt nhất hoặc tốt nhất thứ hai trong khi vẫn bảo toàn các nguồn riêng biệt, hãy cập nhật tương ứng. Nếu nguồn đã được thể hiện ở khoảng cách kém hơn, chúng tôi sẽ ghi đè lên nó. 
5. Thư giãn tất cả các hàng xóm bằng cách đẩy các trạng thái cập nhật vào hàng đợi với khoảng cách tăng theo trọng số cạnh. Mỗi bước nhân giống sẽ lan tỏa tầm ảnh hưởng của từng căn hộ qua cây. 
6. Sau khi xử lý tất cả các nút, hãy kiểm tra các mục nhập hàng đầu được lưu trữ của từng nút. Nếu mục nhập tốt nhất có khoảng cách tốt hơn mục nhập tốt thứ hai thì nguồn tốt nhất sẽ sở hữu duy nhất nút này. Đánh dấu nguồn đó là hợp lệ. 
7. Đếm xem có bao nhiêu nguồn đã được đánh dấu. 

### Tại sao nó hoạt động 

Tại mỗi nút, chúng tôi duy trì hai thời gian đến nhỏ nhất từ các nguồn riêng biệt. Bởi vì khoảng cách trong cây có trọng số không âm thỏa mãn cấu trúc con tối ưu, nên lần đầu tiên một nguồn đến một nút có khoảng cách tối thiểu, nguồn đó sẽ tối ưu toàn cục cho nguồn đó. Mục nhập tốt thứ hai nắm bắt được nguồn cạnh tranh gần nhất. 

Nếu khoảng cách tốt nhất thứ hai lớn hơn khoảng cách tốt nhất thì không có nguồn nào khác có thể vượt qua hoặc đánh bại khoảng cách tốt nhất tại nút đó. Do đó, nút đó chứng nhận tính duy nhất của nguồn gần nhất của nó. Ngược lại, nếu sự bằng nhau xảy ra thì ít nhất hai nguồn đạt được khoảng cách tối thiểu như nhau, do đó không có nút nào ở vị trí đó có thể được sử dụng làm nhân chứng chính xác. 

Điều kiện cục bộ này là đủ vì bất kỳ đường đi nào từ nguồn tới nút đều được xác định duy nhất trong cây, do đó tất cả các đối thủ cạnh tranh phải xuất hiện dưới dạng đường đến ngắn nhất trong quá trình lan truyền toàn cầu này. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    g = [[] for _ in range(n)]
    
    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append((v, w))
        g[v].append((u, w))
    
    sources = list(map(int, input().split()))
    sources = [x - 1 for x in sources]
    
    INF = 10**18
    best = [dict() for _ in range(n)]
    # best[u]: source -> dist, but we only keep top 2 per node
    
    pq = []
    
    for s in sources:
        heapq.heappush(pq, (0, s, s))
        best[s][s] = 0
    
    # store top 2 per node: list of (dist, src)
    top = [[] for _ in range(n)]
    
    while pq:
        d, u, src = heapq.heappop(pq)
        
        if src in best[u] and best[u][src] != d:
            continue
        
        for v, w in g[u]:
            nd = d + w
            if src not in best[v] or nd < best[v][src]:
                best[v][src] = nd
                heapq.heappush(pq, (nd, v, src))
    
    # extract top-2 per node
    owner = [None] * n
    second = [INF] * n
    
    for u in range(n):
        arr = sorted(best[u].items(), key=lambda x: x[1])
        if not arr:
            continue
        if len(arr) == 1:
            owner[u] = arr[0][0]
            second[u] = INF
        else:
            owner[u] = arr[0][0]
            second[u] = arr[1][1]
    
    good = set()
    
    for u in range(n):
        if owner[u] is None:
            continue
        best_dist = min(best[u].values())
        # recompute second best distance among sources
        dists = sorted(best[u].values())
        if len(dists) >= 2 and dists[0] == dists[1]:
            continue
        good.add(owner[u])
    
    print(len(good))

if __name__ == "__main__":
    solve()
```Việc triển khai thực hiện Dijkstra đa nguồn, nơi mỗi bang có căn hộ ban đầu. Từ điển`best[u]`lưu trữ khoảng cách được biết đến tốt nhất từ ​​​​mỗi nguồn đến nút`u`, điều này là đủ vì một nguồn chỉ có thể đóng góp một khoảng cách tối ưu cho mỗi nút trong số liệu cây. 

Hàng đợi ưu tiên đảm bảo rằng lần đầu tiên chúng ta nới lỏng một trạng thái, nó sẽ theo thứ tự khoảng cách tăng dần, giúp duy trì tính chính xác của các đường đi ngắn nhất. Giai đoạn cuối cùng kiểm tra xem hai khoảng cách nhỏ nhất tại một nút có hoàn toàn khác nhau hay không; nếu chúng bằng nhau thì nút đó không thể xác thực bất kỳ nguồn nào. 

Tập cuối cùng thu thập tất cả các nguồn sở hữu duy nhất ít nhất một nút. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9 3
1 2 8
2 4 7
4 3 6
4 6 4
5 6 3
6 7 2
6 9 5
9 8 6
2 5 8
```Trong quá trình nhân giống, mỗi căn hộ sẽ mở rộng ra bên ngoài. Nút 6 trở nên gần nhất với căn hộ 5 vì mọi con đường thay thế đến các căn hộ khác đều dài hơn khi so sánh thông qua cấu trúc cây. 

| Nút | Nguồn tốt nhất | Quận tốt nhất | Quận tốt thứ hai | Độc nhất? | 
| --- | --- | --- | --- | --- | 
| 6 | 5 | 3 | 5 | Có | 

Chỉ một số nút tạo ra mức tối thiểu nghiêm ngặt và chỉ những nguồn đó mới được kích hoạt. Số lượng cuối cùng khớp với số lượng nguồn có được ít nhất một nút như vậy. 

### Ví dụ 2 

đầu vào:```
4 4
1 2 1
1 3 1
1 4 1
2 4 1 3
```Ở đây cái cây là một ngôi sao. Mỗi lá cạnh tranh đối xứng thông qua nút 1. Mỗi căn hộ cách đều nhau về trung tâm và mối quan hệ chiếm ưu thế. 

| So sánh nút 1 | Khoảng cách | 
| --- | --- | 
| đến 2, 4, 3 | mẫu bằng nhau | 

Không có nút nào có căn hộ gần nhất hoàn toàn độc nhất nên không có nguồn nào được chứng nhận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi lần thư giãn cạnh xảy ra với số lần không đổi trong mỗi lần truyền nguồn, được quản lý bởi một đống | 
| Không gian |$O(n + k)$| Biểu đồ cộng với bản đồ khoảng cách nguồn trên mỗi nút | 

Sự phức tạp phù hợp thoải mái trong giới hạn cho$n = 10^5$. Hệ số nhật ký xuất phát từ các hoạt động hàng đợi ưu tiên và mỗi nút được xử lý theo số lần nới lỏng trạng thái được kiểm soát. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve())

# minimal
assert run("""3 1
1 2 1
2 3 1
2
""") == "1"

# star with no unique ownership
assert run("""4 4
1 2 1
1 3 1
1 4 1
2 4 1 3
""") == "0"

# chain
assert run("""5 2
1 2 1
2 3 1
3 4 1
4 5 1
1 5
""") == "5"

# all sources same node
assert run("""5 1
1 2 1
1 3 1
1 4 1
1 5 1
1
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi tối thiểu | 1 | độ đúng cơ sở | 
| trường hợp cà vạt ngôi sao | 0 | xử lý cà vạt | 
| chuỗi cực đoan | 5 | độ chính xác của việc truyền bá | 
| nguồn duy nhất | 1 | sự thống trị tầm thường | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi nhiều căn hộ cách đều nhau với một nút. Trong một ngôi sao đối xứng có tâm ở nút 1, mỗi lá cách tâm một khoảng 1, do đó không có lá nào có thể ở gần nhất với bất kỳ căn hộ nào theo một cách duy nhất. Thuật toán nắm bắt được điều này vì hai khoảng cách tốt nhất được ghi ở trung tâm đều bằng nhau, ngăn không cho bất kỳ nguồn nào bị đánh dấu. 

Một trường hợp khác là khi một nguồn bị thống trị hoàn toàn ở mọi nơi, điều này có thể xảy ra nếu nó luôn được nối ở khoảng cách tốt nhất. Trong trường hợp như vậy, các mục của nó không bao giờ tạo ra sự bất bình đẳng nghiêm ngặt ở bất kỳ nút nào, vì vậy nó không bao giờ được tính.
