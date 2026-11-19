---
title: "CF 2116C - Cá Gellyfish và Hoa mẫu đơn rực lửa"
description: "Chúng ta được cho một mảng các số nguyên dương. Trong một thao tác, chúng ta chọn hai vị trí khác nhau và thay thế một giá trị bằng gcd của hai giá trị. Quá trình này có thể được lặp lại nhiều lần nếu cần thiết."
date: "2026-06-09T04:00:32+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "dp", "math", "number-theory", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 2116
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1028 (Div. 2)"
rating: 1500
weight: 2116
solve_time_s: 123
verified: true
draft: false
---

[CF 2116C - Cá gelly và hoa mẫu đơn rực lửa](https://codeforces.com/problemset/problem/2116/C) 

**Đánh giá:** 1500 
**Tags:** sức mạnh vũ phu, dfs và tương tự, dp, toán, lý thuyết số, đường đi ngắn nhất 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Trong một thao tác, chúng ta chọn hai vị trí khác nhau và thay thế một giá trị bằng gcd của hai giá trị. 

Quá trình này có thể được lặp lại nhiều lần nếu cần thiết. Mục tiêu là làm cho mọi phần tử trong mảng bằng nhau, đồng thời sử dụng số lượng thao tác tối thiểu có thể. 

Câu hỏi quan trọng không phải là liệu điều đó có thể thực hiện được hay không, bởi vì tuyên bố đảm bảo rằng điều đó luôn có thể thực hiện được. Thách thức là tìm ra số lượng hoạt động nhỏ nhất. 

Những hạn chế nhỏ một cách bất thường. Mỗi giá trị mảng tối đa là 5000 và tổng của tất cả độ dài mảng trên toàn bộ đầu vào cũng tối đa là 5000. Điều này ngay lập tức cho thấy rằng các thuật toán liên quan đến các trạng thái trên các giá trị gcd có thể có là khả thi. Vì giá trị gcd không bao giờ có thể vượt quá 5000, nên giải pháp lập trình động hoặc kiểu đường dẫn ngắn nhất trên không gian giá trị là thực tế. 

Việc tìm kiếm brute-force trên các mảng là không thể vì số lượng mảng có thể truy cập tăng theo cấp số nhân. Chúng ta cần khai thác cấu trúc hoạt động của gcd hơn là mô phỏng mọi khả năng. 

Có một số tình huống không rõ ràng có thể dễ dàng dẫn đến suy luận sai. 

Coi như:```
3
2 2 2
```Câu trả lời là`0`. Mảng đã bằng nhau rồi. Bất kỳ giải pháp nào tự động cố gắng xây dựng các giá trị gcd mới sẽ bị tính quá mức. 

Coi như:```
3
2 4 6
```gcd của toàn bộ mảng là`2`, Và`2`đã có mặt rồi. Câu trả lời là`2`. 

Chúng ta có thể biến`4`vào trong`2`trong một thao tác và`6`vào trong`2`trong một cái khác. Một lỗi phổ biến là trước tiên hãy "tạo" một giá trị gcd mặc dù nó đã tồn tại. 

Coi như:```
3
6 10 15
```Gcd toàn cầu là`1`, nhưng không có phần tử nào bằng`1`. 

Câu trả lời không đơn giản`n-1 = 2`. Trước khi chúng ta có thể lan tỏa giá trị`1`, trước tiên chúng ta phải tạo một`1`ở đâu đó. Chi phí tăng thêm này là khó khăn trung tâm của vấn đề. 

Coi như:```
2
6 10
```Gcd toàn cầu là`2`, nhưng không phần tử nào bằng`2`. 

Một thao tác có thể tạo ra`2`từ một trong hai phần tử:```
6 -> gcd(6,10)=2
```cho đi`[2,10]`. 

Thao tác thứ hai thay đổi`10`vào trong`2`. 

Câu trả lời là`2`, không`1`. Việc tạo ra giá trị mục tiêu và truyền bá nó là những nhiệm vụ riêng biệt. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ tự nhiên là coi mỗi cấu hình mảng là một trạng thái và thực hiện BFS cho đến khi tất cả các phần tử trở nên bằng nhau. 

Điều này đúng vì mọi thao tác đều có đơn giá, do đó BFS sẽ tìm số lượng thao tác tối thiểu. 

Thật không may, nó hoàn toàn không khả thi. Ngay cả với các giá trị nhỏ, số lượng mảng có thể tăng theo cấp số nhân với`n`, và mỗi bang có`O(n²)`những chuyển động có thể. Không gian trạng thái bùng nổ gần như ngay lập tức. 

Để tìm thứ gì đó tốt hơn, chúng ta cần hiểu giá trị mà mảng cuối cùng phải chứa. 

Mọi thao tác đều thay thế một giá trị bằng gcd. Hoạt động của Gcd chỉ có thể giảm số lượng, không bao giờ tăng chúng. Do đó, giá trị duy nhất cuối cùng có thể xuất hiện ở mọi nơi là gcd của toàn bộ mảng. 

Cho phép```
G = gcd(a1, a2, ..., an)
```Mảng cuối cùng phải là:```
[G, G, ..., G]
```Bây giờ vấn đề trở nên có cấu trúc hơn nhiều. 

Giả sử một phần tử đã bằng`G`. Sau đó, phần tử đó có thể được sử dụng làm "nguồn". Bất kỳ phần tử nào khác có thể được chuyển đổi thành`G`trong đúng một thao tác:```
x -> gcd(x, G) = G
```Vì vậy nếu đã có`cnt`sự xuất hiện của`G`, chúng ta chỉ cần:```
n - cnt
```hoạt động. 

Trường hợp thú vị là khi không có phần tử nào bằng`G`. 

Trước khi chúng ta có thể lan rộng`G`, trước tiên chúng ta phải tạo một lần xuất hiện của`G`. 

Việc đó đòi hỏi bao nhiêu thao tác? 

Hãy quan sát điều gì xảy ra khi chúng ta liên tục áp dụng các phép toán gcd cho một vị trí. Sau một số thao tác, vị trí đó sẽ trở thành gcd của một số tập hợp con của các phần tử mảng. 

Ví dụ:```
gcd(a3, a7, a2, a5)
```có thể được tạo ra bằng cách liên tục lấy gcd vào một vị trí. 

Vì vậy, việc tạo ra cái đầu tiên`G`tương đương với việc tìm tập con nhỏ nhất có gcd là`G`. 

Nếu một tập hợp con có kích thước`k`, thì việc tạo gcd của nó yêu cầu chính xác`k-1`hoạt động. 

Sau khi có được một bản sao của`G`, chúng tôi vẫn cần`n-1`nhiều thao tác hơn để chuyển đổi tất cả các vị trí còn lại. 

Tổng chi phí:```
(k - 1) + (n - 1)
= n + k - 2
```Vì vậy, vấn đề giảm xuống còn việc tìm kích thước tập hợp con tối thiểu có gcd bằng`G`. 

Vì mỗi giá trị mảng tối đa là 5000 nên chúng tôi có thể chạy DP trên các giá trị gcd. 

Cho phép:```
dp[g] = minimum subset size whose gcd is g
```Xử lý từng phần tử mảng một. Đối với mỗi trạng thái gcd hiện có, hãy bắt đầu một tập hợp con mới với phần tử hiện tại hoặc mở rộng tập hợp con trước đó. 

Số lượng giá trị gcd có thể có chỉ là 5000, làm cho phương pháp này trở nên hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các trạng thái mảng | Hàm mũ | Hàm mũ | Quá chậm | 
| DP về Giá trị GCD | O(n · V) trong đó V=5000 | O(V) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính gcd toàn cầu`G`của toàn bộ mảng. 
2. Đếm xem có bao nhiêu phần tử đã bằng`G`. 
3. Nếu số này dương, hãy trả về`n - count`. 

Mọi phần tử còn lại có thể được chuyển đổi thành`G`trong một thao tác sử dụng sự xuất hiện hiện có của`G`. 
4. Ngược lại, hãy tính kích thước tập con tối thiểu có gcd bằng`G`. 
5. Duy trì mảng DP trong đó`dp[g]`lưu trữ số lượng phần tử được chọn tối thiểu cần thiết để có được gcd`g`. 
6. Đối với mỗi phần tử mảng`x`: 

Bắt đầu một tập hợp con mới chỉ bao gồm`x`. 

Đồng thời mở rộng mọi trạng thái gcd có thể truy cập trước đó`g`sang trạng thái mới`gcd(g, x)`. 
7. Sau khi xử lý tất cả các phần tử, hãy`k = dp[G]`. 

Đây là kích thước tập hợp con tối thiểu có gcd bằng giá trị đích. 
8. Tạo lần xuất hiện đầu tiên của`G`yêu cầu`k - 1`hoạt động. 
9. Một lần`G`tồn tại, việc chuyển đổi tất cả các vị trí còn lại yêu cầu`n - 1`hoạt động. 
10. Trở về:```
n + k - 2
```### Tại sao nó hoạt động 

Giá trị cuối cùng phải là gcd của toàn bộ mảng, vì các phép toán gcd không bao giờ tăng giá trị và mọi phần tử cuối cùng phải chia hết mọi phần tử gốc. 

Nếu xảy ra hiện tượng`G`đã tồn tại, mỗi cái không`G`phần tử có thể được chuyển đổi trực tiếp thành`G`trong một thao tác và không có giải pháp nào nhanh hơn vì mọi phần tử như vậy phải được sửa đổi ít nhất một lần. 

Khi không xảy ra hiện tượng`G`tồn tại, một số chuỗi thao tác phải tạo ra chuỗi thao tác đầu tiên`G`. Bất kỳ giá trị nào thu được bằng cách lấy gcd liên tục tại một vị trí đều bằng gcd của tập hợp con của các phần tử ban đầu. Nếu tập con nhỏ nhất như vậy có kích thước`k`, thì ít nhất`k-1`hoạt động là cần thiết để kết hợp các yếu tố đó, và chính xác`k-1`hoạt động là đủ. 

Sau một bản sao của`G`tồn tại thì mọi vị trí còn lại vẫn cần một thao tác để trở thành`G`, đưa cái khác`n-1`hoạt động. 

DP tính toán kích thước tập hợp con nhỏ nhất cho mọi giá trị gcd có thể tiếp cận để tìm ra giá trị tối ưu`k`. Kết hợp hai giai đoạn mang lại câu trả lời tối thiểu có thể. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

MAXV = 5000
INF = 10**9

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    G = 0
    for x in a:
        G = gcd(G, x)

    cnt = sum(1 for x in a if x == G)

    if cnt > 0:
        print(n - cnt)
        continue

    dp = [INF] * (MAXV + 1)

    for x in a:
        ndp = dp[:]

        ndp[x] = min(ndp[x], 1)

        for g in range(1, MAXV + 1):
            if dp[g] == INF:
                continue

            ng = gcd(g, x)
            ndp[ng] = min(ndp[ng], dp[g] + 1)

        dp = ndp

    k = dp[G]
    print(n + k - 2)
```Phần đầu tiên tính toán gcd toàn cầu`G`. 

Tiếp theo chúng ta đếm xem có bao nhiêu phần tử đã bằng nhau`G`. Nếu ít nhất một tồn tại, câu trả lời là ngay lập tức. Mọi phần tử khác cần chính xác một thao tác và đã có sẵn giá trị nguồn. 

Nhánh thứ hai xử lý trường hợp khó hơn. DP lưu trữ kích thước tập hợp con nhỏ nhất có thể đạt được mỗi giá trị gcd. 

Khi xử lý một số`x`, có hai khả năng. Chúng ta hoặc bắt đầu một tập hợp con mới chỉ chứa`x`, hoặc chúng tôi nối thêm`x`thành một tập con được hình thành trước đó. Việc thêm các thay đổi gcd từ`g`ĐẾN`gcd(g,x)`và tăng kích thước tập hợp con lên một. 

Sử dụng một mảng được sao chép`ndp`là quan trọng. Việc cập nhật DP tại chỗ sẽ cho phép sử dụng cùng một phần tử nhiều lần trong một lần lặp. 

Cuối cùng,`dp[G]`là kích thước tập hợp con tối thiểu có gcd bằng gcd toàn cục. Công thức`n + k - 2`suy ra trực tiếp từ chứng minh trên. 

Tất cả các giá trị vẫn ở mức dưới 5000, do đó không gian trạng thái DP rất nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
12 20 30
```Gcd toàn cầu:```
G = 2
```Không có phần tử nào bằng`2`. 

| Bước | Trạng thái gcd có liên quan | Kích thước tập hợp con tối thiểu | 
| --- | --- | --- | 
| Bắt đầu với 12 | 12 | 1 | 
| Thêm 20 | gcd(12,20)=4 | 2 | 
| Thêm 30 | gcd(4,30)=2 | 3 | 

Tập con nhỏ nhất có gcd`2`có kích thước:```
k = 3
```Trả lời:```
n + k - 2
= 3 + 3 - 2
= 4
```Điều này phù hợp với mẫu. 

### Ví dụ 2 

đầu vào:```
6
1 9 1 9 8 1
```Gcd toàn cầu:```
G = 1
```Số lượng hiện có:```
cnt = 3
```| Biến | Giá trị | 
| --- | --- | 
| n | 6 | 
| G | 1 | 
| cnt | 3 | 

Trả lời:```
n - cnt
= 6 - 3
= 3
```Mỗi phần tử không phải là một có thể được biến thành`1`trong một thao tác duy nhất bằng cách sử dụng bất kỳ`1`. 

Ví dụ này minh họa trường hợp phím tắt trong đó không cần DP. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 5000) | Đối với mỗi phần tử, chúng tôi quét tất cả các giá trị gcd có thể có | 
| Không gian | O(5000) | Mảng DP trên các giá trị gcd | 

Vì tổng của tất cả`n`trên các trường hợp thử nghiệm nhiều nhất là 5000, tổng công việc là khoảng 25 triệu thao tác đơn giản trong trường hợp xấu nhất. Điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import gcd

def solve():
    input = sys.stdin.readline
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        G = 0
        for x in a:
            G = gcd(G, x)

        cnt = sum(1 for x in a if x == G)

        if cnt:
            print(n - cnt)
            continue

        INF = 10**9
        dp = [INF] * 5001

        for x in a:
            ndp = dp[:]
            ndp[x] = min(ndp[x], 1)

            for g in range(1, 5001):
                if dp[g] == INF:
                    continue

                ng = gcd(g, x)
                ndp[ng] = min(ndp[ng], dp[g] + 1)

            dp = ndp

        print(n + dp[G] - 2)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue()

# provided samples
assert run(
"""3
3
12 20 30
6
1 9 1 9 8 1
3
6 14 15
"""
) == "4\n3\n3\n"

# minimum size
assert run(
"""1
1
7
"""
) == "0\n"

# already equal
assert run(
"""1
5
4 4 4 4 4
"""
) == "0\n"

# global gcd already present
assert run(
"""1
3
2 4 6
"""
) == "2\n"

# need to create gcd first
assert run(
"""1
2
6 10
"""
) == "2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[7]`|`0`| Mảng một phần tử | 
|`[4,4,4,4,4]`|`0`| Đã bình đẳng | 
|`[2,4,6]`|`2`| Gcd toàn cầu đã tồn tại | 
|`[6,10]`|`2`| Phải tạo gcd trước khi lan truyền | 

## Vỏ cạnh 

Hãy xem xét:```
1
3
2 2 2
```Gcd toàn cầu là`2`và cả ba phần tử đều bằng nhau`2`. 

Số phần tử bằng`G`là`3`, do đó thuật toán trả về:```
3 - 3 = 0
```Không có hoạt động cần thiết. 

Coi như:```
1
3
2 4 6
```Gcd toàn cầu là`2`. 

Một phần tử đã bằng`2`, Vì thế:```
cnt = 1
```Câu trả lời trở thành:```
3 - 1 = 2
```Giai đoạn DP bị bỏ qua hoàn toàn. Điều này tránh được sai lầm phổ biến là trả thêm chi phí để tạo ra giá trị đã tồn tại. 

Coi như:```
1
3
6 10 15
```Gcd toàn cầu là`1`, nhưng không có phần tử nào bằng`1`. 

DP tìm thấy:```
gcd(6,10,15) = 1
```và không có tập con nhỏ hơn có gcd`1`, Vì thế:```
k = 3
```Câu trả lời là:```
3 + 3 - 2 = 4
```Thuật toán tính toán chính xác cho cả hai giai đoạn: tạo giai đoạn đầu tiên`1`, sau đó lan truyền nó. 

Coi như:```
1
2
6 10
```Gcd toàn cầu là`2`. 

Không có phần tử nào bằng`2`. 

DP tìm thấy một tập hợp con có kích thước`2`gcd của ai`2`, Vì thế:```
k = 2
```Câu trả lời trở thành:```
2 + 2 - 2 = 2
```Một thao tác tạo ra một`2`và một thao tác nữa sẽ chuyển đổi phần tử còn lại. Điều này xác nhận rằng việc tạo ra giá trị mục tiêu và truyền bá nó được tính riêng.
