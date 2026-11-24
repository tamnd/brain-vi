---
title: "CF 63C - Bò đực và bò cái"
description: "Chúng ta đang chơi trò chơi Bulls and Cows cổ điển với các số có bốn chữ số có các chữ số khác nhau. Cho phép sử dụng các số 0 đứng đầu, vì vậy 0123 là hợp lệ, nhưng các chữ số lặp lại như 0012 hoặc 1223 thì không. Mỗi lần đoán trước có hai giá trị."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 63
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 59 (Div. 2)"
rating: 1700
weight: 63
solve_time_s: 108
verified: true
draft: false
---

[CF 63C - Bò đực và bò cái](https://codeforces.com/problemset/problem/63/C) 

**Đánh giá:** 1700 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang chơi trò chơi Bulls and Cows cổ điển với các số có bốn chữ số có các chữ số khác nhau. Cho phép các số 0 đứng đầu, vì vậy`0123`là hợp lệ, nhưng các chữ số lặp lại như`0012`hoặc`1223`thì không. 

Mỗi lần đoán trước có hai giá trị. Một con bò đực có nghĩa là một chữ số khớp cả về giá trị và vị trí. Con bò có nghĩa là chữ số tồn tại trong số ẩn nhưng xuất hiện ở một vị trí khác. 

Nhiệm vụ không phải là mô phỏng chính trò chơi. Chúng ta phải xác định xem các câu trả lời trước đó có xác định duy nhất số ẩn hay không. 

Có ba kết quả có thể xảy ra. 

Nếu chính xác một số hợp lệ thỏa mãn tất cả các dự đoán, chúng tôi sẽ in số đó. 

Nếu không có số hợp lệ nào thỏa mãn dự đoán thì các câu trả lời sẽ mâu thuẫn với nhau và chúng ta in ra`Incorrect data`. 

Nếu vẫn có thể có nhiều số hợp lệ, chúng tôi sẽ in`Need more data`. 

Những hạn chế là rất nhỏ. Có tối đa 10 lần đoán. Một số có bốn chữ số khác nhau có thể lập được như sau:$$10 \times 9 \times 8 \times 7 = 5040$$cách. 

Điều đó ngay lập tức thay đổi bản chất của vấn đề. Chúng ta không cần cắt tỉa thông minh, lập trình động hoặc cây tìm kiếm. Ngay cả việc so sánh mọi ứng viên với mọi dự đoán cũng rất nhỏ:$$5040 \times 10 = 50400$$so sánh. 

Mỗi so sánh chỉ kiểm tra bốn vị trí và nhiều nhất là một vài chữ số trùng khớp. Điều này thoải mái phù hợp trong thời hạn. 

Khó khăn chính không phải là hiệu suất, mà là tính chính xác. Bò đực và bò cái rất dễ nhầm lẫn nếu thực hiện không cẩn thận. 

Một trường hợp khó nhận thấy là xử lý chính xác các số 0 đứng đầu. 

Coi như:```
1
0123 4 0
```Câu trả lời đúng là:```
0123
```Việc triển khai bất cẩn sẽ chuyển đổi mọi thứ thành số nguyên và sau đó trở lại thành chuỗi có thể vô tình biến thành`0123`vào trong`123`, mất số 0 đứng đầu. 

Một sai lầm dễ mắc phải khác là đếm bò không chính xác bằng cách đếm hai lần bò đực. 

Giả sử số ẩn là`0123`và phỏng đoán là`0124`. 

Kết quả đúng là`3 bulls 0 cows`. 

Nếu trước tiên chúng ta đếm các chữ số chung và sau đó đếm riêng từng con bò đực mà không trừ chúng, chúng ta có thể báo cáo sai một con bò cho chữ số.`0`,`1`, hoặc`2`. 

Dữ liệu mâu thuẫn là một trường hợp quan trọng khác.```
2
0123 4 0
0123 0 4
```Không có số nào có thể thỏa mãn cả hai câu lệnh cùng một lúc. Đầu ra đúng là:```
Incorrect data
```Quá trình triển khai dừng sau khi tìm thấy ứng viên hợp lệ đầu tiên sẽ in không chính xác`0123`. 

Trường hợp quan trọng cuối cùng là sự mơ hồ.```
1
0123 0 0
```Điều này chỉ cho chúng ta biết số ẩn không chứa`0,1,2,3`. Nhiều số hợp lệ vẫn còn. Đầu ra đúng là:```
Need more data
```Chương trình phải đếm tất cả các ứng cử viên hợp lệ, không chỉ kiểm tra xem có tồn tại hay không. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. 

Chúng tôi tạo ra mọi số có bốn chữ số hợp lệ với các chữ số riêng biệt, bao gồm các số có số 0 đứng đầu. Đối với mỗi ứng cử viên, chúng tôi so sánh nó với mọi dự đoán trước đó. Nếu tất cả số lượng bò đực và bò cái đều khớp chính xác thì ứng cử viên vẫn có thể tham gia. 

Điều này hoạt động vì không gian tìm kiếm rất nhỏ. Chỉ có 5040 ứng viên. 

Để so sánh hai số, chúng ta đếm số bò đực bằng cách kiểm tra các vị trí bằng nhau. Sau đó chúng ta đếm xem có bao nhiêu chữ số xuất hiện trong cả hai số. Vì các chữ số khác nhau nên số bò trở thành:$$\text{common digits} - \text{bulls}$$Giải pháp vũ phu đã đủ nhanh rồi. Không có hạn chế tiềm ẩn nào lớn hơn yêu cầu tối ưu hóa ngoài điều này. 

Cái nhìn sâu sắc thực sự là nhận ra rằng quy mô của vấn đề về cơ bản bị giới hạn bởi cấu trúc của trò chơi. Bốn chữ số riêng biệt từ mười khả năng tạo ra một vũ trụ cố định chỉ có 5040 trạng thái. Một khi chúng ta nhận ra điều đó, việc kiểm tra toàn diện sẽ trở thành giải pháp sạch sẽ và an toàn nhất. 

Cách tiếp cận phức tạp hơn sẽ chỉ tăng thêm rủi ro khi triển khai mà không cải thiện đáng kể thời gian chạy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với tất cả 5040 ứng viên | O(5040 × n) | O(1) | Đã chấp nhận | 
| Cắt tỉa/tìm kiếm được tối ưu hóa | Không cần thiết | Cao hơn | Được chấp nhận nhưng quá phức tạp | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các dự đoán cùng với số lượng bò đực và bò dự kiến. 
2. Tạo mọi số có bốn chữ số có các chữ số khác nhau. 

Chúng tôi tạo số dưới dạng chuỗi thay vì số nguyên để các số 0 đứng đầu được giữ nguyên một cách tự nhiên. 
3. Đối với mỗi số ứng cử viên, hãy so sánh nó với mọi số liệu dự đoán. 
4. Để so sánh hai số: 

Đếm bò đực bằng cách kiểm tra vị trí`0..3`. 

Đếm các chữ số chung bằng cách kiểm tra xem có bao nhiêu chữ số của ứng viên xuất hiện trong lần đoán. 

Vì tất cả các chữ số đều khác nhau nên các con bò bằng nhau:$$\text{common digits} - \text{bulls}$$5. Nếu bò đực và bò cái được tính toán không khớp với giá trị mong đợi dù chỉ một lần đoán, hãy từ chối ứng viên ngay lập tức. 

Việc từ chối sớm sẽ tránh được những công việc không cần thiết, mặc dù hiệu suất đã dễ dàng đạt được. 
6. Thu thập mọi ứng viên thỏa mãn mọi dự đoán. 
7. Sau khi kiểm tra tất cả các ứng viên: 

Nếu không còn ứng viên nào thì in`Incorrect data`. 

Nếu còn lại chính xác một ứng cử viên, hãy in nó. 

Nếu không thì in`Need more data`. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra mọi số ẩn hợp lệ có thể có chính xác một lần. Một ứng cử viên chỉ sống sót nếu nó tái tạo hoàn hảo mọi phản hồi được ghi lại. 

Vì số ẩn phải thuộc tập hợp tất cả các số có bốn chữ số hợp lệ, nên việc liệt kê đầy đủ đảm bảo rằng: 

Nếu một giải pháp hợp lệ tồn tại, chúng tôi sẽ tìm thấy nó. 

Nếu có nhiều giải pháp thì tất cả các giải pháp đó sẽ được tính. 

Nếu không tồn tại, dữ liệu mâu thuẫn. 

Không có ứng cử viên nào bị bỏ qua và không có ứng cử viên không hợp lệ nào có thể vượt qua được quá trình kiểm tra tính nhất quán. 

## Giải pháp Python```python
import sys
from itertools import permutations

input = sys.stdin.readline

def score(secret, guess):
    bulls = 0

    for i in range(4):
        if secret[i] == guess[i]:
            bulls += 1

    common = 0

    for ch in secret:
        if ch in guess:
            common += 1

    cows = common - bulls

    return bulls, cows

def solve():
    n = int(input())

    guesses = []

    for _ in range(n):
        s, b, c = input().split()
        guesses.append((s, int(b), int(c)))

    valid = []

    for p in permutations("0123456789", 4):
        candidate = "".join(p)

        ok = True

        for g, b, c in guesses:
            cb, cc = score(candidate, g)

            if cb != b or cc != c:
                ok = False
                break

        if ok:
            valid.append(candidate)

    if len(valid) == 0:
        print("Incorrect data")
    elif len(valid) == 1:
        print(valid[0])
    else:
        print("Need more data")

solve()
```các`score`hàm thực hiện các quy tắc Bulls và Cows chính xác. Những con bò đực được tính theo từng vị trí. Các chữ số chung được tính riêng và bò được tính bằng cách trừ đi bò đực. 

Việc sử dụng chuỗi thay vì số nguyên sẽ tự động tránh được mọi vấn đề về số 0 đứng đầu. Ứng viên`"0123"`dài chính xác bốn ký tự trong suốt chương trình.`itertools.permutations`ở đây là hoàn hảo vì nó đã đảm bảo các chữ số riêng biệt. Mỗi bộ dữ liệu được tạo ra chứa bốn ký tự khác nhau được chọn từ`"0123456789"`. 

Vòng lặp chính kiểm tra mọi ứng cử viên dựa trên mọi dự đoán. Khoảnh khắc người ta đoán không đồng ý, ứng viên sẽ bị loại ngay lập tức. 

Quyết định cuối cùng chỉ phụ thuộc vào số lượng ứng viên hợp lệ còn lại. 

Một chi tiết triển khai tinh vi là bò không được tính độc lập. Vì các chữ số là duy nhất nên mỗi chữ số dùng chung là một con bò đực hoặc một con bò cái, không bao giờ là cả hai. Việc trừ số bò đực khỏi tổng số chữ số được chia sẻ sẽ tránh việc vô tình đếm hai lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1263 1 2
8103 2 1
```Chúng tôi kiểm tra từng ứng viên một. 

Giả sử chúng ta đánh giá ứng viên`0123`. 

| Ứng viên | Đoán | Bò đực | Chữ số chung | Bò | Dự kiến ​​| Hợp lệ cho đến nay | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0123 | 1263 | 1 | 3 | 2 | 1 con bò 2 con bò | Có | 
| 0123 | 8103 | 2 | 3 | 1 | 2 con bò 1 con bò | Có |`0123`sống sót. 

Bây giờ giả sử một ứng cử viên khác như`0132`. 

| Ứng viên | Đoán | Bò đực | Chữ số chung | Bò | Dự kiến ​​| Hợp lệ cho đến nay | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0132 | 1263 | 0 | 3 | 3 | 1 con bò 2 con bò | Không | 

Nó sẽ bị từ chối ngay lập tức. 

Sau khi kiểm tra tất cả 5040 ứng viên, vẫn còn nhiều số hợp lệ, do đó kết quả là:```
Need more data
```Ví dụ này chứng tỏ rằng việc đáp ứng mọi dự đoán không nhất thiết quyết định một câu trả lời duy nhất. 

### Ví dụ 2 

đầu vào:```
2
0123 4 0
0123 0 4
```Kiểm tra ứng viên`0123`. 

| Ứng viên | Đoán | Bò đực | Chữ số chung | Bò | Dự kiến ​​| Hợp lệ cho đến nay | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0123 | 0123 | 4 | 4 | 0 | 4 con bò 0 con bò | Có | 
| 0123 | 0123 | 4 | 4 | 0 | 0 con bò 4 con bò | Không | 

Ứng viên thất bại. 

Mọi ứng viên khác đều đã thất bại trong lần đoán đầu tiên. 

Không có ứng cử viên hợp lệ nào sống sót. 

Đầu ra:```
Incorrect data
```Ví dụ này cho thấy các câu trả lời trái ngược nhau đã loại bỏ toàn bộ không gian tìm kiếm như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(5040 × n) | Chúng tôi kiểm tra mọi ứng viên hợp lệ dựa trên tất cả các dự đoán | 
| Không gian | O(1) | Ngoài việc lưu trữ các dự đoán và ứng viên hợp lệ | 

Số lượng ứng cử viên tối đa được cố định là 5040 và`n ≤ 10`. Ngay cả trong trường hợp xấu nhất, số lượng so sánh cũng rất nhỏ. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from itertools import permutations

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def score(secret, guess):
        bulls = 0

        for i in range(4):
            if secret[i] == guess[i]:
                bulls += 1

        common = 0

        for ch in secret:
            if ch in guess:
                common += 1

        cows = common - bulls

        return bulls, cows

    n = int(input())

    guesses = []

    for _ in range(n):
        s, b, c = input().split()
        guesses.append((s, int(b), int(c)))

    valid = []

    for p in permutations("0123456789", 4):
        candidate = "".join(p)

        ok = True

        for g, b, c in guesses:
            cb, cc = score(candidate, g)

            if cb != b or cc != c:
                ok = False
                break

        if ok:
            valid.append(candidate)

    if len(valid) == 0:
        return "Incorrect data"
    elif len(valid) == 1:
        return valid[0]
    else:
        return "Need more data"

# provided sample
assert run(
"""2
1263 1 2
8103 2 1
"""
) == "Need more data", "sample 1"

# unique solution with leading zero
assert run(
"""1
0123 4 0
"""
) == "0123", "leading zero handling"

# contradictory data
assert run(
"""2
0123 4 0
0123 0 4
"""
) == "Incorrect data", "contradictory guesses"

# very weak information
assert run(
"""1
0123 0 0
"""
) == "Need more data", "multiple candidates remain"

# exact full permutation clue
assert run(
"""1
0123 0 4
"""
) == "Need more data", "all digits correct but positions unknown"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0123 4 0`|`0123`| Giữ nguyên các số 0 đứng đầu | 
| Những suy đoán giống hệt nhau đầy mâu thuẫn |`Incorrect data`| Phát hiện dữ liệu không thể | 
|`0123 0 0`|`Need more data`| Xử lý nhiều ứng viên còn lại | 
|`0123 0 4`|`Need more data`| Đếm bò/bò đúng cách | 

## Vỏ cạnh 

Hãy xem xét trường hợp số 0 đứng đầu:```
1
0123 4 0
```Thuật toán tạo ra các ứng cử viên dưới dạng chuỗi, vì vậy`"0123"`vẫn dài đúng bốn ký tự. 

Khi ứng viên này được kiểm tra: 

| Vị trí | Ứng viên | Đoán | Trận đấu | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | Bò | 
| 1 | 1 | 1 | Bò | 
| 2 | 2 | 2 | Bò | 
| 3 | 3 | 3 | Bò | 

Kết quả là`4 bulls 0 cows`, do đó ứng cử viên sống sót. Mọi ứng cử viên khác đều thất bại. Đầu ra trở thành:```
0123
```Bây giờ hãy xem xét thông tin mâu thuẫn:```
2
0123 4 0
0123 0 4
```Lần đoán đầu tiên buộc số ẩn phải chính xác`0123`. 

Lần đoán thứ hai cho biết tất cả các chữ số đều đúng nhưng tất cả các vị trí đều sai, điều này là không thể đối với cùng một số. 

Trong quá trình liệt kê, mọi ứng cử viên đều không đạt được ít nhất một ràng buộc. Danh sách ứng cử viên hợp lệ vẫn trống và thuật toán sẽ in:```
Incorrect data
```Cuối cùng, hãy xem xét sự mơ hồ:```
1
0123 0 0
```Bất kỳ số hợp lệ nào chỉ sử dụng các chữ số từ`{4,5,6,7,8,9}`thỏa mãn điều kiện này. 

Ví dụ,`4567`mang lại: 

| Ứng viên | Chữ số chung | Bò đực | Bò | 
| --- | --- | --- | --- | 
| 4567 | 0 | 0 | 0 | 

Nhiều ứng viên sống sót nên thuật toán đưa ra kết quả chính xác:```
Need more data
```
