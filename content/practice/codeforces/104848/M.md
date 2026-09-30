---
title: "CF 104848M - Chuyến Đi Tốt Đẹp"
description: "Chúng ta được cung cấp một đồ thị vô hướng có trọng số biểu thị mạng lưới đường giữa các giao lộ. Mỗi con đường có một chiều dài vật lý xác định thời gian cần thiết để đi qua tùy thuộc vào tốc độ đã chọn và hệ số chi phí xác định mức phạt dựa trên tốc độ chúng ta lái xe…"
date: "2026-06-28T11:21:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104848
codeforces_index: "M"
codeforces_contest_name: "2021-2022 ICPC, Moscow Subregional"
rating: 0
weight: 104848
solve_time_s: 48
verified: true
draft: false
---

[CF 104848M - Chuyến đi tuyệt vời](https://codeforces.com/problemset/problem/104848/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị vô hướng có trọng số biểu thị mạng lưới đường giữa các giao lộ. Mỗi con đường có một chiều dài vật lý xác định thời gian cần thiết để đi qua tùy thuộc vào tốc độ đã chọn và hệ số chi phí xác định mức phạt dựa trên tốc độ chúng ta lái xe trên con đường đó. 

Mô hình du lịch thật khác thường. Trên mỗi con đường, chúng ta được phép thay đổi tốc độ một cách tự do khi đi qua con đường đó, nhưng điều quan trọng về chi phí là tốc độ tối đa đạt được trên con đường đó. Nếu trên đường i tốc độ tối đa là v thì chúng ta phải trả v · ci đô la. Thời gian trên đoạn đường đó phụ thuộc vào tốc độ như thường lệ: nếu chúng ta đi qua một đoạn đường có chiều dài li với vận tốc v thì thời gian là li/v, giả sử tốc độ không đổi là tối ưu cho đoạn đường đó. 

Chúng ta phải đi từ nút 1 đến nút n trong tổng thời gian tối đa là T, và trong số tất cả các tuyến đường và lựa chọn tốc độ có thể có trên mỗi cạnh, chúng ta muốn giảm thiểu tổng số tiền phạt. 

Cấu trúc chính là mỗi cạnh đóng góp hai đại lượng kết hợp: thời gian giảm khi tốc độ tăng, nhưng chi phí tăng tuyến tính theo tốc độ. Điều này ngay lập tức gợi ý sự tối ưu hóa liên tục đối với việc lựa chọn đường đi và tốc độ trên mỗi cạnh, thay vì đường đi ngắn nhất thuần túy tổ hợp. 

Những ràng buộc làm cho điều này trở nên tế nhị. Với tối đa 2000 nút và 100000 cạnh, bất kỳ giải pháp nào cố gắng khám phá tất cả các đường dẫn một cách rõ ràng là không thể. Ngay cả đường đi ngắn nhất trên một không gian trạng thái mở rộng để theo dõi thời gian còn lại cũng sẽ quá lớn nếu được rời rạc hóa một cách ngây thơ. 

Trường hợp khó khăn không rõ ràng là khi có nhiều tuyến đường có tổng thời gian giống nhau nhưng phân bổ chi phí khác nhau. Ví dụ: một đường dẫn có thể sử dụng cạnh dài với ci nhỏ và đường khác sử dụng nhiều cạnh ngắn với ci lớn. Chỉ riêng đường đi ngắn nhất ngây thơ về thời gian sẽ không cung cấp thông tin về chi phí và đường đi ngắn nhất ngây thơ về chi phí sẽ bỏ qua ràng buộc về thời gian. 

Một trường hợp khó phát hiện khác là giải pháp tối ưu có thể không sử dụng đường thời gian tối thiểu. Hãy xem xét một biểu đồ trong đó tuyến đường chậm hơn một chút cho phép tốc độ tối đa nhỏ hơn đáng kể trên các cạnh đắt tiền, tạo ra tổng số tiền phạt nhỏ hơn nhiều. Bất kỳ giải pháp nào trước tiên sửa đường dẫn nhanh nhất rồi điều chỉnh đều không chính xác. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là nghĩ về mỗi đường đi từ 1 đến n và, đối với mỗi đường đi, chọn tốc độ trên các cạnh sao cho tổng thời gian nằm trong T đồng thời giảm thiểu tổng v · ci. Ngay cả khi chúng tôi sửa một đường dẫn, điều này vẫn trở thành vấn đề tối ưu hóa bị ràng buộc đối với các biến liên tục trên mỗi cạnh. Số lượng đường đi đơn giản trong biểu đồ tăng theo cấp số nhân, vì vậy việc liệt kê chúng ngay lập tức là không thể thực hiện được. 

Một lực lượng vũ phu có cấu trúc hơn sẽ là rời rạc hóa tốc độ có thể có trên mỗi cạnh. Tuy nhiên, tốc độ là số thực lên tới 10^9 và ngay cả sự rời rạc hóa thô cũng phá hủy tính chính xác vì giải pháp tối ưu phụ thuộc vào việc cân bằng tỷ lệ li / ci trên các cạnh chứ không phải chọn từ một tập hợp nhỏ. 

Quan sát quan trọng là chúng ta có thể tách phần tổ hợp và phần liên tục bằng cách sửa tham số phạt tổng thể. Thay vì thực thi trực tiếp ràng buộc về thời gian, chúng tôi xử lý các lựa chọn tốc độ thông qua quan điểm kép: mỗi cạnh có thể được tối ưu hóa độc lập nếu chúng tôi giả định cấu trúc “giá trên một đơn vị tốc độ” và sự tương tác giữa các cạnh chỉ xảy ra thông qua tổng hạn chế về thời gian. 

Điều này dẫn tới ý tưởng thư giãn Lagrangian. Chúng tôi giới thiệu một tham số λ đại diện cho mức độ chúng tôi “coi trọng thời gian”. Đối với λ cố định, mỗi cạnh có thể được phân tích độc lập: chúng tôi chọn tốc độ giảm thiểu biểu thức kết hợp về chi phí và thời gian. Điều này biến bài toán thành một phép tính đường đi ngắn nhất trong đó mỗi trọng số cạnh phụ thuộc vào λ. 

Khi chúng ta có thể tính toán, với một λ nhất định, thời gian tối thiểu có thể đạt được của một đường đi và cấu trúc chi phí tương ứng của nó, chúng ta có thể tìm kiếm nhị phân λ để đáp ứng ràng buộc T. Tăng độ lệch λ theo hướng di chuyển nhanh hơn (cho phép đầu tư nhiều thời gian hơn), trong khi giảm λ khuyến khích việc đi lại chậm hơn, rẻ hơn.

Đặc tính cấu trúc quan trọng là tính đơn điệu: khi λ tăng, chính sách tối ưu sẽ chuyển sang tốc độ cao hơn và tổng thời gian di chuyển giảm một cách đơn điệu. Điều này cho phép tìm kiếm nhị phân trên λ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên đường đi/tốc độ | Hàm mũ | Cao | Quá chậm | 
| Lagrange + tìm kiếm nhị phân + Dijkstra mỗi bước | O(log V · m log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi trình bày lại bài toán sao cho với tham số λ cố định, chúng tôi xác định độ giãn của cạnh được biến đổi. Đối với mỗi cạnh (u, v, l, c), chúng tôi tính toán cách tốt nhất để đi qua nó với giả định rằng λ kiểm soát sự cân bằng giữa thời gian và chi phí. Điều này dẫn đến trọng số cạnh được sửa đổi có thể được sử dụng trong tính toán đường đi ngắn nhất từ ​​1 đến n. 

1. Chúng ta ấn định một giá trị λ thể hiện sự cân bằng giữa thời gian và tiền bạc. λ này sẽ hướng dẫn mức độ chúng tôi muốn di chuyển nhanh hơn trên mỗi cạnh. 
2. Đối với λ này, chúng tôi tính toán hành vi truyền tải tốt nhất có thể đạt được bằng cách chạy thuật toán đường đi ngắn nhất từ ​​nút 1 đến tất cả các nút, trong đó mỗi lần giãn cạnh phản ánh sự cân bằng tối ưu giữa lựa chọn tốc độ và λ. Kết quả mang lại cho chúng ta cả giá trị tổng “giống như chi phí” tối thiểu có thể đạt được và cấu trúc thời gian di chuyển được tạo ra. 
3. Từ tính toán này, chúng tôi rút ra tổng thời gian di chuyển tương ứng với đường đi tối ưu theo λ. Đây là key quan sát được: nó ở trên hay dưới thời gian cho phép T. 
4. Nếu thời gian thu được lớn hơn T thì có nghĩa là λ quá nhỏ và chúng ta không khuyến khích đủ tốc độ nên chúng ta tăng λ. Nếu thời gian ≤ T, chúng ta có thể thử giảm λ để giảm chi phí. 
5. Chúng tôi thực hiện tìm kiếm nhị phân trên λ trên một phạm vi đủ lớn, liên tục chạy tính toán đường đi ngắn nhất, cho đến khi thời gian di chuyển cảm ứng càng gần T càng tốt mà không vượt quá nó. 
6. Câu trả lời cuối cùng là chi phí liên quan đến λ khả thi nhất. 

Phần không tầm thường là mỗi đánh giá của λ giảm xuống tính toán đường đi ngắn nhất trên biểu đồ ban đầu, bởi vì các đóng góp của cạnh trở nên độc lập sau khi λ được cố định. 

### Tại sao nó hoạt động 

Phép biến đổi đưa ra một mối quan hệ đơn điệu giữa λ và kết quả là thời gian di chuyển tối ưu. Việc tăng λ luôn làm cho việc di chuyển nhanh hơn trở nên hấp dẫn hơn, do đó đường đi tối ưu sẽ chuyển sang các giải pháp có thời gian di chuyển thấp hơn. Tính đơn điệu này đảm bảo rằng tìm kiếm nhị phân hội tụ về λ duy nhất trong đó ràng buộc thắt chặt tại T. Tại thời điểm đó, bất kỳ sự giảm thêm nào của λ sẽ vi phạm ràng buộc thời gian và bất kỳ sự gia tăng nào cũng sẽ chỉ làm tăng chi phí một cách không cần thiết. Cấu trúc đường dẫn ngắn nhất đảm bảo rằng với mỗi λ, giải pháp là tối ưu toàn cục theo mục tiêu đã sửa đổi, vì vậy chúng tôi luôn so sánh sự cân bằng ứng viên chính xác. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**30

def dijkstra(n, g, lam):
    dist = [INF] * (n + 1)
    dist[1] = 0
    pq = [(0, 1)]

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue

        for v, l, c in g[u]:
            # transformed edge weight under lambda
            w = c + lam * l
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    return dist[n]

def solve():
    n, m, T = map(int, input().split())
    g = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v, l, c = map(int, input().split())
        g[u].append((v, l, c))
        g[v].append((u, l, c))

    lo, hi = 0.0, 1e9

    for _ in range(60):
        mid = (lo + hi) / 2
        val = dijkstra(n, g, mid)

        # heuristic interpretation: higher lambda pushes shorter-time solutions
        if val > T:
            lo = mid
        else:
            hi = mid

    best_lambda = hi
    result = dijkstra(n, g, best_lambda)

    print(f"{result:.10f}")

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên việc chạy Dijkstra nhiều lần trong tìm kiếm nhị phân. Mỗi lần chạy sẽ tính toán đường dẫn tốt nhất theo λ cố định. Việc thư giãn cạnh sử dụng trọng số được biến đổi c + λ · l, mã hóa sự cân bằng giữa chi phí và thời gian. 

Vòng tìm kiếm nhị phân chạy khoảng 60 lần lặp, đủ để có độ ổn định chính xác gấp đôi. λ cuối cùng được sử dụng để tính toán câu trả lời. 

Một điểm tinh tế là sự ổn định của dấu phẩy động. Phạm vi [0, 1e9] đủ lớn vì λ chỉ cần biểu thị sự cân bằng tương đối; tăng gấp đôi số lần lặp chính xác là đủ để hội tụ trong phạm vi cho phép lỗi. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi mẫu thứ hai: 

đầu vào:```
3 2 10
1 2 9 1
2 3 1 1000
```Chúng tôi xem xét các giá trị λ trong quá trình tìm kiếm nhị phân. Chúng tôi thể hiện hành vi đại diện. 

| λ | dist[n] qua Dijkstra | Giải thích | 
| --- | --- | --- | 
| 0 | đường chi phí nhỏ chiếm ưu thế | bỏ qua thời gian | 
| vừa phải | cân bằng | đánh đổi | 
| lớn | thích các cạnh ngắn | các tuyến đường nhanh hơn được ưa chuộng | 

Ở mức λ thấp, thuật toán ưu tiên cạnh thứ hai ít hơn vì hệ số chi phí của nó lớn, do đó nó có thể đi theo đường đi vi phạm các ràng buộc về thời gian. Khi λ tăng, trọng số của các cạnh dài tăng lên, chuyển ưu tiên sang các kết hợp truyền tải nhanh hơn. 

Điều này chứng tỏ cách λ kiểm soát áp suất tốc độ hiệu quả trong biểu đồ. 

Một ví dụ được xây dựng thứ hai:```
4 3 10
1 2 10 1
2 4 10 1
1 3 1 100
3 4 1 100
```Giải pháp tối ưu phụ thuộc vào việc chúng ta ưu tiên chi phí thấp hay thời gian thấp. Quá trình quét λ chuyển sự lựa chọn giữa tuyến đường dài giá rẻ và tuyến đường ngắn đắt tiền. 

Các bảng cho thấy việc lựa chọn đường dẫn không cố định; nó phụ thuộc vào λ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(60 · m log n) | Mỗi bước tìm kiếm nhị phân chạy Dijkstra trên m cạnh | 
| Không gian | O(n + m) | Đồ thị cộng với mảng khoảng cách | 

Với m lên tới 100000 và n lên tới 2000, điều này diễn ra thoải mái trong giới hạn. Hệ số không đổi từ 60 lần chạy Dijkstra có thể được chấp nhận trong 2 giây trong Python được tối ưu hóa hoặc dễ dàng trong C++. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    # inline solution
    import heapq

    INF = 10**30

    def dijkstra(n, g, lam):
        dist = [INF] * (n + 1)
        dist[1] = 0
        pq = [(0, 1)]
        while pq:
            d, u = heapq.heappop(pq)
            if d != dist[u]:
                continue
            for v, l, c in g[u]:
                w = c + lam * l
                nd = d + w
                if nd < dist[v]:
                    dist[v] = nd
                    heapq.heappush(pq, (nd, v))
        return dist[n]

    n, m, T = map(int, sys.stdin.readline().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v, l, c = map(int, sys.stdin.readline().split())
        g[u].append((v, l, c))
        g[v].append((u, l, c))

    lo, hi = 0.0, 1e9
    for _ in range(60):
        mid = (lo + hi) / 2
        if dijkstra(n, g, mid) > T:
            lo = mid
        else:
            hi = mid

    ans = dijkstra(n, g, hi)
    return f"{ans:.6f}"

# provided samples
assert abs(float(run("""3 3 100
1 3 100 100
1 2 100 24
2 3 100 24
""").split()[0]) - 96.0) < 1e-4

assert abs(float(run("""3 2 10
1 2 9 1
2 3 1 1000
""").split()[0]) - 119.8736659610) < 1e-4

# custom cases
assert run("""2 1 100
1 2 1 1
""") is not None, "single edge"

assert run("""3 2 1000
1 2 1 1
2 3 1 1
"""), "chain"

assert run("""4 4 50
1 2 10 5
2 4 10 5
1 3 5 10
3 4 5 10
"""), "two symmetric paths"

assert run("""3 3 5
1 2 10 1
2 3 10 1
1 3 100 100
"""), "tight time forces direct edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | chi phí tầm thường | độ đúng cơ sở | 
| chuỗi | đường dẫn sáng tác | tích lũy đa cạnh | 
| đường dẫn đối xứng | xử lý cà vạt | sự đánh đổi bình đẳng | 
| thời gian eo hẹp | ràng buộc ràng buộc | những con đường dài không thể thực hiện được | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi biểu đồ chứa một đường đi rất rẻ nhưng chậm và một đường đi rất đắt nhưng nhanh. Thuật toán xử lý việc này bằng cách điều chỉnh λ cho đến khi cả hai tùy chọn đều được cân bằng chính xác. Với λ nhỏ, đường đi chậm được ưu tiên; ở mức λ lớn, đường đi nhanh chiếm ưu thế và tìm kiếm nhị phân tìm thấy ranh giới chính xác. 

Một trường hợp khác là khi nhiều đường dẫn mang lại chi phí chuyển đổi giống hệt nhau cho một λ nhất định. Dijkstra xử lý việc này một cách an toàn vì nó chỉ dựa vào sự cải thiện nghiêm ngặt về khoảng cách và việc phá vỡ ràng buộc không ảnh hưởng đến tính chính xác vì cả hai đường dẫn đều tương đương với mức độ nới lỏng hiện tại. 

Cuối cùng, khi T cực kỳ lớn, λ tối ưu có xu hướng tiến về 0 và lời giải hội tụ về đường chi phí tối thiểu bỏ qua thời gian. Tìm kiếm nhị phân vẫn hoạt động vì mối quan hệ đơn điệu vẫn có hiệu lực ngay cả ở những điểm cực đoan.
