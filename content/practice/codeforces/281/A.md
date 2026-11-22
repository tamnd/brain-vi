---
title: "CF 281A - Viết hoa từ"
description: "Nhiệm vụ là lấy một từ duy nhất bao gồm các chữ cái tiếng Anh và đảm bảo ký tự đầu tiên của nó là chữ hoa. Các chữ cái còn lại phải giữ nguyên chính xác như khi nhập vào. Ví dụ: với đầu vào \"apple\", đầu ra phải là \"Apple\"."
date: "2026-06-05T09:13:28+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 281
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 172 (Div. 2)"
rating: 800
weight: 281
solve_time_s: 283
verified: true
draft: false
---

[CF 281A - Viết hoa từ](https://codeforces.com/problemset/problem/281/A) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 4m 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là lấy một từ duy nhất bao gồm các chữ cái tiếng Anh và đảm bảo ký tự đầu tiên của nó là chữ hoa. Các chữ cái còn lại phải giữ nguyên chính xác như khi nhập vào. Ví dụ: với đầu vào là "quả táo", đầu ra phải là "Apple". Nếu đầu vào đã bắt đầu bằng chữ in hoa, chẳng hạn như "Apple", đầu ra sẽ không thay đổi. Dữ liệu đầu vào có thể kết hợp chữ thường và chữ hoa, chẳng hạn như "ApPLE", trong trường hợp đó chỉ có ký tự đầu tiên cần sửa đổi. 

Các ràng buộc rất đơn giản: từ này không trống và không dài quá 103 ký tự. Đây là kích thước đầu vào rất nhỏ, do đó, bất kỳ thuật toán nào kiểm tra các ký tự một cách tuần tự hoặc thực hiện các thao tác chuỗi thời gian không đổi sẽ chạy tốt trong giới hạn 2 giây. Không cần tối ưu hóa nhiều; ngay cả một cách tiếp cận ngây thơ cũng sẽ hoàn thành trong vài giây. 

Trường hợp cạnh phát sinh khi ký tự đầu tiên đã là chữ hoa, khi từ là một chữ cái hoặc khi ký tự đầu tiên là chữ thường và phần còn lại là sự kết hợp của chữ hoa và chữ thường. Ví dụ: đầu vào của "a" sẽ tạo ra "A". Đầu vào của "Zebra" sẽ vẫn là "Zebra". Nếu việc triển khai bất cẩn chuyển toàn bộ từ thành chữ hoa, nó sẽ chuyển đổi "aPpLe" thành "APPLE" thay vì "APpLe" không chính xác. 

## Phương pháp tiếp cận 

Cách đơn giản nhất để giải quyết vấn đề là kiểm tra ký tự đầu tiên của chuỗi theo cách thủ công. Nếu nó là chữ thường, hãy chuyển nó thành chữ hoa và sau đó nối nó với phần còn lại của chuỗi không thay đổi. Điều này có hiệu quả vì việc lập chỉ mục và nối chuỗi là các hoạt động liên tục trong thời gian có độ dài nhỏ như vậy. Cách tiếp cận mạnh mẽ này vừa chính xác vừa nhanh chóng đối với kích thước đầu vào, chỉ với một số thao tác. 

Một góc nhìn hơi khác là sử dụng các hàm chuỗi tích hợp như của Python`str.capitalize()`. Tuy nhiên,`str.capitalize()`chuyển đổi ký tự đầu tiên thành chữ hoa nhưng cũng buộc tất cả các ký tự khác thành chữ thường, điều này không được chấp nhận ở đây. Điều này chứng tỏ tại sao việc đọc tài liệu cẩn thận lại quan trọng: một chức năng thư viện tiêu chuẩn dường như thực hiện "viết hoa" có thể có tác dụng phụ phá vỡ tính chính xác. 

Do đó, cách tiếp cận tối ưu rất đơn giản: chỉ chuyển đổi ký tự đầu tiên thành chữ hoa và giữ nguyên phần còn lại. Điều này khai thác cấu trúc cố định nhỏ của vấn đề - mỗi lần chúng ta chỉ quan tâm đến một ký tự và phần còn lại của chuỗi không cần kiểm tra hoặc sửa đổi. Không cần lặp lại toàn bộ chuỗi, ngoài việc cắt lát. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra và sửa đổi ký tự đầu tiên theo cách thủ công) | O(1) | O(1) | Đã chấp nhận | 
| sử dụng`str.capitalize()`| O(n) | O(n) | Không chính xác đối với các trường hợp đầu vào hỗn hợp | 
| Tối ưu (ký tự đầu tiên viết hoa, phần còn lại nối) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từ đầu vào và xóa mọi dòng mới hoặc khoảng trắng ở cuối. Điều này đảm bảo chúng ta chỉ làm việc với các chữ cái thực tế. 
2. Trích xuất ký tự đầu tiên của từ. Ký tự này là ký tự duy nhất có thể cần sửa đổi. 
3. Chuyển ký tự đầu tiên thành chữ hoa. Trong Python, điều này có thể được thực hiện bằng`upper()`phương pháp. 
4. Ghép ký tự đầu tiên viết hoa với chuỗi con gồm tất cả các ký tự còn lại trong từ, bắt đầu từ chỉ số 1. Điều này giữ nguyên kiểu viết hoa ban đầu của phần còn lại của từ. 
5. In chuỗi kết quả. 

Điều này hiệu quả vì tính bất biến rất đơn giản: ký tự đầu tiên được đảm bảo là chữ hoa và phần còn lại của chuỗi vẫn giữ nguyên chính xác như trong dữ liệu đầu vào. Vì việc cắt chuỗi giữ nguyên thứ tự và nội dung nên không có thông tin nào bị mất và thuật toán luôn tạo ra từ viết hoa chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

word = input().strip()
capitalized = word[0].upper() + word[1:]
print(capitalized)
```Đoạn mã đầu tiên sẽ loại bỏ bất kỳ dòng mới nào ở cuối bằng cách sử dụng`strip()`để đảm bảo chỉ còn lại các chữ cái. Sau đó nó truy cập`word[0]`để cô lập ký tự đầu tiên và áp dụng`upper()`. Nối với`word[1:]`bảo quản tất cả các chữ cái còn lại chính xác như cũ. Cuối cùng, kết quả được in. Việc triển khai này xử lý chính xác các từ có một ký tự vì việc cắt`word[1:]`trên chuỗi một ký tự sẽ tạo ra một chuỗi trống, do đó phép nối hoạt động không có lỗi. 

## Ví dụ đã hoạt động 

Đầu vào: "ApPLE" 

| Bước | từ | ký tự đầu tiên | phần còn lại | viết hoa | 
| --- | --- | --- | --- | --- | 
| 1 | "Táo" | "A" | "pPLe" | "A" + "pPLe" | 
| 2 | Đầu ra | "Táo" | | | 

Điều này cho thấy rằng không có sửa đổi nào xảy ra nếu ký tự đầu tiên đã là chữ hoa. 

Đầu vào: "táo" 

| Bước | từ | ký tự đầu tiên | phần còn lại | viết hoa | 
| --- | --- | --- | --- | --- | 
| 1 | "táo" | "một" | "quả táo" | "A" + "quả táo" | 
| 2 | Đầu ra | "Táo" | | | 

Điều này thể hiện việc chuyển đổi chữ cái đầu tiên trong khi giữ nguyên phần còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ ký tự đầu tiên được sửa đổi và phần còn lại được cắt mà không lặp lại. Độ dài chuỗi tối đa là 103. | 
| Không gian | O(1) | Không có thêm không gian tỷ lệ thuận với đầu vào; chỉ có một chuỗi mới được tạo. | 

Với những hạn chế, giải pháp này phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    word = input().strip()
    return word[0].upper() + word[1:]

# provided sample
assert run("ApPLe\n") == "ApPLe", "sample 1"

# custom cases
assert run("apple\n") == "Apple", "lowercase first letter"
assert run("z\n") == "Z", "single character lowercase"
assert run("Z\n") == "Z", "single character uppercase"
assert run("aPpLe\n") == "APpLe", "mixed case first letter lowercase"
assert run("AbcDEfG\n") == "AbcDEfG", "already capitalized"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "táo" | "Táo" | chữ cái đầu viết thường | 
| "z" | "Z" | chữ thường một ký tự | 
| "Z" | "Z" | chữ hoa một ký tự | 
| "aPpLe" | "ỨNG DỤNG" | chữ cái đầu tiên viết thường hỗn hợp | 
| "AbcDEfG" | "AbcDEfG" | đã viết hoa | 

## Vỏ cạnh 

Đối với một từ viết thường có một ký tự như "z", thuật toán truy cập`word[0]`, áp dụng`upper()`, dẫn đến "Z" và nối với`word[1:]`, trống rỗng. Đầu ra là chính xác. 

Đối với từ viết hoa một ký tự như "Z",`word[0].upper()`để nó là "Z" và ghép với`word[1:]`(trống) không thay đổi nó. 

Đối với kiểu nhập hỗn hợp như "aPpLe", ký tự đầu tiên được chuyển đổi thành "A" trong khi "PpLe" còn lại không thay đổi. Điều này xác nhận rằng thuật toán không viết sai chữ thường hoặc sửa đổi các chữ cái còn lại.
