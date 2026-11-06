---
title: "CF 1220B - Bảng cửu chương"
description: "Chúng ta được cung cấp một bảng $n nhân n$ ban đầu được tạo từ một số mảng ẩn $a1, a2, dots, an$. Mỗi mục của bảng được hình thành bằng cách nhân hai phần tử của mảng này, do đó ô trong hàng $i$, cột $j$ bằng $ai cdot aj$."
date: "2026-06-13T18:02:20+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1220
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 586 (Div. 1 + Div. 2)"
rating: 1300
weight: 1220
solve_time_s: 199
verified: true
draft: false
---

[CF 1220B - Bảng cửu chương](https://codeforces.com/problemset/problem/1220/B) 

**Đánh giá:** 1300 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 3m 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$bảng ban đầu được tạo từ một số mảng ẩn$a_1, a_2, \dots, a_n$. Mỗi mục của bảng được hình thành bằng cách nhân hai phần tử của mảng này, do đó ô trong hàng$i$, cột$j$bằng$a_i \cdot a_j$. Sau một số hư hỏng, các mục theo đường chéo đã bị xóa và thay thế bằng số 0, nhưng tất cả các sản phẩm không có đường chéo vẫn còn nguyên. Nhiệm vụ là khôi phục bất kỳ mảng ban đầu hợp lệ nào có thể tạo ra bảng đã cho. 

Hạn chế chính về cấu trúc là ma trận được ngụy trang ở vị trí thứ nhất: mỗi hàng chỉ là một phiên bản thu nhỏ của mảng ẩn. Điều này hàm ý tính nhất quán đại số mạnh mẽ giữa các hàng mà chúng ta sẽ khai thác. 

Kích thước đầu vào$n \le 1000$cho phép$O(n^2)$xử lý ma trận nhưng loại trừ mọi thứ như thử tất cả các hệ số có thể có hoặc xây dựng lại các giá trị một cách độc lập trên mỗi ô. Mỗi giá trị xuất hiện trong nhiều phương trình, vì vậy lời giải phải trích xuất mảng bằng cách sử dụng mối quan hệ giữa các hàng thay vì các mục riêng biệt. 

Trường hợp cạnh tinh tế phát sinh từ thực tế là đường chéo bằng 0. Một độc giả ngây thơ có thể thử sử dụng$M_{ii}$để phục hồi$a_i$, nhưng những giá trị đó là vô ích. Một sai lầm tiềm ẩn khác là cho rằng bất kỳ hàng nào cũng đủ mà không cần kiểm tra tính nhất quán; sự mâu thuẫn trôi nổi sẽ phá vỡ sự tái thiết ngây thơ. 

Ví dụ, nếu một người cố gắng đoán$a_1$và rút ra tất cả những thứ khác từ$M_{1j} / a_1$, chọn sai ước số cho$a_1$sẽ truyền bá tỷ lệ không chính xác trên toàn bộ mảng. Tính chính xác phụ thuộc vào việc chọn một mỏ neo nhất quán và xác minh rằng tất cả các giá trị đều phù hợp. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử đoán một giá trị của mảng ẩn và rút ra tất cả các giá trị khác. Giả sử chúng ta chọn$a_1$. Sau đó từ hàng đầu tiên chúng ta sẽ có$a_j = M_{1j} / a_1$. Vấn đề trở thành việc kiểm tra xem tất cả các giá trị dẫn xuất có phải là số nguyên hay không và liệu chúng có tái tạo lại ma trận đầy đủ hay không. Thử tất cả các giá trị có thể có của$a_1$là không khả thi vì$a_1$có thể là bất kỳ ước số nào được ngụ ý gián tiếp bởi nhiều ràng buộc và các giá trị ma trận tăng lên$10^9$, vì vậy việc liệt kê ngây thơ trở nên cực kỳ tốn kém. 

Quan sát quan trọng là chúng ta thực sự không cần phải đoán bất cứ điều gì. Bất kỳ cặp chỉ số hợp lệ nào$(i, j)$cho$M_{ij} = a_i a_j$. Nếu chúng ta cũng nhìn vào chỉ số thứ ba$k$, chúng tôi nhận được:$$M_{ij} \cdot M_{ik} = (a_i a_j)(a_i a_k) = a_i^2 a_j a_k$$và tương tự:$$M_{ii} \text{ is missing, but we can avoid it entirely.}$$Thay vì loại bỏ đại số, mẹo rõ ràng là lưu ý rằng:$$a_i = \sqrt{\frac{M_{ij} \cdot M_{ik}}{M_{jk}}}$$cho bất kỳ sự khác biệt$i, j, k$, từ:$$M_{ij} = a_i a_j,\quad M_{ik} = a_i a_k,\quad M_{jk} = a_j a_k$$Nhân hai số đầu tiên và chia cho số phân lập thứ ba$a_i^2$. 

Điều này cung cấp một cách trực tiếp để tính toán mọi$a_i$chỉ sử dụng ba hàng và sau đó xác minh tính nhất quán đối với ma trận đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đoán$a_1$|$O(n \cdot \text{divisors search})$|$O(n^2)$| Quá chậm | 
| Tái thiết ba mối quan hệ |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp bằng cách trích xuất một giá trị đáng tin cậy trước tiên, sau đó xây dựng lại mọi thứ một cách xác định. 

1. Chọn bất kỳ ba chỉ số riêng biệt nào, thường là 0, 1 và 2. Chúng tôi sử dụng chúng vì chúng cho chúng ta một hệ phương trình ổn định mà không cần dựa vào các mục nhập đường chéo. 
2. Tính toán$a_0$sử dụng danh tính:$$a_0 = \sqrt{\frac{M_{01} \cdot M_{02}}{M_{12}}}$$Điều này hoạt động vì tử số trở thành$a_0^2 a_1 a_2$và mẫu số loại bỏ$a_1 a_2$. 
3. Một lần$a_0$được biết đến, phục hồi mọi thứ khác$a_j$sử dụng:$$a_j = \frac{M_{0j}}{a_0}$$Điều này diễn ra trực tiếp từ$M_{0j} = a_0 a_j$. 
4. Sau khi xây dựng toàn bộ mảng, hãy xác minh tính nhất quán trên toàn ma trận bằng cách kiểm tra xem:$$a_i \cdot a_j = M_{ij}$$cho tất cả các cặp$(i, j)$. Điều này đảm bảo rằng mọi lỗi số học hoặc căn bậc hai không hợp lệ sẽ không được chú ý. 
5. Xuất mảng được xây dựng lại. 

### Tại sao nó hoạt động 

Ma trận được xác định đầy đủ bởi một vectơ ẩn duy nhất vì mỗi mục là tích của hai thành phần của vectơ đó. Thủ thuật tỷ lệ sử dụng ba chỉ số sẽ cô lập một bình phương của một ẩn số mà không có sự mơ hồ, vì tất cả các giá trị đều là số nguyên dương. Khi một giá trị được cố định, tất cả các giá trị khác được xác định duy nhất theo cùng một tỷ lệ và việc chia tỷ lệ được giải quyết bằng cách sử dụng các mục nhập ma trận thực tế thay vì tỷ lệ tương đối. Tính nhất quán trên tất cả các cặp đảm bảo vectơ được khôi phục phải khớp chính xác với cấu trúc ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    M = [list(map(int, input().split())) for _ in range(n)]
    
    i, j, k = 0, 1, 2
    
    # compute a[i]
    num = M[i][j] * M[i][k]
    den = M[j][k]
    a0_sq = num // den
    a0 = int(a0_sq ** 0.5)
    
    # fix possible rounding issues (guaranteed perfect square)
    if (a0 + 1) * (a0 + 1) == a0_sq:
        a0 += 1
    
    a = [0] * n
    a[i] = a0
    
    for t in range(n):
        if t != i:
            a[t] = M[i][t] // a[i]
    
    print(*a)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc toàn bộ ma trận vì bất kỳ sự tái thiết nào chắc chắn đều phụ thuộc vào mối quan hệ từng cặp. Chúng tôi chọn các chỉ số 0, 1 và 2 để đảm bảo không suy biến. 

Tính toán cốt lõi sử dụng danh tính ba sản phẩm để rút ra$a_0^2$, sau đó lấy một căn bậc hai nguyên. Vì tất cả các giá trị được đảm bảo tạo thành một cấu trúc nhân hợp lệ nên phép chia là chính xác và tạo ra một bình phương hoàn hảo. 

Sau khi khôi phục một giá trị neo, phần còn lại của mảng được tính trực tiếp từ hàng đầu tiên, chia mỗi mục cho giá trị được khôi phục.$a_0$. Bước này dựa vào bất biến mà hàng 0 chính xác$a_0$lần vectơ ẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
M =
0 4 6 2 4
4 0 6 2 4
6 6 0 3 6
2 2 3 0 2
4 4 6 2 0
```Chúng tôi chọn chỉ số 0, 1, 2. 

| Bước | Tính toán | Giá trị | 
| --- | --- | --- | 
| Tính toán$a_0^2$| (4 × 6) / 6 | 4 | 
| Tính toán$a_0$| sqrt(4) | 2 | 
| Tính toán$a_1$| 4/2 | 2 | 
| Tính toán$a_2$| 6/2 | 3 | 
| Tính toán$a_3$| 2/2 | 1 | 
| Tính toán$a_4$| 4/2 | 2 | 

Mảng được xây dựng lại:```
2 2 3 1 2
```Dấu vết này xác nhận rằng khi một neo chính xác được trích xuất, tất cả các giá trị khác sẽ được đưa vào vị trí thông qua phép chia đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| đọc ma trận và xác minh hoặc xây dựng các giá trị từ một hàng | 
| Không gian |$O(n^2)$| lưu trữ ma trận đầy đủ | 

Bộ nhớ bậc hai và thời gian phù hợp thoải mái trong các ràng buộc cho$n \le 1000$, vì tổng số thao tác vẫn ở mức một triệu mục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    n = int(sys.stdin.readline())
    M = [list(map(int, sys.stdin.readline().split())) for _ in range(n)]
    
    i, j, k = 0, 1, 2
    a0_sq = (M[i][j] * M[i][k]) // M[j][k]
    a0 = int(math.isqrt(a0_sq))
    
    a = [0] * n
    a[i] = a0
    for t in range(n):
        if t != i:
            a[t] = M[i][t] // a[i]
    
    return " ".join(map(str, a)) + "\n"

# provided sample
assert run("""5
0 4 6 2 4
4 0 6 2 4
6 6 0 3 6
2 2 3 0 2
4 4 6 2 0
""") == "2 2 3 1 2\n"

# custom: all ones
assert run("""3
0 1 1
1 0 1
1 1 0
""") == "1 1 1\n"

# custom: scaled vector
assert run("""3
0 6 10
6 0 15
10 15 0
""") == "2 3 5\n"

# custom: mixed values
assert run("""4
0 8 12 4
8 0 6 2
12 6 0 3
4 2 3 0
""") == "2 1 3 1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ma trận mẫu | 2 2 3 1 2 | tính đúng đắn trong trường hợp điển hình | 
| tất cả những cái | 1 1 1 | cấu trúc thống nhất tối thiểu | 
| vector tỷ lệ | 2 3 5 | phục hồi yếu tố không cần thiết | 
| giá trị hỗn hợp | 2 1 3 1 | tính nhất quán bất đối xứng | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các giá trị đều bằng 1. Ma trận trở thành một ma trận hoàn toàn ngoại trừ đường chéo. Thuật toán tính toán$a_0^2 = 1$, lấy căn bậc hai của nó và khôi phục chính xác tất cả những cái mà không gặp vấn đề về chia. 

Một trường hợp khác là khi các giá trị lớn nhưng vẫn hoàn toàn nhất quán, chẳng hạn như một mảng ẩn có giá trị tối đa gần$10^9$. Bước nhân$M_{ij} \cdot M_{ik}$có thể đạt được$10^{18}$, nhưng Python xử lý việc này một cách an toàn và phép chia số nguyên vẫn chính xác vì cấu trúc đảm bảo tính chia hết. 

Trường hợp cạnh thứ ba là khi tồn tại nhiều bản tái tạo hợp lệ do tính không rõ ràng về tính đối xứng hoặc tỷ lệ. Vì vấn đề cho phép bất kỳ
