---
title: "CF 908A - Thiệp Tết và Đếm"
description: "Chúng tôi nhìn thấy một mặt của mỗi thẻ. Mặt nhìn thấy được có thể là chữ thường hoặc chữ số. Tuyên bố mà chúng tôi muốn xác minh là: \"Bất cứ khi nào một thẻ có một nguyên âm ở một mặt thì mặt kia chứa một chữ số chẵn.\" Chúng tôi có thể lật một số thẻ."
date: "2026-06-12T10:28:05+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 908
codeforces_index: "A"
codeforces_contest_name: "Good Bye 2017"
rating: 800
weight: 908
solve_time_s: 94
verified: true
draft: false
---

[CF 908A - Thẻ năm mới và đếm](https://codeforces.com/problemset/problem/908/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhìn thấy một mặt của mỗi thẻ. Mặt nhìn thấy được có thể là chữ thường hoặc chữ số. 

Tuyên bố chúng tôi muốn xác minh là: 

"Bất cứ khi nào một tấm thẻ có một nguyên âm ở một mặt thì mặt kia sẽ chứa một chữ số chẵn." 

Chúng tôi có thể lật một số thẻ. Nhiệm vụ là tìm số lượng thẻ tối thiểu phải kiểm tra trong trường hợp xấu nhất để xác định xem câu phát biểu có đúng với tất cả các thẻ hay không. 

Quan sát quan trọng là đây là một hàm ý logic: 

> nguyên âm ⇒ chữ số chẵn 

Để xác minh một hàm ý, chúng ta chỉ cần kiểm tra những thẻ có thể vi phạm hàm ý đó. 

Độ dài đầu vào tối đa là 50, rất nhỏ. Ngay cả các giải pháp bậc hai hoặc bậc ba cũng sẽ phù hợp một cách thoải mái. Thách thức không phải là hiệu suất mà là xác định chính xác thẻ nào quan trọng. 

Một lỗi phổ biến là lật từng nguyên âm và từng chữ số. Một số thẻ hiển thị không bao giờ có thể mâu thuẫn với quy tắc, vì vậy việc kiểm tra chúng không cung cấp thông tin gì. 

Hãy xem xét thẻ hiển thị phụ âm`b`.```
b
```Dù bên kia có thế nào thì lá bài này cũng không thể vi phạm hàm ý đó. Nếu mặt ẩn là chữ số lẻ thì mặt hiển thị vẫn không phải là nguyên âm. Nếu mặt ẩn là một chữ cái thì hàm ý đó không liên quan. Đóng góp đúng là 0 lần lật. 

Một sai lầm dễ mắc phải khác là bỏ qua các chữ số lẻ.```
1
```Nếu mặt ẩn là`a`, tấm thẻ sẽ trở thành một ví dụ mẫu: một bên là nguyên âm và một bên là chữ số lẻ. Chúng ta phải lật những lá bài như vậy. Câu trả lời đúng là 1. 

Một trường hợp tinh tế hơn là chữ số`0`.```
0
```Từ`0`chẵn thì nguyên âm ẩn vẫn thỏa mãn quy luật. Lá bài này không bao giờ có thể bác bỏ được hàm ý nên không cần phải lật. Xử lý tất cả các chữ số như nhau sẽ đếm sai. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề là kiểm tra mọi ký tự có thể nhìn thấy và hỏi liệu việc lật lá bài đó có thể tiết lộ sự vi phạm tuyên bố hay không. Vì có tối đa 50 thẻ nên chúng ta có thể phân loại từng thẻ theo cách thủ công. Điều này đã chạy trong thời gian tuyến tính. 

Thách thức thực sự là xác định cách phân loại chính xác. 

Thẻ phải được kiểm tra xem mặt nhìn thấy được của nó có phải là nguyên âm hay không. Mặt ẩn lúc đó phải bằng nhau và chúng ta không thể biết điều đó nếu không nhìn. 

Thẻ cũng phải được kiểm tra xem mặt hiển thị của nó có phải là chữ số lẻ hay không. Nếu phía ẩn là một nguyên âm thì câu phát biểu sẽ sai. 

Tất cả các thẻ khác đều không liên quan. 

Đối với phụ âm, hàm ý không đặt ra hạn chế nào ở phía đối diện. Một phụ âm ghép với một chữ số lẻ là hoàn toàn hợp lệ. 

Đối với các chữ số chẵn, một nguyên âm ẩn vẫn thỏa mãn hàm ý vì chữ số đó là số chẵn. 

Quan sát này làm giảm toàn bộ vấn đề để đếm hai nhóm: 

1. Nguyên âm nhìn thấy được. 
2. Có thể nhìn thấy chữ số lẻ. 

Câu trả lời chỉ đơn giản là quy mô liên minh của họ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân loại Brute Force của mọi thẻ | O(n) | O(1) | Đã chấp nhận | 
| Đếm tối ưu các nguyên âm và chữ số lẻ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. 
2. Tạo một bộ chứa các nguyên âm:`a`,`e`,`i`,`o`,`u`. 
3. Tạo một tập hợp chứa các chữ số lẻ:`1`,`3`,`5`,`7`,`9`. 
4. Khởi tạo đáp án thành 0. 
5. Quét từng ký tự trong chuỗi. 
6. Nếu ký tự là nguyên âm, hãy tăng câu trả lời. 

Chúng ta phải kiểm tra một tấm thẻ như vậy vì mặt ẩn bắt buộc phải là chữ số chẵn. 
7. Ngược lại, nếu ký tự là chữ số lẻ, hãy tăng câu trả lời. 

Chúng ta phải kiểm tra tấm thẻ như vậy vì mặt ẩn có thể là nguyên âm, tạo ra sự vi phạm. 
8. Bỏ qua tất cả các ký tự còn lại. 

Đây là những phụ âm và chữ số chẵn, cả hai đều không thể vi phạm trực tiếp hàm ý. 
9. In số đếm cuối cùng. 

### Tại sao nó hoạt động 

Hàm ý chỉ có thể bị vi phạm bởi một lá bài có hai mặt là:```
vowel + odd digit
```Nếu thẻ hiển thị đã hiển thị một nguyên âm, chúng ta phải kiểm tra xem mặt ẩn có phải là số lẻ hay không. Nếu thẻ hiển thị đã hiển thị một chữ số lẻ, chúng ta phải kiểm tra xem mặt ẩn có phải là nguyên âm hay không. 

Mọi phản ví dụ có thể được phát hiện bằng cách kiểm tra chính xác các thẻ này. Ngược lại, một phụ âm hoặc chữ số chẵn hiển thị không bao giờ có thể tham gia vào hành vi vi phạm bất kể điều gì được ẩn ở phía bên kia. Đếm các nguyên âm nhìn thấy được và các chữ số lẻ nhìn thấy được vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

vowels = {'a', 'e', 'i', 'o', 'u'}
odd_digits = {'1', '3', '5', '7', '9'}

ans = 0

for ch in s:
    if ch in vowels or ch in odd_digits:
        ans += 1

print(ans)
```Việc thực hiện tuân theo lý luận trực tiếp. 

Bộ nguyên âm chứa chính xác các chữ cái kích hoạt hàm ý. Bất cứ khi nào một lá bài xuất hiện, lá bài tương ứng phải được lật. 

Tập hợp chữ số lẻ chứa chính xác các chữ số có thể vi phạm hàm ý nếu một nguyên âm bị ẩn ở phía đối diện. 

Điều kiện sử dụng logic OR vì cả hai trường hợp đều yêu cầu kiểm tra. Không có ký tự nào có thể thuộc cả hai nhóm, nhưng OR thể hiện quy tắc một cách tự nhiên và giữ cho mã đơn giản. 

Kích thước đầu vào rất nhỏ nên chỉ cần quét tuyến tính một lần là quá đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
ee
```| Nhân vật | Nguyên âm? | Chữ số lẻ? | Trả lời | 
| --- | --- | --- | --- | 
| e | Có | Không | 1 | 
| e | Có | Không | 2 | 

Câu trả lời cuối cùng:```
2
```Cả hai thẻ hiển thị đều hiển thị nguyên âm. Mỗi cái phải được kiểm tra vì mặt ẩn của nó bắt buộc phải chẵn. 

### Ví dụ 2 

đầu vào:```
bcdf
```| Nhân vật | Nguyên âm? | Chữ số lẻ? | Trả lời | 
| --- | --- | --- | --- | 
| b | Không | Không | 0 | 
| c | Không | Không | 0 | 
| d | Không | Không | 0 | 
| f | Không | Không | 0 | 

Câu trả lời cuối cùng:```
0
```Mỗi thẻ nhìn thấy được đều là một phụ âm. Hàm ý chỉ hạn chế các thẻ chứa nguyên âm nên không cần lật. 

### Ví dụ 3 

đầu vào:```
a0b7
```| Nhân vật | Nguyên âm? | Chữ số lẻ? | Trả lời | 
| --- | --- | --- | --- | 
| một | Có | Không | 1 | 
| 0 | Không | Không | 1 | 
| b | Không | Không | 1 | 
| 7 | Không | Có | 2 | 

Câu trả lời cuối cùng:```
2
```nguyên âm`a`phải được kiểm tra và chữ số lẻ`7`cũng phải được kiểm tra. Chữ số chẵn`0`và phụ âm`b`không liên quan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần đi qua chuỗi | 
| Không gian | O(1) | Chỉ một số bộ có kích thước cố định nhỏ được lưu trữ | 

Với tối đa 50 ký tự, thuật toán chỉ thực hiện một số thao tác. Nó dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    s = input().strip()

    vowels = {'a', 'e', 'i', 'o', 'u'}
    odd_digits = {'1', '3', '5', '7', '9'}

    ans = 0
    for ch in s:
        if ch in vowels or ch in odd_digits:
            ans += 1

    return str(ans)

# provided sample
assert run("ee\n") == "2", "sample 1"

# custom cases
assert run("b\n") == "0", "single consonant"
assert run("1\n") == "1", "single odd digit"
assert run("0\n") == "0", "single even digit"
assert run("aeiou\n") == "5", "all vowels"
assert run("13579\n") == "5", "all odd digits"
assert run("24680\n") == "0", "all even digits"
assert run("a0b7\n") == "2", "mixed case"
assert run(("a" * 50) + "\n") == "50", "maximum length"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`b`|`0`| Phụ âm không cần kiểm tra | 
|`1`|`1`| Các chữ số lẻ phải được kiểm tra | 
|`0`|`0`| Các chữ số chẵn đều an toàn | 
|`aeiou`|`5`| Mỗi nguyên âm đều góp phần | 
|`13579`|`5`| Mỗi chữ số lẻ đều đóng góp | 
|`24680`|`0`| Không có thẻ liên quan | 
|`a0b7`|`2`| Phân loại hỗn hợp | 
|`a`lặp lại 50 lần |`50`| Độ dài đầu vào tối đa | 

## Vỏ cạnh 

Hãy xem xét một thẻ chỉ hiển thị một phụ âm:```
b
```Thuật toán kiểm tra xem`b`là một nguyên âm hoặc một chữ số lẻ. Không phải cả hai nên đáp án vẫn là 0. Điều này đúng vì không có giá trị ẩn nào có thể làm cho một phụ âm vi phạm hàm ý. 

Xét một tấm thẻ có chữ số lẻ:```
1
```Thuật toán nhận dạng`1`dưới dạng chữ số lẻ và tăng câu trả lời lên 1. Chúng ta phải kiểm tra thẻ này vì mặt ẩn có thể là nguyên âm, tạo ra sự kết hợp bị cấm`(vowel, odd digit)`. 

Xét một tấm thẻ có chữ số chẵn:```
0
```Thuật toán không tính nó. Nếu bên ẩn là nguyên âm thì quy tắc vẫn thỏa vì chữ số là số chẵn. Nếu mặt ẩn không phải là nguyên âm thì quy tắc cũng được thỏa mãn. Không cần lật. 

Hãy xem xét một chuỗi không có nguyên âm nào cả:```
bcdf
```Mọi nhân vật đều bị bỏ qua. Câu trả lời là 0, phù hợp với thực tế là hàm ý tự động đúng khi không có nguyên âm nào xuất hiện trên bất kỳ mặt thẻ nào hiện có.
