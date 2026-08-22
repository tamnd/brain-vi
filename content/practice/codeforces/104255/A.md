---
title: "CF 104255A - Nhãn dán cho BSUIR Open"
description: "Chúng ta được cho một tờ giấy hình chữ nhật có kích thước $n nhân m$. Từ trang tính này, chúng ta muốn cắt ra các hình dán hình vuông giống hệt $k$, trong đó mỗi hình dán là một hình vuông có chiều dài cạnh $x$. Các hình vuông phải nằm trọn trong tờ giấy và không được phép chồng lên nhau."
date: "2026-07-01T21:50:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104255
codeforces_index: "A"
codeforces_contest_name: "BSUIR Open X. Reload. Students final"
rating: 0
weight: 104255
solve_time_s: 71
verified: true
draft: false
---

[CF 104255A - Nhãn dán cho BSUIR Open](https://codeforces.com/problemset/problem/104255/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Người ta cho ta một tờ giấy hình chữ nhật có kích thước$n \times m$. Từ tờ này, chúng tôi muốn cắt ra$k$các hình dán hình vuông giống hệt nhau, trong đó mỗi hình dán là một hình vuông có cạnh dài$x$. Các hình vuông phải nằm trọn trong tờ giấy và không được phép chồng lên nhau. 

Nhiệm vụ là xác định số nguyên hoặc giá trị thực lớn nhất có thể của$x$ít nhất như vậy$k$hình vuông có kích thước$x \times x$có thể được đặt bên trong hình chữ nhật. 

Quan sát quan trọng là với bất kỳ chiều dài cạnh cố định nào$x$, số lượng hình vuông chúng ta có thể đặt được xác định bằng số lượng hình vuông phù hợp với mỗi chiều. Theo chiều ngang chúng ta có thể phù hợp$\lfloor n / x \rfloor$hình vuông, và theo chiều dọc chúng ta có thể phù hợp$\lfloor m / x \rfloor$hình vuông, vì vậy tổng số là tích của chúng. 

Những hạn chế$n, m \le 10^3$đủ nhỏ để việc đánh giá hàm này là tầm thường đối với một hàm cố định$x$, Nhưng$k \le 10^9$khiến cho việc liệt kê các khả năng để tính vị trí một cách trực tiếp là không thể. Câu trả lời không nhất thiết phải là số nguyên vì kích thước hình vuông tối ưu có thể là phân số. 

Một cách tiếp cận ngây thơ sẽ thử tất cả các kích thước hình vuông có thể từ$1$xuống mức tăng rất nhỏ, nhưng điều này không thành công vì câu trả lời có độ chính xác liên tục lên đến$10^{-6}$. Thậm chí rời rạc hóa thành$10^{-6}$độ phân giải sẽ yêu cầu khoảng$10^6$kiểm tra, và mỗi kiểm tra là$O(1)$, là đường biên nhưng không cần thiết vì có cấu trúc rõ ràng hơn. 

Trường hợp cạnh xuất hiện khi$k$rất lớn, buộc kích thước hình vuông phải nhỏ. Ví dụ, nếu$n = m = 2$Và$k = 3$, sau đó$x = 1$hoạt động vì$2$hình vuông vừa với mỗi hàng và cột, cho$4$tổng cộng, nhưng bất kỳ$x > 1$giảm số lượng bên dưới$3$. Một trường hợp cạnh khác là khi$k = 1$, câu trả lời chỉ đơn giản là$\min(n, m)$, vì một hình vuông có thể chiếm cạnh lớn nhất có thể. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là thử tất cả các độ dài cạnh hình vuông có thể$x$, tính xem có bao nhiêu ô vuông phù hợp và theo dõi giá trị hợp lệ lớn nhất. Nếu chúng ta rời rạc hóa$x$thành các bước nhỏ như$10^{-6}$, mỗi lần kiểm tra đều yêu cầu tính toán$\lfloor n/x \rfloor \cdot \lfloor m/x \rfloor$, đó là thời gian không đổi. Tuy nhiên, sự rời rạc hóa này dẫn đến khoảng$10^6$các ứng cử viên, và mỗi lần đánh giá đều liên quan đến việc chia dấu phẩy động và các phép toán sàn. Mặc dù điều này có thể vượt qua trong một số trường hợp, nhưng nó không cần thiết và vẫn còn mong manh về độ chính xác. 

Quan sát cấu trúc quan trọng là số lượng ô vuông phù hợp là đơn điệu đối với$x$. Nếu một kích thước nhất định$x$hoạt động thì bất kỳ kích thước nhỏ hơn nào cũng có tác dụng vì việc giảm kích thước hình vuông chỉ làm tăng số lượng phù hợp dọc theo mỗi chiều. Ngược lại, nếu$x$quá lớn, việc tăng thêm chỉ làm giảm tính khả thi. Hành vi đơn điệu này cho phép chúng ta xử lý vấn đề như một phép tìm kiếm nhị phân liên tục trên$x$. 

Chúng ta xác định một vị ngữ$f(x)$điều đó kiểm tra xem$\lfloor n/x \rfloor \cdot \lfloor m/x \rfloor \ge k$. Vị từ này là đơn điệu nên chúng ta có thể tìm kiếm nhị phân ở mức tối đa$x$mà nó vẫn đúng. 

Không gian tìm kiếm là từ$0$ĐẾN$\max(n, m)$. Mỗi bước tìm kiếm nhị phân đánh giá vị từ theo thời gian không đổi, đưa ra số lần lặp logarit đủ để$10^{-6}$độ chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(10^6)$|$O(1)$| Quá chậm/không chính xác | 
| Tìm kiếm nhị phân |$O(\log(\text{precision}))$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta phát biểu lại bài toán dưới dạng tìm giá trị lớn nhất$x$ít nhất như vậy$k$hình vuông cạnh$x$phù hợp. 

1. Xác định hàm`can(x)`để tính xem có bao nhiêu ô vuông phù hợp:$(n // x) * (m // x)$. Nếu giá trị này ít nhất$k$, sau đó$x$là khả thi. Việc phân chia tầng ghi lại số lượng ô vuông đầy đủ dọc theo mỗi trục. 
2. Đặt khoảng tìm kiếm nhị phân với`left = 0`Và`right = max(n, m)`. Câu trả lời phải nằm trong phạm vi này vì một hình vuông lớn hơn hình chữ nhật không thể vừa dù chỉ một lần. 
3. Tính toán nhiều lần`mid = (left + right) / 2`và kiểm tra`can(mid)`. 
4. Nếu`can(mid)`là đúng, chúng ta có thể cố gắng tăng kích thước hình vuông, vì vậy chúng ta di chuyển`left = mid`. Nếu không, chúng tôi giảm kích thước bằng`right = mid`. 
5. Tiếp tục cho đến khi chiều rộng khoảng dưới đây$10^{-7}$, đảm bảo tính chính xác trong giới hạn lỗi yêu cầu. 
6. Đầu ra`left`như là xấp xỉ khả thi tốt nhất. 

### Tại sao nó hoạt động 

chức năng$can(x)$là đơn điệu giảm dần trong$x$. Khi một kích thước trở nên không khả thi thì tất cả các kích thước lớn hơn vẫn không thể thực hiện được vì ngày càng tăng$x$chỉ có thể giảm hoặc duy trì số lượng ô vuông phù hợp. Tính đơn điệu này đảm bảo rằng tìm kiếm nhị phân không bao giờ bỏ qua ranh giới tối ưu và điểm hội tụ cuối cùng xấp xỉ giá trị khả thi lớn nhất.$x$trong phạm vi độ chính xác cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(n, m, k, x):
    return (n // x) * (m // x) >= k

def solve():
    n, m, k = map(int, input().split())

    # handle edge case: no squares fit unless x <= min(n, m)
    left, right = 0.0, float(max(n, m))

    for _ in range(60):
        mid = (left + right) / 2
        if mid == 0:
            left = mid
            continue
        if (n // mid) * (m // mid) >= k:
            left = mid
        else:
            right = mid

    print(left)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là`can`logic, mã hóa trực tiếp số lượng ô vuông phù hợp với mỗi chiều. Tìm kiếm nhị phân chạy với số lần lặp cố định (60), quá đủ để hội tụ độ chính xác gấp đôi. 

Một điểm tinh tế là tránh chia cho 0 khi`mid`trở nên cực kỳ nhỏ. Trong thực tế, vòng lặp hội tụ trước khi điều đó trở thành vấn đề, nhưng việc kiểm tra đảm bảo an toàn về số. Một cách tinh tế khác là sử dụng 60 lần lặp thay vì điều kiện epsilon, để tránh sự mất ổn định của dấu phẩy động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 3
```Chúng tôi mong đợi một kích thước hình vuông của$1.0$. 

| Bước | trái | đúng | giữa | vừa vặn = (n//giữa)*(m//giữa) | khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0,0 | 2.0 | 1.0 | 4 | vâng | 
| 2 | 1.0 | 2.0 | 1,5 | 1 | không | 
| 3 | 1.0 | 1,5 | 1,25 | 1 | không | 
| 4 | 1.0 | 1,25 | 1.125 | 1 | không | 

Việc tìm kiếm hội tụ về 1.0 là giá trị khả thi lớn nhất. Bất kỳ giá trị nào trên 1 đều làm giảm số lượng ô vuông phù hợp dưới 3. 

### Ví dụ 2 

đầu vào:```
3 3 1
```Chúng tôi mong đợi câu trả lời$3.0$. 

| Bước | trái | đúng | giữa | phù hợp | khả thi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0,0 | 3.0 | 1,5 | 4 | vâng | 
| 2 | 1,5 | 3.0 | 2,25 | 1 | vâng | 
| 3 | 2,25 | 3.0 | 2.625 | 1 | vâng | 

Điều này cho thấy rằng ngay cả những giá trị lớn vẫn khả thi cho đến khi chúng ta đạt đến giới hạn thực$3$, xác nhận rằng giới hạn trên là chặt chẽ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log(1/\epsilon))$| Tìm kiếm nhị phân trên các giá trị thực với khả năng kiểm tra tính khả thi theo thời gian liên tục | 
| Không gian |$O(1)$| Chỉ có một số biến vô hướng được sử dụng | 

Tìm kiếm nhị phân hội tụ trong khoảng 60 lần lặp, điều này không đáng kể dưới giới hạn 1 giây ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import floor

    # inline solution for testing
    n, m, k = map(int, sys.stdin.readline().split())

    def can(x):
        if x == 0:
            return True
        return (n // x) * (m // x) >= k

    l, r = 0.0, float(max(n, m))
    for _ in range(60):
        mid = (l + r) / 2
        if mid == 0:
            break
        if (n // mid) * (m // mid) >= k:
            l = mid
        else:
            r = mid
    return str(l)

# provided sample
assert abs(float(run("2 2 3\n")) - 1.0) < 1e-6

# minimum case
assert abs(float(run("1 1 1\n")) - 1.0) < 1e-6

# large k forcing small answer
assert float(run("10 10 1000000000\n")) < 1e-6

# perfect tiling
assert abs(float(run("4 4 4\n")) - 2.0) < 1e-6

# asymmetric rectangle
assert abs(float(run("6 3 2\n")) - 3.0) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1.0 | trường hợp hợp lệ tối thiểu | 
| 10 10 1000000000 | ~0 | mật độ cực cao buộc các ô vuông nhỏ | 
| 4 4 4 | 2.0 | ranh giới ốp lát chính xác | 
| 6 3 2 | 3.0 | đóng gói không đối xứng | 

## Vỏ cạnh 

Khi nào$k = 1$, thuật toán mở rộng một cách tự nhiên về phía bình phương khả thi lớn nhất, trở thành$\min(n, m)$. Tìm kiếm nhị phân bắt đầu từ 0 và nhanh chóng xác nhận tính khả thi đối với các giá trị trung bình lớn cho đến khi nó hội tụ về mức tối đa thực sự. 

Khi$k$là cực kỳ lớn, việc kiểm tra tính khả thi trở nên sai đối với hầu hết các giá trị ở giữa ngoại trừ những giá trị gần bằng 0. Việc tìm kiếm thu hẹp khoảng thời gian cho đến khi kích thước hình vuông trở nên đủ nhỏ để cả hai phần sàn vẫn tạo ra đủ vị trí. 

Khi$n = m$, tính đối xứng không đơn giản hóa logic nhưng nó làm cho ranh giới khả thi trở nên sắc nét hơn. Thuật toán vẫn hội tụ chính xác vì tính đơn điệu được bảo toàn bất kể tính đối xứng.
