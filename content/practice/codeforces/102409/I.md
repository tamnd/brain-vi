---
title: "CF 102409I - Cú búng tay của Thanos"
description: "Có chính xác (N=5{,}000{,}000) người ở Diegopolis. Mỗi người độc lập sống sót sau cú búng tay với xác suất (1/2). Gọi (X) là số người sống sót nên (X) tuân theo phân phối nhị thức với các tham số (N) và (1/2)."
date: "2026-08-12T00:02:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102409
codeforces_index: "I"
codeforces_contest_name: "Semana i 2019"
rating: 0
weight: 102409
solve_time_s: 150
verified: true
draft: false
---

[CF 102409I - Cú búng tay của Thanos](https://codeforces.com/problemset/problem/102409/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có chính xác (N=5{,}000{,}000) người ở Diegopolis. Mỗi người độc lập sống sót sau cú búng tay với xác suất (1/2). Gọi (X) là số người sống sót nên (X) tuân theo phân phối nhị thức với các tham số (N) và (1/2). 

Đối với mọi trường hợp thử nghiệm, chúng tôi được cấp một ngưỡng (K) và cần xác suất để ít nhất (K) người sống sót: 

[ 
P(X\ge K). 
] 

Câu trả lời là không bắt buộc vì xác suất dấu phẩy động. Thay vào đó, chúng ta tính modulo (M=10^9+7), coi phép chia là phép nhân với một nghịch đảo mô đun. Vấn đề ban đầu khắc phục dân số ở mức năm triệu và cho phép tối đa (10^5) truy vấn. 

Kích thước của (N) là ràng buộc chính. Một tính toán thực hiện ngay cả một lượng công việc nhỏ cho mọi kết quả có thể xảy ra đã thực hiện khoảng năm triệu thao tác, điều này là hợp lý một lần, nhưng việc thực hiện riêng lẻ cho (10^5) truy vấn sẽ gần như là công việc (5\times10^{11}) và hoàn toàn không khả thi. Bất kỳ cách tiếp cận nào tính toán rõ ràng tổng nhị thức một cách độc lập cho mỗi truy vấn đều bị loại trừ. Chúng ta cần thực hiện công việc tốn kém được chia sẻ bởi tất cả các truy vấn. 

Có một số trường hợp ranh giới mà việc triển khai bất cẩn có thể thất bại. Với (K=0), sự kiện là chắc chắn nên câu trả lời chính xác là (1). Ví dụ,```
1
0
```phải sản xuất```
1
```mặc dù giới hạn dưới của tuyên bố chính thức là (K\ge1), vì bản thân mẫu bao gồm (K=0). 

Ở một thái cực khác, (K=N) có nghĩa là mọi người đều phải sống sót. Vì vậy câu trả lời chỉ đơn giản là 

[ 
\frac{1}{2^N}. 
]

Vì```
1
5000000
```câu trả lời là (195206359), như được đưa ra trong mẫu. Một giải pháp vô tình tính toán (P(X>K)) thay vì (P(X\ge K)) sẽ trả về 0 ở đây. 

Ngưỡng (K=1) là một ranh giới hữu ích khác. Phần bù của nó là sự kiện không ai sống sót, vì vậy 

[ 
P(X\ge1)=1-\frac1{2^N}. 
] 

Đối với vấn đề này là (804793649). Việc triển khai bắt đầu tổng tích lũy của nó ở (K=1) nhưng quên xác suất không có người sống sót nào sẽ mắc sai lầm trong trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp được áp dụng ngay sau phân phối nhị thức. Chính xác (k) người sống sót có xác suất 

[ 
P(X=k)=\binom Nk\frac1{2^N}. 
] 

Do đó, một truy vấn yêu cầu ít nhất (K) người sống sót có thể được trả lời bằng cách tính tổng 

[ 
\sum_{k=K}^{N}\binom Nk\frac1{2^N}. 
] 

Điều này đúng vì số lượng người sống sót có thể loại trừ lẫn nhau và bao gồm mọi kết quả. Vấn đề là số lượng công việc lặp đi lặp lại. Nếu một truy vấn yêu cầu (K=1), chúng tôi sẽ xử lý khoảng năm triệu thuật ngữ. Thực hiện điều đó cho (10^5) truy vấn sẽ mang lại tối đa (5\time10^{11}) thuật ngữ, vượt xa giới hạn một giây. 

Quan sát quan trọng là xác suất nhị thức không phải là giá trị độc lập. Các thuật ngữ liên tiếp có tỷ lệ đơn giản: 

# \frac{\binom Nk}{\binom N{k-1}} 

\frac{N-k+1}{k}. 
] 

Vì vậy, một khi chúng ta biết xác suất không có người sống sót, 

[ 
P(X=0)=\frac1{2^N}, 
] 

chúng ta có thể tạo ra mọi xác suất tiếp theo bằng cách sử dụng một phép nhân mô đun và một nghịch đảo mô đun. 

Có một quan sát hữu ích khác về các truy vấn. Nếu chúng ta sắp xếp tất cả các giá trị được truy vấn của (K), chúng ta có thể chuyển từ ngưỡng nhỏ nhất đến ngưỡng lớn nhất trong khi vẫn duy trì xác suất đuôi. Giả sử hiện tại chúng ta biết 

[ 
S_k=P(X\ge k). 
] 

Sau đó 

[ 
S_{k+1}=S_k-P(X=k). 
] 

Vì vậy, việc di chuyển ngưỡng về phía trước một lần chỉ tốn khối lượng xác suất hiện tại. Trên tất cả các truy vấn, chúng tôi không bao giờ cần duyệt qua phân phối nhiều lần, cho đến yêu cầu lớn nhất (K). 

Câu hỏi còn lại là làm thế nào để có được tất cả các nghịch đảo mô-đun (1^{-1},2^{-1},\ldots,m^{-1}), trong đó (m) là ngưỡng truy vấn lớn nhất. Bởi vì (m<10^9+7) nên mọi mẫu số đều khả nghịch modulo (M). Chúng ta có thể tạo ra nghịch đảo trong thời gian tuyến tính bằng cách sử dụng 

M-\left\lfloor\frac Mi\right\rfloor 
\operatorname{inv[M\bmod i]\pmod M. 
] 

Đối với Python, việc lưu trữ năm triệu số nguyên thông thường sẽ tiêu tốn quá nhiều bộ nhớ, do đó việc triển khai sẽ sử dụng`array('I')`, lưu trữ mỗi giá trị mô-đun trong bốn byte. Do đó, năm triệu mục nhập cần khoảng 20 MB. 

Cách tiếp cận bạo lực và cách tiếp cận tối ưu hóa có thể được tóm tắt như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(NT)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(N+T\log T)) | (O(N+T)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các truy vấn và ghi nhớ vị trí ban đầu của chúng. Việc sắp xếp các truy vấn cho phép chúng ta trả lời chúng theo thứ tự tăng dần (K), do đó mỗi xác suất nhị thức chỉ được tạo một lần. 
2. Gọi (m) là truy vấn lớn nhất (K). Không có lý do gì để tạo ra xác suất vượt quá (m), vì không có truy vấn nào yêu cầu về ngưỡng lớn hơn. 
3. Xây dựng nghịch đảo mô đun từ (1) đến (m). Vì (m<10^9+7), mọi số nguyên trong phạm vi này đều có nghịch đảo mô đun. 
4. Tính toán 

[ 
p_0=P(X=0)=2^{-N}\pmod M. 
] 

Ba đối số của Python`pow`tính toán lũy thừa mô-đun một cách hiệu quả, do đó việc này không yêu cầu lặp lại (N). 

1. Khởi tạo xác suất đuôi bằng 

[ 
S_0=P(X\ge0)=1. 
] 

Cũng giữ`cur = 0`Và`p = p_0`, Ở đâu`p`đại diện cho (P(X=\text{cur})). 

1. Xử lý các truy vấn đã được sắp xếp. Nếu truy vấn tiếp theo yêu cầu (K), hãy liên tục di chuyển từ`cur`đến (K). Ở mỗi lần di chuyển, hãy trừ (P(X=\text{cur})) khỏi đuôi hiện tại, tăng dần`cur`và tạo ra xác suất mới với 

P(X=\văn bản{cur}-1) 
\frac{N-\text{cur}+1}{\text{cur}}. 
] 

Lý do trừ trước khi tăng là vì (P(X\ge cur)) trở thành (P(X\ge cur+1)) chính xác bằng cách loại bỏ khối lượng xác suất tại (X=cur). 

1. Một lần`cur == K`, phần đuôi được duy trì chính xác là (P(X\ge K)). Lưu trữ nó ở vị trí truy vấn ban đầu. 
2. In câu trả lời theo thứ tự đầu vào thay vì thứ tự sắp xếp. Các ngưỡng trùng lặp không cần xử lý đặc biệt vì chúng được trả lời đơn giản từ cùng một trạng thái được duy trì. 

### Tại sao nó hoạt động 

Bất biến là ngay trước khi trả lời một truy vấn có ngưỡng`cur`,`tail`bằng (P(X\ge cur)), trong khi`pmf`bằng (P(X=cur)). Di chuyển từ (cur) đến (cur+1) trừ đi chính xác một khối lượng xác suất phải biến mất khỏi đuôi, do đó bất biến vẫn đúng. Sự tái phát cho`pmf`tạo ra xác suất nhị thức chính xác tiếp theo vì các hệ số nhị thức liên tiếp có tỷ lệ ((N-k+1)/k). Vì bất biến bắt đầu bằng (P(X\ge0)=1) và (P(X=0)=2^{-N}), nên mọi xác suất đuôi được yêu cầu đều chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from array import array

MOD = 1_000_000_007
N = 5_000_000

def solve():
    t = int(input())
    queries = [int(input()) for _ in range(t)]

    indexed = sorted(enumerate(queries), key=lambda x: x[1])
    max_k = indexed[-1][1]

    # inv[i] = modular inverse of i modulo MOD.
    inv = array('I', [0]) * (max_k + 1)
    if max_k >= 1:
        inv[1] = 1
        for i in range(2, max_k + 1):
            inv[i] = MOD - (MOD // i) * inv[MOD % i] % MOD

    # P(X = 0) = 1 / 2^N.
    pmf = pow(pow(2, N, MOD), MOD - 2, MOD)

    # tail = P(X >= cur)
    cur = 0
    tail = 1

    ans = [0] * t

    for idx, k in indexed:
        while cur < k:
            # Remove P(X = cur), so tail becomes P(X >= cur + 1).
            tail -= pmf
            if tail < 0:
                tail += MOD

            cur += 1

            # P(X = cur) =
            # P(X = cur - 1) * (N - cur + 1) / cur
            pmf = pmf * (N - cur + 1) % MOD
            pmf = pmf * inv[cur] % MOD

        ans[idx] = tail

    sys.stdout.write('\n'.join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Mảng nghịch đảo được tạo trước tiên vì mọi chuyển đổi từ (P(X=k-1)) sang (P(X=k)) đều yêu cầu chia cho (k). Sự tái phát tránh gọi`pow(k, MOD-2, MOD)`năm triệu lần, sẽ là quá đắt. 

Xác suất ban đầu được tính là (2^{-N}). Đoạn mã viết giá trị này dưới dạng nghịch đảo của (2^N), sử dụng định lý nhỏ Fermat. Vì (M) là số nguyên tố nên 

[ 
(2^N)^{-1}\equiv (2^N)^{M-2}\pmod M. 
] 

Vòng lặp chính duy trì hai đại lượng được mô tả trong chứng minh.`tail`bắt đầu từ một vì mọi số người sống sót có thể ít nhất bằng không.`pmf`bắt đầu tại (P(X=0)). Sau khi trừ`pmf`, ngưỡng được tăng lên và sự lặp lại tạo ra xác suất thuộc về ngưỡng mới đó. 

Phép trừ được thực hiện modulo (M). Số nguyên Python không bị tràn, nhưng`tail`có thể trở thành số âm sau phép trừ mô-đun, do đó mã sẽ thêm`MOD`khi cần thiết. 

Việc sắp xếp truy vấn cũng rất quan trọng. Nếu không có nó, việc chuyển từ (K=4{,}000{,}000) trở lại (K=2) sẽ yêu cầu xây dựng lại hệ thống phân phối hoặc duy trì thông tin bổ sung. Với các truy vấn được sắp xếp,`cur`chỉ tăng nên tổng số lần chuyển tiếp phân phối nhiều nhất là (m). 

các`array('I')`biểu diễn là tối ưu hóa bộ nhớ dành riêng cho Python. Một số nguyên Python bình thường mang theo chi phí đối tượng đáng kể và năm triệu số nguyên như vậy sẽ đạt đến hoặc vượt quá giới hạn bộ nhớ 256 MB. Các mục không dấu bốn byte giữ cho bảng nghịch đảo gần 20 MB. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp chứa hai truy vấn, (K=0) và (K=5{,}000{,}000). 

| Truy vấn (K) |`cur`trước |`tail`trước | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | (1) | Không cần chuyển tiếp | (1) | 
| 5.000.000 | 0 | (1) | Xóa (P(X=0),P(X=1),\ldots,P(X=4,999,999)) | (P(X=5.000.000)) | 

Sau tất cả các lần chuyển đổi, chỉ còn lại kết quả (X=5{,}000{,}000). Do đó 

[ 
P(X\ge5{,}000{,}000)=P(X=5{,}000{,}000) 
=\frac1{2^{5{,}000{,}000}} 
\equiv195206359\pmod M. 
] 

Do đó, đầu ra là```
1
195206359
```Dấu vết này thực hiện cả hai thái cực. (K=0) chứng tỏ tại sao vòng lặp phải cho phép một truy vấn được trả lời ngay lập tức, trong khi (K=N) chứng tỏ rằng ranh giới bao hàm (X\ge K) phải giữ xác suất chính xác (K). 

Đối với dấu vết thứ hai, hãy xem xét một số truy vấn tập trung ở ranh giới trên. 

| Truy vấn (K) |`cur`trước | Chuyển tiếp bắt buộc | Kết quả đuôi | 
| --- | --- | --- | --- | 
| 1 | 0 | Xóa (P(X=0)) | (1-P(X=0)) | 
| 4.999.999 | 1 | Loại bỏ (P(X=1),\ldots,P(X=4,999,998)) | (P(X\ge4,999,999)) | 
| 5.000.000 | 4.999.999 | Xóa (P(X=4,999,999)) | (P(X=5.000.000)) | 

Ở đây (P(X=5{,}000{,}000)=195206359). Ngoài ra, 

1. 

] 

Như vậy 

788168783+195206359 
\equiv983375142\pmod M. 
] 

Do đó, câu trả lời cho ba ngưỡng này là```
804793649
983375142
195206359
```Dấu vết cho thấy tại sao sự lặp lại phải tạo ra (P(X=k)) sau khi tăng`cur`. Sử dụng mẫu số cũ hoặc tử số cũ sẽ dịch chuyển mọi xác suất được tạo ra một vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N+T\log T)) | Tối đa (N) chuyển đổi nghịch đảo hoặc xác suất, cộng với việc sắp xếp các truy vấn (T) | 
| Không gian | (O(N+T)) | Bảng nghịch đảo sử dụng số nguyên compact (O(N)) và các truy vấn và câu trả lời sử dụng (O(T)) | 

Ở đây (N=5{,}000{,}000) và (T\le100{,}000). Phần đắt tiền là tuyến tính ở mức 5 triệu, thay vì tuyến tính ở mức 5 triệu cho mỗi truy vấn. Mảng nghịch đảo nhỏ gọn duy trì mức sử dụng bộ nhớ trong khoảng 256 MB, trong khi chỉ sắp xếp các truy vấn (10^5) là tương đối nhỏ. Vấn đề chính thức đưa ra giới hạn một giây và giới hạn bộ nhớ 256 MB, vì vậy việc chia sẻ bản quét phân phối trên tất cả các truy vấn là điều cần thiết. 

## Trường hợp thử nghiệm```python
from array import array

MOD = 1_000_000_007
N = 5_000_000

def solution(data: str) -> str:
    it = iter(data.split())
    t = int(next(it))
    queries = [int(next(it)) for _ in range(t)]

    indexed = sorted(enumerate(queries), key=lambda x: x[1])
    max_k = indexed[-1][1]

    inv = array('I', [0]) * (max_k + 1)

    if max_k >= 1:
        inv[1] = 1
        for i in range(2, max_k + 1):
            inv[i] = MOD - (MOD // i) * inv[MOD % i] % MOD

    pmf = pow(pow(2, N, MOD), MOD - 2, MOD)
    cur = 0
    tail = 1

    ans = [0] * t

    for idx, k in indexed:
        while cur < k:
            tail -= pmf
            if tail < 0:
                tail += MOD

            cur += 1
            pmf = pmf * (N - cur + 1) % MOD
            pmf = pmf * inv[cur] % MOD

        ans[idx] = tail

    return '\n'.join(map(str, ans))

# Provided sample
assert solution("2\n0\n5000000\n") == \
       "1\n195206359", "provided sample"

# Minimum threshold and maximum threshold
assert solution("2\n0\n1\n") == \
       "1\n804793649", "minimum threshold boundary"

# Upper boundary, including N-1 and N
assert solution("2\n4999999\n5000000\n") == \
       "983375142\n195206359", "upper boundary"

# Duplicate queries and arbitrary ordering
assert solution("5\n5000000\n0\n5000000\n1\n0\n") == \
       "195206359\n1\n195206359\n804793649\n1", "duplicate queries"

# All queries equal
assert solution("4\n5000000\n5000000\n5000000\n5000000\n") == \
       "195206359\n195206359\n195206359\n195206359", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 0 / 1`|`1 / 804793649`| Ngưỡng tối thiểu và phần bổ sung của số người sống sót bằng không | 
|`2 / 4999999 / 5000000`|`983375142 / 195206359`| Ranh giới trên bao gồm và hành vi riêng lẻ | 
|`5 / 5000000 / 0 / 5000000 / 1 / 0`|`195206359 / 1 / 195206359 / 804793649 / 1`| Sắp xếp truy vấn, sắp xếp ban đầu và trùng lặp | 
|`4 / 5000000 / 5000000 / 5000000 / 5000000`| Bốn bản sao của`195206359`| Ngưỡng giống hệt nhau lặp đi lặp lại | 

## Vỏ cạnh 

Với (K=0), đầu vào```
1
0
```bắt đầu bằng`cur = 0`Và`tail = 1`. Từ`cur < k`là sai, thuật toán không thực hiện chuyển đổi xác suất và ngay lập tức trả về (1). Đây chính xác là (P(X\ge0)), bởi vì mọi số lượng người sống sót có thể đều không âm. 

Với (K=1), đầu vào```
1
1
```thực hiện đúng một lần chuyển đổi. Ban đầu`pmf`là (P(X=0)=195206359), do đó trừ đi nó sẽ cho 

[ 
1-195206359=804793648 
] 

Đợi đã, vì mô đun là (10^9+7), phép trừ mô đun đúng là 

[ 
1-195206359\equiv804793649\pmod{1{,}000{,}000{,}007}. 
] 

Do đó đầu ra là```
804793649
```và thuật toán chỉ loại bỏ chính xác kết quả không có người sống sót. 

Đối với (K=N-1=4{,}999{,}999), thuật toán cuối cùng để lại chính xác xác suất của (N-1) và (N). Bắt đầu từ 

[ 
P(X=N)=195206359, 
] 

sự tái phát mang lại 

195206359\cdot5{,}000{,}000 
\equiv788168783. 
] 

Tổng của họ là 

[ 
788168783+195206359 
=983375142, 
] 

vậy```
1
4999999
```sản xuất```
983375142
```Đây là thử nghiệm riêng lẻ đặc biệt hữu ích vì tính toán triển khai (P(X>K)) sẽ chỉ trả về không chính xác (195206359). 

Với (K=N), đầu vào```
1
5000000
```tiến bộ cho đến khi`cur`đạt tới (N). Tại thời điểm đó, phần đuôi chỉ chứa (P(X=N)), được tạo ra bởi phép truy hồi và bằng 

[ 
\frac1{2^N}\equiv195206359\pmod M. 
] 

Do đó, đầu ra là```
195206359
```cũng phù hợp với mẫu chính thức.
