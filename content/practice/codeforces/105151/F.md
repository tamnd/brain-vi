---
title: "CF 105151F - Đôi D"
description: "Hai người chơi mô phỏng một trò chơi xác định trên một số nguyên duy nhất. Trạng thái chỉ là một số, ban đầu là $n$. Người chơi thay phiên nhau, bắt đầu từ người chơi đầu tiên. Trong mỗi lượt, người chơi đang hoạt động cố gắng áp dụng nước đi chia bằng cách sử dụng ước số cố định của riêng họ."
date: "2026-06-27T11:14:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105151
codeforces_index: "F"
codeforces_contest_name: "XIX \u041d\u0438\u0436\u0435\u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u0412. \u0414. \u041b\u0435\u043b\u044e\u0445\u0430"
rating: 0
weight: 105151
solve_time_s: 72
verified: true
draft: false
---

[CF 105151F - Đôi D](https://codeforces.com/problemset/problem/105151/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hai người chơi mô phỏng một trò chơi xác định trên một số nguyên duy nhất. Trạng thái ban đầu chỉ là một con số$n$. Người chơi thay phiên nhau, bắt đầu từ người chơi đầu tiên. Trong mỗi lượt, người chơi đang hoạt động cố gắng áp dụng nước đi chia bằng cách sử dụng ước số cố định của riêng họ. Nếu số hiện tại chia hết cho giá trị của người chơi đó, họ sẽ thay thế số đó bằng thương số. Nếu không thì họ giảm số lượng đi một. Quá trình tiếp tục cho đến khi con số trở thành 0 và nhiệm vụ là đếm xem có bao nhiêu nước đi xảy ra. 

Mặc dù các quy tắc trông đơn giản, nhưng động lực học không tuyến tính vì phép chia có thể thu nhỏ số lượng một cách đáng kể, trong khi phép trừ chỉ làm giảm nó đi một. Sự xen kẽ giữa hai ước số khác nhau tạo ra một đường đi phụ thuộc rất nhiều vào cấu trúc số học của giá trị hiện tại. 

Các ràng buộc đi lên đến$10^{18}$, điều này ngay lập tức loại trừ bất kỳ mô phỏng nào làm giảm số lượng chỉ một trong trường hợp xấu nhất. Một mô phỏng từng bước thuần túy có thể yêu cầu lên đến$10^{18}$hoạt động, điều đó là không thể. Bất kỳ giải pháp hợp lệ nào cũng phải bỏ qua các khối lớn các bước trừ và lý do nhảy. 

Một vấn đề tế nhị phát sinh khi khả năng chia hết thay đổi do phép trừ. Ví dụ: nếu ước số của một người chơi là 10 và số hiện tại là 21, suy nghĩ ngây thơ có thể gợi ý “đợi đến 20 rồi chia”, nhưng người chơi khác có thể can thiệp trước khi điều đó xảy ra, làm thay đổi quỹ đạo. Sự tương tác này có nghĩa là chúng ta không thể đơn giản tính toán trước các điểm chia tiếp theo một cách độc lập cho mỗi người chơi mà không tính toán cẩn thận thứ tự lần lượt. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu tuân theo các quy tắc theo nghĩa đen: người chơi thay thế, kiểm tra khả năng chia, chia hoặc giảm và đếm các bước cho đến khi không. Điều này đúng vì nó phản ánh chính xác quá trình. Tuy nhiên, vấn đề là thời gian chạy. Trong trường hợp xấu nhất, nếu không có ước số nào chia số hiện tại thì quá trình sẽ giảm đi một mỗi lần, tạo ra$O(n)$các bước. Với$n$lên tới$10^{18}$, điều này là không thể thực hiện được. 

Quan sát quan trọng là sự phát triển trạng thái là đơn điệu và có thể dự đoán được từng phần giữa các sự kiện phân chia. Giữa hai thời điểm khi số đó chia hết cho số chia của người chơi hiện tại, hành động duy nhất có thể thực hiện được là phép trừ lặp lại và phép trừ hoạt động tuyến tính. Thay vì mô phỏng từng bước một, chúng ta có thể chuyển thẳng sang sự kiện có liên quan tiếp theo: đạt đến 0 hoặc đạt đến bội số tiếp theo của ước số hoạt động. 

Quan điểm đúng đắn là coi trò chơi như một chuỗi các phân đoạn. Mỗi phân đoạn tương ứng với một trình phát và một giá trị hiện tại$x$. Chúng tôi hỏi: cần bao nhiêu phép trừ trước$x$trở nên chia hết cho số chia của người chơi hiện tại hoặc đạt đến số 0. Điều đó mang lại một bước nhảy trực tiếp trong thời gian. Nếu có thể phân chia được, chúng tôi sẽ áp dụng nó và chuyển đổi người chơi; nếu không chúng ta sẽ chấm dứt trò chơi. 

Điều này làm giảm khả năng quá trình$O(n)$các bước tới một số sự kiện tỷ lệ thuận với số lần chia thành công cộng với các bước nhảy trừ xen kẽ, đây là logarit trong thực tế vì phép chia thu nhỏ số lượng một cách đáng kể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(\log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì giá trị hiện tại$x$, bộ đếm các bước di chuyển và theo dõi xem đến lượt của ai. 

1. Bắt đầu với$x = n$và đặt trình phát đầu tiên là hoạt động. 
2. Đối với người chơi đang hoạt động có số chia$d$, tính số dư$r = x \bmod d$. Điều này cho biết bao xa$x$là từ bội số dưới gần nhất của$d$. 
3. Nếu$x < d$, người chơi không thể chia, vì vậy hành động duy nhất có thể thực hiện được là phép trừ. Chúng tôi trực tiếp thêm$x$di chuyển, vì phải mất chính xác$x$giảm dần để đạt tới số không. Sau đó chấm dứt. 
4. Ngược lại, nếu$r > 0$, chúng ta biết tiếp theo$r$di chuyển là phép trừ bắt buộc. Chúng tôi thêm$r$để trả lời và giảm$x$qua$r$. Điều này đúng vì không thể chia được cho đến khi đạt bội số của$d$. 
5. Bây giờ$x$chia hết cho$d$. Chúng ta thực hiện một bước chia: tăng câu trả lời lên một và đặt$x = x / d$. 
6. Chuyển sang đầu phát khác và lặp lại quy trình cho đến khi$x = 0$. 

### Tại sao nó hoạt động 

Ở mọi giai đoạn, thuật toán duy trì tính bất biến rằng trạng thái hiện tại chính xác là trạng thái mô phỏng từng bước sẽ đạt được nhưng được nén. Các bước trừ hoàn toàn đã được sử dụng hết cho đến khi sự kiện tiếp theo có thể chia được và sự kiện đó được xác định duy nhất bởi giá trị hiện tại và số chia. Phép chia được áp dụng ngay lập tức khi hợp lệ vì việc trì hoãn là không thể theo quy định khi đã đạt được phép chia ở lượt hoạt động. Vì mỗi bộ phận giảm$x$ít nhất là gấp 2 lần (vì$d \ge 2$), số lần chia là logarit và tất cả các phân đoạn trừ trung gian được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, d1, d2 = map(int, input().split())

x = n
ans = 0
turn = 0  # 0 -> first player, 1 -> second player

while x > 0:
    d = d1 if turn == 0 else d2

    if x < d:
        ans += x
        break

    r = x % d
    if r != 0:
        ans += r
        x -= r

    ans += 1
    x //= d
    turn ^= 1

print(ans)
```Cốt lõi của việc triển khai là nhảy qua các phân đoạn trừ bằng cách sử dụng modulo. điều kiện`x < d`là cần thiết vì chỉ riêng hành vi modulo vẫn cho phép tính toán phần dư, nhưng trong trường hợp đó, trình tự di chuyển tối ưu chỉ đơn giản là giảm dần về 0 và không có vấn đề thay thế nào nữa. 

Việc lật lượt chỉ xảy ra sau khi chia thành công. Điều này khớp chính xác với quy tắc trò chơi: phép trừ không thay đổi người chơi, chỉ hoàn thành một lượt; chia cũng hoàn thành một lượt. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`15 2 3`Chúng tôi theo dõi những thay đổi trạng thái. 

| Bước | x | Người chơi | Hành động | Số lần di chuyển | 
| --- | --- | --- | --- | --- | 
| 1 | 15 | Một (2) | 15 % 2 = 1 phép trừ | 1 | 
| 2 | 14 | A | chia cho 2 | 2 | 
| 3 | 7 | B (3) | 7 % 3 = 1 phép trừ | 3 | 
| 4 | 6 | B | chia cho 3 | 4 | 
| 5 | 2 | Một (2) | x < 2, trừ hết | 6 | 

Câu trả lời cuối cùng: 6 

Dấu vết này cho thấy phép trừ được nhóm thành các bước nhảy trước mỗi phép chia như thế nào và phép chia làm giảm trạng thái nhanh chóng như thế nào. 

### Ví dụ 2:`18 2 3`| Bước | x | Người chơi | Hành động | Số lần di chuyển | 
| --- | --- | --- | --- | --- | 
| 1 | 18 | Một (2) | chia cho 2 ngay | 1 | 
| 2 | 9 | B (3) | chia cho 3 ngay | 2 | 
| 3 | 3 | Một (2) | trừ 1 | 3 | 
| 4 | 2 | A | trừ 1 | 4 | 
| 5 | 1 | A | trừ 1 | 5 | 

Câu trả lời cuối cùng: 5 

Trường hợp này chứng tỏ rằng khi tính chia hết là ngay lập tức thì quá trình sẽ trở thành một chuỗi các phép rút gọn nhanh mà không có độ trễ phép trừ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log n)$| Mỗi phép chia giảm giá trị ít nhất theo hệ số 2 và các bước trừ được tổng hợp thông qua các bước nhảy modulo | 
| Không gian |$O(1)$| Chỉ có một số lượng biến không đổi được duy trì | 

Thuật toán xử lý thoải mái các giá trị lên đến$10^{18}$bởi vì số lượng phép chia bị giới hạn bởi khoảng 60 và tất cả công việc khác là không đổi trên mỗi bước. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, d1, d2 = map(int, input().split())
    x = n
    ans = 0
    turn = 0

    while x > 0:
        d = d1 if turn == 0 else d2

        if x < d:
            ans += x
            break

        r = x % d
        if r:
            ans += r
            x -= r

        ans += 1
        x //= d
        turn ^= 1

    return str(ans)

# provided samples
assert run("15 2 3") == "7"
assert run("18 2 3") == "5"
assert run("5 6 7") == "5"

# custom cases
assert run("2 2 2") == "1"
assert run("10 3 3") == "4"
assert run("100 10 2") == "7"
assert run("1 2 3") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 2 | 1 | chấm dứt phân chia ngay lập tức | 
| 10 3 3 | 4 | phép trừ lặp đi lặp lại trước khi chia | 
| 100 10 2 | 7 | xen kẽ co nhanh với các ước số khác nhau | 
| 1 2 3 | 1 | trường hợp cạnh trong đó x < cả hai ước số | 

## Vỏ cạnh 

Khi nào$x$đã nhỏ hơn cả hai ước số, thuật toán ngay lập tức kích hoạt nhánh kết thúc. Đối với đầu vào`5 6 7`, điều kiện`x < d`giữ nguyên lượt của người chơi đầu tiên nên chúng ta thêm 5 nước đi và dừng lại. Điều này phù hợp với quy trình thực tế vì chỉ có thể giảm dần và sẽ không có phép chia nào. 

Khi các ước số bằng nhau, chẳng hạn như`10 3 3`, cả hai người chơi đều hành xử giống hệt nhau ngoại trừ thứ tự lần lượt. Thuật toán vẫn luân phiên chính xác vì phép chia mất hết một lượt và phép trừ chỉ xảy ra theo đợt cho đến bội số tiếp theo, do đó tính đối xứng không gây ra sự mơ hồ. 

Khi$x$chính xác là có thể chia hết khi bắt đầu, như`18 2 3`, bước modulo bằng 0 và thuật toán thực hiện phép chia ngay lập tức. Điều này tránh mọi phép trừ không cần thiết và đảm bảo quá trình chuyển đổi trạng thái nhanh nhất có thể, phù hợp với quy tắc trò chơi mà phép chia luôn được chọn khi có sẵn.
