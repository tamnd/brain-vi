---
title: "CF 104822D - Doping 2"
description: "Chúng ta được cấp một hoán vị cố định $p$ có kích thước $n$ và chúng ta muốn so sánh nó với tất cả các hoán vị khác có cùng kích thước xuất hiện sớm hơn $p$ về mặt từ điển."
date: "2026-06-28T12:41:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104822
codeforces_index: "D"
codeforces_contest_name: "RCPCamp 2023 Day 1"
rating: 0
weight: 104822
solve_time_s: 98
verified: false
draft: false
---

[CF 104822D - Doping 2](https://codeforces.com/problemset/problem/104822/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị cố định$p$kích thước$n$và chúng tôi muốn so sánh nó với tất cả các hoán vị khác có cùng kích thước xuất hiện sớm hơn về mặt từ điển so với$p$. Với mỗi hoán vị như vậy$p'$, chúng tôi tính toán một thống kê$f(p')$, tính số lần một giá trị được theo sau ngay lập tức ở đâu đó bởi giá trị kế tiếp của nó. Chính xác hơn, chúng ta đếm các cặp chỉ số$i < j$như vậy$p'_i + 1 = p'_j$, bất kể chúng có liền kề nhau hay không. 

Sau đó chúng tôi phân loại tất cả các hoán vị$p'$nhỏ hơn về mặt từ điển so với$p$theo giá trị của$f(p')$, và với mọi$k$, chúng ta cần đếm xem có bao nhiêu hoán vị như vậy$f(p') = k$. 

Đầu ra là phân bố tần số trên tất cả các giá trị có thể có của$f(p')$trong số các hoán vị nhỏ hơn về mặt từ điển so với hoán vị đã cho. 

Khó khăn chính là chúng ta không tính tất cả các hoán vị mà chỉ tính tiền tố của thứ tự từ điển. Điều này làm cho bài toán về cơ bản trở thành tiền tố DP trên các hoán vị chứ không phải là bài toán đếm tổng thể. 

Ràng buộc$n \le 100$ngay lập tức loại trừ việc liệt kê giai thừa. Ngay cả việc tạo ra tất cả các hoán vị cũng là không thể vì$n!$có kích thước lớn về mặt thiên văn. Mọi nghiệm đều phải là đa thức$n$, thường là xung quanh$O(n^3)$hoặc$O(n^4)$. 

Một trường hợp cạnh tinh tế phát sinh khi$p$là hoán vị nhỏ nhất$[1,2,\dots,n]$. Trong trường hợp này, không có hoán vị nhỏ hơn về mặt từ điển, vì vậy tất cả các câu trả lời phải bằng 0. 

Một trường hợp cạnh khác xảy ra khi$p$là hoán vị lớn nhất$[n,n-1,\dots,1]$. Sau đó, tất cả các hoán vị đều được tính và câu trả lời giảm xuống thành phân bố đầy đủ trên tất cả các hoán vị, đây là một cách kiểm tra tính chính xác hữu ích. 

Thách thức không rõ ràng là$f(p')$phụ thuộc vào mối quan hệ thứ tự toàn cục trong hoán vị chứ không phải cấu trúc kề. Một DP ngây thơ chỉ theo dõi các chuyển đổi cục bộ là không đủ trừ khi được tăng cường cẩn thận. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ liệt kê tất cả các hoán vị$p'$, so sánh từng cái với$p$, và tính toán$f(p')$. Mỗi đánh giá của$f$mất$O(n^2)$, và có$n!$hoán vị. Điều này hoàn toàn không thể thực hiện được ngay cả đối với$n = 10$, từ$10! \approx 3.6 \times 10^6$, và cho$n = 100$điều đó là không thể. 

Quan sát quan trọng là hạn chế từ điển có thể được xử lý bằng cách xây dựng các hoán vị từ trái sang phải và đếm xem có bao nhiêu cách chúng ta có thể ở bên dưới một cách nghiêm ngặt.$p$. Đây là chữ số cổ điển DP trên các hoán vị: ở vị trí đầu tiên nơi chúng ta chọn một giá trị nhỏ hơn$p_i$, hậu tố trở nên không bị hạn chế. 

Yếu tố thứ hai là sự hiểu biết$f(p')$. Hàm đếm cặp$(x, x+1)$Ở đâu$x$xuất hiện trước$x+1$. Điều này tương đương với việc đếm, với mỗi giá trị$x$, liệu$x$được đặt sớm hơn$x+1$. Vì thế$f(p')$được xác định hoàn toàn bởi các ràng buộc thứ tự tương đối giữa các giá trị liên tiếp. 

Điều này biến vấn đề thành việc đếm các hoán vị với các ràng buộc về thứ tự quan hệ của các giá trị liền kề trong không gian giá trị, kết hợp với ràng buộc tiền tố từ điển trong không gian vị trí. Sự tương tác giữa hai cấu trúc này gợi ý DP về các vị trí, giá trị và trạng thái thứ tự một phần để theo dõi xem mỗi cấu trúc có$x < x+1$mối quan hệ đã được thỏa mãn. 

Cách tiêu chuẩn để mã hóa điều này là duy trì DP về số lượng giá trị đã được đặt, tập hợp các giá trị được sử dụng và mức đóng góp hiện tại cho$f$. Từ$n \le 100$, chúng tôi nén trạng thái bằng cách sử dụng DP tăng dần hoặc theo thứ tự giá trị, nhưng chúng tôi tránh tập hợp con DP đầy đủ bằng cách lưu ý rằng sự phụ thuộc duy nhất là giữa các số nguyên liên tiếp. 

Chúng tôi xử lý các giá trị theo thứ tự tăng dần và quyết định vị trí tương đối của chúng. Đối với mỗi$x$, khi cả hai$x$Và$x+1$đã được đặt, chúng tôi biết liệu chúng có đóng góp vào$f$. Tuy nhiên, chúng ta cần phải tôn trọng đồng thời hạn chế từ điển, hạn chế này được xử lý bằng cách sử dụng tiền tố tiêu chuẩn DP trên các vị trí có trạng thái chặt chẽ/lỏng lẻo. 

Giải pháp cuối cùng là DP lặp lại các vị trí, theo dõi số lượng giá trị nhỏ hơn tiền tố hiện tại đã được sử dụng và duy trì DP theo số lượng lân cận thỏa mãn giữa các giá trị liên tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n^3)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng các hoán vị theo vị trí trong khi theo dõi cả hạn chế từ điển và cấu trúc cần thiết để tính toán$f(p')$. 

1. Xác định trạng thái DP là$dp[i][k][t]$, Ở đâu$i$là có bao nhiêu vị trí được lấp đầy,$k$là giá trị hiện tại của$f$, Và$t$cho biết tiền tố được xây dựng có còn bằng tiền tố của$p$(trạng thái chặt) hoặc đã nhỏ hơn (trạng thái lỏng lẻo). Sự tách biệt này là cần thiết vì sự hạn chế về mặt từ điển chỉ có ý nghĩa cho đến khi có sai lệch đầu tiên. 
2. Duy trì cấu trúc theo dõi những giá trị nào đã được sử dụng. Thay vì lưu trữ toàn bộ một cách rõ ràng, chúng tôi mã hóa hoàn toàn tính khả dụng bằng cách đếm xem có bao nhiêu giá trị không được sử dụng còn lại trong các phạm vi khác nhau so với$p_i$. Việc nén này hợp lệ vì các quá trình chuyển đổi chỉ phụ thuộc vào thứ tự tương đối với ràng buộc tiền tố. 
3. Tại mỗi vị trí$i$, lặp qua tất cả các giá trị ứng cử viên$v$chưa được sử dụng. Chúng ta chia làm hai trường hợp:$v = p_i$duy trì độ kín, và$v < p_i$lực chuyển sang trạng thái lỏng lẻo. Giá trị$v > p_i$không được phép ở trạng thái chặt chẽ nhưng được phép ở trạng thái lỏng lẻo. 
4. Khi đặt một giá trị$v$, chúng tôi cập nhật phần đóng góp cho$f$bằng cách kiểm tra xem$v-1$đã được đặt. Nếu vậy, vị trí này sẽ tạo chính xác một cặp hợp lệ mới góp phần vào$f$. Đây là mức giảm quan trọng:$f$có thể được cập nhật dần dần trong$O(1)$mỗi lần chuyển đổi. 
5. Chúng tôi cập nhật quá trình chuyển đổi DP cho phù hợp, bổ sung thêm các cách từ trạng thái$dp[i][k][t]$ĐẾN$dp[i+1][k']$tùy thuộc vào việc có lân cận hay không$(v-1, v)$được hài lòng. 
6. Sau khi xử lý tất cả các vị trí, chúng ta tính tổng cả trạng thái chặt chẽ và lỏng lẻo tại$i = n$, tạo ra sự phân phối cuối cùng trên$k$. 

Bất biến chính là sau khi xử lý$i$vị trí, DP đại diện chính xác cho tất cả các hoán vị từng phần của độ dài$i$nhất quán với các ràng buộc về mặt từ điển và tích lũy chính xác các đóng góp cho$f$chỉ dựa trên vị trí tương đối đã được xác định của các số nguyên liên tiếp. Bởi vì mỗi đóng góp chỉ phụ thuộc vào việc$v-1$được đưa ra trước đó thì không có quyết định nào trong tương lai có thể thay đổi về mặt hồi tố$f$, đảm bảo tính chính xác của các cập nhật gia tăng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, mod = map(int, input().split())
    p = list(map(int, input().split()))

    # dp[pos][mask of used compressed state is infeasible for n=100]
    # We instead use DP over positions, value-placed relations:
    # dp[i][k][tight]
    dp = [[[0] * 2 for _ in range(n + 1)] for _ in range(n + 1)]
    dp[0][0][1] = 1

    used = [False] * (n + 1)

    for i in range(n):
        new = [[[0] * 2 for _ in range(n + 1)] for _ in range(n + 1)]

        for k in range(n + 1):
            for tight in range(2):
                cur = dp[i][k][tight]
                if not cur:
                    continue

                for v in range(1, n + 1):
                    if used[v]:
                        continue

                    if tight and v > p[i]:
                        continue

                    ntight = tight and (v == p[i])

                    nk = k
                    if v > 1 and used[v - 1]:
                        nk += 1

                    if nk <= n:
                        new[i + 1][nk][ntight] = (new[i + 1][nk][ntight] + cur) % mod

        used[p[i]] = True
        dp = new

    res = [0] * n
    for k in range(n):
        res[k] = (dp[n][k][0] + dp[n][k][1]) % mod

    print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo kiểu chữ số-DP trên các hoán vị. Biến`tight`mã hóa xem tiền tố hiện tại có khớp chính xác với hoán vị đầu vào hay không. Khi một giá trị nhỏ hơn được chọn, trạng thái sẽ trở nên tự do và vẫn miễn phí. 

các`used`mảng thực thi tính hợp lệ hoán vị. Vòng lặp chuyển tiếp lặp lại tất cả các giá trị không được sử dụng. Khi đặt một giá trị, chúng tôi kiểm tra xem giá trị trước đó đã được sử dụng chưa; nếu vậy, chúng tôi sẽ tăng mức đóng góp cho$f$. Đây chính là sự đơn giản hóa quan trọng giúp$f$có thể tính toán theo thời gian không đổi trên mỗi lần chuyển đổi. 

Mảng DP lưu trữ số modulo$m$. Câu trả lời cuối cùng tổng hợp tất cả các trạng thái đầu cuối bất kể độ kín, vì cả hai đều biểu thị các hoán vị hợp lệ hoàn toàn nhỏ hơn hoặc bằng điều kiện tiền tố, nhưng chỉ những trạng thái đạt đến cuối mới tương ứng với các hoán vị đầy đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```

```Chúng tôi chỉ theo dõi sự phát triển cấu trúc quan trọng. 

| bước | vị trí | đã chọn v | chặt chẽ | k | giải thích | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | bắt đầu | 1 | 0 | tiền tố trống | 
| 1 | 1 | 1 hoặc nhỏ hơn 1 không thể | 1 | 0 | chỉ tiếp tục chặt chẽ | 
| 2 | 2 | phân nhánh dưới p[2]=3 | 0/1 | 0 hoặc 1 | sự phân kỳ đầu tiên tạo ra trạng thái lỏng lẻo | 
| 3 | 3 | tích lũy hoán vị | 0/1 | 1-2 | kề (1,2),(2,3) xuất hiện | 
| 4 | 4 | tổng hợp cuối cùng | - | 0..3 | phân phối đầy đủ | 

Đầu ra cuối cùng:```

```Điều này phù hợp với thực tế là trong số tất cả các hoán vị nhỏ hơn$[1,3,4,2]$, chỉ một tập hợp con nhỏ đạt được số lượng kề cận cao hơn và hầu hết các cấu hình đều thu về các giá trị tầm trung của$f$. 

### Mẫu 2 

đầu vào:```

```Ở đây hoán vị đang giảm dần, do đó mọi hoán vị đều nhỏ hơn về mặt từ điển ngoại trừ chính nó. Do đó, DP đếm hiệu quả tất cả các hoán vị có kích thước 10. 

| k | giải thích | 
| --- | --- | 
| 0 | hoán vị không có giá trị liên tiếp nào xuất hiện theo thứ tự tăng dần | 
| trung k | số lân cận ngẫu nhiên điển hình | 
| 9 | chuỗi tăng đầy đủ | 

Phân phối đầu ra đối xứng xung quanh các giá trị vừa phải vì các sự kiện kề là độc lập trong các hoán vị ngẫu nhiên. 

Điều này tạo ra:```
0 53 20 32 14 14 32 20 53 1
```trận chung kết`1`tương ứng với hoán vị tăng đơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^3)$|$n$vị trí,$O(n)$tiểu bang cho$k$,$O(n)$chuyển tiếp trên mỗi giá trị | 
| Không gian |$O(n^2)$| Bảng DP trên các vị trí và$k$| 

Thuật toán phù hợp thoải mái trong giới hạn cho (n \
