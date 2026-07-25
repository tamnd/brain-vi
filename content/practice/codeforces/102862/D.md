---
title: "CF 102862D - Tách văn bản"
description: "Chúng tôi được cung cấp một chuỗi tiếng Anh viết thường. Chúng ta muốn cắt nó thành số mảnh liên tiếp lớn nhất có thể. Một từ chỉ được coi là hợp lệ nếu nó chứa ít nhất một nguyên âm và ít nhất một phụ âm."
date: "2026-07-25T13:50:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102862
codeforces_index: "D"
codeforces_contest_name: "LU ICPC Selection Contest 2020 and KFU Open Contest 2020"
rating: 0
weight: 102862
solve_time_s: 43
verified: true
draft: false
---

[CF 102862D - Tách văn bản](https://codeforces.com/problemset/problem/102862/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi tiếng Anh viết thường. Chúng ta muốn cắt nó thành số mảnh liên tiếp lớn nhất có thể. Một từ chỉ được coi là hợp lệ nếu nó chứa ít nhất một nguyên âm và ít nhất một phụ âm. Đầu ra là số lượng tối đa các phần hợp lệ có thể bao phủ toàn bộ chuỗi. Nếu thậm chí không thể tạo được một mảnh hợp lệ thì câu trả lời là 0. 

Thuộc tính duy nhất của các ký tự quan trọng là loại của chúng. Một ký tự là một trong các nguyên âm`a, i, o, u, e, y`, hoặc nó là một phụ âm. Vị trí của các chữ cái đóng vai trò quan trọng trong việc xây dựng một vách ngăn, nhưng số lượng các phần có thể có chỉ phụ thuộc vào số lượng nguyên âm và phụ âm tồn tại. 

Chiều dài chuỗi có thể đạt tới$10^5$. Điều này loại trừ việc thử mọi tập hợp vị trí cắt có thể vì số lượng phân vùng tăng theo cấp số nhân. Thậm chí một$O(n^2)$Phương pháp lập trình động sẽ không cần thiết và quá chậm so với quét tuyến tính đơn giản. Giải pháp dự định sẽ kiểm tra từng ký tự với số lần không đổi, đưa ra kết quả$O(n)$thuật toán. 

Trường hợp cạnh đầu tiên là khi chuỗi chỉ chứa một loại ký tự. Ví dụ:```
Input:
3
aaa

Output:
0
```Có nguyên âm nhưng không có phụ âm nên không có từ hợp lệ nào có thể tồn tại. Một giải pháp chỉ đếm nguyên âm và chia cho một cái gì đó sẽ tạo ra câu trả lời khẳng định không chính xác. 

Một trường hợp cạnh khác là khi số lượng của loại này nhỏ hơn nhiều so với loại kia:```
Input:
7
aaabbbb

Output:
3
```Có ba nguyên âm và bốn phụ âm. Chúng ta có thể tạo ba từ hợp lệ, nhưng không thể tạo bốn từ, vì mỗi từ đều cần một nguyên âm. Tài nguyên giới hạn là số lượng nhỏ hơn. 

Trường hợp ranh giới cuối cùng là khi cả hai loại xuất hiện đúng một lần:```
Input:
2
ab

Output:
1
```Toàn bộ chuỗi đã là một từ hợp lệ. Cố gắng phân chia ở mọi vị trí có thể sẽ tạo ra các phần một ký tự không hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực tự nhiên là thử mọi phân vùng có thể có của chuỗi và kiểm tra xem mọi phần kết quả có chứa cả nguyên âm và phụ âm hay không. Điều này đúng vì nó kiểm tra mọi câu trả lời có thể, ngoại trừ số phân vùng của một chuỗi có độ dài$n$là$2^{n-1}$, vì mọi khoảng cách giữa các ký tự liền kề đều có thể chứa dấu cắt hoặc không. Vì$n=10^5$, điều này là hoàn toàn không thể. 

Một nỗ lực tốt hơn là xây dựng các từ một cách tham lam từ trái sang phải. Tuy nhiên, quan sát quan trọng là chúng ta thực sự không cần quan tâm đến vị trí chính xác của các vết cắt. Mỗi từ hợp lệ có ít nhất một nguyên âm và ít nhất một phụ âm. Nếu có$v$nguyên âm và$c$phụ âm, không có giải pháp nào có thể có nhiều hơn$\min(v,c)$từ vì mỗi từ yêu cầu một ký tự từ cả hai nhóm. 

Câu hỏi còn lại là liệu giới hạn trên này có luôn luôn đạt được hay không. Nếu tồn tại cả nguyên âm và phụ âm, chúng ta có thể liên tục ghép loại khan hiếm hơn với ký tự của loại khác trong khi vẫn giữ nguyên thứ tự ban đầu. Các ký tự bổ sung thuộc loại phổ biến hơn có thể được gắn đơn giản vào các từ hiện có. Điều này có nghĩa là mọi nguyên âm có sẵn có thể tham gia vào một từ riêng biệt nếu nguyên âm là nguồn hạn chế và lập luận tương tự cũng có tác dụng khi có ít phụ âm hơn. 

Do đó, vấn đề giảm xuống việc đếm hai loại ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu nguyên âm xuất hiện trong chuỗi và bao nhiêu phụ âm xuất hiện trong chuỗi. Câu trả lời chỉ có thể phụ thuộc vào hai đại lượng này vì mỗi từ cần tối thiểu một ký tự từ mỗi danh mục. 
2. Nếu một trong hai số đếm bằng 0, xuất ra`0`. Một từ hợp lệ cần cả hai loại, vì vậy chuỗi không thể được phân vùng. 
3. Ngược lại, xuất ra số nhỏ hơn trong hai số. Đây là số từ tối đa có thể có vì mỗi từ sử dụng một nguyên âm và một phụ âm. 

Tại sao nó hoạt động: 

Số từ tối đa không được vượt quá số nguyên âm hoặc số phụ âm nên không được vượt quá$\min(v,c)$. Nếu cả hai loại đều có mặt, nhóm nhỏ hơn có thể được phân bổ một ký tự cho mỗi từ, trong khi tất cả các ký tự còn lại từ nhóm lớn hơn có thể được đặt vào những từ này mà không làm mất hiệu lực của chúng. Do đó, giới hạn trên luôn có thể đạt được và câu trả lời chính xác là$\min(v,c)$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    vowels = set("aiouey")
    v = 0
    c = 0

    for ch in s:
        if ch in vowels:
            v += 1
        else:
            c += 1

    if v == 0 or c == 0:
        print(0)
    else:
        print(min(v, c))

if __name__ == "__main__":
    solve()
```Mã đầu tiên lưu trữ bộ nguyên âm vì mọi phân loại ký tự đều có cùng một thao tác. Trong quá trình quét, nó tăng một trong hai bộ đếm. Không cần lưu lại thông tin về vị trí vì đáp án cuối cùng được xác định hoàn toàn bằng tổng điểm của cả hai. 

điều kiện`v == 0 or c == 0`trực tiếp xử lý các trường hợp không thể thực hiện được. trận chung kết`min(v, c)`là an toàn vì bằng chứng cho thấy loại nhỏ hơn là yếu tố hạn chế duy nhất. 

Không có hoạt động lập chỉ mục hoặc vòng lặp lồng nhau, do đó không có vấn đề về ranh giới hoặc trường hợp riêng biệt. Số nguyên Python cũng tránh được vấn đề tràn, mặc dù bộ đếm nhiều nhất là$10^5$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
13
brownfoxjumps
```Thuật toán đếm nguyên âm và phụ âm. 

| Bước | Nhân vật | Nguyên âm | Phụ âm | 
| --- | --- | --- | --- | 
| 1 | b | 0 | 1 | 
| 2 | r | 0 | 2 | 
| 3 | o | 1 | 2 | 
| 4 | w | 1 | 3 | 
| 5 | n | 1 | 4 | 
| 6 | f | 1 | 5 | 
| 7 | o | 2 | 5 | 
| 8 | x | 2 | 6 | 
| 9 | j | 2 | 7 | 
| 10 | bạn | 3 | 7 | 
| 11 | m | 3 | 8 | 
| 12 | p | 3 | 9 | 
| 13 | s | 3 | 10 | 

Số cuối cùng là 3 nguyên âm và 10 phụ âm nên đáp án là:```
3
```Điều này chứng tỏ rằng việc thêm phụ âm không làm tăng số lượng từ có thể có. 

### Mẫu 2 

đầu vào:```
4
iota
```| Bước | Nhân vật | Nguyên âm | Phụ âm | 
| --- | --- | --- | --- | 
| 1 | tôi | 1 | 0 | 
| 2 | o | 2 | 0 | 
| 3 | t | 2 | 1 | 
| 4 | một | 3 | 1 | 

Có 3 nguyên âm và 1 phụ âm nên chỉ có thể tạo thành một từ hợp lệ. 

Đầu ra:```
1
```Điều này chứng tỏ rằng loại ký tự hiếm nhất sẽ giới hạn số lượng phân vùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được kiểm tra chính xác một lần. | 
| Không gian | O(1) | Chỉ có hai bộ đếm và một bộ nguyên âm cố định được lưu trữ. | 

Độ dài chuỗi có thể là$10^5$, do đó, một giải pháp tuyến tính dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# provided samples
assert run("13\nbrownfoxjumps\n") == "3\n", "sample 1"
assert run("4\niota\n") == "1\n", "sample 2"
assert run("3\nyou\n") == "0\n", "sample 3"

# custom cases
assert run("2\nab\n") == "1\n", "minimum valid string"
assert run("5\naaaaa\n") == "0\n", "only vowels"
assert run("8\nbbbbbbbb\n") == "0\n", "only consonants"
assert run("7\naaabbbb\n") == "3\n", "different counts"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`ab`|`1`| Từ hợp lệ nhỏ nhất có thể | 
|`aaaaa`|`0`| Thiếu phụ âm | 
|`bbbbbbbb`|`0`| Thiếu nguyên âm | 
|`aaabbbb`|`3`| Danh mục nhỏ hơn giới hạn câu trả lời | 

## Vỏ cạnh 

Đối với một chuỗi chỉ chứa nguyên âm, chẳng hạn như:```
3
aaa
```thuật toán đếm`v = 3`Và`c = 0`. Vì không có phụ âm nên việc kiểm tra số 0 sẽ kích hoạt và đầu ra là`0`. 

Đối với một chuỗi chỉ chứa phụ âm:```
4
bbbb
```các quầy trở thành`v = 0`Và`c = 4`. Một lần nữa, không có từ hợp lệ nào có thể chứa cả hai danh mục bắt buộc, vì vậy câu trả lời là`0`. 

Đối với một chuỗi có một danh mục nhỏ hơn:```
7
aaabbbb
```quá trình quét tìm thấy ba nguyên âm và bốn phụ âm. Câu trả lời là`min(3, 4) = 3`. Ba từ mỗi từ có thể nhận một nguyên âm và một phụ âm, trong khi phụ âm còn lại được thêm vào một trong các từ đó. 

Đối với trường hợp hỗn hợp nhỏ nhất:```
2
ab
```số đếm là một nguyên âm và một phụ âm. Thuật toán trả về`1`, phù hợp với phân vùng duy nhất có thể.
