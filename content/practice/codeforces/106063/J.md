---
title: "CF 106063J - Juan đấu với Frank"
description: "Vấn đề quy giản toàn bộ câu chuyện thành một quyết định kinh tế duy nhất được lặp lại cho càng nhiều bạn bè càng tốt. Mỗi người bạn yêu cầu mua một món đồ giống hệt nhau với số tiền cố định và Juan có ngân sách hạn chế."
date: "2026-06-25T12:15:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106063
codeforces_index: "J"
codeforces_contest_name: "2025 ICPC Gran Premio de Mexico 3ra Fecha"
rating: 0
weight: 106063
solve_time_s: 39
verified: true
draft: false
---

[CF 106063J - Juan vs Frank](https://codeforces.com/problemset/problem/106063/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề quy giản toàn bộ câu chuyện thành một quyết định kinh tế duy nhất được lặp lại cho càng nhiều bạn bè càng tốt. Mỗi người bạn yêu cầu mua một món đồ giống hệt nhau với số tiền cố định và Juan có ngân sách hạn chế. Câu hỏi đặt ra là anh ta có thể hoàn thành số lần mua hàng đầy đủ tối đa mà không vượt quá số tiền của mình. 

Nói một cách cụ thể hơn, đầu vào đưa ra hai số nguyên, giá của một mặt hàng và tổng số tiền có sẵn. Mỗi lần mua hàng sẽ tiêu tốn chính xác mức giá cố định đó từ ngân sách và số tiền còn lại không đủ cho một lần mua đầy đủ khác sẽ không thể được sử dụng. 

Đầu ra là một số nguyên biểu thị số lượng giao dịch mua hoàn chỉnh có thể được thực hiện. 

Các ràng buộc lớn nhưng tầm thường đối với tính toán. Cả hai giá trị có thể lên tới 10^9, điều này ngay lập tức loại trừ bất kỳ mô phỏng nào trừ liên tục từng cái một trong trường hợp xấu nhất, vì điều đó sẽ giảm xuống thời gian tuyến tính tính theo số tiền. Vòng lặp trực tiếp có thể yêu cầu tới 10^9 lần lặp trong trường hợp xấu nhất, vượt xa giới hạn thời gian cho phép. 

Thay vào đó, cấu trúc của bài toán gợi ý rõ ràng rằng một phép toán số học có thời gian không đổi được mong đợi. 

Không có trường hợp cạnh cấu trúc phức tạp nào liên quan đến việc đặt hàng hoặc nhiều đầu vào. Các trường hợp khó phát hiện duy nhất là các điều kiện biên khi ngân sách nhỏ hơn chi phí hoặc chia chính xác cho chi phí. Ví dụ: nếu chi phí là 5 và ngân sách là 4 thì không thể mua hàng và câu trả lời là 0. Nếu chi phí là 5 và ngân sách là 10 thì câu trả lời chính xác là 2. Nếu ngân sách không phải là bội số, chẳng hạn như 39 với chi phí 5, thì chỉ số nguyên của số lần mua đầy đủ là quan trọng và phần còn lại không liên quan. 

Một sai lầm ngây thơ là cố gắng hạch toán số tiền còn sót lại như thể nó có thể đóng góp một phần, chẳng hạn như làm tròn nhầm thay vì làm tròn xuống. Điều đó sẽ tạo ra câu trả lời không chính xác trong các trường hợp như chi phí 5 và ngân sách 6, trong đó chỉ có 1 lần mua hàng hợp lệ nhưng cách tiếp cận dựa trên mức trần sẽ đưa ra sai 2. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực rất đơn giản: liên tục trừ chi phí khỏi ngân sách trong khi nó vẫn không âm, đếm xem điều này có thể xảy ra bao nhiêu lần. Điều này đúng vì mỗi phép trừ tương ứng chính xác với việc mua một mặt hàng và quá trình này tự nhiên dừng lại khi số tiền còn lại không đủ. 

Tuy nhiên, cách làm này quá chậm khi ngân sách lớn. Nếu chi phí là 1 và ngân sách là 10^9 thì vòng lặp sẽ chạy 10^9 lần và không thể vượt qua trong giới hạn thời gian một giây. 

Quan sát quan trọng là phép trừ lặp đi lặp lại này chính xác là phép chia số nguyên. Thay vì mô phỏng mỗi lần mua hàng, chúng tôi có thể tính toán trực tiếp số lần chi phí phù hợp với ngân sách bằng cách sử dụng một phép chia duy nhất. Điều này nén toàn bộ quá trình thành một phép tính theo thời gian không đổi trong khi vẫn đảm bảo tính chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Vòng trừ Brute Force | O(m/c) | O(1) | Quá chậm | 
| Chia số nguyên trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên biểu thị giá của một món hàng và tổng số tiền hiện có. Chúng xác định một quá trình trừ lặp đi lặp lại. 
2. Tính xem có bao nhiêu đơn vị chi phí đầy đủ phù hợp với ngân sách bằng cách sử dụng phép chia số nguyên. Điều này trực tiếp tương ứng với số lần mua hàng hoàn chỉnh có thể thực hiện được trước khi số tiền còn lại trở nên không đủ. 
3. Xuất kết quả tính toán làm kết quả cuối cùng. 

Lý do bước này hợp lệ là vì mỗi giao dịch mua đều tiêu tốn một số tiền cố định giống hệt nhau, do đó số lần mua chỉ phụ thuộc vào số lượng bản sao rời rạc của số tiền đó có thể được gộp vào tổng ngân sách. 

### Tại sao nó hoạt động

Tổng số tiền có thể được xem dưới dạng một đoạn thẳng có độ dài m và mỗi lần mua sẽ tiêu thụ một đoạn có độ dài cố định c. Số lượng tối đa các đoạn có độ dài c không chồng lấp vừa với m chính xác là thương số nguyên của m chia cho c. Không có sự sắp xếp hoặc chiến lược nào thay đổi số lượng này vì mọi mục đều giống hệt nhau và độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

c, m = map(int, input().split())

print(m // c)
```Giải pháp đọc hai số nguyên và áp dụng ngay phép chia số nguyên. Không cần vòng lặp hoặc cấu trúc dữ liệu bổ sung vì bài toán được xác định hoàn toàn bằng một mối quan hệ số học duy nhất. 

Một điểm tinh tế là đảm bảo phép chia số nguyên được sử dụng thay vì phép chia dấu phẩy động. Phép chia dấu phẩy động theo sau là phép chia cắt ngắn có thể gây ra các vấn đề về độ chính xác cho các giá trị lớn, trong khi phép chia số nguyên lại chính xác và an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 10
```| Bước | Chi phí (c) | Tiền (triệu) | m // c | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 10 | 3 | 3 | 

Tại đây, bạn có thể thực hiện 3 lần mua đầy đủ, chỉ để lại 1 đơn vị tiền chưa sử dụng. Phần còn lại đó không thể tài trợ cho việc mua hàng khác nên nó bị bỏ qua. 

### Ví dụ 2 

đầu vào:```
5 39
```| Bước | Chi phí (c) | Tiền (triệu) | m // c | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 39 | 7 | 7 | 

Bảy lần mua tiêu tốn 35 đơn vị tiền, còn lại 4 đơn vị không được sử dụng. Vì 4 nhỏ hơn 5 nên không thể mua thêm. 

Những ví dụ này xác nhận rằng câu trả lời chỉ phụ thuộc vào phép chia số nguyên, không phụ thuộc vào số tiền còn lại ngoài đơn vị chi phí đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một phép tính số học được thực hiện bất kể kích thước đầu vào | 
| Không gian | O(1) | Không có bộ nhớ bổ sung nào ngoài một vài biến được sử dụng | 

Giải pháp dễ dàng nằm trong giới hạn vì nó thực hiện công việc liên tục cho mỗi trường hợp thử nghiệm và chỉ có một trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    c, m = map(int, input().split())
    return str(m // c)

# provided samples
assert run("3 10") == "3"
assert run("2 4") == "2"
assert run("5 39") == "7"

# custom cases
assert run("5 4") == "0", "budget smaller than cost"
assert run("1 1000000000") == "1000000000", "maximal purchases"
assert run("7 7") == "1", "exact division"
assert run("8 15") == "1", "remainder ignored"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 4 | 0 | Không thể mua hàng khi ngân sách quá nhỏ | 
| 1 1000000000 | 1000000000 | Trường hợp căng thẳng tối đa | 
| 7 7 | 1 | Chia hết chính xác | 
| 8 15 | 1 | Hành vi sàn đúng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi ngân sách nhỏ hơn chi phí. Đối với đầu vào`5 4`, thuật toán tính toán`4 // 5`, kết quả chính xác là 0. Mọi lý luận dựa trên phép trừ cũng dừng ngay lập tức, nhưng chỉ có phương pháp chia mới đảm bảo điều này trong thời gian không đổi. 

Một trường hợp khác là chia hết chính xác, chẳng hạn như`7 14`(không phải trong mẫu nhưng mang tính đại diện). Sự tính toán`14 // 7`cho 2, phù hợp với thực tế là không còn dư và không thể mua một phần. 

Trường hợp cuối cùng là khi chi phí bằng 1. Đối với đầu vào như`1 10^9`, câu trả lời chính là ngân sách, vì mỗi đơn vị tiền chỉ mua được đúng một mặt hàng. Điều này kiểm tra xem việc triển khai có xử lý các giá trị tối đa mà không gặp vấn đề về tràn hoặc hiệu suất hay không.
