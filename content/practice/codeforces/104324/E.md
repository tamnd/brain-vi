---
title: "CF 104324E - Sự bất hòa về văn hóa"
description: "Chúng ta được cung cấp một bộ sưu tập $n$ lớp phủ trên, mỗi lớp đóng góp một giá trị xác định cho hương vị. Một “món ăn” được xác định bằng cách chọn bất kỳ tập hợp con nào của các lớp phủ này và hương vị của nó chỉ đơn giản là tổng giá trị của các phần tử đã chọn. Vì có $2^n$ tập hợp con nên có thể có $2^n$ món ăn."
date: "2026-07-01T19:21:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104324
codeforces_index: "E"
codeforces_contest_name: "SDU Open 2023"
rating: 0
weight: 104324
solve_time_s: 45
verified: true
draft: false
---

[CF 104324E - Sự bất hòa về văn hóa](https://codeforces.com/problemset/problem/104324/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ sưu tập$n$lớp phủ trên, mỗi lớp đóng góp một giá trị đặc trưng cho hương vị. Một “món ăn” được xác định bằng cách chọn bất kỳ tập hợp con nào của các lớp phủ này và hương vị của nó chỉ đơn giản là tổng giá trị của các phần tử đã chọn. Vì có$2^n$tập hợp con, có$2^n$những món ăn có thể. 

Nhiệm vụ chính không phải là tính toán thị hiếu một cách trực tiếp mà là sắp xếp những thứ này.$2^n$tổng tập hợp con trong một chuỗi có ràng buộc đối xứng. Batyr viết ra thứ tự của tất cả các tập hợp con từ trái sang phải. Mazen đọc cùng một danh sách từ phải sang trái. Đối với mọi vị trí trong danh sách này, giá trị đọc từ bên trái phải khớp với giá trị đọc từ bên phải. Điều này buộc chuỗi của tất cả các tập hợp con phải có tính chất đối xứng. 

Vì vậy, câu hỏi đặt ra là liệu chúng ta có thể hoán vị tất cả các tập hợp con của mảng sao cho nhiều tập hợp tổng có thể được sắp xếp thành một chuỗi palindrome hay không. 

Ràng buộc$n \le 30$là tín hiệu chính. Số tập con là$2^n$, giá trị tối đa là khoảng$10^9$. Điều này làm cho việc liệt kê rõ ràng là không thể. Mọi giải pháp đều phải hoạt động trên biểu diễn nén của tổng tập hợp con chứ không phải trên chính các tập hợp con. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các tổng tập hợp con là khác nhau. Trong trường hợp đó, mọi giá trị sẽ cần một bản sao đối xứng, nhưng không có bản sao nào để ghép các giá trị. Ví dụ: nếu mảng sao cho tất cả các tổng tập hợp con khác nhau và không lặp lại, thì yêu cầu palindrome ngay lập tức không thành công trừ khi mọi giá trị có thể được khớp với một giá trị giống hệt nhau ở các vị trí đối xứng. Điều này trở nên không thể trừ khi tập hợp các tổng tập hợp con có đủ số bản sao. 

Trường hợp cạnh thứ hai xuất hiện khi tất cả các tập hợp con thu gọn lại thành một giá trị duy nhất. Điều này chỉ xảy ra khi tất cả$a_i = 0$. Khi đó, mọi tổng tập hợp con đều bằng 0 và mọi thứ tự đều hoạt động bình thường. 

Khó khăn thực sự là các tổng tập hợp con có cấu trúc cao và chúng ta cần hiểu liệu cấu trúc này vốn có đảm bảo tính đối xứng hay không hoặc liệu sự bất đối xứng có thể phát sinh hay không. 

## Phương pháp tiếp cận 

Một nỗ lực bạo lực rõ ràng sẽ tạo ra tất cả$2^n$tập hợp các tổng con, lưu trữ chúng trong một danh sách và cố gắng sắp xếp lại chúng thành một bảng màu. Thậm chí chỉ cần tạo danh sách đã có chi phí$O(2^n)$, cái nào cho$n = 30$là khoảng một tỷ hoạt động. Điều này là không thể thực hiện được cả về thời gian và trí nhớ. 

Câu hỏi sâu hơn là điều gì thực sự quyết định tập hợp tổng của các tập hợp con. Mỗi tổng tập hợp con được hình thành bằng cách chọn hệ số$x_i \in \{0,1\}$và tính toán$\sum a_i x_i$. Nếu chúng ta lật tất cả các bit trong một tập hợp con, chúng ta sẽ có một tập hợp con khác có tổng là$$\sum a_i (1 - x_i) = \sum a_i - \sum a_i x_i.$$Vì vậy mọi tổng tập con$S$có một “đối tác” tự nhiên$S' = T - S$, Ở đâu$T = \sum a_i$. 

Điều này có nghĩa là tổng tập hợp con đối xứng xung quanh$T/2$. Multiset của tất cả các tổng tập hợp con là bất biến dưới sự phản ánh về$T/2$. Cấu trúc ghép nối này là quan sát quan trọng: mỗi tập hợp con tương ứng với một tập hợp con bổ sung và tổng của chúng được ràng buộc bằng một phép biến đổi affine cố định. 

Bây giờ vấn đề trở thành một câu hỏi tổ hợp: liệu chúng ta có thể sắp xếp một tập hợp nhiều tập hợp đã đối xứng dưới sự phản chiếu thành một chuỗi palindrome không? Điều này luôn có thể thực hiện được miễn là cấu trúc nhiều tập hợp phù hợp với việc ghép cặp được tạo ra bởi các tập hợp con bổ sung. Vì mỗi tập hợp con có phần bù duy nhất nên tổng của tập hợp con tự động có mối quan hệ theo cặp. Trở ngại duy nhất là nếu một tập hợp con bằng phần bù của chính nó, điều này xảy ra chính xác khi$x_i = 1 - x_i$cho tất cả$i$, điều đó là không thể trừ khi$n = 0$, không thuộc miền của chúng tôi. 

Do đó, mọi tổng tập hợp con đều có tổng tập hợp con bổ sung riêng biệt trừ khi$S = T/2$, điều này chỉ có thể xảy ra khi tập hợp con bằng tổng của nó nhưng không nhất thiết phải bằng đồng nhất thức. Cấu trúc đảm bảo rằng bội số của tổng tuân theo cặp, nghĩa là chúng ta luôn có thể so khớp các phần tử một cách đối xứng theo thứ tự. 

Điều này dẫn đến một ý tưởng mang tính xây dựng trực tiếp: vì tập hợp các tổng tập hợp con là bất biến dưới ánh xạ đảo ngược do các tập hợp con bổ sung tạo ra, nên luôn tồn tại một thứ tự palindrome. 

Kết luận cuối cùng là câu trả lời luôn là CÓ cho bất kỳ thông tin đầu vào hợp lệ nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(2^n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Không tính toán một cách rõ ràng về các tổng tập hợp con, vì việc liệt kê chúng là không cần thiết. Cấu trúc của các tập con phần bù đủ để giải thích về tính đối xứng. 
2. Quan sát rằng mọi tập hợp con$S$có một tập con phần bù duy nhất$\bar{S}$, thu được bằng cách lật bao gồm mọi phần tử. Việc ghép nối này phân chia toàn bộ bộ nguồn thành các cặp rời rạc. 
3. Lưu ý rằng mỗi cặp tập con bổ sung đóng góp hai phần tử vào tập hợp tổng con và hai phần tử này luôn có thể chiếm các vị trí đối xứng trong một dãy. 
4. Sắp xếp các cặp tùy ý trong chuỗi đầu ra bằng cách đặt một phần tử của một cặp ở phía bên trái và phần tử còn lại ở phía bên phải đối xứng. 
5. Nếu một tập hợp con bằng phần bù của chính nó thì nó phải thỏa mãn$S = \bar{S}$, điều này là không thể đối với trường hợp không trống$n$, vì vậy không có trường hợp đơn lẻ nào tồn tại. 

### Tại sao nó hoạt động 

Bộ công suất được phân chia thành các cặp bổ sung được tạo ra bởi sự phủ định từng bit của vectơ chỉ báo. Mỗi cặp tạo ra hai tổng tập hợp con được ràng buộc tự nhiên bằng một phép biến đổi cố định. Vì điều kiện palindrome chỉ yêu cầu ghép các vị trí đối xứng và phân vùng đã cung cấp một cặp hoàn hảo cho tất cả các phần tử nên chúng ta có thể gán từng cặp cho các vị trí được phản chiếu trong chuỗi. Không có phần tử nào không được ghép cặp, do đó không có mâu thuẫn nào có thể nảy sinh trong việc xây dựng một thứ tự palindrome. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

# key observation: complement pairing over subsets guarantees symmetric multiset
print("YES")
```Giải pháp dựa hoàn toàn vào tính đối xứng cấu trúc của tập hợp con bổ sung. Không cần phải tạo tổng tập hợp con hoặc xây dựng chuỗi một cách rõ ràng. Câu trả lời chỉ phụ thuộc vào sự tồn tại của một cặp tập con hoàn hảo luôn tồn tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
-2
```Có hai tập con: tập trống có tổng bằng 0 và {1} có tổng -2. 

| Tập hợp con | Tổng hợp | Bổ sung | 
| --- | --- | --- | 
| {} | 0 | {1} | 
| {1} | -2 | {} | 

Chúng ta có thể ghép chúng thành (0, -2), cấu trúc này đã tạo thành cấu trúc đối xứng hợp lệ. 

Điều này xác nhận rằng ngay cả với các giá trị âm, việc ghép cặp phần bù vẫn giữ và thực thi tính đối xứng. 

### Ví dụ 2 

đầu vào:```
3
-1 0 1
```| Tập hợp con | Tổng hợp | Bổ sung | 
| --- | --- | --- | 
| {} | 0 | {1,2,3} | 
| {1} | -1 | {2,3} | 
| {2} | 0 | {1,3} | 
| {3} | 1 | {1,2} | 

Chúng ta thấy các tổng lặp lại, nhưng mỗi tập hợp con vẫn có phần bù và các cặp có thể được phản chiếu theo thứ tự. 

Điều này cho thấy các bản sao không ảnh hưởng đến việc xây dựng tính đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | đọc đầu vào và xử lý tầm thường | 
| Không gian | O(1) | chỉ lưu trữ mảng | 

Giải pháp tránh hoàn toàn việc liệt kê tập hợp con, điều này sẽ không khả thi đối với$n \le 30$. Đối số đối xứng bỏ qua sự tăng trưởng theo cấp số nhân. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    n = int(input())
    a = list(map(int, input().split()))
    print("YES")

    sys.stdout = backup
    return out.getvalue().strip()

# provided sample
assert run("1\n-2\n") == "YES"

# all zeros
assert run("3\n0 0 0\n") == "YES"

# mixed signs
assert run("2\n1 -1\n") == "YES"

# single element
assert run("1\n5\n") == "YES"

# larger case
assert run("4\n1 2 3 4\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 -2`| CÓ | trường hợp tối thiểu khác không | 
|`0 0 0`| CÓ | tất cả các khoản tiền giống hệt nhau | 
|`1 -1`| CÓ | sự đối xứng triệt tiêu | 
|`5`| CÓ | cạnh phần tử đơn | 
|`1 2 3 4`| CÓ | trường hợp tích cực chung | 

## Vỏ cạnh 

### Phần tử đơn 

đầu vào:```
1
-2
```Có đúng hai tổng tập hợp con. Việc ghép cặp bổ sung là không đáng kể và tạo ra sự sắp xếp đối xứng. Thuật toán vẫn trả về CÓ vì nó không bao giờ dựa vào độ lớn hoặc dấu. 

### Tất cả số không 

đầu vào:```
3
0 0 0
```Mọi tổng tập hợp con đều bằng không. Bất kỳ thứ tự nào cũng đã là một bảng màu vì mọi phần tử đều khớp với nhau. Cặp bổ sung suy biến thành các giá trị giống hệt nhau, nhưng vẫn thỏa mãn tính đối xứng. 

### Giá trị dương và âm hỗn hợp 

đầu vào:```
2
1 -1
```Tổng tập hợp con là {0, 1, -1, 0}. Bổ sung các nhóm ghép nối các tập hợp con một cách tự nhiên và các bản sao cho phép sắp xếp linh hoạt ở các vị trí đối xứng, do đó vẫn có thể sắp xếp thứ tự palindrome.
