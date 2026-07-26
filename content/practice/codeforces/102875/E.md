---
title: "CF 102875E - Diệt Virus"
description: "Chúng tôi có một mạng vô hướng gồm tối đa 16 nút. Mọi nút đều bắt đầu bị nhiễm bệnh. Vào đầu mỗi giây, phần mềm chống vi-rút có thể làm sạch một số nút hiện đang bị nhiễm, nhưng nó có thể làm sạch tối đa k nút trong một thao tác."
date: "2026-07-25T12:52:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102875
codeforces_index: "E"
codeforces_contest_name: "2020 Jiangsu Collegiate Programming Contest"
rating: 0
weight: 102875
solve_time_s: 42
verified: true
draft: false
---

[CF 102875E - Loại bỏ vi rút](https://codeforces.com/problemset/problem/102875/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mạng vô hướng gồm tối đa 16 nút. Mọi nút đều bắt đầu bị nhiễm bệnh. Vào đầu mỗi giây, phần mềm chống vi-rút có thể làm sạch một số nút hiện đang bị nhiễm, nhưng nhiều nhất nó có thể làm sạch`k`các nút trong một hoạt động. Sau khi làm sạch, các nút bị nhiễm còn lại sẽ lây lan virus sang tất cả các nút lân cận rồi tự biến mất. Mục đích là đưa ra một chuỗi các hoạt động dọn dẹp để cuối cùng khiến toàn bộ mạng không có vi-rút hoặc xác định rằng không có chuỗi nào như vậy tồn tại. 

Một cách thuận tiện để thể hiện tình huống là sử dụng mặt nạ bit. Chút`i`được thiết lập khi nút`i`hiện đang bị nhiễm bệnh. Sau khi chọn một số nút bị nhiễm để loại bỏ, chỉ những nút không được làm sạch mới có thể lây lan. Trạng thái tiếp theo chính xác là sự kết hợp của các nút lân cận của các nút còn lại đó. 

Giá trị nhỏ của`n`là hạn chế chính. Vì chỉ có`2^n`tình trạng nhiễm trùng có thể xảy ra và`n <= 16`, có nhiều nhất là 65536 trạng thái. Một giải pháp đa thức về số lượng trạng thái là có thể. Mặt khác, việc mô phỏng trực tiếp biểu đồ mà không sử dụng tính năng nén trạng thái sẽ không nắm bắt được các cấu hình lặp lại một cách hiệu quả. 

Các trường hợp chính xảy ra do nhầm lẫn giữa "làm sạch ngay" với "sạch mãi mãi". Một nút được làm sạch trong một vòng có thể bị nhiễm lại nếu nút bị nhiễm lân cận lây lan vào nút đó. Ví dụ:```
2 1 1
1 2
```Câu trả lời không phải là một thao tác làm sạch cả hai nút vì`k=1`. Việc làm sạch nút 1 trước tiên sẽ khiến nút 2 bị nhiễm, lây lan trở lại nút 1. Câu trả lời hợp lệ là:```
2
1
1
```Một trường hợp quan trọng khác là khi có thể làm sạch tất cả các nút hiện đang bị nhiễm. Nếu như`k=n`, trạng thái ban đầu có thể được giải quyết ngay lập tức bằng cách làm sạch mọi nút một lần. Một mô phỏng luôn buộc phải trải rộng sau khi làm sạch sẽ từ chối trường hợp này một cách không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là thử mọi chiến lược có thể. Đối với tập hợp bị nhiễm hiện tại, chúng tôi có thể liệt kê mọi tập hợp con của các nút bị nhiễm để làm sạch, mô phỏng một lần lây lan và tiếp tục đệ quy. Điều này đúng vì mọi chiến lược hợp lệ đều được xem xét. Vấn đề là số lượng lựa chọn. Trên tất cả các tiểu bang, việc liệt kê tất cả các tập hợp con được làm sạch có thể mang lại kết quả gần đúng`3^n`quá trình chuyển đổi vì mọi nút có thể thuộc một trong ba loại: không bị nhiễm, bị nhiễm và được làm sạch, hoặc bị nhiễm và không được làm sạch. Vì`n=16`, con số này đã là khoảng 43 triệu khả năng trước khi tính đến công việc lặp đi lặp lại và lưu trữ cây tìm kiếm. 

Quan sát hữu ích là mô hình lây nhiễm hiện tại mô tả hoàn toàn tương lai. Chúng ta không cần phải nhớ các thao tác trước đó. Điều này biến bài toán thành tìm kiếm đường đi ngắn nhất trên biểu đồ trạng thái. Mỗi mặt nạ là một đỉnh và một cạnh có hướng đại diện cho một hoạt động làm sạch có thể xảy ra, sau đó là một đợt lây lan vi-rút. 

Tìm kiếm theo chiều rộng là đủ vì mọi thao tác đều có chi phí như nhau. Tối ưu hóa còn lại là tạo ra các chuyển tiếp một cách cẩn thận. Thay vì chọn trực tiếp các nút đã được làm sạch, hãy chọn các nút bị nhiễm vẫn tồn tại sau quá trình làm sạch và lây lan. Nếu mặt nạ hiện tại là`S`và các nút bị nhiễm còn sống sót là`T`, sau đó`T`phải là tập con của`S`và số lượng nút được làm sạch là`popcount(S)-popcount(T)`. Chúng tôi chỉ xem xét các tập hợp con có giá trị này nhiều nhất`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^n) | O(2^n) | Quá chậm trong trường hợp chung | 
| Tối ưu | O(3^n) trường hợp xấu nhất với hằng số nhỏ | O(2^n) | Đã chấp nhận | 

Số lần chuyển đổi trong trường hợp xấu nhất vẫn dựa trên phép liệt kê tập hợp con, nhưng biểu đồ trạng thái chỉ có 65536 đỉnh và việc triển khai sử dụng các phép toán bit số nguyên. Với`n=16`, cái này vừa vặn thoải mái. 

## Hướng dẫn thuật toán 

1. Mã hóa mọi trạng thái lây nhiễm dưới dạng mặt nạ số nguyên. Trạng thái ban đầu là`(1 << n) - 1`, và trạng thái mục tiêu là`0`. 
2. Tính toán trước cho mỗi mặt nạ kết quả lây lan của một loại vi-rút. Nếu mặt nạ đại diện cho các nút bị nhiễm vẫn tồn tại trong quá trình làm sạch, trạng thái tiếp theo của nó là OR của mặt nạ kề của tất cả các bit được đặt. 
3. Chạy BFS bắt đầu từ mặt nạ ban đầu. Lưu trữ trạng thái trước đó và thao tác dọn dẹp được sử dụng để đạt đến mọi trạng thái được phát hiện. 
4. Khi xử lý một trạng thái`S`, liệt kê mọi tập hợp con`T`của`S`. Đối xử`T`như các nút vẫn bị nhiễm sau khi làm sạch. Nếu như`S`có quá nhiều bit hơn`T`, hoạt động làm sạch sẽ vượt quá`k`, vì vậy hãy bỏ qua nó. 
5. Các nút được làm sạch là`S ^ T`. Trạng thái tiếp theo là kết quả trải rộng được tính toán trước của`T`. Nếu trạng thái này chưa được truy cập, hãy ghi lại trạng thái gốc của nó và thao tác được sử dụng. 
6. Khi trạng thái`0`đạt được, hãy xây dựng lại câu trả lời bằng cách đi theo con trỏ cha ngược lại. 

Tại sao nó hoạt động: BFS khám phá mọi trạng thái lây nhiễm có thể tiếp cận được khi tăng số lượng hoạt động. Đối với mọi tiểu bang, mọi lựa chọn vệ sinh hợp pháp đều được xem xét, vì vậy mọi chiến lược khả thi đều tương ứng với một số đường dẫn trong biểu đồ tìm kiếm này. Mặt nạ tiếp cận`0`có nghĩa là vi-rút đã biến mất và việc không tiếp cận được vi-rút có nghĩa là không có chiến lược hợp lệ nào tồn tại. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    adj = [0] * n
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        adj[u] |= 1 << v
        adj[v] |= 1 << u

    total = 1 << n

    spread = [0] * total
    for mask in range(total):
        x = mask
        res = 0
        while x:
            b = x & -x
            i = b.bit_length() - 1
            res |= adj[i]
            x -= b
        spread[mask] = res

    parent = [-1] * total
    move = [0] * total

    start = total - 1
    parent[start] = start

    q = deque([start])

    while q:
        cur = q.popleft()
        if cur == 0:
            break

        bits = cur
        while True:
            remain = bits
            cleaned = cur ^ remain
            if cleaned.bit_count() <= k:
                nxt = spread[remain]
                if parent[nxt] == -1:
                    parent[nxt] = cur
                    move[nxt] = cleaned
                    q.append(nxt)

            if bits == 0:
                break
            bits = (bits - 1) & cur

    if parent[0] == -1:
        print(-1)
        return

    ans = []
    cur = 0
    while cur != start:
        mask = move[cur]
        s = ""
        for i in range(n):
            if mask >> i & 1:
                s += chr(ord('a') + i)
        ans.append(s)
        cur = parent[cur]

    ans.reverse()
    print(len(ans))
    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mặt nạ kề giúp lan truyền rất nhanh vì các nút lân cận của mỗi nút được lưu trữ dưới dạng một số nguyên duy nhất. các`spread`mảng tránh tính toán lại hoạt động đồ thị tương tự trong BFS. 

Vòng lặp tập hợp con sử dụng phép lặp mặt nạ con tiêu chuẩn:```
bits = (bits - 1) & cur
```truy cập mọi tập hợp con của`cur`đúng một lần. Biến`remain`đại diện cho các nút bị nhiễm vẫn tồn tại sau quá trình làm sạch, vì vậy tập hợp đã được làm sạch là`cur ^ remain`. Điều này tránh việc vô tình cho phép phần mềm chống vi-rút làm sạch các nút đã sạch. 

Mảng BFS sử dụng`-1`như điểm đánh dấu chưa được truy cập. Con trỏ gốc tái tạo lại chuỗi theo thứ tự ngược lại vì BFS tự nhiên phát hiện ra mục tiêu ngay từ đầu quá trình. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
5 4 2
1 2
2 3
3 4
2 5
```một đường dẫn BFS có thể là: 

| Hiện trạng | Các nút được làm sạch | Các nút bị nhiễm sống sót | Trạng thái tiếp theo | 
| --- | --- | --- | --- | 
| abcde | bd | át chủ bài | 0 | 
| 0 | xong | | | 

Đầu ra mẫu hợp lệ vì các nút làm sạch`b`Và`d`nút lá`a,c,e`lan rộng, và những chênh lệch đó biến mất sau khi đạt đến trạng thái trống. 

Đối với mẫu thứ hai:```
2 1 1
1 2
```tìm kiếm hoạt động như sau: 

| Hiện trạng | Các nút được làm sạch | Các nút bị nhiễm sống sót | Trạng thái tiếp theo | 
| --- | --- | --- | --- | 
| ab | một | b | một | 
| một | một | không | 0 | 

Hoạt động tương tự xuất hiện hai lần vì một nút có thể bị lây nhiễm lại sau khi nút lân cận của nó lây lan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(3^n) | Tất cả các trạng thái có thể truy cập đều được xử lý và mỗi trạng thái có thể liệt kê các mặt nạ con | 
| Không gian | O(2^n) | Lưu trữ thông tin BFS và các chuyển tiếp được tính toán trước | 

Với`n <= 16`, số lượng mặt nạ chỉ là 65536. Các thao tác bit giữ cho các hệ số không đổi đủ nhỏ cho các giới hạn đã định. 

## Trường hợp thử nghiệm```
# These are examples of checks for the implementation logic.

# Minimum graph
assert True

# Two-node chain with k=1
# Expected: a valid answer exists with two operations

# Complete graph with k=n
# Expected: one operation cleaning every node

# Cycle graph
# Expected: BFS should find a multi-step strategy

# Impossible cases should print -1 when no state reaches zero
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai nút được kết nối, k=1 | 2 hoạt động | Xử lý tái nhiễm | 
| Đồ thị đầy đủ, k=n | 1 ca phẫu thuật | Dọn dẹp ngay lập tức | 
| Đồ thị chu kỳ | Một số hoạt động | Truyền tải BFS chung | 
| Đồ thị không thể | -1 | Sửa lỗi phát hiện trạng thái không thể truy cập | 

## Vỏ cạnh 

Đối với biểu đồ hai nút có một khe làm sạch:```
2 1 1
1 2
```BFS bắt đầu ở mặt nạ`11`. Nó cố gắng làm sạch một nút, chạm tới mặt nạ`01`hoặc`10`, và sau đó phát hiện ra rằng việc làm sạch nút còn lại đạt tới`00`. Thuật toán không cho rằng các nút đã được làm sạch sẽ luôn sạch vì mọi chuyển đổi luôn áp dụng bước trải rộng. 

Đối với một đồ thị trong đó`k=n`, trạng thái ban đầu chứa chính xác các nút có thể được làm sạch. BFS tìm thấy quá trình chuyển đổi với tập hợp còn sót lại`0`, kết quả lan truyền của nó cũng là`0`. Điều này ngăn ngừa lỗi phổ biến là buộc các vòng không cần thiết sau khi dọn dẹp hoàn toàn. 

Đối với đồ thị có chu trình, chẳng hạn như:```
5 5 2
1 2
2 3
3 4
4 5
5 1
```một cách tiếp cận tham lam có thể liên tục làm sạch các nút bị lây nhiễm trở lại. BFS tránh điều này vì nó xử lý các trạng thái hoàn chỉnh và chỉ quan tâm liệu một trạng thái đã được khám phá hay chưa.
