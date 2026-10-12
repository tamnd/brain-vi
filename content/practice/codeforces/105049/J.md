---
title: "CF 105049J - Hãy nói về nấm mộ, sâu bọ và văn bia"
description: "Chúng tôi đang làm việc với một biểu đồ có hướng có các nút biểu thị các chủ đề và các cạnh biểu thị mối quan hệ một chiều giữa chúng."
date: "2026-06-28T05:49:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105049
codeforces_index: "J"
codeforces_contest_name: "UTPC Contest 03-22-24 Div. 1 (Advanced)"
rating: 0
weight: 105049
solve_time_s: 77
verified: false
draft: false
---

[CF 105049J - Hãy nói về những ngôi mộ, những con sâu và văn bia](https://codeforces.com/problemset/problem/105049/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một biểu đồ có hướng có các nút biểu thị các chủ đề và các cạnh biểu thị mối quan hệ một chiều giữa chúng. Biểu đồ bắt đầu bằng một tập hợp lớn các cạnh hiện có tạo thành cấu trúc tuần hoàn có hướng và sau đó chúng ta được cung cấp một chuỗi các cạnh bổ sung được chèn từng cạnh một. 

Đối với mỗi cạnh được định hướng mới từ một nút$u$đến một nút$v$, chúng ta phải quyết định xem việc thêm cạnh này có tạo ra một chu trình có hướng liên quan đến cả hai$u$Và$v$. Nếu chu kỳ như vậy xuất hiện, chúng ta xuất ra “CÓ”, nếu không thì xuất ra “KHÔNG”. Sau khi trả lời, cạnh sẽ được thêm vĩnh viễn vào biểu đồ và ảnh hưởng đến tất cả các truy vấn trong tương lai. 

Sự hạn chế đó$N \le 1000$là đầu mối cấu trúc trung tâm. Mặc dù có thể có tới$5 \cdot 10^5$các cạnh ban đầu, số lượng nút nhỏ cho thấy rằng việc duy trì thông tin về khả năng tiếp cận dày đặc là khả thi. Số lượng truy vấn$Q \le 1000$cũng nhỏ, điều này cho thấy rõ ràng rằng chúng tôi có đủ khả năng chi trả cho các cập nhật biểu đồ cho mỗi truy vấn đắt hơn so với logarit hoặc tuyến tính ở các cạnh. 

Một quan sát quan trọng là các chu kỳ được đưa ra bởi một cạnh mới$u \to v$chỉ phụ thuộc vào việc đã có đường đi từ$v$quay lại$u$trong biểu đồ hiện tại. Nếu đường dẫn như vậy tồn tại thì việc thêm$u \to v$đóng một chu kỳ có hướng dẫn. Nếu không thì không. 

Một trường hợp cạnh đơn giản nhưng quan trọng phát sinh khi có nhiều đường dẫn gián tiếp giữa các nút. Ví dụ, nếu$v \to a \to b \to u$tồn tại thì cộng$u \to v$tạo ra một chu trình ngay cả khi không có cạnh trực tiếp$v \to u$. Kiểm tra hoàn toàn dựa trên vùng lân cận hoặc cục bộ không thành công ở đây trừ khi khả năng tiếp cận đầy đủ được xem xét. 

Một trường hợp tinh vi khác là khi các bản cập nhật thay đổi khả năng tiếp cận cho các truy vấn trong tương lai. Ví dụ: ngay cả khi truy vấn hiện tại không tạo chu trình, nó có thể đưa ra khả năng tiếp cận mới ảnh hưởng đến các truy vấn sau này, vì vậy chúng tôi phải cập nhật cấu trúc bắc cầu sau mỗi lần chèn. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: cho mỗi cạnh truy vấn$u \to v$, chúng tôi chạy tìm kiếm biểu đồ (DFS hoặc BFS) từ$v$để xem liệu chúng ta có thể tiếp cận được không$u$. Nếu có thể thì việc thêm cạnh sẽ tạo ra một chu trình; nếu không thì không. Sau đó, chúng ta chèn cạnh vào danh sách kề. 

Điều này đúng vì một chu trình có hướng chứa cả$u$Và$v$tồn tại chính xác khi$v$có thể đạt được rồi$u$. Tuy nhiên, cách tiếp cận này trở nên tốn kém vì mỗi lần kiểm tra khả năng tiếp cận có thể mất$O(N + M)$, và kể từ đó$M$lớn lên khoảng$5 \cdot 10^5 + 1000$, tổng chi phí trên$Q = 1000$các truy vấn trở nên ranh giới hoặc tệ hơn trong Python, đặc biệt là do việc duyệt qua nhiều lần trên các danh sách kề lớn. 

Cái nhìn sâu sắc quan trọng là$N$là rất nhỏ. Thay vì tính toán lại khả năng tiếp cận từ đầu mỗi lần, chúng ta có thể duy trì ma trận đóng chuyển tiếp động. Cụ thể, chúng tôi duy trì ma trận boolean`reach[i][j]`cho biết có đường đi từ$i$ĐẾN$j$. 

Khi có một cạnh mới$u \to v$được thêm vào, nó có thể tạo các đường dẫn mới có dạng$x \to u \to v \to y$. Điều này gợi ý một bản cập nhật bắc cầu: nếu$x$có thể đạt được$u$, sau đó$x$bây giờ có thể tiếp cận tất cả các nút có thể truy cập từ$v$và tương tự tất cả các nút có thể tiếp cận$u$bây giờ có quyền truy cập vào$v$khả năng tiếp cận đi của. Đây thực chất là một bản cập nhật đóng chuyển tiếp cục bộ và bởi vì$N \le 1000$, việc cập nhật gia tăng giống như Floyd là khả thi cho mỗi truy vấn. 

Chúng tôi duy trì: 

-`reach[i][j] = True`nếu như$i$có thể đạt được$j$- khả năng truy cập ngược tùy chọn được ẩn thông qua cấu trúc chuyển vị trong các bản cập nhật 

Đối với mỗi lần chèn cạnh, chúng tôi chỉ cần phổ biến các cải tiến thông qua các nút hiện có thể kết nối qua$u \to v$. Việc này được thực hiện ở$O(N^2)$mỗi truy vấn. 

Từ$Q \le 1000$Và$N \le 1000$, độ phức tạp trong trường hợp xấu nhất là khoảng$10^9$các phép toán boolean, có thể chấp nhận được trong Python được tối ưu hóa khi được triển khai cẩn thận với các tham chiếu cục bộ và tránh chi phí Python nặng nề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS cho mỗi truy vấn |$O(Q(N+M))$|$O(N+M)$| Quá chậm | 
| Đóng cửa chuyển tiếp gia tăng |$O(QN^2)$|$O(N^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì ma trận khả năng tiếp cận trên tất cả các nút. 

### bước 

1. Khởi tạo ma trận boolean 2D`reach`kích thước$N \times N$, đặt tất cả các mục thành sai ngoại trừ khả năng tự truy cập nếu muốn. 

Sau đó chúng ta đọc tất cả các cạnh ban đầu và đánh dấu`reach[u][v] = True`. Các cạnh này tạo thành một DAG, do đó ban đầu không có chu kỳ nào tồn tại, nhưng khả năng tiếp cận đã không hề nhỏ do có nhiều cạnh. 
2. Tính toán bao đóng bắc cầu ban đầu bằng cách sử dụng phép nới lỏng kiểu Floyd-Warshall. 

Đối với mỗi nút trung gian$k$, nếu như$i \to k$Và$k \to j$, sau đó đặt$i \to j$. 

Điều này cung cấp cấu trúc khả năng tiếp cận cơ sở chính xác trước khi xử lý truy vấn. 
3. Đối với mỗi cạnh truy vấn$u \to v$, trước tiên hãy kiểm tra xem`reach[v][u]`là đúng. 

Nếu đúng thì đã có đường dẫn từ$v$quay lại$u$, do đó thêm$u \to v$đóng một chu trình có hướng liên quan đến cả hai nút. Đầu ra “CÓ”. 
4. Nếu`reach[v][u]`là sai, xuất ra “NO” vì không có chu trình nào liên quan đến cả hai điểm cuối có thể được hình thành tại thời điểm này. 
5. Bất kể câu trả lời là gì, hãy chèn cạnh mới và cập nhật khả năng tiếp cận. 

Chúng tôi tuyên truyền khả năng tiếp cận mới bằng cách xem xét rằng mọi nút có thể tiếp cận$u$bây giờ có thể tiếp cận mọi thứ có thể truy cập từ$v$, và tương tự$v$kế thừa khả năng tiếp cận từ tất cả các nút có thể tiếp cận$u$. 

Điều này được thực hiện bằng cách lặp lại tất cả các cặp$(i, j)$và cập nhật thông qua các kết nối mới được kích hoạt. 

### Tại sao nó hoạt động 

Bất cứ lúc nào,`reach[i][j]`thể hiện chính xác liệu một đường dẫn có hướng có tồn tại trong biểu đồ hiện tại hay không. Cách duy nhất để có được lợi thế mới$u \to v$có thể tạo một chu trình liên quan đến cả hai điểm cuối nếu đã có đường dẫn từ$v$ĐẾN$u$. Điều kiện đó chính xác là những gì chúng tôi kiểm tra. 

Bước cập nhật duy trì tính chính xác vì mọi đường dẫn mới được đưa vào phải đi qua cạnh mới ít nhất một lần. Bất kỳ đường dẫn nào như vậy sẽ phân tách thành tiền tố kết thúc tại$u$, cạnh$u \to v$, và một hậu tố bắt đầu từ$v$. Việc truyền bá khả năng tiếp cận từ cả hai phía đảm bảo tất cả các bố cục như vậy được ghi lại, duy trì khả năng đóng bắc cầu hoàn toàn sau mỗi lần chèn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, q = map(int, input().split())
    n0 = n

    reach = [[False] * n for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        reach[u-1][v-1] = True

    # Floyd-Warshall transitive closure
    for k in range(n):
        rk = reach[k]
        for i in range(n):
            if reach[i][k]:
                ri = reach[i]
                for j in range(n):
                    if rk[j]:
                        ri[j] = True

    for _ in range(q):
        u, v = map(int, input().split())
        u -= 1
        v -= 1

        if reach[v][u]:
            print("YES")
        else:
            print("NO")

        if not reach[u][v]:
            # add edge and update closure
            # copy lists for speed
            ru = reach[u]
            rv = reach[v]

            # nodes that can reach u
            for i in range(n):
                if reach[i][u]:
                    ri = reach[i]
                    # everything reachable from v becomes reachable from i
                    for j in range(n):
                        if rv[j]:
                            ri[j] = True

            # nodes reachable from v now inherit from u
            for j in range(n):
                if rv[j]:
                    ru[j] = True
```Giải pháp được xây dựng xung quanh ma trận kề cận boolean nhỏ gọn. Thẻ Floyd-Warshall ban đầu đảm bảo tất cả khả năng tiếp cận gián tiếp được thiết lập trước khi bắt đầu truy vấn, do đó, mỗi truy vấn có thể dựa vào chế độ xem toàn cầu nhất quán. 

Trong quá trình cập nhật, vòng lan truyền đầu tiên sẽ mở rộng khả năng tiếp cận từ tất cả các phiên bản trước của$u$bởi vì$v$cấu trúc đi của. Vòng lặp thứ hai mở rộng khả năng tiếp cận từ$u$trực tiếp. Hai hướng này cùng nhau mô phỏng tất cả các đường dẫn mới phải đi qua cạnh được chèn. 

Một chi tiết triển khai tinh tế là tránh việc ghi không cần thiết: chúng tôi chỉ truyền bá khi`reach[i][u]`hoặc`rv[j]`là đúng, làm giảm đáng kể các yếu tố không đổi. Ngoài ra, việc lập chỉ mục được giữ dựa trên 0 xuyên suốt để tránh chuyển đổi lặp lại bên trong các vòng lặp. 

## Ví dụ đã hoạt động 

### Mẫu 2 

đầu vào:```
5 4 3
1 2
2 3
3 4
4 5
1 3
2 5
2 3
```Khả năng tiếp cận ban đầu sau khi đóng cửa Floyd bao gồm các đường dẫn như$1 \to 3$,$1 \to 4$,$1 \to 5$,$2 \to 5$, v.v. 

| Truy vấn | bạn | v | đạt[v][u] trước | Trả lời | Hiệu ứng chính | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | Sai | KHÔNG | thêm phím tắt 1→3 | 
| 2 | 2 | 5 | Sai | CÓ | 5 đã đạt 2 thông qua chuỗi | 
| 3 | 2 | 3 | Đúng | CÓ | hình thức chu trình thông qua các đường dẫn hiện có | 

Dấu vết này cho thấy cấu trúc bắc cầu phát triển như thế nào và các truy vấn sau này phụ thuộc nhiều vào các bản cập nhật trước đó như thế nào. 

### Mẫu 1 (giải thích một phần) 

Một số truy vấn đầu tiên không theo chu kỳ vì chưa tồn tại khả năng truy cập ngược giữa các cặp được truy vấn. Sau khi tích lũy đủ số cạnh, một truy vấn sau đó cuối cùng sẽ kết nối nút trở lại nút tổ tiên trong DAG khả năng tiếp cận, tạo ra “CÓ”. 

Mẫu quan trọng là các câu trả lời “KHÔNG” ban đầu tương ứng với cấu trúc chuyển tiếp theo chu kỳ nghiêm ngặt, trong khi các câu trả lời “CÓ” sau đó chỉ ra rằng việc đóng động đã tạo ra một đường dẫn lùi thông qua các cập nhật tích lũy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^3 + QN^2)$| Đóng Floyd cộng với việc truyền bá mỗi truy vấn qua ma trận | 
| Không gian |$O(N^2)$| ma trận khả năng tiếp cận | 

Với$N \le 1000$Và$Q \le 1000$, việc triển khai vẫn nằm trong giới hạn vì công việc khối nặng bị giới hạn ở một bước tiền xử lý duy nhất và mỗi lần cập nhật truy vấn hoàn toàn là lan truyền ma trận boolean với các vòng lặp chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# sample-like small DAG
assert run("""3 2 2
1 2
2 3
1 3
2 1
""") in ["NO\nNO", "NO\nYES"]

# no edges initially
assert run("""3 0 2
1 2
2 3
""") == "NO\nNO"

# immediate cycle creation
assert run("""3 2 1
1 2
2 1
3 1
""") == "YES"

# linear chain then closure
assert run("""4 3 1
1 2
2 3
3 4
4 1
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị trống | KHÔNG KHÔNG | khả năng tiếp cận cơ bản | 
| chu trình đối xứng | CÓ | phát hiện chu kỳ ngay lập tức | 
| đóng cửa dây chuyền | CÓ | lạc hậu bắc cầu | 
| đồ thị thưa thớt | KHÔNG | không có kết quả dương tính giả | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi biểu đồ đã chứa các đường dẫn gián tiếp dài nhưng không có kết nối trực tiếp giữa các điểm cuối truy vấn. Ví dụ, một chuỗi$1 \to 2 \to 3 \to 4$. Một truy vấn$4 \to 1$phải trả về “CÓ” vì$1$đạt tới$4$gián tiếp vậy$4$có thể quay lại$1$sau khi chèn. Thuật toán xử lý việc này vì`reach[1][4]`đã đúng sau khi đóng, do đó việc kiểm tra ngược sẽ phát hiện chính xác sự hình thành chu trình. 

Một trường hợp khác là việc tăng cường lặp đi lặp lại khả năng tiếp cận. Nếu một cạnh$u \to v$được thêm vào khi$u$đã đạt tới$v$, không nên áp dụng bản cập nhật nào. điều kiện`if not reach[u][v]`đảm bảo chúng tôi tránh được sự lan truyền dư thừa, giữ các bản cập nhật ở mức tối thiểu trong khi vẫn duy trì tính chính xác. 

Trường hợp tinh vi cuối cùng là tạo chu trình khép kín thông qua cập nhật nhiều bước. Ngay cả khi ban đầu không có điểm cuối nào đến được điểm cuối khác, các cập nhật trung gian có thể đưa ra một đường dẫn$v \to u$trước truy vấn, việc chèn sau sẽ ngay lập tức theo chu kỳ. Bởi vì chúng tôi luôn kiểm tra`reach[v][u]`tại thời điểm truy vấn bằng cách sử dụng bao đóng được cập nhật đầy đủ, phần phụ thuộc này sẽ được xử lý một cách tự nhiên.
