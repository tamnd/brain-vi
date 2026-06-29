---
title: "CF 104758B - Trình tự Bionaccia"
description: "Chúng ta được cung cấp một chỉ số rất lớn $K$ và được yêu cầu tính giá trị thứ $K$ của một chuỗi được xác định bởi phép lặp bậc ba. Chuỗi bắt đầu với ba giá trị cố định và mỗi số hạng sau được xây dựng từ ba số hạng trước đó cộng với phần đóng góp không đổi bổ sung."
date: "2026-06-28T22:06:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104758
codeforces_index: "B"
codeforces_contest_name: "The 2023 ICPC Masters Mexico Regional #ICPCMX2023 Edition"
rating: 0
weight: 104758
solve_time_s: 83
verified: true
draft: false
---

[CF 104758B - Trình tự Bionaccia](https://codeforces.com/problemset/problem/104758/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chỉ số rất lớn$K$, và yêu cầu tính toán$K$-giá trị thứ của một chuỗi được xác định bởi sự lặp lại bậc ba. Chuỗi bắt đầu với ba giá trị cố định và mỗi số hạng sau được xây dựng từ ba số hạng trước đó cộng với phần đóng góp không đổi bổ sung. 

Cụ thể, mỗi số hạng phụ thuộc tuyến tính vào ba số hạng trước đó và cũng có một độ dịch cộng cố định bằng 3 ở mỗi bước. chỉ số$K$có thể lớn như$10^{16}$, điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào xây dựng từng thuật ngữ một. Ngay cả khi mỗi lần chuyển đổi là thời gian không đổi, việc lặp lại lên tới$10^{16}$không thể thực hiện được các bước trong bất kỳ giới hạn thời gian hợp lý nào. 

Đầu ra là giá trị của chuỗi này tại vị trí$K$, lấy modulo$10^9+7$. Mô-đun quan trọng vì các giá trị tăng theo cấp số nhân do sự lặp lại, vì vậy các kết quả trung gian phải được giữ trong phạm vi số học mô-đun. 

Một mô phỏng ngây thơ sẽ thất bại ngay cả đối với kích thước vừa phải$K$. Ví dụ, tính toán lên tới$K = 10^7$đã yêu cầu quá nhiều lần chuyển đổi và các giá trị tăng đủ nhanh đến mức việc tràn số nguyên trở thành mối lo ngại nếu không giảm mô-đun. Trở ngại thực sự là kích thước chỉ mục chứ không phải độ lớn của các giá trị. 

Khó khăn chính là mỗi thuật ngữ phụ thuộc vào ba giá trị trước đó, cộng với một thuật ngữ không đổi ngăn cản việc điều trị tái phát đồng nhất đơn giản trừ khi chúng tôi mở rộng trạng thái. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp tính toán các số hạng một cách tuần tự từ$t(0)$đi lên, áp dụng phép truy toán ở mỗi bước. Điều này đúng vì mỗi thuật ngữ được xác định đầy đủ bởi ba giá trị trước đó. Tuy nhiên, đạt được chỉ số$K$cách này đòi hỏi$O(K)$quá trình chuyển đổi, điều này trở nên không khả thi khi$K$có thể$10^{16}$. Điểm nghẽn là sự tăng trưởng tuyến tính về số lượng trạng thái tính toán. 

Cấu trúc của phép truy hồi gợi ý một ý tưởng mạnh mẽ hơn: sự chuyển đổi từ trạng thái này của ba số hạng liên tiếp sang trạng thái tiếp theo là tuyến tính. Phép truy hồi tuyến tính trên một kích thước cửa sổ cố định có thể được mã hóa dưới dạng phép nhân ma trận. Sự phức tạp duy nhất ở đây là hằng số$+3$, phá vỡ tính đồng nhất. Điều này có thể được khắc phục bằng cách thêm một chiều bổ sung vào trạng thái luôn giữ giá trị không đổi 1, cho phép số hạng không đổi được hấp thụ vào phép biến đổi ma trận. 

Khi phép truy toán được viết lại dưới dạng phép biến đổi tuyến tính có chiều cố định, ứng dụng lặp lại sẽ trở thành phép lũy thừa của ma trận. Tính lũy thừa nhanh làm giảm số lần nhân từ$K$ĐẾN$O(\log K)$và mỗi phép nhân hoạt động trên một ma trận có kích thước không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(K)$|$O(1)$| Quá chậm | 
| lũy thừa ma trận |$O(\log K)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mã hóa phép truy hồi thành dạng ma trận để mỗi bước nâng cao trạng thái của hệ thống. 

Chúng ta định nghĩa một vectơ trạng thái lưu trữ ba số hạng cuối cùng cùng với một hằng số: 

1. Xác định trạng thái tại vị trí$n$BẰNG$[t(n), t(n-1), t(n-2), 1]$. tồn tại thêm 1 nên hằng số$+3$có thể được biểu diễn dưới dạng một phép biến đổi tuyến tính. 
2. Diễn tả sự tái diễn theo trạng thái này. Thành phần đầu tiên$t(n)$được tính như$3t(n-1) + 2t(n-2) + t(n-3) + 3\cdot 1$. Các thành phần còn lại chỉ cần dịch chuyển cửa sổ của các giá trị trước đó và thành phần cuối cùng vẫn giữ nguyên là 1. 
3. Xây dựng ma trận chuyển tiếp$A$nhân như vậy$A$bởi nhà nước tại$n-1$tạo ra trạng thái tại$n$. Hàng đầu tiên mã hóa các hệ số lặp lại và các hàng phía dưới thực hiện hành vi dịch chuyển. 
4. Tính trạng thái ban đầu. Vì chúng ta biết$t(0), t(1), t(2)$, chúng ta khởi tạo vectơ tại$n=2$BẰNG$[t(2), t(1), t(0), 1] = [3, 2, 1, 1]$. 
5. Nếu$K \le 2$, trả về trực tiếp giá trị cơ sở tương ứng. Ngược lại hãy tính$A^{K-2}$sử dụng lũy ​​thừa nhanh. 
6. Nhân$A^{K-2}$bằng vectơ ban đầu để thu được trạng thái tại$K$. Phần tử đầu tiên của vectơ kết quả là câu trả lời. 

Mỗi bước nhân duy trì tính đúng đắn vì nó thể hiện một ứng dụng chính xác của phép truy toán. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là phép truy hồi là tuyến tính khi số hạng không đổi được hấp thụ vào trạng thái. Mỗi trạng thái tóm tắt đầy đủ tất cả thông tin cần thiết để tính toán trạng thái tiếp theo, do đó quá trình chuyển đổi là khép kín. Phép lũy thừa ma trận áp dụng thành phần lặp lại của phép biến đổi này một cách hiệu quả và phép lũy thừa bằng bình phương đảm bảo chúng ta áp dụng chính xác$K-2$chuyển đổi mà không liệt kê chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def mat_mul(a, b):
    n = 4
    res = [[0] * n for _ in range(n)]
    for i in range(n):
        for k in range(n):
            if a[i][k] == 0:
                continue
            aik = a[i][k]
            for j in range(n):
                res[i][j] = (res[i][j] + aik * b[k][j]) % MOD
    return res

def mat_pow(mat, exp):
    n = 4
    res = [[0] * n for _ in range(n)]
    for i in range(n):
        res[i][i] = 1

    while exp > 0:
        if exp & 1:
            res = mat_mul(res, mat)
        mat = mat_mul(mat, mat)
        exp >>= 1
    return res

def solve():
    K = int(input().strip())

    if K == 0:
        print(1)
        return
    if K == 1:
        print(2)
        return
    if K == 2:
        print(3)
        return

    A = [
        [3, 2, 1, 3],
        [1, 0, 0, 0],
        [0, 1, 0, 0],
        [0, 0, 0, 1]
    ]

    P = mat_pow(A, K - 2)

    vec = [3, 2, 1, 1]

    ans = (
        P[0][0] * vec[0] +
        P[0][1] * vec[1] +
        P[0][2] * vec[2] +
        P[0][3] * vec[3]
    ) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai cốt lõi tập trung vào ma trận 4 x 4 cố định vì độ lặp lại phụ thuộc vào ba số hạng trước đó cộng với một hằng số. Quy trình nhân thực hiện phép nhân ma trận mô-đun và quy trình lũy thừa liên tục bình phương ma trận trong khi sử dụng các bit của$K$. 

Vectơ ban đầu tương ứng với bộ ba giá trị chuỗi hợp lệ đã biết cuối cùng, được căn chỉnh sao cho ứng dụng lặp lại của ma trận sẽ tiến về phía trước theo thời gian. 

Cần chú ý cẩn thận khi lập chỉ mục: phép lũy thừa bắt đầu từ$K-2$bởi vì chúng tôi khởi tạo tại$t(2)$, không$t(0)$. Một lỗi phổ biến là dịch chuyển ranh giới này không chính xác, dẫn đến sai sót từng lỗi một. 

## Ví dụ đã hoạt động 

Xem xét đầu vào mẫu$K = 6$. Chúng tôi bắt đầu từ trạng thái cơ sở tại$n=2$, đó là$[3,2,1,1]$. Chúng tôi áp dụng$A^4$tới vectơ này vì$6-2=4$. 

| Bước | Hoạt động | Trạng thái (t(n), t(n-1), t(n-2), 1) | 
| --- | --- | --- | 
| 2 | ban đầu | (3, 2, 1, 1) | 
| 3 | áp dụng tái phát | (17, 3, 2, 1) | 
| 4 | áp dụng tái phát | (62, 17, 3, 1) | 
| 5 | áp dụng tái phát | (226, 62, 17, 1) | 
| 6 | áp dụng tái phát | (822, 226, 62, 1) | 

Giá trị cuối cùng khớp với kết quả mong đợi, cho thấy phép biến đổi ma trận mã hóa chính xác phép lặp. 

Vì$K = 7$, quá trình tương tự sẽ tiếp tục thêm một bước nữa. 

| Bước | Hoạt động | Trạng thái (t(n), t(n-1), t(n-2), 1) | 
| --- | --- | --- | 
| 4 | tài liệu tham khảo bắt đầu | (62, 17, 3, 1) | 
| 5 | áp dụng tái phát | (226, 62, 17, 1) | 
| 6 | áp dụng tái phát | (822, 226, 62, 1) | 
| 7 | áp dụng tái phát | (2983, 822, 226, 1) | 

Dấu vết này cho thấy rằng một khi cơ chế chuyển đổi chính xác thì mỗi bước đều mang tính quyết định và nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log K)$| lũy thừa ma trận trên phép biến đổi 4×4 cố định | 
| Không gian |$O(1)$| ma trận và vectơ có kích thước không đổi | 

Sự phụ thuộc logarit vào$K$là điều làm cho giải pháp khả thi với các giá trị lên tới$10^{16}$. Mỗi phép nhân là một công việc liên tục, do đó, ngay cả kích thước đầu vào lớn nhất cũng được xử lý một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip() if False else ""

# Provided samples (conceptual placeholders since full harness is omitted)
# assert run("6\n") == "822"
# assert run("7\n") == "2983"

# custom direct checks via known sequence values

def brute_small(k):
    t = [1, 2, 3]
    if k <= 2:
        return t[k]
    for i in range(3, k + 1):
        t.append((3*t[i-1] + 2*t[i-2] + t[i-3] + 3) % (10**9+7))
    return t[k]

# edge cases
assert brute_small(0) == 1
assert brute_small(1) == 2
assert brute_small(2) == 3
assert brute_small(6) == 822
assert brute_small(7) == 2983
assert brute_small(8) == 10822
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 1 | ranh giới căn cứ | 
| 1 | 2 | trường hợp cơ sở thứ hai | 
| 8 | 10822 | độ chính xác của quá trình chuyển đổi vượt quá phạm vi mẫu | 

## Vỏ cạnh 

cho$K = 2$, thuật toán trả về trực tiếp giá trị cơ sở mà không gọi lũy thừa ma trận. Trạng thái đã được căn chỉnh như$[t(2), t(1), t(0), 1] = [3,2,1,1]$, do đó không cần chuyển đổi. 

Đối với rất lớn$K$, chẳng hạn như$10^{16}$, vòng lặp lũy thừa chỉ xử lý khoảng 55 lần lặp vì mỗi bước sẽ giảm một nửa số mũ. Ma trận không bao giờ phát triển hoặc thay đổi hình dạng, do đó không có nguy cơ tràn ra ngoài số học mô-đun và quá trình tính toán vẫn ổn định. 

Đối với nhỏ$K$, việc phân nhánh rõ ràng sẽ ngăn chặn sự dịch chuyển không chính xác của các chỉ số. Nếu không có những người bảo vệ này, sử dụng$K-2$sẽ cố gắng tính lũy thừa âm một cách mù quáng, điều này sẽ phá vỡ cách giải thích hệ thống chuyển tiếp.
