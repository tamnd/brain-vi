---
title: "CF 283C - Rắc rối về tiền xu"
description: "Chúng tôi có n loại tiền xu. Loại tiền i có giá trị a[i] và chúng ta có thể lấy bất kỳ số lượng tiền không âm nào thuộc loại đó. Tổng giá trị của tất cả các đồng tiền được chọn phải chính xác là t. Ngoài ra, chúng ta còn được cho các ràng buộc bất đẳng thức có dạng: count[b] count[c] Các ràng buộc này là đặc biệt."
date: "2026-06-05T09:31:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 283
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 174 (Div. 1)"
rating: 2100
weight: 283
solve_time_s: 90
verified: true
draft: false
---

[CF 283C - Rắc rối về tiền xu](https://codeforces.com/problemset/problem/283/C) 

**Xếp hạng:** 2100 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`các loại tiền xu. Loại tiền xu`i`có giá trị`a[i]`, và chúng ta có thể lấy bất kỳ số lượng tiền không âm nào thuộc loại đó. 

Tổng giá trị của tất cả các đồng tiền được chọn phải chính xác`t`. 

Ngoài ra, ta còn cho ràng buộc bất đẳng thức có dạng:`count[b] > count[c]`Những ràng buộc là đặc biệt. Mỗi chỉ mục xuất hiện nhiều nhất một lần ở phía bên trái của một ràng buộc và nhiều nhất một lần ở phía bên phải. Cấu trúc này hóa ra là quan sát quan trọng. 

Chúng ta phải đếm có bao nhiêu vectơ`(count[1], count[2], ..., count[n])`thỏa mãn mọi bất đẳng thức và tạo ra tổng giá trị`t`. Câu trả lời là bắt buộc theo modulo`1,000,000,007`. 

Các ràng buộc đủ lớn nên việc liệt kê trực tiếp là không thể. Tổng mục tiêu đạt`10^5`, trong khi số lượng các loại tiền xu đạt tới`300`. Bất kỳ giải pháp nào cố gắng lặp lại số lượng xu có thể có sẽ ngay lập tức phát nổ. Tổng mục tiêu gợi ý một giải pháp lập trình động với độ phức tạp gần như tỷ lệ thuận với`n * t`, bởi vì`300 * 100000 = 3 * 10^7`hoạt động vẫn có thể quản lý được trong mã được tối ưu hóa. 

Sự bất bình đẳng đưa ra một số tình huống tinh tế. 

Coi như```
2 2 10
1 1
1 2
2 1
```Những ràng buộc đòi hỏi```
x1 > x2
x2 > x1
```điều đó là không thể. Câu trả lời đúng là`0`. 

Việc triển khai bất cẩn chỉ xử lý các ràng buộc cục bộ có thể không phát hiện được mâu thuẫn này. 

Một ví dụ khác là một chu kỳ dài hơn:```
3 3 20
1 1 1
1 2
2 3
3 1
```Điều này đòi hỏi```
x1 > x2 > x3 > x1
```điều đó cũng là không thể. 

Một cạm bẫy khác xảy ra khi sự bất bình đẳng buộc một số khoản đóng góp tối thiểu vốn đã vượt quá`t`. 

Ví dụ:```
2 1 1
5 5
1 2
```Từ`x1 > x2`, phép gán hợp lệ nhỏ nhất là`(1,0)`, đáng giá`5`. Đạt tổng giá trị`1`là không thể, vì vậy câu trả lời là`0`. 

Giải pháp phải tự động phát hiện tất cả các tình huống này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các số đếm có thể có cho từng loại tiền xu và kiểm tra xem tổng giá trị có bằng không`t`và mọi bất bình đẳng đều giữ nguyên. 

Ngay cả khi mọi giá trị đồng xu đều`1`, một biến duy nhất có thể chiếm tới`100000`những giá trị khác nhau. Với`300`các biến, không gian tìm kiếm là vô cùng lớn. 

Sự bất bình đẳng cho thấy số lượng không độc lập. Bởi vì mỗi nút xuất hiện nhiều nhất một lần dưới dạng nguồn và nhiều nhất một lần dưới dạng đích, nên biểu đồ ràng buộc có hình dạng rất hạn chế. 

Tạo cạnh có hướng:```
b -> c
```nghĩa```
count[b] > count[c]
```Vì mỗi đỉnh có nhiều nhất là bậc`1`và nhiều nhất là bằng cấp ngoài`1`, mọi thành phần được kết nối là một chuỗi hoặc một chu trình. 

Một chu trình ngay lập tức khiến hệ thống không thể thực hiện được:```
x1 > x2 > ... > xk > x1
```Vì vậy chỉ có dây chuyền mới quan trọng. 

Bây giờ hãy xem xét một chuỗi:```
v1 -> v2 -> ... -> vk
```có nghĩa là```
x1 > x2 > ... > xk
```Thay vì lưu trữ số đếm trực tiếp, hãy xác định các biến không âm mới:```
y1 = x1 - x2 - 1
y2 = x2 - x3 - 1
...
y(k-1) = x(k-1) - xk - 1
yk = xk
```Mỗi phép gán hợp lệ tương ứng với chính xác một lựa chọn không âm`y`các giá trị và ngược lại. 

Sau khi thay thế, chuỗi đóng góp:```
constant + Σ(weight_i * yi)
```Hằng số xuất phát từ bắt buộc`+1`những khoảng trống được yêu cầu bởi sự bất bình đẳng nghiêm ngặt. 

Tất cả các biến trở thành biến không âm độc lập. Sau khi xử lý từng chuỗi, toàn bộ vấn đề sẽ trở thành: 

Đếm số nghiệm không âm của```
Σ(weight_i * yi) = remaining_target
```Đây chính xác là DP đếm hoàn chỉnh trong ba lô cổ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n · t) | O(t) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị có hướng trong đó mỗi ràng buộc`b > c`trở thành một cạnh`b -> c`. 
2. Lưu trữ cho mỗi nút lân cận cấp độ và nút đi của nó. Do sự đảm bảo đầu vào, mỗi nút có tối đa mức độ`1`và nhiều nhất là bằng cấp ngoài`1`. 
3. Bắt đầu từ mọi nút có mức độ`0`. Các nút như vậy là đầu của chuỗi. 
4. Đi qua từng chuỗi từ đầu đến đuôi và thu thập các đỉnh của nó theo thứ tự. 
5. Cho chuỗi có đỉnh```
v1, v2, ..., vk
```tính tổng tiền tố```
pref[i] = a[v1] + ... + a[vi]
```6. Thêm```
pref[1] + pref[2] + ... + pref[k-1]
```đóng góp bắt buộc toàn cầu`base`. 

Đây là giá trị tối thiểu bị ép buộc bởi các bất đẳng thức chặt chẽ. 
7. Tạo một trọng số DP cho mỗi biến được chuyển đổi. 

Vì`i < k`, hệ số của`yi`là`pref[i]`. 

Vì`yk`, hệ số là`pref[k]`. 
8. Đánh dấu tất cả các đỉnh trong chuỗi là đã thăm. 
9. Sau khi xử lý tất cả các nghiệm, kiểm tra xem mọi đỉnh đã được thăm chưa. 

Mọi đỉnh chưa thăm đều thuộc một chu trình. Trong trường hợp đó câu trả lời là`0`. 
10. Hãy để```
T = t - base
```Nếu như`T < 0`, đầu ra`0`. 
11. Chạy DP đếm toàn bộ ba lô. 

Khởi tạo:```
dp[0] = 1
```12. Đối với mỗi trọng lượng được tạo ra`w`, trình diễn:```
for s from w to T:
    dp[s] += dp[s - w]
```13. Đầu ra`dp[T]`. 

### Tại sao nó hoạt động 

Bên trong một chuỗi, sự biến đổi```
yi = xi - x(i+1) - 1
```loại bỏ các bất đẳng thức nghiêm ngặt và thay thế chúng bằng các biến không âm đơn giản. 

Ánh xạ giữa hợp lệ`x`bài tập và không âm`y`bài tập là khách quan. Không có giải pháp nào bị mất và không có giải pháp không hợp lệ nào được đưa ra. 

Sau khi thay thế, mỗi chuỗi đóng góp một giá trị bắt buộc cố định cộng với sự kết hợp tuyến tính của các biến không âm độc lập. Kết hợp tất cả các chuỗi mang lại một phương trình duy nhất```
Σ(weight_i * yi) = T
```Việc đếm các nghiệm không âm của một phương trình như vậy chính xác là bài toán đếm hoàn chỉnh trong chiếc ba lô. Vì các chu trình tương ứng với các hệ bất đẳng thức nghiêm ngặt không thể có nên việc bác bỏ chúng là đúng. Do đó, mỗi trạng thái DP được tính tương ứng với một cấu hình đồng xu hợp lệ và ngược lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def solve():
    n, q, t = map(int, input().split())
    a = list(map(int, input().split()))

    nxt = [-1] * n
    indeg = [0] * n

    for _ in range(q):
        b, c = map(int, input().split())
        b -= 1
        c -= 1
        nxt[b] = c
        indeg[c] += 1

    visited = [False] * n
    weights = []
    base = 0

    for i in range(n):
        if indeg[i] == 0:
            chain = []
            cur = i

            while cur != -1 and not visited[cur]:
                visited[cur] = True
                chain.append(cur)
                cur = nxt[cur]

            k = len(chain)
            pref = [0] * k

            for j in range(k):
                pref[j] = a[chain[j]]
                if j:
                    pref[j] += pref[j - 1]

            for j in range(k - 1):
                base += pref[j]
                weights.append(pref[j])

            weights.append(pref[k - 1])

    if not all(visited):
        print(0)
        return

    target = t - base
    if target < 0:
        print(0)
        return

    dp = [0] * (target + 1)
    dp[0] = 1

    for w in weights:
        if w > target:
            continue
        for s in range(w, target + 1):
            dp[s] += dp[s - w]
            if dp[s] >= MOD:
                dp[s] %= MOD

    print(dp[target] % MOD)

solve()
```Việc xây dựng biểu đồ sử dụng thuộc tính đặc biệt là mỗi nút có nhiều nhất một cạnh vào và một cạnh ra. Điều đó cho phép mọi thành phần được biểu diễn dưới dạng một chuỗi hoặc một chu trình đơn giản. 

các`base`biến lưu trữ giá trị tối thiểu bị ép buộc bởi các bất đẳng thức nghiêm ngặt. Nếu mức tối thiểu này đã vượt quá`t`, không có giải pháp tồn tại. 

Việc tạo ra`weights`tương ứng chính xác với các biến không âm được chuyển đổi. Mỗi trọng số có thể được sử dụng nhiều lần vì biến tương ứng có thể nhận bất kỳ giá trị không âm nào. Đó là lý do tại sao quá trình chuyển đổi ba lô hoàn chỉnh lặp lại tổng theo thứ tự tăng dần. 

Việc phát hiện chu kỳ là tinh tế. Mỗi chuỗi bắt đầu từ một nút có mức độ`0`. Nếu sau đó một số đỉnh vẫn chưa được thăm thì nó không thể thuộc vào một chuỗi và phải nằm trong một chu trình. Thành phần như vậy không có phép gán hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2 17
3 1 2 5
4 2
3 4
```Đồ thị là:```
3 -> 4 -> 2
```và nút`1`bị cô lập. 

Xích`3 -> 4 -> 2`có giá trị`[2,5,1]`. 

| Bước | Tổng tiền tố | 
| --- | --- | 
| v3 | 2 | 
| v4 | 7 | 
| v2 | 8 | 

Trọng số được tạo:```
2, 7, 8
```Đóng góp cơ bản:```
2 + 7 = 9
```Nút bị cô lập`1`đóng góp trọng lượng:```
3
```Dữ liệu cuối cùng: 

| Số lượng | Giá trị | 
| --- | --- | 
| Căn cứ | 9 | 
| Mục tiêu | 17 - 9 = 8 | 
| Trọng lượng | [2, 7, 8, 3] | 

DP hoàn chỉnh tìm thấy chính xác`3`các cách kiếm tiền`8`, đưa ra câu trả lời:```
3
```Dấu vết này cho thấy các bất đẳng thức chặt chẽ trở thành một đóng góp bắt buộc cố định cộng với các biến độc lập như thế nào. 

### Mẫu 2 

đầu vào:```
3 2 6
3 1 1
1 2
2 3
```Đồ thị:```
1 -> 2 -> 3
```| Bước | Tổng tiền tố | 
| --- | --- | 
| 1 | 3 | 
| 2 | 4 | 
| 3 | 5 | 

Căn cứ:```
3 + 4 = 7
```| Số lượng | Giá trị | 
| --- | --- | 
| Căn cứ | 7 | 
| t | 6 | 
| Mục tiêu còn lại | -1 | 

Vì mục tiêu còn lại là âm nên câu trả lời là:```
0
```Dấu vết thể hiện trường hợp mức tối thiểu bắt buộc đã vượt quá tổng mức mong muốn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · t) | Tối đa DP trong ba lô`n`trọng lượng được tạo ra | 
| Không gian | O(t) | Một mảng DP có chiều dài`t + 1`| 

Với`n ≤ 300`Và`t ≤ 100000`, trường hợp xấu nhất là khoảng ba mươi triệu bản cập nhật DP, vừa vặn trong giới hạn trong Python được tối ưu hóa. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 1000000007

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, q, t = map(int, input().split())
    a = list(map(int, input().split())

)
    nxt = [-1] * n
    indeg = [0] * n

    for _ in range(q):
        b, c = map(int, input().split())
        b -= 1
        c -= 1
        nxt[b] = c
        indeg[c] += 1

    visited = [False] * n
    weights = []
    base = 0

    for i in range(n):
        if indeg[i] == 0:
            chain = []
            cur = i

            while cur != -1 and not visited[cur]:
                visited[cur] = True
                chain.append(cur)
                cur = nxt[cur]

            pref = []
            s = 0
            for v in chain:
                s += a[v]
                pref.append(s)

            for j in range(len(chain) - 1):
                base += pref[j]
                weights.append(pref[j])

            weights.append(pref[-1])

    if not all(visited):
        return "0"

    target = t - base
    if target < 0:
        return "0"

    dp = [0] * (target + 1)
    dp[0] = 1

    for w in weights:
        for s in range(w, target + 1):
            dp[s] = (dp[s] + dp[s - w]) % MOD

    return str(dp[target])

# provided samples
assert run("4 2 17\n3 1 2 5\n4 2\n3 4\n") == "3", "sample 1"
assert run("3 2 6\n3 1 1\n1 2\n2 3\n") == "0", "sample 2"
assert run("3 2 10\n1 2 3\n1 2\n2 1\n") == "0", "sample 3"

# custom cases
assert run("1 0 5\n1\n") == "1", "single coin type"
assert run("2 0 2\n1 1\n") == "3", "all equal values"
assert run("2 1 1\n5 5\n1 2\n") == "0", "minimum exceeds target"
assert run("2 1 2\n1 1\n1 2\n") == "1", "boundary exact minimum"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 5 / 1`|`1`| Ba lô hoàn chỉnh một biến | 
|`2 0 2 / 1 1`|`3`| Giá trị đồng xu bằng nhau và đếm không giới hạn | 
|`2 1 1 / 5 5 / 1 2`|`0`| Đóng góp bắt buộc tối thiểu vượt quá mục tiêu | 
|`2 1 2 / 1 1 / 1 2`|`1`| Ranh giới chính xác nơi mục tiêu bằng mức tối thiểu | 

## Vỏ cạnh 

Xét chu trình nghịch đảo:```
2 2 10
1 2
1 2
2 1
```Không có nút nào có bằng cấp`0`, do đó không có quá trình truyền tải chuỗi nào bắt đầu. Cả hai đỉnh vẫn chưa được thăm. Thuật toán phát hiện điều này sau giai đoạn truyền tải và ngay lập tức trả về`0`. Điều này phù hợp với thực tế rằng`x1 > x2 > x1`là không thể. 

Hãy xem xét một chu kỳ dài hơn:```
3 3 20
1 1 1
1 2
2 3
3 1
```Một lần nữa mọi đỉnh đều có bậc`1`. Không có gốc tồn tại. Tất cả các đỉnh vẫn chưa được thăm và câu trả lời là`0`. 

Hãy xem xét mục tiêu dưới mức tối thiểu bắt buộc:```
2 1 1
5 5
1 2
```Chuỗi có tổng tiền tố`[5,10]`. Khoản đóng góp bắt buộc là`5`, vì vậy mục tiêu còn lại trở thành`-4`. Thuật toán trả về`0`trước khi chạy DP. 

Những trường hợp này chính xác là những trường hợp việc triển khai đơn giản thường thất bại, nhưng chúng được xử lý một cách tự nhiên bằng cách phân rã chuỗi và chuyển đổi đóng góp tối thiểu.
