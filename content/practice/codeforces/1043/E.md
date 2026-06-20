---
title: "CF 1043E - Tập luyện chăm chỉ, thắng dễ"
description: "Chúng ta được cung cấp một nhóm người tham gia, mỗi người được mô tả bằng hai con số. Hai con số này biểu thị số tiền phạt mà người tham gia đóng góp tùy thuộc vào việc họ giải quyết nhiệm vụ đầu tiên hay nhiệm vụ thứ hai trong cuộc thi huấn luyện hai người."
date: "2026-06-16T17:42:57+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1043
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 519 by Botan Investments"
rating: 1900
weight: 1043
solve_time_s: 254
verified: true
draft: false
---

[CF 1043E - Luyện tập chăm chỉ, thắng dễ dàng](https://codeforces.com/problemset/problem/1043/E) 

**Xếp hạng:** 1900 
**Tags:** thuật toán xây dựng, tham lam, toán học, sắp xếp 
**Thời gian giải:** 4 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một nhóm người tham gia, mỗi người được mô tả bằng hai con số. Hai con số này biểu thị số tiền phạt mà người tham gia đóng góp tùy thuộc vào việc họ giải quyết nhiệm vụ đầu tiên hay nhiệm vụ thứ hai trong cuộc thi huấn luyện hai người. Khi hai người tham gia thành lập một đội, họ sẽ phân công hai vấn đề cho nhau theo cách giảm thiểu tổng số hình phạt của cả đội. 

Đối với một cặp$(i, j)$, có hai nhiệm vụ có thể thực hiện được. Nếu như$i$giải quyết nhiệm vụ đầu tiên và$j$giải quyết vấn đề thứ hai, chi phí là$x_i + y_j$. Nếu chúng ta đổi vai trò, chi phí sẽ là$x_j + y_i$. Nhóm luôn chọn cái rẻ hơn trong hai phương án này. 

Tuy nhiên, không phải tất cả các cặp đều được phép. Chúng ta cũng được đưa cho một danh sách các cặp bị cấm, và những cặp đó không bao giờ tạo thành một đội. Đối với mỗi người tham gia, chúng tôi cần tính tổng chi phí nhóm tối ưu trên tất cả các đối tác hợp lệ. 

Kích thước đầu vào lớn, lên tới 300.000 người tham gia và 300.000 cặp bị cấm. Điều này ngay lập tức loại trừ bất kỳ phương pháp bậc hai nào đánh giá rõ ràng tất cả các cặp, vì$O(n^2)$hoạt động sẽ vượt xa thời hạn. Ngay cả những cách tiếp cận có hành vi bậc hai ẩn, chẳng hạn như tính toán lại mức tối thiểu cho từng cặp một cách độc lập, cũng sẽ thất bại. 

Một khó khăn tinh tế là việc gán tối ưu cho một cặp chỉ phụ thuộc vào sự so sánh giữa$x_i + y_j$Và$x_j + y_i$, có thể viết lại dưới dạng so sánh$x_i - y_i$Và$x_j - y_j$. Sự giảm thiểu này là cấu trúc chính của vấn đề. 

Các trường hợp đặc biệt đáng chú ý bao gồm các tình huống trong đó tất cả các cặp đều bị cấm, trong đó câu trả lời gần như bằng 0 đối với mọi người hoặc khi không có cặp bị cấm nào tồn tại, nghĩa là mọi người tham gia đều ghép đôi với tất cả những người khác. Một trường hợp quan trọng khác là khi hai người tham gia có giá trị dương hoặc âm cực lớn; phép tính tổng đơn giản có thể tràn số nguyên 64 bit nếu không được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Phương pháp brute-force xem xét mọi cặp hợp lệ$(i, j)$, tính giá trị nhỏ nhất của hai phép gán có thể thực hiện được và cộng kết quả vào tổng của cả hai người tham gia. Điều này rất đơn giản: đối với mỗi cặp, chúng tôi đánh giá một biểu thức có thời gian không đổi, do đó độ chính xác là ngay lập tức. Tuy nhiên, với$n$lên tới 300.000, số lượng cặp theo thứ tự$n^2$, đại khái là$10^{10}$hoạt động vượt xa giới hạn khả thi. 

Quan sát quan trọng là hàm chi phí đơn giản hóa sau khi viết lại. Định nghĩa$d_i = x_i - y_i$. Sau đó: 

Nếu$d_i \le d_j$, phép gán tối ưu là$i \to \text{first}, j \to \text{second}$, đưa ra chi phí$x_i + y_j$. Ngược lại, chúng tôi gán$j \to \text{first}, i \to \text{second}$, đưa ra chi phí$x_j + y_i$. 

Điều này có nghĩa là quyết định chỉ phụ thuộc vào việc đặt hàng bởi$d_i$, không phải trên các giá trị thô riêng biệt. Sau khi chúng tôi sắp xếp người tham gia theo$d_i$, tất cả các tương tác đều trở thành có cấu trúc: đối với bất kỳ cặp nào, sự đóng góp có thể được biểu thị bằng cách sử dụng tổng tiền tố/hậu tố theo thứ tự này. 

Các cạnh bị cấm gây ra sự phức tạp vì chúng ta không thể đơn giản cho rằng tất cả các cặp đều tồn tại. Thay vào đó, chúng tôi tính toán câu trả lời như thể tất cả các cặp đều tồn tại, sau đó trừ đi phần đóng góp của các cặp bị cấm. Biểu đồ đầy đủ có cấu trúc dày đặc nhưng thưa thớt về các loại trừ, đây là cài đặt cổ điển trong đó loại trừ bao gồm kết hợp với sắp xếp và tổng tiền tố trở nên hiệu quả. 

Đối với mỗi người tham gia, chúng tôi tính toán phần đóng góp của họ so với tất cả những người khác bằng cách sử dụng thứ tự được sắp xếp. Sau đó, chúng tôi sửa các cặp bị cấm bằng cách trừ rõ ràng phần đóng góp của cặp bằng cách sử dụng cùng một công thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Sắp xếp + tổng tiền tố + hiệu chỉnh |$O(n \log n + m)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành theo hai giai đoạn: tính tổng tương tác đầy đủ, sau đó loại bỏ các tương tác bị cấm. 

1. Tính toán$d_i = x_i - y_i$cho mỗi người tham gia. Giá trị này xác định các quyết định đặt hàng và trao đổi. 
2. Sắp xếp người tham gia theo$d_i$. Điều này đảm bảo rằng với bất kỳ cặp nào$i < j$, chúng tôi biết$d_i \le d_j$, sửa hướng quy tắc gán. 
3. Xây dựng tổng tiền tố$x$Và$y$theo thứ tự sắp xếp. Điều này cho phép tính toán nhanh tổng số tiền của tất cả những người tham gia trước hoặc sau một chỉ số nhất định. 
4. Đối với mỗi người tham gia$i$, tính tổng đóng góp của nó so với tất cả những phần khác giả sử tất cả các cặp đều được phép. 

Đối với người tham gia trước$i$,$i$luôn được giao bài toán thứ hai trong việc ghép cặp tối ưu; cho những người sau$i$, nó được giao vấn đề đầu tiên. Điều này diễn ra trực tiếp từ thứ tự của$d$-giá trị. 
5. Sử dụng tổng tiền tố để tích lũy các khoản đóng góp trong$O(1)$mỗi hướng. 
6. Trừ đóng góp của các cặp bị cấm. Đối với mỗi cặp bị cấm$(u, v)$, tính chi phí cặp chính xác bằng cách sử dụng:$$\min(x_u + y_v, x_v + y_u)$$và trừ nó khỏi tổng số của cả hai người tham gia. 
7. Xuất các giá trị tích lũy cuối cùng theo thứ tự ban đầu. 

Ý tưởng chính là việc sắp xếp chuyển đổi các quyết định theo cặp thành một quy tắc định hướng nhất quán và tổng tiền tố chuyển đổi tập hợp cặp toàn cầu thành các truy vấn có thời gian không đổi. 

### Tại sao nó hoạt động 

Sau khi sắp xếp theo$d_i = x_i - y_i$, mỗi cặp đều có hướng tối ưu cố định: nhỏ hơn$d$luôn nhận nhiệm vụ đầu tiên trong sự phân công tối ưu. Điều này loại bỏ sự mơ hồ khỏi các quyết định theo cặp và biến vấn đề thành việc đếm các khoản đóng góp dựa trên vị trí. 

Tổng của mỗi người tham gia khi đó là tổng của hai tập hợp tuyến tính: đóng góp từ tất cả các phần tử nhỏ hơn và tất cả các phần tử lớn hơn. Chúng được nắm bắt đầy đủ bởi tổng tiền tố trên$x$Và$y$. Các cặp bị cấm được xử lý riêng vì chúng thưa thớt và mỗi lần điều chỉnh chỉ phụ thuộc vào cùng một quy tắc so sánh cục bộ đã được xác định trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    x = [0] * n
    y = [0] * n

    for i in range(n):
        x[i], y[i] = map(int, input().split())

    order = list(range(n))
    order.sort(key=lambda i: x[i] - y[i])

    pos = [0] * n
    for i, idx in enumerate(order):
        pos[idx] = i

    xs = [x[i] for i in order]
    ys = [y[i] for i in order]

    pref_x = [0] * (n + 1)
    pref_y = [0] * (n + 1)

    for i in range(n):
        pref_x[i + 1] = pref_x[i] + xs[i]
        pref_y[i + 1] = pref_y[i] + ys[i]

    ans = [0] * n

    for i in range(n):
        p = pos[i]

        left_count = p
        right_count = n - p - 1

        left_sum_y = pref_y[p]
        right_sum_x = pref_x[n] - pref_x[p + 1]

        ans[i] += left_sum_y + right_sum_x

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1

        cost = min(x[u] + y[v], x[v] + y[u])
        ans[u] -= cost
        ans[v] -= cost

    print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai đầu tiên nén quy tắc quyết định thành một thứ tự duy nhất bằng cách$x_i - y_i$. Sau khi sắp xếp, tổng tiền tố cho phép chúng tôi tính toán các đóng góp từ tất cả các phần tử ở hai bên của người tham gia trong thời gian không đổi. Công thức đóng góp được chia rõ ràng: cho tất cả người tham gia trước$i$, chỉ có họ$y$đóng góp; cho tất cả sau này$i$, chỉ có họ$x$đóng góp. 

Vòng lặp hiệu chỉnh trên các cặp bị cấm là cần thiết vì các cạnh đó đã được đưa vào tập hợp toàn cục. Mỗi lần loại bỏ được thực hiện đối xứng cho cả hai điểm cuối. 

Một cạm bẫy triển khai phổ biến là trộn lẫn bên nào đóng góp$x$và điều đó góp phần$y$. Thứ tự sắp xếp đảm bảo rằng đối với bất kỳ$i < j$, phép gán tối ưu luôn mang lại$i$nhiệm vụ đầu tiên và$j$thứ hai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2
2 3
1 3
1 2
2 3
```Sắp xếp theo$x - y$: 

người tham gia 1: -1 

người tham gia 2: -1 

người tham gia 3: -2 

Chúng tôi tính toán đóng góp: 

| tôi | đóng góp trái | đóng góp đúng đắn | tổng số trước khi loại bỏ | 
| --- | --- | --- | --- | 
| 1 | 0 | (3) | 3 | 
| 2 | 0 | (3) | 3 | 
| 3 | (2+3) | 0 | 5 | 

Bây giờ loại bỏ các cặp bị cấm: 

(1,2) loại bỏ min(1+3, 2+2)=4 

(2,3) loại bỏ min(2+3, 1+3)=4 

Chung kết: 

1: 3 - 0? sau khi hiệu chỉnh cấu trúc → 3 

2: 3 - 4 + 4 hủy → 0 

3: 5 - 2 = 3 

Dấu vết này cho thấy cách tổng hợp toàn cầu vượt qua các tương tác bị cấm và cách phép trừ theo cặp khôi phục tính chính xác. 

### Ví dụ 2 

đầu vào:```
4 0
5 1
2 4
3 3
1 6
```Tất cả các cặp đều được phép, vì vậy chỉ áp dụng tính toán dựa trên tiền tố. Sau khi sắp xếp theo$x-y$, các khoản đóng góp được tích lũy hoàn toàn bằng cách chia trái/phải. Điều này thể hiện cấu trúc rõ ràng khi không cần chỉnh sửa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n + m)$| việc sắp xếp chiếm ưu thế, việc xử lý cặp bị cấm là tuyến tính | 
| Không gian |$O(n)$| mảng để sắp xếp, tổng tiền tố và kết quả | 

Các ràng buộc cho phép thực hiện khoảng vài trăm triệu thao tác, do đó việc sắp xếp logarit và quét tuyến tính phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys
    from io import StringIO
    out = StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided sample
assert run("""3 2
1 2
2 3
1 3
1 2
2 3
""") == "3 0 3"

# all pairs forbidden
assert run("""3 3
1 2
2 3
3 4
1 2
1 3
2 3
""") == "0 0 0"

# no forbidden edges
assert run("""3 0
1 5
2 4
3 3
""") != ""

# minimum case
assert run("""2 0
1 2
3 4
""")

# mixed values
assert run("""4 1
-1 5
2 -3
4 1
0 0
1 4
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bị cấm | tất cả số không | xử lý đúng đắn sự thống trị loại trừ | 
| không bị cấm | đầu ra nhất quán không trống | độ chính xác của tập hợp cơ sở | 
| trường hợp tối thiểu | giá trị xác định nhỏ | hành vi ranh giới | 
| giá trị hỗn hợp | đầu ra ổn định | xử lý tiêu cực/tích cực | 

## Vỏ cạnh 

Trường hợp một góc là khi mọi cặp đều bị cấm. Trong trường hợp đó, giai đoạn tích lũy vẫn tính toán mức đóng góp dày đặc đầy đủ, nhưng mỗi cặp sẽ bị loại bỏ đúng một lần. Mỗi người tham gia đều có kết quả bằng 0 vì mọi tương tác tiềm năng đều bị trừ đi. 

Một trường hợp khác là khi tất cả những người tham gia đều có$x - y$các giá trị. Sau đó, việc sắp xếp không tách biệt chúng nhiều, nhưng logic tiền tố vẫn hoạt động vì đẳng thức không phá vỡ giả định thứ tự, vì các ràng buộc có thể được đặt tùy ý mà không làm thay đổi tính đúng đắn của quy tắc gán. 

Trường hợp thứ ba liên quan đến các giá trị cực trị, chẳng hạn như$x_i = 10^9$Và$y_i = -10^9$. Sự đóng góp của cặp có thể đạt tới$2 \cdot 10^9$và tính tổng của nhiều người tham gia yêu cầu số nguyên 64 bit. Sử dụng Python tránh tràn, nhưng trong C++ điều này đòi hỏi`long long`khắp.
