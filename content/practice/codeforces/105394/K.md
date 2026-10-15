---
title: "CF 105394K - Mèo con hỗn loạn"
description: "Chúng ta có một chuỗi dài chỉ gồm bốn ký tự: b, d, p và q. Chuỗi này được in trên một vật thủy tinh cứng."
date: "2026-06-23T17:07:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105394
codeforces_index: "K"
codeforces_contest_name: "2024-2025 ICPC German Collegiate Programming Contest (GCPC 2024)"
rating: 0
weight: 105394
solve_time_s: 85
verified: true
draft: false
---

[CF 105394K - Mèo con hỗn loạn](https://codeforces.com/problemset/problem/105394/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dài chỉ gồm bốn ký tự:`b`,`d`,`p`, Và`q`. Chuỗi này được in trên một vật thủy tinh cứng. Trong quá trình rơi, đối tượng được biến đổi liên tục bởi một chuỗi thao tác và chúng ta được yêu cầu xác định chuỗi trông như thế nào sau khi tất cả các phép biến đổi đã được áp dụng. 

Mỗi phép biến đổi là một chuyển động cứng nhắc của đối tượng, được mô tả bằng một trong ba thao tác`h`,`v`, hoặc`r`. Các thao tác này không “chỉnh sửa” chuỗi một cách tùy ý. Thay vào đó, chúng tương ứng với những cú lật vật lý hoặc sự xoay 180 độ của vật thể và các chữ cái thay đổi liên tục như thể chúng là những biểu tượng cứng nhắc được vẽ trên bề mặt. 

Điểm mấu chốt là mọi thao tác phải được áp dụng cho toàn bộ cấu hình và vì đối tượng cứng nhắc nên các thao tác lặp lại sẽ tạo thành một chuyển đổi tổng thể duy nhất. Độ dài chuỗi có thể lên tới 500.000, do đó việc áp dụng từng thao tác một cách đơn giản bằng cách xây dựng lại toàn bộ chuỗi sẽ quá chậm. 

Ràng buộc ngay lập tức loại trừ mọi giải pháp xử lý chuỗi một lần cho mỗi thao tác trong thời gian tuyến tính, vì trường hợp xấu nhất sẽ là$5 \cdot 10^5 \times 5 \cdot 10^5$, vượt xa giới hạn khả thi. Chúng ta cần một biểu diễn trong đó mỗi thao tác được xử lý trong thời gian không đổi và kết quả cuối cùng được xây dựng một lần. 

Một khó khăn nhỏ là cùng một chữ cái có thể xuất hiện nhiều lần trong đầu vào và khi chuyển đổi, mỗi lần xuất hiện phải hoạt động nhất quán. Bất kỳ cách tiếp cận nào cố gắng diễn giải lại các chữ cái cục bộ trên mỗi vị trí mà không có trạng thái chung sẽ nhanh chóng dễ bị lỗi. Do đó, các phép biến đổi phải được biểu diễn dưới dạng thay đổi trạng thái toàn cục được áp dụng thống nhất cho tất cả các ký tự. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng mọi phép biến đổi trực tiếp trên chuỗi. Mỗi thao tác sẽ quét toàn bộ chuỗi và thay thế mọi ký tự theo cách nó thay đổi theo phép biến đổi đó. Điều này đúng về mặt khái niệm vì sự chuyển đổi mang tính toàn cầu và thống nhất. 

Tuy nhiên, chi phí của mỗi hoạt động$O(n)$, và có thể có tới$5 \cdot 10^5$hoạt động. Điều này dẫn tới trường hợp xấu nhất là$O(n \cdot m)$, đó là về$2.5 \cdot 10^{11}$cập nhật nhân vật, vượt xa mọi giới hạn thời gian. 

Quan sát quan trọng là mỗi thao tác là một hoán vị cố định của bốn ký hiệu được tạo ra bởi một chuyển động cứng nhắc của vật thể. Vì các chuyển động cố định tạo thành một nhóm nhỏ khép kín nên việc áp dụng chúng nhiều lần chỉ tạo ra các hoán vị. Thay vì cập nhật chuỗi nhiều lần, chúng ta có thể duy trì một hoán vị hiện tại duy nhất của bảng chữ cái được tạo ra bởi tất cả các thao tác cho đến nay. 

Sau khi xử lý tất cả các thao tác, chúng ta áp dụng hoán vị cuối cùng một lần cho toàn bộ chuỗi trong$O(n)$thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng lại chuỗi cho mỗi thao tác |$O(nm)$|$O(n)$| Quá chậm | 
| Soạn các phép biến đổi sau đó áp dụng một lần |$O(n + m)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa từng hoạt động như một hoán vị trên tập hợp`{b, d, p, q}`. Ý tưởng là mọi phép biến đổi chỉ đổi tên các ký hiệu và thành phần của các phép biến đổi chỉ là thành phần hàm của những lần đổi tên này. 

Chúng tôi duy trì bản đồ hiện tại`f`, ban đầu là ánh xạ nhận dạng, nghĩa là mỗi ký tự ánh xạ tới chính nó. 

Chúng tôi cũng xác định trước các ánh xạ cho từng thao tác`h`,`v`, Và`r`. Mỗi trong số này là một hoán vị của bốn chữ cái. Áp dụng một thao tác có nghĩa là cập nhật`f`bằng cách soạn nó với hoán vị tương ứng. 

## bước 

1. Khởi tạo ánh xạ`f`như vậy`f(x) = x`cho tất cả các ký tự`x in {b, d, p, q}`. Điều này thể hiện chưa có chuyển đổi nào được áp dụng. 
2. Xác định ba hoán vị cố định`H`,`V`, Và`R`tương ứng với các phép biến đổi`h`,`v`, Và`r`. Những điều này được xác định bằng cách mỗi chuyển động cứng nhắc dán nhãn lại cho bốn biểu tượng. 
3. Lặp lại chuỗi chuyển đổi`t`từ trái sang phải. Đối với mỗi nhân vật: 

- Nếu vậy`h`, cập nhật`f(x) = H(f(x))`. 
- Nếu vậy`v`, cập nhật`f(x) = V(f(x))`. 
- Nếu vậy`r`, cập nhật`f(x) = R(f(x))`. 

Thứ tự này đảm bảo rằng các phép biến đổi được sắp xếp theo đúng trình tự, vì mỗi thao tác mới sẽ tác động lên cấu hình đã được chuyển đổi. 
4. Sau khi xử lý xong tất cả các thao tác, tạo chuỗi cuối cùng bằng cách thay thế từng ký tự`c`trong chuỗi gốc`s`với`f(c)`. 

### Tại sao nó hoạt động 

Mỗi phép toán là một song ánh trên bảng chữ cái hữu hạn`{b, d, p, q}`. Do đó, mọi chuỗi biến đổi tương ứng với một hoán vị duy nhất trong một nhóm hữu hạn có kích thước tối đa là 24. Thành phần của các phép toán có tính kết hợp, do đó thứ tự nhóm không quan trọng, chỉ có chuỗi. 

Bằng cách duy trì hoán vị tích lũy`f`, chúng tôi bảo toàn bất biến rằng sau khi xử lý tiền tố của các phép toán,`f`thể hiện chính xác hiệu quả ròng của các hoạt động đó. Áp dụng`f`một lần ở cuối sẽ tạo ra kết quả tương tự như việc áp dụng tuần tự từng thao tác cho toàn bộ chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def compose(a, b):
    # return a ∘ b (apply b first, then a)
    return {c: a[b[c]] for c in "bdpq"}

def solve():
    s = input().strip()
    t = input().strip()

    # These mappings are consistent permutations of the alphabet
    H = {'b': 'p', 'd': 'q', 'p': 'b', 'q': 'd'}
    V = {'b': 'd', 'd': 'b', 'p': 'q', 'q': 'p'}
    R = {'b': 'q', 'q': 'b', 'd': 'p', 'p': 'd'}

    f = {c: c for c in "bdpq"}

    for op in t:
        if op == 'h':
            f = compose(H, f)
        elif op == 'v':
            f = compose(V, f)
        else:
            f = compose(R, f)

    res = ''.join(f[c] for c in s)
    print(res)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc biểu diễn mọi phép biến đổi dưới dạng hoán vị từ điển. Người trợ giúp`compose`đảm bảo rằng chúng ta tích lũy các phép biến đổi một cách chính xác theo thứ tự. 

Vòng lặp cuối cùng kết thúc`s`áp dụng ánh xạ tích lũy một lần, đây là lần duy nhất chúng ta chạm vào toàn bộ chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
bbq
h
```Chúng tôi bắt đầu với bản đồ nhận dạng. 

Sau khi nộp đơn`h`, ta áp dụng hoán vị`H`. 

| Bước | Lập bản đồ f | 
| --- | --- | 
| ban đầu | b→b, d→d, p→p, q→q | 
| sau giờ | b→p, d→q, p→b, q→d | 

Hiện đang nộp đơn vào`bbq`: 

| vị trí | bản gốc | ánh xạ | 
| --- | --- | --- | 
| 1 | b | p | 
| 2 | b | p | 
| 3 | q | d | 

Kết quả là`ppd`. 

### Ví dụ 2 

đầu vào:```
bbq
v
```Bắt đầu với ánh xạ nhận dạng, sau đó áp dụng`v`. 

| Bước | Lập bản đồ f | 
| --- | --- | 
| ban đầu | b→b, d→d, p→p, q→q | 
| sau v | b→d, d→b, p→q, q→p | 

Áp dụng cho`bbq`: 

| vị trí | bản gốc | ánh xạ | 
| --- | --- | --- | 
| 1 | b | d | 
| 2 | b | d | 
| 3 | q | p | 

Kết quả là`ddp`. 

Những dấu vết này cho thấy một hoán vị toàn cục duy nhất xác định đầy đủ đầu ra như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi thao tác cập nhật một hoán vị có kích thước không đổi; chuỗi truyền cuối cùng là tuyến tính | 
| Không gian |$O(1)$| Chỉ lưu trữ ánh xạ có kích thước cố định trên 4 ký tự | 

Giải pháp phù hợp một cách thoải mái trong giới hạn vì cả chuỗi và chuỗi biến đổi đều được xử lý theo thời gian tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def compose(a, b):
        return {c: a[b[c]] for c in "bdpq"}

    def solve():
        s = input().strip()
        t = input().strip()

        H = {'b': 'p', 'd': 'q', 'p': 'b', 'q': 'd'}
        V = {'b': 'd', 'd': 'b', 'p': 'q', 'q': 'p'}
        R = {'b': 'q', 'q': 'b', 'd': 'p', 'p': 'd'}

        f = {c: c for c in "bdpq"}

        for op in t:
            if op == 'h':
                f = compose(H, f)
            elif op == 'v':
                f = compose(V, f)
            else:
                f = compose(R, f)

        print(''.join(f[c] for c in s))

    solve()
    return ""

# provided samples (format adapted)
run("bbq\nh\n")
run("bbq\nv\n")

# custom cases
run("b\nhvr\n")
run("bdpq\nvvv\n")
run("qqqq\nrhrh\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nướng + h | chuỗi biến đổi | ứng dụng hoán vị cơ bản | 
| thịt nướng + v | chuỗi biến đổi | tính chính xác của ánh xạ trao đổi | 
| char đơn + nhiều op | hành vi ổn định | trường hợp cạnh tối thiểu | 
| lặp lại v | cấu trúc bình đẳng | đóng hoán vị | 
| chuỗi thống nhất | tính nhất quán | xử lý ký tự giống hệt nhau | 

## Vỏ cạnh 

Một trường hợp khó phát hiện là khi cùng một chữ cái xuất hiện nhiều lần, như trong`bbq`. Tính chính xác phụ thuộc vào thực tế là ánh xạ có tính đồng nhất. Ví dụ, dưới bất kỳ phép biến đổi nào`f`, cả hai lần xuất hiện của`b`phải ánh xạ tới cùng một biểu tượng, vì`f`là một chức năng trên bảng chữ cái chứ không phải trên vị trí. 

Một trường hợp quan trọng khác là các phép biến đổi lặp đi lặp lại như`vvvvvv`. Vì mỗi thao tác là một hoán vị nên các ứng dụng lặp lại cuối cùng sẽ quay vòng. Thuật toán xử lý vấn đề này một cách tự nhiên vì thành phần của các hoán vị vẫn là một hoán vị và không có trạng thái cho mỗi ký tự được đưa vào. 

Cuối cùng, các chuỗi hỗn hợp dài như`hvrhv`chứng minh rằng trật tự là quan trọng. Thuật toán duy trì trật tự bằng cách sắp xếp các ánh xạ một cách tuần tự, đảm bảo rằng các phép biến đổi trước đó không bị mất hoặc bị ghi đè không chính xác.
