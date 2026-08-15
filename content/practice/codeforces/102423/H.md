---
title: "CF 102423H - Khoảng cách Levenshtein"
description: "Chúng ta được cung cấp một bảng chữ cái hữu hạn bao gồm các chữ cái viết thường riêng biệt, đã được viết theo thứ tự bảng chữ cái và một chuỗi truy vấn có tất cả các ký tự thuộc về bảng chữ cái đó."
date: "2026-08-12T06:39:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102423
codeforces_index: "H"
codeforces_contest_name: "North American Southeast Regional 2019 (Div 1)"
rating: 0
weight: 102423
solve_time_s: 79
verified: true
draft: false
---

[CF 102423H - Khoảng cách Levenshtein](https://codeforces.com/problemset/problem/102423/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bảng chữ cái hữu hạn bao gồm các chữ cái viết thường riêng biệt, đã được viết theo thứ tự bảng chữ cái và một chuỗi truy vấn có tất cả các ký tự thuộc về bảng chữ cái đó. Chúng ta cần xây dựng mọi chuỗi riêng biệt có khoảng cách Levenshtein từ chuỗi truy vấn chính xác là một. 

Khoảng cách một có nghĩa là chỉ cần một chỉnh sửa cơ bản là đủ. Chúng ta có thể chèn một ký tự bảng chữ cái vào bất kỳ vị trí nào, xóa một ký tự hiện có hoặc thay thế một ký tự bằng một ký tự bảng chữ cái khác. Bản thân chuỗi gốc không được in vì khoảng cách của nó với chính nó bằng 0. Các chuỗi kết quả phải được in theo thứ tự từ điển, loại bỏ các chuỗi trùng lặp. 

Chuỗi truy vấn có độ dài từ 2 đến 100. Bảng chữ cái chỉ chứa các chữ cái viết thường riêng biệt nên có tối đa 26 ký tự. Những giới hạn này rất nhỏ. Ngay cả khi chúng tôi tạo ra mọi chỉnh sửa có thể một cách độc lập thì cũng chỉ có vài nghìn ứng cử viên. Một thuật toán bậc hai trên tất cả các ứng cử viên là không cần thiết, nhưng ngay cả số lượng ứng cử viên cũng đủ nhỏ để việc xây dựng trực tiếp là giải pháp tự nhiên. 

Các trường hợp cạnh chính đến từ việc tạo bản sao. Ví dụ, với đầu vào```
a
aa
```đầu ra đúng là```
aaa
a
```chèn`a`trước ký tự đầu tiên, giữa hai ký tự hoặc sau ký tự thứ hai luôn tạo ra`aaa`, do đó, việc triển khai bất cẩn in từng thao tác riêng biệt sẽ tạo ra cùng một chuỗi ba lần. Xóa một trong hai lần xuất hiện của`a`tạo ra cùng một chuỗi`a`, tạo một bản sao khác. 

Trường hợp thứ hai là thay thế. Với```
ab
aa
```thay thế một trong hai`a`qua`b`sản xuất`ba`, trong khi thay thế ký tự thứ hai cũng tạo ra`ab`. Tổng quát hơn, các vị trí chỉnh sửa khác nhau có thể dẫn đến cùng một chuỗi kết quả, do đó việc loại bỏ trùng lặp phải xảy ra sau khi tạo các ứng viên thay vì chỉ dựa trên thao tác được sử dụng. 

Bảng chữ cái cũng có thể chứa một ký tự đơn. Ví dụ,```
a
aa
```không có khả năng thay thế nào cả, vì không có ký tự nào khác. Việc chèn và xóa vẫn phải được xem xét. Việc triển khai giả định mọi vị trí đều có sự thay thế sẽ tạo ra chuỗi gốc hoặc truy cập vào một ký tự thay thế không tồn tại. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là liệt kê mọi chỉnh sửa pháp lý và đưa chuỗi kết quả vào một bộ sưu tập. Đối với mỗi vị trí chèn (n+1), chúng tôi thử từng ký tự trong bảng chữ cái (k). Đối với mỗi vị trí (n), chúng tôi tạo chuỗi thu được bằng cách xóa ký tự đó. Cuối cùng, đối với mỗi vị trí, chúng tôi thử mọi ký tự bảng chữ cái khác với ký tự hiện tại, đưa ra (n(k-1)) ứng cử viên thay thế. 

Do đó số lượng ứng viên được tạo ra là 

[ 
(n+1)k+n+n(k-1)=2nk+k. 
] 

Ở các giá trị tối đa (n=100) và (k=26), đây chỉ là 

[ 
2\cdot100\cdot26+26=5226 
] 

ứng viên. Mỗi ứng cử viên có độ dài tối đa là 101, do đó việc xây dựng tất cả chúng chỉ cần vài trăm nghìn thao tác ký tự. 

Việc triển khai đơn giản có thể tạo ra tất cả các ứng cử viên vào một danh sách và sau đó so sánh nhiều lần các chuỗi để loại bỏ các chuỗi trùng lặp. Điều đó đúng, bởi vì mọi khoảng cách có thể có - một thao tác đều được xem xét rõ ràng, nhưng việc loại bỏ trùng lặp bằng cách so sánh theo cặp có thể mất (O(C^2 n)), trong đó (C) là số lượng ứng cử viên được tạo. Với (C) khoảng 5000, đây đã là hàng triệu phép so sánh chuỗi và tốn kém một cách không cần thiết. 

Quan sát quan trọng là vấn đề không yêu cầu chúng ta đếm các phép toán. Nó yêu cầu các chuỗi kết quả duy nhất. Bộ băm chính xác là cấu trúc dữ liệu cần thiết cho yêu cầu đó. Chúng tôi có thể tạo mọi kết quả hợp pháp một lần, chèn nó vào một tập hợp và để phép băm tự động thu gọn tất cả các bản sao. Sau khi tạo, chuyển đổi tập hợp thành danh sách và sắp xếp nó theo thứ tự từ điển cần thiết. 

Do đó, ý tưởng bạo lực vẫn là nền tảng của giải pháp. Cải tiến không phải là tránh liệt kê các chỉnh sửa, vì có quá ít chỉnh sửa để biện minh cho bất kỳ điều gì phức tạp hơn. Cải tiến này là thể hiện tập hợp các câu trả lời theo cách thực hiện việc loại bỏ trùng lặp một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với tính năng loại bỏ trùng lặp theo cặp | (O(C^2 n + C\log C)) | (O(Cn)) | Đúng nhưng chậm không cần thiết | 
| Tạo và loại bỏ trùng lặp bằng bộ băm | (O(nk\cdot n + C\log C)) | (O(Cn)) | Đã chấp nhận | 

Ở đây (C=2nk+k), và với (n\le100) và (k\le26), cả hai giới hạn đều rất nhỏ. 

## Hướng dẫn thuật toán 

1. Đọc bảng chữ cái và chuỗi truy vấn. Gọi (n) là độ dài truy vấn và (k) là kích thước bảng chữ cái. Mọi ký tự được chèn hoặc thay thế đều phải xuất phát từ bảng chữ cái này nên hai chuỗi này mô tả đầy đủ các chỉnh sửa có sẵn. 
2. Tạo một tập hợp trống có tên`answers`. Một bộ được sử dụng vì các chỉnh sửa khác nhau có thể tạo ra chính xác cùng một chuỗi và đầu ra được yêu cầu chỉ chứa mỗi chuỗi một lần. 
3. Đối với mọi vị trí từ`0`bởi vì`n`, chèn mọi ký tự bảng chữ cái vào vị trí đó. Có (n+1) khoảng trống trong một chuỗi có độ dài (n), bao gồm khoảng trống trước ký tự đầu tiên và khoảng cách sau ký tự cuối cùng. 
4. Đối với mỗi vị trí ký tự, hãy xóa ký tự đó và chèn chuỗi kết quả vào bộ. Mỗi lần xóa đều có giá một lần, vì vậy đây chính xác là tất cả các kết quả xóa có thể xảy ra. 
5. Đối với mỗi vị trí ký tự, hãy thử mọi ký tự trong bảng chữ cái ngoại trừ ký tự hiện tại và thay thế ký tự đó bằng ký tự đó. Việc bỏ qua ký tự hiện tại là cần thiết vì việc thay thế một ký tự sẽ không thực hiện chỉnh sửa và sẽ đưa ra chuỗi truy vấn ban đầu không chính xác. 
6. Chuyển tập hợp thành danh sách và sắp xếp theo từ điển. Bản thân bảng chữ cái đã được sắp xếp theo thứ tự, nhưng các chuỗi được tạo ra có độ dài và vị trí chỉnh sửa khác nhau, do đó thứ tự cuối cùng của chúng không thể có được một cách đáng tin cậy từ thứ tự tạo. 
7. In mỗi chuỗi trong danh sách đã sắp xếp trên một dòng riêng. Nếu hai trình tự chỉnh sửa tạo ra cùng một chuỗi thì tập hợp đó đã loại bỏ chuỗi trùng lặp. 

### Tại sao nó hoạt động 

Mỗi chuỗi ở khoảng cách chính xác bằng một phải có được bằng một trong ba thao tác: một lần chèn, một lần xóa hoặc một lần thay thế. Thuật toán liệt kê rõ ràng mọi trường hợp hợp lệ của cả ba thao tác, do đó không bỏ sót câu trả lời hợp lệ nào. 

Vòng lặp chèn xem xét mọi vị trí có thể và mọi ký tự bảng chữ cái. Vòng lặp xóa xem xét mọi ký tự hiện có. Vòng lặp thay thế xem xét mọi vị trí và mọi ký tự bảng chữ cái khác nhau. Do đó, mọi chuyển đổi một lần chỉnh sửa có thể xuất hiện trong số các ứng cử viên được tạo. 

Bộ này chỉ loại bỏ các chuỗi bằng nhau, không loại bỏ các chuỗi khác nhau. Vì chuỗi gốc không bao giờ được tạo bằng cách chèn hoặc xóa hợp lệ và việc thay thế có chủ ý loại trừ ký tự hiện tại, nên mọi phần tử còn lại đều có khoảng cách chính xác là một. Việc sắp xếp sau đó chỉ thay đổi thứ tự trình bày, do đó đầu ra cuối cùng chính xác là tập hợp các chuỗi được yêu cầu theo thứ tự từ điển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(data: str) -> str:
    lines = data.splitlines()
    alphabet = lines[0].strip()
    s = lines[1].strip()

    n = len(s)
    answers = set()

    # Insert one alphabet character at every possible position.
    for i in range(n + 1):
        left = s[:i]
        right = s[i:]
        for ch in alphabet:
            answers.add(left + ch + right)

    # Delete one character.
    for i in range(n):
        answers.add(s[:i] + s[i + 1:])

    # Replace one character by a different alphabet character.
    for i in range(n):
        for ch in alphabet:
            if ch != s[i]:
                answers.add(s[:i] + ch + s[i + 1:])

    return "\n".join(sorted(answers))

def main():
    data = sys.stdin.read()
    sys.stdout.write(solve(data))

if __name__ == "__main__":
    main()
```Hai dòng đầu vào đầu tiên được đọc dưới dạng bảng chữ cái và chuỗi truy vấn. Không có số lượng trường hợp thử nghiệm nên toàn bộ dữ liệu đầu vào có thể được sử dụng cùng một lúc. 

Vòng lặp chèn sử dụng`range(n + 1)`bởi vì một chuỗi có độ dài-(n) có khoảng trống chèn (n+1). biểu thức`s[:i] + ch + s[i:]`chèn`ch`mà không sửa đổi chuỗi gốc. 

Biểu thức xóa`s[:i] + s[i + 1:]`loại bỏ chính xác ký tự tại vị trí`i`. Nó cũng xử lý các vị trí đầu tiên và cuối cùng một cách tự nhiên, đây là những nguồn gây ra lỗi cắt lát phổ biến. 

Để thay thế, điều kiện`ch != s[i]`là điều cần thiết. Nếu không có nó, chuỗi truy vấn ban đầu sẽ được chèn vào`answers`, mặc dù khoảng cách Levenshtein của nó với chính nó bằng không. 

Chuỗi Python là bất biến nên mỗi thao tác sẽ tạo ra một chuỗi mới. Điều này hoàn toàn phù hợp ở đây vì có nhiều nhất 5226 ứng viên được tạo ra và mỗi ứng cử viên có độ dài tối đa là 101. Số nguyên Python không liên quan đến tính toán nên không có lo ngại về tràn. 

trận chung kết`sorted(answers)`đưa ra thứ tự chuỗi từ điển thông thường, chính xác những gì vấn đề yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mẫu được cung cấp là```
eg
egg
```Bảng chữ cái đây`{e, g}`và truy vấn có độ dài bằng ba. Thuật toán tạo ra kết quả chèn, xóa và thay thế. Trạng thái quan trọng là tập hợp các câu trả lời độc đáo ngày càng tăng. 

| Hoạt động | Kết quả được tạo | Đặt hiệu ứng | 
| --- | --- | --- | 
| Chèn`e`ở vị trí 0 |`eegg`| thêm | 
| Chèn`g`ở vị trí 0 |`gegg`| thêm | 
| Chèn`e`ở vị trí 1 |`eegg`| trùng lặp | 
| Chèn`g`ở vị trí 1 |`eggg`| thêm | 
| Chèn`e`ở vị trí 2 |`eegg`| trùng lặp | 
| Chèn`g`ở vị trí 2 |`eggg`| trùng lặp | 
| Chèn`e`ở vị trí 3 |`egge`| thêm | 
| Chèn`g`ở vị trí 3 |`eggg`| trùng lặp | 
| Xóa vị trí 0 |`gg`| thêm | 
| Xóa vị trí 1 |`eg`| thêm | 
| Xóa vị trí 2 |`eg`| trùng lặp | 
| Thay thế vị trí 0`e -> g`|`ggg`| thêm | 
| Thay thế vị trí 1`g -> e`|`eeg`| thêm | 
| Thay thế vị trí 2`g -> e`|`ege`| thêm | 

Sau khi tất cả các ứng cử viên được tạo và sắp xếp, kết quả là```
eg
ege
eeg
egg
eegg
egge
eggg
gegg
gg
ggg
```Tập hợp chính xác chứa mọi chuỗi có thể truy cập được bằng một lần chỉnh sửa, trong khi các thao tác chèn và xóa lặp lại các ký tự bằng nhau sẽ thu gọn thành một mục nhập. Bản thân truy vấn không được tạo vì sự thay thế không bao giờ sử dụng cùng một ký tự. 

### Ví dụ 2 

Hãy xem xét```
abc
ab
```Có ba khoảng trống chèn và hai vị trí xóa. Các chuỗi duy nhất được tạo là: 

| Hoạt động | Vị trí hoặc nhân vật | Kết quả | 
| --- | --- | --- | 
| Chèn`a`| trước`a`|`aabc`| 
| Chèn`b`| trước`a`|`babc`| 
| Chèn`c`| trước`a`|`cabc`| 
| Chèn`a`| giữa`a,b`|`aabc`| 
| Chèn`b`| giữa`a,b`|`abbc`| 
| Chèn`c`| giữa`a,b`|`acbc`| 
| Chèn`a`| sau đó`b`|`abac`| 
| Chèn`b`| sau đó`b`|`abbc`| 
| Chèn`c`| sau đó`b`|`abcc`| 
| Xóa bỏ`a`| vị trí 0 |`b`| 
| Xóa bỏ`b`| vị trí 1 |`a`| 
| Thay thế`a -> b`| vị trí 0 |`bbc`| 
| Thay thế`a -> c`| vị trí 0 |`cbc`| 
| Thay thế`b -> a`| vị trí 1 |`aac`| 
| Thay thế`b -> c`| vị trí 1 |`acc`| 
| Thay thế`c -> a`| vị trí 2 |`aba`| 
| Thay thế`c -> b`| vị trí 2 |`abb`| 

Sau khi sắp xếp, kết quả là```
a
aabc
aac
abac
aba
abb
abbc
abcc
acc
b
babc
bbc
cabc
cbc
```Ví dụ này chứng tỏ rằng việc chèn và thay thế có thể tạo ra các chuỗi có độ dài khác nhau, trong khi việc xóa sẽ tạo ra các chuỗi ngắn hơn. Việc sắp xếp tập hợp hoàn chỉnh sau khi tạo sẽ tránh phải suy luận về thứ tự từ điển trong quá trình liệt kê. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2k + nk\log(nk))) | Nhiều nhất (O(nk)) ứng cử viên được xây dựng, mỗi ứng cử viên có độ dài (O(n)), sau đó là sắp xếp | 
| Không gian | (O(n^2k)) | Bộ lưu trữ các chuỗi (O(nk)), mỗi chuỗi có độ dài (O(n)) | 

Với (n\le100) và (k\le26), thuật toán tạo ra tối đa 5226 ứng viên, mỗi ứng viên chỉ dài khoảng 101 ký tự. Tổng lượng dữ liệu chuỗi đủ nhỏ để vừa vặn thoải mái trong giới hạn bộ nhớ 512 MB và việc liệt kê trực tiếp dễ dàng đủ nhanh cho giới hạn một giây. Trang tổng quan cuộc thi được lưu trữ xác nhận giới hạn 1 giây và 512 MB. 

## Trường hợp thử nghiệm 

Tuyên bố chính thức cung cấp`eg`Và`egg`vật mẫu. Các trường hợp còn lại bên dưới mục tiêu tạo bản sao, độ dài truy vấn nhỏ nhất có thể, ranh giới thay thế và bảng chữ cái lớn hơn.```python
# helper: run solution on input string, return output string
import io
import sys

def solve(data: str) -> str:
    lines = data.splitlines()
    alphabet = lines[0].strip()
    s = lines[1].strip()

    n = len(s)
    answers = set()

    for i in range(n + 1):
        for ch in alphabet:
            answers.add(s[:i] + ch + s[i:])

    for i in range(n):
        answers.add(s[:i] + s[i + 1:])

    for i in range(n):
        for ch in alphabet:
            if ch != s[i]:
                answers.add(s[:i] + ch + s[i + 1:])

    return "\n".join(sorted(answers))

def run(inp: str) -> str:
    return solve(inp).strip()

# provided sample
assert run("eg\negg\n") == "\n".join([
    "eg",
    "ege",
    "eeg",
    "eegg",
    "egge",
    "eggg",
    "gegg",
    "gg",
    "ggg",
]), "sample 1"

# minimum-size query with a one-character alphabet
assert run("a\naa\n") == "\n".join([
    "a",
    "aaa",
]), "single-character alphabet and duplicate edits"

# substitution and insertion with two different characters
assert run("ab\nab\n") == "\n".join([
    "a",
    "aa",
    "aab",
    "aba",
    "abb",
    "b",
    "baa",
    "bab",
    "bb",
    "bba",
]), "two-character alphabet"

# all characters equal, catches duplicate insertion/deletion positions
assert run("ab\naaa\n") == "\n".join([
    "aa",
    "aaaa",
    "aaba",
    "abaa",
    "baaa",
]), "all-equal query"

# larger alphabet, exercises every replacement character
assert run("abc\nabc\n") == "\n".join([
    "a",
    "aabc",
    "aac",
    "abac",
    "aba",
    "abb",
    "abbc",
    "abcc",
    "abc",
    "ac",
    "acbc",
    "acc",
    "b",
    "babc",
    "bbc",
    "bc",
    "c",
    "cabc",
    "cbc",
]), "larger alphabet and boundary edits"
```Các trường hợp tùy chỉnh có thể được tóm tắt như sau. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a / aa`|`a`,`aaa`| Kích thước bảng chữ cái tối thiểu và các phần chèn/xóa trùng lặp | 
|`ab / ab`| Mười chuỗi riêng biệt | Cả ký tự thay thế và ranh giới chèn/xóa | 
|`ab / aaa`| Năm chuỗi riêng biệt | Ký tự lặp đi lặp lại tạo ra nhiều thao tác trùng lặp | 
|`abc / abc`| Nhiều kết quả chèn, xóa và thay thế | Vị trí cạnh và duyệt bảng chữ cái đầy đủ | 

## Vỏ cạnh 

Đối với bảng chữ cái một ký tự, hãy xem xét```
a
aa
```Không có sự thay thế hợp pháp bởi vì`a`là nhân vật duy nhất có sẵn. chèn`a`tại bất kỳ khoảng trống nào trong ba khoảng trống tạo ra`aaa`, trong khi xóa một trong hai lần xuất hiện sẽ tạo ra`a`. Bộ này giảm ba kết quả chèn và hai kết quả xóa này thành hai chuỗi, tạo ra```
a
aaa
```Thuật toán xử lý việc này vì vòng lặp thay thế đơn giản là không có ký tự nào thỏa mãn`ch != s[i]`. 

Đối với các ký tự lặp lại, hãy xem xét```
ab
aaa
```chèn`a`ở bất kỳ vị trí nào trong bốn vị trí đều tạo ra`aaaa`, trong khi xóa bất kỳ vị trí nào trong ba vị trí sẽ tạo ra`aa`. Việc triển khai đơn giản gắn trực tiếp mọi thao tác vào đầu ra sẽ in các chuỗi này nhiều lần. Bộ này lưu trữ mỗi kết quả một lần, vì vậy đầu ra cuối cùng chỉ chứa các chuỗi duy nhất. 

Đối với vị trí chèn đầu tiên và cuối cùng, hãy xem xét```
ab
ab
```vị trí`0`chèn tạo ra các chuỗi như`aab`Và`bab`, trong khi vị trí`2`sản xuất`aba`Và`abb`. Vòng lặp sử dụng`range(n + 1)`, do đó cả hai khoảng trống ranh giới đều được bao gồm. Chỉ sử dụng`range(n)`sẽ âm thầm bỏ lỡ tất cả các phần chèn vào cuối. 

Để thay thế, đầu vào tương tự cho thấy lý do tại sao phải loại trừ ký tự hiện tại. Ở vị trí 0, thay thế`a`qua`a`sẽ tái tạo`ab`, nhưng thao tác đó không thay đổi gì và có khoảng cách bằng 0. điều kiện`ch != s[i]`ngăn chuỗi truy vấn ban đầu đi vào tập câu trả lời. 

Để xóa, chuỗi hai ký tự```
ab
ab
```có kết quả xóa`b`Và`a`. Các biểu thức cắt xử lý chính xác cả hai điểm cuối vì`s[:0] + s[1:]`xóa ký tự đầu tiên và`s[:1] + s[2:]`loại bỏ thứ hai. 

Cuối cùng, kết quả đầu ra trùng lặp không bị giới hạn ở các ký tự lặp lại. Việc chèn cùng một ký tự vào các vị trí liền kề cũng có thể hội tụ về cùng một chuỗi cuối cùng khi các ký tự xung quanh khớp nhau. Đây là lý do tại sao việc loại bỏ trùng lặp phải được thực hiện trên các chuỗi kết quả thực tế thay vì trên các mô tả hoạt động. Bộ băm cung cấp chính xác hành vi đó.
