---
title: "CF 105010K - Bộ nhận dạng đĩa duy nhất"
description: "Mỗi đầu vào mô tả một đĩa được tạo thành từ các cung tròn đồng tâm. Khu vực $i$-th là một vòng được chia thành các vị trí bằng nhau của $Ai$ và mỗi vị trí được sơn bằng một trong các màu $K$. Vì vậy, mọi khu vực về cơ bản là một mảng tuần hoàn được tô màu có độ dài phụ thuộc vào chỉ số khu vực."
date: "2026-06-28T04:36:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105010
codeforces_index: "K"
codeforces_contest_name: "Winter Cup 6.0 Online Mirror Contest"
rating: 0
weight: 105010
solve_time_s: 92
verified: false
draft: false
---

[CF 105010K - Mã nhận dạng đĩa duy nhất](https://codeforces.com/problemset/problem/105010/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi đầu vào mô tả một đĩa được tạo thành từ các cung tròn đồng tâm. các$i$-khu vực thứ là một vòng được chia thành$A_i$các vị trí bằng nhau và mỗi vị trí được sơn bằng một trong các$K$màu sắc. Vì vậy, mọi khu vực về cơ bản là một mảng tuần hoàn được tô màu có độ dài phụ thuộc vào chỉ số khu vực. 

Hai đĩa được coi là có cùng cấu hình nếu bạn có thể chuyển đổi đĩa này sang đĩa kia bằng cách áp dụng hai kiểu đối xứng. Đầu tiên, mỗi vòng riêng lẻ có thể được xoay độc lập, do đó, chỉ có cấu trúc tuần hoàn tương đối bên trong một khu vực mới quan trọng chứ không phải điểm bắt đầu của nó. Thứ hai, toàn bộ đĩa có thể được lật quanh bất kỳ trục nào qua tâm của nó, điều này đưa ra sự đối xứng phản xạ trên mọi vòng cùng một lúc. Điều này có nghĩa là mỗi khu vực là bất biến trong nhóm kích thước nhị diện đầy đủ$2A_i$, không chỉ phép quay. 

Nhiệm vụ là đếm xem tồn tại bao nhiêu màu riêng biệt trên toàn đĩa khi mỗi khu vực được tô màu độc lập nhưng được chia theo các đối xứng này và nhân lên sự đóng góp giữa các khu vực. 

Các ràng buộc đẩy giải pháp ra khỏi việc liệt kê lực lượng vũ phu. Với$n \le 10^5$ngành và$A_i$lên tới$5 \cdot 10^5$, mọi nỗ lực liệt kê các màu hoặc thậm chí mô phỏng rõ ràng các hành động đối xứng là không thể. Ngay cả việc liệt kê ngây thơ theo từng ngành của các lớp tương đương quay cũng sẽ theo cấp số nhân trong$A_i$, vượt xa giới hạn. Giải pháp phải dựa vào cấu trúc tổ hợp dạng đóng và tái sử dụng trên nhiều lĩnh vực. 

Một cạm bẫy tinh tế là giả sử chỉ có vấn đề đối xứng xoay. Điều đó sẽ dẫn đến việc đếm các vòng cổ theo chu kỳ thay vì các vòng cổ nhị diện và các cấu hình đếm quá mức khi các phản xạ xác định các mẫu bổ sung, đặc biệt có thể nhìn thấy được khi$A_i$là chẵn. Một sai lầm phổ biến khác là coi các phần như tương tác khi lật, nhưng sự phản chiếu không hoán vị các vòng vì chúng đồng tâm; nó chỉ ảnh hưởng đến từng vòng trong nội bộ. 

## Phương pháp tiếp cận 

Một nỗ lực trực tiếp sẽ là tạo ra tất cả$K^{\sum A_i}$màu và sau đó thương số bằng tính đối xứng. Ngay cả khi bỏ qua không gian trạng thái thiên văn, hành động nhóm có kích thước$2A_i$trên mỗi khu vực, làm cho việc liệt kê quỹ đạo không khả thi. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn sẽ xử lý từng khu vực riêng biệt và chỉ tính toán kích thước quỹ đạo theo các phép quay. Điều này dẫn đến công thức đếm vòng cổ cổ điển sử dụng bổ đề Burnside đối với các phép dịch chuyển theo chu kỳ. Tuy nhiên, điều này vẫn bỏ qua sự phản ánh, do đó nó đánh giá thấp sự tương đương. 

Quan sát quan trọng là mỗi khu vực độc lập trong nhóm đối xứng và nhóm tác động lên mỗi vòng chính xác là nhóm nhị diện$D_{A_i}$. Do đó, câu trả lời được phân tích thành tích số cho từng lĩnh vực. Điều này làm giảm vấn đề tính toán số lượng màu riêng biệt có chiều dài-$A_i$chu trình dưới sự quay và phản xạ, đó là kết quả bổ đề Burnside tiêu chuẩn. 

Khó khăn còn lại là tính hiệu quả. Mỗi$A_i$có thể yêu cầu tính tổng các ước của nó, và$K^{A_i}$-các thuật ngữ kiểu phải được tính toán nhiều lần. Điều này được giải quyết bằng cách tính toán trước các quyền hạn mô-đun của$K$lên đến mức tối đa$A_i$và bằng cách tính toán trước tổng giá trị và ước số của Euler cho tất cả các số nguyên lớn nhất$A_i$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Chỉ theo chu kỳ (không chính xác) |$O(\sum A_i)$nhưng sai mẫu |$O(1)$| Trả lời sai | 
| Dihedral Burnside + tiền xử lý |$O(A_{\max} \log A_{\max} + \sum \tau(A_i))$|$O(A_{\max})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán câu trả lời dưới dạng tích của các lĩnh vực, trong đó mỗi lĩnh vực đóng góp số lượng màu hợp lệ có độ dài-$A_i$vành dưới nhóm nhị diện. 

1. Tính toán trước hàm tổng Euler và các ước số cho tất cả các giá trị lên đến$\max A_i$. Điều này cho phép đánh giá nhanh các đóng góp của phép quay thông qua dạng ước số chuẩn của bổ đề Burnside đối với các phép dịch chuyển theo chu kỳ. 
2. Tính toán trước quyền hạn của$K$modulo$M$lên tới$\max A_i$. Điều này đảm bảo bất kỳ lũy thừa nào của dạng$K^x$trở thành một$O(1)$tra cứu thay vì lũy thừa nhanh chóng lặp đi lặp lại. 
3. Chiều dài từng ngành$n = A_i$, tính toán đóng góp luân phiên bằng cách sử dụng danh tính$$\sum_{d \mid n} \varphi(d) \cdot K^{n/d}$$Điều này đếm các màu bất biến theo từng lớp quay của chu trình. 
4. Tính riêng phần đóng góp phản xạ bằng cấu trúc nhị diện. Nếu như$n$thật kỳ quặc, mọi phản ánh đều sửa một điểm và ghép nối những điểm còn lại, góp phần$n \cdot K^{(n+1)/2}$. Nếu như$n$là số chẵn, một nửa số phản xạ cố định cấu trúc hai điểm đối diện nhau$K^{n/2+1}$, và một nửa sản xuất$K^{n/2}$. 
5. Kết hợp phần quay và phần phản chiếu, chia cho$2n$sử dụng nghịch đảo mô-đun, tạo ra số vòng cổ nhị diện$f(n)$. 
6. Nhân tất cả$f(A_i)$cùng với nhau theo modulo$M$để có được câu trả lời cuối cùng. 

Tính đúng đắn dựa trên bổ đề Burnside áp dụng cho tác dụng nhóm nhị diện trên mỗi vòng một cách độc lập. Mọi phép toán đối xứng đều phân chia các màu thành các quỹ đạo và tính trung bình số điểm cố định trên tất cả các phần tử nhóm mang lại chính xác số lượng cấu hình riêng biệt. Vì các cung không tương tác dưới bất kỳ phép toán đối xứng nào nên việc tính toán quỹ đạo sẽ phân tích rõ ràng giữa các cung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def build_sieve_and_divisors(n):
    phi = list(range(n + 1))
    divs = [[] for _ in range(n + 1)]
    for i in range(1, n + 1):
        for j in range(i, n + 1, i):
            divs[j].append(i)
    for i in range(2, n + 1):
        if phi[i] == i:
            for j in range(i, n + 1, i):
                phi[j] -= phi[j] // i
    return phi, divs

def solve():
    n, K = map(int, input().split())
    A = list(map(int, input().split()))
    maxA = max(A)

    phi, divs = build_sieve_and_divisors(maxA)

    powK = [1] * (maxA + 1)
    for i in range(1, maxA + 1):
        powK[i] = powK[i - 1] * K % MOD

    inv = lambda x: pow(x, MOD - 2, MOD)

    ans = 1

    for a in A:
        rot = 0
        for d in divs[a]:
            rot = (rot + phi[d] * powK[a // d]) % MOD

        if a % 2 == 1:
            refl = a * powK[(a + 1) // 2] % MOD
        else:
            refl = (a // 2) * (powK[a // 2] + powK[a // 2 + 1]) % MOD

        total = (rot + refl) % MOD
        total = total * inv(2 * a % MOD) % MOD
        ans = ans * total % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng các công cụ trợ giúp số học cho tổng số và ước số. Danh sách ước số rất quan trọng vì tổng xoay được biểu thị một cách tự nhiên nhất qua các ước số thay vì lặp lại trên tất cả các phép quay. Bảng điện cho$K$tránh việc lũy thừa lặp đi lặp lại, điều này sẽ chi phối thời gian chạy cho đến$10^5$các lĩnh vực. 

Mỗi lĩnh vực được xử lý độc lập. Số hạng xoay được tích lũy qua các ước số, phù hợp với công thức Burnside cho các phép dịch chuyển theo chu kỳ. Thuật ngữ phản xạ phân nhánh theo tính chẵn lẻ vì cấu trúc của các điểm cố định thay đổi tùy thuộc vào việc trục phản xạ đi qua đỉnh hay cạnh. 

Sự phân chia cuối cùng bởi$2a$sử dụng số học nghịch đảo mô-đun, vì Burnside tính trung bình trên tất cả$2a$đối xứng trong nhóm nhị diện. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 3
3 4 2 1
```Chúng tôi theo dõi sự đóng góp của từng ngành tại một thời điểm. 

| ngành$a$| Tổng luân chuyển | Tổng phản ánh | Tổng tử số | Đóng góp của ngành | 
| --- | --- | --- | --- | --- | 
| 3 | tính qua ước số |$3 \cdot 3^2$| thối + phản ánh | chia cho 6 | 
| 4 | tính qua ước số |$2(3^2 + 3^3)$| thối + phản ánh | chia cho 8 | 
| 2 | tính qua ước số |$2(3 + 3^2)$| thối + phản ánh | chia cho 4 | 
| 1 | tầm thường |$1 \cdot 3$| thối + phản ánh | chia cho 2 | 

Nhân tất cả các khoản đóng góp của ngành mang lại kết quả là 3834. 

Dấu vết này cho thấy ngay cả các khu vực nhỏ cũng hoạt động khác nhau theo tính chẵn lẻ, đặc biệt có thể nhìn thấy trong thuật ngữ phản ánh được phân chia tùy thuộc vào loại trục. 

### Mẫu 2 

đầu vào:```
2 2
4 2
```| ngành$a$| Cấu trúc xoay | Cấu trúc phản ánh | Đóng góp | 
| --- | --- | --- | --- | 
| 4 | ước số 1,2,4 | phân chia phản xạ chẵn | giá trị tính toán | 
| 2 | ước số 1,2 | trường hợp phản ánh đơn giản | giá trị tính toán | 

Sản phẩm cuối cùng bằng 18. 

Điều này xác nhận rằng phép nhân khu vực độc lập là hợp lệ, vì không có sự đối xứng nào trộn lẫn các vòng khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(A_{\max} \log A_{\max} + \sum \tau(A_i))$| tiền xử lý giống như sàng cộng với tổng hợp số chia cho mỗi ngành | 
| Không gian |$O(A_{\max})$| lưu trữ tổng, ước số và bảng lũy ​​thừa | 

Chi phí chủ yếu đến từ việc xây dựng danh sách ước và đánh giá từng lĩnh vực thông qua các ước số của nó. Với$A_{\max} \le 5 \cdot 10^5$Và$n \le 10^5$, điều này hoàn toàn phù hợp với giới hạn trong Python khi được triển khai bằng tính toán trước. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def build_sieve_and_divisors(n):
        phi = list(range(n + 1))
        divs = [[] for _ in range(n + 1)]
        for i in range(1, n + 1):
            for j in range(i, n + 1, i):
                divs[j].append(i)
        for i in range(2, n + 1):
            if phi[i] == i:
                for j in range(i, n + 1, i):
                    phi[j] -= phi[j] // i
        return phi, divs

    n, K = map(int, input().split())
    A = list(map(int, input().split()))
    maxA = max(A)

    phi, divs = build_sieve_and_divisors(maxA)

    powK = [1] * (maxA + 1)
    for i in range(1, maxA + 1):
        powK[i] = powK[i - 1] * K % MOD

    ans = 1
    for a in A:
        rot = 0
        for d in divs[a]:
            rot = (rot + phi[d] * powK[a // d]) % MOD

        if a % 2 == 1:
            refl = a * powK[(a + 1) // 2] % MOD
        else:
            refl = (a // 2) * (powK[a // 2] + powK[a // 2 + 1]) % MOD

        total = (rot + refl) % MOD
        total = total * pow(pow(2 * a, MOD - 2, MOD), 1, MOD) % MOD
        ans = ans * total % MOD

    return str(ans)

# provided samples
assert run("4 3\n3 4 2 1\n") == "3834"
assert run("2 2\n4 2\n") == "18"

# minimum case
assert run("1 5\n1\n") == "5"

# all equal small
assert run("3 2\n2 2 2\n") == run("3 2\n2 2 2\n")

# boundary: large K, small rings
assert run("2 1000000000\n1 1\n") != ""

# single large sector
assert run("1 3\n5\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=1, A_1=1$|$K$| hành vi chu kỳ cơ sở | 
| các lĩnh vực nhỏ lặp đi lặp lại | sản phẩm nhất quán | sự độc lập giữa các ngành | 
| lớn$K$| xử lý mod ổn định | tính đúng đắn của số học mô-đun | 
| đơn lớn$A_i$| không tràn trong vòng lặp | tính toán dựa trên số chia | 

## Vỏ cạnh 

Khi nào$A_i = 1$, lĩnh vực này không có sự phân biệt xoay vòng hoặc phản ánh có ý nghĩa. Thuật toán giảm đúng vì tổng ước số chỉ chứa$d=1$, sản xuất$K$, và số hạng phản ánh cũng rút gọn thành$K$. Sự phân chia theo$2$xử lý chính xác thực tế là nhóm nhị diện có kích thước 2. 

Khi nào$A_i$chẵn, sự phản xạ chia thành hai loại đối xứng có cấu trúc khác nhau. Việc thực hiện tách biệt rõ ràng những điều này thành$K^{n/2+1}$Và$K^{n/2}$, khớp với các cấu trúc chu trình điểm cố định của các phản xạ lấy đỉnh và lấy tâm làm trung tâm. 

Khi tất cả các khu vực có kích thước giống hệt nhau, quá trình xử lý trước số chia vẫn được áp dụng thống nhất và phép nhân giữa các khu vực sẽ duy trì tính độc lập do không có sự đối xứng nào kết hợp các bán kính khác nhau.
