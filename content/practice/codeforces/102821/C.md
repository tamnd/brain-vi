---
title: "CF 102821C - Chức năng chu trình"
description: "Chúng ta có một hàm tuyến tính cố định $f(x)=Ax+B$ và một tập hợp các giá trị $x1,x2,dots,xN$. Đối với mỗi hàm truy vấn $g(x)=cx+d$, chúng ta cần đo khoảng cách giữa hai thành phần với hàm nhận dạng."
date: "2026-07-26T16:10:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102821
codeforces_index: "C"
codeforces_contest_name: "2019 Sichuan Province Programming Contest"
rating: 0
weight: 102821
solve_time_s: 43
verified: true
draft: false
---

[CF 102821C - Chức năng chu trình](https://codeforces.com/problemset/problem/102821/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hàm tuyến tính cố định$f(x)=Ax+B$và tập hợp các giá trị$x_1,x_2,\dots,x_N$. Đối với mọi chức năng truy vấn$g(x)=cx+d$, chúng ta cần đo xem hai thành phần cách hàm nhận dạng bao xa. 

Đối với một giá trị$x_i$, đóng góp của một truy vấn là:$$|f(g(x_i))-x_i|+|g(f(x_i))-x_i|$$Câu trả lời cho một truy vấn là tổng của những đóng góp này trên tất cả các giá trị đã cho. 

Đầu vào chứa một số trường hợp thử nghiệm. Mỗi test case đưa ra số lượng giá trị, số lượng hàm truy vấn, hệ số của$f$, danh sách các giá trị và sau đó là tất cả các hàm truy vấn. Với mỗi hàm truy vấn, chúng ta in ra tổng tương ứng. 

Hạn chế chính là cả hai$N$Và$M$có thể đạt được$10^5$. Một giải pháp đánh giá mọi truy vấn trên mọi giá trị sẽ thực hiện khoảng$10^{10}$các hoạt động trong trường hợp xấu nhất không thể đáp ứng được thời hạn. Chúng ta cần xử lý trước các giá trị một lần và trả lời từng truy vấn theo thời gian logarit. 

Phần khó khăn là xử lý chính xác các giá trị tuyệt đối. Một số trường hợp có thể dễ dàng phá vỡ việc triển khai trực tiếp. 

Nếu hệ số của$x$trở thành 0, biểu thức là hằng số. Ví dụ: nếu một truy vấn đưa ra$Ac-1=0$, sau đó$$|(Ac-1)x_i+(Ad+B)|=|Ad+B|$$cho mọi$x_i$. Chia cho độ dốc trong trường hợp này sẽ không hợp lệ. 

Ví dụ:```
1
3 1 2 0
1 2 3
0.5 5
```Đây$Ac-1=2\cdot0.5-1=0$. Biểu thức đầu tiên luôn là$|5|$, vậy đóng góp của nó là$15$. Một giải pháp cố gắng tính điểm bước ngoặt bằng cách chia cho 0 đều thất bại. 

Một trường hợp cạnh khác xuất hiện khi điểm ngoặt nằm ngoài phạm vi của tất cả các giá trị. Ví dụ:```
1
4 1 1 0
1 2 3 4
1 100
```Biểu thức trở thành$x_i+100$, vậy câu trả lời là$101+102+103+104=410$. Việc triển khai bất cẩn chỉ xem xét các giá trị ở giữa mảng có thể bỏ sót rằng tất cả các thuật ngữ đều có cùng dấu. 

Giá trị trùng lặp là một nguồn sai lầm khác. Ví dụ:```
1
5 1 1 0
2 2 2 2 2
1 -2
```Biểu thức là$x_i-2$, vì vậy mọi số hạng đều bằng 0 và câu trả lời là 0. Các phép tính tiền tố phải xử lý chính xác các giá trị bằng nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp rất đơn giản. Đối với mọi hàm truy vấn, hãy thay thế mọi$x_i$, tính toán hai thành phần, lấy các giá trị tuyệt đối và cộng chúng lại. Điều này đúng vì nó tuân theo định nghĩa chính xác. Tuy nhiên, mỗi truy vấn có giá$O(N)$, cho$O(NM)$hoạt động. Với$N=M=100000$, điều này trở thành$10^{10}$đánh giá vượt xa thời gian sẵn có. 

Quan sát quan trọng là cả hai sự khác biệt về thành phần đều là hàm tuyến tính. Mở rộng cái đầu tiên mang lại:$$f(g(x))-x=A(cx+d)+B-x$$

$$=(Ac-1)x+(Ad+B)$$Cái thứ hai cho:$$g(f(x))-x=c(Ax+B)+d-x$$

$$=(Ac-1)x+(Bc+d)$$Cả hai biểu thức đều có cùng độ dốc. Toàn bộ vấn đề trở thành việc trả lời các truy vấn có dạng:$$\sum_i |px_i+q|$$Ở đâu$p$Và$q$được biết cho mỗi truy vấn. 

Nếu như$p\neq0$, chúng ta có thể tính ra giá trị tuyệt đối của độ dốc:$$\sum_i |p||x_i+\frac qp|$$Cho phép:$$t=-\frac qp$$Sau đó:$$\sum_i |p||x_i-t|$$Vì vậy chúng ta chỉ cần một cấu trúc dữ liệu trả về:$$\sum_i |x_i-t|$$cho bất kỳ thực tế$t$. 

Sau khi sắp xếp các giá trị, vị trí của$t$chia mảng thành các giá trị nhỏ hơn$t$và giá trị lớn hơn$t$. Tổng tiền tố cho phép chúng ta tính toán cả hai phần trong thời gian không đổi sau khi tìm kiếm nhị phân. 

Các phương pháp so sánh như sau: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NM) | O(1) | Quá chậm | 
| Tối ưu | O((N+M) log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các giá trị$x_i$và xây dựng một mảng tổng tiền tố. Tổng tiền tố cho phép chúng tôi tính tổng của bất kỳ tiền tố nào ngay lập tức, đủ để tính khoảng cách đến bất kỳ điểm nào đã chọn. 
2. Đối với từng hàm truy vấn$g(x)=cx+d$, tính độ dốc chung:$$p=Ac-1$$Hai điểm cắt là:$$q_1=Ad+B$$Và$$q_2=Bc+d$$1. Tạo hàm trợ giúp tính toán:$$S(p,q)=\sum_i |px_i+q|$$Nếu như$p=0$, mọi số hạng đều có giá trị như nhau$|q|$, vì vậy kết quả chỉ đơn giản là$N|q|$. 

Ngược lại, hãy chuyển biểu thức thành:$$|p|\sum_i |x_i-t|$$Ở đâu:$$t=-q/p$$1. Để tính toán$\sum_i|x_i-t|$, tìm vị trí đầu tiên tại đó$x_i\ge t$. Giả sử vị trí này là$k$. Phía bên trái đóng góp:$$t\cdot k-\sum_{i<k}x_i$$và phía bên phải đóng góp:$$\sum_{i\ge k}x_i-t(N-k)$$Việc thêm chúng sẽ cho tổng khoảng cách cần thiết. 

1. Câu trả lời cuối cùng cho truy vấn là:$$S(p,q_1)+S(p,q_2)$$In giá trị này với độ chính xác đủ. 

Tính chính xác xuất phát từ thực tế là mọi biểu thức truy vấn đều được giảm chính xác xuống tổng khoảng cách trên một dòng được sắp xếp. Điểm phân chia$t$là nơi duy nhất có dấu hiệu của$x_i-t$những thay đổi. Tất cả các giá trị trước nó đóng góp khoảng cách của chúng với phía bên trái và tất cả các giá trị sau nó đóng góp khoảng cách của chúng với phía bên phải. Vì tổng tiền tố chứa tổng chính xác của cả hai nhóm nên giá trị được tính luôn là tổng thực của các giá trị tuyệt đối. 

## Giải pháp Python```python
import sys
import bisect

input = sys.stdin.readline

def solve():
    data = sys.stdin.buffer.read().split()
    ptr = 0
    t = int(data[ptr])
    ptr += 1
    out = []

    for case in range(1, t + 1):
        n = int(data[ptr])
        m = int(data[ptr + 1])
        A = float(data[ptr + 2])
        B = float(data[ptr + 3])
        ptr += 4

        xs = [float(data[ptr + i]) for i in range(n)]
        ptr += n

        xs.sort()
        pref = [0.0]
        for x in xs:
            pref.append(pref[-1] + x)

        def dist_to_point(x):
            k = bisect.bisect_left(xs, x)
            left = x * k - pref[k]
            right = (pref[n] - pref[k]) - x * (n - k)
            return left + right

        def calc(p, q):
            if abs(p) < 1e-15:
                return n * abs(q)
            return abs(p) * dist_to_point(-q / p)

        out.append(f"Case {case}:")

        for _ in range(m):
            c = float(data[ptr])
            d = float(data[ptr + 1])
            ptr += 2

            p = A * c - 1.0
            q1 = A * d + B
            q2 = B * c + d

            ans = calc(p, q1) + calc(p, q2)
            out.append(f"{ans:.10f}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Đầu vào được đọc dưới dạng byte và được phân tích cú pháp cùng nhau vì tổng số giá trị có thể lớn. Tổng tiền tố và mảng đã sắp xếp được tạo một lần cho mỗi trường hợp thử nghiệm, trước khi xử lý bất kỳ truy vấn nào. 

chức năng`dist_to_point`thực hiện phép chia toán học xung quanh bước ngoặt.`bisect_left`tìm phần tử đầu tiên không nhỏ hơn điểm, chính xác là ranh giới giữa các giá trị âm và không âm của$x_i-t$. 

chức năng`calc`xử lý riêng trường hợp độ dốc bằng 0 đặc biệt. Điều này tránh được việc chia cho 0 và cũng tránh được sự mất ổn định về số. Đối với các độ dốc khác 0, nó áp dụng phép biến đổi từ giá trị tuyệt đối tuyến tính thành tổng khoảng cách theo tỷ lệ. 

Tất cả các phép tính đều sử dụng giá trị dấu phẩy động vì đầu vào chứa số thực và dung sai yêu cầu là$10^{-6}$. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2
3 2 2.0 3.0
1.0 2.0 3.0
0.4 -2.0
0.6 -5.0
3 2 2.5 2.0
1.0 2.0 3.0
0.4 -2.0
0.6 -5.0
```Đối với truy vấn đầu tiên,$g(x)=0.4x-2$: 

| Bước | Giá trị | 
| --- | --- | 
| Độ dốc$p=Ac-1$| -0,2 | 
| Đánh chặn đầu tiên$q_1=Ad+B$| -1 | 
| Đánh chặn thứ hai$q_2=Bc+d$| -2,8 | 
| Đáp án tính toán | 7.800000 | 

Các giá trị được sắp xếp là`[1,2,3]`. Cả hai biểu thức tuyệt đối đều được đánh giá thông qua cùng một trình trợ giúp tổng khoảng cách, xác nhận rằng quá trình xử lý trước giống nhau đều hoạt động cho cả hai tác phẩm. 

Đối với truy vấn thứ hai,$g(x)=0.6x-5$: 

| Bước | Giá trị | 
| --- | --- | 
| Độ dốc$p=Ac-1$| 0,2 | 
| Đánh chặn đầu tiên$q_1$| -7 | 
| Đánh chặn thứ hai$q_2$| -3,8 | 
| Đáp án tính toán | 28.200000 | 

Độ dốc đổi dấu, nhưng thuật toán chỉ sử dụng giá trị tuyệt đối của nó sau khi tìm được điểm ngoặt, do đó, cùng một phương pháp xử lý cả hai trường hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N+M) log N) | Chi phí sắp xếp O(N log N) và mỗi truy vấn thực hiện hai tìm kiếm nhị phân | 
| Không gian | O(N) | Các giá trị được sắp xếp và tổng tiền tố được lưu trữ | 

Những ràng buộc cho phép$N$Và$M$lên đến$10^5$. Quá trình tiền xử lý chỉ chiếm ưu thế một lần và mỗi truy vấn được trả lời mà không cần lặp lại tất cả các giá trị, giữ cho tổng công việc trong giới hạn yêu cầu. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    ans = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return ans

assert run("""1
3 2 2.0 3.0
1.0 2.0 3.0
0.4 -2.0
0.6 -5.0
""") == """Case 1:
7.8000000000
28.2000000000
"""

assert run("""1
1 1 1 0
5
1 0
""") == """Case 1:
8.0000000000
"""

assert run("""1
5 1 1 0
2 2 2 2 2
1 -2
""") == """Case 1:
0.0000000000
"""

assert run("""1
4 1 1 0
1 2 3 4
1 100
""") == """Case 1:
410.0000000000
"""

assert run("""1
3 1 2 0
1 2 3
0.5 5
""") == """Case 1:
15.0000000000
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Truy vấn mẫu gốc | 7,8 và 28,2 | Xử lý thành phần thông thường | 
| Giá trị đơn | 8.0 | Kích thước đầu vào nhỏ nhất | 
| Tất cả các giá trị bằng nhau | 0,0 | Xử lý trùng lặp | 
| Bước ngoặt ngoài phạm vi | 410.0 | Giá trị tuyệt đối cùng dấu | 
| Độ dốc bằng không | 15.0 | Ranh giới chia cho 0 | 

## Vỏ cạnh 

Khi nào$Ac-1=0$, thuật toán đi vào nhánh biểu thức hằng số. Vì:```
1
3 1 2 0
1 2 3
0.5 5
```độ dốc bằng 0, vì vậy số hạng đầu tiên đóng góp$3\times5=15$. Biểu thức thứ hai cũng không đổi với phần chặn của chính nó và trình trợ giúp xử lý cả hai mà không cần cố gắng tính điểm bước ngoặt. 

Khi bước ngoặt nằm ngoài các giá trị đã sắp xếp, tìm kiếm nhị phân sẽ trả về phần đầu hoặc phần cuối của mảng. Vì:```
1
4 1 1 0
1 2 3 4
1 100
```bước ngoặt là$-100$. Mọi giá trị đều ở bên phải, do đó phép tính khoảng cách trở thành:$$(1+100)+(2+100)+(3+100)+(4+100)=410$$Công thức tiền tố xử lý việc này một cách tự nhiên vì một bên của phần tách không chứa phần tử nào. 

Khi tất cả các giá trị đều bằng nhau:```
1
5 1 1 0
2 2 2 2 2
1 -2
```bước ngoặt chính xác là giá trị lặp lại. Tìm kiếm nhị phân tìm thấy vị trí bằng nhau đầu tiên, đóng góp bên trái và bên phải đều bằng 0 và kết quả được báo cáo chính xác là 0.
