---
title: "CF 104344B - Triplas pitag\u00f3ricas"
description: "Chúng ta có hai số nguyên $m$ và $n$, với $1 le n < m le 10^4$. Từ hai giá trị này, chúng ta phải xây dựng bộ ba số nguyên bằng cách sử dụng công thức đại số cố định và in kết quả theo một thứ tự cụ thể. Việc xây dựng không phải là tùy tiện."
date: "2026-07-01T18:27:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104344
codeforces_index: "B"
codeforces_contest_name: "Maratona dos Bixes 2023 - UNICAMP"
rating: 0
weight: 104344
solve_time_s: 81
verified: true
draft: false
---

[CF 104344B - Triplas pitag\u00f3ricas](https://codeforces.com/problemset/problem/104344/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên$m$Và$n$, với$1 \le n < m \le 10^4$. Từ hai giá trị này, chúng ta phải xây dựng bộ ba số nguyên bằng cách sử dụng công thức đại số cố định và in kết quả theo một thứ tự cụ thể. 

Việc xây dựng không phải là tùy tiện. Nó tuân theo một ánh xạ xác định từ cặp$(m, n)$thành ba giá trị:$a = m^2 - n^2$,$b = 2mn$,$c = m^2 + n^2$. 

Đầu ra chỉ đơn giản là ba giá trị được tính toán theo thứ tự chính xác này. 

Mặc dù câu lệnh đề cập đến trực giác hình học, bản thân nhiệm vụ này hoàn toàn là số học. Yêu cầu thực sự duy nhất là đánh giá và đặt hàng chính xác. 

Những hạn chế là cực kỳ nhỏ. Ngay cả giá trị lớn nhất,$m^2$, nhiều nhất là$10^8$, vì vậy tất cả các phép tính đều nằm gọn trong các số nguyên 32 bit. Điều này có nghĩa là không cần số học mô-đun, số nguyên lớn hoặc tối ưu hóa ngoài tính toán trực tiếp. 

Một kiểu thất bại phổ biến ở đây là sắp xếp lại bộ ba. Biểu thức xác định một thứ tự cụ thể và việc hoán đổi các giá trị vẫn tạo ra bộ ba Pythagore hợp lệ nhưng câu trả lời không hợp lệ cho vấn đề này. Ví dụ, với đầu vào$m=3, n=2$, đầu ra đúng là$5\ 12\ 13$, trong khi$12\ 5\ 13$là không chính xác mặc dù nó thỏa mãn cùng một danh tính. 

Một vấn đề tế nhị khác là giả sử mọi hoán vị đều được chấp nhận vì mọi hoán vị đều thỏa mãn$a^2 + b^2 = c^2$. Thuộc tính đó không liên quan ở đây vì vấn đề khắc phục thứ tự một cách rõ ràng. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ bỏ qua công thức dạng đóng và cố gắng “khám phá” một bộ ba Pythagore hợp lệ khớp với cấu trúc đã cho. Người ta có thể thử liệt kê các số nguyên ứng viên$a, b, c$bắt nguồn từ$m, n$hoặc thậm chí tìm kiếm bộ ba thỏa mãn$a^2 + b^2 = c^2$gần độ lớn của$m^2$. Cách tiếp cận như vậy là không cần thiết và không hiệu quả vì bài toán không yêu cầu khám phá mà chỉ yêu cầu đánh giá một công thức đã biết. 

Nếu chúng ta cố gắng tìm kiếm đơn giản trên các bộ ba có thể có độ lớn$O(m^2)$, trường hợp xấu nhất sẽ liên quan đến việc kiểm tra tới$O(m^4)$sự kết hợp trong không gian 3 vòng đơn giản, vượt xa giới hạn ngay cả đối với$m = 10^4$. Thậm chí giảm xuống một vòng lặp duy nhất$c$vẫn sẽ yêu cầu kiểm tra nhiều phân tách, điều này gây lãng phí công việc vì cấu trúc đã được xác định đầy đủ. 

Quan sát chính là công thức đã cho đã hoàn chỉnh. Nó trực tiếp xây dựng bộ ba mà không có sự mơ hồ. Mỗi trong số$a$,$b$, Và$c$chỉ phụ thuộc vào$m$Và$n$, do đó toàn bộ vấn đề quy về việc đánh giá ba biểu thức số học. 

Điều này loại bỏ bất kỳ thành phần tổ hợp hoặc tìm kiếm nào. Không có bước ra quyết định, chỉ có tính toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force tìm kiếm bộ ba |$O(m^4)$hoặc tệ hơn |$O(1)$| Quá chậm | 
| Đánh giá công thức trực tiếp |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$m$Và$n$. Chúng xác định cặp tham số xác định duy nhất bộ ba. 
2. Tính toán$m^2$một lần, vì nó được sử dụng trong tất cả các biểu thức. Điều này tránh được phép nhân dư thừa và giữ cho tính toán rõ ràng. 
3. Tính toán$a = m^2 - n^2$. Điều này tương ứng với sự khác biệt của cấu trúc hình vuông, đảm bảo$a$là tích cực bởi vì$m > n$. 
4. Tính toán$b = 2mn$. Đây là thuật ngữ chéo có tỷ lệ tuyến tính với cả hai tham số. 
5. Tính toán$c = m^2 + n^2$. Đây là tổng bình phương và sẽ luôn có giá trị lớn nhất trong ba giá trị. 
6. Đầu ra$a, b, c$theo đúng thứ tự này. 

### Tại sao nó hoạt động 

Việc xây dựng là tham số hóa Euclide cổ điển của bộ ba Pythagore. Về mặt đại số, thay thế các biểu thức cho thấy rằng$(m^2 - n^2)^2 + (2mn)^2 = (m^2 + n^2)^2$, 

bởi vì cả hai bên đều mở rộng đến$m^4 + 2m^2n^2 + n^4$. Đẳng thức này đảm bảo rằng bộ ba được tạo ra luôn thỏa mãn điều kiện Pythagore. 

Thứ tự được cố định bởi chính định nghĩa chứ không phải theo kích thước. Mặc dù$c$luôn có giá trị lớn nhất$a$Và$b$không được sắp xếp theo thứ tự tương đối với nhau, vì vậy việc hoán đổi chúng sẽ vẫn đảm bảo tính chính xác về mặt toán học nhưng lại vi phạm đặc tả đầu ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    m, n = map(int, input().split())

    m2 = m * m
    n2 = n * n

    a = m2 - n2
    b = 2 * m * n
    c = m2 + n2

    print(a, b, c)

if __name__ == "__main__":
    main()
```Việc thực hiện trực tiếp theo công thức. Tính toán trước$m^2$Và$n^2$tránh việc tính toán lại và giữ cho mã có thể đọc được. phép nhân$2mn$được tính toán rõ ràng để tránh bất kỳ sự mơ hồ nào về quyền ưu tiên của toán tử, mặc dù Python xử lý nó một cách chính xác. 

Tất cả các giá trị được tính toán bằng cách sử dụng số học số nguyên và kiểu số nguyên của Python dễ dàng chứa các giá trị tối đa có thể. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
```| Bước | m | n | m2 | n² | a = m2−n2 | b = 2 triệu | c = m2+n2 | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 1 | 4 | 1 | - | - | - | 
| Tính toán | 2 | 1 | 4 | 1 | 3 | 4 | 5 | 

Đầu ra:```
3 4 5
```Trường hợp này xác nhận rằng bộ ba Pythagore hợp lệ nhỏ nhất được tạo ra một cách chính xác. Nó cũng cho thấy rằng$a$có thể nhỏ hơn$b$, củng cố thứ tự đó không dựa trên độ lớn. 

### Ví dụ 2 

đầu vào:```
3 2
```| Bước | m | n | m2 | n² | a = m2−n2 | b = 2 triệu | c = m2+n2 | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 2 | 9 | 4 | - | - | - | 
| Tính toán | 3 | 2 | 9 | 4 | 5 | 12 | 13 | 

Đầu ra:```
5 12 13
```Dấu vết này nêu bật thuật ngữ chéo$2mn$tăng nhanh hơn số hạng chênh lệch. Nó cũng xác nhận rằng$c$luôn là thành phần lớn nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ có một số phép tính số học không đổi được thực hiện bất kể kích thước đầu vào | 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên được lưu trữ | 

Việc tính toán chỉ bao gồm một số phép nhân và phép cộng, vì vậy nó thấp hơn nhiều so với giới hạn ngay cả đối với những số lượng lớn. Việc sử dụng bộ nhớ là không đổi và không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    m, n = map(int, input().split())
    a = m*m - n*n
    b = 2*m*n
    c = m*m + n*n
    return f"{a} {b} {c}"

# provided samples
assert run("2 1\n") == "3 4 5"
assert run("3 2\n") == "5 12 13"

# custom cases
assert run("4 1\n") == "15 8 17"
assert run("5 3\n") == "16 30 34"
assert run("10 9\n") == "19 180 181"
assert run("100 1\n") == "9999 200 10001"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 1 | 15 8 17 | bộ ba không nhỏ, kiểm tra thứ tự | 
| 5 3 | 16 30 34 | tính đúng đắn chung cho các giá trị trung bình | 
| 10 9 | 19 180 181 | trường hợp có cạnh đóng m và n | 
| 100 1 | 9999 200 10001 | quy mô chênh lệch lớn | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$n$rất gần với$m$, Ví dụ$m=10, n=9$. Tính toán mang lại:$a = 100 - 81 = 19$,$b = 180$,$c = 181$. 

Mặc dù$a$nhỏ, nó vẫn dương vì bất đẳng thức chặt chẽ$m > n$đảm bảo$m^2 - n^2 > 0$. Thuật toán xử lý việc này một cách tự nhiên mà không cần kiểm tra đặc biệt. 

Một trường hợp khác là khi$n = 1$Và$m$lớn, chẳng hạn như$m=100$. Đây$a$trở thành$9999$, vẫn thoải mái trong giới hạn, trong khi$b$vẫn tuyến tính trong$m$. Việc tính toán không bị tràn trong Python và ngay cả trong các ngôn ngữ số nguyên có chiều rộng cố định, nó vẫn an toàn trong giới hạn 32 bit. 

Kịch bản thứ ba là đầu vào hợp lệ nhỏ nhất$m=2, n=1$. Điều này tạo ra bộ ba cơ sở$3, 4, 5$, xác nhận rằng công thức được xác định rõ ở giới hạn dưới và không yêu cầu xử lý đặc biệt đối với các giá trị nhỏ.
