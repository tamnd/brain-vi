---
title: "CF 102318B - Bàn phím đơn giản"
description: "Sự cố xảy ra khi sử dụng bàn phím tùy chỉnh nhỏ chứa tất cả 26 chữ cái viết thường. Các chữ cái được sắp xếp thành ba hàng: Hai từ được đưa ra cho mỗi trường hợp thử nghiệm. Chúng ta phải phân loại cặp này thành một trong ba loại."
date: "2026-08-13T23:53:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102318
codeforces_index: "B"
codeforces_contest_name: "UCF Locals 2017"
rating: 0
weight: 102318
solve_time_s: 76
verified: true
draft: false
---

[CF 102318B - Bàn phím đơn giản](https://codeforces.com/problemset/problem/102318/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Sự cố xảy ra khi sử dụng bàn phím tùy chỉnh nhỏ chứa tất cả 26 chữ cái viết thường. Các chữ cái được sắp xếp thành ba hàng:```
a b c d e f g h i
j k l m n o p q r
s t u v w x y z
```Hai từ được đưa ra cho mỗi trường hợp thử nghiệm. Chúng ta phải phân loại cặp này thành một trong ba loại. Chúng **giống hệt** nếu chúng có cùng độ dài và mọi ký tự tương ứng đều bằng nhau. Chúng **tương tự** nếu chúng có cùng độ dài, không giống nhau và mọi cặp ký tự tương ứng đều bằng nhau hoặc chiếm các vị trí lân cận trên bàn phím. Nếu không có điều kiện nào xảy ra thì chúng **khác**. 

Tuyên bố cuộc thi ban đầu cung cấp các từ có độ dài từ 1 đến 20 và giá trị đầu vào đầu tiên chỉ định số lượng cặp từ theo sau. 

Độ dài từ nhỏ có nghĩa là không cần cấu trúc dữ liệu nâng cao hoặc các thuật toán phức tạp tiệm cận. Ngay cả việc quét tuyến tính qua từng ký tự cũng rất nhỏ, nhiều nhất là 20 ký tự so sánh cho một trường hợp thử nghiệm. Lựa chọn thiết kế có ý nghĩa duy nhất là chúng ta xác định xem hai chữ cái có phải là hàng xóm bàn phím một cách hiệu quả hay không. Vì bàn phím chỉ có 26 chữ cái cố định nên chúng ta có thể mã hóa trực tiếp từng hàng và cột của từng chữ cái và trả lời câu hỏi đó trong thời gian không đổi. 

Có một số trường hợp việc thực hiện bất cẩn có thể đưa ra phân loại sai. Độ dài khác nhau phải sản xuất ngay`3`. Ví dụ:```
1
ab abc
```Đầu ra đúng là:```
3
```Một chương trình chỉ kiểm tra các ký tự chồng chéo sẽ thấy`a`so với`a`Và`b`so với`b`và có thể gọi sai các từ tương tự. 

Hai từ có cùng độ dài nhưng có ký tự giống nhau phải tạo ra`1`, không`2`. Ví dụ:```
1
cool cool
```Đầu ra đúng là:```
1
```Một chương trình kiểm tra xem mọi cặp có bằng nhau hay lân cận và trả về`2`nếu không kiểm tra trước xem toàn bộ các từ có giống nhau hay không sẽ phân loại sai trường hợp này. 

Sự kề cận dựa trên hình dạng bàn phím, bao gồm cả các đường chéo lân cận. Ví dụ:```
1
knq bxz
```Đầu ra đúng là:```
2
```Đây`k`ở bên cạnh`b`,`n`ở bên cạnh`x`, Và`q`ở bên cạnh`z`. Chỉ coi các phím chạm theo chiều ngang và chiều dọc vì hàng xóm sẽ từ chối cặp này một cách không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp brute-force đơn giản có thể xử lý từng vị trí một cách độc lập. Đối với mỗi cặp chữ cái tương ứng, chúng ta có thể quét tất cả 26 chữ cái trên bàn phím để tìm xem chữ cái thứ hai có thuộc lân cận của chữ cái đầu tiên hay không. Vì một từ chứa tối đa 20 ký tự nên nó thực hiện tối đa (20 \times 26 = 520) kiểm tra chữ cái cho một trường hợp kiểm thử. Trên (n) trường hợp thử nghiệm, trường hợp xấu nhất là (520n) các lần kiểm tra như vậy, ngoài quá trình xử lý đầu vào thông thường. Cách tiếp cận này đúng vì nó trực tiếp kiểm tra định nghĩa về sự giống nhau, nhưng việc tìm kiếm liên tục trên bàn phím 26 chữ cái cố định là công việc không cần thiết. 

Quan sát quan trọng là bàn phím không bao giờ thay đổi. Mỗi chữ cái có một hàng và cột cố định, vì vậy chúng ta có thể ánh xạ một ký tự tới tọa độ của nó bằng chỉ mục bảng chữ cái của nó. Đối với một ký tự có chỉ mục bảng chữ cái`p`, hàng của nó là`p // 9`và cột của nó là`p % 9`. Hai hàng đầu tiên chứa chín chữ cái, trong khi hàng cuối cùng chứa tám chữ cái. 

Hai chữ cái là hàng xóm chính xác khi hiệu số hàng của chúng nhiều nhất là một và hiệu số cột của chúng nhiều nhất là một. Đẳng thức cũng thỏa mãn điều kiện đó nên trước tiên thuật toán có thể xác định xem toàn bộ các từ có giống nhau hay không. Nếu chúng không giống nhau thì chúng ta chỉ cần kiểm tra điều kiện lân cận ở mọi vị trí. 

Điều này làm giảm công việc của từng trường hợp thử nghiệm thành một lần quét các từ. Giải pháp brute-force hoạt động vì bàn phím rất nhỏ nhưng không khai thác được thực tế là hình dạng của nó là cố định. Việc thay thế các tìm kiếm lặp lại bằng số học tọa độ trực tiếp sẽ biến việc phân loại thành một bước tuyến tính đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26L) cho mỗi trường hợp thử nghiệm | O(1) | Đúng nhưng làm việc lặp đi lặp lại không cần thiết | 
| Tối ưu | O(L) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

Ở đây (L) là độ dài từ, với (L \le 20). 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case, sau đó đọc hai từ thuộc test case hiện tại. Việc phân loại là độc lập đối với từng cặp nên mỗi trường hợp đều có thể được xử lý riêng. 
2. Nếu hai từ có độ dài khác nhau, hãy xuất ra`3`. Sự giống nhau đòi hỏi sự tương ứng giữa các ký tự trong toàn bộ các từ, vì vậy độ dài khác nhau khiến cho cả sự đồng nhất và sự giống nhau là không thể. 
3. Nếu hai từ hoàn toàn bằng nhau, xuất ra`1`. Việc kiểm tra này phải diễn ra trước khi kiểm tra độ tương tự vì các từ giống nhau cũng thỏa mãn điều kiện yếu hơn là các chữ cái tương ứng bằng nhau hoặc lân cận. 
4. Nếu không, hãy quét hai từ ở cùng một vị trí. Chuyển đổi từng ký tự thành chỉ mục bảng chữ cái dựa trên số 0 bằng`ord(ch) - ord('a')`. Từ chỉ mục này, tính toán hàng và cột trên bàn phím của nó. 
5. Đối với mỗi cặp tương ứng, hãy kiểm tra xem chênh lệch tuyệt đối của hàng nhiều nhất là một và chênh lệch tuyệt đối của cột nhiều nhất là một. Nếu một trong hai chênh lệch vượt quá một thì cặp đó chứa vị trí không lân cận, do đó đầu ra`3`ngay lập tức. 
6. Nếu toàn bộ quá trình quét kết thúc mà không tìm thấy vị trí sai, hãy xuất ra`2`. Các từ đã được biết là khác nhau và mọi cặp tương ứng đều bằng nhau hoặc lân cận, đó chính xác là định nghĩa về sự giống nhau. 

### Tại sao nó hoạt động 

Điều bất biến trong quá trình quét ký tự là mọi vị trí được xử lý cho đến nay đều chứa các chữ cái bằng nhau hoặc hai phím bàn phím lân cận. Nếu một vị trí vi phạm thuộc tính này thì các từ không thể giống nhau nên việc trả về`3`là đúng. Nếu mọi vị trí đều thỏa mãn tính chất thì các từ có độ dài bằng nhau và thỏa mãn điều kiện giống nhau hoàn toàn. Vì các từ được kiểm tra sự bằng nhau trước tiên nên chúng không giống nhau nên kết quả đúng là`2`. Do đó, ba kết quả đầu ra có thể được phân biệt mà không bị chồng chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def position(ch):
    p = ord(ch) - ord('a')
    return p // 9, p % 9

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        a, b = input().split()

        if len(a) != len(b):
            ans.append("3")
            continue

        if a == b:
            ans.append("1")
            continue

        similar = True

        for x, y in zip(a, b):
            rx, cx = position(x)
            ry, cy = position(y)

            if abs(rx - ry) > 1 or abs(cx - cy) > 1:
                similar = False
                break

        ans.append("2" if similar else "3")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`position`hàm chuyển đổi chỉ mục bảng chữ cái thành tọa độ bàn phím. Sự phân chia theo`9`đúng vì hai hàng đầu tiên có chín chữ cái:`a`bởi vì`i`, sau đó`j`bởi vì`r`. Những chữ cái còn lại`s`bởi vì`z`tạo thành hàng cuối cùng. 

Việc kiểm tra độ dài được ưu tiên hàng đầu vì`zip(a, b)`nếu không sẽ dừng lại ở từ ngắn hơn và âm thầm bỏ qua các ký tự phụ. Điều đó sẽ không chính xác đối với các đầu vào như`ab`Và`abc`. 

Việc kiểm tra đẳng thức diễn ra trước khi quét hàng xóm vì đẳng thức là sự phân loại mạnh hơn. Không có nó, một cặp như`cool`Và`cool`sẽ thỏa mãn điều kiện lân cận và có thể nhận đầu ra không chính xác`2`. 

Kiểm tra hàng xóm sử dụng`abs(row_difference) <= 1`Và`abs(column_difference) <= 1`. Điều này bao gồm các hàng xóm ngang, dọc và chéo. Nó cũng bao gồm cùng một khóa, mặc dù trường hợp đẳng thức đã được xử lý riêng. 

Không có vấn đề tràn số nguyên vì tất cả các tọa độ đều nằm trong khoảng từ 0 đến 8 và dù sao thì số nguyên Python cũng không bị giới hạn. Sớm`break`cũng hữu ích vì một vị trí không hợp lệ cũng đủ để chứng minh rằng các từ khác nhau chứ không phải giống nhau. 

## Ví dụ đã hoạt động 

Vì trang Codeforces hiện tại không hiển thị các khối mẫu chính thức nên các dấu vết sau đây sử dụng các ví dụ từ tuyên bố cuộc thi ban đầu và các trường hợp nhỏ bổ sung. Tuyên bố ban đầu đưa ra một cách rõ ràng`aaaaa`so với`abkja`,`moon`so với`done`, Và`knq`so với`bxz`như những ví dụ tương tự. 

### Ví dụ 1 

đầu vào:```
1
moon done
```Hai từ có độ dài bằng nhau và không giống nhau. Các chữ cái tương ứng của chúng được kiểm tra như sau. 

| Vị trí | Đầu tiên | Thứ hai | Tọa độ đầu tiên | Tọa độ thứ hai | Hàng xóm? | 
| --- | --- | --- | --- | --- | --- | 
| 0 | m | d | (1,3) | (0,3) | vâng | 
| 1 | o | o | (1,5) | (1,5) | vâng | 
| 2 | o | n | (1,5) | (1,4) | vâng | 
| 3 | n | e | (1,4) | (0,4) | vâng | 

Mọi vị trí đều thỏa mãn điều kiện lân cận, vì vậy đầu ra cuối cùng là:```
2
```Dấu vết này chứng tỏ tại sao sự kề cận theo chiều dọc và chiều ngang đều được chấp nhận. Nó cũng chứng minh thực tế là cùng một chữ cái được coi là có thể chấp nhận được trong quá trình kiểm tra tính tương tự. 

### Ví dụ 2 

đầu vào:```
1
ab abc
```Từ đầu tiên có độ dài 2 và từ thứ hai có độ dài 3. 

| Bước | Từ đầu tiên | Từ thứ hai | Tình trạng | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 |`ab`|`abc`| độ dài khác nhau |`3`| 

Thuật toán dừng trước khi so sánh các ký tự vì không có từ nào có độ dài khác nhau có thể giống hệt nhau hoặc tương tự nhau. 

Ví dụ này giải thích tại sao phải kiểm tra độ dài trước khi sử dụng`zip`, nếu không thì vô song`c`sẽ biến mất khỏi so sánh. 

### Ví dụ 3 

đầu vào:```
1
knq bxz
```Việc kiểm tra tọa độ là: 

| Vị trí | Đầu tiên | Thứ hai | Tọa độ đầu tiên | Tọa độ thứ hai | Hàng xóm? | 
| --- | --- | --- | --- | --- | --- | 
| 0 | k | b | (1,1) | (0,1) | vâng | 
| 1 | n | x | (1,4) | (2,5) | vâng | 
| 2 | q | z | (1,7) | (2,7) | vâng | 

Mọi cặp đều là lân cận nên kết quả là:```
2
```Dấu vết này xác nhận cụ thể rằng chuyển động theo đường chéo được cho phép. Cặp đôi`n`Và`x`khác nhau một hàng và một cột, vì vậy nó là cặp lân cận hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nL) | Mỗi (n) trường hợp kiểm thử quét tối đa (L \le 20) cặp ký tự | 
| Không gian | O(n) | Việc triển khai lưu trữ tất cả các chuỗi đầu ra trước khi in | 

Các từ đầu vào cực kỳ ngắn, độ dài tối đa là 20 ký tự. Thuật toán chỉ thực hiện một lượng số học không đổi cho mỗi ký tự, do đó, ngay cả một số lượng lớn các trường hợp thử nghiệm cũng được xử lý thoải mái trong giới hạn 1 giây và 256 MB được chỉ định cho bài toán cuộc thi. 

Danh sách đầu ra sử dụng bộ nhớ O(n). Nó có thể được in ngay lập tức để giảm khoảng trống đó xuống còn O(1), nhưng việc đệm các chuỗi đầu ra nhỏ thì đơn giản hơn và vẫn nằm trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def position(ch):
    p = ord(ch) - ord('a')
    return p // 9, p % 9

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        a, b = input().split()

        if len(a) != len(b):
            ans.append("3")
            continue

        if a == b:
            ans.append("1")
            continue

        similar = True

        for x, y in zip(a, b):
            rx, cx = position(x)
            ry, cy = position(y)

            if abs(rx - ry) > 1 or abs(cx - cy) > 1:
                similar = False
                break

        ans.append("2" if similar else "3")

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Examples from the original statement
assert run("1\ncool cool\n") == "1", "identical words"
assert run("1\nmoon done\n") == "2", "similar words"

# Minimum-size input
assert run("3\na a\na b\na c\n") == "1\n2\n3", "single-character cases"

# Maximum word length, identical
assert run("1\n" + "a" * 20 + " " + "a" * 20 + "\n") == "1", \
    "maximum length identical words"

# Diagonal and vertical neighbors
assert run("2\nknq bxz\naaa bkk\n") == "2\n2", \
    "diagonal and multi-position adjacency"

# Different lengths and a non-neighboring pair
assert run("3\nab abc\nab cb\naz za\n") == "3\n3\n3", \
    "length and boundary failures"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a a`,`a b`,`a c`|`1`,`2`,`3`| Các từ có kích thước tối thiểu và ranh giới liền kề | 
| Hai từ 20 ký tự giống hệt nhau |`1`| Độ dài từ tối đa được phép và xử lý bình đẳng | 
|`knq bxz`,`aaa bkk`|`2`,`2`| Hàng xóm bàn phím dọc và chéo | 
|`ab abc`,`ab cb`,`az za`|`3`,`3`,`3`| Độ dài khác nhau, các chữ cái không liền kề và vị trí đảo ngược | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là độ dài từ khác nhau. Coi như:```
1
ab abc
```Thuật toán so sánh độ dài trước và thấy`2 != 3`. Nó ngay lập tức tạo ra`3`. Không có quá trình quét ký tự nào được thực hiện, vì vậy`c`không thể vô tình bỏ qua. 

Trường hợp cạnh thứ hai là các từ giống hệt nhau:```
1
cool cool
```Độ dài khớp nhau và kiểm tra đẳng thức thành công, do đó thuật toán tạo ra`1`ngay lập tức. Nó không bao giờ đạt đến bài kiểm tra tương tự. Thứ tự này quan trọng vì mọi cặp giống hệt nhau cũng đáp ứng yêu cầu yếu hơn là các ký tự tương ứng phải bằng nhau hoặc lân cận. 

Trường hợp cạnh thứ ba là cạnh chéo:```
1
knq bxz
```Cặp đầu tiên là`k`Tại`(1,1)`Và`b`Tại`(0,1)`, liền kề nhau theo chiều dọc. Cặp thứ hai là`n`Tại`(1,4)`Và`x`Tại`(2,5)`, liền kề nhau theo đường chéo. Cặp thứ ba là`q`Tại`(1,7)`Và`z`Tại`(2,7)`, liền kề nhau theo chiều dọc. Mỗi cặp đều vượt qua, vì vậy câu trả lời là`2`. 

Trường hợp cạnh cuối cùng là một cặp trông gần nhau theo thứ tự bảng chữ cái nhưng không liền kề về mặt hình học:```
1
ab cb
```Vị trí đầu tiên so sánh`a`Tại`(0,0)`với`c`Tại`(0,2)`. Hiệu hàng của chúng bằng 0, nhưng hiệu hiệu cột của chúng là hai, vì vậy chúng không phải là hàng xóm của nhau. Thuật toán ngắt ngay lập tức và xuất ra`3`. Đây là lý do tại sao việc so sánh các chỉ số bảng chữ cái, chẳng hạn như kiểm tra xem sự khác biệt về số của chúng có nhiều nhất là một hay không, sẽ là cách giải thích không chính xác về bố cục bàn phím.
