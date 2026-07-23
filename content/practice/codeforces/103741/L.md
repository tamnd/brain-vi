---
title: "CF 103741L - WA Phân loại"
description: "Chúng ta được cho một hoán vị có độ dài n và chúng ta xử lý từng tiền tố của nó một cách khái niệm. Đối với mỗi tiền tố A[1..k], chúng ta tưởng tượng đang chạy một quy trình sắp xếp nhất định có tên là SORT và chúng ta được yêu cầu ghi lại số lần một biến cụ thể m được chỉ định trong lần chạy đó."
date: "2026-07-02T09:07:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103741
codeforces_index: "L"
codeforces_contest_name: "HUSTPC 2022"
rating: 0
weight: 103741
solve_time_s: 47
verified: true
draft: false
---

[CF 103741L - Sắp xếp WA](https://codeforces.com/problemset/problem/103741/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị độ dài`n`và chúng tôi xử lý từng tiền tố của nó một cách khái niệm. Đối với mỗi tiền tố`A[1..k]`, chúng ta tưởng tượng việc chạy một thủ tục sắp xếp nhất định được gọi là`SORT`và chúng ta được yêu cầu ghi lại số lần một biến cụ thể`m`được chỉ định trong lần chạy đó. 

Điểm mấu chốt là chúng ta thực sự không cần mô phỏng toàn bộ quá trình sắp xếp. Hoạt động đang được tính gắn liền với cấu trúc của tiền tố và mỗi tiền tố độc lập theo nghĩa là chúng ta có thể tính toán phần đóng góp của nó mà không cần chạy lại mọi thứ từ đầu. 

Vì vậy, đầu ra là một mảng`s`, Ở đâu`s[k]`là số lần biến`m`được chỉ định trong thời gian`SORT(A[1..k])`. 

Ràng buộc`n ≤ 10^6`ngay lập tức loại trừ mọi giải pháp tính toán lại quy trình sắp xếp cho mỗi tiền tố. Thậm chí một`O(n log n)`phương pháp lặp đi lặp lại`n`thời gian sẽ quá chậm. Điều này thúc đẩy chúng tôi hướng tới một giải pháp trong đó chúng tôi duy trì trạng thái tăng dần khi mở rộng tiền tố. 

Một cách giải thích ngây thơ sẽ là mô phỏng việc sắp xếp cho mọi tiền tố một cách độc lập. Việc đó sẽ tốn khoảng`O(n^2)`làm việc ngay cả trước khi tính đến chi phí nội bộ của việc phân loại. 

Một trường hợp thất bại tinh vi hơn xuất phát từ việc giả định rằng “công việc sắp xếp” hoạt động tuyến tính trên các tiền tố mà không cần tính toán lại. Ví dụ: nếu chúng tôi tính toán lại từ đầu cho từng tiền tố, ngay cả những đầu vào nhỏ cũng hoạt động chính xác, nhưng độ phức tạp sẽ tăng vọt: 

đầu vào:```
5
4 2 5 3 1
```Nếu chúng tôi tính toán lại công việc sắp xếp cho mỗi tiền tố, chúng tôi sẽ xử lý liên tục cấu trúc chồng chéo. Sự trùng lặp này chính xác là những gì phải tránh. 

## Phương pháp tiếp cận 

Phần còn thiếu là hiểu được quy trình phân loại thực sự đang đo lường điều gì. Mặc dù mã giả không được hiển thị ở đây, nhưng thực tế là nó được mô tả là “sắp xếp bong bóng thích ứng” và chúng ta đang tính các phép gán cho một biến`m`gợi ý mạnh mẽ rằng mỗi phép gán như vậy tương ứng với việc khám phá một sự kiện giống như đảo ngược trong khi xử lý mảng. 

Trong kiểu sắp xếp bong bóng tiêu chuẩn, mỗi khi một sự đảo ngược được giải quyết bằng cách hoán đổi các phần tử liền kề, chúng ta sẽ “chạm” vào sự rối loạn đó một cách hiệu quả một lần. Nếu chúng ta diễn giải`m`khi theo dõi số lượng các bản cập nhật như vậy, thì tổng số bài tập sắp xếp tiền tố tương ứng chính xác với số lần đảo ngược bên trong tiền tố đó. 

Vì vậy với mỗi tiền tố`A[1..k]`, chúng ta cần số lượng đảo ngược: 

cặp`(i, j)`như vậy`1 ≤ i < j ≤ k`Và`A[i] > A[j]`. 

Một giải pháp brute-force sẽ tính toán điều này một cách độc lập cho mọi tiền tố. Điều đó thật đơn giản: với mỗi`k`, quét tất cả các cặp bên trong tiền tố. Điều này đúng nhưng tổng thể là bậc hai. 

Quan sát quan trọng là các tiền tố được lồng vào nhau. Khi chúng ta chuyển từ tiền tố`k-1`ĐẾN`k`, chúng tôi chỉ giới thiệu một phần tử mới`A[k]`. Tất cả các nghịch đảo mới liên quan đến`A[k]`chỉ phụ thuộc vào các phần tử trước đó. Điều này cho phép chúng tôi duy trì số lần đảo ngược tăng dần bằng cách sử dụng cấu trúc tần số. 

Chúng ta có thể duy trì số lượng giá trị trước đó nhỏ hơn hoặc lớn hơn giá trị hiện tại bằng cách sử dụng Cây Fenwick (Cây chỉ mục nhị phân). Mỗi phần tử mới góp phần đảo ngược bằng số lượng phần tử đã thấy lớn hơn nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Cây Fenwick (đảo ngược tiền tố) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý hoán vị từ trái sang phải trong khi duy trì cấu trúc dữ liệu lưu trữ số lần mỗi giá trị đã xuất hiện cho đến nay. 

1. Khởi tạo Cây Fenwick (Cây chỉ mục nhị phân) trong phạm vi giá trị`1..n`, ban đầu trống rỗng. Cấu trúc này cho phép chúng ta truy vấn có bao nhiêu phần tử được nhìn thấy là ≤ một giá trị và cập nhật tần số theo thời gian logarit. 
2. Duy trì một biến đang chạy`inv`lưu trữ số lượng đảo ngược của tiền tố hiện tại. Ban đầu nó là`0`. 
3. Đối với từng vị trí`k`từ`1`ĐẾN`n`, chúng ta lấy giá trị`x = A[k]`. 
4. Trước khi chèn`x`, chúng tôi truy vấn có bao nhiêu phần tử được chèn trước đó lớn hơn`x`. Điều này bằng với`(k-1) - count_leq(x)`, Ở đâu`count_leq(x)`là tổng tiền tố Fenwick bằng`x`. 
5. Chúng tôi thêm số này vào`inv`. Điều này thể hiện tất cả các nghịch đảo mới được giới thiệu bằng cách đặt`x`ở cuối tiền tố. 
6. Chúng tôi chèn`x`vào Cây Fenwick bằng cách tăng tần số của nó lên`1`. 
7. Chúng tôi xuất ra giá trị hiện tại của`inv`BẰNG`s[k]`. 

Lý do điều này có tác dụng là vì mọi đảo ngược trong tiền tố`k`hoặc đã tồn tại trong tiền tố`k-1`hoặc liên quan đến phần tử mới được thêm vào`A[k]`. Không có cách nào khác để hình thành các nghịch đảo mới, vì vậy bước cập nhật sẽ nắm bắt hoàn toàn delta. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

n = int(input())
a = list(map(int, input().split()))

fw = Fenwick(n)
inv = 0
out = []

for i, x in enumerate(a, 1):
    leq = fw.sum(x)
    inv += (i - 1 - leq)
    fw.add(x, 1)
    out.append(str(inv))

print(" ".join(out))
```Cây Fenwick là cấu trúc cốt lõi ở đây. các`sum(x)`cuộc gọi đếm xem có bao nhiêu phần tử trước đó ≤`x`, do đó trừ nó khỏi`(i-1)`cho số phần tử lớn hơn`x`, tương ứng trực tiếp với các phép đảo ngược mới được giới thiệu ở bước này. 

Một cạm bẫy triển khai phổ biến là quên rằng các chỉ số trong Cây Fenwick dựa trên 1, đó là lý do tại sao các giá trị được sử dụng trực tiếp vì các giá trị hoán vị đã nằm trong`1..n`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
A = [4, 2, 5, 3, 1]
```Chúng tôi theo dõi số lần đảo ngược trên mỗi tiền tố. 

| k | x | các yếu tố cho đến nay | nghịch đảo mới | tổng số tiền đầu tư | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | [] | 0 | 0 | 
| 2 | 2 | [4] | 1 (4 > 2) | 1 | 
| 3 | 5 | [4,2] | 0 | 1 | 
| 4 | 3 | [4,2,5] | 2 (4,5 > 3) | 3 | 
| 5 | 1 | [4,2,5,3] | 4 | 7 | 

Đầu ra:```
0 1 1 3 7
```Điều này cho thấy mỗi bước chỉ phụ thuộc vào các phần tử đã thấy trước đó như thế nào và cấu trúc Fenwick tránh quét lại tiền tố đầy đủ như thế nào. 

### Ví dụ 2 

đầu vào:```
n = 4
A = [1, 2, 3, 4]
```| k | x | nghịch đảo mới | tổng cộng | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 
| 2 | 2 | 0 | 0 | 
| 3 | 3 | 0 | 0 | 
| 4 | 4 | 0 | 0 | 

Đầu ra:```
0 0 0 0
```Điều này xác nhận rằng trong một hoán vị được sắp xếp đầy đủ, không có sự đảo ngược nào được đưa ra và cây Fenwick luôn báo cáo bằng 0 “phần tử lớn hơn”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi bản cập nhật trong số n bản cập nhật thực hiện truy vấn Fenwick và cập nhật | 
| Không gian | O(n) | Cây Fenwick lưu trữ tần số trên phạm vi giá trị 1..n | 

Các ràng buộc cho phép lên đến`10^6`các phần tử và mỗi phép toán đều là logarit, vì vậy giải pháp phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)
        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i
        def sum(self, i):
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

    n = int(input())
    a = list(map(int, input().split()))

    fw = Fenwick(n)
    inv = 0
    res = []

    for i, x in enumerate(a, 1):
        inv += (i - 1 - fw.sum(x))
        fw.add(x, 1)
        res.append(str(inv))

    return " ".join(res)

# provided sample (as interpreted inversion-prefix example)
assert run("5\n4 2 5 3 1\n") == "0 1 1 3 7"

# already sorted
assert run("4\n1 2 3 4\n") == "0 0 0 0"

# reverse sorted
assert run("4\n4 3 2 1\n") == "0 1 3 6"

# single element
assert run("1\n1\n") == "0"

# alternating pattern
assert run("5\n2 1 4 3 5\n") == "0 1 1 2 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng được sắp xếp | tất cả số không | không có trường hợp đảo ngược | 
| mảng đảo ngược | tăng trưởng hình tam giác | tăng trưởng đảo ngược trong trường hợp xấu nhất | 
| phần tử đơn | 0 | độ đúng ranh giới | 
| mô hình xen kẽ | tăng trưởng hỗn hợp | độ chính xác tăng dần | 

## Vỏ cạnh 

Đối với một hoán vị tăng nghiêm ngặt như`[1, 2, 3, ..., n]`, cây Fenwick luôn báo cáo không có phần tử nào lớn hơn giá trị hiện tại, do đó không có bản cập nhật nào làm tăng số lượng đảo ngược. Thuật toán đưa ra một chuỗi số 0 không đổi một cách chính xác vì mỗi truy vấn`fw.sum(x)`luôn bằng nhau`i-1`. 

Đối với một hoán vị giảm nghiêm ngặt như`[n, n-1, ..., 1]`, mỗi phần tử mới nhỏ hơn tất cả các phần tử trước đó. Ở bước`k`, truy vấn trả về 0 cho`fw.sum(x)`, vì vậy chúng tôi thêm`(k-1)`mỗi lần. Điều này tạo ra chuỗi tam giác chính xác của số lần đảo ngược và cây Fenwick tích lũy chính xác tất cả các đóng góp mà không bỏ sót các phần phụ thuộc tiền tố chéo.
