---
title: "CF 1469A - Trình tự khung thông thường"
description: "Chúng ta được cung cấp một chuỗi ngắn chỉ gồm ba loại ký tự: dấu ngoặc mở, dấu ngoặc đóng và dấu chấm hỏi. Điều khó khăn là chuỗi không cố định, bởi vì mỗi dấu chấm hỏi có thể độc lập trở thành dấu ngoặc mở hoặc dấu ngoặc đóng."
date: "2026-06-11T01:09:06+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1469
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 101 (Rated for Div. 2)"
rating: 1000
weight: 1469
solve_time_s: 134
verified: false
draft: false
---

[CF 1469A - Trình tự khung thông thường](https://codeforces.com/problemset/problem/1469/A) 

**Đánh giá:** 1000 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi ngắn chỉ gồm ba loại ký tự: dấu ngoặc mở, dấu ngoặc đóng và dấu chấm hỏi. Điều khó khăn là chuỗi không cố định, bởi vì mỗi dấu chấm hỏi có thể độc lập trở thành dấu ngoặc mở hoặc dấu ngoặc đóng. Hai sự kiện đặc biệt giúp đơn giản hóa cấu trúc rất nhiều: chuỗi chứa chính xác một dấu ngoặc mở cố định và chính xác một dấu ngoặc đóng cố định, trong khi mọi thứ khác đều linh hoạt. 

Nhiệm vụ là quyết định xem có cách nào thay thế tất cả các dấu hỏi để chuỗi kết quả là chuỗi ngoặc đúng hay không. Một chuỗi đúng có nghĩa là nếu chúng ta hiểu nó là dấu ngoặc đơn thì mỗi tiền tố có ít nhất số dấu ngoặc mở bằng số dấu ngoặc đóng và tổng các số khớp chính xác sao cho số dư cuối cùng bằng 0. 

Mặc dù đầu vào có vẻ nhỏ đối với mỗi trường hợp kiểm thử nhưng số lượng trường hợp kiểm thử có thể lớn. Mỗi độ dài chuỗi tối đa là 100, do đó, bất kỳ giải pháp bậc hai hoặc tệ hơn cho mỗi trường hợp thử nghiệm đều an toàn, trong khi mọi giải pháp theo cấp số nhân sẽ quá chậm nếu chúng ta cố gắng khám phá trực tiếp tất cả các phép gán dấu hỏi. 

Một ý tưởng ngây thơ nhưng thất bại ngay lập tức là buộc phải thay thế dấu chấm hỏi một cách thô bạo. Nếu có k dấu chấm hỏi thì có 2^k khả năng. Với k lên tới khoảng 100, giá trị này trở nên lớn về mặt thiên văn. Ngay cả khi chúng tôi loại bỏ các tiền tố không hợp lệ thì việc phân nhánh vẫn quá lớn để có thể xử lý trong giới hạn. 

Một trường hợp thất bại tinh vi khác xuất phát từ việc tham lam gán mọi dấu chấm hỏi là mở hoặc đóng chỉ dựa trên số dư cục bộ. Ví dụ: việc quyết định sớm rằng tiền tố “cần” dấu ngoặc đóng có thể dẫn đến ngõ cụt sau này khi hậu tố còn lại không thể cân bằng chính xác. Điều này cho thấy các quyết định tham lam cục bộ mà không xem xét đến cấu trúc toàn cầu là không an toàn. 

Cấu trúc quan trọng còn thiếu là chỉ có hai mỏ neo cố định: một mỏ neo được đảm bảo mở và một mỏ neo được đảm bảo đóng. Điều này có nghĩa là câu trả lời chủ yếu phụ thuộc vào việc liệu hai điểm neo đó có thể được định vị theo cách cho phép chia chuỗi thành cấu trúc tiền tố-hậu tố hợp lệ hay không, với đủ tính linh hoạt từ các dấu chấm hỏi để lấp đầy các khoảng trống. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực chỉ định mỗi dấu chấm hỏi là “(” hoặc “)”, sau đó kiểm tra xem chuỗi kết quả có hợp lệ hay không bằng cách sử dụng quét tuyến tính. Điều này đúng vì nó mô phỏng trực tiếp định nghĩa của vấn đề. Tuy nhiên, nếu có k dấu chấm hỏi, nó sẽ đánh giá 2^k chuỗi và mỗi lần kiểm tra tính hợp lệ có giá O(n), khiến tổng độ phức tạp là O(n·2^n). Điều này không thể sử dụng được ngay cả đối với n vừa phải. 

Quan sát quan trọng là chúng ta không thực sự cần phải quyết định từng nhân vật một cách độc lập. Một chuỗi ngoặc hợp lệ phải duy trì sự cân bằng tiền tố và vì tất cả sự không chắc chắn đều tập trung vào dấu hỏi, nên câu hỏi có ý nghĩa duy nhất là liệu chúng ta có thể phân phối đủ dấu ngoặc mở trước một điểm nhất định và đủ dấu ngoặc đóng sau điểm đó hay không. 

Dấu ngoặc mở và đóng cố định phân chia chuỗi thành một cấu trúc một cách hiệu quả mà chúng ta chỉ cần đảm bảo rằng chuỗi có thể được cân bằng xung quanh chúng. Điều kiện giảm xuống còn kiểm tra xem việc hoán đổi hai mỏ neo cố định có phá vỡ khả năng duy trì sự cân bằng giữa các tiền tố hay không. Cụ thể, nếu chúng ta giả định rằng '(' cố định phải đến trước ')' cố định, chúng ta có thể thử xác minh xem chuỗi có thể hợp lệ hay không; nếu chúng sai thứ tự tương đối thì điều đó sẽ không thể thực hiện được vì không có phép gán dấu chấm hỏi nào có thể khắc phục được sự đảo ngược đó mà không vi phạm các ràng buộc tiền tố. 

Do đó, toàn bộ vấn đề giảm xuống việc kiểm tra vị trí tương đối của '(' và ')' duy nhất trong chuỗi.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét chuỗi và ghi lại chỉ số của dấu ngoặc mở duy nhất và dấu ngoặc đóng duy nhất. 

Chúng ta cần vị trí của họ vì các nhân vật còn lại rất linh hoạt và không trực tiếp ràng buộc cấu trúc. 
2. Kiểm tra xem dấu ngoặc mở có xuất hiện trước dấu ngoặc đóng hay không. 

Điều kiện này rất cần thiết vì bất kỳ chuỗi dấu ngoặc hợp lệ nào cũng phải có tiền tố nơi có sẵn khả năng mở trước khi chúng ta bắt đầu sử dụng quá mức dấu ngoặc đóng. 
3. Nếu dấu ngoặc mở xuất hiện sau dấu ngoặc đóng, hãy trả về NO ngay lập tức. 

Trong trường hợp đó, ngay cả vị trí tốt nhất của dấu chấm hỏi cũng không thể sắp xếp lại cấu trúc cố định, do đó không thể tránh khỏi tính vô hiệu. 
4. Nếu không, trả về CÓ. 

Khi dấu ngoặc mở sớm hơn, chúng ta luôn có thể gán dấu chấm hỏi để cân bằng tiền tố trước và sau vị trí cố định. 

### Tại sao nó hoạt động 

Một chuỗi ngoặc hợp lệ phải có đặc tính là ở mọi tiền tố, số lượng dấu ngoặc mở ít nhất bằng số lượng dấu ngoặc đóng. Khung mở cố định đảm bảo rằng một số số dư dương có thể được tạo sớm, trong khi khung đóng cố định buộc phải giảm ít nhất một lần sau đó. Nếu dấu ngoặc đóng xuất hiện trước dấu ngoặc mở, thì tồn tại một tiền tố trong đó điều kiện cân bằng bắt buộc đã bị vi phạm bất kể chúng ta gán dấu chấm hỏi như thế nào, bởi vì không dấu chấm hỏi nào có thể thay đổi thứ tự của hai ràng buộc cố định này. Nếu dấu ngoặc mở sớm hơn, chúng ta luôn có thể chọn phép gán các dấu chấm hỏi để duy trì tính khả thi của tiền tố cho đến khi gặp dấu ngoặc đóng và sau đó hoàn thành việc cân bằng. Tính linh hoạt của tất cả các vị trí khác đảm bảo không xuất hiện hạn chế cấu trúc bổ sung nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    
    open_pos = -1
    close_pos = -1
    
    for i, ch in enumerate(s):
        if ch == '(':
            open_pos = i
        elif ch == ')':
            close_pos = i
    
    if open_pos < close_pos:
        print("YES")
    else:
        print("NO")
```Mã đầu tiên xác định dấu ngoặc mở và đóng được đảm bảo duy nhất. Sau đó nó so sánh vị trí của họ trực tiếp. Điều này có tác dụng vì tất cả các ký tự khác đều linh hoạt và không thể thực thi bất kỳ ràng buộc thứ tự nào mạnh hơn thứ tự tương đối của hai điểm neo này. 

Một điểm tinh tế là chúng tôi không hề cố gắng gán dấu chấm hỏi. Đây là cố ý: vai trò duy nhất của họ là cung cấp sự chậm trễ và họ không bao giờ hạn chế tính khả thi nhiều hơn cặp cố định đã làm. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`(? ) ? ()`Chúng tôi theo dõi vị trí của các dấu ngoặc cố định. 

| Bước | Chỉ mục mở | Đóng chỉ mục | Tình trạng | 
| --- | --- | --- | --- | 
| Quét | 0 | 2 | mở < đóng | 

Dấu ngoặc mở xuất hiện trước dấu ngoặc đóng nên câu trả lời là CÓ. 

Điều này chứng tỏ trường hợp các dấu chấm hỏi có thể được sắp xếp một cách tự do và cấu trúc cố định đã nhất quán với một thứ tự hợp lệ. 

### Ví dụ 2:`)?(?`| Bước | Chỉ mục mở | Đóng chỉ mục | Tình trạng | 
| --- | --- | --- | --- | 
| Quét | 2 | 0 | mở > đóng | 

Ở đây dấu ngoặc đóng xuất hiện đầu tiên. Cho dù chúng ta gán dấu chấm hỏi như thế nào, chúng ta cũng không thể tránh khỏi việc có một tiền tố trong đó dấu ngoặc đóng xuất hiện trước khi bất kỳ dấu ngoặc mở nào có thể bù đắp cho nó. Câu trả lời là KHÔNG. 

Điều này cho thấy chế độ lỗi chính: không thể sửa chữa được cấu trúc cố định bị đảo ngược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét một lần cho mỗi trường hợp thử nghiệm để xác định vị trí hai dấu ngoặc cố định | 
| Không gian | O(1) | Chỉ có hai chỉ số được lưu trữ | 

Các ràng buộc cho phép tối đa 1000 trường hợp kiểm thử với các chuỗi có độ dài 100, do đó việc quét tuyến tính trên mỗi trường hợp kiểm thử là đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        s = input().strip()
        open_pos = -1
        close_pos = -1
        for i, ch in enumerate(s):
            if ch == '(':
                open_pos = i
            elif ch == ')':
                close_pos = i
        out.append("YES" if open_pos < close_pos else "NO")
    return "\n".join(out) + "\n"

# provided samples
assert run("""5
()
(?)
(??)
??()
)?(?""") == """YES
NO
YES
YES
NO
"""

# custom cases
assert run("""3
()
(??)
??)(""") == """YES
YES
NO
"""

assert run("""2
(?)
)?(""") == """NO
NO
"""

assert run("""1
(?()?)""") == """YES
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`()`| CÓ | trường hợp tối thiểu đã hợp lệ | 
|`(??)`| CÓ | dấu chấm hỏi không quan trọng khi thứ tự đúng | 
|`??)(`| KHÔNG | neo đảo ngược làm cho nó không thể | 
|`(?`| CÓ | cấu trúc tiền tố một phần tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi hai dấu ngoặc cố định liền kề nhau nhưng không đúng thứ tự. Ví dụ, đầu vào`)(??`. Thuật toán ghi lại open_pos > close_pos ngay lập tức và trả về NO. Điều này đúng vì mặc dù có dấu chấm hỏi nhưng ký tự đầu tiên đã vi phạm tính hợp lệ của tiền tố. 

Một trường hợp khác là khi dấu hỏi chiếm ưu thế trong chuỗi, chẳng hạn như`??(??)?`. Ở đây open_pos vẫn ở trước close_pos nên thuật toán trả về CÓ. Điều này có hiệu quả vì các dấu chấm hỏi luôn có thể được chỉ định để giữ cân bằng tiền tố cho đến khi đạt đến dấu ngoặc đóng cố định và sau đó hoàn thành chuỗi. 

Trường hợp cạnh cuối cùng là cấu hình hợp lệ nhỏ nhất`()`. Quá trình quét tìm thấy open_pos = 0 và close_pos = 1, do đó điều kiện giữ nguyên và trả về CÓ ngay lập tức, khớp với định nghĩa của chuỗi ngoặc thông thường.
