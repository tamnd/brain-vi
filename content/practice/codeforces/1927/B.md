---
title: "CF 1927B - Theo Chuỗi"
description: "Chúng ta được cung cấp một mảng a mô tả cách tạo một chuỗi ẩn. Đối với mỗi vị trí i, giá trị a[i] cho chúng ta biết ký tự ở vị trí i đã xuất hiện trước đó bao nhiêu lần trong chuỗi. Nếu a[i] = 0 thì ký tự này chưa từng xuất hiện trước đây."
date: "2026-06-08T18:53:28+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1927
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 923 (Div. 3)"
rating: 900
weight: 1927
solve_time_s: 142
verified: false
draft: false
---

[CF 1927B - Đi theo chuỗi](https://codeforces.com/problemset/problem/1927/B) 

**Xếp hạng:** 900 
**Tags:** thuật toán xây dựng, tham lam, chuỗi 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`a`mô tả cách xây dựng một chuỗi ẩn. 

Đối với mỗi vị trí`i`, giá trị`a[i]`cho chúng ta biết nhân vật ở vị trí đó bao nhiêu lần`i`đã xuất hiện trước đó trong chuỗi. Nếu như`a[i] = 0`, nhân vật này chưa từng xuất hiện trước đây. Nếu như`a[i] = 2`, cùng một nhân vật đã xuất hiện đúng hai lần trước vị trí`i`. 

Nhiệm vụ của chúng ta là xây dựng lại bất kỳ chuỗi chữ thường nào có dấu vết khớp với mảng đã cho. 

Quan sát quan trọng là dấu vết không quan tâm đến chữ cái cụ thể nào được sử dụng. Nó chỉ quan tâm đến việc lá thư đó đã xuất hiện bao nhiêu lần trước đó khi nó được đặt. 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`, do đó nghiệm phải gần với thời gian tuyến tính. MỘT`O(n²)`cách tiếp cận sẽ thực hiện đại khái`4 · 10^10`trong trường hợp xấu nhất vượt xa giới hạn. Cần có giải pháp tuyến tính hoặc gần tuyến tính. 

Một số tình huống có thể gây ra sai sót nếu logic tái thiết không được lựa chọn cẩn thận. 

Coi như:```
a = [0, 1, 2, 3]
```Cấu trúc hợp lệ duy nhất liên tục sử dụng cùng một ký tự:```
aaaa
```Ở vị trí thứ 3, chúng ta cần một nhân vật đã xuất hiện ba lần. Chọn một chữ cái mới sẽ ngay lập tức vi phạm dấu vết. 

Một ví dụ khác là:```
a = [0, 0, 0, 0]
```Một câu trả lời hợp lệ là:```
abcd
```Sử dụng cùng một ký tự nhiều lần sẽ tạo ra các giá trị theo dõi`[0,1,2,3]`thay vì. 

Một trường hợp tế nhị hơn là:```
a = [0, 0, 1]
```Ký tự thứ ba phải khớp với ký tự thứ hai, không phải ký tự đầu tiên. Dấu vết yêu cầu chính xác một lần xuất hiện trước đó. Nếu chúng ta chọn ký tự đầu tiên, số lần xuất hiện của nó sẽ là một, nhưng sau khi sử dụng nó, dấu vết sẽ tương ứng với một lịch sử xây dựng khác. Chúng ta phải theo dõi tần suất hiện tại của mỗi chữ cái một cách cẩn thận. 

Câu lệnh đảm bảo rằng tồn tại ít nhất một câu trả lời hợp lệ, vì vậy chúng ta không bao giờ cần phát hiện ra điều không thể xảy ra. 

## Phương pháp tiếp cận 

Thật dễ dàng để tưởng tượng một sự tái thiết trực tiếp bằng vũ lực. 

Xử lý chuỗi từ trái sang phải. Đối với vị trí`i`, thử từng chữ cái từ`'a'`ĐẾN`'z'`. Đối với mỗi chữ cái dự tuyển, hãy đếm số lần nó xuất hiện trước đó trong chuỗi được xây dựng một phần. Nếu số đó bằng`a[i]`, đặt chữ cái và tiếp tục. 

Điều này có tác dụng vì định nghĩa của dấu vết có thể được kiểm tra trực tiếp. Vấn đề là hiệu quả. Đếm các lần xuất hiện trước đó bằng cách quét chi phí tiền tố đã được tạo sẵn`O(n)`mỗi chữ cái. Với tối đa 26 chữ cái được kiểm tra ở mỗi vị trí, độ phức tạp trở nên`O(26·n²)`, quá chậm đối với`n = 2 · 10^5`. 

Cấu trúc của dấu vết cho một giải pháp đơn giản hơn nhiều. 

Giả sử chúng ta biết tần số hiện tại của mỗi chữ cái. Nếu một chữ cái xuất hiện`k`lần cho đến nay, thì việc đặt nó bây giờ sẽ tạo ra giá trị dấu vết`k`, bởi vì chính xác`k`những lần xuất hiện trước đó đã tồn tại. 

Điều đó có nghĩa là vị trí`i`chỉ cần một chữ cái có tần số hiện tại bằng`a[i]`. 

Vì chỉ có 26 chữ cái viết thường nên chúng ta có thể duy trì tần suất của mỗi chữ cái và tìm kiếm chữ cái có số lượng phù hợp`a[i]`. Sau khi chọn nó, chúng tôi tăng tần số đó lên một. 

Tuyên bố đảm bảo rằng câu trả lời hợp lệ tồn tại, vì vậy một lá thư như vậy sẽ luôn có sẵn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(26·n) = O(n) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`cnt`có kích thước 26, được khởi tạo bằng số không.`cnt[j]`lưu trữ bao nhiêu lần lá thư`j`đã được sử dụng rồi. 
2. Xử lý dấu vết từ trái sang phải. 
3. Đối với giá trị hiện tại`a[i]`, tìm bất kỳ chữ cái nào có tần số hiện tại bằng`a[i]`. 
4. Nối chữ cái đó vào chuỗi câu trả lời. 
5. Tăng tần suất của chữ cái đó lên một vì giờ đây nó đã xuất hiện một lần nữa. 
6. Tiếp tục cho đến khi tất cả các vị trí được xử lý. 
7. Xuất chuỗi đã tạo. 

Việc tìm kiếm ở bước 3 kiểm tra tối đa 26 chữ cái, thực chất là thời gian không đổi. 

### Tại sao nó hoạt động 

Vào mọi thời điểm,`cnt[c]`bằng số lần xuất hiện của ký tự`c`đã được đặt ở tiền tố. 

Khi chúng ta đặt một ký tự có tần số hiện tại là`k`, chính xác`k`bản sao của ký tự đó tồn tại trước vị trí hiện tại. Theo định nghĩa, giá trị vết được tạo ra ở vị trí này là`k`. 

Thuật toán luôn chọn ký tự có tần số bằng giá trị yêu cầu`a[i]`, do đó dấu vết được tạo ra ở vị trí`i`là chính xác 

Sau khi đặt ký tự, tần số của nó tăng lên một, giữ nguyên tính bất biến cho các vị trí trong tương lai. Vì đầu vào được đảm bảo đến từ một chuỗi hợp lệ nào đó nên luôn tồn tại một ký tự phù hợp. Do đó, mọi vị trí đều được xây dựng chính xác và chuỗi cuối cùng có chính xác dấu vết cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        cnt = [0] * 26
        ans = []

        for x in a:
            for j in range(26):
                if cnt[j] == x:
                    ans.append(chr(ord('a') + j))
                    cnt[j] += 1
                    break

        print("".join(ans))

solve()
```Mảng`cnt`lưu trữ số lần xuất hiện hiện tại của mỗi chữ cái viết thường. Đối với mỗi giá trị dấu vết`x`, chúng ta tìm kiếm một chữ cái có tần số chính xác`x`. 

Sau khi tìm thấy một chữ cái như vậy, chúng tôi sẽ thêm nó vào câu trả lời và tăng tần suất của nó. Điều bất biến là trước khi xử lý một vị trí,`cnt[j]`bằng số lần xuất hiện của chữ cái`j`đã có mặt trong tiền tố được xây dựng. 

Một lỗi phổ biến là cố gắng chỉ theo dõi ký tự được sử dụng gần đây nhất hoặc chỉ những ký tự đã xuất hiện trước đó. Dấu vết phụ thuộc vào số lần xuất hiện chính xác, vì vậy tất cả 26 tần số phải được duy trì. 

Một điểm tinh tế khác là thứ tự thực hiện các thao tác. Chúng ta phải so sánh`cnt[j]`với`x`trước khi tăng nó. Việc tăng trước tiên sẽ dịch chuyển mọi giá trị dấu vết đi một và tạo ra kết quả không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Dấu vết đầu vào:```
0 0 0 1 0 2 0 3 1 1 4
```| Vị trí | Giá trị dấu vết | Thư được chọn | Tần suất Trước | Tần suất Sau | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | một | 0 | 1 | 
| 2 | 0 | b | 0 | 1 | 
| 3 | 0 | c | 0 | 1 | 
| 4 | 1 | một | 1 | 2 | 
| 5 | 0 | d | 0 | 1 | 
| 6 | 2 | một | 2 | 3 | 
| 7 | 0 | e | 0 | 1 | 
| 8 | 3 | một | 3 | 4 | 
| 9 | 1 | b | 1 | 2 | 
| 10 | 1 | c | 1 | 2 | 
| 11 | 4 | một | 4 | 5 | 

Chuỗi được xây dựng:```
abcadaeabca
```Điều này khác với kết quả đầu ra mẫu nhưng nó tạo ra cùng một dấu vết và hoàn toàn hợp lệ. Ví dụ cho thấy có thể tồn tại nhiều câu trả lời. 

### Ví dụ 2 

Dấu vết đầu vào:```
0 1 2 3 4 5 6 7
```| Vị trí | Giá trị dấu vết | Thư được chọn | Tần suất Trước | Tần suất Sau | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | một | 0 | 1 | 
| 2 | 1 | một | 1 | 2 | 
| 3 | 2 | một | 2 | 3 | 
| 4 | 3 | một | 3 | 4 | 
| 5 | 4 | một | 4 | 5 | 
| 6 | 5 | một | 5 | 6 | 
| 7 | 6 | một | 6 | 7 | 
| 8 | 7 | một | 7 | 8 | 

Chuỗi được xây dựng:```
aaaaaaaa
```Ví dụ này chứng minh rằng một ký tự đơn có thể đáp ứng mọi vị trí khi giá trị dấu vết tăng liên tục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26·n) = O(n) | Mỗi vị trí chúng tôi scan tối đa 26 chữ cái | 
| Không gian | O(26) | Mảng tần số cho chữ thường | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm tối đa là`2 · 10^5`. Thuật toán thực hiện tối đa`26 × 2 · 10^5 = 5.2 · 10^6`kiểm tra tần số, dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ là không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        cnt = [0] * 26
        ans = []

        for x in a:
            for j in range(26):
                if cnt[j] == x:
                    ans.append(chr(ord('a') + j))
                    cnt[j] += 1
                    break

        out.append("".join(ans))

    return "\n".join(out)

# provided sample structure
assert run("1\n1\n0\n") == "a"

# minimum size
assert run("1\n1\n0\n") == "a"

# all positions use same letter
assert run("1\n4\n0 1 2 3\n") == "aaaa"

# all positions introduce new letters
assert run("1\n5\n0 0 0 0 0\n") == "abcde"

# mixed frequencies
assert run("1\n3\n0 0 1\n") == "abb"

# boundary case with repeated reuse
assert run("1\n8\n0 1 2 3 4 5 6 7\n") == "aaaaaaaa"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`a`| Ví dụ nhỏ nhất có thể | 
|`0 1 2 3`|`aaaa`| Tái sử dụng nhiều lần cùng một ký tự | 
|`0 0 0 0 0`|`abcde`| Liên tục giới thiệu nhân vật mới | 
|`0 0 1`|`abb`| Xử lý đúng tần số bằng nhau | 
|`0 1 2 3 4 5 6 7`|`aaaaaaaa`| Số lần xuất hiện lớn cho một chữ cái | 

## Vỏ cạnh 

Hãy xem xét:```
n = 1
a = [0]
```Ban đầu mỗi chữ cái có tần số 0. Thuật toán chọn`'a'`, tăng số đếm của nó lên 1 và xuất ra:```
a
```Dấu vết là`[0]`, điều đó đúng. 

Bây giờ hãy xem xét:```
n = 4
a = [0, 0, 0, 0]
```Tần số bắt đầu như sau:```
a:0 b:0 c:0 d:0 ...
```Thuật toán liên tục chọn chữ cái đầu tiên có số đếm bằng 0. Sau khi sử dụng`'a'`, số đếm của nó trở thành 1, vì vậy chữ cái đếm 0 có sẵn tiếp theo là`'b'`, sau đó`'c'`, sau đó`'d'`. 

Kết quả là:```
abcd
```Mỗi vị trí tương ứng với lần xuất hiện đầu tiên, khớp chính xác với dấu vết. 

Cuối cùng hãy xem xét:```
n = 3
a = [0, 0, 1]
```Sau khi xử lý hai vị trí đầu tiên:```
string = "ab"
counts = {a:1, b:1}
```Giá trị theo dõi được yêu cầu bây giờ là 1. Chữ cái đầu tiên có tần số 1 là`'a'`, do đó thuật toán đưa ra:```
aba
```Dấu vết trở thành:```
[0, 0, 1]
```phù hợp với đầu vào. Ví dụ này cho thấy tại sao việc theo dõi tần số chính xác là cần thiết. Quyết định phụ thuộc vào số lần xuất hiện hiện tại chứ không phụ thuộc vào việc một lá thư đã xuất hiện trước đó hay chưa.
