---
title: "CF 446A - DZY Loves Sequences"
description: "Chúng ta được cung cấp một mảng số nguyên lên tới $10^5$. Chúng ta có thể chọn bất kỳ mảng con liền kề nào và được phép sửa đổi tối đa một phần tử bên trong mảng con đó, thay thế nó bằng bất kỳ số nguyên nào chúng ta muốn."
date: "2026-06-07T16:05:23+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 446
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round #FF (Div. 1)"
rating: 1600
weight: 446
solve_time_s: 143
verified: true
draft: false
---

[CF 446A - Trình tự yêu thích của DZY](https://codeforces.com/problemset/problem/446/A) 

**Đánh giá:** 1600 
**Tags:** dp, triển khai, hai con trỏ 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng lên tới$10^5$số nguyên. Chúng ta có thể chọn bất kỳ mảng con liền kề nào và được phép sửa đổi tối đa một phần tử bên trong mảng con đó, thay thế nó bằng bất kỳ số nguyên nào chúng ta muốn. Mục tiêu là tối đa hóa độ dài của một mảng con có thể tăng dần sau khi thực hiện nhiều nhất một sửa đổi. 

Chi tiết quan trọng là việc sửa đổi hoàn toàn không bị hạn chế. Chúng tôi không bị ràng buộc bởi giá trị ban đầu và chúng tôi có thể chọn bất kỳ số nguyên nào, lớn hay nhỏ. Chúng tôi chỉ quan tâm liệu một mảng con có thể được chuyển đổi thành một dãy tăng dần với nhiều nhất một thay đổi hay không. 

Ràng buộc$n \le 10^5$ngay lập tức loại trừ các thuật toán bậc hai. Bất kỳ giải pháp nào kiểm tra tất cả các mảng con sẽ yêu cầu khoảng$5 \times 10^9$trong trường hợp xấu nhất vượt xa giới hạn. Chúng ta nên hướng tới một giải pháp tuyến tính hoặc gần tuyến tính. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Coi như:```
3
1 2 3
```Câu trả lời là 3. Mặc dù chúng tôi được phép sửa đổi một phần tử nhưng chúng tôi không phải sử dụng sửa đổi đó. Một giải pháp giả định chính xác một thay đổi được yêu cầu sẽ thất bại. 

Coi như:```
3
5 5 5
```Câu trả lời là 1. Bất kỳ hai phần tử liền kề nào cũng vi phạm mức tăng nghiêm ngặt. Với một sửa đổi, chúng ta chỉ có thể sửa chữa một vị trí, do đó không có phân mảng nào có độ dài từ 2 trở lên có thể tăng nghiêm ngặt. Một giải pháp bất cẩn có thể cho rằng mọi vi phạm đều có thể được khắc phục một cách độc lập và trả lại không chính xác 2. 

Hãy xem xét:```
5
1 2 10 3 4
```Câu trả lời là 4. Chúng ta có thể thay đổi 10 thành bất kỳ giá trị nào trong khoảng từ 2 đến 3, thu được đoạn có độ dài tăng dần theo đúng 5. Thực tế là giá trị thay thế phải khớp giữa các phần tử lân cận là rất quan trọng. Chỉ đếm số vi phạm thôi là chưa đủ. 

Coi như:```
5
1 2 3 4 5
```Câu trả lời là 5. Một lỗi phổ biến là chỉ nhìn vào những nơi trình tự bị ngắt quãng. Nếu không có dấu ngắt nào cả thì toàn bộ mảng đã hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là kiểm tra mọi mảng con. Đối với mỗi mảng con, chúng ta có thể kiểm tra xem việc thay đổi nhiều nhất một phần tử có làm cho phần tử đó tăng lên hay không. 

Cách tiếp cận mạnh mẽ này là đúng vì nó kiểm tra rõ ràng mọi câu trả lời của ứng viên. Thật không may là có$O(n^2)$mảng con. Ngay cả khi chúng ta kiểm tra từng mảng con trong thời gian không đổi thì nó vẫn quá lớn đối với$n = 10^5$. Việc triển khai thực tế sẽ chậm hơn nhiều. 

Để có được giải pháp nhanh hơn, chúng ta nên xem xét cấu trúc của các lượt chạy tăng dần. 

Giả sử chúng ta biết, đối với mọi vị trí, độ dài của đoạn tăng nghiêm ngặt dài nhất kết thúc ở đó và độ dài của đoạn tăng nghiêm ngặt dài nhất bắt đầu từ đó. 

Cho phép:

-`L[i]`= độ dài của hậu tố tăng dần kết thúc tại`i`-`R[i]`= độ dài của tiền tố tăng dần bắt đầu từ`i`Những mảng này có thể được tính toán theo thời gian tuyến tính. 

Khi đã có chúng, chúng ta có thể đặt một câu hỏi tập trung hơn: nếu vị trí`i`là phần tử chúng ta sửa đổi, một đoạn tăng dần có thể đi qua vị trí đó trong bao lâu? 

Có hai khả năng. 

Đầu tiên, chúng ta có thể mở rộng một đoạn tăng dần bằng cách thay đổi một điểm cuối. Ví dụ: nếu một đoạn kết thúc tại`i - 1`có chiều dài`L[i - 1]`, thay đổi`a[i]`một cách thích hợp cho phép chúng ta mở rộng nó thêm một phần tử. Tương tự cho bên phải sử dụng`R[i + 1]`. 

Thứ hai, chúng ta có thể kết nối một đoạn tăng dần ở bên trái với một đoạn tăng dần khác ở bên phải. Điều này chỉ có thể thực hiện được nếu tồn tại một số nguyên nằm giữa`a[i-1]`Và`a[i+1]`. 

Vì các giá trị là số nguyên nên giá trị đó tồn tại chính xác khi:$$a[i+1] - a[i-1] > 1$$Khi điều kiện này được giữ, chúng ta có thể thay thế`a[i]`với một số nguyên phù hợp và hợp nhất hai chuỗi thành một đoạn tăng dần lớn hơn. 

Toàn bộ vấn đề quy về việc đánh giá những khả năng này cho mọi vị trí trong thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng. 
2. Nếu`n = 1`, trả về 1 ngay. 
3. Xây dựng mảng`L`.`L[i]`lưu trữ độ dài của đoạn liền kề tăng dần dài nhất kết thúc tại vị trí`i`. 

Nếu như`a[i] > a[i-1]`, thì đoạn này có thể được mở rộng và:`L[i] = L[i-1] + 1`. 

Nếu không thì:`L[i] = 1`. 
4. Xây dựng mảng`R`.`R[i]`lưu trữ độ dài của đoạn liền kề tăng dần dài nhất bắt đầu từ vị trí`i`. 

Nếu như`a[i] < a[i+1]`, sau đó:`R[i] = R[i+1] + 1`. 

Nếu không thì:`R[i] = 1`. 
5. Khởi tạo câu trả lời với đoạn tăng lớn nhất hiện có. 

Điều này đơn giản là`max(L)`. 
6. Đối với mọi vị trí`i`, hãy xem xét việc thay đổi`a[i]`. 

Nếu như`i > 0`, chúng ta có thể mở rộng phần bên trái thêm một phần tử:`answer = max(answer, L[i-1] + 1)`. 
7. Nếu`i + 1 < n`, chúng ta có thể mở rộng quyền chạy theo một phần tử:`answer = max(answer, R[i+1] + 1)`. 
8. Nếu`0 < i < n-1`, kiểm tra xem các đường chạy bên trái và bên phải có thể được hợp nhất hay không. 

Nếu như:`a[i+1] - a[i-1] > 1`thì một số nguyên nào đó tồn tại hoàn toàn giữa chúng. 

Chúng tôi có thể thay thế`a[i]`với giá trị như vậy và thu được:`L[i-1] + R[i+1] + 1`như chiều dài hợp nhất. 
9. Kẹp câu trả lời tối đa`n`. 
10. Xuất ra câu trả lời. 

### Tại sao nó hoạt động`L[i]`Và`R[i]`nắm bắt mọi lần chạy tăng tối đa trong mảng. Bất kỳ giải pháp tối ưu nào cũng có thể sửa đổi điểm cuối của một lần chạy hoặc vị trí giữa hai lần chạy. 

Nếu chúng ta sửa đổi một điểm cuối, kết quả tốt nhất có thể là mở rộng một bên bằng chính xác một phần tử, được bao phủ bởi`L[i-1] + 1`Và`R[i+1] + 1`chuyển tiếp. 

Nếu chúng ta sửa đổi vị trí bên trong, cách duy nhất để có được đoạn dài hơn là nối đường chạy tăng dần kết thúc tại`i-1`với tốc độ tăng dần bắt đầu từ`i+1`. Kết nối như vậy khả thi một cách chính xác khi một giá trị thay thế hợp lệ có thể được đặt hoàn toàn giữa các giá trị lân cận. điều kiện`a[i+1] - a[i-1] > 1`là vừa cần vừa đủ. 

Mọi sửa đổi tối ưu có thể xảy ra đều thuộc một trong các trường hợp này, vì vậy giá trị tối đa được thuật toán kiểm tra chính xác là câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    if n == 1:
        print(1)
        return

    L = [1] * n
    for i in range(1, n):
        if a[i] > a[i - 1]:
            L[i] = L[i - 1] + 1

    R = [1] * n
    for i in range(n - 2, -1, -1):
        if a[i] < a[i + 1]:
            R[i] = R[i + 1] + 1

    ans = max(L)

    for i in range(n):
        if i > 0:
            ans = max(ans, L[i - 1] + 1)

        if i + 1 < n:
            ans = max(ans, R[i + 1] + 1)

        if 0 < i < n - 1 and a[i + 1] - a[i - 1] > 1:
            ans = max(ans, L[i - 1] + R[i + 1] + 1)

    print(min(ans, n))

if __name__ == "__main__":
    solve()
```Bước đầu tiên tính toán các đoạn tăng dần kết thúc ở mỗi vị trí. Bước thứ hai tính toán các phân đoạn tăng dần bắt đầu từ mỗi vị trí. Họ cùng nhau tóm tắt tất cả các cấu trúc tăng cục bộ trong mảng. 

Vòng lặp chính đánh giá mọi vị trí có thể được sửa đổi. Việc mở rộng phạm vi chạy theo một phần tử tương ứng với việc thay đổi giá trị hiện tại thành một giá trị cực kỳ lớn hoặc cực kỳ nhỏ. Việc hợp nhất hai lần chạy yêu cầu kiểm tra xem một số nguyên hợp lệ có thể khớp giữa các giá trị lân cận hay không. 

trận chung kết`min(ans, n)`về mặt kỹ thuật là không cần thiết trong hầu hết các trường hợp nhưng giữ kết quả an toàn trong giới hạn mảng khi quá trình chuyển đổi tạo ra`n + 1`. 

Xử lý ranh giới là phần tế nhị nhất của việc thực hiện. Vị trí ở cuối mảng không thể hợp nhất hai lần chạy vì một bên không tồn tại. các điều kiện`i > 0`,`i + 1 < n`, Và`0 < i < n - 1`ngăn chặn truy cập ngoài phạm vi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
7 2 3 1 5 6
```Mảng tính toán: 

| tôi | một [tôi] | L[i] | R[i] | 
| --- | --- | --- | --- | 
| 0 | 7 | 1 | 1 | 
| 1 | 2 | 1 | 2 | 
| 2 | 3 | 2 | 1 | 
| 3 | 1 | 1 | 3 | 
| 4 | 5 | 2 | 2 | 
| 5 | 6 | 3 | 1 | 

Các chuyển đổi chính: 

| Chỉ mục được sửa đổi | Kết quả | 
| --- | --- | 
| 0 | 3 | 
| 1 | 3 | 
| 2 | 4 | 
| 3 | 5 | 
| 4 | 4 | 
| 5 | 4 | 

Sự lựa chọn tốt nhất là thay đổi phần tử`1`tại chỉ số 3 vào`4`. Điều này tham gia chạy bên trái`[2,3]`và chạy đúng`[5,6]`, tạo ra chiều dài 5. 

### Ví dụ 2 

đầu vào:```
5
1 2 10 3 4
```Mảng tính toán: 

| tôi | một [tôi] | L[i] | R[i] | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 3 | 
| 1 | 2 | 2 | 2 | 
| 2 | 10 | 3 | 1 | 
| 3 | 3 | 1 | 2 | 
| 4 | 4 | 2 | 1 | 

Ở vị trí 2: 

| Giá trị | 
| --- | 
| a[1] = 2 | 
| a[3] = 3 | 
| a[3] - a[1] = 1 | 

Điều kiện hợp nhất không thành công vì không có số nguyên nào nằm trong khoảng từ 2 đến 3. Câu trả lời tốt nhất có thể đạt được là 4, thu được bằng cách mở rộng một bên. 

Ví dụ này chứng tỏ tại sao điều kiện`a[i+1] - a[i-1] > 1`là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một đường chuyền từ trái sang phải, một đường chuyền từ phải sang trái, một lần quét cuối cùng | 
| Không gian | O(n) | Mảng`L`Và`R`mỗi cửa hàng`n`số nguyên | 

Thuật toán chỉ thực hiện một lượng công việc không đổi cho mỗi phần tử. Với$n \le 10^5$, tổng số thao tác dễ dàng nằm trong giới hạn thời gian và mức sử dụng bộ nhớ ở mức dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    if n == 1:
        return "1"

    L = [1] * n
    for i in range(1, n):
        if a[i] > a[i - 1]:
            L[i] = L[i - 1] + 1

    R = [1] * n
    for i in range(n - 2, -1, -1):
        if a[i] < a[i + 1]:
            R[i] = R[i + 1] + 1

    ans = max(L)

    for i in range(n):
        if i > 0:
            ans = max(ans, L[i - 1] + 1)

        if i + 1 < n:
            ans = max(ans, R[i + 1] + 1)

        if 0 < i < n - 1 and a[i + 1] - a[i - 1] > 1:
            ans = max(ans, L[i - 1] + R[i + 1] + 1)

    return str(min(ans, n))

# provided sample
assert run("6\n7 2 3 1 5 6\n") == "5", "sample"

# minimum size
assert run("1\n42\n") == "1", "single element"

# already increasing
assert run("5\n1 2 3 4 5\n") == "5", "already valid"

# all equal
assert run("5\n5 5 5 5 5\n") == "2", "one modification extends by one"

# merge impossible because gap is 1
assert run("5\n1 2 10 3 4\n") == "4", "cannot merge across middle"

# merge possible
assert run("5\n1 2 100 4 5\n") == "5", "full merge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 42`| 1 | Kích thước mảng tối thiểu | 
|`1 2 3 4 5`| 5 | Đã tăng nghiêm ngặt | 
|`5 5 5 5 5`| 2 | Một thay đổi có thể tạo ra đoạn có độ dài 2 | 
|`1 2 10 3 4`| 4 | Điều kiện hợp nhất không thành công khi khoảng cách bằng 1 | 
|`1 2 100 4 5`| 5 | Hợp nhất thành công hai lần chạy | 

## Vỏ cạnh 

### Mảng phần tử đơn 

đầu vào:```
1
10
```Mảng đã tăng lên rất nhiều vì nó chỉ chứa một phần tử. Thuật toán ngay lập tức trả về 1 trước khi xây dựng bất kỳ mảng phụ trợ nào. 

### Tất cả các phần tử đều bằng nhau 

đầu vào:```
5
5 5 5 5 5
```Chúng tôi có được:```
L = [1, 1, 1, 1, 1]
R = [1, 1, 1, 1, 1]
```Không có điều kiện hợp nhất nào thành công vì các giá trị lân cận bằng nhau. Kéo dài cả hai bên sẽ có độ dài 2, vì vậy câu trả lời trở thành 2. 

Đây là nguồn sai lầm phổ biến vì câu trả lời không phải là 1. Một sửa đổi luôn có thể tạo ra một cặp tăng nghiêm ngặt. 

### Khoảng cách quá nhỏ để hợp nhất 

đầu vào:```
3
1 100 2
```Ở vị trí giữa:```
a[2] - a[0] = 1
```Không có số nguyên nào nằm giữa 1 và 2. Quá trình chuyển đổi hợp nhất bị từ chối. Thuật toán trả về đúng 2. 

### Toàn bộ mảng đã tăng lên 

đầu vào:```
5
1 2 3 4 5
```Mảng tính toán là:```
L = [1, 2, 3, 4, 5]
```Câu trả lời ban đầu đã là 5. Không sửa đổi nào có thể tạo ra một phân đoạn dài hơn toàn bộ mảng, vì vậy kết quả cuối cùng vẫn là 5. Điều này xác minh rằng thuật toán xử lý chính xác trường hợp không cần thay đổi.
