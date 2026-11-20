---
title: "CF 2132A - Bài tập về nhà"
description: "Chúng ta bắt đầu bằng chuỗi a. Một chuỗi b khác chứa các ký tự phải được chèn lần lượt theo thứ tự chúng xuất hiện. Đối với mỗi vị trí i trong b, ký tự tương ứng c[i] cho chúng ta biết ai chèn ký tự đó."
date: "2026-06-08T02:49:34+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 2132
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1043 (Div. 3)"
rating: 800
weight: 2132
solve_time_s: 103
verified: true
draft: false
---

[CF 2132A - Bài tập về nhà](https://codeforces.com/problemset/problem/2132/A) 

**Đánh giá:** 800 
**Tags:** vũ lực, thực hiện, dây 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một chuỗi`a`. Một chuỗi khác`b`chứa các ký tự phải được chèn từng ký tự một, theo thứ tự chúng xuất hiện. 

Đối với mọi vị trí`i`TRONG`b`, ký tự tương ứng`c[i]`cho chúng tôi biết ai chèn ký tự đó. Nếu như`c[i] = 'V'`, Vlad chèn`b[i]`ở đầu chuỗi hiện tại. Nếu như`c[i] = 'D'`, Dima chèn`b[i]`ở cuối. 

Sau khi xử lý tất cả các ký tự của`b`, chúng ta phải xuất chuỗi cuối cùng. 

Những hạn chế là cực kỳ nhỏ. Cả hai`a`Và`b`có độ dài tối đa là 10 và có tối đa 1000 trường hợp thử nghiệm. Ngay cả một mô phỏng trực tiếp sửa đổi chuỗi về mặt vật lý sau mỗi thao tác cũng chỉ thực hiện vài chục thao tác ký tự cho mỗi trường hợp thử nghiệm. Hiệu quả không phải là một mối quan tâm ở đây. Nhiệm vụ hoàn toàn là thực hiện đúng quy trình được mô tả. 

Nguyên nhân chính của sai sót là việc xử lý thứ tự chèn. 

Coi như:```
a = "x"
b = "ab"
c = "VV"
```Các hoạt động là:```
x
ax
bax
```Câu trả lời là:```
bax
```Một giải pháp bất cẩn có thể thu thập tất cả các ký tự được gán cho Vlad và thêm chúng theo thứ tự ban đầu, tạo ra`"abx"`, điều đó không đúng. Mỗi nhân vật Vlad mới đều được đặt trước mọi thứ đã tồn tại. 

Một lỗi dễ mắc phải khác là xử lý tất cả các hoạt động của Vlad trước và tất cả các hoạt động của Dima sau. 

Ví dụ:```
a = "ot"
b = "ad"
c = "DV"
```Đúng quy trình:```
ot
ota
dota
```Trả lời:```
dota
```Nếu chúng ta nhóm các hoạt động theo từng người thay vì tôn trọng thứ tự thời gian của họ, chúng ta sẽ nhận được một kết quả khác. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng chính xác những gì tuyên bố mô tả. 

Chúng tôi duy trì chuỗi hiện tại. Đối với mỗi vị trí`i`, nếu như`c[i]`là`'V'`, chúng tôi đặt`b[i]`ở phía trước. Nếu không thì chúng tôi đặt nó ở phía sau. 

Bởi vì các dây rất nhỏ nên ngay cả việc tạo một chuỗi hoàn toàn mới sau mỗi lần chèn cũng hoàn toàn có thể chấp nhận được. Chuỗi dài nhất có thể có độ dài tối đa`n + m ≤ 20`, vì vậy mỗi lần chèn tốn tối đa 20 bản sao ký tự. 

Trong trường hợp xấu nhất, chúng tôi thực hiện tối đa 10 lần chèn, mỗi lần chèn trên một chuỗi có độ dài tối đa là 20. Đó chỉ là thao tác vài trăm ký tự cho mỗi trường hợp thử nghiệm. 

Có một quan sát rõ ràng hơn một chút. Các ký tự được chèn bởi Vlad luôn tích lũy ở phía bên trái, trong khi các ký tự được Dima chèn vào luôn tích lũy ở phía bên phải. 

Khi Vlad chèn các ký tự ở phía trước, ký tự Vlad gần đây nhất sẽ trở thành ký tự ngoài cùng bên trái. Điều đó có nghĩa là các nhân vật của Vlad xuất hiện theo thứ tự ngược lại với sự xuất hiện của họ trong`b`. 

Khi Dima chèn các ký tự vào cuối, thứ tự của chúng không thay đổi. 

Vì vậy, chúng ta có thể xây dựng:```
left  = all b[i] where c[i] = 'V'
right = all b[i] where c[i] = 'D'
```Vậy thì câu trả lời cuối cùng là:```
reverse(left) + a + right
```Cả hai phương pháp đều được chấp nhận. Cách thứ hai tránh sửa đổi chuỗi lặp đi lặp lại và trực tiếp xây dựng câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(m(n+m)) | O(n+m) | Đã chấp nhận | 
| Xây dựng tối ưu | O(n+m) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi`a`,`b`, Và`c`. 
2. Tạo hai chuỗi (hoặc danh sách) trống`left`Và`right`. 
3. Quét tất cả các vị trí của`b`. 
4. Nếu`c[i]`là`'V'`, nối thêm`b[i]`ĐẾN`left`. 

Hiện tại chúng tôi chỉ ghi lại nhân vật. Vị trí cuối cùng của nó sẽ được xác định sau. 
5. Nếu`c[i]`là`'D'`, nối thêm`b[i]`ĐẾN`right`. 

Các nhân vật của Dima giữ nguyên thứ tự ban đầu vì mỗi nhân vật đều được thêm vào cuối. 
6. Đảo ngược`left`. 

Vlad liên tục chèn vào đầu chuỗi, do đó ký tự Vlad cuối cùng được xử lý sẽ trở thành ký tự ngoài cùng bên trái trong chuỗi cuối cùng. 
7. Đầu ra:```
reverse(left) + a + right
```### Tại sao nó hoạt động 

Mỗi thao tác Vlad sẽ chèn một ký tự trước toàn bộ chuỗi hiện tại. Giả sử Vlad nhận được ký tự:```
x1, x2, x3
```theo thứ tự đó. 

Tác dụng của chúng là:```
x1 + ...
x2 + x1 + ...
x3 + x2 + x1 + ...
```Vì vậy các nhân vật của Vlad xuất hiện theo thứ tự ngược lại. 

Các hoạt động của Dima luôn được nối vào cuối:```
... + y1
... + y1 + y2
... + y1 + y2 + y3
```Vì vậy, các nhân vật của Dima xuất hiện theo thứ tự ban đầu. 

Không gì có thể di chuyển nhân vật từ một phía`a`đến cái khác. Cấu trúc cuối cùng chính xác là:```
(reversed Vlad characters) + a + (Dima characters)
```đó chính xác là những gì thuật toán xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    a = input().strip()

    m = int(input())
    b = input().strip()
    c = input().strip()

    left = []
    right = []

    for ch, who in zip(b, c):
        if who == 'V':
            left.append(ch)
        else:
            right.append(ch)

    ans = ''.join(reversed(left)) + a + ''.join(right)
    print(ans)
```Giải pháp phân tách các ký tự theo người chèn chúng. 

Các ký tự được gán cho Vlad được lưu trữ trong`left`. Chúng được thu thập theo thứ tự như chúng xuất hiện trong`b`, nhưng thứ tự cuối cùng của chúng phải bị đảo ngược vì mọi thao tác chèn Vlad mới đều diễn ra ở phía trước. 

Các ký tự được gán cho Dima được lưu trữ trong`right`. Thứ tự của chúng không thay đổi vì việc thêm vào cuối sẽ giữ nguyên niên đại. 

Cuối cùng, câu trả lời được tập hợp từ ba phần: đoạn Vlad đảo ngược, chuỗi gốc`a`và đoạn Dima. 

Sử dụng danh sách và`''.join()`là cách thực hành tiêu chuẩn trong Python và tránh việc tạo chuỗi trung gian không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = "ot"
b = "ad"
c = "DV"
```| Bước | Nhân vật | Chủ sở hữu | trái | đúng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | "" | "" | 
| 1 | một | D | "" | "một" | 
| 2 | d | V | "d" | "một" | 

Xây dựng cuối cùng:```
reverse("d") + "ot" + "a"
= "dota"
```Đầu ra:```
dota
```Ví dụ này cho thấy thứ tự hoạt động rất quan trọng. Dima hành động trước, sau đó Vlad chèn vào phía trước toàn bộ chuỗi hiện tại. 

### Ví dụ 2 

đầu vào:```
a = "biz"
b = "abon"
c = "VVDD"
```| Bước | Nhân vật | Chủ sở hữu | trái | đúng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | "" | "" | 
| 1 | một | V | "một" | "" | 
| 2 | b | V | "ab" | "" | 
| 3 | o | D | "ab" | "Ồ" | 
| 4 | n | D | "ab" | "bật" | 

Xây dựng cuối cùng:```
reverse("ab") + "biz" + "on"
= "ba" + "biz" + "on"
= "babizon"
```Đầu ra:```
babizon
```Dấu vết này chứng tỏ tại sao các nhân vật của Vlad phải bị đảo ngược trong khi của Dima thì không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Một lần quét qua các chuỗi và tối đa một lần đảo ngược`m`nhân vật | 
| Không gian | O(n + m) | Lưu trữ câu trả lời và danh sách ký tự tạm thời | 

Từ`n`Và`m`cả hai đều nhiều nhất là 10, thời gian chạy rất nhỏ. Ngay cả trên 1000 trường hợp thử nghiệm, tổng công việc vẫn không đáng kể so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = input().strip()

        m = int(input())
        b = input().strip()
        c = input().strip()

        left = []
        right = []

        for ch, who in zip(b, c):
            if who == "V":
                left.append(ch)
            else:
                right.append(ch)

        out.append("".join(reversed(left)) + a + "".join(right))

    return "\n".join(out)

# provided sample
assert run(
"""4
2
ot
2
ad
DV
3
efo
7
rdcoecs
DVDVDVD
3
aca
4
bbaa
DVDV
3
biz
4
abon
VVDD
"""
) == "\n".join([
    "dota",
    "codeforces",
    "abacaba",
    "babizon"
]), "sample"

# minimum sizes
assert run(
"""1
1
a
1
b
V
"""
) == "ba"

# all Dima
assert run(
"""1
1
x
4
abcd
DDDD
"""
) == "xabcd"

# all Vlad
assert run(
"""1
1
x
4
abcd
VVVV
"""
) == "dcbax"

# mixed boundary case
assert run(
"""1
1
m
5
abcde
VDDVD
"""
) == "eambcd"
```### Tóm tắt thử nghiệm tùy chỉnh 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`,`b`,`V`|`ba`| Trường hợp hợp lệ nhỏ nhất | 
| Tất cả`D`hoạt động |`xabcd`| Phần chèn bên phải giữ trật tự | 
| Tất cả`V`hoạt động |`dcbax`| Thứ tự chèn bên trái ngược lại | 
| Hỗn hợp`VDDVD`trường hợp |`eambcd`| Kết hợp cả hai hành vi | 

## Vỏ cạnh 

Hãy xem xét trường hợp mọi ký tự được chèn đều thuộc về Vlad. 

đầu vào:```
1
1
x
4
abcd
VVVV
```Trình tự xử lý:```
x
ax
bax
cbax
dcbax
```Thuật toán lưu trữ:```
left = "abcd"
right = ""
```Sau khi đảo ngược`left`:```
"dcba"
```Câu trả lời cuối cùng:```
dcbax
```phù hợp với mô phỏng trực tiếp. 

Bây giờ hãy xem xét trường hợp ngược lại. 

đầu vào:```
1
1
x
4
abcd
DDDD
```Mỗi ký tự được nối thêm:```
x
xa
xab
xabc
xabcd
```Thuật toán xây dựng:```
left = ""
right = "abcd"
```và trả về:```
xabcd
```không có sự đảo ngược nào ảnh hưởng đến các nhân vật của Dima. 

Cuối cùng, hãy xem xét các hoạt động xen kẽ. 

đầu vào:```
1
1
z
3
abc
VDV
```Mô phỏng:```
z
az
azb
cazb
```Thuật toán:```
left = "ac"
right = "b"
```Kết quả:```
reverse("ac") + "z" + "b"
= "ca" + "z" + "b"
= "cazb"
```Điều này xác nhận rằng việc tách các ký tự cạnh nhau và chỉ đảo ngược phần của Vlad sẽ tạo ra hiệu ứng chính xác của việc chèn theo trình tự thời gian.
