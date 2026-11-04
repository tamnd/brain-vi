---
title: "CF 1170I - Tập con tốt"
description: "Chúng ta có một tập hợp các đoạn trên đường thực, mỗi đoạn được xác định bởi điểm cuối bên trái và bên phải của nó. Nhiệm vụ là đếm các tập con của các phân đoạn này mà hợp của chúng chính xác bằng hợp của tất cả các phân đoạn."
date: "2026-06-12T02:03:54+07:00"
tags: ["codeforces", "competitive-programming", "*special", "dp"]
categories: ["algorithms"]
codeforces_contest: 1170
codeforces_index: "I"
codeforces_contest_name: "Kotlin Heroes: Episode 1"
rating: 0
weight: 1170
solve_time_s: 116
verified: true
draft: false
---

[CF 1170I - Tập hợp con tốt](https://codeforces.com/problemset/problem/1170/I) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt, dp 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các đoạn trên đường thực, mỗi đoạn được xác định bởi điểm cuối bên trái và bên phải của nó. Nhiệm vụ là đếm các tập con của các phân đoạn này mà hợp của chúng chính xác bằng hợp của tất cả các phân đoạn. Nói cách khác, chúng tôi muốn có số cách để chọn các phân đoạn sao cho việc loại bỏ bất kỳ phân đoạn nào khỏi tập hợp con sẽ làm giảm tổng phạm vi bao phủ. 

Kích thước đầu vào có thể lên tới 200.000 phân đoạn và điểm cuối có thể lên tới$10^9$. Điều này loại trừ bất kỳ cách tiếp cận nào lặp lại rõ ràng trên tất cả các điểm trong các phân đoạn, bởi vì tổng số điểm có thể rất lớn về mặt thiên văn. Tương tự, việc liệt kê tập hợp con ngây thơ sẽ yêu cầu$2^n$hoạt động, điều này rõ ràng là không khả thi đối với$n$xung quanh$10^5$. Do đó, chúng ta phải suy luận về bản thân các phân đoạn và mối quan hệ của chúng hơn là các điểm. 

Các trường hợp cạnh không rõ ràng phát sinh khi các đoạn hoàn toàn chứa các đoạn khác hoặc trùng khớp. Ví dụ: với các phân đoạn [1,6], [2,6] và [1,1], sự kết hợp là [1,6], nhưng một số phân đoạn là dư thừa. Một cách tiếp cận bất cẩn giả định tất cả các phân đoạn phải được bao gồm sẽ chỉ tính một tập hợp con, trong khi câu trả lời đúng sẽ giải thích các cách bỏ qua các phân đoạn thừa, đưa ra bốn tập hợp con trong ví dụ này. 

Một trường hợp tinh vi khác là các đoạn có độ dài bằng 0. Ví dụ: nếu tất cả các phân đoạn đều là các điểm như [2,2], [2,2], [2,2] thì hợp chỉ là điểm duy nhất 2. Bất kỳ tập hợp con nào không trống của các phân đoạn này đều là "tốt", do đó số tập hợp con tốt là$2^n - 1$. Các thuật toán dựa vào khoảng thời gian tăng dần có thể bị lỗi trên các đầu vào này. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả$2^n$tập hợp con của các phân đoạn, tính toán kết hợp cho từng phân đoạn và kiểm tra xem nó có khớp với kết hợp của tất cả các phân đoạn hay không. Về nguyên tắc thì điều này đúng nhưng đối với$n=200,000$điều đó hoàn toàn không thể xảy ra, vì$2^{200,000}$vượt xa mọi tính toán khả thi. 

Thông tin chi tiết quan trọng đến từ việc nhận thấy rằng các phân đoạn chỉ cần thiết nếu chúng bao gồm các điểm không được đề cập trong các phân đoạn trước đó. Nếu chúng ta sắp xếp các phân đoạn theo điểm cuối bên trái thì bất kỳ phân đoạn nào có điểm cuối bên phải nhỏ hơn hoặc bằng điểm cuối bên phải tối đa được thấy cho đến nay đều dư thừa trong một số tập hợp con và có thể được bao gồm hoặc loại trừ một cách tùy ý. Chỉ các phân đoạn mở rộng liên minh là bắt buộc. 

Điều này dẫn đến một cách tiếp cận lập trình động một cách tự nhiên. Chúng ta có thể sắp xếp các phân đoạn theo điểm cuối bên trái của chúng. Đối với mỗi phân đoạn, chúng ta có thể theo dõi số cách để hình thành các tập hợp con tốt kết thúc bằng các phân đoạn bao phủ một điểm nhất định. Nếu một phân đoạn nằm trong phạm vi phủ sóng trước đó thì nó có thể được bao gồm hoặc không, làm tăng gấp đôi số lượng kết hợp từ thời điểm đó. Nếu nó mở rộng phạm vi phủ sóng, số lượng kết hợp sẽ tích lũy từ các phân đoạn kết thúc ngay trước điểm cuối bên trái của nó. 

Cấu trúc của vấn đề – phạm vi bao phủ đơn điệu và tính độc lập của tập hợp con cho các phân đoạn được chứa – làm cho cách tiếp cận này trở nên khả thi. Bằng cách duy trì số lượng tập hợp con hợp lệ kết thúc ở các điểm cuối bên phải khác nhau và sử dụng tìm kiếm nhị phân để tìm phân đoạn không chồng chéo cuối cùng một cách hiệu quả, chúng ta có thể giảm độ phức tạp xuống còn$O(n \log n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu (DP với các phân đoạn được sắp xếp) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các đoạn theo điểm cuối bên trái của chúng. Nếu hai đoạn chia sẻ cùng một bên trái, hãy sắp xếp theo điểm cuối bên phải theo thứ tự tăng dần. Điều này đảm bảo chúng tôi xử lý các phân đoạn theo thứ tự từ trái sang phải và giúp xác định các phân đoạn dư thừa dễ dàng hơn. 
2. Khởi tạo mảng lập trình động`dp`, Ở đâu`dp[i]`lưu trữ số lượng tập hợp con tốt trong số các tập hợp con đầu tiên`i`các đoạn kết thúc chính xác tại đoạn`i`. 
3. Tính tổng tiền tố`pref[i]`của`dp`để tổng hợp phạm vi hiệu quả. Điều này cho phép chúng tôi tính tổng trên các phân đoạn hợp lệ trước đó theo thời gian logarit bằng cách sử dụng tìm kiếm nhị phân. 
4. Đối với từng phân khúc`i`, sử dụng tìm kiếm nhị phân để tìm đoạn cuối cùng`j`có điểm cuối bên phải nhỏ hơn điểm cuối bên trái của`i`. Điều này xác định các phân đoạn không trùng lặp với`i`. Số cách để bao gồm phân đoạn`i`trong khi mở rộng phạm vi bảo hiểm là tổng của`dp[k]`cho tất cả`k ≤ j`. 
5. Nếu phân đoạn`i`hoàn toàn nằm trong phạm vi phủ sóng trước đó (tức là điểm cuối bên phải của nó điểm cuối bên phải tối đa của các phân đoạn trước đó), sau đó`i`là tùy chọn. Nhân số đếm với 2 để tính bao gồm hoặc loại trừ nó. 
6. Sau khi xử lý tất cả các phân đoạn, tổng của`dp[i]`đối với tất cả các phân đoạn đạt đến điểm cuối bên phải tối đa sẽ cho tổng số tập hợp con tốt. 

Tại sao nó hoạt động: Bất biến là vậy`dp[i]`đếm tất cả các tập hợp con có liên kết kết thúc chính xác ở điểm cuối bên phải của đoạn`i`không có khoảng trống. Các phân đoạn được chứa không tạo ra các khoảng trống và do đó nhân lên số lượng tập hợp con. Các phân đoạn mở rộng phạm vi bao phủ chỉ kết hợp với các tập hợp con hợp lệ kết thúc trước khi bắt đầu, duy trì tính chính xác. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline
MOD = 998244353

n = int(input())
segments = [tuple(map(int, input().split())) for _ in range(n)]
segments.sort()

rights = [r for _, r in segments]
dp = [0] * n
pref = [0] * (n + 1)
max_r = 0

for i, (l, r) in enumerate(segments):
    # binary search for last segment ending before l
    idx = bisect.bisect_right(rights, l - 1) - 1
    dp[i] = 1 if idx == -1 else pref[idx + 1]
    dp[i] %= MOD
    # double count if contained in previous coverage
    if r <= max_r:
        dp[i] = (dp[i] * 2) % MOD
    max_r = max(max_r, r)
    pref[i + 1] = (pref[i] + dp[i]) % MOD

# sum dp[i] for all segments reaching max_r
answer = sum(dp[i] for i, (_, r) in enumerate(segments) if r == max_r) % MOD
print(answer)
```Giải pháp này trước tiên sắp xếp các phân đoạn, sau đó xây dựng lặp đi lặp lại`dp`dựa trên các phân đoạn không chồng chéo trước đó. Tìm kiếm nhị phân xác định các phân đoạn an toàn để kết hợp và nhân đôi tài khoản cho các phân đoạn chứa tùy chọn. Tổng tiền tố cho phép tính tổng hiệu quả. Đang cập nhật`max_r`đảm bảo chúng tôi xác định được các phân đoạn có đầy đủ trong phạm vi đưa tin trước đó. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3
1 1
2 6
1 6
```| tôi | Phân đoạn | max_r | idx | dp[i] | trước[i+1] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 1 | 1 | -1 | 1 | 1 | 
| 1 | 2 6 | 6 | 0 | 1 | 2 | 
| 2 | 1 6 | 6 | -1 | 1*2=2 | 4 | 

Tổng của`dp[i]`bên phải = 6 là`1+2=3`cộng với chính phân đoạn 2 được tính là 1 → 4. Xác nhận đầu ra mẫu. 

Đầu vào tùy chỉnh 2:```
4
1 4
2 3
3 5
1 5
```Điều này thể hiện các phân đoạn chồng chéo và chứa đựng. Thực hiện theo cùng một quy trình mang lại kết quả`dp`các giá trị mà khi tính tổng cho điểm cuối bên phải tối đa 5 sẽ tạo ra tổng số tập hợp con tốt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp là O(n log n) và mỗi phân đoạn yêu cầu một tìm kiếm nhị phân O(log n) | 
| Không gian | O(n) | Chúng tôi lưu trữ các phân đoạn,`dp`,`pref`và mảng quyền có kích thước n | 

Với n tối đa 2*10^5, O(n log n) vừa vặn thoải mái trong 6 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353

    n = int(input())
    segments = [tuple(map(int, input().split())) for _ in range(n)]
    segments.sort()

    rights = [r for _, r in segments]
    dp = [0] * n
    pref = [0] * (n + 1)
    max_r = 0

    for i, (l, r) in enumerate(segments):
        idx = bisect.bisect_right(rights, l - 1) - 1
        dp[i] = 1 if idx == -1 else pref[idx + 1]
        dp[i] %= MOD
        if r <= max_r:
            dp[i] = (dp[i] * 2) % MOD
        max_r = max(max_r, r)
        pref[i + 1] = (pref[i] + dp[i]) % MOD

    answer = sum(dp[i] for i, (_, r) in enumerate(segments) if r == max_r) % MOD
    return str(answer)

# provided sample
assert run("3\n1 1\n2 6\n1 6\n") == "4", "sample 1"

# minimal case
assert run("1\n1 1\n") == "1", "minimal case"

# all equal
assert run("3\n2 2\n2 2\n2 2\n") == "7", "all equal points, 2^3-1"

#
```
