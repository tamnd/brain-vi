---
title: "CF 105013K - Đi Tìm Tình Yêu"
description: "Nhiệm vụ có thể được hiểu là bài toán đường đi ngắn nhất trên biểu đồ có trọng số có hướng, nhưng có thêm một lớp theo dõi trạng thái. Mỗi nút trong biểu đồ có thể được truy cập nhiều lần tùy thuộc vào số lượng nút “xấu” đặc biệt đã được thông qua cho đến nay."
date: "2026-06-28T02:14:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105013
codeforces_index: "K"
codeforces_contest_name: "The 19th Southeast University Programming Contest (Summer)"
rating: 0
weight: 105013
solve_time_s: 48
verified: true
draft: false
---

[CF 105013K - Đi tìm tình yêu của bạn](https://codeforces.com/problemset/problem/105013/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ có thể được hiểu là bài toán đường đi ngắn nhất trên biểu đồ có trọng số có hướng, nhưng có thêm một lớp theo dõi trạng thái. Mỗi nút trong biểu đồ có thể được truy cập nhiều lần tùy thuộc vào số lượng nút “xấu” đặc biệt đã được thông qua cho đến nay. Khi chúng ta đi qua một nút bình thường, trạng thái của chúng ta không thay đổi. Khi chúng tôi đi qua một nút xấu, chúng tôi tiêu thụ một đơn vị tài nguyên hạn chế và điều này ảnh hưởng đến số lượng nút xấu trong tương lai mà chúng tôi vẫn được phép đi qua. 

Về mặt hình thức, chúng tôi không chỉ quan tâm đến chi phí rẻ nhất để tiếp cận mỗi nút mà còn quan tâm đến chi phí rẻ nhất để tiếp cận từng cặp bao gồm một nút và số lượng nút xấu đã được sử dụng. Đầu ra yêu cầu, đối với mỗi nút, chi phí tối thiểu trong số tất cả các trạng thái hợp lệ. 

Đầu vào mô tả một biểu đồ có hướng có trọng số với tối đa một trăm nghìn nút và cạnh, cộng với một ràng buộc nhỏ k giới hạn số lượng nút xấu mà chúng ta được phép đi qua. Một tập hợp con các nút được đánh dấu là xấu và mỗi lần chúng tôi nhập một nút, chúng tôi sẽ tiêu thụ một đơn vị phụ cấp này. 

Đầu ra là danh sách chi phí có thể tiếp cận ngắn nhất cho mỗi nút hoặc -1 nếu không có đường dẫn hợp lệ nào tồn tại trong ràng buộc. 

Các ràng buộc ngay lập tức loại trừ bất kỳ phương pháp nào tính toán lại các đường dẫn ngắn nhất một cách độc lập trên mỗi nút hoặc mỗi trạng thái. Đường đi ngắn nhất đơn giản cho mỗi trạng thái sẽ bùng nổ thành một thứ như O(nk log n) hoặc tệ hơn tùy thuộc vào chi tiết triển khai, vì vậy chúng ta phải xử lý vấn đề như một đường đi ngắn nhất thống nhất duy nhất trên một không gian trạng thái mở rộng. 

Trường hợp cạnh tinh tế xuất hiện khi k = 0. Trong trường hợp này, bất kỳ đường dẫn nào chạm vào nút xấu sẽ trở thành không hợp lệ, do đó chỉ có đường dẫn qua nút an toàn mới quan trọng. Một trường hợp góc cạnh khác là khi bản thân đích đến không tốt; chúng tôi vẫn phải cho phép nhập nếu chúng tôi còn hạn ngạch tại thời điểm nhập. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là coi mỗi truy vấn “chi phí để tiếp cận nút v là bao nhiêu sau khi sử dụng i nút xấu” dưới dạng trạng thái khoảng cách riêng biệt và chạy đường dẫn ngắn nhất nhiều trạng thái. Điều này dẫn đến việc xác định một bảng lập trình động trong đó dist[u][i] là chi phí tối thiểu để tiếp cận nút u sau khi đã sử dụng i nút xấu. 

Từ góc độ biểu đồ, điều này tương đương với việc xây dựng một biểu đồ phân lớp với k+1 bản sao của mỗi nút. Mỗi cạnh ban đầu u → v có trọng số w trở thành các chuyển tiếp giữa các lớp tùy thuộc vào việc v có xấu hay không. Nếu v không tệ thì chỉ số lớp vẫn giữ nguyên. Nếu v xấu, chỉ số lớp tăng thêm một. Mục tiêu khi đó là đường đi ngắn nhất từ ​​(1, 0) trong biểu đồ mở rộng này. 

Công thức này đúng nhưng vẫn cần một cách hiệu quả để tính toán đường đi ngắn nhất. Phương pháp brute-force sẽ liên tục nới lỏng các cạnh trên tất cả các trạng thái mà không có mức độ ưu tiên, phương pháp này thoái hóa thành một thứ tương tự như SPFA trên biểu đồ có kích thước n·k với các chuyển đổi lên đến m·k. Trong trường hợp xấu nhất, việc này có thể trở nên quá chậm do phải xử lý lại các trạng thái nhiều lần. 

Quan sát quan trọng là biểu đồ mở rộng này có trọng số cạnh không âm, vì vậy chúng ta có thể áp dụng thuật toán Dijkstra một cách an toàn trên biểu đồ trạng thái. Mỗi trạng thái là một nút trong biểu đồ lớn hơn và mỗi chuyển đổi là một cạnh có trọng số. Một khi được nhìn theo cách này, bài toán sẽ trở thành bài toán đường đi ngắn nhất một nguồn tiêu chuẩn. 

Phiên bản SPFA trong tuyên bố và phiên bản Dijkstra chỉ khác nhau ở lựa chọn triển khai: SPFA cố gắng nới lỏng một cách linh hoạt, trong khi Dijkstra thực thi trật tự bằng cách sử dụng hàng đợi ưu tiên, ngăn chặn việc nới lỏng lặp đi lặp lại không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Bạo lực tàn bạo đối với các bang (thư giãn không kiểm soát) | O(n · k · m) trường hợp xấu nhất | O(n · k) | Quá chậm | 
| SPFA trên biểu đồ trạng thái | O(n · k · m) trường hợp xấu nhất | O(n · k) | Rủi ro / ranh giới | 
| Dijkstra trên biểu đồ trạng thái | O((n · k + m · k) log(n · k)) | O(n · k + m · k) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi chuyển đổi biểu đồ gốc thành biểu đồ mở rộng trạng thái trong đó mỗi nút u có k+1 phiên bản biểu thị số lượng nút xấu đã được sử dụng cho đến nay khi tiếp cận u. 

Sau đó chúng tôi chạy Dijkstra từ trạng thái ban đầu. 

1. Xây dựng ánh xạ từ mỗi cặp (nút u, used_bad i) tới một chỉ mục duy nhất trong biểu đồ trạng thái. Điều này cho phép chúng ta coi mỗi trạng thái như một nút bình thường trong thuật toán đường đi ngắn nhất. 
2. Với mỗi cạnh ban đầu u → v có trọng số w, chúng ta thêm các chuyển tiếp giữa các trạng thái. Nếu v là một nút xấu thì từ trạng thái (u, i) chúng ta có thể chuyển sang (v, i+1) với điều kiện i+1 ≤ k. Nếu v không tệ thì ta đi tới (v, i). Điều này mã hóa ràng buộc trực tiếp vào cấu trúc biểu đồ. 
3. Khởi tạo tất cả khoảng cách trong biểu đồ trạng thái thành vô cùng, ngoại trừ trạng thái bắt đầu (1, 0), được đặt thành 0. 
4. Đẩy trạng thái bắt đầu vào hàng ưu tiên. Điều này đảm bảo rằng chúng tôi luôn mở rộng trạng thái có thể truy cập rẻ nhất hiện tại trước tiên. 
5. Liên tục trích xuất trạng thái với khoảng cách tối thiểu. Nếu nó đã được xử lý rồi thì bỏ qua. Ngược lại, hãy nới lỏng tất cả các chuyển đổi đi tới các trạng thái lân cận, cập nhật khoảng cách của chúng nếu tìm thấy đường đi ngắn hơn. 
6. Sau khi thuật toán kết thúc, với mỗi nút gốc u, lấy khoảng cách tối thiểu trên tất cả các trạng thái (u, i) của i từ 0 đến k. Điều này mang lại chi phí tốt nhất có thể để tiếp cận bạn theo bất kỳ cách sử dụng hợp lệ nào của các nút xấu. 

Tại sao nó hoạt động được gắn với một bất biến đơn giản: bất cứ khi nào chúng tôi đưa một trạng thái (u, i) ra khỏi hàng đợi ưu tiên, chúng tôi đã tìm thấy chi phí tối thiểu có thể có để đạt được cấu hình chính xác đó. Bởi vì tất cả các quá trình chuyển đổi đều có trọng số không âm, nên bất kỳ cải tiến nào trong tương lai sẽ yêu cầu một đường dẫn rẻ hơn đến cùng một trạng thái, điều này mâu thuẫn với thuộc tính đặt hàng được thực thi bởi hàng đợi ưu tiên. Điều này đảm bảo rằng một khi trạng thái được hoàn tất, nó sẽ không bao giờ cần phải xem lại. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m, p, k = map(int, input().split())
    bad = [False] * (n + 1)

    for _ in range(p):
        x = int(input())
        bad[x] = True

    g = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v, w = map(int, input().split())
        g[u].append((v, w))

    INF = 10**18
    dist = [[INF] * (k + 1) for _ in range(n + 1)]
    dist[1][0] = 0

    pq = [(0, 1, 0)]

    while pq:
        d, u, used = heapq.heappop(pq)
        if d != dist[u][used]:
            continue

        for v, w in g[u]:
            if bad[v]:
                if used < k and dist[v][used + 1] > d + w:
                    dist[v][used + 1] = d + w
                    heapq.heappush(pq, (dist[v][used + 1], v, used + 1))
            else:
                if dist[v][used] > d + w:
                    dist[v][used] = d + w
                    heapq.heappush(pq, (dist[v][used], v, used))

    for i in range(1, n + 1):
        ans = min(dist[i])
        print(-1 if ans == INF else ans, end=" ")

if __name__ == "__main__":
    solve()
```Giải pháp trực tiếp triển khai Dijkstra trên không gian trạng thái phân lớp. Bảng khoảng cách lưu trữ chi phí đã biết tốt nhất cho mỗi cặp (nút, used_bad). Hàng đợi ưu tiên luôn mở rộng trạng thái hứa hẹn nhất và các mục nhập cũ sẽ bị bỏ qua bằng cách so sánh với khoảng cách tốt nhất hiện tại. 

Chi tiết triển khai chính là sự phân tách giữa các lần chuyển đổi tùy thuộc vào việc nút tiếp theo có hoạt động kém hay không. Điều kiện đó quyết định chiều thứ hai của trạng thái có tăng hay không. Một chi tiết quan trọng khác là lấy mức tối thiểu trên tất cả các lớp ở cuối, vì việc tiếp cận một nút có ít hoặc nhiều nút xấu được sử dụng hơn đều có thể là tối ưu tùy thuộc vào cấu trúc đường dẫn. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ có ba nút trong đó nút 2 là nút xấu và chúng ta được phép có k = 1 nút xấu. Các cạnh là 1 → 2 (giá 5), 2 → 3 (giá 2) và 1 → 3 (giá 10). 

Chúng tôi theo dõi các trạng thái dưới dạng (nút, used_bad). 

Trạng thái ban đầu là (1, 0) với khoảng cách 0. 

| Bước | Trạng thái xuất hiện | Khoảng cách | Cập nhật hành động | 
| --- | --- | --- | --- | 
| 1 | (1,0) | 0 | Thư giãn 1→2 cho (2,1)=5, Thư giãn 1→3 cho (3,0)=10 | 
| 2 | (2,1) | 5 | Thư giãn 2→3 cho (3,1)=7 | 
| 3 | (3,0) | 10 | Không có cải tiến | 
| 4 | (3,1) | 7 | Không có cải tiến | 

Câu trả lời cuối cùng cho nút 3 là min(10, 7) = 7. 

Điều này cho thấy việc sử dụng trợ cấp nút xấu có thể tạo ra tuyến đường dài hơn nhưng rẻ hơn như thế nào. 

Bây giờ hãy xem xét k = 0 trên cùng một đồ thị. Mọi đường dẫn vào nút 2 đều không hợp lệ. 

| Bước | Trạng thái xuất hiện | Khoảng cách | Cập nhật hành động | 
| --- | --- | --- | --- | 
| 1 | (1,0) | 0 | Chỉ cho phép 1→3, vì vậy (3,0)=10 | 
| 2 | (3,0) | 10 | Xong | 

Bây giờ, nút 3 chỉ có thể truy cập được thông qua cạnh trực tiếp, xác nhận cách ràng buộc chặn các nút trung gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n · k + m · k) log(n · k)) | Mỗi trạng thái được xử lý một lần bằng Dijkstra và mỗi cạnh tạo ra tối đa k chuyển tiếp | 
| Không gian | O(n · k + m · k) | Bảng khoảng cách cộng với kề trên biểu đồ trạng thái mở rộng | 

Giá trị của k nhỏ trong các ràng buộc điển hình, làm cho việc mở rộng trạng thái trở nên khả thi. Hệ số logarit từ hàng ưu tiên giữ cho giải pháp thoải mái trong giới hạn ngay cả với n lên tới 100000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like small graph
assert run("""3 3 1 1
2
1 2 5
2 3 2
1 3 10
""") == "0 5 7", "basic case"

# k = 0 blocks bad node
assert run("""3 3 1 0
2
1 2 5
2 3 2
1 3 10
""") == "0 -1 10", "no bad allowed"

# single node
assert run("""1 0 0 0
""") == "0", "single node"

# all nodes bad but k large enough
assert run("""2 1 2 1
2
1
1 2 5
""") == "0 5", "all bad allowed once"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị nhỏ | 0 5 7 | thư giãn theo lớp đúng cách | 
| k = 0 trường hợp | 0 -1 10 | thực thi ràng buộc | 
| nút đơn | 0 | trường hợp cơ bản tầm thường | 
| tất cả đều tệ | 0 5 | logic tiêu dùng tồi | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi k = 0. Trong trường hợp đó, mọi chuyển đổi sang nút xấu đều phải bị chặn hoàn toàn. Thuật toán xử lý việc này một cách tự nhiên vì điều kiện`used < k`ngăn chặn việc tạo ra các trạng thái không hợp lệ. Ví dụ: nếu nút 2 xấu và k = 0 thì từ (1,0) chúng ta không bao giờ đẩy (2,1), nên tất cả các đường đi qua nút 2 đều biến mất. 

Một trường hợp khác là khi nút đích không hoạt động. Thuật toán vẫn cho phép đạt được nó miễn là quá trình chuyển đổi trạng thái diễn ra trong giới hạn. Nếu việc tiếp cận nút v tiêu thụ chính xác hạn ngạch khả dụng cuối cùng thì trạng thái (v, k) vẫn hợp lệ và tham gia vào tính toán tối thiểu cuối cùng. 

Trường hợp thứ ba xuất hiện khi nhiều đường dẫn khác nhau đến cùng một nút với các giá trị used_bad khác nhau. Thuật toán giữ tất cả chúng một cách chính xác và giá trị tối thiểu cuối cùng trên tất cả các trạng thái đảm bảo rằng chúng tôi không loại bỏ nhầm đường dẫn tốt hơn sử dụng số lượng nút xấu khác nhau.
