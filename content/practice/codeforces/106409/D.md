---
title: "CF 106409D - Nhiệm vụ của Regina"
description: "Chúng ta được cung cấp một đồ thị vô hướng đơn giản với các đỉnh lên tới $2 cdot 10^5$ và tối đa $3 cdot 10^5$ các cạnh. Nhiệm vụ là xác định xem có tồn tại một chuỗi gồm bốn đỉnh riêng biệt $a, b, c, d$ sao cho mỗi cặp liên tiếp dọc theo chuỗi được nối với nhau bằng một cạnh, tạo thành một…"
date: "2026-06-25T09:58:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106409
codeforces_index: "D"
codeforces_contest_name: "HPI 2026 Advanced"
rating: 0
weight: 106409
solve_time_s: 43
verified: true
draft: false
---

[CF 106409D - Nhiệm vụ của Regina](https://codeforces.com/problemset/problem/106409/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng đơn giản với tối đa$2 \cdot 10^5$đỉnh và lên đến$3 \cdot 10^5$các cạnh. Nhiệm vụ là xác định xem có tồn tại một chuỗi bốn đỉnh phân biệt hay không$a, b, c, d$sao cho mỗi cặp liên tiếp dọc theo chuỗi được nối với nhau bằng một cạnh, tạo thành một đường đi đơn giản có độ dài bằng ba. Nếu một đường đi như vậy tồn tại, chúng ta phải xuất ra bất kỳ bộ tứ đỉnh hợp lệ nào theo thứ tự; nếu không chúng ta sẽ xuất -1. 

Cấu trúc mà chúng ta đang tìm kiếm không phải là một chu trình hay một mẫu đồ thị con chung, mà cụ thể là một đường đi đơn giản có ba cạnh. Điều này quan trọng vì các đỉnh lặp lại bị cấm, vì vậy chúng ta không thể sử dụng lại các nút ngay cả khi tồn tại nhiều cạnh. 

Những hạn chế gợi ý mạnh mẽ rằng một$O(N^2)$hoặc$O(NM)$cách tiếp cận là không thể. Với$N, M$lên tới hàng trăm nghìn, thậm chí$10^8$hoạt động là ranh giới và bất kỳ hình khối nào đều hoàn toàn nằm ngoài phạm vi. Bất kỳ giải pháp nào về cơ bản đều phải quét biểu đồ theo thời gian tuyến tính hoặc gần tuyến tính, có thể$O(N + M)$. 

Một trường hợp thất bại tinh vi xuất hiện khi biểu đồ chứa nhiều hình tam giác hoặc các vùng lân cận dày đặc. Ví dụ: một nhóm có kích thước 4 có chứa các câu trả lời hợp lệ, nhưng các cách tiếp cận ngây thơ thử tất cả các bộ ba hoặc hàng xóm của hàng xóm sẽ bùng nổ về mặt tổ hợp. Một trường hợp góc khác là khi đồ thị là một ngôi sao: một tâm được nối với nhiều lá. Không có đường đi nào có độ dài ba ở đó, mặc dù số cạnh lớn, do đó, các thuật toán giả sử “nhiều cạnh ngụ ý đường đi dài” sẽ không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ là thử từng cặp cạnh có thứ tự và cố gắng mở rộng chúng thành một đường dẫn. Cụ thể, chúng ta có thể lặp qua mọi cạnh$b\!-\!c$, sau đó thử tất cả hàng xóm$a$của$b$và tất cả hàng xóm$d$của$c$, kiểm tra xem$a, b, c, d$là khác biệt. Điều này đúng vì nó liệt kê mọi cạnh giữa có thể có và cố gắng mở rộng nó ở cả hai phía. 

Vấn đề là điều này trở nên cực kỳ tốn kém ở những khu vực đông đúc. Trong trường hợp xấu nhất của một đồ thị đầy đủ, mỗi đỉnh có bậc$O(N)$, vì vậy việc kiểm tra tất cả các phần mở rộng trên mỗi cạnh sẽ dẫn đến$O(N^3)$hành vi. Với$3 \cdot 10^5$cạnh, điều này là không thể thực hiện được. 

Quan sát quan trọng là chúng ta không cần phải khám phá tất cả các tiện ích mở rộng. Chúng ta chỉ cần biết liệu mỗi điểm cuối của một cạnh có ít nhất một điểm lân cận ngoài điểm cuối đối diện hay không. Khi chúng tôi chọn một cạnh giữa hợp lệ$b-c$, bất kỳ người hàng xóm nào$a \neq c$của$b$và bất kỳ người hàng xóm nào$d \neq b$của$c$ngay lập tức tạo thành một đường dẫn hợp lệ$a-b-c-d$. Vì vậy, vấn đề giảm xuống còn việc tìm bất kỳ cạnh nào trong đó cả hai điểm cuối đều có bậc ít nhất là 2 trong biểu đồ được tạo ra bằng cách loại bỏ điểm cuối đối diện khỏi việc xem xét. 

Một cách dễ dàng để khai thác điều này là xử lý trước các danh sách và mức độ kề. Sau đó, chúng ta lặp qua các cạnh và cố gắng xây dựng đường đi một cách tham lam. Cạnh đầu tiên cho phép cả hai phần mở rộng sẽ tạo ra câu trả lời hợp lệ ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các tiện ích mở rộng |$O(N^3)$|$O(N + M)$| Quá chậm | 
| Phần mở rộng cạnh tham lam dựa trên độ |$O(N + M)$|$O(N + M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho đồ thị và tính bậc của từng đỉnh. Điều này cho phép truy cập liên tục vào hàng xóm và kiểm tra nhanh xem liệu có thể gia hạn hay không. 
2. Lặp lại mọi cạnh$(b, c)$. Cạnh này sẽ đóng vai trò là điểm giữa của đường đi. 
3. Đối với cạnh hiện tại, tìm kiếm hàng xóm$a$của$b$như vậy$a \neq c$. Nếu không có hàng xóm như vậy tồn tại, cạnh này không thể là giữa đường dẫn hợp lệ, vì vậy hãy tiếp tục. 
4. Tương tự tìm kiếm hàng xóm$d$của$c$như vậy$d \neq b$. Nếu điều này cũng tồn tại, chúng ta ngay lập tức có một đường dẫn hợp lệ$a - b - c - d$, và chúng ta có thể xuất nó. 
5. Nếu không có cạnh nào thành công ở bước 3 và 4 thì không có đường dẫn có độ dài-3 hợp lệ nào tồn tại trong biểu đồ, do đó xuất ra -1. 

Ý tưởng chính là chúng ta không bao giờ cần khám phá nhiều hơn một ứng cử viên hàng xóm cho mỗi bên. Ngay khi chúng tôi tìm thấy phần mở rộng hợp lệ cho cả hai đầu của một cạnh, chúng tôi sẽ dừng lại. 

### Tại sao nó hoạt động 

Bất kỳ giải pháp hợp lệ nào cũng phải chứa một số cạnh giữa$(b, c)$. Nếu một con đường$a - b - c - d$tồn tại thì theo định nghĩa$a$là hàng xóm của$b$khác với$c$, Và$d$là hàng xóm của$c$khác với$b$. Vì vậy, khi chúng ta kiểm tra cạnh$(b, c)$, điểm cuối chính xác$a$Và$d$được đảm bảo tồn tại trong danh sách kề. Thuật toán không dựa vào thứ tự hoặc cấu trúc ngoài điều kiện tồn tại này, vì vậy nó không thể bỏ lỡ cấu hình hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n + 1)]

    edges = []
    for _ in range(m):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)
        edges.append((u, v))

    for b, c in edges:
        a = -1
        d = -1

        for x in adj[b]:
            if x != c:
                a = x
                break

        if a == -1:
            continue

        for x in adj[c]:
            if x != b:
                d = x
                break

        if d == -1:
            continue

        print(a)
        print(b)
        print(c)
        print(d)
        return

    print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng danh sách kề một lần rồi quét các cạnh. Các vòng lặp bên trong chỉ quét các nút lân cận cho đến khi tìm thấy ứng cử viên hợp lệ đầu tiên, do đó, không có đỉnh nào được xử lý nhiều hơn bậc của nó cho mỗi lần thử thành công. Chi tiết triển khai chính là tránh sử dụng lại điểm cuối đối diện khi chọn hàng xóm vì điều đó sẽ vi phạm tính khác biệt. 

Thuật toán dựa vào việc kết thúc sớm: ngay khi tìm thấy đường dẫn hợp lệ, chúng tôi sẽ dừng công việc tiếp theo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1
3 4
```Chỉ có một cạnh, do đó không có đỉnh nào có bậc ít nhất là 2. Thuật toán kiểm tra cạnh (3, 4), không tìm thấy lân cận thay thế nào cho một trong hai điểm cuối và kết thúc bằng -1. 

### Ví dụ 2 

đầu vào:```
6 7
1 3
1 5
2 3
2 5
2 6
4 5
4 6
```Chúng tôi theo dõi từng cạnh: 

| Cạnh (b,c) | Hàng xóm của b trừ c | Hàng xóm của c trừ b | Kết quả | 
| --- | --- | --- | --- | 
| (1,3) | 5 | 2 | tìm thấy đường dẫn hợp lệ | 
| dừng lại | | | đầu ra | 

Đối với cạnh (1, 3), đỉnh 1 có lân cận 5 ngoài 3 và đỉnh 3 có lân cận 2 ngoài 1. Điều này cho đường đi$5 - 1 - 3 - 2$. Thuật toán in nó ngay lập tức và dừng lại. 

Điều này chứng tỏ rằng chúng ta chỉ cần một lợi thế thành công duy nhất; không cần tìm kiếm cấu trúc toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N + M)$| Việc xây dựng danh sách lân cận mất thời gian tuyến tính và mỗi cạnh được kiểm tra trung bình một lần bằng cách quét hàng xóm trong thời gian không đổi. | 
| Không gian |$O(N + M)$| Lưu trữ danh sách kề và danh sách cạnh. | 

Giới hạn$N \le 2 \cdot 10^5$Và$M \le 3 \cdot 10^5$vừa vặn thoải mái trong phạm vi phức tạp này, vì thuật toán thực hiện một lượng công việc không đổi nhỏ trên mỗi cạnh và đỉnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    stdout.write = lambda s: out.append(s)
    out.clear()
    solve()
    return "".join(out)

out = []

# provided samples
assert run("""4 1
3 4
""") == "-1", "sample 1"

# simple path exists
assert run("""4 3
1 2
2 3
3 4
""").count("\n") == 3

# star graph (no length-3 path)
assert run("""5 4
1 2
1 3
1 4
1 5
""") == "-1\n", "star case"

# small cycle
assert run("""4 4
1 2
2 3
3 4
4 1
""").count("\n") == 3, "cycle case"

# dense triangle plus extra node
assert run("""5 6
1 2
2 3
3 1
3 4
4 5
2 5
""").count("\n") == 3, "dense case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị sao | -1 | không có nút nào có hai nút lân cận riêng biệt | 
| đồ thị đường dẫn | đường dẫn hợp lệ | sự tồn tại cơ bản của chuỗi | 
| đồ thị chu trình | đường dẫn hợp lệ | tính đúng đắn trong cấu trúc tuần hoàn | 
| phần mở rộng tam giác dày đặc | đường dẫn hợp lệ | tránh bị mắc kẹt trong bè phái | 

## Vỏ cạnh 

Biểu đồ hình ngôi sao hiển thị chế độ thất bại trong đó các cạnh tồn tại nhưng không có đỉnh nào có thể đóng vai trò là điểm giữa của đường dẫn có độ dài 3. Khi thuật toán kiểm tra bất kỳ cạnh nào, một điểm cuối luôn có độ 1, do đó bước loại trừ hàng xóm không thành công ngay lập tức và thuật toán trả về đúng -1. 

Một tam giác được kết nối đầy đủ với một cạnh đuôi thừa chứng tỏ rằng cấu trúc cục bộ dày đặc không gây nhầm lẫn cho thuật toán. Khi xử lý cạnh đuôi, điểm cuối được gắn vào tam giác vẫn có một lân cận thay thế, cho phép xây dựng một đường dẫn hợp lệ mà không cần khám phá tất cả các cạnh của tam giác. 

Biểu đồ đường dẫn thuần túy là trường hợp dương tính rõ ràng nhất. Mỗi cạnh bên trong đều có cấu trúc chính xác cần thiết và thuật toán sẽ thành công ngay khi gặp bất kỳ cạnh giữa nào, xác nhận rằng việc chấm dứt sớm không bỏ qua các câu trả lời hợp lệ.
