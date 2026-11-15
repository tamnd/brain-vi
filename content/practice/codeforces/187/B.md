---
title: "CF 187B - AlgoRace"
description: "Chúng ta có một đồ thị có hướng đầy đủ về n thành phố. Đối với mỗi loại ô tô, chúng tôi biết thời gian di chuyển giữa mỗi cặp thành phố theo thứ tự. Những thời gian di chuyển này không được đảm bảo là đối xứng, vì vậy đi từ u đến v có thể tốn kém hơn so với đi từ v đến u."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 187
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 119 (Div. 1)"
rating: 1800
weight: 187
solve_time_s: 125
verified: true
draft: false
---

[CF 187B - AlgoRace](https://codeforces.com/problemset/problem/187/B) 

**Đánh giá:** 1800 
**Tas:** dp, đường đi ngắn nhất 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có hướng đầy đủ về`n`các thành phố. Đối với mỗi loại ô tô, chúng tôi biết thời gian di chuyển giữa mỗi cặp thành phố theo thứ tự. Những thời gian di chuyển này không được đảm bảo là đối xứng, vì vậy việc đi từ`u`ĐẾN`v`có thể có giá khác với việc đi từ`v`ĐẾN`u`. 

Một vòng đua có ba giá trị: thành phố xuất phát, thành phố đích và số lần thay đổi xe tối đa được phép. Nếu chúng ta sử dụng một ô tô cho toàn bộ tuyến đường, điều đó có nghĩa là không có thay đổi nào. Nếu chúng ta sử dụng ba chiếc xe khác nhau trong chuyến đi, điều đó có nghĩa là có hai sự thay đổi. 

Người lái xe có thể di chuyển qua các thành phố trung gian và có thể chuyển xe ngay lập tức tại bất kỳ thành phố nào. Nhiệm vụ của mỗi truy vấn là tính toán thời gian di chuyển tối thiểu có thể với số lần đổi xe được phép. 

Các ràng buộc xác định hoàn toàn hình dạng giải pháp dự định. Số lượng thành phố và loại ô tô đều nhiều nhất là 60, đủ nhỏ để lập trình động khối hoặc tiền xử lý kiểu Floyd-Warshall. Giá trị nguy hiểm là số vòng, lên tới`10^5`. Điều đó loại trừ mọi thuật toán biểu đồ cho mỗi truy vấn. Thậm chí một`O(n^3)`tính toán cho mỗi truy vấn sẽ là vô vọng. 

Số lượng thay đổi tối đa được phép là 1000, lớn hơn nhiều so với số lượng thành phố. Điều đó gợi ý rõ ràng rằng chúng ta nên xử lý trước các câu trả lời cho tất cả các lần chuyển đổi có thể có đến một giới hạn có ý nghĩa nào đó. 

Một điểm tinh tế là việc sử dụng nhiều thay đổi được phép hơn không có nghĩa là chúng ta phải thực sự sử dụng chúng. Câu trả lời cho`k = 10`vẫn chỉ có thể sử dụng một chiếc xe nếu điều đó là tối ưu. 

Một sai lầm dễ mắc phải khác là hiểu sai ý nghĩa của việc "đổi xe". 

Hãy xem xét ví dụ này:```
2 2 1
0 5
5 0
0 1
1 0
1 2 0
```Nếu không có sự thay đổi nào, ban đầu chúng ta vẫn có thể chọn một trong hai ô tô. Câu trả lời đúng là`1`, không`5`, bởi vì chúng ta có thể bắt đầu đơn giản với chiếc xe thứ hai. 

Cạm bẫy thứ hai là giả sử đường đi ngắn nhất cho một ô tô chỉ sử dụng một cạnh. Dữ liệu đầu vào cung cấp chi phí đường bộ trực tiếp, nhưng biểu đồ đã hoàn chỉnh nên các thành phố trung gian vẫn có thể trợ giúp. 

Ví dụ:```
3 1 1
0 100 1
1 0 1
1 1 0
1 2 0
```Đi ô tô duy nhất, đường thẳng từ 1 đến 2 tốn 100, nhưng đi`1 -> 3 -> 2`chi phí 2. Bất kỳ giải pháp nào bỏ qua các đường đi ngắn nhất của tất cả các cặp bên trong một ô tô đều cho kết quả sai. 

Một trường hợp không rõ ràng khác là khi cho phép thay đổi thêm cũng không mang lại cải thiện gì.```
3 2 1
0 1 10
1 0 1
10 1 0
0 5 5
5 0 5
5 5 0
1 3 5
```Tuyến đường tối ưu chỉ sử dụng chiếc xe đầu tiên trong suốt thời gian đó. Một DP bất cẩn ép buộc chính xác`k`thay đổi thay vì "nhiều nhất`k`những thay đổi" sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Quan điểm vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi có thể nghĩ về tất cả các chuỗi ô tô có thể có và tất cả các thành phố trung gian có thể có nơi chúng tôi chuyển đổi. Nếu một truy vấn cho phép`k`thay đổi thì chúng tôi có thể sử dụng tối đa`k + 1`phân khúc ô tô. Mỗi phân đoạn có thể sử dụng bất kỳ`m`ô tô và việc chuyển đổi có thể xảy ra ở bất kỳ thành phố nào. Ngay cả đối với nhỏ`k`, số lượng kết hợp bùng nổ. 

Một lực lượng vũ phu thiên về đồ thị hơn sẽ xây dựng một biểu đồ trạng thái phân lớp trong đó trạng thái`(city, used_changes, current_car)`và chạy Dijkstra cho mọi truy vấn. Đồ thị có khoảng`n * m * k`tiểu bang. Với`r = 10^5`, cái này vẫn còn quá đắt. 

Quan sát quan trọng là số lượng thành phố rất nhỏ. Từ`n <= 60`, chúng ta có thể đủ khả năng xử lý trước đắt tiền trên tất cả các cặp thành phố. 

Sự giảm thiểu quan trọng đầu tiên là tách chuyển động bên trong một ô tô khỏi việc chuyển đổi ô tô. Nếu chúng ta cam kết sử dụng một ô tô cố định thì cách tốt nhất để đi lại giữa hai thành phố chỉ đơn giản là tìm đường đi ngắn nhất cho ô tô đó. Khi chúng tôi tính toán điều đó, mỗi chiếc ô tô sẽ hoạt động giống như một biểu đồ có trọng số hoàn chỉnh về thời gian di chuyển tối ưu. 

Sau quá trình tiền xử lý này, giả sử chúng ta xác định:`best[u][v] = minimum travel time from u to v using exactly one car`Điều này đã kết hợp tất cả các lựa chọn về ô tô và tất cả các thành phố trung gian trong khi vẫn giữ cố định chiếc ô tô đó. 

Bây giờ vấn đề trở nên đơn giản hơn nhiều. Nếu chúng ta được phép đổi xe thì chuyến đi chỉ là một chuỗi các đoạn đường, trong đó mọi chi phí của đoạn đường đều xuất phát từ đó`best`. 

Nếu chúng ta sử dụng nhiều nhất`k`thay đổi thì chúng ta chỉ sử dụng tối đa`k + 1`phân đoạn. 

Điều này dẫn đến việc lập trình động theo số lượng phân đoạn một cách tự nhiên. Cho phép:`dp[t][u][v] = minimum cost from u to v using at most t segments`Quá trình chuyển đổi là phép nhân ma trận cộng min-cộng cổ điển:```
dp[t][u][v] = min(dp[t-1][u][x] + best[x][v])
```Từ`n`chỉ là 60, chúng tôi có thể xử lý trước DP này cho tất cả`t`từ 1 đến 60. Chúng tôi không bao giờ cần nhiều hơn 60 phân đoạn vì việc thăm lại các thành phố không thể cải thiện đường đi tối ưu theo trọng số dương. Các giải pháp chính thức giới hạn số lượng thay đổi tại`n - 1`. 

Sau đó, mọi truy vấn sẽ trở thành một`O(1)`tra cứu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên mỗi tìm kiếm biểu đồ truy vấn | Quá lớn, đại khái`O(r * n^3 * m * k)`| Lớn | Quá chậm | 
| Floyd-Warshall + DP qua các phân đoạn |`O(m * n^3 + n^4)`|`O(n^3)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các ma trận ô tô. 

Mỗi ma trận mô tả thời gian di chuyển của một ô tô cụ thể giữa mỗi cặp thành phố theo thứ tự. 
2. Chạy Floyd-Warshall độc lập cho mọi xe. 

Điều này tính toán thời gian di chuyển tối thiểu thực sự giữa mỗi cặp thành phố trong khi vẫn sử dụng cùng một chiếc xe trong suốt chuyến đi. 
3. Xây dựng ma trận`best`. 

Đối với mỗi cặp`(u, v)`, bộ:```
best[u][v] = minimum over all cars of dist_car[u][v]
```Điều này thể hiện phân khúc rẻ nhất có thể nếu chúng ta được phép chọn chính xác một chiếc xe cho phân khúc đó. 
4. Khởi tạo bảng DP. 

Cho phép:```
dp[0][u][v] = best[u][v]
```Đây,`0`có nghĩa là không có xe nào thay đổi nên chỉ được phép có một đoạn đường. 
5. Gia hạn DP cho những thay đổi bổ sung về xe. 

Đối với mọi`t >= 1`:```
dp[t][u][v] = min(
    dp[t-1][u][v],
    dp[t-1][u][x] + best[x][v]
)
```trên tất cả các thành phố trung gian`x`. 

Tùy chọn đầu tiên có nghĩa là chúng tôi không sử dụng thay đổi bổ sung được phép. Tùy chọn thứ hai có nghĩa là chặng cuối cùng bắt đầu tại thành phố`x`sử dụng một chiếc ô tô. 
6. Chỉ tính toán trước DP tối đa`n - 1`. 

Bất kỳ tuyến đường tối ưu nào cũng có thể được chuyển đổi thành một tuyến đường không có thành phố lặp lại, do đó hơn`n - 1`những thay đổi là không cần thiết. 
7. Trả lời từng câu hỏi. 

Nếu truy vấn cho phép`k`thay đổi, sử dụng:```
dp[min(k, n - 1)][s][t]
```### Tại sao nó hoạt động 

Sau Floyd-Warshall, mọi giá trị`best[u][v]`đã đại diện cho cách tối ưu để đi lại từ`u`ĐẾN`v`trong khi sử dụng đúng một chiếc xe. Bất kỳ chiến lược đua hợp lệ nào cũng chỉ đơn giản là sự kết hợp của các phân đoạn đó. 

Bất biến DP là:```
dp[t][u][v]
```lưu trữ thời gian di chuyển tối thiểu từ`u`ĐẾN`v`sử dụng nhiều nhất`t + 1`các phân đoạn, tương đương nhiều nhất`t`thay đổi xe hơi. 

Quá trình chuyển đổi xem xét tất cả các thành phố chuyển đổi cuối cùng có thể`x`. Nếu đoạn cuối cùng bắt đầu lúc`x`, thì phần trước của tuyến đường sử dụng tối đa`t`phân khúc và phân khúc cuối cùng sử dụng một phân khúc nữa. Vì mọi điểm phân chia có thể đều được kiểm tra nên chiến lược tối ưu luôn được đưa vào. 

Bởi vì tất cả các chi phí đều không âm nên các thành phố lặp lại không thể cải thiện câu trả lời một cách vô thời hạn. Một tuyến đường được phân đoạn tối ưu luôn có thể được đơn giản hóa để sử dụng tối đa`n`phân đoạn, vì vậy giới hạn tiền xử lý ở`n - 1`thay đổi là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

def solve():
    n, m, r = map(int, input().split())

    cars = []

    for _ in range(m):
        dist = [list(map(int, input().split())) for _ in range(n)]

        # Floyd-Warshall for this car
        for k in range(n):
            for i in range(n):
                dik = dist[i][k]
                for j in range(n):
                    nd = dik + dist[k][j]
                    if nd < dist[i][j]:
                        dist[i][j] = nd

        cars.append(dist)

    # best one-car segment
    best = [[INF] * n for _ in range(n)]

    for i in range(n):
        for j in range(n):
            val = INF
            for c in range(m):
                if cars[c][i][j] < val:
                    val = cars[c][i][j]
            best[i][j] = val

    limit = n - 1

    # dp[t][i][j] = answer with at most t changes
    dp = [[[INF] * n for _ in range(n)] for _ in range(limit + 1)]

    for i in range(n):
        for j in range(n):
            dp[0][i][j] = best[i][j]

    for t in range(1, limit + 1):
        # start from previous layer because we may use fewer changes
        for i in range(n):
            for j in range(n):
                dp[t][i][j] = dp[t - 1][i][j]

        for i in range(n):
            for k in range(n):
                left = dp[t - 1][i][k]
                for j in range(n):
                    nd = left + best[k][j]
                    if nd < dp[t][i][j]:
                        dp[t][i][j] = nd

    out = []

    for _ in range(r):
        s, t, k = map(int, input().split())
        s -= 1
        t -= 1

        k = min(k, limit)

        out.append(str(dp[k][s][t]))

    print("\n".join(out))

solve()
```Khối chính đầu tiên chạy Floyd-Warshall riêng cho từng xe. Điều này rất cần thiết vì một chiếc ô tô vẫn có thể được hưởng lợi khi đi qua các thành phố trung gian. 

các`best`ma trận nén tất cả các ô tô vào một biểu đồ chuyển tiếp phổ quát. Sau bước này, mọi cạnh đều có nghĩa là "chuyển động tốt nhất có thể bằng cách sử dụng một lựa chọn xe không bị gián đoạn". 

Các lớp DP tương ứng với số lần thay đổi ô tô được phép. Một chi tiết tinh tế là sao chép`dp[t - 1]`vào trong`dp[t]`trước những chuyển tiếp. Nếu không có điều này, DP sẽ tính toán câu trả lời bằng cách sử dụng chính xác`t`thay đổi thay vì nhiều nhất`t`những thay đổi. 

Một lựa chọn triển khai quan trọng khác là giới hạn các truy vấn bằng:```
k = min(k, n - 1)
```Vấn đề cho phép`k`lên tới 1000, nhưng việc xử lý trước chỉ cần`n - 1`. 

Tất cả các tính toán đều phù hợp một cách an toàn với số nguyên 64 bit. Số nguyên Python xử lý việc này một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2 3
0 1 5 6
2 0 3 6
1 3 0 1
6 6 7 0
0 3 5 6
2 0 1 6
1 3 0 2
6 6 7 0
1 4 2
1 4 1
1 4 3
```Sau Floyd-Warshall, những giá trị quan trọng trong`best`trở thành: 

| Từ | Đến | Chi phí tốt nhất | 
| --- | --- | --- | 
| 1 | 2 | 1 | 
| 2 | 3 | 1 | 
| 3 | 4 | 1 | 

Bây giờ tính toán các lớp DP. 

| Được phép thay đổi | Đường đi tốt nhất 1 → 4 | Chi phí | 
| --- | --- | --- | 
| 0 | Trực tiếp với một chiếc xe | 5 | 
| 1 | 1 → 2 → 3 → 4 sử dụng hai đoạn | 4 | 
| 2 | 1 → 2, 2 → 3, 3 → 4 | 3 | 

Câu trả lời là:```
3
4
3
```Dấu vết này cho thấy tại sao việc cho phép nhiều thay đổi lại có ích. Mỗi đoạn bổ sung cho phép chúng tôi chuyển sang một chiếc xe đặc biệt mạnh mẽ trên một đoạn đường. 

### Ví dụ tùy chỉnh```
3 1 2
0 100 1
1 0 1
1 1 0
1 2 0
1 2 5
```Sau Floyd-Warshall cho chiếc xe duy nhất: 

| Từ | Đến | Chi phí ngắn nhất | 
| --- | --- | --- | 
| 1 | 2 | 2 | 
| 1 | 3 | 1 | 
| 3 | 2 | 1 | 

Bây giờ: 

| Truy vấn | Thay đổi được phép | Trả lời | 
| --- | --- | --- | 
| 1 → 2 | 0 | 2 | 
| 1 → 2 | 5 | 2 | 

Mặc dù chi phí đường bộ trực tiếp là 100, Floyd-Warshall đã phát hiện chính xác tuyến đường rẻ hơn qua thành phố 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(m * n^3 + n^4 + r)`| Floyd-Warshall cho từng ô tô, sau đó chuyển tiếp DP, sau đó truy vấn thời gian không đổi | 
| Không gian |`O(n^3)`| Lớp DP cộng với ma trận | 

Với`n <= 60`, thuật ngữ chiếm ưu thế là đại khái`60^4 = 12,960,000`hoạt động hoàn toàn an toàn trong Python. Việc sử dụng bộ nhớ cũng ở mức thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

INF = 10**18

def solve():
    input = sys.stdin.readline

    n, m, r = map(int, input().split())

    cars = []

    for _ in range(m):
        dist = [list(map(int, input().split())) for _ in range(n)]

        for k in range(n):
            for i in range(n):
                dik = dist[i][k]
                for j in range(n):
                    nd = dik + dist[k][j]
                    if nd < dist[i][j]:
                        dist[i][j] = nd

        cars.append(dist)

    best = [[INF] * n for _ in range(n)]

    for i in range(n):
        for j in range(n):
            best[i][j] = min(cars[c][i][j] for c in range(m))

    limit = n - 1

    dp = [[[INF] * n for _ in range(n)] for _ in range(limit + 1)]

    for i in range(n):
        for j in range(n):
            dp[0][i][j] = best[i][j]

    for t in range(1, limit + 1):
        for i in range(n):
            for j in range(n):
                dp[t][i][j] = dp[t - 1][i][j]

        for i in range(n):
            for k in range(n):
                left = dp[t - 1][i][k]
                for j in range(n):
                    nd = left + best[k][j]
                    if nd < dp[t][i][j]:
                        dp[t][i][j] = nd

    ans = []

    for _ in range(r):
        s, t, k = map(int, input().split())
        s -= 1
        t -= 1
        k = min(k, limit)

        ans.append(str(dp[k][s][t]))

    print("\n".join(ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup

    return out.getvalue()

# provided sample
assert run(
"""4 2 3
0 1 5 6
2 0 3 6
1 3 0 1
6 6 7 0
0 3 5 6
2 0 1 6
1 3 0 2
6 6 7 0
1 4 2
1 4 1
1 4 3
"""
) == "3\n4\n3\n", "sample"

# minimum size
assert run(
"""2 1 1
0 7
7 0
1 2 0
"""
) == "7\n", "minimum case"

# better indirect path inside one car
assert run(
"""3 1 1
0 100 1
1 0 1
1 1 0
1 2 0
"""
) == "2\n", "Floyd-Warshall needed"

# extra changes do not help
assert run(
"""3 2 1
0 1 10
1 0 1
10 1 0
0 5 5
5 0 5
5 5 0
1 3 5
"""
) == "2\n", "at most k changes"

# switching cars improves answer
assert run(
"""3 2 1
0 1 100
1 0 100
100 100 0
0 100 1
100 0 1
1 1 0
1 3 1
"""
) == "2\n", "car switching"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Biểu đồ tối thiểu 2 thành phố | 7 | Lập chỉ mục và khởi tạo cơ bản | 
| Tuyến gián tiếp rẻ hơn | 2 | Quá trình tiền xử lý Floyd-Warshall là cần thiết | 
| Những thay đổi bổ sung không được sử dụng | 2 | DP phải có nghĩa là “tối đa k thay đổi” | 
| Chuyển đổi ô tô cải thiện lộ trình | 2 | Chuyển đổi DP dựa trên phân khúc | 

## Vỏ cạnh 

Hãy xem xét trường hợp một chiếc ô tô có đường đi gián tiếp tốt hơn đường đi trực tiếp.```
3 1 1
0 100 1
1 0 1
1 1 0
1 2 0
```Cập nhật Floyd-Warshall: 

| Trung cấp | Đã cập nhật Khoảng cách 1 → 2 | 
| --- | --- | 
| Không có | 100 | 
| Qua 3 | 2 | 

Câu trả lời cuối cùng trở thành`2`. Bất kỳ giải pháp nào chỉ sử dụng các mục nhập ma trận trực tiếp sẽ in không chính xác`100`. 

Bây giờ hãy xem xét trường hợp những thay đổi được phép bổ sung là không cần thiết.```
3 2 1
0 1 10
1 0 1
10 1 0
0 5 5
5 0 5
5 5 0
1 3 5
```Lộ trình tối ưu là:```
1 -> 2 -> 3
```chỉ sử dụng chiếc xe đầu tiên, tổng chi phí`2`. 

Trong quá trình tính toán DP: 

| Lớp | dp[lớp] [1] [3] | 
| --- | --- | 
| 0 thay đổi | 2 | 
| 1 thay đổi | 2 | 
| 2 thay đổi | 2 | 

Vì mỗi lớp bắt đầu bằng cách sao chép lớp trước đó nên DP sẽ bảo toàn các giải pháp một cách tự nhiên bằng cách sử dụng ít thay đổi hơn. 

Cuối cùng, hãy xem xét một trường hợp mà việc chuyển đổi là cần thiết.```
3 2 1
0 1 100
1 0 100
100 100 0
0 100 1
100 0 1
1 1 0
1 3 1
```Chỉ sử dụng một chiếc xe: 

| Xe hơi | Chi phí 1 → 3 | 
| --- | --- | 
| 1 | 100 | 
| 2 | 1 | 

Nhưng tuyến đường được phân đoạn tối ưu là:```
1 -> 2 using car 1
2 -> 3 using car 2
```Tổng chi phí`2`. 

Quá trình chuyển đổi DP qua thành phố trung gian`2`phát hiện chính xác sự kết hợp này.
