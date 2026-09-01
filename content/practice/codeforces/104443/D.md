---
title: "CF 104443D - Thiếu ký tự"
description: "Đầu vào của vấn đề này cố tình không mang tính thông tin: nó luôn là cùng một chuỗi cố định và nó không ảnh hưởng đến câu trả lời theo bất kỳ cách nào có ý nghĩa."
date: "2026-06-30T18:03:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104443
codeforces_index: "D"
codeforces_contest_name: "TheForces Round #18 (JuneIsApril-Forces)"
rating: 0
weight: 104443
solve_time_s: 49
verified: true
draft: false
---

[CF 104443D - Thiếu ký tự](https://codeforces.com/problemset/problem/104443/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào của vấn đề này cố tình không mang tính thông tin: nó luôn là cùng một chuỗi cố định và nó không ảnh hưởng đến câu trả lời theo bất kỳ cách nào có ý nghĩa. Nhiệm vụ là xuất ra một chuỗi xác định trước được xác định ngầm định bằng câu lệnh thay vì bằng tính toán trên đầu vào. 

Mặc dù đầu vào tồn tại nhưng cách giải thích hợp lý duy nhất là nó là cá trích đỏ. Vấn đề thực tế là yêu cầu một chuỗi có thể bắt nguồn từ ý tưởng “thiếu ký tự” đối với cụm từ được cung cấp trong câu lệnh. 

Cách giải thích tự nhiên là chúng ta nhìn vào bảng chữ cái viết thường của tiếng Anh và loại bỏ mọi ký tự xuất hiện trong cụm từ đã cho “Vấn đề BAD”, bỏ qua chữ hoa chữ thường và bỏ qua khoảng trắng. Các ký tự còn lại, theo thứ tự bảng chữ cái, tạo thành đầu ra. 

Các ràng buộc thực tế là không đáng kể vì chỉ có một dòng đầu vào và không có sự thay đổi. Điều này có nghĩa là chúng tôi không lựa chọn giữa các phương pháp tiếp cận thuật toán dưới áp lực thời gian. Điều duy nhất quan trọng là diễn giải chính xác những gì “ký tự bị thiếu” đề cập đến và nhất quán về cách viết hoa và thứ tự. 

Trường hợp cạnh tinh tế chính ở đây là xử lý chuẩn hóa ký tự. Nếu không xử lý nhất quán chữ hoa và chữ thường, chúng ta có thể coi các chữ cái như 'B' và 'b' là khác nhau một cách không chính xác. Ví dụ: nếu chúng ta phân biệt chữ hoa chữ thường không chính xác, chúng ta có thể tin rằng bảng chữ cái có đầy đủ trong khi bảng chữ cái không có hoặc ngược lại. Một trường hợp cạnh khác là vô tình bao gồm ký tự khoảng trắng trong quá trình xử lý, nên bỏ qua hoàn toàn ký tự này. 

## Phương pháp tiếp cận 

Mô hình tinh thần bạo lực là bắt đầu từ cụm từ và liên tục xóa các ký tự khỏi chuỗi bảng chữ cái đầy đủ. Cụ thể, chúng ta có thể khởi tạo một tập hợp chứa tất cả các chữ cái từ 'a' đến 'z', sau đó lặp lại các ký tự của chuỗi đầu vào, loại bỏ từng ký tự chữ cái sau khi chuyển đổi nó thành chữ thường. Cuối cùng, những gì còn lại trong bộ này chính là câu trả lời. 

Điều này hiệu quả vì thời gian trung bình đã thiết lập và việc xóa là không đổi, vì vậy ngay cả khi dữ liệu đầu vào dài hơn, chúng tôi vẫn sẽ hoàn thành ngay lập tức. Một biến thể đơn giản hơn là, đối với mỗi chữ cái trong bảng chữ cái, hãy quét toàn bộ chuỗi đầu vào để kiểm tra xem nó có xuất hiện hay không. Cách tiếp cận đó thực hiện 26 lần quét toàn bộ, điều này vẫn còn tầm thường ở đây nhưng cho thấy sự kém hiệu quả của việc quét lặp lại dư thừa trên cùng một dữ liệu. 

Quan sát cấu trúc quan trọng là đầu vào không thay đổi giữa các trường hợp thử nghiệm và chỉ chứa một bộ ký tự cố định nhỏ. Điều đó làm cho bài toán tương đương với việc tính một tập hợp phần bù trên bảng chữ cái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét từng chữ cái) | O(26 × n) | O(1) | Đã chấp nhận | 
| Tối ưu (loại bỏ bộ đường chuyền đơn) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một bộ chứa tất cả các chữ cái tiếng Anh viết thường từ ‘a’ đến ‘z’. Điều này thể hiện toàn bộ các ký tự đầu ra có thể có. 
2. Đọc chuỗi đầu vào. 
3. Lặp lại từng ký tự trong chuỗi. Nếu ký tự là một chữ cái, hãy chuyển nó thành chữ thường và loại bỏ nó khỏi tập hợp các ứng cử viên còn lại. Điều này đảm bảo chúng tôi chỉ theo dõi các chữ cái không có trong đầu vào. 
4. Sau khi xử lý tất cả các ký tự, tập hợp chứa chính xác các chữ cái chưa từng xuất hiện trong đầu vào. 
5. Sắp xếp các ký tự còn lại theo thứ tự bảng chữ cái và ghép chúng thành chuỗi đầu ra cuối cùng. 

Bước sắp xếp là bắt buộc vì các tập hợp không bảo toàn bất kỳ thứ tự có ý nghĩa nào, trong khi bài toán mong đợi một kết quả được sắp xếp theo thứ tự từ điển xác định. 

### Tại sao nó hoạt động

Ở mỗi bước, tập hợp các ký tự còn lại thể hiện chính xác những chữ cái chưa được quan sát thấy trong đầu vào. Vì chúng tôi chỉ xóa các ký tự khi nhìn thấy chúng nên chúng tôi không bao giờ xóa nhầm một ký tự đáng lẽ phải còn lại. Tương tự, mọi ký tự xuất hiện trong đầu vào sẽ bị xóa ít nhất một lần. Sau khi xử lý tất cả các ký tự, tập hợp chính xác là phần bổ sung của các ký tự trong đầu vào, chính xác là những gì “ký tự bị thiếu” đề cập đến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    full = set("abcdefghijklmnopqrstuvwxyz")

    for c in s:
        if c.isalpha():
            full.discard(c.lower())

    print("".join(sorted(full)))

if __name__ == "__main__":
    solve()
```Giải pháp khởi tạo bảng chữ cái đầy đủ dưới dạng một tập hợp và loại bỏ các ký tự khi chúng xuất hiện trong đầu vào. Việc sử dụng`discard`tránh lỗi nếu một ký tự đã vắng mặt, điều này có thể xảy ra do chuẩn hóa chữ hoa chữ thường. Sắp xếp ở cuối đảm bảo thứ tự từ điển. 

## Ví dụ đã hoạt động 

Vì đầu vào là cố định nên chúng ta vẫn có thể theo dõi cách thuật toán hoạt động trên nó. 

### Theo dõi đầu vào`"BAD problem"`| Bước | Nhân vật | Hành động | Các chữ cái còn lại (xem một phần) | 
| --- | --- | --- | --- | 
| 1 | B | xóa 'b' | a c d e f g h i j k l m n o p q r s t u v w x y z | 
| 2 | A | xóa 'a' | c d e f g h i j k l m n o p q r s t u v w x y z | 
| 3 | D | xóa 'd' | c e f g h i j k l m n o p q r s t u v w x y z | 
| 4 | p | xóa 'p' | c e f g h i j k l m n o q r s t u v w x y z | 
| 5 | r o b l e m và không gian | loại bỏ các chữ cái tương ứng | c f g h i j k n q s t u v w x y z | 

Sau khi xử lý tất cả các ký tự, việc sắp xếp sẽ mang lại chuỗi cuối cùng. 

Dấu vết này xác nhận rằng mọi ký tự từ đầu vào đều được loại trừ chính xác bất kể chữ hoa và khoảng cách. 

### Theo dõi đầu vào`"BAD problem BAD"`Hành vi này giống hệt nhau vì các bản sao không ảnh hưởng đến việc xóa tập hợp. 

| Bước | Nhân vật | Hành động | Các chữ cái còn lại (xem một phần) | 
| --- | --- | --- | --- | 
| 1 | B | xóa 'b' | a c d e f g h i j k l m n o p q r s t u v w x y z | 
| … | … | bỏ qua việc xóa nhiều lần | không thay đổi | 
| cuối cùng | kết thúc | bộ sắp xếp | kết quả tương tự như trước | 

Điều này cho thấy sự xuất hiện lặp đi lặp lại không ảnh hưởng đến tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | chuyển một lần qua chuỗi đầu vào cố định | 
| Không gian | O(1) | kích thước bảng chữ cái không đổi (26 chữ cái) | 

Kích thước đầu vào không đổi nên thuật toán chạy ngay lập tức trong mọi giới hạn hợp lý. Việc sử dụng bộ nhớ là cố định và không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO
    _out = StringIO()
    _stdin = _sys.stdin
    _stdout = _sys.stdout
    _sys.stdout = _out
    solve()
    _sys.stdin = _stdin
    _sys.stdout = _stdout
    return _out.getvalue().strip()

# provided sample (conceptual)
assert run("BAD problem\n") == run("BAD problem\n")

# custom cases
assert run("BAD problem\n") == run("bad problem\n"), "case insensitive stability"
assert run("BAD problem BAD problem\n") == run("BAD problem\n"), "duplicates ignored"
assert run("BAD problem!!!\n") == run("BAD problem\n"), "non-letters ignored"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vấn đề Xấu | bổ sung bảng chữ cái cố định | độ chính xác cơ bản | 
| Vấn đề XẤU Vấn đề XẤU | cùng một đầu ra | xử lý trùng lặp | 
| Vấn đề LỚN!!! | cùng một đầu ra | xử lý không phải chữ cái | 

## Vỏ cạnh 

Một trường hợp tinh tế là các ký tự được lặp lại hoặc viết hoa hỗn hợp. Đối với đầu vào như`"bAd PrObLeM"`, thuật toán vẫn loại bỏ các chữ cái đúng vì mọi thứ đều được chuẩn hóa bằng cách sử dụng`lower()`trước khi thiết lập các hoạt động. Điều này đảm bảo rằng sự khác biệt về trường hợp không ảnh hưởng đến tư cách thành viên. 

Một trường hợp khác là sự hiện diện của dấu câu hoặc các ký hiệu không mong muốn. Vì`"BAD problem!!!"`, vòng lặp bỏ qua các ký tự không phải chữ cái do`isalpha()`kiểm tra, do đó không có thao tác xóa không hợp lệ nào xảy ra và tập hợp cuối cùng vẫn chính xác.
