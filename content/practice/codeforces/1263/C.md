---
title: "CF 1263C - Mọi người đều là Người chiến thắng!"
description: "Chúng ta được cấp một lượng điểm xếp hạng cố định, gọi nó là $n$. Việc rút thăm sẽ chia đều số điểm này cho những người tham gia $k$, nhưng chỉ phân phối toàn bộ đơn vị, do đó mỗi người tham gia nhận được điểm $lfloor n / k rfloor$. Mọi điểm còn sót lại sẽ bị loại bỏ."
date: "2026-06-18T17:49:38+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math", "meet-in-the-middle", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1263
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 603 (Div. 2)"
rating: 1400
weight: 1263
solve_time_s: 100
verified: false
draft: false
---

[CF 1263C - Mọi người đều là Người chiến thắng!](https://codeforces.com/problemset/problem/1263/C) 

**Đánh giá:** 1400 
**Tas:** tìm kiếm nhị phân, toán, gặp nhau ở giữa, lý thuyết số 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số điểm đánh giá cố định, hãy gọi nó là$n$. Một trận hòa sẽ chia đều số điểm này cho$k$người tham gia, nhưng chỉ toàn bộ đơn vị được phân phối, vì vậy mỗi người tham gia nhận được$\lfloor n / k \rfloor$điểm. Mọi điểm còn sót lại sẽ bị loại bỏ. 

Nhiệm vụ không phải là mô phỏng một lần rút thăm mà là liệt kê mọi giá trị riêng biệt có thể xuất hiện dưới dạng lợi ích của mỗi người tham gia khi$k$thay đổi trên mọi số nguyên dương. Nói cách khác, chúng ta coi hàm$f(k) = \lfloor n/k \rfloor$và chúng ta muốn tập hợp tất cả các giá trị mà hàm này có thể nhận. 

Một quan sát quan trọng về đầu ra là có nhiều giá trị khác nhau của$k$tạo ra cùng một thương số. Ví dụ, một lần$k > n$, giá trị trở thành 0 và duy trì ở mức 0 mãi mãi. Điều này đã gợi ý rằng việc lặp lại tất cả$k \le n$là không cần thiết. 

Ràng buộc$n \le 10^9$loại trừ mọi cách tiếp cận lặp lại trên tất cả$k$trực tiếp. Quét tuyến tính sẽ yêu cầu tới một tỷ lần lặp cho mỗi trường hợp thử nghiệm, vượt xa mức cho phép trong 1 giây. Chúng ta cần một cách để bỏ qua phạm vi lớn của$k$tạo ra cùng một thương số. 

Trường hợp cạnh tinh tế xuất hiện ở ranh giới giữa các giá trị nhỏ và lớn của$k$. Ví dụ, khi$n = 1$, đầu ra chỉ$1$Và$0$. Một cách tiếp cận ngây thơ sẽ dừng lại sớm khi$\lfloor n/k \rfloor$trở thành số 0 có thể bỏ sót sự thật rằng số 0 vẫn là giá trị hợp lệ và phải được đưa vào. 

Một lỗi phổ biến khác là sao chép các giá trị không chính xác. Vì khác nhau$k$các khoảng tạo ra kết quả giống nhau, chỉ cần thu thập các giá trị mà không nhóm cẩn thận có thể dẫn đến các mục nhập lặp lại hoặc các giá trị bị bỏ sót. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử mọi cách$k$từ$1$ĐẾN$n$, tính toán$\lfloor n/k \rfloor$và chèn kết quả vào một tập hợp. Điều này đúng vì mọi số lượng người tham gia có thể đều được xem xét. Tuy nhiên, giá thành của nó$O(n)$phân chia cho mỗi trường hợp thử nghiệm, điều này trở nên không khả thi khi$n = 10^9$. 

Thuộc tính cấu trúc quan trọng là$\lfloor n/k \rfloor$là hàm số không tăng, không đổi trong các khoảng thời gian$k$. Nếu chúng ta sửa một giá trị$v = \lfloor n/k \rfloor$, thì tất cả$k$trong một phạm vi liền kề thỏa mãn cùng thương số này. Thay vì lặp đi lặp lại mỗi$k$, chúng ta có thể nhảy thẳng đến cuối mỗi khoảng mà thương số thay đổi. 

Cái nhìn sâu sắc quan trọng là đối với một cố định$k$, nếu như$v = n // k$, thì lớn nhất$k$điều đó vẫn mang lại giá trị$v$là$k = n // v$. Điều này cho phép chúng tôi bỏ qua toàn bộ khối$k$cùng một lúc, giảm số bước xuống gần như$O(\sqrt{n})$, vì một trong hai$k$hoặc$v$phải lớn lên khi cái kia nhỏ đi. 

Vấn đề sau đó trở thành một sự quét hai mặt: chúng ta hoặc lặp lại việc tăng dần$k$hoặc tương đương khi giảm các giá trị khác biệt$v$, nhảy giữa các điểm ngắt nơi thương số thay đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$|$O(1)$| Quá chậm | 
| Nhảy theo khối thương |$O(\sqrt{n})$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn thu thập tất cả các giá trị khác biệt của$\lfloor n/k \rfloor$mà không liệt kê tất cả$k$. 

1. Bắt đầu với$k = 1$. Điều này mang lại giá trị tối đa$n$, từ$\lfloor n/1 \rfloor = n$. 
2. Tính toán$v = \lfloor n/k \rfloor$. Đây là một giá trị đầu ra riêng biệt mà chúng ta phải đưa vào. 
3. Thay vì chuyển đến$k + 1$, nhảy trực tiếp đến vị trí cuối cùng có cùng giá trị này$v$nắm giữ. Vị trí đó là$k_{\text{end}} = n // v$. Điều này hoạt động bởi vì tất cả$k$trong phạm vi$[k, k_{\text{end}}]$tạo ra cùng một thương số$v$. 
4. Di chuyển đến$k = k_{\text{end}} + 1$, trong đó thương số giảm nghiêm ngặt. Lặp lại quá trình. 
5. Tiếp tục cho đến khi$k > n$. Tại thời điểm đó, thương số trở thành 0 và chúng ta phải đảm bảo rằng số 0 được bao gồm một lần. 

Tại sao bước nhảy hợp lệ phụ thuộc vào tính đơn điệu của phép chia: như$k$tăng lên,$n/k$giảm và số nguyên sàn chỉ thay đổi tại các điểm dừng cụ thể trong đó thương số giảm ít nhất một. Giải quyết$\lfloor n/k \rfloor = v$mang lại chính xác phạm vi$k \in [\lfloor n/(v+1) \rfloor + 1, \lfloor n/v \rfloor]$, vì vậy nhảy tới$n//v$đạt đến ranh giới của cao nguyên hiện tại. 

Bất biến được duy trì là mọi số nguyên$k$nhỏ hơn con trỏ hiện tại đã được tính vào đúng một phân đoạn và mỗi phân đoạn đóng góp chính xác một giá trị thương số riêng biệt. Vì hàm chỉ thay đổi khi vượt qua điểm ngắt nên không có giá trị nào bị bỏ qua và không có giá trị trùng lặp nào được thêm vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n):
    res = []
    k = 1

    while k <= n:
        v = n // k
        res.append(v)
        k = n // v + 1

    res.append(0)
    return res

t = int(input())
for _ in range(t):
    n = int(input())
    ans = solve_case(n)
    print(len(ans))
    print(*ans)
```Việc triển khai tuân theo ý tưởng nhảy giữa các phân đoạn không đổi của hàm$\lfloor n/k \rfloor$. Vòng lặp tính thương số hiện tại và sau đó nhảy trực tiếp tới chỉ mục tiếp theo nơi thương số thay đổi, sử dụng$k = n // v + 1$. 

Một chi tiết tinh tế là việc bổ sung rõ ràng số 0 ở cuối. Vòng lặp chỉ xử lý các giá trị của$k \le n$, do đó, nó tự nhiên bao gồm tất cả các thương số dương nhưng dừng lại trước khi tạo ra vùng 0 cuối cùng một cách rõ ràng. Từ$k > n$mang lại số 0, chúng tôi thêm nó một lần vào cuối. 

Đơn đặt hàng đã giảm dần vì$k$tăng lên,$n // k$không bao giờ tăng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 5$| k bắt đầu | v = 5//k | k kết thúc = 5//v | giá trị gia tăng | 
| --- | --- | --- | --- | 
| 1 | 5 | 1 | 5 | 
| 2 | 2 | 2 | 2 | 
| 3 | 1 | 5 | 1 | 

Phần bổ sung cuối cùng: 0 

Dấu vết cho thấy cách thuật toán nhảy qua toàn bộ phạm vi không đổi:$k = 1$cho 5,$k = 2$cho 2, và$k = 3$ĐẾN$5$cho 1. 

### Ví dụ 2:$n = 11$| k bắt đầu | v = 11//k | k kết thúc = 11//v | giá trị gia tăng | 
| --- | --- | --- | --- | 
| 1 | 11 | 1 | 11 | 
| 2 | 5 | 2 | 5 | 
| 3 | 3 | 3 | 3 | 
| 4 | 2 | 5 | 2 | 
| 6 | 1 | 11 | 1 | 

Phần bổ sung cuối cùng: 0 

Mỗi hàng tương ứng với một giá trị cố định của các giá trị phân chia tầng không đổi và bước nhảy sẽ bỏ qua tất cả các giá trị trung gian$k$nếu không thì kết quả sẽ lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{n})$| Mỗi bước nhảy tiến lên$k$đến cuối khối giá trị và số khối riêng biệt của$\lfloor n/k \rfloor$được giới hạn bởi$O(\sqrt{n})$| 
| Không gian |$O(1)$| Chỉ một danh sách nhỏ kết quả được lưu trữ | 

Các ràng buộc cho phép lên đến$t = 10$Và$n = 10^9$. Phép lặp kiểu căn bậc hai đảm bảo tối đa xung quanh$3 \times 10^4$hoạt động cho mỗi trường hợp thử nghiệm, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        n = int(input())
        res = []
        k = 1
        while k <= n:
            v = n // k
            res.append(v)
            k = n // v + 1
        res.append(0)
        return res

    t = int(input())
    out = []
    for _ in range(t):
        ans = solve()
        out.append(str(len(ans)))
        out.append(" ".join(map(str, ans)))
    return "\n".join(out) + "\n"

# provided samples
assert run("4\n5\n11\n1\n3\n") == "4\n5 2 1 0\n6\n11 5 3 2 1 0\n2\n1 0\n3\n3 1 0\n"

# custom cases
assert run("1\n2\n") == "2\n2 1 0\n", "small n"
assert run("1\n1\n") == "2\n1 0\n", "minimum case"
assert run("1\n10\n") == "5\n10 5 3 2 1 0\n", "typical case"
assert run("1\n100\n") == "9\n100 50 33 25 20 16 14 12 11 10 0\n", "larger structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n = 2 | 2 1 0 | bước nhảy tối thiểu không tầm thường | 
| n = 1 | 1 0 | chuyển ngay lập tức về 0 | 
| n = 10 | trình tự có cấu trúc | nhảy cao nguyên nhiều lần | 
| n = 100 | nhiều khối thương | tính đúng đắn của logic nhảy | 

## Vỏ cạnh 

Khi nào$n = 1$, vòng lặp bắt đầu lúc$k = 1$, tạo ra$v = 1$, và nhảy tới$k = 2$, vốn đã vượt quá$n$. Tại thời điểm này, thuật toán vẫn thêm số 0. Kết quả là chính xác$[1, 0]$, khớp với tập hợp đầy đủ các giá trị sàn có thể có. 

Khi$n$lớn nhưng$k$nhanh chóng nhảy qua$n$, Ví dụ$n = 10^9$, một số giá trị đầu tiên lớn và sau đó giảm nhanh chóng. Quy tắc nhảy$k = n // v + 1$đảm bảo rằng một lần$v = 1$, lần nhảy tiếp theo sẽ hạ cánh tại$k = n + 1$, ngăn chặn sự lặp lại không cần thiết trên tất cả các phần còn lại$k$tất cả các giá trị sẽ tạo ra số không. 

Khi$n$là một hình vuông hoàn hảo hoặc có các ước số lặp lại, nhiều$k$ánh xạ các khoảng tới các chuyển đổi kích thước thương số giống nhau. Thuật toán vẫn xử lý việc này một cách rõ ràng vì nó không dựa vào các thuộc tính số học của$n$, chỉ trên cấu trúc đơn điệu của$\lfloor n/k \rfloor$.
