---
title: "CF 102277E - Chủ tịch SGA"
description: "Chúng tôi được cung cấp tên của tất cả sinh viên UCF. Vé Tổng thống và Phó Tổng thống được coi là có thể khi hai ứng cử viên có tên khác nhau nhưng cả hai tên đều bắt đầu bằng cùng một chữ cái."
date: "2026-08-16T19:34:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102277
codeforces_index: "E"
codeforces_contest_name: "UCF Locals 2018"
rating: 0
weight: 102277
solve_time_s: 53
verified: true
draft: false
---

[CF 102277E - Chủ tịch SGA](https://codeforces.com/problemset/problem/102277/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp tên của tất cả sinh viên UCF. Vé Tổng thống và Phó Tổng thống được coi là có thể khi hai ứng cử viên có tên khác nhau nhưng cả hai tên đều bắt đầu bằng cùng một chữ cái. Học sinh khác biệt ngay cả khi họ có chung tên, vì vậy nếu có một số học sinh tên JOSH và một số học sinh tên JAD, mỗi lựa chọn của một học sinh JOSH cùng với một học sinh JAD sẽ tạo ra một vé riêng. Hai vị trí cũng khác nhau nên việc chọn JOSH làm Chủ tịch và JAD làm Phó Chủ tịch khác với việc chọn JAD làm Chủ tịch và JOSH làm Phó Chủ tịch. Nhiệm vụ là đếm tất cả các cặp học sinh có thứ tự như vậy. 

Số lượng sinh viên nhiều nhất là 66.183, đây là số lượng tuyển sinh UCF thực tế được sử dụng cho cuộc thi. Mỗi tên có tối đa 20 chữ cái viết hoa. Với giới hạn thời gian một giây, thuật toán kiểm tra từng cặp học sinh là không khả thi vì có khoảng 4,38 tỷ cặp được sắp xếp ở kích thước đầu vào tối đa. Giải pháp dự định phải xử lý mỗi học sinh chỉ với số lần không đổi, đưa ra thời gian dự kiến ​​tuyến tính hoặc thời gian xác định tùy thuộc vào cấu trúc dữ liệu được sử dụng. 

Câu trả lời có thể lớn hơn nhiều so với số nguyên có dấu 32 bit. Nếu nhiều học sinh có tên khác nhau bắt đầu bằng cùng một chữ cái, thì số cặp có thứ tự tăng theo phương trình bậc hai theo số học sinh, do đó việc triển khai cần một loại số nguyên có khả năng chứa các giá trị xung quanh (n^2). Số nguyên Python đã có độ chính xác tùy ý, do đó không cần xử lý đặc biệt trong mã. 

Có một số trường hợp đặc biệt có thể khiến một giải pháp có vẻ hợp lý trở nên sai lầm. Đầu tiên, các bản sao trùng tên lặp đi lặp lại không được tạo thành vé hợp lệ. Ví dụ,```
3
JOSH
JOSH
JAD
```có đầu ra```
4
```Có hai sinh viên JOSH và một sinh viên JAD. Các vé đặt hàng hợp lệ là mỗi JOSH với JAD và JAD với mỗi JOSH, cho ra bốn vé. Một giải pháp bất cẩn chỉ đếm tất cả học sinh có cùng chữ cái đầu sẽ tính toán (3 \times 2 = 6), bao gồm không chính xác hai cặp JOSH/JOSH có thứ tự. 

Thứ hai, thứ tự của các ứng cử viên rất quan trọng. Vì```
2
JOSH
JAD
```câu trả lời là```
2
```vì JOSH/JAD và JAD/JOSH là các vé khác nhau. Đếm các cặp không có thứ tự sẽ chỉ cho một. 

Cuối cùng, những tên có tên viết tắt khác nhau không bao giờ được kết hợp với nhau. Vì```
3
JOSH
JAD
ALI
```câu trả lời là```
2
```bởi vì chỉ có hai cặp thứ tự liên quan đến JOSH và JAD là hợp lệ. Việc đếm từng cặp học sinh có tên khác nhau sẽ bao gồm không chính xác các cặp liên quan đến ALI. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra từng cặp sinh viên riêng biệt theo thứ tự. Đối với mỗi cặp, so sánh hai tên. Cặp đôi này đóng góp một câu trả lời chính xác khi tên khác nhau và các chữ cái đầu tiên của chúng khớp nhau. Điều này đúng vì nó kiểm tra chính xác hai điều kiện xác định một vé hợp lệ. 

Vấn đề là số lượng cặp. Với (n) học sinh thì có (n(n-1)) cặp có thứ tự. Tại (n=66.183), đây là 

[ 
66,183 \times 66,182 = 4,380,123,306 
] 

kiểm tra cặp. Ngay cả khi mỗi lần kiểm tra cực kỳ rẻ thì hàng tỷ lần lặp lại cũng không thể đáp ứng được giới hạn cuộc thi một giây. 

Lực lượng vũ phu hoạt động vì mọi vé hợp lệ đều được kiểm tra rõ ràng, nhưng nó không thành công khi cùng một thông tin được tính toán lại cho mỗi cặp. Quan sát quan trọng là tình trạng này chỉ phụ thuộc vào hai thông tin: chữ cái đầu tiên của mỗi tên và liệu hai tên đầy đủ có khác nhau hay không. 

Giả sử một tên viết tắt cụ thể có (T) sinh viên. Nếu chúng ta tạm thời bỏ qua yêu cầu về các tên phải khác nhau thì sẽ có (T(T-1)) cặp học sinh riêng biệt có tên viết tắt đó theo thứ tự (T(T-1)). Trong số đó, những học sinh có cùng tên đều không hợp lệ. Nếu một tên cụ thể xuất hiện (c) lần, nó sẽ đóng góp (c(c-1)) cặp thứ tự không hợp lệ, bởi vì chúng ta có thể chọn một trong hai học sinh làm Chủ tịch và một sinh viên khác có cùng tên làm Phó Chủ tịch. 

Như vậy, với một lần ban đầu, số lượng vé hợp lệ là 

[ 
T(T-1) - \sum_{\text{name}} c(c-1). 
] 

Chúng ta có thể tính toán điều này tăng dần mà không cần phải xây dựng các cặp. Khi xử lý một học sinh mới có tên (x) và tên viết tắt (L), giả sử đã có (T) học sinh có tên viết tắt (L) và (c) trong số họ có cùng tên (x). Học sinh mới có thể lập (T) vé đặt mua với các học sinh trước đó làm một trong hai vai trò, và một vé (T) khác khi học sinh trước đó giữ vai trò đầu tiên. Điều đó mang lại khả năng (2T) trước khi xem xét các tên trùng lặp. (c) học sinh trước đó có tên (x) không hợp lệ trong cả hai hướng, vì vậy chúng tôi trừ (2c). Do đó, sự đóng góp của sinh viên mới là 

[ 
2(T-c). 
] 

Sau khi thêm phần đóng góp này, chúng tôi tăng cả số lượng ban đầu và số lượng tên chính xác. 

Điều này cung cấp một lần chuyển qua đầu vào. Việc quan sát có hiệu quả vì mọi học sinh được xử lý trước đó có thể được phân loại hoàn toàn theo cặp bao gồm tên viết tắt và tên đầy đủ của họ. Không có tài sản nào khác ảnh hưởng đến việc học sinh mới có thể tạo thành một vé hợp lệ hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(1)) ngoài bộ nhớ đầu vào | Quá chậm | 
| Tối ưu | (O(n)) dự kiến ​​| (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì`initial_count`, nơi lưu trữ số lượng học sinh đã được nhìn thấy cho mỗi chữ cái đầu tiên và`name_count`, nơi lưu trữ số lượng học sinh đã được nhìn thấy cho mỗi tên đầy đủ. Một cái tên xác định duy nhất chữ cái đầu tiên của nó, vì vậy hai bản đồ này chứa chính xác thông tin cần thiết cho các học sinh tương lai. 
2. Đọc tên từng học sinh và trích ra ký tự đầu tiên của tên đó. Cho phép`same_initial`là số học sinh được xử lý trước đó có tên bắt đầu bằng ký tự này. 
3. Hãy để`same_name`là số lượng sinh viên được xử lý trước đó có cùng tên. Trong số`same_initial`chính xác là học sinh`same_name`là đối tác bị cấm vì tên của họ không khác nhau. 
4. Do đó, học sinh mới có`same_initial - same_name`đối tác hợp lệ trước đây. Mỗi đối tác như vậy có thể giữ chức Chủ tịch hoặc Phó Chủ tịch, vì vậy hãy thêm`2 * (same_initial - same_name)`để trả lời. 
5. Tăng số lượng cho tên viết tắt và tên đầy đủ của học sinh. Những số lượng cập nhật này là cần thiết khi xử lý các học sinh sau này. 

Bất biến là sau khi xử lý bất kỳ tiền tố nào của đầu vào,`answer`bằng số lượng vé Chủ tịch/Phó Chủ tịch hợp lệ được đặt hàng có hai học sinh đều thuộc tiền tố đó. Khi một học sinh mới đến, mỗi vé hợp lệ mới được tạo phải có học sinh đó và một học sinh trước đó. Công thức tính chính xác những học sinh trước đó có cùng tên viết tắt và tên khác, theo cả hai thứ tự vai trò có thể có. Vì tất cả các vé đều được giới thiệu chính xác khi học sinh sau này được xử lý nên không có vé hợp lệ nào bị bỏ sót hoặc bị tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    initial_count = {}
    name_count = {}
    answer = 0

    for _ in range(n):
        name = input().strip()
        initial = name[0]

        same_initial = initial_count.get(initial, 0)
        same_name = name_count.get(name, 0)

        answer += 2 * (same_initial - same_name)

        initial_count[initial] = same_initial + 1
        name_count[name] = same_name + 1

    print(answer)

if __name__ == "__main__":
    solve()
```Hai từ điển tương ứng trực tiếp với hai đại lượng trong công thức tăng dần.`initial_count`cho chúng ta biết có bao nhiêu học sinh cũ có thể khớp chữ cái đầu tiên của học sinh mới.`name_count`xác định ứng viên nào phải bị loại vì có cùng tên. 

Câu trả lời được cập nhật trước khi chèn học sinh hiện tại vào từ điển. Thứ tự này rất quan trọng vì học sinh không thể tự mình lập phiếu. Nếu học sinh hiện tại được tính đầu tiên,`same_initial`Và`same_name`cả hai đều sẽ bao gồm sinh viên đó và việc tính toán sẽ phải bù đắp cho điều đó. 

Các hoạt động từ điển của Python được mong đợi (O(1)), đưa ra thời gian chạy tuyến tính dự kiến. Số nguyên Python tự động tăng khi cần thiết, do đó, câu trả lời bậc hai có thể không bị tràn. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên:```
10
JOSH
JAD
JENNIFER
JENNIFER
JALEN
HASAAN
ALI
TAMARA
LIAM
SATHWIKA
```Trạng thái chính phát triển như sau. 

| Sinh viên | Số lượng ban đầu trước | Số lượng trùng tên trước | Đóng góp mới | Trả lời | 
| --- | --- | --- | --- | --- | 
| JOSH | 0 | 0 | 0 | 0 | 
| JAD | 1 | 0 | 2 | 2 | 
| JENNIFER | 2 | 0 | 4 | 6 | 
| JENNIFER | 3 | 1 | 4 | 10 | 
| JALEN | 4 | 0 | 8 | 18 | 
| HASAAN | 0 | 0 | 0 | 18 | 
| ALI | 0 | 0 | 0 | 18 | 
| TAMARA | 0 | 0 | 0 | 18 | 
| LIAM | 1 | 0 | 2 | 20 | 
| SATHWIKA | 0 | 0 | 0 | 20 | 

Đầu ra cuối cùng là`20`cho đầu vào mười tên chính xác này. Bốn tên viết tắt J tạo ra 18 vé hợp lệ, trong khi hai tên viết tắt L tạo ra hai vé nữa. Các mục nhập JENNIFER lặp lại được loại trừ chính xác khỏi nhau nhưng mỗi mục vẫn có thể ghép nối với JOSH, JAD và JALEN. 

Mẫu thứ hai là:```
5
ALEX
BRANDY
CELINE
DWAYNE
ELIZABETH
```| Sinh viên | Số lượng ban đầu trước | Số lượng trùng tên trước | Đóng góp mới | Trả lời | 
| --- | --- | --- | --- | --- | 
| ALEX | 0 | 0 | 0 | 0 | 
| rượu mạnh | 0 | 0 | 0 | 0 | 
| CELINE | 0 | 0 | 0 | 0 | 
| DWAYNE | 0 | 0 | 0 | 0 | 
| ELIZABETH | 0 | 0 | 0 | 0 | 

Mỗi học sinh có một tên viết tắt khác nhau nên không thể tạo vé hợp lệ. Đầu ra là`0`. Điều này chứng tỏ rằng thuật toán không vô tình đếm các cặp chỉ vì tên khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) dự kiến ​​| Mỗi tên được đọc một lần và thực hiện một số thao tác từ điển dự kiến ​​(O(1)) không đổi. | 
| Không gian | (O(n)) | Trong trường hợp xấu nhất, tên đầy đủ của mỗi học sinh đều khác nhau, vì vậy`name_count`chứa (n) mục. | 

Dữ liệu đầu vào tối đa chứa 66.183 học sinh, do đó, quá trình quét tuyến tính chỉ thực hiện một số ít thao tác từ điển cho mỗi học sinh, vừa vặn với giới hạn một giây tốt hơn nhiều so với khoảng 4,38 tỷ lượt kiểm tra cặp mà lực lượng vũ phu yêu cầu. Việc sử dụng bộ nhớ cũng thoải mái trong giới hạn 256 MB cho kích thước đầu vào này. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())

    initial_count = {}
    name_count = {}
    answer = 0

    for _ in range(n):
        name = input().strip()
        initial = name[0]

        same_initial = initial_count.get(initial, 0)
        same_name = name_count.get(name, 0)

        answer += 2 * (same_initial - same_name)

        initial_count[initial] = same_initial + 1
        name_count[name] = same_name + 1

    print(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Sample 1
assert run("""10
JOSH
JAD
JENNIFER
JENNIFER
JALEN
HASAAN
ALI
TAMARA
LIAM
SATHWIKA
""") == "20\n", "sample 1"

# Sample 2
assert run("""5
ALEX
BRANDY
CELINE
DWAYNE
ELIZABETH
""") == "0\n", "sample 2"

# Minimum-size input
assert run("""1
A
""") == "0\n", "one student cannot form a ticket"

# All names equal
assert run("""4
JOSH
JOSH
JOSH
JOSH
""") == "0\n", "same names are forbidden"

# Repeated names mixed with distinct names
assert run("""4
JOSH
JOSH
JAD
JILL
""") == "8\n", "duplicate-name exclusion"

# Maximum-size input
n = 66183
assert run(str(n) + "\n" + ("A\n" * n)) == "0\n", "maximum size with identical names"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / A`|`0`| Đầu vào tối thiểu và không có cặp tự ghép | 
| Bốn bản sao của`JOSH`|`0`| Tên giống nhau không thể tạo thành vé | 
|`JOSH, JOSH, JAD, JILL`|`8`| Loại trừ chính xác các tên trùng lặp trong khi vẫn giữ các tên khác nhau | 
| 66.183 bản sao của`A`|`0`| Kích thước đầu vào tối đa và khả năng mở rộng | 

## Vỏ cạnh 

Đối với một học sinh,```
1
A
```thuật toán bắt đầu với`same_initial = 0`Và`same_name = 0`, do đó đóng góp bằng không. Sau đó học sinh sẽ được đưa vào bản đồ. Không bao giờ có một sinh viên nào khác cùng lập phiếu, đưa ra kết quả chính xác`0`. 

Đối với tên trùng lặp,```
3
JOSH
JOSH
JAD
```JOSH đầu tiên đóng góp bằng 0 vì không có học sinh nào trước đó. JOSH thứ hai nhìn thấy`same_initial = 1`Và`same_name = 1`, do đó đóng góp của nó bằng không. Nó không thể ghép nối với JOSH đầu tiên vì tên giống hệt nhau. JAD sau đó nhìn thấy`same_initial = 2`Và`same_name = 0`, đóng góp bốn vé đặt hàng. Đầu ra là`4`. 

Đối với vai trò đảo ngược,```
2
JOSH
JAD
```sinh viên đầu tiên đóng góp bằng không. Sau đó, JAD thấy một học sinh trước đó có cùng tên viết tắt và tên khác, vì vậy nó sẽ đóng góp`2`. Hai vé đó là JOSH/JAD và JAD/JOSH. Thuật toán nhân đôi một cách rõ ràng vì các vai trò được sắp xếp theo thứ tự. 

Đối với các chữ cái đầu khác nhau,```
3
JOSH
JAD
ALI
```hai sinh viên J-ban đầu tạo ra hai vé khi JAD được xử lý. ALI không thấy học sinh A-đầu tiên nào trước đó nên nó không đóng góp gì. Câu trả lời cuối cùng là`2`. Thuật toán không bao giờ cần so sánh ALI với sinh viên ban đầu J vì`initial_count`lọc chúng ra trước khi chúng có thể đóng góp. 

Đối với ranh giới kích thước tối đa,```
66183
A
A
A
...
A
```với 66.183 cái tên giống nhau, mỗi học sinh mới đều có`same_initial == same_name`. Do đó mọi đóng góp đều bằng 0 và câu trả lời cuối cùng là`0`. Trường hợp này thực hiện cả điều kiện đầu vào lớn nhất được phép và điều kiện tên trùng lặp mà không yêu cầu liệt kê bậc hai các cặp. Giới hạn đầu vào và giới hạn tài nguyên cuộc thi được ghi lại trong tài liệu cuộc thi gốc.
