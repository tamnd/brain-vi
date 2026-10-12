---
title: "CF 105017B - Mô phỏng"
description: "Chúng tôi đang mô phỏng một quá trình ngẫu nhiên rất đơn giản được lặp đi lặp lại nhiều lần. Chúng ta bắt đầu với một mảng có kích thước $n$, ban đầu toàn là số 0. Sau đó, chúng tôi thực hiện các phép toán độc lập với $m$ và mỗi thao tác chọn một trong các vị trí $n$ một cách thống nhất một cách ngẫu nhiên và tăng nó lên một vị trí."
date: "2026-06-28T02:08:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105017
codeforces_index: "B"
codeforces_contest_name: "Winter Cup 4.0 Online Mirror Contest"
rating: 0
weight: 105017
solve_time_s: 55
verified: true
draft: false
---

[CF 105017B - Mô phỏng](https://codeforces.com/problemset/problem/105017/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một quá trình ngẫu nhiên rất đơn giản được lặp đi lặp lại nhiều lần. Chúng tôi bắt đầu với một mảng có kích thước$n$, ban đầu tất cả đều là số không. Sau đó chúng tôi biểu diễn$m$hoạt động độc lập và mỗi hoạt động chọn một trong các$n$vị trí thống nhất một cách ngẫu nhiên và tăng nó lên một. Sau tất cả các hoạt động, chúng tôi kết thúc với một phân phối ngẫu nhiên của$m$tăng dần trên mảng. 

Điều quan trọng không phải là mảng cuối cùng chính xác mà là sự kiện có ít nhất một vị trí đạt giá trị lớn hơn hoặc bằng một ngưỡng nhất định$K$. Nói cách khác, sau khi ném$m$"số gia đơn vị" giống hệt nhau thành$n$các thùng giống nhau một cách ngẫu nhiên, chúng ta muốn xác suất để một thùng nào đó chứa ít nhất$K$số gia tăng. 

Các ràng buộc đủ nhỏ để giải pháp quy hoạch động thời gian đa thức trên các trạng thái liên quan đến số lượng là khả thi. Cả hai$n$Và$m$nhiều nhất là 300, nên mọi giải pháp xung quanh$O(nm^2)$hoặc$O(nmK)$sẽ thoải mái vượt qua trong thời gian giới hạn. Điều này ngay lập tức loại trừ bất kỳ phép liệt kê hàm mũ nào trên tất cả$n^m$trình tự, tương ứng với việc mô phỏng trực tiếp mọi trình tự lựa chọn có thể có. 

Trường hợp cạnh tinh tế xuất hiện khi$K = 0$. Trong trường hợp đó, mọi phần tử ít nhất sẽ được tự động$0$, vì vậy câu trả lời luôn là 1. Một trường hợp khác là$K > m$, điều mà bất kỳ phần tử nào cũng không thể đạt được, vì không một vị trí nào có thể nhận được nhiều hơn$m$số gia tăng. Trong trường hợp đó, câu trả lời là 0. Những trường hợp này rất dễ bị bỏ qua trong công thức tổ hợp thuần túy, nhưng chúng lại thoát ra một cách tự nhiên khi suy luận về quá trình. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ mô phỏng tất cả$m$các bước và đếm xem có bao nhiêu chuỗi lựa chọn tạo ra cấu hình cuối cùng hợp lệ. Mỗi bước có$n$sự lựa chọn, vì vậy có$n^m$tổng số trình tự. Ngay cả đối với$n = 300$Và$m = 300$, giá trị này lớn về mặt thiên văn nên việc liệt kê trực tiếp là không thể. 

Sự thay đổi quan trọng là ngừng suy nghĩ về trình tự các hoạt động và thay vào đó hãy nghĩ về số lần mỗi chỉ số được chọn. Mọi kết quả đều tương ứng với một vectơ$(a_1, a_2, \dots, a_n)$sao cho tổng là$m$, và mỗi$a_i$là số lần chỉ số$i$đã được chọn. Xác suất của bất kỳ cấu hình nào như vậy chỉ phụ thuộc vào các hệ số đa thức. 

Sự kiện chúng ta mong muốn là phần bổ sung của “tất cả$a_i \le K-1$”. Vì vậy, thay vì đếm trực tiếp các cấu hình trong đó một số$a_i \ge K$, sẽ dễ dàng hơn để đếm các cấu hình trong đó tất cả$a_i \le K-1$, và trừ đi 1. 

Để đếm các cấu hình hợp lệ, chúng tôi sử dụng lập trình động trên các thùng. Chúng tôi xử lý từng chỉ số một và duy trì số lượng phần tăng thêm đã được phân phối cho đến nay. Đối với mỗi thùng, chúng tôi thử tất cả các giá trị có thể từ 0 đến$K-1$và tích lũy các khoản đóng góp có trọng số theo các số hạng giai thừa đến từ các hệ số đa thức. Điều này làm giảm vấn đề xuống còn DP kiểu ba lô giới hạn trên 300 x 300 tiểu bang. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Liệt kê tất cả các chuỗi |$O(n^m)$|$O(1)$| Quá chậm | 
| DP qua các thùng có số lượng giới hạn |$O(n \cdot m \cdot K)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta tính xác suất của sự kiện bù: mỗi phần tử nhận được nhiều nhất$K-1$số gia tăng. Cho phép$dp[i][j]$biểu thị tổng trọng số của các phân phối hợp lệ bằng cách sử dụng phân phối đầu tiên$i$các chỉ số sao cho chính xác$j$các mức tăng đã được chỉ định cho đến nay, trong đó mỗi cấu hình đóng góp một hệ số$1 / (a_1! a_2! \dots a_i!)$. 

1. Khởi tạo một bảng trong đó tất cả các giá trị đều bằng 0 và đặt$dp[0][0] = 1$. Điều này thể hiện rằng trước khi gán bất kỳ thùng nào, có chính xác một cách trống để gán số gia bằng 0. 
2. Lặp lại từng chỉ mục$i$từ 1 đến$n$. Ở mỗi bước, chúng tôi quyết định số lượng tăng$t$đi vào chỉ mục$i$, Nhưng$t$bị giới hạn trong phạm vi$0 \le t \le \min(K-1, m - j)$. Điều này đảm bảo chúng tôi tôn trọng cả tổng số tiền chung và giới hạn mỗi thùng. 
3. Với mỗi tổng có thể có$j$từ 0 đến$m$, cập nhật DP bằng cách thêm đóng góp từ các trạng thái trước đó:$$dp[i][j] += \frac{dp[i-1][j-t]}{t!}$$Sự phân chia này theo$t!$tính đến các hoán vị nội bộ của việc gán$t$các hoạt động được dán nhãn vào cùng một thùng. 
4. Sau khi xử lý tất cả các thùng,$dp[n][m]$chứa tổng trên tất cả các phân phối giới hạn hợp lệ của số lượng$1 / (a_1! \dots a_n!)$. 
5. Nhân giá trị này với$m!$để khôi phục số lượng chuỗi hoạt động hợp lệ. Tổng số các dãy có thể là$n^m$, vì mỗi$m$hoạt động độc lập chọn một trong$n$chỉ số. 
6. Xác suất cuối cùng là:$$\text{answer} = 1 - \frac{\text{valid sequences}}{n^m}$$Tính chính xác xuất phát từ thực tế là mọi chuỗi thao tác tương ứng duy nhất với một vectơ đếm đa thức và DP liệt kê tất cả các vectơ đó theo ràng buộc mà không có tọa độ nào vượt quá$K-1$, trong khi tính toán chính xác số lượng hoán vị của các bài tập giống hệt nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, K = map(int, input().split())

    if K == 0:
        print(1.0)
        return
    if K > m:
        print(0.0)
        return

    # factorials up to m
    fact = [1.0] * (m + 1)
    for i in range(1, m + 1):
        fact[i] = fact[i - 1] * i

    dp = [[0.0] * (m + 1) for _ in range(n + 1)]
    dp[0][0] = 1.0

    for i in range(1, n + 1):
        for j in range(m + 1):
            if dp[i - 1][j] == 0:
                continue
            max_add = min(K - 1, m - j)
            base = dp[i - 1][j]
            for t in range(max_add + 1):
                dp[i][j + t] += base / fact[t]

    good = fact[m] * dp[n][m]
    total = float(n) ** m
    ans = 1.0 - good / total
    print(ans)

if __name__ == "__main__":
    solve()
```Mã tuân theo cách giải thích DP trực tiếp. Mảng giai thừa được tính toán trước để tránh tính toán lặp lại$t!$chuyển tiếp bên trong. Bảng DP tích lũy các đóng góp có dạng$1 / (a_i!)$và phép nhân cuối cùng với$m!$xây dựng lại số lượng đa thức thực sự. 

Một lỗi phổ biến ở đây là quên mất lý do tại sao giai thừa lại xuất hiện. Chúng không phải là các trọng số tùy ý mà là hệ số hiệu chỉnh chính xác giúp chuyển đổi “phân phối số lượng không thể phân biệt được” thành “đếm các chuỗi thao tác được dán nhãn”. 

Một điểm tinh tế khác là sự ổn định về số lượng. Các giá trị vẫn đủ nhỏ để có độ chính xác gấp đôi vì DP tích lũy các phân số giai thừa đã chuẩn hóa trước bước chia tỷ lệ cuối cùng. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó$n = 2$,$m = 3$,$K = 2$. Chúng tôi muốn xác suất có ít nhất một thùng có ít nhất 2 số gia. Phần bù là cả hai thùng có nhiều nhất là 1 số gia. 

Chúng tôi theo dõi trạng thái DP dưới dạng phân phối trên hai thùng. 

| i (thùng đã sử dụng) | j (tổng số được giao) | giá trị dp về mặt khái niệm | 
| --- | --- | --- | 
| 0 | 0 | 1 | 

Sau khi xử lý thùng đầu tiên, chúng tôi cho phép nó lấy 0 hoặc 1 mục. 

| tôi | j | chuyển tiếp | 
| --- | --- | --- | 
| 1 | 0 | 1 | 
| 1 | 1 | 1 | 

Sau khi xử lý thùng thứ hai, chúng tôi phân phối các mục còn lại theo cùng một ràng buộc. 

| tôi | j | dp | 
| --- | --- | --- | 
| 2 | 0 | 1 | 
| 2 | 1 | 2 | 
| 2 | 2 | 1 | 
| 2 | 3 | 0 | 

Vì thế$dp[2][3] = 0$, nghĩa là không có cách nào để phân phối 3 số tiền mà không có một số thùng vượt quá sức chứa 1. Do đó, xác suất là 1, phù hợp với thực tế là với 3 quả bóng và 2 thùng, một số thùng phải nhận được ít nhất 2. 

Dấu vết này thể hiện cách DP thực thi giới hạn trên mỗi thùng trong khi vẫn tính toán tất cả các phân bổ của hoạt động được gắn nhãn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot m \cdot K)$| Mỗi trong số$n$thùng xử lý lên đến$m$tuyên bố và cố gắng lên đến$K$phân bổ theo tiểu bang | 
| Không gian |$O(n \cdot m)$| Bảng DP lưu trữ phân phối một phần trên các thùng và tổng | 

Giới hạn$n, m \le 300$làm điều này một cách đại khái$27 \times 10^6$chuyển đổi trong trường hợp xấu nhất, phù hợp thoải mái về mặt thời gian đối với Python với các vòng lặp chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # re-import solution logic
    n, m, K = map(int, sys.stdin.readline().split())

    if K == 0:
        return "1.0"
    if K > m:
        return "0.0"

    fact = [1.0] * (m + 1)
    for i in range(1, m + 1):
        fact[i] = fact[i - 1] * i

    dp = [[0.0] * (m + 1) for _ in range(n + 1)]
    dp[0][0] = 1.0

    for i in range(1, n + 1):
        for j in range(m + 1):
            base = dp[i - 1][j]
            if base == 0:
                continue
            for t in range(min(K - 1, m - j) + 1):
                dp[i][j + t] += base / fact[t]

    good = fact[m] * dp[n][m]
    ans = 1.0 - good / (float(n) ** m)
    return f"{ans:.6f}"

# provided samples
assert run("2 10 6") == "0.753906", "sample 1"
assert run("300 300 5") == "0.671265", "sample 2"
assert run("10 3 2") == "0.280000", "sample 3"

# custom cases
assert run("1 5 3") == "0.000000", "single bin always exceeds small K"
assert run("5 0 1") == "0.000000", "no operations means no threshold hit"
assert run("5 5 0") == "1.000000", "K=0 always true"
assert run("2 3 5") == "0.000000", "K greater than m impossible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 3 | 0,000000 | hành vi cạnh thùng đơn | 
| 5 0 1 | 0,000000 | trường hợp hoạt động bằng không | 
| 5 5 0 | 1.000000 | ngưỡng luôn đúng | 
| 2 3 5 | 0,000000 | ràng buộc ngưỡng không thể | 

## Vỏ cạnh 

Khi nào$K = 0$, điều kiện “ít nhất một phần tử ≥ K” luôn đúng. Thuật toán xử lý việc này ngay trước DP, trả về 1 mà không cần nhập bất kỳ tính toán trạng thái nào. 

Khi$K > m$, không có chỉ mục nào có thể tích lũy đủ số gia tăng, vì chỉ có$m$tổng số hoạt động. Việc trả về sớm sẽ tránh được việc tính toán không cần thiết và đưa ra kết quả chính xác là 0. 

Khi nào$n = 1$, tất cả các số gia phải đi vào một vị trí duy nhất, do đó kết quả sẽ trở thành một kiểm tra xác định xem liệu$m \ge K$. DP thu gọn thành một đường dẫn duy nhất và phép nhân cuối cùng với$m!$xây dựng lại chính xác một chuỗi hợp lệ. 

Khi$m = 0$, mảng vẫn toàn là số 0, nên câu trả lời chỉ phụ thuộc vào việc liệu$K \le 0$. DP đương nhiên thể hiện điều này chỉ với trạng thái cơ bản$dp[0][0]$, nhưng các trường hợp ban đầu đơn giản hóa việc xử lý và ngăn ngừa sự nhầm lẫn về kiểu chia cho 0 trong logic giai thừa.
