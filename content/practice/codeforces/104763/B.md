---
title: "CF 104763B - Đèn sứa"
description: "Chúng ta được cung cấp một chuỗi đèn ngắn trong đường hầm, mỗi đèn tắt (0) hoặc bật (1). Mục tiêu là biến chuỗi này thành một mô hình xen kẽ hoàn hảo, trong đó các đèn liền kề luôn khác nhau."
date: "2026-06-28T21:48:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104763
codeforces_index: "B"
codeforces_contest_name: "UTPC Contest 11-03-23 Div. 2 (Beginner)"
rating: 0
weight: 104763
solve_time_s: 64
verified: true
draft: false
---

[CF 104763B - Đèn sứa](https://codeforces.com/problemset/problem/104763/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi đèn ngắn trong đường hầm, mỗi đèn tắt (0) hoặc bật (1). Mục tiêu là biến chuỗi này thành một mô hình xen kẽ hoàn hảo, trong đó các đèn liền kề luôn khác nhau. Có chính xác hai mẫu mục tiêu hợp lệ: một mẫu bắt đầu bằng 0 và thay thế là 0101..., và một mẫu bắt đầu bằng 1 và thay thế là 1010.... 

Một thao tác duy nhất bao gồm lật đèn, thay đổi từ 0 thành 1 hoặc từ 1 thành 0. Chúng ta cần xác định số lần lật tối thiểu cần thiết để biến cấu hình đã cho thành một trong hai kiểu xen kẽ. 

Kích thước đầu vào nhỏ, với n lên tới 100. Điều này ngay lập tức gợi ý rằng ngay cả các giải pháp kiểm tra mọi vị trí và so sánh với nhiều mẫu cũng dễ dàng đủ nhanh. Quét tuyến tính cho mỗi mẫu ứng cử viên đã thực hiện tối đa vài trăm thao tác, do đó, mọi thao tác O(n) hoặc thậm chí O(n²) đều an toàn. 

Các trường hợp chính xảy ra từ các đầu vào rất nhỏ và từ các đầu vào đã xen kẽ hoặc gần như xen kẽ. Ví dụ: nếu đầu vào là "0", cả hai mẫu mục tiêu đều giảm xuống "0" hoặc "1", do đó câu trả lời là 0 hoặc 1 tùy theo so sánh. Nếu chuỗi đã xen kẽ như "010101", câu trả lời phải là 0 và bất kỳ giải pháp nào giả định không chính xác chỉ một mẫu bắt đầu vẫn có thể hoạt động nhưng có nguy cơ thiếu căn chỉnh tối ưu trong các trường hợp khác. 

Một sai lầm nhỏ xuất hiện khi chỉ xem xét một mẫu mục tiêu. Ví dụ: nếu đầu vào là "1010", nó đã khớp với mẫu bắt đầu bằng 1, nhưng khác hoàn toàn với mẫu bắt đầu bằng 0. Nói chung, chỉ kiểm tra một trong số chúng sẽ đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ xây dựng rõ ràng cả hai chuỗi mục tiêu có độ dài n, sau đó so sánh chúng với đầu vào bằng cách đếm các chuỗi không khớp. Đối với mỗi bit bắt đầu có thể, chúng tôi tạo chuỗi xen kẽ đầy đủ và tính khoảng cách Hamming. Điều này đúng vì mọi giải pháp hợp lệ phải khớp chính xác với một trong hai mẫu cố định này. 

Chi phí brute-force là không đáng kể: tạo ra hai chuỗi có độ dài n và so sánh chúng có giá O(n) cho mỗi mẫu, do đó O(2n). Với n ≤ 100 thì điều này không đáng kể. Ngay cả khi chúng ta ngây thơ hơn và chuyển đổi từng vị trí liên tục để mô phỏng các phép biến đổi, chúng ta vẫn sẽ vẫn nằm trong giới hạn, nhưng điều đó là không cần thiết. 

Quan sát quan trọng là cấu trúc của mục tiêu hoàn toàn cố định. Chỉ có hai ứng cử viên và mỗi vị trí độc lập đóng góp 0 hoặc 1 vào số lượng không khớp. Điều này loại bỏ mọi nhu cầu về mô phỏng hoặc các quyết định tham lam. Chúng tôi có thể tính toán chi phí trực tiếp bằng cách quét một lần và tích lũy những điểm không khớp với cả hai mẫu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng Brute Force + so sánh | O(n) | O(n) | Đã chấp nhận | 
| Đếm lần quét không khớp | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc chuỗi đầu vào và coi nó như một mảng ký tự. Chúng ta sẽ so sánh nó với hai mẫu xen kẽ khái niệm mà không lưu trữ chúng một cách rõ ràng. 
2. Khởi tạo hai bộ đếm: một bộ đếm chi phí khớp với mẫu bắt đầu bằng 0 và một bộ đếm cho mẫu bắt đầu bằng 1. Bộ đếm này biểu thị số lần lật cần thiết cho mỗi mục tiêu. 
3. Lặp lại qua từng chỉ mục i từ 0 đến n - 1. Tại mỗi vị trí, hãy xác định ký tự chính xác trong cả hai mẫu là gì. Đối với mẫu bắt đầu bằng 0, giá trị mong đợi là i % 2. Đối với mẫu bắt đầu bằng 1, giá trị mong đợi là 1 - (i % 2). 
4. So sánh ký tự hiện tại với cả hai giá trị mong đợi. Nếu nó khác, hãy tăng bộ đếm tương ứng. Bước này trực tiếp đo lường số lần lật cần thiết nếu chúng ta chọn mẫu đó. 
5. Sau khi xử lý tất cả các vị trí, lấy giá trị tối thiểu của hai bộ đếm và xuất ra. Điều này thể hiện mô hình xen kẽ tốt nhất có thể. 

### Tại sao nó hoạt động 

Mỗi vị trí trong chuỗi độc lập với chi phí cuối cùng vì việc lật một đèn không ảnh hưởng đến bất kỳ vị trí nào khác. Tổng số lần lật cần thiết cho một mẫu mục tiêu cố định chính xác là số chỉ số không khớp giữa đầu vào và mẫu đó. Vì chỉ có hai mẫu hợp lệ và mọi cấu hình xen kẽ hợp lệ phải khớp chính xác với một trong số chúng, nên số lượng tối thiểu trên hai số lượng không khớp này nhất thiết phải là câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
s = input().strip()

cost0 = 0  # pattern 0101...
cost1 = 0  # pattern 1010...

for i, ch in enumerate(s):
    bit = ord(ch) - 48  # convert '0'/'1' to int

    expected0 = i % 2
    expected1 = 1 - (i % 2)

    if bit != expected0:
        cost0 += 1
    if bit != expected1:
        cost1 += 1

print(min(cost0, cost1))
```Việc triển khai giữ hai số lượng không khớp đang chạy thay vì xây dựng chuỗi mục tiêu. Sự chuyển đổi`ord(ch) - 48`tránh so sánh chuỗi lặp đi lặp lại. Mỗi chỉ mục đóng góp độc lập cho cả hai bộ đếm, đó là lý do tại sao cả hai quá trình kiểm tra đều được thực hiện trong cùng một vòng lặp mà không bị can thiệp. 

Một lỗi phổ biến là chỉ cập nhật một bộ đếm tùy thuộc vào kết quả không khớp đầu tiên được tìm thấy. Cả hai đều phải được đánh giá vì mỗi mẫu là một giả thuyết riêng biệt về cấu hình cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
01011
```Chúng tôi so sánh với cả hai mẫu. 

| tôi | s[i] | dự kiến0 | chi phí0 | dự kiến1 | chi phí1 | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 1 | 1 | 
| 1 | 1 | 1 | 0 | 0 | 2 | 
| 2 | 0 | 0 | 0 | 1 | 3 | 
| 3 | 1 | 1 | 0 | 0 | 4 | 
| 4 | 1 | 0 | 1 | 1 | 4 | 

Chi phí cuối cùng là cost0 = 1 và cost1 = 4, vì vậy câu trả lời là 1. 

Dấu vết này cho thấy mô hình tối ưu được xác định hoàn toàn bằng sự tích lũy không phù hợp toàn cầu, chứ không phải bởi các quyết định cục bộ. Chỉ riêng ký tự cuối cùng đã xác định sự không khớp duy nhất. 

### Ví dụ 2 

đầu vào:```
4
1010
```| tôi | s[i] | dự kiến0 | chi phí0 | dự kiến1 | chi phí1 | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 1 | 0 | 
| 1 | 0 | 1 | 2 | 0 | 0 | 
| 2 | 1 | 0 | 3 | 1 | 0 | 
| 3 | 0 | 1 | 4 | 0 | 0 | 

Câu trả lời cuối cùng là 0 vì chuỗi đã khớp với mẫu bắt đầu bằng 1. 

Điều này xác nhận rằng việc đánh giá cả hai mẫu là cần thiết, vì câu trả lời đúng phụ thuộc hoàn toàn vào việc căn chỉnh với bit bắt đầu tốt nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được xử lý một lần với công việc liên tục | 
| Không gian | O(1) | Chỉ có hai quầy được duy trì | 

Với n ≤ 100, nghiệm này thấp hơn nhiều so với bất kỳ giới hạn thực tế nào. Ngay cả với những hạn chế lớn hơn nhiều, quá trình quét tuyến tính này vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    s = input().strip()

    cost0 = 0
    cost1 = 0

    for i, ch in enumerate(s):
        bit = ord(ch) - 48

        if bit != (i % 2):
            cost0 += 1
        if bit != (1 - i % 2):
            cost1 += 1

    return str(min(cost0, cost1))

assert run("5\n01011\n") == "1", "sample 1"

assert run("1\n0\n") == "0", "already valid single element"

assert run("1\n1\n") == "0", "already valid single element"

assert run("4\n0000\n") == "2", "all equal needs half flips"

assert run("4\n0101\n") == "0", "already alternating start 0"

assert run("4\n1010\n") == "0", "already alternating start 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hộp 1 chiều dài | 0 | độ đúng ranh giới tối thiểu | 
| 0000 | 2 | hành vi không phù hợp thống nhất tồi tệ nhất | 
| 0101 | 0 | nhận dạng chính xác mẫu đầu tiên | 
| 1010 | 0 | nhận dạng chính xác mẫu thứ hai | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là kích thước đầu vào nhỏ nhất n = 1. Đối với đầu vào "0", cả hai mẫu xen kẽ đều là ứng cử viên hợp lệ có độ dài 1, do đó số lượng không khớp lần lượt là 0 và 1, cho câu trả lời 0. Thuật toán xử lý điều này một cách chính xác vì cả hai bộ đếm đều được tính toán độc lập và đạt mức tối thiểu. 

Một trường hợp khác là một chuỗi hoàn toàn đồng nhất như "0000". Thuật toán so sánh nó với cả hai mẫu. So với 0101, nó không khớp ở chỉ số 1 và 3, cho ra chi phí 2. So với 1010, nó không khớp ở chỉ số 0 và 2, cũng cho chi phí 2. Do đó, đầu ra là 2, phù hợp với thực tế là một nửa số vị trí phải được đảo ngược. 

Trường hợp cuối cùng là khi chuỗi đã khớp chính xác với một mẫu, chẳng hạn như "1010". Bộ đếm không khớp cho mẫu đó vẫn bằng 0 trong suốt quá trình quét, trong khi mẫu còn lại tích lũy lỗi. Vì chúng tôi lấy mức tối thiểu nên đầu ra chính xác là 0.
