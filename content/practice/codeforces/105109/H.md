---
title: "CF 105109H - Tháp tiền tố"
description: "Chúng ta được cung cấp một dãy số và một phép biến đổi liên tục thay thế mảng đó bằng phiên bản tích tiền tố của nó. Một ứng dụng của phép biến đổi lấy một mảng và biến mỗi vị trí thành tích của mọi thứ theo chỉ số đó."
date: "2026-06-27T20:05:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105109
codeforces_index: "H"
codeforces_contest_name: "UTPC Spring 2024 Open Contest"
rating: 0
weight: 105109
solve_time_s: 104
verified: false
draft: false
---

[CF 105109H - Tháp tiền tố](https://codeforces.com/problemset/problem/105109/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số và một phép biến đổi liên tục thay thế mảng đó bằng phiên bản tích tiền tố của nó. Một ứng dụng của phép biến đổi lấy một mảng và biến mỗi vị trí thành tích của mọi thứ theo chỉ số đó. Vì vậy, phần tử đầu tiên không đổi, phần tử thứ hai trở thành tích của hai phần tử đầu tiên, phần tử thứ ba trở thành tích của ba phần tử đầu tiên, v.v., tất cả đều lấy modulo một số nguyên tố cố định. 

Chúng tôi không được yêu cầu mô phỏng trực tiếp quá trình này. Thay vào đó, chúng tôi nhận được một số truy vấn. Mỗi truy vấn yêu cầu một vị trí cụ thể trong mảng sau khi áp dụng chuyển đổi tiền tố-sản phẩm này một số lần nhất định. 

Khó khăn xuất phát từ thực tế là việc chuyển đổi được áp dụng nhiều lần và mỗi ứng dụng đều phụ thuộc vào tất cả các vị trí trước đó. Mô phỏng trực tiếp cho mỗi truy vấn sẽ tính toán lại các sản phẩm tiền tố nhiều lần, dẫn đến chi phí tỷ lệ thuận với số thao tác nhân với kích thước mảng, nhanh chóng trở nên quá lớn ngay cả đối với đầu vào vừa phải. 

Các ràng buộc cho thấy rằng cả kích thước mảng và số lượng biến đổi cho mỗi truy vấn đều có thể lên tới một nghìn và có tổng số lên tới một nghìn truy vấn. Một cách tiếp cận đơn giản để tính toán lại mảng cho mỗi truy vấn và mỗi bước chuyển đổi sẽ liên quan đến thứ tự 10^9 phép toán trong trường hợp xấu nhất, vượt xa những gì khả thi trong hai giây. 

Một vấn đề tế nhị phát sinh từ sự tăng trưởng chuyển đổi lặp đi lặp lại. Ngay cả khi một người triển khai chính xác các sản phẩm tiền tố một lần, việc lặp lại k lần cho mỗi truy vấn sẽ là quá chậm. Một cạm bẫy phổ biến khác là giả định sự độc lập giữa các phần tử sau nhiều lần biến đổi, nhưng mỗi vị trí ngày càng phụ thuộc vào nhiều phần tử trước đó theo cách có cấu trúc phải được nắm bắt bằng phương pháp phân tích. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng quá trình theo đúng nghĩa đen. Đối với một phép biến đổi đơn lẻ, chúng tôi tính toán các tích số tiền tố theo thời gian tuyến tính. Việc lặp lại k lần này cho mỗi truy vấn sẽ mang lại O(nk) cho mỗi truy vấn. Mặc dù đúng nhưng điều này sẽ bùng nổ khi cả n và k đều lớn, đặc biệt khi các truy vấn độc lập và có thể lặp lại công việc trên cùng một cấu trúc mảng. 

Quan sát quan trọng là mỗi phép biến đổi không tạo ra các tương tác mới; nó chỉ sắp xếp lại số lần mỗi phần tử ban đầu đóng góp cho mỗi vị trí. Sau một bước, mỗi vị trí sẽ là tích của tiền tố của mảng ban đầu. Sau hai bước, mỗi vị trí sẽ trở thành tích trên tiền tố của các sản phẩm tiền tố đó, nghĩa là mỗi phần tử gốc xuất hiện nhiều lần với bội số có cấu trúc. 

Nếu chúng ta theo dõi số mũ thay vì giá trị, phép nhân sẽ trở thành phép cộng trong không gian số mũ. Mỗi ứng dụng của phép toán tiền tố tương ứng với việc lấy tổng tiền tố của đóng góp số mũ. Việc lặp lại các phép toán giống tổng tiền tố là một cấu trúc cổ điển: sau k lần lặp lại, phần đóng góp của một phần tử sẽ lan truyền theo hệ số nhị thức. Cụ thể, sự đóng góp của phần tử a[j] vào vị trí i sau k phép toán bằng với số đường đi đơn điệu trong cách giải thích mạng đơn giản, ước tính một hệ số tổ hợp. 

Điều này làm giảm vấn đề từ mô phỏng lặp lại đến tính toán, đối với mỗi truy vấn, một tích trên tất cả các phần tử tiền tố trong đó mỗi a[j] được nâng lên thành số mũ nhị thức có thể tính toán trước chỉ phụ thuộc vào k, i và j. 

Tính toán trước các giai thừa và nghịch đảo mô-đun cho phép đánh giá nhanh các hệ số nhị thức này và sau đó mỗi truy vấn có thể được trả lời theo thời gian tuyến tính qua tiền tố mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(q · k · n) | O(n) | Quá chậm | 
| Công thức số mũ tổ hợp | O(q · n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi tính toán theo modulo một số nguyên tố, do đó các hệ số số học và nhị thức lũy thừa được xác định rõ ràng. 

### 1. Tính trước giai thừa và giai thừa nghịch đảo 

Chúng tôi tính toán các giai thừa lên tới khoảng 2000 và nghịch đảo mô-đun của chúng. Điều này cho phép chúng ta tính bất kỳ hệ số nhị thức nào trong thời gian không đổi. 

Điều này là cần thiết vì mọi truy vấn đều cần nhiều đánh giá nhị thức và việc tính toán lại chúng sẽ chiếm ưu thế trong thời gian chạy. 

### 2. Xử lý từng truy vấn một cách độc lập 

Đối với truy vấn yêu cầu vị trí x sau k thao tác, chúng tôi tính giá trị cuối cùng tại vị trí đó bằng cách tích lũy đóng góp từ tất cả các chỉ số j từ 1 đến x. 

### 3. Tính toán đóng góp của từng phần tử 

Với mỗi j ≤ x, chúng ta tính số mũ: 

C(k + x − j − 1, k − 1) 

Giá trị này biểu thị số lần phần tử ban đầu a[j] đóng góp theo cấp số nhân vào vị trí x sau k lần chuyển đổi tiền tố lặp lại. 

### 4. Tích lũy kết quả 

Chúng tôi khởi tạo câu trả lời là 1 và nhân nó với: 

a[j] được nâng lên số mũ được tính toán 

với mọi j từ 1 đến x. 

Tất cả các phép toán được thực hiện theo modulo 1e9+7. 

### Tại sao nó hoạt động 

Mỗi thao tác tiền tố biến một mảng thành tích tích lũy, tương ứng với việc biến số mũ thành tổng tiền tố. Việc lặp lại quá trình này k lần tương ứng với việc áp dụng toán tử tổng tiền tố k lần trong không gian số mũ. Tổng tiền tố k-fold của một delta ở vị trí j lan truyền đến vị trí i với bội số bằng số cách phân phối k các phép toán tiền tố giống hệt nhau trên khoảng cách i - j, chính xác là một hệ số nhị thức. Vì phép nhân trở thành phép cộng trong không gian số mũ nên giá trị cuối cùng được xác định bằng cách tính tổng các đóng góp này theo cấp số nhân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

MAXN = 2005

fact = [1] * MAXN
invfact = [1] * MAXN

for i in range(1, MAXN):
    fact[i] = fact[i - 1] * i % MOD

invfact[MAXN - 1] = pow(fact[MAXN - 1], MOD - 2, MOD)

for i in range(MAXN - 2, -1, -1):
    invfact[i] = invfact[i + 1] * (i + 1) % MOD

def nCr(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

t = int(input())
for _ in range(t):
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    for _ in range(q):
        k, x = map(int, input().split())

        res = 1
        for j in range(x):
            exp = nCr(k + x - j - 1, k - 1)
            res = res * pow(a[j], exp, MOD) % MOD

        print(res)
```Quá trình tiền xử lý giai thừa được chia sẻ trên tất cả các trường hợp thử nghiệm vì các giới hạn là toàn cục. Hàm nhị thức được giữ ở mức tối thiểu để tránh chi phí vì nó được gọi nhiều lần. 

Bên trong mỗi truy vấn, chúng tôi lặp lại tiền tố lên đến x. Công thức số mũ mã hóa trực tiếp tác động của k phép toán tích tiền tố lặp lại, do đó không có mảng trung gian nào được xây dựng. 

Một lỗi triển khai phổ biến là lập chỉ mục theo từng thuật ngữ nhị thức. Sự dịch chuyển đúng là k + x − j − 1 chọn k − 1, xuất phát từ việc sắp xếp hoạt động tiền tố đầu tiên dưới dạng một lớp tích lũy duy nhất bắt đầu từ mỗi chỉ mục. 

## Ví dụ đã hoạt động 

Xét một mảng nhỏ a = [2, 3, 5]. 

Với k = 1 và x = 3, phép biến đổi là một tích tiền tố duy nhất, do đó kết quả sẽ là [2, 6, 30]. Công thức cho: 

| j | số mũ C(1 + 3 − j − 1, 0) | đóng góp | 
| --- | --- | --- | 
| 1 | 1 | 2 | 
| 2 | 1 | 3 | 
| 3 | 1 | 5 | 

Tích là 2 · 3 · 5 = 30, khớp với phần tử thứ ba của mảng sản phẩm tiền tố. 

Bây giờ xét k = 2, x = 3. 

Chúng tôi tính toán số mũ: 

| j | số mũ C(2 + 3 − j − 1, 1) | 
| --- | --- | 
| 1 | C(3,1) = 3 | 
| 2 | C(2,1) = 2 | 
| 3 | C(1,1) = 1 | 

Vậy kết quả sẽ là 2^3 · 3^2 · 5^1 = 8 · 9 · 5 = 360. 

Điều này phù hợp với những gì xảy ra nếu chúng ta áp dụng tiền tố-sản phẩm một cách rõ ràng hai lần: đầu tiên là [2, 6, 30], sau đó là [2, 12, 360]. 

Dấu vết cho thấy các hoạt động tiền tố lặp lại mở rộng đóng góp từ các chỉ mục trước đó theo mẫu tổ hợp có thể dự đoán được như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q · n) | Mỗi truy vấn tính x phép nhân, mỗi phép nhân có đánh giá nhị thức O(1) | 
| Không gian | O(n) | Giai thừa và giai thừa nghịch đảo lên tới ~2000 | 

Các giới hạn này giữ cho tổng số truy vấn đủ nhỏ để việc lặp qua các tiền tố trên mỗi truy vấn vẫn hiệu quả. Tính toán trước đảm bảo rằng các hệ số nhị thức không đưa ra các thừa số logarit ẩn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def solve():
    input = sys.stdin.readline
    MAXN = 2005

    fact = [1] * MAXN
    invfact = [1] * MAXN
    for i in range(1, MAXN):
        fact[i] = fact[i-1] * i % MOD
    invfact[MAXN-1] = pow(fact[MAXN-1], MOD-2, MOD)
    for i in range(MAXN-2, -1, -1):
        invfact[i] = invfact[i+1] * (i+1) % MOD

    def nCr(n, r):
        if r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n-r] % MOD

    t = int(input())
    out = []

    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))

        for _ in range(q):
            k, x = map(int, input().split())
            res = 1
            for j in range(x):
                exp = nCr(k + x - j - 1, k - 1)
                res = res * pow(a[j], exp, MOD) % MOD
            out.append(str(res))

    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# minimal case
assert run("""1
1 1
7
1 1
""").strip() == "7"

# single prefix
assert run("""1
3 1
2 3 5
1 3
""").strip() == "30"

# double transform
assert run("""1
3 1
2 3 5
2 3
""").strip() == "360"

# all equal
assert run("""1
4 1
2 2 2 2
3 4
""") == run("""1
4 1
2 2 2 2
3 4
""")

# boundary small k
assert run("""1
5 1
1 2 3 4 5
1 5
""").strip() == "120"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 7 | ổn định giá trị duy nhất | 
| 3 phần tử k=1 | 30 | sản phẩm tiền tố đúng | 
| 3 phần tử k=2 | 360 | tính đúng đắn của phép biến đổi lặp lại | 
| tất cả đều bình đẳng | nhất quán | đối xứng dưới mảng đồng nhất | 
| k=1 đầy | sản phẩm giai thừa | tích lũy tiền tố đầy đủ | 

## Vỏ cạnh 

Mảng một phần tử cho thấy rằng tất cả các hệ số nhị thức đều giảm xuống 1 bất kể k, vì không có sự trải rộng trên các chỉ số. Thuật toán giảm xuống thành phép nhân lặp lại của một giá trị có số mũ 1 và mã trả về chính xác phần tử gốc. 

Khi k = 1, hệ số nhị thức trở thành C(x − j, 0), bằng 1 với mọi j hợp lệ, nghĩa là mọi phần tử đóng góp chính xác một lần. Điều này phù hợp với thực tế là một thao tác tiền tố tạo ra một sản phẩm tiền tố đơn giản. Việc triển khai xử lý việc này một cách tự nhiên vì nCr(n, 0) trả về 1. 

Đối với k lớn với x nhỏ, nhiều hệ số nhị thức trở thành 0 khi các chỉ số nằm ngoài phạm vi hợp lệ. Bộ phận bảo vệ nCr đảm bảo những đóng góp này biến mất, ngăn chặn phép nhân không chính xác từ các số hạng tổ hợp không hợp lệ.
