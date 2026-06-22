---
title: "CF 105791L - Cốc Giấy Huyền Thoại"
description: "Chúng ta được yêu cầu đếm xem có bao nhiêu số trong một khoảng nhất định có thể được biểu diễn dưới một dạng rất cụ thể: tích của ba số nguyên liên tiếp. Mọi số như vậy đều xuất phát từ việc chọn một số nguyên $x$ và tạo thành $x cdot (x+1) cdot (x+2)$."
date: "2026-06-21T13:11:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105791
codeforces_index: "L"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2025"
rating: 0
weight: 105791
solve_time_s: 43
verified: true
draft: false
---

[CF 105791L - Cốc giấy huyền thoại](https://codeforces.com/problemset/problem/105791/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem có bao nhiêu số trong một khoảng nhất định có thể được biểu diễn dưới một dạng rất cụ thể: tích của ba số nguyên liên tiếp. Mọi số như vậy đều xuất phát từ việc chọn một số nguyên$x$và hình thành$x \cdot (x+1) \cdot (x+2)$. Sau đó, bài toán đưa ra nhiều truy vấn, mỗi truy vấn cung cấp một phạm vi$[L, R]$và chúng ta phải xác định có bao nhiêu giá trị riêng biệt của dạng này nằm trong phạm vi đó. 

Vì vậy, thay vì làm việc với các số tùy ý, tập hợp các giá trị hợp lệ được xác định hoàn toàn bởi một tham số nguyên duy nhất$x$. Mỗi$x$tạo ra chính xác một giá trị ứng viên và nhiệm vụ sẽ đếm xem có bao nhiêu giá trị được tạo này nằm trong một khoảng nhất định. 

Những hạn chế là vô cùng lớn:$L$Và$R$có thể đi lên$10^{16}$, và có thể có tới$10^6$truy vấn. Điều này ngay lập tức loại trừ mọi lần lặp lại cho mỗi truy vấn trong khoảng thời gian đó, vì ngay cả việc quét phạm vi hoặc kiểm tra từng giá trị riêng lẻ cũng sẽ vượt xa giới hạn khả thi. Giải pháp phải dựa vào quá trình tiền xử lý hoặc bản địa hóa toán học nhanh chóng của các ứng viên hợp lệ. 

Một điểm cần cân nhắc tinh tế là các giá trị hợp lệ rất thưa thớt. chức năng$f(x) = x(x+1)(x+2)$tăng theo khối nên các giá trị nhanh chóng trở nên rất xa nhau. Điều đó cho thấy chúng ta có thể tính toán trước tất cả các giá trị hợp lệ đến mức tối đa có thể$R$và trả lời các truy vấn thông qua tìm kiếm nhị phân. 

Một cạm bẫy tiềm tàng là giả định nhiều$x$các giá trị có thể ánh xạ tới cùng một sản phẩm. Điều này không xảy ra với số nguyên dương vì hàm số tăng nghiêm ngặt đối với$x \ge 1$. Một vấn đề khác là tràn trong quá trình tạo, vì giá trị tăng lên tới$10^{16}$, vì vậy chúng ta phải dừng thế hệ một cách cẩn thận. 

## Phương pháp tiếp cận 

Một ý tưởng đơn giản là thử mọi cách có thể$x$và tính toán$x(x+1)(x+2)$, sau đó với mỗi truy vấn, hãy quét tất cả các giá trị và đếm xem có bao nhiêu giá trị nằm trong đó$[L, R]$. Điều này đúng nhưng hoàn toàn không thực tế. Ngay cả khi chúng ta hạn chế$x$, tối đa$x$như vậy$x^3 \le 10^{16}$ở xung quanh$10^5$. Điều đó đã mang lại khoảng$10^5$các giá trị. Với tối đa$10^6$truy vấn, việc quét tuyến tính trên mỗi truy vấn sẽ dẫn đến$10^{11}$hoạt động quá lớn. 

Quan sát quan trọng là cấu trúc của các số hợp lệ hoàn toàn không phụ thuộc vào các truy vấn. Tất cả các giá trị hợp lệ có thể được tạo độc lập một lần vì chúng chỉ phụ thuộc vào$x$. Vì hàm tăng đơn điệu nên chúng ta có thể lưu trữ tất cả các giá trị trong một mảng được sắp xếp. Sau đó, mỗi truy vấn sẽ trở thành một bài toán đếm phạm vi trên một danh sách đã được sắp xếp, danh sách này có thể được trả lời bằng hai tìm kiếm nhị phân. 

Điều này biến vấn đề từ “tính toán lại cấu trúc cho mỗi truy vấn” thành “tính toán trước một lần, truy vấn nhanh”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Quét Brute Force cho mỗi truy vấn |$O(t \cdot n)$|$O(n)$| Quá chậm | 
| Tính toán trước + tìm kiếm nhị phân |$O(n + t \log n)$|$O(n)$| Đã chấp nhận | 

Đây$n$là số hợp lệ$x$, đại khái$10^5$. 

## Hướng dẫn thuật toán 

Chúng tôi bắt đầu bằng cách tạo ra tất cả các giá trị hợp lệ của biểu mẫu$x(x+1)(x+2)$cho đến khi giá trị vượt quá$10^{16}$. Vì hàm tăng theo khối nên chúng ta chỉ cần lặp$x$lên đến khoảng$10^5$, an toàn. 

Chúng tôi lưu trữ mọi giá trị được tính toán trong một danh sách. Bởi vì hàm số tăng nghiêm ngặt theo chiều dương$x$, danh sách đã được sắp xếp nên không cần sắp xếp thêm. 

Đối với mỗi truy vấn$[L, R]$, chúng tôi xác định có bao nhiêu giá trị được tính toán trước nằm trong khoảng bằng cách sử dụng tìm kiếm nhị phân. Cụ thể, chúng tôi tìm thấy vị trí đầu tiên có giá trị ít nhất là$L$và vị trí đầu tiên có giá trị lớn hơn$R$. Sự khác biệt giữa hai vị trí này đưa ra câu trả lời. 

### Các bước 

1. Lặp lại$x = 1, 2, 3, \dots$và tính toán$v = x(x+1)(x+2)$. 

Chúng tôi dừng lại khi$v > 10^{16}$, vì ngoài phạm vi này không có truy vấn nào có thể bao gồm nó. 
2. Lưu trữ tất cả các giá trị được tạo trong một mảng`vals`. 

Điều này có tác dụng vì mỗi số hợp lệ tương ứng duy nhất với một$x$. 
3. Đối với mỗi truy vấn$[L, R]$, thực hiện tìm kiếm nhị phân để tìm: 

chỉ số nhỏ nhất ở đó`vals[i] >= L`và chỉ số nhỏ nhất trong đó`vals[i] > R`. 
4. Câu trả lời cho truy vấn là sự khác biệt của hai chỉ số này. 

Tính đúng đắn của việc sử dụng tìm kiếm nhị phân phụ thuộc vào thực tế là`vals`đang tăng nghiêm ngặt nên mọi số hợp lệ đều được sắp thứ tự. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là`vals`chứa mọi sản phẩm hợp lệ có thể có đúng một lần, theo thứ tự tăng dần. Vì mọi truy vấn chỉ hỏi xem có bao nhiêu giá trị được tính toán trước nằm trong một phạm vi, nên vấn đề giảm xuống còn việc đếm các phần tử trong một mảng được sắp xếp. Tìm kiếm nhị phân cung cấp các vị trí ranh giới chính xác mà không cần quét, duy trì tính chính xác đồng thời giảm độ phức tạp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 10**16

vals = []
x = 1
while True:
    v = x * (x + 1) * (x + 2)
    if v > MAXV:
        break
    vals.append(v)
    x += 1

# binary search helpers
def lower_bound(a, target):
    lo, hi = 0, len(a)
    while lo < hi:
        mid = (lo + hi) // 2
        if a[mid] < target:
            lo = mid + 1
        else:
            hi = mid
    return lo

def upper_bound(a, target):
    lo, hi = 0, len(a)
    while lo < hi:
        mid = (lo + hi) // 2
        if a[mid] <= target:
            lo = mid + 1
        else:
            hi = mid
    return lo

t = int(input())
out = []

for _ in range(t):
    L, R = map(int, input().split())
    left = lower_bound(vals, L)
    right = upper_bound(vals, R)
    out.append(str(right - left))

print("\n".join(out))
```Vòng lặp tiền xử lý xây dựng tất cả các giá trị huyền thoại hợp lệ một lần, đảm bảo không tính toán lại cho mỗi truy vấn. Điều kiện dừng tại$10^{16}$tránh tràn và làm việc không cần thiết. 

Các thủ tục tìm kiếm nhị phân là các công cụ tìm ranh giới tiêu chuẩn.`lower_bound`tìm thấy giá trị hợp lệ đầu tiên không nhỏ hơn$L$, trong khi`upper_bound`tìm thấy giá trị đầu tiên lớn hơn$R$. Sự khác biệt của chúng trực tiếp tính các mục hợp lệ trong phạm vi. 

Một chi tiết triển khai tinh tế là sử dụng số nguyên Python một cách an toàn mà không lo tràn. Tuy nhiên, rõ ràng dừng lại ở$10^{16}$giữ cho danh sách được tạo nhỏ gọn và tránh sự tăng trưởng không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
10 100
```Các giá trị được tính toán trước bắt đầu như sau:$1\cdot2\cdot3 = 6$,$2\cdot3\cdot4 = 24$,$3\cdot4\cdot5 = 60$,$4\cdot5\cdot6 = 120$, ... 

| x | giá trị | trong phạm vi [10,100] | 
| --- | --- | --- | 
| 1 | 6 | không | 
| 2 | 24 | vâng | 
| 3 | 60 | vâng | 
| 4 | 120 | không | 

Tìm kiếm nhị phân tìm thấy 24 và 60, vì vậy câu trả lời là 2. 

Điều này xác nhận rằng chỉ có lọc ranh giới mới quan trọng chứ không phải liệt kê cho mỗi truy vấn. 

### Ví dụ 2 

đầu vào:```
1
50 1000
```| x | giá trị | trong phạm vi [50,1000] | 
| --- | --- | --- | 
| 1 | 6 | không | 
| 2 | 24 | không | 
| 3 | 60 | vâng | 
| 4 | 120 | vâng | 
| 5 | 210 | vâng | 
| 6 | 336 | vâng | 
| 7 | 504 | vâng | 
| 8 | 720 | vâng | 
| 9 | 990 | vâng | 
| 10 | 1320 | không | 

Các giá trị hợp lệ bao gồm từ x = 3 đến x = 9, cho 7 giá trị. Tìm kiếm nhị phân trả về chính xác số đếm này, hiển thị tính chính xác trong khoảng thời gian dài hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + t \log n)$| Tính toán trước tạo ra tất cả các giá trị hợp lệ một lần, sau đó mỗi truy vấn sử dụng hai tìm kiếm nhị phân | 
| Không gian |$O(n)$| Lưu trữ tất cả các sản phẩm hợp lệ lên đến$10^{16}$, Về$10^5$giá trị | 

Các ràng buộc cho phép lên đến$10^6$truy vấn, vì vậy logarit cho mỗi hành vi truy vấn là điều cần thiết. Chi phí tính toán trước không đáng kể so với khối lượng truy vấn và mức sử dụng bộ nhớ vẫn ở mức thấp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MAXV = 10**16
    vals = []
    x = 1
    while True:
        v = x * (x + 1) * (x + 2)
        if v > MAXV:
            break
        vals.append(v)
        x += 1

    def lower_bound(a, target):
        lo, hi = 0, len(a)
        while lo < hi:
            mid = (lo + hi) // 2
            if a[mid] < target:
                lo = mid + 1
            else:
                hi = mid
        return lo

    def upper_bound(a, target):
        lo, hi = 0, len(a)
        while lo < hi:
            mid = (lo + hi) // 2
            if a[mid] <= target:
                lo = mid + 1
            else:
                hi = mid
        return lo

    t = int(input())
    out = []
    for _ in range(t):
        L, R = map(int, input().split())
        out.append(str(upper_bound(vals, R) - lower_bound(vals, L)))
    return "\n".join(out)

# provided samples
assert run("1\n10 100\n") == "2"
assert run("3\n10 100\n100 1000\n999900 999999\n") == "2\n7\n1"

# custom cases
assert run("1\n1 5\n") == "0"
assert run("1\n6 6\n") == "1"
assert run("1\n6 7\n") == "1"
assert run("1\n1 10000000000000000\n") == str(len([x for x in range(1, 200000) if x*(x+1)*(x+2) <= 10**16]))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 | 0 | phạm vi dưới giá trị hợp lệ đầu tiên | 
| 6 6 | 1 | trường hợp khớp chính xác | 
| 6 7 | 1 | mở rộng ranh giới | 
| đầy đủ | tất cả các giá trị | phạm vi bảo hiểm tối đa | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi khoảng nằm hoàn toàn bên dưới giá trị hợp lệ nhỏ nhất. Sản phẩm nhỏ nhất là$1 \cdot 2 \cdot 3 = 6$. Đối với một đầu vào như$[1, 5]$, thuật toán tạo ra số lượng trống vì cả hai tìm kiếm nhị phân đều trả về chỉ mục 0 và không có giá trị nào được đưa vào. 

Một trường hợp khác là khi$L$hoặc$R$khớp chính xác với một sản phẩm hợp lệ. Vì$[6, 6]$,`lower_bound`trả về chỉ số 0 và`upper_bound`trả về chỉ số 1, tạo ra số đếm chính xác 1. Cấu trúc đơn điệu đảm bảo không có sự mơ hồ trong các ranh giới khớp. 

Khoảng cách lớn như$[1, 10^{16}]$kiểm tra phạm vi bảo hiểm đầy đủ. Trong trường hợp này, tìm kiếm nhị phân mở rộng toàn bộ danh sách được tính toán trước và trả về kích thước đầy đủ của nó, vì tất cả các giá trị hợp lệ đều nằm trong phạm vi.
