---
title: "CF 102341D - Dedenne"
description: "Chúng ta cần gán một từ mã nhị phân khác rỗng cho mỗi (n) từ. Mã phải không có tiền tố, do đó các từ mã tạo thành các lá của bộ ba nhị phân. Có một hạn chế bổ sung: từ mã không bao giờ được chứa 00."
date: "2026-08-14T01:26:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102341
codeforces_index: "D"
codeforces_contest_name: "Radewoosh+mnbvmar Contest (supported by AIM Tech)"
rating: 0
weight: 102341
solve_time_s: 393
verified: true
draft: false
---

[CF 102341D - Dedenne](https://codeforces.com/problemset/problem/102341/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần gán một từ mã nhị phân khác rỗng cho mỗi (n) từ. Mã phải không có tiền tố, do đó các từ mã tạo thành các lá của bộ ba nhị phân. Có một hạn chế bổ sung: một từ mã có thể không bao giờ chứa`00`. Chi phí được tính cho mỗi nút trie, bao gồm cả tiền tố trống. Nếu chính xác (k) từ mã đi qua một nút thì nút đó sẽ có giá 

[ 
f(k)=\sum_{j=1}^{k}\left\lfloor 1+\log_2 j\right\rfloor. 
] 

Nhiệm vụ là chọn hình dạng của trie sao cho tổng chi phí là nhỏ nhất. 

Dữ liệu đầu vào chứa tối đa (50.000) giá trị độc lập của (n) và một (n) có thể lớn bằng (10^{15}). Điều này ngay lập tức loại trừ việc xây dựng trie và thậm chí một chương trình động (O(n)) cũng quá lớn. Một DP bậc hai là hoàn toàn không thể, trong khi ngay cả (O(n\log n)) cũng sẽ yêu cầu quá nhiều thao tác đối với (10^{15}). Giải pháp phải khai thác thực tế là hàm DP tối ưu có số lượng thay đổi độ dốc rất nhỏ. Đây là quan sát trung tâm được sử dụng bởi các giải pháp được chấp nhận cho vấn đề. 

Có hai trường hợp nhỏ bộc lộ những lỗi thường gặp. Với (n=2), câu trả lời là (5). Một sự lặp lại bất cẩn luôn làm tăng thêm chi phí cho cây con bên trái sẽ nhận được (6), vì khi cây con bên trái chỉ chứa một lá, từ mã có thể kết thúc ngay sau`0`. phần bổ sung`1`cần thiết sau một`0`chỉ được yêu cầu khi cây con đó có ít nhất hai lá. 

Với (n=4), câu trả lời là (20). Một hình dạng tối ưu tương ứng với các từ mã`0`,`10`,`110`, Và`111`. Gốc có bốn con cháu,`0`nhánh có một lá, trong khi nhánh`1`cành có ba lá. Việc xử lý hai cây gốc một cách đối xứng sẽ bỏ sót việc có một cây con đi qua`0`không thể phân nhánh ngay lập tức, bởi vì làm như vậy sẽ tạo ra một`00`bờ rìa. 

Với (n=10), câu trả lời là (98). Đây cũng là một trường hợp biên hữu ích cho phép truy hồi vì phép phân chia tốt nhất không được cân bằng một cách hiển nhiên. Sự phân chia tối ưu được điều chỉnh bởi hàm chi phí lồi thay vì chỉ đơn giản bằng cách chọn hai cây con có kích thước bằng nhau. Mẫu chính thức xác nhận giá trị (98). 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực tự nhiên là xác định (D(n)) là chi phí tối thiểu của một trie hợp lệ chứa (n) lá. Khi gốc có (n) lá bên dưới nó, giá của chính nó là (f(n)). Nếu cây con bên trái chứa (k) lá và cây con bên phải chứa (n-k) thì cây con bên phải có thể được xây dựng bình thường. Cây con bên trái được tiếp cận thông qua`0`, vì vậy nếu nó chứa nhiều hơn một lá thì cạnh tiếp theo của nó phải là`1`trước khi nó có thể phân nhánh. Tiền tố bổ sung đó đóng góp (f(k)). 

Trường hợp đặc biệt (k=1) không cần thêm nút nào như vậy, vì bản thân nút con bên trái có thể là một lá. Như vậy 

[ 
D(1)=f(1)=1 
] 

và, với (n>1), 

[ 
D(n)=f(n)+ 
\min\left( 
D(n-1)+1,, 
\min_{2\le k<n} 
{D(k)+D(n-k)+f(k)} 
\đúng). 
] 

Phép lặp tương tự thường được viết ở mức tối thiểu trên tất cả (k), với số hạng (k=1) được xử lý riêng. 

Nếu chúng ta tính toán trực tiếp (D(1),D(2),\ldots,D(n)) thì mỗi trạng thái sẽ thử (O(n)) phân chia, cho ra tổng công (O(n^2)). Đối với (n=10^{15}), đó là khoảng (10^{30}) đánh giá phân chia, vì vậy cách tiếp cận này không khả thi chút nào. 

Nhận xét tiếp theo là (f) là hàm lồi rời rạc. Mức tăng của nó từ (k-1) đến (k) chính xác là độ dài bit của (k), không bao giờ giảm. Hàm DP tối ưu (D) cũng là hàm lồi rời rạc. Do đó, đối với cố định (n), biểu thức 

[ 
D(k)+f(k)+D(n-k) 
] 

lồi như một hàm của (k). Do đó, mức tối thiểu của nó có thể được tìm thấy bằng tìm kiếm số nguyên ba chiều thay vì quét mọi phần tách có thể. Điều này làm giảm việc tính toán một đánh giá (D(n)) mới xuống còn khoảng (O(\log n)). Công thức lồi và tìm kiếm bậc ba là phép rút gọn tiêu chuẩn đầu tiên cho bài toán này. 

Điều đó vẫn không giải quyết được các ràng buộc thực tế, vì việc tính toán mọi giá trị lên tới (10^{15}) vẫn là không thể. Quan sát cuối cùng còn bất thường hơn: mặc dù (D(n)) phát triển trên một miền rất lớn nhưng độ dốc rời rạc của nó chỉ thay đổi một số lần nhỏ. Lên đến (10^{15}), chỉ có khoảng (1800) thay đổi độ dốc. Một báo cáo triển khai được công bố có 1833 điểm dừng như vậy, trong khi một giải pháp khác mô tả hiện tượng tương tự chỉ có khoảng 1840 vùng dốc riêng biệt. 

Vì vậy, thay vì lưu trữ (D(n)) cho mọi (n), chúng tôi lưu trữ phần đầu của mỗi đoạn tuyến tính, độ dốc và giá trị của nó ở đó. Giữa hai điểm dừng liên tiếp, 

[ 
D(x)=D(p)+s(x-p). 
] 

Để khám phá điểm dừng tiếp theo, chúng tôi tạm thời mở rộng phân đoạn tuyến tính hiện tại và so sánh phép ngoại suy đó với phép truy toán thực sự được đánh giá bằng hàm tuyến tính từng đoạn đã biết. Điểm cuối cùng mà cả hai cùng thống nhất là điểm cuối của đoạn hiện tại. Bởi vì vị từ đẳng thức thay đổi đơn điệu nên tìm kiếm nhị phân sẽ tìm thấy điểm đó. Bản thân sự lặp lại được giảm thiểu bằng cách tìm kiếm ba ngôi, tạo ra độ phức tạp tiền xử lý (O(M\log^3 N)), trong đó (M) là số lần thay đổi độ dốc. Mỗi truy vấn sau đó chỉ cần tìm kiếm nhị phân giữa các điểm dừng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP | (O(N^2)) | (O(N)) | Quá chậm | 
| DP lồi cho mọi (n) | (O(N\log N)) | (O(N)) | Vẫn còn quá chậm | 
| DP tuyến tính từng phần | (O(M\log^3 N+T\log M)) | (O(M)) | Đã chấp nhận | 

Ở đây (N=10^{15}), (T\le 50.000) và (M) chỉ ở khoảng (1833) cho phạm vi được yêu cầu. 

## Hướng dẫn thuật toán 

1. Xác định 

[ 
f(k)=\sum_{j=1}^{k}\operatorname{bit_length}(j). 
] 

Chúng tôi cần (f(k)) lặp đi lặp lại, vì vậy chúng tôi tính toán nó trực tiếp từ bit được đặt cao nhất thay vì tính tổng tất cả các số hạng (k). Nếu (b=\operatorname{bit_length}(k)), thì 

[ 
f(k)=(b-2)2^{b-1}+1+b(k-2^{b-1}+1). 
] 

Điều này tạo nên một đánh giá về (f(k)) (O(1)). 
2. Xác định (D(n)) là chi phí tối thiểu của một phép thử hợp lệ với (n) từ mã. Trường hợp cơ sở là (D(1)=1). 

Với (n>1), tách các lá ở gốc. Nếu`0`bên có (k) lá và`1`bên có (n-k), gốc đóng góp (f(n)). các`1`bên đóng góp (D(n-k)). Với (k>1),`0`bên đầu tiên phải có một sự ép buộc`1`cạnh, đóng góp (f(k)), theo sau là cấu trúc (D(k)) tối ưu. Khi (k=1), cạnh cưỡng bức đó là không cần thiết vì`0`đứa trẻ đã là một chiếc lá rồi. 

Như vậy 

[ 
D(n)=f(n)+ 
\min\left( 
D(n-1)+1,, 
\min_{2\le k<n} 
{D(k)+D(n-k)+f(k)} 
\đúng). 
] 
3. Lưu trữ phần hiện đã biết của (D) dưới dạng các đoạn tuyến tính từng đoạn. Cho phép`P[i]`là (x) đầu tiên trong đoạn (i),`S[i]`độ dốc của nó và`V[i]`giá trị (D(P[i])). Sau đó 

[ 
D(x)=V[i]+S[i](x-P[i]) 
] 

bất cứ khi nào (P[i]\le x<P[i+1]). 

Ban đầu (P=[1]), (S=[4]) và (V=[1]). Đoạn đầu tiên chứa (D(1)=1) và (D(2)=5), có hiệu là (4). 
4. Thực hiện một chức năng`known(x)`tìm đoạn chứa (x) bằng tìm kiếm nhị phân và đánh giá biểu thức tuyến tính tương ứng. 

Hàm này được cố tình cho phép đánh giá các giá trị lớn hơn nhiều so với điểm dừng hiện đã biết. Nó đại diện cho phép ngoại suy tuyến tính của phân đoạn được phát hiện mới nhất. 
5. Thực hiện`next_value(n)`sử dụng phép truy toán, thay thế mọi ẩn số (D(k)) bằng`known(k)`. chức năng 

[ 
D(k)+f(k)+D(n-k) 
] 

lồi trong (k), do đó tìm kiếm số nguyên ba ngôi tìm giá trị nhỏ nhất của nó. Trường hợp (k=1) được xử lý riêng biệt như`known(n - 1) + 1`. 
6. Giả sử đoạn tuyến tính hiện tại bắt đầu tại (p). Tìm (x>p) lớn nhất mà 

[ 
đã biết(x)=next_value(x). 
] 

Miễn là đường ngoại suy là hàm tối ưu thực tế thì hai giá trị đó trùng nhau. Tại điểm đầu tiên mà phép truy hồi tạo ra giá trị lớn hơn, độ dốc sẽ thay đổi. Điều kiện đẳng thức là đơn điệu trong khoảng tìm kiếm này, do đó tìm kiếm nhị phân tìm thấy điểm bằng cuối cùng. 
7. Thêm điểm dừng được phát hiện. Giá trị của nó được lấy từ đoạn cũ và độ dốc mới là 

[ 
D(p+1)-D(p). 
] 

Chúng tôi thu được (D(p+1)) từ`next_value(p + 1)`. Việc lặp lại quá trình này sẽ tạo ra tất cả các đoạn dốc lên tới (10^{15}). 
8. Với mỗi đầu vào (n), tìm kiếm nhị phân`P`để tìm phân đoạn của nó và đánh giá công thức tuyến tính. Câu trả lời chính xác là (D(n)). 

### Tại sao nó hoạt động 

Phép truy toán cây xem xét mọi số lượng (k) lá có thể có trên`0`bên, vì vậy nó bao gồm mọi hình dạng tri hợp lệ. Hạn chế về cấu trúc đặc biệt duy nhất là sự buộc phải`1`sau một chiếc lá không`0`nút và đó chính xác là số hạng (f(k)) bổ sung trong phép truy hồi. 

Việc tối ưu hóa là hợp lệ vì (f) và (D) là lồi rời rạc. Do đó hàm chi phí phân chia là hàm lồi và tìm kiếm bậc ba tìm thấy giá trị tối thiểu của nó. Biểu diễn từng phần duy trì tính bất biến mà mọi phân đoạn được lưu trữ đều khớp chính xác với hàm DP thực. Tại ranh giới đoạn, đoạn tiếp theo được tìm thấy bằng cách định vị điểm cuối cùng mà phép truy toán vẫn phù hợp với phép ngoại suy tuyến tính hiện tại. Sau thời điểm đó, phép truy toán xác định một độ dốc mới, do đó, việc thêm điểm dừng đó sẽ giữ nguyên bất biến. Vì mọi truy vấn được đánh giá từ một phân đoạn chính xác nên giá trị trả về là giá trị tối ưu thực sự. 

## Giải pháp Python```python
import sys
from bisect import bisect_right

input = sys.stdin.readline

MAX_N = 10**15
SEARCH_HIGH = 10**16

def f(n):
    """sum(bit_length(j) for j = 1..n), for n >= 1"""
    b = n.bit_length()
    first = 1 << (b - 1)

    # Sum_{j=1}^{first-1} bit_length(j)
    base = (b - 2) * first + 1

    # j = first .. n all have bit length b
    return base + b * (n - first + 1)

def build_segments():
    # P[i] = first x of segment i
    # S[i] = slope on segment i
    # V[i] = D(P[i])
    P = [1]
    S = [4]
    V = [1]

    def known(x):
        i = bisect_right(P, x) - 1
        return V[i] + S[i] * (x - P[i])

    def next_value(n):
        # k = 1 is special: the 0-child can itself be a leaf.
        lo = 1
        hi = n - 1

        while lo + 3 < hi:
            x1 = (2 * lo + hi) // 3
            x2 = (lo + 2 * hi) // 3

            v1 = known(x1) + known(n - x1) + f(x1)
            v2 = known(x2) + known(n - x2) + f(x2)

            if v1 > v2:
                lo = x1
            else:
                hi = x2

        best = known(n - 1) + 1

        for k in range(lo, hi + 1):
            if k == 1:
                cur = known(n - 1) + 1
            else:
                cur = known(k) + known(n - k) + f(k)
            if cur < best:
                best = cur

        return f(n) + best

    while True:
        left = P[-1] + 1
        right = SEARCH_HIGH

        # Find the last point where the current linear extrapolation
        # is still equal to the recurrence.
        while left < right:
            mid = (left + right + 1) // 2
            if known(mid) == next_value(mid):
                left = mid
            else:
                right = mid - 1

        p = left

        if p > MAX_N:
            break

        value_at_p = known(p)

        # The slope after p is D(p+1) - D(p).
        new_slope = next_value(p + 1) - value_at_p

        P.append(p)
        V.append(value_at_p)
        S.append(new_slope)

    return P, S, V

P, S, V = build_segments()

def answer(n):
    i = bisect_right(P, n) - 1
    return V[i] + S[i] * (n - P[i])

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(str(answer(n)))
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```chức năng`f`là tối ưu hóa đầu tiên quan trọng đối với việc triển khai Python. Vì mọi (j) trong khoảng ([2^{b-1},2^b-1]) có độ dài bit (b), nên tổng có thể được nhóm theo độ dài bit. Số nguyên Python cũng loại bỏ các mối lo ngại về tràn mà nếu không sẽ cần được chú ý khi triển khai C++. 

Các mảng`P`,`S`, Và`V`là biểu diễn rút gọn của DP.`known(x)`công dụng`bisect_right`vì điểm dừng thuộc về phân đoạn mới của nó. Quy ước ranh giới này là cần thiết. Nếu (P=[1,2,\ldots]), việc truy vấn chính xác (x=2) phải sử dụng đoạn bắt đầu tại (2), chứ không phải đoạn kết thúc ngay trước nó.`next_value`theo dõi sự tái phát một cách chính xác. Biểu thức cho (k=1) được cố tình tách biệt. Viết`known(1) + known(n - 1) + f(1)`sẽ thêm một nút nhân tạo và tạo ra câu trả lời sai cho (n=2). 

Việc tìm kiếm bậc ba dừng lại khi vẫn còn ít hơn bốn ứng cử viên, sau đó kiểm tra rõ ràng các giá trị nguyên còn lại. Điều này tránh việc dựa vào tìm kiếm ba phần dấu phẩy động và ngăn ngừa các lỗi riêng lẻ ở mức tối thiểu. 

Trong quá trình tiền xử lý,`known`là phép ngoại suy chứ không phải là lời hứa rằng DP đã được biết ở tọa độ đó. Tìm kiếm nhị phân chỉ hỏi phép ngoại suy này tiếp tục thỏa mãn phép truy toán ở đâu. Sau khi đạt được sự bất đồng đầu tiên, một độ dốc mới sẽ được đưa ra. Tiền tố được lưu trữ của các phân đoạn vẫn chính xác trong suốt quá trình. 

Quá trình xử lý trước kết thúc sau khi điểm ngắt tiếp theo vượt quá (10^{15}). Giới hạn tìm kiếm trên của (10^{16}) chỉ là giới hạn an toàn thuận tiện được sử dụng để định vị điểm dừng tiếp theo chứ không phải là khẳng định rằng các truy vấn có thể đạt đến giá trị đó. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, (n=2), phép truy hồi chỉ có một khả năng phân chia. 

| (n) | (k) | Chia chi phí trước (f(n)) | (f(n)) | (Đ(n)) | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | (D(1)+1=2) | 3 | 5 | 

Kết quả là (5). Quy tắc đặc biệt (k=1) được hiển thị ngay lập tức: từ mã bên trái có thể đơn giản là`0`, do đó không cần thêm tiền tố có giá (f(1)) . 

Đối với mẫu thứ hai, (n=4), các giá trị DP liên quan là (D(1)=1), (D(2)=5) và (D(3)=11). 

| (n) | (k) | Ứng viên trước (f(n)) | (f(n)) | Tổng số ứng viên | 
| --- | --- | --- | --- | --- | 
| 4 | 1 | (D(3)+1=12) | 8 | 20 | 
| 4 | 2 | (D(2)+D(2)+f(2)=13) | 8 | 21 | 
| 4 | 3 | (D(3)+D(1)+f(3)=17) | 8 | 25 | 

Mức tối thiểu đạt được bằng (k=1), cho (D(4)=20). Điều này tương ứng với việc giữ bên trái`0`nhánh thành một chiếc lá duy nhất và đặt ba lá còn lại dưới`1`nhánh, khớp chính xác với cấu trúc đằng sau từ điển mẫu. 

Đối với mẫu thứ ba, (n=10), mức tối ưu là (98). 

| (n) | (k) | Ứng viên trước (f(10)) | (f(10)) | Tổng số ứng viên | 
| --- | --- | --- | --- | --- | 
| 10 | 1 | (D(9)+1=83) | 29 | 112 | 
| 10 | 2 | (D(2)+D(8)+f(2)=75) | 29 | 104 | 
| 10 | 3 | (D(3)+D(7)+f(3)=69) | 29 | 98 | 
| 10 | 4 | (D(4)+D(6)+f(4)=69) | 29 | 98 | 
| 10 | 5 | (D(5)+D(5)+f(5)=71) | 29 | 100 | 

Có hai phần chia trung tâm tốt như nhau trong dấu vết này, (k=3) và (k=4), cả hai đều cho (98). Do đó, đầu ra mẫu là (98). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(M\log^3 N+T\log M)) | Mỗi điểm dừng sử dụng tìm kiếm nhị phân trên vị trí của nó và tìm kiếm ba bên trong hàm truy hồi, với thời gian không đổi (f). Mỗi truy vấn sử dụng tìm kiếm nhị phân trên các điểm dừng. | 
| Không gian | (O(M)) | Chỉ các vị trí điểm dừng, độ dốc và giá trị được lưu trữ. | 

Ở đây (N=10^{15}), (T\le 50.000) và (M) chỉ nằm trong khoảng (1833) trong phạm vi được yêu cầu. Do đó, quá trình tiền xử lý rất nhỏ so với bất kỳ thứ gì phụ thuộc tuyến tính vào (N) và giai đoạn truy vấn chỉ thực hiện vài chục thao tác tìm kiếm nhị phân cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm 

Khai thác sau đây giả định giải pháp được gửi được đặt trong một hàm có tên`solve_case`. Kiểm tra kích thước tối đa có chủ ý kiểm tra đầu vào hợp pháp lớn nhất thông qua cùng một bộ máy tiền xử lý, trong khi các giá trị lặp lại kiểm tra xem các truy vấn có độc lập hay không và việc tra cứu phân đoạn có ổn định hay không.```python
# Put the solution's solve_case(n) implementation above this test code.

def run(inp: str) -> str:
    import io
    import sys

    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        t = int(sys.stdin.readline())
        ans = []
        for _ in range(t):
            n = int(sys.stdin.readline())
            ans.append(str(solve_case(n)))
        return "\n".join(ans)
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run("""3
2
4
10
""") == """5
20
98""", "provided sample"

# Minimum-size input
assert run("""1
2
""") == "5", "n=2"

# Several small values, including the first slope changes
assert run("""5
3
4
5
6
7
""") == """11
20
30
41
53""", "small DP values"

# Repeated equal values
assert run("""4
10
10
10
10
""") == """98
98
98
98""", "repeated queries"

# Maximum-size input. The result must be a valid integer and the same
# value must be obtained twice, exercising the final piecewise segment.
mx = run("""2
1000000000000000
1000000000000000
""").splitlines()

assert len(mx) == 2, "maximum-size query count"
assert mx[0] == mx[1], "maximum-size repeated query"
assert mx[0].isdigit(), "maximum-size result must be an integer"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 2`|`5`| Pháp lý tối thiểu (n), bao gồm cả quá trình chuyển đổi đặc biệt (k=1) | 
|`3,4,5,6,7`|`11,20,30,41,53`| Chuyển tiếp DP nhỏ và thay đổi độ dốc | 
|`10,10,10,10`|`98,98,98,98`| Giá trị lặp lại và trường hợp thử nghiệm độc lập | 
|`1000000000000000`hai lần | Cùng một số nguyên hai lần | Kích thước đầu vào tối đa và phân đoạn tuyến tính từng phần cuối cùng | 

## Vỏ cạnh 

Với (n=2), phép chia duy nhất có một lá trên`0`bên. Thuật toán sử dụng`known(n-1)+1`, do đó phần đóng góp trước gốc là (1+1=2) và chi phí gốc là (f(2)=3). Kết quả là (5). Công thức chia chung cộng thêm (f(1)) sẽ trả về sai (6). 

Với (n=4), mức phân chia tối ưu là (k=1). Thuật toán so sánh ứng cử viên đặc biệt (D(3)+1=12) với các ứng cử viên thông thường (D(2)+D(2)+f(2)=13) và (D(3)+D(1)+f(3)=17). Sau khi thêm (f(4)=8), câu trả lời là (20). Điều này kiểm tra rằng buộc`1`sau một chiếc lá không`0`cây con được mô hình hóa chính xác. 

Đối với (n=10), mức tối thiểu xảy ra ở nhiều lần phân chia. Các ứng cử viên cho (k=3) và (k=4) đều cho ra (98). Tìm kiếm số nguyên ternary không cần một bộ thu nhỏ duy nhất, nó chỉ cần mục tiêu là lồi, do đó mức tối thiểu phẳng được xử lý chính xác. 

Đối với (n=10^{15}), thuật toán không bao giờ xây dựng (10^{15}) trạng thái DP. Đầu tiên, nó xây dựng một tập hợp nhỏ các điểm dừng độ dốc, sau đó đánh giá giá trị được yêu cầu bằng cách sử dụng một tra cứu phân đoạn và một biểu thức tuyến tính. Đây là trường hợp tách biệt giải pháp mong muốn khỏi bất kỳ DP nào có bộ nhớ hoặc thời gian chạy tăng lên với (n).
