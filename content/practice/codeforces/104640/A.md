---
title: "CF 104640A - \u041f\u043e\u0431\u0435\u0433 \u041c\u0430\u0439\u043b\u0437\u0430"
description: "Chúng ta có hai biểu đồ có trọng số không có trọng số riêng biệt có cùng tập hợp các nhãn đỉnh từ 1 đến n."
date: "2026-06-29T16:49:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104640
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104640
solve_time_s: 77
verified: true
draft: false
---

[CF 104640A - \u041f\u043e\u0431\u0435\u0433 \u041c\u0430\u0439\u043b\u0437\u0430](https://codeforces.com/problemset/problem/104640/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai biểu đồ có trọng số không có trọng số riêng biệt có cùng tập hợp các nhãn đỉnh từ 1 đến n. Bạn có thể coi chúng như hai thành phố song song, mỗi thành phố có n tòa nhà chọc trời được đặt ở tọa độ giống hệt nhau, nhưng có các tuyến đường di chuyển được phép khác nhau trong mỗi thành phố. 

Bên trong thành phố đầu tiên, một số cặp tòa nhà chọc trời được nối với nhau bằng các cạnh hai chiều với thời gian di chuyển nhất định. Thành phố thứ hai có tập hợp các cạnh hai chiều độc lập với thời gian di chuyển riêng. Ngoài những chuyển động nội bộ này, còn có một quá trình chuyển đổi đặc biệt: từ bất kỳ tòa nhà chọc trời i nào ở thành phố đầu tiên, bạn có thể ngay lập tức “chuyển thế giới” sang tòa nhà chọc trời i ở thành phố thứ hai, trả một chi phí cố định x giây và hoạt động tương tự ngược lại. 

Bạn bắt đầu tại tòa nhà chọc trời ở thế giới thứ nhất và muốn đến tòa nhà chọc trời t ở thế giới thứ hai trong tổng thời gian tối thiểu, kết hợp các bước di chuyển bên trong và chuyển đổi thế giới. 

Cấu trúc chính là đây không phải là hai bài toán đường đi ngắn nhất riêng biệt. Sự di chuyển giữa các thế giới kết hợp hai biểu đồ, vì vậy tuyến đường tối ưu có thể luân phiên giữa các thế giới nhiều lần nếu điều đó giúp giảm chi phí. 

Các ràng buộc có ý nghĩa trực tiếp đối với việc lựa chọn thuật toán. Với n lên tới 100000 và tối đa 10^6 cạnh trên mỗi thế giới, biểu đồ kết hợp có thứ tự vài triệu cạnh cộng với 100000 cạnh chéo. Bất kỳ giải pháp nào cố gắng tính toán lại các đường dẫn ngắn nhất cho mỗi truy vấn hoặc sử dụng các biểu diễn dày đặc sẽ không thành công. Truyền tải đồ thị tuyến tính hoặc gần tuyến tính với hệ số logarit, chẳng hạn như Dijkstra với một đống, là lựa chọn thực tế duy nhất. 

Một sai lầm ngây thơ là thử tính toán những con đường ngắn nhất trong mỗi thế giới một cách riêng biệt và sau đó kết hợp các câu trả lời một cách tham lam. Điều đó không thành công vì việc chuyển đổi thế giới có thể mở khóa các tuyến đường rẻ hơn. 

Ví dụ, giả sử ở thế giới 1 đường đi từ s đến t rất đắt, nhưng từ s đến một số i thì rẻ, và ở thế giới 2 đường đi từ i đến t là rẻ. Lộ trình tối ưu phải chuyển đổi thế giới tại i, điều mà một phép tính riêng biệt sẽ không bao giờ xem xét chính xác. 

Một trường hợp thất bại tinh vi khác là giả sử nhiều nhất một lần chuyển đổi thế giới là đủ. Điều đó là sai vì bạn có thể vào thế giới 2 sớm để bỏ qua một đoạn dài ở thế giới 1, sau đó quay lại thế giới 1 thông qua một chỉ mục khác và sau đó quay lại nếu cần. 

## Phương pháp tiếp cận 

Mô hình tinh thần bạo lực là coi mọi chuỗi chuyển động có thể có trên cả hai thế giới như một cuộc tìm kiếm không gian trạng thái. Mỗi trạng thái được xác định không chỉ bởi nút hiện tại mà còn bởi thế giới hiện tại. Từ một trạng thái (nút, thế giới), bạn có thể đi qua tất cả các cạnh trong thế giới đó hoặc chuyển sang thế giới khác với cùng chỉ số giá x. 

Điều này tạo thành một đồ thị có 2n đỉnh. Mỗi cạnh ban đầu đóng góp vào chính xác một lớp và mọi chỉ mục i đóng góp một cạnh chéo giữa (i, world1) và (i, world2). Chạy đường đi ngắn nhất trên biểu đồ mở rộng này sẽ cho câu trả lời đúng. 

Việc triển khai đơn giản đã dẫn trực tiếp đến thuật toán của Dijkstra. Điểm mấu chốt là mặc dù vấn đề trông giống như hai biểu đồ nhưng thực chất nó là một biểu đồ có trọng số thưa thớt với 2n nút. Sự chuyển đổi xuyên thế giới chỉ là những khía cạnh bổ sung. Một khi sự cải tiến này được nhìn thấy, không cần thêm thủ thuật nào nữa. 

Một giải pháp thay thế chậm hơn có thể là thử lập trình động trên “số lượng công tắc” hoặc liên tục nới lỏng các đường dẫn giữa các biểu đồ, nhưng điều đó thoái hóa thành các phép tính đường dẫn ngắn nhất lặp đi lặp lại và quá chậm dưới 10^6 cạnh. 

Cách tiếp cận đúng chỉ đơn giản là chạy Dijkstra trên biểu đồ kết hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên các đường dẫn/tính toán lại nhiều lần | Hàm mũ / O(k·(n+m) log n) | O(n+m) | Quá chậm | 
| Dijkstra trên đồ thị 2 lớp | O((n + m1 + m2) log n) | O(n + m1 + m2) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xây dựng một biểu đồ với 2n nút. Nút i đại diện cho tòa nhà chọc trời i ở thế giới 1 và nút i + n đại diện cho tòa nhà chọc trời i ở thế giới 2. 

Sau đó, chúng tôi thêm tất cả các cạnh từ thế giới thứ nhất vào nửa đầu tiên, tất cả các cạnh từ thế giới thứ hai vào nửa thứ hai và thêm các cạnh chéo của chi phí x giữa i và i + n. 

Chúng tôi chạy Dijkstra từ nút bắt đầu s ở thế giới 1 và tính khoảng cách đến tất cả các nút. Câu trả lời là khoảng cách đến t ở thế giới 2 hoặc vô tận nếu không thể truy cập được. 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị có 2n đỉnh, chia bài toán thành hai lớp, mỗi lớp cho một thế giới. Sự tách biệt này cho phép chúng ta mã hóa chuyển đổi thế giới dưới dạng các cạnh rõ ràng thay vì logic đặc biệt. 
2. Với mỗi cạnh u, v, c ở thế giới thứ nhất, thêm một cạnh hai chiều giữa u và v ở lớp 1 với trọng số c. Điều này bảo toàn tất cả các chuyển động được phép bên trong thế giới 1 chính xác như đã cho. 
3. Với mọi cạnh u, v, c trong thế giới thứ hai, thêm một cạnh hai chiều giữa u + n và v + n có trọng số c. Điều này phản ánh thế giới thứ hai vào nửa sau của biểu đồ. 
4. Với mọi chỉ số i từ 1 đến n, hãy cộng hai cạnh: i với i + n và i + n với i, mỗi cạnh có giá x. Điều này mô hình dịch chuyển tức thời giữa các tòa nhà chọc trời giống hệt nhau trên khắp thế giới. Tính chính xác dựa trên thực tế là việc chuyển đổi thế giới không thay đổi chỉ mục mà chỉ thay đổi lớp. 
5. Chạy Dijkstra bắt đầu từ s ở thế giới 1, là nút s. Điều này khám phá tất cả các kết hợp có thể có của các chuyển động và chuyển đổi trong thế giới theo thứ tự tổng thời gian tăng dần. 
6. Trích xuất khoảng cách đến nút t trong thế giới 2, là nút t + n. Nếu khoảng cách này vẫn là vô hạn, xuất ra -1. 

### Tại sao nó hoạt động 

Bất kỳ hành trình hợp lệ nào đều tương ứng chính xác với một đường dẫn trong biểu đồ 2n nút được xây dựng, bởi vì mọi loại chuyển động trong bài toán ban đầu đều được biểu diễn dưới dạng một cạnh. Ngược lại, mọi đường đi trong biểu đồ được xây dựng đều tương ứng với một chuỗi các bước di chuyển hợp pháp trong cài đặt ban đầu. Vì tất cả các cạnh đều có trọng số không âm, Dijkstra đảm bảo rằng lần đầu tiên chúng tôi hoàn thiện một nút, chúng tôi đã tìm thấy chi phí tối thiểu có thể để tiếp cận nút đó. Điều này đảm bảo khoảng cách được tính toán tới (t ở thế giới 2) là thời gian di chuyển tối ưu trong số tất cả các tuyến đường có thể có của thế giới hỗn hợp. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**30

def solve():
    n, x = map(int, input().split())
    
    m1 = int(input())
    g = [[] for _ in range(2 * n)]
    
    for _ in range(m1):
        u, v, c = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append((v, c))
        g[v].append((u, c))
    
    m2 = int(input())
    for _ in range(m2):
        u, v, c = map(int, input().split())
        u -= 1
        v -= 1
        u += n
        v += n
        g[u].append((v, c))
        g[v].append((u, c))
    
    for i in range(n):
        g[i].append((i + n, x))
        g[i + n].append((i, x))
    
    s, t = map(int, input().split())
    s -= 1
    t -= 1
    
    dist = [INF] * (2 * n)
    dist[s] = 0
    pq = [(0, s)]
    
    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        if u == t + n:
            break
        for v, w in g[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))
    
    ans = dist[t + n]
    print(-1 if ans == INF else ans)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện việc xây dựng đồ thị 2 lớp. Danh sách lân cận`g`nắm giữ cả hai thế giới cùng một lúc, với các chỉ số [0, n-1] đại diện cho thế giới 1 và [n, 2n-1] đại diện cho thế giới 2. 

Phần tế nhị nhất là xử lý chỉ mục. Tất cả các đỉnh được chuyển đổi sang lập chỉ mục dựa trên 0 và thế giới thứ hai được dịch chuyển theo +n. Các cạnh chéo được thêm đồng đều cho mọi i, đảm bảo tính đối xứng giữa các hướng chuyển hướng. 

Vòng lặp Dijkstra sử dụng hàng đợi ưu tiên tiêu chuẩn. Việc nghỉ sớm khi tiếp cận mục tiêu là tùy chọn nhưng an toàn vì Dijkstra đảm bảo rằng lần đầu tiên chúng ta bắn trúng mục tiêu với khoảng cách tối thiểu là tối ưu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi chỉ theo dõi các chuyển đổi trạng thái quan trọng về mặt khái niệm, tập trung vào khoảng cách ngắn nhất được phát hiện. 

| Bước | Nút được xử lý | Khoảng cách | Tác dụng thư giãn chính | 
| --- | --- | --- | --- | 
| 1 | s (thế giới 1) | 0 | Khởi tạo khả năng tiếp cận | 
| 2 | hàng xóm ở thế giới 1 | khác nhau | Mở rộng trong biểu đồ đầu tiên | 
| 3 | chuyển sang thế giới 2 nút | x | Giới thiệu việc bước vào thế giới thứ hai | 
| 4 | xuyên thế giới nội bộ 2 | cải thiện | Tìm tuyến đường rẻ hơn về phía t | 
| cuối cùng | t ở thế giới 2 | 6 | Thời gian họp tối ưu | 

Dấu vết này cho thấy con đường tối ưu có thể yêu cầu vào thế giới 2 sớm thay vì về đích ở thế giới 1 trước. 

### Đã thi công mẫu 2 

Cho n = 3, x = 5. 

Cạnh thế giới 1: 

1-2 giá 2, 2-3 giá 100 

Thế giới 2 cạnh: 

1-2 giá 1, 2-3 giá 1 

Bắt đầu s = 1, t = 3. 

Thuật toán tìm thấy: 

1 → chuyển sang thế giới 2 với chi phí 5 → 1 ở thế giới 2 

Khi đó 1 → 2 → 3 giá 2 

Tổng cộng = 5 + 2 = 7 

Tuyến đường chỉ dành cho thế giới 1 có giá 2 + 100 = 102, cho thấy lý do tại sao việc chuyển đổi sớm lại chiếm ưu thế. 

| Bước | Vị trí | Chi phí | 
| --- | --- | --- | 
| bắt đầu | 1 (w1) | 0 | 
| chuyển đổi | 1 (w2) | 5 | 
| di chuyển | 2 (w2) | 6 | 
| di chuyển | 3 (w2) | 7 | 

Điều này xác nhận rằng các đường dẫn tối ưu phụ thuộc rất nhiều vào quá trình chuyển đổi giữa các thế giới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m1 + m2) log n) | Dijkstra trên 2n nút với tất cả các cạnh trong một đống thưa thớt | 
| Không gian | O(n + m1 + m2) | danh sách kề cho cả hai thế giới cộng với các cạnh chéo | 

Tổng số cạnh là tuyến tính theo kích thước đầu vào, do đó hệ số logarit từ hàng đợi ưu tiên là chi phí duy nhất. Với m lên tới 2·10^6, điều này thoải mái phù hợp với các giới hạn thông thường của Python nếu được triển khai với danh sách I/O và kề cận hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import inf
    import heapq

    input = sys.stdin.readline

    INF = 10**30

    n, x = map(int, input().split())
    m1 = int(input())
    g = [[] for _ in range(2 * n)]

    for _ in range(m1):
        u, v, c = map(int, input().split())
        u -= 1; v -= 1
        g[u].append((v, c))
        g[v].append((u, c))

    m2 = int(input())
    for _ in range(m2):
        u, v, c = map(int, input().split())
        u -= 1; v -= 1
        u += n; v += n
        g[u].append((v, c))
        g[v].append((u, c))

    for i in range(n):
        g[i].append((i + n, x))
        g[i + n].append((i, x))

    s, t = map(int, input().split())
    s -= 1; t -= 1

    dist = [INF] * (2 * n)
    dist[s] = 0
    pq = [(0, s)]

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in g[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    ans = dist[t + n]
    return "-1" if ans == INF else str(ans)

# provided sample
assert run("""6 2
7
1 3 2
6 4 1
4 1 5
5 3 2
1 2 1
1 5 4
2 3 4
6
4 2 1
2 1 5
5 2 3
3 1 5
1 5 4
2 6 1
5 6
""").strip() == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | 6 | tính đúng đắn của định tuyến thế giới hỗn hợp | 
| n=1, x=10, không có cạnh, s=t=1 | 10 | xử lý công tắc đơn | 
| hai nút chỉ trực tiếp cạnh thế giới thứ hai tốt hơn | giá trị nhỏ | lợi thế xuyên thế giới | 
| đồ thị bị ngắt kết nối | -1 | phát hiện không thể truy cập | 

## Vỏ cạnh 

Trường hợp một cạnh là khi không có cạnh nào trong thế giới cả. Những bước di chuyển duy nhất có thể thực hiện được là chuyển đổi thế giới, vì vậy đường dẫn sẽ trở thành một chuỗi các chuyển đổi xen kẽ đơn giản. Thuật toán xử lý vấn đề này vì Dijkstra vẫn khám phá các cạnh chéo một cách bình thường và câu trả lời sẽ trở thành bội số của x nếu có thể truy cập được. 

Một trường hợp khác là khi đường dẫn tối ưu yêu cầu nhiều nút chuyển. Ví dụ: sử dụng thế giới 2 để đi tắt một phần hành trình, quay lại thế giới 1 để đến khu vực khác, sau đó chuyển lại. Biểu đồ 2n nút đương nhiên cho phép điều này và Dijkstra khám phá các chuỗi như vậy mà không bị hạn chế. 

Trường hợp cuối cùng là khi s bằng t. Ngay cả khi đó, câu trả lời không nhất thiết phải là 0, vì đích đến cụ thể là ở thế giới 2. Thuật toán buộc chính xác ít nhất một lần chuyển đổi trừ khi s đã bằng t và một đường dẫn có chi phí bằng 0 tồn tại ở thế giới 2 mà không rời đi.
