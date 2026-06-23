---
title: "CF 105056A - Email Odoo tiềm năng"
description: "Chúng tôi được cung cấp một chuỗi duy nhất được cho là đại diện cho mã định danh giống như email và chúng tôi cần quyết định xem chuỗi đó có khớp với mẫu rất cụ thể được sử dụng bởi một nhóm địa chỉ hư cấu hay không. Một chuỗi hợp lệ phải bao gồm hai phần được phân tách bằng một ký tự '@'."
date: "2026-06-23T12:11:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105056
codeforces_index: "A"
codeforces_contest_name: "International Odoo Programming Contest 2024"
rating: 0
weight: 105056
solve_time_s: 71
verified: false
draft: false
---

[CF 105056A - Email Odoo tiềm năng](https://codeforces.com/problemset/problem/105056/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi duy nhất được cho là đại diện cho mã định danh giống như email và chúng tôi cần quyết định xem chuỗi đó có khớp với mẫu rất cụ thể được sử dụng bởi một nhóm địa chỉ hư cấu hay không. 

Một chuỗi hợp lệ phải bao gồm hai phần được phân tách bằng một ký tự '@'. Phần trước '@' là tên người dùng chỉ được tạo bằng các chữ cái tiếng Anh viết thường. Phần sau '@' phải là văn bản theo nghĩa đen "odoo". Sau tên miền đó, không có hậu tố bổ sung, không có dấu chấm thừa và không được phép ký tự. 

Vì vậy, nhiệm vụ giảm xuống còn kiểm tra cả cấu trúc và nội dung chính xác: một '@', một bên trái không trống và hoàn toàn là chữ cái thường, và một bên phải dài chính xác bốn ký tự và bằng "odoo". 

Kích thước đầu vào tối đa là 50 ký tự, do đó, ngay cả việc quét hoặc phân tách đơn giản cũng không đáng kể về mặt hiệu suất. Bất kỳ cách tiếp cận nào chạy theo thời gian tuyến tính trên chuỗi đều đủ. 

Các trường hợp lỗi phổ biến nhất đến từ các vi phạm định dạng tinh vi. Một chuỗi có thể chứa nhiều ký hiệu '@', ký hiệu này ngay lập tức làm mất hiệu lực của chuỗi đó. Một vấn đề phổ biến khác là chấp nhận các miền chỉ chứa "odoo" nhưng không hoàn toàn bằng nhau, chẳng hạn như "odoocom" hoặc "odoo". hoặc "odoo123". Cuối cùng, các chữ cái hoặc chữ số viết hoa trong tên người dùng sẽ bị từ chối ngay cả khi mọi thứ khác đều khớp. 

Ví dụ làm rõ các trường hợp khó khăn này: 

đầu vào:`palm@odoocom`Đầu ra:`no`Mặc dù chuỗi con "odoo" xuất hiện nhưng miền vẫn dài hơn yêu cầu. 

đầu vào:`im_not_an_email`Đầu ra:`no`Không có dấu phân cách '@' nào cả, do đó cấu trúc không hợp lệ. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để giải quyết vấn đề này là thử diễn giải chuỗi dưới dạng email bằng cách quét mọi vị trí dưới dạng điểm phân tách '@' tiềm năng. Đối với mỗi phần phân chia ứng cử viên, chúng tôi sẽ xác thực chuỗi con bên trái cho các chữ cái viết thường và so sánh chuỗi con bên phải với "odoo". Điều này hiệu quả vì các ràng buộc rất nhỏ nhưng lại tạo ra chi phí không cần thiết vì chúng ta chỉ cần tìm một dấu phân cách và xác thực nó một cách trực tiếp. 

Một cách tiếp cận rõ ràng hơn là định vị ký tự '@' trước tiên. Nếu nó không tồn tại hoặc xuất hiện nhiều lần, chúng tôi sẽ từ chối chuỗi ngay lập tức. Khi đã biết điểm phân tách, chúng tôi xác thực phía bên trái bằng cách kiểm tra các loại ký tự và đảm bảo nó không trống. Sau đó, chúng tôi xác minh phía bên phải chính xác là "odoo" và không có gì khác. 

Cái nhìn sâu sắc quan trọng là cấu trúc đủ cứng nhắc để chúng ta không cần phải quay lại hoặc đoán mò. Có chính xác một phân tách hợp lệ nếu chuỗi hoàn toàn hợp lệ, do đó phân tích cú pháp trực tiếp là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra phân chia Brute Force | O(n²) trường hợp xấu nhất | O(1) | Đã chấp nhận | 
| Xác thực phân chia đơn | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào E. 
2. Đếm số lần '@' xuất hiện trong E. Nếu không đúng một, hãy trả về "no". Định dạng này yêu cầu một dấu phân cách duy nhất, do đó, nhiều lần hoặc không lần xuất hiện nào sẽ ngay lập tức phá vỡ cấu trúc. 
3. Chia E thành hai phần ở vị trí '@': trái và phải. 
4. Kiểm tra xem phần bên trái có trống không và chỉ bao gồm các chữ cái tiếng Anh viết thường từ 'a' đến 'z'. Bất kỳ chữ số, chữ in hoa hoặc dấu gạch dưới nào đều vô hiệu hóa nó. 
5. Kiểm tra xem phần bên phải có chính xác là chuỗi "odoo" không. Bất kỳ sai lệch nào về độ dài hoặc nội dung ký tự đều vô hiệu hóa nó. 
6. Nếu tất cả các lần kiểm tra đều đạt, ghi "có", nếu không thì xuất "không". 

### Tại sao nó hoạt động 

Việc xác thực thực thi việc phân tách cấu trúc duy nhất của chuỗi. Vì có chính xác một vị trí hợp lệ cho '@', nên khi vị trí đó được cố định, cả hai nửa đều bị ràng buộc độc lập mà không có sự mơ hồ. Điều kiện bên trái đảm bảo không có ký tự không hợp lệ nào rò rỉ vào tên người dùng và kiểm tra tính bằng nhau ở bên phải thực thi việc khớp tên miền nghiêm ngặt. Vì mọi định dạng không hợp lệ đều vi phạm ít nhất một trong các ràng buộc độc lập này nên không có chuỗi không hợp lệ nào có thể vượt qua tất cả các bước kiểm tra cùng một lúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

if s.count('@') != 1:
    print("no")
    sys.exit(0)

left, right = s.split('@')

if not left or right != "odoo":
    print("no")
    sys.exit(0)

for c in left:
    if not ('a' <= c <= 'z'):
        print("no")
        sys.exit(0)

print("yes")
```Mã bắt đầu bằng cách đọc chuỗi và ngay lập tức thực thi ràng buộc '@' duy nhất, giúp loại bỏ sớm tất cả các trường hợp không hợp lệ về mặt cấu trúc. 

Sau khi phân tách, nó xác thực phần bên phải bằng cách sử dụng so sánh chuỗi trực tiếp, điều này vừa đơn giản vừa an toàn hơn so với kiểm tra từng ký tự vì mục tiêu đã được cố định. 

Phía bên trái được xác thực theo từng ký tự để đảm bảo chỉ có các chữ cái viết thường và không trống. Vòng lặp rõ ràng này tránh các trường hợp góc cạnh như tên người dùng trống hoặc ký hiệu không hợp lệ có thể trượt qua quá trình kiểm tra tiền tố đơn giản. 

## Ví dụ đã hoạt động 

### Ví dụ 1: trường hợp hợp lệ 

đầu vào:`abc@odoo`| Bước | Trái | Đúng | '@' đếm | Quyết định | 
| --- | --- | --- | --- | --- | 
| Đọc | abc@odoo | - | 1 | tiếp tục | 
| Chia | abc | odoo | 1 | tiếp tục | 
| Xác thực trái | abc | odoo | 1 | chữ cái hợp lệ | 
| Xác thực đúng | abc | odoo | 1 | trận đấu odoo | 

Đầu ra: có 

Điều này xác nhận toàn bộ quy trình chấp nhận chuỗi có cấu trúc rõ ràng với tên miền và tên người dùng viết thường chính xác. 

### Ví dụ 2: tên miền không hợp lệ 

đầu vào:`palm@odoocom`| Bước | Trái | Đúng | '@' đếm | Quyết định | 
| --- | --- | --- | --- | --- | 
| Đọc | palm@odoocom | - | 1 | tiếp tục | 
| Chia | cọ | odoocom | 1 | tiếp tục | 
| Xác thực trái | cọ | odoocom | 1 | chữ cái hợp lệ | 
| Xác thực đúng | cọ | odoocom | 1 | không khớp | 

Đầu ra: không 

Điều này chứng tỏ rằng các kết quả khớp một phần của miền bị từ chối vì yêu cầu phải có sự bình đẳng chứ không phải kết hợp chuỗi con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | quét một lần cho '@' và một lần quét qua bên trái | 
| Không gian | O(1) | chỉ một số chuỗi con và biến được lưu trữ | 

Kích thước đầu vào được giới hạn bởi 50, do đó quá trình quét tuyến tính này thực sự có thời gian không đổi trong thực tế và dễ dàng phù hợp với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        s = input().strip()
        if s.count('@') != 1:
            print("no")
        else:
            left, right = s.split('@')
            if not left or right != "odoo":
                print("no")
            else:
                ok = True
                for c in left:
                    if not ('a' <= c <= 'z'):
                        ok = False
                        break
                print("yes" if ok else "no")
    return out.getvalue().strip()

# provided samples
assert run("[email protected]") == "yes", "sample 1"
assert run("palm@odoocom") == "no", "sample 2"
assert run("im_not_an_email") == "no", "sample 3"

# custom cases
assert run("@odoo") == "no", "empty username"
assert run("abc@odoo@odoo") == "no", "multiple @"
assert run("ABC@odoo") == "no", "uppercase letters"
assert run("a@odoo") == "yes", "minimum valid case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| @odoo | không | từ chối tên người dùng trống | 
| abc@odoo@odoo | không | xử lý nhiều '@' | 
| ABC@odoo | không | thực thi chữ thường | 
| a@odoo | vâng | cấu trúc hợp lệ tối thiểu | 

## Vỏ cạnh 

Một trường hợp quan trọng là tên người dùng trống. Đối với đầu vào`@odoo`, phép chia sẽ tạo ra phần bên trái trống. Thuật toán kiểm tra rõ ràng`if not left`, vì vậy nó sẽ từ chối nó một cách chính xác trước khi thử xác thực bất kỳ ký tự nào. 

Một trường hợp cạnh khác là nhiều dấu phân cách, chẳng hạn như`abc@odoo@odoo`. Vì lần kiểm tra đầu tiên yêu cầu chính xác một '@', đầu vào này bị từ chối ngay lập tức mà không có sự mơ hồ trong việc phân tách, ngăn chặn việc vô tình chấp nhận các cấu trúc không đúng định dạng. 

Trường hợp thứ ba là phân biệt chữ hoa chữ thường trong tên người dùng, ví dụ`ABC@odoo`. Sau khi tách, vòng lặp bên trái sẽ phát hiện các ký tự nằm ngoài phạm vi`'a'`ĐẾN`'z'`, và chuỗi bị từ chối.
