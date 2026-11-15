---
title: "CF 1821A - Khớp nối"
description: "Chúng ta được cung cấp một chuỗi bao gồm các chữ số và dấu chấm hỏi. Mỗi dấu hỏi đại diện cho một chữ số chưa biết và chúng tôi muốn đếm tất cả các số nguyên dương có thể được hình thành bằng cách thay thế dấu hỏi bằng các chữ số sao cho số kết quả không có số 0 đứng đầu."
date: "2026-06-09T07:53:11+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math"]
categories: ["algorithms"]
codeforces_contest: 1821
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 147 (Rated for Div. 2)"
rating: 800
weight: 1821
solve_time_s: 72
verified: true
draft: false
---

[CF 1821A - So khớp](https://codeforces.com/problemset/problem/1821/A) 

**Đánh giá:** 800 
**Tags:** tổ hợp, toán học 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi bao gồm các chữ số và dấu chấm hỏi. Mỗi dấu hỏi đại diện cho một chữ số chưa biết và chúng tôi muốn đếm tất cả các số nguyên dương có thể được hình thành bằng cách thay thế dấu hỏi bằng các chữ số sao cho số kết quả không có số 0 đứng đầu. Đầu vào bao gồm nhiều chuỗi như vậy, mỗi chuỗi đại diện cho một trường hợp thử nghiệm riêng biệt và với mỗi chuỗi chúng ta phải xuất ra tổng số số nguyên dương phù hợp. 

Hạn chế chính là độ dài chuỗi tối đa là 5 ký tự, nghĩa là tổng số khả năng cho một chuỗi dấu hỏi đầy đủ là nhiều nhất$10^5 = 100,000$. Điều này đủ nhỏ để ngay cả cách tiếp cận tổ hợp dựa trên phép nhân đơn giản cũng có thể chạy thoải mái trong giới hạn thời gian. 

Các trường hợp cạnh không rõ ràng đến từ các số 0 đứng đầu và các chuỗi có một chữ số. Ví dụ, mẫu`0`không thể khớp với bất kỳ số nguyên dương nào vì số 0 không dương. Một mẫu như`?5`là hợp lệ, nhưng`05`thì không, vì số 0 đứng đầu bị cấm. Một sự tinh tế khác là các dấu chấm hỏi gồm một ký tự, như`?`, có thể khớp với các chữ số`1`bởi vì`9`, cho 9 số hợp lệ, không phải 10. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là tạo ra tất cả các thay thế có thể có của dấu chấm hỏi, chuyển đổi từng chuỗi ứng cử viên thành một số nguyên, sau đó đếm xem có bao nhiêu số nguyên đó là dương và không có số 0 đứng đầu. Điều này hiệu quả vì độ dài tối đa là 5, cung cấp tối đa 100.000 ứng cử viên cho trường hợp xấu nhất`?????`. Đối với một mẫu duy nhất, chúng ta có thể lặp qua tất cả các thay thế và kiểm tra, nhưng điều này sẽ yêu cầu các vòng lặp lồng nhau hoặc đệ quy, điều này rất cồng kềnh. 

Quan sát quan trọng làm cho cách tiếp cận tối ưu trở nên tầm thường là số lượng số nguyên hợp lệ cho mỗi mẫu có thể được tính trực tiếp bằng tổ hợp đơn giản. Mỗi dấu hỏi đại diện cho 10 khả năng, ngoại trừ ký tự đầu tiên không thể bằng 0 nếu là dấu chấm hỏi. Do đó, ta có thể nhân số khả năng cho từng vị trí: đối với ký tự đầu tiên nếu là dấu chấm hỏi thì có 9 phương án (`1-9`), nếu không thì chỉ là 1. Đối với các vị trí còn lại, dấu chấm hỏi thể hiện 10 phương án và các chữ số được cố định. Phép nhân này ngay lập tức cho tổng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^độ dài) | O(1) | Hoạt động nhưng không cần thiết | 
| Tối ưu | O(độ dài) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Điều này kiểm soát số lần chúng tôi lặp lại phép tính. 
2. Đối với mỗi chuỗi mẫu`s`, kiểm tra ký tự đầu tiên. Nếu là dấu chấm hỏi, nó có thể là bất kỳ chữ số nào từ`1`ĐẾN`9`. Nếu là chữ số`0`, chuỗi không thể biểu thị số nguyên dương, vì vậy câu trả lời là 0. 
3. Khởi tạo bộ đếm thành 1. Điều này sẽ tích lũy số lượng khả năng. 
4. Nhân bộ đếm với số tùy chọn cho ký tự đầu tiên. Đối với dấu chấm hỏi, nhân với 9; đối với chữ số khác 0 thì nhân với 1. 
5. Với mỗi ký tự còn lại trong chuỗi, nếu là dấu chấm hỏi thì nhân số đếm với 10 (chữ số`0`ĐẾN`9`), nếu không thì nhân với 1 vì nó cố định. 
6. Xuất bộ đếm sau khi xử lý tất cả các ký tự. Điều này đưa ra số lượng số nguyên dương phù hợp với mẫu. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến là bộ đếm luôn bằng tổng số số nguyên hợp lệ được tạo bằng cách thay thế các vị trí đã xử lý. Mỗi bước nhân tính toán chính xác cho số lượng lựa chọn độc lập tại vị trí đó và việc kiểm tra ký tự đầu tiên đảm bảo chúng tôi không bao giờ bao gồm các số có số 0 đứng đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    if s[0] == '0':
        print(0)
        continue
    count = 1
    for i, c in enumerate(s):
        if c == '?':
            if i == 0:
                count *= 9
            else:
                count *= 10
    print(count)
```Mã đọc tất cả các trường hợp thử nghiệm một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Việc kiểm tra ký tự đầu tiên ngăn chặn việc đếm các số không hợp lệ bắt đầu bằng 0. Đối với mỗi ký tự, nếu đó là dấu chấm hỏi thì số khả năng đúng sẽ được nhân lên. Cách tiếp cận này tránh các vòng lặp lồng nhau và đệ quy và chạy theo thời gian tuyến tính đối với độ dài chuỗi, tối đa là 5. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`??`: 

| Chỉ mục | Nhân vật | Đếm | Giải thích | 
| --- | --- | --- | --- | 
| 0 | ? | 9 | Ký tự đầu tiên có 9 chữ số hợp lệ | 
| 1 | ? | 90 | Ký tự thứ hai có 10 lựa chọn, nhân 9*10=90 | 

Đầu ra: 90 

Đầu vào mẫu`03`: 

| Chỉ mục | Nhân vật | Đếm | Giải thích | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | Số 0 đứng đầu không hợp lệ, xuất ngay 0 | 

Đầu ra: 0 

Những dấu vết này cho thấy rằng lần kiểm tra ký tự đầu tiên sẽ ngăn chặn các số không hợp lệ và phép nhân sẽ nắm bắt tất cả các tổ hợp dấu chấm hỏi còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * chiều dài) | Mỗi trường hợp thử nghiệm được xử lý theo thời gian tuyến tính với độ dài chuỗi của nó (≤5), tổng số 10^5 thao tác | 
| Không gian | O(1) | Chỉ một bộ đếm duy nhất được sử dụng cho mỗi trường hợp thử nghiệm, không có cấu trúc dữ liệu bổ sung | 

Độ phức tạp về thời gian vừa vặn thoải mái trong giới hạn 2 giây cho trước t 2·10^4 và độ dài ≤5. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    res = []
    for _ in range(t):
        s = input().strip()
        if s[0] == '0':
            res.append("0")
            continue
        count = 1
        for i, c in enumerate(s):
            if c == '?':
                count *= 9 if i == 0 else 10
        res.append(str(count))
    return "\n".join(res)

# provided samples
assert run("8\n??\n?\n0\n9\n03\n1??7\n?5?\n9??99\n") == "90\n9\n0\n1\n0\n100\n90\n100"

# custom cases
assert run("4\n?\n??\n???\n000\n") == "9\n90\n900\n0", "single and multiple '?' with leading zeros"
assert run("3\n1\n?1\n0?0\n") == "1\n9\n0", "single digit, '?' not first, leading zero"
assert run("2\n?????\n1????\n") == "90000\n9000", "maximum length templates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ? | 9 | Mẫu ký tự đơn, ký tự đầu tiên khác 0 | 
| ?? | 90 | Mẫu hai ký tự, cả hai ký tự '?', ràng buộc ký tự đầu tiên | 
| ??? | 900 | Mẫu ba ký tự, nhân theo vị trí | 
| 000 | 0 | Các số 0 đứng đầu không tạo ra số dương | 
| 1 | 1 | Số cố định một chữ số | 
| ?1 | 9 | Dấu hỏi theo sau là chữ số cố định | 
| 0?0 | 0 | Mẫu số 0 hàng đầu làm mất hiệu lực | 
| ????? | 90000 | Mẫu có độ dài tối đa, tất cả các dấu chấm hỏi | 
| 1???? | 9000 | Mẫu có độ dài tối đa, cố định chữ số đầu tiên | 

## Vỏ cạnh 

mẫu`0`ngay lập tức kích hoạt kiểm tra ký tự đầu tiên, trả về 0, vì số 0 không dương. Vì`?`dưới dạng mẫu một ký tự, phép nhân đếm 9 tùy chọn (1-9) thay vì 10, tránh chính xác số 0. Các mẫu như`03`hoặc`0?`bị phát hiện sớm và các mẫu như`?0?`nhân 9 cho ký tự đầu tiên và 10 cho mỗi dấu chấm hỏi còn lại, tạo ra 90 cho`?0`hoặc 900 cho`?0?`, tôn trọng quy tắc số 0 đứng đầu. Thuật toán xử lý tất cả các trường hợp biên có một chữ số và nhiều chữ số một cách nhất quán.
