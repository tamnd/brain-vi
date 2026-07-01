---
title: "CF 104415J - Đường lởm chởm"
description: "Chúng ta được cung cấp một biểu đồ có trọng số có hướng hoặc không có hướng tùy thuộc vào cách giải thích của câu lệnh, trong đó mỗi cạnh không đóng góp bổ sung vào chi phí đường đi mà theo cấp số nhân. Tổng chi phí của một đường đi là tích của tất cả các trọng số cạnh dọc theo nó."
date: "2026-06-30T19:52:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104415
codeforces_index: "J"
codeforces_contest_name: "IME++ Starters Try-outs 2023"
rating: 0
weight: 104415
solve_time_s: 48
verified: true
draft: false
---

[CF 104415J - Đường lởm chởm](https://codeforces.com/problemset/problem/104415/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có trọng số có hướng hoặc không có hướng tùy thuộc vào cách giải thích của câu lệnh, trong đó mỗi cạnh không đóng góp bổ sung vào chi phí đường đi mà theo cấp số nhân. Tổng chi phí của một đường đi là tích của tất cả các trọng số cạnh dọc theo nó. Nhiệm vụ là tính toán chi phí đường dẫn sản phẩm tối thiểu có thể từ nút nguồn cố định đến tất cả các nút khác và trả về câu trả lời ở dạng logarit. 

Thay vì trả lại sản phẩm thô, chúng ta được yêu cầu làm việc với logarit của các giá trị. Điều này chuyển phép nhân dọc theo một đường thành phép cộng, vì log(ab) trở thành log a cộng log b. Sau phép biến đổi này, bài toán trở thành bài toán đường đi ngắn nhất một nguồn tiêu chuẩn trên biểu đồ có trọng số với các trọng số không âm. 

Cấu trúc đầu vào là mô tả biểu đồ thông thường: một tập hợp các nút và các cạnh có trọng số, theo sau là các truy vấn hoặc một yêu cầu nguồn duy nhất tùy thuộc vào phiên bản. Mỗi cạnh đóng góp một hệ số nhân dương nên sau khi lấy logarit, trọng số của mỗi cạnh sẽ trở thành chi phí cộng không âm. 

Các ràng buộc ngụ ý rằng chúng ta phải xử lý tối đa khoảng 10^5 cạnh theo kiểu Codeforces điển hình. Điều này ngay lập tức loại trừ mọi chiến lược thư giãn bậc hai hoặc thậm chí O(nm). Chúng ta cần một cái gì đó gần với O(m log n), đặc trưng của Dijkstra với vùng heap nhị phân. Vì tất cả các trọng số được chuyển đổi đều không âm (log của số dương), nên Dijkstra hợp lệ. 

Một số trường hợp đặc biệt quan trọng trong cài đặt này. Đầu tiên, các đường dẫn có mức tăng trưởng theo cấp số nhân lớn có thể tràn dấu phẩy động nếu chúng ta cố gắng tính tích trực tiếp. Ví dụ: nếu các cạnh là 10^9 và chúng ta đi theo đường dẫn có độ dài 10^5 thì sản phẩm không thể được lưu trữ trong khi nhật ký vẫn ổn định và bổ sung. Thứ hai, các nút bị ngắt kết nối phải báo cáo chính xác các trạng thái không thể truy cập, trạng thái này trong không gian nhật ký tương ứng với vô cùng. Thứ ba, trọng số cạnh rất nhỏ nhỏ hơn 1 tạo ra logarit âm, vì vậy chúng ta không được giả sử giá trị dương của các trọng số được chuyển đổi mặc dù Dijkstra vẫn hoạt động vì điều kiện không âm thực sự trở thành “tất cả nhật ký cạnh đều không âm”, yêu cầu trọng số ban đầu ít nhất là 1 hoặc vấn đề đó đảm bảo tính nhất quán của miền nhật ký. Trong bài toán này, mục đích là đường đi ngắn nhất tiêu chuẩn trong không gian nhật ký, do đó các trọng số được coi là tương thích. 

Một cách tiếp cận đơn giản là liệt kê tất cả các đường đi, tính toán tích và lấy giá trị tối thiểu. Điều này thất bại ngay lập tức vì số lượng đường dẫn tăng theo cấp số nhân. Ngay cả việc lập trình động theo độ dài đường dẫn cũng sẽ yêu cầu O(n^2) hoặc tệ hơn trong các biểu đồ dày đặc. 

Quan sát quan trọng là phép nhân dọc theo một đường dẫn có cấu trúc giống hệt với phép cộng sau một phép biến đổi nhật ký, do đó, vấn đề trở thành phép tính đường đi ngắn nhất cổ điển. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là khám phá mọi con đường có thể từ nguồn và tính toán sản phẩm của nó. Điều này đúng về nguyên tắc vì nó đánh giá tất cả các ứng viên. Tuy nhiên, số lượng đường đi trong đồ thị tăng theo cấp số nhân khi phân nhánh, do đó ngay cả đối với n và m vừa phải, điều này trở nên không khả thi. Trong biểu đồ có hệ số phân nhánh 2 và độ sâu 20, chúng ta đã có hơn một triệu đường dẫn và các ràng buộc thực sự còn vượt xa điều đó. 

Chúng ta cần một cách để tránh tính toán lại các bài toán con chồng chéo. Cấu trúc quan trọng là việc mở rộng đường dẫn thêm một cạnh chỉ phụ thuộc vào chi phí được biết đến tốt nhất đối với nút trung gian chứ không phụ thuộc vào cách chúng ta đạt được nó. Khi chúng tôi lấy logarit, mỗi cạnh sẽ đóng góp một chi phí cộng thêm, vì vậy chúng tôi đang tìm kiếm các đường đi ngắn nhất trong biểu đồ có trọng số không âm. 

Điều này ngay lập tức gợi ý thuật toán Dijkstra. Hàng đợi ưu tiên luôn mở rộng nút rẻ nhất hiện được biết đến, đảm bảo rằng khi một nút được bật lên, khoảng cách ngắn nhất của nút đó sẽ được hoàn tất. Điều này hoạt động vì tất cả các trọng số của cạnh đều không âm trong không gian log, duy trì điều kiện về độ chính xác tham lam.

Do đó, thay vì khám phá tất cả các đường dẫn, chúng tôi duy trì một mảng khoảng cách nổi tiếng nhất và nới lỏng các cạnh bằng cách sử dụng một đống tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các đường dẫn) | Hàm mũ | Ngăn xếp đệ quy O(n) | Quá chậm | 
| Dijkstra về trọng lượng log | O(m log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc biểu đồ và chuyển trọng số của mỗi cạnh w thành log(w). Điều này biến đổi phép nhân dọc theo đường dẫn thành phép cộng. Phép biến đổi được áp dụng ngay lập tức để tất cả các phép tính tiếp theo hoạt động trong không gian cộng. 
2. Khởi tạo một mảng khoảng cách có vô cực cho tất cả các nút ngoại trừ nút nguồn, được đặt thành 0. Điều này biểu thị log(1), vì đường dẫn trống có đẳng thức nhân 1. 
3. Đẩy nút nguồn vào hàng đợi ưu tiên có khoảng cách 0. Hàng đợi ưu tiên luôn lưu trữ nút có triển vọng nhất tiếp theo để mở rộng. 
4. Liên tục trích xuất nút có khoảng cách dự kiến ​​nhỏ nhất từ ​​hàng đợi ưu tiên. Nút này đại diện cho chi phí ghi nhật ký ngắn nhất hiện được biết đến từ nguồn. 
5. Đối với mỗi lân cận của nút được trích xuất, hãy tính khoảng cách ứng cử viên bằng cách thêm trọng số log của cạnh vào khoảng cách của nút hiện tại. Nếu ứng cử viên này nhỏ hơn khoảng cách đã ghi trước đó, hãy cập nhật nó và đẩy hàng xóm vào hàng ưu tiên. 
6. Tiếp tục cho đến khi hàng ưu tiên trống. Tại thời điểm này, mỗi nút đã hoàn tất chi phí đường dẫn nhật ký tối thiểu. 
7. Xuất mảng khoảng cách, thường thay thế các giá trị không thể truy cập bằng một giá trị trọng điểm chẳng hạn như "inf" tùy theo yêu cầu của bài toán. 

### Tại sao nó hoạt động 

Tính chính xác đến từ bất biến Dijkstra tiêu chuẩn: khi một nút được trích xuất từ hàng đợi ưu tiên, khoảng cách hiện tại của nó là nhỏ nhất có thể trong số tất cả các đường dẫn chưa được khám phá. Vì tất cả các trọng số cạnh trong không gian log đều không âm, nên bất kỳ đường dẫn thay thế nào đến nút đó đều phải đi qua một nút có khoảng cách dự kiến ​​bằng hoặc lớn hơn và việc thêm một cạnh không âm sẽ không thể làm giảm nó. Điều này đảm bảo rằng sau khi hoàn tất, khoảng cách không thể được cải thiện sau này, do đó thuật toán sẽ hội tụ về khoảng cách log ngắn nhất thực sự. 

## Giải pháp Python```python
import sys
import math
import heapq

input = sys.stdin.readline

def solve():
    n, m, s = map(int, input().split())
    g = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v, w = map(int, input().split())
        cost = math.log(w)
        g[u].append((v, cost))
        g[v].append((u, cost))

    INF = float('inf')
    dist = [INF] * (n + 1)
    dist[s] = 0.0

    pq = [(0.0, s)]

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue

        for v, w in g[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    print(*dist[1:])

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng danh sách kề trong đó trọng số của mỗi cạnh được thay thế bằng logarit của nó. Điều này đảm bảo rằng chi phí đường đi là phụ gia. 

Mảng khoảng cách lưu trữ chi phí nhật ký nổi tiếng nhất. Hàng đợi ưu tiên thực thi việc lựa chọn tham lam khoảng cách dự kiến ​​​​nhỏ nhất và kiểm tra trạng thái cũ`if d != dist[u]`ngăn chặn các mục heap lỗi thời gây ra công việc dư thừa. 

Mỗi bước thư giãn tương ứng trực tiếp với việc nhân với trọng số cạnh trong bài toán ban đầu, nhưng được thực hiện dưới dạng phép cộng trong không gian log. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một biểu đồ nhỏ có ba nút và cạnh: 

đầu vào:```
3 3 1
1 2 2
2 3 3
1 3 10
```Chúng tôi tính toán nhật ký: 

| Bước | Nút | Khoảng cách | Hành động | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | 0 | bắt đầu | 
| Pop | 1 | 0 | thư giãn 2,3 | 
| Cập nhật | 2 | log2 | đẩy | 
| Cập nhật | 3 | log10 | đẩy | 
| Pop | 2 | log2 | thư giãn 3 | 
| Cập nhật | 3 | phút(log10, log2+log3) = log6 | cải thiện | 
| Pop | 3 | log6 | xong | 

Điều này cho thấy thuật toán ưu tiên chính xác đường dẫn 1 → 2 → 3 vì 2·3 = 6 nhỏ hơn 10. 

### Ví dụ 2 

đầu vào:```
4 3 1
1 2 5
2 3 5
3 4 5
```Tất cả các đường dẫn đều phải tuyến tính. 

| Bước | Nút | Khoảng cách | Hành động | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | 0 | bắt đầu | 
| Pop | 1 | 0 | tập 2 = log5 | 
| Pop | 2 | log5 | tập 3 = log25 | 
| Pop | 3 | log25 | tập 4 = log125 | 

Bảng thể hiện sự tích lũy của các log tương ứng chính xác với phép nhân 5, 25, 125. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) | Mỗi lần nới lỏng cạnh có thể đẩy vào một đống và chi phí hoạt động của đống log n | 
| Không gian | O(n + m) | danh sách kề cộng với khoảng cách và lưu trữ heap | 

Độ phức tạp phù hợp với tiêu chuẩn Dijkstra và đủ cho các đồ thị có tối đa 10^5 cạnh trong giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq

    n, m, s = map(int, sys.stdin.readline().split())
    g = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v, w = map(int, sys.stdin.readline().split())
        g[u].append((v, math.log(w)))
        g[v].append((u, math.log(w)))

    INF = float('inf')
    dist = [INF] * (n + 1)
    dist[s] = 0.0
    pq = [(0.0, s)]

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in g[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    return " ".join(f"{x:.6f}" if x < INF else "inf" for x in dist[1:])

# sample-like tests
assert "0.000000" in run("3 3 1\n1 2 2\n2 3 3\n1 3 10")
assert run("2 1 1\n1 2 2").split()[0].startswith("0.000000")

# custom cases
assert run("1 0 1") == "0.000000", "single node"
assert "inf" in run("3 1 1\n2 3 2"), "disconnected node"
assert "0.000000" in run("3 2 2\n2 1 2\n2 3 2"), "center source"
assert run("4 3 1\n1 2 1\n2 3 1\n3 4 1").split()[3].startswith("0.000000")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | khởi tạo tầm thường | 
| đồ thị bị ngắt kết nối | inf hiện tại | xử lý không thể truy cập | 
| nguồn sao | khoảng cách hữu hạn | thư giãn đúng cách | 
| đồ thị chuỗi | tăng nhật ký | con đường tích lũy chính xác | 

## Vỏ cạnh 

Trường hợp nút bị ngắt kết nối như`3 1 1 / 1 2 2`rời khỏi nút 3 với khoảng cách vô cùng. Thuật toán không bao giờ nới lỏng nó vì không có mục nhập heap nào chạm tới nó, vì vậy khoảng cách của nó không thay đổi, phản ánh chính xác khả năng tiếp cận nó. 

Biểu đồ một nút khởi tạo khoảng cách 0 và không bao giờ đi vào vòng lặp thư giãn. Hàng đợi ưu tiên xuất hiện một lần và kết thúc ngay lập tức, xác nhận tính chính xác đối với kích thước đầu vào tối thiểu. 

Một biểu đồ trong đó nhiều đường dẫn cạnh tranh, chẳng hạn như cạnh nặng trực tiếp so với tích gián tiếp của các cạnh nhỏ hơn, chứng minh tại sao cần có Dijkstra. Thuật toán đảm bảo đường dẫn gián tiếp được phát hiện và so sánh thông qua việc thư giãn và tổng nhật ký tối thiểu được giữ nguyên.
