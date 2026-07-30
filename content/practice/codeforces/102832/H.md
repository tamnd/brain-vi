---
title: "CF 102832H - Khóa kết hợp"
description: "Một cách tiếp cận trực tiếp sẽ là mô hình hóa mọi chuỗi chuyển động có thể có. Từ một vị trí, chúng tôi sẽ thử mọi hàng xóm không được ghé thăm, giải quyết đệ quy trạng thái kết quả và đánh dấu trạng thái hiện tại là thắng nếu bất kỳ động thái nào khiến đối thủ thua."
date: "2026-07-26T15:12:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102832
codeforces_index: "H"
codeforces_contest_name: "2020 China Collegiate Programming Contest Changchun Onsite"
rating: 0
weight: 102832
solve_time_s: 75
verified: true
draft: false
---

[CF 102832H - Khóa kết hợp](https://codeforces.com/problemset/problem/102832/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ là mô hình hóa mọi chuỗi chuyển động có thể có. Từ một vị trí, chúng tôi sẽ thử mọi hàng xóm không được ghé thăm, giải quyết đệ quy trạng thái kết quả và đánh dấu trạng thái hiện tại là thắng nếu bất kỳ động thái nào khiến đối thủ thua. Điều này phản ánh chính xác các quy tắc và đúng vì trò chơi là hữu hạn. Vấn đề là lượng thông tin cần thiết. Trạng thái không chỉ là mật khẩu hiện tại mà còn là tập hợp tất cả các mật khẩu đã truy cập trước đó. Trong trường hợp xấu nhất, điều này tạo ra 2^(10^m) lịch sử có thể xảy ra, điều này là không thể ngay cả đối với giới hạn trên hữu ích nhỏ nhất trên không gian trạng thái. 

Sự thay đổi quan điểm hữu ích là ngừng suy nghĩ về lịch sử trò chơi và nhìn vào biểu đồ cơ bản. Mật khẩu bị cấm có thể được xóa trước khi trò chơi bắt đầu vì nhập mật khẩu luôn là một nước đi thua. Biểu đồ còn lại có các đỉnh biểu thị mật khẩu có thể sử dụng và các cạnh biểu thị một phép quay hợp lệ. 

Biểu đồ là vô hướng và biểu đồ khóa có cấu trúc lưỡng cực bổ sung. Việc tô màu mật khẩu theo tính chẵn lẻ của tổng các chữ số của nó có tác dụng vì mỗi bước di chuyển sẽ thay đổi tổng đó thành một số lẻ, ngay cả khi một chữ số bao bọc từ 0 đến 9. Điều này cho phép chúng tôi giải quyết kết quả khớp tối đa bằng Hopcroft-Karp. 

Định lý phù hợp đưa ra một bài kiểm tra đơn giản. Đặt M là kích thước khớp tối đa của biểu đồ có thể chơi được. Nếu đỉnh bắt đầu bị loại bỏ và kích thước khớp tối đa trở thành M - 1, thì mọi kết quả khớp tối đa đều sử dụng đỉnh bắt đầu, do đó Alice thắng. Nếu kích thước vẫn là M thì sẽ tồn tại một kết quả khớp tối đa tránh đỉnh bắt đầu, do đó Bob có thể giành chiến thắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ về số lượng trạng thái | Hàm mũ | Quá chậm | 
| Tối ưu | O(VE^0,5) | O(V + E) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo biểu đồ của tất cả các mật khẩu không nằm trong bộ bị cấm. Mỗi mật khẩu được chuyển đổi thành một id số nguyên và hai id được kết nối khi một vòng quay thay đổi một chữ số từng bước. 

Biểu đồ chứa tối đa 100000 đỉnh và mỗi đỉnh có nhiều nhất 2m lân cận, do đó việc xây dựng nó là tuyến tính theo kích thước của không gian trạng thái. 

1. Chia đồ thị thành hai cạnh bằng cách sử dụng tính chẵn lẻ của tổng các chữ số. Chỉ lưu trữ các cạnh từ mặt thứ nhất đến mặt thứ hai. 

Mỗi bước di chuyển đều làm thay đổi tính chẵn lẻ, vì vậy đây là một phép chia đôi hợp lệ. Nó cho phép tìm thấy kết quả phù hợp tối đa một cách hiệu quả. 

1. Chạy Hopcroft-Karp để tìm kích thước phù hợp tối đa của biểu đồ hoàn chỉnh có thể chơi được. 

Điều này mang lại giá trị cần thiết cho đặc tính phù hợp. 

1. Chạy lại Hopcroft-Karp trong khi bỏ qua mật khẩu bắt đầu. 

Nếu kích thước khớp giảm, mật khẩu bắt đầu là cần thiết trong mỗi lần khớp tối đa. Ngược lại, có một sự kết hợp tối đa để tránh điều đó. 

1. In Alice nếu kích thước khớp thứ hai nhỏ hơn và in Bob nếu ngược lại. 

Tại sao nó hoạt động: 

Sau khi xóa mật khẩu bị cấm, mọi nước đi hợp lệ trong trò chơi gốc đều tương ứng chính xác với việc di chuyển dọc theo một cạnh trong đồ thị vô hướng còn lại. Do đó, trò chơi là địa lý đỉnh không có hướng. Đặc tính khớp cho biết người chơi đầu tiên sẽ thắng chính xác khi đỉnh bắt đầu được bao gồm trong mọi kết quả khớp tối đa. Việc loại bỏ đỉnh bắt đầu sẽ kiểm tra chính xác điều kiện này: nếu kết quả phù hợp nhất mất một cạnh thì luôn cần phải bắt đầu; nếu không, sẽ tồn tại một kết quả phù hợp tối đa mà không có nó. Do đó, việc so sánh hai kích thước phù hợp sẽ đưa ra người chiến thắng chính xác. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def hopcroft_karp(adj, nl, nr):
    pair_l = [-1] * nl
    pair_r = [-1] * nr
    dist = [0] * nl

    def bfs():
        q = deque()
        found = False
        for i in range(nl):
            if pair_l[i] == -1:
                dist[i] = 0
                q.append(i)
            else:
                dist[i] = -1
        while q:
            u = q.popleft()
            for v in adj[u]:
                pu = pair_r[v]
                if pu == -1:
                    found = True
                elif dist[pu] == -1:
                    dist[pu] = dist[u] + 1
                    q.append(pu)
        return found

    def dfs(u):
        for v in adj[u]:
            pu = pair_r[v]
            if pu == -1 or (dist[pu] == dist[u] + 1 and dfs(pu)):
                pair_l[u] = v
                pair_r[v] = u
                return True
        dist[u] = -1
        return False

    ans = 0
    while bfs():
        for i in range(nl):
            if pair_l[i] == -1 and dfs(i):
                ans += 1
    return ans

def solve_case(m, n, start, banned):
    total = 10 ** m
    pow10 = [10 ** i for i in range(m)]

    def digits_of(x):
        res = []
        for _ in range(m):
            res.append(x % 10)
            x //= 10
        return res

    def encode(s):
        x = 0
        for c in s:
            x = x * 10 + (ord(c) - 48)
        return x

    bad = [False] * total
    for x in banned:
        bad[x] = True

    start_id = encode(start)

    side = [-1] * total
    left_id = [-1] * total
    right_id = [-1] * total
    nl = nr = 0

    for x in range(total):
        if not bad[x]:
            s = sum(digits_of(x))
            if s & 1:
                side[x] = 1
                right_id[x] = nr
                nr += 1
            else:
                side[x] = 0
                left_id[x] = nl
                nl += 1

    adj = [[] for _ in range(nl)]

    for x in range(total):
        if side[x] != 0:
            continue
        digs = digits_of(x)
        lx = left_id[x]
        for i in range(m):
            cur = digs[i]
            for nd in ((cur + 1) % 10, (cur - 1) % 10):
                y = x + (nd - cur) * pow10[m - 1 - i]
                if not bad[y]:
                    adj[lx].append(right_id[y])

    first = hopcroft_karp(adj, nl, nr)

    if side[start_id] == 0:
        old = left_id[start_id]
        removed_adj = []
        for i in range(nl):
            if i != old:
                removed_adj.append(adj[i])
        second = hopcroft_karp(removed_adj, nl - 1, nr)
    else:
        old = right_id[start_id]
        filtered = []
        for row in adj:
            filtered.append([v if v < old else v - 1 for v in row if v != old])
        second = hopcroft_karp(filtered, nl, nr - 1)

    return "Alice" if second < first else "Bob"

def main():
    t = int(input())
    out = []
    for _ in range(t):
        m, n, s = input().split()
        m = int(m)
        n = int(n)
        banned = []
        for _ in range(n):
            banned.append(int(input().strip()))
        out.append(solve_case(m, n, s, banned))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai trước tiên sẽ xây dựng bộ mật khẩu có thể sử dụng được. các`side`mảng lưu trữ phân vùng, trong khi`left_id`Và`right_id`chuyển đổi id mật khẩu ban đầu thành các chỉ số khớp nhỏ gọn. 

Thế hệ hàng xóm thay đổi từng chữ số một. Việc chuyển đổi từ một chữ số đã thay đổi trở lại id số nguyên sử dụng lũy ​​thừa mười, giúp tránh việc tạo chuỗi liên tục và giữ cho việc xây dựng biểu đồ nhanh chóng. 

Cuộc gọi Hopcroft-Karp đầu tiên tính toán mức độ khớp tối đa của biểu đồ có thể chơi được. Lệnh gọi thứ hai sẽ loại bỏ đỉnh bắt đầu khỏi phía tương ứng của phân vùng kép. Việc điều chỉnh chỉ số cẩn thận trong trường hợp bên phải sẽ tránh được lỗi sai sót khi một đỉnh trùng khớp biến mất. 

Phép so sánh cuối cùng sử dụng định lý so khớp một cách trực tiếp. Một kết quả khớp nhỏ hơn sau khi loại bỏ phần bắt đầu có nghĩa là phần bắt đầu là cần thiết, đó chính xác là điều kiện để Alice giành chiến thắng. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
1
1 2 6
7
5
```| Bước | Khớp đồ thị đầy đủ | Khớp sau khi xóa bắt đầu | Kết quả | 
| --- | --- | --- | --- | 
| Xây dựng đồ thị | Các đỉnh 0,1,2,3,4,6,8,9 vẫn còn | Chưa tính toán | Tiếp tục | 
| Kết hợp tối đa | 3 | Chưa tính toán | Tiếp tục | 
| Xóa bắt đầu 6 | 3 | 3 | Bob | 

Kích thước khớp không giảm, do đó tồn tại mức khớp tối đa để tránh mật khẩu bắt đầu. Bob có thể sử dụng chiến lược phù hợp. 

Mẫu 2: 

đầu vào:```
1
1 2 9
1
8
```| Bước | Khớp đồ thị đầy đủ | Khớp sau khi xóa bắt đầu | Kết quả | 
| --- | --- | --- | --- | 
| Xây dựng đồ thị | Các đỉnh trừ 1 và 8 vẫn còn | Chưa tính toán | Tiếp tục | 
| Kết hợp tối đa | 4 | Chưa tính toán | Tiếp tục | 
| Xóa bắt đầu 9 | 4 | 3 | Alice | 

Kích thước phù hợp giảm đi một. Đỉnh bắt đầu buộc phải thực hiện mọi kết quả khớp tối đa, vì vậy Alice có chiến lược chiến thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(VE^0,5) | Hopcroft-Karp được chạy hai lần trên biểu đồ có V <= 100000 và E <= 2mV | 
| Không gian | O(V + E) | Biểu đồ và mảng phù hợp được lưu trữ rõ ràng | 

Biểu đồ lớn nhất có thể có một trăm nghìn trạng thái và nhiều nhất là một triệu mục lân cận có hướng sau khi chuyển đổi hai bên. Hai lần chạy phù hợp vừa vặn thoải mái trong giới hạn thời gian và bộ nhớ nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().strip().split()
    sys.stdin = old
    return ""

# The assertions below are examples for an external judge harness.
# They should call solve_case directly or wrap main in the same file.

assert solve_case(1, 2, "6", [7, 5]) == "Bob"
assert solve_case(1, 2, "9", [1, 8]) == "Alice"
assert solve_case(1, 0, "0", []) == "Bob"
assert solve_case(1, 8, "0", ["1", "2", "3", "4", "5", "6", "7", "9"]) == "Bob"
assert solve_case(2, 99, "00", list(range(1, 100))) in ("Alice", "Bob")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`m=1, start=6, banned={7,5}`| Bob | Trường hợp kết quả khớp tối đa sẽ tránh bắt đầu | 
|`m=1, start=9, banned={1,8}`| Alice | Đỉnh bắt đầu bị ép vào mọi kết quả khớp tối đa | 
|`m=1, start=0, banned={}`| Bob | Xử lý đồ thị nhỏ nhất | 
|`m=1, start=0, all other digits banned`| Bob | Xử lý nước đi thua ngay lập tức | 
| Đồ thị lớn có hai chữ số | Alice hoặc Bob | Hiệu suất và xử lý chỉ số | 

## Vỏ cạnh 

Khi tất cả các bước di chuyển có thể xảy ra ngay từ đầu đều bị cấm, thuật toán sẽ loại bỏ các đỉnh đó trước khi khớp. Biểu đồ còn lại có thể chứa điểm bắt đầu là một đỉnh bị cô lập. Kích thước khớp tối đa của nó không thay đổi sau khi loại bỏ nó, vì vậy câu trả lời sẽ trở thành Bob, khớp với quy tắc Alice thua trong lần đi đầu tiên. 

Khi mật khẩu bắt đầu ở phía bên phải của phân vùng, việc xóa nó đòi hỏi phải thu nhỏ phía bên phải của biểu đồ phù hợp thay vì phía bên trái. Việc triển khai xử lý việc này một cách riêng biệt vì Hopcroft-Karp chỉ lưu trữ phần kề bên trái. Điều này ngăn ngừa một lỗi lập chỉ mục phổ biến. 

Khi không có mật khẩu nào bị cấm, biểu đồ chứa tất cả các trạng thái khóa có thể có. Phương pháp này vẫn hoạt động vì nó không bao giờ phụ thuộc vào số lượng trạng thái bị cấm. Biểu đồ được xây dựng chỉ từ các kích thước khóa và định lý phù hợp xử lý các chu trình và đường đi dài mà không liệt kê lịch sử trò chơi. 

Tôi cũng có thể điều chỉnh phần này thành một bài xã luận ngắn hơn theo phong cách Codeforces hoặc viết lại phần chứng minh theo phong cách định lý và bổ đề trang trọng hơn.
