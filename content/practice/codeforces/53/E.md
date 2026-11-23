---
title: "CF 53E - Ngõ cụt"
description: "Chúng ta được cung cấp một biểu đồ kết nối vô hướng biểu thị các con đường giữa các nút giao. Thị trưởng muốn loại bỏ một số con đường để đồ thị còn lại trở thành một cây, nghĩa là nó vẫn liên thông và chứa đúng n - 1 cạnh."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp"]
categories: ["algorithms"]
codeforces_contest: 53
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 49 (Div. 2)"
rating: 2500
weight: 53
solve_time_s: 238
verified: false
draft: false
---

[CF 53E - Ngõ cụt](https://codeforces.com/problemset/problem/53/E) 

**Đánh giá:** 2500 
**Thẻ:** bitmask, dp 
**Thời gian giải:** 3 phút 58s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ kết nối vô hướng biểu thị các con đường giữa các nút giao. Thị trưởng muốn loại bỏ một số con đường để đồ thị còn lại trở thành một cái cây, nghĩa là nó vẫn được kết nối và chứa chính xác`n - 1`các cạnh. Trong số tất cả các cây bao trùm như vậy, chúng ta chỉ quan tâm đến những cây bao trùm chính xác`k`lá. Lá là một đỉnh có bậc bên trong cây bằng`1`. 

Nhiệm vụ là đếm chính xác có bao nhiêu cây khung của đồ thị ban đầu.`k`lá. 

Đồ thị rất nhỏ, chỉ tối đa`10`đỉnh. Điều đó ngay lập tức gợi ý rằng các thuật toán hàm mũ trên các tập hợp con là có thể thực hiện được. Một không gian trạng thái xung quanh`2^10 = 1024`là nhỏ bé. Thậm chí`3^10`hoặc`10 * 2^10 * 2^10`phong cách lập trình năng động có thể phù hợp thoải mái. 

Khó khăn không phải ở việc tự kiểm tra kết nối. Thử thách thực sự là đếm các cây bao trùm trong khi đồng thời theo dõi độ đỉnh, vì số lượng lá phụ thuộc vào phân bố độ cuối cùng. 

Một giải pháp ngây thơ sẽ liệt kê mọi tập hợp con của`n - 1`các cạnh và kiểm tra xem nó có tạo thành cây bao trùm với chính xác không`k`lá. Vấn đề là biểu đồ có thể chứa tới`45`các cạnh. Số tập con trở thành:$$\binom{45}{9} \approx 8.8 \times 10^8$$điều đó là hoàn toàn không thể trong thời hạn. 

Có một số trường hợp tế nhị phá vỡ các giải pháp bất cẩn. 

Giả sử bản thân đồ thị đã là một cái cây.```
4 3 3
1 2
2 3
2 4
```Chỉ có một cây bao trùm, chính là đồ thị, và nó có đúng ba lá. Câu trả lời đúng là`1`. Việc triển khai có lỗi giả định mọi đỉnh đều bắt đầu như một lá và sau đó loại bỏ các lá khi các cạnh được thêm vào có thể dễ dàng đếm sai đỉnh trung tâm. 

Một trường hợp nguy hiểm khác là xe đạp.```
4 4 2
1 2
2 3
3 4
4 1
```Mỗi cây bao trùm của một chu trình là một đường đi và mỗi đường đi trên bốn đỉnh có đúng hai lá. Câu trả lời đúng là`4`, bởi vì việc loại bỏ bất kỳ cạnh nào sẽ cho một cây hợp lệ. Nếu xử lý kết nối sai, rất dễ tính toán các khu rừng bị ngắt kết nối. 

Cạm bẫy thứ ba xuất hiện khi xây dựng cây tăng dần. Coi như:```
5 6 2
1 2
1 3
1 4
1 5
2 3
4 5
```Cây bao trùm có thể tạm thời chứa nhiều lá trong quá trình xây dựng, nhưng sau khi các cạnh trong tương lai được thêm vào, một số lá đó sẽ biến mất. Bất kỳ DP nào tham lam sửa số lá quá sớm sẽ thất bại. 

Vấn đề cốt lõi là thông tin mức độ chỉ là thông tin cuối cùng sau khi toàn bộ cây đã được xây dựng. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi thử mọi tập hợp con của các cạnh có kích thước`n - 1`. Đối với mỗi tập hợp con, chúng tôi kiểm tra xem nó có tạo thành một biểu đồ được kết nối hay không. Vì đồ thị liên thông với`n - 1`các cạnh tự động là một cây, sau đó chúng ta đếm xem có bao nhiêu đỉnh có bậc`1`. Nếu số đó bằng`k`, chúng tôi thêm một vào câu trả lời. 

Điều này hiệu quả vì định nghĩa về cây bao trùm rất dễ xác minh. Vấn đề là số lượng tập hợp con. Trong trường hợp xấu nhất chúng ta chọn`9`các cạnh ra khỏi`45`, tức là gần một tỷ kết hợp trước khi thực hiện kiểm tra kết nối. 

Giá trị nhỏ của`n`gợi ý rằng các đỉnh, không phải các cạnh, phải là tâm của biểu diễn trạng thái. Một cái cây có một đặc tính rất hữu ích: nếu chúng ta root nó ở đâu đó và tìm ra từng đỉnh một, thì mỗi đỉnh mới được thêm vào sẽ kết nối chính xác một cạnh với phần đã được xây dựng. 

Quan sát đó biến bài toán thành một tập con DP. 

Chúng tôi xây dựng các cấu trúc cảm ứng được kết nối dần dần. Tại mọi thời điểm chúng tôi duy trì: 

- đỉnh nào đã nằm trong thành phần được kết nối hiện tại, 
- đỉnh nào hiện có trạng thái lẻ liên quan đến lá, 
- có bao nhiêu đỉnh đã trở thành đỉnh bên trong. 

Bí quyết chính là mã hóa mức độ tương đương bằng cách sử dụng mặt nạ bit trong khi phát triển từng cạnh của cây. Bất cứ khi nào chúng tôi đính kèm một đỉnh mới`v`đến một đỉnh hiện có`u`: 

-`v`bắt đầu với mức độ`1`, nên nó trở thành một chiếc lá, 
-`u`tăng thêm một bậc, do đó trạng thái lá/nội bộ của nó có thể thay đổi. 

Bản cập nhật cục bộ này chính xác là thứ giúp cho việc lập trình động trở nên khả thi. 

Cách tiêu chuẩn để tránh đếm cùng một cây nhiều lần là lấy gốc của mọi cây ở đỉnh được đánh số nhỏ nhất và chỉ cho phép các chuyển đổi thêm các đỉnh lớn hơn. Thứ tự chuẩn đó sẽ tự động loại bỏ các bản sao. 

DP kết quả có khoảng:$$2^n \times 2^n$$các tiểu bang, rất nhỏ đối với`n ≤ 10`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O\left(\binom{m}{n-1} \cdot (n+m)\right)$|$O(n+m)$| Quá chậm | 
| Tối ưu |$O(3^n \cdot n^2)$|$O(2^n \cdot n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng tập hợp con DP kết nối cổ điển để phát triển các cây có gốc. 

Cho phép`dp[mask][leafMask]`biểu thị số lượng cây khác nhau có tập đỉnh là`mask`và các lá hiện tại của nó chính xác là các đỉnh bên trong`leafMask`. 

Một đỉnh thuộc về`leafMask`nếu mức độ hiện tại của nó bằng`1`. 

### 1. Bắt đầu từ mọi gốc có thể 

Với mọi đỉnh`r`, khởi tạo:```
dp[1 << r][0] = 1
```Một đỉnh cô lập có bậc`0`, không phải bằng cấp`1`, vậy thì nó chưa phải là một chiếc lá. 

Chúng tôi cũng thực thi điều đó`r`là đỉnh nhỏ nhất trong cây cuối cùng. Những chuyển đổi sau này chỉ thêm các đỉnh lớn hơn`r`. Điều này ngăn cản việc đếm cùng một cây nhiều lần từ các gốc khác nhau. 

### 2. Mở rộng các tập con được kết nối 

Đối với mọi tập hợp con hiện tại`mask`, hãy thử thêm một đỉnh mới`v`không ở bên trong`mask`. 

Đỉnh mới phải kết nối thông qua một số cạnh`(u, v)`Ở đâu`u`đã thuộc về`mask`. 

Điều này bảo tồn kết nối tự động. 

### 3. Cập nhật bộ lá 

Khi gắn`v`ĐẾN`u`: 

-`v`lấy bằng`1`, nên nó trở thành một chiếc lá, 
-`u`tăng mức độ của nó bằng cách`1`. 

Hiệu ứng trên`u`phụ thuộc vào loại bằng cấp cũ của nó. 

Nếu như`u`trước đây không phải là lá, nhưng sau khi có thêm một cạnh thì vẫn không phải là lá. 

Nếu như`u`trước đây là một chiếc lá, sau đó mức độ của nó thay đổi từ`1`ĐẾN`2`, nên nó không còn là một chiếc lá nữa. 

Bản cập nhật này có thể được thể hiện hoàn toàn bằng các thao tác bit. 

### 4. Chuyển sang trạng thái mới 

Xây dựng:```
newMask = mask | (1 << v)
```Sau đó sửa đổi`leafMask`: 

- chuyển đổi`u`nếu trước đây nó là một chiếc lá, 
- thêm`v`như một chiếc lá. 

Thêm giá trị DP vào trạng thái mới. 

### 5. Kết thúc với mặt nạ đầy đủ 

Sau khi xử lý tất cả các tập con, mỗi cây khung xuất hiện đúng một lần. 

Đối với mọi tiểu bang nơi:```
mask == (1 << n) - 1
```tính nó nếu:```
popcount(leafMask) == k
```Tổng của tất cả các trạng thái như vậy là câu trả lời. 

### Tại sao nó hoạt động 

Tại mỗi lần chuyển đổi, chúng ta thêm chính xác một đỉnh mới và chính xác một cạnh mới kết nối nó với thành phần được kết nối hiện có. Bắt đầu từ một đỉnh và lặp lại quá trình này luôn tạo ra một cây. 

Ngược lại, mỗi cây có gốc có thể được xây dựng lại một cách duy nhất bằng cách liên tục loại bỏ đỉnh không có gốc lớn nhất. Điều đó có nghĩa là mọi cây bao trùm hợp lệ đều tương ứng với chính xác một chuỗi chuyển tiếp DP. 

Mặt nạ lá vẫn chính xác vì độ đỉnh duy nhất thay đổi trong quá trình chuyển đổi là cấp độ gốc`u`và đỉnh mới được gắn vào`v`. Tất cả các đỉnh khác giữ nguyên bậc. Các quy tắc cập nhật khớp chính xác với mức độ phát triển của cây. 

Vì mỗi cây bao trùm được tạo một lần và chỉ một lần và chúng ta chỉ đếm những cây có chính xác`k`lá, thuật toán là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    adj = [[False] * n for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        adj[u][v] = True
        adj[v][u] = True

    FULL = (1 << n) - 1

    ans = 0

    for root in range(n):
        dp = [[0] * (1 << n) for _ in range(1 << n)]

        dp[1 << root][0] = 1

        for mask in range(1 << n):
            if not (mask & (1 << root)):
                continue

            for leaf_mask in range(1 << n):
                cur = dp[mask][leaf_mask]

                if cur == 0:
                    continue

                for v in range(root + 1, n):
                    if mask & (1 << v):
                        continue

                    for u in range(n):
                        if not (mask & (1 << u)):
                            continue

                        if not adj[u][v]:
                            continue

                        new_mask = mask | (1 << v)

                        new_leaf = leaf_mask

                        if leaf_mask & (1 << u):
                            new_leaf ^= (1 << u)

                        new_leaf |= (1 << v)

                        dp[new_mask][new_leaf] += cur

        for leaf_mask in range(1 << n):
            if leaf_mask.bit_count() == k:
                ans += dp[FULL][leaf_mask]

    print(ans)

solve()
```Ma trận kề giúp kiểm tra sự tồn tại của cạnh theo thời gian không đổi. Từ`n`chỉ là`10`, việc này đơn giản và nhanh hơn danh sách kề cho DP này. 

Vòng ngoài cố định đỉnh nhỏ nhất của cây. Đây là thủ thuật loại bỏ trùng lặp quan trọng. Khi xử lý root`r`, chúng tôi chỉ thêm các đỉnh có chỉ số lớn hơn`r`. Mỗi cây bao trùm có đúng một đỉnh nhỏ nhất nên mỗi cây được tính đúng một lần. 

Trạng thái DP lưu trữ cả tập đỉnh hiện tại và tập chính xác các lá. Một lỗi phổ biến là cố gắng chỉ lưu trữ số lượng lá. Điều đó không thành công vì khi một đỉnh nhận được một cạnh khác sau đó, nó có thể không còn là một chiếc lá nữa. Thuật toán cần biết chính xác đỉnh nào hiện có bậc`1`. 

Logic chuyển tiếp rất tinh tế. Đỉnh mới được thêm vào luôn trở thành một chiếc lá vì bậc của nó chính xác`1`. Đỉnh cha có thể mất trạng thái lá nếu trước đó nó có bậc`1`. Sử dụng XOR để loại bỏ`u`từ bộ lá rất thuận tiện vì chúng ta chỉ thực hiện nó khi`u`đã ở bên trong chiếc mặt nạ lá. 

Việc sử dụng bộ nhớ vẫn ở mức nhỏ vì:$$2^{10} \times 2^{10} = 1,048,576$$trạng thái, dễ dàng phù hợp với bộ nhớ trong Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3 2
1 2
2 3
1 3
```Tất cả các cây bao trùm của một tam giác đều là các đường đi có độ dài bằng 2. 

| Bước | mặt nạ | láMask | Ý nghĩa | 
| --- | --- | --- | --- | 
| Bắt đầu lúc 1 | 001 | 000 | Gốc đơn | 
| Thêm 2 qua 1 | 011 | 010 | Đỉnh 2 trở thành chiếc lá | 
| Thêm 3 qua 1 | 111 | 110 | Đỉnh 2 và 3 là lá | 

Một công trình khác: 

| Bước | mặt nạ | láMask | Ý nghĩa | 
| --- | --- | --- | --- | 
| Bắt đầu lúc 1 | 001 | 000 | Gốc đơn | 
| Thêm 3 qua 1 | 101 | 100 | Đỉnh 3 trở thành chiếc lá | 
| Thêm 2 qua 3 | 111 | 010 | Đỉnh 1 và 2 là lá | 

Có đúng ba cây bao trùm, mỗi cây có hai lá nên đáp án là`3`. 

Dấu vết này cho thấy trạng thái lá thay đổi linh hoạt như thế nào. Một đỉnh có thể tạm thời là một chiếc lá và sau đó không còn là một chiếc lá nữa. 

### Ví dụ 2 

đầu vào:```
4 4 2
1 2
2 3
3 4
4 1
```Đồ thị này là một chu kỳ. 

| Bước | mặt nạ | láMask | Đã thêm cạnh | 
| --- | --- | --- | --- | 
| 0001 | 0000 | Bắt đầu | | 
| 0011 | 0010 | 1-2 | | 
| 0111 | 0100 | 2-3 | | 
| 1111 | 1000 | 3-4 | | 

Lá cuối cùng là đỉnh`1`Và`4`. 

Việc loại bỏ một cạnh bất kỳ khỏi chu trình sẽ tạo ra một đường đi, vì vậy câu trả lời tổng thể là`4`. 

Ví dụ này xác nhận rằng DP không bao giờ tạo ra chu kỳ. Mỗi quá trình chuyển đổi giới thiệu chính xác một đỉnh mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(3^n \cdot n^2)$| Mọi tập hợp con chuyển tiếp sang các đỉnh bên ngoài thông qua tất cả các điểm đính kèm có thể có | 
| Không gian |$O(2^n \cdot 2^n)$| DP lưu trữ trạng thái của tập hợp con và mặt nạ lá | 

Vì`n = 10`, chúng ta có nhiều nhất`1024`tập hợp con và`1024`mặt nạ lá. Tổng số trạng thái là khoảng một triệu, hoàn toàn có thể quản lý được trong giới hạn bộ nhớ. Số lần chuyển tiếp cũng nằm trong giới hạn 5 giây trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n, m, k = map(int, input().split())

        adj = [[False] * n for _ in range(n)]

        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            adj[u][v] = True
            adj[v][u] = True

        FULL = (1 << n) - 1

        ans = 0

        for root in range(n):
            dp = [[0] * (1 << n) for _ in range(1 << n)]

            dp[1 << root][0] = 1

            for mask in range(1 << n):
                if not (mask & (1 << root)):
                    continue

                for leaf_mask in range(1 << n):
                    cur = dp[mask][leaf_mask]

                    if cur == 0:
                        continue

                    for v in range(root + 1, n):
                        if mask & (1 << v):
                            continue

                        for u in range(n):
                            if not (mask & (1 << u)):
                                continue

                            if not adj[u][v]:
                                continue

                            new_mask = mask | (1 << v)

                            new_leaf = leaf_mask

                            if leaf_mask & (1 << u):
                                new_leaf ^= (1 << u)

                            new_leaf |= (1 << v)

                            dp[new_mask][new_leaf] += cur

            for leaf_mask in range(1 << n):
                if leaf_mask.bit_count() == k:
                    ans += dp[FULL][leaf_mask]

        return str(ans)

    return solve()

# provided sample
assert run(
"""3 3 2
1 2
2 3
1 3
"""
) == "3", "sample 1"

# already a tree
assert run(
"""4 3 3
1 2
2 3
2 4
"""
) == "1", "unique spanning tree"

# cycle graph
assert run(
"""4 4 2
1 2
2 3
3 4
4 1
"""
) == "4", "every spanning tree is a path"

# complete graph K4
assert run(
"""4 6 3
1 2
1 3
1 4
2 3
2 4
3 4
"""
) == "4", "all stars"

# path graph
assert run(
"""5 4 2
1 2
2 3
3 4
4 5
"""
) == "1", "single path"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đồ thị tam giác | 3 | Nhiều cây bao trùm có cùng số lá | 
| Cây hiện có | 1 | Không tính trùng lặp | 
| Đồ thị chu kỳ | 4 | Kết nối không có chu kỳ | 
| Đồ thị hoàn chỉnh K4 | 4 | Đếm cây hình ngôi sao | 
| Biểu đồ đường dẫn | 1 | Số lượng lá tối thiểu có thể | 

## Vỏ cạnh 

Hãy xem xét biểu đồ đã là một cây:```
4 3 3
1 2
2 3
2 4
```Thuật toán bắt đầu từ gốc nhỏ nhất, đỉnh`1`. Nó bổ sung dần dần`2`, sau đó`3`, sau đó`4`. đỉnh`2`ban đầu trở thành một chiếc lá sau khi kết nối với`1`, nhưng sau đó mất trạng thái lá khi có các đỉnh`3`Và`4`gắn vào nó. Mặt nạ lá cuối cùng chứa chính xác`{1, 3, 4}`. Không có công trình thay thế nào tồn tại, vì vậy câu trả lời trở thành`1`. 

Bây giờ hãy xem xét chu kỳ:```
4 4 2
1 2
2 3
3 4
4 1
```DP chỉ cho phép thêm một đỉnh chưa có trong tập hợp con hiện tại. Điều đó có nghĩa là không có quá trình chuyển đổi nào có thể tạo ra một chu kỳ. Mỗi cấu trúc hoàn thành tự động chứa chính xác`n - 1`các cạnh và vẫn được kết nối, vì vậy mọi trạng thái cuối cùng đều là một cây hợp lệ. 

Cuối cùng, hãy xem xét trường hợp trạng thái lá thay đổi liên tục:```
5 4 4
1 2
1 3
1 4
1 5
```Đây là ngôi sao có tâm ở`1`. Trong quá trình xây dựng, đỉnh`1`đầu tiên trở thành một chiếc lá sau khi thêm một hàng xóm, sau đó ngay lập tức ngừng là một chiếc lá khi một hàng xóm khác gắn vào. Thuật toán xử lý việc này một cách tự nhiên thông qua các bản cập nhật mặt nạ lá. Những chiếc lá cuối cùng chính xác là`{2,3,4,5}`, đưa ra câu trả lời`1`.
