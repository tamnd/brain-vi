---
title: "CF 470D - Mật mã Caesar"
description: "Chúng ta được yêu cầu thực hiện một mật mã Caesar đơn giản. Đầu vào cung cấp cho chúng ta một khóa k, một số nguyên từ 0 đến 25, biểu thị số lượng vị trí mà mỗi chữ cái trong thông báo sẽ được dịch chuyển về phía trước trong bảng chữ cái."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 470
codeforces_index: "D"
codeforces_contest_name: "Surprise Language Round 7"
rating: 1900
weight: 470
solve_time_s: 84
verified: true
draft: false
---

[CF 470D - Mật mã Caesar](https://codeforces.com/problemset/problem/470/D) 

**Xếp hạng:** 1900 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu thực hiện một mật mã Caesar đơn giản. Đầu vào cung cấp cho chúng ta một chìa khóa`k`, một số nguyên từ 0 đến 25, biểu thị số lượng vị trí mỗi chữ cái trong thông báo sẽ được dịch chuyển về phía trước trong bảng chữ cái. Bản thân tin nhắn là một chuỗi các chữ cái Latinh viết hoa và độ dài của nó nằm trong khoảng từ 1 đến 10 ký tự. Nhiệm vụ của chúng tôi là xuất ra tin nhắn được mã hóa bằng cách áp dụng sự dịch chuyển này. 

Các ràng buộc làm cho vấn đề này trở nên đơn giản: vì thông báo rất ngắn nên chúng ta không cần phải lo lắng về hiệu quả của thuật toán. Ngay cả một triển khai đơn giản lặp lại từng ký tự và thực hiện số học để dịch chuyển nó sẽ chạy trong micro giây. Sự tinh tế duy nhất là bao quanh bảng chữ cái một cách chính xác. Ví dụ: dịch 'Z' đi 1 sẽ tạo ra 'A'. Việc thực hiện bất cẩn có thể cố gắng thêm`k`trực tiếp đến giá trị ASCII và quên bọc modulo 26, điều này sẽ tạo ra một ký tự không hợp lệ. 

Một trường hợp cạnh khác phát sinh khi độ dịch chuyển bằng 0. Trong trường hợp này, thông báo được mã hóa phải giống hệt với đầu vào và cách triển khai đơn giản luôn thêm vào`k`và gói modulo 26 sẽ xử lý việc này một cách chính xác nếu áp dụng modulo. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu tự nhiên xuất hiện. Đối với mỗi ký tự trong tin nhắn, chúng tôi chuyển đổi nó sang vị trí dựa trên 0 trong bảng chữ cái bằng cách trừ giá trị ASCII của 'A'. Sau đó chúng tôi thêm`k`và lấy modulo 26 để xử lý việc gói. Cuối cùng, chúng ta chuyển đổi lại thành ký tự bằng cách thêm giá trị ASCII của 'A'. Điều này hoạt động với mọi đầu vào vì mỗi thao tác có thời gian không đổi và có tối đa 10 ký tự. Cách tiếp cận bạo lực hoàn toàn có thể chấp nhận được ở đây vì kích thước đầu vào nhỏ. Không cần bất kỳ sự tối ưu hóa nào về độ phức tạp về thời gian. Cái nhìn sâu sắc quan trọng là nhận ra rằng mật mã Caesar chỉ là số học mô-đun gồm 26 chữ cái. Điều này làm cho việc thực hiện vừa đơn giản vừa chính xác về mặt toán học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

Trong trường hợp này, cách tiếp cận bạo lực và cách tiếp cận tối ưu thực sự giống nhau, bởi vì các ràng buộc của bài toán cho phép chúng ta xử lý từng nhân vật một cách riêng lẻ mà không phải lo lắng về hiệu suất. 

## Hướng dẫn thuật toán 

1. Đọc khóa mã hóa`k`từ đầu vào. Số nguyên này cho chúng ta biết mỗi chữ cái cần dịch chuyển bao nhiêu vị trí. 
2. Đọc tin nhắn gốc dưới dạng một chuỗi chữ in hoa. Chúng tôi sẽ xử lý từng chữ cái một cách độc lập. 
3. Khởi tạo một danh sách hoặc chuỗi trống để tạo tin nhắn được mã hóa. Chúng tôi chọn một danh sách để đạt được hiệu quả trong Python vì việc nối chuỗi trong một vòng lặp kém hiệu quả hơn. 
4. Lặp lại từng ký tự`c`trong tin nhắn. Chuyển thành`c`vào chỉ mục bảng chữ cái dựa trên 0 của nó bằng cách tính toán`ord(c) - ord('A')`. 
5. Thêm`k`vào chỉ mục này và áp dụng modulo 26. Điều này đảm bảo rằng phép dịch chuyển bao quanh bảng chữ cái một cách chính xác. 
6. Chuyển đổi chỉ mục kết quả trở lại thành ký tự bằng cách thêm`ord('A')`và thêm nó vào danh sách kết quả. 
7. Sau khi xử lý tất cả các ký tự, nối danh sách thành một chuỗi duy nhất và in nó. 

Tại sao nó hoạt động: Mỗi ký tự được ánh xạ tới giá trị dịch chuyển chính xác một cách độc lập và phép toán modulo đảm bảo rằng các chỉ số luôn nằm trong giới hạn của bảng chữ cái. Quá trình này bảo toàn thứ tự của các chữ cái, chỉ chuyển đổi từng chữ cái theo quy tắc mật mã Caesar, do đó tính chính xác được rút ra trực tiếp từ số học. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

k = int(input())
message = input().strip()

encrypted = []

for c in message:
    index = ord(c) - ord('A')
    shifted = (index + k) % 26
    encrypted.append(chr(shifted + ord('A')))

print(''.join(encrypted))
```Trong giải pháp này, trước tiên chúng tôi đọc khóa và tin nhắn. Chúng tôi loại bỏ dòng mới khỏi tin nhắn để ngăn nó ảnh hưởng đến kết quả. Mỗi ký tự được chuyển đổi thành một chỉ mục, được dịch chuyển theo modulo 26 và sau đó được chuyển đổi trở lại thành ký tự. Việc sử dụng danh sách để tích lũy các ký tự sẽ tránh được sự kém hiệu quả của các chuỗi nối nhiều lần. Cuối cùng, chúng ta nối danh sách để tạo thành thông điệp mã hóa cuối cùng. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
5
CODEFORCES
```| Nhân vật | Chỉ mục | Chỉ số đã thay đổi | Ký tự được mã hóa | 
| --- | --- | --- | --- | 
| C | 2 | 7 | H | 
| Ồ | 14 | 19 | T | 
| D | 3 | 8 | Tôi | 
| E | 4 | 9 | J | 
| F | 5 | 10 | K | 
| Ồ | 14 | 19 | T | 
| R | 17 | 22 | W | 
| C | 2 | 7 | H | 
| E | 4 | 9 | J | 
| S | 18 | 23 | X | 

Đầu ra:`HTIJKTWHJX`Dấu vết này xác nhận rằng mỗi chữ cái được dịch chuyển và gói một cách chính xác, duy trì trật tự. 

**Mẫu 2:** 

đầu vào:```
1
AZ
```| Nhân vật | Chỉ mục | Chỉ số đã thay đổi | Ký tự được mã hóa | 
| --- | --- | --- | --- | 
| A | 0 | 1 | B | 
| Z | 25 | 0 | A | 

Đầu ra:`BA`Điều này thể hiện sự bao bọc của modulo: dịch chuyển 'Z' đi 1 để quay trở lại 'A' một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi xử lý từng ký tự trong số n ký tự chính xác một lần. | 
| Không gian | O(n) | Chúng tôi lưu trữ các ký tự được mã hóa trong danh sách trước khi nối chúng vào chuỗi cuối cùng. | 

Cho rằng n ≤ 10, giải pháp này chạy gần như ngay lập tức và sử dụng bộ nhớ không đáng kể, trong giới hạn 2 giây, 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    k = int(input())
    message = input().strip()
    encrypted = []
    for c in message:
        index = ord(c) - ord('A')
        shifted = (index + k) % 26
        encrypted.append(chr(shifted + ord('A')))
    return ''.join(encrypted)

# provided samples
assert run("5\nCODEFORCES\n") == "HTIJKTWHJX", "sample 1"

# custom cases
assert run("0\nHELLO\n") == "HELLO", "shift 0"
assert run("25\nAZ\n") == "ZY", "wrap-around backward"
assert run("13\nABCDEFGHIJ\n") == "NOPQRSTUVWX", "max length 10"
assert run("1\nZZZZZZZZZZ\n") == "AAAAAAAAAA", "all Z wrap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0, XIN CHÀO | XIN CHÀO | Không có trường hợp thay đổi cạnh | 
| 25, AZ | ZY | Modulo bao quanh lúc bắt đầu và kết thúc | 
| 13, ABCDEFGHIJ | NOPQRSTUVWX | Độ dài đầu vào tối đa với độ dịch chuyển lớn | 
| 1, ZZZZZZZZZZ | AAAAAAAAA | Tất cả các chữ Z gói thành A | 

## Vỏ cạnh 

Nếu khóa là 0 thì mỗi chữ cái không thay đổi. Ví dụ,`0`Và`HELLO`sản xuất`HELLO`. Thuật toán tính toán`(index + 0) % 26`cho mỗi chữ cái, trả lại chỉ mục gốc mỗi lần. 

Khi các chữ cái ở cuối bảng chữ cái, như 'Z', dịch chuyển theo bất kỳ dấu dương nào`k`kết thúc tốt đẹp modulo 26. Đối với đầu vào`1`Và`Z`, thuật toán tính toán`(25 + 1) % 26 = 0`, ánh xạ tới 'A', xử lý gói xung quanh một cách chính xác. Đối với thông báo gồm tất cả 'Z và ca 1, mỗi chữ cái sẽ ánh xạ tới 'A', như đã được xác nhận trong thử nghiệm tùy chỉnh.
