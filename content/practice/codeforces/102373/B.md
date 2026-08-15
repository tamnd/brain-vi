---
title: "CF 102373B - Lâu Đài Gỗ"
description: "Chúng ta có một cây có các đỉnh được tô bằng hai màu, được biểu thị bằng 0 và 1. Chúng ta có thể lật màu của một đỉnh vẫn còn tồn tại, thực hiện một thao tác hoặc chọn một đỉnh và hủy toàn bộ thành phần được kết nối của màu hiện tại chứa đỉnh đó, cũng…"
date: "2026-08-14T03:03:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102373
codeforces_index: "B"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434 \u0434\u043b\u044f \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102373
solve_time_s: 143
verified: false
draft: false
---

[CF 102373B - Lâu đài bằng gỗ](https://codeforces.com/problemset/problem/102373/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có các đỉnh được tô bằng hai màu, biểu thị bằng`0`Và`1`. Chúng ta có thể lật màu của một đỉnh vẫn còn tồn tại, thực hiện một thao tác hoặc chọn một đỉnh và hủy toàn bộ thành phần được kết nối của màu hiện tại chứa đỉnh đó, đồng thời thực hiện một thao tác. Mục tiêu là loại bỏ mọi đỉnh với càng ít thao tác càng tốt. 

Câu trả lời không chỉ đơn giản là số lượng thành phần đơn sắc hiện tại, bởi vì việc tô màu lại một đỉnh được lựa chọn cẩn thận có thể hợp nhất một số thành phần và tạo ra một phản ứng dây chuyền loại bỏ tất cả chúng. Mẫu minh họa chính xác điều này: trong ngôi sao có màu trung tâm`1`và màu ba lá`0`, thay đổi tâm thành`0`tạo ra một thành phần đơn sắc, vì vậy hai thao tác là đủ. 

Cây chứa tới`200000`đỉnh. Vì một cái cây có chính xác`n - 1`các cạnh, việc đọc và xử lý đầu vào đã yêu cầu thời gian tuyến tính. Một thuật toán có hành vi bậc hai quá chậm ở kích thước này và việc tìm kiếm theo cấp số nhân trên các màu là hoàn toàn không khả thi. Giải pháp dự định chỉ cần xử lý mọi đỉnh và cạnh một số lần không đổi, đưa ra`O(n)`thời gian. 

Có một số trường hợp nhỏ mà giải pháp hời hợt có thể thất bại. Ví dụ, với một đỉnh,```
1
0
```câu trả lời là`1`, bởi vì đỉnh đơn có thể bị phá hủy một cách đơn giản. Một công thức luôn thêm thao tác đổi màu sẽ trả về sai`2`. 

Một cây hoàn toàn bằng nhau như```
3
000
1 2
2 3
```cũng có câu trả lời`1`. Mọi đỉnh đều thuộc về cùng một thành phần đơn sắc, vì vậy một phản ứng dây chuyền sẽ loại bỏ toàn bộ cây. 

Cực đoan ngược lại là một con đường xen kẽ:```
3
010
1 2
2 3
```Câu trả lời của nó là`2`. Đổi màu đỉnh giữa mang lại`000`, sau đó một phản ứng dây chuyền sẽ loại bỏ mọi thứ. Một phương pháp chỉ đếm các thành phần đơn sắc ban đầu sẽ thấy ba thành phần và bỏ lỡ việc đổi màu hữu ích. 

Cấu trúc của cây cũng quan trọng. TRONG```
4
1000
1 2
1 3
1 4
```câu trả lời là`2`, không`4`: hoặc phá hủy phần trung tâm màu đen và sau đó là các lá màu trắng làm một thành phần hoặc đổi màu phần trung tâm thành`0`và phá hủy cây toàn màu trắng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp là quyết định màu sắc mà mọi đỉnh phải có ngay trước khi nó bị phá hủy. có`2^n`màu sắc mục tiêu có thể. Đối với mỗi màu mục tiêu, chúng ta có thể đếm xem có bao nhiêu đỉnh thay đổi màu và bao nhiêu thành phần đơn sắc được kết nối mà nó chứa, sau đó lấy giá trị tối thiểu. Đánh giá một màu mất`O(n)`thời gian vì chúng tôi kiểm tra mọi đỉnh và cạnh, do đó độ phức tạp tổng cộng là`Theta(n * 2^n)`. Mấy chục đỉnh thế này đã vô vọng rồi chứ đừng nói chi`200000`. 

Quan sát hữu ích là chúng ta thực sự không phải mô phỏng các phản ứng dây chuyền. 

Hãy xem xét bất kỳ chuỗi hoạt động hoàn chỉnh nào. Cung cấp cho mỗi đỉnh màu sắc có tại thời điểm nó bị phá hủy. Mỗi đỉnh có màu hủy cuối cùng khác với màu ban đầu của nó phải được tô lại ít nhất một lần. Bây giờ hãy nhìn vào hai đỉnh liền kề có cùng màu hủy diệt. Nếu chúng bị phá hủy trong các phản ứng dây chuyền khác nhau, về mặt khái niệm, chúng ta có thể hợp nhất các phản ứng đó thành một thành phần đơn sắc, điều này chỉ có thể làm giảm số lượng hoạt động phá hủy. Do đó, một chiến lược tối ưu có thể được biểu diễn bằng màu nhị phân cuối cùng của cây. 

Đối với màu cuối cùng cố định, số thao tác tô màu lại cần thiết chính xác bằng khoảng cách Hamming của nó so với màu ban đầu. Số phản ứng dây chuyền là số thành phần đơn sắc được kết nối trong màu cuối cùng đó. 

Vì biểu đồ là một cây nên số thành phần này có dạng đặc biệt đơn giản. Bắt đầu với một thành phần. Mỗi cạnh có điểm cuối có màu cuối cùng khác nhau sẽ phân tách hai thành phần, trong khi cạnh có điểm cuối có cùng màu sẽ nằm trong một thành phần. Do đó,`number of components = 1 + number of edges whose endpoints have different colors`. 

Do đó, toàn bộ vấn đề trở nên giảm thiểu`1 + sum over vertices [final_color != initial_color] + sum over edges [final_color differs across the edge]`. 

Đây là cây DP hai trạng thái tiêu chuẩn. Đối với mỗi đỉnh, chúng tôi tính toán chi phí tối thiểu của cây con của nó khi đỉnh đó nhận được màu cuối cùng`0`hoặc`1`. Sự đóng góp của một đứa trẻ chỉ phụ thuộc vào màu cuối cùng của đứa trẻ có bằng màu cuối cùng của cha mẹ hay không, vì vậy mỗi đứa trẻ có thể được xử lý độc lập. 

Lực lượng vũ phu hoạt động vì mọi màu cuối cùng có thể mô tả một cách hợp lệ để tổ chức quá trình hủy diệt, nhưng nó không thành công vì có nhiều màu theo cấp số nhân. Nhận xét rằng mục tiêu chỉ bao gồm chi phí đỉnh và chi phí cạnh độc lập cho phép chúng ta tối ưu hóa những mục tiêu đó`2^n`các lựa chọn với DP hai trạng thái trên cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n * 2^n)`|`O(n)`| Quá chậm | 
| Cây tối ưu DP |`O(n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại đỉnh`0`. Việc chọn gốc là tùy ý vì mục tiêu được xác định trên cây vô hướng. 
2. Với mọi đỉnh`v`, duy trì hai giá trị`dp[v][0]`Và`dp[v][1]`.`dp[v][c]`là chi phí tối thiểu được đóng góp bởi cây con của`v`nếu như`v`nhận được màu sắc cuối cùng`c`, không bao gồm toàn cầu`+1`tương ứng với thành phần được kết nối đầu tiên. 
3. Khởi tạo trạng thái`v`với chi phí đổi màu của nó. Nếu màu gốc của`v`đã rồi`c`, chi phí này là`0`; nếu không thì nó là`1`. 
4. Xử lý từng đứa trẻ`u`của`v`. Nếu như`u`nhận được cùng một màu`c`BẰNG`v`, cạnh`(v,u)`không tạo thêm ranh giới thành phần đơn sắc, do đó trẻ góp phần`dp[u][c]`. Nếu như`u`nhận được màu đối lập, cạnh là ranh giới giữa hai thành phần và góp phần`dp[u][1-c] + 1`. 

Do đó, quá trình chuyển đổi cho màu gốc cố định`c`là`dp[v][c] += min(dp[u][c], dp[u][1-c] + 1)`. 

các`+1`trong tùy chọn thứ hai biểu thị cạnh có điểm cuối có màu cuối cùng khác nhau. 
5. Xử lý các đỉnh theo thứ tự sau để giá trị DP của mọi nút con đều được biết trước khi tính toán nút cha của nó. Việc truyền tải lặp đi lặp lại được ưu tiên hơn trong Python vì cây có thể là một đường dẫn có độ dài`200000`, sẽ vượt quá độ sâu cuộc gọi đệ quy mặc định của Python. 
6. Ở gốc, chọn màu cuối cùng nào rẻ hơn. Cuối cùng thêm`1`đối với thuật ngữ đếm thành phần toàn cầu, đưa ra`answer = 1 + min(dp[root][0], dp[root][1])`. 

phần bổ sung`1`luôn tồn tại vì mọi cây khác rỗng đều có ít nhất một thành phần đơn sắc. 

### Tại sao nó hoạt động 

Đối với bất kỳ màu cuối cùng nào được chọn, mỗi đỉnh có màu đã thay đổi sẽ tốn chính xác một thao tác tô màu lại và mỗi cạnh nối các màu cuối cùng khác nhau sẽ tăng số lượng thành phần đơn sắc lên đúng một. Vì một cây bắt đầu bằng một thành phần nên tổng chi phí cho việc tô màu đó chính xác là mục tiêu DP cộng thêm một. 

Ngược lại, bất kỳ chuỗi thao tác thực tế nào cũng có thể được chuyển đổi thành màu cuối cùng như vậy bằng cách gán màu cho mỗi đỉnh khi nó bị phá hủy. Số lần tô màu lại trong dãy ít nhất là số đỉnh có màu cuối cùng khác với màu ban đầu của chúng, trong khi số lần thao tác hủy ít nhất là số thành phần đơn sắc của màu cuối cùng. Do đó, mục tiêu DP không thể lớn hơn chi phí của một chuỗi hoạt động tối ưu. Đối với mỗi màu DP, trước tiên chúng ta có thể thực hiện các lần đổi màu cần thiết và sau đó hủy từng thành phần đơn sắc để có thể đạt được giá trị DP. Hai hướng thiết lập sự tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    graph = [[] for _ in range(n)]

    for _ in range(n - 1):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        graph[a].append(b)
        graph[b].append(a)

    parent = [-1] * n
    order = [0]
    parent[0] = n

    for v in order:
        for u in graph[v]:
            if u == parent[v]:
                continue
            parent[u] = v
            order.append(u)

    dp0 = [0] * n
    dp1 = [0] * n

    for v in reversed(order):
        original = ord(s[v]) - ord('0')

        dp0[v] = 1 if original != 0 else 0
        dp1[v] = 1 if original != 1 else 0

        for u in graph[v]:
            if parent[u] != v:
                continue

            dp0[v] += min(dp0[u], dp1[u] + 1)
            dp1[v] += min(dp1[u], dp0[u] + 1)

    print(1 + min(dp0[0], dp1[0]))

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ cây ở dạng vô hướng thông thường. Vì có chính xác`n - 1`các cạnh, nó sử dụng bộ nhớ tuyến tính. 

Các cấu trúc truyền tải đầu tiên`parent`Và`order`. Bởi vì`order`được tạo ra bằng cách rời khỏi gốc, đảo ngược nó sẽ mang lại một thứ tự hợp lệ từ dưới lên. Điều này tránh hoàn toàn đệ quy, điều này rất hữu ích cho cây có hình đường dẫn với`200000`đỉnh. 

Hai mảng DP lưu trữ hai màu cuối cùng có thể có một cách độc lập. Đối với một đỉnh có màu ban đầu là`0`,`dp0[v]`bắt đầu lúc`0`trong khi`dp1[v]`bắt đầu lúc`1`. Điều ngược lại xảy ra với bản gốc`1`. 

Khi con`u`được gắn vào cha mẹ buộc phải tô màu`0`, chỉ có hai khả năng. Giữ`u`Tại`0`chi phí`dp0[u]`, trong khi tô màu cho nó`1`chi phí`dp1[u] + 1`vì cạnh kết nối trở thành ranh giới giữa các thành phần đơn sắc. Lý do tương tự đưa ra sự chuyển đổi cho màu gốc`1`. 

Bài kiểm tra`if parent[u] != v`là cần thiết vì danh sách kề chứa cả hai hướng của mỗi cạnh. Nó ngăn không cho cha mẹ bị xử lý như thể đó là một đứa trẻ khác. 

Tất cả các chi phí nhiều nhất là bội số nhỏ của`n`, vì vậy số nguyên Python không có vấn đề tràn. Việc sử dụng`sys.stdin.readline`giữ cho quá trình xử lý đầu vào tuyến tính và đủ nhanh để`200000`các cạnh. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
4
1000
1 2
1 3
1 4
```Gốc cây ở đỉnh`1`. Lá là đỉnh`2`,`3`, Và`4`, tất cả ban đầu`0`. 

| Đỉnh | Bản gốc |`dp0`trước trẻ em |`dp1`trước trẻ em | Cuối cùng`dp0`| Cuối cùng`dp1`| 
| --- | --- | --- | --- | --- | --- | 
| 4 | 0 | 0 | 1 | 0 | 1 | 
| 3 | 0 | 0 | 1 | 0 | 1 | 
| 2 | 0 | 0 | 1 | 0 | 1 | 
| 1 | 1 | 1 | 0 | 1 | 3 | 

Để phần gốc kết thúc bằng màu sắc`0`, mỗi chiếc lá cũng có thể kết thúc bằng màu sắc`0`, đưa ra chi phí`0`cho mỗi đứa trẻ. Bản thân gốc cần được đổi màu một lần, vì vậy chi phí DP là`1`. 

Để phần gốc kết thúc bằng màu sắc`1`, giữ mỗi lá ở`0`tạo ra ba cạnh ranh giới, đưa ra chi phí`3`. 

Câu trả lời cuối cùng là`1 + min(1, 3) = 2`. 

Chiến lược tương ứng chính xác là chiến lược của mẫu: tô màu lại đỉnh`1`, sau đó phá hủy thành phần toàn màu trắng. 

### Đã thi công mẫu 2 

Hãy xem xét```
3
010
1 2
2 3
```Cây là một con đường và màu sắc của nó xen kẽ nhau. 

| Đỉnh | Bản gốc |`dp0`|`dp1`| 
| --- | --- | --- | --- | 
| 3 | 0 | 0 | 1 | 
| 2 | 1 | 1 | 1 | 
| 1 | 0 | 1 | 2 | 

Tại đỉnh`3`, giữ màu`0`không tốn kém gì, trong khi thay đổi nó thành`1`tốn một cái. 

Tại đỉnh`2`, chọn màu`0`tốn một lần để đổi màu và sau đó có thể sử dụng đỉnh`3`ở màu sắc`0`không có ranh giới cạnh, cho`1`. Chọn màu`1`tốn một lần để tô màu lại, trong khi đứa trẻ có thể vẫn còn`0`với chi phí cạnh bổ sung, cũng mang lại`1`. 

Tại đỉnh`1`, chọn màu`0`cho phép đỉnh`2`sử dụng màu sắc`0`với chi phí`1`, vậy chi phí của cây con là`1`. Chọn màu`1`đắt hơn. 

Câu trả lời là`1 + min(1, 2) = 2`. 

Điều này tương ứng với việc thay đổi đỉnh giữa từ`1`ĐẾN`0`, sau đó cả ba đỉnh tạo thành một thành phần đơn sắc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Mỗi đỉnh và mỗi cạnh cây được xử lý một số lần không đổi. | 
| Không gian |`O(n)`| Danh sách kề, mảng truyền tải, mảng cha và hai mảng DP đều tuyến tính trong`n`. | 

Với`n <= 200000`, xử lý tuyến tính là thích hợp. Thuật toán chỉ thực hiện một số phép tính số học cho mỗi cạnh và tránh được cả các vấn đề về độ sâu đệ quy cũng như liệt kê hàm mũ, do đó nó phù hợp với các ràng buộc dự định một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    s = input().strip()

    graph = [[] for _ in range(n)]

    for _ in range(n - 1):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        graph[a].append(b)
        graph[b].append(a)

    parent = [-1] * n
    order = [0]
    parent[0] = n

    for v in order:
        for u in graph[v]:
            if u == parent[v]:
                continue
            parent[u] = v
            order.append(u)

    dp0 = [0] * n
    dp1 = [0] * n

    for v in reversed(order):
        original = ord(s[v]) - ord('0')

        dp0[v] = original
        dp1[v] = 1 - original

        for u in graph[v]:
            if parent[u] != v:
                continue

            dp0[v] += min(dp0[u], dp1[u] + 1)
            dp1[v] += min(dp1[u], dp0[u] + 1)

    return str(1 + min(dp0[0], dp1[0]))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve().strip()
    finally:
        sys.stdin = old_stdin

assert run("""4
1000
1 2
1 3
1 4
""") == "2", "sample 1"

assert run("""1
0
""") == "1", "single vertex"

assert run("""3
000
1 2
2 3
""") == "1", "all vertices already form one component"

assert run("""3
010
1 2
2 3
""") == "2", "recolor the middle vertex"

assert run("""4
0101
1 2
2 3
3 4
""") == "3", "alternating path"

n = 200000
edges = "\n".join(f"{i} {i + 1}" for i in range(1, n))
large_case = f"{n}\n" + ("0" * n) + "\n" + edges + "\n"
assert run(large_case) == "1", "maximum-size all-equal tree"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`1`| Cây có kích thước tối thiểu và không tồn tại cạnh phụ. | 
|`000`trên một con đường |`1`| Tất cả các đỉnh đều thuộc về một thành phần đơn sắc. | 
|`010`trên một con đường |`2`| Việc đổi màu có thể hợp nhất nhiều thành phần. | 
|`0101`trên một con đường |`3`| Màu sắc xen kẽ và các quyết định ranh giới cạnh lặp đi lặp lại. | 
|`200000`số không trên một con đường |`1`| Kích thước đầu vào tối đa, truyền tải lặp lại và độ phức tạp tuyến tính. | 

## Vỏ cạnh 

Trường hợp một đỉnh được xử lý trực tiếp bởi DP. Vì```
1
0
```trạng thái duy nhất có màu cuối cùng`0`có chi phí`0`, trong khi trạng thái có màu cuối cùng`1`có chi phí`1`. trận chung kết`+1`cho`1`, đó chính xác là một thao tác hủy cần thiết. 

Đối với một cây đã đơn sắc,```
3
000
1 2
2 3
```mọi đỉnh đều có thể giữ lại màu`0`. Cả hai cạnh đều có điểm cuối bằng nhau, do đó không đóng góp một ranh giới thành phần nào. Giá trị DP trước hằng số toàn cầu là`0`, và câu trả lời là`1`. Một phương pháp chỉ dựa trên số lần đổi màu sẽ bỏ sót rằng vẫn cần một thao tác phá hủy. 

Đối với đường đi xen kẽ```
3
010
1 2
2 3
```màu ban đầu có ba thành phần. DP chọn màu cuối cùng`000`, trả tiền một lần để tô màu lại cho đỉnh`2`và chi phí ranh giới bằng không. Việc thêm thành phần đầu tiên bắt buộc sẽ mang lại`2`. Điều này mắc phải sai lầm phổ biến khi cho rằng việc phá hủy các thành phần gốc luôn là tối ưu. 

Đối với ngôi sao```
4
1000
1 2
1 3
1 4
```DP chọn màu cuối cùng`0`cho mọi đỉnh. Chỉ có phần trung tâm thay đổi màu sắc nên chi phí đổi màu là`1`và cả ba cạnh đều có điểm cuối bằng nhau nên không có ranh giới thành phần bổ sung. Câu trả lời là`1 + 1 = 2`. 

Cuối cùng, một con đường`200000`các đỉnh có màu bằng nhau nhấn mạnh việc thực hiện hơn là toán học. Quá trình truyền tải gốc lặp lại xử lý đường dẫn mà không cần đệ quy và mỗi chuyển đổi DP được xử lý một lần. Kết quả vẫn còn`1`, trong khi tổng công việc tăng tuyến tính theo số đỉnh.
