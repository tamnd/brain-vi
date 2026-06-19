---
title: "CF 101B - Xe buýt"
description: "Chúng ta có các điểm dừng xe buýt được đặt trên một tuyến từ 0 đến n. Gerald bắt đầu ở điểm dừng 0 và muốn đến điểm dừng n. Mỗi bus được mô tả bằng một khoảng [s,t]. Gerald có thể lên xe buýt đó ở bất kỳ điểm dừng nào từ s đến t - 1, nhưng một khi đã lên xe, anh ấy phải ở lại cho đến điểm dừng t."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 101
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 79 (Div. 1 Only)"
rating: 1700
weight: 101
solve_time_s: 117
verified: true
draft: false
---

[CF 101B - Xe buýt](https://codeforces.com/problemset/problem/101/B) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có các điểm dừng xe buýt được đặt trên một tuyến từ`0`ĐẾN`n`. Gerald bắt đầu tại điểm dừng`0`và muốn đến điểm dừng`n`. 

Mỗi bus được mô tả bằng một khoảng`[s, t]`. Gerald có thể lên xe buýt đó ở bất kỳ điểm dừng nào từ`s`bởi vì`t - 1`, nhưng một khi anh ấy đã cưỡi nó, anh ấy phải ở lại cho đến khi dừng lại`t`. Anh ta không thể lùi lại và không thể đi giữa các điểm dừng. 

Hai tuyến đường được coi là khác nhau nếu tồn tại ít nhất một đoạn giữa các điểm dừng liền kề nơi Gerald đi một xe buýt khác. Trong thực tế, điều này có nghĩa là trình tự chính xác của các xe buýt rất quan trọng. 

Nhiệm vụ là đếm xem có bao nhiêu cách hợp lệ để đến điểm dừng`n`. 

Các ràng buộc hoàn toàn định hình giải pháp. Số điểm dừng`n`có thể lớn như`10^9`, vì vậy bất kỳ thuật toán nào lặp qua mỗi điểm dừng đều không thể thực hiện được. Đồng thời, số lượng xe buýt chỉ`10^5`, điều này gợi ý rõ ràng rằng chỉ những điểm dừng xuất hiện trong đầu vào mới có liên quan. 

Việc giải thích biểu đồ trực tiếp sẽ giúp ích. Mỗi xe buýt tạo ra sự chuyển tiếp từ mỗi điểm dừng trong`[s, t-1]`dừng lại`t`. Nếu chúng ta định nghĩa`dp[x]`như số cách để đến điểm dừng`x`, thì mọi xe buýt đều đóng góp:$$dp[t] += dp[s] + dp[s+1] + \dots + dp[t-1]$$Khó khăn là tính toán các tổng phạm vi này một cách hiệu quả. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
3 1
0 2
```Câu trả lời đúng là`0`. Gerald có thể đến điểm dừng`2`, nhưng không có cách nào để tiếp tục dừng lại`3`. Việc triển khai bất cẩn cho rằng việc đạt đến điểm cuối xe buýt xa nhất là đủ sẽ trả về sai`1`. 

Một trường hợp tinh tế khác là nhiều xe buýt kết thúc ở cùng một điểm dừng:```
2 2
0 2
1 2
```Câu trả lời đúng là`1`. 

Xe buýt thứ hai vô dụng vì dừng lại`1`bản thân nó không thể truy cập được. Một cách triển khai đơn giản chỉ tính các xe buýt kết thúc tại`2`có thể sản xuất không chính xác`2`. 

Xe buýt chồng chéo cũng có vấn đề:```
4 4
0 1
0 2
1 4
2 4
```Câu trả lời đúng là`2`. 

Gerald có thể lấy`0→1→4`hoặc`0→2→4`. Các xe buýt chồng chéo lên nhau rất nhiều, do đó việc tính toán các chuyển tiếp một cách độc lập mà không tôn trọng khả năng tiếp cận sẽ gây ra tình trạng đếm quá mức. 

Sai lầm nguy hiểm nhất đến từ việc cố gắng xử lý các điểm dừng trực tiếp. Từ`n`Có lẽ`10^9`, thậm chí phân bổ một mảng kích thước`n+1`là không thể. 

## Phương pháp tiếp cận 

Ý tưởng vũ lực theo sau sự tái diễn trực tiếp. 

Định nghĩa`dp[x]`như số cách để đến điểm dừng`x`. Ban đầu,`dp[0] = 1`. Đối với mỗi xe buýt`(s, t)`, chúng tôi thêm số cách để đến mọi điểm dừng lên máy bay có thể:$$dp[t] += \sum_{i=s}^{t-1} dp[i]$$Nếu chúng ta tính tổng này theo đúng nghĩa đen cho mỗi bus thì độ phức tạp sẽ trở thành bậc hai trong trường hợp xấu nhất. Với`10^5`xe buýt và mỗi khoảng thời gian có khả năng kéo dài`10^5`các vị trí liên quan, tổng công việc có thể tiếp cận`10^{10}`hoạt động. 

Nút thắt là rõ ràng: liên tục tính tổng các phạm vi lớn. 

Quan sát quan trọng là sự truy hồi chỉ phụ thuộc vào tổng tiền tố của`dp`. 

Nếu chúng ta duy trì:$$pref[i] = dp[0] + dp[1] + \dots + dp[i]$$sau đó:$$dp[t] += pref[t-1] - pref[s-1]$$Bây giờ mỗi lần chuyển đổi trở thành thời gian không đổi. 

Chỉ thế thôi vẫn chưa đủ vì số điểm dừng đã đạt tới`10^9`. Quan sát thứ hai là chỉ có điểm cuối xe buýt mới quan trọng. Nếu không có xe buýt nào bắt đầu hoặc kết thúc ở một điểm dừng nào đó thì chẳng có gì thú vị xảy ra ở đó cả. 

Chúng tôi nén tọa độ bằng cách thu thập tất cả các giá trị điểm dừng riêng biệt xuất hiện trong các xe buýt, cùng với`0`Và`n`. Sau khi sắp xếp chúng, chúng tôi chỉ làm việc trên các chỉ mục nén. 

Các xe buýt sau đó được sắp xếp theo điểm dừng cuối cùng của chúng. Khi chúng tôi quét từ trái sang phải, chúng tôi duy trì tổng tiền tố trên các vị trí được nén. Mỗi xe buýt đóng góp một khoản tiền vào điểm đến của nó. 

Điều này chuyển vấn đề thành lập trình động với tổng tiền tố và nén tọa độ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m2) | O(m) | Quá chậm | 
| Tối ưu | O(m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các xe buýt và thu thập mọi điểm dừng xuất hiện trong đầu vào, cùng với`0`Và`n`. 

Chúng tôi không thể làm việc trên phạm vi tọa độ ban đầu vì các điểm dừng có thể lớn bằng`10^9`. 
2. Sắp xếp và sao chép tọa độ đã thu thập. 

Điều này tạo ra hệ tọa độ nén. 
3. Ánh xạ mọi giá trị dừng ban đầu vào chỉ mục nén của nó. 

Sau khi nén, tất cả các điểm dừng liên quan đều nằm trong một phạm vi dày đặc`[0, k-1]`. 
4. Nhóm xe buýt theo điểm dừng. 

Chúng tôi xử lý các điểm dừng từ trái sang phải, vì vậy khi tính toán cách đạt đến một số điểm dừng`t`, mọi điểm dừng trước đó đều đã có giá trị cuối cùng. 
5. Khởi tạo`dp[start_index_of_0] = 1`. 

Gerald bắt đầu tại điểm dừng`0`theo đúng một cách. 
6. Duy trì cây Fenwick lưu trữ tổng tiền tố của`dp`. 

Cây Fenwick hỗ trợ: 

- thêm một giá trị vào điểm dừng nén 
- truy vấn tổng trên một phạm vi các điểm dừng nén 
7. Xử lý các điểm dừng nén theo thứ tự tăng dần. 

Đối với mỗi xe buýt`(s, t)`kết thúc tại điểm dừng hiện tại: 

- truy vấn tổng số cách để đến bất kỳ điểm dừng nào trong`[s, t-1]`- thêm giá trị đó vào`dp[t]`Điều này khớp chính xác với mối quan hệ tái phát. 
8. Sau khi hoàn thành tất cả các xe buýt kết thúc tại một điểm dừng, hãy chèn`dp[stop]`vào cây Fenwick. 

Các xe buýt trong tương lai có thể sử dụng điểm dừng này làm điểm lên xe. 
9. Đầu ra`dp[n] mod 10^9+7`. 

### Tại sao nó hoạt động 

Bất biến là:`dp[x]`luôn bằng số cách hợp lệ để đến điểm dừng`x`chỉ sử dụng các xe buýt có điểm đến đã được xử lý. 

Khi xử lý một xe buýt`(s, t)`, Gerald có thể lên tàu tại bất kỳ điểm dừng nào có thể đến được ở`[s, t-1]`. Cây Fenwick lưu trữ chính xác tổng của`dp`các giá trị cho các điểm dừng được xử lý, do đó việc truy vấn phạm vi đó sẽ cung cấp tổng số cách hợp lệ để sử dụng bus này. 

Vì các điểm dừng được xử lý theo thứ tự tăng dần nên mọi đường đi góp phần vào`dp[t]`đã được hoàn thiện trước đó`t`được tính toán. Không có sự chuyển tiếp nào trong tương lai có thể tạo ra đường dẫn đến điểm dừng sớm hơn vì xe buýt chỉ di chuyển về phía trước. 

Do đó, mọi tuyến đường hợp lệ đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, idx, val):
        idx += 1
        while idx <= self.n:
            self.bit[idx] = (self.bit[idx] + val) % MOD
            idx += idx & -idx

    def sum(self, idx):
        idx += 1
        res = 0
        while idx > 0:
            res = (res + self.bit[idx]) % MOD
            idx -= idx & -idx
        return res

    def range_sum(self, left, right):
        if left > right:
            return 0
        return (self.sum(right) - self.sum(left - 1)) % MOD

def main():
    n, m = map(int, input().split())

    buses = []
    coords = {0, n}

    for _ in range(m):
        s, t = map(int, input().split())
        buses.append((s, t))
        coords.add(s)
        coords.add(t)

    coords = sorted(coords)
    comp = {x: i for i, x in enumerate(coords)}

    k = len(coords)

    ending = [[] for _ in range(k)]

    for s, t in buses:
        cs = comp[s]
        ct = comp[t]
        ending[ct].append(cs)

    dp = [0] * k

    start = comp[0]
    dp[start] = 1

    fw = Fenwick(k)
    fw.add(start, 1)

    for t_idx in range(k):
        if t_idx == start:
            continue

        ways = 0

        for s_idx in ending[t_idx]:
            ways += fw.range_sum(s_idx, t_idx - 1)
            ways %= MOD

        dp[t_idx] = ways
        fw.add(t_idx, ways)

    print(dp[comp[n]] % MOD)

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng việc nén tọa độ. Vì giá trị điểm dừng có thể rất lớn nên chúng tôi chỉ giữ các điểm dừng thực sự xuất hiện trên xe buýt hoặc dưới dạng điểm cuối`0`Và`n`. 

Các xe buýt được nhóm theo chỉ số điểm đến. Điều này tránh việc quét liên tục tất cả các xe buýt trong quá trình quét. 

Cây Fenwick lưu trữ tổng tiền tố của các đường đi có thể tiếp cận. Của nó`range_sum(l, r)`phương pháp tính toán:$$dp[l] + dp[l+1] + \dots + dp[r]$$theo thời gian logarit. 

Một chi tiết triển khai tinh tế là thứ tự xử lý. Trước tiên chúng ta phải hoàn thành tất cả các đóng góp vào`dp[t]`trước khi chèn`dp[t]`vào cây Fenwick. Nếu không thì xe buýt kết thúc lúc`t`có thể sử dụng sai lệnh dừng`t`chính nó như một điểm lên máy bay. 

Một sai lầm dễ mắc phải khác là xử lý các phạm vi trống. Nếu như`s_idx > t_idx - 1`, thì xe buýt không đóng góp gì cả. Phương thức trợ giúp trả về`0`trong trường hợp đó. 

Các phép toán Modulo được áp dụng sau mỗi lần cập nhật vì số lượng đường dẫn tăng theo cấp số nhân. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2
0 1
1 2
```Tọa độ nén:```
[0, 1, 2]
```| Điểm dừng hiện tại | Xe buýt đến | Phạm vi truy vấn | Đã thêm cách | dp | 
| --- | --- | --- | --- | --- | 
| 0 | không | không | không | [1,0,0] | 
| 1 | (0,1) | [0,0] | 1 | [1,1,0] | 
| 2 | (1,2) | [1,1] | 1 | [1,1,1] | 

Câu trả lời cuối cùng:`1`. 

Dấu vết này cho thấy sự tái diễn cơ bản. Mỗi xe buýt thu thập tất cả các vị trí lên xe có thể tiếp cận trước khi chuyển đến điểm cuối của nó. 

### Ví dụ 2 

đầu vào:```
4 4
0 1
0 2
1 4
2 4
```Tọa độ nén:```
[0,1,2,4]
```| Điểm dừng hiện tại | Xe buýt đến | Phạm vi truy vấn | Đã thêm cách | dp | 
| --- | --- | --- | --- | --- | 
| 0 | không | không | không | [1,0,0,0] | 
| 1 | (0,1) | [0,0] | 1 | [1,1,0,0] | 
| 2 | (0,2) | [0,1] | 2 | [1,1,2,0] | 
| 4 | (1,4), (2,4) | [1,2], [2,2] | 3 + 2 | [1,1,2,5] | 

Câu trả lời cuối cùng:`5`. 

Năm tuyến đường đó là: 

1.`0→1→4`2.`0→2→4`3.`0→1→2→4`4.`0→2→4`sử dụng xe buýt thứ hai trực tiếp 
5.`0→1→2→4`thông qua các lựa chọn xe buýt khác nhau 

Điều này chứng tỏ tại sao các khoảng chồng chéo lại kết hợp một cách tự nhiên thông qua các tổng tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Phối hợp nén, cập nhật Fenwick và truy vấn phạm vi | 
| Không gian | O(m) | Xe buýt được lưu trữ, tọa độ nén, cây Fenwick và mảng DP | 

Với nhiều nhất`10^5`xe buýt,`O(m log m)`dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ cũng giữ nguyên tuyến tính theo số lượng xe buýt, thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, idx, val):
            idx += 1
            while idx <= self.n:
                self.bit[idx] = (self.bit[idx] + val) % MOD
                idx += idx & -idx

        def sum(self, idx):
            idx += 1
            res = 0
            while idx > 0:
                res = (res + self.bit[idx]) % MOD
                idx -= idx & -idx
            return res

        def range_sum(self, l, r):
            if l > r:
                return 0
            return (self.sum(r) - self.sum(l - 1)) % MOD

    n, m = map(int, input().split())

    buses = []
    coords = {0, n}

    for _ in range(m):
        s, t = map(int, input().split())
        buses.append((s, t))
        coords.add(s)
        coords.add(t)

    coords = sorted(coords)
    comp = {x: i for i, x in enumerate(coords)}

    k = len(coords)

    ending = [[] for _ in range(k)]

    for s, t in buses:
        ending[comp[t]].append(comp[s])

    dp = [0] * k

    start = comp[0]
    dp[start] = 1

    fw = Fenwick(k)
    fw.add(start, 1)

    for t_idx in range(k):
        if t_idx == start:
            continue

        ways = 0

        for s_idx in ending[t_idx]:
            ways += fw.range_sum(s_idx, t_idx - 1)
            ways %= MOD

        dp[t_idx] = ways
        fw.add(t_idx, ways)

    return str(dp[comp[n]]) + "\n"

# provided sample
assert run(
"""2 2
0 1
1 2
"""
) == "1\n", "sample 1"

# unreachable destination
assert run(
"""3 1
0 2
"""
) == "0\n", "unreachable destination"

# direct bus only
assert run(
"""5 1
0 5
"""
) == "1\n", "single direct bus"

# overlapping intervals
assert run(
"""4 4
0 1
0 2
1 4
2 4
"""
) == "5\n", "overlapping buses"

# no buses
assert run(
"""1 0
"""
) == "0\n", "empty graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 1 / 0 2`|`0`| Điểm đến không thể truy cập | 
|`5 1 / 0 5`|`1`| Tuyến đường đơn | 
| Ví dụ về khoảng chồng chéo |`5`| Nhiều chuyển tiếp tương tác | 
|`1 0`|`0`| Không có xe buýt nào cả | 

## Vỏ cạnh 

Hãy xem xét trường hợp đích không thể truy cập được:```
3 1
0 2
```Tọa độ nén trở thành`[0,2,3]`. 

Ban đầu:```
dp[0] = 1
```Dừng xử lý`2`: 

- xe buýt`(0,2)`phạm vi truy vấn`[0,1]`- chỉ dừng lại`0`đóng góp 
-`dp[2] = 1`Dừng xử lý`3`: 

- không có xe buýt nào kết thúc ở đây 
-`dp[3] = 0`Thuật toán xuất ra chính xác`0`bởi vì không có chuyển tiếp đến trường. 

Bây giờ hãy xem xét các xe buýt chồng chéo:```
2 2
0 2
1 2
```tọa độ nén là`[0,1,2]`. 

Khi dừng xử lý`2`: 

- xe buýt`(0,2)`đóng góp cách từ`[0,1]`- chỉ dừng lại`0`có thể truy cập được 
- đóng góp =`1`- xe buýt`(1,2)`đóng góp cách từ`[1,1]`- dừng lại`1`không thể truy cập được 
- đóng góp =`0`Tổng cộng:```
dp[2] = 1
```Điều này xác nhận thuật toán không bao giờ tính các xe buýt có phạm vi lên xe chỉ chứa các điểm dừng không thể tiếp cận được. 

Cuối cùng, hãy xem xét tọa độ thưa thớt:```
1000000000 2
0 5
5 1000000000
```Thuật toán nén tọa độ thành:```
[0,5,1000000000]
```Chỉ có ba trạng thái được lưu trữ, mặc dù phạm vi tọa độ ban đầu rất lớn. Đây chính xác là lý do tại sao nén tọa độ là cần thiết.
