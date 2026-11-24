---
title: "CF 78A - Haiku"
description: "Chúng ta được cho ba dòng tượng trưng cho ba cụm từ của một bài thơ. Một bài thơ haiku hợp lệ phải chứa chính xác 5 chữ cái nguyên âm trong cụm từ đầu tiên, 7 chữ cái ở cụm từ thứ hai và 5 ở cụm từ thứ ba. Đối với vấn đề này, các âm tiết được đơn giản hóa thành số nguyên âm."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 78
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 70 (Div. 2)"
rating: 800
weight: 78
solve_time_s: 210
verified: true
draft: false
---

[CF 78A - Haiku](https://codeforces.com/problemset/problem/78/A) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 3 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba dòng tượng trưng cho ba cụm từ của một bài thơ. Một bài thơ haiku hợp lệ phải chứa chính xác 5 chữ cái nguyên âm trong cụm từ đầu tiên, 7 chữ cái ở cụm từ thứ hai và 5 ở cụm từ thứ ba. 

Đối với vấn đề này, các âm tiết được đơn giản hóa thành số nguyên âm. Chỉ các chữ cái viết thường`a`,`e`,`i`,`o`, Và`u`được coi là nguyên âm. Dấu cách không quan trọng và các từ có thể được phân tách bằng nhiều dấu cách. Khoảng trống ở đầu và cuối cũng được cho phép. 

Nhiệm vụ chỉ đơn giản là đếm các nguyên âm trong mỗi dòng trong số ba dòng và kiểm tra xem số đếm có khớp với số lượng yêu cầu hay không.`5-7-5`mẫu. 

Những hạn chế là rất nhỏ. Mỗi dòng có độ dài tối đa là 100, do đó tổng kích thước đầu vào tối đa là 300 ký tự. Ngay cả một giải pháp không hiệu quả cũng sẽ dễ dàng phù hợp trong thời hạn. Một lần đi qua mỗi chuỗi là quá đủ. 

Khó khăn chính không phải là hiệu năng mà là việc xử lý định dạng một cách chính xác. Nhiều khoảng trắng có thể xuất hiện giữa các từ và có thể có khoảng trắng ở đầu hoặc cuối dòng. Giải pháp cố gắng phân tách các từ một cách bất cẩn có thể vô tình bỏ qua các ký tự hợp lệ hoặc xử lý sai các mã thông báo trống. 

Hãy xem xét đầu vào này:```
on  codeforces  
beta round is running
   a rustling of keys
```Dòng đầu tiên chứa nhiều dấu cách và dấu cách. Đầu ra đúng là`YES`vì khoảng trắng không ảnh hưởng đến việc đếm nguyên âm. 

Một sai lầm dễ mắc phải khác là quên rằng chỉ tính các nguyên âm viết thường. Ví dụ:```
bcdfg
aeiouuu
hello
```Số nguyên âm là`0`,`7`, Và`2`, vậy câu trả lời là`NO`. 

Việc triển khai bất cẩn cũng có thể tính các nguyên âm riêng biệt thay vì tổng số lần xuất hiện nguyên âm. Ví dụ:```
aaaaa
aeiouae
eeeee
```Số lượng là`5`,`7`, Và`5`. Mọi sự xuất hiện của nguyên âm đều quan trọng, không chỉ các chữ cái duy nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xử lý từng ký tự dòng theo ký tự và đếm xem có bao nhiêu ký tự thuộc về bộ nguyên âm`{a, e, i, o, u}`. Sau khi tính toán số lượng cho cả ba dòng, chúng tôi so sánh chúng với`[5, 7, 5]`. 

Ngay cả cách diễn giải thô bạo cũng có thể hoạt động thoải mái ở đây vì đầu vào cực kỳ nhỏ. Giả sử chúng ta liên tục quét từng chuỗi để tìm từng nguyên âm riêng biệt. Mỗi dòng có độ dài tối đa là 100, do đó vẫn chỉ có vài trăm thao tác. 

Quan sát rõ ràng hơn là chúng ta không bao giờ cần ranh giới từ nào cả. Các khoảng trắng không liên quan và mọi phụ âm đều có thể bị bỏ qua. Toàn bộ vấn đề giảm xuống việc đếm các ký tự phù hợp trong mỗi dòng. 

Cái nhìn sâu sắc đó mang lại một giải pháp tuyến tính rất nhỏ gọn. Chúng tôi quét từng dòng một lần, đếm nguyên âm và so sánh kết quả với mẫu được yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(5 × n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

Đây,`n`là tổng số ký tự trên ba dòng. 

## Hướng dẫn thuật toán 

1. Đọc ba dòng đầu vào chính xác như chúng xuất hiện. 
2. Lưu trữ số nguyên âm cần thiết dưới dạng`[5, 7, 5]`. 
3. Với mỗi dòng, quét từng ký tự và đếm xem có bao nhiêu ký tự thuộc bộ nguyên âm`{a, e, i, o, u}``. 
4. So sánh số lượng dòng hiện tại với giá trị mong đợi cho vị trí đó. 

Dòng đầu tiên phải có 5 nguyên âm, dòng thứ hai phải có 7 nguyên âm và dòng thứ ba phải có 5 nguyên âm. 
5. Nếu bất kỳ dòng nào không so sánh được, hãy in`NO`ngay lập tức. 
6. Nếu cả ba dòng đều khớp với số lượng yêu cầu, hãy in`YES`. 

### Tại sao nó hoạt động 

Định nghĩa của một bài thơ haiku hợp lệ trong bài toán này chỉ phụ thuộc vào số lượng chữ cái nguyên âm trong mỗi cụm từ. Thuật toán đếm chính xác những ký tự đó và bỏ qua mọi thứ khác. Vì mỗi ký tự được kiểm tra một lần và mỗi lần xuất hiện nguyên âm đóng góp chính xác một vào số đếm nên tổng số được tính toán là chính xác. So sánh những tổng số này với`5-7-5`khớp trực tiếp với định nghĩa bài toán, do đó thuật toán không thể chấp nhận một bài thơ không hợp lệ hoặc từ chối một bài thơ hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# solution

vowels = set("aeiou")
target = [5, 7, 5]

lines = [input().rstrip('\n') for _ in range(3)]

for i in range(3):
    count = 0

    for ch in lines[i]:
        if ch in vowels:
            count += 1

    if count != target[i]:
        print("NO")
        sys.exit()

print("YES")
```Giải pháp bắt đầu bằng cách lưu trữ tất cả các nguyên âm bên trong một bộ. Việc kiểm tra tư cách thành viên trong một nhóm diễn ra theo thời gian không đổi, giúp quá trình triển khai luôn rõ ràng và hiệu quả. 

Ba dòng được đọc chính xác như đã cho. Chúng tôi chỉ xóa ký tự dòng mới ở cuối bằng`rstrip('\n')`. Sử dụng trơn`strip()`cũng sẽ loại bỏ các khoảng trắng ở đầu và cuối, điều này là không cần thiết. Mặc dù khoảng trắng không ảnh hưởng đến tính chính xác ở đây nhưng việc giữ nguyên định dạng đầu vào ban đầu sẽ an toàn hơn. 

Đối với mỗi dòng, chúng tôi lặp qua từng ký tự và tăng bộ đếm bất cứ khi nào ký tự đó là nguyên âm. 

Sau khi đếm các nguyên âm cho một dòng, ta so sánh ngay kết quả với giá trị yêu cầu. Nếu số đếm khác nhau thì không cần tiếp tục kiểm tra các dòng còn lại nên ta in`NO`và chấm dứt sớm. 

Nếu cả ba dòng đều thỏa mãn số lượng yêu cầu thì bài thơ phù hợp với cấu trúc haiku và chúng ta in ra`YES`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
on  codeforces  
beta round is running
   a rustling of keys
```| Dòng | văn bản | Đếm nguyên âm | Dự kiến ​​| 
| --- | --- | --- | --- | 
| 1 |`on  codeforces`| 5 | 5 | 
| 2 |`beta round is running`| 7 | 7 | 
| 3 |`a rustling of keys`| 5 | 5 | 

Vì cả ba số đếm đều khớp với mẫu được yêu cầu nên kết quả đầu ra là:```
YES
```Ví dụ này chứng minh rằng nhiều khoảng trắng và khoảng trắng ở đầu không quan trọng. Chỉ tính các lần xuất hiện nguyên âm. 

### Ví dụ 2 

đầu vào:```
hello
aeiouae
world
```| Dòng | văn bản | Đếm nguyên âm | Dự kiến ​​| 
| --- | --- | --- | --- | 
| 1 |`hello`| 2 | 5 | 
| 2 |`aeiouae`| 7 | 7 | 
| 3 |`world`| 1 | 5 | 

Dòng đầu tiên đã không đạt yêu cầu, vì vậy kết quả đầu ra đúng là:```
NO
```Dấu vết này cho thấy chỉ một sự không phù hợp cũng đủ để bác bỏ bài thơ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được quét một lần | 
| Không gian | O(1) | Chỉ sử dụng một số quầy và cấu trúc cố định nhỏ | 

Tổng kích thước đầu vào tối đa chỉ là 300 ký tự, vì vậy giải pháp về cơ bản sẽ chạy ngay lập tức. Việc sử dụng bộ nhớ là không đổi và thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    vowels = set("aeiou")
    target = [5, 7, 5]

    lines = [input().rstrip('\n') for _ in range(3)]

    for i in range(3):
        count = 0

        for ch in lines[i]:
            if ch in vowels:
                count += 1

        if count != target[i]:
            print("NO")
            return

    print("YES")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run(
    "on  codeforces  \n"
    "beta round is running\n"
    "   a rustling of keys\n"
) == "YES\n", "sample 1"

# minimum-size invalid case
assert run(
    "a\n"
    "a\n"
    "a\n"
) == "NO\n", "minimum-size case"

# exact 5-7-5 using repeated vowels
assert run(
    "aaaaa\n"
    "aeiouae\n"
    "eeeee\n"
) == "YES\n", "exact vowel counts"

# spaces everywhere
assert run(
    "   aeiou   \n"
    " aeiouae \n"
    "  eeeee\n"
) == "YES\n", "leading and trailing spaces"

# consonants only
assert run(
    "bcdfg\n"
    "hjklmnp\n"
    "qrst\n"
) == "NO\n", "zero vowels"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a / a / a`|`NO`| Đầu vào không hợp lệ kích thước tối thiểu | 
| Nguyên âm lặp đi lặp lại với số lượng chính xác |`YES`| Tổng số lần xuất hiện quan trọng | 
| Đầu vào có nhiều khoảng trắng |`YES`| Dấu cách phải được bỏ qua | 
| Chỉ phụ âm |`NO`| Xử lý nguyên âm không | 

## Vỏ cạnh 

Một trường hợp khó khăn thường gặp là có nhiều dấu cách giữa các từ và xung quanh câu. 

đầu vào:```
   aeiou   
 aeiouae 
  eeeee
```Thuật toán quét từng ký tự. Dấu cách bị bỏ qua vì chúng không phải là nguyên âm. Số đếm trở thành`5`,`7`, Và`5`, do đó đầu ra là:```
YES
```Một trường hợp cạnh khác là các dòng không có nguyên âm nào cả. 

đầu vào:```
bcdfg
hjklmnp
qrst
```Số nguyên âm là`0`,`0`, Và`0`. Dòng đầu tiên đã không đạt được số lượng yêu cầu`5`, do đó thuật toán in ngay lập tức:```
NO
```Một lỗi nhỏ là đếm các nguyên âm riêng biệt thay vì tất cả các lần xuất hiện. 

đầu vào:```
aaaaa
aeiouae
eeeee
```Dòng đầu tiên chứa năm bản sao của cùng một nguyên âm. Thuật toán đếm chính xác từng lần xuất hiện riêng biệt, tạo ra số lượng`5`,`7`, Và`5`. Đầu ra là:```
YES
```
