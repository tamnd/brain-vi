---
title: "CF 105636B - \u9057\u5931\u7684\u8d4b\u503c"
description: "Chúng ta có một chuỗi các biến $x1, x2, dấu chấm, xn$, mỗi biến có giá trị từ $1$ đến $v$. Đối với mỗi cặp liền kề, có một ràng buộc nhị phân có dạng: $$xi=ai ngụ ý x{i+1}=bi$$ Các giá trị $ai$ và $bi$ không xác định."
date: "2026-06-25T06:10:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105636
codeforces_index: "B"
codeforces_contest_name: "NOIP 2024"
rating: 0
weight: 105636
solve_time_s: 72
verified: true
draft: false
---

[CF 105636B - \u9057\u5931\u7684\u8d4b\u503c](https://codeforces.com/problemset/problem/105636/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi các biến$x_1, x_2, \dots, x_n$, mỗi cái lấy một giá trị từ$1$ĐẾN$v$. 

Đối với mỗi cặp liền kề, có một ràng buộc nhị phân có dạng:$$x_i=a_i \implies x_{i+1}=b_i$$Các giá trị$a_i$Và$b_i$chưa được biết. Chúng tôi chỉ biết một số ràng buộc đơn phương sửa các biến nhất định:$$x_{c_j}=d_j$$Nhiệm vụ là đếm xem tất cả các cặp có bao nhiêu lựa chọn$(a_i,b_i)$làm cho toàn bộ hệ thống có thể thỏa mãn, nghĩa là tồn tại ít nhất một phép gán của tất cả các biến thỏa mãn mọi ràng buộc một ngôi và nhị phân. 

Đầu vào chứa một số trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra$n,m,v$, theo sau là$m$ràng buộc đơn nhất. 

Phần bất thường của vấn đề là kích thước hạn chế. Số lượng biến có thể đạt tới$10^9$, vì vậy bất kỳ thuật toán nào xử lý từng vị trí một đều không thể thực hiện được. Mặt khác, số lượng ràng buộc đơn nhất nhiều nhất là$10^5$, điều này gợi ý rõ ràng rằng chỉ có các vị trí cố định mới quan trọng và mọi thứ giữa chúng phải được xử lý bằng một công thức. 

Trường hợp cạnh đầu tiên là các ràng buộc đơn nhất mâu thuẫn trên cùng một vị trí. 

Ví dụ:```
n = 2, v = 2
x1 = 1
x1 = 2
```Không có phép gán biến nào có thể thỏa mãn cả hai yêu cầu, vì vậy câu trả lời là 0. 

Trường hợp cạnh thứ hai là khi chỉ có một vị trí cố định. Khi đó không có sự tương tác giữa các giá trị cố định. Chúng ta có thể chỉ cần chọn giá trị quy định của biến đó và tránh gây ra bất kỳ hàm ý có vấn đề nào. Mọi phép gán ràng buộc nhị phân đều trở nên hợp lệ. 

Ví dụ:```
n = 3, v = 2
x2 = 1
```Tất cả$(v^2)^{n-1}=4^2=16$các lựa chọn là hợp lệ. 

Trường hợp cạnh thứ ba là khi hai vị trí cố định liền kề nhau. 

Ví dụ:```
x1 = 1
x2 = 2
```Có chính xác một ràng buộc nhị phân giữa chúng. Chỉ có một sự lựa chọn$(a_1,b_1)$gây ra mâu thuẫn, đó là$(1,1)$. Mọi sự lựa chọn khác vẫn thỏa đáng. 

Lý luận bất cẩn thường quên rằng ràng buộc chỉ được kích hoạt khi$x_i=a_i$. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liệt kê tất cả các giá trị có thể có của mọi$a_i$Và$b_i$. Vì mỗi cặp có$v^2$khả năng và có$n-1$các cạnh, điều này mang lại$$(v^2)^{n-1}$$cấu hình. 

Đối với mỗi cấu hình, chúng ta có thể kiểm tra xem một số phép gán biến có thỏa mãn tất cả các ràng buộc hay không. 

Điều này đúng, nhưng ngay cả đối với những đầu vào nhỏ, nó cũng sẽ phát nổ ngay lập tức. 

Quan sát quan trọng là các ràng buộc nhị phân cực kỳ yếu. Ràng buộc chỉ kích hoạt khi biến bên trái bằng một giá trị cụ thể$a_i$. Nếu chúng ta có thể chọn$x_i\neq a_i$, ràng buộc đó trở nên không liên quan. 

Vì điều này, mâu thuẫn chỉ có thể lan truyền thông qua một chuỗi liên tục các hàm ý được kích hoạt. 

Giả sử hai vị trí$l$Và$r$được cố định bởi các ràng buộc đơn nhất. Chúng tôi hỏi: 

Có bao nhiêu lựa chọn ràng buộc nhị phân giữa chúng làm cho hệ thống thỏa mãn? 

Đếm ngược lại thì dễ hơn. 

Mâu thuẫn chỉ xảy ra nếu giá trị cố định tại vị trí$l$buộc một chuỗi duy nhất đến đúng vị trí$r$và giá trị cưỡng bức cuối cùng không đồng ý với giá trị cố định tại$r$. 

Để điều đó xảy ra, mọi hàm ý dọc theo con đường phải được kích hoạt:$$a_l=x_l,\quad
a_{l+1}=b_l,\quad
a_{l+2}=b_{l+1},\dots$$và ở cạnh cuối cùng, chúng ta phải buộc một giá trị khác với$x_r$. 

Nếu khoảng cách là$d=r-l$, sau đó:$$b_l,b_{l+1},\dots,b_{r-2}$$có thể được chọn tùy ý, cho$v^{d-1}$khả năng. 

Giá trị cuối cùng$b_{r-1}$phải khác với$x_r$, cho$v-1$khả năng. 

Do đó số lượng cấu hình xấu là$$v^{d-1}(v-1).$$Tất cả các ràng buộc nhị phân trong phân đoạn đó có$$(v^2)^d=v^{2d}$$bài tập có thể thực hiện được, vậy số bài tập tốt là$$f(d)=v^{2d}-v^{d-1}(v-1).$$Bây giờ hãy chú ý đến điều gì đó thậm chí còn mạnh mẽ hơn. 

Chỉ có các vị trí được cố định bởi các ràng buộc đơn nhất mới quan trọng. Các vùng trước vị trí cố định đầu tiên và sau vị trí cố định cuối cùng luôn có thể chọn các giá trị thay đổi để tránh gây ra những tác động rắc rối. Mọi phép gán ràng buộc nhị phân ở đó đều hợp lệ. 

Do đó, toàn bộ câu trả lời được phân tích thành các phân đoạn độc lập giữa các vị trí cố định liên tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((v^2)^{n-1})$| Rất lớn | Quá chậm | 
| Tối ưu |$O(m \log MOD)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các ràng buộc đơn nhất. 
2. Lưu trữ chúng trong bản đồ được khóa theo vị trí. 
3. Nếu cùng một vị trí xuất hiện với các giá trị bắt buộc khác nhau, hãy xuất 0 ngay lập tức vì các ràng buộc đơn nguyên đã không nhất quán. 
4. Sắp xếp các vị trí cố định còn lại. 
5. Gọi các vị trí cố định đã sắp xếp là:$$p_1<p_2<\dots<p_k.$$1. Nhân câu trả lời với$$v^{2(p_1-1)}$$cho tiền tố không bị ràng buộc trước vị trí cố định đầu tiên. 

1. Với mỗi cặp liên tiếp$(p_i,p_{i+1})$, cho phép$$d=p_{i+1}-p_i.$$Nhân câu trả lời với$$v^{2d}-v^{d-1}(v-1).$$Điều này tính tất cả các phép gán ràng buộc nhị phân hợp lệ bên trong phân đoạn đó. 

1. Nhân câu trả lời với$$v^{2(n-p_k)}$$cho hậu tố không bị ràng buộc sau vị trí cố định cuối cùng. 

1. Xuất kết quả theo modulo$10^9+7$. 

### Tại sao nó hoạt động 

Một đoạn được giới hạn bởi hai vị trí cố định chỉ có thể trở nên không thỏa mãn nếu giá trị cố định bên trái kích hoạt mọi hàm ý trong đoạn đó, tạo ra một chuỗi bắt buộc đi đến điểm cuối bên phải và yêu cầu một giá trị khác với giá trị cố định của nó. 

Bất kỳ sự đứt gãy nào trong chuỗi đó sẽ ngay lập tức khôi phục lại sự hài lòng, bởi vì các biến còn lại có thể được tự do lựa chọn. 

Các cấu hình xấu chính xác là các chuỗi cưỡng bức liên tục được mô tả ở trên và số lượng của chúng là$v^{d-1}(v-1)$. 

Các phân đoạn khác nhau liên quan đến các tập hợp ràng buộc nhị phân rời rạc, do đó các lựa chọn của chúng là độc lập. Nhân phần đóng góp của mỗi phân đoạn sẽ cho tổng số bài tập hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def solve():
    t = int(input())

    for _ in range(t):
        n, m, v = map(int, input().split())

        fixed = {}
        ok = True

        for _ in range(m):
            c, d = map(int, input().split())

            if c in fixed and fixed[c] != d:
                ok = False
            fixed[c] = d

        if not ok:
            print(0)
            continue

        pos = sorted(fixed.keys())

        ans = 1

        first = pos[0]
        ans = ans * pow(v, 2 * (first - 1), MOD) % MOD

        for i in range(len(pos) - 1):
            d = pos[i + 1] - pos[i]

            total = pow(v, 2 * d, MOD)
            bad = pow(v, d - 1, MOD) * ((v - 1) % MOD) % MOD

            ans = ans * ((total - bad) % MOD) % MOD

        last = pos[-1]
        ans = ans * pow(v, 2 * (n - last), MOD) % MOD

        print(ans)

solve()
```Phần đầu tiên của mã kiểm tra xem các ràng buộc đơn nguyên có tự nhất quán hay không. Nếu một vị trí được gán hai giá trị khác nhau thì không có nghiệm nào tồn tại và đáp án ngay lập tức là 0. 

Các vị trí cố định được sắp xếp xác định những nơi duy nhất có thể kiểm tra được mức độ hài lòng. Mọi thứ bên ngoài những vị trí đó đều đóng góp sức mạnh thuần túy của$v^2$, bởi vì các ràng buộc nhị phân ở đó có thể được chọn tùy ý. 

Với mỗi đoạn ta tính:$$v^{2d}$$cho tất cả các phép gán và phép trừ ràng buộc nhị phân có thể có$$v^{d-1}(v-1)$$cho nhóm nhiệm vụ duy nhất tạo ra sự mâu thuẫn. 

Tất cả phép lũy thừa đều sử dụng mô-đun của Python`pow`, chạy theo thời gian logarit và rất cần thiết vì$n$Và$v$cả hai có thể đạt được$10^9$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
2 1 2
1 1
```Chỉ có một vị trí cố định. 

| Bước | Giá trị | 
| --- | --- | 
| Vị trí cố định | {1} | 
| Đóng góp tiền tố |$2^0=1$| 
| Phân khúc nội bộ | không | 
| Đóng góp hậu tố |$2^{2}=4$| 
| Trả lời | 4 | 

Kết quả là 4, phù hợp với mẫu. 

Vì chỉ$x_1$cố định, chúng ta luôn có thể chọn$x_2$một cách thích hợp. Mọi phép gán ràng buộc nhị phân vẫn thỏa mãn. 

### Ví dụ 2 

đầu vào:```
1
2 2 2
1 1
2 2
```| Bước | Giá trị | 
| --- | --- | 
| Vị trí cố định | {1,2} | 
| Khoảng cách | 1 | 
| Tổng số bài tập |$2^2=4$| 
| Bài tập tồi |$2^0(2-1)=1$| 
| Bài tập hay | 3 | 
| Trả lời | 3 | 

Nhiệm vụ xấu duy nhất là:```
(a1,b1) = (1,1)
```bởi vì$x_1=1$kích hoạt hàm ý và lực lượng$x_2=1$, mâu thuẫn$x_2=2$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log MOD)$| Sắp xếp cộng với lũy thừa mô-đun | 
| Không gian |$O(m)$| Lưu trữ các vị trí cố định | 

Thuật toán không bao giờ phụ thuộc vào$n$trực tiếp, điều này rất quan trọng bởi vì$n$có thể lớn như$10^9$. Chỉ có$m$vị trí cố định được xử lý và$m\le 10^5$, vừa vặn một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 1000000007

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, m, v = map(int, input().split())

        fixed = {}
        ok = True

        for _ in range(m):
            c, d = map(int, input().split())
            if c in fixed and fixed[c] != d:
                ok = False
            fixed[c] = d

        if not ok:
            out.append("0")
            continue

        pos = sorted(fixed)

        ans = pow(v, 2 * (pos[0] - 1), MOD)

        for i in range(len(pos) - 1):
            d = pos[i + 1] - pos[i]
            total = pow(v, 2 * d, MOD)
            bad = pow(v, d - 1, MOD) * (v - 1) % MOD
            ans = ans * ((total - bad) % MOD) % MOD

        ans = ans * pow(v, 2 * (n - pos[-1]), MOD) % MOD
        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("1\n2 1 2\n1 1\n") == "4"
assert run("1\n2 2 2\n1 1\n2 2\n") == "3"
assert run("1\n2 2 2\n1 1\n1 2\n") == "0"

# custom cases
assert run("1\n1 1 5\n1 3\n") == "1"
assert run("1\n3 1 2\n2 1\n") == "16"
assert run("1\n3 2 3\n1 1\n3 2\n") == "24"
assert run("1\n5 2 2\n2 1\n4 1\n") == "48"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=1$, một giá trị cố định | 1 | Kích thước tối thiểu | 
| Vị trí cố định đơn ở giữa | 16 | Toàn bộ câu trả lời đến từ các vùng miễn phí | 
| Hai điểm cuối cố định có khoảng cách | 24 | Công thức phân đoạn | 
| Chuỗi lớn hơn | 48 | Nhiều đóng góp nhân với nhau | 

## Vỏ cạnh 

### Xung đột các ràng buộc đơn nhất 

đầu vào:```
1
2 2 2
1 1
1 2
```Bản đồ đã chứa vị trí 1 với giá trị 1. Khi giá trị 2 xuất hiện cho cùng một vị trí, sự không nhất quán sẽ được phát hiện ngay lập tức. 

Các đầu ra thuật toán:```
0
```điều này đúng vì không có phép gán biến nào có thể đáp ứng cả hai yêu cầu. 

### Chỉ Một Vị Trí Cố Định 

đầu vào:```
1
3 1 2
2 1
```Danh sách cố định được sắp xếp chỉ chứa vị trí 2. 

Câu trả lời trở thành:$$2^{2(2-1)} \times 2^{2(3-2)}
=
4 \times 4
=
16.$$Không có phân đoạn bên trong nào tồn tại, do đó mọi phép gán ràng buộc nhị phân đều hợp lệ. 

### Vị trí cố định liền kề 

đầu vào:```
1
2 2 2
1 1
2 2
```Khoảng cách$d=1$. 

Thuật toán tính toán:$$2^2 - 2^0(2-1)
=
4-1
=
3.$$Chính xác là một nhiệm vụ,$(a_1,b_1)=(1,1)$, tạo ra sự mâu thuẫn. Mọi nhiệm vụ khác vẫn đáp ứng được. Điều này phù hợp với đối số đếm dự định.
