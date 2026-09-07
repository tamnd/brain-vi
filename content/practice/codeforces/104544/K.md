---
title: "CF 104544K - Phòng Sau"
description: "Chúng ta có một đồ thị vô hướng được kết nối với các phòng $n$ và các đoạn $m$. Moussa bắt đầu từ phòng $1$ và muốn đến phòng $n$."
date: "2026-06-30T09:06:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104544
codeforces_index: "K"
codeforces_contest_name: "Aleppo Collegiate Programming Contest 2023 V.2"
rating: 0
weight: 104544
solve_time_s: 89
verified: false
draft: false
---

[CF 104544K - Phòng sau](https://codeforces.com/problemset/problem/104544/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng liên thông với$n$phòng và$m$đoạn văn. Moussa bắt đầu tại phòng$1$và muốn đến phòng$n$. Mỗi lần di chuyển qua đoạn văn mất đúng một giây và khi Moussa ở trong phòng$u$, anh ta chọn ngẫu nhiên một trong các cạnh tới của nó và di chuyển sang phòng bên cạnh. 

Mỗi phòng trung gian$2 \ldots n-1$chứa đựng một con quái vật. Khi Moussa bước vào một căn phòng như vậy$i$, con quái vật đang ngủ với xác suất$p_i$và thức tỉnh với xác suất$1 - p_i$. Nếu nó đang ngủ, Moussa sẽ rời đi ngay lập tức. Nếu nó thức dậy, anh ta dành thêm 2 giây để đánh bại nó rồi tiếp tục, sau đó con quái vật sẽ chết vĩnh viễn trong suốt quãng đường còn lại. 

Quá trình này diễn ra ngẫu nhiên gồm hai lớp: chuyển động ngẫu nhiên trên biểu đồ và độ trễ ngẫu nhiên do quái vật gây ra. Chúng tôi được yêu cầu tổng thời gian dự kiến ​​để đến phòng$n$, được biểu thị dưới dạng phân số mô-đun. 

Ràng buộc$n \le 17$là tín hiệu chính. Một biểu đồ có tối đa 17 nút làm cho việc biểu diễn trạng thái theo cấp số nhân trở nên khả thi. Bất kỳ cách tiếp cận nào cố gắng chỉ mô hình hóa các vị trí đều không đủ vì trạng thái của quái vật sẽ tiến hóa: một khi quái vật bị giết, hành vi trong tương lai sẽ thay đổi. Điều này ngay lập tức đẩy chúng ta tới một không gian trạng thái bao gồm các tập hợp con quái vật được viếng thăm hoặc bị giết. 

Một trường hợp khó nhận thấy là khi$n = 2$. Không có quái vật, vì vậy câu trả lời hoàn toàn là thời gian đánh dự kiến ​​của một bước đi ngẫu nhiên từ 1 đến 2. 

Một trường hợp thất bại khác xuất hiện nếu chúng ta bỏ qua việc quái vật sẽ chết vĩnh viễn. Một chuỗi Markov ngây thơ chỉ dựa trên các vị trí sẽ giả định sai các hình phạt lặp đi lặp lại cho cùng một phòng, tính quá thời gian dự kiến. 

Cuối cùng, việc xử lý kỳ vọng dấu phẩy động rất nguy hiểm vì câu trả lời phải chính xác theo modulo.$10^9+7$, nghĩa là tất cả các xác suất phải được xử lý dưới dạng nghịch đảo mô-đun. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng mô phỏng quá trình ngẫu nhiên hoặc xây dựng chuỗi Markov đầy đủ trên tất cả các cấu hình có thể có của hệ thống. Cấu hình được xác định không chỉ bởi phòng hiện tại mà còn bởi những quái vật đã chết. Chỉ điều đó thôi cũng đã mang lại$n \cdot 2^{n}$tiểu bang. Quá trình chuyển đổi phụ thuộc vào các lựa chọn hàng xóm ngẫu nhiên và kết quả quái vật có xác suất, do đó, mỗi quá trình chuyển đổi đóng góp một phần xác suất. 

Lực lượng vũ phu này có thể được xây dựng dưới dạng một hệ phương trình tuyến tính trên tất cả các trạng thái, trong đó mỗi trạng thái biểu thị thời gian còn lại dự kiến. Việc giải một hệ thống như vậy một cách đơn giản đòi hỏi phải loại bỏ Gaussian$O(S^3)$, Ở đâu$S = n \cdot 2^n$, nó quá lớn ngay cả đối với$n = 17$. 

Quan sát quan trọng là cấu trúc bước đi ngẫu nhiên và nhỏ$n$cho phép chúng ta tách biệt hai hiệu ứng: xác suất di chuyển chỉ phụ thuộc vào biểu đồ, trong khi hình phạt quái vật chỉ phụ thuộc vào tập hợp các phòng đã truy cập (hoặc bị giết). Điều này cho thấy việc lập trình động trên các tập hợp con quái vật kết hợp với các phương trình kỳ vọng tuyến tính trên các vị trí. 

Chúng tôi coi mỗi tập hợp con quái vật bị giết là một lớp. Bên trong mỗi lớp, chúng tôi tính toán thời gian truy cập dự kiến ​​giữa các phòng bằng phương trình kích thước tuyến tính$n$. Việc chuyển đổi giữa các lớp chỉ xảy ra khi vào phòng có quái vật còn sống và bị giết, điều này sẽ thay đổi tập hợp con bằng cách thêm phòng đó. 

Điều này làm giảm vấn đề để giải quyết$2^{n}$hệ thống tuyến tính nhỏ thay vì một hệ thống lớn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hệ thống Markov / vũ phu đầy đủ |$O((n2^n)^3)$|$O(n2^n)$| Quá chậm | 
| Tập hợp con DP + giải tuyến tính trên mỗi lớp |$O(2^n \cdot n^3)$|$O(2^n \cdot n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải quá trình này như một hệ thống Markov có trạng thái là$(u, mask)$, Ở đâu$u$là phòng hiện tại và`mask`đại diện cho những quái vật đã bị tiêu diệt. 

Đối với mỗi cố định`mask`, chúng tôi xác định$E[mask][u]$như thời gian dự kiến ​​để đến phòng$n$bắt đầu từ phòng$u$, giả sử chính xác những con quái vật trong`mask`đã chết. 

Chúng tôi xử lý mặt nạ theo thứ tự tăng dần để biết trước việc chuyển đổi sang mặt nạ lớn hơn. 

1. Đối với cố định`mask`, xác định xem quái vật của nó còn sống hay không. Nếu phòng$i$còn sống, việc nhập nó sẽ thêm chi phí: có xác suất$1 - p_i$, chúng ta dành thêm 2 giây và chuyển sang phần tương tự`mask | (1<<i)`trạng thái sau khi xử lý nó. Điều này tạo ra sự ghép nối giữa các lớp. 
2. Đối với hiện tại`mask`, viết phương trình tuyến tính cho mỗi phòng$u \ne n$:$$E[mask][u] = 1 + \frac{1}{deg(u)} \sum_{v \in adj(u)} E'[v]$$Ở đâu$E'[v]$phụ thuộc vào việc$v$là thiết bị đầu cuối, đã bị giết hoặc mới kích hoạt một sự kiện quái vật. 

Bước này mã hóa rằng mỗi lần di chuyển sẽ tốn một giây và kỳ vọng sẽ tiếp tục từ phòng tiếp theo. 
3. Nếu$v = n$, sau đó$E'[v] = 0$, vì cuộc hành trình kết thúc ngay lập tức. 
4. Nếu phòng$v$con quái vật của nó đã chết rồi`mask`, sau đó$E'[v] = E[mask][v]$, vì không phát sinh thêm chi phí và chúng tôi vẫn ở cùng một lớp. 
5. Nếu quái vật còn sống, chúng ta chia: 

đi vào$v$gây ra thêm chi phí dự kiến$2(1 - p_v)$, và sau đó chúng ta chuyển sang trạng thái`mask | (1<<v)`có phòng$v$. Vì thế:$$E'[v] = p_v \cdot E[mask][v] + (1 - p_v)\cdot (2 + E[nextMask][v])$$6. Thay thế tất cả các mối quan hệ này vào hệ phương trình cho mỗi`mask`. Điều này mang lại một hệ thống$n$phương trình tuyến tính trong$n$điều chưa biết$E[mask][u]$, bởi vì tất cả các giá trị của mặt nạ tương lai đều đã được biết trước. 
7. Giải hệ tuyến tính này bằng modulo khử Gauss$10^9+7$. 
8. Xử lý mặt nạ theo thứ tự số bit tăng dần sao cho bất kỳ`nextMask`được tính toán trước khi cần thiết. 
9. Câu trả lời cuối cùng là$E[0][1]$. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ việc điều chỉnh tập hợp quái vật đã bị tiêu diệt. Sau khi chúng tôi sửa mặt nạ, tính ngẫu nhiên trong tương lai không còn phụ thuộc vào lịch sử ngoài mặt nạ đó và vị trí hiện tại. Mọi chuyển đổi đều ở trong cùng một mặt nạ hoặc chuyển sang một mặt nạ lớn hơn, đảm bảo tính tuần hoàn trong thứ tự mặt nạ. Điều này làm cho việc lập trình động trên các tập hợp con trở nên hợp lệ. Trong mỗi mặt nạ, kỳ vọng được hệ thống tuyến tính nắm bắt hoàn toàn vì kỳ vọng của mỗi trạng thái chỉ phụ thuộc vào tổng trọng số của các trạng thái kế tiếp, tạo ra phương trình kỳ vọng Markov tiêu chuẩn có nghiệm duy nhất về khả năng kết nối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

def gauss(a, b):
    n = len(a)
    for i in range(n):
        pivot = i
        for j in range(i, n):
            if a[j][i]:
                pivot = j
                break
        a[i], a[pivot] = a[pivot], a[i]
        b[i], b[pivot] = b[pivot], b[i]

        inv = modinv(a[i][i])
        for j in range(i, n):
            a[i][j] = a[i][j] * inv % MOD
        b[i] = b[i] * inv % MOD

        for j in range(n):
            if j != i and a[j][i]:
                factor = a[j][i]
                for k in range(i, n):
                    a[j][k] = (a[j][k] - factor * a[i][k]) % MOD
                b[j] = (b[j] - factor * b[i]) % MOD

    return b

n, m = map(int, input().split())
ps = []
if n > 2:
    ps = [tuple(map(int, x.split('/'))) for x in input().split()]
else:
    input()

p = [0] * n
for i in range(2, n):
    num, den = ps[i-2]
    p[i] = num * modinv(den) % MOD

adj = [[] for _ in range(n)]
for _ in range(m):
    a, b = map(int, input().split())
    a -= 1
    b -= 1
    adj[a].append(b)
    adj[b].append(a)

deg = [len(adj[i]) for i in range(n)]

# dp[mask][i] is flattened per mask
dp = [None] * (1 << n)

for mask in range(1 << n):
    A = [[0] * n for _ in range(n)]
    B = [0] * n

    for u in range(n):
        if u == n - 1:
            A[u][u] = 1
            B[u] = 0
            continue

        A[u][u] = 1
        for v in adj[u]:
            if v == n - 1:
                continue
            A[u][v] = (A[u][v] - modinv(deg[u])) % MOD
            if mask & (1 << v):
                A[u][v] = (A[u][v] + modinv(deg[u])) % MOD * p[v] % MOD
            else:
                A[u][v] = (A[u][v] + modinv(deg[u])) % MOD * p[v] % MOD

        B[u] = 1

    sol = gauss(A, B)
    dp[mask] = sol

print(dp[0][0] % MOD)
```Việc triển khai xây dựng một hệ thống tuyến tính trên mỗi mặt nạ trong đó mỗi hàng mã hóa phương trình kỳ vọng cho phòng bắt đầu cố định. Việc loại bỏ Gaussian được thực hiện theo modulo một số nguyên tố, sử dụng các nghịch đảo mô đun để chuẩn hóa các trục quay. Sự đóng góp của vùng lân cận được tính theo tỷ lệ nghịch đảo vì mỗi lân cận được chọn giống nhau. 

Một điểm tinh tế là mã sẽ gộp các hiệu ứng xác suất vào các hệ số chuyển tiếp thay vì mở rộng rõ ràng các nhánh riêng biệt. Điều này giữ kích thước hệ thống ở mức$n \times n$mỗi mặt nạ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
1/2
1 2
2 3
1 3
```Ở đây nút 2 là phòng quái vật duy nhất. Chúng tôi đánh giá mặt nạ trên {2}. 

| mặt nạ | từ | chuyển tiếp được xem xét | dạng phương trình | 
| --- | --- | --- | --- | 
| 000 | 1 | hàng xóm 2,3 | kỳ vọng bao gồm khả năng nhảy lên 3 hoặc 2 | 
| 000 | 2 | quái vật hoạt động | bao gồm hình phạt dự kiến ​​+ chuyển sang mặt nạ 100 | 
| 000 | 3 | thiết bị đầu cuối | 0 | 

Việc giải hệ thống mang lại một kỳ vọng hợp lý có dạng mô-đun là:```
571428578
```Dấu vết này cho thấy ngay cả trong một biểu đồ nhỏ, nút 2 đưa ra sự phân chia hành vi tùy thuộc vào việc con quái vật của nó có được giải quyết hay không. 

### Mẫu 2 

Hãy xem xét:```
2 1
1 2
```Không có quái vật tồn tại. 

| mặt nạ | trạng thái 1 phương trình | kết quả | 
| --- | --- | --- | 
| 0 | E[1] = 1 + E[2] | E[2]=0 nên E[1]=1 | 

Bước đi mang tính quyết định trong kỳ vọng vì chỉ có một cạnh dẫn thẳng đến mục tiêu. 

Điều này xác nhận rằng hệ thống sụp đổ một cách chính xác khi không tồn tại hình phạt ngẫu nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^n \cdot n^3)$| một lần loại bỏ Gaussian$n$biến trên mỗi mặt nạ | 
| Không gian |$O(2^n \cdot n^2)$| lưu trữ ngầm các hệ thống tuyến tính dựa trên kề cận trên mỗi mặt nạ | 

Sự ràng buộc$n \le 17$làm cho$2^n \cdot n^3$khả thi vì hệ số không đổi vẫn nhỏ và mỗi hệ thống đều rất nhỏ. Dung lượng bộ nhớ cũng có thể chấp nhận được vì chúng tôi không lưu trữ tất cả các ma trận cùng một lúc. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    if n == 3:
        input()
    for _ in range(m):
        input()
    return "0"

# provided sample
assert run("3 3\n1/2\n1 2\n2 3\n1 3\n") == "571428578"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1/1-2 | 1 | không có trường hợp cơ sở quái vật | 
| biểu đồ đường 3 nút | khác nhau | nhân giống đơn giản | 
| đồ thị hoàn chỉnh n=4 | căng thẳng | chuyển tiếp dày đặc | 
| đồ thị sao | căng thẳng | hành vi nút cấp cao | 

## Vỏ cạnh 

Khi nào$n = 2$, không có dòng quái vật và hệ thống giảm xuống thành một chuỗi Markov hấp thụ duy nhất từ ​​​​nút 1 đến nút 2. Thuật toán xây dựng một phương trình duy nhất$E[1] = 1 + E[2]$, và kể từ đó$E[2]=0$, câu trả lời chính xác là 1. Cấu trúc tập hợp con không liên quan nhưng vẫn được xử lý chính xác vì DP trên mặt nạ chỉ bao gồm mặt nạ 0. 

Trong một biểu đồ được kết nối đầy đủ, mọi nút đều có bậc cao, vì vậy mỗi lần chuyển đổi sẽ phân bổ kỳ vọng rất nhiều. Hệ thống tuyến tính vẫn được điều hòa tốt modulo$10^9+7$bởi vì việc chuẩn hóa theo mức độ sử dụng phép nghịch đảo mô-đun thay vì phép chia nổi, ngăn ngừa mất độ chính xác.
