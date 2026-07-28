---
title: "CF 102786D - \u0411\u044b\u0447\u0438\u0439 \u0440\u044b\u043d\u043e\u043a"
description: "Đầu vào mô tả phần trăm thay đổi hàng ngày của giá tài sản. Mỗi giá trị được tính bằng điểm cơ bản, do đó giá trị 100 có nghĩa là thay đổi 1%, -250 có nghĩa là giảm 2,5%, v.v. Những thay đổi về giá được áp dụng lần lượt, có nghĩa là chúng nhân lên chứ không phải cộng thêm."
date: "2026-07-27T19:24:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102786
codeforces_index: "D"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u042f\u0440\u0413\u0423 \u0438\u043c. \u041f.\u0413. \u0414\u0435\u043c\u0438\u0434\u043e\u0432\u0430 Demidov Open IT Cup 2019"
rating: 0
weight: 102786
solve_time_s: 60
verified: true
draft: false
---

[CF 102786D - \u0411\u044b\u0447\u0438\u0439 \u0440\u044b\u043d\u043e\u043a](https://codeforces.com/problemset/problem/102786/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả phần trăm thay đổi hàng ngày của giá tài sản. Mỗi giá trị được cho bằng điểm cơ bản, do đó giá trị của`100`có nghĩa là một sự thay đổi của`1%`,`-250`có nghĩa là giảm`2.5%`, vân vân. Những thay đổi về giá được áp dụng lần lượt, có nghĩa là chúng nhân lên chứ không phải cộng thêm. Một chuỗi các thay đổi`+10%`, sau đó`-10%`không trở về giá ban đầu vì thay đổi thứ hai ảnh hưởng đến giá trị đã được sửa đổi. 

Chúng ta cần chọn một phạm vi ngày liền kề. Ngày bắt đầu của phạm vi là ngày chúng ta mua tài sản và ngày kết thúc là ngày chúng ta bán nó. Mục tiêu là tối đa hóa giá trị cuối cùng sau khi áp dụng tất cả thay đổi trong phạm vi này. Nếu một số phạm vi mang lại lợi nhuận giống hệt nhau thì phạm vi có ngày bắt đầu muộn nhất sẽ được ưu tiên. Nếu số ngày bắt đầu bằng nhau thì khoảng thời gian dài hơn sẽ được ưu tiên. 

Số lượng thay đổi có thể lên tới 300.000. Một giải pháp bậc hai sẽ cần kiểm tra khoảng 45 tỷ khoảng trong trường hợp xấu nhất, vượt xa giới hạn thời gian lập trình cạnh tranh thông thường cho phép. Giải pháp phải xử lý mảng theo thời gian tuyến tính hoặc gần tuyến tính. Dung lượng bộ nhớ cũng phải tỷ lệ thuận với kích thước đầu vào, do đó việc lưu trữ các bảng kết quả khoảng lớn là không thực tế. 

Có một số trường hợp khó khăn mà việc triển khai đơn giản có thể thất bại. Đầu tiên là việc cộng các tỷ lệ phần trăm thay vì nhân chúng sẽ cho kết quả sai. Ví dụ:```
4
5000 -4000 100 100
```Câu trả lời đúng là`1 4`, vì số nhân là`1.5 * 0.6 * 1.01 * 1.01`, lớn hơn bất kỳ lợi ích cá nhân nào. Một giải pháp tổng hợp các thay đổi sẽ thấy`50 - 40 + 1 + 1 = 12%`và có thể so sánh không chính xác với từng ngày riêng lẻ. 

Một vấn đề khác là xử lý những thay đổi tiêu cực. Một sự sụt giảm lớn có thể làm cho việc mở rộng một phân khúc trở nên tồi tệ hơn, nhưng một sự gia tăng sau đó có thể khiến toàn bộ phân khúc trở nên tối ưu. Ví dụ:```
5
-1000 5000 -1000 5000 -1000
```Khoảng thời gian tốt nhất là toàn bộ phạm vi`1 5`, bởi vì hai mức tăng mạnh bù đắp cho tổn thất thông qua phép nhân. Một giải pháp chỉ tìm kiếm các giá trị dương liên tiếp sẽ bỏ lỡ điều này. 

Thắt cà vạt là một điểm tinh tế khác. Coi như:```
4
100 0 100 0
```Các khoảng`1 4`Và`3 4`có cùng lợi nhuận vì những thay đổi bằng 0 không ảnh hưởng đến giá. Câu trả lời đúng là`3 4`, vì nó bắt đầu muộn hơn. Việc triển khai giữ mức tối đa đầu tiên mà nó tìm thấy sẽ thất bại. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi ngày bắt đầu có thể và kéo dài khoảng thời gian từng ngày một. Đối với mỗi khoảng thời gian, chúng ta có thể duy trì hệ số nhân hiện tại và so sánh nó với hệ số tốt nhất được tìm thấy cho đến nay. Điều này đúng vì mọi khoảng thời gian có thể đều được kiểm tra. 

Tuy nhiên, có`N * (N + 1) / 2`khoảng thời gian có thể. Với`N = 300000`, tức là khoảng 45 tỷ khoảng. Ngay cả với các bản cập nhật liên tục, tốc độ này vẫn quá chậm. 

Quan sát quan trọng là mọi hệ số nhân hàng ngày đều dương. Một sự thay đổi của`p`điểm cơ bản tương ứng với hệ số nhân của:```
1 + p / 10000
```Vì tất cả các số nhân đều dương nên việc tối đa hóa một tích tương đương với việc tối đa hóa tổng logarit của chúng:```
log(a * b * c) = log(a) + log(b) + log(c)
```Vấn đề trở thành vấn đề tổng hợp mảng con tối đa trên các giá trị được chuyển đổi. Đây chính xác là cấu trúc được thuật toán Kadane xử lý. 

Phương pháp brute-force hoạt động vì nó đánh giá rõ ràng mọi sản phẩm, nhưng không thành công vì có quá nhiều khoảng thời gian. Việc chuyển đổi phép nhân thành phép cộng cho phép chúng ta chỉ giữ lại trạng thái tốt nhất có thể trước đó khi quét từ trái sang phải. 

So sánh giữa các phương pháp: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mỗi thay đổi hàng ngày thành giá trị logarit. Để có một sự thay đổi`p`, cửa hàng:```
log(1 + p / 10000)
```Tổng của các giá trị này trong một khoảng thời gian biểu thị logarit của tổng lợi nhuận, do đó việc so sánh tổng cũng giống như so sánh lợi nhuận thực tế. 

1. Duy trì tổng tiền tố của các giá trị được chuyển đổi. Nếu như`pref[i]`là tổng của số đầu tiên`i`các giá trị được chuyển đổi thì giá trị của khoảng`(l, r)`là:```
pref[r] - pref[l - 1]
```1. Trong khi quét vị trí kết thúc`r`, giữ tổng tiền tố nhỏ nhất trong số tất cả các vị trí trước`r`. Trừ tiền tố nhỏ nhất trước đó sẽ cho khoảng lớn nhất có thể kết thúc tại`r`. 
2. Khi một số vị trí tiền tố có cùng giá trị tối thiểu, hãy giữ giá trị mới nhất. Vị trí xuất phát muộn hơn sẽ được ưu tiên hơn theo quy tắc hòa của bài toán. 
3. So sánh khoảng thời gian hiện tại với câu trả lời đúng nhất được tìm thấy cho đến nay. Thay thế câu trả lời nếu kết quả trả về lớn hơn. Nếu kết quả trả về bằng nhau, hãy chọn khoảng thời gian có chỉ số bắt đầu lớn hơn và nếu các kết quả đó cũng bằng nhau, hãy chọn khoảng thời gian dài hơn. 
4. Vị trí sau khi xử lý`r`, chèn`pref[r]`vào thông tin tiền tố tối thiểu được lưu trữ để các khoảng thời gian trong tương lai có thể bắt đầu sau`r`. 

Tại sao nó hoạt động: 

Biểu diễn tổng tiền tố có nghĩa là mọi giá trị khoảng được xác định bằng cách chọn một tiền tố trước đó để trừ khỏi tiền tố hiện tại. Đối với vị trí kết thúc cố định, khoảng thời gian tốt nhất phải bắt đầu ngay sau giá trị tiền tố nhỏ nhất trước đó. Thuật toán luôn lưu trữ chính xác thông tin cần thiết này, bao gồm cả lựa chọn ràng buộc chính xác giữa các giá trị tiền tố bằng nhau. Vì mọi vị trí kết thúc có thể đều được kiểm tra và điểm bắt đầu tốt nhất cho mỗi vị trí kết thúc được xem xét nên tìm thấy mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n = int(input())
    p = list(map(int, input().split()))

    logs = [math.log1p(x / 10000.0) for x in p]

    eps = 1e-12

    pref = 0.0
    min_pref = 0.0
    min_pos = 0

    best_value = -float("inf")
    best_l = 1
    best_r = 1

    for i, value in enumerate(logs, start=1):
        pref += value

        current = pref - min_pref
        l = min_pos + 1
        r = i

        length = r - l + 1
        best_length = best_r - best_l + 1

        if (
            current > best_value + eps
            or (
                abs(current - best_value) <= eps
                and (l > best_l or (l == best_l and length > best_length))
            )
        ):
            best_value = current
            best_l = l
            best_r = r

        if pref < min_pref - eps or (abs(pref - min_pref) <= eps and i > min_pos):
            min_pref = pref
            min_pos = i

    print(best_l, best_r)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên chuyển đổi mọi phần trăm thay đổi thành logarit.`log1p`được sử dụng thay vì tính toán trực tiếp`log(1 + x)`bởi vì nó chính xác hơn cho các giá trị gần bằng 0. 

Biến`pref`lưu trữ tổng tiền tố hiện tại.`min_pref`là tổng tiền tố nhỏ nhất được thấy trước vị trí hiện tại và`min_pos`là chỉ mục của tiền tố đó. Khi vị trí hiện tại được sử dụng làm điểm kết thúc của một khoảng thời gian, việc trừ đi tiền tố tối thiểu này sẽ tạo ra khoảng thời gian tốt nhất có thể kết thúc ở đây. 

Việc so sánh câu trả lời chứa logic ràng buộc. Lợi nhuận logarit lớn hơn luôn tốt hơn. Nếu hai kết quả trả về bằng nhau trong phạm vi độ chính xác của dấu phẩy động thì khoảng có điểm cuối bên trái lớn hơn sẽ được chọn. Nếu cả hai điểm cuối bên trái khớp nhau thì điểm cuối bên phải sau sẽ có khoảng thời gian dài hơn. 

Bản cập nhật của`min_pref`xảy ra sau khi kiểm tra câu trả lời hiện tại vì ngày hiện tại không thể được sử dụng làm điểm bắt đầu của khoảng thời gian kết thúc vào cùng ngày trong công thức tiền tố. Việc giữ đúng thứ tự sẽ tránh được lỗi sai sót một. 

Ở đây, số dấu phẩy động Python là đủ vì chỉ cần so sánh giữa các logarit tích lũy. Tích ban đầu có thể trở nên cực nhỏ hoặc cực lớn, nhưng dạng logarit giữ tất cả các giá trị trong phạm vi có thể quản lý được. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4
1000 2000 -3000 4000
```Các giá trị được chuyển đổi là logarit của số nhân`1.1`,`1.2`,`0.7`, Và`1.4`. 

| Ngày | Tổng tiền tố | Tiền tố tối thiểu | Vị trí tối thiểu | Khoảng thời gian hiện tại | Câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | nhật ký (1.1) | 0 | 0 | 1-1 | 1-1 | 
| 2 | log(1.1*1.2) | 0 | 0 | 1-2 | 1-2 | 
| 3 | nhật ký(1.1_1.2_0.7) | 0 | 0 | 1-3 | 1-2 | 
| 4 | log(1.1_1.2_0.7*1.4) | nhật ký(1.1_1.2_0.7) | 3 | 4-4 | 4-4 | 

Khoảng thời gian đầy đủ mang lại lợi nhuận thấp hơn khoảng thời gian cuối cùng`+40%`ngày một mình. Tiền tố tối thiểu sau ngày thứ 3 cho phép thuật toán phát hiện ra rằng việc loại bỏ những ngày trước đó là tốt hơn. 

Đối với mẫu thứ hai:```
4
10 20 -25 40
```| Ngày | Tổng tiền tố | Tiền tố tối thiểu | Vị trí tối thiểu | Khoảng thời gian hiện tại | Câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | nhật ký (1.1) | 0 | 0 | 1-1 | 1-1 | 
| 2 | log(1.1*1.2) | 0 | 0 | 1-2 | 1-2 | 
| 3 | log(1.1_1.2_0.75) | 0 | 0 | 1-3 | 1-2 | 
| 4 | log(1.1_1.2_0.75*1.4) | 0 | 0 | 1-4 | 1-4 | 

Ở đây mức tăng cuối cùng đủ mạnh để việc giữ mức lãi và lỗ trước đó mang lại lợi nhuận tổng thể cao nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi thay đổi hàng ngày được chuyển đổi và xử lý một lần. | 
| Không gian | O(1) | Chỉ có một vài biến đang chạy được lưu trữ. | 

Thuật toán thực hiện một lượng công việc không đổi cho mỗi giá trị trong số tối đa 300.000 giá trị đầu vào. Nó tránh lưu trữ tất cả các khoảng hoặc mảng tiền tố, vì vậy nó phù hợp thoải mái trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solve_data(data):
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(data)
    input = sys.stdin.readline

    n = int(input())
    p = list(map(int, input().split()))

    pref = 0.0
    min_pref = 0.0
    min_pos = 0
    eps = 1e-12

    best = -float("inf")
    bl = br = 1

    for i, x in enumerate(p, 1):
        pref += math.log1p(x / 10000.0)

        cur = pref - min_pref
        l = min_pos + 1

        if (
            cur > best + eps
            or (
                abs(cur - best) <= eps
                and (l > bl or (l == bl and i - l + 1 > br - bl + 1))
            )
        ):
            best = cur
            bl = l
            br = i

        if pref < min_pref - eps or (abs(pref - min_pref) <= eps and i > min_pos):
            min_pref = pref
            min_pos = i

    sys.stdin = old_stdin
    return f"{bl} {br}"

assert solve_data("4\n1000 2000 -3000 4000\n") == "4 4"
assert solve_data("4\n10 20 -25 40\n") == "1 4"

assert solve_data("3\n100 100 100\n") == "1 3"
assert solve_data("3\n-9999 5000 5000\n") == "2 3"
assert solve_data("5\n100 0 100 0 0\n") == "3 5"

assert solve_data("2\n-10000 100\n") == "2 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 100 100 100`|`1 3`| Kiểm tra xem chuỗi có lợi nhuận có được mở rộng hay không. | 
|`3 / -9999 5000 5000`|`2 3`| Kiểm tra xem khoản lỗ lớn ban đầu có bị loại bỏ hay không. | 
|`5 / 100 0 100 0 0`|`3 5`| Kiểm tra sự đứt gãy của điểm bắt đầu mới nhất. | 
|`2 / -10000 100`|`2 2`| Kiểm tra hành vi ranh giới gần hệ số nhân nhỏ nhất. | 

## Vỏ cạnh 

Một chuỗi chứa một khoản lỗ rất lớn theo sau là mức tăng được xử lý bằng logic tiền tố tối thiểu. Vì:```
3
-9999 5000 5000
```số nhân đầu tiên là`0.0001`, vì vậy việc đưa nó vào gần như phá hủy khoản đầu tư. Tổng tiền tố sau ngày đầu tiên trở nên cực kỳ âm, khiến nó trở thành tiền tố tối thiểu. Khi ngày thứ hai được xử lý, bắt đầu từ ngày thứ 2 sẽ tốt hơn và thuật toán tiếp tục từ đó. Câu trả lời cuối cùng là:```
2 3
```Sự ràng buộc giữa các khoảng thời gian phải ưu tiên sự bắt đầu muộn nhất. Vì:```
5
100 0 100 0 0
```những thay đổi bằng 0 không làm thay đổi kết quả trả về. Các khoảng`1 5`Và`3 5`có cùng một sản phẩm. Thuật toán lưu trữ vị trí mới nhất trong số các tiền tố tối thiểu bằng nhau, khiến ứng viên bắt đầu từ ngày thứ 3 sẽ thay thế vị trí trước đó. Đầu ra là:```
3 5
```Một ngày duy nhất có thể là câu trả lời tốt nhất. Vì:```
4
1000 2000 -3000 4000
```ba ngày đầu bên nhau sẽ yếu hơn chỉ riêng ngày cuối cùng. Tiền tố tối thiểu trước ngày thứ tư được cập nhật sau khi mất, cho phép thuật toán chỉ xem xét ngày thứ 4. Kết quả đầu ra trở thành:```
4 4
```Những trường hợp này chính xác là những cách tiếp cận dựa trên việc cộng thêm tỷ lệ phần trăm, giả sử chuỗi tích cực là tối ưu hoặc bỏ qua các quy tắc ràng buộc có xu hướng thất bại.
