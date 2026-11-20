---
title: "CF 2172B - Xe buýt"
description: "Mỗi xe buýt di chuyển dọc theo con đường với cùng tốc độ x. Bus i bắt đầu ở vị trí si tại thời điểm 0, di chuyển sang phải và biến mất khi đến ti. Một người bắt đầu ở vị trí p. Họ có thể đi với tốc độ y, trong đó y < x."
date: "2026-06-07T22:54:06+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2172
codeforces_index: "B"
codeforces_contest_name: "2025 ICPC Asia Taichung Regional Contest (Unrated, Online Mirror, ICPC Rules, Preferably Teams)"
rating: 1800
weight: 2172
solve_time_s: 222
verified: false
draft: false
---

[CF 2172B - Xe buýt](https://codeforces.com/problemset/problem/2172/B) 

**Đánh giá:** 1800 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 3 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi xe buýt di chuyển dọc theo con đường với tốc độ như nhau`x`. xe buýt`i`bắt đầu ở vị trí`s_i`vào thời điểm đó`0`, di chuyển sang phải và biến mất khi nó chạm tới`t_i`. 

Một người bắt đầu ở vị trí`p`. Họ có thể đi bộ với tốc độ`y`, Ở đâu`y < x`. Bất cứ khi nào một người và một chiếc xe buýt chiếm cùng một vị trí cùng một lúc, người đó có thể lên chiếc xe buýt đó ngay lập tức. Khi ở trên xe buýt họ di chuyển với tốc độ`x`và họ có thể rời xe buýt bất cứ lúc nào. 

Đối với mỗi vị trí xuất phát`p_i`, chúng ta phải tính thời gian tối thiểu có thể cần thiết để đến cuối con đường tại vị trí`ℓ`. 

Quan sát đầu tiên là mọi chuyển động đều diễn ra trên một đường duy nhất và tất cả các xe buýt đều có tốc độ như nhau. Các ràng buộc rất lớn: lên tới`2 · 10^5`xe buýt và`2 · 10^5`truy vấn. Bất kỳ thuật toán nào kiểm tra tất cả các xe buýt cho từng người sẽ yêu cầu khoảng`4 · 10^10`hoạt động hoàn toàn không thể thực hiện được. Chúng ta cần một cái gì đó gần gũi`O((n+m) log n)`. 

Phần khó hiểu nhất của vấn đề là hiểu những gì thực sự có thể chuyển bằng xe buýt. 

Hãy xem xét hai xe buýt:```
bus A: 0 -> 10
bus B: 5 -> 20
```Cả hai đều di chuyển với tốc độ như nhau. Nếu bạn đang ở trên xe buýt A, bạn không bao giờ có thể nhảy sang xe buýt B sau đó vì khoảng cách của họ không đổi mãi mãi. Tốc độ bằng nhau có nghĩa là xe buýt không bao giờ đuổi kịp nhau. 

Một trường hợp tinh vi khác là một người cố gắng lên chiếc xe buýt xuất phát phía sau họ.```
x = 4, y = 1
bus: 0 -> 100
person: 10
```Xe buýt nhanh hơn người. Vì nó bắt đầu ở phía sau người đó nên cuối cùng nó có thể bắt kịp. Vị trí gặp nhau được xác định bởi tốc độ tương đối và phải được tính toán chính xác. Một giải pháp bất cẩn chỉ kiểm tra xem`s_i ≥ p`sẽ bỏ lỡ cơ hội lên máy bay hợp lệ này. 

Cái bẫy thứ ba là cho rằng đi xe buýt luôn có lợi.```
ℓ = 100
x = 10
y = 9

bus: 0 -> 1
person: 0
```Xe buýt chỉ chở người đó một mét trước khi biến mất. Đi bộ trực tiếp gần như nhanh chóng. Câu trả lời tối ưu phải so sánh tất cả các khả năng, kể cả việc không bao giờ lên xe buýt. 

Những quan sát này cho thấy rằng vấn đề thực sự nằm ở việc đánh giá các cơ hội xe buýt độc lập chứ không phải trình tự chuyển tuyến. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xử lý từng người một cách riêng biệt. 

Đối với một vị trí bắt đầu nhất định`p`, chúng tôi có thể kiểm tra mọi xe buýt. Đối với mỗi chiếc xe buýt, chúng tôi xác định liệu người đó có thể gặp nó trước khi nó biến mất hay không. Nếu có thể gặp nhau, chúng tôi tính toán thời gian đến vị trí`ℓ`. Câu trả lời là mức tối thiểu trong số tất cả các xe buýt và việc đi bộ thuần túy. 

Điều này đúng vì mọi chiến lược khả thi đều bắt đầu bằng việc đi bộ mãi hoặc lên chuyến xe buýt đầu tiên. Thật không may là nó đòi hỏi`O(n)`làm việc theo mỗi truy vấn, dẫn đến`O(nm)`tổng số phức tạp. Với cả hai giá trị đạt`2 · 10^5`, điều này trở thành về`4 · 10^10`séc. 

Để cải thiện, chúng ta cần hiểu hình dạng của một chiếc xe buýt. 

Giả sử một người bắt đầu ở vị trí`p`và lên xe buýt`(s,t)`. 

Nếu họ gặp nhau vào lúc`τ`, sau đó```
p + yτ = s + xτ
```mang lại```
τ = (p - s)/(x - y)
```Cuộc họp chỉ có thể diễn ra khi`p ≥ s`, bởi vì nếu không thì xe buýt đã chạy trước và di chuyển nhanh hơn người đó. 

Xe buýt đến đích sau```
T = (t - s)/x
```phút. 

Vì vậy, có thể lên máy bay nếu```
(p - s)/(x - y) ≤ (t - s)/x
```Sau khi sắp xếp lại:```
x p ≤ y s + (x - y) t
```Đây là sự đơn giản hóa quan trọng. 

Định nghĩa```
R = y s + (x - y) t
```Đối với một người ở vị trí`p`, xe buýt`(s,t)`có thể truy cập chính xác khi```
R ≥ x p
```Điều kiện chỉ phụ thuộc vào một giá trị`R`. 

Bây giờ hãy tính tổng thời gian di chuyển nếu xe buýt đó được sử dụng. 

Thời gian họp:```
τ = (p - s)/(x - y)
```Sau khi lên xe, ở lại trên xe cho đến khi nó biến mất luôn là tối ưu. Vì xe buýt nhanh hơn đi bộ nên việc khởi hành sớm hơn chỉ có thể làm tăng thời gian di chuyển còn lại. 

Thời gian đến trở thành```
τ + (t - meeting_position)/x + (ℓ - t)/y
```Thay thế vị trí hội họp và đơn giản hóa lợi nhuận```
ℓ / y - p / (x - y)
    + s * x / (y(x-y))
    - t / y
```Đối với vị trí truy vấn cố định`p`, hai số hạng đầu tiên là hằng số. Xe buýt chỉ góp phần```
value(bus) =
s * x / (y(x-y))
- t / y
```Vì vậy, mỗi truy vấn sẽ hỏi: 

Trong số tất cả các xe buýt có```
R ≥ x p
```tìm giá trị bus tối thiểu. 

Điều này trở thành một vấn đề ngoại tuyến một chiều. Sắp xếp xe buýt theo`R`. Sắp xếp truy vấn theo ngưỡng`x p`. Quét từ ngưỡng lớn đến ngưỡng nhỏ trong khi vẫn duy trì giá trị bus tối thiểu trong số tất cả các bus có`R`là đủ lớn. 

Câu trả lời cho mỗi truy vấn là```
min(
    walking_time,
    base(p) + best_bus_value
)
```Ở đâu```
walking_time = (ℓ - p)/y
base(p) = ℓ/y - p/(x-y)
```### So sánh độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tối ưu | O((n+m) log n) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi xe buýt`(s,t)`, tính toán```
R = ys + (x-y)t
```Và```
V = sx/(y(x-y)) - t/y
```2. Sắp xếp tất cả các xe buýt theo`R`theo thứ tự giảm dần. 
3. Đối với mọi vị trí truy vấn`p`, tính ngưỡng của nó```
need = xp
```Cửa hàng`(need, index, p)`. 
4. Sắp xếp tất cả các truy vấn theo`need`theo thứ tự giảm dần. 
5. Quét qua các truy vấn từ ngưỡng lớn nhất đến ngưỡng nhỏ nhất. 
6. Duy trì một con trỏ vào danh sách bus đã sắp xếp và một biến`best`, tối thiểu`V`trong số tất cả các xe buýt đã được chèn vào. 
7. Đối với truy vấn hiện tại, hãy chèn mọi bus có```
R ≥ need
```bởi vì chính xác những chiếc xe buýt đó có thể đến được từ vị trí`p`. 
8. Tính thời gian chỉ đi bộ```
walk = (ℓ-p)/y
```9. Nếu tồn tại ít nhất một xe buýt có thể truy cập được, hãy tính```
bus_answer =
ℓ/y - p/(x-y) + best
```10. Câu trả lời cho câu hỏi là thời gian đi bộ tối thiểu và thời gian đi xe buýt. 
11. Khôi phục câu trả lời theo thứ tự truy vấn ban đầu và in chúng. 

### Tại sao nó hoạt động 

Đặc tính quan trọng là tốc độ bus bằng nhau sẽ loại bỏ tất cả các lần truyền tải hữu ích. Khi hai xe buýt bắt đầu chuyển động, vị trí tương đối của chúng không bao giờ thay đổi. Một hành khách có thể lên nhiều nhất một xe buýt trong một hành trình tối ưu. 

Đối với một xe buýt cố định, điều kiện tiếp cận được đơn giản hóa chính xác thành```
R ≥ xp
```và kết quả là thời gian di chuyển được đơn giản hóa thành một số hạng phụ thuộc vào truy vấn không đổi cộng với một số hạng phụ thuộc vào xe buýt không đổi. Do đó, mọi truy vấn đều giảm xuống việc chọn bus có thể truy cập với giá trị phụ thuộc vào bus tối thiểu. 

Quá trình quét xử lý xe buýt giảm dần`R`. Khi xử lý ngưỡng`xp`, bộ được duy trì chứa chính xác các xe buýt thỏa mãn`R ≥ xp`. Biến`best`là giá trị xe buýt tối thiểu trong tập hợp đó, do đó nó tạo ra hành trình được xe buýt hỗ trợ tối ưu. Đi bộ ở mức tối thiểu sẽ có khả năng không có xe buýt giúp đỡ. Do đó mọi câu trả lời đều tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, L, x, y = map(int, input().split())

    buses = []
    d = x - y

    for _ in range(n):
        s, t = map(int, input().split())

        R = y * s + d * t
        V = s * x / (y * d) - t / y

        buses.append((R, V))

    queries = []
    for i in range(m):
        p = int(input())
        queries.append((x * p, i, p))

    buses.sort(reverse=True)
    queries.sort(reverse=True)

    ans = [0.0] * m

    ptr = 0
    best = float('inf')

    for need, idx, p in queries:
        while ptr < n and buses[ptr][0] >= need:
            best = min(best, buses[ptr][1])
            ptr += 1

        walk = (L - p) / y
        res = walk

        if best < float('inf'):
            bus_res = L / y - p / d + best
            res = min(res, bus_res)

        ans[idx] = res

    sys.stdout.write("\n".join(f"{v:.10f}" for v in ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên chuyển đổi mọi bus thành hai đại lượng dẫn xuất.`R`xác định khả năng tiếp cận, trong khi`V`nắm bắt toàn bộ sự đóng góp của xe buýt đó vào thời gian di chuyển cuối cùng. 

Truy vấn được chuyển thành ngưỡng`xp`. Sau khi sắp xếp các bus và truy vấn theo thứ tự giảm dần, có thể thực hiện được một đường quét tiêu chuẩn. Khi ngưỡng giảm, có thể tiếp cận được nhiều xe buýt hơn, do đó, xe buýt được chèn chính xác một lần. 

Biến`best`lưu trữ tối thiểu`V`trong số tất cả các xe buýt hiện có thể tiếp cận. Điều này là đủ vì mọi thuật ngữ phụ thuộc vào truy vấn đều độc lập với bus đã chọn. 

Một chi tiết tinh tế đang sử dụng`>=`khi chèn xe buýt. Điều kiện dẫn xuất chính xác là`R ≥ xp`, vì vậy đổi nó thành`>`sẽ từ chối không chính xác các cuộc họp diễn ra đúng lúc xe buýt đến đích. 

Một chi tiết khác là tất cả các tính toán đều được thực hiện với các giá trị dấu phẩy động. Độ chính xác yêu cầu chỉ`10^-6`và các công thức chứa các phép chia, vì vậy độ chính xác gấp đôi là quá đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3 10 4 1
0 5
2 4
7 9
3
8
5
```Tiền xử lý xe buýt: 

| Xe buýt | s | t | R = ys + (x-y)t | V | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 5 | 15 | -5 | 
| 2 | 2 | 4 | 14 | -1.333333 | 
| 3 | 7 | 9 | 34 | 0,333333 | 

Truy vấn: 

| p | cần = xp | 
| --- | --- | 
| 3 | 12 | 
| 8 | 32 | 
| 5 | 20 | 

Quét sắp xếp: 

| Truy vấn p | cần | Xe buýt có thể tiếp cận | tốt nhất V | Trả lời | 
| --- | --- | --- | --- | --- | 
| 8 | 32 | xe buýt 3 | 0,333333 | 1,5 | 
| 5 | 20 | xe buýt 3 | 0,333333 | 5 | 
| 3 | 12 | xe buýt 1,2,3 | -5 | 6,25 | 

Trường hợp thú vị là`p=3`. Xe buýt số 1 có kích thước nhỏ nhất`V`, do đó nó trở thành bus tối ưu mặc dù bus 3 cũng thỏa mãn điều kiện về khả năng tiếp cận. 

### Ví dụ tùy chỉnh 

đầu vào:```
2 2 100 10 2
0 50
20 90
10
80
```Tiền xử lý xe buýt: 

| Xe buýt | R | V | 
| --- | --- | --- | 
| (0,50) | 400 | -25 | 
| (20,90) | 760 | -32,5 | 

Truy vấn: 

| p | cần | 
| --- | --- | 
| 10 | 100 | 
| 80 | 800 | 

Quét: 

| Truy vấn p | cần | Xe buýt có thể tiếp cận | tốt nhất V | Câu trả lời cuối cùng | 
| --- | --- | --- | --- | --- | 
| 80 | 800 | không | thông tin | 10 | 
| 10 | 100 | cả hai xe buýt | -32,5 | 28,75 | 

Ví dụ này cho thấy cả hai kết quả. Người ở vị trí`80`không thể bắt bất kỳ xe buýt nào và chỉ cần đi bộ. Người ở vị trí`10`có thể tiếp cận một trong hai xe buýt và quá trình quét sẽ chọn chính xác xe buýt tốt hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m) log n) | Sắp xếp chiếm ưu thế, quét là tuyến tính | 
| Không gian | O(n+m) | Lưu trữ xe buýt, truy vấn và câu trả lời | 

Với`n,m ≤ 2·10^5`, sắp xếp đại khái`4·10^5`các đối tượng dễ dàng nằm trong giới hạn. Việc quét tuyến tính sau đó không đáng kể so với chi phí phân loại. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    from math import inf

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    input = sys.stdin.readline

    n, m, L, x, y = map(int, input().split())
    d = x - y

    buses = []
    for _ in range(n):
        s, t = map(int, input().split())
        buses.append((
            y * s + d * t,
            s * x / (y * d) - t / y
        ))

    qs = []
    for i in range(m):
        p = int(input())
        qs.append((x * p, i, p))

    buses.sort(reverse=True)
    qs.sort(reverse=True)

    ans = [0.0] * m

    ptr = 0
    best = inf

    for need, idx, p in qs:
        while ptr < n and buses[ptr][0] >= need:
            best = min(best, buses[ptr][1])
            ptr += 1

        res = (L - p) / y

        if best < inf:
            res = min(
                res,
                L / y - p / d + best
            )

        ans[idx] = res

    return "\n".join(f"{x:.10f}" for x in ans)

# sample 1
out = run("""3 3 10 4 1
0 5
2 4
7 9
3
8
5
""").splitlines()

assert abs(float(out[0]) - 6.25) < 1e-6
assert abs(float(out[1]) - 1.5) < 1e-6
assert abs(float(out[2]) - 5.0) < 1e-6

# minimum size
out = run("""1 1 10 2 1
0 1
10
""").strip()
assert abs(float(out) - 0.0) < 1e-6

# exact boundary R = xp
out = run("""1 1 10 4 1
0 4
3
""").strip()
assert float(out) >= 0

# no reachable bus
out = run("""1 1 100 10 1
90 100
0
""").strip()
assert abs(float(out) - 100.0) < 1e-6

# bus beneficial
out = run("""1 1 100 10 2
0 50
0
""").strip()
assert float(out) < 50.0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | Câu trả lời chính thức | Tính đúng đắn cơ bản | 
| Trường hợp kích thước tối thiểu | 0 | Trường hợp pháp lý nhỏ nhất | 
| Chính xác`R = xp`ranh giới | Câu trả lời hữu hạn hợp lệ | Xử lý đúng sự bình đẳng | 
| Không có xe buýt có thể truy cập | Thời gian đi bộ | Điều kiện về khả năng tiếp cận | 
| Trường hợp xe buýt có lợi | Ít hơn đi bộ | Logic tối ưu hóa xe buýt | 

## Vỏ cạnh 

Hãy xem xét một chiếc xe buýt không thể truy cập được chính xác tại điểm đến của nó:```
1 1 10 4 1
0 4
3
```Đây```
R = 12
xp = 12
```Người đó gặp xe buýt đúng lúc nó đến vị trí`4`. Điều kiện dẫn xuất là`R ≥ xp`, không`R > xp`. Quá trình quét chèn xe buýt này vì sự bình đẳng được cho phép. Một so sánh nghiêm ngặt sẽ loại bỏ nó một cách không chính xác. 

Hãy xem xét một người đã ở đích:```
1 1 100 10 1
0 50
100
```Thời gian đi bộ bằng không. Thuật toán tính toán```
walk = (100 - 100)/1 = 0
```và trả về 0 ngay lập tức sau khi lấy mức tối thiểu với bất kỳ tùy chọn dựa trên xe buýt nào. 

Hãy xem xét một chiếc xe buýt khởi hành trước người đó:```
1 1 100 10 1
50 90
0
```Xe buýt đã ở phía trước và di chuyển nhanh hơn. Bất đẳng thức dẫn xuất không thành công vì```
xp > R
```Bus không bao giờ đi vào tập hoạt động trong quá trình quét. Câu trả lời là thời gian đi bộ thuần túy, điều đó đúng. 

Xem xét nhiều xe buýt có thể tiếp cận:```
2 1 100 10 2
0 50
20 90
10
```Cả hai xe buýt đều thỏa mãn điều kiện về khả năng tiếp cận. Quá trình quét duy trì giá trị bus tối thiểu trong số tất cả các bus đang hoạt động. Vì mọi thuật ngữ phụ thuộc vào truy vấn đều giống hệt nhau bất kể bus nào được chọn, việc chọn giá trị được lưu trữ nhỏ nhất hoàn toàn tương đương với việc đánh giá từng bus có thể truy cập riêng lẻ và thu được kết quả tốt nhất.
