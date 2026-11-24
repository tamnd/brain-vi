---
title: "CF 70B - Nhắn tin văn bản"
description: "Chúng tôi được cung cấp độ dài tối đa của một tin nhắn SMS và một văn bản hoàn chỉnh bao gồm các câu được phân tách bằng dấu cách. Một câu luôn kết thúc bằng một trong ., ?, hoặc !. Từ chỉ chứa các chữ cái."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "expression-parsing", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 70
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 64"
rating: 1600
weight: 70
solve_time_s: 99
verified: true
draft: false
---

[CF 70B - Nhắn tin văn bản](https://codeforces.com/problemset/problem/70/B) 

**Đánh giá:** 1600 
**Tags:** phân tích biểu thức, tham lam, chuỗi 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp độ dài tối đa của một tin nhắn SMS và một văn bản hoàn chỉnh bao gồm các câu được phân tách bằng dấu cách. Một câu luôn kết thúc bằng một trong`.`,`?`, hoặc`!`. Từ chỉ chứa các chữ cái. 

Nhiệm vụ là chia văn bản thành số lượng tin nhắn tối thiểu có thể sao cho không có câu nào bị ngắt giữa các tin nhắn. Một tin nhắn có thể chứa một số câu liên tiếp nếu chúng phù hợp với giới hạn độ dài. Khi hai câu liên tiếp được đặt vào các tin nhắn khác nhau, khoảng cách sẽ biến mất vì Fangy đơn giản là không gõ nó. 

Chi tiết về không gian này là toàn bộ thủ thuật của vấn đề. Bên trong một tin nhắn, các khoảng trống ban đầu phải được giữ nguyên. Giữa hai thông điệp khác nhau có thể bỏ qua khoảng cách. 

Độ dài văn bản tối đa là`10^4`, đủ nhỏ để xử lý tuyến tính hoặc bậc hai. Vì giới hạn tin nhắn`n`nhiều nhất là`255`, việc kiểm tra độ dài câu là chuyện nhỏ. Giải pháp khối sẽ có rủi ro vì việc xây dựng lại chuỗi con nhiều lần hoặc tính toán lại độ dài có thể đẩy số lượng thao tác lên hàng chục hoặc hàng trăm triệu. Một giải pháp lập trình tham lam hoặc năng động rõ ràng sẽ phù hợp hơn. 

Trường hợp đặc biệt quan trọng đầu tiên là khi một câu đã dài hơn giới hạn tin nhắn. 

đầu vào:```
5
Hello!
```Câu`"Hello!"`có chiều dài`6`, do đó không tồn tại sự phân chia hợp lệ. 

Đầu ra đúng:```
Impossible
```Việc triển khai bất cẩn có thể chỉ kiểm tra độ dài tin nhắn tổng hợp và quên xác thực từng câu. 

Một trường hợp tinh tế khác là khoảng cách biến mất giữa các tin nhắn. 

đầu vào:```
7
Hi. Bye.
```Toàn bộ văn bản có độ dài`8`, nhưng nó vẫn có thể được gửi bằng hai tin nhắn:`"Hi."`Và`"Bye."`Đầu ra đúng:```
2
```Việc triển khai luôn đếm khoảng trắng giữa các câu sẽ nghĩ sai rằng điều này là không thể. 

Một tình huống khó khăn nữa xuất hiện khi nhiều câu ngắn gộp lại vượt quá giới hạn đúng một khoảng cách. 

đầu vào:```
10
Hi. Hello.
```

`"Hi. Hello."`có chiều dài`10`, vì vậy nó phù hợp với một tin nhắn. 

Nếu giới hạn đã`9`, chúng ta sẽ cần hai tin nhắn vì khoảng trống bên trong phải được giữ nguyên khi cả hai câu ở cùng nhau. 

Đầu ra đúng cho`n = 9`:```
2
```Sự khác biệt giữa không gian bên trong và không gian ranh giới bị loại bỏ là nơi xảy ra nhiều lỗi riêng lẻ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đầu tiên chia văn bản thành các câu. Sau đó thử mọi cách nhóm các câu liên tiếp có thể thành tin nhắn và tính số lượng tin nhắn tối thiểu bằng lập trình động. 

Giả sử có`k`câu. Một DP ngây thơ có thể định nghĩa`dp[i]`là số lượng tin nhắn tối thiểu cần thiết cho lần đầu tiên`i`câu. Đối với mọi`i`, chúng tôi thử mọi vị trí trước đó`j`và kiểm tra xem câu`j...i-1`phù hợp với một tin nhắn. 

Phần tốn kém là tính toán nhiều lần độ dài của một nhóm. Nếu chúng tôi xây dựng lại các chuỗi con hoặc kể lại các khoảng trắng mỗi lần, mỗi lần chuyển đổi có thể tốn kém`O(k)`, sản xuất một`O(k^3)`giải pháp. Với khoảng`10^4`ký tự, số lượng câu cũng có thể đủ lớn khiến việc này trở nên quá chậm. 

Quan sát quan trọng là độ dài câu là tất cả những gì chúng ta thực sự cần. Khi văn bản được phân tích thành các độ dài câu, việc kiểm tra xem một số câu liên tiếp có khớp với nhau hay không sẽ trở thành một biểu thức số học đơn giản. 

Giả sử độ dài câu là:```
len[0], len[1], ..., len[k-1]
```Nếu chúng ta đặt câu`l...r`vào một tin nhắn, tổng chiều dài sẽ trở thành:```
sum(len[l...r]) + (r - l)
```phần bổ sung`(r - l)`xuất phát từ khoảng trắng giữa các câu liên tiếp trong cùng một thông điệp. Khoảng cách giữa các tin nhắn khác nhau sẽ biến mất và không được tính. 

Bây giờ vấn đề trở nên đơn giản hơn nhiều. Chúng ta chỉ cần phân chia dãy thành ít nhóm nhất sao cho tổng độ dài của mỗi nhóm không vượt quá`n`. 

Lúc này, tính tham lam trở nên tự nhiên. Bắt đầu từ câu hiện tại, chúng ta nên gói càng nhiều câu liên tiếp càng tốt vào tin nhắn hiện tại. Để lại khoảng trống không sử dụng không bao giờ có thể giúp ích được gì sau này vì tất cả các câu trong tương lai vẫn phải được đặt ở đâu đó. Lấy tiền tố hợp lệ tối đa sẽ giảm thiểu số lượng nhóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k³) | O(k) | Quá chậm | 
| Tối ưu | O( | văn bản | ) | 

## Hướng dẫn thuật toán 

1. Phân tích văn bản và trích xuất từng độ dài câu. 

Chúng tôi quét từng ký tự chuỗi. Mỗi lần chúng ta gặp phải`.`,`?`, hoặc`!`, chúng ta kết thúc một câu. Độ dài câu bao gồm mọi ký tự từ đầu cho đến dấu chấm câu. 
2. Kiểm tra xem có câu nào có độ dài vượt quá`n`. 

Nếu thậm chí một câu quá lớn thì sẽ không có sự sắp xếp hợp lệ nào vì các câu không thể được chia thành các tin nhắn. 
3. Bắt đầu xây dựng thông điệp một cách tham lam. 

Duy trì độ dài tin nhắn hiện tại và số lượng tin nhắn được sử dụng cho đến nay. 
4. Đặt câu đầu tiên vào tin nhắn hiện tại. 

Vì chúng tôi đã xác minh rằng mỗi câu đều phù hợp nên điều này luôn có thể thực hiện được. 
5. Đối với mỗi câu tiếp theo, hãy xác định xem nó có thể nối với tin nhắn hiện tại hay không. 

Nếu tin nhắn hiện tại đã chứa nội dung nào đó thì việc thêm một câu khác sẽ cần thêm một khoảng trắng. Vậy chi phí tăng thêm sẽ là:```
1 + next_sentence_length
```6. Nếu tổng số mới không vượt quá`n`, nối câu vào tin nhắn hiện tại. 

Điều này giữ cho số lượng tin nhắn không thay đổi trong khi tối đa hóa việc sử dụng tin nhắn hiện tại. 
7. Nếu không, hãy bắt đầu tin nhắn mới bằng câu này. 

Khoảng cách ngăn cách biến mất vì các câu giờ đây ở những thông điệp khác nhau. 
8. Sau khi xử lý tất cả các câu, xuất ra số lượng thông báo đã sử dụng. 

### Tại sao nó hoạt động 

Lựa chọn tham lam là tối ưu vì tin nhắn chỉ chứa các câu liên tiếp. Giả sử tin nhắn hiện tại vẫn có thể phù hợp với một câu khác. Chia ra sớm hơn thay vì thêm câu đó chỉ lãng phí không gian sẵn có và không thể tạo thêm cơ hội trong tương lai. Mỗi câu sau vẫn phải xuất hiện sau câu hiện tại, vì vậy việc trì hoãn phân tách không bao giờ làm tăng số lượng tin nhắn cần thiết. 

Thuật toán duy trì sự bất biến rằng tin nhắn hiện tại chứa số lượng câu liên tiếp tối đa có thể nằm trong giới hạn. Bởi vì mỗi tin nhắn được đóng gói tối đa nên tổng số tin nhắn là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    text = input().rstrip()

    sentences = []

    start = 0

    for i, ch in enumerate(text):
        if ch in ".?!":
            sentences.append(i - start + 1)

            if i + 1 < len(text):
                start = i + 2

    for length in sentences:
        if length > n:
            print("Impossible")
            return

    messages = 1
    current = sentences[0]

    for length in sentences[1:]:
        if current + 1 + length <= n:
            current += 1 + length
        else:
            messages += 1
            current = length

    print(messages)

solve()
```Phần đầu tiên quét văn bản và trích xuất độ dài câu. Biến`start`lưu trữ chỉ mục nơi câu hiện tại bắt đầu. Bất cứ khi nào tìm thấy dấu câu, độ dài câu chỉ đơn giản là:```
i - start + 1
```Sau khi kết thúc một câu, câu tiếp theo sẽ bắt đầu muộn hơn hai vị trí vì ngữ pháp đảm bảo chính xác một khoảng cách giữa các câu liên tiếp. 

Vòng lặp thứ hai kiểm tra tính khả thi. Điều này phải xảy ra trước giai đoạn đóng gói tham lam vì một câu nói quá khổ ngay lập tức khiến nhiệm vụ trở nên bất khả thi. 

Cấu trúc tham lam chỉ giữ lại độ dài tin nhắn hiện tại. Khi nối thêm một câu khác, chúng ta thêm một ký tự phụ cho khoảng trắng bên trong:```
current + 1 + length
```Khoảng trống này chỉ được tính khi có hai câu nằm trong cùng một thông điệp. Bắt đầu một tin nhắn mới chỉ đặt lại độ dài cho câu mới. 

Một điều kiện biên tinh tế là khởi tạo:```
messages = 1
current = sentences[0]
```Văn bản được đảm bảo chứa ít nhất một câu, vì vậy điều này luôn an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
25
Hello. I am a little walrus.
```Độ dài câu được trích xuất:```
[6, 21]
```| Bước | Độ dài câu hiện tại | Độ dài tin nhắn hiện tại | Hành động | Tin nhắn | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 6 | 6 | Bắt đầu tin nhắn đầu tiên | 1 | 
| Tiếp theo | 21 | 6 |`6 + 1 + 21 = 28 > 25`, bắt đầu tin nhắn mới | 2 | 

Câu trả lời cuối cùng:```
2
```Dấu vết này cho thấy tại sao không gian ngăn cách lại quan trọng. Chiều dài kết hợp trở thành`28`bởi vì cả hai câu sẽ yêu cầu một khoảng trống bên trong nếu được giữ cùng nhau. 

### Ví dụ 2 

đầu vào:```
7
Hi. Bye.
```Độ dài câu được trích xuất:```
[3, 4]
```| Bước | Độ dài câu hiện tại | Độ dài tin nhắn hiện tại | Hành động | Tin nhắn | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 3 | 3 | Bắt đầu tin nhắn đầu tiên | 1 | 
| Tiếp theo | 4 | 3 |`3 + 1 + 4 = 8 > 7`, bắt đầu tin nhắn mới | 2 | 

Câu trả lời cuối cùng:```
2
```Ví dụ này thể hiện quy tắc khoảng trắng biến mất giữa các tin nhắn. Mặc dù độ dài văn bản gốc là`8`, hai thông báo riêng biệt là hợp lệ vì khoảng cách phân tách bị bỏ qua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | văn bản | 
| Không gian | O(k) | Lưu trữ độ dài câu cho`k`câu | 

Độ dài đầu vào tối đa là`10^4`, do đó quá trình quét tuyến tính diễn ra cực kỳ nhanh chóng. Việc sử dụng bộ nhớ cũng rất nhỏ vì chúng ta chỉ lưu trữ một số nguyên cho mỗi câu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    text = input().rstrip()

    sentences = []

    start = 0

    for i, ch in enumerate(text):
        if ch in ".?!":
            sentences.append(i - start + 1)

            if i + 1 < len(text):
                start = i + 2

    for length in sentences:
        if length > n:
            return "Impossible"

    messages = 1
    current = sentences[0]

    for length in sentences[1:]:
        if current + 1 + length <= n:
            current += 1 + length
        else:
            messages += 1
            current = length

    return str(messages)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run("25\nHello. I am a little walrus.\n") == "2", "sample 1"

# impossible because one sentence is too long
assert run("5\nHello!\n") == "Impossible", "single oversized sentence"

# exactly fits into one message
assert run("10\nHi. Hello.\n") == "1", "boundary fit"

# requires split because of one extra space
assert run("9\nHi. Hello.\n") == "2", "space handling"

# minimum size style case
assert run("2\nA.\n") == "1", "smallest valid sentence"

# many short sentences
assert run("3\nA. B. C.\n") == "3", "every sentence isolated"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 / Hello!`|`Impossible`| Phát hiện câu quá khổ | 
|`10 / Hi. Hello.`|`1`| Phù hợp với ranh giới chính xác | 
|`9 / Hi. Hello.`|`2`| Không gian bên trong được tính chính xác | 
|`2 / A.`|`1`| Đầu vào hợp lệ nhỏ nhất | 
|`3 / A. B. C.`|`3`| Chia tin nhắn thường xuyên | 

## Vỏ cạnh 

Xét trường hợp không thể xảy ra:```
5
Hello!
```Trình phân tích cú pháp trích xuất một câu có độ dài`6`. Từ`6 > 5`, thuật toán in ngay:```
Impossible
```Không có việc đóng gói tham lam nào xảy ra vì đầu vào đã không hợp lệ. 

Bây giờ hãy xem xét kịch bản không gian biến mất:```
7
Hi. Bye.
```Độ dài câu được trích xuất là`[3, 4]`. 

Thuật toán đứng đầu`"Hi."`vào tin nhắn hiện tại. Khi cố gắng nối thêm`"Bye."`, độ dài cần thiết sẽ trở thành:```
3 + 1 + 4 = 8
```Điều này vượt quá giới hạn nên một tin nhắn mới sẽ được bắt đầu. Khoảng cách giữa các tin nhắn không được tính nữa, tạo ra chính xác hai tin nhắn hợp lệ. 

Cuối cùng, kiểm tra điều kiện biên:```
10
Hi. Hello.
```Độ dài là`[3, 6]`. 

Nối thêm câu thứ hai sẽ cho:```
3 + 1 + 6 = 10
```Vì sự bình đẳng được cho phép nên cả hai câu đều ở cùng nhau trong một tin nhắn. Điều này xác nhận rằng thuật toán xử lý các khớp chính xác một cách chính xác mà không có sai sót nào.
