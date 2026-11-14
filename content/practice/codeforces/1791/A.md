---
title: "CF 1791A - Kiểm tra lực lượng mã"
description: "Chúng ta được giao một vấn đề quyết định rất nhỏ được lặp đi lặp lại nhiều lần. Mỗi trường hợp thử nghiệm cung cấp một chữ cái tiếng Anh viết thường và chúng ta phải quyết định xem chữ cái đó có thuộc một chuỗi tham chiếu cố định hay không, cụ thể là “codeforces”."
date: "2026-06-09T10:28:36+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1791
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 849 (Div. 4)"
rating: 800
weight: 1791
solve_time_s: 63
verified: true
draft: false
---

[CF 1791A - Kiểm tra Codeforces](https://codeforces.com/problemset/problem/1791/A) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một vấn đề quyết định rất nhỏ được lặp đi lặp lại nhiều lần. Mỗi trường hợp thử nghiệm cung cấp một chữ cái tiếng Anh viết thường và chúng ta phải quyết định xem chữ cái đó có thuộc một chuỗi tham chiếu cố định hay không, cụ thể là “codeforces”. 

Vì vậy, nhiệm vụ không phải là xây dựng hoặc sửa đổi các chuỗi mà là về tư cách thành viên: chúng tôi đang kiểm tra một cách hiệu quả xem một ký tự có được chứa trong một tập hợp ký tự được xác định trước hay không. 

Kích thước đầu vào cực kỳ nhỏ, tối đa 26 trường hợp thử nghiệm. Điều đó ngay lập tức loại trừ mọi nhu cầu tối ưu hóa ngoài thời gian không đổi cho mỗi truy vấn. Ngay cả việc quét lặp lại một chuỗi ngắn cũng đủ. 

Những sai lầm tinh tế duy nhất có thể xảy ra ở đây là do coi vấn đề là vị trí chứ không phải dựa trên tư cách thành viên. Ví dụ: ai đó có thể giả định không chính xác rằng chỉ các chỉ số cụ thể mới quan trọng mà không nhận ra rằng các chỉ số trùng lặp là không liên quan. Một sai lầm khác có thể xảy ra là quên rằng việc kiểm tra tư cách thành viên phải phân biệt chữ hoa chữ thường; chữ in hoa không bao giờ là một phần của tập hợp lệ. 

Một trường hợp lỗi cụ thể sẽ diễn giải chuỗi không chính xác theo thứ tự khớp: 

đầu vào:```
1
c
```Đầu ra đúng:```
YES
```Một cách tiếp cận có lỗi có thể kiểm tra xem ký tự có ở một chỉ mục cụ thể trong chuỗi chứ không phải ở bất kỳ vị trí nào trong chuỗi hay không và từ chối nó một cách không chính xác trừ khi nó khớp với vị trí đã chọn. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải quyết vấn đề là coi chuỗi “codeforces” như một danh sách các ký tự và đối với mỗi truy vấn, hãy quét qua chuỗi đó để xem ký tự đã cho có khớp với bất kỳ vị trí nào không. Vì chuỗi tham chiếu có độ dài 10 nên việc này mất thời gian không đổi cho mỗi trường hợp thử nghiệm nhưng vẫn liên quan đến việc lặp lại. 

Phương pháp bạo lực này đã đủ vì giới hạn đầu vào rất nhỏ. Tuy nhiên, cấu trúc của bài toán thậm chí còn làm cho nó đơn giản hơn: thành viên trong một tập hợp cố định nhỏ có thể được tính toán trước một lần và mỗi truy vấn sẽ trở thành một tra cứu liên tục. 

Quan sát quan trọng là chúng ta không quan tâm đến thứ tự hoặc tần số, chỉ quan tâm đến việc ký tự có tồn tại trong một tập hợp đã biết hay không. Điều đó cho phép chúng tôi thay thế việc quét bằng kiểm tra tư cách thành viên trực tiếp bằng cách sử dụng một tập hợp hoặc mảng boolean được lập chỉ mục theo ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu | O(10 · t) | O(1) | Đã chấp nhận | 
| Đặt tra cứu | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cấu trúc dữ liệu chứa tất cả các ký tự riêng biệt từ chuỗi “codeforces”. Đây có thể là một tập hợp các ký tự. Mục đích là để cho phép các truy vấn thành viên liên tục. 
2. Đọc số lượng test case t. 
3. Với mỗi test, hãy đọc ký tự c. Chúng tôi coi nó như một chuỗi hoặc ký tự một phần tử. 
4. Kiểm tra xem c có tồn tại trong tập hợp được tính toán trước hay không. Nếu có, xuất ra “CÓ”; nếu không thì xuất ra “NO”. 
5. Lặp lại cho đến khi tất cả các trường hợp kiểm thử được xử lý. 

Lý do đằng sau việc tính toán trước tập hợp là chuỗi tham chiếu không bao giờ thay đổi. Bất kỳ lần quét lặp lại nào cũng sẽ tính toán lại cùng một thông tin một cách dư thừa. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính bất biến là tập hợp luôn chứa chính xác các ký tự có trong “codeforces” và không có gì khác. Mỗi truy vấn là độc lập và việc kiểm tra thành viên tương đương với việc kiểm tra xem c có phải là một phần tử của tập hợp cố định đó hay không. Vì tư cách thành viên đã thiết lập là chính xác và không mất dữ liệu nên không thể xảy ra kết quả dương tính giả hoặc âm tính giả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = "codeforces"
    allowed = set(s)

    t = int(input())
    out = []

    for _ in range(t):
        c = input().strip()
        if c in allowed:
            out.append("YES")
        else:
            out.append("NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp tính toán trước một tập hợp từ chuỗi một lần, giúp tránh việc quét tuyến tính lặp lại. Mỗi truy vấn sau đó trở thành một tra cứu băm theo thời gian liên tục. sử dụng`strip()`đảm bảo chúng tôi xóa chính xác ký tự dòng mới mà không ảnh hưởng đến chính chữ cái đó. 

Đầu ra được tích lũy trong một danh sách và được in cùng một lúc, điều này tránh được chi phí I/O lặp lại bên trong vòng lặp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
a
c
d
x
o
```| Bước | Đầu vào c | Bộ chứa c? | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | một | Không | KHÔNG | 
| 2 | c | Có | CÓ | 
| 3 | d | Có | CÓ | 
| 4 | x | Không | KHÔNG | 
| 5 | o | Có | CÓ | 

Dấu vết này hiển thị đánh giá thành viên trực tiếp đối với bộ ký tự cố định. Mỗi quyết định là độc lập và không yêu cầu trạng thái nào ngoài tập hợp được tính toán trước. 

### Ví dụ 2 

đầu vào:```
3
f
z
e
```| Bước | Đầu vào c | Bộ chứa c? | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | f | Có | CÓ | 
| 2 | z | Không | KHÔNG | 
| 3 | e | Có | CÓ | 

Điều này xác nhận rằng các chữ cái bên ngoài chuỗi bị từ chối một cách chính xác, trong khi tất cả các chữ cái hợp lệ đều được chấp nhận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm thực hiện tra cứu theo thời gian cố định | 
| Không gian | O(1) | Bộ có tối đa 10 ký tự | 

Các ràng buộc đủ nhỏ để ngay cả quá trình quét O(10·t) kém tối ưu hơn cũng có thể dễ dàng vượt qua. Giải pháp được chọn đơn giản hơn và tránh truyền tải lặp đi lặp lại, đảm bảo hiệu suất rõ ràng và có thể dự đoán được trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import sys as _sys

    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""10
a
b
c
d
e
f
g
h
i
j
""") == """NO
NO
YES
YES
YES
YES
NO
NO
NO
NO"""

# custom cases
assert run("""1
c
""") == "YES", "single valid char"

assert run("""1
z
""") == "NO", "invalid char"

assert run("""3
o
c
x
""") == """YES
YES
NO""", "mixed membership"

assert run("""5
d
e
f
g
h
""") == """YES
YES
YES
NO
NO""", "boundary mix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| c đơn | CÓ | trường hợp tích cực tối thiểu | 
| z đơn | KHÔNG | trường hợp tiêu cực tối thiểu | 
| ồ cx | CÓ CÓ KHÔNG | logic thành viên hỗn hợp | 
| d e f g h | CÓ CÓ CÓ KHÔNG KHÔNG | nhiều chuyển tiếp hợp lệ + không hợp lệ liền kề | 

## Vỏ cạnh 

Trường hợp một cạnh là một ký tự không nằm trong phạm vi bảng chữ cái tiếng Anh mà trực giác mong đợi, chẳng hạn như 'z'. Thuật toán xử lý việc này một cách chính xác vì tư cách thành viên hoàn toàn dựa trên tập hợp. Đối với đầu vào`z`, việc tra cứu thiết lập không thành công và đầu ra là “KHÔNG”. 

Một trường hợp đặc biệt khác được lặp lại các ký tự hợp lệ trên nhiều trường hợp thử nghiệm. Đối với đầu vào:```
3
c
c
c
```mỗi tra cứu là độc lập. Tập hợp này không thay đổi, do đó mọi truy vấn đều trả về “CÓ”, xác nhận rằng không có rò rỉ trạng thái giữa các trường hợp thử nghiệm. 

Cuối cùng, trường hợp biên là kích thước đầu vào nhỏ nhất có thể:```
1
a
```Vì 'a' không có trong "codeforces", kiểm tra tập hợp trả về chính xác "KHÔNG", cho thấy thuật toán không giả định bất kỳ giá trị dương mặc định nào.
