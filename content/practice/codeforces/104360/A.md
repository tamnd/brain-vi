---
title: "CF 104360A - \u0421\u0442\u0430\u0440\u0442 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b"
description: "Có $n$ người tham gia một cuộc thi Olympic. Mỗi người tham gia $i$ bắt đầu tại một thời điểm cố định tạo thành một cấp số cộng: người đầu tiên bắt đầu tại thời điểm $0$, người thứ hai bắt đầu tại thời điểm $x$, người thứ ba bắt đầu tại thời điểm $2x$, v.v., vì vậy người tham gia $i$ bắt đầu tại $(i-1)x$."
date: "2026-07-01T17:56:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104360
codeforces_index: "A"
codeforces_contest_name: "\u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u041c\u0441\u0442\u0438\u0441\u043b\u0430\u0432\u0430 \u041a\u0435\u043b\u0434\u044b\u0448\u0430 - 2021"
rating: 0
weight: 104360
solve_time_s: 59
verified: true
draft: false
---

[CF 104360A - \u0421\u0442\u0430\u0440\u0442 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b](https://codeforces.com/problemset/problem/104360/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

có$n$thí sinh tham gia Olympic. Mỗi người tham gia$i$bắt đầu tại một thời điểm cố định tạo thành một cấp số cộng: lần đầu tiên bắt đầu tại thời điểm$0$, lần thứ hai$x$, thứ ba tại$2x$, v.v., vì vậy người tham gia$i$bắt đầu lúc$(i-1)x$. 

Mỗi người tham gia chi tiêu chính xác$t$biên bản cuộc thi nên người tham gia$i$kết thúc vào thời điểm$(i-1)x + t$. 

Khi một người tham gia kết thúc, chúng tôi xem có bao nhiêu người tham gia khác vẫn còn tham gia cuộc thi vào đúng thời điểm đó. Một người tham gia được coi là đang hoạt động nếu họ đã bắt đầu nhưng chưa kết thúc. Sự “bất mãn” của người tham gia$i$là số người tham gia khác có khoảng thời gian trùng nhau$(i-1)x + t$. 

Nhiệm vụ là tính toán tổng mức độ bất mãn đối với tất cả những người tham gia. 

Những hạn chế là vô cùng lớn, với$n$lên tới$2 \cdot 10^9$. Điều này ngay lập tức loại trừ mọi giải pháp mô phỏng từng người tham gia hoặc kiểm tra sự chồng chéo theo từng cặp. Thậm chí một$O(n)$giải pháp sẽ quá chậm vì nó đòi hỏi hàng tỷ thao tác. 

Cấu trúc thời gian cũng đồng nhất: tất cả những người tham gia đều có độ dài khoảng thời gian giống hệt nhau$t$và các điểm bắt đầu cách đều nhau bởi$x$. Tính đều đặn mạnh mẽ này cho thấy số lượng trùng lặp chỉ phụ thuộc vào vị trí tương đối chứ không phụ thuộc vào mô phỏng riêng lẻ. 

Trường hợp cạnh tinh tế xuất hiện khi khoảng cách giữa các khoảng chiếm ưu thế trong thời lượng. Nếu như$t < x$, thì không có người tham gia nào trùng lặp với bất kỳ người tham gia nào trong tương lai vào thời điểm kết thúc. 

Ví dụ, nếu$x = 10$,$t = 3$, Và$n = 5$, mỗi người tham gia sẽ kết thúc trước khi người tiếp theo bắt đầu, vì vậy câu trả lời là$0$. Một mô phỏng đơn giản vẫn có thể cố gắng đếm các phần trùng lặp nhưng sẽ không tìm thấy gì sau công việc tốn kém. 

Một trường hợp cạnh khác là khi$t$là rất lớn so với$x$. Sau đó, mỗi người tham gia trùng lặp với nhiều người tham gia sau đó, có thể là gần như tất cả trong số họ, và tổng tăng theo cấu trúc bậc hai mặc dù nó phải được tính trong thời gian không đổi. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ tính toán cho mỗi người tham gia$i$, có bao nhiêu khoảng thời gian chồng chéo$(i-1)x + t$. Điều này yêu cầu phải quét tất cả những người tham gia khác, kiểm tra xem thời điểm bắt đầu của họ có trước thời gian đó và thời điểm kết thúc của họ có sau thời gian đó hay không. Điều này mang lại$O(n^2)$hành vi trong trường hợp xấu nhất, vì đối với mỗi$n$những người tham gia chúng tôi có thể quét tới$n$người khác. Với$n$lên tới$2 \cdot 10^9$, điều này là không thể. 

Quan sát quan trọng là cấu trúc hoàn toàn đồng nhất. Khoảng thời gian của mọi người tham gia có cùng độ dài$t$và các điểm bắt đầu cách đều nhau. Vì vậy, thay vì suy luận về các khoảng riêng lẻ, chúng ta có thể chuyển đổi điều kiện thành các bất đẳng thức về chỉ số. 

Tại thời điểm kết thúc đối với người tham gia$i$, chúng tôi chỉ quan tâm đến người tham gia$j > i$, vì những người tham gia trước đó đã hoàn thành vào thời điểm đó do thời lượng giống nhau và thời gian bắt đầu hoàn toàn sớm hơn. Trong số những người tham gia sau, chỉ những người đã bắt đầu trước hoặc vào thời điểm kết thúc$i$đóng góp. 

Điều này làm giảm vấn đề đếm xem có bao nhiêu chỉ mục nằm trong một cửa sổ trượt có kích thước cố định được xác định bằng số bước bắt đầu phù hợp.$t$, tức là$\lfloor t / x \rfloor$. 

Khi việc rút gọn này được thực hiện, câu trả lời sẽ trở thành tổng dạng đóng trên một hàm tuyến tính từng phần đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Đếm dạng đóng |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để$k = \left\lfloor \frac{t}{x} \right\rfloor$. Giá trị này biểu thị có bao nhiêu khoảng thời gian bắt đầu đầy đủ của kích thước$x$phù hợp với thời lượng$t$. 

1. Viết lại điều kiện cho người tham gia chồng chéo khi kết thúc$i$. Người tham gia$j$đóng góp nếu khoảng thời gian của họ bao gồm$(i-1)x + t$. Điều này dẫn đến hai bất đẳng thức:$j > i$Và$(j-1)x \le (i-1)x + t$. 
2. Rút gọn bất đẳng thức thứ hai bằng cách trừ$(i-1)x$, mang lại$(j-i)x \le t$. Vì tất cả các giá trị đều là số nguyên nên điều này trở thành$j - i \le k$. 
3. Kết hợp cả hai ràng buộc. Người đóng góp hợp lệ cho người tham gia$i$chính xác là những người có$$i < j \le i + k$$giao nhau với phạm vi hợp lệ$1 \le j \le n$. 

1. Chuyển đổi số này thành số đếm:$$\text{discontent}_i = \min(k, n - i)$$bởi vì có nhiều nhất$k$những người tham gia tương lai trong phạm vi, nhưng cũng nhiều nhất$n - i$những người tham gia thực tế còn lại. 

1. Tổng hợp tất cả$i$:$$\sum_{i=1}^{n} \min(k, n-i)$$1. Chia tổng thành hai vùng. Vì$i \le n-k$, giá trị luôn là$k$. Vì$i > n-k$, nó giảm tuyến tính từ$k-1$xuống tới$0$. 
2. Tính toán: 

Nếu$k \ge n$, thì tất cả các điều khoản là$n-i$, cho$\frac{n(n-1)}{2}$. 

Nếu không thì:$$(n-k)\cdot k + \frac{k(k-1)}{2}$$### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là sự chồng chéo tại thời điểm hoàn thiện chỉ phụ thuộc vào khoảng cách chỉ số tương đối chứ không phụ thuộc vào thời gian tuyệt đối. Mọi người tham gia đều nhìn thấy một “cửa sổ” tối đa$k$những người tham gia sau này vẫn hoạt động. Vì thời gian bắt đầu cách đều nhau nên cửa sổ này có hình dạng giống hệt nhau cho mọi$i$, vừa dịch chuyển. Tính bất biến này biến một bài toán chồng chéo động thành một tổng số học xác định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    x = int(input().strip())
    t = int(input().strip())

    k = t // x

    if k == 0:
        print(0)
        return

    if k >= n:
        print(n * (n - 1) // 2)
        return

    ans = (n - k) * k + k * (k - 1) // 2
    print(ans)

if __name__ == "__main__":
    solve()
```Cốt lõi của việc triển khai là giảm xuống một tham số duy nhất$k$. Một lần$k$được tính toán, mọi hành vi được xác định mà không cần mô phỏng. Hai nhánh xử lý xem “cửa sổ chồng chéo” có lớn hơn toàn bộ phạm vi người tham gia hay không. Các công thức số học xuất phát trực tiếp từ việc tính tổng một tiền tố không đổi theo sau là một đuôi giảm dần. 

Phải cẩn thận khi chia số nguyên:$k = t // x$hợp lệ vì tất cả các tham số đều là số nguyên và chúng ta chỉ cần số nguyên lớn nhất của các ca đầy đủ vẫn vừa với bên trong$t$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

hãy để$n = 5$,$x = 2$,$t = 5$. Sau đó$k = 2$. 

Chúng tôi tính toán mức độ bất mãn của mỗi người tham gia. 

| tôi | n - tôi | phút(k, n - i) | 
| --- | --- | --- | 
| 1 | 4 | 2 | 
| 2 | 3 | 2 | 
| 3 | 2 | 2 | 
| 4 | 1 | 1 | 
| 5 | 0 | 0 | 

Tổng cộng là$2 + 2 + 2 + 1 + 0 = 7$. 

Điều này phù hợp với công thức:$(5 - 2)\cdot 2 + \frac{2 \cdot 1}{2} = 6 + 1 = 7$. 

### Ví dụ 2 

hãy để$n = 4$,$x = 5$,$t = 2$. Sau đó$k = 0$. 

| tôi | phút(k, n - i) | 
| --- | --- | 
| 1 | 0 | 
| 2 | 0 | 
| 3 | 0 | 
| 4 | 0 | 

Tổng cộng là$0$. Điều này xác nhận rằng khi thời lượng ngắn hơn khoảng cách thì không tồn tại sự chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một vài phép tính số học sau khi đọc đầu vào | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép lên đến$2 \cdot 10^9$, vì vậy chỉ có số học theo thời gian không đổi là khả thi. Giải pháp đáp ứng cả giới hạn thời gian và bộ nhớ một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    x = int(input().strip())
    t = int(input().strip())

    k = t // x

    if k == 0:
        return "0\n"
    if k >= n:
        return str(n * (n - 1) // 2) + "\n"
    return str((n - k) * k + k * (k - 1) // 2) + "\n"

# minimal case
assert run("1\n5\n10\n") == "0\n"

# no overlap case
assert run("5\n10\n3\n") == "0\n"

# full overlap case
assert run("4\n1\n10\n") == "6\n"

# moderate case
assert run("5\n2\n5\n") == "7\n"

# boundary k = n-1
assert run("4\n1\n3\n") == "6\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 5, 10 | 0 | trường hợp cạnh người tham gia duy nhất | 
| 5, 10, 3 | 0 | chế độ không chồng chéo | 
| 4, 1, 10 | 6 | tổng bậc hai chồng chéo đầy đủ | 
| 5, 2, 5 | 7 | chế độ hỗn hợp đúng đắn | 
| 4, 1, 3 | 6 | ranh giới trong đó k = n-1 | 

## Vỏ cạnh 

Khi nào$k = 0$, thuật toán ngay lập tức trả về 0. Điều này tương ứng với$t < x$, nghĩa là mỗi người tham gia đều kết thúc trước khi bất kỳ người tham gia nào sau đó bắt đầu. Đối với đầu vào$n=5, x=10, t=3$, chúng tôi nhận được$k=0$và mọi giá trị không hài lòng đều bằng 0 vì không có khoảng thời gian nào trùng nhau ở thời điểm kết thúc. 

Khi$k \ge n$, mọi người tham gia đều thấy tất cả những người tham gia sau đó vẫn hoạt động vào thời điểm kết thúc. Vì$n=4, x=1, t=100$, chúng tôi nhận được$k=100 \ge n$, vì vậy câu trả lời trở thành$4 \cdot 3 / 2 = 6$. Điều này phù hợp với thực tế là người tham gia 1 nhìn thấy 3 người khác, người tham gia 2 nhìn thấy 2, người tham gia 3 nhìn thấy 1 và người tham gia 4 không nhìn thấy gì. 

Khi$k = n-1$, cấu trúc chuyển tiếp suôn sẻ từ hành vi không đổi sang hành vi tam giác. Vì$n=4, x=1, t=3$, chúng tôi nhận được$k=3$, và những đóng góp là$3,2,1,0$, tính tổng đúng thành$6$. Điều này kiểm tra tính chính xác của sự phân chia giữa tiền tố không đổi và hậu tố giảm dần.
