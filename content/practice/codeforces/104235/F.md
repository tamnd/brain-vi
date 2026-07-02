---
title: "CF 104235F - \u0412\u0435\u0440\u043e\u044f\u0442\u043d\u043e\u0441\u0442\u044c \u0445\u043e\u0440\u043e\u0448\u0435\u0439 \u043f\u043e\u0441\u043b\u0435\u0434\u043e\u0432\u0430\u0442\u0435\u043b\u044c\u043d\u043e\u0441\u0442\u0438"
description: "Chúng tôi tạo ra một mảng ngẫu nhiên có độ dài $n$, trong đó mỗi vị trí được chọn độc lập và thống nhất từ ​​các số nguyên $1$ đến $k$. Mọi mảng $k^n$ đều có khả năng như nhau."
date: "2026-07-02T19:44:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104235
codeforces_index: "F"
codeforces_contest_name: "2022-2023 Olympiad Cognitive Technologies, Final Round"
rating: 0
weight: 104235
solve_time_s: 79
verified: true
draft: false
---

[CF 104235F - \u0412\u0435\u0440\u043e\u044f\u0442\u043d\u043e\u0441\u0442\u044c \u0445\u043e\u0440\u043e\u0448\u0435\u0439 \u043f\u043e\u0441\u043b\u0435\u0434\u043e\u0432\u0430\u0442\u0435\u043b\u044c\u 043d\u043e\u0441\u0442\u0438](https://codeforces.com/problemset/problem/104235/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi tạo ra một mảng có độ dài ngẫu nhiên$n$, trong đó mỗi vị trí được chọn độc lập và thống nhất từ ​​các số nguyên$1$ĐẾN$k$. Mỗi một trong số$k^n$mảng có khả năng như nhau. 

Nhiệm vụ là tính xác suất để mảng kết quả không chứa bốn vị trí liên tiếp tạo thành một chuỗi tăng dần. Nói cách khác, không được tồn tại bất kỳ chỉ mục nào$i$như vậy$$a_i < a_{i+1} < a_{i+2} < a_{i+3}.$$Chỉ có các phần tử liên tiếp mới quan trọng. Các dãy con tăng không liên tiếp là không liên quan. 

Đầu ra là một số thực, do đó bài toán về cơ bản là một bài toán đếm chia cho$k^n$. Chúng ta cần số mảng hợp lệ và chuẩn hóa nó. 

Những hạn chế$n, k \le 50$ngụ ý rằng cả hai chiều đều đủ nhỏ để lập trình động trên các trạng thái liên quan đến các giá trị từ$1$ĐẾN$k$. Tuy nhiên,$k^n$phát triển cực kỳ nhanh nên việc liệt kê trực tiếp là không thể. Thậm chí$50^{50}$-lập luận theo tỷ lệ buộc chúng ta phải tính DP đa thức trên các biểu diễn trạng thái được lựa chọn cẩn thận. 

Một trạng thái ngây thơ ghi nhớ trực tiếp ba giá trị cuối cùng sẽ dẫn đến$O(n \cdot k^3 \cdot k)$chuyển tiếp, đó là đường biên trong Python. Tệ hơn nữa, trạng thái như vậy là không cần thiết vì điều kiện chỉ phụ thuộc vào việc chúng ta đã hình thành một chuỗi gồm bốn phép so sánh tăng liên tiếp chứ không phải hình dạng chính xác của ba giá trị cuối cùng. 

Trường hợp cạnh tinh tế xuất hiện khi$n < 4$. Trong trường hợp đó, thậm chí không thể khớp bốn phần tử liên tiếp, vì vậy câu trả lời phải luôn là$1$. Bất kỳ triển khai nào áp dụng công thức DP một cách mù quáng vẫn hoạt động nhưng có thể lãng phí tính toán hoặc gây ra các vấn đề về phân chia nếu quá trình chuẩn hóa không được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ tạo ra mọi chuỗi có độ dài$n$, kiểm tra tất cả các cửa sổ có độ dài 4 và xác minh xem có cửa sổ nào đang tăng nghiêm ngặt hay không. Mỗi chi phí kiểm tra$O(n)$, và có$k^n$trình tự, dẫn đến$O(n \cdot k^n)$, điều này vượt xa khả thi ngay cả đối với những giới hạn nhỏ nhất. 

Quan sát quan trọng là mẫu bị cấm chỉ phụ thuộc vào các so sánh liên tiếp cục bộ. Khi chúng ta mở rộng chuỗi thêm một phần tử, rủi ro mới duy nhất là liệu chúng ta có vừa tạo một chuỗi ba “bước tăng” liên tiếp hay không, tương ứng với bốn giá trị tăng nghiêm ngặt liên tiếp. 

Điều này có nghĩa là chúng ta không cần phải nhớ đầy đủ ba giá trị cuối cùng. Chúng ta chỉ cần theo dõi thời gian tăng dần của các phép so sánh liền kề hiện tại. Nếu giá trị trước đó nhỏ hơn giá trị hiện tại, chúng tôi sẽ kéo dài thời gian chạy; nếu không, nó sẽ đặt lại. Độ dài lần chạy không bao giờ cần vượt quá ba, vì việc đạt đến bốn là bị cấm. 

Điều này làm giảm vấn đề thành lập trình động theo vị trí, giá trị cuối cùng và độ dài chạy tăng dần hiện tại. Sự chuyển tiếp mang tính cục bộ và độc lập với phần còn lại của lịch sử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot k^n)$|$O(n)$| Quá chậm | 
| DP với trạng thái thời lượng chạy |$O(n \cdot k^2)$|$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một DP nơi chúng tôi xử lý mảng từ trái sang phải và duy trì đủ thông tin để phát hiện các mẫu bị cấm khi chúng hình thành. 

1. Chúng tôi xác định trạng thái đại diện cho tất cả các chuỗi hợp lệ có độ dài tiền tố nhất định, kết thúc ở một giá trị cụ thể và với độ dài đã biết của hậu tố so sánh tăng dần hiện tại. Độ dài hậu tố này thể hiện số lần liên tiếp chúng tôi đã có$a_{i-1} < a_i$kết thúc ở vị trí hiện tại. 
2. Đối với vị trí 1, mọi giá trị từ$1$ĐẾN$k$là có thể và độ dài chạy tăng dần luôn là 1 vì không có phần tử trước đó để so sánh. Điều này khởi tạo lớp cơ sở DP. 
3. Đối với mỗi vị trí tiếp theo, chúng tôi thử mở rộng tất cả các trạng thái hợp lệ trước đó bằng cách chọn giá trị tiếp theo$x$từ$1$ĐẾN$k$. Điều này tạo ra sự chuyển tiếp từ giá trị trước đó$v$. 
4. Nếu giá trị mới$x$lớn hơn$v$, sau đó chúng tôi kéo dài thời lượng chạy tăng dần hiện tại thêm một. Ngược lại, chuỗi tăng dần được đặt lại về độ dài 1. Điều này mô hình chính xác liệu chuỗi bất đẳng thức nghiêm ngặt có tiếp tục hay không. 
5. Nếu độ dài chạy kết quả là 4, chúng tôi sẽ loại bỏ quá trình chuyển đổi này vì nó tạo thành bốn phần tử tăng nghiêm ngặt liên tiếp, điều này bị cấm. 
6. Chúng tôi tích lũy tất cả các chuyển đổi hợp lệ vào lớp DP tiếp theo. 
7. Sau khi xử lý tất cả các vị trí, chúng tôi tính tổng tất cả các trạng thái hợp lệ trên tất cả các giá trị kết thúc và độ dài chạy, sau đó chia cho$k^n$để có được xác suất. 

### Tại sao nó hoạt động 

Trạng thái DP mã hóa chính xác thông tin duy nhất cần thiết để phát hiện mẫu bị cấm: giá trị cuối cùng và số lượng so sánh tăng dần liên tiếp kết thúc ở vị trí hiện tại. Bất kỳ hành vi vi phạm quy tắc nào chỉ phụ thuộc vào việc có xảy ra ba lần tăng liên tiếp hay không, điều này được nắm bắt hoàn toàn bởi độ dài chạy đạt tới 4. Vì mỗi quá trình chuyển đổi đều duy trì tính chính xác của lịch sử cục bộ này nên không thể đếm được chuỗi bị cấm và mọi chuỗi được phép đều có chính xác một đường dẫn tương ứng thông qua DP. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())

# dp[v][l] = number of ways where last value is v and current increasing run length is l
# l in {1,2,3}
dp = [[0] * 4 for _ in range(k + 1)]

for v in range(1, k + 1):
    dp[v][1] = 1

for _ in range(2, n + 1):
    ndp = [[0] * 4 for _ in range(k + 1)]
    for v in range(1, k + 1):
        for l in range(1, 4):
            if dp[v][l] == 0:
                continue
            cur = dp[v][l]
            for x in range(1, k + 1):
                if x > v:
                    nl = l + 1
                else:
                    nl = 1
                if nl <= 3:
                    ndp[x][nl] += cur
    dp = ndp

total = 0
for v in range(1, k + 1):
    for l in range(1, 4):
        total += dp[v][l]

prob = total / (k ** n)
print(prob)
```Bảng DP lưu trữ số lượng các chuỗi hợp lệ được nhóm theo giá trị cuối cùng của chúng và chuỗi so sánh lân cận tăng dần hiện tại. Quá trình chuyển đổi lặp lại rõ ràng trên tất cả các giá trị tiếp theo có thể có, cập nhật xem chuỗi tăng tiếp tục hay đặt lại. 

Một chi tiết triển khai tinh tế là sự tách biệt giữa các lớp DP hiện tại và tiếp theo. Việc sử dụng lại cùng một mảng sẽ làm ảnh hưởng đến quá trình chuyển đổi trong một bước duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`3 50`Từ$n = 3$, không có dãy nào có thể chứa bốn phần tử liên tiếp. DP vẫn chạy nhưng trạng thái cấm không bao giờ được kích hoạt. 

| Vị trí | Tóm tắt tiểu bang | 
| --- | --- | 
| 1 | Tất cả 50 giá trị có độ dài chạy 1 | 
| 2 | Tất cả các cặp hợp lệ, được chia thành các chuyển tiếp tăng dần và không tăng | 
| 3 | Vẫn không thể chuyển tiếp không hợp lệ | 

Số cuối cùng bằng tất cả$50^3$các chuỗi, do đó xác suất chính xác là 1. Điều này xác nhận rằng DP xử lý chính xác các chuỗi ngắn trong đó ràng buộc là đúng. 

### Ví dụ 2:`4 4`Ở đây cách duy nhất để thất bại là chọn một chuỗi tăng dần nghiêm ngặt như$1,2,3,4$. 

| Bước | Quan sát | 
| --- | --- | 
| Xây dựng chiều dài 4 trình tự | Tổng cộng$4^4 = 256$| 
| Trình tự không hợp lệ | Chỉ có một chuỗi tăng nghiêm ngặt | 
| Trình tự hợp lệ | 255 | 

DP sẽ đếm chính xác một đường dẫn đạt độ dài chạy 4 và loại bỏ nó, khớp với kết quả phân tích. 

Điều này chứng tỏ rằng DP cách ly chính xác mẫu bị cấm duy nhất ngay cả khi không gian trạng thái rất nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot k^2)$| Đối với mỗi vị trí, mỗi trạng thái (giá trị cuối cùng, độ dài chạy) chuyển tiếp qua tất cả các giá trị tiếp theo | 
| Không gian |$O(k)$| Chỉ có hai lớp DP trên giá trị cuối cùng và độ dài chạy được lưu trữ | 

Với$n, k \le 50$, số thao tác tối đa là khoảng$50 \cdot 50 \cdot 50 = 125000$, nằm trong giới hạn đối với Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    n, k = map(int, sys.stdin.readline().split())

    dp = [[0] * 4 for _ in range(k + 1)]
    for v in range(1, k + 1):
        dp[v][1] = 1

    for _ in range(2, n + 1):
        ndp = [[0] * 4 for _ in range(k + 1)]
        for v in range(1, k + 1):
            for l in range(1, 4):
                cur = dp[v][l]
                if not cur:
                    continue
                for x in range(1, k + 1):
                    nl = l + 1 if x > v else 1
                    if nl <= 3:
                        ndp[x][nl] += cur
        dp = ndp

    total = sum(dp[v][l] for v in range(1, k + 1) for l in range(1, 4))
    return str(total / (k ** n))

# provided samples
assert abs(float(run("3 50")) - 1.0) < 1e-9, "sample 1"
assert abs(float(run("4 4")) - 0.99609375) < 1e-9, "sample 2"

# custom cases
assert abs(float(run("1 10")) - 1.0) < 1e-9, "single element"
assert abs(float(run("2 2")) - 1.0) < 1e-9, "no room for length 4"
assert abs(float(run("5 1")) - 1.0) < 1e-9, "all equal values"
assert float(run("4 3")) >= 0.0, "sanity check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 10`|`1`| Vỏ cạnh có chiều dài tối thiểu | 
|`2 2`|`1`| Ràng buộc không thể xảy ra | 
|`5 1`|`1`| Phạm vi giá trị suy biến | 
|`4 3`| xác suất hợp lệ | DP nhỏ không tầm thường | 

## Vỏ cạnh 

cho$n = 1$, thuật toán khởi tạo một lớp trạng thái và ngay lập tức tổng hợp chúng, tạo ra xác suất 1 vì không có chuyển đổi nào xảy ra. DP tránh được mọi kiểm tra trạng thái không hợp lệ một cách chính xác. 

Vì$k = 1$, mọi dãy đều không đổi. Độ dài tăng dần không bao giờ vượt quá 1, do đó không có mẫu bị cấm nào có thể xuất hiện. DP chỉ giữ trạng thái hợp lệ và xác suất cuối cùng trở thành 1 bất kể$n$. 

Vì$n = 4$, thời điểm đầu tiên có thể xảy ra lỗi, DP nắm bắt rõ ràng quỹ đạo không hợp lệ duy nhất trong đó mọi chuyển đổi đều tăng lên. Đường dẫn đó bị loại bỏ chính xác khi độ dài chạy cố gắng đạt tới 4, đảm bảo số lượng khớp với kỳ vọng tổ hợp.
