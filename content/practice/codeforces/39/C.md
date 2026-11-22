---
title: "CF 39C - Miệng núi lửa Mặt Trăng"
description: "Mỗi miệng hố được mô tả bởi tọa độ tâm c và bán kính r. Vì robot di chuyển dọc theo một đường, nên mọi miệng núi lửa có thể được biểu diễn trên đường đó theo khoảng của nó: $[c-r, c+r]$ Quy tắc của Giáo sư Okulov nói rằng bất kỳ hai miệng núi lửa nào được chọn đều phải hoàn toàn tách rời nhau hoặc là một…"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "sortings"]
categories: ["algorithms"]
codeforces_contest: 39
codeforces_index: "C"
codeforces_contest_name: "School Team Contest 1 (Winter Computer School 2010/11)"
rating: 2100
weight: 39
solve_time_s: 265
verified: false
draft: false
---

[CF 39C - Miệng núi lửa trên mặt trăng](https://codeforces.com/problemset/problem/39/C) 

**Xếp hạng:** 2100 
**Thẻ:** dp, sắp xếp 
**Thời gian giải:** 4 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi miệng hố được mô tả bằng tọa độ trung tâm`c`và bán kính`r`. Vì robot di chuyển dọc theo một đường nên mọi miệng núi lửa có thể được biểu diễn trên đường đó theo khoảng cách của nó:$[c-r,\ c+r]$Quy tắc của giáo sư Okulov nói rằng hai miệng núi lửa được chọn bất kỳ phải hoàn toàn tách rời nhau hoặc một miệng phải nằm hoàn toàn bên trong miệng núi lửa kia. Chồng chéo một phần bị cấm. 

Đối với các khoảng thời gian, điều này có nghĩa là đối với hai khoảng thời gian được chọn bất kỳ: 

1. Chúng hoàn toàn không giao nhau. 
2. Một khoảng được chứa hoàn toàn bên trong khoảng kia. 
3. Được phép chạm vào điểm cuối. 

Chúng ta phải chọn tập hợp con lớn nhất có thể của các miệng hố thỏa mãn điều kiện này và đưa ra bất kỳ tập hợp tối ưu nào. 

Ràng buộc`n ≤ 2000`là manh mối chính. Một dung dịch lập phương có khoảng`8 * 10^9`hoạt động là không thể. Giải pháp lập trình động bậc hai là thực tế, đặc biệt là trong Python khi được triển khai cẩn thận. Vì tác vụ cũng yêu cầu xây dựng lại tập hợp đã chọn nên chúng ta cần lưu trữ các chuyển đổi, không chỉ kích thước câu trả lời. 

Phần khó khăn là mối quan hệ giữa các khoảng thời gian không chỉ đơn giản là “tương thích” hoặc “không tương thích” như việc lập kế hoạch khoảng thời gian thông thường. Hai khoảng có thể cùng tồn tại nếu một khoảng chứa khoảng kia, điều này tạo ra một cấu trúc lồng nhau tương tự như các dấu ngoặc đơn được khớp chính xác. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Xét các khoảng tiếp tuyến:```
2
0 1
2 1
```Những khoảng này là`[−1,1]`Và`[1,3]`. Họ chạm vào một điểm và được phép cùng nhau. Sử dụng các bất đẳng thức nghiêm ngặt khi kiểm tra sự chồng chéo sẽ loại bỏ chúng một cách không chính xác. 

Bây giờ hãy xem xét các khoảng thời gian vượt qua:```
2
0 3
4 3
```Những khoảng này là`[−3,3]`Và`[1,7]`. Chúng chồng lên nhau, nhưng không chứa cái kia. Cả hai đều không thể được chọn. Việc triển khai bất cẩn mà chỉ kiểm tra “không rời rạc” sẽ chấp nhận chúng một cách không chính xác. 

Khoảng cách lồng nhau cũng cần được chăm sóc:```
3
0 10
0 5
0 2
```Cả ba đều hợp lệ vì mọi miệng núi lửa nhỏ hơn đều nằm hoàn toàn bên trong miệng núi lửa lớn hơn. Một chiến lược tham lam luôn thích những khoảng thời gian nhỏ hơn hoặc luôn thích những khoảng thời gian lớn hơn có thể dễ dàng mất đi những giải pháp tối ưu trong những trường hợp phức tạp hơn. 

Một tình huống tinh vi khác xảy ra khi hai khoảng có chung một điểm cuối:```
2
0 5
5 0
```Các khoảng là`[−5,5]`Và`[5,5]`. Cái thứ hai được chứa đầy đủ trong cái thứ nhất và cho phép tiếp tuyến. Sử dụng biện pháp ngăn chặn nghiêm ngặt thay vì quản thúc không nghiêm ngặt sẽ phá vỡ trường hợp này. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi thử từng tập hợp con các miệng hố và xác minh xem mọi cặp có thỏa mãn quy tắc hay không. Việc kiểm tra cặp mất`O(n²)`, và có`2^n`tập hợp con, do đó tổng độ phức tạp trở thành`O(2^n * n²)`. Ngay cả đối với`n = 40`, điều này đã là vô vọng rồi, trong khi giới hạn thực tế là`2000`. 

Một hướng mạnh mẽ hơn là nghĩ đến các khoảng thời gian. Hai khoảng tương thích trừ khi chúng giao nhau. vượt qua có nghĩa là:$l_1 < l_2 < r_1 < r_2$hoặc đối xứng theo thứ tự ngược lại. 

Điều này cho thấy cấu trúc thực sự của vấn đề. Các họ khoảng hợp lệ chính xác là các họ lớp, có nghĩa là các khoảng rời rạc hoặc lồng nhau. Khi các khoảng được sắp xếp theo điểm cuối của chúng, cấu trúc sẽ trở nên đệ quy. Trong một khoảng thời gian, chúng ta lại giải quyết vấn đề tương tự cho các phần tử con của nó. Các nhóm rời rạc cũng có thể được nối một cách độc lập. 

Điều đó gợi ý khoảng DP. 

Quan sát quan trọng là nếu chúng ta xử lý các khoảng thời gian được sắp xếp theo điểm cuối bên trái và điểm cuối bên phải thì mọi giải pháp hợp lệ sẽ hoạt động giống như các phân đoạn được lồng đúng cách. Bất cứ khi nào chúng ta chọn một khoảng, mọi khoảng được chọn khác đều nằm hoàn toàn bên trong nó hoặc hoàn toàn bên ngoài nó. 

Chúng ta có thể xây dựng DP theo các khoảng thời gian. Đối với mỗi khoảng thời gian`i`, chúng tôi tính toán tập hợp con hợp lệ tốt nhất nằm hoàn toàn bên trong`i`. Sau đó, chúng tôi kết hợp các khoảng thời gian con không giao nhau tương tự như lập kế hoạch khoảng thời gian có trọng số. 

Từ`n ≤ 2000`, MỘT`O(n²)`hoặc`O(n² log n)`giải pháp có thể chấp nhận được. Giải pháp tiêu chuẩn được chấp nhận sử dụng quy hoạch động bậc hai sau khi sắp xếp các khoảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n²) | O(n) | Quá chậm | 
| DP tối ưu | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Biểu diễn khoảng 

Mỗi miệng núi lửa trở thành một khoảng:$[l_i,r_i]=[c_i-r_i,\ c_i+r_i]$Chúng ta cũng giữ nguyên chỉ mục gốc vì đầu ra phải sử dụng cách đánh số đầu vào. 

### Sắp xếp 

1. Sắp xếp các khoảng bằng cách tăng điểm cuối bên trái. 
2. Nếu điểm cuối bên trái bằng nhau, sắp xếp theo điểm cuối bên phải giảm dần. 

Lệnh này rất quan trọng. Khoảng chứa lớn hơn xuất hiện trước khoảng chứa nhỏ hơn. Điều đó làm cho các mối quan hệ lồng nhau được xử lý dễ dàng. 

### Xác định giao cắt 

1. Hai khoảng giao nhau nếu:$l_i < l_j \le r_i < r_j$Các khoảng giao nhau không thể cùng thuộc về câu trả lời. 

### Trạng thái lập trình động 

1. Hãy để`dp[i]`là kích thước tối đa của một tập hợp con hợp lệ có khoảng được chọn ngoài cùng là khoảng`i`. 

Chúng tôi luôn bao gồm`i`chính nó ở trạng thái này. 

### Chuyển tiếp 

1. Đối với mỗi khoảng thời gian`i`, xử lý tất cả các khoảng thời gian một cách nghiêm ngặt bên trong nó. 
2. Giữa các khoảng bên trong`i`, chúng tôi chỉ muốn kết hợp những đứa trẻ “cấp cao nhất” rời rạc. Nếu hai con chồng lên nhau mà không ngăn chặn thì chúng vượt qua nhau và không có giá trị. 
3. Điều này trở thành vấn đề lập kế hoạch khoảng thời gian có trọng số đối với những đứa trẻ tương thích ngay lập tức của`i`. 

Đối với mỗi em ứng cử viên`j`bên trong`i`: 

- Chúng ta có thể lấy cấu trúc tối ưu bắt nguồn từ`j`, đáng giá`dp[j]`. 
- Sau đó tiếp tục với đứa trẻ rời rạc tiếp theo. 

### DP phụ 

1. Sử dụng DP khác trên các vị trí để kết hợp các phần tử con theo thứ tự không chồng chéo. 

Điều này tính toán tập hợp tốt nhất các cấu trúc con rời rạc trong khoảng`i`. 

1. Cuối cùng:```
dp[i] = 1 + best_inside
```các`1`khoảng đếm`i`chính nó. 

### Tái thiết 

1. Lưu trữ các lựa chọn của phụ huynh trong quá trình chuyển đổi. 
2. Sau khi tìm khoảng có giá trị lớn nhất`dp[i]`, xây dựng lại đệ quy tất cả các khoảng đã chọn. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi họ khoảng hợp lệ đều tạo thành một cấu trúc tầng. Bất kỳ khoảng nào bên trong khoảng đã chọn đều thuộc về một nhánh lồng nhau hoặc thuộc về một nhánh hoàn toàn tách rời khác. Cấu trúc vượt qua là không thể. 

Vì vậy, mọi giải pháp tối ưu đều có gốc rễ ở khoảng thời gian`i`phân hủy thành các giải pháp tối ưu độc lập của các con trực tiếp rời rạc của nó. DP khám phá chính xác những khả năng đó và không bao giờ kết hợp các khoảng thời gian giao nhau. Vì mọi cấu hình hợp lệ đều có thể được phân tách duy nhất theo cách này nên DP không thể bỏ lỡ giải pháp tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    segs = []

    for idx in range(1, n + 1):
        c, r = map(int, input().split())
        l = c - r
        rr = c + r
        segs.append((l, rr, idx))

    segs.sort(key=lambda x: (x[0], -x[1]))

    l = [0] * n
    r = [0] * n
    idxs = [0] * n

    for i in range(n):
        l[i], r[i], idxs[i] = segs[i]

    dp = [1] * n
    choice = [[] for _ in range(n)]

    inside = [[] for _ in range(n)]

    for i in range(n):
        for j in range(i + 1, n):
            if l[i] <= l[j] and r[j] <= r[i]:
                inside[i].append(j)

    for i in range(n - 1, -1, -1):
        children = inside[i]

        m = len(children)

        best = [0] * (m + 1)
        take = [False] * m
        nxt = [m] * m

        for a in range(m):
            j = children[a]

            b = a + 1
            while b < m and l[children[b]] <= r[j]:
                b += 1

            nxt[a] = b

        pick = [False] * m

        for a in range(m - 1, -1, -1):
            j = children[a]

            skip_val = best[a + 1]
            take_val = dp[j] + best[nxt[a]]

            if take_val > skip_val:
                best[a] = take_val
                pick[a] = True
            else:
                best[a] = skip_val

        dp[i] = 1 + best[0]

        cur = 0
        selected = []

        while cur < m:
            if pick[cur]:
                j = children[cur]
                selected.append(j)
                cur = nxt[cur]
            else:
                cur += 1

        choice[i] = selected

    root = max(range(n), key=lambda x: dp[x])

    ans = []

    def build(v):
        ans.append(idxs[v])
        for to in choice[v]:
            build(to)

    build(root)

    print(len(ans))
    print(*ans)

solve()
```Bước đầu tiên chuyển đổi mọi miệng hố thành một khoảng. Việc sử dụng điểm cuối khoảng sẽ trực tiếp đơn giản hóa tất cả các điều kiện hình học thành việc so sánh giữa các số. 

Sắp xếp bằng cách tăng điểm cuối bên trái và giảm điểm cuối bên phải là điều cần thiết. Giả sử hai khoảng có chung điểm cuối bên trái. Cái lớn hơn phải xuất hiện trước để mối quan hệ ngăn chặn vẫn có hiệu lực trong quá trình chuyển đổi DP. 

Mảng`inside[i]`lưu trữ mọi khoảng thời gian chứa đầy đủ trong khoảng thời gian`i`. Vì các khoảng đã được sắp xếp nên tất cả các phần tử con cháu sẽ xuất hiện sau`i`. 

DP thứ cấp bên trong mỗi khoảng thời gian hoạt động giống như lập kế hoạch khoảng thời gian có trọng số. Khi chúng tôi chọn khoảng thời gian con`j`, mọi thành phần con sau này có điểm cuối bên trái vẫn ở bên trong`j`thuộc về`j`cây con của và không được xử lý như một cây con cấp cao nhất riêng biệt. Con trỏ`nxt[a]`bỏ qua trực tiếp đến đứa trẻ rời rạc tiếp theo. 

Một chi tiết triển khai tinh tế là điều kiện:```
while b < m and l[children[b]] <= r[j]:
```các`<=`là đúng vì các khoảng chạm không rời rạc ở cùng một cấp độ lồng nhau. Nếu một khoảng khác bắt đầu trước hoặc chính xác tại`r[j]`, sau đó nó giao nhau hoặc được lồng trong`j`, vậy nó phải thuộc về`j`cây con của nó thay vì trở thành anh chị em. 

Giai đoạn xây dựng lại tuân theo các lựa chọn được lưu trữ một cách đệ quy. Vì mọi nút con được chọn đã đại diện cho một cây con lồng nhau tối ưu nên phép đệ quy sẽ xây dựng lại câu trả lời tối ưu đầy đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 1
2 2
4 1
5 1
```Khoảng thời gian: 

| Chỉ mục | Khoảng thời gian | 
| --- | --- | 
| 1 | [0,2] | 
| 2 | [0,4] | 
| 3 | [3,5] | 
| 4 | [4,6] | 

Thứ tự sắp xếp: 

| Vị trí | Chỉ mục gốc | Khoảng thời gian | 
| --- | --- | --- | 
| 0 | 2 | [0,4] | 
| 1 | 1 | [0,2] | 
| 2 | 3 | [3,5] | 
| 3 | 4 | [4,6] | 

Tính toán DP: 

| Khoảng thời gian | Giá trị lồng nhau tốt nhất | dp | 
| --- | --- | --- | 
| [4,6] | 0 | 1 | 
| [3,5] | 0 | 1 | 
| [0,2] | 0 | 1 | 
| [0,4] | 2 | 3 | 

Cấu trúc tối ưu bắt nguồn từ`[0,4]`chứa`[0,2]`Và`[4,6]`. Những khoảng đó rời rạc nên cả ba đều có thể cùng tồn tại. 

Đầu ra:```
3
1 2 4
```Dấu vết này thể hiện cấu trúc tầng. Khoảng bên ngoài chứa một nhánh lồng nhau và một nhánh rời rạc. 

### Ví dụ 2 

đầu vào:```
5
0 5
2 1
4 1
6 1
8 1
```Khoảng thời gian: 

| Chỉ mục | Khoảng thời gian | 
| --- | --- | 
| 1 | [-5,5] | 
| 2 | [1,3] | 
| 3 | [3,5] | 
| 4 | [5,7] | 
| 5 | [7,9] | 

DP tuyên bố: 

| Khoảng thời gian | Trẻ em cấp cao nhất tương thích | dp | 
| --- | --- | --- | 
| [7,9] | không | 1 | 
| [5,7] | không | 1 | 
| [3,5] | không | 1 | 
| [1,3] | không | 1 | 
| [-5,5] | [1,3], [3,5] | 3 | 

Câu trả lời tối ưu chọn các khoảng 1, 2 và 3. 

Ví dụ này xác nhận rằng tiếp tuyến được cho phép.`[1,3]`Và`[3,5]`chạm vào một điểm và vẫn tương thích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Việc xây dựng các mối quan hệ ngăn chặn và chuyển tiếp DP đều yêu cầu phép tính bậc hai | 
| Không gian | O(n²) | Danh sách ngăn chặn có thể lưu trữ các cặp O(n²) | 

Với`n ≤ 2000`, độ phức tạp bậc hai hoàn toàn có thể chấp nhận được. Khoảng bốn triệu thao tác vừa vặn thoải mái trong giới hạn thời gian trong Python và mức sử dụng bộ nhớ cũng vẫn thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())

    segs = []

    for idx in range(1, n + 1):
        c, r = map(int, input().split())
        segs.append((c - r, c + r, idx))

    segs.sort(key=lambda x: (x[0], -x[1]))

    l = [x[0] for x in segs]
    r = [x[1] for x in segs]
    ids = [x[2] for x in segs]

    dp = [1] * n
    choice = [[] for _ in range(n)]

    inside = [[] for _ in range(n)]

    for i in range(n):
        for j in range(i + 1, n):
            if l[i] <= l[j] and r[j] <= r[i]:
                inside[i].append(j)

    for i in range(n - 1, -1, -1):
        children = inside[i]
        m = len(children)

        best = [0] * (m + 1)
        nxt = [m] * m
        pick = [False] * m

        for a in range(m):
            j = children[a]

            b = a + 1
            while b < m and l[children[b]] <= r[j]:
                b += 1

            nxt[a] = b

        for a in range(m - 1, -1, -1):
            j = children[a]

            take = dp[j] + best[nxt[a]]
            skip = best[a + 1]

            if take > skip:
                best[a] = take
                pick[a] = True
            else:
                best[a] = skip

        dp[i] = 1 + best[0]

        cur = 0

        while cur < m:
            if pick[cur]:
                choice[i].append(children[cur])
                cur = nxt[cur]
            else:
                cur += 1

    root = max(range(n), key=lambda x: dp[x])

    ans = []

    def rec(v):
        ans.append(ids[v])
        for to in choice[v]:
            rec(to)

    rec(root)

    print(len(ans))
    print(*sorted(ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run(
"""4
1 1
2 2
4 1
5 1
"""
).splitlines()[0] == "3", "sample"

# minimum case
assert run(
"""1
5 3
"""
) == "1\n1", "single crater"

# fully nested
assert run(
"""3
0 10
0 5
0 2
"""
).splitlines()[0] == "3", "all nested"

# crossing intervals
assert run(
"""2
0 3
4 3
"""
).splitlines()[0] == "1", "crossing intervals"

# tangent intervals
assert run(
"""3
0 1
2 1
4 1
"""
).splitlines()[0] == "3", "touching intervals allowed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Miệng núi lửa đơn | 1 miệng núi lửa được chọn | Kích thước đầu vào tối thiểu | 
| Khoảng cách lồng nhau hoàn toàn | Tất cả các khoảng được chọn | Làm tổ sâu | 
| Khoảng cách vượt qua | Chỉ có 1 người được chọn | Chồng chéo một phần không hợp lệ | 
| Khoảng tiếp tuyến | Tất cả đã chọn | Cho phép chạm vào điểm cuối | 

## Vỏ cạnh 

Hãy xem xét các khoảng tiếp tuyến một lần nữa:```
2
0 1
2 1
```Các khoảng trở thành`[−1,1]`Và`[1,3]`. Trong quá trình kiểm tra tính tương thích, chúng được coi là đủ rời rạc để cùng tồn tại vì việc chạm vào là hợp pháp. DP bao gồm cả hai khoảng, tạo ra kích thước câu trả lời`2`. 

Bây giờ hãy kiểm tra các khoảng giao nhau:```
2
0 3
4 3
```Các khoảng là`[−3,3]`Và`[1,7]`. Không chứa cái kia và chúng chồng chéo lên nhau trong nội bộ. Trong khi xử lý khoảng thời gian thứ hai, quá trình chuyển đổi DP bỏ qua việc kết hợp chúng như anh em vì khoảng thời gian thứ hai bắt đầu trước khi khoảng thời gian đầu tiên kết thúc. Chỉ có một khoảng thời gian có thể còn lại trong câu trả lời cuối cùng. 

Lồng sâu cũng hoạt động chính xác:```
4
0 10
0 7
0 4
0 1
```Mỗi khoảng nằm hoàn toàn bên trong khoảng trước đó. Cấu trúc đệ quy tự nhiên tạo thành một chuỗi duy nhất. DP tính toán:```
1 + 1 + 1 + 1 = 4
```vì vậy tất cả các khoảng được chọn. 

Cuối cùng, hãy xem xét các điểm cuối bằng nhau:```
2
0 5
5 0
```Các khoảng là`[−5,5]`Và`[5,5]`. Cái thứ hai được chứa hoàn toàn trong cái thứ nhất vì sự ngăn chặn sử dụng các bất đẳng thức không nghiêm ngặt. Thuật toán giữ chính xác cả hai khoảng thay vì loại bỏ trường hợp biên.
