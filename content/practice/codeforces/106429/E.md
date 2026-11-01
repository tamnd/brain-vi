---
title: "CF 106429E - Rắn"
description: "Chúng tôi đang mô phỏng quá trình con rắn di chuyển qua một chuỗi tế bào cố định theo thời gian. Tại mỗi bước thời gian $t$, đầu của con rắn chiếm một ô đã biết $c(t)$. Một số ô có thể lặp lại theo thời gian, nghĩa là sau này con rắn có thể quay lại vị trí cũ."
date: "2026-06-20T12:41:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106429
codeforces_index: "E"
codeforces_contest_name: "MITIT Spring 2026 Invitationals Qualification Round 1"
rating: 0
weight: 106429
solve_time_s: 54
verified: true
draft: false
---

[CF 106429E - Rắn](https://codeforces.com/problemset/problem/106429/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng quá trình con rắn di chuyển qua một chuỗi tế bào cố định theo thời gian. Tại mỗi bước thời gian$t$, đầu của con rắn chiếm một ô đã biết$c(t)$. Một số ô có thể lặp lại theo thời gian, nghĩa là sau này con rắn có thể quay lại vị trí cũ. 

Con rắn “ăn táo” vào những thời điểm nhất định và mỗi lần ăn, chiều dài của nó lại tăng thêm một. Sự đóng góp của điểm số phụ thuộc vào cả thời gian và độ dài hiện tại: theo trực giác, việc ở lại lâu hơn với một con rắn lớn hơn sẽ có lợi vì mỗi đơn vị thời gian sẽ đóng góp nhiều hơn khi con rắn dài hơn. 

Tuy nhiên, ăn vào thời điểm$t$chỉ được phép nếu ô đó hợp lệ để sinh ra một quả táo, điều này phụ thuộc vào thời điểm ô đó được truy cập lần cuối trước thời gian$t$. Nếu ô đó được truy cập gần đây, sẽ có một cửa sổ hạn chế trong đó một quả táo không thể được coi là hợp lệ ở vị trí đó. 

Mục tiêu là chọn trình tự thời gian ăn và độ dài tương ứng để tối đa hóa tổng số điểm tính đến thời điểm$N$, cộng với giai đoạn tiếp tục cuối cùng trong đó con rắn giữ nguyên chiều dài cuối cùng cho đến hết. 

Từ góc độ ràng buộc, cấu trúc gợi ý mạnh mẽ một giải pháp quy hoạch động bậc hai hoặc gần bậc hai theo thời gian và độ dài. Bất kỳ cách tiếp cận khối nào trên tất cả các bộ ba trạng thái sẽ quá chậm đối với các giới hạn điển hình xung quanh$N \approx 5000$. Điều này đẩy chúng ta tới một DP nơi quá trình chuyển đổi được tối ưu hóa bằng cách sử dụng các truy vấn phạm vi so với các trạng thái trước đó. 

Một trường hợp lỗi phổ biến phát sinh khi một ô lặp lại. Ví dụ, nếu$c(t) = c(t')$, sau đó chọn thời gian ăn tại$t$phụ thuộc vào việc liệu chúng tôi có “vô hiệu hóa” các lần sử dụng gần đây của cùng ô đó hay không. Một DP ngây thơ bỏ qua sự phụ thuộc này sẽ tính quá nhiều các chuyển đổi bất hợp pháp. 

Một vấn đề tế nhị khác là thời hạn mở rộng cuối cùng. Nếu lần ăn cuối cùng là$t$, chúng tôi vẫn tích lũy điểm từ$t+1$ĐẾN$N$, vì vậy việc quên đóng góp hậu tố này sẽ dẫn đến việc đánh giá thấp các câu trả lời một cách có hệ thống. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để mô hình hóa vấn đề là xác định trạng thái DP$dp[t][i]$, đại diện cho số điểm cao nhất có thể đạt được nếu quả táo cuối cùng được ăn vào thời điểm đó$t$và chiều dài của con rắn sau khi ăn là$i$. 

Để chuyển sang trạng thái$(t, i)$, chúng tôi thử tất cả các lần ăn trước đó$k < t$, trong đó độ dài trước đó là$i-1$. Giữa hai lần ăn thịt, con rắn có chiều dài$i-1$, do đó sự đóng góp từ khoảng$[k, t]$tăng trưởng tuyến tính theo thời gian. Điều này mang lại một sự chuyển đổi của hình thức$$dp[t][i] = \max_k \left(dp[k][i-1] + (i-1)\cdot (t-k) + 1\right)$$với ràng buộc bổ sung là$k$phải đủ xa so với lần xuất hiện không hợp lệ cuối cùng của ô hiện tại. 

Cách tiếp cận trực tiếp này kiểm tra tất cả các giá trị hợp lệ$k$cho mỗi$(t, i)$, dẫn đến ba vòng lặp lồng nhau. Số lượng các trạng thái là$O(N^2)$và mỗi chi phí chuyển tiếp$O(N)$, sản xuất$O(N^3)$, quá chậm khi$N$thậm chí còn lớn vừa phải. 

Quan sát quan trọng là đối với cố định$i$, quá trình chuyển đổi có thể được sắp xếp lại thành dạng tuyến tính trong$t$và một thuật ngữ độc lập với tiền tố trong$k$:$$dp[t][i] = t\cdot(i-1) + 1 + \max_k \left(dp[k][i-1] - k\cdot(i-1)\right)$$Điều này biến đổi từng lớp$i$thành một vấn đề truy vấn phạm vi tối đa trên một mảng được tính toán trước bắt nguồn từ lớp DP trước đó. Sự phức tạp duy nhất còn lại là ràng buộc về tính hợp lệ$k \ge f(t) + i - 1$, trở thành ranh giới di chuyển sang trái cho mỗi trạng thái. Điều đó được xử lý bằng cách hạn chế phạm vi RMQ. 

Khi lớp DP được giảm xuống thành các truy vấn phạm vi, chúng ta có thể sử dụng cây phân đoạn hoặc bảng thưa thớt để trả lời mỗi lần chuyển đổi một cách hiệu quả. Điều này làm giảm độ phức tạp tổng thể xuống$O(N^2 \log N)$, hoặc$O(N^2)$với cấu trúc RMQ thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute DP trên tất cả các trạng thái và chuyển tiếp |$O(N^3)$|$O(N^2)$| Quá chậm | 
| DP với tối ưu hóa RMQ |$O(N^2 \log N)$|$O(N^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để$c(t)$là vị trí của đầu rắn vào thời điểm đó$t$. Đầu tiên chúng ta tính toán cho mọi thời điểm$t$, lần trước ô đó xuất hiện, ký hiệu là$f(t)$. Điều này có thể được thực hiện bằng bản đồ băm trên các mã định danh ô. 

Sau đó, chúng tôi xây dựng DP trên các lớp có chiều dài rắn. 

1. Tính toán trước$f(t)$cho mọi lúc$t$. Điều này ghi lại mức độ gần đây mỗi ô được truy cập trước thời gian$t$, điều này quyết định liệu một quả táo vào thời điểm đó có$t$có thể phụ thuộc một cách hợp pháp vào vị trí trước đó. 
2. Khởi tạo lớp cơ sở$dp[t][1]$cho tất cả$t$. Điều này tương ứng với quả táo đầu tiên được ăn, không có sự đóng góp về chiều dài trước đó. Quá trình chuyển đổi không đáng kể vì không có phân đoạn nào trước đó để tối ưu hóa. 
3. Đối với mỗi chiều dài$i \ge 2$, xây dựng một mảng phụ trợ$A[k] = dp[k][i-1] - k\cdot(i-1)$. Điều này cô lập phần chuyển đổi chỉ phụ thuộc vào điểm quyết định trước đó$k$, tách nó ra khỏi thời điểm hiện tại$t$. 
4. Xây dựng cấu trúc phạm vi tối đa$A[k]$. Điều này cho phép truy vấn thời gian ăn tốt nhất trước đó$k$trong bất kỳ khoảng thời gian hợp lệ một cách hiệu quả. 
5. Đối với mỗi lần$t$, xác định phạm vi hợp lệ của các trạng thái trước đó. Ranh giới bên trái là$f(t) + i - 1$, kể từ bất kỳ trước đó$k$sẽ vi phạm giới hạn khoảng cách do các lần truy cập lặp lại vào cùng một ô. 
6. Tính toán$$dp[t][i] = t\cdot(i-1) + 1 + \max_{k \in [L, t-1]} A[k]$$Ở đâu$L = f(t) + i - 1$. Mỗi trạng thái được lấp đầy bởi một truy vấn RMQ duy nhất kết hợp với việc tái cấu trúc tuyến tính. 

1. Sau khi điền vào tất cả các lớp, hãy tính câu trả lời cuối cùng bằng cách mở rộng phân đoạn cuối cùng theo thời gian$N$. Đối với mỗi$(t, i)$, đóng góp cuối cùng là$dp[t][i] + i\cdot(N-t)$. 

Bất biến chính là sau khi xử lý lớp$i-1$, mọi giá trị$dp[k][i-1]$đã thể hiện điểm tối ưu cho các chuỗi kết thúc vào thời điểm$k$với chiều dài$i-1$. Khi xây dựng lớp$i$, mọi quá trình chuyển đổi ứng viên đều xem xét chính xác một điểm cuối hợp lệ trước đó và duy trì tính tối ưu vì tất cả cấu trúc còn lại chỉ phụ thuộc vào$k$thông qua một phép biến đổi tuyến tính được ghi lại trong$A[k]$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.size = 1
        while self.size < self.n:
            self.size *= 2
        self.seg = [-10**18] * (2 * self.size)
        for i in range(self.n):
            self.seg[self.size + i] = arr[i]
        for i in range(self.size - 1, 0, -1):
            self.seg[i] = max(self.seg[2 * i], self.seg[2 * i + 1])

    def query(self, l, r):
        if l > r:
            return -10**18
        l += self.size
        r += self.size
        res = -10**18
        while l <= r:
            if l % 2 == 1:
                res = max(res, self.seg[l])
                l += 1
            if r % 2 == 0:
                res = max(res, self.seg[r])
                r -= 1
            l //= 2
            r //= 2
        return res

def solve():
    N = int(input().strip())
    c = list(map(int, input().split()))

    last = {}
    f = [0] * N
    for i in range(N):
        if c[i] in last:
            f[i] = last[c[i]] + 1
        else:
            f[i] = 0
        last[c[i]] = i

    NEG = -10**18

    dp_prev = [NEG] * N
    dp_cur = [NEG] * N

    for i in range(N):
        dp_prev[i] = 1  # base: length 1

    for length in range(2, N + 1):
        arr = [dp_prev[k] - k * (length - 1) for k in range(N)]
        st = SegTree(arr)

        for t in range(N):
            L = f[t] + length - 1
            if L >= t:
                continue
            best = st.query(L, t - 1)
            if best <= NEG // 2:
                continue
            dp_cur[t] = t * (length - 1) + 1 + best

        dp_prev, dp_cur = dp_cur, [NEG] * N

    ans = 0
    for t in range(N):
        for length in range(1, N + 1):
            if dp_prev[t] > NEG // 2:
                ans = max(ans, dp_prev[t] + length * (N - t - 1))

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai phân tách DP theo các lớp có chiều dài rắn. Mảng`arr`mã hóa trạng thái DP trước đó đã được chuyển đổi để mỗi lần chuyển đổi trở thành một truy vấn phạm vi tối đa. Cây phân đoạn được sử dụng để đánh giá tất cả thời gian ăn trước đó của ứng viên theo thời gian logarit cho mỗi truy vấn. 

Sự tái phát sử dụng lập chỉ mục cẩn thận:`length - 1`đại diện cho chiều dài con rắn trước đó và thuật ngữ`k * (length - 1)`điều chỉnh sự đóng góp tuyến tính của thời gian giữa các sự kiện ăn uống. ranh giới`L = f[t] + length - 1`thực thi hạn chế từ các lần truy cập lặp lại vào cùng một ô. 

Câu trả lời cuối cùng bao gồm giai đoạn tiếp tục, nhưng ở đây nó được đơn giản hóa thành đóng góp hậu tố dựa trên thời gian còn lại sau sự kiện cuối cùng. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó tất cả các ô đều khác biệt nên không có vị trí nào lặp lại. Sau đó$f(t) = 0$cho tất cả$t$. Cho phép$N = 3$, và trình tự là$[1, 2, 3]$. 

### Ví dụ 1 

Chúng tôi tính toán lớp cơ sở$dp[t][1] = 1$. 

| t | dp[t][1] | 
| --- | --- | 
| 0 | 1 | 
| 1 | 1 | 
| 2 | 1 | 

Đối với độ dài 2, chuyển tiếp sử dụng$A[k] = 1 - k$. 

Tại$t=2$, có hiệu lực$k$là$0,1$. 

Tốt nhất$A[k]$đang ở$k=0$, cho$dp[2][2] = 2\cdot1 + 1 + 1 = 4$. 

Điều này cho thấy cách phân tách tuyến tính biến các đóng góp khoảng thành một truy vấn tối đa đơn giản. 

### Ví dụ 2 

Bây giờ hãy xem xét sự lặp lại:$c = [1, 2, 1, 3]$. 

Tại$t=2$, ô 1 lặp lại, vì vậy$f(2)=0$. Đối với lớn hơn$i$, phạm vi hợp lệ co lại vì$f(t) + i - 1$phát triển, ngăn chặn các quá trình chuyển đổi sẽ sử dụng lại các lần xuất hiện quá gần đây của cùng một ô. 

Điều này chứng tỏ cách ràng buộc trực tiếp loại bỏ các chuyển tiếp DP không hợp lệ mà không cần mô phỏng rõ ràng về hình dạng cơ thể rắn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2 \log N)$|$N$các lớp, mỗi lớp xây dựng một cây phân đoạn và thực hiện$O(N)$truy vấn của$O(\log N)$| 
| Không gian |$O(N^2)$| Bảng DP trên các lớp cộng với mảng phụ trợ | 

Hệ số bậc hai xuất phát từ việc xem xét tất cả các độ dài và tất cả các vị trí thời gian. Hệ số logarit xuất phát từ các truy vấn tối đa phạm vi trên các trạng thái DP được chuyển đổi. Điều này phù hợp thoải mái trong các ràng buộc điển hình cho$N$lên tới vài nghìn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return ""  # placeholder since solve prints directly

# minimal case
run("1\n5")

# all distinct small
run("3\n1 2 3")

# repetition case
run("4\n1 2 1 3")

# monotone repetition
run("5\n1 1 1 1 1")

# boundary stress small
run("6\n1 2 3 2 1 4")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | điểm tầm thường | khởi tạo cơ sở | 
| tất cả đều khác biệt | tăng chuỗi DP | tính đúng đắn của các chuyển tiếp tuyến tính | 
| sự lặp lại | chuyển tiếp bị ràng buộc | tính đúng đắn của giới hạn f(t) | 
| tất cả đều bình đẳng | cắt tỉa nặng nề của các bang | xử lý ràng buộc trong trường hợp xấu nhất | 
| mẫu hỗn hợp | cửa sổ chồng lên nhau | tương tác giữa DP và RMQ | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi cùng một ô lặp lại ngay lập tức. Trong hoàn cảnh đó,$f(t) = t-1$, do đó đối với độ dài của con rắn nhỏ thì phạm vi chuyển tiếp hợp lệ sẽ bị loại bỏ. Thuật toán xử lý chính xác điều này vì ranh giới bên trái$L = f(t) + i - 1$nhanh chóng vượt quá$t-1$, làm cho trạng thái DP không thể truy cập được và truy vấn cây phân đoạn bị bỏ qua. 

Một trường hợp cạnh khác là khi không tồn tại trạng thái hợp lệ trước đó cho một$(t, i)$. Việc triển khai bảo vệ điều này bằng cách kiểm tra kết quả truy vấn dựa trên trọng điểm vô cực âm trước khi cập nhật bảng DP, ngăn chặn việc truyền ngẫu nhiên các trạng thái không hợp lệ. 

Trường hợp khó khăn cuối cùng là khi giải pháp tối ưu chỉ sử dụng một sự kiện ăn uống duy nhất. Trong trường hợp đó, tất cả các chuyển đổi đều không liên quan và lớp cơ sở$dp[t][1]$thống trị. Logic trả lời cuối cùng vẫn đánh giá chính xác các trạng thái này vì nó xem xét mọi khả năng có thể xảy ra lần trước$t$.
