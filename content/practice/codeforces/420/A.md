---
title: "CF 420A - Khởi động"
description: "Chúng tôi được yêu cầu xác định xem tên công ty nhất định có đối xứng với gương dọc hay không. Trong thực tế, chúng ta có một chuỗi duy nhất bao gồm các chữ cái tiếng Anh viết hoa và chúng ta cần kiểm tra xem chuỗi đó có trông giống hệt nếu được phản ánh trong…"
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 420
codeforces_index: "A"
codeforces_contest_name: "Coder-Strike 2014 - Finals (online edition, Div. 1)"
rating: 1000
weight: 420
solve_time_s: 84
verified: true
draft: false
---

[CF 420A - Khởi động](https://codeforces.com/problemset/problem/420/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định xem tên công ty nhất định có đối xứng với gương dọc hay không. Trong thực tế, chúng ta có một chuỗi đơn bao gồm các chữ cái tiếng Anh viết hoa và chúng ta cần kiểm tra xem chuỗi đó có trông giống hệt nếu được phản chiếu trong gương đặt thẳng đứng dọc theo trục trái-phải của nó hay không. Chỉ có một số chữ cái nhất định là đối xứng theo cách này, ví dụ`A`,`H`,`I`,`M`,`O`,`T`,`U`,`V`,`W`,`X`,`Y`. Những chữ cái như`B`hoặc`C`sẽ trông khác khi được phản chiếu và do đó không thể xuất hiện dưới dạng tên hợp lệ. 

Ràng buộc đầu vào cho phép độ dài chuỗi lên tới 100.000 ký tự. Với giới hạn thời gian 1 giây, điều này ngụ ý rằng thuật toán phải chạy trong thời gian tuyến tính, tệ nhất là O(n). Bất cứ điều gì bậc hai hoặc liên quan đến các phép lặp lồng nhau trên chuỗi sẽ quá chậm. Bộ nhớ rất lớn ở mức 256 MB, do đó việc lưu trữ một tập hợp nhỏ các ký tự hợp lệ hoặc một mảng boolean có độ dài n là chuyện nhỏ. 

Các trường hợp chính cần cẩn thận là các chuỗi trống hoặc chuỗi ký tự đơn, mặc dù vấn đề đảm bảo đầu vào không trống. Một ký tự đơn hợp lệ (như`A`) nên quay lại`YES`. Một chuỗi có một ký tự không hợp lệ (như`B`) nên quay lại`NO`. Một kịch bản tinh tế khác là các chuỗi có tất cả các ký tự hợp lệ nhưng được sắp xếp theo cách không đối xứng, chẳng hạn`AHAH`không hợp lệ vì nó đảo ngược`HAHA`khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản rất đơn giản: đảo ngược chuỗi và kiểm tra xem mọi ký tự có được phản chiếu và nằm trong tập hợp được phép hay không. Điều này có tác dụng vì việc sao chép tương đương với việc đọc ngược chuỗi trong khi vẫn đảm bảo mỗi ký tự đều đối xứng. Tuy nhiên, trong cách triển khai đơn giản, người ta có thể vô tình cố gắng ánh xạ mọi ký tự tới phiên bản được phản chiếu của nó, điều này là không cần thiết đối với các chữ cái viết hoa vì chỉ một tập hợp con cụ thể là hợp lệ. Điều này có thể đưa ra các hoạt động dư thừa nhưng vẫn sẽ là O(n). 

Điểm mấu chốt là vấn đề được rút gọn thành hai bước kiểm tra: thứ nhất, mọi ký tự phải thuộc tập hợp các chữ cái đối xứng theo chiều dọc; thứ hai, chuỗi phải đọc xuôi và ngược như nhau. Bằng cách kết hợp các bước kiểm tra này trong một lần duy nhất, chúng ta có thể giải quyết vấn đề một cách hiệu quả trong thời gian O(n) mà không cần thêm bộ nhớ ngoài một tập hợp nhỏ các chữ cái có kích thước không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force: lùi + soi gương | O(n) | O(n) | Đã chấp nhận | 
| Tối ưu: kiểm tra một lần đối với tập hợp và tính đối xứng được phép | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một bộ chứa tất cả các chữ cái đối xứng với gương đứng:`A, H, I, M, O, T, U, V, W, X, Y`. Điều này cho phép kiểm tra tư cách thành viên liên tục cho từng ký tự. 
2. Khởi tạo hai con trỏ, một ở đầu chuỗi và một ở cuối chuỗi. Chúng ta sẽ lặp lại ở giữa. 
3. Ở mỗi bước, hãy kiểm tra xem cả hai ký tự ở con trỏ đầu và con trỏ cuối có nằm trong tập hợp được phép hay không. Nếu một trong hai ký tự không hợp lệ, hãy in`NO`và chấm dứt. Điều này ngay lập tức xử lý các trường hợp trong đó một ký tự không được phép làm mất hiệu lực toàn bộ chuỗi. 
4. Kiểm tra xem các ký tự ở con trỏ đầu và cuối có bằng nhau không. Nếu không thì in`NO`và chấm dứt. Điều này đảm bảo rằng chuỗi đọc xuôi và ngược giống nhau. 
5. Di chuyển con trỏ bắt đầu về phía trước một bước và con trỏ kết thúc lùi lại một bước, lặp lại bước 3 và 4 cho đến khi các con trỏ gặp nhau hoặc cắt nhau. 
6. Nếu vòng lặp hoàn thành mà không kết thúc, hãy in`YES`. Chuỗi thỏa mãn cả tính đối xứng và tính hợp lệ của ký tự. 

Tại sao nó hoạt động: Điều bất biến là ở mỗi lần lặp, tất cả các ký tự bên ngoài con trỏ hiện tại đã được xác thực về tính đối xứng và vị trí chính xác. Thuật toán kiểm tra tất cả các cặp chính xác một lần. Vì chỉ cho phép các chữ cái hợp lệ và mỗi cặp khớp nhau, chuỗi cuối cùng được đảm bảo giống hệt với phản chiếu trong gương của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
valid_letters = {'A', 'H', 'I', 'M', 'O', 'T', 'U', 'V', 'W', 'X', 'Y'}

left, right = 0, len(s) - 1
while left <= right:
    if s[left] not in valid_letters or s[right] not in valid_letters:
        print("NO")
        sys.exit()
    if s[left] != s[right]:
        print("NO")
        sys.exit()
    left += 1
    right -= 1

print("YES")
```Đầu tiên, mã sẽ đọc chuỗi đầu vào và xóa mọi ký tự dòng mới ở cuối. Bộ chữ cái phản chiếu hợp lệ cho phép kiểm tra tư cách thành viên nhanh chóng. Sau đó, chúng tôi sử dụng kỹ thuật hai con trỏ để kiểm tra tính đối xứng từ ngoài vào trong. Việc thoát ngay khi gặp lỗi sẽ tránh được những công việc không cần thiết. Điều này đảm bảo thuật toán chạy trong thời gian O(n) và chỉ sử dụng không gian bổ sung O(1). 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
AHA
```| trái | đúng | s[trái] | s[phải] | kiểm tra hợp lệ | kiểm tra bằng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | A | A | vâng | vâng | 
| 1 | 1 | H | H | vâng | vâng | 

Vòng lặp hoàn thành nên kết quả đầu ra là`YES`. Cả tính đối xứng và tính hợp lệ đều được thỏa mãn. 

Đầu vào mẫu 2:```
ABBA
```| trái | đúng | s[trái] | s[phải] | kiểm tra hợp lệ | kiểm tra bằng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | A | A | vâng | vâng | 
| 1 | 2 | B | B | không | - | 

Từ`B`không hợp lệ, đầu ra là`NO`. Điều này chứng tỏ bộ lọc ký tự loại bỏ chính xác các chữ cái không đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được truy cập đúng một lần với hai con trỏ di chuyển về phía giữa. | 
| Không gian | O(1) | Tập hợp các chữ cái hợp lệ có kích thước không đổi; không có quy mô lưu trữ bổ sung với n. | 

Thuật toán dễ dàng xử lý các chuỗi có độ dài lên tới 100.000 trong vòng 1 giây và sử dụng bộ nhớ không đáng kể, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    valid_letters = {'A', 'H', 'I', 'M', 'O', 'T', 'U', 'V', 'W', 'X', 'Y'}
    left, right = 0, len(s) - 1
    while left <= right:
        if s[left] not in valid_letters or s[right] not in valid_letters:
            return "NO"
        if s[left] != s[right]:
            return "NO"
        left += 1
        right -= 1
    return "YES"

# Provided sample
assert run("AHA\n") == "YES", "sample 1"

# Custom cases
assert run("ABBA\n") == "NO", "invalid letters"
assert run("AAA\n") == "YES", "all valid, odd length"
assert run("AHHHA\n") == "YES", "odd length palindrome"
assert run("XYX\n") == "YES", "symmetric X/Y letters"
assert run("XYZ\n") == "NO", "contains invalid Z"
assert run("A\n") == "YES", "single valid letter"
assert run("B\n") == "NO", "single invalid letter"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ABBA | KHÔNG | Chữ cái không hợp lệ ở giữa | 
| AAA | CÓ | Các chữ cái hợp lệ lặp lại, độ dài lẻ | 
| AHHHA | CÓ | Bảng màu có độ dài lẻ với các chữ cái hợp lệ | 
| XYZ | KHÔNG | Sự hiện diện của các chữ cái không hợp lệ | 
| A | CÓ | Ký tự hợp lệ đơn | 
| B | KHÔNG | Ký tự đơn không hợp lệ | 

## Vỏ cạnh 

Đối với một nhân vật duy nhất`A`, con trỏ`left`Và`right`cả hai đều trỏ đến chỉ số 0. Vòng lặp chạy một lần, tìm thấy`A`trong tập hợp lệ và`s[left] == s[right]`, sau đó thoát ra và quay lại`YES`. Đối với một ký tự không hợp lệ`B`, vòng lặp tương tự ngay lập tức phát hiện ra rằng`B`không nằm trong tập hợp lệ và trả về`NO`. Các chuỗi có độ dài tối đa và tất cả các ký tự hợp lệ sẽ được xử lý trong thời gian O(n) mà không cần thêm bộ nhớ ngoài tập hợp nhỏ, xác nhận tính chắc chắn vượt qua các ràng buộc.
