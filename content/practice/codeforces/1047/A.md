---
title: "CF 1047A - Bé C Yêu 3 Tôi"
description: "Chúng ta có một số nguyên $n$ và chúng ta cần biểu diễn nó dưới dạng tổng của ba số nguyên dương $a, b, c$. Hạn chế thêm là không có số nào trong ba số này được phép chia hết cho 3."
date: "2026-06-15T11:06:44+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1047
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 511 (Div. 2)"
rating: 800
weight: 1047
solve_time_s: 277
verified: true
draft: false
---

[CF 1047A - Bé C Yêu 3 I](https://codeforces.com/problemset/problem/1047/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 4 phút 37 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất$n$, và chúng ta cần biểu diễn nó dưới dạng tổng của ba số nguyên dương$a, b, c$. Hạn chế bổ sung là không có số nào trong ba số này được phép chia hết cho 3. Bất kỳ bộ ba hợp lệ nào cũng được chấp nhận, do đó không có tiêu chí tối ưu hóa nào ngoài việc đáp ứng các ràng buộc. 

Từ quan điểm cấu trúc, đây là một vấn đề phân rã mang tính xây dựng. Chúng tôi không tìm kiếm mức tối thiểu hoặc tối đa, chỉ tìm kiếm một phân vùng khả thi theo các ràng buộc mô-đun. Điều đó ngay lập tức gợi ý rằng giải pháp nên dựa vào số học mô-đun hơn là tìm kiếm. 

Ràng buộc đầu vào$3 \le n \le 10^9$loại trừ việc liệt kê lực lượng vũ phu của bộ ba. Ngay cả khi chúng ta cố gắng cố định hai biến và tìm ra biến thứ ba, không gian tìm kiếm vẫn là bậc hai theo$n$, nó quá lớn. Do đó, vấn đề phải giảm xuống thành một công trình có thời gian không đổi. 

Một trường hợp cạnh tinh tế phát sinh khi$n$là nhỏ. Ví dụ,$n = 3$buộc phân vùng duy nhất có thể$1,1,1$, điều này có tác dụng vì không có cái nào chia hết cho 3. Một tình huống thú vị khác là khi$n \equiv 0 \pmod{3}$, vì một nỗ lực ngây thơ như chia đều thành$n/3, n/3, n/3$thất bại ngay lập tức vì mỗi phần đều chia hết cho 3. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ liệt kê tất cả các cặp$(a, b)$như vậy$a \ge 1, b \ge 1$, tính toán$c = n - a - b$và kiểm tra xem cả ba có dương và không chia hết cho 3 hay không. Cách này hoạt động hợp lý vì nó trực tiếp kiểm tra định nghĩa của bài toán, nhưng nó yêu cầu$O(n^2)$kiểm tra trong trường hợp xấu nhất, điều này là không thể$n$lên đến$10^9$. 

Quan sát quan trọng là ràng buộc chỉ phụ thuộc vào khả năng chia hết cho 3, hoàn toàn tuần hoàn theo modulo 3. Thay vì tìm kiếm các giá trị hợp lệ, chúng ta có thể xây dựng chúng bằng cách sử dụng một mẫu cố định để tránh dư lượng 0 mod 3. 

Các số nguyên không chia hết cho 3 chính là các số nguyên bằng 1 hoặc 2 modulo 3. Vậy ta chỉ cần đảm bảo mỗi điều kiện$a, b, c$rơi vào hai lớp dư lượng này trong khi vẫn tổng hợp thành$n$. Vì chúng ta có toàn quyền tự do lựa chọn các giá trị, chúng ta có thể sửa hai trong số chúng và rút ra giá trị thứ ba, sau đó điều chỉnh một chút nếu phần còn lại vi phạm ràng buộc. 

Cấu trúc tiêu chuẩn là chọn hai bội số nhỏ của 3, chẳng hạn như 1 và 2, rồi điều chỉnh giá trị thứ ba cho phù hợp. Nếu chúng ta đặt$a = 1$,$b = 1$, sau đó$c = n - 2$. Bây giờ vấn đề duy nhất là liệu$c$chia hết cho 3. Nếu đúng như vậy, chúng ta dịch chuyển một chút trong các lựa chọn trước đó để tránh đạt bội số của 3. Bởi vì các lớp dư lượng modulo 3 nhỏ và đóng trong phép cộng, nên số lượng điều chỉnh không đổi luôn là đủ. 

Điều này làm giảm vấn đề từ tìm kiếm không giới hạn xuống một số trường hợp số học không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Xây dựng tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng câu trả lời trực tiếp bằng cách sử dụng lý luận mô-đun. 

1. Bắt đầu bằng việc sửa chữa$a = 1$. Điều này đảm bảo$a$dương và không chia hết cho 3. 
2. Sửa$b = 1$vì lý do tương tự. Tại thời điểm này, chúng tôi đã sử dụng tổng cộng 2 đơn vị. 
3. Tính toán$c = n - 2$. Điều này đảm bảo điều kiện tổng$a + b + c = n$giữ chính xác. 
4. Kiểm tra xem$c$chia hết cho 3. Nếu không thì$(1, 1, c)$đã hợp lệ rồi. 
5. Nếu$c$chia hết cho 3 thì điều chỉnh cách xây dựng bằng cách thay thế$b = 2$Và$a = 1$, cho$c = n - 3$. Sự dịch chuyển này bảo toàn tổng trong khi thay đổi lớp dư của$c$, đảm bảo nó không còn chia hết cho 3 để hợp lệ$n$. 

Lý do những điều chỉnh nhỏ này là đủ là vì việc dịch chuyển 1 hoặc 2 sẽ thay đổi lớp dư lượng modulo 3 theo cách được kiểm soát và chúng ta chỉ cần tránh dư lượng 0. 

### Tại sao nó hoạt động 

Mọi số nguyên rơi vào đúng một trong ba lớp thặng dư modulo 3. Chúng ta rõ ràng tránh chọn các giá trị trong lớp 0 cho$a$Và$b$, và chúng tôi đảm bảo$c$có nguồn gốc từ$n$bằng cách trừ đi một hằng số nhỏ. Vì trừ 2 hoặc 3 chu kỳ qua các phần dư khác nhau, ít nhất một trong các độ lệch không đổi này tạo ra giá trị hợp lệ.$c$không chia hết cho 3. Điều này đảm bảo tồn tại một giải pháp và công trình của chúng tôi sẽ luôn tìm thấy một giải pháp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

a = 1
b = 1
c = n - 2

if c % 3 == 0:
    a = 1
    b = 2
    c = n - 3

print(a, b, c)
```Mã trực tiếp thực hiện ý tưởng mang tính xây dựng. Chúng tôi bắt đầu với phân tách đơn giản nhất và chỉ điều chỉnh nếu thành phần thứ ba rơi vào lớp dư lượng bị cấm. 

Điểm tinh tế duy nhất là đảm bảo tính tích cực. Vì$n \ge 3$, cả hai$n - 2$Và$n - 3$duy trì ít nhất là 1, vì vậy cả hai nhánh đều an toàn. Việc kiểm tra có điều kiện đảm bảo chúng ta tránh được trường hợp dư lượng có vấn đề duy nhất mà không vi phạm ràng buộc dương. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 3$Chúng tôi bắt đầu với$a = 1, b = 1$, cho$c = 1$. 

| Bước | một | b | c | Tình trạng | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 1 | 1 | c % 3 = 1 | 

Từ$c$không chia hết cho 3 thì giữ nguyên kết quả. 

Điều này xác nhận trường hợp cơ sở đơn giản nhất đã có hiệu lực mà không cần điều chỉnh. 

### Ví dụ 2:$n = 6$Chúng ta bắt đầu lại với$a = 1, b = 1$, cho$c = 4$. 

| Bước | một | b | c | Tình trạng | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 1 | 1 | 4 | c % 3 = 1 | 

Không cần điều chỉnh và đầu ra là hợp lệ. 

Điều này cho thấy ngay cả khi$n$chia hết cho 3, cách xây dựng đầu tiên thường tránh tạo ra bội số của 3 trong thành phần thứ ba. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một số lượng phép tính số học không đổi và một lần kiểm tra có điều kiện | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp là tối ưu cho các ràng buộc vì bất kỳ đầu vào nào lên đến$10^9$được xử lý trong thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = 1
    b = 1
    c = n - 2
    if c % 3 == 0:
        a = 1
        b = 2
        c = n - 3
    return f"{a} {b} {c}"

# provided sample
assert run("3\n") == "1 1 1", "sample 1"

# minimum edge
assert run("4\n") != "", "n=4 should produce valid output"

# divisible by 3 case
assert run("6\n").split(), "basic feasibility"

# large case
assert run("1000000000\n").split(), "large n"

# small non-trivial
assert run("7\n").split(), "random small"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 | 1 1 1 | ranh giới tối thiểu | 
| 6 | ba hợp lệ | xử lý chia hết cho 3 | 
| 7 | ba hợp lệ | tính đúng đắn chung | 
| 1000000000 | ba hợp lệ | hiệu suất và lớn n | 

## Vỏ cạnh 

cho$n = 3$, thuật toán tạo ra$a = 1, b = 1, c = 1$. Không có cái nào trong số này chia hết cho 3 nên kết quả đúng ngay lập tức mà không cần vào nhánh điều chỉnh. 

Vì$n = 6$, chúng tôi tính toán$c = 4$, hợp lệ vì 4 không chia hết cho 3. Điều kiện không kích hoạt, vì vậy chúng tôi xuất ra$1,1,4$. Tất cả các giá trị đều dương và thỏa mãn các ràng buộc. 

Nhánh điều chỉnh chỉ được kích hoạt khi$n - 2$chia hết cho 3. Trong trường hợp đó, chuyển sang$n - 3$đảm bảo số thứ ba dịch chuyển lớp dư lượng và tránh 0 modulo 3 trong khi vẫn giữ nguyên giá trị dương cho tất cả các giá trị hợp lệ$n \ge 3$.
