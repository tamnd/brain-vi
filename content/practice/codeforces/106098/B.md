---
title: "CF 106098B - Farouk và mật khẩu"
description: "Chúng ta bắt đầu bằng một chuỗi và có thể liên tục hoán đổi các ký tự giữa các vị trí i và j bất cứ khi nào $$i oplus j < min(i,j)$$ trong đó các vị trí được đánh số từ 1. Thao tác có thể được áp dụng bao nhiêu lần cũng được."
date: "2026-06-25T11:54:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106098
codeforces_index: "B"
codeforces_contest_name: "The American University in Cairo CSEA Fall 2025 contest"
rating: 0
weight: 106098
solve_time_s: 48
verified: true
draft: false
---

[CF 106098B - Farouk và Mật khẩu](https://codeforces.com/problemset/problem/106098/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu bằng một chuỗi và có thể hoán đổi nhiều lần các ký tự giữa các vị trí`i`Và`j`bất cứ khi nào$$i \oplus j < \min(i,j)$$trong đó các vị trí được đánh số từ 1. 

Thao tác này có thể được áp dụng nhiều lần. Trong số tất cả các chuỗi có thể truy cập được thông qua các hoán đổi này, chúng ta phải xuất ra chuỗi nhỏ nhất về mặt từ điển. 

Đầu vào chứa một số trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm là một chuỗi chữ thường. Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Bất kỳ giải pháp nào gần với thời gian bậc hai sẽ là quá chậm, trong khi$O(n \log n)$giải pháp dễ dàng đủ nhanh. 

Khó khăn chính là hiểu vị trí nào có thể trao đổi ký tự. Điều kiện liên quan đến XOR, do đó việc mô phỏng trực tiếp các giao dịch hoán đổi là không thực tế. 

Một sai lầm phổ biến là cho rằng chỉ những vị trí có thể hoán đổi trực tiếp mới quan trọng. Khả năng tiếp cận phụ thuộc vào việc thực hiện nhiều lần hoán đổi, vì vậy chúng tôi thực sự cần các thành phần được kết nối của biểu đồ hoán đổi. 

Hãy xem xét chuỗi:```
dog
```Vị trí 2 và 3 thỏa mãn điều kiện vì:```
2 xor 3 = 1 < 2
```Vì thế`o`Và`g`có thể được trao đổi, tạo ra:```
dgo
```Một giải pháp chỉ kiểm tra xem toàn bộ chuỗi có thể được sắp xếp hay không sẽ trả về không chính xác`dgo`cho nhiều trường hợp không liên quan. 

Một trường hợp tế nhị khác là:```
abcdefgh
```Vị trí 1 và 2 không thể kết nối được. Vị trí 1 tạo thành thành phần riêng của nó, vị trí 2 và 3 tạo thành vị trí khác và vị trí từ 4 đến 7 tạo thành thành phần khác. Mặc dù toàn bộ chuỗi đã được sắp xếp nhưng lý do vẫn không thay đổi là các ký tự không thể di chuyển qua ranh giới thành phần. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xây dựng một biểu đồ về các vị trí. Đối với mỗi cặp`(i,j)`, chúng ta kiểm tra xem điều kiện hoán đổi có đúng hay không và thêm một cạnh. Sau đó chúng ta tìm các thành phần được kết nối và sắp xếp các ký tự bên trong mỗi thành phần. 

Điều này đúng vì mọi thành phần được kết nối đều cho phép sắp xếp lại các ký tự của nó một cách tùy ý thông qua một chuỗi hoán đổi. Vấn đề là việc xây dựng đồ thị. có$O(n^2)$các cặp vị trí, trở nên đại khái$4 \cdot 10^{10}$kiểm tra khi nào$n = 2 \cdot 10^5$. Điều đó hoàn toàn không thể thực hiện được. 

Điều quan trọng nhất là điều kiện XOR có cấu trúc rất đơn giản. 

Lấy hai chỉ số với`i < j`. 

Nếu như`i`Và`j`có cùng bit được đặt cao nhất thì cả hai đều nằm trong khoảng$$[2^k,\;2^{k+1}-1].$$XOR của họ loại bỏ bit cao nhất đó, vì vậy$$i \oplus j < 2^k \le i.$$Do đó việc trao đổi được cho phép. 

Nếu các bit được thiết lập cao nhất của chúng khác nhau thì`j`có bit cao nhất lớn hơn`i`. Bit đó tồn tại trong kết quả XOR, mang lại$$i \oplus j \ge 2^m > i,$$nên điều kiện không thành công. 

Do đó, hai vị trí có thể hoán đổi trực tiếp khi và chỉ khi chúng có cùng bit quan trọng nhất. 

Điều đó có nghĩa là mỗi khoảng$$[1,1],\ [2,3],\ [4,7],\ [8,15], \ldots$$tạo thành một thành phần liên thông hoàn chỉnh và không có cạnh nào giữa các khoảng khác nhau. 

Khi đã biết cấu trúc này, chuỗi có thể tiếp cận nhỏ nhất về mặt từ điển sẽ dễ dàng được xây dựng. Đối với mọi thành phần, hãy thu thập các ký tự của nó, sắp xếp chúng và đặt các ký tự đã sắp xếp trở lại vị trí thành phần theo thứ tự chỉ mục tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng đồ thị Brute Force | O(n²) | O(n²) | Quá chậm | 
| Sắp xếp thành phần theo khối MSB | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi. 
2. Chia các vị trí thành các khối có bit được đặt cao nhất bằng nhau. 

Các khối này chính xác là:```
[1,1]
[2,3]
[4,7]
[8,15]
...
```bị cắt bớt khi chúng vượt quá độ dài chuỗi. 
3. Với mỗi khối, hãy thu thập các ký tự hiện có ở các vị trí đó. 
4. Sắp xếp các ký tự đã thu thập. 

Vì mọi cặp vị trí bên trong khối đều có thể được hoán đổi trực tiếp nên mọi hoán vị của khối đều có thể truy cập được. 
5. Xếp các ký tự đã sắp xếp lại vào các vị trí khối từ trái sang phải. 

Để giảm thiểu chuỗi tổng thể theo từ điển, ký tự nhỏ nhất có sẵn phải chiếm vị trí nhỏ nhất trong thành phần, ký tự nhỏ nhất tiếp theo ở vị trí tiếp theo, v.v. 
6. Sau khi xử lý tất cả các khối, xuất chuỗi kết quả. 

### Tại sao nó hoạt động 

Hai chỉ số thuộc về cùng một thành phần được kết nối chính xác khi chúng chia sẻ cùng một bit được đặt cao nhất. Bên trong một thành phần như vậy, mỗi cặp đỉnh được kết nối bằng một cạnh, do đó, mọi hoán vị ký tự của thành phần đều có thể truy cập được. 

Các ký tự không bao giờ có thể chuyển từ thành phần này sang thành phần khác vì không có cạnh giữa các thành phần. Do đó, bài toán phân rã thành các khối độc lập. 

Đối với một khối cố định, cách sắp xếp tốt nhất về mặt từ điển sẽ đặt các ký tự của nó theo thứ tự được sắp xếp trên các vị trí của khối. Vì các khối là độc lập và các vị trí được xử lý từ trái sang phải, nên việc kết hợp các cách sắp xếp tối ưu cục bộ này sẽ mang lại chuỗi có thể tiếp cận nhỏ nhất về mặt từ điển trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
ans = []

for _ in range(t):
    s = list(input().strip())
    n = len(s)

    res = s[:]

    start = 1
    while start <= n:
        end = min(n, 2 * start - 1)

        idxs = list(range(start - 1, end))
        chars = sorted(res[i] for i in idxs)

        for i, ch in zip(idxs, chars):
            res[i] = ch

        start *= 2

    ans.append("".join(res))

sys.stdout.write("\n".join(ans))
```Giải pháp xử lý từng khối MSB một cách độc lập. 

Biến`start`đại diện cho chỉ mục đầu tiên của một khối trong lập chỉ mục dựa trên 1. Khối tương ứng là$$[start,\;2 \cdot start - 1].$$Ví dụ:```
start = 1  -> [1,1]
start = 2  -> [2,3]
start = 4  -> [4,7]
start = 8  -> [8,15]
```Khối cuối cùng có thể vượt quá độ dài chuỗi, vì vậy chúng tôi cắt bớt nó bằng`min(n, 2 * start - 1)`. 

Các vị trí được chuyển đổi thành các chỉ số dựa trên 0 trước khi truy cập danh sách Python. 

Việc sắp xếp các ký tự của một khối sẽ mang lại sự sắp xếp nhỏ nhất về mặt từ điển bên trong thành phần đó. Viết chúng lại theo thứ tự chỉ mục sẽ thể hiện sự sắp xếp đó. 

Bởi vì tổng độ dài của tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, tổng công việc phân loại vẫn nằm trong giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
dog
```Thành phần là:```
[1]
[2,3]
```| Chặn | Nhân vật trước | Đã sắp xếp | Nhân vật sau | 
| --- | --- | --- | --- | 
| [1] | d | d | d | 
| [2,3] | ồ | đi | đi | 

Kết quả:```
dgo
```Ví dụ này cho thấy chỉ vị trí 2 và 3 mới có thể trao đổi ký tự. 

### Ví dụ 2 

đầu vào:```
goodpassword
```Thành phần là:```
[1]
[2,3]
[4,7]
[8,12]
```| Chặn | Nhân vật trước | Đã sắp xếp | 
| --- | --- | --- | 
| [1] | g | g | 
| [2,3] | ồ | ồ | 
| [4,7] | d p ​​a s | a d p s | 
| [8,12] | s từ | làm ồ s w | 

Việc xây dựng lại chuỗi mang lại:```
gooadpsdorsw
```Điều này chứng tỏ rằng các nhân vật có thể di chuyển tự do bên trong một khối nhưng không bao giờ di chuyển giữa các khối khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp ký tự của tất cả các thành phần | 
| Không gian | O(n) | Mảng đầu ra và lưu trữ ký tự tạm thời | 

Tổng kích thước đầu vào tối đa là$2 \cdot 10^5$, vì vậy một$O(n \log n)$giải pháp dễ dàng phù hợp trong giới hạn lập trình cạnh tranh 1 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        s = list(input().strip())
        n = len(s)

        res = s[:]

        start = 1
        while start <= n:
            end = min(n, 2 * start - 1)

            idxs = list(range(start - 1, end))
            chars = sorted(res[i] for i in idxs)

            for i, ch in zip(idxs, chars):
                res[i] = ch

            start *= 2

        out.append("".join(res))

    return "\n".join(out)

# provided samples
assert run("4\ndog\nabcdefgh\ngoodpassword\nonetwothreefour\n") == \
       "dgo\nabcdefgh\ngooadpsdorsw\noenottweefhorru"

# minimum size
assert run("1\na\n") == "a"

# block [2,3]
assert run("1\ncba\n") == "cab"

# all equal
assert run("1\naaaaaaa\n") == "aaaaaaa"

# full block sorting inside [4,7]
assert run("1\nabcdgfed\n") == "abcdefgd"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`a`| Chiều dài tối thiểu | 
|`cba`|`cab`| Chỉ trao đổi bên trong khối`[2,3]`| 
|`aaaaaaa`|`aaaaaaa`| Tất cả các ký tự bằng nhau | 
|`abcdgfed`|`abcdefgd`| Sắp xếp trong khối`[4,7]`chỉ | 

## Vỏ cạnh 

Hãy xem xét:```
1
ab
```Vị trí 1 thuộc khối`[1]`, vị trí 2 thuộc khối`[2,3]`. Chúng có các thành phần khác nhau nên không thể hoán đổi được. 

Thuật toán xử lý hai khối riêng biệt:```
[1] -> a
[2] -> b
```Đầu ra vẫn còn:```
ab
```Bây giờ hãy xem xét:```
1
cba
```Các khối là:```
[1]
[2,3]
```Nhân vật trong`[2,3]`là`b`Và`a`. Sắp xếp chúng mang lại`a b`. 

Chuỗi kết quả là:```
cab
```Một giải pháp cố gắng sắp xếp toàn bộ chuỗi sẽ tạo ra sai`abc`, không thể truy cập được. 

Cuối cùng, hãy xem xét:```
1
hgfedcba
```Các khối là:```
[1]
[2,3]
[4,7]
[8]
```Thuật toán chỉ sắp xếp bên trong mỗi khối:```
[1]      -> h
[2,3]    -> f g
[4,7]    -> b c d e
[8]      -> a
```Kết quả:```
hfgbcdea
```Các ký tự không thể di chuyển giữa các khối, vì vậy đây là chuỗi có thể truy cập nhỏ nhất về mặt từ điển.
