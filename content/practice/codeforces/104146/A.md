---
title: "CF 104146A - ABC của Nam và Nữ"
description: "Chúng ta được cung cấp một chuỗi ngắn đại diện cho một thẻ tên bị mờ. Tên ban đầu được biết chính xác là một trong ba chuỗi cố định: Alice, Bob hoặc Cindy. Tuy nhiên, chuỗi được quan sát có thể chứa chữ thường hoặc chữ in hoa và một số vị trí có thể không đọc được, được hiển thị dưới dạng dấu chấm."
date: "2026-07-02T01:32:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104146
codeforces_index: "A"
codeforces_contest_name: "Abakoda Long Contest 2022"
rating: 0
weight: 104146
solve_time_s: 45
verified: true
draft: false
---

[CF 104146A - ABC về nam giới và phụ nữ](https://codeforces.com/problemset/problem/104146/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi ngắn đại diện cho một thẻ tên bị mờ. Tên ban đầu được biết chính xác là một trong ba chuỗi cố định: Alice, Bob hoặc Cindy. Tuy nhiên, chuỗi được quan sát có thể chứa chữ thường hoặc chữ in hoa và một số vị trí có thể không đọc được, được hiển thị dưới dạng dấu chấm. Mỗi dấu chấm có thể đại diện cho bất kỳ chữ cái tiếng Anh nào. 

Nhiệm vụ là xác định tên nào trong số ba tên vẫn có thể khớp với mẫu đã quan sát sau khi thay thế từng dấu chấm bằng một chữ cái phù hợp. Một tên được coi là hợp lệ nếu nó khớp với từng ký tự trong chuỗi được quan sát, phân biệt chữ hoa chữ thường và cho phép các dấu chấm khớp với bất kỳ thứ gì. 

Đầu ra phụ thuộc vào số lượng trong số ba tên tương thích. Nếu chính xác một tên phù hợp, chúng tôi sẽ xuất tên đó. Nếu có nhiều tên trùng khớp thì thông tin sẽ không đủ và chúng tôi xuất ra CAN'T TELL. Nếu không có tên nào phù hợp, chúng tôi sẽ xuất ra SOMETHING'S WRONG. 

Kích thước đầu vào rất nhỏ, tối đa 5 ký tự. Điều này ngay lập tức loại trừ mọi nhu cầu xử lý trước hoặc tối ưu hóa phức tạp. Chỉ cần so sánh trực tiếp với từng tên ứng cử viên là đủ. 

Trường hợp cạnh tinh tế phát sinh khi đầu vào chỉ chứa các dấu chấm. Ví dụ: dữ liệu nhập như "...." khớp với cả ba tên vì mỗi ký tự có thể được chọn tự do. Trong trường hợp đó, kết quả đầu ra chính xác là CAN'T TELL, không phải một trong các tên. Một trường hợp khác là khi xảy ra trường hợp không khớp chữ, chẳng hạn như "bob" so với "Bob", trong đó sự bằng nhau phải chính xác; một so sánh không phân biệt chữ hoa chữ thường sẽ chấp nhận các kết quả khớp không hợp lệ một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực đã phù hợp với cấu trúc của vấn đề. Chúng tôi chỉ cần thử từng tên trong số ba tên ứng cử viên và kiểm tra xem nó có khớp với chuỗi đầu vào hay không bằng cách xác minh từng vị trí. Dấu chấm trong dữ liệu nhập đóng vai trò như một ký tự đại diện nên nó luôn khớp. Bất kỳ ký tự cố định nào cũng phải khớp chính xác. 

Vì chỉ có ba ứng cử viên và mỗi chuỗi có độ dài tối đa là 5 nên tổng công việc là không đổi. Ngay cả khi chúng tôi mở rộng tập ứng cử viên, cấu trúc vẫn duy trì việc khớp mẫu đơn giản. 

Quan sát quan trọng là chúng ta không cần phải xây dựng tất cả các cách diễn giải có thể có về dấu chấm. Điều đó sẽ bùng nổ theo cấp số nhân về số lượng dấu chấm. Thay vào đó, chúng tôi kiểm tra tính tương thích một cách trực tiếp: một ứng viên hợp lệ nếu nó không bao giờ xung đột với một ký tự cố định trong đầu vào. Điều này làm giảm vấn đề từ việc tạo tổ hợp sang kiểm tra xác định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng mô hình vũ phu | O(3 · 26^k) | O(1) | Về nguyên tắc quá chậm | 
| Kiểm tra đối sánh trực tiếp | O(3 · n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào đại diện cho thẻ tên bị mờ. Chúng tôi coi nó như một mẫu trong đó các chữ cái là các ràng buộc cố định và dấu chấm là các ký tự đại diện. 
2. Lưu trữ ba tên ứng cử viên Alice, Bob và Cindy dưới dạng chuỗi tham chiếu cố định. 
3. Đối với mỗi tên ứng viên, hãy so sánh nó với từng ký tự trong chuỗi đầu vào. 
4. Tại mỗi vị trí, nếu ký tự đầu vào là dấu chấm, chúng tôi chấp nhận bất kỳ ký tự tương ứng nào từ ứng viên. Nếu đó không phải là dấu chấm, chúng tôi yêu cầu phải khớp chính xác với tính cách của ứng viên. 
5. Nếu tất cả các vị trí đều phù hợp với một ứng cử viên, hãy đánh dấu vị trí đó là hợp lệ. 
6. Sau khi kiểm tra cả ba ứng viên, hãy đếm xem có bao nhiêu ứng viên hợp lệ. 
7. Nếu có chính xác một ứng viên hợp lệ, hãy xuất nó ra. Nếu có nhiều hơn một giá trị hợp lệ, đầu ra KHÔNG THỂ TELL. Nếu không có giá trị nào hợp lệ, hãy xuất ra SOMETHING'S SAI. 

### Tại sao nó hoạt động

Mỗi dấu chấm đại diện cho một sự lựa chọn tự do, do đó nó không áp đặt ràng buộc nào đối với chuỗi ứng cử viên. Mỗi ký tự không phải dấu chấm là một ràng buộc cứng phải được thỏa mãn. Do đó, tính hợp lệ giảm xuống còn việc kiểm tra xem chuỗi ứng cử viên có phù hợp với tất cả các ràng buộc cố định hay không. Vì chúng tôi xác minh độc lập tất cả các ràng buộc đối với từng ứng viên nên chúng tôi không thể chấp nhận sai tên vi phạm bất kỳ ký tự cố định nào và chúng tôi không thể từ chối tên trừ khi nó xung đột với ít nhất một ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def matches(pattern, name):
    if len(pattern) != len(name):
        return False
    for pc, nc in zip(pattern, name):
        if pc != '.' and pc != nc:
            return False
    return True

def solve():
    s = input().strip()
    candidates = ["Alice", "Bob", "Cindy"]

    valid = []
    for name in candidates:
        if matches(s, name):
            valid.append(name)

    if len(valid) == 1:
        print(valid[0])
    elif len(valid) > 1:
        print("CAN'T TELL")
    else:
        print("SOMETHING'S WRONG")

if __name__ == "__main__":
    solve()
```Hàm so khớp mã hóa ý tưởng cốt lõi: các dấu chấm bị bỏ qua dưới dạng ràng buộc, trong khi tất cả các ký tự khác phải khớp chính xác. Vòng lặp chính chỉ lọc ba ứng cử viên. 

Một lỗi phổ biến là quên phân biệt chữ hoa chữ thường, đặc biệt khi so sánh các đầu vào như "bob" với "Bob". điều kiện`pc != nc`thực thi sự kết hợp chặt chẽ. Một điểm tinh tế khác là tính nhất quán về độ dài: mặc dù vấn đề nêu rõ độ dài luôn chính xác, bao gồm cả việc kiểm tra sẽ ngăn ngừa việc vô tình sử dụng sai trong các biến thể mở rộng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
Ali.e
```| Vị trí | Mẫu | Alice | Có hiệu lực cho đến nay | 
| --- | --- | --- | --- | 
| 1 | A | A | vâng | 
| 2 | tôi | tôi | vâng | 
| 3 | tôi | tôi | vâng | 
| 4 | . | c | vâng | 
| 5 | e | e | vâng | 

Chỉ có Alice vẫn còn hiệu lực. 

Đầu ra:```
Alice
```Dấu vết này cho thấy cách một ký tự đại diện duy nhất cho phép hoàn thành một kết quả khớp gần như hoàn hảo. 

### Ví dụ 2 

đầu vào:```
bob
```| Ứng viên | Kết quả bước | 
| --- | --- | 
| Alice | ký tự đầu tiên không khớp | 
| Bob | không khớp do trường hợp | 
| Cindy | ký tự đầu tiên không khớp | 

Không có ứng cử viên nào sống sót ngay cả bước so sánh đầu tiên. 

Đầu ra:```
SOMETHING'S WRONG
```Điều này thể hiện sự phân biệt chữ hoa chữ thường nghiêm ngặt và sự bác bỏ ngay lập tức đối với xung đột. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(3 · n) | Mỗi trong số ba ứng cử viên được so sánh từng ký tự với một chuỗi có độ dài tối đa 5 | 
| Không gian | O(1) | Chỉ sử dụng một số chuỗi và bộ đếm cố định | 

Các ràng buộc làm cho thời gian này không đổi một cách hiệu quả. Ngay cả trong phiên bản tổng quát có tên dài hơn, giải pháp vẫn tuyến tính về số lượng ứng cử viên và độ dài chuỗi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    def matches(pattern, name):
        if len(pattern) != len(name):
            return False
        for pc, nc in zip(pattern, name):
            if pc != '.' and pc != nc:
                return False
        return True

    s = input().strip()
    candidates = ["Alice", "Bob", "Cindy"]

    valid = []
    for name in candidates:
        if matches(s, name):
            valid.append(name)

    if len(valid) == 1:
        return valid[0]
    elif len(valid) > 1:
        return "CAN'T TELL"
    else:
        return "SOMETHING'S WRONG"

# provided samples
assert run("Ali.e") == "Alice"
assert run("bob") == "SOMETHING'S WRONG"

# custom cases
assert run(".....") == "CAN'T TELL"
assert run("A....") == "Alice"
assert run("Cindy") == "Cindy"
assert run("B.b") == "Bob"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "......" | KHÔNG THỂ NÓI | tất cả các ứng cử viên phù hợp thông qua ký tự đại diện | 
| "A...." | Alice | ràng buộc tiền tố một phần | 
| "Cindy" | Cindy | khớp chính xác đầy đủ | 
| "B.b" | Bob | kết hợp cố định và ký tự đại diện | 

## Vỏ cạnh 

Chuỗi ký tự đại diện đầy đủ như "...." là trường hợp quan trọng nhất nhưng không rõ ràng. Mọi ứng viên đều tương thích vì mọi vị trí đều không bị giới hạn. Thuật toán đánh giá cả ba tên và đánh dấu chúng là hợp lệ, tạo ra CAN'T TELL. 

Trường hợp không khớp nghiêm ngặt như "Zzzzz" cho thấy hành vi ngược lại. Mỗi ứng cử viên đều thất bại ở lần so sánh ký tự đầu tiên, do đó danh sách hợp lệ vẫn trống và kết quả đầu ra trở thành SAI ĐÓ. 

Phân biệt chữ hoa chữ thường là một điều kiện cạnh quan trọng khác. Các thông tin đầu vào như "bob" không được khớp với "Bob" vì sự so sánh chính xác ở mọi ký tự. Thuật toán thực thi điều này bằng cách kiểm tra tính bằng nhau mà không cần chuẩn hóa, đảm bảo tính chính xác cho các trường hợp đầu vào hỗn hợp.
