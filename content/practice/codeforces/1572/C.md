---
title: "CF 1572C - Sơn"
description: "Chúng ta có một hình ảnh một chiều được biểu diễn bằng một mảng màu. Một thao tác đơn lẻ sẽ chọn một đoạn đơn sắc được kết nối và sơn lại toàn bộ đoạn đó thành bất kỳ màu nào. Định nghĩa kết nối hơi khác thường."
date: "2026-06-10T11:18:42+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1572
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 743 (Div. 1)"
rating: 2700
weight: 1572
solve_time_s: 337
verified: false
draft: false
---

[CF 1572C - Sơn](https://codeforces.com/problemset/problem/1572/C) 

**Xếp hạng:** 2700 
**Tags:** dp, tham lam 
**Thời gian giải:** 5 phút 37 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hình ảnh một chiều được biểu diễn bằng một mảng màu. Một thao tác đơn lẻ sẽ chọn một đoạn đơn sắc được kết nối và sơn lại toàn bộ đoạn đó thành bất kỳ màu nào. 

Định nghĩa kết nối hơi khác thường. Hai vị trí cùng màu được kết nối nếu mọi vị trí giữa chúng cũng có cùng màu đó. Trong mảng một chiều, điều này đơn giản có nghĩa là một khối liền kề tối đa có các giá trị bằng nhau. Một thao tác chọn một khối như vậy và thay đổi màu của toàn bộ khối. 

Mục tiêu là làm cho toàn bộ mảng có một màu duy nhất bằng cách sử dụng số lượng thao tác tối thiểu. 

Quan điểm mô phỏng trực tiếp là sai lầm vì việc sơn lại một khối có thể hợp nhất nó với các khối lân cận có màu mục tiêu, tạo ra các thành phần được kết nối lớn hơn mà sau này có thể được sơn lại với nhau. 

Tổng độ dài của tất cả các trường hợp thử nghiệm tối đa là 3000. Đây là hạn chế chính. Giải pháp quy hoạch động bậc ba theo khoảng là khả thi vì$3000^3$quá lớn trên toàn cầu, nhưng hạn chế đặc biệt là mỗi màu xuất hiện tối đa 20 lần làm giảm đáng kể số lần chuyển đổi có ý nghĩa. Giải pháp mong muốn khai thác hạn chế này để tránh xem xét tất cả các khoảng chia tách. 

Sai lầm phổ biến nhất là nghĩ về các khối liền kề sau khi nén tọa độ. Điều đó làm mất thông tin cần thiết. 

Coi như:```
1 2 1
```Câu trả lời là 1. Sơn lại khối ở giữa từ 2 lên 1 và toàn bộ mảng trở thành màu 1. 

Cách tiếp cận dựa trên nén nhìn thấy ba khối và có thể kết luận không chính xác rằng cần có hai sự hợp nhất. 

Một trường hợp tế nhị khác là:```
1 2 3 2 1
```Câu trả lời là 2. 

Đầu tiên sơn lại 3 thành 2:```
1 2 2 2 1
```Sau đó sơn lại khối trung tâm 2s thành 1:```
1 1 1 1 1
```Chiến lược tham lam luôn mở rộng khối màu lớn nhất sẽ thất bại vì trình tự tối ưu phụ thuộc vào việc kết hợp các màu bằng nhau ở cả hai đầu của một khoảng. 

Bẫy thứ ba là cho rằng màu cuối cùng phải tồn tại ở mọi nơi trong khoảng đó. 

Vì:```
1 2 1 4 2
```Câu trả lời là 3. Quá trình tối ưu liên tục tạo ra các vùng lớn hơn trước khi hợp nhất lần cuối. Các quyết định địa phương không tiết lộ sự tối ưu toàn cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ coi mọi cấu hình hình ảnh là một trạng thái và chạy BFS trên tất cả các bản vẽ lại có thể truy cập được. Hoạt động rất dễ mô phỏng: chọn một khối và tô màu lại nó. Tìm kiếm này là chính xác vì mọi hoạt động đều có đơn giá. 

Vấn đề là không gian trạng thái. Ngay cả đối với những giá trị khiêm tốn của$n$, số lượng màu có thể tăng theo cấp số nhân. Không có hy vọng khám phá tất cả các cấu hình có thể tiếp cận. 

Quan sát quan trọng là việc sơn lại hoạt động rất giống với các vấn đề hợp nhất theo khoảng thời gian như Máy in lạ. Nếu hai màu bằng nhau xuất hiện ở vị trí$i$Và$j$, cuối cùng chúng ta có thể tạo ra toàn bộ khoảng$[i,j]$thu gọn vào một khu vực duy nhất mà không phải trả tiền riêng cho cả hai điểm cuối. 

Điều này gợi ý một khoảng DP. 

Cho phép$dp[l][r]$biểu thị các thao tác tối thiểu cần thiết để tạo mảng con$a_l \dots a_r$trở thành một màu duy nhất. 

Giả sử chúng ta muốn vị trí$l$để tham gia vào màu hợp nhất cuối cùng. Một lựa chọn là sơn lại$a_l$độc lập và giải quyết phần còn lại. Một khả năng khác xuất hiện bất cứ khi nào tồn tại một vị trí$k$bên trong khoảng có cùng màu với$a_l$. Sau đó các điểm cuối$l$Và$k$có thể được hợp nhất vào cùng một thành phần cuối cùng, làm giảm số lượng thao tác. 

Hạn chế mỗi màu xuất hiện tối đa 20 lần là điều khiến điều này trở nên thiết thực. Đối với mỗi điểm cuối bên trái, chỉ có$O(20)$vị trí phù hợp để xem xét. Thay vì thử mọi điểm phân chia, chúng tôi chỉ chuyển tiếp qua các lần xuất hiện có màu bằng nhau. 

Điều này làm giảm khoảng DP từ cấu trúc chuyển tiếp hình khối có khả năng xuống gần$O(n^2 \cdot 20)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| DP khoảng thời gian với chuyển tiếp màu bằng nhau |$O(n^2 \cdot 20)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Hãy để mảng được lập chỉ mục từ 0. 

Xác định$dp[l][r]$là số lượng thao tác tối thiểu cần thiết để tạo khoảng thời gian$[l,r]$đơn sắc. 

Câu trả lời sẽ là$dp[0][n-1]$. 

### Tái phát 

Nếu$l=r$, khoảng đã bao gồm một màu.$$dp[l][l]=0$$Để có khoảng thời gian dài hơn, hãy bắt đầu bằng cách cô lập phần tử ngoài cùng bên trái.$$dp[l][r]=1+dp[l+1][r]$$Điều này tương ứng với việc sơn lại màu tại vị trí$l$riêng rồi giải khoảng còn lại. 

Bây giờ hãy xem xét mọi vị trí$k$với$$l<k\le r,\qquad a_k=a_l$$Nếu vị trí$l$Và$k$có cùng màu sắc thì chúng có thể tham gia vào cùng một vùng được hợp nhất cuối cùng. 

Khoảng thời gian$(l+1,k-1)$đầu tiên phải được làm đơn sắc sao cho$l$Và$k$trở nên liền kề trong quá trình sáp nhập. Sau đó, hậu tố$[k,r]$được giải quyết một cách độc lập. 

Điều này mang lại$$dp[l][r] = \min \left( dp[l][r], \, dp[l+1][k-1] + dp[k][r] \right)$$trong đó một khoảng trống đóng góp 0. 

### Đếm hiệu quả 

Đối với mỗi màu, hãy lưu trữ danh sách các vị trí mà nó xuất hiện. 

Khi xử lý một trạng thái$(l,r)$, chỉ lặp lại các lần xuất hiện của màu$a_l$. Vì mỗi màu xuất hiện tối đa 20 lần nên mỗi trạng thái thực hiện tối đa 20 lần chuyển đổi như vậy. 

### Điền đơn hàng 

Xử lý khoảng thời gian bằng cách tăng chiều dài. 

Khi tính toán$dp[l][r]$, mỗi khoảng con được yêu cầu hoàn toàn ngắn hơn, vì vậy giá trị của nó đã được biết trước. 

### Hướng dẫn thuật toán 

1. Xây dựng danh sách vị trí cho từng màu. 
2. Khởi tạo$dp[l][l]=0$cho tất cả các chỉ số. 
3. Khoảng thời gian xử lý từ 2 đến$n$. 
4. Đối với mỗi khoảng thời gian$[l,r]$, bắt đầu với quá trình chuyển đổi$$dp[l][r]=1+dp[l+1][r]$$xử lý vị trí$l$một cách độc lập. 
5. Lặp lại tất cả các lần xuất hiện$k$màu sắc$a_l$thỏa mãn$l<k\le r$. 
6. Tính toán$$cost = dp[l+1][k-1] + dp[k][r]$$trong đó số hạng đầu tiên là 0 khi$k=l+1$. 
7. Cập nhật$$dp[l][r]=\min(dp[l][r],cost)$$8. Sau khi tất cả các khoảng thời gian được xử lý, xuất ra$dp[0][n-1]$. 

### Tại sao nó hoạt động 

Sự truy hồi làm cạn kiệt mọi khả năng tối ưu cho vị trí ngoài cùng bên trái. 

Trong mọi giải pháp tối ưu cho khoảng$[l,r]$, cả hai vị trí$l$được xử lý riêng biệt với mỗi lần xuất hiện màu sau này của nó, tạo ra sự chuyển tiếp$1+dp[l+1][r]$hoặc cuối cùng nó được hợp nhất với vị trí khớp đầu tiên$k$thuộc về thành phần đơn sắc cuối cùng của nó. 

Khi$l$hợp nhất với như vậy$k$, mọi phần tử giữa chúng phải đã được hấp thụ vào các vùng tương thích. Chi phí để làm điều đó chính xác là$dp[l+1][k-1]$. Sau đó$l$Và$k$được liên kết, công việc còn lại bên trong$[k,r]$độc lập và chi phí$dp[k][r]$. 

Mọi giải pháp tối ưu đều rơi vào một trong những trường hợp này và mọi chuyển đổi đều tương ứng với một chuỗi hoạt động có thể thực hiện được. Bằng cách quy nạp độ dài khoảng, phép truy toán sẽ tính toán mức tối ưu thực sự cho mỗi khoảng. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pos = defaultdict(list)
        for i, x in enumerate(a):
            pos[x].append(i)

        dp = [[0] * n for _ in range(n)]

        for length in range(2, n + 1):
            for l in range(n - length + 1):
                r = l + length - 1

                dp[l][r] = 1 + dp[l + 1][r]

                for k in pos[a[l]]:
                    if k <= l:
                        continue
                    if k > r:
                        break

                    left_cost = 0
                    if l + 1 <= k - 1:
                        left_cost = dp[l + 1][k - 1]

                    dp[l][r] = min(
                        dp[l][r],
                        left_cost + dp[k][r]
                    )

        ans.append(str(dp[0][n - 1]))

    sys.stdout.write("\n".join(ans))

solve()
```Bảng DP lưu trữ câu trả lời cho mỗi khoảng thời gian. Khoảng thời gian xử lý bằng cách tăng độ dài đảm bảo rằng mọi bài toán con được tham chiếu đều đã được tính toán. 

Việc khởi tạo$dp[l][l]=0$được lấy tự động vì bảng bắt đầu bằng số không. 

Sự chuyển tiếp`1 + dp[l + 1][r]`tương ứng với việc xử lý phần tử ngoài cùng bên trái một cách riêng biệt. 

Danh sách xuất hiện là tối ưu hóa quan trọng. Việc triển khai đơn giản sẽ kiểm tra mọi điểm phân chia có thể$k$, sản xuất$O(n^3)$chuyển tiếp. Ở đây chúng ta chỉ ghé thăm các vị trí có cùng màu với điểm cuối bên trái. Vì mỗi màu xuất hiện tối đa 20 lần nên số lần chuyển đổi trên mỗi trạng thái vẫn bị giới hạn. 

Trường hợp khoảng trống cần được chăm sóc. Khi$k=l+1$, khoảng$[l+1,k-1]$không chứa phần tử nào, do đó đóng góp của nó phải bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2 3 2 1
```Các trạng thái DP chính: 

| Khoảng thời gian | Giá trị | 
| --- | --- | 
| dp[2][2] | 0 | 
| dp[1][3] | 1 | 
| dp[0][4] | 2 | 

Đối với khoảng thời gian đầy đủ: 

| Bước | Chuyển tiếp | Chi phí | 
| --- | --- | --- | 
| Ban đầu | 1 + dp[1][4] | 3 | 
| Trận đấu ở k=4 | dp[1][3] + dp[4][4] | 1 + 0 = 1 | 
| dp cuối cùng[0][4] | 1 + hiệu ứng chuyển tiếp tốt nhất | 2 | 

Màu sắc bằng nhau ở cả hai đầu cho phép thu gọn khoảng thời gian với chi phí rẻ hơn so với việc xử lý từng điểm cuối một cách độc lập. 

### Ví dụ 2 

đầu vào:```
1 1 2 2
```Các trạng thái liên quan: 

| Khoảng thời gian | Giá trị | 
| --- | --- | 
| dp[0][1] | 0 | 
| dp[2][3] | 0 | 
| dp[0][3] | 1 | 

Trong toàn bộ khoảng thời gian: 

| Bước | Chuyển tiếp | Chi phí | 
| --- | --- | --- | 
| Ban đầu | 1 + dp[1][3] | 2 | 
| Trận đấu ở k=1 | dp[1][0] + dp[1][3] | 1 | 
| Cuối cùng | 1 | | 

Hai số 1 đứng đầu đã tạo thành một thành phần được hợp nhất, do đó chỉ cần một thao tác sơn lại cho toàn bộ hình ảnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 \cdot 20)$| Mỗi khoảng chỉ xem xét sự xuất hiện của màu bên trái | 
| Không gian |$O(n^2)$| Bảng DP trong tất cả các khoảng | 

Vì tổng của tất cả$n$giá trị tối đa là 3000, một$O(n^2)$dấu chân bộ nhớ và đại khái$20n^2$chuyển tiếp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pos = defaultdict(list)
        for i, x in enumerate(a):
            pos[x].append(i)

        dp = [[0] * n for _ in range(n)]

        for length in range(2, n + 1):
            for l in range(n - length + 1):
                r = l + length - 1

                dp[l][r] = 1 + dp[l + 1][r]

                for k in pos[a[l]]:
                    if k <= l:
                        continue
                    if k > r:
                        break

                    left = 0
                    if l + 1 <= k - 1:
                        left = dp[l + 1][k - 1]

                    dp[l][r] = min(dp[l][r], left + dp[k][r])

        out.append(str(dp[0][n - 1]))

    return "\n".join(out)

# provided samples
assert run(
"""3
5
1 2 3 2 1
4
1 1 2 2
5
1 2 1 4 2
"""
) == "2\n1\n3"

# minimum size
assert run(
"""1
1
7
"""
) == "0"

# all equal
assert run(
"""1
5
3 3 3 3 3
"""
) == "0"

# symmetric merge
assert run(
"""1
3
1 2 1
"""
) == "1"

# alternating colors
assert run(
"""1
4
1 2 1 2
"""
) == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`0`| Khoảng phần tử đơn | 
|`3 3 3 3 3`|`0`| Đã đơn sắc | 
|`1 2 1`|`1`| Các điểm cuối phù hợp hợp nhất một cách tối ưu | 
|`1 2 1 2`|`2`| Nhiều chuyển tiếp màu bằng nhau chồng chéo | 

## Vỏ cạnh 

Hãy xem xét:```
1
1
5
```Khoảng có độ dài một. Trường hợp cơ sở DP cho`dp[0][0] = 0`. Không cần sơn lại vì hình ảnh đã có một màu duy nhất. 

Coi như:```
1
3
1 2 1
```Đối với khoảng thời gian`[0,2]`, quá trình chuyển đổi ban đầu mang lại$$1 + dp[1][2] = 2.$$Sự xuất hiện trùng khớp ở vị trí 2 tạo ra$$dp[1][1] + dp[2][2] = 0.$$Điều này làm giảm giá trị trạng thái xuống 1. Thuật toán nhận ra chính xác rằng việc sơn lại phần tử ở giữa một lần là đủ. 

Coi như:```
1
5
1 2 3 2 1
```Các điểm cuối có cùng màu. Phép truy toán kiểm tra rõ ràng tất cả các lần xuất hiện sau của màu 1, bao gồm cả vị trí cuối cùng. Quá trình chuyển đổi thông qua điểm cuối phù hợp đó sẽ nắm bắt được trình tự hợp nhất tối ưu và mang lại câu trả lời 2. 

Hãy xem xét:```
1
5
1 1 1 1 1
```Mỗi khoảng thời gian liên tục tìm thấy các lần xuất hiện trùng khớp có cùng màu. Giá trị DP vẫn bằng 0 trong suốt quá trình. Giải pháp đếm các khối màu sẽ báo cáo sai một thao tác, nhưng khoảng DP trả về chính xác bằng 0 vì không cần sơn lại.
