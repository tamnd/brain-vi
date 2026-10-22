---
title: "CF 105900K - Koga cần bạn"
description: "Chúng tôi được cung cấp một dòng duy nhất mô tả kịch bản chiến đấu theo chu kỳ loại Pokémon được đơn giản hóa. Dòng này chứa một tiền tố cố định và sau đó là một tên Pokémon duy nhất trong số ba khả năng: Torterra, Staraptor hoặc Luxray."
date: "2026-06-21T12:24:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105900
codeforces_index: "K"
codeforces_contest_name: "VI UnBalloon Contest Mirror"
rating: 0
weight: 105900
solve_time_s: 44
verified: true
draft: false
---

[CF 105900K - Koga cần bạn](https://codeforces.com/problemset/problem/105900/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng duy nhất mô tả kịch bản chiến đấu theo chu kỳ loại Pokémon được đơn giản hóa. Dòng này chứa một tiền tố cố định và sau đó là một tên Pokémon duy nhất trong số ba khả năng: Torterra, Staraptor hoặc Luxray. Nhiệm vụ của chúng tôi là đáp lại Pokémon có lợi thế về chủng loại hơn loại đã cho. 

Các mối quan hệ tạo thành một chu kỳ chứ không phải là một hệ thống phân cấp. Torterra bị Staraptor đánh bại, Staraptor bị Luxray đánh bại và Luxray bị Torterra đánh bại. Vì vậy, đầu ra không được tính toán bằng số học hoặc sắp xếp mà bằng cách tra cứu trực tiếp trong một hoán vị cố định. 

Kích thước đầu vào không đổi trong thực tế vì chúng ta chỉ đọc một dòng và so sánh với ba chuỗi đã biết. Điều này ngay lập tức loại trừ mọi lo ngại về độ phức tạp của hiệu suất ngoài việc phân tích chuỗi đơn giản. Ngay cả việc tìm kiếm chuỗi con lặp lại đơn giản cũng đủ theo các ràng buộc. 

Trường hợp chính là độ nhạy định dạng. Đầu vào bao gồm cụm từ tiền tố và dấu câu, đồng thời tên Pokémon xuất hiện ở cuối. Ví dụ: đầu vào mẫu là`Vamos la, Torterra!`. Đầu ra phải giữ nguyên kiểu dấu câu chính xác:`Staraptor, eu escolho voce!`. Một cách tiếp cận ngây thơ không trích xuất chính xác tên hoặc quản lý sai dấu câu sẽ thất bại ngay cả khi loại logic đúng. 

Cạm bẫy tinh vi thứ hai là giả sử các mã thông báo được phân tách bằng khoảng trắng. Tên Pokémon được gắn với dấu chấm câu, do đó việc phân chia đơn giản bằng dấu cách có thể mang lại kết quả`Torterra!`thay vì`Torterra`, điều này sẽ phá vỡ ánh xạ trừ khi được làm sạch đúng cách. 

## Phương pháp tiếp cận 

Giải thích bạo lực sẽ quét chuỗi đầu vào để tìm từng tên Pokémon có thể có và quyết định dựa trên kết quả khớp chuỗi con. Vì chỉ có ba ứng cử viên nên một chuỗi có thể kiểm tra từng chuỗi bằng kiểm tra ngăn chặn và sau đó áp dụng quy tắc ánh xạ. Đây đã là thời gian có hiệu lực không đổi, bởi vì bảng chữ cái của các trạng thái hợp lệ là cố định và rất nhỏ. 

Cách tiếp cận có cấu trúc hơn trước tiên là trích xuất tên Pokémon một cách đáng tin cậy, sau đó áp dụng ánh xạ từ điển trực tiếp để mã hóa chu trình. Khi chúng tôi tách mã thông báo hậu tố, chúng tôi sẽ chuẩn hóa nó bằng cách xóa dấu câu như`!`nếu có mặt. Sau đó, chúng tôi thực hiện tra cứu một lần để xác định Pokémon chiến thắng. 

Cái nhìn sâu sắc quan trọng là toàn bộ vấn đề giảm xuống một ánh xạ hoán vị cố định. Không có tính toán động, không có phân nhánh có điều kiện ngoài bảng tra cứu. Brute-force hoạt động vì việc kiểm tra một vài chuỗi là chuyện nhỏ, nhưng nó trở nên lộn xộn về mặt khái niệm khi xử lý dấu câu. Giải pháp rõ ràng là tách phân tích cú pháp khỏi việc ra quyết định và mã hóa chu trình một cách rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra chuỗi con Brute Force | O(1) | O(1) | Đã chấp nhận | 
| Phân tích cú pháp + tra cứu tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc toàn bộ dòng đầu vào dưới dạng một chuỗi. Dòng này chứa tiền tố cố định, dấu phẩy, tên Pokémon và dấu chấm than. Chúng tôi coi nó là văn bản thô chứ không phải là mã thông báo có cấu trúc. 
2. Trích xuất đoạn giống từ cuối cùng sau dấu phẩy. Điều này hiệu quả vì tên Pokémon luôn xuất hiện ở cuối câu. Chúng tôi chia ra các khoảng trống và lấy mã thông báo cuối cùng. 
3. Xóa dấu chấm than ở cuối mã thông báo được trích xuất nếu nó tồn tại. Bước chuẩn hóa này là cần thiết vì so sánh chuỗi phải khớp chính xác với tên Pokémon. 
4. Sử dụng ánh xạ được xác định trước để mã hóa chu trình thuận lợi. Mỗi Pokémon ánh xạ tới con đánh bại nó. 
5. Xuất kết quả theo định dạng được yêu cầu:`<winner>, eu escolho voce!`. 

### Tại sao nó hoạt động 

Thuật toán dựa trên bất biến rằng chính xác một trong ba tên Pokémon đã biết sẽ xuất hiện ở cuối dòng đầu vào. Sau khi được trích xuất và chuẩn hóa, không gian trạng thái sẽ chuyển sang chu trình ba nút. Ánh xạ có tính xác định và tổng trên tập hợp này, vì vậy mọi đầu vào hợp lệ sẽ tạo ra chính xác một đầu ra hợp lệ và không có sự mơ hồ. Vì không có chuyển đổi nào ngoại trừ việc phân tích cú pháp được thực hiện nên độ chính xác sẽ giảm xuống khi trích xuất chính xác mã thông báo cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

line = input().strip()

# last token contains the Pokémon name with punctuation
token = line.split()[-1]

# remove trailing punctuation if present
if token.endswith('!'):
    token = token[:-1]

# cycle mapping: who beats whom
beat = {
    "Torterra": "Staraptor",
    "Staraptor": "Luxray",
    "Luxray": "Torterra"
}

winner = beat[token]

print(f"{winner}, eu escolho voce!")
```Giải pháp đọc đầu vào một lần và tách mã thông báo cuối cùng bằng cách sử dụng dấu cách phân tách, điều này an toàn vì tên Pokémon luôn là từ cuối cùng. Bước dọn dẹp xử lý dấu chấm than, điều này rất quan trọng vì nếu không tra cứu từ điển sẽ không thành công. 

Từ điển ánh xạ mã hóa trực tiếp toàn bộ logic trò chơi. Điều này tránh các chuỗi có điều kiện và đảm bảo tra cứu liên tục. Định dạng đầu ra được cố định và phù hợp với cấu trúc câu tiếng Bồ Đào Nha được yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`Vamos la, Torterra!`Sau khi tách và làm sạch, chúng ta nhận được: 

| Bước | Mã thông báo | 
| --- | --- | 
| Mã thông báo cuối cùng thô | Torterra! | 
| Sau khi dọn dẹp | Torterra | 

Bản đồ mang lại cho Staraptor. 

Đầu ra:`Staraptor, eu escolho voce!`Điều này thể hiện việc xử lý dấu câu chính xác và độ phân giải chu kỳ chính xác. 

### Ví dụ 2 

đầu vào:`Vamos la, Luxray!`| Bước | Mã thông báo | 
| --- | --- | 
| Mã thông báo cuối cùng thô | Luxray! | 
| Sau khi dọn dẹp | Luxray | 

Lập bản đồ mang lại cho Torterra. 

Đầu ra:`Torterra, eu escolho voce!`Điều này xác nhận tính chất tuần hoàn được triển khai chính xác, đặc biệt là trường hợp bao trùm từ Luxray trở lại Torterra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ thực hiện phân tách một chuỗi và tra cứu từ điển | 
| Không gian | O(1) | Đã sửa lỗi ánh xạ ba mục và xử lý đầu vào có kích thước không đổi | 

Các ràng buộc là tối thiểu nên giải pháp này thấp hơn nhiều so với bất kỳ giới hạn thực tế nào. Ngay cả trong những giới hạn thời gian cực kỳ nghiêm ngặt, các thao tác chuỗi trên đầu vào nhỏ như vậy là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        line = _sys.stdin.readline().strip()
        token = line.split()[-1]
        if token.endswith('!'):
            token = token[:-1]
        beat = {
            "Torterra": "Staraptor",
            "Staraptor": "Luxray",
            "Luxray": "Torterra"
        }
        print(f"{beat[token]}, eu escolho voce!")
    return out.getvalue().strip()

# provided sample
assert run("Vamos la, Torterra!") == "Staraptor, eu escolho voce!"

# cycle checks
assert run("Vamos la, Staraptor!") == "Luxray, eu escolho voce!"
assert run("Vamos la, Luxray!") == "Torterra, eu escolho voce!"

# formatting edge case: multiple spaces before last token
assert run("Vamos la,   Torterra!") == "Staraptor, eu escolho voce!"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp Torterra | Staraptor | Lập bản đồ cơ bản | 
| Trường hợp Staraptor | Luxray | Chu kỳ đúng đắn | 
| Vỏ Luxray | Torterra | Logic bao quanh | 
| Khoảng cách bổ sung | Staraptor | Phân tích cú pháp mạnh mẽ | 

## Vỏ cạnh 

Một trường hợp tinh tế là khoảng cách bổ sung giữa các mã thông báo. Ví dụ,`Vamos la,   Torterra!`vẫn sản xuất`Torterra!`làm mã thông báo phân tách cuối cùng nên việc dọn dẹp và ánh xạ vẫn hợp lệ. Thuật toán không phụ thuộc vào cấu trúc khoảng cách chính xác ngoài vị trí mã thông báo cuối cùng. 

Một trường hợp khác là đính kèm dấu chấm câu. Nếu dấu chấm than không được loại bỏ, việc tra cứu từ điển sẽ thất bại vì`"Torterra!"`không phải là một khóa hợp lệ. Bước loại bỏ rõ ràng đảm bảo chuẩn hóa trước khi tra cứu. 

Cuối cùng, trường hợp ranh giới tuần hoàn giữa Luxray và Torterra xác nhận tính đúng đắn của logic bao quanh. Ánh xạ mã hóa rõ ràng quá trình chuyển đổi này, do đó không cần xử lý cạnh có điều kiện.
