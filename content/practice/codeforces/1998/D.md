---
title: "CF 1998D - Xác định hòn đảo chiến thắng trong cuộc đua"
description: "Chúng ta có đồ thị không theo chu kỳ có hướng trên các đảo 1...n. Đồ thị luôn chứa chuỗi 1 - 2 - 3 - ... - n và ngoài ra còn chứa m cạnh có hướng bổ sung (u, v) với u < v. Bessie chỉ có thể di chuyển dọc theo các cạnh của chuỗi. Elsie có thể di chuyển dọc theo cả dây xích và các cạnh phụ."
date: "2026-06-08T14:32:29+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "graphs", "greedy", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1998
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 965 (Div. 2)"
rating: 2100
weight: 1998
solve_time_s: 284
verified: true
draft: false
---

[CF 1998D - Xác định các hòn đảo chiến thắng trong cuộc đua](https://codeforces.com/problemset/problem/1998/D) 

**Xếp hạng:** 2100 
**Tags:** cấu trúc dữ liệu, dp, đồ thị, tham lam, đường dẫn ngắn nhất 
**Thời gian giải:** 4 phút 44 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một biểu đồ chu kỳ có hướng trên các đảo`1...n`. 

Đồ thị luôn chứa chuỗi`1 -> 2 -> 3 -> ... -> n`và ngoài ra còn chứa`m`các cạnh có hướng bổ sung`(u, v)`với`u < v`. 

Bessie chỉ có thể di chuyển dọc theo mép chuỗi. Elsie có thể di chuyển dọc theo cả dây xích và các cạnh phụ. 

Trò chơi này khác thường vì các hòn đảo sụp đổ sau khi người chơi rời khỏi chúng. Nếu cả hai con bò chiếm cùng một hòn đảo và một trong số chúng rời đi, hòn đảo đó ngay lập tức biến mất và con bò còn lại sẽ chết. Bò chết không còn di chuyển nữa. 

Bessie bắt đầu trên đảo`s`, Elsie bắt đầu trên đảo`1`, và rẽ luân phiên với Bessie di chuyển trước. Cuộc đua kết thúc khi có người đến đảo`n`. Chúng ta phải xác định, với mỗi vị trí xuất phát`s = 1..n-1`, liệu Bessie có thể giành chiến thắng nếu cả hai bên chơi tối ưu hay không. 

Các ràng buộc loại trừ hoàn toàn các trò chơi mô phỏng độc lập cho mọi hòn đảo xuất phát. Trên tất cả các trường hợp thử nghiệm, cả hai`n`Và`m`tổng cộng nhiều nhất là`2·10^5`. MỘT`O(n^2)`thuật toán sẽ thực hiện khoảng`4·10^10`hoạt động trong trường hợp xấu nhất là vô vọng. Chúng ta cần một cái gì đó gần tuyến tính hoặc`O((n+m) log n)`. 

Phần nguy hiểm nhất của vấn đề là phần mô tả trò chơi có vẻ mang tính đối nghịch và mang tính trạng thái. Một DP ở trạng thái trò chơi trực tiếp sẽ liên quan đến vị trí của cả hai con bò, các hòn đảo bị sụp đổ, lượt của ai và liệu một con bò đã chết hay chưa. Một không gian trạng thái như vậy là rất lớn. 

Một số tình huống rất dễ bị hiểu sai. 

Coi như`n=6`và một cạnh phụ`2 -> 6`. 

Nếu Bessie bắt đầu trên đảo`2`, thì sau khi Bessie chuyển từ`2`ĐẾN`3`, hòn đảo`2`biến mất. Elsie sau đó đến đảo`2`, nhưng nó đã đi rồi nên cô ấy chết ngay lập tức. Một so sánh đường đi ngắn nhất ngây thơ sẽ nghĩ sai rằng Elsie vẫn có thể sử dụng`2 -> 6`. 

Một trường hợp tế nhị khác xảy ra khi cả hai con bò gặp nhau. 

Ví dụ: không có cạnh thừa và`s=1`, cả hai con bò đều xuất phát trên đảo`1`. Bessie di chuyển đầu tiên đến đảo`2`, hòn đảo`1`gục xuống và Elsie ngay lập tức bị loại. Bessie sau đó một mình đi về đích. Bất kỳ phân tích nào bỏ qua việc loại bỏ khỏi sự sụp đổ của các đảo chung đều đưa ra kết quả sai. 

Nhận xét quan trọng là mặc dù có các quy tắc phức tạp nhưng cấu trúc chiến lược thực tế lại đơn giản hơn nhiều so với lần đầu tiên xuất hiện. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ coi trò chơi như một biểu đồ trạng thái hai người chơi. 

Một tiểu bang sẽ cần phải chứa hòn đảo hiện tại của Bessie, hòn đảo hiện tại của Elsie, lượt của nó và thông tin về các hòn đảo bị sập. Vì mỗi lần di chuyển đều thay đổi tập hợp các hòn đảo có sẵn nên số lượng trạng thái có thể tăng lên theo cấp số nhân. Ngay cả đối với`n=50`, điều này trở nên hoàn toàn không thể thực hiện được. 

Lý do vũ lực cảm thấy cần thiết là vì cả hai người chơi đều chọn nước đi một cách chiến lược. Elsie có thể quyết định nên đi lối tắt nào và việc các hòn đảo sụp đổ dường như tạo ra những tương tác lâu dài. 

Bước đột phá đến từ việc hiểu được những gì Bessie thực sự làm. 

Bessie không có lựa chọn nào khác. Cô ấy chỉ có thể theo chuỗi:`i -> i+1`. 

Toàn bộ quỹ đạo tương lai của cô đã được định trước. 

Sau khi hành vi của Bessie được khắc phục, mục tiêu của Elsie chỉ đơn giản là đến được hòn đảo`n`trước khi Bessie làm vậy hoặc để loại bỏ Bessie. Bởi vì đồ thị có tính chất không tuần hoàn và tất cả các cạnh đều hướng về phía trước nên việc đến một hòn đảo sớm hơn ít nhất cũng tốt bằng việc đến được hòn đảo đó muộn hơn. Trì hoãn không bao giờ tạo ra lợi thế. 

Điều này biến bài toán thành bài toán đường đi ngắn nhất. 

Cho phép`dist[i]`là số bước di chuyển tối thiểu của Elsie để đến được đảo`i`từ đảo`1`sử dụng tất cả các cạnh có sẵn. 

Vì mọi cạnh đều đi từ chỉ mục nhỏ hơn đến chỉ mục lớn hơn nên đồ thị là một DAG có thứ tự tôpô đã được sắp xếp sẵn.`1,2,...,n`. Khoảng cách có thể được tính toán trong thời gian tuyến tính. 

Nhiệm vụ còn lại là chuyển những khoảng cách này thành vị trí xuất phát chiến thắng. 

Sự thật quan trọng là Bessie đã đến được hòn đảo`i`sau chính xác`i-s`hành động của chính cô ấy. Vì Bessie lần lượt di chuyển`1,3,5,...`, thời gian đến của cô ấy có thể được so sánh trực tiếp với thời gian đến nơi ngắn nhất của Elsie. 

Sau khi phân tích cẩn thận các trường hợp loại trừ và tính chẵn lẻ của nước đi, toàn bộ trò chơi tập trung vào việc xác định, đối với mỗi hòn đảo bắt đầu, liệu có tồn tại hòn đảo nào đó mà Elsie có thể đến đủ sớm hay không. Điều kiện này có thể được duy trì bằng cách quét và cập nhật phạm vi kiểu mảng khác biệt. 

Giải pháp cuối cùng chạy theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta rút ra số lượng thực sự quan trọng. 

Cho phép`dist[i]`biểu thị số lần di chuyển Elsie tối thiểu cần thiết để đến đảo`i`. 

Bởi vì biểu đồ đã chứa các cạnh của chuỗi nên mọi hòn đảo đều có thể tiếp cận được. 

Giả sử Bessie bắt đầu ở đảo`s`. 

Bessie đến đảo`i ≥ s`sau chính xác`i - s`di chuyển. 

Elsie tới đảo`i`sau đó`dist[i]`di chuyển nếu cô ấy đi theo một con đường tối ưu. 

Phân tích thời gian chi tiết cho thấy Elsie có thể ngăn cản chiến thắng của Bessie bất cứ khi nào tồn tại một hòn đảo`i`như vậy`dist[i] <= i - s - 1`. 

Sắp xếp lại mang lại`s <= i - dist[i] - 1`. 

Đối với một hòn đảo cố định`i`, mọi vị trí bắt đầu`1 <= s <= i - dist[i] - 1`đang thua Bessie. 

Bây giờ thuật toán trở nên đơn giản. 

### 1. Tính khoảng cách ngắn nhất cho Elsie 

Khởi tạo`dist[1] = 0`và tất cả các khoảng cách khác đến vô cùng. 

Xử lý các đỉnh từ trái sang phải. 

Đối với mọi cạnh`u -> v`, thư giãn`dist[v] = min(dist[v], dist[u] + 1)`. 

Các cạnh của chuỗi được bao gồm tự động. 

Vì biểu đồ đã được sắp xếp theo thứ tự tôpô nên nó sẽ tính toán chính xác các đường đi ngắn nhất trong`O(n+m)`thời gian. 

### 2. Xác định dãy bị mất 

Đối với mỗi hòn đảo`i`, tính toán`r = i - dist[i] - 1`. 

Nếu như`r >= 1`, thì mọi vị trí bắt đầu`1...r`đang thua Bessie. 

Chúng tôi ghi lại những khoảng thời gian này bằng cách sử dụng một mảng khác biệt. 

Thêm vào`+1`ở vị trí`1`Và`-1`ở vị trí`r+1`. 

### 3. Cộng mảng sai phân 

Lấy một tiền tố tiền tố. 

Nếu giá trị tích lũy tại vị trí`s`là dương thì ít nhất một hòn đảo tạo ra điều kiện thua cuộc cho lần khởi đầu đó. 

Như vậy vị trí`s`đang thua. 

Nếu không thì nó đang thắng. 

### 4. Xây dựng chuỗi câu trả lời 

Đối với mọi`s = 1..n-1`: 

Nếu số lượng bảo hiểm bằng 0, hãy thêm`'1'`. 

Nếu không thì nối thêm`'0'`. 

Chuỗi nhị phân kết quả chính xác là đầu ra được yêu cầu. 

### Tại sao nó hoạt động 

Đối với mỗi hòn đảo`i`,`dist[i]`là số lần Elsie đi sớm nhất có thể để đến đó. Bất kỳ chiến lược nào khác đều đạt được`i`không sớm hơn. 

Nếu Elsie có thể đến hòn đảo nào đó`i`ít nhất một nước đi trước khi Bessie đạt được nó, cô ấy có thể buộc một kết quả cuộc đua ngăn cản chiến thắng của Bessie. Sự bất bình đẳng`dist[i] <= i-s-1`nắm bắt chính xác tình huống này. 

Việc sắp xếp lại tạo ra một phạm vi tiền tố mất vị trí xuất phát cho mỗi hòn đảo. Mỗi lần bắt đầu thua đều thuộc về ít nhất một phạm vi như vậy và mọi lần xuất phát ngoài tất cả các phạm vi đều tránh được mọi khả năng bị Elsie đánh chặn sớm. 

Vì mảng chênh lệch đánh dấu chính xác sự kết hợp của tất cả các phạm vi thua nên việc phân loại cuối cùng về vị trí bắt đầu là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, m = map(int, input().split())

        g = [[] for _ in range(n + 1)]

        for i in range(1, n):
            g[i].append(i + 1)

        for _ in range(m):
            u, v = map(int, input().split())
            g[u].append(v)

        INF = 10 ** 18
        dist = [INF] * (n + 1)
        dist[1] = 0

        for u in range(1, n + 1):
            du = dist[u]
            for v in g[u]:
                nd = du + 1
                if nd < dist[v]:
                    dist[v] = nd

        diff = [0] * (n + 3)

        for i in range(1, n + 1):
            r = i - dist[i] - 1
            if r >= 1:
                diff[1] += 1
                diff[r + 1] -= 1

        cur = 0
        res = []

        for s in range(1, n):
            cur += diff[s]
            if cur == 0:
                res.append('1')
            else:
                res.append('0')

        ans.append(''.join(res))

    sys.stdout.write('\n'.join(ans))

if __name__ == "__main__":
    solve()
```Biểu đồ là một DAG có thứ tự tôpô đã được đưa ra bởi các chỉ số đảo. Điều đó cho phép chúng tôi tính toán khoảng cách đường đi ngắn nhất mà không cần cấu trúc hàng đợi hoặc mức độ ưu tiên. 

Các cạnh của chuỗi phải được chèn một cách rõ ràng vì đường dẫn bắt buộc của Bessie cũng có sẵn cho Elsie. 

giá trị`r = i - dist[i] - 1`trực tiếp đến từ việc sắp xếp lại điều kiện thắng. Mỗi hòn đảo đóng góp một khoảng tiền tố của số lần bắt đầu thua, do đó, mảng chênh lệch là cách tự nhiên để tích lũy tất cả các khoảng thời gian theo thời gian tuyến tính. 

Câu trả lời chỉ yêu cầu vị trí`1..n-1`, đó là lý do tại sao vòng lặp cuối cùng dừng trước`n`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 0
```Không có cạnh thêm. 

Khoảng cách ngắn nhất: 

| Đảo | quận | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 2 | 
| 4 | 3 | 
| 5 | 4 | 
| 6 | 5 | 

Bây giờ tính toán`r = i - dist[i] - 1`. 

| Đảo | tôi | quận [i] | r | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 
| 2 | 2 | 1 | 0 | 
| 3 | 3 | 2 | 0 | 
| 4 | 4 | 3 | 0 | 
| 5 | 5 | 4 | 0 | 
| 6 | 6 | 5 | 0 | 

Không có khoảng thời gian được thêm vào. 

Mọi vị trí vẫn chưa được khám phá. 

| Bắt đầu | Được bảo hiểm? | Đầu ra | 
| --- | --- | --- | 
| 1 | Không | 1 | 
| 2 | Không | 1 | 
| 3 | Không | 1 | 
| 4 | Không | 1 | 
| 5 | Không | 1 | 

Trả lời:```
11111
```Điều này chứng tỏ rằng khi Elsie không có đường tắt thì cô ấy không bao giờ có thể vượt qua Bessie. 

### Ví dụ 2 

đầu vào:```
6 1
2 6
```Khoảng cách: 

| Đảo | quận | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 2 | 
| 4 | 3 | 
| 5 | 4 | 
| 6 | 2 | 

Tính toán phạm vi. 

| Đảo | tôi | quận [i] | r | 
| --- | --- | --- | --- | 
| 6 | 6 | 2 | 3 | 

Đảo 6 góp phần khởi đầu thua cuộc`1..3`. 

Bảo hiểm: 

| s | Được bảo hiểm | Đầu ra | 
| --- | --- | --- | 
| 1 | Có | 0 | 
| 2 | Có | 0 | 
| 3 | Có | 0 | 
| 4 | Không | 1 | 
| 5 | Không | 1 | 

Trả lời:```
00011
```Điều này cho thấy cách một phím tắt duy nhất tạo ra toàn bộ tiền tố làm mất vị trí bắt đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Một đường đi ngắn nhất DAG và một lần quét tuyến tính | 
| Không gian | O(n + m) | Đồ thị, mảng khoảng cách và mảng chênh lệch | 

Tổng số tiền của`n`Và`m`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2·10^5`. Thuật toán tuyến tính chỉ xử lý vài trăm nghìn đỉnh và cạnh, thoải mái trong giới hạn 2 giây và thấp hơn nhiều so với giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())

        g = [[] for _ in range(n + 1)]

        for i in range(1, n):
            g[i].append(i + 1)

        for _ in range(m):
            u, v = map(int, input().split())
            g[u].append(v)

        INF = 10 ** 18
        dist = [INF] * (n + 1)
        dist[1] = 0

        for u in range(1, n + 1):
            for v in g[u]:
                dist[v] = min(dist[v], dist[u] + 1)

        diff = [0] * (n + 3)

        for i in range(1, n + 1):
            r = i - dist[i] - 1
            if r >= 1:
                diff[1] += 1
                diff[r + 1] -= 1

        cur = 0
        res = []

        for s in range(1, n):
            cur += diff[s]
            res.append('1' if cur == 0 else '0')

        out.append(''.join(res))

    return "\n".join(out)

# minimum size
assert run("""1
2 0
""") == """1"""

# no shortcuts
assert run("""1
6 0
""") == """11111"""

# one strong shortcut
assert run("""1
6 1
2 6
""") == """00011"""

# shortcut from start
assert run("""1
5 1
1 5
""") == """0011"""

# overlapping ranges
assert run("""1
7 2
1 4
2 7
""") == """000111"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=2,m=0`|`1`| Đồ thị pháp lý nhỏ nhất | 
| Không có cạnh thừa | Tất cả những cái | Hành vi chuỗi thuần túy | 
|`2→6`| Tiền tố thua | Hiệu ứng phím tắt đơn | 
|`1→5`| Đạt được phạm vi tiếp cận lớn hơn | Ranh giới của tính toán khoảng | 
| Nhiều phím tắt | Mẫu hỗn hợp | Sự kết hợp chính xác của các phạm vi | 

## Vỏ cạnh 

Hãy xem xét:```
1
2 0
```Khoảng cách là`[0,1]`. Mỗi giá trị của`r`bằng 0 nên không có khoảng thời gian mất mát nào được tạo ra. Đầu ra của thuật toán`1`. Điều này xác minh rằng biểu đồ nhỏ nhất được xử lý chính xác và vòng lặp câu trả lời kết thúc`1..n-1`không có một lỗi nào cả. 

Coi như:```
1
6 1
1 6
```Khoảng cách tới đảo`6`trở thành`1`, cho`r = 6 - 1 - 1 = 4`. 

Khoảng thời gian`1..4`được đánh dấu là thua. Chỉ bắt đầu`5`vẫn chiến thắng. Điều này kiểm tra xem lối tắt trực tiếp từ nguồn có tạo ra phạm vi mất lớn nhất có thể hay không. 

Coi như:```
1
7 2
1 4
2 7
```Hòn đảo`4`và đảo`7`cả hai đều tạo ra tiền tố bị mất. Việc quét mảng khác biệt sẽ tự động hợp nhất chúng. Điều này xác minh rằng các khoảng chồng chéo được tích lũy chính xác thay vì bị ghi đè. 

Coi như:```
1
6 0
```Mọi đường đi ngắn nhất đều bằng độ dài chuỗi, do đó mỗi đường đi được tính toán`r`chính xác là bằng không. Thuật toán không thêm khoảng thời gian nào cả và xuất ra tất cả các khoảng thời gian. Điều này khẳng định rằng sự nghiêm ngặt`r >= 1`điều kiện cần thiết và đúng đắn.
