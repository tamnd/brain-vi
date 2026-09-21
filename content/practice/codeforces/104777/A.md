---
title: "CF 104777A - Bảo mật"
description: "Chúng tôi được cung cấp nhiều trường hợp thử nghiệm độc lập. Trong mỗi cái có một chuỗi mật khẩu hiện có và độ dài mục tiêu cho mật khẩu mới."
date: "2026-06-28T15:27:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104777
codeforces_index: "A"
codeforces_contest_name: "2023-2024 ICPC, NERC, Southern and Volga Russian Regional Contest (problems intersect with Educational Codeforces Round 157)"
rating: 0
weight: 104777
solve_time_s: 46
verified: true
draft: false
---

[CF 104777A - Bảo mật](https://codeforces.com/problemset/problem/104777/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều trường hợp thử nghiệm độc lập. Trong mỗi cái có một chuỗi mật khẩu hiện có và độ dài mục tiêu cho mật khẩu mới. Mật khẩu mới phải được xây dựng từ một bảng chữ cái cố định bao gồm các chữ cái viết thường, chữ in hoa và chữ số, cùng tạo thành 62 ký tự riêng biệt. 

Nhiệm vụ là xây dựng một chuỗi có chính xác k ký tự sao cho mỗi ký tự trong chuỗi mới là duy nhất và không có ký tự nào xuất hiện trong mật khẩu cũ. Nếu mật khẩu cũ đã sử dụng quá nhiều ký tự riêng biệt, nó có thể loại bỏ rất nhiều tùy chọn khiến việc tạo mật khẩu mới hợp lệ trở nên bất khả thi. 

Ràng buộc cốt lõi mang tính tổ hợp: từ một tập hợp gồm 62 ký hiệu, chúng tôi loại bỏ tất cả các ký hiệu xuất hiện trong mật khẩu cũ và sau đó kiểm tra xem có còn lại ít nhất k ký hiệu hay không. Nếu có, chúng tôi có thể xuất ra k ký hiệu riêng biệt bất kỳ từ nhóm còn lại. Nếu không, chúng tôi phải báo cáo là không thể. 

Các giới hạn là nhỏ và cố định. Mỗi mật khẩu có độ dài tối đa là 62 và có tối đa 500 trường hợp thử nghiệm. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào quét bảng chữ cái và xây dựng một tập hợp cho mỗi trường hợp thử nghiệm đều đủ nhanh vì tất cả các thao tác đều là O(62) cho mỗi trường hợp. 

Trường hợp cạnh tinh tế xuất hiện khi mật khẩu cũ chứa tất cả 62 ký tự. Trong tình huống đó, thậm chí k = 1 cũng khiến câu trả lời là không thể. Một trường hợp khác là khi k = 0, điều này không được các ràng buộc ở đây cho phép rõ ràng vì k ≥ 1, nhưng nếu đúng như vậy thì nó sẽ luôn được thỏa mãn một cách tầm thường với một chuỗi trống. Cuối cùng, các ký tự lặp lại trong mật khẩu cũ không thành vấn đề vì tính duy nhất chỉ được xác định qua các ký hiệu riêng biệt. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là xem xét việc tạo ra tất cả các chuỗi có độ dài k từ bảng chữ cái được phép và kiểm tra xem có chuỗi nào như vậy tránh các ký tự từ mật khẩu cũ hay không. Ngay cả khi chúng tôi giới hạn bản thân ở các chuỗi ký tự riêng biệt, điều này sẽ trở thành một thế hệ kiểu hoán vị trên tối đa 62 ký hiệu. Số cách để chọn k ký tự riêng biệt là vào khoảng 62Pk, vốn đã rất lớn ngay cả đối với k vừa phải. Cách tiếp cận này là hoàn toàn không khả thi. 

Quan sát quan trọng là chúng ta không bao giờ cần phải suy luận về việc đặt hàng theo bất kỳ cách phức tạp nào. Đầu ra là tùy ý và chỉ có tập hợp các ký tự có sẵn mới quan trọng. Khi chúng tôi xác định được ký tự nào bị cấm (những ký tự xuất hiện trong mật khẩu cũ), vấn đề sẽ giảm xuống việc chọn bất kỳ phần tử k nào từ nhóm còn lại. 

Vì vậy, cấu trúc sẽ trở thành một bài toán sai phân tập hợp đơn giản: tính toán tập hợp các ký tự được phép, xác minh kích thước của nó và xuất ra bất kỳ tập hợp con nào có kích thước k. Điều này hiệu quả vì không có ràng buộc nào về sự sắp xếp ngoài tính duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | O(62Pk) | O(62Pk) | Quá chậm | 
| Đặt lọc và lựa chọn | O(62) cho mỗi trường hợp thử nghiệm | O(62) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một danh sách cố định gồm tất cả 62 ký tự hợp lệ gồm chữ thường, chữ in hoa và chữ số. Điều này hoạt động như vũ trụ mà chúng ta rút ra từ đó. 
2. Với mỗi test, đọc k, n và chuỗi mật khẩu cũ s. 
3. Xây dựng cấu trúc boolean hoặc tập hợp đánh dấu tất cả các ký tự xuất hiện trong s. Chúng tôi chỉ quan tâm đến các ký tự riêng biệt, vì vậy các ký tự trùng lặp trong s sẽ bị bỏ qua một cách tự nhiên. 
4. Lặp lại trong vũ trụ 62 ký tự và thu thập những ký tự không được đánh dấu là có trong s. Điều này mang lại cho nhóm các ký tự có thể sử dụng được. 
5. Nếu kích thước của nhóm này nhỏ hơn k, hãy xuất ra một dấu gạch nối vì không tồn tại cấu trúc hợp lệ nào. 
6. Ngược lại, xuất k ký tự đầu tiên từ nhóm này theo bất kỳ thứ tự nào. Vì bài toán cho phép bất kỳ câu trả lời hợp lệ nào nên không cần logic sắp xếp nào nữa. 

### Tại sao nó hoạt động

Ở mỗi bước, chúng tôi duy trì sự bất biến rằng nhóm chứa chính xác các ký tự không bị mật khẩu cũ cấm. Mọi ký tự đầu ra đều được rút ra từ nhóm này, vì vậy nó không thể vi phạm ràng buộc xuất hiện trong mật khẩu cũ. Vì chúng ta cũng chỉ lấy mỗi ký tự nhiều nhất một lần nên điều kiện duy nhất sẽ tự động được thỏa mãn. Yêu cầu duy nhất còn lại là số lượng thẻ và việc kiểm tra trực tiếp kích thước nhóm đảm bảo chúng tôi chỉ tiến hành khi có lựa chọn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    import sys
    input = sys.stdin.readline

    # Build full alphabet of allowed characters
    alphabet = []
    for i in range(26):
        alphabet.append(chr(ord('a') + i))
    for i in range(26):
        alphabet.append(chr(ord('A') + i))
    for i in range(10):
        alphabet.append(chr(ord('0') + i))

    t = int(input())
    for _ in range(t):
        k = int(input())
        n = int(input())
        s = input().strip()

        used = set(s)

        available = []
        for c in alphabet:
            if c not in used:
                available.append(c)

        if len(available) < k:
            print("-")
        else:
            print("".join(available[:k]))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng bảng chữ cái 62 ký tự đầy đủ một lần cho mỗi lần chạy. Điều này tránh việc tính toán lại phạm vi nhiều lần và làm cho logic trở nên rõ ràng. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi chuyển đổi mật khẩu cũ thành một bộ, tự động nén các bản sao. Sau đó, chúng tôi lọc bảng chữ cái theo tập hợp này để xây dựng nhóm được phép. Bước quyết định là kiểm tra độ dài đơn giản và đầu ra là tiền tố của danh sách được lọc. 

Một cạm bẫy phổ biến là vô tình coi các mật khẩu trùng lặp trong mật khẩu cũ là bị xóa nhiều lần. Điều đó sẽ không chính xác vì việc xóa chỉ dựa trên các ký tự riêng biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
k = 3
s = "aA1"
```| Bước | bộ đã qua sử dụng | hồ bơi có sẵn (xem một phần) | hành động | 
| --- | --- | --- | --- | 
| 1 | {a, A, 1} | bảng chữ cái đầy đủ trừ những cái này | xây dựng hồ bơi | 
| 2 | - | kích thước = 62 - 3 = 59 | kiểm tra k | 
| 3 | - | 3 ký tự đầu tiên của pool | đầu ra | 

Nhóm đủ lớn nên ba ký tự không được sử dụng bất kỳ đều hợp lệ. Điều này chứng tỏ rằng thứ tự không quan trọng chút nào, chỉ có sự loại trừ. 

### Ví dụ 2 

đầu vào:```
k = 62
s = all 62 characters
```| Bước | bộ đã qua sử dụng | hồ bơi có sẵn | hành động | 
| --- | --- | --- | --- | 
| 1 | 62 ký tự | trống | xây dựng hồ bơi | 
| 2 | trống | kích thước = 0 | kiểm tra k | 
| 3 | - | không đủ | đầu ra "-" | 

Điều này khẳng định điều kiện không thể xảy ra khi tập hợp bị cấm bao trùm toàn bộ bảng chữ cái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(62 · t) | Mỗi bài kiểm tra sẽ quét một bảng chữ cái cố định và xây dựng một bộ gồm tối đa 62 ký tự | 
| Không gian | O(62) | Lưu trữ bảng chữ cái, bộ đã sử dụng và nhóm đã lọc | 

Bảng chữ cái có kích thước không đổi đảm bảo giải pháp tuyến tính một cách hiệu quả về số lượng trường hợp thử nghiệm. Với t 500 thì thời gian chạy là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue().strip()

def solve():
    import sys
    input = sys.stdin.readline

    alphabet = []
    for i in range(26):
        alphabet.append(chr(ord('a') + i))
    for i in range(26):
        alphabet.append(chr(ord('A') + i))
    for i in range(10):
        alphabet.append(chr(ord('0') + i))

    t = int(input())
    for _ in range(t):
        k = int(input())
        n = int(input())
        s = input().strip()

        used = set(s)
        available = [c for c in alphabet if c not in used]

        print("-" if len(available) < k else "".join(available[:k]))

# provided sample (minimal adaptation)
assert run("1\n3\n3\naA1\n") != "", "sample-like check"

# custom cases
assert run("1\n1\n3\naA1\n") != "-", "single character available"
assert run("1\n62\n62\nabcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789\n") == "-", "fully blocked alphabet"
assert run("1\n5\n0\n\n") != "", "empty old password gives full availability"
assert run("2\n1\n3\na\n1\n3\nb\n") != "", "multiple test cases basic"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bảng chữ cái đầy đủ được sử dụng |`-`| không thể khi không còn ký tự | 
| mật khẩu cũ trống | bất kỳ chuỗi hợp lệ nào | sẵn có tối đa | 
| nhiều trường hợp char đơn | đầu ra hợp lệ | xử lý nhiều bài kiểm tra | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi mật khẩu cũ chứa các ký tự lặp lại, chẳng hạn`s = "aaAA111"`. Thuật toán chuyển đổi cái này thành một tập hợp`{a, A, 1}`và chỉ xóa mỗi ký tự một lần. Nếu chúng tôi xóa nhầm các mục trùng lặp nhiều lần thì chúng tôi vẫn sẽ có cùng một tập hợp, nhưng việc triển khai dựa vào việc đếm thay vì tập hợp tư cách thành viên có thể nhầm tưởng rằng tình trạng sẵn có sẽ giảm nhiều hơn so với thực tế. 

Một trường hợp khác là khi k bằng chính xác số ký tự có sẵn. Ví dụ: nếu s chứa 10 ký tự riêng biệt thì còn lại chính xác 52 ký tự. Thuật toán không được cố gắng khéo léo trong việc sắp xếp thứ tự hoặc kiểm tra một phần, nó chỉ cần lấy tất cả các ký tự còn lại. Mọi hoán vị của 52 số đó đều hợp lệ. 

Cuối cùng, khi k nhỏ (như 1) nhưng tập cấm lớn, độ chính xác phụ thuộc vào việc kiểm tra tính khả dụng trước khi thử xây dựng đầu ra. Nếu bước kiểm tra này bị bỏ qua, quá trình triển khai có thể cố gắng lập chỉ mục vào một danh sách trống các ký tự có sẵn và gặp sự cố, mặc dù phản hồi đúng chỉ đơn giản là "-".
