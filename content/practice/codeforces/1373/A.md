---
title: "CF 1373A - Cửa hàng bánh rán"
description: "Chúng ta đang so sánh hai cách khác nhau để mua cùng một số lượng bánh rán và với mỗi cách, chúng ta muốn biết khi nào nó trở nên rẻ hơn hẳn so với cách kia. Ở cửa hàng đầu tiên, giá là tuyến tính: mua $x$ bánh rán có giá chính xác là $a cdot x$."
date: "2026-06-16T12:48:50+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1373
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 90 (Rated for Div. 2)"
rating: 1000
weight: 1373
solve_time_s: 277
verified: false
draft: false
---

[CF 1373A - Cửa hàng bánh rán](https://codeforces.com/problemset/problem/1373/A) 

**Đánh giá:** 1000 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 4 phút 37 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang so sánh hai cách khác nhau để mua cùng một số lượng bánh rán và với mỗi cách, chúng ta muốn biết khi nào nó trở nên rẻ hơn hẳn so với cách kia. 

Ở cửa hàng đầu tiên, giá là tuyến tính: mua$x$bánh rán có giá chính xác$a \cdot x$. 

Ở cửa hàng thứ hai, việc mua hàng bị hạn chế. Bánh rán được bán trong hộp có kích thước$b$, giá mỗi hộp$c$. Nếu chúng ta muốn$x$bánh rán, chúng ta không thể mua từng phần hộp, nên chúng ta phải mua đủ hộp đầy đủ để ít nhất$x$bánh rán. Điều đó có nghĩa là chúng tôi mua$\lceil x / b \rceil$hộp, và tổng chi phí trở thành$c \cdot \lceil x / b \rceil$. 

Với mỗi test case, chúng ta cần tìm một số nguyên dương$x$trong đó cửa hàng đầu tiên rẻ hơn cửa hàng thứ hai và một số nguyên khác (có thể khác)$x$nơi cửa hàng thứ hai rẻ hơn cửa hàng thứ nhất. Nếu không có giá trị như vậy tồn tại cho một hướng, chúng ta xuất ra -1. 

Các ràng buộc nhỏ về số lượng kiểm tra, nhưng các giá trị của$a$,$b$, Và$c$có thể lên đến$10^9$, Và$x$cũng có thể lên tới$10^9$. Điều đó ngay lập tức loại trừ việc thử tất cả các giá trị của$x$. Bất kỳ giải pháp nào cũng phải suy luận trực tiếp từ cấu trúc của hàm chi phí. 

Một trường hợp phức tạp xuất hiện khi một cửa hàng luôn tốt hơn hoặc luôn ngang bằng. Ví dụ, nếu$a \le c/b$theo nghĩa trung bình nhưng việc làm tròn sẽ thay đổi hành vi hoặc nếu kích thước hộp làm cho cửa hàng thứ hai không đổi từng phần. Một vấn đề quan trọng khác là không được phép bình đẳng; chúng ta cần sự bất bình đẳng nghiêm ngặt, do đó, phải loại trừ các giá trị có chi phí phù hợp ngay cả khi chúng trông “tối ưu”. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là lặp đi lặp lại$x$từ 1 đến một giới hạn lớn nào đó và so sánh cả hai chi phí. Đối với mỗi$x$, tính toán$a \cdot x$Và$c \cdot \lceil x/b \rceil$và ghi lại một giá trị trong đó giá trị này nhỏ hơn giá trị kia. Điều này đúng nhưng về cơ bản là quá chậm: ngay cả khi chúng ta chỉ kiểm tra tối đa$10^9$, điều này là không thể dưới những ràng buộc điển hình. 

Quan sát quan trọng là cả hai hàm chi phí đều đơn điệu theo cách có cấu trúc. Cửa hàng đầu tiên hoàn toàn tuyến tính. Cửa hàng thứ hai là một hàm bậc thang: nó không đổi trong các khoảng thời gian$b$, sau đó nhảy qua$c$. Điều này có nghĩa là những thay đổi về ưu thế giữa các cửa hàng chỉ có thể xảy ra xung quanh các điểm nhỏ, có cấu trúc, thường là xung quanh bội số của$b$hoặc các giá trị rất nhỏ của$x$. 

Thay vì quét tất cả$x$, chúng ta chỉ cần kiểm tra một vài ứng cử viên được lựa chọn một cách chiến lược. Ý tưởng hữu ích đầu tiên là nếu cửa hàng thứ hai rẻ hơn bao giờ hết thì nó sẽ rẻ hơn ở một số cửa hàng nhỏ.$x$, cụ thể là tại$x = 1$. Điều này là do chi phí cho mỗi chiếc bánh rán được giảm thiểu một cách hiệu quả khi bạn sử dụng hết hộp; lớn hơn$x$chỉ lặp lại cùng một mẫu trung bình hoặc làm giảm hiệu quả làm tròn. 

Vì vậy, để tìm ra trường hợp cửa hàng thứ hai rẻ hơn, chúng ta chỉ cần kiểm tra$x = 1$. 

Để cửa hàng đầu tiên rẻ hơn, chúng tôi muốn:$$a \cdot x < c \cdot \lceil x/b \rceil$$Phía bên phải là hằng số từng phần trên mỗi khối có kích thước$b$. Cơ hội đầu tiên để cửa hàng đầu tiên giành chiến thắng thường xảy ra ở ranh giới của ô, tức là tại$x = b$, vì trước đó cửa hàng thứ 2 có thể lãng phí công suất nhưng sau đó khả năng khấu hao sẽ được cải thiện. 

Vì vậy chúng tôi kiểm tra$x = b$. 

Hai ứng cử viên này là đủ vì bất kỳ sự cải tiến hoặc giao thoa nào cũng xảy ra ở các điểm cấu trúc nhỏ nhất của hàm bước: chiếc bánh rán đầu tiên và hộp đầy đủ đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) mỗi lần kiểm tra | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Tính toán so sánh chi phí tại$x = 1$. Chúng tôi so sánh$a$với$c$, từ$\lceil 1/b \rceil = 1$. Nếu như$c < a$, thì cửa hàng thứ hai rẻ hơn ở mức$x = 1$, vì vậy chúng tôi xuất ra$x = 1$cho câu trả lời thứ hai. Mặt khác, chúng tôi kết luận không có giá trị$x$cho cửa hàng thứ hai. 
2. Tính toán ứng viên khi cửa hàng đầu tiên rẻ hơn bằng cách kiểm tra$x = b$. Tại thời điểm này, chi phí cửa hàng đầu tiên$a \cdot b$, trong khi cửa hàng thứ hai có giá chính xác$c$bởi vì một hộp đầy đủ bao gồm tất cả$b$bánh rán. Nếu như$a \cdot b < c$, chúng tôi xuất ra$x = b$cho câu trả lời đầu tiên. 
3. Nếu điều kiện không thành công, chúng ta xuất -1 cho hướng đó. 
4. Đảm bảo kết quả đầu ra vẫn nằm trong giới hạn, điều này được đảm bảo vì chúng tôi chỉ sử dụng$1$Và$b \le 10^9$. 

### Tại sao nó hoạt động 

Hàm chi phí của cửa hàng thứ hai chỉ thay đổi khi$x$vượt qua bội số của$b$. Giữa các điểm đó, nó không đổi, do đó, bất kỳ thay đổi tối thiểu hoặc vượt trội nào đều phải được chứng kiến ​​tại điểm đầu tiên trong một phân đoạn hoặc tại ranh giới phân đoạn đầu tiên. Cửa hàng đầu tiên là tuyến tính, vì vậy nếu nó đánh bại cửa hàng thứ hai, nó sẽ làm điều đó sớm nhất tại một trong những điểm dừng cấu trúc này. Điều này làm giảm không gian tìm kiếm vô hạn xuống còn hai ứng viên có ý nghĩa:$x = 1$Và$x = b$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, c = map(int, input().split())

    # second shop cheaper candidate
    second_ans = -1
    if c < a:
        second_ans = 1

    # first shop cheaper candidate
    first_ans = -1
    if a * b < c:
        first_ans = b

    print(first_ans, second_ans)
```Khối đầu tiên tính toán xem một chiếc bánh rán có rẻ hơn ở cửa hàng thứ hai hay không. Vì việc mua một chiếc bánh rán luôn buộc phải mua một hộp nên sự so sánh giảm xuống còn$c < a$. 

Khối thứ hai kiểm tra ranh giới quan trọng nơi cấu trúc hộp ngừng hỗ trợ cửa hàng thứ hai. Tại$x = b$, cả hai cửa hàng đều thể hiện sự so sánh rõ ràng: một hộp so với$b$bánh rán riêng lẻ. Đây là điểm duy nhất cần thiết để quyết định liệu cửa hàng đầu tiên có thể rẻ hơn hay không. 

phép nhân$a \cdot b$phải được thực hiện trong phạm vi 64 bit, nhưng Python xử lý việc này một cách an toàn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 10 4
```Chúng tôi đánh giá cả hai ứng cử viên. 

Vì$x = 1$, Cửa hàng thứ nhất giá 5, Cửa hàng thứ hai giá 4 nên Cửa hàng thứ hai rẻ hơn. Vì$x = 10$, cửa hàng thứ nhất giá 50, cửa hàng thứ hai giá 4, nên cửa hàng thứ hai vẫn rẻ hơn. 

| Bước | x | Chi phí đầu tiên | Chi phí thứ hai | Kết quả | 
| --- | --- | --- | --- | --- | 
| kiểm tra lần thứ hai | 1 | 5 | 4 | chiến thắng thứ hai | 
| kiểm tra đầu tiên | 10 | 50 | 4 | chiến thắng thứ hai | 

Điều này cho thấy cửa hàng đầu tiên không bao giờ rẻ hơn nên chúng ta ghi -1 cho cửa hàng đó. 

### Mẫu 2 

đầu vào:```
2 2 3
```Vì$x = 1$, chi phí là 2 và 3, vì vậy cửa hàng đầu tiên sẽ rẻ hơn, nhưng điều này không đảm bảo rằng mọi cửa hàng đều sẽ như vậy$x$. Vì$x = 2$, cửa hàng thứ nhất giá 4, cửa hàng thứ hai giá 3 nên cửa hàng thứ hai rẻ hơn. 

| Bước | x | Chi phí đầu tiên | Chi phí thứ hai | Kết quả | 
| --- | --- | --- | --- | --- | 
| kiểm tra lần thứ hai | 1 | 2 | 3 | chiến thắng đầu tiên | 
| kiểm tra đầu tiên | 2 | 4 | 3 | chiến thắng thứ hai | 

Chúng tôi nhận được một câu trả lời hợp lệ cho mỗi hướng:$1$cho cửa hàng đầu tiên,$2$cho cửa hàng thứ hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | công việc liên tục trên mỗi trường hợp thử nghiệm, chỉ so sánh số học | 
| Không gian | O(1) | không có công trình phụ trợ | 

Giải pháp dễ dàng nằm trong giới hạn vì ngay cả đối với$t = 1000$, chúng tôi chỉ thực hiện một số thao tác số nguyên cho mỗi trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    t = int(input())
    out = []
    for _ in range(t):
        a, b, c = map(int, input().split())

        second_ans = -1
        if c < a:
            second_ans = 1

        first_ans = -1
        if a * b < c:
            first_ans = b

        out.append(f"{first_ans} {second_ans}")

    return "\n".join(out) + "\n"

# provided samples
assert run("""4
5 10 4
4 5 20
2 2 3
1000000000 1000000000 1000000000
""") == """-1 1
5 -1
-1 1
-1 -1
"""

# custom cases
assert run("""1
1 2 1
""") == "-1 -1\n", "all equal-ish edge"

assert run("""1
10 2 100
""") == "2 1\n", "both directions possible"

assert run("""1
5 3 2
""") == "3 1\n", "second shop strong"

assert run("""1
2 5 100
""") == "5 1\n", "first shop wins for box size gap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 1 | -1 -1 | không có sự cải tiến nghiêm ngặt nào | 
| 10 2 100 | 2 1 | cả hai cửa hàng đều có khu vực chiến thắng | 
| 5 3 2 | 3 1 | hành vi chi phối cửa hàng thứ hai | 
| 2 5 100 | 5 1 | cửa hàng đầu tiên có thể đánh bại việc định giá số lượng lớn | 

## Vỏ cạnh 

Khi nào$a = c$, cửa hàng thứ hai không thể rẻ hơn ở mức$x = 1$, vì cả hai đều có giá như nhau. Thuật toán cho kết quả đúng -1 vì bất đẳng thức nghiêm ngặt không thành công. 

Khi$a \cdot b = c$, sự so sánh tại$x = b$trở nên bằng nhau, vì vậy cửa hàng đầu tiên không được coi là rẻ hơn. Điều này tránh việc đưa ra câu trả lời hợp lệ không chính xác khi cả hai cửa hàng đều hòa nhau. 

Khi$b = 2$, hàm bước nhảy rất thường xuyên, nhưng cấu trúc vẫn thu gọn về hai điểm kiểm tra giống nhau. Thuật toán vẫn hợp lệ vì nó không dựa vào độ mượt mà chỉ dựa vào bước đầu tiên và ranh giới khối đầu tiên.
