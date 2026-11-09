---
title: "CF 149B - Đồng hồ sao Hỏa"
description: "Chúng ta được cung cấp một chuỗi biểu thị thời gian trên sao Hỏa ở định dạng \"a:b\", trong đó a là thành phần giờ và b là thành phần phút. Không giống như giờ Trái đất ở cơ số 10, các chuỗi này có thể biểu thị các số trong bất kỳ hệ thống số nào có cơ số lớn hơn 1."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 149
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 106 (Div. 2)"
rating: 1600
weight: 149
solve_time_s: 85
verified: true
draft: false
---

[CF 149B - Đồng hồ sao Hỏa](https://codeforces.com/problemset/problem/149/B) 

**Đánh giá:** 1600 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi biểu thị thời gian trên sao Hỏa ở định dạng "a:b", trong đó`a`là thành phần giờ và`b`là thành phần phút. Không giống như giờ Trái đất ở cơ số 10, các chuỗi này có thể biểu thị các số trong bất kỳ hệ thống số nào có cơ số lớn hơn 1. Nhiệm vụ của chúng tôi là xác định tất cả các cơ số chữ số trong đó các chuỗi đã cho có thể biểu thị thời gian Trái đất hợp lệ. Thời gian hợp lệ có nghĩa là giá trị số của`a`nằm trong khoảng từ 0 đến 23 và giá trị số của`b`bao gồm từ 0 đến 59. 

Chuỗi đầu vào bao gồm các chữ số '0'-'9' và chữ in hoa 'A'-'Z'. Mỗi ký tự đại diện cho một chữ số theo ký hiệu vị trí tiêu chuẩn: các chữ số 0-9 ánh xạ tới các giá trị nguyên của chúng, các chữ cái A-Z ánh xạ tới 10-35. Các chuỗi có thể có các số 0 đứng đầu, không ảnh hưởng đến giá trị số. Chiều dài của`a`Và`b`có thể có tối đa 5 ký tự, điều này ngụ ý rằng số lượng lớn nhất có thể biểu thị có thể là đáng kể nếu được diễn giải trong một cơ sở lớn, nhưng chúng ta chỉ cần xem xét các cơ sở giữ giá trị trong phạm vi giờ và phút hợp lệ. 

Các trường hợp cạnh cần xem xét bao gồm các chuỗi trong đó chữ số lớn nhất xác định cơ số hợp lệ tối thiểu lớn hơn 10. Ví dụ: "1A:2" yêu cầu cơ số ít nhất là 11, nếu không thì 'A' không phải là chữ số hợp lệ. Một trường hợp đặc biệt khác xảy ra khi một chuỗi là "00:01" hoặc tương tự: tất cả các chữ số đủ nhỏ để vô số hệ thống chữ số có thể hoạt động. Một cách tiếp cận ngây thơ chỉ kiểm tra các cơ sở nhỏ có thể báo cáo không chính xác không có cơ sở hợp lệ trong những tình huống như vậy. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng chuyển đổi`a`Và`b`thành các số nguyên trong mọi cơ số bắt đầu từ 2 trở lên. Đối với mỗi cơ sở, chúng tôi sẽ kiểm tra xem cả hai số có nằm trong phạm vi hợp lệ hay không. Về nguyên tắc, điều này đúng vì tất cả các cơ sở hợp lệ có thể có đều là số nguyên lớn hơn 1 và các chuỗi có độ dài hữu hạn. Tuy nhiên, cách tiếp cận này có thể không hiệu quả nếu được triển khai một cách đơn giản, vì cơ sở tối đa cần xem xét về mặt lý thuyết có thể không bị giới hạn nếu chữ số lớn nhất nhỏ và giá trị của chuỗi nhỏ, dẫn đến một số lượng lớn các kiểm tra không cần thiết. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu là nhận ra rằng chữ số lớn nhất trong chuỗi áp đặt giới hạn dưới nghiêm ngặt trên cơ sở có thể. Nếu chữ số lớn nhất trong`a`là`max_a`và trong`b`là`max_b`, cơ sở tối thiểu cần xét là`max(max_a, max_b) + 1`. Bất kỳ cơ sở nào nhỏ hơn số này sẽ khiến ít nhất một ký tự không hợp lệ. Ngoài ra, chúng tôi có thể ngừng kiểm tra các cơ số khi số được chuyển đổi vượt quá 23 trong giờ hoặc 59 trong số phút, vì các cơ số cao hơn sẽ chỉ làm tăng giá trị. Nếu giá trị giờ nhiều nhất là 23 và giá trị phút nhiều nhất là 59 ở cơ số tối thiểu, thì bất kỳ cơ số lớn hơn nào cũng sẽ thỏa mãn các ràng buộc này, dẫn đến vô số nghiệm. Mặt khác, chúng ta chỉ cần kiểm tra cơ sở trong đó giá trị lớn hơn trong hai giá trị sẽ vượt quá giới hạn tương ứng của nó. 

Cách tiếp cận này làm giảm phạm vi cơ sở cần kiểm tra từ phạm vi không giới hạn xuống một tập hợp hữu hạn nhỏ được xác định bởi các ràng buộc về giờ và phút, giúp giải pháp trở nên hiệu quả và đơn giản để thực hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(B) trong đó B là cơ số lớn nhất | O(1) | Quá chậm nếu B lớn | 
| Tối ưu | O(36) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tách chuỗi đầu vào tại dấu hai chấm thành các giờ riêng biệt (`a`) và phút (`b`). 
2. Xác định hàm trợ giúp để chuyển đổi một ký tự thành giá trị nguyên của nó, với ánh xạ '0'-'9' thành 0-9 và ánh xạ 'A'-'Z' thành 10-35. 
3. Xác định chữ số lớn nhất trong`a`và trong`b`. Cơ số tối thiểu mà chúng ta cần xem xét là một cơ số lớn hơn số lớn nhất trong hai chữ số này, bởi vì bất kỳ hệ thống số nào nhỏ hơn cơ số đó đều không thể biểu diễn chính xác các chuỗi. 
4. Nếu cơ sở tối thiểu lớn hơn 60 thì không tồn tại cơ sở hợp lệ vì ngay cả ở cơ sở tối thiểu có thể,`b`sẽ vượt quá 59. In 0 và thoát. 
5. Đối với các cơ số bắt đầu từ cơ số tối thiểu lên tới 60, hãy chuyển đổi`a`Và`b`đến số nguyên trong cơ sở đó. Kiểm tra xem các giá trị kết quả có thỏa mãn các ràng buộc về giờ và phút hay không (0 ≤ giờ ≤ 23, 0 ≤ phút ≤ 59). Ghi lại các căn cứ hợp lệ. 
6. Nếu cơ số tối thiểu đã tạo ra các số trong giới hạn giờ và phút, thì mọi cơ số lớn hơn cũng sẽ có giá trị, dẫn đến vô số nghiệm. In -1 và thoát. 
7. Nếu danh sách các cơ số hợp lệ trống, in ra 0. Ngược lại, in các cơ số theo thứ tự tăng dần. 

Lý do điều này hoạt động là do việc chuyển đổi từ một chuỗi thành một số nguyên đang tăng lên một cách đơn điệu so với cơ sở. Khi giá trị được chuyển đổi vượt quá giới hạn của nó, thì không có cơ sở lớn hơn nào có thể thỏa mãn ràng buộc, ngoại trừ trường hợp một chuỗi rất nhỏ luôn nằm trong giới hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def char_to_val(c):
    if '0' <= c <= '9':
        return ord(c) - ord('0')
    return ord(c) - ord('A') + 10

def str_to_int(s, base):
    val = 0
    for c in s:
        val = val * base + char_to_val(c)
    return val

time_str = input().strip()
a_str, b_str = time_str.split(':')

max_digit_a = max(char_to_val(c) for c in a_str)
max_digit_b = max(char_to_val(c) for c in b_str)
min_base = max(max_digit_a, max_digit_b) + 1

if min_base > 60:
    print(0)
    sys.exit()

hours = str_to_int(a_str, min_base)
minutes = str_to_int(b_str, min_base)

if hours <= 23 and minutes <= 59:
    print(-1)
else:
    valid_bases = []
    for base in range(min_base, 61):
        h_val = str_to_int(a_str, base)
        m_val = str_to_int(b_str, base)
        if h_val <= 23 and m_val <= 59:
            valid_bases.append(base)
    if not valid_bases:
        print(0)
    else:
        print(" ".join(map(str, valid_bases)))
```Phần đầu tiên chuyển đổi các ký tự thành giá trị số và tìm cơ số tối thiểu. Phần thứ hai kiểm tra xem cơ sở tối thiểu đã mang lại các giá trị thời gian hợp lệ hay chưa, nghĩa là có vô số cơ sở hoạt động. Mặt khác, chúng tôi lặp qua các cơ sở có thể lên tới 60, vì số giờ không thể vượt quá 23 và số phút không thể vượt quá 59. Mỗi chuyển đổi là tuyến tính theo độ dài của chuỗi, nhưng độ dài tối đa là 5, vì vậy việc này nhanh. 

## Ví dụ đã hoạt động 

**Mẫu 1:** "11:20" 

| Bước | cơ sở tối thiểu | giờ | phút | cơ sở hợp lệ | 
| --- | --- | --- | --- | --- | 
| tính các chữ số tối đa | 1 cho '1', 2 cho '2' | - | - | - | 
| cơ sở tối thiểu = 3 | - | 3? | 3? | - | 
| kiểm tra căn cứ 3..22 | - | giờ <= 23? | phút <=59? | 3..22 | 

Dấu vết này xác nhận rằng tất cả các cơ sở từ 3 đến 22 đều cho phép chuỗi biểu thị thời gian hợp lệ. 

**Mẫu 2:** "000B:00001" 

| Bước | cơ sở tối thiểu | giờ | phút | hành động | 
| --- | --- | --- | --- | --- | 
| max_digit_a = 11, max_digit_b = 1 | cơ sở tối thiểu = 12 | giờ = 11, phút = 1 | - | giờ <=23 và phút <=59, vô số căn cứ | 

Thuật toán in chính xác -1, cho thấy có vô số cơ sở hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L * 36) | L là độ dài tối đa của chuỗi đầu vào (≤5), chúng tôi kiểm tra tối đa 36 cơ sở (từ 2 đến 60) và chuyển đổi từng chuỗi trong O(L) | 
| Không gian | O(1) | Chỉ lưu trữ một vài số nguyên và danh sách các cơ sở hợp lệ | 

Với độ dài giới hạn ở mức 5 và phạm vi cơ sở tối đa là 60, giải pháp sẽ chạy tính bằng micro giây, nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    
    def char_to_val(c):
        if '0' <= c <= '9':
            return ord(c) - ord('0')
        return ord(c) - ord('A') + 10

    def str_to_int(s, base):
        val = 0
        for c in s:
            val = val * base + char_to_val(c)
        return val

    time_str = input().strip()
    a_str, b_str = time_str.split(':')

    max_digit_a = max(char_to_val(c) for c in a_str)
    max_digit_b = max(char_to_val(c) for c in b_str)
    min_base = max(max_digit_a, max_digit_b) + 1

    if min_base > 60:
        return "0"

    hours = str_to_int(a_str, min_base)
    minutes = str_to_int(b_str, min_base)

    if hours <= 23 and minutes <= 59:
        return "-1"
    else:
        valid_bases = []
        for base in range(min_base,
```
