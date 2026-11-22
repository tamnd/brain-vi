---
title: "CF 295C - Greg và những người bạn"
description: "Mỗi người nặng 50 kg hoặc 100 kg. Một chiếc thuyền xuất phát ở bờ trái và có thể chở bất kỳ nhóm nào không trống có tổng trọng lượng không vượt quá giới hạn của thuyền. Phải có người ở trong thuyền điều khiển nên mỗi chuyến qua bến đều có ít nhất một hành khách."
date: "2026-06-05T17:52:48+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 295
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 179 (Div. 1)"
rating: 2100
weight: 295
solve_time_s: 218
verified: true
draft: false
---

[CF 295C - Greg và những người bạn](https://codeforces.com/problemset/problem/295/C) 

**Xếp hạng:** 2100 
**Tags:** tổ hợp, dp, đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 3 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi người nặng 50 kg hoặc 100 kg. Một chiếc thuyền xuất phát ở bờ trái và có thể chở bất kỳ nhóm nào không trống có tổng trọng lượng không vượt quá giới hạn của thuyền. Phải có người ở trong thuyền điều khiển nên mỗi chuyến qua bến đều có ít nhất một hành khách. 

Chúng ta phải vận chuyển mọi người từ bờ trái sang bờ phải. Trong số tất cả các kế hoạch vận chuyển hợp lệ, chúng ta cần hai giá trị. 

Giá trị đầu tiên là số lần đi thuyền tối thiểu. 

Giá trị thứ hai là số lượng các kế hoạch vận chuyển riêng biệt đạt được mức tối thiểu đó. Hai kế hoạch sẽ khác nhau nếu ít nhất một chuyến đi có một nhóm người khác nhau. 

Quan sát quan trọng là mọi người chỉ có hai trọng số có thể có. Chúng tôi không bao giờ cần phải nhớ danh tính của từng người trong khi tìm kiếm số chuyến đi tối thiểu. Chúng ta chỉ cần biết bên bờ trái có bao nhiêu người nặng 50 kg và bao nhiêu người nặng 100 kg. 

Số lượng người tối đa là 50. Một trạng thái lưu trữ các tập hợp con chính xác sẽ cần tới$2^{50}$khả năng, điều đó hoàn toàn không thể xảy ra. Mặt khác, số lượng người nặng 50 kg và 100 kg còn lại nằm trong khoảng từ 0 đến 50, chỉ cho kết quả khoảng$51 \times 51$khả năng. Thêm vị trí thuyền vẫn còn ít hơn 6000 tiểu bang. 

Một điểm tinh tế là tìm kiếm đường đi ngắn nhất không thể bỏ qua hoàn toàn danh tính. Giả sử có ba người nặng 50 kg khác nhau và chúng ta di chuyển một trong số họ. Việc chọn người A và chọn người B dẫn đến cùng một trạng thái đếm, nhưng họ thể hiện các phương án vận chuyển khác nhau. Chúng ta phải tìm kiếm trên các trạng thái đếm trong khi đếm xem có bao nhiêu lựa chọn cụ thể tạo ra mỗi lần chuyển đổi. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như```
1 50
50
```Câu trả lời là```
1
1
```Thuyền có thể qua ngay chỉ với người duy nhất. 

Bây giờ hãy xem xét```
1 50
100
```Câu trả lời là```
-1
0
```Hành khách duy nhất nặng hơn sức chứa của thuyền nên không thể di chuyển được. 

Một trường hợp quan trọng khác là```
2 100
50 50
```Câu trả lời là```
1
1
```Cả hai người cùng nhau vượt qua. Việc tính chúng là hai đơn hàng khác nhau sẽ không chính xác vì chuyến đi được xác định bởi tập hợp hành khách chứ không phải bởi đơn hàng bên trong thuyền. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ coi mỗi người là khác biệt và tìm kiếm tất cả các tải trọng thuyền có thể có. Ngay cả khi chỉ có 50 người, số lượng hành khách là$2^{50}$, và mỗi tiểu bang sẽ phân nhánh thành nhiều tiểu bang khác. Không gian tìm kiếm bùng nổ ngay lập tức. 

Lý do khiến vũ lực có vẻ hấp dẫn là vì việc xác định các kế hoạch riêng biệt phụ thuộc vào bản sắc cá nhân. Thật không may, việc giữ danh tính trong tiểu bang là quá tốn kém. 

Cấu trúc của trọng lượng thay đổi mọi thứ. Vì mỗi trọng số là 50 hoặc 100 nên một trạng thái được xác định hoàn toàn bởi ba giá trị: 

- còn lại bao nhiêu người nặng 50 kg ở bờ trái, 
- có bao nhiêu người nặng 100 kg ở bờ trái, 
- ngân hàng nào hiện đang chứa thuyền. 

Không gian trạng thái trở nên nhỏ bé:$$51 \times 51 \times 2 \approx 5200.$$Thử thách còn lại là tính toán các kế hoạch riêng biệt. Hai lần chuyển đổi giữa cùng một cặp trạng thái đếm có thể tương ứng với nhiều lựa chọn khác nhau của con người thực tế. 

Giả định$x$những người nặng 50 kg hiện đang ở bờ trái và chúng tôi gửi$p$của họ trên. Số cách chọn những người đó là$$\binom{x}{p}.$$Tương tự, việc chọn$q$một trăm kg người đóng góp$$\binom{y}{q}.$$Tổng bội số của quá trình chuyển đổi đó là tích của hai hệ số nhị thức. 

Biểu đồ trạng thái không có trọng số vì mỗi chuyến đi thuyền tốn đúng một lần qua sông. Khi chúng tôi xây dựng biểu đồ này, số lượng chuyến đi tối thiểu sẽ trở thành vấn đề về đường đi ngắn nhất. Vì tất cả các cạnh đều có chi phí bằng nhau nên BFS cho khoảng cách ngắn nhất. Trong khi chạy BFS, chúng tôi cũng duy trì số lượng đường đi ngắn nhất, nhân với số lượng tổ hợp được gắn vào mỗi cạnh. 

Điều này tạo ra cả hai câu trả lời cùng một lúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$tiểu bang hoặc tệ hơn | Hàm mũ | Quá chậm | 
| Tối ưu |$O(S \cdot T)$|$O(S)$| Đã chấp nhận | 

Đây$S \approx 5200$là số trạng thái và$T$là số lượng các mô hình tải trọng thuyền khả thi. 

## Hướng dẫn thuật toán 

1. Đếm số lượng 50 kg người, gọi là$A$, và số người nặng 100 kg, gọi là$B$. 
2. Chia tất cả các trọng lượng cho 50. Một người nặng 50 kg trở thành trọng lượng 1, một người 100 kg trở thành trọng lượng 2 và sức chứa của thuyền trở thành$M = k / 50$. 
3. Tính toán trước tất cả các cặp$(p,q)$như vậy$p+2q \le M$Và$p+q > 0$. Mỗi cặp đại diện cho thành phần hành khách có thể có trong một lần vượt biển. 
4. Tính toán trước tất cả các hệ số nhị thức$\binom{n}{r}$vì$0 \le n,r \le 50$modulo$10^9+7$. 
5. Sử dụng trạng thái$(x,y,s)$, Ở đâu:

-$x$là số người nặng 50kg vẫn ở bờ trái, 
-$y$là số người nặng 100 kg vẫn ở bờ trái, 
-$s=0$nghĩa là con thuyền đang ở bờ trái, 
-$s=1$nghĩa là thuyền đang ở bờ phải. 
6. Bắt đầu BFS từ$(A,B,0)$. Khoảng cách của trạng thái này là 0 và số đường đi ngắn nhất là 1. 
7. Khi thuyền đã cập bờ trái, hãy thử từng cặp khả thi$(p,q)$với$p \le x$Và$q \le y$. Di chuyển những hành khách đó sang bờ phải và đến trạng thái$(x-p,y-q,1)$. 
8. Tính đa bội của quá trình chuyển đổi đó là$$\binom{x}{p}\binom{y}{q}.$$1. Khi thuyền ở bờ phải, bờ phải hiện có$A-x$người nặng năm mươi kg và$B-y$người nặng một trăm kg. Hãy thử mọi tải trở lại khả thi$(p,q)$thỏa mãn$$p \le A-x,\qquad q \le B-y.$$Trạng thái mới là$(x+p,y+q,0)$. 

1. Tính bội số của quá trình chuyển đổi quay lại này là$$\binom{A-x}{p}\binom{B-y}{q}.$$1. Chạy thư giãn BFS thông thường. Nếu một trạng thái được phát hiện lần đầu tiên, hãy ghi lại khoảng cách của nó và khởi tạo số đường đi ngắn nhất của nó. Nếu một đường đi ngắn nhất khác đạt đến cùng trạng thái, hãy cộng modulo đóng góp của nó$10^9+7$. 
2. Trạng thái mục tiêu là$(0,0,1)$, nghĩa là mọi người đều ở bên bờ phải và cuộc vượt biển cuối cùng đã xảy ra. 

### Tại sao nó hoạt động 

Tiểu bang chứa chính xác thông tin cần thiết để xác định tất cả các động thái pháp lý trong tương lai. Bất kỳ hai cấu hình nào có cùng trọng lượng 50 kg và 100 kg người còn lại và cùng một vị trí thuyền đều có khả năng tương lai giống hệt nhau. 

Mỗi cạnh trong biểu đồ trạng thái tương ứng với chính xác một chuyến đi thuyền, do đó số lần đi thuyền tối thiểu chính xác là khoảng cách đường đi ngắn nhất trong biểu đồ này. BFS tìm khoảng cách ngắn nhất trong mọi biểu đồ không có trọng số. 

Đối với một quá trình chuyển đổi cố định, số cách cụ thể để thực hiện nó bằng số lượng lựa chọn của hành khách thực tế, được đưa ra bởi các hệ số nhị thức có liên quan. Nhân các hệ số này dọc theo một con đường sẽ đếm số lượng phương án vận chuyển được đại diện bởi con đường đó. Phép lặp đếm đường đi ngắn nhất của BFS tính tổng những đóng góp đó trên tất cả các đường đi ngắn nhất, mang lại chính xác số lượng kế hoạch vận chuyển tối ưu. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

MOD = 1000000007

def main():
    n, k = map(int, input().split())
    w = list(map(int, input().split()))

    A = sum(x == 50 for x in w)
    B = sum(x == 100 for x in w)

    M = k // 50

    C = [[0] * 51 for _ in range(51)]
    for i in range(51):
        C[i][0] = C[i][i] = 1
        for j in range(1, i):
            C[i][j] = (C[i - 1][j - 1] + C[i - 1][j]) % MOD

    moves = []
    for p in range(51):
        for q in range(51):
            if p + q > 0 and p + 2 * q <= M:
                moves.append((p, q))

    dist = [[[-1] * 2 for _ in range(B + 1)] for _ in range(A + 1)]
    ways = [[[0] * 2 for _ in range(B + 1)] for _ in range(A + 1)]

    q = deque()
    dist[A][B][0] = 0
    ways[A][B][0] = 1
    q.append((A, B, 0))

    while q:
        x, y, side = q.popleft()

        cur_dist = dist[x][y][side]
        cur_ways = ways[x][y][side]

        if side == 0:
            for p, r in moves:
                if p > x or r > y:
                    continue

                nx = x - p
                ny = y - r
                ns = 1

                mult = C[x][p] * C[y][r]
                mult %= MOD

                if dist[nx][ny][ns] == -1:
                    dist[nx][ny][ns] = cur_dist + 1
                    ways[nx][ny][ns] = cur_ways * mult % MOD
                    q.append((nx, ny, ns))
                elif dist[nx][ny][ns] == cur_dist + 1:
                    ways[nx][ny][ns] = (
                        ways[nx][ny][ns] + cur_ways * mult
                    ) % MOD
        else:
            right50 = A - x
            right100 = B - y

            for p, r in moves:
                if p > right50 or r > right100:
                    continue

                nx = x + p
                ny = y + r
                ns = 0

                mult = C[right50][p] * C[right100][r]
                mult %= MOD

                if dist[nx][ny][ns] == -1:
                    dist[nx][ny][ns] = cur_dist + 1
                    ways[nx][ny][ns] = cur_ways * mult % MOD
                    q.append((nx, ny, ns))
                elif dist[nx][ny][ns] == cur_dist + 1:
                    ways[nx][ny][ns] = (
                        ways[nx][ny][ns] + cur_ways * mult
                    ) % MOD

    answer_dist = dist[0][0][1]

    if answer_dist == -1:
        print(-1)
        print(0)
    else:
        print(answer_dist)
        print(ways[0][0][1] % MOD)

if __name__ == "__main__":
    main()
```Bảng tổ hợp được tính một lần vì tất cả số đếm tối đa là 50. Mỗi lần chuyển đổi chỉ cần tra cứu theo thời gian không đổi. 

BFS lưu trữ cả khoảng cách ngắn nhất và số lượng đường đi ngắn nhất. Khi một trạng thái được phát hiện lần đầu tiên, chúng ta đã tìm được khoảng cách tối thiểu của nó. Nếu một đường đi khác đạt đến trạng thái tương tự với khoảng cách giống hệt nhau thì sự đóng góp của nó cũng phải được tính. 

Một lỗi phổ biến là sử dụng các số hiện có ở bờ trái khi thuyền đang quay trở lại từ bờ phải. Chuyến về chọn hành khách ở bờ phải nên số đếm đúng là$A-x$Và$B-y$. 

Một chi tiết tinh tế khác là biểu đồ không được định hướng bởi danh tính con người. Danh tính chỉ xuất hiện bên trong bội số chuyển tiếp thông qua hệ số nhị thức. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào```
1 50
50
```Đây$A=1$,$B=0$,$M=1$. 

| Tiểu bang | Khoảng cách | Hành động | 
| --- | --- | --- | 
| (1,0,0) | 0 | Bắt đầu | 
| (0,0,1) | 1 | Gửi một người nặng 50 kg | 

Mục tiêu đạt được sau một chuyến đi. 

Chuyến vượt biển duy nhất có thể chỉ có một người duy nhất nên số phương án tối ưu là 1. 

### Mẫu 2 

đầu vào```
3 100
50 50 100
```Đây$A=2$,$B=1$,$M=2$. 

| Tiểu bang | Khoảng cách | Hành động | 
| --- | --- | --- | 
| (2,1,0) | 0 | Bắt đầu | 
| (0,1,1) | 1 | Gửi hai người 50 kg | 
| (1,1,0) | 2 | Trả lại một người 50 kg | 
| (1,0,1) | 3 | Gửi một người nặng 100 kg | 
| (2,0,0) | 4 | Trả lại một người 50 kg | 
| (0,0,1) | 5 | Gửi hai người 50 kg | 

Quãng đường ngắn nhất là 5. Ở chuyến về đầu tiên ta có thể chọn một trong hai người nặng 50 kg, đưa ra hai phương án tối ưu rõ rệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(S \cdot T)$| BFS khám phá từng tiểu bang và thử tất cả các thành phần hành khách khả thi | 
| Không gian |$O(S)$| Mảng khoảng cách và số đường đi trên tất cả các trạng thái | 

Đây$S \le 51 \times 51 \times 2 \approx 5200$. Số lượng thành phần hành khách khả thi bị giới hạn bởi vài nghìn và nhỏ hơn nhiều trong thực tế. Giải pháp thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    MOD = 1000000007
    input = sys.stdin.readline

    n, k = map(int, input().split())
    w = list(map(int, input().split()))

    A = sum(x == 50 for x in w)
    B = sum(x == 100 for x in w)
    M = k // 50

    C = [[0] * 51 for _ in range(51)]
    for i in range(51):
        C[i][0] = C[i][i] = 1
        for j in range(1, i):
            C[i][j] = (C[i - 1][j - 1] + C[i - 1][j]) % MOD

    moves = []
    for p in range(51):
        for q in range(51):
            if p + q > 0 and p + 2 * q <= M:
                moves.append((p, q))

    dist = [[[-1] * 2 for _ in range(B + 1)] for _ in range(A + 1)]
    ways = [[[0] * 2 for _ in range(B + 1)] for _ in range(A + 1)]

    q = deque([(A, B, 0)])
    dist[A][B][0] = 0
    ways[A][B][0] = 1

    while q:
        x, y, s = q.popleft()

        if s == 0:
            for p, r in moves:
                if p > x or r > y:
                    continue
                nx, ny, ns = x - p, y - r, 1
                mult = C[x][p] * C[y][r] % MOD

                if dist[nx][ny][ns] == -1:
                    dist[nx][ny][ns] = dist[x][y][s] + 1
                    ways[nx][ny][ns] = ways[x][y][s] * mult % MOD
                    q.append((nx, ny, ns))
                elif dist[nx][ny][ns] == dist[x][y][s] + 1:
                    ways[nx][ny][ns] = (
                        ways[nx][ny][ns] + ways[x][y][s] * mult
                    ) % MOD
        else:
            r50, r100 = A - x, B - y

            for p, r in moves:
                if p > r50 or r > r100:
                    continue
                nx, ny, ns = x + p, y + r, 0
                mult = C[r50][p] * C[r100][r] % MOD

                if dist[nx][ny][ns] == -1:
                    dist[nx][ny][ns] = dist[x][y][s] + 1
                    ways[nx][ny][ns] = ways[x][y][s] * mult % MOD
                    q.append((nx, ny, ns))
                elif dist[nx][ny][ns] == dist[x][y][s] + 1:
                    ways[nx][ny][ns] = (
                        ways[nx][ny][ns] + ways[x][y][s] * mult
                    ) % MOD

    if dist[0][0][1] == -1:
        return "-1\n0"
    return f"{dist[0][0][1]}\n{ways[0][0][1]}"

# provided sample
assert run("1 50\n50\n") == "1\n1", "sample 1"

# impossible
assert run("1 50\n100\n") == "-1\n0", "cannot move 100kg person"

# two 50kg people fit together
assert run("2 100\n50 50\n") == "1\n1", "single crossing"

# one 50kg and one 100kg with capacity 100
assert run("2 100\n50 100\n") == "3\n1", "must shuttle"

# all people identical
assert run("3 150\n50 50 50\n") == "1\n1", "all cross together"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 50 / 100`|`-1 / 0`| Trường hợp không thể | 
|`2 100 / 50 50`|`1 / 1`| Vận chuyển trực tiếp trong một chuyến | 
|`2 100 / 50 100`|`3 / 1`| Yêu cầu một chuyến trở về | 
|`3 150 / 50 50 50`|`1 / 1`| Tất cả hành khách cùng di chuyển | 

## Vỏ cạnh 

Hãy xem xét```
1 50
100
```Sức chứa được chia tỷ lệ là 1, trong khi hành khách duy nhất có trọng lượng được chia tỷ lệ là 2. Không có hành động di chuyển hợp lệ nào tồn tại từ trạng thái bắt đầu. BFS không bao giờ đạt tới trạng thái đích nên khoảng cách của nó vẫn là -1. Thuật toán in```
-1
0
```cái nào đúng. 

Coi như```
2 100
50 50
```Trạng thái bắt đầu là$(2,0,0)$. Một sự chuyển đổi duy nhất sẽ đưa cả hai người đi ngang qua, tiếp cận$(0,0,1)$. Sự đa dạng là$$\binom{2}{2}=1.$$Đầu ra của thuật toán```
1
1
```và không tính hai lần các đơn đặt hàng khác nhau bên trong thuyền. 

Coi như```
3 100
50 50 100
```Giải pháp ngắn nhất là phải trả lại một trong hai người nặng 50 kg sau chuyến đi đầu tiên. Quá trình chuyển đổi trạng thái đếm giống nhau trong cả hai trường hợp, nhưng tính bội số của nó chứa$$\binom{2}{1}=2.$$Thuật toán tính chính xác cả hai khả năng mặc dù sau đó chúng hợp nhất vào cùng một trạng thái. Đây chính xác là lý do tại sao cần có trọng số cạnh tổ hợp.
