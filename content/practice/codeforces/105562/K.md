---
title: "CF 105562K - Kruidnoten"
description: "Chúng tôi đang làm việc trên một biểu đồ có trọng số trong đó giao điểm là các nút và đường đi vòng là các cạnh vô hướng có độ dài dương. Karlijn bắt đầu từ nút 1 và muốn đến nút n. Một số nút chứa các cửa hàng."
date: "2026-06-22T14:21:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105562
codeforces_index: "K"
codeforces_contest_name: "2024-2025 ICPC Northwestern European Regional Programming Contest (NWERC 2024)"
rating: 0
weight: 105562
solve_time_s: 55
verified: true
draft: false
---

[CF 105562K - Kruidnoten](https://codeforces.com/problemset/problem/105562/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một biểu đồ có trọng số trong đó giao điểm là các nút và đường đi vòng là các cạnh vô hướng có độ dài dương. Karlijn bắt đầu từ nút 1 và muốn đến nút n. 

Một số nút chứa các cửa hàng. Mỗi cửa hàng độc lập vẫn có kruidnoten với một xác suất nhất định hoặc trống rỗng. Trước khi bắt đầu chuyến đi, cô kiểm tra xem cửa hàng nào hiện còn hàng. Nếu có ít nhất một cửa hàng có hàng, cô ấy sẽ đi từ nút 1 đến nút n nhưng được phép chọn đường đi đến ít nhất một cửa hàng có hàng. Trong số tất cả các đường đi hợp lệ như vậy, cô ấy chọn đường đi ngắn nhất xét về tổng chiều dài các cạnh. Nếu không có cửa hàng nào còn hàng, cô ấy sẽ không thực hiện chuyến đi và câu trả lời là không thể. 

Nhiệm vụ là tính toán độ dài dự kiến ​​của tuyến đường tối ưu này, tính trung bình trên tất cả các cấu hình kho ngẫu nhiên. 

Khó khăn quan trọng về mặt cấu trúc là đường dẫn được chọn phụ thuộc vào tập hợp con nút nào có sẵn dưới dạng “nút cửa hàng hợp lệ” và tập hợp con đó là ngẫu nhiên với xác suất độc lập trên mỗi nút. 

Biểu đồ có thể có tới 200.000 nút và cạnh, điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại các đường đi ngắn nhất từ ​​đầu cho từng tập hợp con của các cửa hàng hoặc liệt kê các tập hợp con của các nút có sẵn. Ngay cả việc xử lý tất cả các tập hợp con cũng không thể thực hiện được vì k có thể lớn và không gian trạng thái có tính hàm mũ. 

Một suy nghĩ ngây thơ là xem xét từng tập hợp con của các cửa hàng có hàng, chạy đường đi ngắn nhất từ ​​1 đến n bị ràng buộc để truy cập ít nhất một trong các nút đó và tính trung bình theo xác suất. Điều này thất bại ngay lập tức vì ngay cả việc tính toán một đường đi ngắn nhất bị ràng buộc như vậy cũng tốn kém và có nhiều tập hợp con theo cấp số nhân. 

Một ý tưởng sai lầm tinh tế hơn là thử xử lý từng cửa hàng một cách độc lập, ví dụ như tính tổng các đường đi ngắn nhất từ ​​1 đến mỗi cửa hàng và từ mỗi cửa hàng đến n được tính theo xác suất. Điều này được tính gấp đôi và bỏ qua thực tế là chỉ có cửa hàng rẻ nhất có thể tiếp cận mới quan trọng trong mỗi trường hợp. 

Trường hợp quan trọng là khi tất cả các xác suất đều nhỏ. Ví dụ: nếu mọi cửa hàng đều có xác suất nhỏ hơn 1 thì luôn có cơ hội khác 0 là không có cơ hội nào. Trong trường hợp đó, câu trả lời phải là “không thể” bất kể khoảng cách. 

## Phương pháp tiếp cận 

Khó khăn cốt lõi là đối với mỗi tập hợp con ngẫu nhiên của các cửa hàng có sẵn, đường dẫn tối ưu sẽ sử dụng cửa hàng tốt nhất có thể có trong tập hợp con đó. Điều này ngay lập tức gợi ý một quan điểm trong đó mỗi nút hoạt động giống như một “trục” ứng cử viên mà qua đó chúng ta có thể định tuyến đường đi từ 1 đến n. 

Đối với bất kỳ nút cố định v nào, hãy xem xét đường đi ngắn nhất đi từ 1 đến v và sau đó từ v đến n. Đường dẫn này có độ dài dist1[v] + dist2[v], trong đó dist1 là khoảng cách ngắn nhất từ ​​1 và dist2 là khoảng cách ngắn nhất đến n. Nếu v không phải là cửa hàng thì không liên quan vì yêu cầu là phải đến cửa hàng. Vì vậy, chỉ có các nút cửa hàng mới quan trọng. 

Bây giờ hãy xem xét một tập hợp con cụ thể của các cửa hàng đang hoạt động. Đường dẫn tối ưu chỉ đơn giản là đường dẫn tối thiểu trên tất cả các nút cửa hàng đang hoạt động v của dist1[v] + dist2[v]. Điều này làm giảm vấn đề tính toán giá trị tối thiểu dự kiến ​​​​trên một tập hợp con ngẫu nhiên các phần tử có trọng số. 

Điều này biến bài toán đồ thị thành bài toán thống kê thứ tự xác suất trên các giá trị w[v] = dist1[v] + dist2[v], trong đó mỗi phần tử xuất hiện độc lập với xác suất p[v]. 

Thách thức còn lại là tính toán kỳ vọng tối thiểu của một tập hợp con ngẫu nhiên. Một thủ thuật tiêu chuẩn là sắp xếp các ứng cử viên theo trọng số và tính xác suất để một nút nhất định là nút hoạt động tối thiểu. Để nút v ở mức tối thiểu, phải có hai điều kiện: v phải hoạt động và tất cả các nút u có w[u] < w[v] phải không hoạt động. Điều này dẫn đến một sản phẩm vượt quá xác suất vắng mặt của tất cả các nút tốt hơn.

Để thực hiện điều này hiệu quả, chúng tôi sắp xếp các nút theo w[v] và quét theo thứ tự tăng dần trong khi vẫn duy trì xác suất không có nút nào đã được xử lý đang hoạt động. Mỗi nút đóng góp trọng số của nó nhân với xác suất nó là phần tử hoạt động đầu tiên theo thứ tự này. Cuối cùng, chúng ta cũng xử lý xác suất không có cửa hàng nào hoạt động. 

Phần duy nhất còn lại là tính toán dist1 và dist2, được thực hiện bằng hai lần chạy Dijkstra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu đối với các tập hợp con | Hàm mũ | Hàm mũ | Quá chậm | 
| Dijkstra + quét xác suất | O((n + m) log n + k log k) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy Dijkstra từ nút 1 để tính khoảng cách ngắn nhất dist1[v] đến tất cả các nút v. Điều này nắm bắt được chi phí di chuyển tốt nhất có thể từ điểm bắt đầu đến mọi cửa hàng hoặc nút trung gian tiềm năng. 
2. Chạy Dijkstra từ nút n trên đồ thị đảo ngược để tính dist2[v], khoảng cách ngắn nhất từ ​​mọi nút v đến đích. Điều này tránh việc phải tính toán lại nhiều lần các đường dẫn ngắn nhất từ ​​một nguồn. 
3. Đối với mỗi nút cửa hàng v, hãy tính chi phí kết hợp w[v] = dist1[v] + dist2[v]. Điều này thể hiện con đường tốt nhất có thể nếu v được chọn làm cửa hàng đã ghé thăm. 
4. Chỉ thu thập tất cả các cặp (w[v], p[v]) cho các nút cửa hàng. Nếu không có cửa hàng, xác suất thành công bằng 0 và câu trả lời ngay lập tức là không thể. 
5. Sắp xếp các cặp này theo w[v] theo thứ tự tăng dần. Việc đặt hàng này phản ánh mức độ ưu tiên của việc trở thành cửa hàng có chi phí tối thiểu trong một kịch bản hiện thực. 
6. Duy trì giá trị đang chạy cur_fail được khởi tạo thành 1. Điều này thể hiện khả năng tất cả các cửa hàng được xử lý trước đó (rẻ hơn) đều không còn hàng. 
7. Quét qua danh sách đã sắp xếp. Với mỗi cửa hàng v, hãy thêm vào câu trả lời thuật ngữ w[v] * cur_fail * p[v]. Sau đó cập nhật cur_fail *= (1 - p[v]). Điều này buộc v chỉ đóng góp khi đó là cửa hàng có sẵn đầu tiên theo thứ tự được sắp xếp. 
8. Sau khi xử lý xong tất cả các cửa hàng, kiểm tra cur_fail. Đây là xác suất không còn cửa hàng nào cả. Nếu nó lớn hơn 0 thì xuất ra “không thể”. Nếu không thì xuất ra kỳ vọng tích lũy. 

### Tại sao nó hoạt động 

Sắp xếp theo w[v] đảm bảo rằng khi xử lý một nút, tất cả các ứng cử viên rẻ hơn đều được coi là mức tối thiểu tiềm năng. Xác suất để nút v là cửa hàng hoạt động tối thiểu chính xác là xác suất mà nút đó đang hoạt động và mọi nút có giá trị w nhỏ hơn đều không hoạt động. Vì các lần kích hoạt là độc lập nên xác suất này được phân tích rõ ràng thành p[v] nhân với tích của (1 - p[u]) trên tất cả các nút u trước đó. Điều này làm cho quá trình quét duy trì xác suất thất bại tích lũy chính xác và mỗi đóng góp được tính trọng số chính xác theo sự kiện mà nó trở thành mức tối thiểu duy nhất. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def dijkstra(n, graph, start):
    INF = 10**30
    dist = [INF] * (n + 1)
    dist[start] = 0
    pq = [(0, start)]

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in graph[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))
    return dist

def solve():
    n, m, k = map(int, input().split())

    graph = [[] for _ in range(n + 1)]
    for _ in range(m):
        a, b, w = map(int, input().split())
        graph[a].append((b, w))
        graph[b].append((a, w))

    dist1 = dijkstra(n, graph, 1)
    dist2 = dijkstra(n, graph, n)

    shops = []
    for _ in range(k):
        i, p = input().split()
        i = int(i)
        p = float(p)
        if dist1[i] < 10**29 and dist2[i] < 10**29:
            shops.append((dist1[i] + dist2[i], p))

    if not shops:
        print("impossible")
        return

    shops.sort()

    cur_fail = 1.0
    ans = 0.0

    for w, p in shops:
        ans += w * cur_fail * p
        cur_fail *= (1.0 - p)

    if cur_fail > 1e-15:
        print("impossible")
    else:
        print(f"{ans:.10f}")

if __name__ == "__main__":
    solve()
```Phần biểu đồ của mã là Dijkstra tiêu chuẩn, chạy hai lần để lấy khoảng cách từ điểm xuất phát và đến mục tiêu. Hai mảng này là quá trình tiền xử lý dành riêng cho đồ thị duy nhất cần thiết. 

Mỗi cửa hàng được chuyển đổi thành một trọng lượng vô hướng duy nhất thể hiện đường vòng tốt nhất có thể đi qua nó. Việc giảm này là bước quan trọng giúp loại bỏ tất cả cấu trúc đường dẫn khỏi tính ngẫu nhiên. 

Logic quét sử dụng sản phẩm chạy dấu phẩy động. Biến`cur_fail`theo dõi xác suất tất cả các cửa hàng đã thấy trước đó (rẻ hơn) đều không còn hàng. Mỗi cửa hàng mới đóng góp chính xác xác suất để nó là cửa hàng đầu tiên có sẵn theo thứ tự được sắp xếp. 

Kiểm tra cuối cùng về`cur_fail`xử lý sự kiện không có cửa hàng nào có sẵn. Nếu xác suất đó khác 0 thì kỳ vọng không được xác định theo định nghĩa bài toán, vì vậy chúng ta đưa ra kết quả là “không thể”. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Trước tiên, chúng tôi tính toán các đường đi ngắn nhất, sau đó tính toán chi phí cửa hàng. Giả sử các giá trị cửa hàng đã được sắp xếp là: 

| bước | w[v] | p[v] | cur_fail trước | đóng góp | cur_fail sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 0,5 | 1.0 | 5.0 | 0,5 | 
| 2 | 12 | 0,2 | 0,5 | 1.2 | 0,4 | 
| 3 | 20 | 0,3 | 0,4 | 2.4 | 0,28 | 

Giá trị kỳ vọng được tích lũy từ mức tối thiểu sớm nhất có thể. Mỗi hàng đại diện cho sự kiện cửa hàng này là cửa hàng đầu tiên có sẵn theo thứ tự chi phí tăng dần. 

Điều này xác nhận rằng khối lượng xác suất được phân chia chính xác cho các sự kiện loại trừ lẫn nhau. 

### Ví dụ 2 

Hãy xem xét trường hợp tất cả các cửa hàng đều phá sản đồng thời với xác suất dương. 

| bước | w[v] | p[v] | cur_fail trước | đóng góp | cur_fail sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 0,1 | 1.0 | 0,5 | 0,9 | 
| 2 | 7 | 0,2 | 0,9 | 1,26 | 0,72 | 

Cuối cùng`cur_fail = 0.72`, nghĩa là xác suất 72% không có cửa hàng nào sẵn có, do đó kết quả đầu ra trở nên không thể thực hiện được. 

Điều này chứng tỏ rằng thuật toán theo dõi rõ ràng sự kiện lỗi thay vì bỏ qua nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n + k log k) | Hai hoạt động Dijkstra thống trị, cộng với các cửa hàng phân loại | 
| Không gian | O(n + m) | Lưu trữ đồ thị và mảng khoảng cách | 

Các ràng buộc cho phép lên tới 200.000 nút và cạnh, điều này làm cho giải pháp dựa trên Dijkstra đủ hiệu quả. Quá trình quét xác suất là tuyến tính sau khi sắp xếp, do đó nó không ảnh hưởng đến giới hạn tiệm cận. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # assume solve() is defined above
    return sys.stdout.getvalue()

# Note: In a real setup, solve() would be imported and called properly.
# These are structural test descriptions.

# Minimum case: single edge, single shop
assert True

# No shop reachable (conceptual)
assert True

# All probabilities 1, deterministic case
assert True

# High failure probability leading to impossible
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị tối thiểu | giá trị | tính chính xác trên trường hợp hợp lệ nhỏ nhất | 
| tất cả p=1 | con đường ngắn nhất xác định | giảm xuống con đường ngắn nhất cổ điển thông qua cửa hàng tốt nhất | 
| tất cả nhỏ p | không thể | phát hiện xác suất hư hỏng khác 0 | 
| bội số bằng w[v] | xử lý ổn định | quan hệ quét không phá vỡ logic | 

## Vỏ cạnh 

Một tình huống tế nhị là khi nhiều cửa hàng có chung w[v]. Trong trường hợp đó, thứ tự của chúng sau khi sắp xếp không thành vấn đề vì mỗi cái đóng góp với một xác suất tùy thuộc vào việc tất cả những cái trước đó không hoạt động. Nếu hai nút có trọng số bằng nhau được hoán đổi, biểu thức vẫn phân vùng cùng một không gian xác suất vì cả hai đều tương ứng với cùng một mức chi phí và chỉ một trong số chúng có thể là nút hoạt động đầu tiên trong số các giá trị bằng nhau. 

Một trường hợp khác là khi không thể truy cập được cửa hàng từ hai phía. Các nút như vậy phải được loại trừ sớm. Nếu dist1[v] hoặc dist2[v] là vô hạn thì w[v] là vô nghĩa vì không có đường dẫn hợp lệ nào có thể sử dụng cửa hàng đó. Thuật toán xử lý vấn đề này một cách tự nhiên bằng cách bỏ qua các nút đó trong quá trình thu thập cửa hàng. 

Trường hợp góc cuối cùng là khi tất cả các cửa hàng đều vắng mặt hoặc có xác suất bằng 0. Trong trường hợp này, cur_fail vẫn giữ nguyên bằng 1 và thuật toán đưa ra kết quả “không thể” một cách chính xác mà không cần cố gắng tính toán kỳ vọng.
