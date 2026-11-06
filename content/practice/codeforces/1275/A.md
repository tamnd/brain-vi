---
title: "CF 1275A - \u0421\u043a\u0440\u044b\u0442\u044b\u0439 \u0434\u0440\u0443\u0433"
description: "Chúng ta được cung cấp một biểu đồ tình bạn trực tiếp của $n$ người dùng, trong đó mỗi người dùng liệt kê một số người dùng khác là bạn bè. Chi tiết quan trọng là tình bạn không đảm bảo sẽ có nhau. Nếu người dùng $u$ liệt kê $v$, điều đó không có nghĩa là $v$ liệt kê $u$."
date: "2026-06-16T01:29:22+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 1275
codeforces_index: "A"
codeforces_contest_name: "VK Cup 2019 - \u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f (Engine)"
rating: 0
weight: 1275
solve_time_s: 285
verified: false
draft: false
---

[CF 1275A - \u0421\u043a\u0440\u044b\u0442\u044b\u0439 \u0434\u0440\u0443\u0433](https://codeforces.com/problemset/problem/1275/A) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 4 phút 45 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ tình bạn có hướng của$n$người dùng, trong đó mỗi người dùng liệt kê một số người dùng khác là bạn bè. Chi tiết quan trọng là tình bạn không đảm bảo sẽ có nhau. Nếu người dùng$u$danh sách$v$, điều đó không có nghĩa là$v$danh sách$u$. 

Nhiệm vụ là phát hiện tất cả “tình bạn ẩn”, nghĩa là tất cả các cặp có thứ tự$(u, v)$như vậy$u$xem xét$v$một người bạn nhưng$v$không xem xét$u$một người bạn. Trong thuật ngữ đồ thị, chúng ta đang tìm kiếm tất cả các cạnh có hướng không tương ứng trong danh sách kề. 

Đầu vào về cơ bản là biểu diễn danh sách kề của đồ thị có hướng với các đỉnh từ 1 đến$n$. Đầu ra yêu cầu liệt kê mọi cạnh có hướng không có cạnh ngược và đếm xem tồn tại bao nhiêu cạnh như vậy. 

Từ$n \le 100$, tổng số cạnh nhiều nhất là khoảng$n^2$, nhiều nhất là 10.000. Giá trị này đủ nhỏ để bất kỳ giải pháp nào kiểm tra tính kề cận trong$O(1)$hoặc thậm chí$O(n)$mỗi cạnh sẽ đủ nhanh. Một sự ngây thơ$O(n^3)$cách tiếp cận này cũng sẽ trôi qua, nhưng không cần thiết. 

Một điểm tinh tế là các bản sao không tồn tại trong mỗi danh sách kề, nhưng sự bất đối xứng giữa các danh sách chính xác là điều xác định câu trả lời. Một trường hợp khác là những người dùng không có bạn bè, điều này sẽ không tạo ra bất kỳ lợi thế ẩn nào nhưng vẫn có thể xuất hiện dưới dạng mục tiêu trong danh sách của người khác. 

Một sai lầm phổ biến là giả sử tính đối xứng hoặc chỉ kiểm tra một hướng một lần mà không xác minh sự tồn tại trong danh sách đối diện. Một sai lầm khác là quên rằng đầu ra phải bao gồm tất cả các cạnh không đối xứng, không chỉ các cặp người dùng khác nhau theo cả hai hướng cùng một lúc. 

Ví dụ: nếu người dùng 1 liệt kê 2, nhưng người dùng 2 không liệt kê 1 thì chúng ta phải xuất (1, 2). Nếu người dùng 2 liệt kê 1 nhưng 1 không liệt kê 2, chúng tôi cũng xuất ra (2, 1). Đây là những kiểm tra có chỉ đạo độc lập, không phải là sự mâu thuẫn không có chỉ đạo. 

## Phương pháp tiếp cận 

Ý tưởng đơn giản là lặp lại mọi người dùng$u$và cho mọi người bạn$v$TRONG$u$danh sách của, hãy kiểm tra xem$u$xuất hiện ở$v$danh sách của. Nếu không thì$(u, v)$là một tình bạn giấu kín. 

Trong quá trình triển khai brute-force, việc kiểm tra tư cách thành viên trong$v$danh sách của sử dụng tìm kiếm tuyến tính dẫn đến$O(n)$mỗi lần kiểm tra. Vì mỗi người dùng có thể có tối đa$O(n)$các bạn và chúng tôi thực hiện kiểm tra từng cạnh, tổng độ phức tạp sẽ trở thành$O(n^3)$trong trường hợp xấu nhất. Với$n = 100$, đây vẫn là ranh giới có thể chấp nhận được, nhưng không hiệu quả về mặt cấu trúc và không cần thiết. 

Cải tiến quan trọng đến từ việc biểu diễn từng danh sách kề dưới dạng tập hợp băm. Điều này biến các truy vấn thành viên thành mức trung bình$O(1)$hoạt động. Khi đó, với mọi cạnh có hướng$u \to v$, chúng ta chỉ cần kiểm tra xem$u \in adj[v]$. Nếu không, chúng tôi xuất nó. 

Điều này có tác dụng vì sự cố giảm xuống còn các truy vấn tồn tại cạnh trong biểu đồ có hướng và các tập hợp dựa trên hàm băm cung cấp các kiểm tra thành viên theo thời gian không đổi, thu gọn cấu trúc ba lồng nhau thành quét bậc hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tìm kiếm danh sách) |$O(n^3)$|$O(n^2)$| Được chấp nhận nhưng không hiệu quả | 
| Tra cứu bộ băm |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc danh sách kề cho mỗi người dùng và lưu trữ dưới dạng một bộ. Điều này đảm bảo kiểm tra tư cách thành viên nhanh chóng khi xác minh tính có đi có lại. 
2. Khởi tạo một danh sách trống để lưu trữ câu trả lời. 
3. Đối với mỗi người dùng$u$, lặp lại từng người bạn$v$trong bộ của họ. Điều này liệt kê tất cả các cạnh có hướng trong biểu đồ. 
4. Cho mỗi cặp$(u, v)$, kiểm tra xem$u$có mặt trong tập kề của$v$. Nếu không thì nối thêm$(u, v)$vào danh sách câu trả lời. Điều này trực tiếp xác định các cạnh ngược bị thiếu. 
5. Sau khi xử lý tất cả người dùng, xuất ra tổng số cặp được lưu trữ theo sau mỗi cặp. 

Lý do đằng sau bước 4 là tính tương hỗ trong đồ thị có hướng chính xác là điều kiện$u \in adj[v]$. Không đạt được điều kiện này có nghĩa là cạnh bị lệch một phía. 

### Tại sao nó hoạt động 

Chúng ta duy trì bất biến mà mọi cặp ứng cử viên$(u, v)$xuất phát trực tiếp từ mối quan hệ kề cận rõ ràng trong đầu vào. Chúng tôi chỉ xuất ra một cặp nếu không có mối quan hệ ngược lại của nó. Vì chúng tôi quét mọi cạnh có hướng chính xác một lần và kiểm tra sự tồn tại nghịch đảo của nó một cách độc lập, nên không thể bỏ qua tình hữu nghị ẩn nào và không có cạnh đối ứng hợp lệ nào được đưa vào không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
adj = [set() for _ in range(n + 1)]

for i in range(1, n + 1):
    data = list(map(int, input().split()))
    cnt = data[0]
    for v in data[1:]:
        adj[i].add(v)

res = []

for u in range(1, n + 1):
    for v in adj[u]:
        if u not in adj[v]:
            res.append((u, v))

print(len(res))
for u, v in res:
    print(u, v)
```Việc triển khai lưu trữ từng danh sách kề dưới dạng một tập hợp để việc kiểm tra tư cách thành viên diễn ra liên tục. Vòng lặp bên ngoài lặp qua tất cả người dùng và vòng lặp bên trong lặp qua những người bạn đã khai báo của họ. Quyết định quan trọng là điều kiện`u not in adj[v]`, mã hóa trực tiếp yêu cầu bất đối xứng. 

Một sai lầm phổ biến là quên chuyển đổi danh sách thành tập hợp, điều này âm thầm làm giảm hiệu suất đối với việc kiểm tra thành viên bậc hai bên trong vòng lặp bậc hai. Một vấn đề tế nhị khác là việc lặp lại các danh sách đầu vào thô mà không duy trì tính nhất quán của chỉ mục, điều này có thể gây ra từng lỗi một. Ở đây chúng tôi phân bổ rõ ràng`n + 1`để việc đánh số người dùng phù hợp với các chỉ số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2
1 3
1 1
```Chúng ta xây dựng các tập kề: 

| bạn | adj[u] | 
| --- | --- | 
| 1 | {2} | 
| 2 | {3} | 
| 3 | {1} | 

Bây giờ chúng tôi quét các cạnh: 

| bạn | v | adj[v] | bạn ở adj[v]? | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | {3} | Không | (1,2) | 
| 2 | 3 | {1} | Không | (2,3) | 
| 3 | 1 | {2} | Không | (3,1) | 

Đầu ra là cả ba cạnh. 

Điều này xác nhận rằng thuật toán xử lý từng cạnh có hướng một cách độc lập và không giả định tính đối xứng. 

### Ví dụ 2 

đầu vào:```
4
2 2 3
1 1
1 2
0
```liền kề: 

| bạn | adj[u] | 
| --- | --- | 
| 1 | {2,3} | 
| 2 | {1} | 
| 3 | {2} | 
| 4 | {} | 

Quét: 

| bạn | v | adj[v] | bạn ở adj[v]? | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | {1} | Có | - | 
| 1 | 3 | {2} | Không | (1,3) | 
| 2 | 1 | {2,3} | Có | - | 
| 3 | 2 | {1} | Có | - | 

Điều này cho thấy các cạnh tương hỗ được lọc chính xác trong khi các cạnh không đối xứng vẫn còn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi cái nhiều nhất$n^2$các cạnh có hướng được kiểm tra một lần với tra cứu thành viên O(1) | 
| Không gian |$O(n^2)$| Lưu trữ các tập kề trong trường hợp xấu nhất | 

Với$n \le 100$, các hoạt động tối đa theo thứ tự 10.000 lần kiểm tra cạnh, điều này không đáng kể trong các ràng buộc điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    n = int(input())
    adj = [set() for _ in range(n + 1)]

    for i in range(1, n + 1):
        data = list(map(int, input().split()))
        for v in data[1:]:
            adj[i].add(v)

    res = []
    for u in range(1, n + 1):
        for v in adj[u]:
            if u not in adj[v]:
                res.append((u, v))

    out = [str(len(res))]
    for u, v in res:
        out.append(f"{u} {v}")
    return "\n".join(out)

# sample
assert run("""3
1 2
1 3
1 1
""") == """3
1 2
2 3
3 1"""

# custom 1: no edges
assert run("""2
0
0
""") == """0"""

# custom 2: fully mutual
assert run("""3
1 2
1 1 3
1 2
""") == """0"""

# custom 3: chain asymmetry
assert run("""4
1 2
1 3
1 4
0
""") == """3
1 2
2 3
3 4"""

# custom 4: single node edges missing back
assert run("""2
1 2
0
""") == """1
1 2"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị trống | 0 | trường hợp không có cạnh | 
| đồ thị lẫn nhau | 0 | lọc đối xứng | 
| đồ thị chuỗi | 3 cạnh | truyền định hướng | 
| đơn thiếu đảo ngược | 1 cạnh | sự bất đối xứng cơ bản | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một nút không có cạnh đi ra. Ví dụ: nếu người dùng 2 có danh sách trống, nó sẽ không bao giờ tạo ra các cặp đi nhưng vẫn có thể là mục tiêu của tình bạn ẩn giấu của người khác. Thuật toán xử lý việc này một cách tự nhiên vì chúng tôi chỉ lặp lại các mục lân cận hiện có. 

Một trường hợp khác là một chuỗi hoàn toàn bất đối xứng, trong đó mọi cạnh đều hướng về phía trước nhưng không bao giờ lùi lại. Thuật toán sẽ xuất ra tất cả các cạnh vì mọi tra cứu ngược lại đều thất bại. Điều này xác nhận rằng tính định hướng được bảo toàn chính xác và không có sự đối xứng tiềm ẩn nào được giả định. 

Trường hợp tinh tế cuối cùng là các cặp tương hỗ được nhúng trong các biểu đồ lớn hơn. Ví dụ: nếu 1 liệt kê 2 và 2 liệt kê 1, nhưng 1 cũng liệt kê 3 trong khi 3 không liệt kê 1 thì chỉ có (1,3) xuất hiện. Thuật toán cô lập từng cạnh có hướng một cách độc lập, do đó sự hiện diện của các cạnh lẫn nhau không liên quan không ảnh hưởng đến tính chính xác.
