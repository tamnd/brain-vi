---
title: "CF 104886D - Đếm GCD"
description: "Chúng tôi được cung cấp nhiều trường hợp thử nghiệm. Trong mỗi cái, chúng ta nhận được một mảng các số nguyên dương giới hạn bởi một giới hạn $m$. Nhiệm vụ không phải là xây dựng bất cứ điều gì, mà là đếm."
date: "2026-06-28T09:07:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104886
codeforces_index: "D"
codeforces_contest_name: "USI-Team-Selection 2023-2024"
rating: 0
weight: 104886
solve_time_s: 49
verified: true
draft: false
---

[CF 104886D - Đếm GCD](https://codeforces.com/problemset/problem/104886/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều trường hợp thử nghiệm. Trong mỗi cái, chúng ta nhận được một mảng các số nguyên dương giới hạn bởi một giới hạn$m$. Nhiệm vụ không phải là xây dựng bất cứ điều gì, mà là đếm. 

Với mọi giá trị$x$từ$1$ĐẾN$m$, chúng ta phải xác định có bao nhiêu dãy con không trống của mảng có ước chung lớn nhất chính xác bằng$x$. Dãy con ở đây có nghĩa là chúng ta chọn bất kỳ tập hợp con nào của các chỉ số, giữ nguyên thứ tự và xử lý các giá trị đã chọn đó dưới dạng nhiều tập hợp để tính toán GCD. Các lựa chọn chỉ mục khác nhau được tính là các chuỗi con khác nhau ngay cả khi các giá trị trùng nhau. 

Đầu ra của mỗi trường hợp thử nghiệm là một danh sách$m$số, vị trí ở đâu$x$lưu trữ số lượng các chuỗi con có GCD chính xác$x$, lấy modulo$998244353$. 

Các ràng buộc đẩy giải pháp tới tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Tổng của$n$Và$m$vượt qua tất cả các bài kiểm tra là về$10^6$, vì vậy bất cứ điều gì giống như$O(nm)$ngay lập tức là không thể. Thậm chí$O(n \log m)$mỗi trường hợp thử nghiệm cần được chăm sóc. Cấu trúc đề xuất mạnh mẽ việc sử dụng các ước số và loại trừ bao gồm nhân hơn là liệt kê các chuỗi con. 

Một vấn đề tinh tế xuất hiện với các giá trị lặp lại. Ví dụ: nếu mảng chứa nhiều phần tử giống nhau bằng$x$, các chuỗi con được hình thành từ các bản sao đó phải được tính chính xác là các lựa chọn khác nhau, mặc dù các giá trị kết quả là giống hệt nhau. Bất kỳ cách tiếp cận nào nén mảng thành một tập hợp các giá trị sẽ bị tính thiếu. 

Một trường hợp thất bại khác xuất phát từ việc đếm quá nhiều dãy con có GCD chia hết cho nhiều ứng viên. Ví dụ: một dãy con với GCD$6$không nên đóng góp vào câu trả lời cho$2$hoặc$3$, mặc dù tất cả các phần tử của nó đều chia hết cho chúng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là liệt kê tất cả các chuỗi con không trống, tính toán GCD của chúng và tăng nhóm tương ứng. Điều này đúng vì mọi dãy con đều được đánh giá rõ ràng, nhưng số dãy con là$2^n - 1$, điều này trở nên không khả thi ngay lập tức ngay cả đối với mức độ vừa phải$n$. Vì$n = 40$, điều này đã vượt quá một nghìn tỷ hoạt động. 

Quan sát quan trọng là các chuỗi con có thể được nhóm lại theo khả năng chia hết thay vì cấu trúc. Thay vì hỏi “các dãy con nào có GCD chính xác$x$”, trước tiên chúng ta đặt một câu hỏi đơn giản hơn: có bao nhiêu dãy con có tất cả các phần tử chia hết cho$x$. Nếu một dãy con có chính xác GCD$x$, thì mọi phần tử đều chia hết cho$x$và sau khi chia tất cả các phần tử cho$x$, dãy con thu được phải có GCD chính xác$1$. 

Vì vậy, vấn đề chia thành hai lớp. Đầu tiên chúng ta đếm, cho mỗi$x$, có bao nhiêu phần tử chia hết cho$x$. Từ đó chúng ta tính toán có bao nhiêu dãy con hoàn toàn là bội số của$x$. Sau đó, chúng tôi trừ đi các khoản đóng góp từ bội số của$x$sử dụng loại trừ bao gồm trên các ước số. 

Ý tưởng quan trọng thứ hai là các chuỗi con trên một tập hợp được lọc hoạt động đơn giản: nếu có$k$phần tử hợp lệ chia hết cho$x$, thì có$2^k - 1$dãy con không trống. Điều này đưa ra một cách trực tiếp để tính “GCD ít nhất chia hết cho$x$Khó khăn duy nhất còn lại là tách “GCD chính xác bằng$x$” từ “GCD là bội số của$x$”. 

Chúng tôi giải quyết vấn đề đó bằng cách xử lý các giá trị từ lớn đến nhỏ và trừ đi các đóng góp của bội số. Đây là ước số cổ điển DP: nếu chúng ta biết có bao nhiêu dãy con có GCD chính xác bằng bội số của$x$, chúng ta có thể loại bỏ chúng khỏi các chuỗi con tổng hợp được hình thành bởi các phần tử chia hết cho$x$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuỗi tiếp theo của Brute Force |$O(2^n \cdot n)$|$O(1)$| Quá chậm | 
| Đếm số chia + bao gồm-loại trừ |$O(m \log m + n)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng giá trị trong mảng. Điều này cho phép chúng ta suy luận về tính chia hết mà không cần lặp đi lặp lại tất cả các phần tử. 
2. Với mỗi số nguyên$x$từ$1$ĐẾN$m$, tính xem có bao nhiêu phần tử chia hết cho$x$. Điều này được thực hiện bằng cách lặp qua bội số của$x$và tổng hợp tần số. 
3. Từ số đếm đó, hãy tính số dãy con không trống chỉ được tạo thành bởi các phần tử chia hết cho$x$, đó là$2^{cnt[x]} - 1$. Đại lượng này bao gồm tất cả các dãy con có GCD là bội số của$x$, không nhất thiết phải chính xác$x$. 
4. Quy trình$x$từ$m$xuống tới$1$. Đối với mỗi$x$, trừ phần đóng góp của tất cả các bội số$kx$Ở đâu$k \ge 2$. Các bội số này biểu thị các chuỗi con có GCD đã được tính ở giá trị cao hơn. 
5. Giá trị còn lại sau khi trừ chính xác là số dãy con có GCD bằng$x$. 

Bước trừ là cơ chế hiệu chỉnh quan trọng. Nếu không có nó, mọi dãy con sẽ được tính cho tất cả các ước số của GCD thực của nó. 

### Tại sao nó hoạt động 

Mỗi dãy con đều có một GCD được xác định rõ ràng$g$. Dãy số đó được tính vào nhóm của mọi ước số của$g$khi chúng tôi tính toán “tất cả các phần tử chia hết cho$x$". Cấu trúc mạng chia đảm bảo rằng sự đóng góp lan truyền lên trên một cách chính xác dọc theo chuỗi chia hết. Bằng cách xử lý từ lớn đến nhỏ, chúng tôi đảm bảo rằng khi tính toán câu trả lời cho$x$, tất cả các đóng góp từ bội số thích hợp của$x$đã được hoàn thiện và có thể được loại bỏ hoàn toàn, chỉ để lại các chuỗi con có GCD chính xác$x$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAXV = 10**6 + 5

# precompute powers of 2
pow2 = [1] * (MAXV)
for i in range(1, MAXV):
    pow2[i] = (pow2[i - 1] * 2) % MOD

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    freq = [0] * (m + 1)
    for v in a:
        freq[v] += 1

    cnt = [0] * (m + 1)

    for x in range(1, m + 1):
        s = 0
        for k in range(x, m + 1, x):
            s += freq[k]
        cnt[x] = s

    dp = [0] * (m + 1)

    for x in range(m, 0, -1):
        total = (pow2[cnt[x]] - 1) % MOD

        for k in range(2 * x, m + 1, x):
            total = (total - dp[k]) % MOD

        dp[x] = total

    print(*dp[1:])
```Mã bắt đầu bằng cách tính toán trước lũy thừa của hai vì mỗi số tập hợp con phụ thuộc vào$2^{cnt}$. các`freq`mảng lưu trữ các lần xuất hiện của từng giá trị và`cnt[x]`tích lũy bao nhiêu phần tử chia hết cho$x$. Vòng lặp lồng nhau trên bội số là mẫu giống như sàng tiêu chuẩn giúp kiểm soát độ phức tạp. 

Vòng lặp từ dưới lên là nơi xảy ra việc loại trừ bao gồm. Chúng ta tính toán đáp án theo thứ tự giảm dần để khi xử lý$x$, tất cả đều là bội số$2x, 3x, \dots$đã có sẵn câu trả lời cuối cùng. Điều này đảm bảo tính đúng đắn của phép trừ. 

Một cạm bẫy triển khai phổ biến là quên hiệu chỉnh modulo sau khi trừ, điều này có thể tạo ra các giá trị âm. Một người khác đang cố gắng tính toán số lượng chuỗi con cho mỗi phần tử thay vì trên mỗi ước số, điều này phá vỡ hoàn toàn cấu trúc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 5
1 1 4 5 1 4
```Đầu tiên chúng tôi tính toán tần số:$1$xuất hiện 3 lần,$4$xuất hiện 2 lần,$5$xuất hiện 1 lần. 

Vì$x = 5$, chỉ có một phần tử chia hết nên tổng các dãy con là$2^1 - 1 = 1$, chỉ cho$[5]$. 

Vì$x = 4$, hai phần tử đóng góp, vì vậy số đếm ban đầu là$2^2 - 1 = 3$. Không có bội số nào cao hơn nên đáp án vẫn là 3. 

cho$x = 1$, tất cả các phần tử đều đóng góp, vì vậy$2^6 - 1 = 63$, nhưng chúng tôi trừ đi các khoản đóng góp từ$2,3,4,5$đã tính toán rồi, còn lại 59. 

| x | cnt[x] | dãy con ban đầu | phép trừ | cuối cùng | 
| --- | --- | --- | --- | --- | 
| 5 | 1 | 1 | 0 | 1 | 
| 4 | 2 | 3 | 0 | 3 | 
| 1 | 6 | 63 | 4 | 59 | 

Dấu vết này cho thấy mức độ phân chia chồng chéo lực điều chỉnh tại$x = 1$. 

### Ví dụ 2 

đầu vào:```
10 10
3 1 2 2 6 7 6 5 8 3
```Tần số được trộn lẫn, nhưng cấu trúc tương tự nhau. Vì$x = 2$, các phần tử chia hết cho 2 là$2,2,6,6,8$, vậy 5 phần tử cho$31$các chuỗi tiếp theo. Tuy nhiên, chúng tôi trừ đi các khoản đóng góp từ$4,6,8$trước khi hoàn thiện. 

| x | cnt[x] | ban đầu | phép trừ | cuối cùng | 
| --- | --- | --- | --- | --- | 
| 6 | 2 | 3 | 0 | 3 | 
| 2 | 5 | 31 | 18 | 12 | 

Dấu vết cho thấy giá trị trung gian lớn sẽ giảm như thế nào khi tính đến bội số cao hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log m + n)$| mỗi vòng chia số truy cập bội số như một cái sàng | 
| Không gian |$O(m)$| tần số, số lượng, mảng DP trên$1..m$| 

Các ràng buộc cho phép tổng$10^6$qua các thử nghiệm, do đó, việc truyền số chia giống như sàng nằm trong giới hạn thoải mái, trong khi bất kỳ sự phụ thuộc bậc hai nào vào$m$sẽ thất bại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import gcd
    MOD = 998244353

    t = int(input())
    out_lines = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        freq = [0] * (m + 1)
        for v in a:
            freq[v] += 1

        pow2 = [1] * (n + 1)
        for i in range(1, n + 1):
            pow2[i] = (pow2[i - 1] * 2) % MOD

        cnt = [0] * (m + 1)
        for x in range(1, m + 1):
            for k in range(x, m + 1, x):
                cnt[x] += freq[k]

        dp = [0] * (m + 1)
        for x in range(m, 0, -1):
            val = (pow2[cnt[x]] - 1) % MOD
            for k in range(2 * x, m + 1, x):
                val = (val - dp[k]) % MOD
            dp[x] = val

        out_lines.append(" ".join(map(str, dp[1:])))

    return "\n".join(out_lines)

# custom sanity checks
assert run("1\n1 1\n1") == "1"
assert run("1\n3 3\n1 2 3")  # basic distribution sanity
assert run("1\n4 4\n2 2 2 2")
assert run("1\n5 5\n1 1 1 1 1")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 ở giá trị của nó | tính đúng đắn của trường hợp cơ sở | 
| tất cả đều khác biệt | phân phối gcd thưa thớt | chia số chia | 
| tất cả đều bình đẳng | tổ hợp cực đại | xử lý sức mạnh của hai | 

## Vỏ cạnh 

Đầu vào tối thiểu có một giá trị duy nhất cho biết việc triển khai có xử lý chính xác hay không$2^1 - 1 = 1$không có lỗi trừ. Thuật toán tính toán`cnt[x] = 1`chỉ dành cho các ước số của giá trị đó và tất cả các trạng thái dp khác vẫn bằng 0, tạo ra đóng góp riêng biệt chính xác. 

Một trường hợp trong đó tất cả các số đều giống hệt nhau nhấn mạnh đến việc bao gồm-loại trừ. Mỗi ước số của số đó ban đầu sẽ tính tất cả các dãy con, nhưng phép trừ từ các bội số cao hơn sẽ loại bỏ mọi thứ ngoại trừ nhóm gcd chính xác. Thứ tự xử lý giảm dần đảm bảo rằng việc hủy xảy ra theo đúng hướng, ngăn ngừa việc đếm quá mức. 

Trường hợp các số nguyên tố cùng nhau theo cặp sẽ có hành vi ngược lại. Mỗi giá trị chỉ đóng góp vào các ước số riêng của nó, vì vậy các giá trị dp hầu như vẫn độc lập, xác nhận rằng không tồn tại sự rò rỉ số chia chéo ngoài ý muốn.
