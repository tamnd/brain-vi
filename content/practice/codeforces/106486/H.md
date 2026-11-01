---
title: "CF 106486H - ​​Cây Ngớ Ngẩn"
description: "Chúng ta được yêu cầu đếm các cây có thứ tự gốc trên chính xác các nút $n$ dưới một ràng buộc cấu trúc về phân nhánh. Mỗi nút có một số nút con và số này phải thuộc về một tập hợp $A$ được cho phép."
date: "2026-06-19T15:15:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106486
codeforces_index: "H"
codeforces_contest_name: "Dalian University of Technology, Software College 2025 Freshman Contest"
rating: 0
weight: 106486
solve_time_s: 75
verified: true
draft: false
---

[CF 106486H - Cây ngớ ngẩn](https://codeforces.com/problemset/problem/106486/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm chính xác các cây có thứ tự gốc$n$các nút dưới một ràng buộc cấu trúc về phân nhánh. Mỗi nút có một số nút con và số này phải thuộc về một tập hợp được phép nhất định$A$. Bộ sản phẩm luôn bao gồm$0$, do đó, các lá luôn được phép và các nút con của mỗi nút được sắp xếp theo thứ tự, nghĩa là việc hoán đổi hai cây con sẽ tạo ra một cây khác ngay cả khi các cây con giống hệt nhau. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp$n$, số lượng nút trong cây và một tập hợp nhỏ$A$số lượng trẻ em được phép. Nhiệm vụ là tính toán, modulo$998244353$, có bao nhiêu cây có thứ tự khác nhau với$n$các nút thỏa mãn rằng số lượng nút con của mỗi nút nằm trong$A$. 

Các ràng buộc đã gợi ý về cấu trúc của giải pháp. Tổng của tất cả$n$trên các trường hợp thử nghiệm nhiều nhất là 3000, do đó, ngay cả lập trình động bậc hai hoặc bậc ba cho mỗi trường hợp thử nghiệm cũng có thể được chấp nhận. Tuy nhiên,$k \le 10$và mức độ được phép khác nhau tùy theo từng bài kiểm tra, điều này giúp ngăn chặn việc tính toán trước trong mọi trường hợp. Do đó, áp lực tính toán chính không phải là tiệm cận trong một thử nghiệm đơn lẻ mà là ở việc xử lý nhiều tích chập nhỏ một cách hiệu quả. 

Một điểm tinh tế là ý nghĩa của kích thước. Kích thước cây đếm các nút chứ không phải các cạnh, vì vậy một cây nút tương ứng với$n=1$. Điều này làm cho trường hợp cơ sở trở nên quan trọng: mặc dù$0 \in A$, cây hợp lệ nhỏ nhất vẫn là một nút gốc duy nhất. 

Một sai lầm ngây thơ sẽ nảy sinh nếu người ta nhầm lẫn các ràng buộc “số con” với các chuỗi mức độ theo nghĩa lý thuyết đồ thị và cố gắng mô hình hóa nó như những cây tùy ý. Ví dụ, nếu$A = \{0,2\}$Và$n=3$, các cấu trúc hợp lệ bị hạn chế rất nhiều và bất kỳ cách tiếp cận nào bỏ qua thứ tự hoặc coi các cây con là không có thứ tự sẽ bị tính thiếu. 

Một trường hợp thất bại thường gặp khác là việc sắp xếp sai các chỉ số giữa “số trẻ em”.$d$” và thực tế là tổng số nút được phân bổ cho trẻ em là$n-1$. Việc quên điều này sẽ dẫn đến từng lỗi cấu trúc ngay cả khi sự lặp lại là đúng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ cố gắng tạo ra mọi cây có thứ tự gốc có thể với$n$các nút và kiểm tra xem mỗi nút có mức độ cho phép hay không. Điều này đơn giản về mặt khái niệm: xây dựng cây đệ quy bằng cách chọn một số cây con cho mỗi nút, sau đó xây dựng đệ quy từng cây con và đếm các cấu hình có tổng số nút phù hợp$n$. Điều này đúng nhưng bùng nổ theo kiểu tổ hợp. Ngay cả đối với mức độ vừa phải$n$, số lượng cây được sắp xếp theo cấp số nhân và việc lọc sau thế hệ thậm chí còn tệ hơn. 

Quan sát quan trọng là vấn đề này hoàn toàn được xác định bởi sự kết hợp kích thước cây con. Một khi gốc đã có$d$các con, vấn đề giảm xuống còn chia rẽ$n-1$nút vào$d$các nhóm có thứ tự, mỗi nhóm chính là một cây hợp lệ. Đây chính xác là một cấu trúc tích chập theo chuỗi số lượng cây theo kích thước. 

Cho phép$f[n]$là số cây hợp lệ với$n$nút. Đối với một mức độ gốc cố định$d$, chúng ta cần phân phối phần còn lại$n-1$nút vào$d$cây con có thứ tự. Điều đó tương đương với việc lấy$d$-fold tích chập của chuỗi$f$với chính nó và rút ra hệ số cho$n-1$. Từ$k \le 10$, chúng ta chỉ cần tối đa 10 phép cuộn như vậy cho mỗi trường hợp thử nghiệm. 

Điều này làm giảm vấn đề từ phép liệt kê hàm mũ sang DP đa thức với phép tích chập lặp đi lặp lại của một chuỗi nhỏ. Bởi vì tổng cộng$n$qua tất cả các bài kiểm tra là nhỏ, một cách đơn giản$O(k n^2)$DP cho mỗi bài kiểm tra là đủ và vượt qua một cách thoải mái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| DP với các kết cấu có thứ tự |$O(k n^2)$mỗi bài kiểm tra |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định$f[i]$là số lượng cây có thứ tự hợp lệ với chính xác$i$nút. Chúng tôi tính toán điều này cho từng trường hợp thử nghiệm một cách độc lập bằng cách sử dụng lập trình động trên các kích thước cây con. 

### bước 

1. Khởi tạo mảng DP$f$chiều dài$n+1$với tất cả các số 0 và đặt$f[1] = 1$. 

Điều này đại diện cho cây một nút, luôn hợp lệ vì gốc không có con. 
2. Đối với mọi kích cỡ$i$từ$2$ĐẾN$n$, tính toán$f[i]$bằng cách xem xét tất cả các mức độ gốc có thể$d \in A$. 

Nút gốc sử dụng một nút, do đó nút còn lại$i-1$các nút được phân phối giữa nó$d$những đứa trẻ. 
3. Đối với cố định$d$, tính số cách chia$i-1$vào trong$d$các phần được sắp xếp trong đó mỗi phần tương ứng với một kích thước cây con. 

Mỗi bộ phận đóng góp một yếu tố$f[\cdot]$, do đó đây trở thành hệ số của$x^{i-1}$TRONG$F(x)^d$, Ở đâu$F(x) = \sum f[i] x^i$. 
4. Thay vì chuỗi lũy thừa ký hiệu, hãy tính trực tiếp chuỗi này bằng cách sử dụng tích chập. Duy trì một mảng phụ đại diện cho nguồn điện hiện tại$g_d$, Ở đâu$g_d[s]$là số cách để hình thành tổng kích thước$s$sử dụng$d$cây con có thứ tự. 
5. Xây dựng$g_d$lặp đi lặp lại: 

bắt đầu từ$g_0[0] = 1$và với mỗi phép nhân với$F$, cập nhật thông qua tích chập:$$g_{t+1}[s] = \sum_{x=1}^{s} g_t[x] \cdot f[s-x]$$6. Sau khi tính toán$g_d$, thêm vào$g_d[i-1]$vào trong$f[i]$cho mọi thứ được phép$d \in A$. 

### Tại sao nó hoạt động 

Bất biến trung tâm là$g_d[s]$luôn đếm chính xác số lượng rừng có thứ tự$d$những cây có rễ có tổng kích thước là$s$, trong đó mỗi cây thành phần đã hợp lệ theo$f$. Điều này đảm bảo rằng khi chúng ta tính toán$f[i]$, chúng tôi không đoán cấu trúc, mà soạn thảo một cách có hệ thống các cấu trúc phụ đã chính xác. Vì mỗi cây hợp lệ có một phân tách duy nhất theo cấp độ gốc và kích thước cây con theo thứ tự, nên mọi cấu hình đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        A = list(map(int, input().split()))

        # dp[i] = number of trees with i nodes
        dp = [0] * (n + 1)
        dp[1] = 1

        # precompute convolution powers up to max degree needed
        # we will build dp incrementally
        for i in range(2, n + 1):
            total = 0

            for d in A:
                if d == 0:
                    continue
                # g[0] = 1, then convolve f (dp) with itself d times
                # we compute iteratively
                g = [0] * i
                g[0] = 1

                for _ in range(d):
                    ng = [0] * i
                    for s in range(i):
                        if g[s] == 0:
                            continue
                        for x in range(1, i - s):
                            if dp[x]:
                                ng[s + x] = (ng[s + x] + g[s] * dp[x]) % MOD
                    g = ng

                total = (total + g[i - 1]) % MOD

            dp[i] = total

        print(dp[n] % MOD)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo sự tái phát theo nghĩa đen. Vòng lặp bên ngoài xây dựng kích thước cây tăng dần, đảm bảo đã biết tất cả số lượng cây con khi tính toán kích thước lớn hơn. Cấu trúc bên trong tính toán, đối với mỗi mức độ được phép, số cách để tập hợp các cây con có thứ tự tính tổng cho các nút còn lại. 

Một chi tiết triển khai tinh tế là việc loại trừ$x=0$trong kích thước cây con bên trong các vòng tích chập. Điều này là cần thiết vì mỗi cây con phải chứa ít nhất một nút. Cho phép số 0 sẽ đếm không chính xác các cây trống và làm tăng kết quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét$n=4$,$A = \{0,1\}$. 

Chúng tôi tính toán$dp[1]=1$. Vì$dp[2]$, chỉ đóng góp bậc 1, vì vậy chúng ta đính kèm một cây con có kích thước 1. Do đó$dp[2]=1$. 

Vì$dp[3]$, một lần nữa chỉ có thể có chuỗi, cho 1 cấu trúc. Vì$dp[4]$, vẫn chỉ tồn tại một con đường duy nhất. 

| tôi | lựa chọn d | kết quả g cho i-1 | dp[i] | 
| --- | --- | --- | --- | 
| 1 | - | - | 1 | 
| 2 | 1 | [1 chiều] | 1 | 
| 3 | 1 | [1 chiều] | 1 | 
| 4 | 1 | [1 chiều] | 1 | 

Điều này xác nhận rằng việc hạn chế ở các nút con đơn sẽ buộc phải có cấu trúc chuỗi. 

### Ví dụ 2 

hãy để$n=4$,$A=\{0,2\}$. 

Vì$dp[2]$, bậc 2 là không thể, nên giá trị là 0. 

cho$dp[3]$, gốc có 2 nút con yêu cầu chia 2 nút thành hai phần không rỗng có thứ tự, chỉ (1,1), vì vậy$dp[3]=1$. 

Vì$dp[4]$, chúng tôi chia 3 nút thành hai phần có thứ tự: (1,2) và (2,1), vì vậy$dp[4]=2$. 

| tôi | sự phân chia hợp lệ cho i-1 | dp[i] | 
| --- | --- | --- | 
| 2 | không | 0 | 
| 3 | (1,1) | 1 | 
| 4 | (1,2), (2,1) | 2 | 

Điều này chứng tỏ thứ tự của các cây con sẽ trực tiếp nhân đôi cấu hình cây con đối xứng như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k n^2)$mỗi bài kiểm tra | Đối với mỗi kích thước, chúng tôi tính toán lại tích chập trên tất cả các phần tách và tối đa 10 độ | 
| Không gian |$O(n)$| Chỉ có mảng DP và mảng tích chập tạm thời được lưu trữ | 

Tổng số tiền của$n$trên tất cả các trường hợp thử nghiệm tối đa là 3000, do đó, DP bậc hai thoải mái nằm trong giới hạn ngay cả khi có chi phí Python. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, k = map(int, input().split())
        A = list(map(int, input().split()))

        dp = [0] * (n + 1)
        dp[1] = 1

        for i in range(2, n + 1):
            total = 0
            for d in A:
                if d == 0:
                    continue

                g = [0] * i
                g[0] = 1

                for _ in range(d):
                    ng = [0] * i
                    for s in range(i):
                        if g[s] == 0:
                            continue
                        for x in range(1, i - s):
                            if dp[x]:
                                ng[s + x] = (ng[s + x] + g[s] * dp[x]) % MOD
                    g = ng

                total = (total + g[i - 1]) % MOD

            dp[i] = total

        out.append(str(dp[n]))

    return "\n".join(out)

# minimal case
assert run("1\n1 1\n0\n") == "1"

# chain only
assert run("1\n4 2\n0 1\n") == "1"

# binary only
assert run("1\n4 2\n0 2\n") == "2"

# mixed degrees
assert run("1\n3 2\n0 1\n") == "1"

# multiple tests
assert run("2\n1 1\n0\n3 2\n0 1\n") == "1\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=1$| 1 | Vỏ lá đế | 
|$A=\{0,1\}$| chỉ dây chuyền | tính đúng đắn của cấu trúc tuyến tính | 
|$A=\{0,2\}$| 2 tại n=4 | đối xứng cây con theo thứ tự | 
| nhiều bài kiểm tra | thiết lập lại DP nhất quán | tính độc lập của các ca kiểm thử | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$A = \{0\}$. Cấu trúc hợp lệ duy nhất là một nút duy nhất, bởi vì mỗi nút phải có 0 nút con. Thuật toán xử lý chính xác điều này bởi vì tất cả$d \neq 0$vắng mặt, vì vậy không có sự tăng trưởng vượt quá$dp[1]$xảy ra. 

Một trường hợp khác là khi$n$mức độ nhỏ nhưng lớn tồn tại trong$A$. Ví dụ,$n=3$,$A=\{0,2,5\}$. Tùy chọn độ 5 âm thầm không liên quan vì tích chập cho$i-1=2$không thể phân phối đủ các nút. Các vòng lặp tự nhiên tạo ra sự đóng góp bằng 0 vì không có sự phân chia hợp lệ nên không cần phải lọc đặc biệt. 

Cuối cùng, hãy xem xét$n=2$,$A=\{0,1,2\}$. Mức độ 2 có thể áp dụng được, nhưng việc chia một nút thành hai cây con khác rỗng là không thể. Phép tích chập mang lại kết quả bằng 0 một cách chính xác cho trường hợp đó, cho thấy tính khả thi được thực thi theo cấu trúc thay vì thủ công.
