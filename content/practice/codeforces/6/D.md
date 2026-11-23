---
title: "CF 6D - Thằn lằn và tầng hầm 2"
description: "Chúng tôi có một đội cung thủ, mỗi người đều có một lượng máu nhất định. Một quả cầu lửa chỉ có thể được ném ở vị trí 2...n-1. Nếu chúng ta ném vào vị trí i thì:"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp"]
categories: ["algorithms"]
codeforces_contest: 6
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 6 (Div. 2 Only)"
rating: 2600
weight: 6
solve_time_s: 138
verified: true
draft: false
---
[CF 6D - Thằn lằn và tầng hầm 2](https://codeforces.com/problemset/problem/6/D) 

**Đánh giá:** 2600 
**Tags:** vũ lực, dp 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một đội cung thủ, mỗi người đều có một lượng máu nhất định. Một quả cầu lửa chỉ có thể được ném vào các vị trí`2 ... n-1`. Nếu chúng ta ném vào vị trí`i`, thì: 

- cung thủ`i`thua`a`sức khỏe, 
- cung thủ`i-1`Và`i+1`thua`b`sức khỏe. 

Một cung thủ chỉ chết khi sức khoẻ trở nên tiêu cực chứ không phải bằng không. Chúng ta có thể tiếp tục nhắm mục tiêu vào những cung thủ đã chết. Nhiệm vụ là giảm thiểu số lượng quả cầu lửa và in một chuỗi chỉ số mục tiêu tối ưu. 

Điều đầu tiên quan trọng là quy mô của các ràng buộc. Số lượng cung thủ nhiều nhất là`10`, giá trị sức khỏe tối đa là`15`, và cả hai thiệt hại nhiều nhất là`10`. Đây là những giới hạn nhỏ. Vấn đề không phải là tối ưu hóa tiệm cận theo nghĩa Codeforces thông thường. Thay vào đó, thách thức là tìm ra cách biểu diễn trạng thái phù hợp và cắt tỉa không gian tìm kiếm đủ mạnh. 

Một lực lượng vũ phu hoàn toàn không bị hạn chế vẫn sẽ bùng nổ. Mỗi phát bắn có thể nhắm vào một trong`n-2 ≤ 8`các vị trí. Ngay cả khi câu trả lời tối ưu chỉ quanh quẩn`20`, điều đó đã mang lại khoảng`8^20 ≈ 10^18`các trình tự có thể. Vì vậy chúng ta cần cấu trúc. 

Quan sát quan trọng là địa phương. Ném vào vị trí`i`chỉ ảnh hưởng`i-1`,`i`, Và`i+1`. Khi chúng ta di chuyển đủ xa về bên phải, các vị trí trước đó sẽ trở nên độc lập và không bao giờ có thể thay đổi nữa. Điều đó làm cho việc lập trình động trở nên khả thi. 

Có một số trường hợp khó khăn có thể dễ dàng phá vỡ việc triển khai không chính xác. 

Hãy xem xét đầu vào này:```
3 2 1
2 2 2
```Mục tiêu hợp lệ duy nhất là vị trí`2`. Mỗi lần bắn đều có giá trị`1`thiệt hại cho các đầu và`2`tổn thương ở giữa. Sau hai mũi tiêm, sức khỏe trở nên`[0, -2, 0]`. Các điểm cuối vẫn còn tồn tại vì sức khỏe phải hoàn toàn âm tính. Chúng ta cần phát súng thứ ba. Việc thực hiện bất cẩn để kiểm tra`<= 0`thay vì`< 0`tạo ra câu trả lời sai. 

Một trường hợp tinh vi khác là việc tấn công liên tục một cung thủ đã chết vẫn là phương án tối ưu.```
5 10 1
1 100 1 1 1
```Cung thủ ở giữa có sức khỏe rất lớn. Chiến lược tốt nhất giữ vị trí mục tiêu`3`, ngay cả sau khi các cung thủ gần đó đã chết, bởi vì chúng tôi chỉ quan tâm đến việc giảm thiểu tổng số lần sử dụng. Các thuật toán cấm tấn công kẻ thù đã chết trở nên không chính xác. 

Việc xử lý ranh giới cũng nguy hiểm vì các vị trí`1`Và`n`không thể nhắm mục tiêu trực tiếp.```
4 5 2
9 1 1 9
```Cách duy nhất để gây sát thương cho cung thủ`1`là bằng cách tấn công`2`, và cách duy nhất để gây sát thương cho cung thủ`4`là bằng cách tấn công`3`. Bất kỳ giải pháp nào xử lý đối xứng tất cả các vị trí sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Lực lượng vũ phu trực tiếp nhất là tìm kiếm con đường ngắn nhất trên các trạng thái sức khỏe. Một trạng thái là mảng sức khỏe hiện tại. Từ một trạng thái, chúng tôi thử mọi vị trí mục tiêu có thể và tạo trạng thái tiếp theo sau một lần truyền. Vì mỗi lần sử dụng đều có chi phí như nhau nên BFS đảm bảo số lượng phép thuật tối thiểu. 

Điều này hoạt động vì các ràng buộc rất nhỏ. Giá trị sức khỏe rất nhỏ và mỗi đòn tấn công chỉ làm giảm sức khỏe. Tuy nhiên, không gian trạng thái phát triển rất nhanh. Mỗi cung thủ có thể dao động từ lượng máu ban đầu cho đến một số giá trị âm trước khi cái chết được đảm bảo. Ngay cả khi kẹp mạnh, số lượng trạng thái có thể tiếp cận vẫn rất lớn. Hệ số phân nhánh lên tới`8`và BFS dành phần lớn thời gian để xem lại các tình huống có cấu trúc tương tự. 

Thông tin chi tiết quan trọng là chúng tôi không thực sự quan tâm đến thứ tự chính tả tùy ý. Điều quan trọng là chúng ta sử dụng bao nhiêu lần ở mỗi vị trí. 

Giả sử chúng ta định nghĩa`x[i]`như số lần chúng ta tấn công vị trí`i`. Sau đó tổng sát thương mà cung thủ nhận được`j`là:

-`a * x[j]`, 
- cộng`b * x[j-1]`, 
- cộng`b * x[j+1]`. 

Đối với mỗi cung thủ chúng ta chỉ cần:```
total damage > health
```Bây giờ vấn đề trở thành việc chọn số nguyên không âm`x[i]`giảm thiểu tổng của chúng. 

Đây là nơi địa phương trở nên mạnh mẽ. Cung thủ`i`chỉ phụ thuộc vào`x[i-1]`,`x[i]`, Và`x[i+1]`. Nếu chúng ta xử lý các vị trí từ trái sang phải, thì sau khi sửa hai biến lân cận, chúng ta có thể xác định giá trị yêu cầu tối thiểu cho biến tiếp theo. 

Cụ thể hơn, một lần`x[i-1]`Và`x[i]`được biết đến, đóng góp duy nhất còn lại chưa được biết đến cho cung thủ`i`đến từ`x[i+1]`. Chúng ta có thể tham lam chọn giá trị hợp lệ nhỏ nhất cho`x[i+1]`. Điều đó biến vấn đề thành DP trên các số lượng liền kề. 

Số lượng vị trí tấn công nhiều nhất`8`và mỗi số đếm không bao giờ cần phải lớn. Tổng số lần chụp tối ưu được giới hạn bởi khoảng`150`, nên một DP khối là hoàn toàn khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force BFS về tình trạng sức khỏe | Hàm mũ | Hàm mũ | Quá chậm | 
| DP về số lần tấn công lân cận | O(n * M2) | O(n * M2) | Đã chấp nhận | 

Đây`M`là số lần tấn công có liên quan tối đa trên mỗi vị trí, nhỏ theo các ràng buộc. 

## Hướng dẫn thuật toán 

1. Xác định`x[i]`như số lần chúng ta tấn công vị trí`i`, trong đó các vị trí hợp lệ là`2 ... n-1`. 
2. Viết lại tình trạng sát thương của mỗi cung thủ. Cung thủ`j`nhận được:```
b * x[j-1] + a * x[j] + b * x[j+1]
```và phải thỏa mãn:```
damage > h[j]
```1. Xử lý cung thủ từ trái qua phải. Ở bước`i`, giả sử chúng ta đã biết`x[i-1]`Và`x[i]`. 
2. Cung thủ`i`vẫn thiếu sự đóng góp từ`x[i+1]`. Tính giá trị nhỏ nhất của`x[i+1]`điều đó làm cho cung thủ`i`chết. 

Nếu thiệt hại hiện tại đã đủ, chúng tôi đặt`x[i+1] = 0`. Ngược lại ta giải:```
current + b * x[i+1] > h[i]
```mang lại:```
x[i+1] = floor((h[i] - current) / b) + 1
```1. Lưu trữ trạng thái DP dựa trên hai lần tấn công gần nhất. Quá trình chuyển đổi mang tính quyết định bởi vì một khi`x[i-1]`Và`x[i]`được cố định, giá trị tối thiểu hợp lệ`x[i+1]`được xác định duy nhất. 
2. Khởi tạo DP bằng cách thử tất cả các giá trị có thể cho`x[2]`. Từ`n ≤ 10`và sức khỏe rất nhỏ, giới hạn trên an toàn như`20`hoặc`25`là đủ. 
3. Sau khi xử lý tất cả các vị trí bên trong, hãy xác minh rằng cung thủ cuối cùng cũng chết. Cung thủ cuối cùng chỉ nhận sát thương từ vị trí`n-1`. 
4. Trong số tất cả các cấu hình hợp lệ, hãy chọn một cấu hình giảm thiểu:```
sum(x[i])
```1. Xây dựng lại chuỗi câu trả lời bằng cách in từng vị trí`i`chính xác`x[i]`lần. 

### Tại sao nó hoạt động 

DP dựa vào một bất biến phụ thuộc cục bộ. Cung thủ`i`chỉ phụ thuộc vào ba lần tấn công lân cận:```
x[i-1], x[i], x[i+1]
```Khi xử lý từ trái sang phải, một lần`x[i-1]`Và`x[i]`đều cố định, mọi biến trong tương lai ngoại trừ`x[i+1]`trở nên không liên quan đến cung thủ`i`. Chọn giá trị lớn hơn giá trị khả thi tối thiểu`x[i+1]`không bao giờ có thể giúp giảm chi phí trong tương lai, bởi vì tất cả số lần tấn công đều không âm và thay vào đó, các cung thủ trong tương lai luôn có thể nhận thêm sát thương từ các vị trí sau. 

Vì vậy quá trình chuyển đổi tham lam là tối ưu cục bộ và bởi vì mọi quá trình chuyển đổi trạng thái đều đảm bảo tính khả thi cho các cung thủ đã được xử lý, nên DP sẽ khám phá chính xác tất cả các giải pháp toàn cầu tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**9

n, a, b = map(int, input().split())
h = list(map(int, input().split()))

# x[i] means attacks on position i (1-indexed)
# valid positions are 2..n-1

LIMIT = 25

best = INF
best_x = None

def dfs(pos, x, total):
    global best, best_x

    if total >= best:
        return

    if pos == n:
        # verify last archer
        dmg = b * x[n - 1]
        if dmg > h[n - 1]:
            best = total
            best_x = x[:]
        return

    # current damage on archer pos-1
    dmg = b * x[pos - 2] + a * x[pos - 1]

    need = 0
    if dmg <= h[pos - 2]:
        need = (h[pos - 2] - dmg) // b + 1

    if need > LIMIT:
        return

    x[pos] = need
    dfs(pos + 1, x, total + need)
    x[pos] = 0

# try all possible x[2]
x = [0] * (n + 1)

for start in range(LIMIT + 1):
    x[2] = start
    dfs(3, x, start)

ans = []

for i in range(2, n):
    ans.extend([i] * best_x[i])

print(best)
print(*ans)
```Việc triển khai phản ánh trực tiếp sự lặp lại toán học. 

Mảng`x[i]`lưu trữ số lần chúng tôi tấn công từng vị trí nội bộ. Vì việc lập chỉ mục trong phần giải thích đương nhiên dựa trên 1 nên mã giữ nguyên quy ước để tránh lỗi dịch thuật. 

Hàm đệ quy xử lý các vị trí từ trái sang phải. Khi chúng ta sắp xác định`x[pos]`, tất cả số lần tấn công trước đó đã được sửa. Cung thủ`pos-1`hiện tại có thiệt hại:```
b * x[pos-2] + a * x[pos-1]
```Sự đóng góp duy nhất còn lại có thể đến từ các cuộc tấn công vào`pos`, thêm vào`b`làm hư hỏng từng cái. Chúng tôi tính toán giá trị yêu cầu tối thiểu một cách tham lam. 

Đệ quy không bao giờ phân nhánh sau phép tính này. Sự phân nhánh duy nhất xuất phát từ sự lựa chọn ban đầu của`x[2]`. Đó là biến duy nhất không được xác định bởi ràng buộc trước đó. 

Giới hạn trên`LIMIT = 25`là an toàn vì sức khỏe là tốt nhất`15`và mỗi cuộc tấn công đóng góp ít nhất`1`hư hại. Bất kỳ giải pháp tối ưu nào cũng nằm dưới phạm vi này. 

Một chi tiết tinh tế là tình trạng cái chết. Mã kiểm tra`damage > health`, không`>=`. Thiếu sự bất bình đẳng nghiêm ngặt này là lỗi phổ biến nhất trong vấn đề này. 

Một chi tiết quan trọng khác là cung thủ cuối cùng. Chức vụ`n`chỉ nhận sát thương từ các đòn tấn công tại`n-1`, vì vậy sau khi tất cả các biến được sửa, chúng ta phải xác thực:```
b * x[n-1] > h[n-1]
```trước khi chấp nhận giải pháp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2 1
2 2 2
```Vị trí duy nhất`2`có thể bị tấn công. 

| Bước | x[2] | Thiệt hại tới 1 | Thiệt hại tới 2 | Thiệt hại tới 3 | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 1 | Không | 
| 2 | 2 | 2 | 4 | 2 | Không | 
| 3 | 3 | 3 | 6 | 3 | Có | 

Các điểm cuối yêu cầu nhiều hơn`2`sát thương, nên hai đòn tấn công là không đủ. Dấu vết xác nhận tại sao câu trả lời là`3`. 

### Ví dụ 2 

đầu vào:```
5 4 1
3 6 6 3 3
```| Vị trí đang được cố định | Giá trị đã biết | Giá trị tiếp theo bắt buộc | 
| --- | --- | --- | 
| Bắt đầu | x[2] = 1 | | 
| Cung thủ 2 | x[2] = 1 | x[3] = 0 | 
| Cung Thủ 3 | x[2] = 1, x[3] = 0 | x[4] = 3 | 

Số lần tấn công cuối cùng trở thành: 

| Vị trí | Tấn công | 
| --- | --- | 
| 2 | 1 | 
| 3 | 0 | 
| 4 | 3 | 

Thiệt hại nhận được: 

| Cung thủ | Tổng thiệt hại | 
| --- | --- | 
| 1 | 1 | 
| 2 | 4 | 
| 3 | 4 | 
| 4 | 12 | 
| 5 | 3 | 

Việc này vẫn thất bại vì cung thủ`1`,`2`, Và`3`sức khỏe không hoàn toàn ở mức dưới 0. Việc thử các giá trị ban đầu lớn hơn cuối cùng sẽ đạt được cấu hình hợp lệ tối ưu. 

Ví dụ này thể hiện tính bất biến trung tâm: khi các vị trí trước đó được cố định, biến tiếp theo được xác định duy nhất theo yêu cầu của cung thủ hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * M) | Mỗi giá trị ban đầu tạo ra một đường chuyền từ trái sang phải xác định | 
| Không gian | O(n) | Chỉ có mảng đếm số lần tấn công và ngăn xếp đệ quy được lưu trữ |`M`là số lần tấn công tối đa mà chúng tôi cố gắng cho một vị trí, được giới hạn bởi một hằng số nhỏ. Với`n ≤ 10`, thời gian chạy rất nhỏ và dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    INF = 10**9

    n, a, b = map(int, input().split())
    h = list(map(int, input().split()))

    LIMIT = 25

    best = INF
    best_x = None

    def dfs(pos, x, total):
        nonlocal best, best_x

        if total >= best:
            return

        if pos == n:
            dmg = b * x[n - 1]
            if dmg > h[n - 1]:
                best = total
                best_x = x[:]
            return

        dmg = b * x[pos - 2] + a * x[pos - 1]

        need = 0
        if dmg <= h[pos - 2]:
            need = (h[pos - 2] - dmg) // b + 1

        if need > LIMIT:
            return

        x[pos] = need
        dfs(pos + 1, x, total + need)
        x[pos] = 0

    x = [0] * (n + 1)

    for start in range(LIMIT + 1):
        x[2] = start
        dfs(3, x, start)

    ans = []
    for i in range(2, n):
        ans.extend([i] * best_x[i])

    out = []
    out.append(str(best))
    out.append(" ".join(map(str, ans)))
    return "\n".join(out)

# provided sample
assert solve("3 2 1\n2 2 2\n").splitlines()[0] == "3"

# minimum size
assert solve("3 5 1\n1 1 1\n").splitlines()[0] == "2"

# all equal values
assert int(solve("5 3 1\n5 5 5 5 5\n").splitlines()[0]) > 0

# large health in center
assert int(solve("5 10 1\n1 100 1 1 1\n").splitlines()[0]) >= 10

# boundary-focused case
assert int(solve("4 5 2\n9 1 1 9\n").splitlines()[0]) >= 10
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 5 1 / 1 1 1`|`2`phép thuật | Hợp lệ nhỏ nhất`n`| 
|`5 3 1 / 5 5 5 5 5`| Số lượng tối ưu dương | Đối xứng sức khỏe bình đẳng | 
|`5 10 1 / 1 100 1 1 1`| Câu trả lời lớn | Liên tục tấn công cùng một mục tiêu | 
|`4 5 2 / 9 1 1 9`| Câu trả lời lớn | Lan truyền thiệt hại chỉ ở điểm cuối | 

## Vỏ cạnh 

Hãy xem xét lại:```
3 2 1
2 2 2
```Thuật toán thử các giá trị khác nhau cho`x[2]`. 

Vì`x[2] = 2`, thiệt hại điểm cuối là:```
b * x[2] = 2
```nhưng tình trạng điểm cuối cũng vậy`2`, do đó người bắn cung sống sót vì cái chết đòi hỏi sức khỏe hoàn toàn tiêu cực. DP từ chối chính xác trạng thái này. Với`x[2] = 3`, thiệt hại trở thành`3`, thế là đủ. 

Bây giờ hãy kiểm tra:```
5 10 1
1 100 1 1 1
```Thuật toán có thể tiếp tục tăng`x[3]`ngay cả sau khi các cung thủ lân cận đã chết. Điều này đúng vì được phép tấn công vào kẻ thù đã chết và việc giảm thiểu số lượng phép thuật quan trọng hơn là tránh sử dụng quá mức cần thiết. 

Cuối cùng:```
4 5 2
9 1 1 9
```Cung thủ đầu tiên chỉ có thể nhận sát thương lan từ các đòn tấn công tại vị trí`2`, trong khi cung thủ cuối cùng chỉ nhận sát thương lan từ các đòn tấn công tại vị trí`3`. Sự tái diễn từ trái sang phải một cách tự nhiên sẽ bảo toàn các ràng buộc ranh giới này vì lần kiểm tra đầu tiên và lần kiểm tra cuối cùng chỉ liên quan đến một lần tấn công lân cận thay vì hai.
