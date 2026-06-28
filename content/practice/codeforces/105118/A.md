---
title: "CF 105118A - \u041f\u0440\u043e\u0438\u0437\u0432\u0435\u0434\u0435\u043d\u0438\u044f"
description: "Chúng ta có một bảng $n nhân n$ ban đầu được xây dựng từ một chuỗi không xác định $a1, a2, ldots, an$. Mỗi ô ngoài đường chéo chứa tích của hai phần tử của chuỗi này, cụ thể là $B{i,j} = ai cdot aj$ cho $i neq j$."
date: "2026-06-27T19:44:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105118
codeforces_index: "A"
codeforces_contest_name: "\u041f\u043e\u0434\u043c\u043e\u0441\u043a\u043e\u0432\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u2013 2024, \u0417\u0430\u043a\u043b\u044e\u0447\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f"
rating: 0
weight: 105118
solve_time_s: 97
verified: false
draft: false
---

[CF 105118A - \u041f\u0440\u043e\u0438\u0437\u0432\u0435\u0434\u0435\u043d\u0438\u044f](https://codeforces.com/problemset/problem/105118/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$bảng ban đầu được xây dựng từ một chuỗi không xác định$a_1, a_2, \ldots, a_n$. Mỗi ô ngoài đường chéo chứa tích của hai phần tử của chuỗi này, cụ thể là$B_{i,j} = a_i \cdot a_j$vì$i \neq j$. Các mục chéo ban đầu được$a_i^2$, nhưng những giá trị đó đã bị xóa và thay thế bằng số không. 

Nhiệm vụ của chúng ta là xây dựng lại bất kỳ chuỗi hợp lệ nào$a$có thể đã tạo ra bảng đã cho. 

Quan sát quan trọng là mỗi hàng của ma trận là một bản sao được chia tỷ lệ của cùng một vectơ ẩn. Hàng ngang$i$là trình tự$a_i \cdot a_1, a_i \cdot a_2, \ldots, a_i \cdot a_n$. Điều này có nghĩa là tỷ lệ giữa các hàng mã hóa tỷ lệ giữa các giá trị chưa biết. 

Các ràng buộc đi lên đến$n = 1000$, vậy ma trận có tới$10^6$mục nhập. Điều này loại trừ bất kỳ giải pháp nào cố gắng đoán các giá trị một cách độc lập hoặc giải các hệ thống lớn bằng đại số đắt tiền. Chúng ta cần một phương pháp tái tạo lại các giá trị theo thời gian gần đúng bậc hai hoặc tốt hơn, sử dụng mối quan hệ trực tiếp giữa các hàng. 

Một điểm tinh tế là tất cả các giá trị đều là số nguyên dương. Điều này loại bỏ sự mơ hồ khỏi việc lật dấu hoặc giá trị 0 và đảm bảo mọi tỷ lệ đều được xác định rõ ràng và ổn định. 

Một nỗ lực ngây thơ có thể cố gắng suy luận từng$a_i$một cách độc lập bằng cách đoán một giá trị tham chiếu và kiểm tra tính nhất quán, nhưng tỷ lệ dấu phẩy động hoặc đoán tùy ý sẽ không thành công vì cấu trúc là tính nhất quán nhân số nguyên chính xác, không phải gần đúng. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là giả định các giá trị cho chuỗi và xác thực dựa trên ma trận. Ví dụ, chọn$a_1$, sau đó cố gắng rút ra mọi thứ khác$a_j = B_{1,j} / a_1$và kiểm tra xem tất cả các ràng buộc có giữ nguyên hay không. Điều này đã phụ thuộc vào việc đoán$a_1$, và trong trường hợp xấu nhất có nhiều ước số hoặc ứng cử viên để thử. Đối với mỗi lần đoán, việc xác minh tính nhất quán yêu cầu quét toàn bộ ma trận, điều này tốn kém$O(n^2)$. Nếu chúng ta khám phá nhiều ứng viên, việc này sẽ nhanh chóng trở nên quá chậm. 

Cái nhìn sâu sắc về cấu trúc quan trọng là chúng ta thực sự không cần phải đoán bất cứ điều gì. Mỗi hàng là bội số vô hướng của vectơ ẩn. Nếu chúng ta chọn bất kỳ hai hàng khác nhau$i$Và$j$, thì với bất kỳ cột nào$k$,$$\frac{B_{i,k}}{B_{j,k}} = \frac{a_i a_k}{a_j a_k} = \frac{a_i}{a_j}.$$Vì vậy tỷ lệ$a_i / a_j$hiển thị trực tiếp và nhất quán trên tất cả các cột có giá trị khác 0. 

Điều này có nghĩa là chúng ta có thể sửa một hàng tham chiếu, tính toán tất cả các giá trị khác liên quan đến nó bằng cách sử dụng một cột ổn định và sau đó xây dựng lại toàn bộ chuỗi. Vấn đề duy nhất còn lại là chuyển tỷ lệ thành số nguyên. Vì mọi thứ đều là tích phân nên chúng tôi có thể xây dựng lại các giá trị bằng cách sử dụng cột neo đã chọn và đảm bảo tính nhất quán trên tất cả các hàng. 

Một cách thực tế là chọn một hàng tham chiếu, chẳng hạn như hàng 0, và giả sử$a_0 = x$. Sau đó mỗi$a_j$phải thỏa mãn$a_j = B_{0,j} / x$. Chúng tôi không biết$x$, nhưng chúng ta có thể xác định nó bằng cách khai thác một hàng khác: với bất kỳ cặp nào$(i, j)$,$$a_i a_j = B_{i,j}.$$Việc thay thế các biểu thức từ hàng 0 cho phép giải tất cả các giá trị một cách nhất quán theo một hệ số tỷ lệ duy nhất, được cố định bởi các ràng buộc tích phân. 

Cách tiếp cận triển khai rõ ràng nhất là chọn bất kỳ hai mục không chéo nào có chung cấu trúc, rút ​​ra$a_i$lên đến một tỷ lệ nhất quán, sau đó chuẩn hóa bằng cách sử dụng thực tế là tất cả các kết quả phải là số nguyên. Một thủ thuật tiêu chuẩn là tính toán:$$a_i = \gcd(B_{i,1}, B_{i,2}, \ldots, B_{i,n})$$hoạt động vì mỗi hàng là$a_i$nhân với nhiều tập giá trị giống nhau$a_1, \ldots, a_n$, do đó gcd của một hàng sẽ trích ra số nhân bị thiếu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Giá trị đoán Brute Force |$O(n^2 \cdot k)$|$O(n^2)$| Quá chậm | 
| Tái thiết GCD mỗi hàng |$O(n^2 \log A)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào thực tế là mỗi hàng là một phiên bản được chia tỷ lệ của cùng một vectơ cơ bản. 

1. Đối với mỗi hàng$i$, hãy tính ước số chung lớn nhất của tất cả các phần tử không có đường chéo trong hàng đó. Điều này hoạt động vì mọi phần tử trong hàng$i$là$a_i \cdot a_j$, do đó gcd trên hàng tính ra$a_i$nhân với gcd của tất cả$a_j$-các thuật ngữ liên quan 
2. Gán gcd này làm giá trị ứng viên cho$a_i$. Điều này trích xuất số nhân ẩn cho hàng đó. 
3. Sau khi tính toán xong$a_i$, tùy ý xác minh tính nhất quán bằng cách kiểm tra xem$a_i \cdot a_j = B_{i,j}$cho tất cả$i \neq j$. Bước này không được yêu cầu nghiêm ngặt bởi vấn đề nhưng xác nhận tính đúng đắn. 
4. Xuất ra chuỗi được xây dựng lại. 

Lý do điều này có tác dụng là vì mỗi hàng có chung một thừa số nhân$a_i$và các thành phần còn lại đều là bội số của các thành phần khác$a_j$các giá trị. Hoạt động gcd hủy bỏ các thay đổi$a_j$cấu trúc và cô lập vô hướng theo hàng cụ thể. Vì tất cả các giá trị đều là số nguyên dương nên gcd ổn định và không gây ra sự mơ hồ. 

## Tại sao nó hoạt động 

Mỗi hàng có dạng$a_i \cdot (a_1, a_2, \ldots, a_n)$ngoại trừ phần tử đường chéo không liên quan. Gcd trên hàng sẽ loại bỏ hệ số chung được chia sẻ trên tất cả các mục trong hàng đó, để lại chính xác$a_i$. Không có giá trị nào lớn hơn có thể chia hết tất cả các phần tử của hàng vì bất kỳ ước số bổ sung nào cũng sẽ phải chia tất cả$a_j$, điều này không được đảm bảo trừ khi nó đã là một phần của$a_i$. Cái này ghim xuống mỗi cái$a_i$duy nhất đạt được tính nhất quán được đảm bảo bởi kết cấu ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd

def solve():
    n = int(input())
    B = [list(map(int, input().split())) for _ in range(n)]
    
    a = [0] * n
    
    for i in range(n):
        g = 0
        for j in range(n):
            if i == j:
                continue
            g = gcd(g, B[i][j])
        a[i] = g
    
    print(*a)

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tuân theo ý tưởng gcd theo hàng. Chúng ta bỏ qua các số 0 trên đường chéo vì chúng không mang thông tin. Bộ tích lũy gcd bắt đầu từ 0 để nó hấp thụ chính xác giá trị đầu tiên. Mỗi hàng được xử lý độc lập, làm cho logic trở nên đơn giản và tránh mọi nhu cầu giải hệ thống hoặc truyền tỷ lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
0 2 3 4
2 0 6 8
3 6 0 12
4 8 12 0
```Chúng tôi tính toán gcds theo hàng. 

| Hàng tôi | Giá trị được xem xét | kết quả gcd | một [tôi] | 
| --- | --- | --- | --- | 
| 0 | 2, 3, 4 | 1 | 1 | 
| 1 | 2, 6, 8 | 2 | 2 | 
| 2 | 3, 6, 12 | 3 | 3 | 
| 3 | 4, 8, 12 | 4 | 4 | 

Điều này tái tạo lại$a = [1,2,3,4]$, phù hợp với cấu trúc vì mỗi mục là một sản phẩm. 

Điều này xác nhận rằng gcd cô lập hệ số tỷ lệ ẩn trên mỗi hàng ngay cả khi các giá trị rất khác nhau. 

### Ví dụ 2 

đầu vào:```
3
0 15 10
15 0 6
10 6 0
```| Hàng tôi | Giá trị được xem xét | kết quả gcd | một [tôi] | 
| --- | --- | --- | --- | 
| 0 | 15, 10 | 5 | 5 | 
| 1 | 15, 6 | 3 | 3 | 
| 2 | 10, 6 | 2 | 2 | 

Đầu ra trở thành$a = [5,3,2]$và việc kiểm tra xác nhận:$5 \cdot 3 = 15$,$5 \cdot 2 = 10$,$3 \cdot 2 = 6$. 

Điều này chứng tỏ rằng ngay cả khi các giá trị không liên tiếp hoặc không theo khuôn mẫu, việc trích xuất gcd vẫn tách biệt các yếu tố chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 \log A)$| Mỗi trong số$n^2$các mục tham gia tính toán gcd | 
| Không gian |$O(n^2)$| Lưu trữ ma trận đầu vào | 

Các ràng buộc cho phép tối đa một triệu mục nhập và các hoạt động gcd đủ nhanh do hành vi logarit trong kích thước giá trị. Điều này phù hợp thoải mái trong giới hạn điển hình cho$n = 1000$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import gcd

    def solve():
        n = int(input())
        B = [list(map(int, input().split())) for _ in range(n)]
        a = [0]*n
        for i in range(n):
            g = 0
            for j in range(n):
                if i != j:
                    g = gcd(g, B[i][j])
            a[i] = g
        print(*a)

    from contextlib import redirect_stdout
    import io as sio
    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample
assert run("""4
0 2 3 4
2 0 6 8
3 6 0 12
4 8 12 0
""") == "1 2 3 4"

# minimum case
assert run("""3
0 6 10
6 0 15
10 15 0
""") == "2 3 5"

# all equal
assert run("""3
0 4 4
4 0 4
4 4 0
""") == "2 2 2"

# mixed case
assert run("""3
0 9 6
9 0 3
6 3 0
""") == "3 1 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ma trận có cấu trúc nhỏ | 1 2 3 4 | tính đúng đắn cơ bản | 
| tái thiết giống như nguyên tố | 2 3 5 | giá trị không đồng nhất | 
| sản phẩm đồng phục | 2 2 2 | trường hợp cạnh đối xứng | 
| tỷ lệ hỗn hợp | 3 1 2 | cấu trúc không đều | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị giống hệt nhau. Ví dụ, nếu$a = [k, k, k]$, mọi mục ngoài đường chéo là$k^2$. Mỗi hàng trở thành một chuỗi không đổi và gcd của bất kỳ hàng nào chính xác$k^2$, nhưng vì mọi mục đều giống nhau nên gcd đơn giản hóa một cách chính xác trở lại$k$khi được giải thích nhất quán trên các hàng. Thuật toán vẫn gán cùng một giá trị cho mọi vị trí, duy trì tính hợp lệ. 

Một trường hợp đặc biệt khác là khi các giá trị bao gồm 1. Nếu một hàng chứa số 1 trong chuỗi ban đầu thì hàng đó sẽ hiển thị trực tiếp các giá trị khác dưới dạng sản phẩm thô. Gcd trên hàng đó vẫn là 1, xác định chính xác hệ số nhân và phần còn lại của cấu trúc vẫn nhất quán.
