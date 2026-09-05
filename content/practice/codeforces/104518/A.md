---
title: "CF 104518A - Tổng tỷ lệ cược"
description: "Chúng ta được cho một số nguyên $n$, và chúng ta được yêu cầu tính tổng của các số lẻ $n$ đầu tiên. Nói cách khác, về mặt khái niệm, chúng tôi xây dựng một chuỗi bắt đầu từ 1, mỗi lần tăng thêm 2 và dừng sau $n$ số hạng. Nhiệm vụ là trả về tổng của chuỗi đó."
date: "2026-06-30T10:36:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104518
codeforces_index: "A"
codeforces_contest_name: "UNICAMP Selection Contest 2023"
rating: 0
weight: 104518
solve_time_s: 51
verified: true
draft: false
---

[CF 104518A - Tổng tỷ lệ cược](https://codeforces.com/problemset/problem/104518/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất$n$, và chúng ta được yêu cầu tính tổng của số đầu tiên$n$số lẻ. Nói cách khác, về mặt khái niệm, chúng tôi xây dựng một chuỗi bắt đầu từ 1, tăng thêm 2 mỗi lần và dừng sau$n$điều khoản. Nhiệm vụ là trả về tổng của chuỗi đó. 

Vì vậy, nếu chúng ta tưởng tượng việc tạo ra các số một cách rõ ràng, chúng ta sẽ tạo thành một danh sách như 1, 3, 5, 7, v.v.$n$-thuật ngữ thứ , tức là$2n - 1$, rồi tổng hợp mọi thứ. 

Giới hạn kích thước đầu vào$n \le 10^7$đủ nhỏ để về nguyên tắc chúng ta có thể tạo ra các con số, nhưng đủ lớn để chúng ta phải cẩn thận về hiệu quả và quan trọng hơn là về sự tăng trưởng số nguyên. Vòng lặp tính tổng trực tiếp có độ dài mười triệu là ranh giới trong các ngôn ngữ được dịch nếu được thực hiện nhiều lần hoặc bên trong nhiều trường hợp thử nghiệm nhưng vẫn khả thi một lần. Tuy nhiên, áp lực hạn chế thực sự không phải là độ phức tạp về thời gian mà là việc nhận ra cấu trúc toán học để chúng tôi tránh hoàn toàn những công việc không cần thiết. 

Việc triển khai đơn giản cũng có thể thất bại ở các ngôn ngữ có số nguyên có chiều rộng cố định. Tổng tăng lên như$n^2$, vì vậy đối với$n = 10^7$, kết quả là khoảng$10^{14}$, vượt quá giới hạn 32 bit nhưng vừa vặn thoải mái với số nguyên 64 bit hoặc Python. 

Một trường hợp khó phát hiện nếu ai đó giả định sai các công thức cấp số cộng hoặc quên rằng chuỗi bắt đầu từ 1 chứ không phải 0. Ví dụ: sử dụng không chính xác$n \cdot (2n) / 2$thay vì công thức đúng sẽ dẫn đến kết quả đầu ra luôn quá lớn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực xây dựng từng số lẻ từng số một và tích lũy tổng. Chúng ta bắt đầu từ 1 và liên tục cộng 2 cho đến khi đạt được$n$-học kỳ thứ Điều này đơn giản và đảm bảo tính chính xác vì nó phản ánh trực tiếp định nghĩa của chuỗi. 

Tuy nhiên, điều này đòi hỏi$n$các lần lặp và mỗi lần lặp thực hiện công việc không đổi. Vì$n = 10^7$, đây là khoảng mười triệu lần bổ sung, vẫn ở mức giới hạn nhưng không cần thiết. Quan trọng hơn, nó ẩn cấu trúc toán học của chuỗi. 

Quan sát quan trọng là dãy số lẻ tạo thành một cấp số cộng. Thay vì tính tổng từng số hạng, chúng ta có thể nhận ra một đồng nhất thức cổ điển: tổng của số hạng đầu tiên$n$số lẻ bằng$n^2$. Điều này có thể được suy ra từ việc ghép các đối số hoặc bằng cách sử dụng công thức chuỗi số học:$$\frac{n}{2} \cdot (2 \cdot 1 + (n-1)\cdot 2)$$mà đơn giản hóa trực tiếp để$n^2$. 

Điều này biến đổi vấn đề từ tính toán thời gian tuyến tính thành số học thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$|$O(1)$| Quá chậm đối với kích thước lớn$n$| 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$từ đầu vào. Điều này thể hiện có bao nhiêu số lẻ chúng ta đang tính tổng. 
2. Nhận biết rằng$i$-số lẻ thứ 2 là$2i - 1$, vậy tổng số tiền là$\sum_{i=1}^{n} (2i - 1)$. 
3. Sử dụng cách đơn giản hóa đã biết của phép tính tổng này, rút ​​gọn thành$n^2$. 
4. Tính toán$n \times n$sử dụng loại số nguyên chính xác an toàn hoặc tùy ý 64-bit. 
5. Xuất kết quả trực tiếp. 

Bước quan trọng là chuyển từ bài toán tính tổng sang biểu thức dạng đóng. Nếu không có điều này, chúng ta sẽ buộc phải tính toán lặp đi lặp lại, nhưng một khi đã được nhận ra, nghiệm sẽ trở thành một phép nhân đơn lẻ. 

### Tại sao nó hoạt động 

Mỗi học kỳ$2i - 1$góp phần tăng cấu trúc tuyến tính, nhưng hiệu ứng tích lũy tạo thành một hình vuông hoàn hảo. Một cách để thấy điều này là quy nạp: giả sử tổng của số đầu tiên$n$số lẻ là$n^2$, sau đó cộng số lẻ tiếp theo$2(n+1)-1 = 2n+1$mang lại:$$n^2 + (2n+1) = (n+1)^2$$vì vậy tài sản giữ cho tất cả$n$. Bất biến này đảm bảo rằng ở mỗi bước, tổng vẫn là một bình phương hoàn hảo, đảm bảo tính đúng đắn của biểu thức cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
print(n * n)
```Việc triển khai trực tiếp mã hóa công thức dạng đóng. Đầu vào được đọc một lần, loại bỏ khoảng trắng và được chuyển đổi thành số nguyên. Phép tính này là một phép nhân đơn, an toàn trong Python do số học số nguyên có độ chính xác tùy ý. 

Một lỗi phổ biến ở đây là cố gắng xây dựng trình tự một cách rõ ràng trong một vòng lặp, việc này không cần thiết và chậm hơn. Một sai lầm khác là sử dụng số học dấu phẩy động cho các số lớn$n$, có thể gây ra các vấn đề về độ chính xác. Bám sát phép nhân số nguyên sẽ tránh được cả hai vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 3$Chúng tôi tính tổng của 1, 3, 5. 

| Bước | Giá trị của tôi | Số lẻ | Tổng chạy | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 3 | 4 | 
| 3 | 3 | 5 | 9 | 

Đầu ra cuối cùng là 9, phù hợp$3^2 = 9$. 

Dấu vết này xác nhận rằng phép truy hồi xây dựng các hình vuông hoàn hảo tăng dần. 

### Ví dụ 2:$n = 5$Chúng tôi tính toán 1 + 3 + 5 + 7 + 9. 

| Bước | Giá trị của tôi | Số lẻ | Tổng chạy | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 3 | 4 | 
| 3 | 3 | 5 | 9 | 
| 4 | 4 | 7 | 16 | 
| 5 | 5 | 9 | 25 | 

Kết quả cuối cùng là 25, phù hợp$5^2$. Điều này chứng tỏ rằng cấu trúc vẫn tồn tại thống nhất trên tất cả các tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ một phép tính số học duy nhất được thực hiện | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì ngay cả đầu vào lớn nhất có thể cũng chỉ kích hoạt một phép nhân. Việc sử dụng bộ nhớ là không đổi và không phụ thuộc vào kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline
    n = int(input().strip())
    return str(n * n)

# provided samples (conceptual, since samples are implicit)
assert run("1\n") == "1"
assert run("2\n") == "4"
assert run("3\n") == "9"

# minimum edge case
assert run("1\n") == "1"

# small even case
assert run("4\n") == "16"

# large case
assert run("10000000\n") == str(10000000 * 10000000)

# square boundary behavior
assert run("99999\n") == str(99999 * 99999)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | tính đúng đắn của trường hợp tối thiểu | 
| 4 | 16 | tính đúng đắn cho chẵn nhỏ n | 
| 10^7 | 10^14 | xử lý ràng buộc lớn | 
| 99999 | 9999800001 | độ chính xác hình vuông lớn không tầm thường | 

## Vỏ cạnh 

Đầu vào nhỏ nhất$n = 1$rất quan trọng vì nó kiểm tra xem việc triển khai có xử lý chính xác các chuỗi tầm thường hay không. Thuật toán tính toán$1 \times 1 = 1$, khớp với số lẻ duy nhất trong dãy. 

Đối với lớn$n$, chẳng hạn như$n = 10^7$, việc tính toán giảm xuống còn$10^{14}$. Vì Python hỗ trợ các số nguyên chính xác tùy ý nên không có rủi ro tràn. Dấu vết tinh thần từng bước cho thấy rằng không có sự lặp lại nào xảy ra, chỉ có một phép nhân duy nhất, do đó hiệu suất vẫn ổn định bất kể kích thước đầu vào. 

Việc triển khai không chính xác có thể xảy ra sẽ cố gắng xây dựng trình tự một cách rõ ràng. Vì$n = 10^7$, điều này sẽ cố gắng thực hiện mười triệu lần lặp, đây là chi phí không cần thiết. Cách tiếp cận tối ưu hóa hoàn toàn tránh được điều này bằng cách dựa vào đặc điểm cấu trúc của các số lẻ tạo thành các hình vuông hoàn hảo.
