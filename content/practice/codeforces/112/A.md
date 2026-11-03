---
title: "CF 112A - Petya và Dây"
description: "Chúng ta được cho hai chuỗi làm bằng các chữ cái tiếng Anh. Cả hai chuỗi đều có cùng độ dài nhưng các chữ cái có thể xuất hiện ở dạng chữ hoa hoặc chữ thường. Nhiệm vụ là so sánh hai chuỗi theo từ điển trong khi hoàn toàn bỏ qua trường hợp chữ cái."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 112
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 85 (Div. 2 Only)"
rating: 800
weight: 112
solve_time_s: 97
verified: true
draft: false
---

[CF 112A - Petya và Dây](https://codeforces.com/problemset/problem/112/A) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi làm bằng các chữ cái tiếng Anh. Cả hai chuỗi đều có cùng độ dài nhưng các chữ cái có thể xuất hiện ở dạng chữ hoa hoặc chữ thường. Nhiệm vụ là so sánh hai chuỗi theo từ điển trong khi hoàn toàn bỏ qua trường hợp chữ cái. 

Bỏ qua trường hợp có nghĩa là`'A'`Và`'a'`phải được coi là các ký tự giống hệt nhau. Sau khi so sánh, chúng tôi in: 

-`-1`nếu chuỗi đầu tiên nhỏ hơn về mặt từ điển 
-`1`nếu chuỗi đầu tiên lớn hơn về mặt từ điển 
-`0`nếu cả hai chuỗi đều bằng nhau 

Các chuỗi rất nhỏ, tối đa là 100. Ngay cả giải pháp từng ký tự không hiệu quả cũng dễ dàng nằm trong giới hạn. Thách thức thực sự không phải là hiệu suất, mà là xử lý chuyển đổi trường hợp một cách chính xác trước khi so sánh. 

Một lỗi phổ biến là so sánh trực tiếp các chuỗi gốc. Python so sánh chữ hoa và chữ thường bằng cách sử dụng các giá trị ASCII, trong đó chữ hoa đứng trước chữ thường. Điều đó tạo ra câu trả lời không chính xác cho vấn đề này. 

Hãy xem xét đầu vào này:```
aaaa
aaaA
```Đầu ra đúng là:```
0
```So sánh trực tiếp mà không chuyển đổi trường hợp sẽ xử lý không chính xác`'a'`Và`'A'`khác nhau như thế nào. 

Một lỗi dễ mắc phải khác là chỉ chuyển đổi một chuỗi thành chữ thường. 

Ví dụ:```
aBc
ABC
```Đầu ra đúng là:```
0
```Nếu chỉ có chuỗi đầu tiên được chuyển đổi thì phép so sánh sẽ trở thành`"abc"`vs`"ABC"`, vẫn không thành công vì giá trị ASCII chữ hoa và chữ thường khác nhau. 

Một trường hợp khác xuất hiện khi các chuỗi chỉ khác nhau ở gần cuối. 

Ví dụ:```
abcz
abca
```Đầu ra đúng là:```
1
```Việc triển khai bất cẩn dừng quá sớm hoặc chỉ so sánh các tiền tố sẽ bỏ lỡ ký tự khác biệt cuối cùng. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là so sánh từng chuỗi ký tự. Đối với mỗi vị trí, chúng ta có thể chuyển đổi thủ công cả hai ký tự sang cùng một kiểu chữ rồi quyết định chuỗi nào lớn hơn. Vị trí khác nhau đầu tiên xác định câu trả lời. 

Phương pháp vũ phu này đã đủ nhanh rồi. Mỗi chuỗi có tối đa 100 ký tự, vì vậy chúng tôi thực hiện tối đa 100 so sánh. Đó là hiệu quả ngay lập tức. 

Chúng ta có thể đơn giản hóa việc triển khai hơn nữa bằng cách nhận thấy rằng Python đã hỗ trợ so sánh chuỗi từ điển. Vấn đề duy nhất là phân biệt chữ hoa chữ thường. Sau khi cả hai chuỗi được chuyển đổi hoàn toàn thành chữ thường, tính năng so sánh tích hợp sẽ hoạt động chính xác theo yêu cầu. 

Phiên bản brute-force hoạt động vì thứ tự từ điển chỉ phụ thuộc vào ký tự khác nhau đầu tiên. Giải pháp sạch hơn hoạt động vì việc chuyển đổi cả hai chuỗi sang cùng một kiểu chữ sẽ giữ nguyên thứ tự bảng chữ cái tương đối của chúng trong khi loại bỏ hoàn toàn sự khác biệt về kiểu chữ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

Đây,`n`là độ dài chuỗi. 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu tiên từ đầu vào. 
2. Đọc chuỗi thứ hai từ đầu vào. 
3. Chuyển cả hai chuỗi thành chữ thường. 

Điều này loại bỏ tất cả sự khác biệt chỉ do chữ hoa và chữ thường gây ra. 
4. So sánh các chuỗi chữ thường theo từ điển. 
5. Nếu chuỗi đầu tiên nhỏ hơn, hãy in`-1`. 
6. Nếu chuỗi đầu tiên lớn hơn, hãy in`1`. 
7. Nếu không, hãy in`0`. 

### Tại sao nó hoạt động 

So sánh từ điển phụ thuộc vào thứ tự tương đối của các ký tự từ trái sang phải. Việc chuyển đổi cả hai chuỗi thành chữ thường sẽ giữ nguyên thứ tự bảng chữ cái trong khi làm cho các phiên bản chữ hoa và chữ thường của cùng một chữ cái giống hệt nhau. Sau khi chuyển đổi, so sánh chuỗi thông thường sẽ tạo ra kết quả chính xác như yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s1 = input().strip().lower()
s2 = input().strip().lower()

if s1 < s2:
    print(-1)
elif s1 > s2:
    print(1)
else:
    print(0)
```Bước đầu tiên đọc cả hai chuỗi và ngay lập tức chuyển chúng thành chữ thường bằng cách sử dụng`.lower()`. Việc thực hiện chuyển đổi sớm sẽ giúp phần còn lại của logic đơn giản và tránh việc xử lý ký tự lặp lại sau này. 

sử dụng`.strip()`là quan trọng bởi vì`input()`bao gồm ký tự dòng mới ở cuối. Nếu không loại bỏ nó, sự so sánh sẽ bao gồm`'\n'`, tạo ra kết quả không chính xác. 

Python đã thực hiện so sánh chuỗi từ điển một cách chính xác. Nó so sánh các ký tự từ trái sang phải và dừng ở chỗ không khớp đầu tiên. Điều đó phù hợp với định nghĩa chính xác mà vấn đề yêu cầu. 

Ba nhánh so sánh ánh xạ trực tiếp tới các đầu ra được yêu cầu. Không cần vòng lặp thủ công vì Python xử lý việc so sánh nội bộ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
aaaa
aaaA
```Sau khi chuyển đổi chữ thường: 

| Biến | Giá trị | 
| --- | --- | 
|`s1`|`"aaaa"`| 
|`s2`|`"aaaa"`| 

Kết quả so sánh: 

| Kiểm tra | Kết quả | 
| --- | --- | 
|`s1 < s2`| Sai | 
|`s1 > s2`| Sai | 
| Đầu ra cuối cùng |`0`| 

Ví dụ này chứng minh tại sao việc chuẩn hóa trường hợp là cần thiết. Các chuỗi gốc chỉ khác nhau về cách viết hoa nên chúng phải so sánh bằng nhau. 

### Ví dụ 2 

đầu vào:```
abs
Abz
```Sau khi chuyển đổi chữ thường: 

| Biến | Giá trị | 
| --- | --- | 
|`s1`|`"abs"`| 
|`s2`|`"abz"`| 

So sánh nhân vật: 

| Vị trí |`s1[i]`|`s2[i]`| Kết quả | 
| --- | --- | --- | --- | 
| 0 |`a`|`a`| bằng | 
| 1 |`b`|`b`| bằng | 
| 2 |`s`|`z`|`s < z`| 

Đầu ra cuối cùng:```
-1
```Dấu vết này cho thấy sự so sánh từ điển phụ thuộc như thế nào vào ký tự khác nhau đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần trong quá trình chuyển đổi và so sánh chữ thường | 
| Không gian | O(n) | Bản sao chuỗi chữ thường được tạo | 

Các ràng buộc cực kỳ nhỏ, với các chuỗi có độ dài tối đa là 100. Giải pháp O(n) dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    s1 = input().strip().lower()
    s2 = input().strip().lower()

    if s1 < s2:
        print(-1)
    elif s1 > s2:
        print(1)
    else:
        print(0)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run("aaaa\naaaA\n") == "0\n", "sample 1"

# custom cases
assert run("a\nb\n") == "-1\n", "single character smaller"
assert run("Z\na\n") == "1\n", "uppercase handling"
assert run("Codeforces\ncodeforces\n") == "0\n", "all equal ignoring case"
assert run("abcz\nabca\n") == "1\n", "difference at last character"

# maximum-size style case
s1 = "A" * 100
s2 = "a" * 100
assert run(f"{s1}\n{s2}\n") == "0\n", "maximum equal strings"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a / b`|`-1`| So sánh kích thước tối thiểu | 
|`Z / a`|`1`| Chuẩn hóa chữ hoa đúng cách | 
|`Codeforces / codeforces`|`0`| Trường hợp bỏ qua bình đẳng | 
|`abcz / abca`|`1`| Sự khác biệt ở vị trí cuối cùng | 
| 100 chữ cái giống nhau pha chữ hoa |`0`| Xử lý độ dài tối đa | 

## Vỏ cạnh 

Hãy xem xét các chuỗi chỉ khác nhau ở cách viết hoa. 

đầu vào:```
ABC
abc
```Thuật toán chuyển đổi cả hai chuỗi thành chữ thường:```
abc
abc
```Các chuỗi trở nên giống hệt nhau, vì vậy đầu ra là:```
0
```Nếu không chuẩn hóa, so sánh ASCII sẽ xử lý không chính xác các chữ cái viết hoa một cách khác nhau. 

Bây giờ hãy xem xét sự không khớp ở ký tự cuối cùng. 

đầu vào:```
abcy
abcz
```Sau khi chuyển đổi:```
abcy
abcz
```Ba vị trí đầu tiên phù hợp. Ở vị trí cuối cùng,`'y' < 'z'`, do đó thuật toán in ra:```
-1
```Điều này xác nhận rằng việc so sánh tiếp tục cho đến khi sự khác biệt thực tế đầu tiên xuất hiện. 

Cuối cùng, hãy xem xét các chuỗi ký tự đơn. 

đầu vào:```
B
a
```Sau khi chuyển đổi chữ thường:```
b
a
```Từ`'b' > 'a'`, đầu ra trở thành:```
1
```Điều này xác nhận rằng thuật toán hoạt động chính xác ngay cả đối với kích thước đầu vào tối thiểu.
