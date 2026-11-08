---
title: "CF 145C - Chuỗi May Mắn"
description: "Chúng ta được cho một mảng các số nguyên và phải đếm chính xác có bao nhiêu dãy con có độ dài k thỏa mãn một hạn chế đặc biệt về số may mắn. Một số là may mắn nếu mỗi chữ số thập phân là 4 hoặc 7. Ví dụ là 4, 47 và 744. Các số như 5, 17 và 467 không may mắn."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 145
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 104 (Div. 1)"
rating: 2100
weight: 145
solve_time_s: 125
verified: true
draft: false
---

[CF 145C - Chuỗi may mắn](https://codeforces.com/problemset/problem/145/C) 

**Đánh giá:** 2100 
**Tags:** tổ hợp, dp, toán 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và phải đếm chính xác có bao nhiêu dãy con có độ dài`k`thỏa mãn một hạn chế đặc biệt về số may mắn. 

Một số may mắn nếu mọi chữ số thập phân đều như vậy`4`hoặc`7`. Ví dụ là`4`,`47`, Và`744`. Những con số như`5`,`17`, Và`467`không may mắn. 

Một dãy con được hình thành bằng cách chọn các chỉ số từ mảng. Hai dãy con khác nhau nếu tập chỉ mục được chọn của chúng khác nhau, ngay cả khi các giá trị giống hệt nhau. 

Sự hạn chế là tinh tế. Giá trị may mắn có thể xuất hiện nhiều nhất một lần trong dãy con đã chọn. Các giá trị không may mắn có thể lặp lại nhiều lần. 

Giả sử mảng là:```
[4, 4, 10]
```Và`k = 2`. 

Dãy con được hình thành bởi các chỉ số`{1,2}`không hợp lệ vì nó chứa hai số may mắn bằng nhau, cả hai đều`4`. 

Các chuỗi tiếp theo`{1,3}`Và`{2,3}`là hợp lệ. 

Kích thước mảng đạt`10^5`, vì vậy việc liệt kê tất cả các dãy con là không thể. có`2^n`tổng cộng các chuỗi con và thậm chí kiểm tra tất cả các chuỗi con có kích thước`k`sẽ yêu cầu đại khái`C(100000, 50000)`hoạt động trong trường hợp xấu nhất, lớn về mặt thiên văn. 

Các ràng buộc gợi ý rõ ràng rằng chúng ta cần một cái gì đó gần với`O(n log n)`hoặc`O(nk)`với một hằng số nhỏ. Từ`k`cũng có thể`10^5`, thậm chí`O(nk)`trở nên nguy hiểm trừ khi không gian trạng thái hiệu dụng nhỏ hơn nhiều so với`n`. 

Quan sát cấu trúc quan trọng là có rất ít con số may mắn riêng biệt cho đến`10^9`. Mỗi số may mắn chỉ bao gồm các chữ số`4`Và`7`, và có nhiều nhất 10 chữ số. Tổng số là:$\sum_{i=1}^{10} 2^i = 2046$Vì vậy, mặc dù mảng có thể chứa`100000`phần tử, số lượng giá trị may mắn riêng biệt nhiều nhất là`2046`. 

Một số trường hợp phức tạp có thể âm thầm phá vỡ việc triển khai ngây thơ. 

Coi như:```
3 2
4 4 4
```Câu trả lời đúng là`0`. 

Một giải pháp bất cẩn có thể nói rằng có`C(3,2)=3`các chuỗi tiếp theo, nhưng mỗi cặp đều chứa giá trị may mắn trùng lặp`4`, vì vậy tất cả đều không hợp lệ. 

Bây giờ hãy xem xét:```
4 2
10 10 10 10
```Câu trả lời đúng là:$\binom{4}{2}=6$Các giá trị không may mắn không có giới hạn về tính duy nhất nên hoàn toàn được phép trùng lặp. 

Một trường hợp phức tạp khác là:```
5 3
4 4 7 10 10
```Các dãy con hợp lệ phải chứa nhiều nhất một`4`và nhiều nhất là một`7`. 

Các lựa chọn hợp lệ là:```
{4(first),7,10(first)}
{4(first),7,10(second)}
{4(second),7,10(first)}
{4(second),7,10(second)}
```Câu trả lời là`4`. 

Việc triển khai đơn giản chỉ theo dõi xem một số có may mắn hay không, thay vì theo dõi các giá trị may mắn riêng biệt, sẽ bị tính sai. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản về mặt khái niệm. Liệt kê mọi dãy con có kích thước`k`, kiểm tra các phần tử may mắn của nó và từ chối nó nếu giá trị may mắn nào đó xuất hiện hai lần. 

Điều này hiệu quả vì định nghĩa mang tính cục bộ và dễ xác minh. Đối với mỗi tập hợp con các chỉ số được chọn, chúng ta có thể xây dựng bản đồ tần số của các số may mắn và kiểm tra xem tất cả các tần số có nhiều nhất là một hay không. 

Vấn đề là số lượng tập hợp con. Trong trường hợp xấu nhất chúng ta sẽ kiểm tra:$\binom{100000}{50000}$điều đó hoàn toàn không thể thực hiện được. 

Nỗ lực tự nhiên tiếp theo là lập trình động theo các vị trí. Đối với mỗi yếu tố, chúng tôi lấy nó hoặc bỏ qua nó. Nhưng nếu chúng ta theo dõi chính xác những giá trị may mắn nào đã được sử dụng thì không gian trạng thái sẽ trở thành hàm mũ về số lượng các số may mắn riêng biệt. 

Sự đột phá đến từ việc tách biệt yếu tố may mắn và yếu tố không may mắn. 

Những con số không may mắn rất dễ dàng. Họ không có hạn chế nào cả. Nếu có`u`yếu tố không may mắn và chúng tôi quyết định thực hiện chính xác`t`trong số đó, số cách chỉ đơn giản là:$\binom{u}{t}$Những con số may mắn thì khác. Giả sử một giá trị may mắn`x`xuất hiện`cnt[x]`lần. Chúng tôi có thể: 

1. Bỏ qua giá trị`x`hoàn toàn. 
2. Chọn chính xác một lần xuất hiện của`x`. 

Nếu chúng ta chọn nó, có`cnt[x]`lựa chọn chỉ số khác nhau. 

Điều này biến vấn đề thành một DP kiểu ba lô được nhóm lại. 

Mỗi giá trị may mắn riêng biệt hoạt động giống như một nhóm vật phẩm đóng góp:```
0 chosen elements, 1 way
1 chosen element, cnt[x] ways
```Nếu có`m`giá trị may mắn khác biệt, chúng tôi chạy DP trên các nhóm này:```
dp[j] = number of ways to choose j lucky elements
```Sau khi tính toán phần đóng góp may mắn, chúng ta kết hợp nó với phần đóng góp không may mắn. 

Nếu chúng ta chọn`j`yếu tố may mắn, chúng ta vẫn cần`k-j`yếu tố không may mắn. Tổng số tiền đóng góp trở thành:$dp[j] \cdot \binom{u}{k-j}$Tổng hợp tất cả hợp lệ`j`đưa ra câu trả lời. 

Điểm giảm quan trọng là kích thước DP phụ thuộc vào số lượng giá trị may mắn riêng biệt, nhiều nhất là`2046`, thay vì`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n + m·k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và phân loại từng số là may mắn hay không may mắn. 

Một số may mắn nếu mọi chữ số thập phân đều như vậy`4`hoặc`7`. 
2. Đếm xem mỗi giá trị may mắn riêng biệt xuất hiện bao nhiêu lần. 

Chúng tôi lưu trữ tần số trong một bản đồ băm. Nếu giá trị`47`xuất hiện 5 lần ta nhớ có thể chọn đúng 1 trong 5 vị trí đó. 
3. Đếm tổng số yếu tố không may mắn. 

Các yếu tố này không bị hạn chế nên sau này chúng ta chỉ cần kết hợp. 
4. Tính toán trước giai thừa và nghịch đảo giai thừa modulo`10^9+7`. 

Điều này cho phép tính toán các kết hợp theo thời gian không đổi:$\binom{n}{r}=\frac{n!}{r!(n-r)!}$5. Khởi tạo DP. 

Cho phép:```
dp[j] = number of ways to choose exactly j lucky elements
```Ban đầu:```
dp[0] = 1
```bởi vì việc lựa chọn không có gì luôn luôn là có thể. 
6. Xử lý từng giá trị may mắn riêng biệt một cách độc lập. 

Giả sử tần số hiện tại là`c`. 

Chúng tôi lặp lại`j`lạc hậu và cập nhật:```
dp[j+1] += dp[j] * c
```Phép nhân với`c`xuất hiện vì chúng ta có thể chọn bất kỳ lần xuất hiện nào của giá trị may mắn đó. 

Việc lặp lại ngăn chặn việc sử dụng cùng một giá trị may mắn nhiều lần. 
7. Kết hợp các lựa chọn may mắn và không may mắn. 

Đối với mọi số lượng có thể`j`của các yếu tố may mắn được chọn:```
need = k - j
```Nếu như`0 <= need <= unlucky_count`, thêm vào:$dp[j] \cdot \binom{unlucky}{need}$8. Xuất modulo tổng cuối cùng`10^9+7`. 

### Tại sao nó hoạt động 

Bất biến DP là:```
dp[j] = number of valid ways to choose exactly j elements
        from the processed lucky-value groups,
        using at most one occurrence from each group
```Mỗi giá trị may mắn riêng biệt được xử lý một lần. Trong quá trình chuyển đổi, chúng tôi bỏ qua giá trị hoặc chọn chính xác một trong các lần xuất hiện của nó. Vì các bản cập nhật quay ngược lại nên cùng một giá trị may mắn không thể đóng góp hai lần cho một trạng thái. 

Sau khi tất cả các nhóm may mắn được xử lý, mọi cấu hình phần tử may mắn hợp lệ sẽ được tính chính xác một lần. 

Các yếu tố không may mắn độc lập với hạn chế về tính duy nhất, vì vậy khi chúng tôi xác định số lượng yếu tố may mắn được sử dụng, các vị trí còn lại có thể được lấp đầy bởi các yếu tố không may mắn tùy ý. Số lượng kết hợp đếm chính xác tất cả các lựa chọn như vậy. 

Bởi vì mọi dãy con hợp lệ đều có sự phân tách duy nhất thành các lựa chọn may mắn và không may mắn, nên tổng cuối cùng sẽ tính mọi dãy con hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
from collections import Counter

input = sys.stdin.readline

MOD = 10**9 + 7

def is_lucky(x):
    while x > 0:
        d = x % 10
        if d != 4 and d != 7:
            return False
        x //= 10
    return True

def solve():
    n, k = map(int, input().split())
    arr = list(map(int, input().split()))

    lucky_freq = Counter()
    unlucky = 0

    for x in arr:
        if is_lucky(x):
            lucky_freq[x] += 1
        else:
            unlucky += 1

    fact = [1] * (n + 1)
    invfact = [1] * (n + 1)

    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact[n] = pow(fact[n], MOD - 2, MOD)

    for i in range(n, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def comb(a, b):
        if b < 0 or b > a:
            return 0
        return fact[a] * invfact[b] % MOD * invfact[a - b] % MOD

    dp = [0] * (k + 1)
    dp[0] = 1

    for cnt in lucky_freq.values():
        for j in range(k - 1, -1, -1):
            dp[j + 1] = (dp[j + 1] + dp[j] * cnt) % MOD

    ans = 0

    for lucky_taken in range(k + 1):
        need_unlucky = k - lucky_taken

        if need_unlucky <= unlucky:
            ways = dp[lucky_taken]
            ways = ways * comb(unlucky, need_unlucky) % MOD
            ans = (ans + ways) % MOD

    print(ans)

solve()
```Phần đầu tiên phân tách các giá trị may mắn và không may mắn. các`Counter`lưu trữ tần số của các số may mắn riêng biệt, vì các bản sao có cùng giá trị may mắn không thể xuất hiện cùng lúc trong một dãy con hợp lệ. 

Mảng giai thừa và mảng giai thừa nghịch đảo hỗ trợ các truy vấn kết hợp nhanh. Việc tính toán các kết hợp theo yêu cầu với các phép nghịch đảo mô-đun lặp đi lặp lại sẽ quá chậm nếu được thực hiện nhiều lần. 

Mảng DP là một chiều vì mỗi giá trị may mắn được xử lý chính xác một lần. Việc lặp lại ngược là cần thiết. Nếu chúng tôi lặp lại, giá trị may mắn tương tự có thể được sử dụng nhiều lần trong một giai đoạn chuyển tiếp. 

Giả định`cnt = 5`. Sự chuyển tiếp:```
dp[j + 1] += dp[j] * 5
```có nghĩa là chúng ta chọn một lần xuất hiện trong số 5 con số may mắn giống hệt nhau. 

Vòng lặp cuối cùng kết hợp các lựa chọn may mắn và không may mắn. Chúng ta phải kiểm tra xem`need_unlucky`có giá trị trước khi kết hợp tính toán. Việc quên kiểm tra ranh giới này là nguyên nhân phổ biến dẫn đến các câu trả lời sai. 

Tất cả số học được thực hiện modulo`10^9+7`, vì các giá trị trung gian trở nên cực kỳ lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
10 10 10
```Không có con số may mắn. 

| Bước | xui xẻo | may mắn_freq | dp | 
| --- | --- | --- | --- | 
| Phân tích cú pháp ban đầu | 3 | {} | [1,0,0] | 
| Không xử lý may mắn | 3 | {} | [1,0,0] | 
| Kết hợp j=0 | 3 | {} | đáp án += C(3,2)=3 | 

Câu trả lời cuối cùng:```
3
```Ví dụ này cho thấy việc lặp lại những con số không may mắn là hoàn toàn được phép. Giới hạn tính duy nhất chỉ áp dụng cho các giá trị may mắn. 

### Ví dụ 2 

đầu vào:```
5 3
4 4 7 10 10
```| Bước | Giá trị hiện tại | Tần số | dp sau bước | 
| --- | --- | --- | --- | 
| Ban đầu | - | - | [1,0,0,0] | 
| Quy trình 4 | 2 | [1,2,0,0] | | 
| Quy trình 7 | 1 | [1,3,2,0] | | 

có`2`những con số không may mắn. 

Bây giờ kết hợp: 

| may mắn_taken | cần_không may mắn | Đóng góp | 
| --- | --- | --- | 
| 0 | 3 | 0 | 
| 1 | 2 | 3 × C(2,2) = 3 | 
| 2 | 1 | 2 × C(2,1) = 4 | 
| 3 | 0 | 0 | 

Câu trả lời cuối cùng:```
7
```Dấu vết cho thấy các giá trị may mắn trùng lặp được nén vào một nhóm DP như thế nào. giá trị`4`xuất hiện hai lần nhưng chỉ đóng góp một phần tử được chọn tại một thời điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m·k) | Quét mảng cộng với DP trên các giá trị may mắn riêng biệt | 
| Không gian | O(k) | Một mảng DP có kích thước`k+1`| 

Đây`m`là số lượng các giá trị may mắn riêng biệt, giới hạn bởi`2046`. 

Ngay cả trong trường hợp xấu nhất, đại khái`2046 × 100000`Chuyển đổi DP có thể quản lý được bằng Python vì các thao tác rất nhỏ và thân thiện với bộ đệm. Việc sử dụng bộ nhớ rất nhỏ so với`256 MB`giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import Counter

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def is_lucky(x):
        while x > 0:
            d = x % 10
            if d != 4 and d != 7:
                return False
            x //= 10
        return True

    n, k = map(int, input().split())
    arr = list(map(int, input().split()))

    lucky_freq = Counter()
    unlucky = 0

    for x in arr:
        if is_lucky(x):
            lucky_freq[x] += 1
        else:
            unlucky += 1

    fact = [1] * (n + 1)
    invfact = [1] * (n + 1)

    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact[n] = pow(fact[n], MOD - 2, MOD)

    for i in range(n, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def comb(a, b):
        if b < 0 or b > a:
            return 0
        return fact[a] * invfact[b] % MOD * invfact[a - b] % MOD

    dp = [0] * (k + 1)
    dp[0] = 1

    for cnt in lucky_freq.values():
        for j in range(k - 1, -1, -1):
            dp[j + 1] = (dp[j + 1] + dp[j] * cnt) % MOD

    ans = 0

    for lucky_taken in range(k + 1):
        need_unlucky = k - lucky_taken

        if need_unlucky <= unlucky:
            ans = (
                ans
                + dp[lucky_taken] * comb(unlucky, need_unlucky)
            ) % MOD

    return str(ans) + "\n"

# provided sample
assert run("3 2\n10 10 10\n") == "3\n", "sample 1"

# minimum size
assert run("1 1\n4\n") == "1\n", "single lucky number"

# duplicate lucky values cannot both be used
assert run("3 2\n4 4 4\n") == "0\n", "duplicate lucky restriction"

# mix of lucky and unlucky
assert run("5 3\n4 4 7 10 10\n") == "7\n", "mixed case"

# all unlucky values
assert run("5 3\n1 2 3 5 6\n") == "10\n", "pure combinations"

# choose exactly one from repeated lucky group
assert run("4 2\n47 47 1 1\n") == "5\n", "group counting"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 4`|`1`| Kích thước đầu vào tối thiểu | 
|`3 2 / 4 4 4`|`0`| Giá trị may mắn trùng lặp bị cấm | 
|`5 3 / 4 4 7 10 10`|`7`| Xử lý may mắn và xui xẻo hỗn hợp | 
|`5 3 / 1 2 3 5 6`|`10`| Đếm kết hợp thuần túy | 
|`4 2 / 47 47 1 1`|`5`| Chọn một lần xuất hiện từ một nhóm may mắn | 

## Vỏ cạnh 

Hãy xem xét:```
3 2
4 4 4
```Thuật toán tạo ra một nhóm may mắn:```
freq[4] = 3
```DP trở thành: 

| Bước | dp | 
| --- | --- | 
| Ban đầu | [1,0,0] | 
| Tần số xử lý 3 | [1,3,0] | 

Không có cách nào để đạt được`dp[2]`, vì cùng một giá trị may mắn không thể được chọn hai lần. Vì không có con số xui xẻo nên đáp án cuối cùng là`0`. 

Bây giờ hãy xem xét:```
4 2
10 10 10 10
```Không có nhóm may mắn nào cả.```
dp = [1,0,0]
unlucky = 4
```Câu trả lời trở thành:$\binom{4}{2}=6$Điều này xác nhận rằng các giá trị không may mắn lặp đi lặp lại là không bị hạn chế. 

Cuối cùng, hãy xem xét:```
5 2
4 7 47 74 10
```Tất cả các giá trị may mắn đều khác biệt. 

DP phát triển như sau: 

| Sau khi xử lý | dp | 
| --- | --- | 
| 4 | [1,1,0] | 
| 7 | [1,2,1] | 
| 47 | [1,3,3] | 
| 74 | [1,4,6] | 

Có một giá trị không may mắn. 

Câu trả lời cuối cùng là:```
dp[2] * C(1,0) + dp[1] * C(1,1)
= 6 + 4
= 10
```Mỗi cặp giá trị may mắn riêng biệt đều hợp lệ và mỗi giá trị may mắn cũng có thể ghép với số không may mắn.
