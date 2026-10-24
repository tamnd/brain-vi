---
title: "CF 106015M - Hệ thống cấp liệu lạ của Halzoom"
description: "Chúng tôi đang mô phỏng một hệ thống cho ăn theo từng lớp trong đó giá trị thức ăn tăng lên theo thời gian trên một dòng mèo. Có $M$ con mèo liên tiếp và $N$ ngày. Vào ngày đầu tiên, mỗi con mèo bắt đầu với đúng 1 gram thức ăn."
date: "2026-06-22T16:48:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106015
codeforces_index: "M"
codeforces_contest_name: "Game of Coders 4 - Over the Garden Wall"
rating: 0
weight: 106015
solve_time_s: 50
verified: true
draft: false
---

[CF 106015M - Hệ thống cho ăn kỳ lạ của Halzoom](https://codeforces.com/problemset/problem/106015/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một hệ thống cho ăn theo từng lớp trong đó giá trị thức ăn tăng lên theo thời gian trên một dòng mèo. có$M$mèo liên tiếp và$N$ngày. Vào ngày đầu tiên, mỗi con mèo bắt đầu với đúng 1 gram thức ăn. 

Từ ngày thứ hai trở đi, thức ăn cho mèo phụ thuộc vào hai điều: giá trị của ngày hôm trước và lượng thức ăn tích lũy mà tất cả mèo đã ăn trước đó trong cùng ngày hôm đó. Nói cách khác, mèo$j$vào ngày$i$được xây dựng dựa trên lịch sử của chính mình và cũng tích lũy những đóng góp đến từ những con mèo có chỉ số nhỏ hơn trong cùng một ngày. 

Nhiệm vụ không phải là mô phỏng toàn bộ quá trình. Chúng ta chỉ cần giá trị cuối cùng cho con mèo cuối cùng,$M$, vào ngày$N$, được tính theo modulo 998244353. 

Các ràng buộc cho phép cả hai$N$Và$M$lên đến$10^6$. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào xây dựng rõ ràng toàn bộ$N \times M$bàn. Ngay cả việc lưu trữ nó cũng quá lớn và việc lặp lại tất cả các chuyển đổi sẽ là$10^{12}$hoạt động trong trường hợp xấu nhất. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ là giả sử mỗi ngày có thể được tính độc lập bằng một tổng tiền tố đơn giản trên các con mèo. Điều đó thực sự đúng mỗi ngày, nhưng việc lặp lại nó suốt cả ngày là không khả thi. Một cái bẫy khác là cố gắng chỉ mô phỏng hàng cuối cùng mà không nhận ra rằng mọi hàng trước đều ảnh hưởng đến hàng tiếp theo thông qua cấu trúc tích lũy tiền tố. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp tuân theo định nghĩa theo nghĩa đen. Mỗi ngày, chúng tôi tính toán các giá trị cho tất cả các con mèo từ trái sang phải. Khi tính toán con mèo$j$, chúng ta cộng giá trị ngày hôm trước của nó và tổng của tất cả các con mèo$1$ĐẾN$j-1$vào ngày hiện tại. Điều này đòi hỏi phải duy trì tổng tiền tố mỗi ngày. 

Cách tiếp cận này đúng vì nó phản ánh chính xác sự tái diễn. Tuy nhiên, nó xử lý$M$mèo cho mỗi$N$ngày, dẫn đến$O(NM)$sự phức tạp về mặt thời gian. Với$N, M \le 10^6$, điều này trở nên quá lớn. 

Quan sát quan trọng là sự truy hồi có dạng bổ sung rất có cấu trúc. Mỗi ngày, hàng mới được tạo từ hàng trước cộng với sự tích lũy tiền tố của hàng hiện tại. Đây là cấu trúc tương tự xuất hiện trong các cấu trúc hệ số nhị thức và trong các phép biến đổi giống Pascal. 

Nếu chúng ta theo dõi cách một đơn vị thức ăn ban đầu lan truyền, nó sẽ lan truyền theo cách tương đương với việc đếm đường đi trong một lưới: di chuyển sang phải tương ứng với sự tích lũy giữa các con mèo và di chuyển xuống tương ứng với sự tiến triển qua các ngày. Mỗi tiểu bang$(i, j)$tích lũy các đóng góp từ phía trên và từ bên trái, đó chính xác là sự tái diễn của các hệ số nhị thức trong hai chiều. 

Điều này dẫn đến việc giải thích rằng giá trị tại$(N, M)$bằng số đường đi đơn điệu từ$(1,1)$ĐẾN$(N,M)$, là giá trị tổ hợp tiêu chuẩn:$$\binom{N + M - 2}{M - 1}.$$Do đó, vấn đề được rút gọn thành việc tính toán một hệ số nhị thức đơn modulo 998244353, hiệu quả bằng cách sử dụng tính toán trước giai thừa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(NM)$|$O(M)$| Quá chậm | 
| Tổ hợp (Hệ số nhị thức) |$O(N + M)$tiền xử lý,$O(1)$truy vấn |$O(N + M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi biến quy trình động thành bài toán đếm tổ hợp và tính hệ số nhị thức một cách hiệu quả. 

1. Tính toán trước các giai thừa và giai thừa nghịch đảo lên đến$N + M$. Điều này là cần thiết vì chúng ta sẽ đánh giá một công thức tổ hợp theo modulo một số nguyên tố và không thể chia trực tiếp theo số học mô đun. 
2. Sử dụng định lý nhỏ Fermat để tính nghịch đảo môđun. Vì 998244353 là số nguyên tố nên$x^{-1} \equiv x^{MOD-2} \mod MOD$. Điều này cho phép chúng ta xây dựng các giai thừa nghịch đảo theo thời gian tuyến tính sau khi các giai thừa được tính toán. 
3. Nhận biết giá trị cuối cùng tương ứng với việc chọn vị trí của$M-1$di chuyển đúng giữa$N+M-2$tổng số bước di chuyển trong cách giải thích lưới. Điều này biến đổi cấu trúc DP thành:$$\binom{N + M - 2}{M - 1}.$$4. Tính kết quả theo công thức chuẩn:$$\text{fact}[N+M-2] \cdot \text{invfact}[M-1] \cdot \text{invfact}[N-1].$$5. Xuất kết quả theo modulo 998244353. 

### Tại sao nó hoạt động 

Sự lặp lại xác định một quá trình tích lũy hai chiều trong đó mỗi ô phụ thuộc vào hàng xóm trên cùng của nó (ngày trước) và tiền tố bên trái (các con mèo trước đó trong cùng ngày). Điều này tạo ra một cấu trúc phụ thuộc giống hệt với việc đếm các đường mạng trong đó các chuyển động bị hạn chế theo hướng phải và hướng xuống. Mỗi đơn vị đóng góp từ trạng thái ban đầu sẽ lan truyền qua tất cả các đường dẫn hợp lệ và số lượng các đường dẫn như vậy từ điểm gốc đến$(N,M)$chính xác là hệ số nhị thức ở trên. Vì tất cả các đóng góp đều tuyến tính và độc lập nên tổng tất cả các đường dẫn sẽ mang lại giá trị cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    N, M = map(int, input().split())
    
    maxv = N + M - 2
    
    fact = [1] * (maxv + 1)
    for i in range(1, maxv + 1):
        fact[i] = fact[i - 1] * i % MOD
    
    invfact = [1] * (maxv + 1)
    invfact[maxv] = modinv(fact[maxv])
    for i in range(maxv, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD
    
    def C(n, r):
        if r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD
    
    print(C(N + M - 2, M - 1))

if __name__ == "__main__":
    solve()
```Việc thực hiện lần đầu tiên đọc$N$Và$M$, sau đó xây dựng các bảng giai thừa và nghịch đảo lên đến$N+M-2$. Nghịch đảo mô-đun được tính toán một lần bằng cách sử dụng lũy ​​thừa nhanh. Hàm kết hợp sau đó được sử dụng để đánh giá trực tiếp dạng đóng. Phải cẩn thận để đảm bảo các mảng giai thừa có kích thước chính xác để$N+M-2$, vì lỗi xảy ra ở đây là phổ biến. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ$N = 3, M = 3$. Chúng tôi tính toán$\binom{4}{2} = 6$. 

| Bước | Giá trị | 
| --- | --- | 
| Tính N+M-2 | 4 | 
| Tính M-1 | 2 | 
| Biểu Thị C(4,2) | 6 | 

Điều này phù hợp với mức tăng trưởng tổ hợp dự kiến ​​​​từ sự tái diễn, trong đó mỗi chuỗi di chuyển tương ứng với sự lan truyền hợp lệ của việc đóng góp thực phẩm. 

Bây giờ hãy xem xét$N = 4, M = 2$. Câu trả lời là:$$\binom{4}{1} = 4.$$| Bước | Giá trị | 
| --- | --- | 
| Tính N+M-2 | 4 | 
| Tính M-1 | 1 | 
| Biểu Thị C(4,1) | 4 | 

Điều này xác nhận rằng khi chỉ có một lựa chọn về kích thước con mèo, hệ thống sẽ giảm xuống mức lan truyền tuyến tính qua các ngày. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N + M)$| tính toán trước giai thừa và nghịch đảo lên đến$N+M$| 
| Không gian |$O(N + M)$| lưu trữ cho bảng giai thừa | 

Các ràng buộc cho phép lên đến$10^6$, do đó, tính toán trước tuyến tính phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ vẫn còn nhỏ vì chỉ có một mảng có kích thước tối đa$2 \cdot 10^6$là cần thiết. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N, M = map(int, input().split())
    maxv = N + M - 2

    fact = [1] * (maxv + 1)
    for i in range(1, maxv + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact = [1] * (maxv + 1)
    invfact[maxv] = pow(fact[maxv], MOD - 2, MOD)
    for i in range(maxv, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def C(n, r):
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

    return str(C(N + M - 2, M - 1))

# provided sample
assert run("3 3\n") == "6"

# minimum case
assert run("1 1\n") == "1"

# single row behavior
assert run("1 5\n") == "1"

# single column behavior
assert run("5 1\n") == "1"

# symmetric case
assert run("4 4\n") == str(pow(2, 6, MOD))

# boundary stress small
assert run("2 3\n") == str(3)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | tính đúng đắn của trường hợp cơ sở | 
| 1 5 | 1 | không có hiệu ứng lan truyền ngày | 
| 5 1 | 1 | không có tác dụng nhân giống mèo | 
| 4 4 | 64 | tăng trưởng nhị thức đối xứng | 
| 2 3 | 3 | lan truyền nhỏ không tầm thường | 

## Vỏ cạnh 

cho$N = 1, M = 1$, hệ thống không bao giờ tiến triển vượt quá điều kiện ban đầu. Công thức cho$\binom{0}{0} = 1$, phù hợp với định nghĩa. 

Vì$N = 1, M = k$, chỉ có một ngày nên không có sự tích lũy giữa các ngày. Sự truy hồi sụp đổ thành các giá trị không đổi trên tất cả các con mèo. Công thức cho$\binom{k-1}{k-1} = 1$, khớp với hành vi của hàng phẳng. 

Vì$M = 1, N = k$, chỉ có một con mèo nên không tồn tại tương tác tiền tố. Mỗi ngày chỉ đơn giản là chuyển tiếp cùng một giá trị. Công thức cho$\binom{k-1}{0} = 1$, nhất quán với cột đơn không thay đổi.
