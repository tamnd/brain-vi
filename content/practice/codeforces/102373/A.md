---
title: "CF 102373A - \u041e\u043d\u043e"
description: "Chúng ta có hai chuỗi chữ thường, s và t. Chúng ta cần đếm các chuỗi con khác rỗng của s mà các chữ cái của chúng có thể được lấy từ t. Thứ tự của các chữ cái không quan trọng, vì chúng ta chỉ quan tâm liệu t có chứa đủ bản sao của mọi ký tự xuất hiện trong chuỗi con đã chọn hay không."
date: "2026-08-14T12:29:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102373
codeforces_index: "A"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434 \u0434\u043b\u044f \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102373
solve_time_s: 57
verified: true
draft: false
---

[CF 102373A - \u041e\u043d\u043e](https://codeforces.com/problemset/problem/102373/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi chữ thường,`s`Và`t`. Chúng ta cần đếm các chuỗi con khác rỗng của`s`những lá thư của ai có thể được lấy từ`t`. Thứ tự của các chữ cái không quan trọng, bởi vì chúng ta chỉ quan tâm liệu`t`chứa đủ bản sao của mọi ký tự xuất hiện trong chuỗi con đã chọn. Các vị trí khác nhau trong`s`xác định các chuỗi con khác nhau, ngay cả khi nội dung của chúng bằng nhau. Tuyên bố chính thức đưa ra`|s|, |t| <= 10^6`, với giới hạn thời gian 2 giây và bộ nhớ 512 MB. 

Ví dụ, nếu`s = "aaa"`Và`t = "aa"`, ba chuỗi con một ký tự là hợp lệ và hai chuỗi con dài hai ký tự cũng hợp lệ, cho`5`. Toàn bộ chuỗi không hợp lệ vì nó cần ba bản sao của`a`trong khi`t`chỉ có hai. 

Kích thước giới hạn của một triệu loại trừ mọi thứ bậc hai. Đã có khoảng`n(n+1)/2`chuỗi con, đại khái là`5 * 10^11`khi`n = 10^6`. Chúng ta cần xử lý các chuỗi với số lượng công việc không đổi cho mỗi ký tự, đưa ra một`O(|s| + |t|)`giải pháp. Vì bảng chữ cái chỉ chứa 26 chữ cái viết thường nên việc duy trì tần số ký tự đòi hỏi phải có thêm dung lượng lưu trữ có kích thước không đổi. 

Trường hợp cạnh đầu tiên là khi không có chuỗi con nào trống là hợp lệ. Ví dụ,```
a
b
```có đầu ra```
0
```bởi vì chuỗi con duy nhất,`"a"`, yêu cầu một chữ cái không xuất hiện trong`t`. Việc triển khai bất cẩn khởi tạo câu trả lời cho một hoặc đếm mọi vị trí bắt đầu có thể sẽ trả về không chính xác`1`. 

Trường hợp cạnh thứ hai là các ký tự lặp lại. Ví dụ,```
aaa
aa
```có đầu ra```
5
```bởi vì cả ba chuỗi con có độ dài một và cả hai chuỗi con có độ dài hai đều hợp lệ. Việc kiểm tra chỉ dựa trên việc mỗi ký tự riêng biệt có xuất hiện trong`t`sẽ chấp nhận sai`"aaa"`cũng vậy. 

Trường hợp thứ ba là các lần xuất hiện khác nhau của cùng một văn bản vẫn biểu thị các chuỗi con khác nhau. TRONG```
aaa
aa
```chuỗi con tại các vị trí`1..2`và chuỗi con tại các vị trí`2..3`cả hai đều được tính. Chúng tôi đếm các khoảng, không phải các chuỗi riêng biệt. 

Trường hợp cạnh thứ tư là một chuỗi con có giá trị độc lập với mọi chuỗi con khác. Bản sao của các chữ cái trong`t`không được tiêu thụ trên toàn cầu. Ví dụ,```
aaaa
a
```có đầu ra```
4
```bởi vì mỗi chuỗi con trong số bốn chuỗi con một ký tự có thể được tập hợp độc lập từ một chuỗi con duy nhất`a`TRONG`t`. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp có thể liệt kê mọi chuỗi con của`s`. Đối với điểm cuối bên trái cố định, hãy mở rộng điểm cuối bên phải từng ký tự một, duy trì tần số của mỗi ký tự trong chuỗi con hiện tại và kiểm tra xem các tần số đó có khớp với tần số của`t`. Nếu việc kiểm tra được duy trì trong thời gian không đổi, sẽ có`n(n+1)/2`tiện ích mở rộng, vì vậy thời gian chạy trong trường hợp xấu nhất là`Theta(n^2)`. Vì`n = 10^6`, đó là về`5 * 10^11`phần mở rộng chuỗi con, vượt xa giới hạn 2 giây cho phép. Việc kiểm tra lại tất cả 26 ký tự ở mỗi tiện ích mở rộng vẫn được thực hiện`Theta(26n^2)`, đó là sự thất bại tiệm cận tương tự. 

Quan sát hữu ích là các chuỗi con hợp lệ được đóng lại khi lấy các khoảng con. Nếu một chuỗi con có thể được tập hợp từ`t`, việc xóa ký tự không thể làm cho nó không hợp lệ. Điều này tạo ra một ranh giới đơn điệu cho mọi vị trí bắt đầu. Đối với mỗi vị trí`i`, định nghĩa`r[i]`là điểm cuối bên phải lớn nhất sao cho`s[i..r[i]]`là hợp lệ. Sau đó, mọi chuỗi con bắt đầu tại`i`và kết thúc vào hoặc trước`r[i]`cũng hợp lệ. 

Có một tính chất đơn điệu quan trọng khác. Khi vị trí ban đầu di chuyển từ`i`ĐẾN`i + 1`, điểm cuối bên phải hợp lệ tối đa không thể di chuyển sang trái. Đang xóa`s[i]`làm cho cửa sổ hiện tại dễ dàng được đáp ứng hơn, vì vậy`r[i+1] >= r[i]`. Bài xã luận chính thức sử dụng chính xác thuộc tính này để biến các tìm kiếm ranh giới độc lập thành một cửa sổ trượt. 

Do đó chúng ta có thể giữ một cửa sổ`[left, right)`và số lượng ký tự cho cửa sổ đó. Chúng tôi mở rộng`right`trong khi cửa sổ vẫn hợp lệ. Khi thêm ký tự tiếp theo sẽ vi phạm số lượng có sẵn từ`t`, chúng tôi dừng lại. Mỗi chuỗi con bắt đầu từ`left`và kết thúc trước`right`là hợp lệ, vì vậy có`right - left`của họ. Sau đó chúng tôi loại bỏ`s[left]`và di chuyển`left`phía trước. Bởi vì`right`không bao giờ lùi lại, mỗi nhân vật vào và ra khỏi cửa sổ nhiều nhất một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O( | s | ²) | O(26) | Quá chậm | 
| Cửa sổ trượt tối ưu | O( | s | + | t | ) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`s`Và`t`và đếm xem mỗi chữ cái viết thường xuất hiện bao nhiêu lần trong`t`. Số lượng này là tần số tối đa mà một chuỗi con hợp lệ có thể sử dụng. 
2. Giữ nguyên một mảng tần số`have`cho cửa sổ hiện tại của`s`. Cũng giữ`right = 0`và một câu trả lời được khởi tạo bằng 0. Cửa sổ hiện tại là`[left, right)`, Vì thế`right`là vị trí đầu tiên hiện không được bao gồm. 
3. Đối với mỗi`left`từ`0`ĐẾN`len(s) - 1`, cố gắng kéo dài`right`trong khi thêm`s[right]`sẽ không làm cho tần số của nó vượt quá tần số tương ứng trong`t`. Mọi tiện ích mở rộng thành công đều duy trì tính hợp lệ. 
4. Khi không thể thêm ký tự tiếp theo, cửa sổ hiện tại sẽ`[left, right)`là chuỗi con hợp lệ dài nhất bắt đầu từ`left`. Mọi vị trí kết thúc từ`left`bởi vì`right - 1`là hợp lệ, vì vậy hãy thêm`right - left`để trả lời. 
5. Trước khi chuyển sang vị trí bắt đầu tiếp theo, hãy tháo`s[left]`từ`have`. Điều này tương ứng với việc thay đổi cửa sổ từ`[left, right)`ĐẾN`[left + 1, right)`. Chúng tôi không bao giờ di chuyển`right`ngược lại, vì việc loại bỏ ký tự ngoài cùng bên trái không thể làm cho cửa sổ còn lại kém hợp lệ hơn. 
6. In câu trả lời tích lũy. Một số nguyên Python là đủ vì số chuỗi con tối đa là`n(n+1)/2`, đó là về`5 * 10^11`vì`n = 10^6`. 

### Tại sao nó hoạt động 

Đối với mọi`left`, thuật toán duy trì bất biến rằng`[left, right)`là cửa sổ hợp lệ dài nhất bắt đầu từ`left`. Mọi cửa sổ ngắn hơn đều kết thúc trước`right`cũng hợp lệ vì nó không sử dụng thêm bản sao của bất kỳ ký tự nào. Không thể thêm ký tự tiếp theo vì làm như vậy sẽ vượt quá`t`tần số khả dụng của ký tự đó, do đó không còn chuỗi con bắt đầu từ`left`là hợp lệ. 

Sau khi loại bỏ`s[left]`, trước đó`right`vẫn là ranh giới hợp lệ cho vị trí bắt đầu mới, vì việc xóa một ký tự không thể tăng bất kỳ tần số nào. Thuật toán sau đó mở rộng`right`chỉ trong chừng mực cần thiết. Do đó, mỗi chuỗi con hợp lệ được tính chính xác một lần, tại điểm cuối bên trái của chính nó và không có chuỗi con không hợp lệ nào được tính. Từ`right`chỉ di chuyển về phía trước, tổng số lần mở rộng và xóa cửa sổ là tuyến tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    t = input().strip()

    limit = [0] * 26
    for ch in t:
        limit[ord(ch) - 97] += 1

    have = [0] * 26
    n = len(s)
    right = 0
    answer = 0

    for left in range(n):
        while right < n:
            c = ord(s[right]) - 97
            if have[c] == limit[c]:
                break

            have[c] += 1
            right += 1

        answer += right - left

        c = ord(s[left]) - 97
        if left < right:
            have[c] -= 1

    print(answer)

if __name__ == "__main__":
    solve()
```Phần đầu tiên xây dựng`limit`, Ở đâu`limit[c]`chính xác có bao nhiêu bản sao của ký tự`c`có sẵn ở`t`. Chúng ta không bao giờ cần lưu trữ vị trí thực tế của các ký tự trong`t`, bởi vì thứ tự của chúng không có liên quan.`have`lưu trữ các tần số bên trong chuỗi con hiện tại. điều kiện`have[c] == limit[c]`có nghĩa là việc thêm một bản sao khác của ký tự`c`sẽ làm cho cửa sổ không hợp lệ. Chúng ta có thể dừng ngay lập tức thay vì thêm nó và sửa chữa cửa sổ sau đó. 

biểu thức`right - left`là số chuỗi con hợp lệ bắt đầu tại`left`. Cửa sổ sử dụng lập chỉ mục nửa mở, vì vậy`[left, right)`chứa chính xác`right - left`nhân vật. Tiền tố không trống của nó kết thúc tại`left`,`left + 1`, bởi vì`right - 1`, đưa ra chính xác nhiều sự lựa chọn. 

Sau khi đếm các chuỗi con đó, ký tự tại`left`được gỡ bỏ. các`if left < right`Guard xử lý trường hợp cửa sổ trống. Trong tình huống đó`right == left`, do đó không có ký tự nào cần xóa khỏi cửa sổ được duy trì. Điều này cũng xử lý các chuỗi trong đó một ký tự cụ thể không xuất hiện trong`t`. 

Không cần tìm kiếm nhị phân, cấu trúc hậu tố hoặc bảng tần số hai chiều. Bảng chữ cái chỉ có 26 ký tự và sự đơn điệu của ranh giới bên phải mang lại toàn bộ giải pháp với một cửa sổ chuyển động. Cách tiếp cận thời gian tuyến tính là ý tưởng cốt lõi tương tự được mô tả trong bài xã luận chính thức của ITMO. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
aaa
aa
```Giới hạn tần số là hai bản sao của`a`. Cửa sổ phát triển như sau. 

|`left`|`right`trước khi gia hạn | Cửa sổ sau khi mở rộng | Đã thêm chuỗi con |`answer`| 
| --- | --- | --- | --- | --- | 
| 0 | 0 |`aa`| 2 | 2 | 
| 1 | 2 |`aa`| 2 | 4 | 
| 2 | 3 |`a`| 1 | 5 | 

Tại`left = 0`, cửa sổ có thể chứa hai`a`ký tự nhưng không phải ba. Loại bỏ cái đầu tiên`a`làm cho cửa sổ hai ký tự bắt đầu ở vị trí 2 hợp lệ, vì vậy`right`không cần phải lùi lại. Ở vị trí cuối cùng chỉ còn lại một ký tự. Câu trả lời là`5`. 

### Mẫu 2 

Đầu vào là```
abacaba
abc
```Giới hạn tần số là một`a`, một`b`, và một`c`. 

|`left`|`right`trước khi gia hạn | Thời hạn hợp lệ dài nhất | Đã thêm chuỗi con |`answer`| 
| --- | --- | --- | --- | --- | 
| 0 | 0 |`ab`| 2 | 2 | 
| 1 | 2 |`bac`| 3 | 5 | 
| 2 | 4 |`ac`| 2 | 7 | 
| 3 | 4 |`ca`| 2 | 9 | 
| 4 | 6 |`ab`| 2 | 11 | 
| 5 | 6 |`ba`| 2 | 13 | 
| 6 | 7 |`a`| 1 | 14 | 

Bảng này dường như cung cấp`14`, nhưng cửa sổ ở`left = 0`thực sự có thể được mở rộng để`aba`chỉ khi hai`a`các ký tự đã có sẵn, nhưng chúng không có. Bảng liệt kê chính xác từ câu lệnh chứa bảy chuỗi con một ký tự, sáu chuỗi con có độ dài hai ký tự hợp lệ và hai chuỗi con có độ dài ba hợp lệ, ví dụ:`15`. Cửa sổ dài ba còn thiếu là`bac`Tại`left = 1`, đã được tính là ba chuỗi con:`b`,`ba`, Và`bac`. Việc tính toán lại các phép cộng một cách cẩn thận sẽ mang lại`2 + 3 + 2 + 2 + 3 + 2 + 1 = 15`. Do đó, đầu ra cuối cùng là`15`. 

Ví dụ này giải thích tại sao thuật toán đếm tất cả các tiền tố của cửa sổ hợp lệ dài nhất, thay vì chỉ đếm chính chuỗi con dài nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | s | + | t | ) | Xây dựng tần số quét giới hạn`t`một lần, trong khi con trỏ phải và con trỏ trái mỗi lần di chuyển`s`nhiều nhất một lần. | 
| Không gian | O(1) | Chỉ có hai mảng tần số 26 ký tự được duy trì. | 

Đối với các chuỗi có độ dài lên tới`10^6`, thuật toán chỉ thực hiện một lượng công việc không đổi cho mỗi ký tự. Câu trả lời có thể đạt được khoảng`5 * 10^11`, nhưng số nguyên Python xử lý việc này một cách trực tiếp. Việc sử dụng bộ nhớ không đổi ngoại trừ hai chuỗi đầu vào, do đó giải pháp vừa vặn thoải mái với giới hạn bộ nhớ 512 MB đã nêu. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    data = inp.split()
    s = data[0]
    t = data[1]

    limit = [0] * 26
    for ch in t:
        limit[ord(ch) - 97] += 1

    have = [0] * 26
    right = 0
    answer = 0

    for left in range(len(s)):
        while right < len(s):
            c = ord(s[right]) - 97
            if have[c] == limit[c]:
                break
            have[c] += 1
            right += 1

        answer += right - left

        c = ord(s[left]) - 97
        if left < right:
            have[c] -= 1

    return str(answer)

assert solution("""aaa
aa
""") == "5", "sample 1"

assert solution("""abacaba
abc
""") == "15", "sample 2"

assert solution("""a
b
""") == "0", "no valid substring"

assert solution("""aaaa
a
""") == "4", "all valid substrings have length one"

assert solution("""abcabc
abc
""") == "15", "boundary at exactly three characters"

s = "a" * 1000000
t = "a" * 1000000
expected = 1000000 * 1000001 // 2
assert solution(s + "\n" + t + "\n") == str(expected), "maximum-size all-equal case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`Và`b`|`0`| Đầu vào có kích thước tối thiểu và trường hợp không có chuỗi con nào hợp lệ. | 
|`aaaa`Và`a`|`4`| Các ký tự lặp lại và thực tế là cùng một ký tự ở các vị trí khác nhau xác định các chuỗi con khác nhau. | 
|`abcabc`Và`abc`|`15`| Ranh giới chính xác nơi một cửa sổ có thể chứa từng ký tự khả dụng một lần, cộng với các lỗi ký tự lặp lại. | 
| Một triệu`a`ký tự trong cả hai chuỗi |`500000500000`| Kích thước đầu vào tối đa, câu trả lời lớn và chuyển động con trỏ tuyến tính. | 

## Vỏ cạnh 

Khi không có nhân vật của`s`có sẵn ở`t`, cửa sổ không thể phát triển. Vì```
a
b
```ban đầu`right`là`0`và nỗ lực thêm`s[0]`ngay lập tức nhìn thấy`have[a] == limit[a] == 0`. Như vậy`right - left`bằng 0 và câu trả lời vẫn còn`0`. Sau khi thăng tiến`left`, thuật toán kết thúc mà không tính chuỗi con không hợp lệ. 

Các ký tự lặp lại được xử lý bằng cách so sánh tần số thay vì chỉ kiểm tra sự hiện diện của ký tự. Vì```
aaa
aa
```giới hạn cho`a`là`2`. Bắt đầu từ vị trí 0, thuật toán thêm hai`a`ký tự và dừng lại trước ký tự thứ ba. Nó góp phần`2`, tương ứng với`a`Và`aa`. Sau khi loại bỏ cái đầu tiên`a`, cửa sổ vẫn chứa hai`a`ký tự, do đó vị trí bắt đầu thứ hai cũng góp phần`2`. Vị trí cuối cùng góp phần`1`, sản xuất`5`. 

Một cửa sổ có thể đạt chính xác tần số sẵn có mà không bị vô hiệu. Vì```
abcabc
abc
```ba ký tự đầu tiên tạo thành một cửa sổ hợp lệ vì tần số chính xác là một`a`, một`b`, và một`c`. Tiếp theo`a`không thể thêm được vì`t`không có thứ hai`a`. Do đó thuật toán đếm cả ba tiền tố khác rỗng của`abc`, sau đó dịch chuyển ranh giới bên trái trong khi vẫn giữ nguyên ranh giới bên phải bất cứ khi nào có thể. 

Cuối cùng, trường hợp kích thước tối đa cho biết việc triển khai có vô tình di chuyển con trỏ phải về phía sau hoặc quét lại cửa sổ hay không. Với một triệu`a`ký tự trong cả hai chuỗi, mọi chuỗi con đều hợp lệ, vì vậy câu trả lời là`1 + 2 + ... + 10^6 = 500000500000`. Con trỏ bên phải tiến lên chính xác một triệu lần, con trỏ bên trái tiến lên chính xác một triệu lần và không có phép liệt kê bậc hai nào xảy ra.
