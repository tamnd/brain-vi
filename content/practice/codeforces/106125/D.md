---
title: "CF 106125D - Dralinpome"
description: "Chúng ta được cấp một chuỗi chữ thường và được yêu cầu quyết định xem các chữ cái của nó có thể được sắp xếp lại để tạo thành một bảng màu hay không. Nhiệm vụ không phải là xây dựng bảng màu mà chỉ để xác định xem sự sắp xếp lại như vậy có tồn tại hay không."
date: "2026-06-20T01:40:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106125
codeforces_index: "D"
codeforces_contest_name: "Delft Algorithm Programming Contest 2025 (DAPC 2025)"
rating: 0
weight: 106125
solve_time_s: 52
verified: true
draft: false
---

[CF 106125D - Dralinpome](https://codeforces.com/problemset/problem/106125/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi chữ thường và được yêu cầu quyết định xem các chữ cái của nó có thể được sắp xếp lại để tạo thành một bảng màu hay không. Nhiệm vụ không phải là xây dựng bảng màu mà chỉ để xác định xem sự sắp xếp lại như vậy có tồn tại hay không. 

Một palindrome có đặc tính là các ký tự phản chiếu xung quanh tâm của nó. Điều này ngay lập tức hạn chế số lần mỗi nhân vật có thể xuất hiện. Các ký tự phải được ghép đối xứng, ngoại trừ một ký tự ở giữa khi độ dài là số lẻ. 

Độ dài đầu vào có thể lên tới 100000, loại trừ mọi cách tiếp cận bậc hai hoặc tệ hơn. Bất cứ điều gì cố gắng mô phỏng hoán vị hoặc xây dựng các chuỗi ứng cử viên sẽ bùng nổ về mặt tổ hợp. Ngay cả các cấu trúc dựa trên sắp xếp cũng được, nhưng bất cứ điều gì liên quan đến việc sắp xếp lại tất cả các hoán vị đều không thể thực hiện được. 

Một sai lầm ngây thơ xuất hiện khi chỉ tập trung vào việc liệu bản thân chuỗi có phải là một bảng màu hay không hơn là liệu nó có thể được hoán vị thành một chuỗi hay không. Ví dụ: một chuỗi như`cacao`không phải là một bảng màu như đã viết, nhưng nó có thể được sắp xếp lại thành`acoca`, hợp lệ. 

Một trường hợp phức tạp khác là các chuỗi có nhiều ký tự có tần số lẻ. Ví dụ,`aabbcc`không thể tạo thành bất kỳ bảng màu nào vì nhiều hơn một ký tự sẽ cần chiếm vị trí trung tâm. Việc triển khai bất cẩn chỉ kiểm tra tính chẵn lẻ của tổng độ dài sẽ chấp nhận sai những trường hợp như vậy. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ thử tất cả các hoán vị của chuỗi và kiểm tra xem có bất kỳ hoán vị nào trong số đó là một bảng màu hay không. Điều này đúng về mặt khái niệm vì nó trực tiếp phù hợp với định nghĩa của dralinpome, nhưng nó không khả thi. Số lượng hoán vị là n giai thừa và mỗi phép kiểm tra palindrome là tuyến tính, dẫn đến thời gian chạy vô cùng lớn ngay cả đối với n vừa phải như 20. 

Quan sát quan trọng là chúng tôi không quan tâm đến sự sắp xếp mà chỉ quan tâm đến việc đếm tần số. Trong một bảng màu, các ký tự phải xuất hiện theo cặp phản chiếu xung quanh trung tâm. Điều này có nghĩa là mọi số ký tự phải là số chẵn, ngoại trừ một ký tự có thể có số lẻ nếu độ dài chuỗi là số lẻ. 

Vì vậy, toàn bộ vấn đề giảm xuống việc đếm xem có bao nhiêu ký tự xuất hiện với số lần lẻ. Nếu số lượng này nhiều nhất là một, thì sẽ tồn tại sự sắp xếp lại palindrome. Điều này tránh được bất kỳ cấu trúc nào và nén vấn đề thành một tần số duy nhất truyền qua chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Đếm tần số | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi bằng cách đếm số lần xuất hiện của từng ký tự, sau đó đánh giá xem có bao nhiêu số trong số đó là số lẻ. 

1. Đếm tần số của mỗi ký tự trong chuỗi. Điều này đưa ra một bản tóm tắt đầy đủ về số lần mỗi chữ cái xuất hiện, không phụ thuộc vào thứ tự. 
2. Tra cứu bảng tần số và đếm xem có bao nhiêu ký tự có tần số lẻ. Mỗi tần số lẻ biểu thị một ký tự chưa ghép đôi cần chiếm vị trí phản chiếu trong bảng màu. 
3. Nếu số lượng ký tự tần số lẻ lớn hơn một, hãy kết luận ngay rằng không thể sắp xếp lại bảng màu. 
4. Ngược lại, kết luận rằng có sự sắp xếp lại hợp lệ. 

### Tại sao nó hoạt động 

Một palindrome ghép các ký tự đối xứng xung quanh tâm của nó. Mọi vị trí i từ bên trái phải khớp với vị trí tương ứng từ bên phải. Điều này buộc các lần xuất hiện của mỗi ký tự phải được chia thành từng cặp, ngoại trừ một ký tự còn sót lại có thể nằm ở giữa khi độ dài là số lẻ. Nếu có nhiều hơn một ký tự xuất hiện không theo cặp, không có cách nào để đặt chúng mà không phá vỡ tính đối xứng, do đó không hoán vị nào có thể tạo thành một bảng màu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    s = input().strip()
    
    freq = [0] * 26
    for ch in s:
        freq[ord(ch) - ord('a')] += 1
    
    odd_count = 0
    for f in freq:
        if f % 2 == 1:
            odd_count += 1
    
    if odd_count <= 1:
        print("yes")
    else:
        print("no")

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách đọc chuỗi và xây dựng mảng tần số có kích thước cố định cho các chữ cái viết thường. Điều này tránh chi phí từ điển và giữ cho các hoạt động không đổi trên mỗi ký tự. 

Sau đó chúng tôi quét 26 số đếm và tích lũy số lẻ. Logic quyết định là một bản dịch trực tiếp của điều kiện sắp xếp lại palindrome. 

Một cạm bẫy triển khai phổ biến là sử dụng tính chẵn lẻ độ dài chuỗi thay vì tính chẵn lẻ tần số. Điều đó không chính xác vì các chuỗi có độ dài chẵn vẫn có thể không được thực hiện (ví dụ:`abcabcx`) và các chuỗi có độ dài lẻ vẫn có thể hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`zoo`Chúng tôi tính toán tần số ký tự. 

| Nhân vật | Đếm | Số lẻ? | 
| --- | --- | --- | 
| z | 1 | vâng | 
| o | 2 | không | 

Số lượng số lẻ là 1. 

Điều này thể hiện trường hợp cho phép một ký tự ở giữa. Chuỗi có thể được sắp xếp lại thành`oz o`phong cách palindrome như`oz o`→`ozo`. 

### Ví dụ 2:`racecars`| Nhân vật | Đếm | Số lẻ? | 
| --- | --- | --- | 
| r | 2 | không | 
| một | 2 | không | 
| c | 2 | không | 
| e | 1 | vâng | 
| s | 2 | không | 

Số lẻ là 1 nên câu trả lời là hợp lệ. 

Điều này cho thấy rằng thậm chí các chuỗi dài hơn có cấu trúc không đều vẫn có thể tạo thành một bảng màu miễn là chỉ có một ký tự không được ghép nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần và chúng tôi quét một mảng có kích thước 26 cố định | 
| Không gian | O(1) | Kích thước mảng tần số không đổi | 

Quét tuyến tính là tối ưu vì mỗi ký tự phải được đọc ít nhất một lần. Các ràng buộc cho phép tối đa 100000 ký tự, được xử lý thoải mái trong một lần truyền. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    
    s = input().strip()
    freq = [0]*26
    for c in s:
        freq[ord(c)-97] += 1
    
    odd = 0
    for f in freq:
        if f % 2:
            odd += 1
    
    print("yes" if odd <= 1 else "no")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples (conceptual, since formatting may vary)
assert run("zoo\n") == "yes"
assert run("racecars\n") == "no"

# custom cases
assert run("a\n") == "yes"
assert run("ab\n") == "no"
assert run("aabbcc\n") == "yes"
assert run("abcabcx\n") == "yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`| vâng | trường hợp cạnh ký tự đơn | 
|`ab`| không | nhiều số lẻ | 
|`aabbcc`| vâng | tất cả các tần số chẵn | 
|`abcabcx`| vâng | đúng một tần số lẻ | 

## Vỏ cạnh 

Một chuỗi tối thiểu như`a`chứa một ký tự có tần số lẻ và thuật toán đếm chính xác một mục nhập lẻ và chấp nhận nó. Điều này tương ứng với một palindrome tầm thường. 

Một chuỗi như`ab`tạo ra hai tần số lẻ, một tần số cho mỗi ký tự. Quá trình quét qua mảng tần số sẽ tăng bộ đếm số lẻ hai lần và điều kiện cuối cùng sẽ loại bỏ nó, phù hợp với việc không thể ghép cả hai ký tự trong một cấu trúc đối xứng. 

Một trường hợp có cấu trúc lớn hơn như`abcabcx`cho thấy thứ tự đó không có ý nghĩa. Tần số là`{a:2, b:2, c:2, x:1}`, do đó chỉ tồn tại một số lẻ. Thuật toán chấp nhận nó, phản ánh chính xác rằng`abcxcba`là một sự sắp xếp lại hợp lệ.
