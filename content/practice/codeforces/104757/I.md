---
title: "CF 104757I - Chuyển đổi ISBN"
description: "Mỗi chuỗi đầu vào đại diện cho một mã ISBN-10 ứng cử viên có thể chứa các chữ số, dấu gạch nối và có thể cả ký tự X dưới dạng chữ số tổng kiểm tra."
date: "2026-06-28T22:49:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104757
codeforces_index: "I"
codeforces_contest_name: "2023-2024 ICPC East North America Regional Contest (ECNA 2023)"
rating: 0
weight: 104757
solve_time_s: 33
verified: true
draft: false
---

[CF 104757I - Chuyển đổi ISBN](https://codeforces.com/problemset/problem/104757/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi chuỗi đầu vào đại diện cho một mã ISBN-10 ứng cử viên có thể chứa các chữ số, dấu gạch nối và có thể cả ký tự`X`như một chữ số tổng kiểm tra. Nhiệm vụ của chúng ta trước tiên là xác định xem chuỗi này có phải là ISBN-10 hợp lệ theo các quy tắc đã cho hay không và nếu nó hợp lệ, hãy chuyển đổi nó thành biểu diễn ISBN-13 tương ứng. 

Xác nhận là cổng đầu tiên. Chúng ta phải hiểu chuỗi này là ISBN gồm 10 chữ số sau khi xóa dấu gạch nối, trong đó ký tự cuối cùng là chữ số tổng kiểm tra. Quy tắc tổng kiểm tra là tổng có trọng số từ 10 xuống 1 và phải chia hết cho 11, với quy tắc đặc biệt là chữ số`X`đại diện cho giá trị 10 nhưng chỉ được phép ở vị trí tổng kiểm tra. 

Nếu ISBN-10 hợp lệ, quá trình chuyển đổi sẽ được tiến hành bằng cách loại bỏ tổng kiểm tra của nó, thêm tiền tố vào trước`978`, sau đó tính toán lại tổng kiểm tra ISBN-13 mới theo các trọng số xen kẽ 1 và 3. 

Các ràng buộc rất nhỏ: tối đa 25 chuỗi, mỗi chuỗi có độ dài lên tới 13. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu nặng hoặc tối ưu hóa. Mọi thao tác đều có thể tuyến tính theo độ dài chuỗi và ngay cả cách tiếp cận phân tích cú pháp và tính toán lại đơn giản cũng đủ. 

Các trường hợp phức tạp chính là cấu trúc hơn là tính toán. 

Một vấn đề là xử lý dấu gạch nối. Đầu vào cho phép dấu gạch nối ở bất kỳ đâu ngoại trừ vị trí đầu, cuối hoặc liên tiếp. Một cách tiếp cận đơn giản có thể chỉ cần loại bỏ dấu gạch nối và xác thực các chữ số, nhưng điều này là không đủ trừ khi chúng tôi cũng đảm bảo rằng sau khi loại bỏ, chúng tôi vẫn có chính xác 10 ký tự có ý nghĩa. 

Một vấn đề khác là`X`tính cách. Việc thực hiện bất cẩn có thể dẫn đến`X`hoàn toàn không hợp lệ, nhưng nó chỉ hợp lệ ở dạng chữ số tổng kiểm tra và chỉ đóng góp giá trị 10 ở vị trí cuối cùng. 

Điểm tinh tế thứ ba là định dạng không hợp lệ và tổng kiểm tra không hợp lệ đều tạo ra cùng một đầu ra`"invalid"`. Ví dụ,`3-540-4258-02`về mặt cấu trúc vẫn ổn sau khi loại bỏ dấu gạch nối, nhưng không kiểm tra được tổng, trong khi các mẫu dấu gạch nối không đúng định dạng cũng dẫn đến không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng diễn giải chuỗi theo nhiều cách một cách rõ ràng, kiểm tra tất cả các vị trí dấu gạch nối và cách diễn giải chữ số có thể có. Điều này là không cần thiết vì sự cố đã khắc phục được cấu trúc: dấu gạch nối không liên quan đến ý nghĩa số ngoại trừ định dạng và quy tắc tổng kiểm tra xác định tính hợp lệ duy nhất. 

Thay vào đó, quan sát quan trọng là chúng ta có thể quy bài toán thành hai đường tuyến tính độc lập. Đầu tiên, chuẩn hóa chuỗi bằng cách loại bỏ dấu gạch nối trong khi vẫn giữ nguyên ý nghĩa của chuỗi`X`chỉ ở vị trí cuối cùng. Sau đó tính trực tiếp tổng kiểm tra ISBN-10. Nếu nó vượt qua, hãy xây dựng ISBN-13 bằng cách nối và tính tổng kiểm tra của nó trong một lần. 

Cấu trúc của bài toán đảm bảo rằng một khi việc chuẩn hóa được thực hiện, mọi thứ đều là số học xác định. Không có tìm kiếm hoặc mơ hồ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Các biến thể phân tích cú pháp bạo lực | Hàm mũ | O(n) | Quá chậm | 
| Chuẩn hóa + tính toán tổng kiểm tra trực tiếp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Xác thực và chuyển đổi ISBN-10 

1. Đọc chuỗi đầu vào và loại bỏ tất cả dấu gạch nối, tạo thành dạng thu gọn. 
2. Xác minh rằng chuỗi kết quả có đúng 10 ký tự. Nếu không, nó không hợp lệ. 
3. Kiểm tra xem các ký tự từ 1 đến 9 chỉ là chữ số. Nếu bất kỳ không phải là chữ số, hãy từ chối chuỗi. 
4. Xử lý riêng ký tự cuối cùng: có thể là chữ số hoặc`X`, Ở đâu`X`đại diện cho 10. 
5. Tính tổng có trọng số ISBN-10 bằng cách sử dụng trọng số từ 10 đến 1. 
6. Nếu tổng không chia hết cho 11, ghi`"invalid"`. 
7. Mặt khác, hãy xây dựng chuỗi cơ sở ISBN-13 bằng cách lấy 9 chữ số đầu tiên và thêm vào trước`"978"`, chèn dấu gạch nối sau tiền tố để định dạng. 
8. Tính chữ số tổng kiểm tra ISBN-13 bằng cách sử dụng trọng số xen kẽ 1 và 3 trên 12 chữ số đầu tiên. 
9. Nối chữ số tổng kiểm tra và xuất ra chuỗi ISBN-13 cuối cùng với các dấu gạch nối được giữ nguyên từ chuỗi gốc (không bao gồm vị trí tổng kiểm tra cũ) cộng với dấu gạch nối mới sau`978`. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là tính hợp lệ của ISBN-10 hoàn toàn được nắm bắt bởi một modulo đồng dư tuyến tính duy nhất 11 trên các trọng số cố định và tính hợp lệ của ISBN-13 tương tự như modulo đồng dư tuyến tính 10. Sau khi chuỗi được chuẩn hóa, dấu gạch nối không còn ảnh hưởng đến cấu trúc số học, do đó quá trình tính toán giảm xuống còn đánh giá hai tổng có trọng số xác định. Vì cả hai quy tắc tổng kiểm tra đều xác định duy nhất chữ số cuối cùng cho các chữ số trước đó nên bước chuyển đổi được xác định đầy đủ và không gây ra sự mơ hồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_valid_isbn10(s):
    # s is string without hyphens
    if len(s) != 10:
        return False, None

    total = 0
    for i in range(9):
        if not s[i].isdigit():
            return False, None
        total += (10 - i) * int(s[i])

    # last digit
    if s[9] == 'X':
        d10 = 10
    elif s[9].isdigit():
        d10 = int(s[9])
    else:
        return False, None

    total += 1 * d10

    if total % 11 != 0:
        return False, None

    return True, s[:9]

def isbn13_checksum(digits12):
    total = 0
    for i, ch in enumerate(digits12):
        d = int(ch)
        if i % 2 == 0:
            total += d
        else:
            total += 3 * d
    return (10 - (total % 10)) % 10

def convert(isbn10_raw):
    s = isbn10_raw.strip()

    # keep hyphen pattern info
    parts = []
    cur = []
    for ch in s:
        if ch == '-':
            if cur:
                parts.append(''.join(cur))
                cur = []
            parts.append('-')
        else:
            cur.append(ch)
    if cur:
        parts.append(''.join(cur))

    compact = ''.join(ch for ch in s if ch != '-')

    ok, base9 = is_valid_isbn10(compact)
    if not ok:
        return "invalid"

    # build ISBN-13 digits
    digits12 = "978" + base9

    check = isbn13_checksum(digits12)
    full_digits = digits12 + str(check)

    # formatting: prepend 978- then keep original hyphens except last checksum position removed
    # We reconstruct simply: 978- + original structure without last char
    rebuilt = []
    rebuilt.append("978-")

    # reuse original hyphen structure except last char removed
    core = s.replace('-', '')[:-1]
    idx = 0
    for ch in s:
        if ch == '-':
            rebuilt.append('-')
        else:
            if idx < len(core):
                rebuilt.append(core[idx])
                idx += 1

    rebuilt.append(str(check))
    return ''.join(rebuilt)

t = int(input())
for _ in range(t):
    print(convert(input().strip()))
```Việc triển khai đầu tiên sẽ loại bỏ và xác nhận ISBN-10, tách biệt lõi số và c
