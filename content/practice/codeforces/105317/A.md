---
title: "CF 105317A - Juan và Alfino"
description: "Chúng ta được cho hai số nguyên. Một biểu thị giá của một chiếc bánh sandwich, và cái còn lại biểu thị số tiền mà Juan có sẵn. Juan muốn chỉ tiêu tiền của mình vào những chiếc bánh mì đầy đủ và tặng chúng cho bạn bè, nơi mỗi người bạn nhận được chính xác một chiếc bánh sandwich."
date: "2026-06-23T15:12:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105317
codeforces_index: "A"
codeforces_contest_name: "JPC 1.0"
rating: 0
weight: 105317
solve_time_s: 56
verified: true
draft: false
---

[CF 105317A – Juan và Alfino](https://codeforces.com/problemset/problem/105317/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên. Một biểu thị giá của một chiếc bánh sandwich, và cái còn lại biểu thị số tiền mà Juan có sẵn. Juan muốn chỉ tiêu tiền của mình vào những chiếc bánh mì đầy đủ và tặng chúng cho bạn bè, nơi mỗi người bạn nhận được chính xác một chiếc bánh sandwich. 

Nhiệm vụ là xác định xem anh ta có thể mua được bao nhiêu chiếc bánh mì đầy đủ, tương đương với số lượng bạn bè mà anh ta có thể chiêu đãi. Vì mỗi chiếc bánh sandwich có giá cố định nên vấn đề giảm xuống còn việc chia tổng số tiền sẵn có cho giá mỗi chiếc bánh sandwich và chỉ tính số lần mua hoàn chỉnh. 

Các ràng buộc đi lên đến$10^9$cho cả hai giá trị, điều này ngay lập tức loại trừ bất kỳ mô phỏng nào về việc tiêu từng chiếc bánh sandwich một lần. Một vòng lặp trừ đi chi phí nhiều lần sẽ chiếm tới$10^9$lặp đi lặp lại trong trường hợp xấu nhất, vượt xa mức phù hợp trong một giây trong Python. Cách tiếp cận khả thi duy nhất là tính toán số học theo thời gian không đổi. 

Không có trường hợp cạnh cấu trúc phức tạp nào như mảng hoặc đồ thị, nhưng vẫn có một mối lo ngại về tính chính xác tinh tế xung quanh ngữ nghĩa phân chia số nguyên. Kết quả phải là số lượng bánh sandwich hoàn chỉnh, nghĩa là khả năng chi trả một phần phải được loại bỏ. Ví dụ: nếu chi phí là 5 và số tiền là 4 thì câu trả lời đúng là 0 chứ không phải 0,8 hay 1 do làm tròn sai. Bất kỳ triển khai nào sử dụng phép chia dấu phẩy động đều có nguy cơ gặp phải các vấn đề về độ chính xác đối với đầu vào lớn gần$10^9$. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ phản ánh trực tiếp câu chuyện. Chúng tôi liên tục trừ chi phí của một chiếc bánh sandwich vào tiền của Juan cho đến khi anh ấy không còn đủ tiền mua một chiếc bánh khác. Mỗi phép trừ tương ứng với việc mua một chiếc bánh sandwich và tăng số lượng bạn bè được phục vụ. Điều này đúng vì mỗi bước sẽ giảm ngân sách còn lại theo các đơn vị riêng biệt hợp lệ. 

Tuy nhiên, phương pháp này trở nên cực kỳ chậm khi chi phí nhỏ và ngân sách lớn. Nếu chi phí là 1 và số tiền là$10^9$, chúng tôi sẽ biểu diễn$10^9$lặp đi lặp lại, mỗi lần thực hiện công việc liên tục. Điều đó đã vượt quá giới hạn thời gian thông thường theo cấp độ lớn. 

Quan sát quan trọng là phép trừ lặp đi lặp lại này chính xác là phép chia số nguyên. Thay vì mô phỏng mỗi lần mua hàng, chúng tôi có thể tính toán trực tiếp số lần chi phí phù hợp với ngân sách. Điều này làm giảm toàn bộ quá trình thành một phép toán số học duy nhất. 

Cấu trúc của vấn đề đảm bảo tính độc lập giữa các lần mua hàng, không có chiết khấu, không có hiệu ứng tích lũy và không có ràng buộc liên kết việc mua hàng với nhau. Điều đó làm cho thương số$m \div c$đủ để mô tả câu trả lời cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (trừ nhiều lần) | O(m/c) | O(1) | Quá chậm | 
| Tối ưu (chia số nguyên) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên$c$Và$m$từ đầu vào. Chúng đại diện cho chi phí cho mỗi chiếc bánh sandwich và tổng số tiền hiện có. 
2. Tính xem có bao nhiêu chiếc bánh mì nguyên miếng phù hợp với ngân sách bằng cách sử dụng phép chia số nguyên$m // c$. Hoạt động này sẽ tự động loại bỏ bất kỳ phần còn lại nào, tương ứng với số tiền còn sót lại không thể sử dụng được và không thể mua được một chiếc bánh sandwich đầy đủ. 
3. Xuất ra giá trị tính toán là số lượng bạn bè mà Juan có thể chiêu đãi. 

### Tại sao nó hoạt động 

Mỗi chiếc bánh sandwich tiêu thụ chính xác$c$đơn vị tiền, và không có cách nào để kết hợp một phần tiền từ các loại bánh sandwich khác nhau. Bất kỳ trình tự mua hàng hợp lệ nào cũng phải tiêu tốn tiền theo khối lượng$c$. Do đó, số lượng khối hợp lệ tối đa phù hợp với$m$chính xác là thương số của$m$chia cho$c$. Không có sự sắp xếp nào có thể vượt quá mức này vì nó có nghĩa là vượt quá tổng ngân sách, và không có sự sắp xếp nào có thể nhỏ hơn vì chúng ta luôn có thể mua bánh mì một cách tham lam cho đến khi số tiền còn lại ít hơn$c$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

c, m = map(int, input().split())
print(m // c)
```Giải pháp áp dụng trực tiếp phép chia số nguyên. Việc sử dụng`//`là rất quan trọng vì nó đảm bảo sự phân chia sàn, mô hình này chỉ tính chính xác những chiếc bánh sandwich hoàn chỉnh. 

Không cần vòng lặp hoặc bộ nhớ bổ sung. Toàn bộ quá trình tính toán là một thao tác liên tục trong thời gian sau khi phân tích cú pháp dữ liệu đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 10
```Chúng tôi theo dõi tính toán: 

| Bước | c | m | m // c | Đầu ra cho đến nay | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 3 | 10 | - | - | 
| Tính toán | 3 | 10 | 3 | 3 | 

Kết quả là 3 vì 10 chứa ba nhóm đầy đủ cỡ 3, với 1 đơn vị tiền còn lại chưa được sử dụng. 

Điều này khẳng định rằng số tiền còn sót lại không góp phần tạo ra một chiếc bánh sandwich bổ sung. 

### Ví dụ 2 

đầu vào:```
5 4
```| Bước | c | m | m // c | Đầu ra cho đến nay | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 5 | 4 | - | - | 
| Tính toán | 5 | 4 | 0 | 0 | 

Ở đây ngân sách không đủ cho dù chỉ một chiếc bánh sandwich, vì vậy câu trả lời là 0. Điều này cho thấy thuật toán xử lý chính xác các trường hợp trong đó$m < c$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ phân tích cú pháp đầu vào và thực hiện một phép chia đơn lẻ | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Các ràng buộc cho phép lên đến$10^9$, nhưng giải pháp thực hiện số học theo thời gian không đổi, do đó, nó phù hợp thoải mái trong giới hạn ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    c, m = map(int, input().split())
    return str(m // c)

# provided samples
assert run("3 10") == "3"
assert run("5 39") == "7"

# custom cases
assert run("1 1") == "1", "minimum valid equal case"
assert run("1 1000000000") == "1000000000", "maximum budget single cost"
assert run("10 9") == "0", "just below threshold"
assert run("7 49") == "7", "exact division case"
assert run("6 1") == "0", "budget smaller than cost"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | trường hợp khác 0 nhỏ nhất | 
| 1 1000000000 | 1000000000 | quy mô đầu ra tối đa | 
| 10 9 | 0 | thực sự không đủ ngân sách | 
| 7 49 | 7 | tính chia hết chính xác | 
| 6 1 | 0 | trường hợp không thể mua được | 

## Vỏ cạnh 

Trường hợp một cạnh là khi ngân sách nhỏ hơn chi phí. Đối với đầu vào`c = 10, m = 3`, thuật toán tính toán`3 // 10 = 0`. Điều này đúng vì không thể mua được một chiếc bánh sandwich đầy đủ và phép chia số nguyên thực hiện điều này một cách tự nhiên mà không cần xử lý đặc biệt. 

Một trường hợp khác là khi ngân sách chia hết cho chi phí, chẳng hạn như`c = 4, m = 12`. Tính toán mang lại kết quả`12 // 4 = 3`, và không có phần dư. Thuật toán đếm chính xác tất cả các giao dịch mua có sẵn mà không tính quá mức. 

Trường hợp cuối cùng là khi cả hai giá trị đều ở kích thước tối đa, ví dụ:`c = 10^9, m = 10^9`. Kết quả là`1`và quá trình tính toán vẫn an toàn vì số học số nguyên của Python xử lý các số nguyên lớn và phép chia là thời gian không đổi ở quy mô này trong bối cảnh lập trình cạnh tranh.
