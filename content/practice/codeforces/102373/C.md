---
title: "CF 102373C - Kim cương"
description: "Chúng ta có một đồ thị vô hướng đơn giản với tối đa 300.000 đỉnh và 300.000 cạnh. Một viên kim cương bao gồm hai hình tam giác khác nhau có cùng một cạnh. Nếu một cạnh có nhiều đỉnh nối với cả hai điểm cuối của nó thì mỗi cặp cạnh chung đó tạo thành một viên kim cương."
date: "2026-08-12T22:52:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102373
codeforces_index: "C"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434 \u0434\u043b\u044f \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102373
solve_time_s: 519
verified: true
draft: false
---

[CF 102373C - Kim cương](https://codeforces.com/problemset/problem/102373/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 39 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng đơn giản với tối đa 300.000 đỉnh và 300.000 cạnh. Một viên kim cương bao gồm hai hình tam giác khác nhau có cùng một cạnh. Nếu một cạnh có nhiều đỉnh nối với cả hai điểm cuối của nó thì mỗi cặp cạnh chung đó tạo thành một viên kim cương. 

Giả sử cạnh là (u-v) và chính xác các đỉnh (c) liền kề với cả (u) và (v). Mỗi lân cận chung cho một tam giác chứa (u-v), do đó, việc chọn hai lân cận chung bất kỳ sẽ tạo ra một cặp tam giác có chung cạnh đó. Do đó, sự đóng góp của cạnh này là 

[ 
\binom{c}{2}=\frac{c(c-1)}2. 
] 

Nhiệm vụ là tính tổng giá trị này trên mọi cạnh. 

Biểu đồ có thể chứa 300.000 cạnh, do đó thuật toán kiểm tra từng cặp đỉnh đã quá lớn. Thuật toán (O(n^2)) sẽ có khoảng (9\cdot10^{10}) cặp đỉnh trong trường hợp lớn nhất, trong khi cách tiếp cận (O(n^3)) hoặc (O(n^4)) hoàn toàn nằm ngoài phạm vi. Giới hạn chính thức là 2 giây và 512 MB, vì vậy giải pháp cần xử lý đồ thị gần tuyến tính hoặc gần đúng (m\sqrt m) thay vì liệt kê đầy đủ. 

Có một số trường hợp khó xử lý. Một biểu đồ có thể chứa các hình tam giác mà không chứa bất kỳ viên kim cương nào. Ví dụ,```
4 4
1 2
2 3
3 4
4 1
```không có hình tam giác nào cả nên đáp án là`0`. Việc triển khai đếm tam giác coi mỗi tam giác là một viên kim cương sẽ trả về không chính xác`1`. 

Một biểu đồ hoàn chỉnh trên bốn đỉnh là một trường hợp hữu ích khác:```
4 6
1 2
1 3
1 4
2 3
2 4
3 4
```Câu trả lời là`6`, không`1`. Mỗi cạnh thuộc về hai hình tam giác và mỗi cạnh trong số sáu cạnh cho một cặp hình tam giác. Việc thực hiện bất cẩn chỉ đếm từng bộ bốn đỉnh một lần sẽ bỏ lỡ năm viên kim cương. 

Cũng có thể có nhiều hình tam giác xung quanh một cạnh. Coi như```
5 7
1 2
1 3
2 3
1 4
2 4
1 5
2 5
```Cạnh (1-2) thuộc ba hình tam giác, sử dụng các đỉnh (3,4,5). Hai hình tam giác bất kỳ tạo thành một hình thoi, cho ra (\binom32=3). Đếm các hình tam giác và chỉ chia cho hai sẽ không hiệu quả vì mỗi hình tam giác có nhiều viên kim cương. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp nhất sẽ xem xét mọi tập hợp bốn đỉnh. Một viên kim cương luôn có đúng bốn đỉnh phân biệt. Trong số bốn đỉnh đó có sáu cạnh có thể. Nếu có chính xác năm thì bốn đỉnh tạo thành một viên kim cương. Nếu có tất cả sáu viên, chúng tạo thành một (K_4), chứa sáu viên kim cương khác nhau, mỗi viên đại diện cho một lựa chọn của cạnh chung. 

Do đó, với mỗi bộ tứ, chúng ta có thể kiểm tra sáu cạnh có thể có của nó và thêm số 0, một hoặc sáu vào câu trả lời. Điều này đúng nhưng có 

[ 
\binom n4=O(n^4) 
] 

tăng gấp bốn lần. Tại (n=300000), giá trị này ở mức (3,4\cdot10^{20}) gấp bốn lần, trước cả khi tính đến sáu lần kiểm tra cạnh trên mỗi bốn lần. Cách tiếp cận này chỉ hữu ích như một đường cơ sở mang tính khái niệm. 

Quan sát quan trọng là ngừng nhìn trực tiếp vào bốn đỉnh. Đối với mỗi cạnh (u-v), thông tin duy nhất cần có là số láng giềng chung của nó. Thay vì xây dựng rõ ràng từng cặp tam giác, trước tiên chúng ta có thể liệt kê từng tam giác và ghi lại ba cạnh nào thuộc về tam giác đó. 

Nếu một cạnh đã xuất hiện trong (k) tam giác và một tam giác khác chứa cạnh đó được tìm thấy, thì tam giác mới đó sẽ tạo thành chính xác (k) hình thoi mới với các tam giác được tìm thấy trước đó. Vì vậy, khi phát hiện ra một hình tam giác, đối với mỗi cạnh trong số ba cạnh của nó, chúng ta cộng số lượng tam giác hiện tại của cạnh đó vào câu trả lời, sau đó tăng số lượng tam giác của cạnh đó. 

Vấn đề còn lại là liệt kê tam giác hiệu quả. Việc kiểm tra từng cặp lân cận của mỗi đỉnh có thể là phương trình bậc hai trên một ngôi sao hoặc một đồ thị khác có đỉnh bậc cao. Cách tiêu chuẩn để giải quyết vấn đề này là định hướng mọi cạnh theo độ đỉnh. Một cạnh đi từ điểm cuối có bậc nhỏ hơn đến điểm cuối có bậc lớn hơn, phá vỡ các mối quan hệ có bậc bằng nhau theo số đỉnh. Sau đó, chúng tôi chỉ tìm kiếm các hình tam giác thông qua các cạnh phía trước này. 

Thứ tự bậc này giới hạn tổng lượng thăm dò hình nêm phía trước bằng (O(m\sqrt m)), giới hạn đếm tam giác định hướng theo độ tiêu chuẩn. Ý tưởng tương tự thường được sử dụng để liệt kê tam giác và bốn chu kỳ hiệu quả trong các biểu đồ thưa thớt. 

Lực lượng vũ phu hoạt động vì mọi viên kim cương đều có cấu trúc bốn đỉnh, nhưng không thành công vì có quá nhiều bộ bốn đỉnh. Nhận xét rằng một viên kim cương chỉ đơn giản là một cặp hình tam giác có chung một cạnh cho phép chúng ta đơn giản hóa vấn đề thành phép liệt kê tam giác cộng với một bộ đếm nhỏ cho mỗi cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^4)) | (O(m)) | Quá chậm | 
| Tối ưu | (O(m\sqrt m)) | (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đồ thị và tính bậc của mỗi đỉnh. Chúng ta cần độ trước khi định hướng các cạnh vì hướng là yếu tố giữ cho việc tìm kiếm tam giác sau này nhỏ gọn. 
2. Sắp xếp thứ tự cho mỗi đỉnh dựa trên`(degree, vertex_id)`. Một cạnh được hướng từ đỉnh nhỏ hơn theo thứ tự này tới đỉnh lớn hơn. Độ bằng nhau được giải quyết bằng số đỉnh, do đó mỗi cạnh nhận được chính xác một hướng. 
3. Chỉ lưu trữ các cạnh đi của mỗi đỉnh. Cùng với đích đến, hãy lưu trữ ID cạnh ban đầu, vì sau này chúng ta cần cập nhật số lượng tam giác thuộc cạnh đó. 
4. Duy trì một mảng`mark`. Trong khi xử lý một đỉnh (v), hãy đánh dấu mọi hàng xóm đi ra (w) bằng ID cạnh của (v-w). Điều này cho phép truy cập liên tục vào cạnh giữa (v) và một đỉnh được đánh dấu. 
5. Đối với mỗi cạnh đi ra (v\rightarrow u), quét tất cả các cạnh đi ra (u\rightarrow w). Nếu (w) hiện được đánh dấu bằng (v) thì (v,u,w) tạo thành một hình tam giác. Ba ID cạnh được biết ngay lập tức: cạnh (v-u), cạnh (u-w) và cạnh được đánh dấu (v-w). 
6. Khi tìm thấy một hình tam giác, giả sử ba cạnh của nó hiện thuộc về (a), (b) và (c) các hình tam giác đã tìm thấy trước đó. Tam giác mới tạo thành (a+b+c) những viên kim cương mới, bởi vì nó có thể được ghép với mọi tam giác trước đó có chung bất kỳ cạnh nào trong số đó. Thêm giá trị đó vào câu trả lời, sau đó tăng cả ba bộ đếm cạnh. 
7. Xóa các dấu thuộc về các láng giềng đi ra của (v) trước khi chuyển sang đỉnh tiếp theo. Bước xóa sẽ ngăn chặn một đỉnh được đánh dấu cho một trung tâm khỏi bị nhầm lẫn với đỉnh lân cận của một trung tâm khác. 

### Tại sao nó hoạt động 

Mỗi tam giác có một đỉnh nhỏ nhất duy nhất theo tổng thứ tự đã chọn. Hai đỉnh còn lại của nó đều lớn hơn đỉnh đó và cạnh giữa hai đỉnh lớn hơn đó hướng từ đỉnh nhỏ hơn đến đỉnh lớn hơn. Do đó, tam giác được phát hiện đúng một lần khi đỉnh nhỏ nhất của nó được xử lý. 

Đối với mỗi cạnh, bộ đếm của nó chính xác là số lượng hình tam giác được phát hiện cho đến nay có chứa cạnh đó. Khi tìm thấy một hình tam giác mới chứa cạnh, nó sẽ tạo thành một viên kim cương mới với mỗi hình tam giác được tính trước đó trên cạnh đó. Việc thêm bộ đếm cũ trước khi tăng nó sẽ đếm từng cặp hình tam giác chính xác một lần. Vì mỗi viên kim cương chính xác là một cặp hình tam giác có chung một cạnh nên câu trả lời tích lũy chính xác là số lượng kim cương cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    degree = [0] * n
    edges = [None] * m

    for eid in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        edges[eid] = (u, v)
        degree[u] += 1
        degree[v] += 1

    order = sorted(range(n), key=lambda v: (degree[v], v))
    pos = [0] * n
    for i, v in enumerate(order):
        pos[v] = i

    out = [[] for _ in range(n)]

    for eid, (u, v) in enumerate(edges):
        if pos[u] < pos[v]:
            out[u].append((v, eid))
        else:
            out[v].append((u, eid))

    triangle_count = [0] * m
    mark = [-1] * n
    answer = 0

    for v in range(n):
        ov = out[v]

        for w, eid in ov:
            mark[w] = eid

        for u, eid_vu in ov:
            for w, eid_uw in out[u]:
                eid_vw = mark[w]

                if eid_vw != -1:
                    answer += (
                        triangle_count[eid_vu]
                        + triangle_count[eid_uw]
                        + triangle_count[eid_vw]
                    )

                    triangle_count[eid_vu] += 1
                    triangle_count[eid_uw] += 1
                    triangle_count[eid_vw] += 1

        for w, _ in ov:
            mark[w] = -1

    print(answer)

if __name__ == "__main__":
    solve()
```Bản dựng đường chuyền đầu tiên`degree`và lưu trữ mọi cạnh bằng ID ổn định. ID là cần thiết vì câu trả lời cuối cùng phụ thuộc vào số lượng hình tam giác sử dụng mỗi cạnh cụ thể, chứ không chỉ phụ thuộc vào tổng số hình tam giác tồn tại. 

các`order`mảng thực hiện định hướng độ.`pos[v]`đưa ra vị trí của một đỉnh theo thứ tự đó, do đó mọi cạnh vô hướng có thể được định hướng bằng một so sánh. Danh sách lân cận gửi đi chứa`(neighbor, edge_id)`cặp, cho phép tìm kiếm tam giác xác định tất cả ba bộ đếm cạnh mà không cần tra cứu từ điển khác. 

các`mark`mảng được sử dụng lại cho mọi đỉnh. Khi xử lý`v`,`mark[w]`chứa ID cạnh của (v-w) cho mọi hàng xóm chuyển tiếp (w). Vì vậy, khi vòng lặp bên trong đạt tới một đỉnh`w`từ`u`,`mark[w] != -1`chính xác là điều kiện (v-w) cũng tồn tại. 

Câu trả lời được cập nhật trước khi ba bộ đếm được tăng lên. Nếu một cạnh đã xuất hiện trong (k) các tam giác trước đó thì tam giác mới sẽ tạo ra (k) cặp mới với cạnh đó. Cập nhật trước tiên sẽ tính hình tam giác mới được ghép nối với chính nó, điều này không hợp lệ. 

Số nguyên Python có độ chính xác tùy ý, do đó, ngay cả câu trả lời lớn nhất có thể cũng không bị tràn. Ví dụ: một biểu đồ hoàn chỉnh có 775 đỉnh đã có 89.491.021.650 viên kim cương. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đồ thị là bốn chu kỳ. Mỗi đỉnh đều có bậc hai, do đó ID đỉnh sẽ phá vỡ các mối ràng buộc. Không tìm kiếm chuyển tiếp tìm thấy một hình tam giác. 

| Đỉnh được xử lý | Chuyển tiếp hàng xóm | Tìm thấy tam giác | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 2, 4 | không | 0 | 
| 2 | 3, 4 | không | 0 | 
| 3 | 4 | không | 0 | 
| 4 | không | không | 0 | 

Tính chất quan trọng ở đây là một cạnh có thể xuất hiện mà không tham gia vào một tam giác. Thuật toán không bao giờ tạo ra một viên kim cương chỉ vì hai cạnh gặp nhau tại một đỉnh. 

Đối với Mẫu 2, có hai hình tam giác (1-2-3) và (1-3-4), có chung cạnh (1-3). Thứ tự mức độ là (2,4,1,3). Tam giác đầu tiên tăng số đếm của các cạnh (1-2), (2-3) và (1-3). Tam giác thứ hai tăng các bộ đếm của (1-4), (3-4) và (1-3), và bộ đếm trước đó của (1-3) đóng góp một viên kim cương mới. 

| Đỉnh được xử lý | Tam giác | Bộ đếm cạnh trước | Đã thêm vào câu trả lời | Trả lời | 
| --- | --- | --- | --- | --- | 
| 2 | (1,2,3) | (0,0,0) | 0 | 0 | 
| 4 | (1,3,4) | cạnh (1-3) có 1 | 1 | 1 | 
| 1 | không | không thay đổi | 0 | 1 | 
| 3 | không | không thay đổi | 0 | 1 | 

Dấu vết chứng tỏ tại sao chỉ đếm các hình tam giác là không đủ. Hình tam giác đầu tiên không tạo ra viên kim cương nào vì không có gì để ghép với nó. Tam giác thứ hai tạo đúng một cặp với tam giác thứ nhất, đưa ra câu trả lời cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(m\sqrt m+n\log n)) | Sắp xếp các đỉnh mất (O(n\log n)); phép liệt kê tam giác định hướng độ lấy (O(m\sqrt m)) | 
| Không gian | (O(n+m)) | Độ, mảng thứ tự, nhãn hiệu, ID cạnh, bộ đếm và danh sách kề cận có định hướng đều là tuyến tính | 

Với (m\le300000), giới hạn định hướng độ gần như là (m\sqrt m), chứ không phải là hàm bậc hai hoặc bậc ba của số đỉnh. Bản thân biểu đồ cũng chỉ có (O(m)) mục nhập kề được lưu trữ, do đó mức sử dụng bộ nhớ vẫn tuyến tính và phù hợp với giới hạn 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    m = int(next(it))

    degree = [0] * n
    edges = [None] * m

    for eid in range(m):
        u = int(next(it)) - 1
        v = int(next(it)) - 1
        edges[eid] = (u, v)
        degree[u] += 1
        degree[v] += 1

    order = sorted(range(n), key=lambda v: (degree[v], v))
    pos = [0] * n

    for i, v in enumerate(order):
        pos[v] = i

    out = [[] for _ in range(n)]

    for eid, (u, v) in enumerate(edges):
        if pos[u] < pos[v]:
            out[u].append((v, eid))
        else:
            out[v].append((u, eid))

    cnt = [0] * m
    mark = [-1] * n
    ans = 0

    for v in range(n):
        for w, eid in out[v]:
            mark[w] = eid

        for u, e1 in out[v]:
            for w, e2 in out[u]:
                e3 = mark[w]
                if e3 != -1:
                    ans += cnt[e1] + cnt[e2] + cnt[e3]
                    cnt[e1] += 1
                    cnt[e2] += 1
                    cnt[e3] += 1

        for w, _ in out[v]:
            mark[w] = -1

    return str(ans)

# Provided sample 1
assert solve_data("""\
4 4
1 2
2 3
3 4
4 1
""") == "0", "sample 1"

# Provided sample 2
assert solve_data("""\
4 5
1 2
2 3
3 4
4 1
1 3
""") == "1", "sample 2"

# Provided sample 3
assert solve_data("""\
4 6
1 2
2 3
3 4
4 1
1 3
2 4
""") == "6", "sample 3"

# Minimum-size graph with exactly two triangles sharing one edge
assert solve_data("""\
4 5
1 2
2 3
3 1
1 4
2 4
""") == "1", "minimum diamond"

# Three triangles sharing the same edge: C(3, 2) = 3
assert solve_data("""\
5 7
1 2
1 3
2 3
1 4
2 4
1 5
2 5
""") == "3", "three triangles around one edge"

# Complete graph K4: every one of the six edges is a shared edge
assert solve_data("""\
4 6
1 2
1 3
1 4
2 3
2 4
3 4
""") == "6", "complete K4"

# Maximum-size sparse graph: 300000 vertices and 300000 edges.
# A star plus one extra edge still contains no triangle.
n = 300000
parts = [f"{n} 300000"]
parts.extend(f"1 {v}" for v in range(2, n + 1))
parts.append("2 3")
max_case = "\n".join(parts) + "\n"

assert solve_data(max_case) == "0", "maximum-size sparse graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bốn chu kỳ 4 cạnh | 0 | Đồ thị có cạnh nhưng không có hình tam giác | 
| Hai tam giác có chung một cạnh | 1 | Kim cương tối thiểu có thể | 
| Ba hình tam giác xung quanh một cạnh | 3 | Quy tắc đếm (\binom c2) | 
| Hoàn thành (K_4) | 6 | Nhiều viên kim cương trên bốn đỉnh giống nhau | 
| Đồ thị thưa thớt 300.000 đỉnh | 0 | Kích thước đầu vào tối đa và xử lý đỉnh ở mức độ cao | 

## Vỏ cạnh 

Bốn chu kỳ```
4 4
1 2
2 3
3 4
4 1
```không chứa hình tam giác. Trong quá trình định hướng, thuật toán có thể khám phá một số đường dẫn hai cạnh, nhưng không có đường dẫn nào đóng vào cạnh thuận. Không có bộ đếm cạnh nào được tăng lên, vì vậy câu trả lời cuối cùng vẫn còn`0`. 

Kim cương tối thiểu```
4 5
1 2
2 3
3 1
1 4
2 4
```chứa các hình tam giác (1-2-3) và (1-2-4). Khi tìm thấy tam giác đầu tiên, bộ đếm cạnh (1-2) sẽ trở thành một. Khi tìm thấy tam giác thứ hai, bộ đếm cũ của (1-2) đóng góp một phần vào câu trả lời. Đầu ra cuối cùng là`1`. 

đồ thị```
5 7
1 2
1 3
2 3
1 4
2 4
1 5
2 5
```có ba hình tam giác chia sẻ (1-2). Bộ đếm của (1-2) thay đổi từ 0 thành 1 sau tam giác đầu tiên, từ 1 thành 2 sau tam giác thứ hai và từ 2 thành 3 sau tam giác thứ ba. Câu trả lời nhận được là (0+1+2=3), chính xác là (\binom32). 

Cuối cùng, trong```
4 6
1 2
1 3
1 4
2 3
2 4
3 4
```mọi cạnh đều thuộc hai tam giác. Mỗi cạnh đóng góp (\binom22=1) và có sáu cạnh, vì vậy câu trả lời là`6`. Đây là trường hợp bắt được các triển khai chỉ tính mỗi sơ đồ con hình kim cương bốn đỉnh một lần và vô tình coi (K_4) là một hình thoi.
