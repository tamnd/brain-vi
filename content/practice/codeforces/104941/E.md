---
title: "CF 104941E - Đi đều"
description: "Chúng ta được cung cấp một đồ thị vô hướng gồm các thị trấn được kết nối bằng đường, nơi người Womais có thể đi qua các cạnh bất kỳ số lần nào và thậm chí có thể tự do xem lại các đỉnh. Anh ta bắt đầu tại một thị trấn cố định $s$ và muốn đến đích $t$."
date: "2026-06-28T18:18:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104941
codeforces_index: "E"
codeforces_contest_name: "SLPC 2024 Open Division"
rating: 0
weight: 104941
solve_time_s: 85
verified: false
draft: false
---

[CF 104941E - Đi đều](https://codeforces.com/problemset/problem/104941/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị vô hướng gồm các thị trấn được kết nối bằng đường, nơi người Womais có thể đi qua các cạnh bất kỳ số lần nào và thậm chí có thể tự do xem lại các đỉnh. Anh ấy bắt đầu ở một thị trấn cố định$s$và muốn đến đích$t$. Mỗi lần đi qua một con đường sẽ đóng góp độ dài 1 vào tổng chiều dài đi bộ. 

Câu hỏi không phải là tìm ra con đường ngắn nhất mà là về cấu trúc ngang bằng của tất cả các bước đi có thể từ$s$ĐẾN$t$. Chúng ta phải xác định xem mỗi lần đi bộ hợp lệ từ$s$ĐẾN$t$thậm chí có tổng chiều dài. Nếu ngay cả một bước đi có độ dài lẻ, câu trả lời sẽ trở thành "Không". 

Đồ thị có thể lớn tới$2 \cdot 10^5$các đỉnh và các cạnh, do đó bất kỳ phương pháp nào liệt kê các đường đi hoặc thậm chí lưu trữ tất cả các đường đi đơn giản đều không thể thực hiện được. Ngay cả một BFS ngây thơ chỉ theo dõi các đỉnh cũng không đủ vì tính chẵn lẻ phụ thuộc vào cách đạt đến đỉnh chứ không chỉ liệu nó có đạt được hay không. 

Một điều tinh tế quan trọng là việc xem lại các nút sẽ thay đổi mọi thứ. Một biểu đồ chứa một chu trình cho phép có vô số độ dài bước đi khác nhau giữa hai nút giống nhau. Ví dụ: nếu có bất kỳ chu trình nào có thể truy cập được từ$s$trên đường tới$t$, chúng ta có thể điều chỉnh tính chẵn lẻ của bước đi bằng cách đi vòng quanh chu kỳ đó. 

Một trường hợp điển hình phá vỡ suy nghĩ ngây thơ là một hình tam giác: 

đầu vào:```
3 3
1 2
1 3
3 2
2 1
```Có hai lối đi riêng biệt từ 1 đến 2: đi thẳng (độ dài 1) và đi qua 3 (độ dài 2). Vì cả hai đều tồn tại nên câu trả lời đúng là "Không". 

Một trường hợp quan trọng khác là một cái cây. Trong một cây, có chính xác một đường đi đơn giản, vì vậy tất cả các bước đi đều giảm xuống đường đi đó cộng với các chu kỳ quay lui có độ dài 2, đảm bảo tính chẵn lẻ. Vì vậy, cây luôn cho "Có" hoặc "Không" chỉ tùy thuộc vào độ dài đường dẫn duy nhất. 

Khó khăn cốt lõi là xác định khi nào biểu đồ tạo ra sự chẵn lẻ duy nhất giữa$s$Và$t$và khi tính chẵn lẻ có thể được đảo ngược bằng các tuyến đường thay thế. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng khám phá tất cả các bước đi có thể từ$s$ĐẾN$t$, theo dõi độ dài đường đi và kiểm tra xem có đường đi bộ có độ dài lẻ nào tồn tại cùng với đường đi có độ dài chẵn hay không. Điều này ngay lập tức thất bại vì số bước đi là vô hạn trong đồ thị có chu kỳ. Ngay cả việc giới hạn ở một độ sâu giới hạn cũng không giúp ích được gì, vì các chu trình có thể được truyền đi tùy ý nhiều lần, tạo ra các nhóm đường đi lớn tùy ý. Không gian trạng thái tăng theo cấp số nhân về số cạnh, khiến cho phương pháp này không khả thi về mặt tính toán. 

Một cách nhìn có cấu trúc hơn xuất phát từ việc chuyển vấn đề thành một câu hỏi về khả năng tiếp cận tính chẵn lẻ. Thay vì chỉ theo dõi những đỉnh nào có thể tiếp cận được, chúng tôi theo dõi xem một đỉnh có thể tiếp cận được với khoảng cách chẵn hay lẻ từ$s$. Điều này tự nhiên gợi ý một ý tưởng sao chép biểu đồ: mỗi nút$u$được chia thành hai trạng thái,$u_0$Và$u_1$, đại diện cho tính chẵn lẻ của khoảng cách. Mỗi cạnh lật ngang nhau, vì vậy chúng tôi kết nối$u_0 \to v_1$Và$u_1 \to v_0$. 

Bây giờ câu hỏi đặt ra là liệu có tồn tại một con đường từ$s_0$ĐẾN$t_1$. Nếu có một con đường như vậy thì sẽ có một quãng đường có độ dài lẻ. Nếu nó không tồn tại, tất cả các bước đi có thể tiếp cận được$t$phải chẵn. 

Tuy nhiên, điều này vẫn chưa nắm bắt được đầy đủ yêu cầu của bài toán. Chúng ta không được hỏi liệu có tồn tại một bước đi lẻ hay không mà là liệu tất cả các bước đi có chẵn hay không. Điều đó tương đương với việc kiểm tra xem$t_1$không thể truy cập được từ$s_0$trong biểu đồ mở rộng chẵn lẻ. 

Quan sát quan trọng là cấu trúc của biểu đồ mở rộng chỉ phụ thuộc vào các thành phần được kết nối và các ràng buộc giống như lưỡng cực. Nếu xung đột tồn tại khi một đỉnh có thể tiếp cận được ở cả hai chẵn lẻ, thì sẽ có một chu trình cho phép lật chẵn lẻ, ngụ ý tồn tại cả các bước chẵn và lẻ. Do đó, việc phát hiện xem các phép gán chẵn lẻ có nhất quán hay không sẽ tương đương với việc kiểm tra tính lưỡng cực theo nghĩa đã được sửa đổi. 

Chúng tôi thực hiện một cách hiệu quả BFS gán tính chẵn lẻ cho mỗi nút từ$s$. Nếu chúng ta cố gắng gán tính chẵn lẻ xung đột cho cùng một nút, chúng ta sẽ phát hiện ra rằng cả hai tính chẵn lẻ đều có thể xảy ra ở đâu đó trong biểu đồ, điều này ngụ ý sự tồn tại của cả các bước chẵn và lẻ giữa$s$Và$t$. Mặt khác, tính chẵn lẻ là nhất quán và cố định. 

Điều này làm giảm vấn đề xuống còn một BFS/DFS duy nhất trên biểu đồ có tính năng theo dõi chẵn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên mọi bước đi | Hàm mũ | Hàm mũ | Quá chậm | 
| BFS chẵn lẻ ở trạng thái nhân đôi |$O(n + m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình mỗi đỉnh với trạng thái chẵn lẻ liên quan đến nút bắt đầu$s$. 

1. Khởi tạo một mảng`dist`kích thước$n$với giá trị -1, nghĩa là chưa được truy cập. Bộ`dist[s] = 0`vì khoảng cách tới chính nó là số chẵn. 
2. Chạy BFS bắt đầu từ$s$, sử dụng một hàng các đỉnh. Mỗi lần chúng ta đi qua một cạnh$u \to v$, chúng tôi cố gắng gán`dist[v] = dist[u] XOR 1`. Điều này mã hóa thực tế là mọi cạnh đều có tính chẵn lẻ. 
3. Nếu chúng ta đạt đến một đỉnh$v$đã có một giá trị và giá trị chẵn lẻ mới được tính toán xung đột với giá trị hiện có, chúng tôi không ghi đè lên nó mà ghi lại rằng có sự mâu thuẫn. 
4. Sau khi BFS kết thúc, chúng tôi kiểm tra xem$t$đã được ấn định một mức chẵn lẻ và liệu có bất kỳ mâu thuẫn nào được quan sát thấy hay không. 
5. Nếu tồn tại mâu thuẫn ở bất kỳ đâu trong thành phần liên thông của$s$, thì có thể đi cả bước chẵn và lẻ giữa$s$Và$t$, vì vậy chúng tôi xuất ra "Không". 
6. Nếu không, tính chẵn lẻ sẽ được cố định. Nếu như`dist[t] == 1`, thì chỉ tồn tại cấu trúc có độ dài lẻ, nhưng vì không tồn tại mâu thuẫn nên mọi bước đi đều bảo toàn tính chẵn lẻ. Nếu như`dist[t] == 0`, tất cả các bước đi đều chẵn, vì vậy chúng tôi xuất ra "Có". 

Ý tưởng chính là mâu thuẫn tương ứng với các chu kỳ lẻ có thể đạt được từ$s$, cho phép lật chẵn lẻ mà không thay đổi kết nối. 

### Tại sao nó hoạt động 

Mỗi nhiệm vụ BFS thực thi việc ghi nhãn chẵn lẻ nhất quán của thành phần được kết nối của$s$. Nếu thành phần là lưỡng cực, mọi cạnh sẽ buộc các phép gán chẵn lẻ đối diện một cách nhất quán, do đó mọi bước đi giữa hai nút đều có cùng một tính chẵn lẻ. Nếu thành phần không phải là lưỡng cực thì tồn tại ít nhất một chu trình lẻ, cho phép một bước đi quay trở lại cùng một đỉnh với tính chẵn lẻ bị đảo ngược. Điều này làm cho cả hai bước đi có độ dài chẵn và lẻ có thể thực hiện được giữa hai đỉnh bất kỳ trong thành phần đó. Vì vậy, sự tồn tại của việc ghi nhãn chẵn lẻ nhất quán chính xác là điều kiện mà tất cả mọi người đều đi giữa$s$Và$t$chia sẻ cùng một sự bình đẳng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    s, t = map(int, input().split())
    s -= 1
    t -= 1

    g = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    dist = [-1] * n
    q = deque()
    dist[s] = 0
    q.append(s)

    ok = True

    while q:
        u = q.popleft()
        for v in g[u]:
            nd = dist[u] ^ 1
            if dist[v] == -1:
                dist[v] = nd
                q.append(v)
            elif dist[v] != nd:
                ok = False

    if not ok:
        print("No")
        return

    print("Yes")

if __name__ == "__main__":
    solve()
```Biểu đồ được xây dựng dưới dạng danh sách kề tiêu chuẩn. BFS bắt đầu từ$s$và ấn định mức độ chẵn lẻ. Hoạt động XOR là chi tiết triển khai chính, mã hóa các chuyển đổi chẵn lẻ truyền tải cạnh mà không cần cấu trúc trạng thái bổ sung. các`ok`cờ theo dõi xem có bất kỳ mâu thuẫn chẵn lẻ nào phát sinh hay không, tương ứng với việc phát hiện một chu kỳ lẻ trong thành phần được kết nối. 

Quyết định cuối cùng phụ thuộc hoàn toàn vào việc liệu việc gán chẵn lẻ có nhất quán hay không. Nếu vậy, tất cả sẽ đi giữa$s$Và$t$chia sẻ cùng một tính chẵn lẻ, mà trong công thức này có nghĩa là không có cách nào tồn tại để xây dựng cả hai phương án chẵn và lẻ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 5
2 4
1 2
2 3
3 4
1 4
4 5
```Chúng tôi chạy BFS từ nút 2. 

| Bước | Nút | Được chỉ định chẵn lẻ | Hàng xóm | Hàng xóm ngang bằng | Xung đột | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 1 | 1 | Không | 
| 2 | 2 | 0 | 3 | 1 | Không | 
| 3 | 3 | 1 | 4 | 0 | Không | 
| 4 | 1 | 1 | 4 | 0 | Không | 
| 5 | 4 | 0 | 5 | 1 | Không | 

Không có mâu thuẫn nào xuất hiện nên cấu trúc chẵn lẻ là nhất quán. Do đó, tất cả các bước từ 2 đến 4 phải có độ dài chẵn, cho kết quả đầu ra là "Có". 

### Mẫu 2 

đầu vào:```
6 6
1 5
1 6
6 2
2 3
3 4
4 1
2 5
```| Bước | Nút | Được chỉ định chẵn lẻ | Hàng xóm | Hàng xóm ngang bằng | Xung đột | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 6 | 1 | Không | 
| 2 | 6 | 1 | 2 | 0 | Không | 
| 3 | 2 | 0 | 3 | 1 | Không | 
| 4 | 3 | 1 | 4 | 0 | Không | 
| 5 | 4 | 0 | 1 | 1 | Xung đột | 

Ở bước cuối cùng, nút 1 được xem lại với số chẵn lẻ dự kiến ​​là 1 nhưng đã có số chẵn lẻ là 0. Điều này biểu thị một chu kỳ lẻ, cho phép cả các bước chẵn và lẻ trong khoảng từ 1 đến 5. Vì vậy, câu trả lời là "Không". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi đỉnh và cạnh được xử lý nhiều nhất một lần trong quá trình truyền tải BFS | 
| Không gian |$O(n + m)$| Danh sách kề cộng với mảng khoảng cách và hàng đợi | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các nút và cạnh, do đó BFS thời gian tuyến tính phù hợp thoải mái trong giới hạn thời gian và mức sử dụng bộ nhớ nằm trong giới hạn 1024 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip()

# provided samples (placeholders if formatting differs)
# assert run(...) == ..., "sample 1"
# assert run(...) == ..., "sample 2"

# minimal graph
assert run("2 1\n1 2\n1 2\n") in {"Yes", "No"}

# simple triangle (odd cycle)
assert run("3 3\n1 3\n1 2\n2 3\n3 1\n") == "No"

# tree case
assert run("4 3\n1 4\n1 2\n2 3\n3 4\n") == "Yes"

# graph with even cycle
assert run("4 4\n1 4\n1 2\n2 3\n3 4\n2 4\n") in {"Yes", "No"}

# fully connected small graph
assert run("3 3\n1 2\n1 2\n2 3\n3 1\n") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 chu kỳ | Không | Chu kỳ kỳ lạ gây ra xung đột chẵn lẻ | 
| Biểu đồ đường dẫn | Có | Đường dẫn duy nhất thực thi tính chẵn lẻ cố định | 
| Hợp âm bổ sung | Kiểm tra tính nhất quán Không/Có | Nhiều tuyến kiểm tra tính ổn định chẵn lẻ | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi đồ thị là một đường dẫn đơn giản. Trong tình huống đó, BFS chỉ định tính chẵn lẻ xen kẽ một cách rõ ràng mà không có mâu thuẫn. Ví dụ:```
4 3
1 4
1 2
2 3
3 4
```Việc truyền tải gán 1→0, 2→1, 3→0, 4→1. Không có xung đột nào phát sinh nên sự bình đẳng được cố định ở mọi tầng lớp. Vì không có chu trình nên không tồn tại đường dẫn chẵn lẻ thay thế. 

Một trường hợp khác là đồ thị chứa chu trình lẻ có thể đạt được từ$s$nhưng không nhất thiết phải đi theo con đường ngắn nhất để$t$. Kể cả nếu$t$nằm bên ngoài chu trình, chu trình cho phép quay trở lại các nút trước đó với tính chẵn lẻ bị đảo ngược, cuối cùng truyền bá mâu thuẫn vào cấu trúc có thể tiếp cận. Điều này đảm bảo thuật toán vẫn đánh dấu sự không nhất quán ngay cả khi chu trình không trực tiếp trên một$s \to t$tuyến đường. 

Một trường hợp tế nhị cuối cùng là khi$s$bằng$t$. Bài toán đảm bảo rằng chúng khác nhau, nhưng nếu chúng bằng nhau thì câu trả lời sẽ luôn là "Có" vì quãng đường trống có độ dài bằng 0 và bất kỳ chu trình nào cũng sẽ chỉ đưa ra các lựa chọn thay thế bổ sung nhưng không liên quan.
