---
title: "CF 105789A - Ananna"
description: "Chúng ta đang làm việc với một đồ thị có hướng có các cạnh được gắn nhãn bằng các ký tự. Nhiệm vụ không phải là trả lời khả năng tiếp cận theo nghĩa thông thường mà là khám phá tất cả các cặp đỉnh có thể được kết nối bằng một bước đi có chuỗi các nhãn cạnh tạo thành một bảng màu."
date: "2026-06-21T13:21:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105789
codeforces_index: "A"
codeforces_contest_name: "The 2025 ICPC Latin America Championship"
rating: 0
weight: 105789
solve_time_s: 52
verified: true
draft: false
---

[CF 105789A - Ananna](https://codeforces.com/problemset/problem/105789/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc với một đồ thị có hướng có các cạnh được gắn nhãn bằng các ký tự. Nhiệm vụ không phải là trả lời khả năng tiếp cận theo nghĩa thông thường mà là khám phá tất cả các cặp đỉnh có thể được kết nối bằng một bước đi có chuỗi các nhãn cạnh tạo thành một bảng màu. 

Một điều kiện palindrome kết hợp hai đầu của một đường đi. Nếu chúng ta biết có một bước đi đối xứng từ`u`ĐẾN`v`, thì ký tự đầu tiên và cuối cùng của bước đi đó phải khớp nhau và việc loại bỏ chúng sẽ để lại một bước đi đối xứng khác giữa cặp đỉnh tiếp theo hướng vào trong. Điều này cho thấy rằng kết nối palindromic không cục bộ trên một đường dẫn duy nhất mà lan truyền thông qua việc mở rộng đối xứng từ cả hai đầu. 

Biểu đồ đầu vào có thể đủ lớn để việc lặp qua tất cả các đường dẫn là không thể. Ngay cả việc liệt kê tất cả các cặp đỉnh cũng đã là bậc hai và thuật toán được mô tả trong câu lệnh xây dựng rõ ràng một quy trình trên các cặp`(u, v)`thay vì các đỉnh hoặc cạnh riêng lẻ. Điều này ngay lập tức gợi ý rằng không gian trạng thái là tập hợp các cặp đỉnh, có khả năng`O(n^2)`. 

Một cách giải thích ngây thơ sẽ là cố gắng xây dựng tất cả các bước đi palindromic một cách rõ ràng hoặc chạy BFS trên các trạng thái`(u, v)`trong đó quá trình chuyển đổi phụ thuộc vào nhãn cạnh phù hợp. Điều đó đã gợi ý về một vụ nổ trong trường hợp xấu nhất, vì mỗi cặp có thể tạo ra nhiều cặp mới. 

Một trường hợp phức tạp xuất phát từ các vòng tự lặp và các đường dẫn tầm thường. Mỗi đỉnh`(u, u)`ban đầu có giá trị vì bước đi trống là một palindrome. Một trường hợp cạnh khác là khi nhiều cạnh chia sẻ cùng một nhãn, vì chúng có thể tạo ra nhiều chuyển tiếp giống hệt nhau. Việc triển khai bất cẩn có thể trùng lặp công việc hoặc thêm lại các cặp đã được xử lý, gây ra sự cố. 

Ví dụ: nếu chúng ta có một đồ thị có hai đỉnh`1 -> 2`dán nhãn`a`Và`2 -> 1`dán nhãn`a`, sau đó`(1, 1)`Và`(2, 2)`là tầm thường, và`(1, 2)`Và`(2, 1)`được kết nối ngay lập tức. Từ đó, quy tắc mở rộng sẽ liên tục xác nhận các cặp giống nhau thông qua các cạnh vào và ra, nhưng chúng tôi phải đảm bảo rằng chúng tôi không xử lý lại các cặp đã thấy. 

Đầu ra đơn giản là số cặp đỉnh phân biệt`(u, v)`với`u != v`được phát hiện bởi quá trình đóng cửa này. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là xử lý từng cặp`(u, v)`như một trạng thái và cố gắng mô phỏng quá trình một cách trực tiếp. Chúng tôi bắt đầu với tất cả`(u, u)`và tất cả các cạnh`(u, v)`như các trạng thái ban đầu. Sau đó, chúng tôi liên tục cố gắng mở rộng: với mọi cặp hiện tại`(u, v)`, chúng tôi xem xét mọi cạnh đến vào`u`và mọi cạnh đi từ`v`. Nếu các nhãn khớp nhau, chúng tôi sẽ tạo một cặp mới`(a, b)`. 

Điều này đúng vì nó phản ánh chính xác sự phân rã đệ quy của bước đi palindromic: việc khớp các ký tự bên ngoài sẽ giảm vấn đề thành bước đi palindromic nhỏ hơn bên trong. Tuy nhiên, mô phỏng này có thể quét liên tục cùng một thông tin lân cận cho cùng một cặp và có khả năng`O(n^2)`cặp. Mỗi cặp chi phí mở rộng`deg_in(u) * deg_out(v)`, trong đồ thị dày đặc dẫn đến sự lặp lại thảm khốc. 

Quan sát chính là thuật toán về cơ bản là một tính toán đóng trên quan hệ nhị phân trên các đỉnh. Mỗi cặp`(u, v)`được chèn tối đa một lần và khi nó được xử lý, chúng tôi chỉ cố gắng tạo các cặp mới. Công việc nặng nhọc nằm ở việc khớp các cạnh vào và ra có cùng nhãn. Thay vì nghĩ về những con đường, chúng ta nghĩ về việc kết hợp hai nửa bước: một bước lùi vào trong.`u`và một bước tiến ra khỏi`v`, được đồng bộ hóa bởi sự bình đẳng nhãn. 

Điều này biến vấn đề thành một mở rộng hai con trỏ trên các cạnh biểu đồ được nhóm theo nhãn. Mỗi cạnh chỉ tham gia ghép nối thông qua sự đóng góp mức độ của nó và trên tất cả các cặp, tổng công việc sẽ bị giới hạn bởi tổng trên tất cả các đỉnh của`indeg(u)`lần`outdeg(v)`tổng hợp trên toàn bộ cấu trúc, sụp đổ thành`O(M^2)`trong trường hợp xấu nhất nhưng được kiểm soát trong thực tế bằng cách đếm các tương tác cặp cạnh thay vì trạng thái cặp. 

Sự thay đổi cấu trúc quan trọng là nhận ra rằng chúng tôi không liệt kê các đường dẫn mà liệt kê các cặp cạnh tương thích và mỗi kết quả khớp thành công tương ứng với một sự kiện tạo duy nhất trong bao đóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Cặp đôi BFS bạo lực mà không cần quan tâm | O(n2 · độ2) | O(n²) | Quá chậm | 
| Đóng cặp được tối ưu hóa với ghép nối cạnh | O(M²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một hàng các cặp đỉnh`(u, v)`được biết là được kết nối bằng một bước đi palindromic. Chúng tôi cũng duy trì một bảng boolean`seen[u][v]`để đảm bảo mỗi cặp được xử lý một lần. 

1. Khởi tạo hàng đợi với tất cả các cặp`(u, u)`. Chúng tương ứng với các palindrome trống, vì việc ở tại một đỉnh tạo thành một bước đi tầm thường hợp lệ. 
2. Đồng thời khởi tạo hàng đợi với tất cả các cạnh được định hướng`(u, v)`. Bất kỳ cạnh đơn nào tạo thành một palindrome có độ dài bằng một, vì vậy những cạnh này có giá trị ngay lập tức. 
3. Trong khi hàng đợi không trống, hãy bật một cặp`(u, v)`. Chúng tôi coi điều này như là sự xác nhận rằng có tồn tại một bước đi palindromic từ`u`ĐẾN`v`. 
4. Đối với cặp này, lặp lại mọi cạnh đến`(a -> u)`có nhãn`c1`. Đồng thời, lặp lại mọi cạnh đi ra`(v -> b)`có nhãn`c2`. 
5. Bất cứ khi nào`c1 == c2`, chúng ta có thể hình thành một bước đi palindromic mới từ`a`ĐẾN`b`bằng cách gói gọn lối đi palindromic hiện có`(u, v)`với các cạnh phù hợp ở cả hai đầu. Điều này tạo ra một cặp ứng cử viên`(a, b)`. 
6. Nếu`(a, b)`chưa được nhìn thấy trước đó, đánh dấu nó là đã thấy và đẩy nó vào hàng đợi. 
7. Tiếp tục cho đến khi không tạo được cặp mới nào. Câu trả lời cuối cùng là số cặp phân biệt`(u, v)`với`u != v`đã từng được đánh dấu là đã nhìn thấy. 

### Tại sao nó hoạt động 

Mỗi bước đi palindromic có sự phân rã tự nhiên thành các cạnh khớp bên ngoài và một bước đi phụ palindromic bên trong. Nếu đi bộ từ`a`ĐẾN`b`, cạnh đầu tiên của nó`(a -> u)`phải khớp với cạnh cuối cùng`(v -> b)`trong nhãn và hướng. Việc loại bỏ các cạnh này sẽ để lại một bước đi palindromic hợp lệ từ`u`ĐẾN`v`. Điều này chứng minh rằng mọi cặp hợp lệ đều được tạo ra bằng cách liên tục mở rộng các cặp hợp lệ đã biết. Ngược lại, mọi cặp được tạo đều tương ứng với một bước gói hợp lệ, do đó không có cặp không hợp lệ nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque, defaultdict

def solve():
    n, m = map(int, input().split())
    
    in_edges = [[] for _ in range(n)]
    out_edges = [[] for _ in range(n)]
    
    edges = []
    
    for _ in range(m):
        u, v, c = input().split()
        u = int(u) - 1
        v = int(v) - 1
        in_edges[v].append((u, c))
        out_edges[u].append((v, c))
        edges.append((u, v))
    
    seen = [[False] * n for _ in range(n)]
    q = deque()
    
    for i in range(n):
        if not seen[i][i]:
            seen[i][i] = True
            q.append((i, i))
    
    for u, v in edges:
        if not seen[u][v]:
            seen[u][v] = True
            q.append((u, v))
    
    while q:
        u, v = q.popleft()
        
        for a, c1 in in_edges[u]:
            for b, c2 in out_edges[v]:
                if c1 == c2:
                    if not seen[a][b]:
                        seen[a][b] = True
                        q.append((a, b))
    
    ans = 0
    for i in range(n):
        for j in range(n):
            if i != j and seen[i][j]:
                ans += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp quá trình đóng cửa. Vùng lân cận được chia thành các danh sách đến và đi để mỗi bước mở rộng có thể ghép các cạnh một cách hiệu quả. các`seen`ma trận ngăn chặn việc xử lý lặp lại cùng một trạng thái, điều này rất cần thiết vì nếu không có nó, cùng một cặp có thể được tạo ra nhiều lần thông qua các đường dẫn mở rộng khác nhau. 

Bước đếm cuối cùng loại trừ các cặp đường chéo vì`(u, u)`là tầm thường và không được bao gồm trong định nghĩa đầu ra. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ có ba đỉnh và các cạnh được dán nhãn:`1 -> 2 (a)`,`2 -> 3 (a)`, Và`3 -> 2 (a)`. 

Chúng tôi khởi tạo với`(1,1)`,`(2,2)`,`(3,3)`và các cạnh trực tiếp`(1,2)`,`(2,3)`,`(3,2)`. 

| Bước | Cặp (u,v) | Hành động | Cặp mới | 
| --- | --- | --- | --- | 
| Ban đầu | (1,1),(2,2),(3,3) | đường chéo enqueue | không | 
| Ban đầu | (1,2),(2,3),(3,2) | cạnh xếp hàng | không | 
| Quy trình | (2,3) | khớp đến/đi qua nhãn a | (1,2) qua 1->2 và 3->2 | 
| Quy trình | (3,2) | khai triển đối xứng | không có gì mới | 
| Quy trình | (1,2) | không có trận đấu nào nữa | không | 

Điều này chứng tỏ cấu trúc palindromic lan truyền đồng thời tới và lui, liên kết các đường dẫn không phải là các cạnh trực tiếp. 

Bây giờ hãy xem xét một biểu đồ trong đó tất cả các cạnh có cùng nhãn`a`: một chuỗi hai chiều hoàn chỉnh`1 <-> 2 <-> 3`. 

Mọi cặp đều có thể truy cập được thông qua việc gói lặp đi lặp lại vì bất kỳ cạnh nào đến đều có thể khớp với bất kỳ cạnh nào đi ra. Hàng đợi nhanh chóng bão hòa tất cả`n^2`cặp, cho thấy lý do tại sao thuật toán phải dựa vào sự trùng lặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M²) | Mỗi bản mở rộng so sánh các cạnh vào và ra; tổng số tương tác cạnh theo cặp được giới hạn bởi các tổ hợp cạnh bậc hai | 
| Không gian | O(n²) | Bảng Boolean lưu trữ xem mỗi cặp đỉnh có được nhìn thấy hay không | 

Cấu trúc của thuật toán đảm bảo rằng mỗi cặp được xếp vào hàng đợi tối đa một lần và mỗi trình kích hoạt trong hàng đợi hoạt động tỷ lệ thuận với các sản phẩm cấp độ cục bộ. Vì tổng số tương tác cạnh bị giới hạn bởi sự kết hợp của các cạnh, quá trình tổng thể vẫn nằm trong giới hạn bậc hai về số cạnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque, defaultdict

    n, m = map(int, sys.stdin.readline().split())
    in_edges = [[] for _ in range(n)]
    out_edges = [[] for _ in range(n)]
    edges = []

    for _ in range(m):
        u, v, c = sys.stdin.readline().split()
        u = int(u) - 1
        v = int(v) - 1
        in_edges[v].append((u, c))
        out_edges[u].append((v, c))
        edges.append((u, v))

    seen = [[False]*n for _ in range(n)]
    q = deque()

    for i in range(n):
        seen[i][i] = True
        q.append((i, i))

    for u, v in edges:
        if not seen[u][v]:
            seen[u][v] = True
            q.append((u, v))

    while q:
        u, v = q.popleft()
        for a, c1 in in_edges[u]:
            for b, c2 in out_edges[v]:
                if c1 == c2 and not seen[a][b]:
                    seen[a][b] = True
                    q.append((a, b))

    return str(sum(1 for i in range(n) for j in range(n) if i != j and seen[i][j]))

# provided sample placeholder (no samples given)
assert run("3 3\n1 2 a\n2 3 a\n3 2 a\n") == "4"

# small cycle
assert run("2 2\n1 2 a\n2 1 a\n") == "2"

# single edge
assert run("2 1\n1 2 a\n") == "1"

# self only
assert run("1 0\n") == "0"

# symmetric labels
assert run("3 4\n1 2 a\n2 1 a\n2 3 a\n3 2 a\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chu kỳ chuỗi 3 nút | 4 | truyền qua nút trung gian | 
| 2 nút hai chiều | 2 | sự đối xứng đóng ngay lập tức | 
| cạnh đơn | 1 | độ chính xác khởi tạo cơ sở | 
| nút đơn | 0 | loại trừ các cặp đường chéo | 
| 3 nút đối xứng hoàn toàn | 6 | tăng trưởng đóng cửa dày đặc | 

## Vỏ cạnh 

Trường hợp một cạnh là một biểu đồ chỉ có các vòng tự lặp ngầm thông qua quá trình khởi tạo. Thuật toán bắt đầu bằng cách chèn tất cả`(u, u)`ngay cả khi không có cạnh nào tồn tại. Vì đầu ra không bao gồm các cặp đường chéo nên một biểu đồ có`n`các đỉnh bị cô lập sẽ quay trở lại`0`. Các quá trình xếp hàng`(u, u)`nhưng không tìm thấy các cạnh vào và ra phù hợp, do đó không có cặp mới nào được tạo ra. 

Một trường hợp cạnh khác là khi nhiều cạnh chia sẻ điểm cuối và nhãn giống hệt nhau. Ví dụ: nếu có nhiều cạnh song song`u -> v`dán nhãn`a`, mỗi cái có thể liên tục kích hoạt việc mở rộng. các`seen`bảng đảm bảo rằng`(u, v)`chỉ được xử lý một lần, ngăn chặn việc khuếch đại bậc hai lặp đi lặp lại từ các cạnh trùng lặp. 

Trường hợp cạnh cuối cùng là một biểu đồ được kết nối chặt chẽ trong đó tất cả các cạnh đều có chung nhãn. Trong tình huống này, mọi cạnh vào có thể ghép nối với mọi cạnh đi ở mỗi bước. Thuật toán vẫn kết thúc đúng vì mỗi cặp`(u, v)`chỉ được chèn một lần và tổng số cặp được giới hạn bởi`n^2`, mặc dù công việc kết hợp trung gian rất dày đặc.
