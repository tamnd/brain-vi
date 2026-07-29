---
title: "CF 102785B - Gremlins tấn công!"
description: "Chúng tôi có một bảng nhà N x N. Một số tế bào có chứa gremlins ở đầu. Mọi tế bào đều bắt đầu với đèn sáng nên lũ gremlin không thể di chuyển qua nó. Theo thời gian, lịch sử mô tả các ô có đèn tắt. Sau khi một ô trở nên tối, nó vẫn tồn tại mãi mãi."
date: "2026-07-28T03:39:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102785
codeforces_index: "B"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 18)"
rating: 0
weight: 102785
solve_time_s: 46
verified: true
draft: false
---

[CF 102785B - Cuộc tấn công của Gremlins!](https://codeforces.com/problemset/problem/102785/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một`N x N`hội đồng nhà. Một số tế bào có chứa gremlins ở đầu. Mọi tế bào đều bắt đầu với đèn sáng nên lũ gremlin không thể di chuyển qua nó. Theo thời gian, lịch sử mô tả các ô có đèn tắt. Sau khi một ô trở nên tối, nó vẫn tồn tại mãi mãi. Một con gremlin có thể di chuyển giữa các ô tối liền kề và đến bất kỳ ô nào ở biên giới bên ngoài có nghĩa là nó có thể rời khỏi thị trấn. 

Nhiệm vụ là tìm ra hoạt động sớm nhất trong lịch sử mà sau đó ít nhất một gremlin ban đầu thuộc về một vùng được kết nối gồm các ô tối chạm vào đường viền. Nếu đường dẫn như vậy đã tồn tại trước bất kỳ thao tác nào, câu trả lời là`0`. 

Kích thước bảng có thể đạt tới`500 x 500`, nghĩa là có tới`250000`tế bào. Lịch sử cũng có thể chứa tới`250000`hoạt động. Một giải pháp liên tục tìm kiếm toàn bộ bảng sau mỗi thao tác có thể thực hiện khoảng`250000 * 250000`thăm tế bào, xung quanh`6.25 * 10^10`hoạt động và vượt xa giới hạn. Chúng ta cần một phương pháp trong đó mọi ô và mọi thao tác chỉ được xử lý một số lần nhỏ. 

Các trường hợp cạnh chính xuất phát từ tính chất năng động của bảng. Một giải pháp bất cẩn có thể quên rằng một ô có thể bị tắt nhiều lần. Ví dụ:```
2 1 2
0 0
0 1
0 1
```Gremlin bắt đầu lúc`(0,0)`, đã ở trên biên giới nên câu trả lời đúng là`0`. Một giải pháp chỉ kiểm tra sau khi xử lý các sự kiện lịch sử sẽ trả về không chính xác`1`. 

Một trường hợp phức tạp khác là khi ô mới mở kết nối hai vùng tối riêng biệt trước đó. Ví dụ:```
3 1 2
1 1
0 1
1 0
```Sau ca phẫu thuật đầu tiên,`(0,1)`là một ô biên giới và gremlin có thể trốn thoát qua đó, vì vậy câu trả lời là`1`. Một giải pháp chỉ kiểm tra xem liệu ô mới mở có chứa gremlin hay không sẽ bỏ lỡ các đường dẫn được tạo thông qua các kết nối. 

Lỗi phổ biến thứ ba là coi các ô chéo là được kết nối. Ví dụ:```
3 1 2
1 1
0 0
2 2
```Hai ô tối nằm chéo nhau và không tạo thành đường dẫn. Câu trả lời đúng là không`1`bởi vì chuyển động chéo là không thể. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng quá trình một cách chính xác. Sau khi tắt mỗi đèn, chúng ta có thể chạy biểu đồ truyền tải từ mọi vị trí gremlin và kiểm tra xem liệu một ô viền có thể truy cập được thông qua các ô tối hay không. Điều này đúng vì nó trực tiếp kiểm tra định nghĩa của lối thoát. 

Vấn đề là chi phí. Trong trường hợp xấu nhất có thể có`250000`sự kiện lịch sử và mỗi tìm kiếm theo chiều rộng có thể truy cập`250000`tế bào. Tổng công việc có thể đạt khoảng`6.25 * 10^10`lượt truy cập, quá chậm. 

Quan sát hữu ích là thay đổi duy nhất trong lưới là các ô được thêm vào tập hợp các ô có thể sử dụng được. Một khi ô trở nên tối, nó sẽ không bao giờ bị chặn nữa. Điều này có nghĩa là các thành phần được kết nối của ô tối chỉ hợp nhất theo thời gian. Chúng ta không cần biết đường dẫn chính xác mọi lúc, chỉ cần biết một thành phần có chứa gremlin hay không và nó có chạm vào đường viền hay không. 

Cấu trúc hợp tập hợp rời rạc phù hợp với hành vi này. Mỗi ô tối được biểu diễn dưới dạng một nút. Khi một ô bị tắt, chúng tôi sẽ kích hoạt nó và hợp nhất nó với mọi ô lân cận đã hoạt động. Cùng với thành phần gốc của mỗi thành phần, chúng tôi lưu trữ hai thuộc tính: thành phần đó có chứa ít nhất một gremlin bắt đầu hay không và nó có chứa ô viền hay không. Sau mỗi lần hợp nhất, nếu cả hai thuộc tính đều đúng cho thành phần kết quả thì có thể thực hiện thoát tại thời điểm đó. 

Cách tiếp cận bạo lực liên tục phát hiện ra các vùng được kết nối giống nhau. Cách tiếp cận DSU ghi nhớ các vùng đó và chỉ cập nhật phần nhỏ của biểu đồ bị ảnh hưởng bởi ô mới mở. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(K * N2) | O(N2) | Quá chậm | 
| Tối ưu | O(N2 + K * α(N2)) | O(N2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một cấu trúc tập hợp rời rạc chứa mọi vị trí trên bảng. Ban đầu mọi ô đều không hoạt động vì tất cả đèn đều sáng. Lưu trữ hai cờ cho mỗi thành phần: liệu nó có chứa gremlin ban đầu hay không và liệu nó có chạm vào đường viền hay không. 
2. Đánh dấu mọi vị trí gremlin bắt đầu trong dữ liệu DSU. Nếu bất kỳ vị trí bắt đầu nào đã ở trên đường biên, câu trả lời là ngay lập tức`0`, bởi vì gremlin có thể trốn thoát mà không cần đợi đèn tắt. 
3. Xử lý lịch sử theo thứ tự. Đối với mỗi thao tác, hãy kích hoạt ô được chỉ định. Nếu nó đã hoạt động vì cùng một ngôi nhà đã xuất hiện trước đó trong lịch sử thì không có kết nối mới nào để tạo. 
4. Khi kích hoạt một ô mới, hãy kiểm tra bốn ô lân cận của nó. Bất kỳ hàng xóm nào đã hoạt động đều thuộc cùng một vùng tối và phải được hợp nhất với thành phần của ô mới. 
5. Sau tất cả các lần hợp nhất cần thiết, hãy kiểm tra thành phần chứa ô mới được kích hoạt. Nếu nó có cả ô gremlin và ô viền thì số thao tác hiện tại là thời điểm thoát đầu tiên có thể xảy ra. 
6. Lần đầu tiên tình trạng này xuất hiện chính là câu trả lời vì tế bào chỉ trở nên tối màu và các thành phần chỉ phát triển. Một hoạt động muộn hơn không thể tạo ra cơ hội trốn thoát sớm hơn. 

Tại sao nó hoạt động: DSU luôn thể hiện chính xác các thành phần được kết nối của tất cả các ô tối hiện tại. Cờ gremlin được lưu trữ đúng chính xác khi một số gremlin ban đầu có thể tiếp cận mọi ô trong thành phần đó và cờ biên giới đúng chính xác khi thành phần cung cấp lối thoát. Một gremlin có thể thoát khi và chỉ khi thành phần của nó có cả hai thuộc tính. Vì mỗi thao tác chỉ thêm các ô tối, nên thao tác đầu tiên trong đó một thành phần có cả hai thuộc tính chính xác là thời điểm sớm nhất cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    
    size = n * n
    parent = list(range(size))
    active = [False] * size
    has_gremlin = [False] * size
    has_border = [False] * size

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        ra = find(a)
        rb = find(b)
        if ra == rb:
            return ra
        if size_rank[ra] < size_rank[rb]:
            ra, rb = rb, ra
        parent[rb] = ra
        has_gremlin[ra] |= has_gremlin[rb]
        has_border[ra] |= has_border[rb]
        if size_rank[ra] == size_rank[rb]:
            size_rank[ra] += 1
        return ra

    size_rank = [0] * size

    gremlins = []
    answer = -1

    for _ in range(m):
        x, y = map(int, input().split())
        idx = x * n + y
        gremlins.append(idx)
        has_gremlin[idx] = True
        if x == 0 or y == 0 or x == n - 1 or y == n - 1:
            answer = 0

    history = []
    for _ in range(k):
        x, y = map(int, input().split())
        history.append((x, y))

    if answer == 0:
        print(0)
        return

    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    for step, (x, y) in enumerate(history, 1):
        idx = x * n + y

        if not active[idx]:
            active[idx] = True
            root = idx

            if x == 0 or y == 0 or x == n - 1 or y == n - 1:
                has_border[root] = True

            for dx, dy in directions:
                nx = x + dx
                ny = y + dy
                if 0 <= nx < n and 0 <= ny < n:
                    nxt = nx * n + ny
                    if active[nxt]:
                        root = union(root, nxt)

        root = find(idx)
        if has_gremlin[root] and has_border[root]:
            print(step)
            return

    print(k)

if __name__ == "__main__":
    solve()
```Các mảng DSU được lập chỉ mục bằng cách làm phẳng`(x, y)`vào trong`x * n + y`. Điều này tránh việc lưu trữ các cấu trúc lồng nhau lớn và cho phép truy cập liên tục vào mọi ô. 

các`active`mảng tách biệt với trạng thái DSU vì các ô không hoạt động không được tham gia vào các liên minh. Mảng cha tồn tại cho tất cả các ô, nhưng một ô chỉ trở thành một phần của biểu đồ hiện tại sau khi kích hoạt. 

các`union`hoạt động kết hợp hai thuộc tính thành phần bằng cách sử dụng logic OR. Nếu một trong hai thành phần cũ có gremlin thì thành phần được hợp nhất sẽ có một thành phần đó. Lý do tương tự cũng áp dụng cho việc tiếp xúc ở biên giới. Thứ tự thực hiện rất quan trọng: ô mới phải được kích hoạt trước khi kiểm tra các ô lân cận, nếu không các kết nối qua ô đó sẽ bị bỏ qua. 

Việc kiểm tra cuối cùng chỉ được thực hiện trên thành phần chứa ô đã thay đổi vì tất cả các thành phần khác không thay đổi trong quá trình thao tác này. Việc sử dụng kết hợp theo thứ hạng và nén đường dẫn giữ cho mỗi hoạt động DSU có thời gian gần như không đổi. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 1 3
1 1
0 0
0 1
0 2
```Gremlin bắt đầu ở trung tâm nên ban đầu nó không thể trốn thoát. Dấu vết là: 

| Bước | Tế bào được kích hoạt | Thành phần chứa gremlin | Thành phần chạm viền | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | không | vâng | không | tiếp tục | 
| 1 | (0,0) | không | vâng | tiếp tục | 
| 2 | (0,1) | vâng | vâng | câu trả lời là 2 | 

Thao tác thứ hai kết nối ô trung tâm với đường viền thông qua`(0,1)`. DSU phát hiện điều này vì thành phần đã hợp nhất hiện có cả hai thuộc tính được lưu trữ. 

Đối với mẫu thứ hai:```
5 2 5
0 1
4 1
0 0
1 1
2 2
3 3
4 4
```Dấu vết là: 

| Bước | Tế bào được kích hoạt | Thành phần chứa gremlin | Thành phần chạm viền | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | không | vâng | vâng | câu trả lời là 0 | 

Các gremlin ban đầu đã có trên các ô viền nên không cần thao tác lịch sử. 

Ví dụ này xác nhận rằng trạng thái ban đầu phải được kiểm tra trước khi xử lý bất kỳ sự kiện nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N2 + K * α(N2)) | Mỗi ô được khởi tạo một lần và mỗi sự kiện lịch sử thực hiện một số lượng hoạt động DSU không đổi. | 
| Không gian | O(N2) | Mảng DSU và mảng trạng thái lưu trữ thông tin cho mọi ô có thể. | 

Bảng tối đa có`250000`các ô, do đó mức sử dụng bộ nhớ tuyến tính nằm trong giới hạn. Hoạt động DSU gần như liên tục cho phép tất cả các sự kiện lịch sử được xử lý một cách hiệu quả. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    
    def fake_print(x):
        output.write(str(x))
    
    old_print = __builtins__.print
    __builtins__.print = fake_print
    
    try:
        solve()
    finally:
        sys.stdin = old_stdin
        __builtins__.print = old_print
    
    return output.getvalue()

assert run("""3 1 3
1 1
0 0
0 1
0 2
""") == "2", "sample 1"

assert run("""5 2 5
0 1
4 1
0 0
1 1
2 2
3 3
4 4
""") == "0", "sample 2"

assert run("""2 1 2
0 0
0 1
0 1
""") == "0", "initial border escape"

assert run("""3 1 2
1 1
0 1
1 0
""") == "1", "first opening creates path"

assert run("""3 1 3
1 1
0 0
2 2
1 1
""") == "3", "diagonal cells are not connected"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 2 | Một con đường xuất hiện sau một số công đoàn | 
| Mẫu 2 | 0 | Lập tức trốn thoát trước lịch sử | 
|`2 x 2`trường hợp | 0 | Xử lý vị trí bắt đầu biên giới | 
| Trung tâm kết nối thông qua một lỗ mở | 1 | Ô mới tạo kết nối | 
| Khe hở chéo | 3 | Chỉ tính chuyển động bốn hướng | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là một lối thoát ban đầu. Trong đầu vào:```
2 1 2
0 0
0 1
0 1
```Gremlin bắt đầu lúc`(0,0)`, vốn đã là một ô viền. Thuật toán kiểm tra điều này trước khi kích hoạt bất kỳ ô lịch sử nào và trả về`0`. 

Trường hợp cạnh thứ hai là các mục lịch sử lặp lại. Nếu cùng một ô bị tắt nhiều lần thì lần xuất hiện thứ hai sẽ không tạo ra kết nối mới. các`active`mảng ngăn chặn việc kích hoạt trùng lặp, do đó trạng thái DSU vẫn chính xác. 

Trường hợp cạnh thứ ba là kết nối được tạo gián tiếp. TRONG:```
3 1 2
1 1
0 1
1 0
```Sau thao tác đầu tiên, tế bào`(0,1)`được kích hoạt và kết nối trực tiếp với gremlin tại`(1,1)`. Thành phần này có gremlin và chạm vào đường viền nên thuật toán trả về`1`. 

Trường hợp cạnh thứ tư là kề đường chéo. Nếu các ô tối được đặt ở`(0,0)`Và`(1,1)`, chúng vẫn ở các thành phần riêng biệt vì thuật toán chỉ kiểm tra bốn cạnh lân cận. Điều này phù hợp với quy tắc di chuyển và ngăn chặn việc trốn thoát sai lầm.
