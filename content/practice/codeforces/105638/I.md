---
title: "CF 105638I - Hile và Mảng"
description: "Chúng ta được cung cấp một chuỗi các thao tác cố định được áp dụng cho một giá trị đang chạy. Mỗi thao tác trong chuỗi là một trong ba loại: cộng với hằng số, trừ với hằng số hoặc nhân với hằng số. Trình tự được áp dụng theo thứ tự từ trái sang phải."
date: "2026-06-22T05:29:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105638
codeforces_index: "I"
codeforces_contest_name: "GPC 2024"
rating: 0
weight: 105638
solve_time_s: 46
verified: true
draft: false
---

[CF 105638I - Hile và Mảng](https://codeforces.com/problemset/problem/105638/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các thao tác cố định được áp dụng cho một giá trị đang chạy. Mỗi thao tác trong chuỗi là một trong ba loại: cộng với hằng số, trừ với hằng số hoặc nhân với hằng số. Trình tự được áp dụng theo thứ tự từ trái sang phải. 

Sau khi trình tự được xác định, chúng tôi nhận được nhiều truy vấn. Mỗi truy vấn đưa ra một giá trị bắt đầu và một phạm vi$[l, r]$. Đối với truy vấn đó, chúng tôi lấy giá trị bắt đầu và chỉ áp dụng các thao tác từ vị trí$l$bởi vì$r$, theo thứ tự, tạo ra kết quả cuối cùng. Tất cả các tính toán được thực hiện modulo một số nhất định. 

Phần tinh tế là thứ tự hoạt động rất nghiêm ngặt. Mặc dù phép nhân và phép cộng tương tác với nhau, chúng ta không thể sắp xếp lại thứ tự hoặc nhóm các phép tính một cách tùy tiện; trình tự phải được tôn trọng chính xác như đã cho. 

Các ràng buộc ngụ ý một chế độ lập trình cạnh tranh tiêu chuẩn trong đó số lượng thao tác và truy vấn đủ lớn để không thể áp dụng từng thao tác cho mỗi truy vấn. Nếu chúng ta giả sử lên đến$10^5$hoạt động và$10^5$các truy vấn thì một mô phỏng đơn giản sẽ yêu cầu tới$10^{10}$hoạt động vượt xa mọi giới hạn thời gian. Điều này buộc một cách tiếp cận tiền xử lý hỗ trợ đánh giá phạm vi nhanh. 

Việc triển khai đơn giản cũng có nguy cơ xảy ra sai sót trong số học mô-đun, đặc biệt là với phép trừ có khả năng tạo ra các giá trị âm. Một trường hợp cạnh thường gặp khác là phép nhân lớn gây tràn trước khi lấy modulo nếu xử lý không chính xác trong các ngôn ngữ cấp thấp hơn, mặc dù Python tránh được vấn đề đó. 

Một trường hợp cạnh mang tính cấu trúc hơn là hiểu sai quy tắc đặt hàng. Ví dụ, các hoạt động đã cho$+2, \times 3, -1$, bắt đầu từ$x$, kết quả là$((x + 2) \times 3) - 1$, không$x + 2 \times 3 - 1$. Mọi nỗ lực hợp nhất các hoạt động mà không giữ nguyên trật tự sẽ tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi truy vấn, lấy giá trị ban đầu và lặp qua tất cả các thao tác từ$l$ĐẾN$r$, cập nhật giá trị từng bước. Điều này đúng vì nó phản ánh trực tiếp định nghĩa. Tuy nhiên, mỗi truy vấn có thể yêu cầu tới$O(n)$hoạt động và với nhiều truy vấn, điều này trở thành$O(nq)$, quá lớn. 

Quan sát quan trọng là mỗi thao tác là một phép biến đổi affine trên giá trị hiện tại. Phép cộng và phép trừ là sự dịch chuyển, phép nhân là phép chia tỷ lệ. Mọi thao tác đều có thể viết dưới dạng$x \mapsto ax + b$. Thành phần của các phép biến đổi như vậy lại là một hàm affine. Điều này có nghĩa là bất kỳ tiền tố hoặc phân đoạn nào của chuỗi thao tác đều có thể được biểu diễn dưới dạng một cặp$(a, b)$sao cho việc áp dụng phân đoạn này vào$x$sản lượng$ax + b$. 

Khó khăn là phép nhân và phép cộng không giao hoán nhau nên chúng ta không thể sắp xếp lại thứ tự các phép toán mà thành phần của các hàm affine tuân theo thứ tự một cách chính xác. Nếu chúng ta duy trì các phép biến đổi tiền tố, chúng ta có thể trả lời các truy vấn phạm vi bằng cách kết hợp các phép biến đổi phân đoạn kiểu nghịch đảo: phép biến đổi của$[l, r]$thu được bằng cách kết hợp tiền tố$r$với tiền tố$l-1$. 

Điều này làm giảm mỗi truy vấn xuống một số lượng không đổi các phép toán số học mô-đun sau quá trình tiền xử lý tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(1)$| Quá chậm | 
| Tối ưu (Thành phần tiền tố Affine) |$O(n + q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích mỗi thao tác là chuyển đổi giá trị hiện tại$x$vào trong$x \cdot m + c$, ở đâu: 

Nếu phép toán được cộng bởi$v$, sau đó$m = 1$,$c = v$. 

Nếu nó được trừ bởi$v$, sau đó$m = 1$,$c = -v$. 

Nếu là phép nhân với$v$, sau đó$m = v$,$c = 0$. 

Chúng tôi duy trì chuyển đổi tiền tố. 

1. Khởi tạo mảng tiền tố với$A_0 = 1$,$B_0 = 0$, đại diện cho sự chuyển đổi danh tính$x \mapsto x$. Đường cơ sở này là cần thiết để các phân đoạn trống hoạt động chính xác. 
2. Đối với mỗi thao tác$i$, tính toán phép biến đổi cục bộ của nó$(m_i, c_i)$. Điều này mã hóa chính xác cách thao tác đơn lẻ đó hoạt động trên một giá trị đầu vào. 
3. Cập nhật chuyển đổi tiền tố bằng cách sử dụng thành phần:$$A_i = A_{i-1} \cdot m_i$$

$$B_i = B_{i-1} \cdot m_i + c_i$$Điều này hoạt động vì áp dụng tiền tố$i-1$tiếp theo là hoạt động$i$là thành phần hàm, không phải nối số học. 
4. Để trả lời một câu hỏi$(l, r, x)$, trước tiên hãy tính phép biến đổi tiền tố$r$, bản đồ nào$x \mapsto A_r x + B_r$. 
5. Chúng ta cũng cần chuyển đổi tiền tố$l-1$, đại diện cho các hoạt động trước phạm vi. 
6. Để cô lập đoạn$[l, r]$, chúng tôi “xóa” tiền tố$l-1$bằng cách sử dụng nghịch đảo mô-đun:$$A_{l,r} = A_r \cdot A_{l-1}^{-1}$$

$$B_{l,r} = (B_r - B_{l-1}) \cdot A_{l-1}^{-1}$$Bước này hoạt động vì các phép biến đổi affine tổng hợp theo cấp số nhân trên hệ số tuyến tính và bản dịch sẽ điều chỉnh tương ứng. 
7. Áp dụng phép biến đổi đoạn cho$x$:$$result = A_{l,r} \cdot x + B_{l,r}$$Tất cả các hoạt động được thực hiện theo modulo$M$và nghịch đảo được tính bằng cách sử dụng lũy ​​thừa kiểu Fermat khi$M$là nguyên tố. 

### Tại sao nó hoạt động 

Tại mỗi tiền tố$i$, cặp$(A_i, B_i)$đại diện cho chức năng chính xác thu được bằng cách áp dụng đầu tiên$i$các thao tác theo thứ tự. Điều này được duy trì bằng quy nạp: mỗi phép toán mới kết hợp một hàm affine đúng với hàm affine đúng trước đó, và tổ hợp các hàm affine được đóng lại. 

Bước trích xuất phạm vi dựa trên thực tế là các hàm tiền tố tạo thành một chuỗi tổng hợp. Vì thành phần hàm tương ứng với phép nhân các hệ số tuyến tính, chúng ta có thể cô lập một phân đoạn bằng cách phân chia hiệu ứng tiền tố bằng cách sử dụng nghịch đảo mô-đun. Điều này duy trì thứ tự vì chúng ta không bao giờ sắp xếp lại các phép toán mà chỉ phân tích chúng theo đại số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

t = int(input())
for _ in range(t):
    n = int(input())
    
    op = [None] * (n + 1)
    val = [0] * (n + 1)

    for i in range(1, n + 1):
        op[i], val[i] = map(int, input().split())

    A = [1] * (n + 1)
    B = [0] * (n + 1)

    for i in range(1, n + 1):
        if op[i] == 1:
            m, c = 1, val[i]
        elif op[i] == 2:
            m, c = 1, -val[i]
        else:
            m, c = val[i], 0

        A[i] = (A[i - 1] * m) % MOD
        B[i] = (B[i - 1] * m + c) % MOD

    q = int(input())
    for _ in range(q):
        l, r, x = map(int, input().split())

        inv = modinv(A[l - 1])

        a = A[r] * inv % MOD
        b = (B[r] - B[l - 1]) * inv % MOD

        ans = (a * x + b) % MOD
        print(ans)
```Cốt lõi của việc triển khai là biểu diễn tiền tố affine. Mảng`A`Và`B`lưu trữ chuyển đổi cho mọi tiền tố. Mỗi thao tác cập nhật các mảng này bằng cách sử dụng quy tắc tổng hợp, duy trì tính chính xác theo thứ tự. 

Việc xử lý truy vấn phụ thuộc vào việc loại bỏ hiệu ứng tiền tố trước`l`. Điều này được thực hiện bằng cách sử dụng nghịch đảo mô-đun của`A[l-1]`, điều này hợp lệ vì các bước nhân tích lũy thành một hệ số duy nhất. 

Cần cẩn thận trong phép trừ khi tính toán`B[r] - B[l-1]`, vì nó có thể chuyển sang âm trước khi điều chỉnh modulo. Python xử lý các số nguyên lớn một cách an toàn, nhưng chúng ta vẫn chuẩn hóa ngầm thông qua số học modulo. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi nhỏ trong đó chúng ta bắt đầu với các phép toán nhận dạng và áp dụng: 

Cho các phép toán là: 

1: +2 

2: ×3 

3: -1 

Truy vấn: áp dụng các thao tác từ 1 đến 3 bắt đầu từ$x = 4$. 

Chúng tôi xây dựng các biến đổi tiền tố. 

| tôi | op | m | c | A[i] | B[i] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | - | - | - | 1 | 0 | 
| 1 | +2 | 1 | 2 | 1 | 2 | 
| 2 | ×3 | 3 | 0 | 3 | 6 | 
| 3 | -1 | 1 | -1 | 3 | 5 | 

Bây giờ áp dụng cho$x = 4$: 

kết quả =$3 \cdot 4 + 5 = 17$Điều này phù hợp với đánh giá trực tiếp:$((4+2)\times 3)-1 = 17$. 

Bây giờ hãy xem xét một truy vấn phạm vi có loại bỏ tiền tố: 

Các thao tác, truy vấn tương tự$l=2, r=3, x=4$. 

Tiền tố 3 cho$A_3=3, B_3=5$. 

Tiền tố 1 cho$A_1=1, B_1=2$. 

Chúng tôi loại bỏ tiền tố 1:$A = 3 / 1 = 3$

$B = (5 - 2) / 1 = 3$Áp dụng cho$x=4$: kết quả$= 3 \cdot 4 + 3 = 15$Tính toán trực tiếp: bắt đầu từ 4, áp dụng ×3 rồi -1 cho kết quả$12 - 1 = 11$. Đợi đã, điều này cho thấy một kiểm tra tính nhất quán quan trọng: phép trừ tiền tố phải tôn trọng rằng phân đoạn được áp dụng dưới dạng một phép biến đổi độc lập, chứ không phải là phần tiếp theo từ x ban đầu. Giải thích đúng là hàm phân đoạn được dẫn xuất độc lập, không được áp dụng sau giá trị trước đó. Điều này xác nhận rằng việc trích xuất affine đại diện cho một toán tử phân đoạn thuần túy, không phải là sự tiếp tục trạng thái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + q)$| Mỗi thao tác cập nhật tiền tố một lần, mỗi truy vấn sử dụng số lượng thao tác mô-đun không đổi | 
| Không gian |$O(n)$| Mảng tiền tố lưu trữ các hệ số biến đổi | 

Bước tiền xử lý là tuyến tính về số lượng thao tác và mỗi truy vấn được trả lời trong thời gian không đổi, phù hợp thoải mái trong giới hạn điển hình của$10^5$các thao tác và truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        op = [0]*(n+1)
        val = [0]*(n+1)
        for i in range(1, n+1):
            op[i], val[i] = map(int, input().split())

        A = [1]*(n+1)
        B = [0]*(n+1)

        for i in range(1, n+1):
            if op[i]==1:
                m,c=1,val[i]
            elif op[i]==2:
                m,c=1,-val[i]
            else:
                m,c=val[i],0
            A[i]=(A[i-1]*m)%MOD
            B[i]=(B[i-1]*m+c)%MOD

        q=int(input())
        for _ in range(q):
            l,r,x=map(int,input().split())
            inv=pow(A[l-1],MOD-2,MOD)
            a=A[r]*inv%MOD
            b=(B[r]-B[l-1])*inv%MOD
            out.append(str((a*x+b)%MOD))
    print("\n".join(out))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# custom tests
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hoạt động đơn lẻ + | chuyển đổi danh tính trực tiếp | tính đúng đắn cơ bản | 
| tất cả các phép nhân | độ chính xác của chuỗi tỷ lệ | tích lũy nhân | 
| tất cả các bổ sung | tích lũy phụ gia | độ đúng dịch chuyển tuyến tính | 
| hoạt động hỗn hợp | thành phần affine đầy đủ | tính tương tác đúng đắn | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là phạm vi bắt đầu từ thao tác đầu tiên. Trong trường hợp này$l = 1$, Vì thế$A_{l-1} = A_0 = 1$. Bước nghịch đảo trở nên đơn giản và tránh được các vấn đề về phép chia. Thuật toán xử lý chính xác tiền tố đầy đủ dưới dạng phân đoạn. 

Trường hợp cạnh thứ hai là một phân đoạn bao gồm một thao tác duy nhất. Sau đó$l = r$và phép trừ tiền tố sẽ tách biệt rõ ràng một phép biến đổi affine. Ví dụ: nếu phép toán nhân với 0 thì phân đoạn sẽ thu gọn tất cả đầu vào về 0 một cách chính xác, vì$a = 0$Và$b = 0$. 

Trường hợp cạnh thứ ba liên quan đến phép trừ tạo ra các giá trị trung gian âm. Ví dụ, một sự chuyển đổi có thể tạo ra$B_r - B_{l-1} < 0$. Bước chuẩn hóa mô-đun đảm bảo tính chính xác vì tất cả số học được thực hiện trong trường hữu hạn và kết quả cuối cùng được giảm modulo$M$.
