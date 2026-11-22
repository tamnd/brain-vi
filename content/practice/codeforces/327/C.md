---
title: "CF 327C - Năm Phép Thuật"
description: "Chúng ta được cho một chuỗi chữ số a và một số nguyên k. Tấm thực tế không phải là chính nó mà là chuỗi thu được bằng cách nối a với chính nó k lần. Từ chuỗi dài này, chúng ta có thể xóa bất kỳ tập hợp con vị trí nào, ngoại trừ việc chúng ta không được phép xóa từng chữ số."
date: "2026-06-06T09:00:50+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math"]
categories: ["algorithms"]
codeforces_contest: 327
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 191 (Div. 2)"
rating: 1700
weight: 327
solve_time_s: 97
verified: true
draft: false
---

[CF 327C - Năm phép thuật](https://codeforces.com/problemset/problem/327/C) 

**Đánh giá:** 1700 
**Tags:** tổ hợp, toán học 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi chữ số`a`và một số nguyên`k`. Tấm thực tế không`a`chính nó, nhưng chuỗi thu được bằng cách nối`a`với chính nó`k`lần. 

Từ chuỗi dài này, chúng ta có thể xóa bất kỳ tập hợp con vị trí nào, ngoại trừ việc chúng ta không được phép xóa từng chữ số. Các chữ số còn lại giữ nguyên thứ tự tương đối ban đầu và tạo thành một số, có thể có số 0 đứng đầu. 

Chúng ta phải đếm xem có bao nhiêu lựa chọn xóa khác nhau tạo ra một số chia hết cho`5`. Hai lựa chọn được coi là khác nhau nếu chúng xóa các nhóm vị trí khác nhau. 

Chiều dài của`a`nhiều nhất là`10^5`, nhưng chuỗi cuối cùng có độ dài`|a| · k`, Và`k`có thể lớn như`10^9`. Xây dựng chuỗi đầy đủ là không thể. Trong trường hợp xấu nhất nó sẽ chứa`10^14`chữ số. Bất kỳ thuật toán nào chạm vào từng chữ số của chuỗi mở rộng sẽ bị loại trừ ngay lập tức. 

Câu trả lời là bắt buộc theo modulo`10^9+7`, đây là một gợi ý rõ ràng rằng số lượng bộ xóa hợp lệ tăng theo cấp số nhân và số học mô-đun sẽ được tham gia trong suốt giải pháp. 

Các trường hợp cạnh chính xuất phát từ thực tế là chúng ta đang đếm các chuỗi con chứ không phải các chuỗi con. 

Coi như:```
a = "1256"
k = 1
```Các số hợp lệ là`5`,`15`,`25`, Và`125`. Trình tự tiếp theo`5`vẫn hợp lệ ngay cả khi tất cả các chữ số trước khi nó bị xóa. Việc giải thích dựa trên chuỗi con sẽ bỏ lỡ một số câu trả lời. 

Một trường hợp tế nhị khác là:```
a = "0"
k = 1
```Dãy con duy nhất không trống là`"0"`, chia hết cho`5`, vậy câu trả lời là`1`. Một giải pháp bất cẩn có thể loại trừ các số 0 đứng đầu và trả về sai`0`. 

Một ví dụ thú vị hơn là:```
a = "5"
k = 2
```Chuỗi mở rộng là`"55"`. 

Các dãy con hợp lệ là:```
first 5
second 5
55
```Câu trả lời là`3`. Chỉ đếm các vị trí chứa`5`sẽ cho`2`, quên rằng có thể giữ nhiều chữ số. 

Cuối cùng, khi mỗi chữ số là một trong hai`0`hoặc`5`, mọi dãy con không rỗng đều chia hết cho`5`. 

Ví dụ:```
a = "55"
k = 1
```Các dãy con hợp lệ là:```
first 5
second 5
55
```cho đi`2² - 1 = 3`. Bất kỳ giải pháp nào cũng phải đếm chính xác tất cả các lựa chọn có thể có của các chữ số trước đó. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Xây dựng chuỗi đầy đủ`s`, liệt kê mọi tập hợp con không trống của các vị trí, xây dựng dãy con tương ứng và kiểm tra xem giá trị của nó có chia hết cho không`5`. 

Một số có thể chia hết cho`5`chính xác khi nào chữ số cuối cùng của nó là`0`hoặc`5`, do đó việc kiểm tra tính chia hết là dễ dàng. Vấn đề là số lượng các chuỗi con. Một chuỗi có độ dài`n`có`2^n - 1`dãy con không trống. Ngay cả đối với`n = 60`, điều này đã vượt quá`10^18`. Vấn đề thực tế có thể đạt tới độ dài khoảng`10^14`, nên sức mạnh vũ phu là vô vọng. 

Quan sát làm thay đổi mọi thứ đó là sự chia hết cho`5`chỉ phụ thuộc vào chữ số cuối cùng. 

Giả sử chúng ta quyết định vị trí đó`i`của chuỗi cuối cùng là chữ số được giữ cuối cùng. Để số kết quả chia hết cho`5`, chữ số`i`phải là một trong hai`0`hoặc`5`. 

Một lần vị trí`i`được chọn làm chữ số cuối cùng, mọi vị trí sau`i`phải bị xóa. Mỗi vị trí trước`i`có thể được giữ lại hoặc xóa một cách độc lập. 

Nếu có`i`vị trí trước nó, có chính xác`2^i`cách để chọn cái nào trong số chúng còn lại. 

Điều này có nghĩa là mỗi lần xuất hiện của`0`hoặc`5`đóng góp sức mạnh của hai. 

Nếu các vị trí được đánh số từ`0`, sự đóng góp của vị trí kết thúc hợp lệ là:```
2^position
```Vì vậy, câu trả lời cho một chuỗi`s`là```
Σ 2^i
```trên tất cả các vị trí`i`chữ số của ai là`0`hoặc`5`. 

Bây giờ chúng ta khai thác cấu trúc lặp lại. 

Cho phép`m = |a|`. 

Chức vụ`j`bên trong một bản sao của`a`xuất hiện trên toàn cầu tại các vị trí```
j,
j + m,
j + 2m,
...
j + (k-1)m
```Nếu như`a[j]`cũng không`0`cũng không`5`, nó không đóng góp gì cả. 

Nếu không thì tổng đóng góp của nó là```
2^j (1 + 2^m + 2^(2m) + ... + 2^((k-1)m))
```Yếu tố thứ hai là một chuỗi hình học. 

Cho phép```
r = 2^m
```Sau đó câu trả lời trở thành```
(Σ valid positions j of 2^j)
×
(1 + r + r² + ... + r^(k-1))
```Mọi số học đều là modulo`10^9+7`. 

Tổng hình học có thể được tính là```
(r^k - 1) / (r - 1)
```modulo`MOD`, sử dụng nghịch đảo mô đun. 

### So sánh phương pháp tiếp cận 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu | O( | a | + log k) | 

## Hướng dẫn thuật toán 

1. Đọc`a`Và`k`. 
2. Hãy để`m = len(a)`. 
3. Đi ngang`a`từ trái sang phải và tính```
base = Σ 2^j
```trên tất cả các vị trí`j`chữ số ở đâu`0`hoặc`5`. 

Mỗi vị trí như vậy đại diện cho tất cả các chuỗi con có chữ số được giữ cuối cùng xuất phát từ vị trí đó trong một bản sao. 
4. Tính toán```
r = 2^m mod MOD
```bởi vì việc dịch chuyển toàn bộ một bản sao sẽ làm tăng số mũ lên`m`. 
5. Tính toán```
geom = 1 + r + r² + ... + r^(k-1)
```modulo`MOD`. 

Sử dụng công thức chuỗi hình học:```
geom = (r^k - 1) * inverse(r - 1)
```modulo`MOD`. 
6. Nhân:```
answer = base * geom mod MOD
```7. Xuất kết quả. 

### Tại sao nó hoạt động 

Mỗi dãy con hợp lệ đều có một vị trí được lưu giữ cuối cùng duy nhất. Vị trí cuối cùng đó phải chứa một trong hai`0`hoặc`5`, nếu không thì số kết quả không chia hết cho`5`. 

Sửa vị trí như vậy`i`. Mỗi vị trí sau`i`phải bị xóa, trong khi mỗi vị trí trước`i`có thể được giữ hoặc xóa một cách độc lập. Điều này mang lại chính xác`2^i`dãy số kết thúc tại`i`. 

Các vị trí kết thúc khác nhau tạo ra các tập hợp dãy con rời rạc, vì mỗi dãy con đều có chính xác một vị trí cuối cùng. 

Tổng hợp`2^i`trên tất cả các vị trí chứa`0`hoặc`5`đếm mọi dãy con hợp lệ chính xác một lần. 

Cấu trúc lặp lại của chuỗi cho phép nhóm các độ lệch bằng nhau trên các bản sao thành một chuỗi hình học, tạo ra công thức được thuật toán sử dụng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def solve():
    a = input().strip()
    k = int(input())

    base = 0
    p2 = 1

    for ch in a:
        if ch == '0' or ch == '5':
            base = (base + p2) % MOD
        p2 = (p2 * 2) % MOD

    m = len(a)
    r = pow(2, m, MOD)

    geom = (pow(r, k, MOD) - 1) % MOD
    geom = geom * pow(r - 1, MOD - 2, MOD) % MOD

    ans = base * geom % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```Biến`base`lưu trữ sự đóng góp từ một bản sao của`a`. Trong khi quét chuỗi,`p2`luôn bằng nhau`2^j`modulo`MOD`, Ở đâu`j`là vị trí hiện tại. 

Đối với mỗi lần xuất hiện của`0`hoặc`5`, chúng ta cộng lũy ​​thừa tương ứng của hai. Điều này thực hiện việc đếm các chuỗi con kết thúc ở vị trí đó. 

giá trị`r = 2^m`đại diện cho hệ số nhân thu được khi di chuyển một bản sao đầy đủ sang phải. Các vị trí có cùng độ lệch bên trong các bản sao khác nhau góp phần tạo nên một cấp số nhân. 

Tổng hình học được tính theo modulo`MOD`sử dụng định lý nhỏ Fermat:```
inverse(x) = x^(MOD-2)
```bởi vì`MOD`là nguyên tố. 

Một lỗi phổ biến là cố gắng tính tổng hình học bằng phép chia số nguyên thông thường. Tất cả các hoạt động phải ở dạng số học mô-đun. 

Một lỗi dễ mắc khác là quên modulo trước khi nhân với nghịch đảo module. biểu thức```
(pow(r, k, MOD) - 1) % MOD
```tránh các giá trị trung gian âm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1256
1
```Vị trí chứa`0`hoặc`5`: 

| Vị trí j | Chữ số | 2^j | căn cứ | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 
| 1 | 2 | 2 | 0 | 
| 2 | 5 | 4 | 4 | 
| 3 | 6 | 8 | 4 | 

Bây giờ: 

| Số lượng | Giá trị | 
| --- | --- | 
| m | 4 | 
| r = 2^m | 16 | 
| k | 1 | 
| địa chất | 1 | 
| trả lời | 4 | 

Đầu ra:```
4
```Bốn dãy con được đếm chính xác`5`,`15`,`25`, Và`125`. 

### Mẫu 2 

đầu vào:```
105
2
```Chuỗi mở rộng là:```
105105
```Đối với một bản sao: 

| Vị trí j | Chữ số | 2^j | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 
| 1 | 0 | 2 | 2 | 
| 2 | 5 | 4 | 4 | 

Như vậy: 

| Số lượng | Giá trị | 
| --- | --- | 
| căn cứ | 6 | 
| m | 3 | 
| r | 8 | 
| địa lý = 1 + 8 | 9 | 
| trả lời | 54 | 

Đầu ra:```
54
```Dấu vết này cho thấy sự đóng góp từ các vị trí tương ứng trong các bản sao khác nhau tạo thành một cấp số nhân như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(| a | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Chuỗi`a`chứa nhiều nhất`10^5`các ký tự, do đó việc quét tuyến tính đơn lẻ là chuyện nhỏ. Phép lũy thừa mô đun mất thời gian logarit trong`k`, vẫn còn rất nhỏ ngay cả khi`k = 10^9`. Giải pháp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 1000000007

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    a = input().strip()
    k = int(input())

    base = 0
    p2 = 1

    for ch in a:
        if ch in "05":
            base = (base + p2) % MOD
        p2 = (p2 * 2) % MOD

    r = pow(2, len(a), MOD)
    geom = (pow(r, k, MOD) - 1) % MOD
    geom = geom * pow(r - 1, MOD - 2, MOD) % MOD

    return str(base * geom % MOD)

# provided sample
assert run("1256\n1\n") == "4", "sample 1"

# custom cases
assert run("0\n1\n") == "1", "single valid digit"
assert run("1\n1\n") == "0", "no valid ending digit"
assert run("5\n2\n") == "3", "subsequences of 55"
assert run("55\n1\n") == "3", "all non-empty subsequences valid"
assert run("05\n1\n") == "3", "leading zero subsequences allowed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0, 1`|`1`| Một chữ số hợp lệ | 
|`1, 1`|`0`| Không có vị trí kết thúc hợp lệ | 
|`5, 2`|`3`| Đóng góp sao chép nhiều lần | 
|`55, 1`|`3`| Mọi dãy con không trống đều hợp lệ | 
|`05, 1`|`3`| Các số 0 đứng đầu phải được tính | 

## Vỏ cạnh 

Hãy xem xét:```
0
1
```Quét chuỗi cho`base = 1`. Yếu tố hình học là`1`, vậy câu trả lời là`1`. Tiếp theo duy nhất là`"0"`, chia hết cho`5`. Thuật toán tự nhiên bao gồm nó vì khả năng chia hết chỉ phụ thuộc vào chữ số cuối cùng. 

Coi như:```
1
1
```Không có vị trí nào chứa`0`hoặc`5`, Vì thế`base = 0`. Mọi dãy con đều kết thúc bằng chữ số`1`, do đó không có số nào chia hết cho`5`. Câu trả lời trở thành`0`. 

Coi như:```
5
2
```Chuỗi mở rộng là`"55"`. 

Vị trí đầu tiên góp phần`2^0 = 1`. Thứ hai đóng góp`2^1 = 2`. 

Tổng cộng:```
1 + 2 = 3
```tương ứng với các dãy con:```
first 5
second 5
55
```Công thức chuỗi hình học tái tạo chính xác cùng một số đếm. 

Coi như:```
05
1
```Các dãy con hợp lệ là:```
0
5
05
```Câu trả lời là`3`. 

Giải pháp loại bỏ các số 0 đứng đầu sẽ chỉ được tính sai`0`Và`5`. Vì thuật toán chỉ quan tâm đến chữ số cuối cùng nên nó bao gồm chính xác`"05"`cũng vậy.
