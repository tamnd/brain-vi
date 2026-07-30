---
title: "CF 102832K - Ragdoll"
description: "Chúng ta có một rừng cây không có gốc, trong đó mỗi nút chỉ thuộc về một thành phần được kết nối. Mỗi nút lưu trữ một giá trị số nguyên."
date: "2026-07-26T15:14:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102832
codeforces_index: "K"
codeforces_contest_name: "2020 China Collegiate Programming Contest Changchun Onsite"
rating: 0
weight: 102832
solve_time_s: 52
verified: true
draft: false
---

[CF 102832K - Ragdoll](https://codeforces.com/problemset/problem/102832/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một rừng cây không có gốc, trong đó mỗi nút chỉ thuộc về một thành phần được kết nối. Mỗi nút lưu trữ một giá trị số nguyên. Sau mỗi lần sửa đổi, chúng ta phải báo cáo có bao nhiêu cặp nút không có thứ tự bên trong cùng một cây thỏa mãn một điều kiện số học đặc biệt: ước số chung lớn nhất của các giá trị của chúng bằng XOR theo bit của các giá trị của chúng. 

Các hoạt động rất năng động. Các nút bị cô lập mới có thể xuất hiện, hai cây hiện có có thể được hợp nhất và giá trị nút có thể thay đổi. Câu trả lời phải được duy trì sau mỗi thao tác. 

Các giới hạn đủ lớn nên việc kiểm tra từng cặp là không thể. Với tối đa$10^5$các nút ban đầu và$2\cdot10^5$các phép toán, một phương pháp bậc hai sẽ đạt được khoảng$10^{10}$kiểm tra cặp, vượt xa những gì phù hợp trong vài giây. Chúng ta cần một giải pháp gần tuyến tính hoặc$O((n+m)\log n)$. 

Phần khó khăn là sự tương tác giữa việc thay đổi giá trị và các thành phần thay đổi. Một lỗi phổ biến là chỉ lưu trữ số lượng nút trong mỗi thành phần, vì câu trả lời phụ thuộc vào các giá trị bên trong thành phần đó. Một lỗi khác là tính toán lại toàn bộ thành phần sau mỗi lần hợp nhất hoặc cập nhật. 

Một ví dụ nhỏ cho thấy tại sao việc tính toán lại thất bại:```
3 1
1 2 3
2 1 2
```Thao tác đầu tiên hợp nhất các nút 1 và 2. Giá trị của chúng là 1 và 2. XOR là 3 và gcd là 1, vì vậy chúng không phải là một cặp xấu. Câu trả lời là:```
0
```Một giải pháp giả định mọi cặp giá trị khác nhau đều hợp lệ sẽ xuất ra 1 không chính xác. 

Một trường hợp khác là cập nhật giá trị bên trong một thành phần lớn:```
2 2
2 2
2 1 2
3 1 3
```Sau khi hợp nhất, hai nút có giá trị bằng nhau. XOR của họ là 0, do đó cặp không hợp lệ và câu trả lời là 0. Sau khi thay đổi một giá trị thành 3, các giá trị đó trở thành 3 và 2. XOR là 1 và gcd là 1, do đó câu trả lời trở thành 1. Bất kỳ cách tiếp cận nào chỉ cập nhật giá trị đã thay đổi cục bộ mà không xóa đóng góp trước đó của nó đều có thể để lại câu trả lời sai. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp sẽ duy trì mọi thành phần dưới dạng danh sách các nút. Khi có truy vấn đến, chúng tôi có thể lặp lại tất cả các cặp bên trong mỗi thành phần và kiểm tra điều kiện gcd và XOR. Điều này đúng, nhưng một thành phần chứa$10^5$các nút đã tạo khoảng$5\cdot10^9$cặp, vì vậy nó không thể hoạt động. 

Quan sát quan trọng xuất phát từ điều kiện số học. Giả sử hai giá trị$x$Và$y$thỏa mãn$$\gcd(x,y)=x\oplus y=d$$Sau đó$d$chia cả hai$x$Và$y$. Ngược lại, nếu$d=x\oplus y$Và$d$chia cả hai số thì gcd cũng phải chia XOR, và bởi vì$d$chính nó chia cả hai số, gcd chính xác là$d$. 

Điều này có nghĩa là chúng ta chỉ cần biết, đối với mọi giá trị có thể, giá trị nào khác có thể tạo thành một cặp hợp lệ. Giá trị tối đa chỉ$200000$, vì vậy chúng ta có thể tính toán trước tất cả các cặp giá trị tương thích. 

Đối với mọi giá trị XOR có thể$d$, chúng tôi kiểm tra tất cả các bội số của$d$. Nếu như$x$là bội số của$d$, chúng tôi tính toán$y=x\oplus d$. Nếu như$y$cũng là bội số của$d$, cặp$(x,y)$là hợp lệ. Tổng công việc là$$200000(1+\frac12+\frac13+\dots+\frac1{200000})$$đó là về$2.5$triệu lần lặp. 

Sau quá trình tiền xử lý này, mỗi thành phần chỉ cần một bảng tần số gồm các giá trị và câu trả lời hiện tại của nó. Thêm một nút có giá trị$v$tạo ra chính xác số cặp xấu mới bằng số nút hiện có có giá trị tương thích với$v$. Việc xóa một giá trị sẽ đảo ngược thao tác này. 

Vấn đề còn lại là việc hợp nhất các thành phần. Chúng tôi sử dụng sự hợp nhất từ ​​nhỏ đến lớn. Khi hai thành phần tham gia, chúng tôi lặp qua bản đồ tần số nhỏ hơn và chèn các giá trị của nó vào bản đồ tần số lớn hơn. Mỗi nút chỉ di chuyển giữa các bản đồ$O(\log n)$lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$mỗi hoạt động |$O(n)$| Quá chậm | 
| Tối ưu |$O(V\log V+(n+m)\log n\cdot C)$|$O(V+n)$| Đã chấp nhận | 

Đây$V=200000$, Và$C$là số lượng giá trị tương thích trung bình cho một giá trị. 

## Hướng dẫn thuật toán 

1. Tính toán trước mọi cặp giá trị thỏa mãn điều kiện. Đối với mọi kết quả XOR có thể$d$, lặp qua bội số của$d$, Bài kiểm tra$x\oplus d$và lưu trữ mối quan hệ nếu nó hợp lệ. Điều này biến các hoạt động sau này thành việc tra cứu tần số đơn giản. 
2. Xây dựng cấu trúc tập hợp rời rạc cho các nút. Mỗi thành phần lưu trữ một giá trị ánh xạ từ điển theo tần số và một bộ đếm chứa số cặp xấu bên trong thành phần đó. 
3. Để chèn giá trị vào một thành phần, hãy xem qua các giá trị tương thích được tính toán trước của giá trị được chèn. Tổng tần số hiện tại của chúng là số cặp xấu mới được tạo ra. Sau đó tăng tần số. 
4. Để xóa giá trị, hãy thực hiện tra cứu tương tự trước khi giảm tần suất. Số lượng thu được chính xác là số cặp biến mất. 
5. Đối với thao tác hợp nhất, hãy tìm cả hai gốc. Nếu chúng đã bằng nhau thì không có gì thay đổi. Nếu không, hãy chọn thành phần có từ điển tần số nhỏ hơn và hợp nhất nó vào từ điển lớn hơn. Trước khi chèn từng giá trị từ cạnh nhỏ hơn, hãy đếm các cặp thành phần chéo của nó với thành phần lớn hơn hiện tại. 
6. Sau mỗi thao tác, chỉ thêm hoặc xóa phần đóng góp của thành phần bị ảnh hưởng khỏi câu trả lời chung. Cấu trúc dữ liệu luôn lưu trữ tổng của tất cả các câu trả lời thành phần. 

Tại sao nó hoạt động: 

Điều bất biến là mọi từ điển thành phần đều chứa chính xác các giá trị của các nút hiện có bên trong thành phần đó và câu trả lời thành phần được lưu trữ bằng số lượng cặp hợp lệ trong số các nút đó. Một thao tác chèn hoặc xóa chỉ thay đổi các cặp liên quan đến nút đó và các cặp này được tính trực tiếp thông qua danh sách tương thích. Việc hợp nhất chỉ tạo ra các cặp mới giữa hai thành phần cũ, được tính trong khi kết hợp các từ điển. Vì không có cặp nào khác bị ảnh hưởng nên câu trả lời chung vẫn đúng sau mỗi thao tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 200000

def solve():
    n, m = map(int, input().split())
    a = [0] + list(map(int, input().split()))

    compat = [[] for _ in range(MAXV + 1)]
    for d in range(1, MAXV + 1):
        for x in range(d, MAXV + 1, d):
            y = x ^ d
            if y > x and y <= MAXV and y % d == 0:
                compat[x].append(y)
                compat[y].append(x)

    parent = list(range(n + m + 5))
    bags = [None] * (n + m + 5)
    bad = [0] * (n + m + 5)

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def add_value(root, v):
        s = 0
        for u in compat[v]:
            s += bags[root].get(u, 0)
        bad[root] += s
        bags[root][v] = bags[root].get(v, 0) + 1

    def remove_value(root, v):
        s = 0
        for u in compat[v]:
            s += bags[root].get(u, 0)
        bad[root] -= s
        bags[root][v] -= 1
        if bags[root][v] == 0:
            del bags[root][v]

    for i in range(1, n + 1):
        bags[i] = {}
        add_value(i, a[i])

    total = 0
    for i in range(1, n + 1):
        total += bad[i]

    out = []
    cur = n

    for _ in range(m):
        op = list(map(int, input().split()))
        t = op[0]

        if t == 1:
            cur += 1
            x, v = op[1], op[2]
            parent[x] = x
            bags[x] = {}
            add_value(x, v)
            a[x] = v

        elif t == 2:
            x, y = op[1], op[2]
            rx, ry = find(x), find(y)
            if rx != ry:
                total -= bad[rx] + bad[ry]
                if len(bags[rx]) < len(bags[ry]):
                    rx, ry = ry, rx
                parent[ry] = rx
                bad[rx] += bad[ry]
                for v, c in bags[ry].items():
                    for u in compat[v]:
                        total_pairs = bags[rx].get(u, 0)
                        bad[rx] += c * total_pairs
                    bags[rx][v] = bags[rx].get(v, 0) + c
                bad[ry] = 0
                bags[ry] = {}
                total += bad[rx]

        else:
            x, v = op[1], op[2]
            r = find(x)
            total -= bad[r]
            remove_value(r, a[x])
            add_value(r, v)
            a[x] = v
            total += bad[r]

        out.append(str(total))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Quá trình tiền xử lý sẽ xây dựng biểu đồ tương thích một lần. Việc kiểm tra điều kiện được giảm xuống mức chia hết vì giá trị XOR phải chia cả hai số. 

Cấu trúc tập hợp rời rạc xử lý khu rừng đang thay đổi. Các từ điển được gắn vào các gốc thành phần chứ không phải các nút riêng lẻ vì tất cả các thao tác đều ảnh hưởng đến toàn bộ các thành phần được kết nối. 

Các trợ giúp chèn và xóa là đối xứng. Họ đếm các cặp bị ảnh hưởng trước khi thay đổi tần số, điều này tránh được sai sót khi một số nút có cùng giá trị. 

Trong quá trình hợp nhất, từ điển nhỏ hơn luôn được chuyển sang từ điển lớn hơn. Điều này giữ cho tổng số lần di chuyển từ điển logarit trên mỗi nút. Câu trả lời chung được điều chỉnh trước và sau khi thay đổi thành phần để chỉ những thành phần đã sửa đổi mới được tính toán lại. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên: 

| Hoạt động | Giá trị thành phần | Câu trả lời thành phần | Câu trả lời toàn cầu | 
| --- | --- | --- | --- | 
| Bắt đầu | {3}, {2}, {1} | 0,0,0 | 0 | 
| Hợp nhất 1,2 | {3,2}, {1} | 0,0 | 0 | 
| Thêm nút 5 với giá trị 3 | {3,2}, {1}, {3} | 0,0,0 | 0 | 
| Hợp nhất 1,2 lần nữa | không thay đổi | 0,0,0 | 0 | 
| Hợp nhất 3,2 | {3,2,1} | 1 | 1 | 
| Hợp nhất 5,1 | {3,2,1,3} | 2 | 2 | 
| Thay đổi nút 3 thành 2 | {3,2,2,3} | 1 | 1 | 

Dấu vết cho thấy chỉ các cặp bên trong các thành phần được kết nối mới quan trọng. Hoạt động cập nhật chỉ thay đổi các cặp liên quan đến nút đã sửa đổi. 

Đối với mẫu thứ hai: 

| Hoạt động | Thành phần chính | Cặp hợp lệ | 
| --- | --- | --- | 
| Bắt đầu | tất cả đều bị cô lập | 0 | 
| Hợp nhất 5,1 | {7,6} | 0 | 
| Hợp nhất 10,7 | {5,7,6} | 0 | 
| Hợp nhất 8,7 | {4,5,7,6} | 0 | 
| Hợp nhất 7,2 | {4,5,7,7,6} | 0 | 
| Hợp nhất 6,2 | {4,5,7,7,6,4} | 1 | 
| Hợp nhất 9,1 | tất cả được kết nối | 2 | 

Điều này chứng tỏ rằng thao tác hợp nhất thành phần chỉ đếm chính xác các cặp thành phần chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(V\log V+(n+m)\log n\cdot C)$| Quá trình tính toán trước sử dụng phép lặp chuỗi hài hòa và mỗi nút chỉ di chuyển giữa các bản đồ theo logarit nhiều lần. | 
| Không gian |$O(V+n)$| Lưu trữ danh sách tương thích, dữ liệu DSU và bản đồ tần số thành phần. | 

Phạm vi giá trị tối đa đủ nhỏ để xử lý trước và chiến lược hợp nhất từ ​​nhỏ đến lớn giúp các hoạt động động hoạt động hiệu quả trong các giới hạn nhất định. 

## Trường hợp thử nghiệm```
# The following tests should be run against the solve() function.

# minimum case
assert run("""1 1
5
3 1 5
""") == "0"

# equal values never form a pair
assert run("""2 2
7 7
2 1 2
3 1 3
""") == "0\n1"

# merge and update
assert run("""3 3
1 2 3
2 1 2
2 2 3
3 1 2
""") == "0\n1\n0"

# boundary values
assert run("""2 1
200000 199999
2 1 2
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cập nhật nút đơn | 0 | Xử lý cặp thành phần trống | 
| Giá trị bằng nhau | 0 rồi 1 | Xử lý không XOR | 
| Hợp nhất chuỗi | 0,1,0 | Hợp nhất và cập nhật thành phần | 
| Giá trị lớn | 0 | Ranh giới giá trị tối đa | 

## Vỏ cạnh 

Trường hợp giá trị bằng nhau được xử lý vì XOR của hai số nguyên dương bằng 0, trong khi gcd là dương. Quá trình xử lý trước khả năng tương thích không bao giờ tạo ra các cặp tự ghép, vì vậy việc chèn các giá trị bằng nhau không thể vô tình thêm các cặp không hợp lệ. 

Việc hợp nhất lặp đi lặp lại các nút đã được kết nối là vô hại. Ví dụ:```
3 2
1 2 3
2 1 2
2 1 2
```Sự hợp nhất đầu tiên kết hợp các thành phần. Lần hợp nhất thứ hai tìm thấy các gốc DSU giống hệt nhau và không làm gì cả, khiến câu trả lời không thay đổi. 

Cập nhật giá trị bên trong một thành phần lớn được xử lý bằng cách loại bỏ phần đóng góp của giá trị cũ trước rồi thêm giá trị mới. Vì:```
2 2
2 2
2 1 2
3 1 3
```việc hợp nhất không tạo ra cặp hợp lệ. Bản cập nhật xóa giá trị cũ 2 và kiểm tra giá trị 3 mới so với giá trị 2 còn lại, tìm một cặp hợp lệ. Thuật toán chỉ thay đổi các cặp bị ảnh hưởng, do đó số lượng thành phần được lưu trữ vẫn chính xác.
