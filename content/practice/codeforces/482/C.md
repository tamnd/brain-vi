---
title: "CF 482C - Trò chơi có dây"
description: "Chúng tôi biết trước tất cả các chuỗi ứng cử viên. Một trong số chúng được chọn ngẫu nhiên thống nhất. Chúng tôi tiết lộ thông tin bằng cách hỏi về vị trí nhân vật. Thứ tự các câu hỏi không được lựa chọn một cách chiến lược. Ở mỗi bước chúng tôi chọn thống nhất các vị trí chưa được hỏi."
date: "2026-06-07T17:20:13+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 482
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 275 (Div. 1)"
rating: 2600
weight: 482
solve_time_s: 222
verified: true
draft: false
---

[CF 482C - Trò chơi có dây](https://codeforces.com/problemset/problem/482/C) 

**Đánh giá:** 2600 
**Thẻ:** bitmask, dp, xác suất 
**Thời gian giải:** 3 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi biết trước tất cả các chuỗi ứng cử viên. Một trong số chúng được chọn ngẫu nhiên thống nhất. Chúng tôi tiết lộ thông tin bằng cách hỏi về vị trí nhân vật. 

Thứ tự các câu hỏi không được lựa chọn một cách chiến lược. Ở mỗi bước chúng tôi chọn thống nhất các vị trí chưa được hỏi. 

Sau khi một số vị trí được tiết lộ, một số chuỗi có thể vẫn tương thích với tất cả các câu trả lời được thấy cho đến nay. Trò chơi dừng ngay khi chỉ còn một chuỗi có thể. Chúng ta phải tính toán số lượng câu hỏi dự kiến ​​được hỏi. 

Các thông số chính nhỏ một cách rất cụ thể. Có nhiều nhất là 50 chuỗi, nhưng độ dài chuỗi nhiều nhất là 20. Độ dài 20 ngay lập tức cho thấy rằng các tập hợp con của các vị trí có thể quản lý được vì$2^{20} = 1{,}048{,}576$. Đó là một con số lớn nhưng vẫn nằm trong tầm tay của một DP mặt nạ bit được thiết kế cẩn thận. Mặt khác, bất kỳ trạng thái nào liên quan đến các tập con chuỗi tùy ý đều không thể thực hiện được vì có thể có 50 chuỗi. 

Một điểm tinh tế là quá trình ngẫu nhiên chỉ phụ thuộc vào vị trí nào đã được hỏi chứ không phụ thuộc vào thứ tự chúng được hỏi. Khi đã biết một tập hợp các vị trí, các chuỗi ứng cử viên còn lại sẽ được xác định hoàn toàn. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một chuỗi duy nhất:```
1
abc
```Câu trả lời đúng là 0. Chuỗi được chọn đã được biết trước khi hỏi bất kỳ điều gì. Một giải pháp mù quáng giả định ít nhất một câu hỏi là bắt buộc sẽ thất bại. 

Xét hai chuỗi chỉ khác nhau ở một vị trí:```
2
aab
aac
```Trò chơi có thể yêu cầu 1, 2 hoặc 3 câu hỏi tùy thuộc vào thời điểm yêu cầu phân biệt vị trí. Một giải pháp chỉ tính các vị trí phân biệt và bỏ qua thứ tự ngẫu nhiên sẽ tạo ra kỳ vọng sai lầm. 

Hãy xem xét các chuỗi chỉ có thể phân biệt được sau khi kết hợp thông tin từ nhiều vị trí:```
3
ab
ac
cb
```Không có vị trí nào xác định duy nhất mỗi chuỗi. DP phải theo dõi toàn bộ các vị trí được tiết lộ chứ không chỉ có bao nhiêu vị trí đã được tiết lộ. 

## Phương pháp tiếp cận 

Một quan điểm vũ phu là đơn giản. Giả sử trạng thái là: 

- chuỗi đã chọn, 
- tập hợp các vị trí đã được yêu cầu. 

Từ trạng thái như vậy, chúng ta có thể xác định xem chuỗi đã được xác định duy nhất hay chưa. Nếu không, chúng tôi chọn ngẫu nhiên một vị trí không được yêu cầu và tiếp tục đệ quy. 

Điều này mang lại sự tái diễn chính xác, nhưng không gian trạng thái chứa$$n \cdot 2^m$$tiểu bang. Với$n=50$Và$m=20$, tức là hơn năm mươi triệu tiểu bang. Ngay cả việc lưu trữ chúng là không thực tế. 

Quan sát mở ra vấn đề là chúng ta thực sự không bao giờ cần các giá trị DP riêng biệt cho các chuỗi được chọn khác nhau. 

Sửa một tập hợp các vị trí đã được hỏi$M$. 

Đối với một chuỗi cụ thể$i$, hãy xác định nó là chưa được giải quyết nếu ít nhất một chuỗi khác vẫn nhất quán với tất cả các câu trả lời ở các vị trí trong$M$. 

Giả định$F(M)$là tổng số câu hỏi còn lại dự kiến ​​trên tất cả các chuỗi có thể được chọn. 

Đối với mỗi chuỗi chưa được giải quyết, chúng ta phải dành thêm một câu hỏi trước khi chuyển sang trạng thái con. Đối với mỗi chuỗi được giải quyết, kỳ vọng đã bằng không. 

Điều này cho phép chúng ta viết một phép truy toán chỉ liên quan đến mặt nạ vị trí$M$. Chuỗi đã chọn sẽ biến mất hoàn toàn khỏi trạng thái DP. 

Thử thách còn lại là tính toán cho mỗi chiếc mặt nạ$M$, có bao nhiêu chuỗi chưa được giải quyết. Đây là lúc độ dài chuỗi nhỏ trở nên hữu ích. 

Đối với hai chuỗi$i$Và$j$, cho phép`diff(i,j)`là tập hợp các vị trí mà chúng khác nhau. 

Họ vẫn không thể phân biệt được sau khi hỏi vị trí$M$chính xác khi không có vị trí khác nhau nào được tiết lộ:$$M \cap diff(i,j)=\varnothing$$Tương đương,$$M \subseteq (\text{all positions}) \setminus diff(i,j)$$Điều này trở thành một vấn đề lan truyền tập hợp con-SOS cổ điển trên$2^m$mặt nạ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Các trạng thái Brute Force trên (chuỗi, mặt nạ) |$O(n2^m m)$trạng thái và chuyển tiếp |$O(n2^m)$| Quá chậm | 
| Tuyên truyền Bitmask DP + SOS |$O(m2^m)$|$O(2^m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đánh số các vị trí từ 0 đến$m-1$. 
2. Với mỗi cặp chuỗi riêng biệt có thứ tự$(i,j)$, tạo mặt nạ`sameMask`chứa các vị trí mà hai chuỗi bằng nhau. 

Nếu các vị trí hiện được tiết lộ là một tập hợp con của`sameMask`, sau đó là chuỗi$i$Và$j$vẫn không thể phân biệt được. 
3. Tạo một mảng`alive[mask]`của các bit 64-bit. 

Đối với mỗi cặp đặt hàng$(i,j)$, đặt bit`i`bên trong`alive[sameMask]`. 

Cái này ghi lại chuỗi đó`i`không được giải quyết bất cứ khi nào mặt nạ vị trí lộ diện là tập hợp con của`sameMask`. 
4. Chạy truyền bá SOS từ tập hợp con này sang tập hợp con khác. 

Sau khi nhân giống,`alive[M]`chứa chính xác tập hợp các chuỗi vẫn chưa được giải quyết sau khi tiết lộ vị trí`M`. 

Việc truyền bá hoạt động vì mọi tập hợp con của`sameMask`kế thừa mối quan hệ không thể phân biệt được. 
5. Xác định$$K(M)=\text{number of set bits in } alive[M]$$Đây là số chuỗi chưa được xác định duy nhất. 
6. Hãy để`dp[M]`là tổng số câu hỏi còn lại dự kiến ​​trên tất cả các chuỗi có thể được chọn khi vị trí`M`đã được tiết lộ rồi. 
7. Xử lý mặt nạ theo thứ tự giảm dần. 

Nếu như`rem`các vị trí vẫn chưa được yêu cầu, sau đó$$dp[M] = K(M) + \frac{1}{rem} \sum_{p \notin M} dp[M \cup \{p\}]$$Thuật ngữ đầu tiên đóng góp một câu hỏi cho mỗi chuỗi chưa được giải quyết. Số hạng thứ hai tính trung bình theo sự lựa chọn ngẫu nhiên của vị trí tiếp theo. 
8. Trạng thái đeo mặt nạ đầy đủ không có con. Giá trị của nó chỉ đơn giản là`K(fullMask)`, which is zero.
 9. Câu trả lời bắt buộc là$$\frac{dp[0]}{n}$$bởi vì`dp[0]`là tổng kỳ vọng của tất cả các chuỗi có khả năng được chọn như nhau. 

### Tại sao nó hoạt động 

Đối với mặt nạ vị trí lộ cố định$M$, mọi chuỗi được chọn đều thuộc một trong hai loại. 

Nếu nó đã được xác định duy nhất thì kỳ vọng còn lại của nó là bằng không. 

Nếu nó không được xác định duy nhất, chính xác một câu hỏi nữa sẽ được hỏi trước khi chuyển sang mặt nạ trẻ em được chọn thống nhất trong số các vị trí còn lại. 

Tính tổng các phương trình này trên tất cả các chuỗi sẽ tạo ra phép truy toán cho`dp[M]`. Không cần thông tin về chuỗi đã chọn riêng lẻ nữa; chỉ có số lượng chuỗi chưa được giải quyết mới quan trọng. 

Giai đoạn SOS tính toán chính xác các chuỗi chưa được giải quyết vì hai chuỗi vẫn không thể phân biệt chính xác khi tất cả các vị trí được tiết lộ đều thuộc về mặt nạ đẳng thức của chúng. Việc truyền thông tin từ mặt nạ đến tất cả các tập hợp con của nó sẽ liệt kê chính xác các tập hợp vị trí được tiết lộ đó. 

Cùng với nhau, hai sự kiện này hoàn toàn mô tả quá trình ngẫu nhiên, do đó DP tính toán kỳ vọng chính xác. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    n = int(input())
    s = [input().strip() for _ in range(n)]

    m = len(s[0])
    N = 1 << m
    full = N - 1

    alive = array('Q', [0]) * N

    for i in range(n):
        for j in range(n):
            if i == j:
                continue

            same_mask = 0
            si = s[i]
            sj = s[j]

            for k in range(m):
                if si[k] == sj[k]:
                    same_mask |= 1 << k

            alive[same_mask] |= (1 << i)

    for b in range(m):
        bit = 1 << b
        for mask in range(N):
            if (mask & bit) == 0:
                alive[mask] |= alive[mask | bit]

    pc = bytearray(N)
    for mask in range(1, N):
        pc[mask] = pc[mask >> 1] + (mask & 1)

    dp = array('d', [0.0]) * N

    for mask in range(full, -1, -1):
        unresolved = alive[mask].bit_count()

        rem = m - pc[mask]
        if rem == 0:
            dp[mask] = float(unresolved)
            continue

        total = 0.0
        free = full ^ mask

        while free:
            bit = free & -free
            total += dp[mask | bit]
            free ^= bit

        dp[mask] = unresolved + total / rem

    print("{:.15f}".format(dp[0] / n))

solve()
```các`alive`mảng là trung tâm của quá trình tiền xử lý. Mỗi mục lưu trữ một bitset 64 bit vì$n \le 50$, do đó mỗi chuỗi vừa với một từ máy. 

Việc truyền bá SOS di chuyển thông tin từ mặt nạ đến tất cả các tập hợp con của nó. Sau khi nó kết thúc,`alive[M]`đã chứa tập hợp chính xác các chuỗi chưa được giải quyết cho mặt nạ vị trí được tiết lộ đó. 

DP chạy theo thứ tự mặt nạ ngược. Mỗi quá trình chuyển đổi đều chuyển sang một tập hợp lớn nghiêm ngặt, vì vậy tất cả các trạng thái con đều đã được tính toán sẵn. 

Một chi tiết triển khai dễ mắc sai sót là việc diễn giải`K(M)`. Nó không phải là số lượng chuỗi ứng cử viên còn lại. Đó là số lượng chuỗi được chọn vẫn chưa được giải quyết. Đó là những số lượng khác nhau. 

Một điểm tinh tế khác là việc sử dụng các cặp có thứ tự. Nếu chuỗi`i`vẫn có thể bị nhầm lẫn với chuỗi`j`, sau đó xâu chuỗi`i`phải được đánh dấu là chưa giải quyết. Hướng ngược lại cũng phải được ghi riêng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2
aab
aac
```Các chuỗi chỉ khác nhau ở vị trí 2. 

| Mặt nạ | Vị trí được tiết lộ | Chuỗi chưa được giải quyết K(mask) | 
| --- | --- | --- | 
| 000 | không | 2 | 
| 001 | vị trí 0 | 2 | 
| 010 | vị trí 1 | 2 | 
| 100 | vị trí 2 | 0 | 
| 111 | tất cả | 0 | 

Giá trị DP: 

| Mặt nạ | rem | dp | 
| --- | --- | --- | 
| 111 | 0 | 0 | 
| 110 | 1 | 0 | 
| 101 | 1 | 0 | 
| 011 | 1 | 2 | 
| 100 | 2 | 0 | 
| 010 | 2 | 3 | 
| 001 | 2 | 3 | 
| 000 | 3 | 4 | 

Câu trả lời là:$$dp[0]/2 = 2$$phù hợp với mẫu. 

Dấu vết này cho thấy rằng DP đang tính tổng các kỳ vọng trên cả hai chuỗi có thể được chọn cùng một lúc. 

### Ví dụ 2 

đầu vào:```
3
ab
ac
cb
```| Mặt nạ | Vị trí được tiết lộ | K(mặt nạ) | 
| --- | --- | --- | 
| 00 | không | 3 | 
| 01 | vị trí đầu tiên | 2 | 
| 10 | vị trí thứ hai | 2 | 
| 11 | cả hai vị trí | 0 | 

Tính toán DP: 

| Mặt nạ | rem | dp | 
| --- | --- | --- | 
| 11 | 0 | 0 | 
| 10 | 1 | 2 | 
| 01 | 1 | 2 | 
| 00 | 2 | 5 | 

Câu trả lời cuối cùng:$$5/3$$Ví dụ này chứng tỏ rằng một vị trí được tiết lộ không phải lúc nào cũng đủ ngay cả khi nó loại bỏ một số ứng viên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m2^m)$| Tuyên truyền SOS và DP từng xử lý mọi mặt nạ cho mọi vị trí | 
| Không gian |$O(2^m)$| Mảng bitset, mảng popcount và mảng DP | 

Với$m \le 20$, chúng ta có nhiều nhất$2^{20} = 1{,}048{,}576$mặt nạ. Cả mức sử dụng bộ nhớ và số lượng thao tác đều nằm trong giới hạn một cách thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from array import array

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    s = [input().strip() for _ in range(n)]

    m = len(s[0])
    N = 1 << m
    full = N - 1

    alive = array('Q', [0]) * N

    for i in range(n):
        for j in range(n):
            if i == j:
                continue

            same_mask = 0
            for k in range(m):
                if s[i][k] == s[j][k]:
                    same_mask |= 1 << k

            alive[same_mask] |= (1 << i)

    for b in range(m):
        bit = 1 << b
        for mask in range(N):
            if (mask & bit) == 0:
                alive[mask] |= al
```
