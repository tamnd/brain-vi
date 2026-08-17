---
title: "CF 102348C - Đá cẩm thạch"
description: "Chúng ta có một hàng (n) viên bi, trong đó mỗi viên bi có một trong tối đa 20 màu. Chúng ta có thể hoán đổi các viên bi lân cận và mục tiêu là làm cho mọi màu chiếm một khối liền kề. Bản thân các khối có thể xuất hiện theo bất kỳ thứ tự nào."
date: "2026-08-16T01:38:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102348
codeforces_index: "C"
codeforces_contest_name: "ICPC 2019-2020 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102348
solve_time_s: 1338
verified: false
draft: false
---

[CF 102348C - Viên bi](https://codeforces.com/problemset/problem/102348/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 22 phút 18 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng (n) viên bi, trong đó mỗi viên bi có một trong tối đa 20 màu. Chúng ta có thể hoán đổi các viên bi lân cận và mục tiêu là làm cho mọi màu chiếm một khối liền kề. Bản thân các khối có thể xuất hiện theo bất kỳ thứ tự nào. 

Khó khăn chính là thứ tự cuối cùng của màu sắc không được xác định. Khi thứ tự đó được khắc phục, vấn đề sẽ trở thành vấn đề hoán đổi liền kề tối thiểu tiêu chuẩn. Nhiệm vụ thực sự là chọn hoán vị tốt nhất của các màu riêng biệt. 

Gọi các màu riêng biệt là (k) màu. Vì mỗi màu nằm trong khoảng từ 1 đến 20, (k\leq20), mặc dù (n) có thể lớn bằng (4\cdot10^5). Giới hạn nhỏ về số lượng màu này là lý do khiến giải pháp lập trình động tập hợp con trở nên khả thi. Một nghiệm đa thức theo (n) và chỉ hàm mũ trong (k) là thực tế, trong khi mọi số mũ trong (n) là hoàn toàn không thể. 

Giới hạn (n\leq4\cdot10^5) cũng loại trừ việc mô phỏng hoán đổi liên tục. Một phép biến đổi đơn lẻ có thể yêu cầu (\frac{n(n-1)}2) hoán đổi liền kề, tối đa (n) là 

[ 
\frac{400000\cdot399999}{2}=79,999,800,000. 
] 

Vì vậy, ngay cả việc xử lý một sự sắp xếp mục tiêu đặc biệt tồi tệ bằng các hoán đổi rõ ràng cũng vượt xa giới hạn thời gian. 

Có một số trường hợp khó xử lý. Nếu tất cả các viên bi đã tạo thành khối thì câu trả lời là 0 ngay cả khi các màu không được sắp xếp theo số. Ví dụ,```
4
2 2 1 1
```có câu trả lời (0). Một giải pháp giả định các khối phải được sắp xếp theo giá trị màu sẽ trả sai khi thay đổi (2,1) thành (1,2). 

Nếu chỉ xuất hiện một màu thì không cần thực hiện thao tác nào bất kể (n). Ví dụ,```
4
20 20 20 20
```có câu trả lời (0). Giải pháp phân bổ mù quáng các trạng thái cho tất cả 20 màu có thể có có thể lãng phí thời gian đáng kể, trong khi việc triển khai đúng cách sẽ nén các màu thực sự xuất hiện. 

Màu sắc lặp đi lặp lại cũng rất đáng kể. Vì```
4
1 2 1 2
```câu trả lời là (1), vì đổi chỗ hai viên bi ở giữa sẽ có (1,1,2,2). Coi các viên bi riêng lẻ như những đồ vật được sắp xếp độc lập có thể đếm những sự hoán đổi không cần thiết giữa các viên bi cùng màu. 

Cuối cùng, các giá trị màu không bị giới hạn trong một phạm vi nhỏ liên tiếp như (1,\dots,k). Vì```
4
20 1 20 1
```câu trả lời lại là (1). Việc triển khai sẽ nén các màu thực sự xuất hiện thay vì sử dụng các giá trị số của chúng làm chỉ số DP. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp bắt đầu bằng cách quan sát rằng sự sắp xếp hợp lệ cuối cùng hoàn toàn được xác định bởi thứ tự các khối màu của nó. Nếu các màu riêng biệt là (c_1,c_2,\ldots,c_k), thì có thể có (k!) thứ tự. 

Đối với một thứ tự cố định, chúng ta có thể gán cho mỗi màu thứ hạng khối mong muốn và quét mảng ban đầu. Mỗi cặp bi có thứ tự tương đối khác với thứ tự khối mong muốn sẽ đóng góp chính xác một lần hoán đổi liền kề. Đây là cách giải thích đảo ngược thông thường của các giao dịch hoán đổi liền kề. Do đó, một đơn hàng cố định có thể được đánh giá theo (O(n)), đưa ra thời gian (O(k!,n)) cho tất cả các đơn hàng có thể. 

Tại (k=20), điều này là vô vọng. có 

[ 
20! = 2.432.902.008.176.640.000 
] 

các đơn đặt hàng có thể. Ngay cả việc quét các viên bi (4\cdot10^5) một lần cho mỗi đơn hàng cũng cần khoảng 

[ 
20!\cdot4\cdot10^5 \khoảng 9,73\cdot10^{23} 
] 

thăm phần tử. 

Quan sát hữu ích là chi phí của một đơn hàng có thể được phân tách theo từng cặp. Giả sử màu (x) được đặt trước màu (y) trong dãy cuối cùng. Mọi lần xuất hiện của (y) ban đầu xuất hiện trước sự xuất hiện của (x) tạo thành một cặp có thứ tự phải đảo ngược. Mỗi cặp như vậy có giá đúng bằng một lần hoán đổi liền kề. 

Xác định 

[ 
chi phí[x][y] 
] 

là số cặp trong đó (x) xuất hiện trước a (y) trong mảng ban đầu. 

Bây giờ hãy tưởng tượng việc xây dựng thứ tự màu cuối cùng từ trái sang phải. Nếu chúng ta nối màu (c) sau một tập hợp (S) gồm các màu đã được chọn thì mọi màu (x\in S) bắt buộc phải xuất hiện trước (c). Các giao dịch hoán đổi được đưa ra bằng cách đặt (c) sau khi tất cả chúng đều chính xác 

[ 
\sum_{x\in S} giá[c][x]. 
] 

Phần quan trọng là điều này chỉ phụ thuộc vào (S) và (c), không phụ thuộc vào thứ tự màu sắc của (S) được sắp xếp trước đó. Điều đó mang lại cho chúng ta cấu trúc con tối ưu cần thiết cho tập hợp con DP. 

Chúng tôi sử dụng bitmask để thể hiện tập hợp màu đã được đặt. Trạng thái DP ghi lại chi phí tối thiểu để sắp xếp chính xác tập hợp đó làm tiền tố. Việc thêm một màu mới sẽ tạo ra sự chuyển tiếp. 

Sự so sánh là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(k!,n)) | (O(k+n)) | Quá chậm | 
| Tối ưu | (O(nk+k2^k)) | (O(k2^k)) | Đã chấp nhận | 

Ở đây (k\leq20), do đó phần mũ được giới hạn bởi khoảng (20\cdot2^{20}), có thể quản lý được. 

## Hướng dẫn thuật toán 

1. Nén các màu thực sự xuất hiện trong mảng thành các chỉ số (0,1,\ldots,k-1). Chỉ những màu này mới có thể xuất hiện dưới dạng khối cuối cùng và (k\leq20). 
2. Xây dựng ma trận chi phí theo cặp. Với mỗi cặp màu (x,y), gọi (cost[x][y]) là số cặp vị trí (i<j) sao cho (a_i=x) và (a_j=y). Trong quá trình quét từ trái sang phải, khi màu hiện tại là (y), mỗi (x) nhìn thấy trước đó sẽ đóng góp một vào (cost[x][y]). Vì có tối đa 20 màu nên chi phí tiền xử lý này (O(nk)). 
3. Xem xét thứ tự cuối cùng của các màu và tập trung vào thời điểm màu cuối cùng (c) được thêm vào. Tất cả các màu khác tạo thành một tập hợp (S) và (c\notin S). Chi phí tăng thêm do thêm (c) là 

[ 
cộng(c,S)=\sum_{x\in S}chi phí[c][x]. 
] 

Công thức này đếm chính xác các cặp có thứ tự tương đối sai khi (c) bắt buộc phải đứng sau mỗi màu trong (S). 

1. Xác định (dp[mask]) là số lần hoán đổi liền kề tối thiểu cần thiết để sắp xếp chính xác các màu được biểu thị bằng`mask`thành một tiền tố hợp lệ của hàng cuối cùng. Màu sắc trong mặt nạ có thể xuất hiện theo thứ tự nào mang lại chi phí tối thiểu. 
2. Đối với mỗi mặt nạ không trống, hãy thử từng màu (c) có trong đó làm màu cuối cùng của tiền tố đó. Nếu như`prev = mask`không có (c), quá trình chuyển đổi là 

\min_{c\in mặt nạ} 
\left( 
dp[trước] 
+ 
\sum_{x\in prev}chi phí[c][x] 
\đúng). 
] 

Màu trước đó đã được tối ưu theo định nghĩa của DP và thuật ngữ được thêm vào sẽ tính cho mọi cặp liên quan đến màu mới được thêm vào. 

1. Để duy trì quá trình triển khai Python nhanh chóng, hãy tính trước tổng tập hợp con 

[ 
sum_c[S]=\sum_{x\in S}chi phí[c][x] 
] 

với mọi màu (c) và mọi tập con (S) không chứa (c). Mỗi bảng như vậy có (2^{k-1}) mục nhập, vì vậy tất cả chúng cùng chứa các giá trị (k2^{k-1}). Bản thân các tổng tập hợp con được tính bằng cách loại bỏ một bit đã đặt: 

[ 
sum_c[S]=sum_c[S\setminus{x}]+chi phí[c][x]. 
] 

Việc triển khai lưu trữ các giá trị này trong mảng 64 bit nhỏ gọn. 

1. Lặp lại qua tất cả các mặt nạ và áp dụng chuyển tiếp cho mọi màu cuối cùng có thể. Câu trả lời là`dp[(1 << k) - 1]`, bởi vì mặt nạ đó chứa mọi màu sắc riêng biệt. 

Tại sao nó hoạt động: sửa mọi thứ tự cuối cùng của màu sắc. Mỗi cặp viên bi có màu sắc khác nhau sẽ giữ nguyên thứ tự tương đối hoặc đảo ngược nó. Một cặp có thứ tự ban đầu không giống với thứ tự khối cuối cùng phải giao nhau đúng một lần và việc hoán đổi liền kề có thể thay đổi thứ tự tương đối của chỉ hai viên bi được hoán đổi. Do đó, số lượng các cặp đảo ngược như vậy chính xác là số lần hoán đổi liền kề tối thiểu cho thứ tự cuối cùng đó. 

DP xem xét mọi yêu cầu cuối cùng có thể xảy ra một cách ngầm định. Khi màu (c) được thêm vào, sự đóng góp của nó chỉ phụ thuộc vào màu nào đã có trước nó chứ không phụ thuộc vào thứ tự bên trong của chúng. Do đó, mọi thứ tự được thể hiện bằng một chuỗi các chuyển đổi DP và mỗi chuyển đổi sẽ thêm chính xác các giao dịch hoán đổi liên quan đến khối mới được thêm vào. Do đó, lấy mức tối thiểu trên tất cả các màu cuối cùng có thể có sẽ mang lại chi phí tối thiểu cho tất cả các đơn hàng khối hợp lệ. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve_case(a):
    # Compress the colors that actually occur.
    colors = sorted(set(a))
    k = len(colors)
    index = {x: i for i, x in enumerate(colors)}
    b = [index[x] for x in a]

    # cost[x][y] = number of pairs (x before y) in the original array.
    cost = [[0] * k for _ in range(k)]
    seen = [0] * k

    for y in b:
        for x in range(k):
            cost[x][y] += seen[x]
        seen[y] += 1

    if k == 1:
        return 0

    # For every color c, store subset sums for subsets not containing c.
    # Such a subset has k-1 bits, hence 2^(k-1) entries.
    half = 1 << (k - 1)
    total_sum_entries = k * half
    subset_sum = array('q', [0]) * total_sum_entries

    for c in range(k):
        base = c * half

        others = []
        for x in range(k):
            if x != c:
                others.append(x)

        row = cost[c]

        for mask in range(1, half):
            lb = mask & -mask
            bit = lb.bit_length() - 1
            prev = mask ^ lb
            subset_sum[base + mask] = (
                subset_sum[base + prev] + row[others[bit]]
            )

    size = 1 << k
    inf = 10**30
    dp = [inf] * size
    dp[0] = 0

    lower_mask = [(1 << c) - 1 for c in range(k)]

    for mask in range(1, size):
        bits = mask
        best = inf

        while bits:
            lb = bits & -bits
            c = lb.bit_length() - 1
            prev = mask ^ lb

            # Remove bit c from prev to obtain its compressed
            # (k-1)-bit representation.
            compressed = (
                (prev & lower_mask[c])
                | ((prev >> (c + 1)) << c)
            )

            value = dp[prev] + subset_sum[c * half + compressed]

            if value < best:
                best = value

            bits ^= lb

        dp[mask] = best

    return dp[-1]

def main():
    n = int(input())
    a = list(map(int, input().split()))
    print(solve_case(a))

if __name__ == "__main__":
    main()
```Phần đầu tiên của`solve_case`nén màu sắc. Điều này tránh việc phân bổ trạng thái DP cho các màu không bao giờ xuất hiện và cũng xử lý các giá trị như 20 mà không xử lý chúng một cách đặc biệt. 

các`cost`ma trận lưu trữ số cặp định hướng. Khi màu hiện tại là`y`, mọi màu nhìn thấy trước đó`x`tặng một cặp với`x`trước`y`, Vì thế`cost[x][y]`được tăng lên bởi`seen[x]`. Định hướng này rất quan trọng. Khi`y`được thêm vào sau`x`, các cặp có vấn đề là những cặp mà`y`ban đầu xuất hiện trước đây`x`, được lưu trữ dưới dạng`cost[y][x]`. 

Bảng tổng tập hợp con được lập chỉ mục riêng cho từng màu cuối cùng có thể có. Vì màu cuối cùng (c) không thể có trong mặt nạ tiền nhiệm của nó nên chỉ cần các bit (k-1). Điều này cắt bộ nhớ từ các mục (k2^k) thành các mục (k2^{k-1}). 

Mặt nạ nén loại bỏ bit`c`. Các bit thấp hơn của nó có thể giữ nguyên vị trí của chúng, trong khi mọi bit ở trên`c`được dịch chuyển xuống một. biểu thức```
(prev & lower_mask[c]) | ((prev >> (c + 1)) << c)
```thực hiện chính xác chuyển đổi đó. 

DP bắt đầu với`dp[0] = 0`, vì tiền tố trống không yêu cầu hoán đổi. Đối với mọi mặt nạ không trống, mỗi bit được đặt được coi là màu của khối cuối cùng. Người tiền nhiệm có được với`mask ^ lb`và tổng tập hợp con được tính toán trước sẽ cung cấp chi phí để đặt màu đã chọn sau tất cả các màu trước đó. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn mặc dù câu trả lời có thể lớn hơn nhiều so với (2^{31}). nhỏ gọn`array('q')`được sử dụng cho các tổng tập hợp con vì các số nguyên thông thường của Python mang chi phí bộ nhớ đáng kể cho mỗi đối tượng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
7
3 4 2 3 4 2 2
```Các màu riêng biệt là (3,4,2). Sau khi nén, gọi chúng lần lượt là (0,1,2). 

Một số chi phí cặp có liên quan là 

[ 
chi phí[4][3]=1, 
\qquad 
chi phí[2][3]=1, 
\qquad 
chi phí[2][4]=1. 
] 

Đây chính xác là ba cặp phải vượt qua nếu thứ tự cuối cùng là (3,4,2). 

Đường dẫn DP tối ưu có thể được hiển thị như sau. 

| Mặt nạ | Màu cuối cùng | Mặt nạ trước | Chi phí bổ sung | Giá trị DP | 
| --- | --- | --- | --- | --- | 
|`001`| 3 |`000`| 0 | 0 | 
|`011`| 4 |`001`| (chi phí[4][3]=1) | 1 | 
|`111`| 2 |`011`| (chi phí[2][3]+chi phí[2][4]=2) | 3 | 

Thứ tự khối kết quả là (3,4,2), đưa ra sự sắp xếp cuối cùng```
3 3 4 4 2 2 2
```và tối thiểu là (3). 

Dấu vết này chứng tỏ tại sao chi phí lại gắn liền với màu được thêm vào. Khi số 4 được thêm vào sau số 3, chỉ những cặp có số 4 đứng trước số 3 mới có ý nghĩa. Khi 2 được thêm vào, nó phải vượt qua 3 và 4 trước đó một cách chính xác ở những cặp đó xuất hiện sai thứ tự. 

### Mẫu 2 

Đầu vào là```
5
20 1 14 10 2
```Mỗi màu xảy ra đúng một lần. Vì mỗi màu đã tạo thành một đoạn có độ dài bằng 1 nên mọi thứ tự trong năm khối đơn đều hợp lệ và cách sắp xếp ban đầu không yêu cầu hoán đổi. 

Tất cả chi phí của cặp đều không liên quan đến mức tối ưu vì bản thân chuỗi ban đầu đã là thứ tự khối hợp lệ. 

| Mặt nạ | Màu cuối cùng | Mặt nạ trước | Chi phí bổ sung | Giá trị DP | 
| --- | --- | --- | --- | --- | 
|`00001`| 20 |`00000`| 0 | 0 | 
|`00011`| 1 |`00001`| 0 | 0 | 
|`00111`| 14 |`00011`| 0 | 0 | 
|`01111`| 10 |`00111`| 0 | 0 | 
|`11111`| 2 |`01111`| 0 | 0 | 

Mọi trạng thái DP vẫn ở mức 0, vì vậy câu trả lời cuối cùng là (0). 

Bài tập này xảy ra trong trường hợp mỗi màu xuất hiện một lần. Một giải pháp đúng phải nhận ra rằng các màu đơn lẻ đã liền kề nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nk+k2^k)) | Chi phí xây dựng cặp mất (O(nk)), tổng tập hợp con và DP mất (O(k2^k)) | 
| Không gian | (O(k2^k)) | DP có (2^k) trạng thái và bảng tổng tập hợp con nhỏ gọn có (k2^{k-1}) mục | 

Với (k\leq20), DP chứa tối đa (2^{20}=1.048.576) trạng thái. Quá trình chuyển đổi kiểm tra tối đa 20 màu cuối cùng có thể có trên mỗi trạng thái. Quá trình xử lý trước quét tối đa 20 bộ đếm màu cho mỗi viên bi trong số (4\cdot10^5). Các giới hạn này được thiết kế xung quanh bảng chữ cái màu nhỏ nên thuật toán vẫn khả thi ngay cả ở mức tối đa (n). 

## Trường hợp thử nghiệm```python
import sys
import io
from array import array

def solve(inp: str) -> str:
    reader = io.StringIO(inp).readline
    n = int(reader())
    a = list(map(int, reader().split()))

    colors = sorted(set(a))
    k = len(colors)
    index = {x: i for i, x in enumerate(colors)}
    b = [index[x] for x in a]

    cost = [[0] * k for _ in range(k)]
    seen = [0] * k

    for y in b:
        for x in range(k):
            cost[x][y] += seen[x]
        seen[y] += 1

    if k == 1:
        return "0"

    half = 1 << (k - 1)
    subset_sum = array('q', [0]) * (k * half)

    for c in range(k):
        base = c * half
        others = [x for x in range(k) if x != c]
        row = cost[c]

        for mask in range(1, half):
            lb = mask & -mask
            bit = lb.bit_length() - 1
            prev = mask ^ lb
            subset_sum[base + mask] = (
                subset_sum[base + prev] + row[others[bit]]
            )

    size = 1 << k
    inf = 10**30
    dp = [inf] * size
    dp[0] = 0

    lower_mask = [(1 << c) - 1 for c in range(k)]

    for mask in range(1, size):
        bits = mask
        best = inf

        while bits:
            lb = bits & -bits
            c = lb.bit_length() - 1
            prev = mask ^ lb

            compressed = (
                (prev & lower_mask[c])
                | ((prev >> (c + 1)) << c)
            )

            value = dp[prev] + subset_sum[c * half + compressed]

            if value < best:
                best = value

            bits ^= lb

        dp[mask] = best

    return str(dp[-1])

# Provided samples
assert solve("""7
3 4 2 3 4 2 2
""") == "3", "sample 1"

assert solve("""5
20 1 14 10 2
""") == "0", "sample 2"

assert solve("""13
5 5 4 4 3 5 7 6 5 4 4 6 5
""") == "21", "sample 3"

# Minimum-size input
assert solve("""2
1 1
""") == "0", "minimum size, all equal"

assert solve("""2
1 2
""") == "0", "minimum size, two singleton blocks"

# Repeated colors requiring exactly one swap
assert solve("""4
1 2 1 2
""") == "1", "one crossing pair"

# Boundary color value and non-consecutive colors
assert solve("""4
20 1 20 1
""") == "1", "color value 20"

# Already grouped, but block order is not numerical
assert solve("""6
3 3 1 1 2 2
""") == "0", "arbitrary valid block order"

# Maximum-size input, all marbles have one color
maximum_case = "400000\n" + "20 " * 399999 + "20\n"
assert solve(maximum_case) == "0", "maximum n, all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 1`|`0`| Kích thước tối thiểu và một màu riêng biệt | 
|`2 / 1 2`|`0`| Kích thước tối thiểu với hai khối đơn | 
|`4 / 1 2 1 2`|`1`| Chính xác một lần vượt qua bắt buộc | 
|`4 / 20 1 20 1`|`1`| Giá trị màu biên và nén màu | 
|`6 / 3 3 1 1 2 2`|`0`| Thứ tự khối hợp lệ không cần sắp xếp theo số | 
|`400000 / all 20`|`0`| Tối đa (n), xử lý một màu | 

## Vỏ cạnh 

Nếu mỗi viên bi có cùng màu thì đã có đúng một đoạn liền kề. Vì```
4
20 20 20 20
```quá trình nén tạo ra (k=1). Thuật toán trả về ngay lập tức bằng 0 thay vì xây dựng DP lớn. Điều này tránh được những công việc không cần thiết và sai lầm phổ biến khi cho rằng có ít nhất hai màu tồn tại. 

Nếu mỗi màu xuất hiện một lần thì mỗi viên bi đã là khối liền kề của chính nó. Vì```
5
20 1 14 10 2
```có năm khối đơn, vì vậy hàng ban đầu đã hợp lệ và câu trả lời là 0. Chi phí cặp không bắt buộc bất kỳ đơn hàng cụ thể nào vì bản thân đơn hàng ban đầu là một trong những đơn hàng cuối cùng được phép. 

Nếu các màu được xen kẽ, công thức đếm cặp sẽ nắm bắt chính xác các giao dịch hoán đổi cần thiết. Vì```
4
1 2 1 2
```thứ tự cuối cùng (1,2) yêu cầu viên bi thứ hai và viên bi thứ ba vượt qua. Có chính xác một cặp trong đó số 2 xuất hiện trước số 1 sau đó, do đó DP chỉ định chi phí (1) cho đơn hàng (1,2). Thứ tự khối đảo ngược sẽ có giá cao hơn và câu trả lời là (1). 

Nếu các giá trị màu thưa thớt hoặc nằm ở ranh giới của chúng, việc nén sẽ ngăn chặn các lỗi chỉ mục mảng. Vì```
4
20 1 20 1
```màu sắc được nén thành hai chỉ số DP mặc dù giá trị ban đầu của chúng là 20 và 1. Cần phải thực hiện một lần giao nhau, vì vậy câu trả lời là (1). 

Nếu các khối hiện có theo thứ tự tùy ý thì thứ tự đó phải được chấp nhận. Vì```
6
3 3 1 1 2 2
```mỗi màu đã liền kề nhau nên không cần phải hoán đổi. DP không bị ràng buộc với thứ tự màu số. Có thể tự do chọn (3,1,2) làm thứ tự khối cuối cùng và nhận được giá trị 0. 

Kích thước đầu vào lớn nhất cũng an toàn khi một màu chiếm ưu thế. Với (400000) bản sao của màu 20, chỉ có một màu riêng biệt nên thuật toán sẽ thoát sau khi nén. Trường hợp này hữu ích vì việc triển khai luôn xây dựng DP ở trạng thái (2^{20}) sẽ thực hiện những công việc không cần thiết mặc dù phiên bản thực tế chỉ có một trạng thái.
