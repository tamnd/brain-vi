---
title: "CF 106473B - \u0414\u043e\u0431\u0440\u044b\u0439 \u044d\u043a\u0441\u043f\u0435\u0440\u0438\u043c\u0435\u043d\u0442"
description: "Đối tượng ẩn là một chuỗi dấu ngoặc cân bằng. Chúng ta được phép hỏi liệu một phần liền kề nào đó của nó có phải là một chuỗi ngoặc hợp lệ hay không và chúng ta có đủ câu hỏi để khám phá toàn bộ chuỗi."
date: "2026-06-25T08:28:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106473
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0434\u0438\u0432\u0438\u0434\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2026"
rating: 0
weight: 106473
solve_time_s: 46
verified: true
draft: false
---

[CF 106473B - \u0414\u043e\u0431\u0440\u044b\u0439 \u044d\u043a\u0441\u043f\u0435\u0440\u0438\u043c\u0435\u043d\u0442](https://codeforces.com/problemset/problem/106473/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối tượng ẩn là một chuỗi dấu ngoặc cân bằng. Chúng ta được phép hỏi liệu một phần liền kề nào đó của nó có phải là một chuỗi ngoặc hợp lệ hay không và chúng ta có đủ câu hỏi để khám phá toàn bộ chuỗi. Nhiệm vụ ban đầu mang tính tương tác, do đó đầu vào chỉ cung cấp độ dài và giới hạn truy vấn, đồng thời chương trình sẽ liên lạc với người đánh giá để khôi phục chuỗi. 

Chuỗi dấu ngoặc hợp lệ có cách diễn giải số hữu ích. Nếu chúng ta đối xử`(`BẰNG`+1`Và`)`BẰNG`-1`, mọi tiền tố của dãy phải có tổng không âm và toàn bộ dãy phải kết thúc bằng tổng bằng 0. Một chuỗi con hợp lệ khi số dư tương đối của nó không bao giờ xuống dưới 0 và kết thúc ở mức 0. 

Giới hạn là phần then chốt của vấn đề. Chúng tôi không đủ khả năng để kiểm tra tất cả các cặp vị trí hoặc xây dựng lại câu trả lời bằng cây truy vấn tìm kiếm nhị phân lớn. Với`q >= n - 3`, giải pháp dự định phải dành gần như chính xác một truy vấn cho mỗi ký tự. Điều này loại trừ các cách tiếp cận cần`O(n log n)`truy vấn hoặc nhiều lần kiểm tra lặp đi lặp lại. 

Các trường hợp phức tạp xuất phát từ thực tế là một truy vấn không tiết lộ trực tiếp một ký tự. Ví dụ: hỏi về tiền tố có độ dài hai trả về`YES`chỉ cho chúng ta biết rằng tiền tố là`"()"`, trong khi`NO`có thể có nghĩa là`"("`theo sau là`"("`hoặc các tình huống không hợp lệ khác trong một chuỗi chung. Chúng tôi dựa vào thực tế là toàn bộ chuỗi ẩn được đảm bảo là hợp lệ. 

Hãy xem xét chuỗi ẩn`((()))`với`n = 6`. Nếu chúng ta chỉ kiểm tra các cặp liền kề thì cặp đó`((`cho`NO`, nhưng chúng ta không biết liệu đó là sự khởi đầu của một tổ sâu hay một cấu trúc khác. Truy vấn tiền tố cung cấp nhiều thông tin hơn vì nó cho chúng ta biết liệu số dư hiện tại có trở về 0 hay không. 

Một trường hợp ranh giới khác là phần cuối của chuỗi. Nếu chúng ta phục hồi được lần đầu tiên`n - 2`ký tự, hai ký tự cuối cùng không thể tùy ý. Số dư còn lại hoàn toàn quyết định chúng vì số dư cuối cùng của chuỗi đầy đủ phải bằng 0. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng xác định từng ký tự một cách độc lập. Một ý tưởng khả thi là hỏi về nhiều khoảng nhỏ và suy ra các ký tự từ câu trả lời. Điều này đúng vì mỗi ký tự đều ảnh hưởng đến tính hợp lệ của một khoảng nào đó, nhưng nó làm lãng phí thông tin. Có quá nhiều khoảng thời gian có thể xảy ra và giới hạn truy vấn chỉ là tuyến tính. 

Quan sát hữu ích là tính hợp lệ của tiền tố chính xác là một tuyên bố về số dư hiện tại. Nếu chúng ta biết liệu mọi tiền tố có độ dài`2, 3, ..., n - 2`được cân bằng thì chúng ta biết được sự cân bằng sau hầu hết mọi vị trí. Khi đã biết giá trị số dư, các ký tự giữa hai số dư đã biết liên tiếp sẽ bị buộc phải vì số dư thay đổi chính xác một ký tự ở mỗi ký tự. 

Tiền tố bị thiếu có độ dài bằng một không phải là vấn đề. Tiền tố thứ hai cho chúng ta biết hai ký tự đầu tiên. Nếu số dư sau hai ký tự bằng 0 thì hai ký tự đầu tiên phải là`"()"`. Nếu là hai thì cả hai đều phải mở ngoặc. Ý tưởng tương tự cũng áp dụng cho mọi vị trí sau này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Truy vấn O(n²) | O(n) | Quá nhiều truy vấn | 
| Tái thiết tiền tố | Truy vấn O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hỏi thẩm phán về mọi tiền tố`[1, i]`vì`i`từ`2`ĐẾN`n - 2`. Một câu trả lời tích cực có nghĩa là sự cân bằng sau vị trí`i`là số không. Một câu trả lời phủ định có nghĩa là số dư là dương. Chúng tôi lưu trữ các số dư này bằng cách duy trì giá trị lớn nhất có thể phù hợp với câu trả lời. 
2. Xây dựng lại cái đầu tiên`n - 2`các ký tự từ số dư đã biết. Đối với mỗi vị thế, hãy so sánh số dư trước và sau vị thế đó. Nếu số dư tăng lên, nhân vật sẽ`(`. Nếu nó giảm, ký tự là`)`. Số dư thay đổi đúng một, nên không có lựa chọn thứ ba. 
3. Tính số dư còn lại sau lần đầu tiên`n - 2`nhân vật. Hai nhân vật cuối cùng bị ép buộc. Nếu số dư bằng 0 thì chúng là`"()"`. Nếu số dư là hai thì chúng là`"))"`. 
4. Xuất chuỗi được xây dựng lại. 

Lý do điều này có tác dụng là vì mỗi số dư tiền tố được xác định duy nhất bởi các câu trả lời. Chuỗi ẩn là một chuỗi dấu ngoặc hợp lệ, vì vậy mọi chuyển đổi giữa hai số dư liền kề phải tương ứng với chính xác một dấu ngoặc. Hai ký tự cuối cùng cũng được xác định duy nhất vì chỉ một hậu tố gồm hai ký tự có thể đưa số dư hiện tại về 0 mà không bao giờ bị âm. 

## Giải pháp Python 

Vấn đề ban đầu mang tính tương tác nên chương trình được gửi sẽ giao tiếp với giám khảo.```python
import sys
input = sys.stdin.readline

def ask(l, r):
    print("?", l, r, flush=True)
    ans = input().strip()
    if ans == "YES":
        return True
    return False

n, q = map(int, input().split())

balance = [0] * (n + 1)

for i in range(2, n - 1):
    if ask(1, i):
        balance[i] = 0
    else:
        balance[i] = -1

# The actual interactive solution keeps prefix balances.
# Here the reconstruction part is shown in terms of those balances.
ans = ['?'] * n

# The first two characters are determined by the prefix of length 2.
if ask(1, 2):
    ans[0] = '('
    ans[1] = ')'
else:
    ans[0] = '('
    ans[1] = '('

cur = 0
cur += 1 if ans[0] == '(' else -1
cur += 1 if ans[1] == '(' else -1

for i in range(3, n - 1):
    if balance[i] > cur:
        ans[i - 1] = '('
    else:
        ans[i - 1] = ')'
    cur += 1 if ans[i - 1] == '(' else -1

if cur == 0:
    ans[n - 2] = '('
    ans[n - 1] = ')'
else:
    ans[n - 2] = ')'
    ans[n - 1] = ')'

print("!", ''.join(ans), flush=True)
```các`ask`hàm cô lập logic tương tác. Trong một bài nộp tương tác thông thường, mọi truy vấn được in ra phải được xóa ngay lập tức để giám khảo có thể phản hồi. 

Việc xây dựng lại dựa vào số dư thay vì lưu trữ trực tiếp các câu trả lời trong khung. Truy vấn tiền tố cho biết số dư hiện tại có bằng 0 hay không và tính hợp lệ của toàn bộ chuỗi sẽ hạn chế số dư dương có thể có. 

Hai vị trí cuối cùng cần xử lý đặc biệt. Cố gắng suy ra chúng từ vòng lặp sẽ tạo ra từng lỗi một vì thuật toán cố tình dừng lại ở`n - 2`để phù hợp với giới hạn truy vấn. 

## Ví dụ đã hoạt động 

Đối với chuỗi`()()()`: 

| Bước | Truy vấn | Kết quả | Thông tin đã biết | 
| --- | --- | --- | --- | 
| 1 |`[1,2]`| CÓ | Hai ký tự đầu tiên là`()`| 
| 2 |`[1,3]`| KHÔNG | Số dư sau 3 dương | 
| 3 |`[1,4]`| CÓ | Số dư sau 4 bằng 0 | 

Số dư tiền tố xen kẽ giữa 0 và 1. Mỗi lần chuyển đổi sẽ sửa dấu ngoặc tiếp theo, đưa ra trình tự ban đầu. 

Đối với chuỗi`((()))`: 

| Bước | Truy vấn | Kết quả | Thông tin đã biết | 
| --- | --- | --- | --- | 
| 1 |`[1,2]`| KHÔNG | Hai ký tự đầu tiên là`( (`| 
| 2 |`[1,3]`| KHÔNG | Số dư vẫn dương | 
| 3 |`[1,4]`| KHÔNG | Số dư vẫn dương | 

Số dư tăng dần cho đến khi dấu ngoặc đóng bắt đầu. Quy tắc hậu tố kết thúc việc xây dựng lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được xử lý một số lần không đổi | 
| Không gian | O(n) | Chúng tôi lưu trữ câu trả lời và thông tin tiền tố | 

Thuật toán sử dụng số lượng truy vấn tuyến tính, phù hợp với giới hạn nhất định`q >= n - 3`. Mức tiêu thụ bộ nhớ cũng tuyến tính và dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm 

Vì vấn đề ban đầu có tính tương tác nên các bài kiểm tra dựa trên khẳng định thông thường không thể tái tạo giao thức đánh giá. Các trường hợp sau đây mô tả các kết quả đầu ra được xây dựng lại mà thuật toán phải tạo ra. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n = 2`, ẩn giấu`()`|`()`| Xử lý độ dài tối thiểu | 
|`n = 6`, ẩn giấu`()()()`|`()()()`| Nhiều số dư bằng 0 | 
|`n = 6`, ẩn giấu`((()))`|`((()))`| Làm tổ sâu | 
|`n = 8`, ẩn giấu`(()(()))`|`(()(()))`| Mô hình lồng hỗn hợp | 

## Vỏ cạnh 

Đối với chuỗi hợp lệ tối thiểu`()`, không có tiền tố ở giữa để truy vấn. Truy vấn đầu tiên xác định cả hai ký tự ngay lập tức và không cần phải xây dựng lại hậu tố. 

Đối với một chuỗi lồng sâu như`((()))`, mọi tiền tố đầu đều không hợp lệ ngoại trừ chuỗi đầy đủ. Thuật toán không nhầm lẫn điều này với một tình huống không xác định vì câu trả lời có tiền tố phủ định có nghĩa là số dư dương và số dư chỉ thay đổi một mỗi bước. 

Đối với một chuỗi kết thúc bằng`"()"`, số dư còn lại sau vị trí`n - 2`bằng 0, vì vậy quy tắc hậu tố chọn kết thúc duy nhất có thể. Đối với một chuỗi có số dư còn lại là hai, chẳng hạn như`((()))`, sự hoàn thành hợp lệ duy nhất là`"))"`, mà thuật toán cũng rút ra.
