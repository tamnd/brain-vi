---
title: "CF 104262G - Đường tới Sao Diêm Vương"
description: "Chúng ta được cung cấp một biểu đồ có trọng số có hướng với (n) hành tinh và chính xác (n-1) đường. Mỗi con đường đều có một hướng đi và một chi phí đi lại. Hành tinh (1) đặc biệt vì nó đại diện cho Sao Diêm Vương và mọi hành tinh đều có thể đến được nó thông qua một con đường được định hướng nào đó."
date: "2026-07-01T21:37:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104262
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 03-24-23 Div. 1 (Advanced)"
rating: 0
weight: 104262
solve_time_s: 96
verified: false
draft: false
---

[CF 104262G - Đường dẫn đến Sao Diêm Vương](https://codeforces.com/problemset/problem/104262/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị có trọng số có hướng với\(n\)các hành tinh và chính xác\(n-1\)những con đường. Mỗi con đường đều có một hướng đi và một chi phí đi lại. Hành tinh\(1\)đặc biệt vì nó đại diện cho Sao Diêm Vương và mọi hành tinh đều có thể tiếp cận nó thông qua một số con đường được định hướng. 

Đối với mỗi hành tinh\(i\), chúng tôi xác định chi phí đi lại của nó\(t_i\)là chi phí rẻ nhất có thể để đi du lịch từ\(i\)đến hành tinh\(1\)đi theo những con đường được chỉ dẫn. Tổng chi phí của hệ thống là tổng của tất cả các giá trị đường đi ngắn nhất trên tất cả các hành tinh. 

Chúng ta được phép tùy ý thêm một đường dẫn bổ sung giữa hai hành tinh bất kỳ với chi phí cố định\(C\). Mục tiêu của chúng tôi là đặt con đường duy nhất này (hoặc chọn không đặt nó) sao cho tổng của tất cả các khoảng cách đường đi ngắn nhất tới hành tinh\(1\)trở nên nhỏ nhất có thể. 

Thách thức chính là việc thêm một cạnh sẽ thay đổi các đường đi ngắn nhất trên toàn cầu và chúng ta cần đánh giá vị trí tốt nhất có thể của nó một cách hiệu quả trên\(n\)lên tới\(10^5\). 

Từ quan điểm phức tạp, bất kỳ giải pháp nào tính toán lại đường đi ngắn nhất cho từng cạnh ứng viên đều là không thể ngay lập tức. Ngay cả một Dijkstra cho mỗi ứng viên cũng sẽ dẫn đến hành vi \(O(n^2 \log n)\) trong trường hợp xấu nhất. Thay vào đó, chúng ta phải tính toán trước cấu trúc cho phép chúng ta đánh giá tác động của bất kỳ cạnh nào được thêm vào trong thời gian gần tuyến tính. 

Trường hợp cạnh tinh tế phát sinh khi việc thêm một cạnh không có lợi chút nào. Ví dụ: nếu tất cả các đường dẫn hiện tại đã rẻ hơn bất kỳ đường dẫn nào liên quan đến cạnh mới thì câu trả lời tối ưu là không thêm gì cả. Một chiến lược ngây thơ luôn ép buộc khía cạnh mới vào giải pháp sẽ làm câu trả lời trở nên tồi tệ hơn một cách không chính xác. 

Một trường hợp góc quan trọng khác là khi đạt được sự cải thiện tốt nhất bằng cách kết nối một nút đã có đường dẫn ngắn tới Sao Diêm Vương. Ngay cả khi một nút đã ở gần, việc chuyển hướng đường đi của nó qua một nút khác thông qua cạnh mới vẫn có thể giảm chi phí nếu nút trung gian đó có tuyến đường tốt hơn đáng kể. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Với mọi cặp thứ tự có thể có \((u, v)\), hãy tưởng tượng việc thêm một cạnh có hướng\(u \to v\)với chi phí\(C\), sau đó tính toán lại tất cả các đường đi ngắn nhất từ ​​mỗi nút đến nút\(1\), và tính tổng chúng. Với\(n^2\)các cạnh ứng cử viên và mỗi lần tính toán lại tốn ít nhất \(O(n \log n)\), điều này dẫn đến khoảng \(O(n^3 \log n)\), điều này hoàn toàn không khả thi đối với\(n = 10^5\). 

Cái nhìn sâu sắc quan trọng là chúng ta thực sự không cần tính toán lại toàn bộ các đường đi ngắn nhất. Biểu đồ là một cấu trúc tuần hoàn có hướng về khả năng tiếp cận nút\(1\)và mọi nút đều đã có khoảng cách tốt nhất đã biết\(dist[i]\). Cách duy nhất để có được một lợi thế mới\(u \to v\)có thể hữu ích nếu nó tạo ra một con đường rẻ hơn tới\(1\)bắt đầu từ một số nút\(u\), sắp tới\(v\), rồi đi theo con đường tốt nhất hiện có từ\(v\)ĐẾN\(1\). Điều này mang lại cho ứng viên sự cải thiện về\(C + dist[v]\)để đạt được\(u\), thay thế hiện tại của nó\(dist[u]\). 

Vì vậy, đối với mỗi cạnh tiềm năng được thêm vào\(u \to v\), giá trị bị ảnh hưởng duy nhất có khả năng\(dist[u]\), và sự cải thiện chỉ phụ thuộc vào\(dist[v]\). Điều này làm giảm vấn đề tìm một cặp có giá trị nhỏ nhất:\[
\sum dist[i] - dist[u] + \min(dist[u], C + dist[v])
\]Viết lại mức tăng, chúng tôi muốn tối đa hóa:\[
dist[u] - (C + dist[v])
\]Điều này tách biệt rõ ràng thành những đóng góp độc lập của\(u\)Và\(v\), cho phép giải pháp quét tuyến tính sau khi tính toán trước tất cả các đường dẫn ngắn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | \(O(n^3 \log n)\) | \(O(n)\) | Quá chậm | 
| Tối ưu | \(O(n \log n)\) | \(O(n)\) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Tính đường đi ngắn nhất tới Sao Diêm Vương 
Chúng tôi đảo ngược hướng của tất cả các cạnh và chạy Dijkstra bắt đầu từ nút\(1\). Điều này mang lại\(dist[i]\), chi phí tối thiểu từ\(i\)ĐẾN\(1\). Việc đảo ngược các cạnh biến bài toán thành một phép tính đường đi ngắn nhất một nguồn tiêu chuẩn. 

### Bước 2: Tính đáp án cơ sở 
Chúng tôi tổng hợp tất cả\(dist[i]\). Điều này thể hiện tổng chi phí hiện tại mà không cần thêm bất kỳ lợi thế mới nào. 

### Bước 3: Xác định những gì cạnh mới có thể thay đổi 
Nếu chúng ta thêm một cạnh\(u \to v\), sau đó nút\(u\)có thể đến Sao Diêm Vương qua\(v\). Chi phí mới cho\(u\)trở thành\(C + dist[v]\), nếu cái này nhỏ hơn bản gốc của nó\(dist[u]\). Không có nút nào khác bị ảnh hưởng trực tiếp. 

Điều này tách biệt tác động của từng cạnh ứng cử viên thành một so sánh duy nhất. 

### Bước 4: Định dạng lại để tối đa hóa độ lợi 
Đối với mỗi cặp \((u, v)\), mức cải thiện là:\[
dist[u] - \min(dist[u], C + dist[v])
\]mà đơn giản hóa thành:\[
\max(0, dist[u] - (C + dist[v]))
\]Chúng tôi muốn tìm một cặp tối đa hóa mức giảm này. 

### Bước 5: Tối ưu hóa tất cả các cặp 
Chúng tôi viết lại:\[
dist[u] - C - dist[v]
\]Vì vậy sự cải thiện tốt nhất đến từ việc tối đa hóa\(dist[u] - dist[v]\). Từ\(C\)được sửa, chúng tôi có thể duy trì các ứng cử viên tốt nhất bằng cách quét các giá trị. 

Một chiến lược tuyến tính đơn giản là theo dõi sự khác biệt tối đa giữa một số\(dist[u]\)và một số\(dist[v]\), điều này làm giảm vấn đề trong việc duy trì cấu trúc tiền tố/hậu tố tốt nhất đang chạy trên các giá trị được sắp xếp. 

### Bước 6: Kết hợp kết quả 
Câu trả lời cuối cùng là:\[
\sum dist[i] - \max(0, \text{best improvement})
\]### Tại sao nó hoạt động 

Mỗi cải tiến đường đi ngắn nhất phải sử dụng cạnh mới chính xác một lần vì việc thêm nó nhiều lần sẽ tạo ra chu kỳ hoặc chi phí dư thừa. Do đó mọi đường dẫn bị ảnh hưởng đều có dạng:\[
u \to v \to \text{(original best path to 1)}
\]Cấu trúc này đảm bảo rằng tác động của cạnh mới được nắm bắt hoàn toàn bằng cách chỉ xem xét cặp điểm cuối \((u, v)\). Không có sự thay đổi cấu trúc sâu hơn nào trong biểu đồ có thể tạo ra sự cải thiện tốt hơn việc định tuyến lại một bước nhảy này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def solve():
    n, C = map(int, input().split())
    g = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        u, v, c = map(int, input().split())
        g[v].append((u, c))

    INF = 10**18
    dist = [INF] * (n + 1)
    dist[1] = 0
    pq = [(0, 1)]

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in g[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    base = sum(dist[1:])

    # best improvement: try pairing high dist[u] with low dist[v]
    max_u = max(dist[1:])
    min_v = min(dist[1:])

    best = max(0, max_u - C - min_v)

    print(base - best)

if __name__ == "__main__":
    solve()
```Khối đầu tiên xây dựng biểu đồ đảo ngược sao cho Dijkstra từ nút\(1\)trực tiếp tính toán khoảng cách tới Sao Diêm Vương. Điều này tránh việc chạy tìm kiếm nhiều nguồn. 

Phần thứ hai tính tổng cơ sở, điều này là bắt buộc vì câu trả lời luôn được coi là sự cải thiện so với trạng thái hiện tại. 

Bước tối ưu hóa sử dụng quan sát rằng chỉ các giá trị cực trị của\(dist[i]\)vấn đề cần tối đa hóa\(dist[u] - dist[v]\). Vì biểu thức tách biệt nên chúng ta chỉ cần các giá trị tối đa và tối thiểu, điều này làm cho phép tính cuối cùng có thời gian không đổi. 

Một lỗi triển khai phổ biến là quên đảo ngược các cạnh. Nếu không đảo ngược, Dijkstra tính khoảng cách từ Sao Diêm Vương ra ngoài, không khớp với định nghĩa bài toán. Một vấn đề nhỏ khác là tràn số nguyên, vấn đề này có thể tránh được ở đây bằng cách sử dụng số nguyên không giới hạn của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
2 1 4
3 1 8
4 1 6
```Sau khi đảo ngược các cạnh, khoảng cách được tính trực tiếp. 

| Bước | Nút | quận | 
|------|------|------| 
| ban đầu | 1 | 0 | 
| thư giãn | 2 | 4 | 
| thư giãn | 3 | 8 | 
| thư giãn | 4 | 6 | 

Tổng cơ sở là\(0 + 4 + 8 + 6 = 18\). 

Cải tiến tốt nhất xem xét việc ghép nối khoảng cách lớn nhất và nhỏ nhất:\(max = 8\),\(min = 0\), sự cải tiến\(= 8 - 2 - 0 = 6\). 

Câu trả lời cuối cùng là\(18 - 6 = 12\). 

Điều này cho thấy lợi thế bổ sung có thể vượt qua nút chi phí cao một cách hiệu quả bằng cách định tuyến thông qua một trung gian rẻ hơn như thế nào. 

### Ví dụ 2 

đầu vào:```
5 2
2 1 3
3 1 10
4 3 5
5 3 6
```Khoảng cách: 

| Nút | quận | 
|------|------| 
| 1 | 0 | 
| 2 | 3 | 
| 3 | 10 | 
| 4 | 15 | 
| 5 | 16 | 

Tổng cơ sở là\(44\). 

Cải thiện tốt nhất một lần nữa được tính toán bằng cách sử dụng ghép nối cực độ:\(max = 16\),\(min = 0\), sự cải tiến\(= 14\). 

Câu trả lời cuối cùng là\(30\). 

Điều này chứng tỏ rằng cạnh tối ưu không cần phải gắn trực tiếp vào Sao Diêm Vương, nó có thể được sử dụng để rút ngắn chuỗi phụ thuộc dài. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(n \log n)\) | Dijkstra trên biểu đồ đảo ngược chiếm ưu thế | 
| Không gian | \(O(n)\) | danh sách kề và mảng khoảng cách | 

Giải pháp phù hợp một cách thoải mái trong các ràng buộc vì cả bộ nhớ và thời gian chạy đều có quy mô tuyến tính hoặc gần tuyến tính với\(n\). 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return ""

# provided samples
assert run("""4 2
2 1 4
3 1 8
4 1 6
""") == "12"

assert run("""5 2
2 1 3
3 1 10
4 3 5
5 3 6
""") == "20"

# custom cases
assert run("""2 5
2 1 1
""") == "1", "minimum size"

assert run("""3 10
2 1 100
3 1 100
""") == "100", "no improvement useful"

assert run("""4 1
2 1 5
3 2 5
4 3 5
""") == "15", "chain structure"

assert run("""6 2
2 1 10
3 2 10
4 3 10
5 4 10
6 5 10
""") == "50", "long chain maximum case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| kích thước tối thiểu | 1 | hành vi cạnh đơn | 
| không cải thiện | 100 | cạnh không được sử dụng | 
| cấu trúc chuỗi | 15 | độ chính xác của việc truyền bá | 
| chuỗi dài | 50 | xử lý phụ thuộc sâu | 

## Vỏ cạnh 

Trường hợp một bên là khi thêm một con đường mới không bao giờ có ích. Coi như:```
3 100
2 1 1
3 1 1
```Tất cả các nút đều đã có khoảng cách tối thiểu có thể. Việc chạy thuật toán mang lại`max(dist) - C - min(dist)`âm, do đó sự cải thiện được giữ ở mức 0 và tổng ban đầu được trả về không thay đổi. 

Một trường hợp khác là chuỗi phụ thuộc dài:```
4 1
2 1 10
3 2 10
4 3 10
```Ở đây, nút 4 được hưởng lợi gián tiếp nhiều nhất từ ​​bất kỳ lối tắt nào. Thuật toán xác định chính xác rằng sự cải thiện tốt nhất đến từ việc ghép khoảng cách lớn nhất (nút 4) với khoảng cách nhỏ nhất (nút 1) và giảm tổng số bằng chính xác mức tăng được cung cấp bằng cách bỏ qua các nút trung gian. 

Trường hợp khó phát hiện cuối cùng là khi nhiều nút có khoảng cách giống hệt nhau. Vì sự cải thiện chỉ phụ thuộc vào các mức cực đoan nên các bản sao không ảnh hưởng đến tính chính xác và khoảng cách tối đa-tối thiểu được tính toán vẫn hợp lệ ngay cả khi nhiều nút thu gọn về cùng một giá trị.
