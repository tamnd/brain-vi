---
title: "CF 102899J - KK\u4e0e\u82f1\u8bed"
description: "Chúng ta được cung cấp một dòng văn bản đại diện cho một câu có thể chứa từ “is” trong các ngữ cảnh khác nhau. Nhiệm vụ là quét câu này và thay thế mọi lần xuất hiện của từ viết thường “is” khi nó xuất hiện dưới dạng một mã thông báo độc lập được bao quanh bởi khoảng trắng trên cả hai…"
date: "2026-07-04T08:22:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102899
codeforces_index: "J"
codeforces_contest_name: "The 2nd Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 102899
solve_time_s: 38
verified: true
draft: false
---

[CF 102899J - KK\u4e0e\u82f1\u8bed](https://codeforces.com/problemset/problem/102899/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng văn bản đại diện cho một câu có thể chứa từ “is” trong các ngữ cảnh khác nhau. Nhiệm vụ là quét câu này và thay thế mọi lần xuất hiện của từ viết thường “is” khi nó xuất hiện dưới dạng một mã thông báo độc lập được bao quanh bởi khoảng trắng ở cả hai bên. Các hình thức khác không được thay đổi, bao gồm “Is” viết hoa, “is” ở đầu hoặc cuối dòng hoặc “is” gắn với dấu câu hoặc các ký tự khác. 

Kích thước đầu vào có thể lên tới 100.000 ký tự, điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào liên quan đến nối chuỗi lặp lại hoặc thay thế toàn cục lặp lại trên chuỗi con sẽ gặp rủi ro nếu được triển khai một cách ngây thơ theo cách bậc hai. Cần phải quét tuyến tính vì bất kỳ phương pháp nào liên tục tìm kiếm hoặc xây dựng lại chuỗi không hiệu quả đều có thể giảm xuống O(n²) trong trường hợp xấu nhất khi xảy ra nhiều thay thế. 

Một số trường hợp cạnh xuất hiện một cách tự nhiên từ định nghĩa “được bao quanh bởi không gian”. Nếu chuỗi bắt đầu bằng “is” thì không nên thay thế vì không có khoảng trắng ở đầu. Ví dụ: đầu vào “là một cuốn sách” phải không thay đổi. Tương tự, nếu “is” xuất hiện ở cuối giống như “this is”, nó chỉ được thay thế nếu có dấu cách ở bên phải theo sau, nghĩa là các vị trí ở cuối cũng rất nhạy cảm. Một trường hợp tinh tế khác là sự liền kề của dấu câu: không được thay thế “is” hoặc “(is)” vì từ này không bị giới hạn chặt chẽ bởi dấu cách. 

Một cách tiếp cận đơn giản là phân chia theo dấu cách và xây dựng lại câu cũng không thành công nếu có nhiều dấu cách xuất hiện giữa các từ hoặc nếu dấu câu được gắn vào mã thông báo, vì nó có nguy cơ làm mất định dạng ban đầu. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là liên tục tìm kiếm chuỗi con cho chuỗi con`" is "`và thay thế nó bằng`" was "`. Điều này đúng về mặt logic vì nó mã hóa trực tiếp tình trạng có khoảng trắng ở cả hai bên. Tuy nhiên, mỗi lần thay thế đều có khả năng dịch chuyển chuỗi và yêu cầu xây dựng lại chuỗi đó, và nếu được thực hiện bằng cách sử dụng lặp lại`find`và cắt, mỗi thao tác có thể mất O(n). Trong trường hợp xấu nhất khi việc thay thế diễn ra thường xuyên, điều này dẫn đến hành vi O(n²). 

Một quan sát đáng tin cậy hơn là chúng ta không bao giờ cần sửa đổi chuỗi tại chỗ nhiều lần. Mỗi ký tự có thể được xử lý một lần từ trái sang phải trong khi kiểm tra ngữ cảnh cục bộ. Tại bất kỳ vị trí nào, chúng ta chỉ cần quyết định xem cửa sổ ba ký tự hiện tại có hình thành hay không`" is "`với ranh giới thích hợp. Nếu có, chúng tôi phát ra`" was "`thay vì sao chép chuỗi con gốc. Nếu không, chúng tôi sao chép nguyên ký tự hiện tại và tiến về phía trước. Điều này làm giảm vấn đề xuống còn một lần với việc kiểm tra liên tục theo thời gian cho mỗi vị trí. 

Đặc tính cấu trúc quan trọng là quyết định thay thế “is” chỉ phụ thuộc vào các hàng xóm trực tiếp của nó. Không cần thông tin toàn cầu nên việc xây dựng luồng là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Thay thế chuỗi Brute Force | O(n²) | O(n) | Quá chậm | 
| Quét một lần với Kiểm tra cửa sổ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào thành một biến`s`. Chúng ta sẽ xây dựng câu trả lời tăng dần thay vì sửa đổi`s`tại chỗ, vì các chuỗi là bất biến trong Python. 
2. Khởi tạo danh sách trống`out`để lưu trữ các ký tự đầu ra một cách hiệu quả. Việc sử dụng danh sách sẽ tránh được chi phí nối chuỗi lặp lại. 
3. Lặp lại chuỗi bằng chỉ mục`i`từ`0`ĐẾN`len(s) - 1`. 
4. Tại mỗi vị trí`i`, kiểm tra xem có thể thay thế được không. Điều kiện yêu cầu rằng: 

chuỗi con bắt đầu từ`i`chính xác là`"is"`, và có một khoảng trắng ngay trước và sau nó, nghĩa là`s[i-1] == ' '`Và`s[i+2] == ' '`. 

Việc kiểm tra này đảm bảo chúng tôi chỉ thay thế các từ riêng biệt chứ không thay thế các phần của từ khác hoặc mã thông báo đính kèm dấu câu. 
5. Nếu điều kiện đúng, hãy nối thêm chuỗi`"was"`vào danh sách đầu ra và tiến lên`i`bằng 2, vì chúng ta đã sử dụng hai ký tự. 
6. Nếu điều kiện không đúng, hãy nối thêm`s[i]`vào danh sách đầu ra và tiến lên`i`bằng 1. 
7. Sau khi kết thúc vòng lặp, nối danh sách thành chuỗi cuối cùng và in nó. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ thực tế là mọi quyết định đều mang tính cục bộ và không thể đảo ngược. Bất cứ khi nào chúng tôi thay thế`" is "`với`" was "`, chúng tôi đảm bảo rằng sau này không có sự trùng lặp hoặc khớp một phần nào có thể bị ảnh hưởng vì việc thay thế chỉ xảy ra khi cả hai ký tự xung quanh đều là khoảng trắng. Điều này ngăn ngừa xung đột giữa các trận đấu liền kề. Mỗi ký tự được kiểm tra chính xác một lần như một phần của trường hợp trùng khớp hoặc không khớp, đảm bảo bao phủ đầy đủ mà không bị trùng lặp hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().rstrip('\n')
    n = len(s)
    out = []
    i = 0

    while i < n:
        if i > 0 and i + 2 < n:
            if s[i:i+2] == "is" and s[i-1] == ' ' and s[i+2] == ' ':
                out.append("was")
                i += 2
                continue

        out.append(s[i])
        i += 1

    print("".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên việc kiểm tra cửa sổ trượt xung quanh từng vị trí ứng viên. Sự tinh tế quan trọng là đảm bảo an toàn ranh giới: điều kiện`i > 0 and i + 2 < n`ngăn chặn truy cập ngoài phạm vi khi kiểm tra hàng xóm. Tiến trình của vòng lặp khác nhau tùy thuộc vào việc thay thế có xảy ra hay không, điều này tránh được việc xử lý kép các ký tự bên trong một phân đoạn phù hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
It is my boy friend.
```| tôi | s[i:i+2] | char trái | đúng char | hành động | đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 0 | Nó | - | - | sao chép | Tôi | 
| 1 | t | - | - | sao chép | Nó | 
| 2 | " tôi" | t | s | sao chép | Nó | 
| 3 | là | không gian | không gian | thay thế | Đó là | 
| 5 | của tôi... | - | - | sao chép phần còn lại | Đó là bạn trai của tôi. | 

Dấu vết này cho thấy rằng chỉ có chữ “is” được giới hạn chính xác mới được thay thế, trong khi dấu câu và các từ khác vẫn không bị ảnh hưởng. 

### Ví dụ 2 

đầu vào:```
Is it your pencil?
```| tôi | chuỗi con | tình trạng | hành động | đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | Là | vốn tôi | không thay thế | Tôi | 
| 1 | s | - | sao chép | Là | 
| 2 | không gian+i | - | sao chép | Là | 
| ... | ... | ... | ... | cuối cùng không thay đổi | 

Chữ “Is” viết hoa không được sửa đổi vì việc so khớp có phân biệt chữ hoa chữ thường và yêu cầu chữ “is” viết thường chính xác. 

Điều này xác nhận rằng các quy tắc ranh giới và phân biệt chữ hoa chữ thường đều được thực thi một cách tự nhiên bằng logic quét. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được truy cập một lần và mỗi lần kiểm tra là thời gian không đổi | 
| Không gian | O(n) | Danh sách đầu ra lưu trữ chuỗi chuyển đổi cuối cùng | 

Quá trình quét tuyến tính vừa vặn thoải mái trong giới hạn 1 giây cho n lên tới 100.000 và mức sử dụng bộ nhớ vẫn tỷ lệ thuận với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    buf = sio.StringIO()
    with redirect_stdout(buf):
        solve()
    return buf.getvalue().strip()

def solve():
    s = sys.stdin.readline().rstrip('\n')
    n = len(s)
    out = []
    i = 0
    while i < n:
        if i > 0 and i + 2 < n and s[i:i+2] == "is" and s[i-1] == ' ' and s[i+2] == ' ':
            out.append("was")
            i += 2
        else:
            out.append(s[i])
            i += 1
    print("".join(out))

assert run("It is my boy friend.") == "It was my boy friend."
assert run("Is it your pencil?") == "Is it your pencil?"
assert run("This is an island.") == "This was an island."
assert run("is") == "is"
assert run("A is B is C") == "A was B was C"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nó là bạn trai của tôi. | Đó là bạn trai của tôi. | Thay thế cơ bản | 
| Có phải bút chì của bạn không? | Có phải bút chì của bạn không? | Phân biệt chữ hoa chữ thường | 
| là | là | Không có không gian xung quanh | 
| A là B là C | A là B là C | Nhiều lần thay thế | 

## Vỏ cạnh 

Trường hợp quan trọng nhất là khi “is” xuất hiện ngay từ đầu. Ví dụ: đầu vào “ở đây” sẽ không thay đổi vì không có khoảng trắng trước đó. Trong quá trình lặp ở chỉ số 0, việc kiểm tra ranh giới`i > 0`không thành công ngay lập tức, do đó thuật toán sao chép ký tự một cách an toàn mà không cần cố gắng so khớp. 

Một trường hợp khác là các lần xuất hiện kéo theo như “đây là”. Khi quá trình quét đạt đến vị trí hợp lệ cuối cùng, điều kiện`i + 2 < n`ngăn chặn việc đọc ngoài chuỗi, do đó không có quyền truy cập không hợp lệ nào xảy ra và chữ “is” cuối cùng chỉ được thay thế nếu theo sau là một khoảng trắng, nhưng không phải vậy. Đầu ra vẫn đúng. 

Trường hợp thứ ba liên quan đến dấu chấm câu liền kề, chẳng hạn như “là”. Mặc dù chuỗi con bắt đầu bằng “is”, ký tự theo sau nó là dấu phẩy chứ không phải dấu cách, do đó điều kiện thay thế không thành công. Thuật toán bảo tồn chính xác văn bản gốc, chứng minh rằng việc kiểm tra ranh giới nghiêm ngặt thực thi chính xác định nghĩa của một từ độc lập.
