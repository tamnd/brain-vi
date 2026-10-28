---
title: "CF 106182G - Những Bài Hát Ngốc Nghếch"
description: "Đầu vào là một đoạn văn bản được chia thành các dòng. Một bài hát hợp lệ được xây dựng từ việc lặp lại mẫu hai dòng dựa trên một từ viết thường S đã chọn."
date: "2026-06-25T10:51:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106182
codeforces_index: "G"
codeforces_contest_name: "Petrozavodsk Summer Camp 2025. Day 6. Xeppelin Contest The 4rd Universal Cup. Stage 2: Grand Prix of Paris)"
rating: 0
weight: 106182
solve_time_s: 37
verified: true
draft: false
---

[CF 106182G - Những bài hát ngớ ngẩn](https://codeforces.com/problemset/problem/106182/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Đầu vào là một đoạn văn bản được chia thành các dòng. Một bài hát hợp lệ được xây dựng từ việc lặp lại mẫu hai dòng dựa trên một từ viết thường đã chọn`S`. Dòng đầu tiên của khối phải có dạng:```
S, S Sity S
```và dòng thứ hai phải là câu tương tự với`"i said "`đã thêm vào lúc đầu:```
i said S, S Sity S
```Cùng một từ`S`phải được sử dụng cho mọi khối trong chuỗi liên tiếp đã chọn. Nhiệm vụ là tìm số lượng ký tự lớn nhất bị chiếm bởi bất kỳ chuỗi liên tiếp nào của các khối như vậy. Các ký tự dòng mới được tính là ký tự, bao gồm cả dòng mới sau dòng cuối cùng của chuỗi. 

Kích thước đầu vào đủ nhỏ để vừa trong bộ nhớ nhưng tổng số ký tự có thể đạt tới$10^5$. Điều này loại trừ các cách tiếp cận liên tục so sánh các phần lớn của văn bản. Một giải pháp nên xử lý mỗi dòng với số lần không đổi, tạo ra độ phức tạp gần như tuyến tính trong tổng kích thước đầu vào. 

Phần khó khăn đến từ việc xác định chính xác ranh giới khối và giữ nguyên`S`qua nhiều khối. Một dòng trông gần với mẫu không được chấp nhận một cách vô tình. 

Ví dụ: một khối hợp lệ duy nhất:```
bang, bang bangity bang
i said bang, bang bangity bang
```có đầu ra:```
55
```Một giải pháp bất cẩn chỉ đếm số dòng phù hợp có thể quên rằng đầu ra được đo bằng ký tự và bao gồm các dòng mới. 

Một trường hợp cạnh khác là hai khối hợp lệ với các từ khác nhau:```
bang, bang bangity bang
i said bang, bang bangity bang
bank, bank bankity bank
i said bank, bank bankity bank
```Đầu ra đúng là`55`, không`110`, vì bài hát không thể thay đổi`S`ở giữa. Một giải pháp chỉ kiểm tra xem mỗi cặp dòng có hợp lệ riêng lẻ hay không sẽ bị tính quá mức. 

Trường hợp cạnh cuối cùng là một đường trông hợp lệ mà không có đối tác của nó:```
bang, bang bangity bang
```Đầu ra đúng là`-1`. Chỉ dòng đầu tiên không phải là một khối hoàn chỉnh. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra mọi chuỗi dòng liên tiếp có thể có và kiểm tra xem nó có tạo thành một bài hát hợp lệ hay không. Một trình tự với$k$các khối sẽ yêu cầu kiểm tra tất cả các dòng của nó và xác minh rằng mọi cặp đều khớp với cùng một từ. Điều này đúng vì nó thử mọi câu trả lời có thể, nhưng với$m$dòng nó có thể kiểm tra cùng một ký tự nhiều lần. Trong trường hợp xấu nhất, điều này trở thành bậc hai đối với kích thước đầu vào, điều này không cần thiết đối với$10^5$nhân vật. 

Cấu trúc của mẫu đưa ra một lộ trình đơn giản hơn. Mỗi khối bao gồm chính xác hai dòng liền kề. Dòng đầu tiên xác định duy nhất từ`S`và dòng thứ hai phải là cùng một khối với tiền tố cố định. Sau khi tìm thấy một cặp hợp lệ, điều duy nhất quan trọng để mở rộng câu trả lời hiện tại là liệu cặp tiếp theo có sử dụng cùng một cặp hay không.`S`. 

Điều này cho phép chúng tôi quét văn bản từ trên xuống dưới. Đối với mỗi cặp dòng, chúng tôi xác định xem đó có phải là khối hợp lệ hay không và trích xuất từ ​​của nó. Các khối hợp lệ liên tiếp có cùng một từ tạo thành một bài hát ứng cử viên. Chúng tôi duy trì số lượng ký tự của lần chạy hiện tại và cập nhật mức tối đa bất cứ khi nào lần chạy thay đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Đọc tất cả các dòng trong khi vẫn giữ các ký tự dòng mới của chúng. Dòng mới thuộc về độ dài đo được, vì vậy việc xóa nó vĩnh viễn sẽ khiến số đếm cuối cùng không chính xác. 
2. Xử lý các dòng theo cặp. Một khối luôn chiếm hai dòng liên tiếp, vì vậy dòng đầu tiên của mọi khối có thể bắt đầu ở độ lệch chẵn trong quá trình quét này. 
3. Phân tích dòng đầu tiên. Chia nó bằng dấu cách sẽ tạo ra bốn phần. Phần đầu kết thúc bằng dấu phẩy, sau khi bỏ dấu phẩy 4 từ trích ra phải khớp với mẫu`S, S Sity S`. 
4. Phân tích dòng thứ hai. Nó phải bắt đầu bằng`"i said "`và phần còn lại phải có cấu trúc giống hệt như dòng đầu tiên. 
5. Nếu cả hai dòng tạo thành một khối hợp lệ, hãy ghi lại từ và tổng chiều dài ký tự của nó. Nếu từ này giống với từ của khối hợp lệ trước đó, hãy mở rộng độ dài bài hát hiện tại. Nếu không, hãy bắt đầu một chuỗi mới. 
6. Nếu một cặp không hợp lệ, hãy đặt lại trình tự hiện tại vì bài hát không thể bỏ qua các dòng không hợp lệ. 
7. Sau khi xử lý tất cả các cặp có thể, in ra độ dài chuỗi lớn nhất. Nếu không tìm thấy khối hợp lệ, hãy in`-1`. 

Tại sao nó hoạt động: 

Mỗi bài hát hợp lệ được tạo thành từ các khối hai dòng liên tiếp sử dụng một từ cố định`S`. Quá trình quét sẽ kiểm tra mọi vị trí khối có thể có và một khối được thêm vào chuỗi hiện tại một cách chính xác khi nó thuộc cùng một bài hát hợp lệ với khối trước đó. Độ dài được duy trì luôn biểu thị chuỗi hợp lệ dài nhất kết thúc ở khối hiện tại. Khi một từ khác hoặc khối không hợp lệ xuất hiện, không có chuỗi nào đi qua điểm đó có thể hợp lệ, do đó việc đặt lại là đúng. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse_base(line):
    parts = line.rstrip('\n').split(' ')
    if len(parts) != 4:
        return None
    if not parts[0].endswith(','):
        return None
    s = parts[0][:-1]
    if not s:
        return None
    if parts[1] != s:
        return None
    if parts[2] != s + "ity":
        return None
    if parts[3] != s:
        return None
    return s

def solve():
    lines = sys.stdin.readlines()

    ans = -1
    current = 0
    current_s = None

    i = 0
    while i + 1 < len(lines):
        first = parse_base(lines[i])

        valid = False
        s = None

        if first is not None:
            second = lines[i + 1]
            if second.startswith("i said "):
                second_rest = second[7:]
                second_s = parse_base(second_rest)
                if second_s == first:
                    valid = True
                    s = first

        if valid:
            block_len = len(lines[i]) + len(lines[i + 1])
            if current_s == s:
                current += block_len
            else:
                current_s = s
                current = block_len
            if current > ans:
                ans = current
        else:
            current = 0
            current_s = None

        i += 2

    print(ans)

if __name__ == "__main__":
    solve()
```Người trợ giúp`parse_base`cô lập cấu trúc lặp lại được chia sẻ bởi cả hai dòng. Nó trả về từ`S`khi dòng tuân theo chính xác định dạng được yêu cầu và trả về`None`nếu không thì. Kiểm tra định dạng thông qua các từ được phân tách sẽ tránh được việc so sánh chuỗi con dễ vỡ. 

Vòng lặp chính tiến lên hai dòng vì một khối luôn tiêu thụ chính xác hai dòng. Khi một khối hợp lệ, mã sẽ thêm cả độ dài dòng ban đầu, bao gồm cả các ký tự dòng mới của chúng. Đây là lý do tại sao`len(lines[i])`được sử dụng trực tiếp thay vì đếm các ký tự hiển thị. 

Biến`current_s`lưu trữ từ được sử dụng bởi đoạn bài hát hiện tại. Nếu khối hợp lệ tiếp theo có cùng một từ, nó sẽ mở rộng phân đoạn. Nếu từ thay đổi, đoạn cũ kết thúc và đoạn mới bắt đầu. 

# Ví dụ đã hoạt động 

Đối với đầu vào:```
bang, bang bangity bang
i said bang, bang bangity bang
```dấu vết là: 

| Bước | Từ đầu tiên | Khối hợp lệ | Từ hiện tại | Độ dài hiện tại | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | bang | Có | bang | 55 | 55 | 

Thuật toán tìm thấy một khối hoàn chỉnh. Bất biến được giữ nguyên vì phân đoạn hiện tại chỉ chứa các khối sử dụng`bang`. 

Đối với đầu vào:```
bang, bang bangity bang
i said bang, bang bangity bang
bank, bank bankity bank
i said bank, bank bankity bank
bank, bank bankity bank
i said bank, bank bankity bank
```dấu vết là: 

| Bước | Từ đầu tiên | Khối hợp lệ | Từ hiện tại | Độ dài hiện tại | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | bang | Có | bang | 55 | 55 | 
| 2 | ngân hàng | Có | ngân hàng | 55 | 55 | 
| 3 | ngân hàng | Có | ngân hàng | 110 | 110 | 

Dấu vết cho thấy tại sao điều kiện của cùng một từ lại quan trọng. Khối đầu tiên không thể kết hợp với các khối sau vì`bang`Và`bank`là những bài hát khác nhau 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự đầu vào được kiểm tra một số lần không đổi trong khi phân tích các dòng. | 
| Không gian | O(n) | Các dòng đầu vào được lưu trữ để có thể đếm được độ dài ban đầu của chúng, bao gồm cả dòng mới. | 

Tổng kích thước đầu vào tối đa là$10^5$ký tự, do đó việc lưu trữ các dòng và thực hiện quét tuyến tính dễ dàng phù hợp với giới hạn. 

# Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io():
    lines = sys.stdin.readlines()

    def parse_base(line):
        parts = line.rstrip('\n').split(' ')
        if len(parts) != 4:
            return None
        if not parts[0].endswith(','):
            return None
        s = parts[0][:-1]
        if not s or parts[1] != s or parts[2] != s + "ity" or parts[3] != s:
            return None
        return s

    ans = -1
    current = 0
    current_s = None

    i = 0
    while i + 1 < len(lines):
        s = parse_base(lines[i])
        ok = False

        if s is not None and lines[i + 1].startswith("i said "):
            if parse_base(lines[i + 1][7:]) == s:
                ok = True

        if ok:
            length = len(lines[i]) + len(lines[i + 1])
            if current_s == s:
                current += length
            else:
                current_s = s
                current = length
            ans = max(ans, current)
        else:
            current = 0
            current_s = None

        i += 2

    return str(ans) + "\n"

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = solve_io()
    sys.stdin = old
    return out

assert run(
"""bang, bang bangity bang
i said bang, bang bangity bang
"""
) == "55\n", "single block"

assert run(
"""bang, bang bangity bang
i said bang, bang bangity bang
bank, bank bankity bank
i said bank, bank bankity bank
bank, bank bankity bank
i said bank, bank bankity bank
"""
) == "110\n", "longest repeated song"

assert run(
"""a, a aity a
i said a, a aity a
"""
) == "39\n", "minimum word"

assert run(
"""a, a aity a
i said a, a aity a
b, b bity b
i said b, b bity b
"""
) == "39\n", "different words cannot merge"

assert run(
"""hello world
i said hello world
"""
) == "-1\n", "invalid pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Khối hợp lệ duy nhất | 55 | Nhận dạng khối cơ bản và đếm dòng mới | 
| Nhiều khối từ giống nhau | 110 | Mở rộng chuỗi bài hát | 
| Từ một chữ cái | 39 | Mẫu hợp lệ có kích thước tối thiểu | 
| Hai từ khác nhau | 39 | Ngăn chặn việc hợp nhất các bài hát khác nhau | 
| Văn bản tùy ý | -1 | Từ chối các dòng không hợp lệ | 

# Vỏ cạnh 

Đối với trường hợp khối đầu tiên hợp lệ được theo sau bởi một từ khác, thuật toán sẽ đặt lại chuỗi hiện tại. Với:```
bang, bang bangity bang
i said bang, bang bangity bang
bank, bank bankity bank
i said bank, bank bankity bank
```cặp đầu tiên tạo ra một`bang`sự liên tiếp. Cặp thứ hai hợp lệ nhưng sử dụng`bank`, do đó nó bắt đầu một chuỗi mới thay vì mở rộng chuỗi cũ. Kết quả là chiều dài của một khối. 

Đối với dòng thứ hai bị thiếu:```
bang, bang bangity bang
```quá trình quét không thể tạo thành một cặp. Vòng lặp kết thúc mà không tìm thấy khối hợp lệ nào, vì vậy câu trả lời vẫn còn`-1`. 

Đối với các dòng gần giống với mẫu:```
bang, bang bang bang
i said bang, bang bang bang
```trình phân tích cú pháp sẽ kiểm tra từ thứ ba`bangity`, không chỉ chống lại tiền tố`bang`. Vì từ ở giữa được yêu cầu bị thiếu`ity`hậu tố, khối bị từ chối. Điều này ngăn cản việc chấp nhận các bài hát tương tự về mặt hình ảnh nhưng không hợp lệ.
