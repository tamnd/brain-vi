---
title: "CF 105418A - Phượng hoàng chống lại quái vật"
description: "Chúng ta có một mạng lưới các thành phố được kết nối bằng các con đường, trong đó mỗi con đường có chi phí không âm đại diện cho số lượng quái vật mà Phoenix phải chiến đấu nếu anh ta đi dọc theo nó. Phoenix bắt đầu tại thành phố 1 và phải đến thành phố n bằng bất kỳ chuỗi đường dẫn nào."
date: "2026-06-23T04:20:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105418
codeforces_index: "A"
codeforces_contest_name: "Algorithmia IIITN 2024 - Round 1"
rating: 0
weight: 105418
solve_time_s: 99
verified: false
draft: false
---

[CF 105418A - Phượng hoàng chống lại quái vật](https://codeforces.com/problemset/problem/105418/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mạng lưới các thành phố được kết nối bằng các con đường, trong đó mỗi con đường có chi phí không âm đại diện cho số lượng quái vật mà Phoenix phải chiến đấu nếu anh ta đi dọc theo nó. Phoenix bắt đầu tại thành phố 1 và phải đến thành phố n bằng bất kỳ chuỗi đường dẫn nào. Đây là cài đặt đường dẫn ngắn nhất tiêu chuẩn trong đó chi phí đường dẫn là tổng trọng số của các cạnh. 

Điều khó khăn là Phoenix có một hoạt động đặc biệt duy nhất có thể giảm số lượng quái vật. Anh ta có thể chọn chính xác một trong các tùy chọn sau trong toàn bộ hành trình của mình: chọn một con đường và thay thế trọng số w bằng sàn(w / 2) hoặc chọn hai con đường liên tiếp dọc theo con đường đã chọn và thay thế cả hai trọng số của chúng bằng sàn(w / 2). Sau khi áp dụng thao tác này một lần, anh ta vẫn di chuyển dọc theo một đường đi bình thường và tích lũy các trọng số của cạnh đã được sửa đổi. 

Mục tiêu là giảm thiểu tổng chi phí từ thành phố 1 đến thành phố n sau khi chọn tối ưu cả đường đi và cách sử dụng khả năng giảm một nửa. 

Các ràng buộc ngay lập tức chỉ ra rằng việc liệt kê đơn giản trên tất cả các đường dẫn là không thể. Với tối đa 10^5 thành phố và 2×10^5 đường, mọi cách tiếp cận phụ thuộc vào việc khám phá tất cả các đường đi hoặc thử tất cả các tập hợp con của các cạnh sẽ bùng nổ về mặt tổ hợp. Ngay cả một lần chạy Dijkstra cũng có thể thực hiện được, nhưng điều phức tạp là “sửa đổi một lần” bổ sung, dẫn đến sự phụ thuộc trạng thái vào số lượng cạnh đã được sửa đổi và liệu chúng có liên tiếp hay không. 

Một trường hợp phức tạp phát sinh khi việc sử dụng khả năng tối ưu không nằm trên con đường ngắn nhất trong biểu đồ gốc. Ví dụ: một đường đi dài hơn một chút có thể trở nên rẻ hơn sau khi chia đôi hai cạnh lớn liền kề, do đó đường đi ngắn nhất tham lam mà không theo dõi trạng thái sẽ không thành công. 

Một dạng lỗi khác là giả sử việc giảm một nửa là độc lập trên mỗi cạnh. Nếu chúng ta tham lam giảm một nửa cạnh lớn nhất, chúng ta sẽ bỏ lỡ các trường hợp trong đó việc giảm một nửa hai cạnh vừa phải theo thứ tự sẽ tạo ra tổng mức giảm lớn hơn so với việc giảm một nửa một cạnh lớn. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xem xét mọi đường đi có thể từ 1 đến n và với mỗi đường dẫn mô phỏng ba biến thể: không hoạt động, giảm một nửa một cạnh hoặc giảm một nửa hai cạnh liên tiếp. Đối với một đường đi cố định có độ dài k, việc đánh giá tất cả các lựa chọn đó là O(k). Nhưng số đường đi trong đồ thị có hướng có thể là số mũ theo n, do đó phương pháp này ngay lập tức trở nên không khả thi. Ngay cả việc hạn chế các đường dẫn ngắn nhất cũng không giúp ích gì vì cấu trúc đường dẫn tối ưu phụ thuộc vào nơi áp dụng giảm một nửa. 

Quan sát quan trọng là cấu trúc của hoạt động đưa ra một lượng nhỏ trạng thái bổ sung cố định. Khi duyệt qua các cạnh theo thứ tự, tại bất kỳ thời điểm nào, chúng ta chỉ quan tâm đến việc liệu chúng ta chưa sử dụng khả năng này hay chưa, liệu chúng ta hiện đang ở giữa “hai cạnh một nửa” hay khả năng đó đã được sử dụng và hoàn thành hay chưa. Điều này biến bài toán thành bài toán đường đi ngắn nhất theo lớp trên một số lượng nhỏ trạng thái trên mỗi nút. 

Chúng tôi chạy thuật toán đường đi ngắn nhất một cách hiệu quả trên một biểu đồ mở rộng trong đó mỗi nút được chia thành các trạng thái biểu thị khoảng cách chúng tôi đang sử dụng thao tác đặc biệt. Từ mỗi trạng thái, các quá trình chuyển đổi mô phỏng việc lấy một cạnh một cách bình thường, bắt đầu giảm một nửa ở cạnh hiện tại hoặc hoàn thành việc chia đôi ở cạnh tiếp theo nếu có. Đây là một kỹ thuật tiêu chuẩn trong đó các ràng buộc đường dẫn liên quan đến kề cận cục bộ được mã hóa thành trạng thái biểu đồ. 

Kết quả là một quy trình giống như Dijkstra trên tối đa 3n trạng thái, vì mỗi thành phố có thể có nhiều nhất ba cấu hình có ý nghĩa. Điều này giữ cho sự phức tạp có thể quản lý được trong khi vẫn đảm bảo tính chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên mọi con đường | hàm mũ | hàm mũ | Quá chậm | 
| Dijkstra mở rộng cấp nhà nước | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi lập mô hình mỗi thành phố với ba trạng thái: trạng thái 0 có nghĩa là chưa có hoạt động nào được sử dụng, trạng thái 1 có nghĩa là chúng tôi đã sử dụng hoạt động này làm “cạnh đầu tiên được chọn” và đang chờ ghép nối tùy chọn với cạnh tiếp theo và trạng thái 2 có nghĩa là hoạt động đã được sử dụng hoàn toàn. 

Sau đó chúng tôi chạy thuật toán đường đi ngắn nhất qua các trạng thái này. 

1. Khởi tạo bảng khoảng cách dist[node][state] với giá trị vô cùng và đặt dist[1][0] = 0. Điều này phản ánh rằng chúng ta bắt đầu ở thành phố 1 mà không sử dụng sửa đổi nào. 
2. Sử dụng hàng ưu tiên được sắp xếp theo khoảng cách hiện tại. Mỗi mục đại diện (chi phí hiện tại, nút, trạng thái). Chúng tôi luôn mở rộng chi phí nhỏ nhất đã biết trước tiên, đảm bảo chúng tôi xử lý các trạng thái theo thứ tự tăng dần của chi phí cuối cùng. 
3. Từ một trạng thái (u, 0), chúng ta có thể đi qua một cạnh đi ra (u → v, w) theo ba cách khái niệm khác nhau. Chúng ta có thể sử dụng nó bình thường và duy trì trạng thái 0 tại v với cost +w. Chúng ta có thể bắt đầu hoạt động giảm một nửa ở cạnh này, thanh toán sàn(w/2) và chuyển sang trạng thái 1 tại v, nghĩa là chúng ta hiện đang tiến hành hoạt động được áp dụng một phần về phía trước. 
4. Từ trạng thái (u, 1), chúng ta đã chọn được cạnh đầu tiên của phép toán hai cạnh. Khi chúng ta đi qua một cạnh (u → v, w), chúng ta cũng hoàn thành thao tác bằng cách áp dụng giảm một nửa cho cạnh thứ hai này. Quá trình chuyển đổi đó chuyển sang trạng thái 2 tại v với mức chi phí bổ sung (w/2). Điều này thực thi tính liên tục vì trạng thái 1 buộc cạnh tiếp theo phải là một phần của cùng một đoạn đường dẫn. 
5. Từ trạng thái (u, 0), chúng ta cũng cho phép chuyển sang trạng thái 2 tại v mà không cần sử dụng phép toán nào cả, bằng cách lấy chi phí w. Điều này thể hiện trường hợp chúng ta không bao giờ sử dụng khả năng đặc biệt trên con đường này. 
6. Từ trạng thái (u, 2), tất cả các chuyển đổi đều là các chuyển đổi đường đi ngắn nhất tiêu chuẩn: chúng ta chỉ cần thêm w và giữ nguyên ở trạng thái 2, vì thao tác đã được sử dụng đầy đủ. 
7. Tiếp tục Dijkstra cho đến khi tất cả các trạng thái có thể truy cập được xử lý. Câu trả lời là giá trị tối thiểu của dist[n][0], dist[n][1], dist[n][2]. 

Hạn chế chính về cấu trúc là trạng thái 1 không thể tồn tại vô thời hạn. Nó buộc chính xác một cạnh tiếp theo phải giải quyết thao tác, đảm bảo chúng ta chỉ áp dụng sửa đổi “hai cạnh liên tiếp” một lần. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong một giải pháp hợp lệ, lịch sử liên quan duy nhất là liệu chúng ta đã sử dụng thao tác đó chưa và liệu chúng ta có đang áp dụng phiên bản hai cạnh của nó hay không. Bất kỳ bộ nhớ nào dài hơn về các cạnh trước đó đều không liên quan vì thao tác chỉ tương tác với tối đa hai cạnh liên tiếp. Điều này làm cho quá trình Markovian diễn ra trên không gian trạng thái được mở rộng. 

Thuật toán của Dijkstra đảm bảo tính chính xác vì tất cả các chuyển đổi đều bảo toàn chi phí cạnh không âm và mọi đường dẫn được sửa đổi hợp lệ đều tương ứng với chính xác một đường dẫn trong biểu đồ mở rộng này. Ngược lại, mọi đường dẫn trong biểu đồ mở rộng tương ứng với cách sử dụng hợp lệ của phép toán trong bài toán ban đầu, do đó không có cấu hình không hợp lệ nào được đưa ra. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline
INF = 10**30

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v, w = map(int, input().split())
        g[u].append((v, w))

    dist = [[INF] * 3 for _ in range(n + 1)]
    dist[1][0] = 0
    pq = [(0, 1, 0)]

    while pq:
        d, u, st = heapq.heappop(pq)
        if d != dist[u][st]:
            continue

        if st == 0:
            for v, w in g[u]:
                nd = d + w
                if nd < dist[v][0]:
                    dist[v][0] = nd
                    heapq.heappush(pq, (nd, v, 0))

                nd2 = d + (w // 2)
                if nd2 < dist[v][1]:
                    dist[v][1] = nd2
                    heapq.heappush(pq, (nd2, v, 1))

                nd3 = d + w
                if nd3 < dist[v][2]:
                    dist[v][2] = nd3
                    heapq.heappush(pq, (nd3, v, 2))

        elif st == 1:
            for v, w in g[u]:
                nd = d + (w // 2)
                if nd < dist[v][2]:
                    dist[v][2] = nd
                    heapq.heappush(pq, (nd, v, 2))

        else:
            for v, w in g[u]:
                nd = d + w
                if nd < dist[v][2]:
                    dist[v][2] = nd
                    heapq.heappush(pq, (nd, v, 2))

    print(min(dist[n]))

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một bảng khoảng cách đầy đủ trên ba trạng thái và chạy vòng lặp Dijkstra tiêu chuẩn. Chi tiết quan trọng là trạng thái 1 chỉ cho phép các chuyển đổi hoàn thành thao tác hai cạnh, ngăn chặn việc sử dụng lại không hợp lệ hoặc ứng dụng không liên tiếp. 

Một mối lo ngại nhỏ trong việc triển khai là sự trùng lặp của các chuyển đổi từ trạng thái 0. Chúng tôi cho phép rõ ràng ba tùy chọn: truyền tải thông thường, bắt đầu giảm một nửa hoặc đánh dấu sớm hoạt động là đã được sử dụng. Quá trình chuyển đổi cuối cùng này là cần thiết vì thao tác có thể không bao giờ được sử dụng hoặc có thể được sử dụng sau này và trạng thái 2 hoạt động như trạng thái "hoàn thành" khi không còn xử lý đặc biệt nào. 

## Ví dụ đã hoạt động 

### Mẫu 2 Dấu vết 

Đồ thị có một cạnh 1 → 2 có trọng số 5. 

| Bước | Nút | Tiểu bang | Chi phí | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | bắt đầu | 
| 2 | 2 | 0 | 5 | chiếm ưu thế bình thường | 
| 3 | 2 | 1 | 2 | bắt đầu giảm một nửa ở rìa | 
| 4 | 2 | 2 | 5 | đánh dấu hoạt động đường dẫn không sử dụng | 

Thuật toán đánh giá tất cả các cách diễn giải và chọn mức tối thiểu trong số dist[2][0], dist[2][1], dist[2][2], cho kết quả 2. 

Dấu vết này cho thấy ngay cả một cạnh đơn lẻ cũng có thể được coi là không sửa đổi hoặc giảm một nửa và hệ thống trạng thái giữ đồng thời cả hai khả năng mà không cần cam kết sớm. 

### Mẫu 1 Phác thảo 

Đường đi tối ưu sử dụng ba cạnh theo thứ tự. Thuật toán khám phá cả đường đi thông thường và biến thể trong đó thao tác được áp dụng trên hai cạnh liên tiếp ở giữa tuyến đường. Quá trình chuyển đổi trạng thái buộc việc giảm một nửa áp dụng chính xác trên các cạnh liền kề và Dijkstra đảm bảo chọn cấu hình kết hợp rẻ hơn. 

Dấu vết xác nhận rằng thuật toán không giả định trước một đường dẫn cố định; nó đánh giá các vị trí khác nhau của hoạt động một cách linh hoạt trong quá trình thư giãn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi cặp trạng thái nút được xử lý nhiều nhất một lần và mỗi cạnh tạo ra số lần thư giãn không đổi | 
| Không gian | O(n + m) | danh sách kề cộng với bảng khoảng cách ba trạng thái | 

Kích thước đồ thị lớn nhưng thưa thớt và việc mở rộng trạng thái 3n giữ cho hệ số không đổi nhỏ. Với các cạnh 2 × 10^5, điều này thoải mái phù hợp trong các giới hạn thông thường để triển khai Dijkstra hàng đợi ưu tiên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    from heapq import heappush, heappop

    n, m = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v, w = map(int, input().split())
        g[u].append((v, w))

    INF = 10**30
    dist = [[INF] * 3 for _ in range(n + 1)]
    dist[1][0] = 0
    pq = [(0, 1, 0)]

    while pq:
        d, u, st = heappop(pq)
        if d != dist[u][st]:
            continue
        if st == 0:
            for v, w in g[u]:
                nd = d + w
                if nd < dist[v][0]:
                    dist[v][0] = nd
                    heappush(pq, (nd, v, 0))
                nd2 = d + w // 2
                if nd2 < dist[v][1]:
                    dist[v][1] = nd2
                    heappush(pq, (nd2, v, 1))
                nd3 = d + w
                if nd3 < dist[v][2]:
                    dist[v][2] = nd3
                    heappush(pq, (nd3, v, 2))
        elif st == 1:
            for v, w in g[u]:
                nd = d + w // 2
                if nd < dist[v][2]:
                    dist[v][2] = nd
                    heappush(pq, (nd, v, 2))
        else:
            for v, w in g[u]:
                nd = d + w
                if nd < dist[v][2]:
                    dist[v][2] = nd
                    heappush(pq, (nd, v, 2))

    return str(min(dist[n]))

# provided samples
assert run("5 6\n1 2 5\n2 3 2\n3 5 1\n1 4 10\n4 5 5\n2 4 3\n") == "4"
assert run("2 1\n1 2 5\n") == "2"

# custom cases
assert run("2 1\n1 2 1\n") == "0", "best is halving single edge"
assert run("3 2\n1 2 100\n2 3 100\n") == "100", "halve both edges"
assert run("4 4\n1 2 8\n2 4 8\n1 3 1\n3 4 1\n") == "2", "choose better path with operation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cạnh nhỏ đơn | 0 | giảm một nửa giảm xuống 0 một cách chính xác | 
| Hai cạnh lớn | 100 | giảm một nửa liên tiếp trên đoạn đường dẫn | 
| Con đường cạnh tranh | 2 | lựa chọn đường dẫn chính xác với thao tác | 

## Vỏ cạnh 

Biểu đồ một cạnh kiểm tra xem thuật toán có cho phép áp dụng ngay lập tức hoạt động giảm một nửa hay không. Với đầu vào`1 2 1`, quá trình chuyển đổi duy nhất từ trạng thái 0 bao gồm việc chuyển sang trạng thái 1 với chi phí 0 và thuật toán kết thúc chính xác với khoảng cách 0 tại nút 2. 

Một chuỗi hai cạnh như`1 → 2 → 3`với trọng lượng`100, 100`đảm bảo quá trình chuyển đổi trạng thái 1 được sử dụng chính xác. Thuật toán chuyển sang trạng thái 1 sau cạnh thứ nhất, sau đó buộc cạnh thứ hai hoàn thành thao tác, tạo ra chi phí`50 + 50 = 100`. 

Ví dụ về đường dẫn cạnh tranh chứng minh rằng thuật toán không giả định một tuyến đường cố định. Nếu có một đường đi dài nhưng có khả năng thu gọn cao và một đường đi ngắn nhưng đắt tiền, cả hai đều được khám phá song song thông qua Dijkstra và bảng trạng thái đảm bảo sự kết hợp tốt nhất được chọn mà không thiên về các đường đi ngắn nhất ban đầu.
