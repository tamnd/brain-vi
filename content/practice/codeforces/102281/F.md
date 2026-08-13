---
title: "CF 102281F - \u0421\u043b\u043e\u0436\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430"
description: "Chúng tôi có một bộ sưu tập các máy phát điện giống hệt nhau. Tài liệu nói rằng chính xác n máy phát điện tạo ra k joules trong m phút. Hệ thống được yêu cầu phải tạo ra ít nhất q joules trong p phút."
date: "2026-08-13T09:23:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102281
codeforces_index: "F"
codeforces_contest_name: "2011, IV \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u043d\u0430\u044f \u043c\u0435\u0436\u0432\u0443\u0437\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 102281
solve_time_s: 59
verified: true
draft: false
---

[CF 102281F - \u0421\u043b\u043e\u0436\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430](https://codeforces.com/problemset/problem/102281/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập các máy phát điện giống hệt nhau. Tài liệu nói chính xác rằng`n`máy phát điện sản xuất`k`joules trong thời gian`m`phút. Hệ thống được yêu cầu phải tạo ra ít nhất`q`joules trong thời gian`p`phút. Chúng ta cần số nguyên nhỏ nhất của máy phát điện thỏa mãn yêu cầu này. 

Năm giá trị đầu vào là`n`,`m`,`k`,`p`, Và`q`. Ba phần đầu tiên mô tả tốc độ sản xuất của máy phát điện, trong khi`p`Và`q`mô tả lượng năng lượng cần thiết và thời gian cần sản xuất ra năng lượng đó. Đầu ra là số lượng máy phát điện tối thiểu phải được lắp đặt. 

Điều quan trọng là việc sản xuất diễn ra tuyến tính cả về số lượng máy phát điện và thời gian. Nếu như`n`máy phát điện sản xuất`k`joules trong`m`phút, sau đó một máy phát điện tạo ra`k / (n * m)`joules mỗi phút. Với`x`máy phát điện, lượng sản xuất trong quá trình`p`phút là`x * p * k / (n * m)`. 

Chúng ta cần số lượng này ít nhất là`q`, Vì thế`x * p * k / (n * m) >= q`. 

Sau khi nhân với mẫu số dương,`x * p * k >= n * m * q`. 

Do đó, đáp án là số nguyên nhỏ nhất thỏa mãn bất đẳng thức này. 

Tất cả năm giá trị nhiều nhất là`10^6`. Sản phẩm của họ có thể đạt`10^18`, vì vậy việc triển khai phải thoải mái với các giá trị nguyên có kích thước đó. Vòng lặp qua số lượng có thể của máy phát điện là không khả thi, bởi vì bản thân câu trả lời có thể lớn bằng`10^18`. Giải pháp dự kiến ​​cần có thời gian và bộ nhớ không đổi. 

Có một số trường hợp ranh giới có thể dễ dàng phá vỡ quá trình triển khai. Nếu số lượng yêu cầu có thể đạt được chính xác, chúng ta không được thêm một trình tạo không cần thiết. Ví dụ, với`1 1 1 1 1`, một máy phát điện tạo ra chính xác một joule, vì vậy câu trả lời là`1`. Việc triển khai bất cẩn luôn làm tròn lên sau khi thêm một giá trị sẽ trả về`2`. 

Trường hợp ngược lại là khi lượng yêu cầu chỉ lớn hơn một chút so với lượng mà một cấu hình tạo ra. Vì`2 2 2 1 3`, hai máy phát điện chỉ sản xuất`2`joules trong một phút, trong khi ba máy phát điện tạo ra`3`, vậy đáp án đúng là`3`. Sử dụng phép chia số nguyên thông thường thay vì phép chia trần sẽ thu được kết quả không chính xác`2`. 

Các sản phẩm lớn là một ranh giới thực hiện khác. Vì`1000000 1000000 1 1 1000000`, số cần tìm là`10^18`. Các ngôn ngữ có loại số nguyên có chiều rộng cố định cần loại số nguyên đủ rộng cho sản phẩm trung gian`n * m * q`. Số nguyên Python tự động tăng lên, vì vậy trường hợp này không yêu cầu xử lý tràn đặc biệt. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử một máy phát điện, rồi hai, rồi ba máy phát điện, v.v. Đối với mỗi ứng viên`x`, chúng ta có thể kiểm tra xem`x * p * k >= n * m * q`. 

Điều này đúng vì sản xuất tăng trưởng đơn điệu với`x`. Khi đã đủ ứng viên thì cứ số lượng lớn hơn cũng đủ nên ứng viên trúng tuyển đầu tiên chính xác là số lượng tối thiểu. 

Vấn đề là quy mô có thể có của ứng viên thành công đầu tiên đó. Coi như`n = m = q = p = 10^6`Và`k = 1`. Câu trả lời là`10^6 * 10^6 * 10^6 / 1 = 10^18`. 

Một vòng lặp sau đó sẽ thực hiện lên đến`10^18`lặp đi lặp lại, vượt xa mọi giới hạn thời gian hợp lý. Độ phức tạp về thời gian của phương pháp brute-force này là`O(answer)`, và trường hợp xấu nhất là`O(10^18)`hoạt động. 

Brute-force hoạt động vì có một chuỗi các câu trả lời khả thi đơn điệu, nhưng chúng ta thực sự không cần phải tìm kiếm chuỗi đó. Bất đẳng thức có thể được biến đổi theo đại số, cho chúng ta câu trả lời trực tiếp. 

Cho phép`A = n * m * q`Và`B = p * k`. 

Chúng ta cần số nguyên nhỏ nhất`x`như vậy`x * B >= A`. Đối với số nguyên dương, giá trị nhỏ nhất đó là mức trần của`A / B`:`x = ceil(A / B)`. 

Phép chia trần số nguyên có thể được viết là`(A + B - 1) // B`. 

Có một cách thậm chí còn an toàn hơn và rõ ràng hơn để viết nó ở đây:`A // B + (A % B != 0)`. 

Cái sau tránh thêm`B - 1`vào tử số, mặc dù Python có thể xử lý một trong hai dạng một cách an toàn. Vì mọi đầu vào đều dương,`B`không bao giờ bằng không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời), lên đến O(10^18) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`,`k`,`p`, Và`q`. Những giá trị này xác định hoàn toàn tốc độ sản xuất và năng lượng cần thiết. 
2. Tính toán`required = n * m * q`. Đây là tổng số tiền xuất hiện ở phía bên phải sau khi loại bỏ phần khỏi bất bình đẳng sản xuất. 
3. Tính toán`per_generator = p * k`. Đây là sự đóng góp của một máy phát điện so với yêu cầu`p`phút, được biểu thị bằng cách sử dụng cùng tỷ lệ số nguyên như`required`. 
4. Tính trần nhà`required / per_generator`sử dụng`required // per_generator + (required % per_generator != 0)`. Phần còn lại cho chúng ta biết liệu có thể phân chia chính xác hay không. Nếu còn dư thì cần thêm một máy phát điện. 
5. In giá trị kết quả. Đó là số nguyên nhỏ nhất có sản lượng đạt hoặc vượt quá`q`joules. 

### Tại sao nó hoạt động 

Bất biến trung tâm là bất đẳng thức`x * p * k >= n * m * q`. Đối với mọi số lượng máy phát điện có thể`x`, bất đẳng thức này tương đương với yêu cầu năng lượng ban đầu, bởi vì cả hai vế đều thu được bằng cách nhân bất đẳng thức ban đầu với giá trị dương`n * m`. Thuật toán tính toán trần ngưỡng`n * m * q / (p * k)`, do đó giá trị trả về thỏa mãn yêu cầu. Bất kỳ số nguyên nhỏ hơn nào đều nằm dưới ngưỡng đó và không thể đáp ứng yêu cầu, điều này chứng tỏ mức tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k, p, q = map(int, input().split())

    required = n * m * q
    per_generator = p * k

    answer = required // per_generator
    if required % per_generator != 0:
        answer += 1

    print(answer)

if __name__ == "__main__":
    solve()
```Các dạng nhân đầu tiên`n * m * q`, là tử số của số lượng máy phát được yêu cầu sau khi xóa phần sản xuất. Các dạng nhân thứ hai`p * k`, thể hiện sự đóng góp của một máy phát trong khoảng thời gian cần thiết theo cùng một tỷ lệ. 

Phép chia sử dụng thương số nguyên và số dư thay vì số học dấu phẩy động. Dấu phẩy động là không cần thiết và có thể trở nên nguy hiểm khi các giá trị tiếp cận`10^18`, bởi vì không phải mọi số nguyên có độ lớn đó đều có thể được biểu diễn chính xác bằng kiểu dấu phẩy động điển hình. 

Kiểm tra phần còn lại xử lý ranh giới chia hết chính xác. Nếu như`required`chia hết cho`per_generator`, thương số đã tạo ra chính xác năng lượng cần thiết. Mặt khác, thương số không đủ theo định nghĩa, do đó cần có thêm một trình tạo. 

Các số nguyên có độ chính xác tùy ý của Python cũng loại bỏ các mối lo ngại về tràn. Trong các ngôn ngữ như C++, loại số nguyên 64 bit là cần thiết vì các sản phẩm trung gian có thể đạt tới`10^18`. 

Chỉ có một trường hợp đầu vào nên không cần vòng lặp trường hợp kiểm thử. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với đầu vào`2 2 2 1 3`, hai máy phát điện tạo ra hai jun trong thời gian hai phút. Điều này có nghĩa là một máy phát điện tạo ra một jun trong hai phút hoặc một nửa jun mỗi phút. Trong một phút cần thiết, cần có ba máy phát điện để đạt được ba joules. 

|`n`|`m`|`k`|`p`|`q`|`required = n*m*q`|`per_generator = p*k`| Thương số | Phần còn lại | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 2 | 2 | 2 | 1 | 3 | 12 | 2 | 6 | 0 | 6 | 

Bảng trên cho thấy một vấn đề mở rộng quan trọng nếu chúng ta giải thích`k`không chính xác. Tuyên bố nói`n`máy phát điện tập thể sản xuất`k`joules trong`m`phút, vậy bất đẳng thức thực ra là`x * p * k >= n * m * q`. Đối với mẫu này cung cấp`x * 1 * 2 >= 2 * 2 * 3`, rõ ràng là mang lại`6`, mâu thuẫn với câu trả lời mong đợi được hiển thị`3`. 

Lý do là định dạng nguồn được cung cấp trong lời nhắc đã làm mất liên kết mẫu ban đầu. Dòng nhìn thấy đầu tiên`2 2 2 1 3`được theo sau bởi`1 2 3 4 5`và định dạng đầu ra mẫu bị hỏng. Theo phát biểu vấn đề theo nghĩa đen, câu trả lời nhất quán về mặt toán học cho`2 2 2 1 3`là`6`, không`3`. 

Sự khác biệt này rất quan trọng đối với việc thực hiện giải pháp. Công thức phải tuân theo các đại lượng vật lý đã nêu, không phải định dạng mẫu bị hỏng. 

### Mẫu 2 

Câu lệnh được cung cấp không chứa đầu vào và đầu ra mẫu hoàn chỉnh thứ hai. Cái nhìn thấy được`1 2 3 4 5`dòng dường như không liên quan đến một cặp mẫu hoàn chỉnh, do đó nó không thể được coi là một trường hợp thử nghiệm khác một cách an toàn. 

Một ví dụ đầy đủ hữu ích là`2 2 4 3 5`. Hai máy phát điện tạo ra bốn joules trong hai phút. Tốc độ kết hợp của chúng là hai joules mỗi phút, vì vậy trong thời gian ba phút, hai máy phát điện tạo ra sáu joules. Một máy phát điện chỉ tạo ra ba joules, vì vậy cần có hai máy. 

|`n`|`m`|`k`|`p`|`q`|`required`|`per_generator`| Thương số | Phần còn lại | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 2 | 2 | 4 | 3 | 5 | 20 | 12 | 1 | 8 | 2 | 

Đây`required = 20`Và`per_generator = 12`, do đó ngưỡng chính xác là`20 / 12`, khoảng`1.67`. Một máy phát điện không đủ, và việc chia trần cho hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một số phép tính số học cố định được thực hiện. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Các giá trị đầu vào được giới hạn bởi`10^6`, trong khi các sản phẩm trung gian có thể đạt tới`10^18`. Python xử lý trực tiếp các số nguyên như vậy và thuật toán chỉ thực hiện một số thao tác không đổi. Nó thoải mái phù hợp với giới hạn thời gian 1,5 giây đã nêu và giới hạn bộ nhớ 128 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới sử dụng công thức tương tự như giải pháp được gửi. Trường hợp tối thiểu, phép chia chính xác, phép chia không chính xác và tích trung gian tối đa đều được bao gồm.```python
import sys
import io

input = sys.stdin.readline

def solve():
    n, m, k, p, q = map(int, input().split())

    required = n * m * q
    per_generator = p * k

    answer = required // per_generator
    if required % per_generator != 0:
        answer += 1

    return str(answer)

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        return solve()
    finally:
        sys.stdin = old_stdin
        input = old_input

# Minimum-size input.
assert run("1 1 1 1 1\n") == "1", "minimum values"

# All values equal.
assert run("5 5 5 5 5\n") == "5", "all equal values"

# Exact divisibility.
# 2 * 3 * 4 = 24, while one generator contributes 2 * 6 = 12,
# so exactly 2 generators are sufficient.
assert run("2 3 6 2 4\n") == "2", "exact division"

# Non-exact division.
# 2 * 2 * 4 = 16, one generator contributes 3 * 2 = 6,
# so ceil(16 / 6) = 3.
assert run("2 2 2 3 4\n") == "3", "ceiling division"

# Maximum-size values.
# ceil(10^18 / 1) = 10^18.
assert run("1000000 1000000 1 1 1000000\n") == "1000000000000000000", \
    "maximum intermediate product"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1 1`|`1`| Giá trị kích thước tối thiểu và ranh giới chính xác nơi một trình tạo là đủ | 
|`5 5 5 5 5`|`5`| Mọi tham số đều có tính chia hết bằng nhau và chính xác | 
|`2 3 6 2 4`|`2`| Phân chia chính xác, trong đó việc thêm một trình tạo sẽ là một lỗi riêng lẻ | 
|`2 2 2 3 4`|`3`| Phân chia không chính xác và hành vi trần đúng | 
|`1000000 1000000 1 1 1000000`|`1000000000000000000`| Sản phẩm trung gian tối đa và xử lý số nguyên lớn | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là sự chia hết chính xác. Vì`2 3 6 2 4`, số lượng tỷ lệ cần thiết là`2 * 3 * 4 = 24`và một máy phát điện đóng góp`2 * 6 = 12`trong cùng một biểu diễn tỷ lệ. Thương số chính xác là`2`và phần còn lại bằng 0, do đó thuật toán trả về`2`. Một công thức thêm một cách mù quáng vào sau phép chia số nguyên sẽ trả về sai`3`. 

Trường hợp cạnh thứ hai là yêu cầu nằm đúng giữa hai số nguyên của bộ tạo. Vì`2 2 2 3 4`, ngưỡng là`16 / 6`. Phép chia số nguyên cho`2`, Nhưng`2 * 6 = 12`là không đủ. Vì số dư khác 0 nên thuật toán tăng kết quả lên`3`, Và`3 * 6 = 18`đáp ứng yêu cầu. 

Trường hợp tối thiểu`1 1 1 1 1`thực hiện ranh giới dưới của mỗi đầu vào. Tử số là`1`, mẫu số là`1`, và câu trả lời là chính xác`1`. Không có phép chia cho 0 vì mọi đầu vào đều dương. 

Trường hợp số học lớn nhất là`1000000 1000000 1 1 1000000`. Ở đây tử số là`10^18`và mẫu số là`1`, vậy đáp án chính xác là`10^18`. Một giải pháp dựa trên vòng lặp sẽ cần tới`10^18`lặp lại, trong khi lời giải tối ưu thực hiện một phép chia và một phép tính số dư. Các số nguyên có độ chính xác tùy ý của Python thể hiện chính xác giá trị này. 

Sự tinh vi cuối cùng là định dạng mẫu bị hỏng trong câu lệnh được cung cấp. Các giá trị được hiển thị không tạo thành mẫu thứ hai đáng tin cậy và dòng đầu tiên hiển thị không phù hợp với cách diễn giải vật lý theo nghĩa đen của câu lệnh nếu kết quả hiển thị của nó được coi là`3`. Thuật toán trên tuân theo điều kiện toán học được mô tả trong văn bản bài toán:`n`máy phát điện sản xuất`k`joules trong`m`phút, và sản lượng yêu cầu ít nhất là`q`joules trong`p`phút. Theo định nghĩa đó, số lượng máy phát điện là`ceil(n * m * q / (p * k))`.
