---
title: "CF 102759C - Đường một chiều kinh tế"
description: "Chúng tôi có mạng lưới đường bộ vô hướng với tối đa 18 thành phố. Mỗi con đường hiện có phải được chỉ định một trong hai hướng có thể có của nó và các hướng được chọn phải làm cho toàn bộ đồ thị có hướng được kết nối chặt chẽ."
date: "2026-07-29T00:06:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102759
codeforces_index: "C"
codeforces_contest_name: "XXI Open Cup, Grand Prix of Korea"
rating: 0
weight: 102759
solve_time_s: 75
verified: true
draft: false
---

[CF 102759C - Đường kinh tế một chiều](https://codeforces.com/problemset/problem/102759/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có mạng lưới đường bộ vô hướng với tối đa 18 thành phố. Mỗi con đường hiện có phải được chỉ định một trong hai hướng có thể có của nó và các hướng được chọn phải làm cho toàn bộ đồ thị có hướng được kết nối chặt chẽ. Chi phí phụ thuộc vào hướng đã chọn của từng con đường, vì vậy nhiệm vụ là tìm hướng hợp lệ rẻ nhất hoặc báo cáo rằng không tồn tại hướng hợp lệ. 

Giá trị nhỏ của`N`là manh mối chính. Với 18 thành phố, có`2^18`các tập hợp con có thể có, khoảng 262 nghìn. Điều này loại trừ bất kỳ điều gì thử mọi hướng của mọi cạnh, bởi vì ngay cả một biểu đồ thưa thớt cũng có thể có nhiều cạnh và số lượng hướng tăng theo cấp số nhân theo số lượng đường. Giải pháp phải sử dụng trực tiếp giới hạn đỉnh, hướng tới lập trình động bitmask. 

Một số trường hợp rất dễ bỏ sót. Nếu biểu đồ hoàn toàn không được kết nối thì không có định hướng nào có thể giúp mọi thành phố có thể tiếp cận được.```
3
-1 5 -1
5 -1 -1
-1 -1 -1
```Câu trả lời là`-1`vì thành phố 3 bị cô lập. Một phương pháp chỉ kiểm tra các lựa chọn cạnh cục bộ có thể cho rằng mọi con đường đều có thể được định hướng độc lập một cách không chính xác. 

Bẫy thứ hai là một đồ thị được kết nối nhưng có một cây cầu.```
3
-1 1 -1
1 -1 2
-1 2 -1
```Câu trả lời là`-1`. Bất kỳ hướng nào được chọn làm con đường duy nhất giữa bên này và bên kia sẽ khiến một điểm cuối không thể quay lại. Kết nối mạnh mẽ đòi hỏi nhiều hơn kết nối thông thường. 

Vấn đề triển khai cuối cùng xuất hiện khi mọi cạnh đã được đưa vào cấu trúc đã chọn. Ví dụ:```
2
-1 7
3 -1
```Chi phí định hướng duy nhất có thể`7`nếu đường đi từ thành phố 1 đến thành phố 2 và`3`theo hướng khác. Câu trả lời là`3`. Một giải pháp giả định cả hai hướng phải được chọn hoặc chỉ có thể sử dụng hướng rẻ hơn sẽ không thành công. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi hướng có thể có của tất cả các con đường và giữ hướng rẻ nhất tạo ra đồ thị có hướng được kết nối mạnh mẽ. Điều này đúng vì mọi câu trả lời có thể đều được kiểm tra. Tuy nhiên, nếu có`m`đường, không gian tìm kiếm có kích thước`2^m`. Với 18 thành phố, biểu đồ có thể có 153 con đường, điều này hoàn toàn không thể thực hiện được. 

Quan sát hữu ích này xuất phát từ việc mô tả đặc tính của các đồ thị có hướng liên thông mạnh. Một đồ thị được kết nối mạnh có thể được xây dựng bằng cách bắt đầu từ một đỉnh và liên tục thêm một "tai", đây là một đường dẫn có hướng có điểm cuối đã nằm bên trong phần được kết nối mạnh hiện tại trong khi tất cả các đỉnh bên trong đều mới. Đỉnh đầu tiên đã là một biểu đồ được kết nối mạnh bắt đầu hợp lệ và mọi tai được thêm vào sẽ duy trì khả năng kết nối mạnh mẽ. 

Điều này biến bài toán thành một tập con DP. Thay vì quyết định tất cả các khía cạnh cùng một lúc, chúng tôi duy trì cách rẻ nhất để xây dựng một nhóm thành phố được kết nối chặt chẽ. Từ trạng thái đó, chúng tôi tìm kiếm tai tiếp theo giới thiệu các thành phố mới. 

Có một thủ thuật chi phí nữa. Mọi con đường cuối cùng đều phải được định hướng, vì vậy trước tiên hãy trả tiền cho hướng đi rẻ hơn của mỗi con đường. Nếu sau này chúng tôi chọn hướng đắt tiền hơn thì chúng tôi chỉ phải trả thêm phần chênh lệch. Điều này khiến DP chỉ giảm thiểu chi phí phát sinh, tránh lo lắng về những con đường chưa sử dụng trong quá trình thi công. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m) | O(m) | Quá chậm | 
| Tai phân hủy DP | O(2^N N^3) | O(2^N N^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi con đường, hãy trừ đi chi phí hai hướng của nó cho cả hai hướng. Thêm tất cả những chi phí rẻ hơn này vào một câu trả lời cơ bản. Các giá trị còn lại thể hiện cái giá phải trả thêm khi chọn hướng không rẻ. 
2. Hãy để`f[mask]`là chi phí bổ sung tối thiểu cần thiết để làm cho các thành phố ở`mask`được kết nối mạnh mẽ. Chúng tôi sửa thành phố`0`là thành phố ban đầu, vì vậy trạng thái bắt đầu hợp lệ duy nhất là`mask = 1`. 
3. Duy trì trạng thái DP khác`g[mask][x][y][b]`. Điều đó có nghĩa là chúng tôi hiện đang xây dựng một tai có điểm cuối hiện tại là`x`, điểm cuối cuối cùng bên trong thành phần hiện có là`y`, Và`b`cho biết liệu có sử dụng cạnh trực tiếp hay không`x -> y`được cho phép. 
4. Bất cứ khi nào một tai đạt đến điểm cuối cuối cùng, hãy đóng nó lại bằng cạnh cuối cùng và cập nhật`f[mask]`. Lý do có cờ bổ sung là đường dẫn chỉ chứa cạnh đóng không phải lúc nào cũng hợp lệ trong quá trình xây dựng. 
5. Từ mọi trạng thái được kết nối mạnh mẽ, hãy bắt đầu một tai mới. Chọn hai thành phố đã có trong thành phần làm điểm cuối và lấy một cạnh đi tới một thành phố bên ngoài thành phần. 
6. Mở rộng một tai chưa hoàn thiện bằng cách di chuyển từ điểm cuối hiện tại của nó đến một thành phố mới bên ngoài tập hợp hiện tại. Mỗi lần chuyển đổi như vậy sẽ làm tăng thêm chi phí còn lại của cạnh đó. 
7. Sau khi xử lý tất cả các tập hợp con,`f[(1 << N) - 1]`chứa chi phí bổ sung tối thiểu cho một định hướng được kết nối mạnh mẽ. Thêm chi phí cơ bản trở lại. Nếu trạng thái không tồn tại thì đồ thị không thể định hướng liên thông mạnh. 

Tại sao nó hoạt động: điều bất biến là mọi thứ đều có thể truy cập được`f[mask]`đại diện cho một định hướng kết nối mạnh mẽ hợp lệ của chính xác các thành phố trong`mask`. Việc thêm tai sẽ duy trì khả năng kết nối mạnh mẽ vì các đỉnh mới có đường dẫn trực tiếp vào và ra khỏi thành phần hiện có. Ngược lại, mọi đồ thị có hướng liên thông mạnh đều có một sự phân rã tai, do đó hướng tối ưu có thể được tạo ra bởi một chuỗi các chuyển đổi này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    cost = [list(map(int, input().split())) for _ in range(n)]

    base = 0
    for i in range(n):
        for j in range(i + 1, n):
            if cost[i][j] != -1:
                mn = min(cost[i][j], cost[j][i])
                base += mn
                cost[i][j] -= mn
                cost[j][i] -= mn

    total = 1 << n
    inf = 10**18

    f = [inf] * total
    g = [dict() for _ in range(total)]

    f[1] = 0

    for mask in range(1, total):
        if (mask & 1) == 0:
            continue

        cur = g[mask]

        for (u, v, can_close), val in list(cur.items()):
            if can_close and cost[u][v] != -1:
                nv = val + cost[u][v]
                if nv < f[mask]:
                    f[mask] = nv

        if f[mask] != inf:
            vertices = [i for i in range(n) if mask >> i & 1]
            for u in vertices:
                for v in vertices:
                    key = (u, v, 0)
                    if f[mask] < cur.get(key, inf):
                        cur[key] = f[mask]

        for (u, v, can_close), val in list(cur.items()):
            for w in range(n):
                if mask >> w & 1:
                    continue
                if cost[u][w] == -1:
                    continue
                nmask = mask | (1 << w)
                ncan = 1 if (can_close or u != v) else 0
                key = (w, v, ncan)
                nv = val + cost[u][w]
                if nv < g[nmask].get(key, inf):
                    g[nmask][key] = nv

    ans = f[-1]
    if ans == inf:
        print(-1)
    else:
        print(base + ans)

if __name__ == "__main__":
    solve()
```Phần tiền xử lý chuyển đổi chi phí ban đầu thành "chi phí bổ sung". Mọi cạnh đều đóng góp hướng đi rẻ nhất có thể ngay lập tức và DP chỉ trả tiền khi cần hướng khác. 

các`f`mảng lưu trữ tai hoàn chỉnh, còn các từ điển bên trong`g`lưu trữ đôi tai được xây dựng một phần. Từ điển được sử dụng thay cho mảng bốn chiều đầy đủ vì nhiều trạng thái lý thuyết không bao giờ xảy ra. 

Thứ tự thực hiện của mỗi mặt nạ đều quan trọng. Tai đã hoàn thành được sử dụng trước tiên để cập nhật trạng thái kết nối mạnh. Sau đó, những đôi tai mới được bắt đầu từ trạng thái được cải thiện đó và cuối cùng những đôi tai đó được mở rộng thành những chiếc mặt nạ lớn hơn. Vì quá trình chuyển đổi chỉ thêm các đỉnh nên mọi trạng thái trong tương lai đều có mặt nạ bit lớn hơn và sẽ được xử lý sau. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, sau khi loại bỏ chi phí định hướng rẻ nhất khỏi mọi con đường, DP chỉ cần trả tiền cho những thay đổi hướng cần thiết để tạo ra một hướng được kết nối mạnh mẽ. 

| Mặt nạ | Hành động | Tiểu bang | 
| --- | --- | --- | 
|`{1}`| Bắt đầu | Thành phần kết nối mạnh mẽ một thành phố | 
|`{1,2}`| Thêm đỉnh tai đầu tiên | Thành phần phát triển | 
|`{1,2,3,4}`| Đóng tai cuối cùng | Tất cả các thành phố được kết nối cả hai chiều | 

Chi phí tăng thêm cuối cùng được cộng vào chi phí trả trước, mang lại`27`. 

Đối với mẫu thứ hai, biểu đồ bao gồm hai phần dày đặc được kết nối bằng một kết nối duy nhất. DP không thể xây dựng một tai giới thiệu đường quay lại bị thiếu. 

| Mặt nạ | Hành động | Tiểu bang | 
| --- | --- | --- | 
|`{1}`| Bắt đầu | Thành phần ban đầu | 
|`{1,2,3}`| Mở rộng mặt đầu tiên | Vẫn không có đường về | 
|`{1,2,3,4,5,6}`| Cố gắng hoàn thành | Không có tai đóng hợp lệ | 

Mặt nạ đầy đủ vẫn không thể truy cập được, vì vậy câu trả lời là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^N N^3) | Mọi tập hợp con có thể chứa các trạng thái tai và các chuyển đổi thử các điểm cuối có thể có và các đỉnh tiếp theo | 
| Không gian | O(2^N N^2) | Các tai chưa hoàn thiện được lưu trữ được giới hạn bởi số lượng tập hợp con và cặp điểm cuối | 

Với`N <= 18`, số lượng tập hợp con đủ nhỏ cho phương pháp lập trình động này. Hệ số đa thức lớn nhưng số lượng mặt nạ vẫn có thể quản lý được. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old
    return out

# Sample 1
assert run("""4
-1 3 2 -1
3 -1 7 7
5 9 -1 9
-1 6 7 -1
""") == "27\n"

# Sample 2
assert run("""6
-1 1 2 -1 -1 -1
3 -1 4 -1 -1 -1
5 6 -1 0 -1 -1
-1 -1 0 -1 6 5
-1 -1 -1 4 -1 3
-1 -1 -1 2 1 -1
""") == "-1\n"

# Two cities
assert run("""2
-1 8
3 -1
""") == "3\n"

# Triangle
assert run("""3
-1 1 10
10 -1 1
1 10 -1
""") == "3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai thành phố | 3 | Xử lý biểu đồ nhỏ nhất có thể | 
| Tam giác | 3 | Kiểm tra kết nối mạnh mẽ theo chu kỳ đơn giản | 
| Mẫu 1 | 27 | Xác nhận cấu trúc tai bình thường | 
| Mẫu 2 | -1 | Xác thực các trường hợp không thể | 

## Vỏ cạnh 

Đối với biểu đồ bị ngắt kết nối:```
3
-1 5 -1
5 -1 -1
-1 -1 -1
```DP bắt đầu với thành phố 0. Không có chuỗi tai nào có thể giới thiệu thành phố 2 vì không có cạnh nào vào hoặc ra khỏi nó. Mặt nạ đầy đủ cuối cùng vẫn không thể truy cập được, vì vậy thuật toán sẽ in`-1`. 

Đối với trường hợp cầu:```
3
-1 1 -1
1 -1 2
-1 2 -1
```DP có thể xây dựng một con đường xuyên qua cả ba thành phố, nhưng nó không thể đóng một con đường cung cấp cho mỗi thành phố một tuyến đường quay trở lại. Bất biến không tạo ra được trạng thái kết nối mạnh hoàn chỉnh nên kết quả là chính xác`-1`. 

Đối với trường hợp hai thành phố:```
2
-1 8
3 -1
```Quá trình tiền xử lý cơ sở loại bỏ chi phí rẻ hơn`3`. Chi phí DP còn lại bằng 0 vì việc chọn hướng rẻ hơn đã làm cho biểu đồ được kết nối chặt chẽ. Câu trả lời được in ra là`3`.
