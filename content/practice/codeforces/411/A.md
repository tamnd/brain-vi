---
title: "CF 411A - Kiểm tra mật khẩu"
description: "Chúng tôi được yêu cầu kiểm tra xem chuỗi mật khẩu có \"đủ phức tạp\" hay không dựa trên bốn tiêu chí. Mật khẩu là một chuỗi gồm tối đa 100 ký tự bao gồm chữ hoa, chữ thường, chữ số và một vài ký tự đặc biệt."
date: "2026-06-07T02:15:35+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 411
codeforces_index: "A"
codeforces_contest_name: "Coder-Strike 2014 - Qualification Round"
rating: 800
weight: 411
solve_time_s: 254
verified: true
draft: false
---

[CF 411A - Kiểm tra mật khẩu](https://codeforces.com/problemset/problem/411/A) 

**Đánh giá:** 800 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 4 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu kiểm tra xem chuỗi mật khẩu có "đủ phức tạp" hay không dựa trên bốn tiêu chí. Mật khẩu là một chuỗi gồm tối đa 100 ký tự bao gồm chữ hoa, chữ thường, chữ số và một vài ký tự đặc biệt. Mật khẩu được chấp nhận nếu nó dài ít nhất năm ký tự, chứa ít nhất một chữ cái viết hoa, ít nhất một chữ cái viết thường và ít nhất một chữ số. 

Đầu vào bao gồm một dòng duy nhất có mật khẩu. Đầu ra là một thông báo duy nhất: "Chính xác" nếu mật khẩu đáp ứng tất cả các yêu cầu và nếu không thì "Quá yếu". 

Những hạn chế là rất nhỏ. Tối đa 100 ký tự có nghĩa là chúng tôi có thể đủ khả năng kiểm tra từng ký tự riêng lẻ nhiều lần mà không phải lo lắng về hiệu suất. Điều này loại trừ bất kỳ mối lo ngại nào về việc tối ưu hóa các lần quét tuyến tính phụ. 

Sự tinh tế nằm ở việc kiểm tra cẩn thận cả bốn điều kiện. Một cách tiếp cận ngây thơ chỉ kiểm tra độ dài hoặc chỉ kiểm tra các chữ cái mà không xem xét các chữ số có thể tạo ra kết quả sai. Ví dụ, đầu vào`Abc!`quá ngắn, mặc dù nó chứa cả chữ hoa và ký tự đặc biệt. đầu vào`abcdef`dài và chứa các chữ cái viết thường nhưng thiếu chữ cái và chữ số viết hoa nên sẽ bị từ chối. Điều này cho thấy việc tuân thủ một phần là chưa đủ và phải xác minh cả bốn điều kiện. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: chúng tôi kiểm tra từng điều kiện một cách độc lập. Đầu tiên, chúng tôi đo độ dài mật khẩu. Tiếp theo, chúng tôi quét chuỗi để xem liệu nó có chứa ít nhất một chữ cái viết hoa, ít nhất một chữ cái viết thường và ít nhất một chữ số hay không. Điều này có thể được thực hiện bằng bốn lần quét riêng biệt trên chuỗi hoặc bằng một lần quét duy nhất theo dõi cả bốn cờ. Vì độ dài mật khẩu được giới hạn ở mức 100 nên bốn lần quét sẽ thực hiện kiểm tra tối đa 400 ký tự, con số này không đáng kể. 

Thông tin chi tiết quan trọng giúp đơn giản hóa việc triển khai là chúng ta có thể kiểm tra tất cả các yêu cầu về loại ký tự chỉ trong một lần chuyển. Chúng tôi duy trì ba cờ boolean, một cho chữ hoa, một cho chữ thường và một cho chữ số. Khi lặp qua chuỗi, chúng tôi sẽ cập nhật các cờ này cho phù hợp. Nếu tất cả các cờ được đặt ở cuối thẻ và độ dài đủ thì mật khẩu hợp lệ. Điều này tránh được nhiều lần lặp lại và đơn giản hơn về mặt khái niệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (nhiều đường chuyền) | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu (đạt một lần) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi mật khẩu. Điều đầu tiên cần kiểm tra là chiều dài. Nếu nó nhỏ hơn 5 ký tự, hãy trả về ngay "Quá yếu" vì không có bước kiểm tra nào thêm có thể khiến mật khẩu hợp lệ. 
2. Khởi tạo ba biến boolean:`has_upper`,`has_lower`, Và`has_digit`. Chúng sẽ theo dõi xem mật khẩu có chứa ít nhất một ký tự của từng loại được yêu cầu hay không. 
3. Lặp lại từng ký tự của mật khẩu. Nếu ký tự là chữ in hoa, hãy đặt`has_upper`ĐẾN`True`. Nếu là chữ thường thì đặt`has_lower`ĐẾN`True`. Nếu là chữ số thì đặt`has_digit`ĐẾN`True`. 
4. Sau vòng lặp, kiểm tra tất cả bốn điều kiện: độ dài,`has_upper`,`has_lower`, Và`has_digit`. Nếu tất cả đều hài lòng, hãy in "Đúng". Nếu không thì in ra "Quá yếu". 

Tại sao nó hoạt động: Thuật toán duy trì ba cờ boolean thể hiện sự hiện diện của các loại ký tự bắt buộc. Việc lặp qua chuỗi đảm bảo rằng mỗi ký tự được đánh giá chính xác một lần. Bằng cách kết hợp điều này với việc kiểm tra độ dài, chúng tôi đảm bảo rằng không có mật khẩu nào có thể bị chấp nhận sai và tất cả mật khẩu hợp lệ đều được nhận dạng chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

password = input().strip()

if len(password) < 5:
    print("Too weak")
else:
    has_upper = False
    has_lower = False
    has_digit = False
    
    for ch in password:
        if ch.isupper():
            has_upper = True
        elif ch.islower():
            has_lower = True
        elif ch.isdigit():
            has_digit = True
    
    if has_upper and has_lower and has_digit:
        print("Correct")
    else:
        print("Too weak")
```Giải pháp bắt đầu bằng cách cắt bớt khoảng trắng để xử lý mọi khoảng trắng ở đầu hoặc cuối vô tình. Độ dài được kiểm tra trước tiên để nhanh chóng từ chối các mật khẩu rõ ràng là ngắn. Ba lá cờ theo dõi các loại nhân vật. sử dụng`isupper()`,`islower()`, Và`isdigit()`tránh so sánh thủ công các phạm vi ký tự, điều này an toàn hơn và dễ đọc hơn. Cuối cùng, kết quả được xác định bằng cách đánh giá tất cả bốn điều kiện trong một biểu thức logic duy nhất. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**:`abacaba`| Nhân vật | has_upper | has_low | has_digit | 
| --- | --- | --- | --- | 
| một | Sai | Đúng | Sai | 
| b | Sai | Đúng | Sai | 
| một | Sai | Đúng | Sai | 
| c | Sai | Đúng | Sai | 
| một | Sai | Đúng | Sai | 
| b | Sai | Đúng | Sai | 
| một | Sai | Đúng | Sai | 

Độ dài = 7, has_upper = Sai, has_low = Đúng, has_digit = Sai → Đầu ra: Quá yếu 

**Đầu vào tùy chỉnh 2**:`A1b2C`| Nhân vật | has_upper | has_low | has_digit | 
| --- | --- | --- | --- | 
| A | Đúng | Sai | Sai | 
| 1 | Đúng | Sai | Đúng | 
| b | Đúng | Đúng | Đúng | 
| 2 | Đúng | Đúng | Đúng | 
| C | Đúng | Đúng | Đúng | 

Độ dài = 5, tất cả các cờ Đúng → Đầu ra: Đúng 

Các bảng cho thấy thuật toán theo dõi chính xác tất cả các cờ bắt buộc trong suốt quá trình lặp và chỉ chấp nhận mật khẩu đáp ứng mọi điều kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền một lần qua chuỗi có độ dài n (<100) | 
| Không gian | O(1) | Chỉ sử dụng ba cờ boolean, không có cấu trúc bổ sung | 

Độ phức tạp thời gian là tuyến tính, nhưng vì n ≤ 100 nên điều này thực tế là không đổi. Việc sử dụng không gian là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    password = input().strip()
    if len(password) < 5:
        print("Too weak")
    else:
        has_upper = has_lower = has_digit = False
        for ch in password:
            if ch.isupper():
                has_upper = True
            elif ch.islower():
                has_lower = True
            elif ch.isdigit():
                has_digit = True
        if has_upper and has_lower and has_digit:
            print("Correct")
        else:
            print("Too weak")
    return output.getvalue().strip()

# Provided sample
assert run("abacaba") == "Too weak", "sample 1"

# Custom cases
assert run("A1b2C") == "Correct", "mixed chars, exact length"
assert run("abc") == "Too weak", "too short"
assert run("ABCDE") == "Too weak", "uppercase only"
assert run("abcde123") == "Too weak", "missing uppercase"
assert run("Ab1!") == "Too weak", "exact 4 chars, fails length"
assert run("Abcdef1") == "Correct", "long enough, meets all"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| A1b2C | Đúng | Tất cả các tiêu chí đều được đáp ứng, độ dài tối thiểu 5 | 
| abc | Quá yếu | Độ dài ngắn, thiếu chữ hoa và chữ số | 
| ABCDE | Quá yếu | Chỉ viết hoa, thiếu chữ thường và chữ số | 
| abcde123 | Quá yếu | Thiếu chữ hoa | 
| Ab1! | Quá yếu | Độ dài < 5 | 
| Abcdef1 | Đúng | Mật khẩu dài hơn, đáp ứng đủ điều kiện | 

## Vỏ cạnh 

Các trường hợp biên chủ yếu liên quan đến độ dài đường biên và sự kết hợp của các loại ký tự. Vì`Ab1!`, trước tiên thuật toán sẽ kiểm tra độ dài, ngay lập tức loại bỏ nó mà không cần kiểm tra các loại ký tự, thể hiện đánh giá ngắn mạch. Đối với một mật khẩu như`ABCDE`, thuật toán theo dõi chữ hoa một cách chính xác nhưng nhận thấy cờ chữ thường và chữ số vẫn sai, đảm bảo nó xuất ra "Quá yếu". Điều này xác nhận rằng phương pháp này xử lý cả ranh giới độ dài và loại ký tự mà không có lỗi riêng lẻ hoặc thiếu điều kiện.
