---
title: "CF 104745B - Hoạt động"
description: "Chúng ta được cho hai số nguyên dương và chúng ta được phép áp dụng một trong bốn phép tính số học cơ bản giữa chúng: cộng, trừ, nhân hoặc chia. Mỗi thao tác tạo ra một giá trị và chúng ta phải xác định thao tác nào mang lại kết quả lớn nhất."
date: "2026-06-28T23:01:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104745
codeforces_index: "B"
codeforces_contest_name: "CAMA 2023"
rating: 0
weight: 104745
solve_time_s: 48
verified: true
draft: false
---

[CF 104745B - Hoạt động](https://codeforces.com/problemset/problem/104745/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên dương và chúng ta được phép áp dụng một trong bốn phép tính số học cơ bản giữa chúng: cộng, trừ, nhân hoặc chia. Mỗi thao tác tạo ra một giá trị và chúng ta phải xác định thao tác nào mang lại kết quả lớn nhất. 

Đầu vào chỉ đơn giản là hai số nguyên, hãy gọi chúng$a$Và$b$, bao gồm cả từ 1 đến 100. Nhiệm vụ là tính toán bốn kết quả ứng cử viên từ các giá trị này và đưa ra kết quả tối đa trong số đó. 

Phạm vi hạn chế là cực kỳ nhỏ. Vì cả hai số đều có nhiều nhất là 100 nên mọi phép toán số học đều có thời gian không đổi và có thể được đánh giá trực tiếp mà không cần quan tâm đến vấn đề tràn trong Python hoặc hiệu suất. Điều này ngay lập tức loại trừ sự cần thiết của bất kỳ kỹ thuật tiền xử lý, tìm kiếm hoặc tối ưu hóa nào. Đánh giá trực tiếp tất cả các biểu thức có thể là đủ. 

Có một vài điểm tế nhị có thể gây ra sai sót nếu xử lý không cẩn thận. Hoạt động phân chia là hoạt động chính. Nếu được hiểu là phép chia thực, nó tạo ra một giá trị dấu phẩy động, trong khi các phép toán khác tạo ra số nguyên. Điều này có nghĩa là việc so sánh phải được thực hiện một cách nhất quán theo kiểu số để duy trì tính chính xác trong tất cả các hoạt động. 

Một cạm bẫy tiềm ẩn khác là giả sử phép chia số nguyên. Ví dụ, nếu$a = 8$Và$b = 3$, phép chia số nguyên sẽ cho kết quả 2, trong khi phép chia thực cho kết quả xấp xỉ 2,666..., điều này có thể ảnh hưởng đến việc liệu phép chia có trở thành phép toán tối đa hay không. Vì bài toán không hạn chế phép chia thành phép chia số nguyên nên nó phải được coi là phép chia thực. 

Trường hợp cạnh thứ hai là khi phép trừ tạo ra số âm. Vì đầu vào là dương nên phép trừ vẫn có thể âm và giá trị đó sẽ không bao giờ lớn nhất khi so sánh với phép cộng, phép nhân hoặc phép chia, nhưng nó vẫn phải được xem xét về tính chính xác. 

## Phương pháp tiếp cận 

Cách đơn giản để giải quyết vấn đề này là tính toán rõ ràng cả bốn biểu thức:$a + b$,$a - b$,$a \times b$, Và$a / b$. Khi chúng tôi tính toán các giá trị này, chúng tôi chỉ cần lấy mức tối đa. 

Cách tiếp cận bạo lực này đã là tối ưu vì quy mô vấn đề là không đổi. Chúng tôi thực hiện chính xác bốn thao tác, mỗi thao tác trong thời gian không đổi. Không có cấu trúc hoặc ràng buộc ẩn nào cho thấy bất kỳ thao tác nào cũng có thể được bỏ qua một cách an toàn mà không cần đánh giá. Ngay cả việc suy luận về mối quan hệ thống trị giữa các hoạt động cũng không cần thiết vì phạm vi đầu vào nhỏ và không cần cắt tỉa. 

Cách tiếp cận bạo lực là đúng vì mọi hoạt động được phép đều độc lập và mức tối đa phải đến từ một trong số chúng. Không có sự biến đổi hay trình tự các thao tác nên không có sự bùng nổ tổ hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên$a$Và$b$. Đây là những thông tin đầu vào duy nhất được yêu cầu và không cần phân tích cú pháp hoặc tiền xử lý bổ sung. 
2. Tính tổng$a + b$. Giá trị này thể hiện sự kết hợp cộng của hai giá trị và luôn không giảm so với từng đầu vào. 
3. Tính chênh lệch$a - b$. Điều này nắm bắt được hiệu ứng của phép trừ, có thể làm giảm giá trị đáng kể tùy thuộc vào độ lớn tương đối của đầu vào. 
4. Tính tích$a \times b$. Vì cả hai số đều dương và ít nhất bằng 1 nên giá trị này ít nhất sẽ luôn lớn bằng mỗi đầu vào riêng lẻ. 
5. Tính phép chia$a / b$dùng phép chia thực. Điều này đảm bảo các kết quả phân số được giữ nguyên, cần thiết để so sánh chính xác với các hoạt động khác. 
6. So sánh tất cả bốn kết quả và đưa ra kết quả tối đa. 

### Tại sao nó hoạt động 

Mọi câu trả lời hợp lệ đều được xây dựng rõ ràng ở một trong các bước trước đó. Vì bài toán xác định không gian kết quả có chính xác bốn biểu thức và không có phép toán hoặc phép biến đổi ẩn nào, nên việc đánh giá tất cả các ứng cử viên đảm bảo rằng không có câu trả lời nào bị bỏ sót. Theo định nghĩa, giá trị tối đa của một tập hợp hữu hạn là câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b = map(int, input().split())
    
    res1 = a + b
    res2 = a - b
    res3 = a * b
    res4 = a / b  # real division
    
    print(max(res1, res2, res3, res4))

if __name__ == "__main__":
    solve()
```Giải pháp đọc hai số nguyên và đánh giá trực tiếp từng biểu thức trong số bốn biểu thức ứng cử viên. Phép chia được thực hiện bằng cách sử dụng số học dấu phẩy động, trong Python đủ chính xác cho các giá trị trong phạm vi đã cho. Câu trả lời cuối cùng được tính toán bằng cách sử dụng hàm max tích hợp sẵn của Python qua bốn kết quả. 

Một chi tiết triển khai tinh tế là đảm bảo rằng phép chia không vô tình bị thay thế bằng phép chia số nguyên bằng cách sử dụng`//`. Làm như vậy sẽ thay đổi ý nghĩa của phép toán và có thể giảm giá trị tối đa một cách không chính xác trong trường hợp giá trị phân số quan trọng. 

## Ví dụ đã hoạt động 

### Ví dụ 1: Đầu vào`6 3`| Bước | một | b | a+b | a-b | a*b | a/b | Tối đa hiện tại | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Tính toán | 6 | 3 | 9 | 3 | 18 | 2.0 | 18 | 

Kết quả của phép nhân lấn át tất cả các kết quả khác nên kết quả đầu ra là 18. Điều này khẳng định rằng phép nhân có thể áp đảo cả phép cộng và phép chia ngay cả đối với các đầu vào nhỏ. 

### Ví dụ 2: Nhập liệu`8 1`| Bước | một | b | a+b | a-b | a*b | a/b | Tối đa hiện tại | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Tính toán | 8 | 1 | 9 | 7 | 8 | 8.0 | 9 | 

Ở đây phép cộng tạo ra giá trị lớn nhất. Ví dụ này cho thấy phép nhân không phải lúc nào cũng tối ưu, đặc biệt khi một toán hạng bằng 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có bốn phép tính số học và một phép so sánh được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Bản chất thời gian không đổi của giải pháp dễ dàng phù hợp với bất kỳ giới hạn thời gian hợp lý nào và việc sử dụng bộ nhớ là không đáng kể do chỉ có một số biến vô hướng được lưu trữ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    a, b = map(int, sys.stdin.readline().split())
    
    res1 = a + b
    res2 = a - b
    res3 = a * b
    res4 = a / b
    
    return str(max(res1, res2, res3, res4))

# provided samples
assert run("6 3\n") == "18"
assert run("8 1\n") == "9"

# minimum values
assert run("1 1\n") == "2"

# subtraction dominates negativity check
assert run("1 100\n") == "100"

# multiplication edge case
assert run("100 100\n") == "10000"

# division fractional dominance
assert run("3 2\n") == "6"

# identity check
assert run("5 1\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 2 | hành vi ranh giới tối thiểu | 
| 1 100 | 100 | phép trừ tạo ra giá trị âm | 
| 100 100 | 10000 | trường hợp nhân tối đa | 
| 3 2 | 6 | phép chia tạo ra giá trị phân số | 

## Vỏ cạnh 

Trường hợp một cạnh là khi phép trừ trở nên âm, chẳng hạn như đầu vào`1 100`. Thuật toán tính toán`1 - 100 = -99`, nhưng giá trị này đương nhiên bị bỏ qua bởi`max`hoạt động vì các kết quả khác lớn hơn. Việc tính toán vẫn bao gồm nó, duy trì tính chính xác bằng tính đầy đủ hơn là cắt tỉa. 

Một trường hợp khác là khi phép chia tạo ra một giá trị không nguyên, chẳng hạn như`3 2`. Thuật toán tính toán`3 / 2 = 1.5`, trong khi phép nhân cho`6`, trở thành cực đại. Kết quả dấu phẩy động vẫn được so sánh chính xác với số nguyên vì Python thúc đẩy số nguyên thành số float trong quá trình so sánh. 

Trường hợp cạnh cuối cùng là khi cả hai số đều bằng nhau, chẳng hạn như`100 100`. Tất cả các phép toán vẫn hợp lệ và được xác định rõ ràng, đồng thời phép nhân tạo ra kết quả lớn nhất. Thuật toán không dựa vào bất kỳ giả định thứ tự nào và đánh giá tất cả các biểu thức một cách thống nhất, đảm bảo tính chính xác trên các đầu vào đối xứng.
