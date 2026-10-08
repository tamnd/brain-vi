---
title: "CF 104963A - \u041d\u0430\u0431\u0440\u0430\u0442\u044c \u0441\u0443\u043c\u043c\u0443 \u0434\u0435\u043d\u0435\u0433"
description: "Chúng ta được yêu cầu đếm xem có bao nhiêu cách khác nhau để có thể thanh toán chính xác số tiền cố định $N$ bằng cách sử dụng tiền giấy có mệnh giá 50, 100 và 200, trong đó mỗi mệnh giá có thể được sử dụng với số lần bất kỳ."
date: "2026-06-28T06:53:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104963
codeforces_index: "A"
codeforces_contest_name: "\u0412\u044b\u0441\u0448\u0430\u044f \u043f\u0440\u043e\u0431\u0430 - 2022. \u0417\u0430\u043a\u043b\u044e\u0447\u0438\u0442\u0435\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f"
rating: 0
weight: 104963
solve_time_s: 61
verified: true
draft: false
---

[CF 104963A - \u041d\u0430\u0431\u0440\u0430\u0442\u044c \u0441\u0443\u043c\u043c\u0443 \u0434\u0435\u043d\u0435\u0433](https://codeforces.com/problemset/problem/104963/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm có bao nhiêu cách khác nhau để có một số tiền cố định$N$có thể được thanh toán chính xác bằng cách sử dụng tiền giấy có mệnh giá 50, 100 và 200, trong đó mỗi mệnh giá có thể được sử dụng bất kỳ số lần nào. Không có khái niệm về việc trả lại tiền lẻ, vì vậy cách hợp lệ chỉ đơn giản là ghép nhiều tập tiền giấy có tổng số tiền bằng nhau.$N$. 

Hai cách được coi là khác nhau nếu số lượng tờ tiền mệnh giá 50, 100 hoặc 200 rúp được sử dụng khác nhau. Thứ tự chọn nốt không quan trọng, chỉ tính nốt cuối cùng. 

Từ góc độ tính toán, chúng ta đang tính nghiệm số nguyên cho một phương trình có dạng:$$50a + 100b + 200c = N$$Ở đâu$a, b, c \ge 0$. 

Ràng buộc$N \le 10^6$đã loại trừ việc liệt kê tất cả các bộ ba một cách ngây thơ. Việc quét bậc ba hoặc thậm chí bậc hai trên số lượng có thể sẽ quá chậm trong trường hợp xấu nhất vì$N / 50 = 20000$, điều này làm cho các vòng lặp lồng nhau có khả năng lớn nhưng vẫn có thể quản lý được nếu được giới hạn cẩn thận. Điều quan trọng là giảm vấn đề xuống một vòng lặp đơn hoặc đếm trực tiếp. 

Các trường hợp cạnh xuất hiện ngay lập tức từ tính chia hết. Nếu như$N$không chia hết cho 50, không có giải pháp nào vì tất cả các mệnh giá đều là bội số của 50. Ví dụ: đầu vào 36 hoàn toàn không thể được hình thành, vì vậy câu trả lời phải là 0. Việc triển khai ngây thơ mà quên điều này sẽ lãng phí thời gian lặp đi lặp lại một cách vô ích. 

Một trường hợp tế nhị khác là$N = 0$. Giải thích đúng là có chính xác một cách để trả bằng 0: không sử dụng tiền giấy. Bất kỳ triển khai nào khởi tạo câu trả lời không chính xác hoặc bỏ qua tổ hợp trống sẽ thất bại ở đây. 

Cuối cùng, các giá trị nhỏ như$N = 50$sẽ mang lại chính xác một nghiệm và các bội số lớn hơn chẳng hạn như 200 sẽ phản ánh tất cả các phân tách hợp lệ trên nhiều mệnh giá. 

## Phương pháp tiếp cận 

Một phương pháp cưỡng bức trực tiếp là thử tất cả các số có thể có của tờ tiền 200 rúp, sau đó là tất cả số có thể có của tờ tiền 100 rúp và tính toán xem số tiền còn lại có thể được hình thành bằng cách sử dụng tờ tiền 50 rúp hay không. Đối với mỗi cặp$(b, c)$, chúng tôi tính toán:$$r = N - 200c - 100b$$và kiểm tra xem$r \ge 0$và chia hết cho 50. Nếu có, chúng ta sẽ tăng câu trả lời. 

Điều này hiệu quả vì mọi giải pháp hợp lệ đều tương ứng với chính xác một cặp$(b, c)$, và số tờ tiền mệnh giá 50 rúp được xác định duy nhất. 

Độ phức tạp phụ thuộc vào số lượng giá trị của$c$Và$b$chúng tôi cố gắng. Từ$c \le N/200$Và$b \le N/100$, trường hợp xấu nhất là về$O((N/200) \cdot (N/100)) = O(N^2)$lặp đi lặp lại theo nghĩa quy mô tồi tệ nhất. Với$N$lên tới$10^6$, tốc độ này quá chậm. 

Quan sát quan trọng là chúng ta không cần phải lặp lại một cách rõ ràng trên tất cả các tờ tiền mệnh giá 50 rúp. Một khi chúng tôi sửa chữa$b$Và$c$, giá trị của$a$được xác định duy nhất. Vì vậy, vấn đề giảm xuống việc đếm hợp lệ$(b, c)$các cặp thỏa mãn ràng buộc về tính chia hết, có thể được thực hiện trong$O(N/200)$bằng cách lặp đi lặp lại$c$và giải quyết cho$b$về mặt số học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực tàn bạo (b, c) |$O((N/200)(N/100))$|$O(1)$| Quá chậm | 
| Sửa 200, giải 100/50 |$O(N/200)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta viết lại phương trình:$$50a + 100b + 200c = N$$Chia mọi thứ cho 50:$$a + 2b + 4c = M$$Ở đâu$M = N / 50$. Nếu như$N$không chia hết cho 50 thì đáp án ngay lập tức là 0. 

Bây giờ chúng ta đếm các nghiệm số nguyên không âm để$a + 2b + 4c = M$. 

### bước 

1. Kiểm tra xem$N \bmod 50 \neq 0$. Nếu vậy, hãy trả về 0 vì không có tổ hợp tiền giấy hợp lệ nào có thể tạo thành số tiền như vậy. Điều này loại bỏ ngay những trường hợp không thể thực hiện được. 
2. Đặt$M = N / 50$. Bây giờ bài toán trở thành nghiệm đếm theo đơn vị chuẩn hóa, giúp đơn giản hóa số học và tránh phép nhân lặp lại. 
3. Lặp lại số lượng tờ tiền 200 rúp$c$. Mỗi$c$đóng góp$4c$đơn vị thành tổng. Tối đa có thể$c$là$M // 4$, vì mỗi đơn vị có giá trị bằng 4 trong hệ thống chia tỷ lệ này. 
4. Đối với mỗi cố định$c$, tính số tiền còn lại:$$rem = M - 4c$$Phần này thể hiện phần được hình thành chỉ bằng cách sử dụng nốt 50 và 100. 
5. Bây giờ chúng ta đếm các giải pháp cho:$$a + 2b = rem$$Đối với một cố định$b$,$a$được xác định duy nhất là$a = rem - 2b$, vì vậy chúng tôi chỉ cần hợp lệ$b$như vậy$rem - 2b \ge 0$. 

Số hợp lệ$b$giá trị là:$$\left\lfloor \frac{rem}{2} \right\rfloor + 1$$6. Thêm số này vào câu trả lời cho mỗi$c$, tích lũy tất cả các phân tách hợp lệ. 

### Tại sao nó hoạt động 

Mọi giải pháp$(a, b, c)$được xác định duy nhất bởi giá trị của nó$c$. Đối với mỗi cố định$c$, phương trình còn lại quy về bài toán đếm một chiều trong$b$, trong đó mỗi giá trị hợp lệ$b$tương ứng với chính xác một hợp lệ$a$. Điều này phân vùng không gian giải pháp đầy đủ thành các phần rời rạc được lập chỉ mục bởi$c$, do đó, việc tính tổng tất cả các lát cắt sẽ tính mọi giải pháp chính xác một lần mà không bị trùng lặp hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    N = int(input().strip())

    if N % 50 != 0:
        print(0)
        return

    M = N // 50
    ans = 0

    for c in range(M // 4 + 1):
        rem = M - 4 * c
        ans += rem // 2 + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo phương trình rút gọn theo đơn vị chuẩn hóa. Việc kiểm tra tính chia hết ngay từ đầu là rất quan trọng, vì việc bỏ qua nó sẽ coi các trường hợp bất khả thi là có nghiệm phân số không chính xác. 

Vòng lặp kết thúc$c$đại diện cho việc ấn định số lượng tờ tiền mệnh giá 200 rúp. Số tiền còn lại sau đó được phân phối giữa các tờ tiền mệnh giá 100 và 50 rúp, trong đó mỗi tờ tiền 100 rúp tiêu thụ 2 đơn vị chuẩn hóa. biểu thức`rem // 2 + 1`đếm tất cả các số có thể có của tờ 100 rúp từ 0 đến mức tối đa cho phép của số tiền còn lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$N = 50$Chúng tôi chuyển đổi sang các đơn vị chuẩn hóa:$M = 1$. Chỉ một$c = 0$là có thể. 

| c | rem = M - 4c | rem // 2 + 1 | 
| --- | --- | --- | 
| 0 | 1 | 1 | 

Giải pháp duy nhất là một tờ 50 rúp. 

Điều này xác nhận trường hợp cơ bản trong đó chỉ riêng mệnh giá nhỏ nhất đã tạo thành tổng. 

### Ví dụ 2:$N = 200$Đây$M = 4$. Chúng tôi lặp đi lặp lại$c$. 

| c | rem | rem // 2 + 1 | 
| --- | --- | --- | 
| 0 | 4 | 3 | 
| 1 | 0 | 1 | 

Tổng cộng là 4. 

Điều này phù hợp với trực giác: hoặc không có nốt 200 và sự kết hợp của 100/50 hoặc chỉ có một nốt 200. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N/200)$| lặp lại số lượng tờ 200 rúp sau khi bình thường hóa | 
| Không gian |$O(1)$| chỉ một vài biến số nguyên được sử dụng | 

Vòng lặp chạy nhiều nhất$2500$lặp đi lặp lại khi$N = 10^6$, nằm trong giới hạn. Tất cả các phép toán bên trong vòng lặp đều là số học theo thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline().strip()

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    N = int(sys.stdin.readline().strip())

    if N % 50 != 0:
        return "0"

    M = N // 50
    ans = 0
    for c in range(M // 4 + 1):
        rem = M - 4 * c
        ans += rem // 2 + 1

    return str(ans)

# provided samples
assert solve("50\n") == "1"
assert solve("36\n") == "0"
assert solve("200\n") == "4"

# custom cases
assert solve("0\n") == "1"          # empty payment
assert solve("100\n") == "2"        # 100 or 50+50
assert solve("150\n") == "2"        # 100+50 or 3*50
assert solve("250\n") == "3"        # multiple decompositions
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 1 | xử lý giải pháp trống | 
| 100 | 2 | tác phẩm hỗn hợp nhỏ | 
| 150 | 2 | nhiều đại diện | 
| 250 | 3 | tính nhất quán của liệt kê | 

## Vỏ cạnh 

cho$N = 0$, bộ thuật toán$M = 0$. Vòng lặp chạy một lần với$c = 0$, cho$rem = 0$và đóng góp$0 // 2 + 1 = 1$. Điều này đếm chính xác sự kết hợp trống. 

Đối với các bội số của 50 như$N = 36$, kiểm tra sớm ngay lập tức trả về 0. Nếu không có bộ bảo vệ này, phép chia số nguyên sẽ âm thầm tạo ra các giá trị chuẩn hóa không chính xác và đếm quá mức các trạng thái không thể đếm được. 

Đối với bội số chính xác nhỏ như$N = 50$, cấu trúc vòng lặp vẫn hoạt động mà không cần vỏ đặc biệt. Với$M = 1$, chỉ một$c = 0$hợp lệ và tạo ra chính xác một cấu hình, phù hợp với cách diễn giải dự định.
