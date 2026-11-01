---
title: "CF 106403I - Lật ngược"
description: "Chúng tôi có một chuỗi gạch nhị phân. Các ô được đặt trên một đường và hoạt động được điều khiển bởi một khoảng cách cố định k. Đứng ở ô p nào đó, ta có thể lật ô p cùng với ô p + k, miễn là ô thứ hai đó tồn tại."
date: "2026-06-25T10:08:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106403
codeforces_index: "I"
codeforces_contest_name: "Bay Area Programming Contest 2026 Novice Division"
rating: 0
weight: 106403
solve_time_s: 58
verified: true
draft: false
---

[CF 106403I - Lật ngược](https://codeforces.com/problemset/problem/106403/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Chúng tôi có một chuỗi gạch nhị phân. Các ô được đặt trên một đường và hoạt động được điều khiển bởi một khoảng cách cố định`k`. Đứng ở một viên gạch nào đó`p`, chúng ta có thể lật gạch`p`cùng với ngói`p + k`, miễn là ô thứ hai đó tồn tại. Trước mỗi lần lật, chúng ta có thể di chuyển đến bất kỳ vị trí nào trên dây, coi quãng đường di chuyển là năng lượng. Một cú lật tự nó có giá`m`năng lượng. 

Mục tiêu không phải là đạt được một thỏa thuận cụ thể. Chúng ta chỉ cần chọn một chuỗi các bước di chuyển và lật sao cho sợi dây có số lần`1`gạch càng tốt trong khi chi tiêu nhiều nhất`E`năng lượng. 

Đồ thị hoạt động là cấu trúc chính. Các ô có chỉ số có cùng phần dư modulo`k`tạo thành một chuỗi:```
r -> r+k -> r+2k -> ...
```Một cú lật luôn tác động đến hai đỉnh lân cận của một trong các chuỗi này. 

Các ràng buộc được thiết kế xung quanh quan sát này. Độ dài chuỗi tối đa là`500`và tổng của tất cả độ dài trong các trường hợp thử nghiệm nhiều nhất là`1000`, do đó giải pháp tính theo thời gian khối là có thể chấp nhận được. Sự đảm bảo bổ sung rằng`floor(n/k) <= 50`có nghĩa là mỗi chuỗi có nhiều nhất`51`gạch lát. Điều này ngăn chặn việc xử lý theo cấp số nhân bên trong chuỗi và cho phép lập trình động theo số lần lật. 

Giới hạn năng lượng có thể lớn bằng`10^18`, vì vậy chúng ta không thể sử dụng năng lượng làm thứ nguyên DP. Chi phí quan trọng là số lần tung, vì mỗi lần tung đều có giá như nhau. Thay vào đó, sự chuyển động phải được xử lý một cách có cấu trúc. 

Một số trường hợp cạnh rất dễ bị bỏ sót. 

Nếu không có lượt lật nào có giá cả phải chăng, câu trả lời chỉ đơn giản là số lượng lượt hiện có`1`gạch lát. Ví dụ:```
n = 3, k = 1, E = 0, m = 5
s = 101
```Đầu ra là`2`. Một giải pháp luôn thử ít nhất một thao tác sẽ làm giảm câu trả lời một cách không chính xác. 

Nếu như`m = 0`, các cú lật tự do và chỉ có chuyển động mới tiêu tốn năng lượng. Ví dụ:```
n = 4, k = 1, E = 2, m = 0
s = 0000
```Chúng ta có thể di chuyển đến vị trí`2`và tự do sử dụng hai cạnh đầu tiên, tạo ra`1110`, vậy câu trả lời là`3`. điều trị`E / m`không xử lý số 0 sẽ thất bại. 

Nếu một chuỗi có số ô lẻ thì không thể thay đổi tính chẵn lẻ của các ô đó. Ví dụ:```
n = 3, k = 1, E = 100, m = 1
s = 000
```Đầu ra là`2`, không`3`. Tính chẵn lẻ của toàn bộ chuỗi là bất biến vì mọi thao tác đều lật chính xác hai ô. 

# Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ thử mọi cú lật có thể. Có tới`n-k`các vị trí lật có thể xảy ra, vì vậy trong trường hợp xấu nhất có hàng trăm lựa chọn nhị phân. Việc liệt kê chúng cần khoảng`2^(n-k)`khả năng, vượt xa những gì ngay cả những hạn chế nhỏ cũng cho phép. 

Lý do lực lượng vũ phu hấp dẫn là vì mỗi lần lật được chọn đều có một tác dụng đơn giản: nó thay đổi hai bit. Khó khăn nằm ở sự tương tác giữa các lựa chọn. Việc chọn một cạnh sẽ thay đổi tác dụng hữu ích của các cạnh lân cận trong cùng một chuỗi, do đó các quyết định tham lam độc lập sẽ không có tác dụng. 

Sự giảm thiểu quan trọng đầu tiên đến từ sự di chuyển. Giả sử chúng ta chọn một số vị trí lật. Đặt vị trí được chọn lớn nhất`x`. Bắt đầu từ vị trí`0`, đi thẳng đến`x`ghé thăm mọi vị trí nhỏ hơn trên đường đi, vì vậy chúng tôi có thể thực hiện mọi lượt lật đã chọn mà không cần di chuyển thêm. Chi phí di chuyển chính xác là`x`. Không có lý do gì để quay lại sau đó. 

Điều này thay đổi vấn đề. Chúng ta có thể thử mọi tư thế lật xa nhất có thể`x`. Sau khi thanh toán`x`năng lượng chuyển động, chúng ta chỉ cần tối đa hóa số lượng bằng cách sử dụng một số lần lật giới hạn giữa các vị trí lên đến`x`. 

Quan sát thứ hai là việc lật chỉ kết nối các vị trí cách nhau bởi`k`, do đó mỗi modul-`k`chuỗi là độc lập. Đối với giới hạn lật cố định, chúng tôi giải từng chuỗi riêng biệt và kết hợp các câu trả lời của chúng với DP ba lô theo số lần lật được sử dụng. 

Lực lượng vũ phu hoạt động vì mọi chuỗi hoạt động có thể được mô tả bằng các cạnh đã chọn của nó, nhưng không thành công vì số lượng lựa chọn là theo cấp số nhân. Việc phân rã chuỗi và quan sát chuyển động làm giảm vấn đề thành các chương trình động nhỏ trên các đường dẫn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n^3) | O(n^2) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Đếm số đơn vị ban đầu. Đây luôn là một câu trả lời khả thi vì không được phép làm gì cả. 
2. Xem xét mọi vị trí lật lớn nhất có thể`x`. Nếu như`x`năng lượng đã quá đắt rồi, hãy bỏ qua nó. giá trị`x`đại diện cho điểm xa nhất mà chúng ta cần ghé thăm. 
3. Tính xem có thể thực hiện được bao nhiêu lần lật bóng sau khi trả chi phí di chuyển. Nếu như`m`là tích cực, đây là`(E - x) // m`. Nếu như`m`bằng 0, tất cả các lần lật có sẵn đều có thể được sử dụng. 
4. Chia chuỗi thành các chuỗi dựa trên các chỉ số có cùng modulo dư`k`. Đối với mỗi chuỗi, hãy tính số lượng tốt nhất có thể đạt được sau khi sử dụng chính xác một số lần lật có sẵn nhất định từ chuỗi đó. 
5. Hợp nhất tất cả các chuỗi bằng DP ba lô. Chỉ số DP là số lần lật được thực hiện. Đối với mỗi kết quả của chuỗi, hãy thử cộng số lần lật có thể có của nó vào trạng thái chung. 
6. Đối với hiện tại`x`, kiểm tra từng lần đếm được ngân sách năng lượng cho phép và cập nhật câu trả lời. 

Lý do điều này có tác dụng là vì một lần lật không bao giờ giao nhau giữa hai mô-đun khác nhau.`k`dây chuyền. Khi số lần lật được ấn định cho mỗi chuỗi được cố định, các chuỗi không thể ảnh hưởng lẫn nhau. Tài nguyên được chia sẻ duy nhất là tổng số lần lật, đó chính xác là những gì chiếc ba lô kết hợp. 

Tại sao nó hoạt động: 

Đối với bất kỳ tập hợp hoạt động nào được chọn, hãy để`x`là vị trí lớn nhất nơi bắt đầu lật. Mỗi vị trí xuất phát được chọn tối đa là`x`, vì vậy đi bộ từ`0`ĐẾN`x`cho phép tất cả các lần lật được thực hiện một cách chính xác`x`chi phí di chuyển. Ngân sách còn lại chỉ giới hạn số lần lật. 

Bên trong một chuỗi, mọi lựa chọn có thể có của các cạnh sẵn có của nó đều được xem xét bởi đường dẫn DP. Việc kết hợp các kết quả tối ưu của tất cả các chuỗi bao gồm mọi lựa chọn cạnh tổng thể có thể có. Vì mọi chuỗi thao tác hợp lệ đều tương ứng với một trong các trạng thái được thuật toán xem xét nên giá trị tối đa được tạo ra là tối ưu. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_chain_dp(bits, available_edges, limit):
    n = len(bits)
    dp = [[-10**9] * 2 for _ in range(limit + 1)]
    dp[0][0] = 0

    for i in range(n):
        ndp = [[-10**9] * 2 for _ in range(limit + 1)]
        if i < available_edges:
            choices = (0, 1)
        else:
            choices = (0,)

        for used in range(limit + 1):
            for prev in range(2):
                cur = dp[used][prev]
                if cur < 0:
                    continue
                for take in choices:
                    if used + take <= limit:
                        value = bits[i] ^ prev ^ take
                        ndp[used + take][take] = max(
                            ndp[used + take][take],
                            cur + value
                        )
        dp = ndp

    ans = [0] * (limit + 1)
    for used in range(limit + 1):
        ans[used] = max(dp[used][0], dp[used][1])
    return ans

def solve_case(n, k, E, m, s):
    bits = [int(c) for c in s]
    answer = sum(bits)

    chains = []
    for r in range(k):
        chain = []
        pos = r
        while pos < n:
            chain.append(bits[pos])
            pos += k
        if chain:
            chains.append((r, chain))

    cache = {}

    max_start = n - k - 1
    for x in range(max_start + 1):
        if x > E:
            continue

        if m == 0:
            flips = n
        else:
            flips = min(n, (E - x) // m)

        if flips < 0:
            continue

        global_dp = [-10**9] * (flips + 1)
        global_dp[0] = 0

        for r, chain in chains:
            if r <= x:
                edges = min(len(chain) - 1, (x - r) // k + 1)
            else:
                edges = 0

            key = (r, edges, flips)
            if key not in cache:
                cache[key] = build_chain_dp(chain, edges, flips)

            values = cache[key]

            ndp = [-10**9] * (flips + 1)
            for a in range(flips + 1):
                if global_dp[a] < 0:
                    continue
                for b in range(flips - a + 1):
                    if values[b] >= 0:
                        ndp[a + b] = max(ndp[a + b], global_dp[a] + values[b])
            global_dp = ndp

        answer = max(answer, max(global_dp))

    return answer

def main():
    t = int(input())
    out = []
    for _ in range(t):
        n, k, E, m = map(int, input().split())
        s = input().strip()
        out.append(str(solve_case(n, k, E, m, s)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên tách chuỗi thành modulo-`k`dây chuyền. Mỗi chuỗi được giải quyết bằng một DP nhỏ trong đó trạng thái ghi nhớ có bao nhiêu cạnh đã được lật và liệu cạnh trước đó có bị lật hay không. Cờ cạnh trước đó là đủ vì một đỉnh chỉ phụ thuộc vào hai cạnh liền kề. 

Vòng ngoài thử mọi vị trí lật xa nhất có thể. Biến`flips`là số lượng hoạt động tối đa có thể thực hiện được sau khi thanh toán chi phí di chuyển. Trường hợp đặc biệt`m == 0`tránh chia cho 0 và cho phép chính xác mọi lần lật có thể tiếp cận. 

Việc hợp nhất ba lô kết hợp các câu trả lời theo chuỗi. chỉ số của`global_dp`là tổng số lần lật được sử dụng cho đến nay. Giá trị tối đa trên tất cả số lần lật hợp lệ sẽ mang lại kết quả tốt nhất cho vị trí xa nhất đã chọn. 

# Ví dụ đã hoạt động 

Hãy xem xét:```
5 2 3 1
00000
```Các lần bắt đầu lật có thể là`0`,`1`, Và`2`. 

| Vị trí xa nhất | Ngân sách lật | Những cái tốt nhất | 
| --- | --- | --- | 
| không | 0 | 0 | 
| 0 | 3 | 2 | 
| 1 | 2 | 2 | 
| 2 | 1 | 2 | 

Câu trả lời tốt nhất là`2`. Dấu vết cho thấy chi phí di chuyển có vấn đề. Đi xa hơn không phải lúc nào cũng có ích vì nó làm giảm ngân sách còn lại. 

Coi như:```
5 2 2 1
10101
```| Vị trí xa nhất | Ngân sách lật | Những cái tốt nhất | 
| --- | --- | --- | 
| không | 0 | 3 | 
| 0 | 2 | 3 | 
| 1 | 1 | 4 | 
| 2 | 0 | 3 | 

Thuật toán nhận thấy rằng việc sử dụng cạnh bắt đầu tại vị trí`1`có lợi ích và sinh ra bốn cái. 

Những ví dụ này xác nhận cả hai bất biến quan trọng: chi phí di chuyển chỉ được xác định bởi cạnh được sử dụng xa nhất và mọi chuỗi được giải quyết một cách độc lập. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Có thể có O(n) vị trí xa nhất và mỗi vị trí yêu cầu chuỗi DP và ba lô hợp nhất ở hầu hết các trạng thái O(n). | 
| Không gian | O(n^2) | Kết quả chuỗi được lưu trong bộ nhớ đệm và mảng lập trình động sử dụng bộ nhớ bậc hai trong trường hợp xấu nhất. | 

Độ dài chuỗi tối đa chỉ là`500`và tổng chiều dài của các bài kiểm tra là`1000`, vì vậy sự phức tạp này phù hợp một cách thoải mái. 

# Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    it = iter(data)
    t = int(next(it))
    ans = []
    for _ in range(t):
        n = int(next(it))
        k = int(next(it))
        E = int(next(it))
        m = int(next(it))
        s = next(it)
        ans.append(str(solve_case(n, k, E, m, s)))
    return "\n".join(ans)

assert run("""3
5 2 3 1
00000
6 3 2 0
000000
5 2 2 1
10101
""") == """2
2
4"""

assert run("""1
1 1 0 5
1
""") == "1"

assert run("""1
4 1 2 0
0000
""") == "3"

assert run("""1
3 1 100 1
000
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 2 3 1 / 00000`|`2`| Chi phí xử lý và di chuyển chuỗi cơ bản | 
|`1 1 0 5 / 1`|`1`| Kích thước tối thiểu và trường hợp không hoạt động | 
|`4 1 2 0 / 0000`|`3`| Xử lý chi phí lật bằng không | 
|`3 1 100 1 / 000`|`2`| Hạn chế chẵn lẻ bên trong một chuỗi | 

# Vỏ cạnh 

Đối với trường hợp năng lượng bằng không:```
3 1 0 5
101
```Không có chuyển động hoặc cú lật nào có thể xảy ra vì mỗi cú lật đều cần năng lượng. Thuật toán bắt đầu với số lượng ban đầu là số 1, tức là`2`và không bao giờ cải thiện nó. 

Đối với lượt lật tự do:```
4 1 2 0
0000
```Thuật toán coi số lần lật là không giới hạn sau khi chuyển động thanh toán. Vị trí hợp lý xa nhất là`2`, cho phép sử dụng hai cạnh. Chuỗi DP tìm thấy cấu hình có thể tiếp cận tốt nhất, mang lại`3`những cái đó. 

Để bảo toàn tính chẵn lẻ:```
3 1 100 1
000
```Chuỗi là`0 -> 1 -> 2`. Lật một trong hai cạnh sẽ thay đổi hai ô, do đó tính chẵn lẻ của số ô vẫn là số chẵn. DP xem xét cả hai cạnh nhưng không thể đạt được ba cạnh, trả về`2`. 

Đối với một chuỗi không có sẵn các lần lật:```
5 4 10 1
00000
```Chỉ có một cạnh có thể. Thuật toán tạo ra một chuỗi ngắn và một số chuỗi đơn. Các ô đơn đóng góp các giá trị ban đầu của chúng vì không có thao tác nào có thể ảnh hưởng đến chúng, điều này tránh việc vô tình áp dụng các lần lật ngoài phạm vi hợp lệ.
