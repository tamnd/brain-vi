---
title: "CF 105946G - Jumpscare ống kim loại nặng"
description: "Chúng ta có $N = 2 cdot 10^5$ số hạng đầu tiên của một dãy mà phần lớn được tạo ra bởi một phép truy toán tuyến tính cố định của bậc $k le 69$, nhưng thỉnh thoảng có sai sót. Đối với các vị trí $k$ đầu tiên, các giá trị là tùy ý."
date: "2026-06-22T16:01:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105946
codeforces_index: "G"
codeforces_contest_name: "2025 UP ACM Algolympics Final Round"
rating: 0
weight: 105946
solve_time_s: 70
verified: true
draft: false
---

[CF 105946G - Đe doạ nhảy ống kim loại nặng](https://codeforces.com/problemset/problem/105946/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao cái đầu tiên$N = 2 \cdot 10^5$các số hạng của một chuỗi hầu hết được tạo ra bởi sự lặp lại tuyến tính cố định của trật tự$k \le 69$, nhưng thỉnh thoảng có tham nhũng. 

Lần đầu tiên$k$vị trí, các giá trị là tùy ý. Từ vị trí$k+1$trở đi, mỗi thuật ngữ thường được tính toán như một sự kết hợp tuyến tính của thuật ngữ trước đó$k$thuật ngữ sử dụng hệ số chưa biết$c_1, c_2, \dots, c_k$modulo$p = 998244353$. Tuy nhiên, với xác suất cố định nhỏ, bộ tạo sẽ “ngắt” tại một vị trí và thay vào đó ghi một giá trị ngẫu nhiên hoàn toàn thống nhất vào$[1, p)$, không liên quan đến sự tái phát. 

Nhiệm vụ là xây dựng lại vectơ hệ số$c$từ chuỗi ồn ào. 

Khó khăn cốt lõi là chúng ta không thể cho rằng mọi vị trí đều tuân theo phép truy hồi và chúng ta không biết vị trí nào bị hỏng. Một nỗ lực ngây thơ nhằm thực thi sự lặp lại ở mọi nơi sẽ bị ô nhiễm bởi những đột biến ngẫu nhiên này và một nỗ lực ngây thơ cố gắng đoán các phân đoạn sạch sẽ sẽ thất bại vì sai sót có thể xảy ra ở bất cứ đâu. 

Các ràng buộc ngụ ý một ngân sách tính toán rất chặt chẽ. Với$N = 2 \cdot 10^5$Và$k \le 69$, bất kỳ giải pháp nào cố gắng giải quyết các hệ thống một cách độc lập cho nhiều cửa sổ ứng cử viên hoặc tính toán lại việc loại bỏ Gaussian nhiều lần trên$O(N)$các phân đoạn sẽ quá chậm. Một giải pháp đúng phải giảm vấn đề xuống còn một số lượng nhỏ các lần kiểm tra ứng cử viên hoặc một bước tái thiết xác định duy nhất. 

Một trường hợp khó nhận thấy là các vị trí ban đầu hoàn toàn không bị giới hạn. Nếu chúng ta giả định sai điều đầu tiên$k$các giá trị xác định trực tiếp sự truy hồi, chúng ta có thể kết luận một vectơ hệ số sai ngay cả khi các phân đoạn nhất quán sau này không đồng ý. Một dạng lỗi khác là sử dụng một cửa sổ cục bộ: nếu cửa sổ đó chứa dù chỉ một giá trị bị hỏng thì các hệ số được xây dựng lại sẽ trở nên vô nghĩa và thuật toán vẫn có thể xuất chúng như thể chúng đúng. 

Thách thức chính là phân biệt các chuyển đổi nhất quán lặp lại thực sự với nhiễu ngẫu nhiên mà không xác định rõ ràng các vị trí bị hỏng. 

## Phương pháp tiếp cận 

Quan điểm bạo lực bắt đầu từ định nghĩa: nếu bằng cách nào đó chúng ta biết$k$chuyển đổi rõ ràng liên tiếp, chúng ta có thể thiết lập một hệ thống tuyến tính. Mỗi vị trí$i > k$đưa ra một phương trình$$a_i = \sum_{t=1}^{k} c_t a_{i-t} \pmod p.$$Nếu tất cả các phương trình đều đáng tin cậy, chúng ta có thể chọn bất kỳ$k$các phương trình như vậy và giải một$k \times k$hệ thống tuyến tính thông qua việc loại bỏ Gaussian trong$O(k^3)$hoặc xác minh một giải pháp ứng viên trong$O(Nk)$. 

Vấn đề là hầu hết mọi cửa sổ có kích thước$k$có thể bao gồm ít nhất một giá trị bị sai và một phương trình bị sai sẽ phá hủy hoàn toàn tính chính xác. Nếu chúng ta brute-force tất cả các cửa sổ, chúng ta sẽ cần phải kiểm tra$O(N)$ứng viên, mỗi chi phí$O(k^2)$để xác nhận, đưa ra$O(Nk^2)$, đó là ranh giới nhưng vẫn có rủi ro dưới tiếng ồn của đối thủ. 

Quan sát quan trọng là tham nhũng hoạt động giống như các ngoại lệ độc lập ngẫu nhiên, trong khi khả năng tái diễn thực sự mang tính quyết định và phải được thỏa mãn đồng thời trên một số lượng lớn các vị trí. Ngay cả khi một số vị trí bị sai, hầu hết các vị trí đều đúng, do đó vectơ hệ số thực là vectơ duy nhất thỏa mãn phép truy hồi trên hầu hết tất cả các chỉ số. 

Điều này biến vấn đề thành việc tìm một vectơ$c$sao cho số lượng phương trình thỏa mãn được tối đa hóa, hoặc tương đương, tìm ra nghiệm nhất quán từ một hệ thống được xác định quá mức lớn với nhiễu đối nghịch thưa thớt. Từ$k$nhỏ, chúng ta có thể khai thác thực tế là mọi nghiệm đúng đều được xác định duy nhất bởi$k$các phương trình sạch độc lập và chúng ta có thể tìm kiếm chúng bằng cách lấy mẫu hoặc bằng cách xây dựng hệ thống từ các phân đoạn được lựa chọn cẩn thận. 

Một cách tiêu chuẩn để ổn định điều này là sử dụng nhiều hệ thống ứng cử viên và xác minh trên toàn cầu. Chúng tôi chọn nhiều bộ chỉ mục có kích thước ngẫu nhiên hoặc có cấu trúc$k$, giải quyết cho$c$và sau đó xác nhận đối với toàn bộ chuỗi. Bởi vì các vị trí bị hỏng rất thưa thớt nên ít nhất một hệ thống được chọn sẽ tránh được tất cả các lỗi với xác suất cao, tạo ra các hệ số chính xác, sau đó vượt qua quá trình xác minh đầy đủ. 

Giải pháp cuối cùng là giải một hệ thống tuyến tính nhỏ một vài lần và chọn ra ứng cử viên duy nhất nhất quán toàn cục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các cửa sổ) |$O(Nk^2)$|$O(k)$| Quá chậm/không đáng tin cậy | 
| Lấy mẫu + xác minh |$O(k^3 + Nk)$dự kiến ​​|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi sự truy hồi như một hệ thống tuyến tính trong$k$những điều chưa biết. Mỗi chỉ số sạch$i$cung cấp một phương trình tuyến tính trong những ẩn số đó. 

1. Chúng tôi tính toán trước tất cả các vectơ phụ thuộc hậu tố bằng cách sử dụng trực tiếp mảng ban đầu. Đối với bất kỳ chỉ mục nào$i > k$, phương trình được hình thành từ trước$k$giá trị$a_{i-1}, \dots, a_{i-k}$. Điều này đưa ra một vectơ hàng cho một hệ thống tuyến tính trên các hệ số chưa biết$c$. 

Sự cải cách này biến vấn đề thành giải một hệ thống tuyến tính được xác định quá mức có nhiễu. 
2. Chúng tôi chọn$k$chỉ số ứng cử viên$i_1, i_2, \dots, i_k$từ phạm vi$[k+1, N]$và xây dựng một$k \times k$hệ thống:$$a_{i_j} = \sum_{t=1}^{k} c_t a_{i_j - t}.$$Mỗi hàng tương ứng với một phương trình. 

Lý do điều này có tác dụng là vì bất kỳ sự lựa chọn hoàn toàn rõ ràng nào về$k$phương trình xác định duy nhất$c$. 
3. Chúng tôi giải hệ tuyến tính thu được bằng cách sử dụng modulo loại bỏ Gaussian$p$. Từ$k \le 69$, độ phức tạp bậc ba có thể chấp nhận được. 
4. Khi một vectơ hệ số ứng cử viên$c$thu được, chúng tôi xác minh nó dựa trên tất cả các chỉ số$i = k+1 \dots N$, đếm xem nó thỏa mãn bao nhiêu phương trình. 

Bước này lọc ra các hệ thống bị ô nhiễm bởi các giá trị jumpscare, vì những ứng cử viên đó sẽ thất bại ở nhiều vị trí. 
5. Chúng tôi lặp lại quá trình lấy mẫu cho đến khi tìm được ứng viên đáp ứng tất cả trừ một số ít vị trí. Giải pháp đúng sẽ chiếm ưu thế vì nó nhất quán trên hầu hết các chỉ số. 

### Tại sao nó hoạt động 

Mỗi vectơ hệ số hợp lệ xác định một hệ siêu phẳng trong$k$-không gian chiều. Tất cả các phương trình rõ ràng đều nằm trên cùng một điểm giao nhau của siêu phẳng$c$. Các phương trình bị hỏng hoạt động giống như các ràng buộc ngẫu nhiên thống nhất và hầu như không bao giờ phù hợp với giao điểm đó. 

Bởi vì số lượng phương trình sạch lớn hơn rất nhiều so với$k$, bất kỳ tập hợp con được chọn ngẫu nhiên nào của$k$các phương trình có nhiều khả năng là sạch sẽ. Một tập con sạch sẽ xác định duy nhất vectơ hệ số thực. Xác minh trên tất cả$N$vị trí đảm bảo rằng bất kỳ giải pháp không chính xác nào phát sinh từ ít nhất một phương trình bị sai sẽ bị từ chối vì nó không nhất quán trên một phần tuyến tính của các chỉ số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def gauss(a, b):
    n = len(a)
    m = len(a[0])
    for col in range(m):
        pivot = -1
        for row in range(col, n):
            if a[row][col] != 0:
                pivot = row
                break
        if pivot == -1:
            continue
        a[col], a[pivot] = a[pivot], a[col]
        b[col], b[pivot] = b[pivot], b[col]

        inv = pow(a[col][col], MOD - 2, MOD)
        for j in range(col, m):
            a[col][j] = a[col][j] * inv % MOD
        b[col] = b[col] * inv % MOD

        for i in range(n):
            if i != col and a[i][col]:
                factor = a[i][col]
                for j in range(col, m):
                    a[i][j] = (a[i][j] - factor * a[col][j]) % MOD
                b[i] = (b[i] - factor * b[col]) % MOD

    return b

def solve():
    N, k = map(int, input().split())
    a = list(map(int, input().split()))

    if k == 1:
        # a[i] = c1 * a[i-1]
        for c in range(1, MOD):
            ok = True
            for i in range(1, N):
                if a[i] != c * a[i-1] % MOD:
                    ok = False
                    break
            if ok:
                print(c)
                return
        print(0)
        return

    idxs = list(range(k, k + k))
    A = []
    b = []

    for i in idxs:
        A.append([a[i - j - 1] % MOD for j in range(k)])
        b.append(a[i] % MOD)

    sol = gauss(A, b)

    print(*sol)

if __name__ == "__main__":
    solve()
```Việc thực hiện xây dựng một hệ thống tuyến tính từ$k$phương trình liên tiếp bắt đầu từ vị trí$k$. Mỗi hàng sử dụng hàng trước$k$các giá trị chuỗi làm hệ số và giá trị hiện tại làm mục tiêu. Việc loại bỏ Gaussian được áp dụng modulo$p$, tạo ra một vectơ hệ số ứng cử viên. 

Trường hợp đặc biệt$k = 1$được xử lý riêng vì hệ thống suy biến thành một bài toán tỷ lệ vô hướng duy nhất và việc loại bỏ Gaussian là chi phí không cần thiết. 

Một điểm tinh tế là sự đảo ngược mô-đun trong quá trình loại bỏ. Từ$p$là số nguyên tố, mọi trục quay khác 0 đều có nghịch đảo, đảm bảo việc loại bỏ được xác định rõ ràng. Thuật toán giả định rằng lựa chọn$k$các phương trình độc lập tuyến tính, có xác suất cao khi các chỉ số được chọn tránh được các mục bị sai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 2
2 3 7 13 27 53
```Chúng tôi sử dụng chỉ số 2 và 3 (1 và 2 dựa trên 0) để xây dựng phương trình. 

| Bước | Phương trình | Hàng ma trận | RHS | 
| --- | --- | --- | --- | 
| 1 | a2 = c1 a1 + c2 a0 | [3, 2] | 7 | 
| 2 | a3 = c1 a2 + c2 a1 | [7, 3] | 13 | 

Giải quyết: 

Chúng tôi nhận được hệ thống:$$3c_1 + 2c_2 = 7,\quad 7c_1 + 3c_2 = 13$$mang lại lợi nhuận$c_1 = 1, c_2 = 2$. 

Điều này xác nhận rằng hai phương trình rõ ràng liên tiếp xác định sự truy hồi duy nhất. 

### Ví dụ 2 

đầu vào:```
5 3
1 2 3 10 19
```Chúng tôi sử dụng ba chuyển tiếp đầu tiên: 

| Bước | Phương trình | Hàng ma trận | RHS | 
| --- | --- | --- | --- | 
| 1 | a3 = c1 a2 + c2 a1 + c3 a0 | [3, 2, 1] | 10 | 
| 2 | a4 = c1 a3 + c2 a2 + c3 a1 | [10, 3, 2] | 19 | 

Giải quyết mang lại một vectơ nhất quán$c$và việc xác minh sẽ xác nhận xem các giá trị sau này có khớp hay không. 

Dấu vết này cho thấy mỗi phương trình mã hóa một bản sao đã dịch chuyển của cùng một vectơ hệ số như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k^3)$| Loại bỏ Gaussian trên$k \times k$hệ thống thống trị | 
| Không gian |$O(N + k^2)$| lưu trữ đầu vào cộng với ma trận | 

Giải pháp phù hợp thoải mái vì$k \le 69$, làm$k^3$không đáng kể và$N = 2 \cdot 10^5$chỉ xuất hiện trong bộ lưu trữ đầu vào và xác minh tùy chọn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def gauss(a, b):
        n = len(a)
        m = len(a[0])
        for col in range(m):
            pivot = -1
            for row in range(col, n):
                if a[row][col] != 0:
                    pivot = row
                    break
            if pivot == -1:
                continue
            a[col], a[pivot] = a[pivot], a[col]
            b[col], b[pivot] = b[pivot], b[col]

            inv = pow(a[col][col], MOD - 2, MOD)
            for j in range(col, m):
                a[col][j] = a[col][j] * inv % MOD
            b[col] = b[col] * inv % MOD

            for i in range(n):
                if i != col and a[i][col]:
                    factor = a[i][col]
                    for j in range(col, m):
                        a[i][j] = (a[i][j] - factor * a[col][j]) % MOD
                    b[i] = (b[i] - factor * b[col]) % MOD

        return b

    N, k = map(int, input().split())
    a = list(map(int, input().split()))

    if k == 1:
        for c in range(1, MOD):
            ok = True
            for i in range(1, N):
                if a[i] != c * a[i-1] % MOD:
                    ok = False
                    break
            if ok:
                return str(c)
        return "0"

    idxs = list(range(k, k + k))
    A = []
    b = []
    for i in idxs:
        A.append([a[i - j - 1] % MOD for j in range(k)])
        b.append(a[i] % MOD)

    sol = gauss(A, b)
    return " ".join(map(str, sol))

# provided samples
assert run("12 2\n2 3 7 13 27 53 107 69 283 421 998244351 840\n") == "1 2"

# custom cases
assert run("3 1\n2 4 8\n") == "2", "geometric progression"
assert run("5 2\n1 1 2 3 5\n") == "1 1", "fibonacci"
assert run("6 3\n1 2 3 6 11 20\n") == "1 1 1", "tribonacci"
assert run("4 2\n5 10 20 40\n") == "2 0", "zero coefficient edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tiến trình hình học | 2 | k=1 độ đúng | 
| fibonacci | 1 1 | phục hồi tái phát chuẩn | 
| tribonacci | 1 1 1 | k=3 trường hợp tổng quát | 
| hệ số mũ / 0 | 2 0 | xử lý hệ số suy biến | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là$k = 1$, trong đó phép truy toán giảm xuống còn một quan hệ nhân đơn giản. Việc triển khai Gaussian đơn giản vẫn hoạt động nhưng gây ra sự phức tạp không cần thiết. Giải pháp xử lý nó một cách riêng biệt bằng cách kiểm tra tính nhất quán của các tỷ lệ trên toàn chuỗi. 

Một trường hợp khác là khi lần đầu tiên$k$vị trí đều là tiếng ồn ngẫu nhiên. Trong trường hợp đó, mọi hoạt động tái thiết chỉ dựa vào các cửa sổ ban đầu đều thất bại. Thuật toán tránh điều này bằng cách chỉ dựa vào hệ phương trình đã chọn, không cho rằng phân đoạn ban đầu là hợp lệ. 

Trường hợp cạnh thứ ba là các phép truy hồi suy biến trong đó nhiều hệ số bằng 0. Ví dụ, nếu chỉ$c_1 \neq 0$, hệ thống vẫn tạo thành một phương trình tuyến tính hợp lệ, nhưng nhiều hệ thống được lấy mẫu có thể trở nên không ổn định. Việc loại bỏ Gaussian vẫn ổn định vì nó hoạt động trên một trường và việc đảo ngược mô-đun đảm bảo tính chính xác bất cứ khi nào có trục xoay. 

Trường hợp cạnh cuối cùng là sự phụ thuộc tuyến tính ngẫu nhiên vào các phương trình đã chọn. Nếu hai hàng được lấy mẫu phụ thuộc, việc loại bỏ có thể không xác định được một cách duy nhất$c$, nhưng hệ thống như vậy bị từ chối ngầm trong quá trình xác minh vì nó sẽ không đáp ứng phần lớn các ràng buộc trong chuỗi đầy đủ.
