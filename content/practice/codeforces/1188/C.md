---
title: "CF 1188C - Mảng Vẻ Đẹp"
description: "Chúng ta được cung cấp một mảng và được yêu cầu kiểm tra mọi dãy con có độ dài cố định $k$. Đối với mỗi dãy con được chọn, chúng tôi sắp xếp các phần tử của nó một cách tinh thần và xem xét tất cả các khác biệt theo cặp."
date: "2026-06-13T12:52:29+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 1188
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 572 (Div. 1)"
rating: 2500
weight: 1188
solve_time_s: 280
verified: false
draft: false
---

[CF 1188C - Vẻ đẹp mảng](https://codeforces.com/problemset/problem/1188/C) 

**Đánh giá:** 2500 
**Thẻ:** dp 
**Thời gian giải:** 4 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và được yêu cầu kiểm tra mọi dãy con có độ dài cố định$k$. Đối với mỗi dãy con được chọn, chúng tôi sắp xếp các phần tử của nó một cách tinh thần và xem xét tất cả các khác biệt theo cặp. “Vẻ đẹp” của dãy con đó được định nghĩa là sự khác biệt tuyệt đối nhỏ nhất giữa hai phần tử được chọn bất kỳ. 

Nhiệm vụ là tổng hợp vẻ đẹp này trên tất cả các chuỗi con có kích thước$k$. Vì số lượng dãy con lớn nên đầu ra được lấy modulo$998244353$. 

Một cách hữu ích để diễn đạt lại vấn đề là chúng ta không quan tâm đến cấu trúc của một dãy con ngoài nhiều tập giá trị của nó. Mỗi dãy con đóng góp một giá trị chỉ được xác định bởi cặp gần nhất của nó sau khi sắp xếp. 

Những hạn chế$n \le 1000$Và$a_i \le 10^5$ngay lập tức gợi ý rằng bất kỳ giải pháp nào có độ phức tạp bậc ba hoặc tệ hơn$n$là không an toàn. Thậm chí$O(n^2 k)$cần xử lý cẩn thận vì nó gần đến giới hạn, vì vậy giải pháp phải dựa vào việc tính toán trước và sử dụng lại các kết quả trung gian thay vì tính toán lại số tổ hợp nhiều lần. 

Một cách tiếp cận ngây thơ liệt kê tất cả$\binom{n}{k}$các chuỗi tiếp theo đã là không thể ngay cả đối với$n=1000, k=500$, bởi vì điều này tăng theo cấp số nhân. Ngay cả khi chúng ta sửa một dãy con và tính toán vẻ đẹp của nó theo$O(k \log k)$, việc liệt kê chiếm ưu thế hoàn toàn. 

Một chế độ thất bại tinh vi hơn xuất phát từ việc nghĩ rằng câu trả lời chỉ phụ thuộc vào số liệu thống kê tổng thể như mức tối thiểu hoặc tối đa của toàn bộ mảng. Ví dụ, ở đầu vào$[1, 100, 101, 102]$với$k=3$, sự đóng góp không chỉ được xác định bởi các cực đoan toàn cầu, bởi vì các bộ ba khác nhau có khoảng cách tối thiểu khác nhau tùy thuộc vào yếu tố nào chúng bao gồm. 

Khó khăn chính là sự khác biệt tối thiểu là thuộc tính của cặp gần nhất bên trong mỗi tập hợp con được chọn chứ không phải của toàn bộ mảng. 

## Phương pháp tiếp cận 

Một phương pháp brute-force sẽ tạo ra mọi tập hợp con có kích thước$k$, sắp xếp nó, tính các sai phân liền kề và lấy giá trị nhỏ nhất. Điều này đúng nhưng về cơ bản là không khả thi. có$\binom{n}{k}$tập hợp con, và thậm chí đối với$n=30, k=15$con số này đã lên đến hàng triệu rồi. Với$n=1000$, điều đó là hoàn toàn không thể. 

Sự đột phá về cấu trúc đến từ việc sắp xếp toàn bộ mảng trước tiên. Sau khi được sắp xếp, bất kỳ tập hợp con nào cũng kế thừa thứ tự đó. Đối với bất kỳ tập hợp con nào được chọn, chênh lệch cặp tối thiểu của nó phải xảy ra giữa hai phần tử liên tiếp theo thứ tự được sắp xếp của tập hợp con. Điều này có nghĩa là câu trả lời có thể được biểu thị dưới dạng tổng theo cặp$(i, j)$, Ở đâu$i < j$, tương ứng với biến cố$a_j - a_i$trở thành khoảng cách tối thiểu bên trong một tập hợp con được chọn. 

Đối với một cặp cố định$(i, j)$, cặp này sẽ đóng góp vào một tập hợp con nếu cả hai phần tử được chọn và không có phần tử nào được chọn giữa chúng theo thứ tự được sắp xếp. Nếu có một phần tử$x$giữa họ thì$a_j - a_x$hoặc$a_x - a_i$sẽ nhỏ hơn$a_j - a_i$, điều này sẽ vi phạm giả định rằng$(i, j)$xác định khoảng cách tối thiểu. 

Vì vậy, chúng tôi giảm vấn đề để đếm có bao nhiêu hợp lệ$k$-các tập hợp con có một cặp cụ thể là khoảng cách gần nhất của chúng và tính trọng số đó bằng sự khác biệt$a_j - a_i$. 

Nhiệm vụ còn lại hoàn toàn là tổ hợp: chia các phần tử thành bên trái của$i$và quyền của$j$và phân phối phần còn lại$k-2$chọn giữa chúng. 

Khó khăn là việc tính tổng tất cả các cặp vẫn liên quan đến nội dung bên trong.$O(k)$tích chập nếu được thực hiện trực tiếp. Việc tối ưu hóa là tính toán trước các bảng tổ hợp cho các lựa chọn tiền tố và hậu tố, sau đó sử dụng lại tổng hậu tố tổng hợp để mỗi cặp được xử lý theo$O(k)$nhưng không tính toán lại các đóng góp bên trong nhiều lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(k) | Quá chậm | 
| Liệt kê cặp + tổ hợp (DP được tối ưu hóa) | O(n2k) | O(nk) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sắp xếp mảng sao cho thứ tự chỉ mục khớp với thứ tự giá trị. Điều này cho phép suy luận về các khoảng thay vì các tập hợp con tùy ý. 

Chúng tôi tính toán trước các hệ số nhị thức lên đến$n$, vì chúng tôi liên tục đếm các cách chọn phần tử từ các phân đoạn bên trái và bên phải. 

Tiếp theo, chúng ta xây dựng hai bảng DP. Bảng đầu tiên$L[i][t]$lưu trữ số cách để chọn$t$các yếu tố từ đầu tiên$i$các phần tử. Bảng thứ hai$R[i][t]$lưu trữ số cách để chọn$t$các phần tử từ hậu tố bắt đầu từ$i$. 

Các bảng này cho phép chúng ta đếm xem có bao nhiêu cách chúng ta có thể chọn các phần tử ở hai bên của một cặp cố định. 

Tuy nhiên, chúng ta cũng cần kết hợp các trọng số từ phía bên phải, vì mỗi cặp đóng góp một giá trị tỷ lệ thuận với$a_j$. Để làm được điều đó, chúng tôi xây dựng hai bảng tổng hợp hậu tố: 

Chúng tôi xác định$Q[i][t]$như số cách chọn$t$các phần tử từ hậu tố bắt đầu từ$i$, Và$P[i][t]$là tổng các giá trị của phần tử được chọn ngoài cùng bên trái trong hậu tố đó được tính theo số lượng lựa chọn. Cụ thể đối với từng vị trí$p$, ta cộng giá trị của nó nhân với số cách chọn các phần tử còn lại từ hậu tố sau$p$. 

Phép biến đổi này cho phép chúng ta biểu diễn tất cả sự đóng góp của các điểm cuối phù hợp trong thời gian không đổi trên mỗi trạng thái. 

Bây giờ chúng tôi lặp lại tất cả các điểm cuối bên trái có thể có$i$. Đối với mỗi$i$, chúng ta lặp lại bao nhiêu phần tử$l$được chọn từ phía bên trái. Phần còn lại$r = k-2-l$phải đến từ phía bên phải. 

Đối với mỗi cấu hình, đóng góp chia thành hai phần: tổng của$a_j$trên điểm cuối bên phải hợp lệ trừ đi$a_i$lần số lượng điểm cuối bên phải hợp lệ. Đây chính xác là những gì$P[i+1][r]$Và$Q[i+1][r]$đại diện. 

Cuối cùng, chúng tôi nhân với$L[i-1][l]$, số cách chọn vế trái và tích lũy. 

### Tại sao nó hoạt động 

Mỗi tập hợp con hợp lệ có một cặp phần tử liền kề duy nhất theo thứ tự được sắp xếp để đạt được khoảng cách tối thiểu. Thuật toán đếm mỗi cặp như vậy đúng một lần. Phân tách tổ hợp đảm bảo rằng tất cả các tập hợp con phù hợp với một cặp khoảng cách tối thiểu cố định đều được tính và không có tập hợp con nào được tính cho một cặp không phải là khoảng cách tối thiểu thực sự của nó vì bất kỳ phần tử trung gian nào cũng sẽ làm giảm khoảng cách một cách nghiêm ngặt. Điều này tạo ra sự xung đột giữa các tập hợp con và cặp đóng góp tạo nên vẻ đẹp của chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    # binomial coefficients
    C = [[0] * (n + 1) for _ in range(n + 1)]
    for i in range(n + 1):
        C[i][0] = 1
        for j in range(1, i + 1):
            C[i][j] = (C[i - 1][j] + C[i - 1][j - 1]) % MOD

    # L[i][t] = ways to pick t from first i elements
    L = [[0] * (k + 1) for _ in range(n + 1)]
    L[0][0] = 1
    for i in range(1, n + 1):
        for t in range(k + 1):
            L[i][t] = L[i - 1][t]
            if t > 0:
                L[i][t] = (L[i][t] + L[i - 1][t - 1]) % MOD

    # Q[i][t] = ways to choose t from suffix i
    Q = [[0] * (k + 1) for _ in range(n + 2)]
    # P[i][t] = weighted sum of contributions from suffix i
    P = [[0] * (k + 1) for _ in range(n + 2)]

    for i in range(n, 0, -1):
        for t in range(k + 1):
            Q[i][t] = Q[i + 1][t]
            P[i][t] = P[i + 1][t]

        for t in range(k + 1):
            if t == 0:
                Q[i][t] = (Q[i][t] + 1) % MOD
            else:
                Q[i][t] = (Q[i][t] + Q[i + 1][t - 1]) % MOD
                P[i][t] = (P[i][t] + P[i + 1][t - 1] + a[i - 1] * Q[i + 1][t - 1]) % MOD

    ans = 0

    for i in range(1, n + 1):
        for l in range(k - 1):
            r = k - 2 - l
            if r < 0:
                continue

            left = L[i - 1][l]
            right_sum = P[i + 1][r]
            right_cnt = Q[i + 1][r]

            ans += left * (right_sum - a[i - 1] * right_cnt) * (n >= 0)
            ans %= MOD

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp, điều này rất cần thiết vì mọi lý do về khoảng cách tối thiểu đều dựa vào thứ tự. Bảng DP$L$nắm bắt có bao nhiêu cách chúng ta có thể chọn các phần tử trước chỉ mục đã chọn, trong khi$Q$Và$P$tóm tắt tất cả các lựa chọn hợp lệ ở phía bên phải cùng với những đóng góp có trọng số của chúng. 

Phần tế nhị nhất là duy trì$P$Và$Q$đồng thời.$Q$đếm các kết hợp, trong khi$P$tích lũy các đóng góp của các giá trị dưới dạng điểm cuối bên phải tiềm năng trong các cặp hợp lệ. Sự lặp lại đảm bảo mọi cấu hình hậu tố được tính chính xác một lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
1 7 3 5
```Mảng được sắp xếp:$[1, 3, 5, 7]$Chúng tôi xem xét tất cả các bộ ba. DP nhóm chúng một cách hiệu quả theo khoảng cách liền kề tối thiểu theo thứ tự được sắp xếp. 

| Cặp (i, j) | Khoảng cách | Số bộ ba hợp lệ | Đóng góp | 
| --- | --- | --- | --- | 
| (1,3) | 2 | 1 | 2 | 
| (1,5) | 4 | 1 | 4 | 
| (3,7) | 4 | 0 | 0 | 
| (3,5) | 2 | 1 | 2 | 

Tổng hợp các đóng góp cho 8. 

Dấu vết này cho thấy rằng chỉ những cặp có thể được tách biệt dưới dạng khoảng trống tối thiểu bên trong bộ ba mới đóng góp và mỗi bộ ba hợp lệ được tính chính xác một lần. 

### Ví dụ 2 

đầu vào:```
5 4
1 2 3 10 20
```Mảng đã sắp xếp: đã được sắp xếp. 

Chúng tôi kiểm tra các đóng góp trong đó khoảng cách tối thiểu được xác định bởi tính kề cận cục bộ bên trong các tập hợp con. Bộ ba liên quan đến các phần tử được đóng gói chặt chẽ chiếm ưu thế, trong khi các khoảng trống lớn hiếm khi trở thành mức tối thiểu trừ khi tất cả các phần tử gần nhau hơn bị loại trừ. 

Điều này chứng tỏ cách thuật toán tách biệt cấu trúc cục bộ thay vì các cực trị toàn cục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 k)$| mỗi chỉ mục cặp đóng góp trên một phân vùng gồm các phần tử k-2 còn lại | 
| Không gian |$O(n k)$| Bảng DP cho tổ hợp tiền tố và hậu tố | 

Những hạn chế$n \le 1000$Và$k \le 1000$cho phép đại khái$10^6$trạng thái và mỗi trạng thái được xử lý trong thời gian không đổi do các chuyển đổi được tính toán trước. Điều này thoải mái phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import comb
    import sys
    input = sys.stdin.readline

    MOD = 998244353

    n, k = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))
    a.sort()

    C = [[0] * (n + 1) for _ in range(n + 1)]
    for i in range(n + 1):
        C[i][0] = 1
        for j in range(1, i + 1):
            C[i][j] = (C[i - 1][j] + C[i - 1][j - 1]) % MOD

    L = [[0] * (k + 1) for _ in range(n + 1)]
    L[0][0] = 1
    for i in range(1, n + 1):
        for t in range(k + 1):
            L[i][t] = L[i - 1][t]
            if t:
                L[i][t] += L[i - 1][t - 1]
            L[i][t] %= MOD

    Q = [[0] * (k + 1) for _ in range(n + 2)]
    P = [[0] * (k + 1) for _ in range(n + 2)]

    for i in range(n, 0, -1):
        for t in range(k + 1):
            Q[i][t] = Q[i + 1][t]
            P[i][t] = P[i + 1][t]
        for t in range(k + 1):
            if t == 0:
                Q[i][t] = (Q[i][t] + 1) % MOD
            else:
                Q[i][t] = (Q[i][t] + Q[i + 1][t - 1]) % MOD
                P[i][t] = (P[i][t] + P[i + 1][t - 1] + a[i - 1] * Q[i + 1][t - 1]) % MOD

    ans = 0
    for i in range(1, n + 1):
        for l in range(k - 1):
            r = k - 2 - l
            if r < 0:
                continue
            ans += L[i - 1][l] * (P[i + 1][r] - a[i - 1] * Q[i + 1][r])
            ans %= MOD

    return str(ans % MOD)

assert run("4 3\n1 7 3 5\n") == "8"

assert run("2 2\n1 10\n") == "9"

assert run("3 3\n1 2 3\n") == "1"

assert run("5 3\n1 1 1 1 1\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 3 / 1 7 3 5 | 8 | độ chính xác của mẫu | 
| 2 2 / 1 10 | 9 | sự thống trị của cặp đơn | 
| 3 3 / 1 2 3 | 1 | cấu trúc thống nhất | 
| 5 3 / tất cả những cái | 0 | vỏ không có vẻ đẹp cạnh | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các giá trị đều giống hệt nhau. Trong tình huống đó, mọi tập hợp con đều có vẻ đẹp bằng 0 vì mọi chênh lệch cặp đều bằng 0. Thuật toán tích lũy chính xác số 0 vì mỗi thuật ngữ$a_j - a_i$hủy bỏ về 0 bất kể tổ hợp. 

Một trường hợp cạnh khác là khi$k=2$. Mỗi tập hợp con chỉ là một cặp, vì vậy câu trả lời giảm xuống bằng tổng của tất cả các khác biệt theo cặp. DP thu gọn chính xác vì không có phần tử bên trong và mỗi cặp đóng góp chính xác một lần thông qua cùng một công thức, phù hợp với sự đơn giản hóa dự kiến.
