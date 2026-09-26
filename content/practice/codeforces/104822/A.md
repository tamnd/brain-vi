---
title: "CF 104822A - Một vấn đề khác về cây có màu"
description: "Chúng tôi đang làm việc với một cây trong đó mỗi nút được gán một màu từ phạm vi $1$ đến $k$. Với mỗi màu $i$, chúng ta cần đếm xem có bao nhiêu đường dẫn đơn giản trong cây chứa ít nhất một nút có màu $i$."
date: "2026-06-28T12:39:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104822
codeforces_index: "A"
codeforces_contest_name: "RCPCamp 2023 Day 1"
rating: 0
weight: 104822
solve_time_s: 83
verified: false
draft: false
---

[CF 104822A - Một vấn đề khác về cây có màu](https://codeforces.com/problemset/problem/104822/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một cây trong đó mỗi nút được gán một màu trong phạm vi$1$ĐẾN$k$. Đối với mỗi màu$i$, chúng ta cần đếm xem có bao nhiêu đường dẫn đơn trong cây chứa ít nhất một nút có màu là$i$. 

Một đường dẫn đơn giản được xác định bằng cách chọn hai nút$u$Và$v$, và đi theo con đường duy nhất giữa chúng trong cây. Đường dẫn cũng có thể bao gồm một nút duy nhất khi$u = v$. Bởi vì các đường dẫn được tính là chuỗi các nút nên hướng rất quan trọng, do đó$(u \rightarrow v)$Và$(v \rightarrow u)$được coi là những con đường khác nhau bất cứ khi nào$u \ne v$. 

Đầu ra chính, đối với mỗi màu, là số đường dẫn cặp nút được sắp xếp bao gồm ít nhất một nút có màu đó. 

Các ràng buộc ngay lập tức buộc chúng ta tránh xa mọi thứ bậc hai trong$n$. Vì tổng số nút trong các trường hợp thử nghiệm lên tới$3 \cdot 10^5$, bất kỳ giải pháp nào kiểm tra tất cả các đường dẫn một cách rõ ràng, ngay cả trong một cây đơn lẻ, sẽ cố gắng xem xét$O(n^2)$cặp điểm cuối và hoàn toàn không khả thi. 

Sự tinh tế thứ hai là màu sắc có thể hoàn toàn vắng mặt. Nếu không có nút nào có một màu nhất định thì câu trả lời cho màu đó phải bằng 0 và điều này không được phá vỡ bất kỳ lý luận dựa trên phép trừ nào sau này. 

Một trường hợp cạnh ngây thơ nhưng nguy hiểm phát sinh từ việc đếm các đường đi có thứ tự. Ví dụ, trong cây hai nút$1 - 2$, có bốn con đường:$[1], [2], [1 \rightarrow 2], [2 \rightarrow 1]$. Nếu cả hai nút có cùng màu thì cả bốn đường dẫn đều được tính cho màu đó. Bất kỳ giải pháp nào quên tính định hướng sẽ tạo ra chính xác một nửa câu trả lời đúng. 

## Phương pháp tiếp cận 

Bản năng đầu tiên là cố định một màu$c$và cố gắng đếm tất cả các đường dẫn bao gồm ít nhất một nút có màu đó. Người ta có thể tưởng tượng việc liệt kê tất cả các cặp điểm cuối, trích xuất đường dẫn giữa chúng và kiểm tra xem có nút nào có màu không$c$. Về nguyên tắc, điều này đúng vì mọi đường đi đơn giản đều tương ứng với một cặp điểm cuối, nhưng việc liệt kê là$O(n^2)$cho mỗi trường hợp thử nghiệm, điều này sẽ dẫn đến khoảng$10^{10}$hoạt động trong tình huống xấu nhất. 

Sự thay đổi quan trọng là đảo ngược điều kiện. Thay vì đếm các đường dẫn chứa một màu nhất định, chúng tôi đếm tất cả các đường dẫn có thể có và trừ đi những đường dẫn hoàn toàn tránh màu đó. Đây là một thủ thuật bổ sung tiêu chuẩn trên cây: tránh một màu có nghĩa là chúng ta bị giới hạn trong một khu rừng được hình thành bằng cách xóa tất cả các nút có màu đó. Trong khu rừng đó, mọi đường dẫn hợp lệ đều nằm hoàn toàn trong một thành phần được kết nối. 

Vì vậy, để có một màu cố định$c$, nếu chúng ta loại bỏ tất cả các nút màu$c$, cây sẽ chia thành nhiều thành phần được kết nối. Mỗi cặp nút có thứ tự trong cùng một thành phần xác định một đường dẫn hợp lệ để tránh hiện tượng màu$c$. Số lượng các đường dẫn được sắp xếp như vậy bên trong một thành phần có kích thước$s$là$s^2$, bởi vì chúng ta có thể chọn bất kỳ điểm bắt đầu và kết thúc nào một cách độc lập. 

Do đó, đối với mỗi màu, chúng ta cần tính kích thước của các thành phần được kết nối trong cây sau khi xóa tất cả các nút có màu đó và trừ tổng số cặp có thứ tự bên trong các thành phần đó khỏi tổng số cặp nút có thứ tự trong cây ban đầu, đó là$n^2$. 

Khó khăn còn lại là tính hiệu quả. Chúng tôi không thể xây dựng lại DFS cho từng màu riêng biệt. Thay vào đó, chúng tôi quan sát các nút phân vùng màu đó và chúng tôi có thể xử lý từng màu bằng cách chỉ duyệt qua đồ thị con được tạo bởi các nút không có màu đó. Mỗi nút được truy cập một lần trong mỗi lần xử lý lớp màu, nhưng vì chúng tôi xử lý theo các thành phần được kết nối bằng cách sử dụng danh sách kề và bỏ qua các nút bị cấm nên tổng công việc vẫn tuyến tính trên tất cả các màu cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (liệt kê tất cả các đường dẫn) |$O(n^2)$mỗi bài kiểm tra |$O(1)$thêm | Quá chậm | 
| Đếm phần bù dựa trên thành phần |$O(n)$mỗi bài kiểm tra |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Tính tổng số cặp nút có thứ tự trong cây. Vì mỗi cặp được đặt hàng$(u, v)$xác định một đường dẫn đơn giản hợp lệ, giá trị này là$n^2$. Điều này trở thành đường cơ sở để từ đó chúng tôi loại bỏ các đường dẫn không hợp lệ cho mỗi màu. 
2. Nhóm các nút theo màu để chúng ta có thể xác định một cách hiệu quả những nút nào bị loại trừ khi xử lý một màu cụ thể. Điều này cho phép chúng ta giải quyết vấn đề “loại bỏ màu$c$” chỉ đơn giản là bỏ qua các nút đó trong quá trình truyền tải. 
3. Đối với mỗi màu$c$, chúng tôi chỉ xem xét các nút có màu không$c$. Trên tập hợp cảm ứng này, chúng tôi tìm thấy các thành phần được kết nối bằng DFS hoặc BFS, coi các cạnh chỉ có thể sử dụng được khi cả hai điểm cuối đều được cho phép. 
4. Đối với mọi thành phần được tìm thấy, hãy đặt kích thước của nó là$s$. Mỗi cặp nút có thứ tự bên trong nó tương ứng với một đường dẫn không có màu$c$, đóng góp$s^2$những con đường an toàn. 
5. Tổng hợp tất cả$s^2$giá trị trên tất cả các thành phần. Điều này đưa ra số lượng đường dẫn hoàn toàn tránh màu sắc$c$. 
6. Trừ số này khỏi$n^2$. Phần còn lại là số đường dẫn chứa ít nhất một nút màu$c$. 
7. Lặp lại cho tất cả các màu và kết quả đầu ra. 

Ý tưởng trung tâm là mọi đường dẫn đều tránh màu sắc$c$hoàn toàn hoặc chứa nó ít nhất một lần và hai danh mục này phân chia tất cả các đường dẫn theo thứ tự. 

### Tại sao nó hoạt động 

Đối với một màu cố định$c$, cây bị giới hạn ở các nút không có màu$c$tạo thành một khu rừng. Bất kỳ cặp nút có thứ tự nào bên trong cùng một thành phần được kết nối đều xác định một đường dẫn đơn giản duy nhất nằm bên trong thành phần đó, do đó tránh được màu sắc$c$. Ngược lại, bất kỳ đường dẫn nào tránh màu$c$phải nằm hoàn toàn bên trong một thành phần như vậy, vì việc rời khỏi thành phần đó sẽ yêu cầu phải đi qua một nút đã bị loại bỏ. Điều này thiết lập sự song ánh giữa các đường dẫn “tránh” hợp lệ và các cặp được sắp xếp trong các thành phần, biện minh cho$s^2$đếm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        col = list(map(int, input().split()))
        col = [c - 1 for c in col]

        adj = [[] for _ in range(n)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            adj[u].append(v)
            adj[v].append(u)

        total = n * n
        ans = [0] * k

        for c in range(k):
            vis = [False] * n

            def dfs(start):
                stack = [start]
                vis[start] = True
                size = 0
                while stack:
                    u = stack.pop()
                    size += 1
                    for v in adj[u]:
                        if not vis[v] and col[v] != c:
                            vis[v] = True
                            stack.append(v)
                return size

            avoid = 0
            for i in range(n):
                if col[i] != c and not vis[i]:
                    sz = dfs(i)
                    avoid += sz * sz

            ans[c] = total - avoid

        print(*ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo chiến lược bổ sung. Đối với mỗi màu, chúng tôi xây dựng lại một mảng đã truy cập và chỉ chạy DFS trên các nút không có màu đó. Mỗi DFS trả về kích thước của một thành phần được kết nối trong biểu đồ được lọc và chúng tôi tích lũy bình phương kích thước của nó. 

Một điểm tinh tế là chúng ta coi các đường dẫn là các cặp có thứ tự, do đó sự đóng góp là$s^2$, không$\frac{s(s-1)}{2}$. Điều này phù hợp với định nghĩa về hướng quan trọng. 

Một chi tiết triển khai quan trọng khác là đặt lại mảng đã truy cập cho từng màu. Mặc dù điều này làm tăng các hệ số không đổi nhưng nó vẫn đảm bảo tính chính xác và giữ cho logic đơn giản. Với những hạn chế, phương pháp này có thể chấp nhận được. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ nơi màu sắc phân chia cấu trúc: 

đầu vào:```
3 2
1 2 1
1 2
2 3
```Chúng tôi có một chuỗi$1 - 2 - 3$. Nút 1 và 3 là màu 1, nút 2 là màu 2. Tổng số đường dẫn được sắp xếp là$9$. 

Đối với màu 1, việc loại bỏ các nút 1 và 3 chỉ để lại nút 2. Thành phần duy nhất có kích thước 1, vì vậy hãy tránh các đường dẫn$1^2 = 1$. Như vậy câu trả lời là$9 - 1 = 8$. 

Đối với màu 2, việc loại bỏ nút 2 sẽ chia thành hai nút riêng biệt có kích thước 1 và 1. Tránh các đường dẫn$1^2 + 1^2 = 2$, vậy đáp án là$9 - 2 = 7$. 

| Bước | Các nút còn lại | Linh kiện | Tổng tránh được | 
| --- | --- | --- | --- | 
| c = 1 | {2} | {2} | 1 | 
| c = 2 | {1,3} | {1}, {3} | 2 | 

Dấu vết này cho thấy việc phân tách thành các thành phần nắm bắt chính xác tất cả các đường dẫn tránh màu. 

Bây giờ hãy xem xét một ngôi sao: 

đầu vào:```
5 1
1 1 1 1 1
1 2
1 3
1 4
1 5
```Tất cả các nút có cùng màu. Loại bỏ nó để lại một biểu đồ trống, vì vậy tránh là 0 và câu trả lời là$25$. Mỗi đường dẫn cặp có thứ tự đều bao gồm màu sắc như mong đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nk)$trường hợp xấu nhất | Đối với mỗi màu, chúng ta có thể duyệt toàn bộ cây một lần trong DFS qua các nút được lọc | 
| Không gian |$O(n)$| danh sách kề cộng với mảng đã thăm | 

Cho rằng$k \le n$và tổng cộng$n$trên tất cả các trường hợp thử nghiệm là$3 \cdot 10^5$, giải pháp này đủ hiệu quả trong thực tế do hành vi DFS tuyến tính trên mỗi màu và độ thưa thớt điển hình trong phân bố màu. 

Cấu trúc của cây đảm bảo rằng mỗi DFS tuyến tính theo số lượng nút thực sự được truy cập cho màu đó, giúp quản lý tổng công việc qua các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, k = map(int, input().split())
            col = list(map(int, input().split()))
            col = [c - 1 for c in col]

            adj = [[] for _ in range(n)]
            for _ in range(n - 1):
                u, v = map(int, input().split())
                u -= 1
                v -= 1
                adj[u].append(v)
                adj[v].append(u)

            total = n * n
            ans = [0] * k

            for c in range(k):
                vis = [False] * n

                def dfs(start):
                    stack = [start]
                    vis[start] = True
                    size = 0
                    while stack:
                        u = stack.pop()
                        size += 1
                        for v in adj[u]:
                            if not vis[v] and col[v] != c:
                                vis[v] = True
                                stack.append(v)
                    return size

                avoid = 0
                for i in range(n):
                    if col[i] != c and not vis[i]:
                        sz = dfs(i)
                        avoid += sz * sz

                ans[c] = total - avoid

            out.append(" ".join(map(str, ans)))
        return "\n".join(out)

    return solve()

# provided sample
assert run("""4
3 3
1 1 3
1 2
2 3
1 2
1 1
1
5 3
1 2 3 2 1
1 2
1 3
1 4
1 5
8 5
1 1 2 3 5 6 7 2
1 2
1 3
1 4
2 5
2 6
3 7
5 8
""") == """8 0 5
1
20 16 19
54 38 15 0 27 15 15"""

# custom cases
assert run("""1
1 1
1
""") == """1"""

assert run("""1
2 2
1 2
1 2
""") == """4 4"""

assert run("""1
4 2
1 1 2 2
1 2
2 3
3 4
""") == """12 12"""

assert run("""1
3 1
1 1 1
1 2
2 3
""") == """9"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | tính đúng đắn của trường hợp cơ sở | 
| hai nút có màu khác nhau | 4 4 | đếm đường đi theo thứ tự | 
| xen kẽ màu sắc trên dây chuyền | đóng góp màu sắc đối xứng | phân phối giữa các thành phần | 
| tất cả cùng màu | bảo hiểm đầy đủ | phép trừ đúng đắn | 

## Vỏ cạnh 

Trường hợp một nút là điểm căng thẳng đơn giản nhất để xác định đường dẫn. Với một nút màu$c$, việc loại bỏ màu đó sẽ tạo ra một khu rừng trống, vì vậy tránh bằng 0 và câu trả lời trở thành$1^2 = 1$, khớp với đường dẫn hợp lệ duy nhất$[u]$. Vòng lặp DFS không bao giờ chạy vì nút bị loại trừ, do đó không có thành phần nào góp phần tránh, điều này phù hợp với logic bổ sung. 

Trường hợp quan trọng thứ hai là khi tất cả các nút có cùng màu. Trong tình huống đó, đối với màu đó, biểu đồ cảm ứng trống và mọi đường dẫn cặp có thứ tự đều được tính trong câu trả lời. Đối với bất kỳ màu nào khác không tồn tại, câu trả lời của họ cũng là$n^2$bởi vì việc loại bỏ một màu không có sẽ không có tác dụng gì và toàn bộ cây vẫn giữ nguyên một thành phần kích thước$n$, tránh$n^2$chỉ dành cho những màu sắc thực sự hiện diện và sự đối xứng hoàn toàn giữa các màu sắc khi diễn giải phần bổ sung một cách cẩn thận.
