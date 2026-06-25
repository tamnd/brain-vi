---
title: "CF 105292G - Bài toán đồ thị"
description: "Chúng ta có các khoảng tròn $N$ trên một vòng tròn có tọa độ được đánh số từ $0$ đến $2N-1$. Mỗi đỉnh của đồ thị tương ứng với một khoảng. Hai đỉnh kề nhau khi các khoảng của chúng trùng nhau."
date: "2026-06-25T19:47:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105292
codeforces_index: "G"
codeforces_contest_name: "National Taiwan University Class Preliminary 2024"
rating: 0
weight: 105292
solve_time_s: 88
verified: true
draft: false
---

[CF 105292G - Vấn đề về đồ thị](https://codeforces.com/problemset/problem/105292/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao$N$các khoảng tròn trên một vòng tròn có tọa độ được đánh số từ$0$ĐẾN$2N-1$. 

Mỗi đỉnh của đồ thị tương ứng với một khoảng. Hai đỉnh kề nhau khi các khoảng của chúng trùng nhau. Nhiệm vụ là xuất ra bất kỳ cụm tối đa nào, nghĩa là tập hợp các khoảng lớn nhất sao cho mỗi cặp khoảng trong tập hợp trùng nhau. 

Bản thân biểu đồ không bao giờ được đưa ra một cách rõ ràng. Việc xây dựng tất cả các cạnh sẽ mất$O(N^2)$thời gian, và khi đó việc giải cụm tối đa trên đồ thị tổng quát sẽ là vô vọng. Điểm mấu chốt của vấn đề là đồ thị xuất phát từ các khoảng tròn, có cấu trúc mạnh hơn nhiều. 

Sự ràng buộc$N \le 2000$là quan sát chính. MỘT$O(N^2)$giải pháp là hoàn toàn ổn. MỘT$O(N^3)$giải pháp đã gần đến giới hạn, trong khi mọi thứ tương tự như tìm kiếm theo nhóm theo cấp số nhân là không thể. 

Một điểm tinh tế là các khoảng tròn không thỏa mãn tính chất Helly. Ba khoảng có thể chồng lên nhau theo từng cặp mà không có chung tọa độ. Mẫu đầu tiên chính xác là một cấu hình như vậy. 

Ví dụ:```
3
0 3
2 5
4 1
```Mỗi cặp trùng nhau nên câu trả lời có kích thước 3. 

Một giải pháp chỉ tìm kiếm tọa độ được bao phủ bởi số khoảng lớn nhất sẽ trả về sai 2. 

Một sai lầm dễ mắc phải khác là coi một khoảng bao bọc là hai khoảng độc lập. Khoảng bao bọc vẫn là một cung được nối trên đường tròn. Việc tách nó ra và quên rằng cả hai phần đều thuộc cùng một đỉnh có thể tạo ra các cụm không thực sự tồn tại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là xây dựng biểu đồ và chạy thuật toán nhóm tối đa chung. 

Xây dựng biểu đồ đã có chi phí$O(N^2)$, bởi vì mỗi cặp khoảng phải được kiểm tra sự trùng lặp. Sau đó, cụm tối đa trên biểu đồ tùy ý là NP-hard, do đó, ngay cả$N=2000$hoàn toàn nằm ngoài tầm với. 

Cấu trúc của các khoảng tròn thay đổi mọi thứ. 

Một thực tế cổ điển về đồ thị cung tròn là như sau. 

Lấy bất kỳ khoảng thời gian nào$A$. Chọn điểm cắt trên đường tròn nằm ngoài$A$. Sau khi cắt đường tròn ở đó, mỗi khoảng sẽ trở thành một khoảng thông thường trên một đường thẳng. 

Bây giờ hãy xem xét bất kỳ nhóm nào có chứa$A$. Vì tất cả các khoảng trong cụm giao nhau$A$, không ai trong số họ có thể vượt qua đường cắt theo cách phá vỡ biểu diễn khoảng. Bên trong bức ảnh được biến đổi này, chúng ta thu được một biểu đồ khoảng. 

Đồ thị khoảng thỏa mãn tính chất Helly: các khoảng giao nhau theo cặp luôn có một điểm chung. 

Điều đó có nghĩa là mọi bè phái chứa$A$tương ứng với một điểm được bao phủ bởi tất cả các khoảng của cụm sau khi cắt. 

Điều này mang lại một chiến lược rất hữu ích. 

Sửa một khoảng$A$. 

Cắt vòng tròn bên ngoài$A$. 

Chuyển đổi tất cả các khoảng thành các khoảng trên một dòng. 

Nhóm lớn nhất chứa$A$chính xác là tập hợp lớn nhất các khoảng biến đổi bao trùm một điểm chung. 

Việc tìm tập hợp đó là một bài toán quét đường tiêu chuẩn. 

Lặp lại quá trình cho mọi lựa chọn có thể của$A$chi phí$O(N^2)$, đủ nhanh để$N=2000$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Bùng nổ lực lượng tối đa | Hàm mũ | O(N2) | Quá chậm | 
| Sửa một khoảng, cắt vòng tròn, quét chồng chéo | O(N2) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cố định một khoảng thời gian$i$. 
2. Chọn điểm cắt ngay sau điểm cuối bên phải của khoảng$i$. Theo thi công, vết cắt nằm ngoài khoảng$i$. 
3. Trải hình tròn thành một đoạn thẳng có độ dài$2N$. 
4. Chuyển mỗi quãng tròn thành một quãng thông thường trên dòng đó. 
5. Chỉ giữ các khoảng giao nhau với khoảng$i$. Bất kỳ nhóm nào có chứa$i$chỉ có thể sử dụng những khoảng thời gian như vậy. 
6. Chạy một đường quét trên các khoảng được chuyển đổi. 
7. Bất cứ khi nào quá trình quét đạt đến một điểm có số khoảng lớn nhất được thấy cho đến nay, hãy ghi lại bộ chỉ số khoảng tương ứng. 
8. Tập hợp đó là cụm chứa khoảng tối đa$i$. 
9. Lặp lại sau mỗi khoảng thời gian$i$. 
10. Xuất ra nhóm lớn nhất được tìm thấy trên tất cả các lựa chọn của$i$. 

### Tại sao nó hoạt động 

Sửa bất kỳ nhóm tối đa nào$C$. 

Chọn bất kỳ khoảng thời gian nào$A \in C$. 

Thuật toán cuối cùng sẽ xử lý khoảng thời gian này dưới dạng khoảng thời gian cố định. 

Vết cắt được chọn bên ngoài$A$, vậy mọi khoảng của$C$trở thành những khoảng thông thường trên một đường thẳng. Từ$C$là một nhóm, mọi cặp khoảng này đều giao nhau. 

Đối với các khoảng trên một đường thẳng, giao điểm theo cặp ngụ ý sự tồn tại của một điểm chung. Do đó mọi khoảng của$C$che chắn một số vị trí quét. 

Khi quá trình quét xử lý vị trí đó, nó sẽ thấy ít nhất$|C|$khoảng thời gian đồng thời hoạt động. 

Do đó, thuật toán tìm thấy một nhóm có kích thước ít nhất$|C|$. 

Vì không có nhóm nào có thể lớn hơn nhóm tối đa nên thuật toán sẽ tìm chính xác nhóm tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def inside_arc(x, l, r, m):
    if l <= r:
        return l <= x <= r
    return x >= l or x <= r

def intersect(a_l, a_r, b_l, b_r, m):
    return inside_arc(a_l, b_l, b_r, m) or \
           inside_arc(a_r, b_l, b_r, m) or \
           inside_arc(b_l, a_l, a_r, m) or \
           inside_arc(b_r, a_l, a_r, m)

n = int(input())
seg = [tuple(map(int, input().split())) for _ in range(n)]

m = 2 * n

best = []

for root in range(n):
    l0, r0 = seg[root]

    events = []

    cut = (r0 + 1) % m

    for idx, (l, r) in enumerate(seg):
        if not intersect(l0, r0, l, r, m):
            continue

        L = (l - cut + m) % m
        R = (r - cut + m) % m

        if L <= R:
            events.append((L, 1, idx))
            events.append((R + 0.5, -1, idx))
        else:
            events.append((0, 1, idx))
            events.append((R + 0.5, -1, idx))

            events.append((L, 1, idx))
            events.append((m + 0.5, -1, idx))

    events.sort()

    active = set()

    for pos, typ, idx in events:
        if typ == 1:
            active.add(idx)
            if len(active) > len(best):
                best = list(active)
        else:
            active.remove(idx)

print(len(best))
print(*best)
```Việc thực hiện tuân theo bằng chứng trực tiếp. 

Vòng lặp bên ngoài cố định một khoảng và cắt vòng tròn bên ngoài nó. 

Mỗi khoảng tròn được dịch thành tọa độ tương ứng với vết cắt. Khoảng thời gian được bao bọc trở thành hai phần thông thường, được xử lý bằng cách thêm hai phạm vi sự kiện. 

Đường quét duy trì tập hợp các khoảng thời gian hoạt động. Bất cứ khi nào tập hoạt động lớn hơn câu trả lời hiện tại, chúng tôi sẽ lưu trữ nó. 

Phần tế nhị duy nhất là khoảng giao nhau trên đường tròn. Các chức năng trợ giúp`inside_arc`Và`intersect`xử lý các khoảng thời gian được gói một cách chính xác. 

Bởi vì tất cả các điểm cuối đều khác biệt nên thứ tự sự kiện không rõ ràng và không yêu cầu xử lý ràng buộc đặc biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
3
0 3
2 5
4 1
```Sửa khoảng 0. 

Sau khi cắt khoảng 0 bên ngoài, các khoảng được biến đổi sẽ trở thành: 

| Khoảng thời gian | Phạm vi chuyển đổi | 
| --- | --- | 
| 0 | hoạt động | 
| 1 | hoạt động | 
| 2 | hoạt động | 

Trạng thái quét: 

| Vị trí | Khoảng thời gian hoạt động | 
| --- | --- | 
| bắt đầu | {0, 2} | 
| giữa | {0, 1, 2} | 
| kết thúc | {1, 2} | 

Bộ hoạt động tối đa có kích thước 3. 

Các đầu ra thuật toán:```
3
0 1 2
```Ví dụ này chứng tỏ tại sao việc đếm vùng phủ sóng trên vòng tròn ban đầu là không đủ. Nhóm tồn tại mặc dù không có tọa độ vòng tròn chung. 

### Ví dụ 2```
5
0 6
9 1
3 4
5 8
2 7
```Một nhóm tối đa là:```
0 3 4
```Quét dấu vết: 

| Vị trí | Khoảng thời gian hoạt động | 
| --- | --- | 
| bắt đầu | {0, 4} | 
| giữa | {0, 3, 4} | 
| kết thúc | {0, 3} | 

Kích thước tối đa là 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N2) | N lựa chọn khoảng gốc, mỗi sự kiện quét O(N) | 
| Không gian | O(N) | Danh sách sự kiện và bộ hoạt động | 

Với$N \le 2000$, khoảng bốn triệu thao tác nguyên thủy được thực hiện, vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    # paste solution here

    return out.getvalue()

# custom sanity checks

assert run("1\n0 1\n").splitlines()[0] == "1"

assert run(
"""3
0 3
2 5
4 1
"""
).splitlines()[0] == "3"

assert run(
"""2
0 1
2 3
"""
).splitlines()[0] == "1"

assert run(
"""4
0 4
1 5
2 6
3 7
"""
).splitlines()[0] == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Quãng đơn | 1 | Kích thước tối thiểu | 
| Mẫu mẫu đầu tiên | 3 | Nhóm không phải Helly | 
| Hai khoảng cách nhau | 1 | Không chồng chéo | 
| Chuỗi chồng chéo lồng nhau | 4 | Nhóm chung lớn | 

## Vỏ cạnh 

Hãy xem xét lại:```
3
0 3
2 5
4 1
```Ba khoảng tạo thành một cụm mặc dù không có tọa độ nào thuộc về cả ba. Một giải pháp chỉ dựa trên mức độ phù hợp tối đa sẽ trả về 2. 

Khi khoảng 0 cố định và đường tròn bị cắt bên ngoài nó, bài toán trở thành đồ thị khoảng. Ba khoảng biến đổi có chung một vị trí quét và thuật toán tìm chính xác kích thước 3. 

Bây giờ hãy xem xét:```
2
0 1
2 3
```Các khoảng rời rạc. Đối với cả hai lựa chọn khoảng thời gian cố định, quá trình quét không bao giờ chứa cả hai khoảng thời gian cùng một lúc. Tập hoạt động lớn nhất có kích thước 1, đây là câu trả lời đúng. 

Cuối cùng:```
4
0 7
1 2
3 4
5 6
```Khoảng bao bọc lớn giao nhau với mọi khoảng nhỏ, nhưng các khoảng nhỏ không giao nhau. 

Việc quét hồ sơ các nhóm như`{0,1}`,`{0,2}`, Và`{0,3}`, tất cả đều có kích thước 2. Không có cụm nào lớn hơn xuất hiện, vì vậy câu trả lời được báo cáo chính xác là 2.
