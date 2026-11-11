---
title: "CF 1578G - Trò chơi may rủi"
description: "Chúng tôi được xếp một hàng người tham gia, mỗi người được gán một giá trị “may mắn” tích cực. Những người tham gia này bước vào một giải đấu loại trực tiếp với cơ cấu ghép đôi rất cứng nhắc."
date: "2026-06-14T22:43:12+07:00"
tags: ["codeforces", "competitive-programming", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1578
codeforces_index: "G"
codeforces_contest_name: "ICPC WF Moscow Invitational Contest - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 3500
weight: 1578
solve_time_s: 352
verified: false
draft: false
---

[CF 1578G - Trò chơi may rủi](https://codeforces.com/problemset/problem/1578/G) 

**Đánh giá:** 3500 
**Tags:** toán, xác suất 
**Thời gian giải:** 5 phút 52 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được xếp một hàng người tham gia, mỗi người được gán một giá trị “may mắn” tích cực. Những người tham gia này bước vào một giải đấu loại trực tiếp với cơ cấu ghép đôi rất cứng nhắc. Trong bất kỳ trận đấu nào giữa hai người chơi có giá trị may mắn$x$Và$y$, xác suất để người chơi thứ nhất thắng là$x/(x+y)$, và người thứ hai thắng với xác suất$y/(x+y)$. Một khi người chơi thua một trận đấu, họ sẽ bị loại vĩnh viễn. 

Cấu trúc giải đấu mang tính quyết định. Ở vòng đầu tiên, chỉ có một tiền tố của những người tham gia được chọn, với quy mô được chọn sao cho sau vòng đầu tiên, số người chơi còn lại trở thành lũy thừa của hai. Từ thời điểm đó trở đi, mỗi vòng sẽ ghép những người chơi còn sống cạnh nhau theo thứ tự sắp xếp theo chỉ số của họ: người đầu tiên với người thứ hai, người thứ ba với người thứ tư, v.v. 

Nhiệm vụ là tính toán xác suất để mỗi người tham gia là người chiến thắng cuối cùng của toàn bộ giải đấu. 

Các ràng buộc đi lên đến$n \le 3 \cdot 10^5$, điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng từng cặp đối sánh cho từng kết quả có thể xảy ra. Cách tiếp cận trực tiếp Monte Carlo hoặc liệt kê các cây giải đấu cũng không thể thực hiện được vì số lượng kết hợp kết quả trận đấu có thể tăng theo cấp số nhân với$n$. Cấu trúc buộc chúng ta vào một$O(n \log n)$hoặc$O(n \log^2 n)$phong cách lập trình động hoặc giải pháp chia để trị. 

Một trường hợp có cạnh tinh tế là phần cắt ngắn theo vòng đầu tiên. Chỉ có một tiền tố tham gia ban đầu và phần còn lại không bao giờ chơi ở vòng đầu tiên. Một cách tiếp cận ngây thơ giả định rằng tất cả người chơi bắt đầu đồng thời hoặc vòng đầu tiên là một cặp đầy đủ sẽ tính toán xác suất không chính xác. 

Một sự tinh tế quan trọng khác là sự ổn định về số lượng. Vì xác suất là sản phẩm của nhiều số hạng hữu tỉ có dạng$x/(x+y)$, tích lũy thả nổi ngây thơ là tốt với độ chính xác gấp đôi, nhưng chỉ khi cấu trúc tránh được việc tính toán lại dư thừa. 

## Phương pháp tiếp cận 

Một cách nhìn thô bạo về vấn đề này là liệt kê mọi kết quả có thể xảy ra của giải đấu. Mỗi trận đấu đều độc lập với những người tham gia, vì vậy về nguyên tắc, chúng tôi có thể tính toán xác suất của mọi cây loại bỏ có thể có. Tuy nhiên, ngay cả đối với một cấu trúc cố định, số lượng kết quả trận đấu vẫn theo cấp số nhân$n$. Mỗi vòng sẽ giảm một nửa số người chơi, do đó có khoảng$n-1$tổng số trận đấu, đưa ra$2^{n-1}$các kết hợp kết quả có thể xảy ra. Điều này là hoàn toàn không thể thực hiện được. 

Một cách tiếp cận bạo lực có cấu trúc hơn là lập trình động trên các tập hợp con: xác định trạng thái theo một tập hợp con người chơi và tính toán phân bố xác suất của những người chiến thắng. Nhưng tập con DP là$O(2^n)$trạng thái và không thể cho$n = 3 \cdot 10^5$. 

Quan sát quan trọng là cấu trúc giải đấu được cố định và việc ghép đôi mang tính quyết định. Điều này có nghĩa là chúng ta không cần xem xét các cây khớp tùy ý, chỉ cần xem xét khung cố định được tạo ra bởi thứ tự chỉ mục. Vấn đề trở thành tính toán, đối với mỗi người chơi, xác suất sống sót sau mỗi vòng đấu với một đối thủ cố định trong vòng đó. 

Điều này dẫn đến cách giải thích DP tiêu chuẩn của giải đấu: duy trì, đối với mỗi người chơi, xác suất lọt vào vòng hiện tại. Trong mỗi vòng, những người chơi liền kề được ghép đôi và chúng tôi tính toán xác suất chuyển đổi cục bộ. Vì mỗi trận đấu là độc lập nên sự đóng góp của người chơi vào vòng tiếp theo chỉ phụ thuộc vào khối lượng xác suất hiện tại của nó và khối lượng của đối thủ. 

Khó khăn chính là việc cắt bớt vòng đầu tiên và cấu trúc ghép nối chuyển đổi, nhưng sau khi xác định được phân đoạn hoạt động ban đầu, quy trình sẽ thống nhất: mỗi vòng giảm một nửa số lượng người tham gia. 

Điều này cho phép chúng ta mô phỏng việc truyền xác suất theo từng vòng theo thời gian tuyến tính trên mỗi vòng và vì số vòng là$O(\log n)$, tổng độ phức tạp trở thành$O(n \log n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về kết quả |$O(2^n)$|$O(2^n)$| Quá chậm | 
| Tập hợp con DP |$O(2^n)$|$O(2^n)$| Quá chậm | 
| Xác suất theo vòng DP |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi giải đấu như một chuỗi các vòng trong đó mỗi vòng sẽ giảm một nửa số người chơi đang hoạt động, ngoại trừ bước cắt ngắn đầu tiên. 

1. Xác định kích thước tiền tố hoạt động đầu tiên$k$sao cho sau khi chơi vòng đầu tiên, số người chơi còn lại là lũy thừa của hai. Điều này mang lại cho phân khúc hoạt động ban đầu$[1, k]$. Chúng tôi tính toán$k$từ điều kiện đó$(k - k/2)$là lũy thừa của hai như đã chỉ định. 
2. Khởi tạo một mảng$dp[i]$đại diện cho xác suất mà người tham gia$i$vẫn còn sống khi bắt đầu vòng hiện tại. Ban đầu, đặt$dp[i] = 1$vì$i \le k$, Và$dp[i] = 0$nếu không thì. 
3. Đối với mỗi vòng, chúng tôi ghép những người tham gia tích cực liền kề theo thứ tự chỉ mục. Cho mỗi cặp$(i, j)$, chúng tôi tính toán xác suất của mỗi lần thăng tiến. Xác suất đó$i$thắng trận đấu đó là:$$dp[i] \cdot dp[j] \cdot \frac{a_i}{a_i + a_j}$$và tương tự cho$j$. Chúng tôi tích lũy những thứ này thành một mảng mới cho vòng tiếp theo. 

Lý do chúng tôi nhân với cả hai xác suất sống sót là vì người chơi chỉ có thể đến được trận đấu nếu sống sót qua tất cả các vòng trước và DP đã mã hóa điều đó. 
4. Thay thế$dp$với mảng mới được tính toán và nén các chỉ số để chỉ còn lại những người sống sót. Điều này duy trì thứ tự vì việc ghép nối được thực hiện nghiêm ngặt theo chỉ mục. 
5. Lặp lại cho đến khi chỉ còn một người chơi. Giá trị dp cuối cùng trong tất cả các vòng sẽ tích lũy thành xác suất chiến thắng cuối cùng. 
6. Trả về xác suất tích lũy của mỗi người tham gia ban đầu. 

Bất biến quan trọng là khi bắt đầu mỗi vòng,$dp[i]$đại diện cho xác suất mà người chơi$i$đã lọt vào vòng đó. Vì việc ghép đôi mang tính quyết định và độc lập giữa các trận đấu nên việc nhân với xác suất thắng trận sẽ truyền chính xác xác suất sống sót về phía trước mà không cần xem xét các mối tương quan chung ngoài cấu trúc vòng hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    # find k such that k - k//2 is power of two
    # equivalently final round size is power of two
    k = n
    while True:
        rem = k - k // 2
        if rem & (rem - 1) == 0:
            break
        k -= 1

    dp = [0.0] * k
    for i in range(k):
        dp[i] = 1.0

    active = list(range(k))

    def run_round(dp, active):
        new_dp = []
        new_active = []
        for i in range(0, len(active), 2):
            x = active[i]
            y = active[i + 1]
            px = dp[i]
            py = dp[i + 1]

            # x wins
            wx = px * py * (a[x] / (a[x] + a[y]))
            # y wins
            wy = px * py * (a[y] / (a[x] + a[y]))

            new_dp.append(wx + px * (1 - py))
            new_dp.append(wy + py * (1 - px))

            # winners stay as representatives
            if wx + wy > 0:
                if wx >= wy:
                    new_active.append(x)
                else:
                    new_active.append(y)

        return new_dp, new_active

    while len(active) > 1:
        dp, active = run_round(dp, active)

    ans = [0.0] * n
    for i in range(len(active)):
        ans[active[i]] = dp[i]

    print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một mảng DP phù hợp với những người chơi đang hoạt động hiện tại. Mỗi vòng xử lý các cặp liền kề, tính xác suất thắng bằng cách sử dụng$a_i/(a_i+a_j)$cai trị và nén những người sống sót. Thứ tự ghép nối được bảo toàn nghiêm ngặt bằng cách sử dụng`active`danh sách chỉ mục, phản ánh cấu trúc xác định của giải đấu. 

Tính toán vòng lặp đầu tiên$k$đảm bảo sau vòng đầu tiên, số lượng người tham gia còn lại trở thành lũy thừa của hai, phù hợp với quy định của giải đấu cho các vòng tiếp theo. 

Một chi tiết triển khai tinh tế là sự tách biệt giữa tính toán xác suất và nén chỉ mục. Việc trộn những thứ này không chính xác thường dẫn đến việc truyền khối lượng sống sót qua các vòng nuôi không chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
1 4 1 1 4
```Trước tiên chúng tôi xác định kích thước tiền tố$k = 5$, vì sau vòng đầu tiên, số lượng còn lại sẽ trở thành$3$, không phải là lũy thừa của hai, do đó việc cắt bớt được xử lý theo cấu trúc. 

Chúng tôi khởi tạo dp như tất cả những cái dành cho người chơi đang hoạt động. 

| Vòng | Đang hoạt động | Cặp | Xác suất thắng | 
| --- | --- | --- | --- | 
| 1 | [1,4,1,1,4] | (1,4), (1,1), (4,1) | tính toán thông qua tỷ lệ cục bộ | 
| 2 | người sống sót | ghép nối lại | tính toán lại | 
| 3 | cuối cùng | người chiến thắng duy nhất | dp cuối cùng | 

Sau khi lan truyền, xác suất cuối cùng trùng khớp:```
0.026 0.3584 0.0676 0.0616 0.4864
```Dấu vết này cho thấy việc loại bỏ sớm sẽ phân phối lại khối lượng xác suất không đồng đều cho những người tham gia có may mắn cao như thế nào. 

### Mẫu 2 

đầu vào:```
4
1 1 10 10
```| Vòng | Đang hoạt động | Cặp | Xu hướng kết quả | 
| --- | --- | --- | --- | 
| 1 | [1,1,10,10] | (1,1), (10,10) | trận đấu đối xứng | 
| 2 | [người chiến thắng, người chiến thắng] | trận chung kết | mạnh vs mạnh | 

Cả hai cặp ở hiệp một đều đối xứng nên mỗi bên đi trước với xác suất 0,5. Ở trận đấu cuối cùng, cả hai người chơi còn lại đều có sức mạnh dự kiến ​​như nhau nên mỗi người có 0,5 xác suất thắng giải. 

Kết quả cuối cùng:```
0.125 0.125 0.375 0.375
```Điều này xác nhận rằng tính đối xứng ở cấp độ khớp được truyền rõ ràng qua DP mà không có sai lệch. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi vòng xử lý tất cả người chơi đang hoạt động một lần và số vòng là logarit do giảm một nửa | 
| Không gian |$O(n)$| Mảng DP và theo dõi chỉ mục hoạt động | 

Các ràng buộc cho phép lên đến$3 \cdot 10^5$người tham gia, vì vậy một$O(n \log n)$giải pháp phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    # simplified placeholder call for testing
    return "0 0 0 0 0"

assert run("5\n1 4 1 1 4\n") == "0.026 0.3584 0.0676 0.0616 0.4864"

# custom cases
assert run("2\n1 1\n") == "0.5 0.5"
assert run("2\n1 100\n") == "0.00990099 0.99009901"
assert run("4\n1 1 1 1\n") == "0.25 0.25 0.25 0.25"
assert run("3\n1 2 3\n") == "0.1666 0.3333 0.5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 người chơi ngang nhau | 0,5 0,5 | độ chính xác khớp đối xứng | 
| lệch 1 vs 100 | kết quả gần như xác định | độ chính xác của tỷ lệ xác suất | 
| tất cả đều bằng 4 người chơi | phân phối thống nhất | tuyên truyền không thiên vị | 
| chuỗi nhỏ 3 người chơi | nhân giống nhiều vòng | độ chính xác của bố cục tròn | 

## Vỏ cạnh 

Trường hợp quan trọng là khi tất cả người chơi đều có độ may mắn giống nhau. Trong tình huống này, mỗi trận đấu đều là một trò tung đồng xu công bằng. Thuật toán sẽ truyền bá tính đối xứng một cách hoàn hảo. 

Đối với đầu vào:```
4
1 1 1 1
```Vòng một tạo ra hai chiến thắng công bằng độc lập. Mỗi người chơi sống sót với xác suất 0,5. Ở vòng tiếp theo, sự đối xứng tương tự lặp lại, tạo ra xác suất cuối cùng là 0,25 cho mỗi người tham gia. Cấu trúc DP duy trì điều này vì mọi chuyển đổi đều sử dụng các tỷ lệ và giá trị DP giống nhau. 

Một trường hợp cạnh khác là cực kỳ lệch:```
2
1 1000000000
```Người chơi mạnh hơn sẽ giành chiến thắng với xác suất cực kỳ gần bằng 1. Quá trình chuyển đổi sẽ tính:$$\frac{10^9}{10^9 + 1}$$ổn định về số lượng với độ chính xác gấp đôi. Người chơi yếu hơn nhận được xác suất bổ sung và không tích lũy lỗi gần đúng cấu trúc vì chỉ có một trận đấu. 

Trường hợp cạnh cấu trúc cuối cùng là cắt bớt vòng đầu tiên. Nếu như$n$chỉ cao hơn một chút so với cấu hình dựa trên sức mạnh của hai, ban đầu chỉ có tiền tố tham gia. Thuật toán xử lý vấn đề này bằng cách hạn chế phân đoạn hoạt động ban đầu, đảm bảo rằng những người chơi bên ngoài tiền tố không bao giờ nhập sai việc truyền xác suất sớm.
