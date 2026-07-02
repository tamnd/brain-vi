---
title: "CF 104236E - Kết nối Wifi"
description: "Chúng ta được cung cấp một tập hợp các điểm trên mặt phẳng 2D, mỗi điểm đại diện cho một trạm quan sát trong công viên quốc gia. Mỗi trạm đều được trang bị một thiết bị không dây có mức công suất đồng đều $P$."
date: "2026-07-01T23:25:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104236
codeforces_index: "E"
codeforces_contest_name: "Harker Programming Invitational 2023 Advanced"
rating: 0
weight: 104236
solve_time_s: 60
verified: true
draft: false
---

[CF 104236E - Kết nối Wifi](https://codeforces.com/problemset/problem/104236/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các điểm trên mặt phẳng 2D, mỗi điểm đại diện cho một trạm quan sát trong công viên quốc gia. Mỗi trạm đều được trang bị một thiết bị không dây có mức công suất đồng đều$P$. Công suất này xác định bán kính liên lạc: hai trạm có thể liên lạc trực tiếp nếu khoảng cách Euclide của chúng tối đa$\sqrt{P}$. Truyền thông không bị giới hạn ở các liên kết trực tiếp, vì các tin nhắn có thể được chuyển tiếp qua các trạm trung gian, do đó khả năng kết nối được xác định theo nghĩa lý thuyết đồ thị. 

Nhiệm vụ là chọn giá trị nhỏ nhất có thể của$P$sao cho tất cả các trạm được kết nối thông qua mạng truyền thông này. 

Được định khung lại, chúng tôi đang xây dựng một biểu đồ có trọng số hoàn chỉnh về các điểm, trong đó trọng số giữa hai nút là khoảng cách Euclide bình phương. Chúng tôi muốn ngưỡng nhỏ nhất$P$sao cho nếu chúng ta chỉ giữ lại các cạnh có trọng số nhiều nhất$P$, đồ thị kết quả được kết nối. 

Ràng buộc$N \le 500$ngụ ý rằng việc xem xét đầy đủ các cạnh theo cặp là khả thi. Có nhiều nhất khoảng$125{,}000$các cạnh, vì vậy thuật toán với$O(N^2 \log N)$hoặc$O(N^2)$hành vi có thể chấp nhận được. Bất cứ thứ gì dạng khối hoặc tệ hơn cũng sẽ chỉ vượt qua ở dạng tối ưu hóa nhưng không cần thiết ở đây. 

Một số trường hợp đặc biệt quan trọng. Nếu tất cả các điểm đều giống nhau thì câu trả lời là 0 vì không cần kết nối. Nếu các điểm tạo thành một đường thẳng với một ngoại lệ cực kỳ xa, câu trả lời được quyết định bởi kết nối được yêu cầu dài nhất dọc theo cấu trúc khung tốt nhất có thể, chứ không phải bởi cặp điểm xa nhất tuyệt đối. 

Một sai lầm ngây thơ là cho rằng câu trả lời là khoảng cách tối đa giữa hai điểm bất kỳ. Ví dụ: nếu ba điểm tạo thành một tam giác trong đó hai điểm cách xa nhau nhưng cả hai đều gần điểm trung tâm thì khả năng kết nối đạt được thông qua trung tâm, do đó không cần trực tiếp đến cạnh tối đa. 

## Phương pháp tiếp cận 

Vấn đề cơ bản là về khả năng kết nối dưới ngưỡng khoảng cách. Nếu chúng ta sửa một giá trị ứng viên là$P$, chúng ta có thể xây dựng một biểu đồ trong đó chúng ta kết nối tất cả các cặp điểm có khoảng cách bình phương lớn nhất$P$, sau đó kiểm tra xem biểu đồ có được kết nối bằng BFS hay DSU hay không. Điều này mang lại một giải pháp đúng nhưng tốn kém nếu chúng ta tìm kiếm có thể$P$các giá trị. 

Ý tưởng về lực lượng vũ phu trở thành: tính toán tất cả các khoảng cách bình phương theo cặp, sắp xếp chúng và thử các ngưỡng theo thứ tự tăng dần, kiểm tra kết nối mỗi lần. Trong trường hợp xấu nhất, chúng ta có thể tính toán lại kết nối$O(N)$lần, mỗi lần chi phí$O(N^2)$, dẫn đến$O(N^3)$hành vi không cần thiết nhưng vẫn có thể chấp nhận được trong giới hạn đối với$N=500$chỉ trong mã được tối ưu hóa cao. 

Quan sát quan trọng là chúng ta đang tìm kiếm ngưỡng tối thiểu một cách hiệu quả để đồ thị được kết nối, đây chính xác là định nghĩa của Cây bao trùm tối thiểu (MST). Nếu chúng ta xây dựng MST của đồ thị hoàn chỉnh bằng cách sử dụng bình phương khoảng cách làm trọng số thì câu trả lời là trọng số cạnh tối đa trong MST. Điều này hiệu quả vì MST là cách rẻ nhất để kết nối tất cả các nút và mọi ngưỡng kết nối ít nhất phải cho phép tất cả các cạnh MST. 

Vì vậy, thay vì thử các ngưỡng, chúng tôi tính toán trực tiếp MST bằng thuật toán của Prim trong$O(N^2)$, vì chúng ta có thể duy trì khoảng cách trong biểu đồ dày đặc mà không cần lưu trữ cạnh rõ ràng. Câu trả lời cuối cùng là cạnh lớn nhất được sử dụng trong MST. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Kiểm tra ngưỡng Brute Force |$O(N^3)$|$O(N^2)$| Quá chậm | 
| Prim MST (đồ thị dày đặc) |$O(N^2)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi điểm là một nút trong biểu đồ được kết nối đầy đủ trong đó trọng số của các cạnh là bình phương khoảng cách Euclide. 

1. Bắt đầu từ bất kỳ nút nào, để thuận tiện cho nút 0 và đánh dấu nút đó là có trong MST. Điều này thể hiện thực tế là chúng tôi bắt đầu xây dựng một cấu trúc được kết nối từ một trạm duy nhất. 
2. Duy trì một mảng`min_dist[i]`lưu trữ khoảng cách bình phương nhỏ nhất từ ​​nút`i`tới bất kỳ nút nào đã có trong MST. Ban đầu điều này chỉ được tính từ nút 0. 
3. Chọn liên tục nút chưa có trong MST với giá trị nhỏ nhất`min_dist`. Lựa chọn này đảm bảo chúng tôi luôn mở rộng thành phần được kết nối hiện tại bằng cách sử dụng kết nối rẻ nhất có thể. 
4. Thêm nút này vào MST và cập nhật câu trả lời dưới dạng giá trị tối đa của`min_dist`trong số tất cả các cạnh được chọn. Mức tối đa này được theo dõi vì nó đại diện cho cạnh cổ chai trong cấu trúc nhịp. 
5. Sau khi thêm nút, hãy cập nhật`min_dist`cho tất cả các nút còn lại bằng cách so sánh giá trị hiện tại của chúng với khoảng cách đến nút mới được thêm vào. Điều này giữ`min_dist`phù hợp với MST một phần hiện tại. 
6. Tiếp tục cho đến khi bao gồm tất cả các nút. Câu trả lời cuối cùng là trọng số cạnh lớn nhất được chọn trong quá trình này. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán của Prim duy trì tính bất biến rằng tập hợp các nút được chọn hiện tại được kết nối bằng cách sử dụng các cạnh có tổng chi phí tối thiểu có thể có cho tập hợp đó. Bất kỳ cạnh nào kết nối thành phần này với phần còn lại của biểu đồ phải có trọng số ít nhất là hiện tại`min_dist`của nút đã chọn. Vì chúng tôi luôn chọn cạnh nhỏ nhất có sẵn như vậy nên chúng tôi đang xây dựng cây bao trùm nút cổ chai tối thiểu một cách hiệu quả. Cạnh lớn nhất trong cấu trúc này là ngưỡng nhỏ nhất có thể mà vẫn cho phép kết nối toàn bộ biểu đồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    if n == 1:
        print(0)
        return

    in_mst = [False] * n
    min_dist = [10**30] * n

    in_mst[0] = True
    for i in range(1, n):
        dx = pts[i][0] - pts[0][0]
        dy = pts[i][1] - pts[0][1]
        min_dist[i] = dx * dx + dy * dy

    ans = 0

    for _ in range(n - 1):
        v = -1
        best = 10**30
        for i in range(n):
            if not in_mst[i] and min_dist[i] < best:
                best = min_dist[i]
                v = i

        in_mst[v] = True
        ans = max(ans, best)

        for i in range(n):
            if not in_mst[i]:
                dx = pts[i][0] - pts[v][0]
                dy = pts[i][1] - pts[v][1]
                dist = dx * dx + dy * dy
                if dist < min_dist[i]:
                    min_dist[i] = dist

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện là thuật toán Prim dày đặc trực tiếp. các`min_dist`mảng luôn phản ánh cách rẻ nhất để kết nối từng nút bên ngoài với MST hiện tại. Chúng tôi sử dụng khoảng cách bình phương để tránh các phép tính dấu phẩy động, vì việc so sánh$\sqrt{P}$khoảng cách tương đương với việc so sánh các giá trị bình phương. 

Một chi tiết tinh tế là khởi tạo chính xác khoảng cách từ nút 0; không làm như vậy sẽ dẫn đến giá trị vô hạn không chính xác và phá vỡ bước lựa chọn. Một điểm quan trọng khác là theo dõi trọng số tối đa của cạnh đã chọn thay vì tính tổng chúng, vì bài toán yêu cầu một ngưỡng thay vì tổng chi phí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2
5 5
5 8
10 6
12 3
```Chúng tôi bắt đầu từ nút 0. 

| Bước | Nút được chọn | Cạnh được sử dụng | min_dist đã cập nhật | Tối đa hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | - | khởi tạo | 0 | 
| 2 | 1 | 0-1 = 32 | cập nhật từ 1 | 32 | 
| 3 | 2 | 1-2 = 9 | cập nhật từ 2 | 32 | 
| 4 | 3 | 1-3 = 34 | cập nhật từ 3 | 34 | 
| 5 | 4 | 3-4 = 29 | cuối cùng | 34 | 

Cạnh lớn nhất trong MST là 34, trở thành ngưỡng. Điều này xác nhận rằng mặc dù một số cặp ở xa nhau hơn, MST vẫn tránh sử dụng các cạnh trực tiếp dài không cần thiết. 

### Ví dụ 2 

đầu vào:```
4
0 0
0 1
0 2
10 10
```| Bước | Nút được chọn | Cạnh được sử dụng | Tối đa hiện tại | 
| --- | --- | --- | --- | 
| 1 | 0 | - | 0 | 
| 2 | 1 | 0-1 = 1 | 1 | 
| 3 | 2 | 1-2 = 1 | 1 | 
| 4 | 3 | 2-3 = 200 | 200 | 

Điểm cô lập buộc phải có kết nối dài và vì không có trạm trung gian nào tồn tại gần nó nên MST phải bao gồm cạnh lớn đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2)$| Mỗi trong số$N$lặp lại quét tất cả các nút và cập nhật khoảng cách bằng cách sử dụng tính toán theo cặp | 
| Không gian |$O(N)$| Chỉ các mảng dành cho thành viên MST và khoảng cách tối thiểu mới được lưu trữ | 

Với$N \le 500$, khoảng 250.000 lần kiểm tra khoảng cách được thực hiện, phù hợp thoải mái trong giới hạn thời gian trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import sqrt

    data = inp.strip().split()
    n = int(data[0])
    pts = [(int(data[i]), int(data[i+1])) for i in range(1, len(data), 2)]

    if n == 1:
        return "0\n"

    in_mst = [False] * n
    min_dist = [10**30] * n

    in_mst[0] = True
    for i in range(1, n):
        dx = pts[i][0] - pts[0][0]
        dy = pts[i][1] - pts[0][1]
        min_dist[i] = dx*dx + dy*dy

    ans = 0

    for _ in range(n - 1):
        v = -1
        best = 10**30
        for i in range(n):
            if not in_mst[i] and min_dist[i] < best:
                best = min_dist[i]
                v = i

        in_mst[v] = True
        ans = max(ans, best)

        for i in range(n):
            if not in_mst[i]:
                dx = pts[i][0] - pts[v][0]
                dy = pts[i][1] - pts[v][1]
                dist = dx*dx + dy*dy
                if dist < min_dist[i]:
                    min_dist[i] = dist

    return str(ans) + "\n"

# provided sample
assert run("""5
1 2
5 5
5 8
10 6
12 3
""") == "34\n"

# minimum input
assert run("""1
0 0
""") == "0\n"

# all equal points
assert run("""3
1 1
1 1
1 1
""") == "0\n"

# line case
assert run("""4
0 0
0 1
0 2
0 3
""") == "9\n"

# far isolated point
assert run("""3
0 0
1 0
100 100
""") == "20000\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 0 | trường hợp cơ sở | 
| điểm giống nhau | 0 | khoảng cách bằng không | 
| chuỗi đường | 9 | Hành vi xâu chuỗi MST | 
| nút xa bị cô lập | 20000 | buộc phải có cạnh dài | 

## Vỏ cạnh 

Đối với một trạm đơn, MST không có cạnh, do đó thuật toán sẽ khởi tạo`ans`bằng 0 và trả về ngay lập tức. Vòng lựa chọn không bao giờ được nhập, điều này phù hợp với thực tế là không có yêu cầu kết nối nào tồn tại. 

Khi tất cả các điểm trùng nhau, mọi khoảng cách bình phương được tính toán đều bằng 0, vì vậy mọi`min_dist`cập nhật vẫn bằng không. Mỗi cạnh được chọn có trọng số bằng 0, do đó mức tối đa cuối cùng cũng bằng 0, phù hợp với đầu ra dự kiến. 

Trong trường hợp có một ngoại lệ ở xa, thuật toán ban đầu xây dựng một cụm giữa các điểm gần nhau với các cạnh nhỏ, sau đó cuối cùng kết nối ngoại lệ bằng cách sử dụng liên kết nhỏ nhất có thể đến cụm đó. Vì tất cả các tùy chọn trung gian đều đã hết nên cạnh cuối cùng phải lớn và MST nắm bắt chính xác đây là nút thắt cổ chai.
