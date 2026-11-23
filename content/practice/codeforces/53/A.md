---
title: "CF 53A - Tự động hoàn thành"
description: "Chúng tôi được yêu cầu triển khai chức năng tự động hoàn thành được đơn giản hóa. Đầu vào bao gồm một chuỗi s, đại diện cho văn bản mà người dùng đã nhập cho đến nay, theo sau là danh sách các trang đã truy cập trước đó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 53
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 49 (Div. 2)"
rating: 1100
weight: 53
solve_time_s: 208
verified: true
draft: false
---

[CF 53A - Tự động hoàn thành](https://codeforces.com/problemset/problem/53/A) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 3 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu triển khai chức năng tự động hoàn thành được đơn giản hóa. Đầu vào bao gồm một chuỗi`s`, đại diện cho văn bản người dùng đã nhập cho đến nay, theo sau là danh sách các trang đã truy cập trước đó. Mục tiêu là mở rộng`s`vào một URL trang hoàn chỉnh từ danh sách đã truy cập, chọn tùy chọn nhỏ nhất về mặt từ điển nếu nhiều trang bắt đầu bằng`s`. Nếu không có trang nào bắt đầu bằng`s`, chúng tôi trở lại`s`không thay đổi. 

Giới hạn đầu vào nhỏ: tối đa 100 trang được truy cập, mỗi trang có độ dài tối đa 100. Điều này ngụ ý rằng ngay cả tiền tố so sánh quét tuyến tính đơn giản cũng sẽ chạy thoải mái trong giới hạn thời gian, vì tổng số so sánh ký tự trong trường hợp xấu nhất là khoảng 100 * 100 = 10.000, không đáng kể trong giới hạn thời gian 2 giây. 

Các trường hợp khó khăn không rõ ràng bao gồm tình huống trong đó`s`đã bằng một số trang trong danh sách hoặc khi nhiều trang có`s`làm tiền tố và chỉ có một là nhỏ nhất về mặt từ điển. Ví dụ, nếu`s = "a"`và các trang đã truy cập là`["apple", "apricot", "banana"]`, đầu ra đúng là`"apple"`, không`"apricot"`. Một trường hợp tinh vi khác xảy ra khi không có trang nào bắt đầu bằng`s`, ví dụ.,`s = "xyz"`và các trang là`["abc", "def"]`; đầu ra phải là`"xyz"`. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: duyệt qua tất cả các trang đã truy cập và kiểm tra xem mỗi trang có bắt đầu bằng`s`. Thu thập tất cả các kết quả phù hợp và chọn kết quả nhỏ nhất theo từ điển. Điều này hoạt động vì số lượng trang nhỏ. Việc kiểm tra tiền tố có thể được thực hiện bằng cách sử dụng tính năng tích hợp sẵn của Python`startswith`phương pháp. Trong trường hợp xấu nhất, chúng tôi so sánh từng ký tự của mỗi trang với`s`, dẫn đến khoảng`n * len(page)`so sánh, có thể chấp nhận được ở đây. 

Cách tiếp cận tối ưu dựa trên cùng một lần quét tuyến tính nhưng tránh việc lưu trữ không cần thiết. Thay vì thu thập tất cả các trang phù hợp, chúng tôi duy trì một biến số cho ứng cử viên tốt nhất hiện tại. Đối với mỗi trang bắt đầu bằng`s`, chúng tôi cập nhật ứng viên này nếu nó nhỏ hơn về mặt từ điển so với ứng viên trước đó. Điều này làm giảm độ phức tạp của không gian và đơn giản hóa logic. Các ràng buộc của vấn đề đủ nhỏ để không cần đến các cấu trúc dữ liệu nâng cao như các lần thử. 

Lực lượng vũ phu hoạt động vì việc lặp qua tất cả các trang đảm bảo rằng chúng tôi sẽ tìm thấy mọi ứng cử viên. Việc tối ưu hóa giúp giảm mức sử dụng bộ nhớ và cải thiện độ rõ ràng, nhưng độ phức tạp về thời gian cơ bản vẫn tuyến tính trong tổng kích thước của tất cả các trang. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n * m) | Đã chấp nhận | 
| Quét tuyến tính được tối ưu hóa | O(n * m) | O(1) | Đã chấp nhận | 

Đây`n`là số trang và`m`là độ dài trung bình của một trang. 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào`s`và số lượng trang đã truy cập`n`. Việc đọc dữ liệu đầu vào trước tiên đảm bảo chúng ta biết tiền tố nào phù hợp và có bao nhiêu ứng cử viên tồn tại. 
2. Khởi tạo một biến`best_candidate`BẰNG`None`. Điều này sẽ giữ trang nhỏ nhất về mặt từ điển hiện tại bắt đầu bằng`s`. 
3. Lặp lại danh sách`n`trang. Đối với mỗi trang, hãy kiểm tra xem nó có bắt đầu bằng`s`sử dụng`startswith`phương pháp. Bước này xác định các ứng cử viên hợp lệ để tự động hoàn thành. 
4. Nếu một trang bắt đầu bằng`s`Và`best_candidate`là`None`, chỉ định trang này cho`best_candidate`. Điều này đảm bảo chúng tôi luôn có ít nhất một ứng cử viên nếu có trang nào phù hợp. 
5. Nếu một trang bắt đầu bằng`s`Và`best_candidate`không phải`None`, so sánh trang với`best_candidate`về mặt từ điển. Nếu nó nhỏ hơn, hãy cập nhật`best_candidate`. Bước này duy trì tính bất biến`best_candidate`luôn là trang phù hợp nhỏ nhất cho đến nay. 
6. Sau khi duyệt qua tất cả các trang, hãy kiểm tra xem`best_candidate`vẫn còn`None`. Nếu vậy, không có trang nào khớp và chúng tôi in`s`không thay đổi. Ngược lại, in`best_candidate`. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến tại bất kỳ thời điểm nào,`best_candidate`là trang nhỏ nhất về mặt từ điển gặp phải và bắt đầu bằng`s`. Việc lặp qua tất cả các trang đảm bảo chúng tôi không bỏ sót bất kỳ ứng cử viên nào, vì vậy cuối cùng,`best_candidate`chứa câu trả lời đúng nếu có trang nào khớp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
n = int(input())
best_candidate = None

for _ in range(n):
    page = input().strip()
    if page.startswith(s):
        if best_candidate is None or page < best_candidate:
            best_candidate = page

print(best_candidate if best_candidate is not None else s)
```Mã đầu tiên đọc chuỗi đã gõ`s`và số lượng trang đã truy cập. Mỗi trang được đọc và loại bỏ khoảng trắng. các`startswith`phương pháp xác định kết quả phù hợp một cách hiệu quả. Sự so sánh`page < best_candidate`đảm bảo trang nhỏ nhất về mặt từ điển được chọn. sử dụng`None`đối với ứng cử viên ban đầu, tránh đặt trước nó thành một giá trị có thể so sánh không chính xác với các trang thực tế. 

## Ví dụ đã hoạt động 

Sử dụng mẫu 1: 

| Bước | Trang | Bắt đầu với`s`? | Ứng viên xuất sắc nhất | Lý do | 
| --- | --- | --- | --- | --- | 
| 1 | hoán vị tiếp theo | vâng | hoán vị tiếp theo | Trận đầu tiên | 
| 2 | phần tử tiếp theo | vâng | phần tử tiếp theo | Nhỏ hơn ứng viên trước | 

Đầu ra:`nextelement`. Điều này chứng tỏ thuật toán chọn chính xác kết quả từ điển nhỏ nhất. 

Một ví dụ khác: 

đầu vào:```
abc
3
abcd
abce
abd
```| Bước | Trang | Bắt đầu với`s`? | Ứng viên xuất sắc nhất | Lý do | 
| --- | --- | --- | --- | --- | 
| 1 | abcd | vâng | abcd | Trận đầu tiên | 
| 2 | abce | vâng | abcd | abce > abcd, không thay đổi | 
| 3 | abd | vâng | abcd | abd > abcd, không thay đổi | 

Đầu ra:`abcd`. Thuật toán bảo toàn kết quả khớp tiền tố nhỏ nhất trong suốt quá trình lặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m) | Mỗi trang trong số n trang được kiểm tra từng ký tự cho đến độ dài m để khớp tiền tố | 
| Không gian | O(1) | Chỉ một biến chuỗi duy nhất được duy trì bất kể kích thước đầu vào | 

Cho n ≤ 100 và m ≤ 100, thuật toán thực hiện tối đa 10.000 phép so sánh ký tự, nằm trong giới hạn 2 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    n = int(input())
    best_candidate = None
    for _ in range(n):
        page = input().strip()
        if page.startswith(s):
            if best_candidate is None or page < best_candidate:
                best_candidate = page
    return best_candidate if best_candidate is not None else s

# provided sample
assert run("next\n2\nnextpermutation\nnextelement\n") == "nextelement", "sample 1"

# s matches exactly one page
assert run("abc\n3\nabcd\nabce\nabd\n") == "abcd", "smallest lex first"

# s matches no page
assert run("xyz\n3\nabc\ndef\nghi\n") == "xyz", "no match"

# multiple identical pages
assert run("go\n4\ngo\ngo\ngone\ngood\n") == "go", "multiple identical pages"

# s equals a page exactly
assert run("home\n2\nhome\nhomepage\n") == "home", "exact match"

# minimum input size
assert run("a\n1\na\n") == "a", "single page"

# maximum page length
assert run("longprefix\n2\nlongprefixaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\nlongprefixbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb\n") == "longprefixaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa", "max length comparison"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`abc\n3\nabcd\nabce\nabd\n`|`abcd`| Tiền tố nhỏ nhất về mặt từ điển | 
|`xyz\n3\nabc\ndef\nghi\n`|`xyz`| Không có trường hợp trùng khớp | 
|`go\n4\ngo\ngo\ngone\ngood\n`|`go`| Nhiều trang giống hệt nhau | 
|`home\n2\nhome\nhomepage\n`|`home`| Xử lý khớp chính xác | 
|`a\n1\na\n`|`a`| Đầu vào kích thước tối thiểu | 
| tiền tố dài | lex đầu tiên nhỏ nhất | Xử lý độ dài tối đa | 

## Vỏ cạnh 

Khi nào`s`không khớp với các trang đã truy cập, chẳng hạn như`s = "xyz"`và các trang`["abc", "def"]`, thuật toán không bao giờ cập nhật`best_candidate`, bỏ nó đi`None`. Bản in kiểm tra cuối cùng`s`, xử lý chính xác tình huống này. 

Khi nhiều trang bắt đầu bằng`s`và giống hệt nhau, bộ trận đấu đầu tiên`best_candidate`và các trang giống hệt tiếp theo không thay đổi nó. Vì`s = "go"`và các trang`["go", "go", "gone", "good"]`, thuật toán trả về`"go"`bởi vì nó đã nhỏ nhất về mặt từ điển. 

Khi`s`chính xác bằng một trang, chẳng hạn như`s = "home"`và các trang`["home", "homepage"]`, thuật toán chọn`"home"`chính xác vì nó bắt đầu bằng`s`và nhỏ nhất trong
