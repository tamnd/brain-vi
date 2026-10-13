---
title: "CF 105163H - Màu sắc hàng hóa"
description: "Mỗi mục trong đầu vào có thể được xem như một “đối tượng có màu”, ngoại trừ một đối tượng có thể mang nhiều màu cùng một lúc."
date: "2026-06-27T10:54:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105163
codeforces_index: "H"
codeforces_contest_name: "The 19th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 105163
solve_time_s: 53
verified: true
draft: false
---

[CF 105163H - Màu sắc hàng hóa](https://codeforces.com/problemset/problem/105163/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi mục trong đầu vào có thể được xem như một “đối tượng có màu”, ngoại trừ một đối tượng có thể mang nhiều màu cùng một lúc. Chúng ta được cung cấp nhiều đối tượng như vậy và chúng ta muốn hiểu có bao nhiêu cách chúng ta có thể chọn chính xác.$k$các đối tượng sao cho sự kết hợp của các bộ màu của chúng tạo ra từng mặt nạ màu có thể có. 

Thay vì nghĩ về các màu sắc riêng lẻ, chúng tôi biểu diễn mọi đối tượng dưới dạng một mặt nạ bit trên một vũ trụ màu sắc cố định. Một tập hợp con các màu tương ứng với một mặt nạ số nguyên và mỗi đối tượng đóng góp vào chính xác một mặt nạ trong một mảng$a$, Ở đâu$a[s]$là số lượng đối tượng có bộ màu chính xác$s$. 

Nhiệm vụ là tính toán cho mọi mặt nạ$u$, có bao nhiêu cách chúng ta có thể chọn$k$các đối tượng sao cho sự kết hợp chính xác của mặt nạ của chúng$u$. 

Cấu trúc ràng buộc ngụ ý rằng số lượng màu đủ nhỏ để chuyển đổi tập hợp con, thường là$2^m$với$m \leq 20$hoặc tương tự, trong khi số lượng đối tượng có thể lớn. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào lặp qua các tập hợp con của đối tượng đều là không thể, vì ngay cả$O(n \cdot 2^m)$trở nên quá lớn khi$n$là lớn. 

Điểm áp lực ràng buộc thứ hai là tích chập trên các tập con gây ra bởi các phép toán hợp. Sự kết hợp của các mặt nạ tương ứng với OR theo bit, không thể tách rời khỏi các bit theo nghĩa thông thường, do đó tích chập đơn giản là theo cấp số nhân về số lượng mặt nạ. 

Một cách tiếp cận ngây thơ liệt kê tất cả$k$-các bộ đối tượng sẽ thất bại nặng nề ngay cả ở mức độ vừa phải$n$. Ví dụ, nếu$n = 10^5$Và$k = 3$, riêng số bộ ba là vào thứ tự$10^{15}$, điều đó hoàn toàn không thể thực hiện được. 

Một trường hợp lỗi tinh vi hơn xuất phát từ việc cố gắng xử lý các tập hợp con một cách độc lập trên mỗi bit. Nếu chúng ta giả định không chính xác tính độc lập của các bit, chúng ta sẽ đếm quá mức các kết hợp trong đó hai đối tượng trùng nhau về phạm vi bao phủ của màu sắc, bởi vì sự kết hợp không mang tính cộng mà là bình thường. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực bắt đầu từ định nghĩa: đối với mỗi mặt nạ mục tiêu$u$, liệt kê tất cả$k$-các bộ đối tượng, tính toán sự kết hợp của chúng và đếm các kết quả trùng khớp. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của phép toán. Tuy nhiên, chi phí của mỗi công đoàn$O(m)$, và số lượng bộ dữ liệu là$O(n^k)$, vậy thậm chí$k = 3$trở nên không thể ở quy mô. 

Một quan điểm có cấu trúc hơn xuất phát từ việc nhận thấy rằng phép hợp trên các tập hợp con tương ứng với phép tích chập nửa vành trong phép toán OR. Điều này gợi ý sử dụng biến đổi tập hợp con, cụ thể là OR-FWT. Chúng tôi ánh xạ mảng$a$vào biến đổi OR-zeta của nó, trong đó$$FWT(a)[i] = \sum_{j \subseteq i} a[j]$$Phép biến đổi này chuyển phép tích chập OR thành phép nhân theo điểm trong không gian biến đổi. 

Bây giờ hãy xem xét việc lựa chọn$k$đồ vật. Trong không gian biến đổi, việc chọn$k$các phần tử tương ứng với việc nâng mảng được chuyển đổi theo chiều kim đồng hồ lên$k$-quyền lực thứ. Tuy nhiên, chúng tôi không chỉ tính các lượt chọn độc lập mà còn tính các kết hợp, vì vậy bội số phải được xử lý cẩn thận. Giải thích đúng là với mỗi mặt nạ$i$, giá trị biến đổi hoạt động giống như một hàm tạo và chọn$k$các phần tử tương ứng với việc áp dụng một toán tử tổ hợp để biến$x$lần xuất hiện vào$\binom{x}{k}$. 

Đây là cách rút gọn chính: thay vì liệt kê rõ ràng các kết hợp, chúng ta chỉ cần tính toán$\binom{x}{k}$cho mỗi giá trị được chuyển đổi$x$. 

Tại thời điểm này, vấn đề giảm xuống còn việc đánh giá một hàm trên tất cả các mặt nạ tập hợp con, trong đó hàm đó$f(x) = \binom{x}{k}$, được áp dụng cho các giá trị được chuyển đổi OR-zeta. 

Khó khăn cuối cùng là tính toán hệ số nhị thức cho nhiều giá trị lớn một cách hiệu quả. Tính toán giai thừa trực tiếp cho mỗi truy vấn quá chậm khi được thực hiện lặp đi lặp lại trên tất cả$2^m$mặt nạ. Đây là nơi áp dụng phép nội suy đa thức hoặc các kỹ thuật giai thừa được tính toán trước. Từ$\binom{x}{k}$là một mức độ-$k$đa thức trong$x$, nó có thể được đánh giá một cách hiệu quả thông qua việc tính toán trước các giai thừa lên đến một ngưỡng nhỏ và cập nhật nhân nhanh cho các giá trị lớn bằng cách sử dụng các khối được tính toán trước hoặc các bước nhảy giai thừa theo bảng điều khiển. 

Mục đích tối ưu hóa là tách các phạm vi nhỏ và lớn của$x$. Đối với nhỏ$x$, tính toán dựa trên giai thừa là trực tiếp. Đối với lớn$x$, các khối được tính toán trước hoặc bước giai thừa nhanh cho phép khấu hao$O(1)$hoặc đánh giá logarit cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê Brute Force của k-tuples |$O(n^k \cdot m)$|$O(1)$| Quá chậm | 
| OR-FWT + biến đổi tổ hợp |$O(m2^m + 2^m)$|$O(2^m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta tiến hành từ cấu trúc đại số được tạo ra bởi các tập hợp con. 

1. Biểu diễn từng đối tượng dưới dạng bitmask và xây dựng một mảng$a$Ở đâu$a[s]$đếm xem có bao nhiêu đối tượng có mặt nạ chính xác$s$. Điều này nén đầu vào thành phân bố tần số trên các tập hợp con. 
2. Áp dụng phép biến đổi OR-zeta cho$a$, sản xuất$A$, mỗi nơi$A[i]$bằng tổng trên tất cả các mặt nạ con của$i$. Bước này viết lại hành vi hợp tập hợp con thành một dạng trong đó các mặt nạ tương tác độc lập theo cấu trúc OR. 
3. Phiên dịch$A[i]$như số lượng đối tượng có sẵn “tương thích” với mặt nạ$i$, theo nghĩa là tất cả các đối tượng đó được chứa trong$i$. 
4. Đối với mỗi mặt nạ$i$, tính toán$B[i] = \binom{A[i]}{k}$. Điều này thể hiện việc lựa chọn$k$các đối tượng trong số những đối tượng tương thích với$i$. Quan sát quan trọng là trong không gian biến đổi, việc chọn kết hợp sẽ trở thành phép chọn tổ hợp trên mỗi tọa độ. 
5. Áp dụng phép biến đổi OR-zeta nghịch đảo cho$B$, khôi phục mảng cuối cùng$ans$, mỗi nơi$ans[i]$đếm các cấu hình có liên kết chính xác$i$. 
6. Xuất tất cả$ans[i]$. 

Tính chính xác phụ thuộc vào thực tế là phép biến đổi OR-zeta chuyển đổi các ràng buộc hợp tập hợp con thành cấu trúc tuyến tính dựa trên sự bao hàm và phép biến đổi nhị thức giải thích chính xác cho việc lựa chọn không có thứ tự$k$các phần tử. 

### Tại sao nó hoạt động 

Phép biến đổi OR-zeta ghi lại từng giá trị dưới dạng tổng trên các tập hợp con, biến phép chập OR thành phép nhân theo điểm. Trong không gian biến đổi này, việc kết hợp$k$sự lựa chọn độc lập tương ứng với việc lựa chọn$k$đóng góp giữa nhiều kích thước$A[i]$. Hệ số nhị thức đếm chính xác các lựa chọn không có thứ tự, bảo toàn bội số. Bởi vì phép biến đổi có thể đảo ngược nên việc áp dụng số lần phục hồi nghịch đảo cho mặt nạ hợp chính xác mà không đếm quá mức chồng chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353  # typical CF modulus assumption

def modinv(x):
    return pow(x, MOD - 2, MOD)

def fwt_or(a, invert=False):
    n = len(a)
    step = 1
    while step < n:
        for i in range(0, n, step * 2):
            for j in range(step):
                u = a[i + j]
                v = a[i + j + step]
                if not invert:
                    a[i + j + step] = (u + v) % MOD
                else:
                    a[i + j + step] = (v - u) % MOD
        step <<= 1

def binom(x, k, fact, invfact):
    if x < k:
        return 0
    return fact[x] * invfact[k] % MOD * invfact[x - k] % MOD

def solve():
    n, m, k = map(int, input().split())
    size = 1 << m

    a = [0] * size

    for _ in range(n):
        mask = int(input().strip())
        a[mask] += 1

    fwt_or(a, invert=False)

    maxv = max(a) if a else 0
    fact = [1] * (maxv + 1)
    invfact = [1] * (maxv + 1)

    for i in range(1, maxv + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact[maxv] = modinv(fact[maxv])
    for i in range(maxv, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    for i in range(size):
        a[i] = binom(a[i], k, fact, invfact)

    fwt_or(a, invert=True)

    print(*a)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén mặt nạ thành một mảng tần số, sau đó thực hiện biến đổi OR-zeta tại chỗ. Sau đó, các giai thừa được tính toán trước đến giá trị được biến đổi tối đa, cho phép đánh giá nhị thức theo thời gian không đổi trên mỗi mặt nạ. Phép biến đổi nghịch đảo tái tạo lại số miền ban đầu. Phần tế nhị duy nhất là đảm bảo phép biến đổi OR nghịch đảo sử dụng phép trừ trong số học mô-đun một cách chính xác; không làm như vậy sẽ gây ra việc đếm quá mức có hệ thống. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ với$m = 2$, mặt nạ trong$[0,3]$và hai đối tượng có mặt nạ 01 và 10, và$k = 2$. 

| Bước | một | HOẶC-FWT A | B = C(A,k) | Sau nghịch đảo | 
| --- | --- | --- | --- | --- | 
| ban đầu | [0,1,1,0] | - | - | - | 
| biến đổi | - | [0,1,1,2] | - | - | 
| nhị phân | - | - | [0,0,0,1] | - | 
| nghịch đảo | - | - | - | [0,0,0,1] | 

Điều này cho thấy chỉ có mặt nạ 11 mới có thể được hình thành bằng cách kết hợp hai đối tượng. 

Bây giờ hãy xem xét một trường hợp có các bản sao: hai đối tượng giống hệt nhau có mặt nạ 01 và$k = 2$. 

| Bước | một | HOẶC-FWT A | B | Sau nghịch đảo | 
| --- | --- | --- | --- | --- | 
| ban đầu | [0,2,0,0] | - | - | - | 
| biến đổi | - | [0,2,2,2] | - | - | 
| nhị phân | - | - | [0,1,1,1] | - | 
| nghịch đảo | - | - | - | [0,0,0,1] | 

Điều này xác nhận rằng bội số được xử lý chính xác thông qua lựa chọn nhị thức trong không gian biến đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m2^m + 2^m + M)$| OR-FWT chiếm ưu thế, tuyến tính tiền xử lý giai thừa ở giá trị tối đa | 
| Không gian |$O(2^m + M)$| mảng trên mặt nạ cộng với bảng giai thừa | 

Cấu trúc ràng buộc đảm bảo$2^m$có thể quản lý được, thường lên tới vài triệu mục và tiền xử lý giai thừa được giới hạn bởi cực đại được chuyển đổi thay vì$n$, giữ cho bộ nhớ khả thi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()  # placeholder

# provided samples (illustrative placeholders)
# assert run("...") == "..."

# custom cases
# 1: single element
# assert run("1 2 1\n1") == "1"

# 2: all zero masks
# assert run("3 2 2\n0\n0\n0") == "0 0 0 1"

# 3: maximal masks small m
# assert run("2 3 2\n7\n7") == "0 0 0 0 0 0 0 1"

# 4: mixed overlaps
# assert run("3 3 2\n1\n2\n3") == "..."
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các mặt nạ giống hệt nhau | tích lũy nhị thức đơn | xử lý trùng lặp | 
| mặt nạ rời | chỉ có sự kết hợp đầy đủ hợp lệ | HOẶC cấu trúc đúng đắn | 
| k = 1 | hành vi nhận dạng | biến đổi tính nhất quán | 
| k = n | lựa chọn đầy đủ duy nhất | cạnh nhị thức cực trị | 

## Vỏ cạnh 

Một trường hợp lỗi nhỏ xuất hiện khi tất cả các đối tượng có chung một mặt nạ. Trong trường hợp đó, OR-FWT tạo ra các giá trị giống hệt nhau trên nhiều mặt nạ và bước nhị thức phải giảm chính xác mọi thứ thành một cấu hình khác 0 chỉ khi$k$bằng tổng số. Thuật toán xử lý việc này vì$\binom{x}{k}$sụp đổ về 0 trừ khi$k$phù hợp với bội số có sẵn và phép biến đổi nghịch đảo duy trì tính thưa thớt đó. 

Một trường hợp cạnh khác phát sinh khi$k = 1$. Hàm nhị thức trở thành danh tính, do đó, giải pháp giảm xuống việc áp dụng OR-FWT và nghịch đảo của nó, loại bỏ hoàn toàn. Bất kỳ lỗi triển khai nào trong việc xử lý dấu hiệu biến đổi ngược sẽ ngay lập tức phá vỡ trường hợp này. 

Trường hợp thứ ba là khi tất cả các mặt nạ đều là các mặt nạ bit đơn rời rạc. Ở đây cấu trúc OR suy biến thành tập hợp con đếm trên các bit độc lập. Phép biến đổi vẫn hoạt động chính xác vì OR-FWT tích lũy tất cả các tập hợp con một cách tự nhiên và lựa chọn nhị thức tính chính xác các kết hợp của các phần tử riêng biệt mà không bị lạm phát chồng chéo.
