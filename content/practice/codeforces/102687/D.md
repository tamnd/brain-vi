---
title: "CF 102687D - Kapuluan ng Kalayaan 2"
description: "Chúng tôi có một tập hợp các hòn đảo được kết nối bằng các tuyến phà. Mỗi tuyến đường có yêu cầu về độ tuổi tối thiểu, nghĩa là một người chỉ có thể sử dụng tuyến đường đó nếu tuổi của họ ít nhất bằng giá trị yêu cầu của tuyến đường."
date: "2026-08-01T10:32:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102687
codeforces_index: "D"
codeforces_contest_name: "2020 National Olympiad in Informatics - Philippines (NOI.PH) Online Finals, Day 1"
rating: 0
weight: 102687
solve_time_s: 53
verified: true
draft: false
---

[CF 102687D - Kapuluan ng Kalayaan 2](https://codeforces.com/problemset/problem/102687/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một tập hợp các hòn đảo được kết nối bằng các tuyến phà. Mỗi tuyến đường có yêu cầu về độ tuổi tối thiểu, nghĩa là một người chỉ có thể sử dụng tuyến đường đó nếu tuổi của họ ít nhất bằng giá trị yêu cầu của tuyến đường. Đối với mỗi kẻ nổi loạn, chúng tôi biết tuổi của họ và phải chọn hai hòn đảo: một hòn đảo nơi phiến quân sinh sống và một nơi đồng đội của họ sinh sống. Đối tác không bao giờ di chuyển, trong khi kẻ nổi loạn có thể đi qua mọi tuyến phà mà độ tuổi của họ cho phép. 

Đối với một kẻ nổi loạn, vị trí chỉ không hợp lệ khi đảo xuất phát của kẻ nổi loạn và đảo của đối tác thuộc cùng một thành phần được kết nối sau khi chỉ giữ lại các tuyến phà có yêu cầu tối đa là độ tuổi của kẻ nổi loạn. Chúng ta cần đếm số lượng vị trí hợp lệ cho mỗi kẻ nổi loạn và nhân lên những lựa chọn độc lập này. 

Kích thước đầu vào lớn. Có thể có tới một triệu hòn đảo và hai trăm nghìn tuyến phà và phiến quân. Một giải pháp khám phá biểu đồ riêng biệt cho từng kẻ nổi loạn sẽ đòi hỏi quá nhiều công sức vì cùng một thông tin kết nối sẽ được tính toán lại nhiều lần. Với vài trăm nghìn truy vấn, chúng tôi cần xử lý biểu đồ trên toàn cầu và trả lời từng ngưỡng tuổi một cách hiệu quả. 

Các trường hợp khó khăn đến từ việc kết nối thay đổi khi độ tuổi tăng lên. Ví dụ, hãy xem xét:```
3 2 1
1 2 5
2 3 10
5
```Lúc 5 tuổi, chỉ có chiếc phà đầu tiên hoạt động. Các thành phần là`{1,2}`Và`{3}`. Số lượng vị trí đặt hàng không hợp lệ là`2*2 + 1*1 = 5`, vậy câu trả lời hợp lệ là`9 - 5 = 4`. Một giải pháp bất cẩn khi xây dựng biểu đồ bằng cách sử dụng tất cả các tuyến đường sẽ nghĩ sai rằng tất cả các hòn đảo đều được kết nối. 

Một trường hợp khác là khi mọi hòn đảo đều được kết nối:```
3 3 1
1 2 1
2 3 1
1 3 1
10
```Chỉ có một thành phần có kích thước 3, vì vậy mọi vị trí có thể đều cho phép kẻ nổi loạn cuối cùng tiếp cận được đối tác. Câu trả lời là`0`. Việc quên trường hợp này thường dẫn đến việc coi hòn đảo khởi đầu của kẻ nổi loạn là địa điểm an toàn. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ xử lý từng kẻ nổi loạn một cách riêng biệt. Đối với một kẻ nổi loạn ở tuổi`H`, chúng ta có thể chạy đồ thị truyền tải trong khi bỏ qua mọi phà có yêu cầu lớn hơn`H`, đánh dấu các hòn đảo có thể tiếp cận và đếm các hòn đảo bên ngoài tập hợp đó. Điều này đúng vì nó mô phỏng chính xác quy luật di chuyển của phiến quân. Tuy nhiên, làm điều này với mọi kẻ nổi loạn có thể mất`O(k(n+m))`thời gian. Với`k`Và`m`xung quanh`2 * 10^5`, điều này vượt xa những gì có thể. 

Quan sát quan trọng là các tuyến phà chỉ khả dụng khi độ tuổi tăng lên. Nếu chúng ta sắp xếp tất cả các tuyến phà theo độ tuổi yêu cầu và xử lý các điểm nổi loạn theo thứ tự độ tuổi tăng dần thì biểu đồ sẽ tiến triển bằng cách chỉ thêm các cạnh. Đây chính xác là tình huống mà cấu trúc hợp tập hợp rời rạc trở nên hữu ích. 

Đối với một độ tuổi cố định, giả sử kích thước thành phần được kết nối là`s1, s2, ...`. Số cặp đảo có thứ tự xấu là:```
s1^2 + s2^2 + ...
```bởi vì kẻ nổi loạn và đối tác không thể tránh mặt nhau khi họ được chọn trong cùng một thành phần. Tổng số vị trí đặt hàng là`n^2`, do đó số vị trí hợp lệ là:```
n^2 - (s1^2 + s2^2 + ...)
```Trong khi hợp nhất hai thành phần kích thước`a`Và`b`, phần đóng góp thay đổi từ`a^2 + b^2`ĐẾN`(a+b)^2`. Việc duy trì giá trị này cho phép chúng tôi trả lời mọi truy vấn ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k(n+m)) | O(n) | Quá chậm | 
| Tối ưu | O((n+m+k) log m) | O(n+m+k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các tuyến phà theo yêu cầu về độ tuổi. Sắp xếp tất cả những kẻ nổi loạn theo độ tuổi của họ trong khi ghi nhớ vị trí ban đầu của họ. 
2. Bắt đầu với mỗi hòn đảo trong thành phần riêng biệt của nó. Ban đầu tổng kích thước thành phần bình phương là`n`, bởi vì mọi thành phần đều có kích thước bằng một. 
3. Quy trình nổi loạn từ trẻ nhất đến lớn tuổi nhất. Trước khi trả lời một kẻ nổi loạn bằng tuổi tác`H`, gộp tất cả các tuyến phà có yêu cầu tối đa`H`. 
4. Trong quá trình hợp nhất, hãy loại bỏ hai khoản đóng góp thành phần cũ khỏi tổng được duy trì và thêm phần đóng góp của thành phần đã hợp nhất. Nếu kích thước thành phần là`a`Và`b`, cập nhật:```
sum = sum - a^2 - b^2 + (a+b)^2
```1. Sau khi thêm tất cả các chuyến phà có thể có cho độ tuổi này, các vị trí hợp lệ của kẻ nổi loạn là:```
n^2 - sum
```Lưu trữ giá trị này cho kẻ nổi loạn đó. 

1. Nhân tất cả các giá trị được lưu trữ theo modulo`10^9+7`bởi vì mỗi kẻ nổi loạn đều chọn hai ngôi nhà cho riêng mình một cách độc lập. 

Tại sao nó hoạt động: 

Sau khi xử lý tất cả các cạnh có yêu cầu tối đa là độ tuổi của phiến quân hiện tại, cấu trúc tập hợp rời rạc thể hiện chính xác các hòn đảo mà phiến quân có thể tiếp cận. Tổng kích thước bình phương được duy trì sẽ tính mọi vị trí không hợp lệ vì mọi cặp bên trong cùng một thành phần đều có thể truy cập được. Trừ nó khỏi tất cả các cặp đảo có thứ tự có thể để lại chính xác các vị trí an toàn. Vì sự lựa chọn của mỗi kẻ nổi loạn không hạn chế sự lựa chọn của bất kỳ kẻ nổi loạn nào khác, nên việc nhân các số liệu này sẽ đưa ra câu trả lời cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, m, k = map(int, input().split())

    edges = []
    for _ in range(m):
        u, v, d = map(int, input().split())
        edges.append((d, u - 1, v - 1))

    ages = list(map(int, input().split()))

    edges.sort()
    queries = sorted((h, i) for i, h in enumerate(ages))

    parent = list(range(n))
    size = [1] * n

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        ra = find(a)
        rb = find(b)
        if ra == rb:
            return
        if size[ra] < size[rb]:
            ra, rb = rb, ra

        nonlocal_sum[0] -= size[ra] * size[ra] + size[rb] * size[rb]
        parent[rb] = ra
        size[ra] += size[rb]
        nonlocal_sum[0] += size[ra] * size[ra]

    nonlocal_sum = [n]
    ans = [0] * k
    edge_ptr = 0
    total_pairs = n * n

    for h, idx in queries:
        while edge_ptr < m and edges[edge_ptr][0] <= h:
            _, u, v = edges[edge_ptr]
            union(u, v)
            edge_ptr += 1

        ans[idx] = total_pairs - nonlocal_sum[0]

    result = 1
    for x in ans:
        result = (result * x) % MOD

    print(result)

if __name__ == "__main__":
    solve()
```Danh sách cạnh được sắp xếp một lần và con trỏ`edge_ptr`chỉ tiến về phía trước. Điều này có nghĩa là mỗi tuyến phà được xem xét đúng một lần. Cấu trúc tập hợp rời rạc lưu trữ các thành phần được kết nối hiện tại và việc kết hợp theo kích thước cộng với việc nén đường dẫn giúp mỗi thao tác có thời gian gần như không đổi. 

Biến`nonlocal_sum`đại diện cho tổng số cặp được đặt hàng không hợp lệ cho ngưỡng tuổi hiện tại. Nó phải được cập nhật trước và sau khi hợp nhất vì hai thành phần cũ không còn tồn tại sau khi hợp nhất. Phép trừ được thực hiện trước khi thay đổi mảng gốc, điều này tránh việc vô tình làm mất kích thước thành phần ban đầu. 

Tất cả các phép tính liên quan đến số lượng cặp đảo đều sử dụng số nguyên Python, do đó không có vấn đề tràn. Phép nhân cuối cùng được giảm modulo`10^9+7`vì số lượng bài tập có thể cực kỳ lớn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4 3 2
1 2 567600000
2 3 662300000
3 4 567600000
536100000 630700000
```Kẻ nổi loạn đầu tiên còn quá trẻ để sử dụng bất kỳ chiếc phà nào. 

| Tuổi | Đã thêm cạnh | Kích thước thành phần | Cặp không hợp lệ | Cặp hợp lệ | 
| --- | --- | --- | --- | --- | 
| 536100000 | không | 1,1,1,1 | 4 | 12 | 
| 630700000 | 1-2 và 3-4 | 2,2 | 8 | 8 | 

Sản phẩm là`12 * 8 = 96`. 

Dấu vết này cho thấy tại sao mỗi kẻ nổi loạn phải được đánh giá ở độ tuổi của chính họ. Biểu đồ tương tự không áp dụng cho mọi người. 

Đối với mẫu thứ hai:```
3 4 2
1 2 599200000
2 3 599200000
3 1 599200000
1 3 410000000
504600000 1009200000
```| Tuổi | Đã thêm cạnh | Kích thước thành phần | Cặp không hợp lệ | Cặp hợp lệ | 
| --- | --- | --- | --- | --- | 
| 504600000 | 1-3 | 2,1 | 5 | 4 | 
| 1009200000 | các cạnh còn lại | 3 | 9 | 0 | 

Kẻ nổi loạn lớn tuổi hơn có thể đi bất cứ đâu nên không có vị trí nào là an toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m+k) log m) | Việc sắp xếp chiếm ưu thế, trong khi mỗi thao tác tìm kiếm liên kết có thời gian gần như không đổi | 
| Không gian | O(n+m+k) | Lưu trữ biểu đồ, truy vấn và các mảng tập hợp rời rạc | 

Các ràng buộc yêu cầu tránh việc duyệt đồ thị lặp đi lặp lại. Việc sắp xếp các cạnh và sử dụng DSU cho phép xử lý tất cả các thay đổi về kết nối trong một lần, vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = inp.strip().split()
    if not data:
        return ""
    n, m, k = map(int, data[:3])
    pos = 3
    edges = []
    for _ in range(m):
        u, v, d = map(int, data[pos:pos+3])
        pos += 3
        edges.append((u, v, d))
    ages = list(map(int, data[pos:pos+k]))

    sys.stdin = old
    return ""

# samples and custom cases should be executed against the full solve function.
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 2 1 / 1 2 5 / 2 3 10 / 5`|`4`| Kết nối một phần | 
|`3 3 1 / 1 2 1 / 2 3 1 / 1 3 1 / 10`|`0`| Đồ thị được kết nối đầy đủ | 
|`2 1 1 / 1 2 100 / 1`|`2`| Biểu đồ tối thiểu không có cạnh | 
| Biểu đồ có nhiều tuyến phà trùng lặp | đúng sản phẩm | Xử lý cạnh trùng lặp | 

## Vỏ cạnh 

Khi độ tuổi nhỏ hơn mọi yêu cầu của phà, tập hợp rời rạc vẫn còn với mỗi hòn đảo riêng biệt. Vì`n=3`, điều này mang lại số lượng cặp không hợp lệ là`3`, vậy số đếm hợp lệ là`9-3=6`. Thuật toán xử lý việc này một cách tự nhiên vì không có cạnh nào được hợp nhất. 

Khi có nhiều tuyến phà kết nối cùng một hòn đảo, chỉ có sự hợp nhất thành công đầu tiên mới quan trọng. Các tuyến sau sẽ tìm thấy cả hai hòn đảo đã nằm trong cùng một thành phần và không làm gì cả. Điều này ngăn các tuyến đường trùng lặp thay đổi số lượng thành phần. 

Khi tất cả các hòn đảo được kết nối, tổng số tiền duy trì sẽ trở thành`n^2`. Câu trả lời trở thành số 0 vì mọi vị trí đối tác và kẻ nổi loạn có thể đều nằm bên trong một thành phần có thể tiếp cận được. 

Khi một số phiến quân có cùng độ tuổi, họ đều sử dụng cùng một trạng thái DSU. Sắp xếp truy vấn theo độ tuổi có nghĩa là biểu đồ được tạo một lần cho ngưỡng đó thay vì được xây dựng lại riêng cho từng kẻ nổi loạn. 

Tôi cũng có thể cung cấp phiên bản biên tập cuộc thi ngắn hơn hoặc phiên bản tập trung vào bằng chứng chính thức hơn nếu bạn muốn điều chỉnh nó để xuất bản.
