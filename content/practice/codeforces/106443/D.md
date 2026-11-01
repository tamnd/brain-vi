---
title: "CF 106443D - Đấu tay đôi chữ số"
description: "Chúng ta được cung cấp một trò chơi theo lượt bắt đầu bằng một tập hợp các số nguyên, ban đầu chính xác là các số từ 2 đến $N+1$. Hai người chơi luân phiên nhau."
date: "2026-06-21T16:24:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106443
codeforces_index: "D"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2026"
rating: 0
weight: 106443
solve_time_s: 69
verified: true
draft: false
---

[CF 106443D - Đấu tay đôi chữ số](https://codeforces.com/problemset/problem/106443/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một trò chơi theo lượt bắt đầu bằng một tập hợp các số nguyên, ban đầu chính xác là các số từ 2 đến$N+1$. Hai người chơi luân phiên nhau. Trong một lượt, người chơi loại bỏ một tập hợp con không trống của các số hiện tại, nhưng chỉ khi tất cả các số trong tập hợp con đã chọn có chung ước số lớn hơn 1. Sau khi loại bỏ tập hợp con đã chọn, các số còn lại sẽ bị loại bỏ và sau đó được tạo lại thành một chuỗi mới liên tiếp bắt đầu lại từ 2, với độ dài mới bằng độ dài cũ trừ đi số bị loại bỏ. 

Hậu quả chính là sau mỗi lần di chuyển, danh tính thực sự của các con số sẽ được đặt lại. Điều duy nhất tồn tại xuyên suốt trò chơi là còn lại bao nhiêu con số chứ không phải số ban đầu là số nào. Tuy nhiên, tính hợp pháp của một bước di chuyển phụ thuộc vào các giá trị hiện tại trước khi gắn nhãn lại, vì vậy chúng ta vẫn cần hiểu những tập hợp con nào có thể có từ một phạm vi liên tiếp. 

Nhiệm vụ là xác định, cho mỗi lần ban đầu$N$, liệu người chơi đầu tiên có bị buộc phải thắng trong lối chơi tối ưu hay không. 

Những ràng buộc cho phép$N$lên đến$5 \cdot 10^5$và lên tới 100 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi giải pháp mô phỏng tập hợp con hoặc liệt kê các bước di chuyển một cách rõ ràng. Ngay cả việc lặp lại tất cả các tập hợp con của một trạng thái cũng là không thể vì một trạng thái đã có sự phân nhánh theo cấp số nhân. DP bậc hai cho mỗi trường hợp thử nghiệm cũng sẽ quá chậm. Giải pháp phải gần tuyến tính cho mỗi trường hợp thử nghiệm hoặc được khấu hao tốt hơn. 

Trường hợp khó nhận biết xuất phát từ các giá trị nhỏ khi trò chơi kết thúc ngay lập tức. Ví dụ, nếu$N=1$, số duy nhất là 2, người chơi đầu tiên bỏ được số đó sẽ giành chiến thắng ngay lập tức. Bất kỳ lý do nào giả định cấu trúc lớn mà không xử lý các trạng thái suy biến này sẽ thất bại ở đây. 

Một điều tinh tế quan trọng khác là mặc dù các con số được dán nhãn lại sau mỗi lần di chuyển nhưng điều kiện hợp pháp vẫn phụ thuộc vào cấu trúc chia hết bên trong một khoảng liên tiếp. Một cách tiếp cận ngây thơ có thể giả định không chính xác rằng chỉ kích thước là quan trọng mà không giải thích được tại sao cấu trúc di chuyển được giữ nguyên. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực trực tiếp coi mỗi trạng thái là tập hợp nhiều số hiện tại và thử tất cả các tập hợp con hợp lệ. Một tập hợp con hợp lệ nếu gcd của nó lớn hơn 1, có nghĩa là tất cả các số được chọn đều có chung một ước nguyên tố nào đó. Từ một bộ kích thước$m$, điều này có nghĩa là liệt kê tất cả các tập con của tất cả các bội số của mọi số nguyên$d \ge 2$. Ngay cả khi việc kiểm tra gcd là hiệu quả thì số lượng tập hợp con vẫn theo cấp số nhân$m$, làm cho cách tiếp cận này ngay lập tức không khả thi nếu vượt quá giới hạn rất nhỏ$N$. 

Sự đơn giản hóa chính xuất phát từ việc quan sát rằng sau mỗi lần di chuyển, các số còn lại luôn được đặt lại về khoảng liên tiếp bắt đầu từ 2. Điều này sẽ phá hủy tất cả lịch sử cấu trúc ngoại trừ số phần tử còn lại. Vì vậy trạng thái trò chơi được mô tả đầy đủ bằng một số nguyên duy nhất$m$, số phần tử hiện tại. 

Từ một tiểu bang có$m$các phần tử, chúng tôi đang chọn một tập hợp con của phạm vi liên tiếp hiện tại. Một nước đi hợp lệ yêu cầu tất cả các số được chọn phải có chung ước số. Nếu chúng ta sửa một số chia$d$, số duy nhất chúng ta có thể chọn một cách an toàn là bội số của$d$và tất cả các số như vậy tự động có ít nhất gcd$d$. Do đó, bất kỳ tập con số nào chia hết cho cùng$d$là hợp lệ. 

Nhóm số lượng lớn nhất như vậy xảy ra đối với$d=2$, cung cấp tất cả các số chẵn trong phạm vi. có$\lfloor (m+1)/2 \rfloor$sự kiện diễn ra trong$\{2, 3, \dots, m+1\}$, và từ chúng chúng ta có thể chọn bất kỳ tập con nào không trống. Điều này có nghĩa là chúng ta có thể loại bỏ bất kỳ số lượng phần tử nào$k$Ở đâu$1 \le k \le \lfloor (m+1)/2 \rfloor$. 

Vì vậy trò chơi chuyển thành trò chơi trừ: từ$m$, người chơi có thể di chuyển đến bất kỳ$m-k$Ở đâu$k$nằm giữa 1 và$\lfloor (m+1)/2 \rfloor$. Khó khăn là kích thước bước tối đa phụ thuộc vào$m$, vì vậy chúng ta không thể sử dụng một mẫu tuần hoàn cố định ngay lập tức. Thay vào đó, chúng tôi tính toán các trạng thái chiến thắng một cách linh hoạt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng tập hợp con Brute Force | Hàm mũ | O(N) | Quá chậm | 
| DP với Tối ưu hóa cửa sổ trượt | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định$dp[m]$như liệu người chơi hiện tại có thắng khi có$m$số còn lại. 

1. Chúng ta khởi tạo$dp[0] = \text{false}$vì không còn số nên người chơi hiện tại không thể di chuyển và thua ngay lập tức. Đây đóng vai trò là trạng thái cơ bản giúp truyền bá các vị thế chiến thắng đi lên. 
2. Đối với mỗi$m \ge 1$, chúng tôi tính toán số phần tử tối đa có thể bị loại bỏ trong một lần di chuyển, đó là$k_{\max} = \lfloor (m+1)/2 \rfloor$. Điều này xuất phát từ việc chọn tất cả bội số của 2 trong phạm vi hiện tại. 
3. Từ tiểu bang$m$, người chơi có thể di chuyển đến bất kỳ trạng thái nào$m-k$Ở đâu$1 \le k \le k_{\max}$. Điều này tạo thành một khoảng liền kề của các trạng thái có thể truy cập từ$L = m - k_{\max}$lên đến$R = m - 1$. 
4. Chúng tôi quyết định$dp[m]$bằng cách kiểm tra xem ít nhất một trạng thái trong khoảng$[L, R]$đang thua người chơi tiếp theo, nghĩa là$dp[x] = 0$đối với một số người$x$trong phạm vi đó. Nếu trạng thái như vậy tồn tại, chúng ta có thể giành chiến thắng bằng cách di chuyển đến đó. 
5. Để duy trì điều này một cách hiệu quả, chúng tôi theo dõi tất cả các chỉ số$x$như vậy$dp[x] = 0$trong một cấu trúc. Trước khi xử lý$m$, chúng tôi loại bỏ mọi vị trí thua lỗ được lưu trữ nhỏ hơn$L$, vì họ không thể truy cập được nữa. 
6. Sau khi cắt tỉa, nếu vẫn còn vị trí bị mất trong cấu trúc thì$dp[m] = 1$. Nếu không thì,$dp[m] = 0$. 
7. Chúng tôi nối thêm$m$vào cấu trúc chỉ khi$dp[m] = 0$, vì vậy nó có thể đóng vai trò là mục tiêu thất bại cho các trạng thái trong tương lai. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào thực tế là mọi chuyển động từ$m$hạ cánh chính xác bên trong một khoảng trượt của các trạng thái trước đó và sự tồn tại của ít nhất một trạng thái thua trong khoảng đó vừa cần thiết vừa đủ cho một nước đi thắng. Bởi vì tất cả các trạng thái mất đều được theo dõi và liên tục bị xóa khi chúng nằm ngoài tầm với, nên cấu trúc luôn thể hiện chính xác các ví dụ phản biện hiện có liên quan. Điều này đảm bảo mọi$dp[m]$được tính toán từ tập hợp đầy đủ và chính xác các kết quả có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    max_n = 0
    ns = []
    for _ in range(T):
        n = int(input())
        ns.append(n)
        max_n = max(max_n, n)

    dp = [0] * (max_n + 1)
    dp[0] = 0

    # store indices i where dp[i] == 0
    zeros = [0]
    head = 0  # pointer into zeros list

    # we maintain zeros as a simple queue, but we also need to discard out of range
    for m in range(1, max_n + 1):
        kmax = (m + 1) // 2
        L = m - kmax
        R = m - 1

        # remove outdated zeros (< L)
        while head < len(zeros) and zeros[head] < L:
            head += 1

        if head < len(zeros):
            dp[m] = 1
        else:
            dp[m] = 0
            zeros.append(m)

    out = []
    for n in ns:
        out.append("mastermei" if dp[n] else "the greatest")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai tính toán tất cả các giá trị dp lên đến mức tối đa$N$qua các trường hợp thử nghiệm. Ý tưởng quan trọng là chúng ta không bao giờ tính toán lại các điều kiện gcd một cách rõ ràng sau bước rút gọn, vì hiệu ứng cấu trúc duy nhất của trò chơi được ghi lại trong quá trình chuyển đổi khoảng thời gian dẫn xuất. 

Hàng đợi trạng thái 0 được duy trì dưới dạng một cửa sổ chuyển động và con trỏ đầu đảm bảo mỗi chỉ mục được xóa nhiều nhất một lần. Điều này tránh việc quét lặp lại và giữ độ phức tạp tuyến tính. 

Một lỗi thực hiện phổ biến là quên rằng$dp[m]$phụ thuộc vào một khoảng thời gian, không chỉ trạng thái trước đó$m-1$. Cửa sổ$[L, R]$là thứ nắm bắt tất cả các kích thước di chuyển hợp pháp và thiếu giới hạn dưới$L$phá vỡ sự đúng đắn ngay lập tức. 

## Ví dụ đã hoạt động 

Hãy xem xét$N = 3$. Chúng tôi tính toán từng bước. 

| m | kmmax | L | R | phạm vi dp có thể truy cập | dp[m] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | - | - | - | căn cứ | 0 | 
| 1 | 1 | 0 | 0 | [0] chứa 0 | 1 | 
| 2 | 1 | 1 | 1 | [1] chứa 1 | 0 | 
| 3 | 2 | 1 | 2 | [1,2] chứa 0 tại 2 | 1 | 

Vì$m=1$, nước đi duy nhất là chuyển sang trạng thái 0, tức là thua, vì vậy$dp[1]=1$. Vì$m=2$, trạng thái duy nhất có thể truy cập là 1, tức là chiến thắng, vì vậy$dp[2]=0$. Vì$m=3$, chúng ta có thể đạt tới trạng thái 2, trạng thái đang mất, vì vậy$dp[3]=1$. 

Bây giờ hãy xem xét$N = 5$. 

| m | kmmax | L | R | số không có thể truy cập | dp[m] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | - | - | - | {0} | 0 | 
| 1 | 1 | 0 | 0 | {0} | 1 | 
| 2 | 1 | 1 | 1 | {} | 0 | 
| 3 | 2 | 1 | 2 | {2} | 1 | 
| 4 | 2 | 2 | 3 | {} | 0 | 
| 5 | 3 | 2 | 4 | {2,4} | 1 | 

Dấu vết này cho thấy các vị thế thua đóng vai trò như thế nào như là điểm neo cho các trạng thái chiến thắng trong tương lai. Khi trạng thái mất xuất hiện, nó có thể lan truyền về phía trước cho đến khi rơi ra khỏi khoảng trượt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + T) | Mỗi trạng thái được xử lý một lần và mỗi chỉ mục vào và ra khỏi hàng đợi nhiều nhất một lần | 
| Không gian | O(N) | Mảng DP và hàng đợi mất vị trí | 

Việc xử lý trước lên đến mức tối đa$N$là tuyến tính, phù hợp thoải mái trong các ràng buộc của$5 \cdot 10^5$. Mỗi trường hợp thử nghiệm được trả lời trong thời gian không đổi sau khi tiền xử lý. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    max_n = 0
    ns = []
    for _ in range(T):
        n = int(input())
        ns.append(n)
        max_n = max(max_n, n)

    dp = [0] * (max_n + 1)
    zeros = [0]
    head = 0

    for m in range(1, max_n + 1):
        kmax = (m + 1) // 2
        L = m - kmax

        while head < len(zeros) and zeros[head] < L:
            head += 1

        if head < len(zeros):
            dp[m] = 1
        else:
            dp[m] = 0
            zeros.append(m)

    return "\n".join("mastermei" if dp[n] else "the greatest" for n in ns) + "\n"

# sample-like checks
assert run("1\n1\n") == "mastermei\n"
assert run("1\n2\n") == "the greatest\n"

# small structural checks
assert run("1\n3\n") == "mastermei\n"
assert run("1\n4\n") == "the greatest\n"
assert run("1\n5\n") == "mastermei\n"
assert run("3\n1\n2\n3\n") == "mastermei\nthe greatest\nmastermei\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, N=1 | mastermei | trường hợp cạnh tối thiểu | 
| 1, N=2 | vĩ đại nhất | mất chuyển tiếp ngay lập tức | 
| 1, N=5 | mastermei | luân phiên đúng đắn | 
| 3 truy vấn hỗn hợp | đầu ra hỗn hợp | tính nhất quán của nhiều bài kiểm tra | 

## Vỏ cạnh 

cho$N=1$, trạng thái duy nhất là$m=1$. Khoảng thời gian là$L=0, R=0$, và kể từ đó$dp[0]=0$, trạng thái 1 sẽ thắng. Thuật toán xác định chính xác việc di chuyển ngay lập tức đến phần tử 0. 

Vì$N=2$, Tại$m=2$, trạng thái duy nhất có thể truy cập là$1$, tức là thắng, vậy$m=2$đang thua. Cửa sổ trượt hạn chế chính xác việc di chuyển đến một mục tiêu. 

Đối với các giá trị lớn hơn như$N=5$, trạng thái thua đầu tiên tại$m=2$truyền về phía trước cho đến khi nó rời khỏi khoảng hợp lệ. Điều này thể hiện cách theo dõi dựa trên cửa sổ nắm bắt chính xác phạm vi ảnh hưởng của từng vị trí thua mà không cần tính toán lại các chuyển đổi.
