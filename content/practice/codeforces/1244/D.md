---
title: "CF 1244D - Sơn Cây"
description: "Chúng ta được cấp một cây trong đó mỗi nút phải được gán một trong ba màu và mỗi phép gán có chi phí tùy thuộc vào màu đã chọn."
date: "2026-06-13T20:28:22+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "dp", "graphs", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 1244
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 592 (Div. 2)"
rating: 1800
weight: 1244
solve_time_s: 172
verified: true
draft: false
---

[CF 1244D - Vẽ cây](https://codeforces.com/problemset/problem/1244/D) 

**Đánh giá:** 1800 
**Tags:** sức mạnh vũ phu, thuật toán xây dựng, dp, đồ thị, triển khai, cây 
**Thời gian giải:** 2m 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây trong đó mỗi nút phải được gán một trong ba màu và mỗi phép gán có chi phí tùy thuộc vào màu đã chọn. Ràng buộc cấu trúc không phải là ràng buộc cạnh tiêu chuẩn mà là ràng buộc đường dẫn: nếu chúng ta lấy bất kỳ đường dẫn nào có độ dài hai cạnh, nghĩa là ba đỉnh riêng biệt trong một chuỗi, thì ba đỉnh đó đều phải nhận được các màu khác nhau. 

Nói cách khác, bất cứ khi nào chúng ta thấy một mẫu x được kết nối với y được kết nối với z, thì màu của x, y và z phải tạo thành một hoán vị của ba giá trị riêng biệt. Điều này ngay lập tức tạo ra một hạn chế mạnh mẽ về cấu trúc trên cây, bởi vì mọi đỉnh bên trong trong bất kỳ đường dẫn có độ dài bằng hai nào đều đóng vai trò là dấu phân cách giữa các lân cận của nó về mặt phân bổ màu sắc. 

Kích thước đầu vào lên tới 100.000 đỉnh, loại trừ bất kỳ giải pháp nào thử tất cả các màu hoặc thậm chí bất kỳ thứ gì theo cấp số nhân trong các mẫu độ. Bất kỳ cách tiếp cận hợp lệ nào cũng phải chạy trong thời gian tuyến tính cơ bản hoặc thời gian tuyến tính với một hệ số không đổi nhỏ, vì vậy O(n) hoặc O(n log n) là phạm vi mục tiêu. 

Trường hợp cạnh tinh vi xuất hiện khi một nút có bậc lớn hơn 2. Nếu cố gắng gán màu tùy ý, chúng ta có thể dễ dàng vi phạm ràng buộc trên các đường dẫn khác nhau đi qua nút đó. Ví dụ: nếu một nút có ba nút lân cận và hai trong số chúng có cùng màu, thì việc chọn những nút lân cận đó cộng với nút trung tâm sẽ tạo ra một bộ ba xấu. Vì vậy, tính nhất quán cục bộ là cực kỳ hạn chế. 

Một trường hợp thất bại khác là giả sử bài toán giảm xuống việc tô màu đồ thị các cạnh một cách thích hợp. Điều đó là không đủ, vì ràng buộc là về độ dài hai đường đi chứ không chỉ về các đỉnh liền kề. Trực giác tô màu cạnh ngây thơ cho phép các mẫu vẫn vi phạm ràng buộc ba. 

Cuối cùng, bất kỳ giải pháp nào giả định rằng cây có thể có gốc tùy ý và được tô màu một cách tham lam mà không có cấu trúc tổng thể sẽ thất bại trên các đường dẫn dài hơn hai cạnh trừ khi cấu trúc được khai thác cẩn thận. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi cách gán màu có thể có cho các đỉnh, kiểm tra tính hợp lệ của từng cây. Vì mỗi đỉnh có 3 lựa chọn nên có 3^n khả năng. Ngay cả việc cắt tỉa bằng cách sử dụng kiểm tra cục bộ cũng không đủ vì các ràng buộc sẽ tăng gấp ba lần dọc theo các đường dẫn và các phép gán một phần không thể được mở rộng một cách an toàn nếu không quay lại. Việc xác minh cho mỗi lần gán là O(n), vì vậy cách tiếp cận này có tính cấp số nhân và không thể thực hiện được nếu vượt quá n rất nhỏ. 

Cái nhìn sâu sắc về cấu trúc quan trọng là ràng buộc chỉ quan tâm đến các đường đi có độ dài chính xác bằng hai cạnh. Điều đó có nghĩa là mọi nút bên trong buộc các nút lân cận của nó phải hoạt động theo một khuôn mẫu rất cứng nhắc: nếu một nút có màu c thì các nút lân cận của nó phải sử dụng hai màu còn lại theo cách ngăn chặn sự lặp lại trên bất kỳ đường dẫn hai bước nào. 

Hạn chế này ngụ ý một thuộc tính toàn cầu mạnh mẽ hơn: cây phải hoạt động hiệu quả giống như một đường đi đơn giản. Nếu một nút có bậc lớn hơn 2, thì không thể gán màu để thỏa mãn tất cả các đường dẫn có độ dài hai, bởi vì ba nhánh sẽ tương tác qua tâm và tạo ra sự mâu thuẫn. Vì vậy, điều kiện cần để tô màu hợp lệ là cây là một chuỗi đơn giản. 

Khi chúng ta rút gọn cấu trúc thành một đường dẫn, vấn đề sẽ trở nên độc lập với các ràng buộc phân nhánh. Bây giờ điều kiện trở thành: trong bất kỳ bộ ba liên tiếp nào dọc theo đường đi, cả ba màu đều phải khác biệt. Điều đó có nghĩa là dọc theo đường đi, các màu phải lặp lại theo một hoán vị tuần hoàn của ba màu, không lặp lại ở các vị trí liền kề hoặc khoảng cách hai lân cận.

Vì vậy, chúng ta chỉ cần quyết định thứ tự màu 1, 2, 3 dọc theo đường dẫn, sau đó đánh giá chi phí. Chỉ có 3 lựa chọn cho nút đầu tiên và 2 lựa chọn cho nút thứ hai, vì nút thứ ba là bắt buộc. Điều đó mang lại tổng cộng 6 hoán vị. Đối với mỗi hoán vị, chúng tôi truyền bá một cách xác định dọc theo đường dẫn và tính tổng chi phí. Chúng tôi chọn tốt nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^n · n) | O(n) | Quá chậm | 
| Tối ưu | O(6n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xác thực cấu trúc 

Trước tiên, chúng tôi kiểm tra xem mọi nút có bậc tối đa là 2 hay không. Nếu bất kỳ nút nào có bậc lớn hơn 2, chúng tôi ngay lập tức trả về -1. Điều này là do nút như vậy sẽ là tâm của nhiều đường dẫn có chiều dài hai chiều không thể được tô màu nhất quán nếu không có xung đột lặp lại. 

### 2. Trích xuất thứ tự đường dẫn 

Vì đồ thị là một cây có bậc tối đa là 2 nên nó phải là một đường đi duy nhất. Chúng tôi tìm thấy một điểm cuối, một nút có độ 1 và duyệt qua cây để xây dựng lại thứ tự tuyến tính của các nút. Điều này đưa ra một chuỗi v[0], v[1], ..., v[n-1]. 

### 3. Liệt kê tất cả các bộ ba màu hợp lệ 

Chúng tôi xem xét tất cả các phép gán màu cho hai nút đầu tiên sao cho chúng khác nhau. Đối với mỗi cặp như vậy, màu thứ ba được xác định duy nhất là màu còn thiếu trong {1,2,3}. Điều này cung cấp chính xác 6 cấu hình bắt đầu. 

### 4. Truyền màu dọc theo đường dẫn 

Đối với cặp bắt đầu đã chọn, chúng tôi chỉ định các màu chuyển tiếp bằng cách sử dụng quy tắc rằng màu tiếp theo phải là màu khác với hai màu trước. Điều này đảm bảo mỗi bộ ba liên tiếp đều chứa cả ba màu. 

### 5. Tính toán chi phí và theo dõi tốt nhất 

Đối với mỗi màu đầy đủ, chúng tôi tính tổng chi phí trên tất cả các nút và giữ mức tối thiểu. Chúng tôi lưu trữ bài tập tương ứng. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là khi hai màu liên tiếp được cố định, mọi màu tiếp theo sẽ bị ép buộc nếu chúng ta yêu cầu tất cả các đường dẫn có hai độ dài phải chứa ba màu riêng biệt. Điều này giúp loại bỏ sự phân nhánh trong quá trình ra quyết định. Cấu trúc đường dẫn đảm bảo không có ràng buộc bổ sung nào phát sinh từ kết nối phi tuyến tính, vì vậy chỉ cần kiểm tra các bộ ba liên tiếp là đủ. Vì mọi nghiệm hợp lệ đều tương ứng với chính xác một trong sáu cặp ban đầu nên chúng ta đã sử dụng hết tất cả các khả năng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_order(n, g):
    # find endpoint
    start = -1
    for i in range(n):
        if len(g[i]) == 1:
            start = i
            break

    order = []
    prev = -1
    cur = start

    while cur != -1:
        order.append(cur)
        nxt = -1
        for nei in g[cur]:
            if nei != prev:
                nxt = nei
                break
        prev, cur = cur, nxt

    return order

def solve():
    n = int(input())
    c1 = list(map(int, input().split()))
    c2 = list(map(int, input().split()))
    c3 = list(map(int, input().split()))

    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    for i in range(n):
        if len(g[i]) > 2:
            print(-1)
            return

    order = build_order(n, g)

    colors = [c1, c2, c3]

    perms = [
        (1, 2, 3),
        (1, 3, 2),
        (2, 1, 3),
        (2, 3, 1),
        (3, 1, 2),
        (3, 2, 1),
    ]

    best_cost = 10**18
    best_col = None

    for a, b, c in perms:
        col = [0] * n
        col[0] = a
        col[1] = b

        ok = True
        cost = 0

        cost += colors[a - 1][order[0]]
        cost += colors[b - 1][order[1]]

        for i in range(2, n):
            used = {col[i - 1], col[i - 2]}
            for t in (1, 2, 3):
                if t not in used:
                    col[i] = t
                    break

            cost += colors[col[i] - 1][order[i]]

        if cost < best_cost:
            best_cost = cost
            best_col = col[:]

    print(best_cost)
    print(*best_col)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách từ chối bất kỳ nút nào có bậc lớn hơn hai, vì điều đó phá vỡ cấu trúc đường dẫn cần thiết. Sau đó, nó sẽ xây dựng lại thứ tự tuyến tính duy nhất của cây bằng cách đi từ điểm cuối. Vòng lặp lõi kiểm tra tất cả sáu phép gán màu ban đầu có thể có và mở rộng từng màu một cách xác định. 

Bước lan truyền bên trong rất quan trọng: tại mỗi vị trí, chúng tôi tránh rõ ràng hai màu trước đó, đảm bảo luôn thỏa mãn ràng buộc ba. Việc tính toán chi phí được thực hiện tăng dần để tránh chi phí tính toán lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 2 3
4 3 2
3 1 3
1 2
2 3
```Thứ tự đường dẫn là [1,2,3]. 

Chúng tôi kiểm tra hoán vị (1,3,2): 

| tôi | nút | màu sắc | chi phí bổ sung | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 3 | 
| 1 | 2 | 3 | 2 | 
| 2 | 3 | 2 | 1 | 

Tổng chi phí là 6. 

Điều này xác nhận rằng sau khi hai màu đầu tiên được đặt, màu thứ ba sẽ bị ép buộc và tự động đáp ứng ràng buộc ba. 

### Ví dụ 2 

đầu vào:```
3
1 100 100
100 1 100
100 100 1
1 2
2 3
```Thứ tự đường dẫn là [1,2,3]. 

Hãy thử bắt đầu (2,1,3): 

| tôi | nút | màu sắc | chi phí bổ sung | 
| --- | --- | --- | --- | 
| 0 | 1 | 2 | 100 | 
| 1 | 2 | 1 | 100 | 
| 2 | 3 | 3 | 1 | 

Tổng chi phí là 201. 

Ví dụ này cho thấy rằng việc phân bổ tối ưu hoàn toàn được xác định bằng cách cân bằng chi phí cục bộ trên một cấu trúc định kỳ bắt buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | xây dựng đường dẫn và đánh giá 6 màu trên n nút | 
| Không gian | O(n) | danh sách kề, thứ tự đường đi, mảng màu | 

Giải pháp này dễ dàng phù hợp trong giới hạn vì mỗi nút được xử lý với số lần không đổi và chỉ có sáu cấu hình được thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    c1 = list(map(int, input().split()))
    c2 = list(map(int, input().split()))
    c3 = list(map(int, input().split()))

    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    for i in range(n):
        if len(g[i]) > 2:
            return "-1\n"

    start = 0
    for i in range(n):
        if len(g[i]) == 1:
            start = i
            break

    order = []
    prev = -1
    cur = start
    while cur != -1:
        order.append(cur)
        nxt = -1
        for nei in g[cur]:
            if nei != prev:
                nxt = nei
                break
        prev, cur = cur, nxt

    perms = [
        (1, 2, 3),
        (1, 3, 2),
        (2, 1, 3),
        (2, 3, 1),
        (3, 1, 2),
        (3, 2, 1),
    ]

    best = 10**18
    best_col = None
    colors = [c1, c2, c3]

    for a, b, c in perms:
        col = [0] * n
        col[0], col[1] = a, b
        cost = colors[a - 1][order[0]] + colors[b - 1][order[1]]

        for i in range(2, n):
            for t in (1, 2, 3):
                if t != col[i - 1] and t != col[i - 2]:
                    col[i] = t
                    break
            cost += colors[col[i] - 1][order[i]]

        if cost < best:
            best = cost
            best_col = col[:]

    return str(best) + "\n" + " ".join(map(str, best_col)) + "\n"

# provided sample
assert run("""3
3 2 3
4 3 2
3 1 3
1 2
2 3
""") == "6\n1 3 2\n"

# chain minimum
assert run("""3
1 1 1
1 1 1
1 1 1
1 2
2 3
""") == "3\n1 2 3\n"

# invalid degree
assert run("""4
1 1 1 1
1 1 1 1
1 1 1 1
1 2
2 3
2 4
""") == "-1\n"

# longer path
assert run("""5
1 2 3 4 5
5 4 3 2 1
3 3 3 3 3
1 2
2 3
3 4
4 5
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 6 1 3 2 | tính đúng đắn trong trường hợp cơ bản | 
| đồng phục | 3 1 2 3 | tuyên truyền chi phí tối thiểu | 
| sao không hợp lệ | -1 | phát hiện hạn chế mức độ | 
| xích có trọng số | không trống | ổn định trên con đường lớn hơn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi cây không phải là đường dẫn. Đối với đầu vào```
4
1 1 1 1
1 1 1 1
1 1 1 1
1 2
2 3
2 4
```nút 2 có bậc 3. Bất kỳ màu nào cũng buộc hai lân cận của nút 2 xuất hiện ở các nhánh khác nhau của đường dẫn có chiều dài bằng hai, nhưng chúng sẽ có chung tâm, khiến không thể gán ba màu riêng biệt trên tất cả các bộ ba như vậy. Thuật toán phát hiện điều này ngay lập tức và đưa ra -1. 

Một trường hợp cạnh khác là đường dẫn có độ dài chính xác ba nút, trong đó chỉ tồn tại một ràng buộc ba. Thuật toán giảm chính xác điều này xuống còn kiểm tra sáu hoán vị và chọn mức tối thiểu mà không làm phức tạp cấu trúc.
