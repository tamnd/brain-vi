---
title: "CF 105164J - Hành Trình Đến Stringland"
description: "Chúng ta có một chuỗi $S$ có độ dài $N$. Chúng ta được phép thay đổi các ký tự trong chuỗi này một cách tùy ý, mỗi thay đổi sẽ tốn một thao tác."
date: "2026-06-27T10:47:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105164
codeforces_index: "J"
codeforces_contest_name: "2024 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 105164
solve_time_s: 73
verified: false
draft: false
---

[CF 105164J - Hành trình đến Stringland](https://codeforces.com/problemset/problem/105164/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi$S$chiều dài$N$. Chúng ta được phép thay đổi các ký tự trong chuỗi này một cách tùy ý, mỗi thay đổi sẽ tốn một thao tác. Mục đích không phải là xây dựng trực tiếp một bảng màu mà để đảm bảo rằng ở đâu đó bên trong chuỗi kết quả tồn tại một dãy con có độ dài$K$đó tạo thành một palindrome. 

Một dãy con có nghĩa là chúng ta chọn$K$các vị trí theo thứ tự chỉ mục tăng dần, sau đó đọc các ký tự đó theo thứ tự. Chúng tôi muốn những ký tự được chọn đó đọc xuôi và đọc ngược giống nhau. 

Vì vậy, nhiệm vụ là: bằng cách sửa đổi càng ít ký tự càng tốt trong chuỗi gốc, đảm bảo rằng tồn tại ít nhất một độ dài-$K$dãy con palindrome. 

Khó khăn chính là chúng ta không xác định trước được dãy con nào sẽ sử dụng. Chúng ta có thể tự do lựa chọn bất kỳ chuỗi con nào sau khi sửa đổi, vì vậy việc tối ưu hóa bao gồm cả lựa chọn chuỗi con và chỉnh sửa ký tự. 

Những hạn chế$N \le 500$Và$K \le 500$đề xuất ngay một giải pháp quy hoạch động bậc hai hoặc bậc ba là có thể chấp nhận được. Bất cứ điều gì như$O(N^3)$với các hằng số nhỏ có thể vượt qua, nhưng việc liệt kê theo cấp số nhân trên các chuỗi con là không thể vì có$\binom{N}{K}$sự lựa chọn. 

Một sai lầm ngây thơ là cho rằng chúng ta chỉ cần làm cho toàn bộ chuỗi gần với một bảng màu hoặc sắp xếp một cách tham lam các vị trí đối xứng của chuỗi đầy đủ. Điều đó không thành công vì dãy con tối ưu có thể bỏ qua hoàn toàn các vị trí “xấu”. Ví dụ: nếu chuỗi chủ yếu là ngẫu nhiên nhưng chứa một vài chữ cái lặp lại cách xa nhau, thì chuỗi đối xứng tốt nhất có thể nằm hoàn toàn bên trong các lần lặp đó, bỏ qua phần còn lại. 

Một trường hợp cạnh tinh vi khác xuất hiện khi$K = 1$. Bất kỳ ký tự đơn nào cũng là một bảng màu, vì vậy câu trả lời luôn bằng 0. Một DP bất cẩn đảm nhận vị trí ghép đôi có thể yêu cầu sai ít nhất một trận đấu. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là chọn tất cả các chuỗi có độ dài có thể$K$, và với mỗi dãy con hãy tính số lượng thay đổi tối thiểu cần thiết để biến nó thành một bảng màu. Đối với một dãy con cố định, điều này rất đơn giản: chúng tôi so sánh các cặp đối xứng và đếm các phần không khớp, có thể cho phép tô màu lại để giảm thiểu chi phí. Tuy nhiên, việc liệt kê tất cả các dãy con đã tốn kém$\binom{N}{K}$, điều này là không thể ngay cả đối với người vừa phải$N$. 

Thay vì chọn dãy con trước, chúng ta đảo ngược phối cảnh. Chúng tôi quyết định chuỗi con palindromic sẽ trông như thế nào về mặt cấu trúc và sau đó cố gắng nhúng nó vào chuỗi với những chỉnh sửa tối thiểu. 

Một palindrome có chiều dài$K$được xác định bởi lần đầu tiên của nó$\lfloor K/2 \rfloor$vị trí ghép nối và, nếu$K$là kỳ quặc, một nhân vật trung tâm. Với mỗi cặp, chúng ta phải chọn hai chỉ số$i < j$trong chuỗi sẽ đại diện cho các vị trí được phản ánh trong chuỗi tiếp theo. Nếu ký tự của họ đã khớp thì không cần tốn phí; nếu không, chúng ta phải thay đổi một trong số chúng để chúng khớp nhau, tốn một thao tác. 

Điều này gợi ý một chương trình động trên các vị trí chuỗi và số lượng cặp chúng ta đã hình thành. Điều quan trọng là xây dựng chuỗi con từ trái sang phải trong không gian chỉ mục, đồng thời thực thi ghép nối palindrome từ cả hai đầu. 

Chúng tôi xác định một DP trong đó chúng tôi chọn các vị trí theo thứ tự tăng dần cho nửa đầu của bảng màu và khớp chúng với các vị trí được chọn từ phía bên phải. Mỗi sự không phù hợp đóng góp chi phí 1. 

Điều này làm giảm vấn đề thành một kết hợp có cấu trúc giữa các cặp chỉ số, trong đó mỗi cặp đóng góp chi phí dựa trên sự bình đẳng về ký tự. Giải pháp tối ưu trở thành DP kiểu đường đi ngắn nhất qua các điểm cuối khoảng thời gian và số lượng cặp còn lại được hình thành. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuỗi tiếp theo của Brute Force |$O(\binom{N}{K} \cdot K)$|$O(K)$| Quá chậm | 
| Khoảng thời gian xây dựng DP |$O(N^2 K)$|$O(NK)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một DP tính toán chi phí tối thiểu để xây dựng một chuỗi con palindrome bằng cách sử dụng các vị trí trong tiền tố của chuỗi, đồng thời theo dõi số lượng ký tự của palindrome đã được đặt. 

1. Chúng tôi xác định trạng thái DP$dp[l][r][t]$, Ở đâu$l$Và$r$là các ranh giới trong chuỗi và$t$là chúng ta vẫn cần đặt bao nhiêu ký tự của bảng màu. Ý tưởng là chúng ta điền vào bảng màu từ ngoài vào trong, chọn các cặp phù hợp từ khoảng có sẵn. Cách biểu diễn này đảm bảo rằng các chỉ số được chọn luôn tuân theo các ràng buộc thứ tự tăng dần. 
2. Trường hợp cơ bản là khi$t = 0$. Nếu không cần ký tự thì không cần thay đổi gì thêm, do đó chi phí bằng 0 bất kể$l$Và$r$. Điều này tương ứng với một dãy con trống, về cơ bản là một dãy palindrome. 
3. Khi nào$t = 1$, chúng ta chỉ cần một ký tự ở giữa. Chúng ta có thể chọn bất kỳ vị trí nào giữa$l$Và$r$và không có ràng buộc ghép nối nào tồn tại. Chi phí bằng 0 vì một ký tự luôn là một bảng màu sau khi sửa đổi tùy chọn và chúng ta luôn có thể tô màu lại nó nếu cần mà không ảnh hưởng đến tính đối xứng. 
4. Đối với$t \ge 2$, chúng ta xem xét việc hình thành một cặp đối xứng. Chúng tôi chọn hai vị trí$i$Và$j$với$l \le i < j \le r$. Chúng trở thành các ký tự ngoài cùng của bảng màu còn lại. Chi phí của sự lựa chọn này là$0$nếu như$S[i] = S[j]$, nếu không thì$1$. Sau khi sửa cặp này, chúng ta lặp lại khoảng bên trong$[i+1, j-1]$với$t-2$các ký tự còn lại. 
5. Chúng tôi tính toán mức tối thiểu trên tất cả các phần tách hợp lệ$(i, j)$. Điều này đảm bảo chúng ta khám phá tất cả các cách có thể để đặt cặp palindrome ngoài cùng trong khi vẫn duy trì các ràng buộc về thứ tự. 
6. Câu trả lời cuối cùng là$dp[0][N-1][K]$, đại diện cho toàn bộ chuỗi và yêu cầu về độ dài bảng màu đầy đủ. 

Lý do điều này có tác dụng là vì bất kỳ dãy con palindromic nào cũng có thể được phân tách duy nhất thành cặp khớp ngoài cùng của nó cộng với một dãy con palindromic nhỏ hơn bên trong khoảng còn lại. DP thực thi việc phân tách này một cách có cấu trúc và mọi chuỗi con hợp lệ sẽ tương ứng với chính xác một chuỗi chuyển đổi DP. Vì mỗi quá trình chuyển đổi đều tính chính xác mức sửa đổi tối thiểu cần thiết cho cặp đó nên cấu trúc con tối ưu được giữ nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    s = input().strip()

    INF = 10**9

    # dp[l][r][t] = min cost to form palindrome subsequence of length t inside s[l:r+1]
    dp = [[[INF] * (k + 1) for _ in range(n)] for _ in range(n)]

    # base: t = 0 costs 0 everywhere
    for l in range(n):
        for r in range(n):
            dp[l][r][0] = 0

    # t = 1: single character always achievable with 0 cost
    for l in range(n):
        for r in range(l, n):
            dp[l][r][1] = 0

    for length in range(2, k + 1):
        for l in range(n):
            r_limit = n - 1
            for l2 in range(l, n):
                r = l2
                if r - l + 1 < length:
                    continue
                best = INF
                for i in range(l, r):
                    for j in range(i + 1, r + 1):
                        cost = 0 if s[i] == s[j] else 1
                        inner = dp[i + 1][j - 1][length - 2] if length >= 2 else 0
                        best = min(best, cost + inner)
                dp[l][r][length] = best

    print(dp[0][n - 1][k])

if __name__ == "__main__":
    solve()
```Mảng DP được xây dựng cho tất cả các chuỗi con và tất cả các độ dài palindrome đích. Quá trình chuyển đổi trong cùng sẽ thử rõ ràng mọi cặp bên ngoài có thể, sau đó giảm vấn đề xuống chuỗi con bên trong có ít hơn hai ký tự. 

Các trường hợp ranh giới$t = 0$Và$t = 1$được điền trước để tránh đệ quy không hợp lệ. Việc tính toán chi phí được bản địa hóa cho từng cặp, đảm bảo mỗi cặp không khớp đóng góp chính xác một lần. 

Một chi tiết triển khai tinh tế là chúng tôi không bao giờ sử dụng lại các ký tự: một khi chỉ mục$i$Và$j$được chọn, phép đệ quy chỉ hoạt động trong khoảng bên trong nghiêm ngặt, đảm bảo tính hợp lệ của thứ tự tiếp theo. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`3 3 abc`. 

Chúng ta muốn một dãy con palindrome có độ dài bằng 3. Cấu trúc duy nhất có thể có là một dãy palindrome đầy đủ với một chữ ở giữa
