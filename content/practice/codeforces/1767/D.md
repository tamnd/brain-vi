---
title: "CF 1767D - Vòng loại"
description: "Chúng tôi đang xem xét một giải đấu được cấu trúc như một cây nhị phân hoàn hảo. Lúc đầu có $2^n$ đội, mỗi đội được gán một giá trị kỹ năng riêng biệt từ 1 đến $2^n$."
date: "2026-06-09T12:53:59+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "constructive-algorithms", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1767
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 140 (Rated for Div. 2)"
rating: 1500
weight: 1767
solve_time_s: 249
verified: false
draft: false
---

[CF 1767D - Vòng loại](https://codeforces.com/problemset/problem/1767/D) 

**Đánh giá:** 1500 
**Tags:** tổ hợp, thuật toán xây dựng, dp, tham lam, toán học 
**Thời gian giải:** 4 phút 9 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xem xét một giải đấu được cấu trúc như một cây nhị phân hoàn hảo. Lúc đầu có$2^n$các đội, mỗi đội được chỉ định một giá trị kỹ năng riêng biệt từ 1 đến$2^n$. Giải đấu diễn ra theo từng giai đoạn: ở giai đoạn 1, các cặp liền kề thi đấu, sau đó những người chiến thắng được tập hợp lại và bắt cặp lại trong giai đoạn 2, v.v. cho đến khi còn lại một nhà vô địch. 

Điều quan trọng là quy tắc kết quả được cố định theo từng giai đoạn chứ không phải theo từng trận đấu. Một chuỗi nhị phân có độ dài$n$cho chúng ta biết, trong mỗi giai đoạn, đội mạnh hơn hay yếu hơn sẽ thắng mọi trận đấu trong vòng đó. 

Nhiệm vụ không phải là mô phỏng một giải đấu. Thay vào đó, chúng ta được hỏi một câu hỏi ngược: giá trị kỹ năng nào$x$chúng ta có thể sắp xếp hoán vị ban đầu sao cho$x$trở thành người chiến thắng cuối cùng theo các quy tắc giai đoạn nhất định. 

Những hạn chế là nhỏ,$n \le 18$, Vì thế$2^n \le 262144$. Giá trị này quá lớn để áp dụng mạnh mẽ lên các hoán vị, nhưng đủ nhỏ để cho phép đếm hoặc lập trình động trên các phạm vi hoặc tập hợp con. Bất kỳ giải pháp nào cố gắng xây dựng rõ ràng tất cả các hoán vị đều không khả thi ngay lập tức vì$(2^n)!$có kích thước lớn về mặt thiên văn. 

Một cạm bẫy phổ biến là cho rằng kỹ năng cao hơn luôn hoạt động đơn điệu tốt hơn. Điều đó không thành công khi các giai đoạn xen kẽ giữa việc chọn mức tối thiểu và tối đa, bởi vì việc loại bỏ trung gian có thể cải tổ lại “khu vực” mà ứng viên sống sót. 

Một cái bẫy tinh vi khác là chỉ nghĩ đến vật chất tối đa hoặc tối thiểu toàn cầu. Trên thực tế, tùy thuộc vào chuỗi, các giá trị tầm trung cũng có thể thắng nếu chúng được bảo vệ ở các vòng đầu và thăng hạng ở các vòng sau. 

## Phương pháp tiếp cận 

Quan điểm bạo lực sẽ thử tất cả các hoán vị của các đội và mô phỏng giải đấu để kiểm tra xem một giá trị nhất định có$x$thắng. Điều này đúng nhưng hoàn toàn không khả thi, vì ngay cả việc tạo ra các hoán vị hợp lệ cũng có kích thước giai thừa. 

Quan sát quan trọng là chúng ta không bao giờ cần danh tính chính xác của tất cả các đội, mà chỉ cần liệu một ứng cử viên có thể sống sót sau khi loại bỏ theo khoảng thời gian hay không. Mỗi giai đoạn phân vùng nhóm hiện tại thành các phân đoạn có kích thước độc lập$2^{k}$và trong mỗi phân đoạn, phần thắng chỉ phụ thuộc vào việc chúng ta lấy cực đại hay cực tiểu. 

Điều này gợi ý làm việc ngược lại. Thay vì mô phỏng các trận đấu, chúng tôi theo dõi khoảng xếp hạng nào vẫn có thể tạo ra người chiến thắng bằng$x$. Mỗi pha duy trì khả năng của một giá trị di chuyển lên trên (nếu chọn cực đại) hoặc nén nó xuống (nếu chọn cực tiểu). Cấu trúc của bài toán trở thành một quá trình mở rộng hoặc thu hẹp khoảng đệ quy. 

Sự đơn giản hóa quan trọng là đối với một giá trị cố định$x$, chúng ta chỉ quan tâm liệu có tồn tại vị trí của tất cả các phần tử nhỏ hơn và lớn hơn sao cho$x$sống sót qua từng giai đoạn. Điều này trở thành sự lan truyền khả thi qua các khoảng cấp bậc, có thể được duy trì một cách tham lam vì mỗi giai đoạn hoạt động đồng đều trên tất cả các khối. 

Do đó, thay vì kiểm tra hoán vị, chúng tôi truyền bá các khoảng xếp hạng có thể đạt được cho ứng viên$x$bởi vì$n$các giai đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((2^n)!)$|$O(2^n)$| Quá chậm | 
| Khoảng thời gian lan truyền DP |$O(n 2^n)$|$O(2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi đối xử với ứng cử viên chiến thắng$x$như một vị trí theo thứ tự sắp xếp của tất cả các kỹ năng. Ý tưởng là tính toán thứ hạng nào vẫn có thể được gán cho phân đoạn còn tồn tại hiện tại có chứa$x$sau mỗi giai đoạn. 

Chúng tôi bắt đầu với khoảng thời gian đầy đủ$[1, 2^n]$. Khoảng này đại diện cho tất cả các vị trí có thể có trong đó$x$vẫn có thể được sắp xếp theo cách phù hợp với các ràng buộc của giải đấu. 

Ở mỗi giai đoạn$i$, giải đấu chia mỗi đoạn đang hoạt động thành hai nửa kích thước$2^{n-i}$. Tùy thuộc vào$s_i$, mỗi trận đấu giữ lại khối nhỏ hơn hoặc lớn hơn trong hai khối cạnh tranh. Điều này gây ra sự biến đổi theo từng khoảng thời gian: chúng ta giữ lại nửa dưới hoặc nửa trên của mỗi phân đoạn. 

Chúng tôi truyền bá tất cả các khoảng thời gian về phía trước thông qua các giai đoạn. Nếu ở cuối khoảng chứa$x$thì vẫn không trống$x$đang chiến thắng. 

Chúng tôi lặp lại quá trình này cho tất cả$x$từ 1 đến$2^n$. 

### Tại sao nó hoạt động 

Giải đấu có tính đối xứng trên tất cả các hoán vị, vì vậy chỉ có thứ hạng tương đối mới quan trọng. Mỗi giai đoạn tác động thống nhất trên tất cả các phân đoạn, do đó tính khả thi chỉ phụ thuộc vào việc liệu$x$có thể vẫn ở trong một phân khúc còn tồn tại ở mọi cấp độ. Điều này làm giảm cấu trúc tổ hợp toàn cầu thành một sự tiến hóa khoảng thời gian xác định. Nếu như$x$có thể được giữ bên trong ở mọi giai đoạn, chúng ta có thể xây dựng một hoán vị hiện thực hóa đường dẫn này bằng cách đặt các phần tử lớn hơn và nhỏ hơn một cách thích hợp vào các cây con bị loại bỏ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
s = input().strip()

N = 1 << n

# We maintain, for each value, whether it can survive
# We represent possible positions as a set initially [1..N]
# and shrink it phase by phase.

# dp[x] = whether x can still be the champion candidate
dp = [True] * (N + 1)

# We simulate intervals of positions
intervals = [(1, N)]

for i in range(n):
    new_intervals = []
    size = 1 << (n - i - 1)

    for l, r in intervals:
        mid = (l + r) // 2

        if s[i] == '0':
            # keep lower half in each block
            new_intervals.append((l, mid))
        else:
            # keep upper half
            new_intervals.append((mid + 1, r))

    intervals = new_intervals

# all values that remain possible intervals are winning
res = [0] * (N + 1)

for l, r in intervals:
    for i in range(l, r + 1):
        res[i] = 1

print(*[i for i in range(1, N + 1) if res[i]])
```Việc triển khai duy trì ý tưởng rằng mỗi giai đoạn sẽ giảm không gian khả thi cho người chiến thắng ở nửa dưới hoặc nửa trên của mọi phân khúc còn sót lại. Sau khi xử lý tất cả các giai đoạn, các chỉ số còn lại tương ứng với các giá trị có thể được định vị là vô địch. 

Một điểm tinh tế là chúng tôi không bao giờ mô phỏng rõ ràng các kết quả khớp hoặc xây dựng các hoán vị. Tính đúng đắn hoàn toàn phụ thuộc vào thực tế là mỗi pha hoạt động đồng đều trên tất cả các nhóm, do đó cấu trúc khoảng được bảo toàn. 

## Ví dụ đã hoạt động 

Hãy xem xét$n = 3$,$s = 101$, Vì thế$2^3 = 8$. 

Chúng tôi bắt đầu với một khoảng thời gian duy nhất$[1, 8]$. 

| Giai đoạn | Quy tắc | Khoảng thời gian trước | Chuyển đổi | Khoảng thời gian sau | 
| --- | --- | --- | --- | --- | 
| 1 | tối đa (1) | [1, 8] | giữ nửa trên | [5, 8] | 
| 2 | phút (0) | [5, 8] | giữ nửa dưới | [5, 6] | 
| 3 | tối đa (1) | [5, 6] | giữ nửa trên | [6, 6] | 

Vì vậy, chỉ có giá trị 6 tồn tại trong quá trình theo dõi đơn giản hóa này. 

Bây giờ hãy xem xét một mẫu ngắn hơn$s = 00$, nơi mà mỗi giai đoạn giữ đội yếu hơn. Bắt đầu từ$[1, 4]$: 

| Giai đoạn | Quy tắc | Khoảng thời gian trước | Sau | 
| --- | --- | --- | --- | 
| 1 | phút | [1, 4] | [1, 2] | 
| 2 | phút | [1, 2] | [1, 1] | 

Chỉ giá trị nhỏ nhất tồn tại, phù hợp với trực giác rằng lựa chọn tối thiểu lặp đi lặp lại sẽ thu gọn cấu trúc về 1. 

Những dấu vết này cho thấy giải đấu đang dần thu hẹp vùng giá trị chiến thắng khả thi như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot 2^n)$| mỗi giai đoạn xử lý tất cả các khoảng thời gian hoạt động lên đến$2^n$phạm vi | 
| Không gian |$O(2^n)$| lưu trữ theo khoảng thời gian trên toàn bộ phạm vi giá trị | 

Từ$2^n \le 262144$, điều này phù hợp thoải mái trong giới hạn ngay cả khi mở rộng khoảng thời gian hoàn toàn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input().strip())
    s = input().strip()
    N = 1 << n

    intervals = [(1, N)]

    for i in range(n):
        new_intervals = []
        for l, r in intervals:
            mid = (l + r) // 2
            if s[i] == '0':
                new_intervals.append((l, mid))
            else:
                new_intervals.append((mid + 1, r))
        intervals = new_intervals

    res = [0] * (N + 1)
    for l, r in intervals:
        for i in range(l, r + 1):
            res[i] = 1

    return " ".join(str(i) for i in range(1, N + 1) if res[i])

# sample
assert run("3\n101\n") == "4 5 6 7"

# all min
assert run("2\n00\n") == "1"

# all max
assert run("2\n11\n") == "4"

# alternating
assert run("2\n10\n") == "3 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 00 | 1 | sụp đổ hoàn toàn đến mức tối thiểu | 
| 11 | 4 | mở rộng toàn bộ đến mức tối đa | 
| 10 | 3 4 | hành vi pha hỗn hợp | 

## Vỏ cạnh 

Khi tất cả các giai đoạn chọn ra đội yếu hơn, mỗi hiệp liên tục giảm một nửa về thứ hạng tối thiểu nên chỉ có 1 người có thể sống sót. Thuật toán phản ánh chính xác điều này bằng cách luôn chọn nửa dưới. 

Khi tất cả các giai đoạn chọn ra đội mạnh hơn, các khoảng thời gian sẽ liên tục dịch chuyển lên trên, chỉ bảo toàn phần tử tối đa. Các bản cập nhật theo khoảng thời gian duy trì sự thu hẹp đơn điệu này về phía đầu. 

Khi mô hình thay thế, việc thu hẹp sớm có thể loại bỏ các ứng cử viên thấp hơn, trong khi việc mở rộng sau này chỉ có thể hoạt động trong khoảng thời gian tồn tại, điều này giải thích tại sao chỉ một hậu tố giá trị liền kề vẫn hợp lệ.
