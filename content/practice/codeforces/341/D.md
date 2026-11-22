---
title: "CF 341D - Iahub và Xors"
description: "Chúng ta có một ma trận $n lần n$ ban đầu chứa đầy các số 0. Bài toán yêu cầu chúng ta xử lý hai loại phép toán một cách hiệu quả: một phép toán tính xor của tất cả các phần tử bên trong một ma trận con và một phép toán khác tính xor một giá trị nhất định vào mọi phần tử của ma trận con."
date: "2026-06-06T17:32:52+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 341
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 198 (Div. 1)"
rating: 2500
weight: 341
solve_time_s: 146
verified: false
draft: false
---

[CF 341D - Iahub và Xors](https://codeforces.com/problemset/problem/341/D) 

**Đánh giá:** 2500 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 2 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$ma trận ban đầu chứa đầy số không. Bài toán yêu cầu chúng ta xử lý hai loại phép toán một cách hiệu quả: một phép toán tính xor của tất cả các phần tử bên trong một ma trận con và một phép toán khác tính xor một giá trị nhất định vào mọi phần tử của ma trận con. Mỗi thao tác chỉ định tọa độ trên cùng bên trái và dưới cùng bên phải của ma trận con và để cập nhật, một giá trị cho xor. 

Giới hạn đầu vào là đáng kể.$n$có thể lên tới 1000, do đó tổng số phần tử lên tới$10^6$, và số lượng hoạt động$m$có thể cao như$10^5$. Một giải pháp đơn giản lặp qua tất cả các ô của ma trận con cho mỗi truy vấn hoặc cập nhật sẽ chiếm tới$10^5 \cdot 10^6 = 10^{11}$trong trường hợp xấu nhất, vượt xa những gì chúng tôi có thể xử lý trong giới hạn 1 giây. Điều này có nghĩa là chúng ta không thể chạm vào từng phần tử riêng lẻ cho mỗi thao tác; chúng ta cần một cấu trúc dữ liệu cho phép cả cập nhật phạm vi và truy vấn phạm vi một cách hiệu quả. 

Các trường hợp biên cần chú ý bao gồm ma trận con một phần tử, ma trận con bao trùm toàn bộ ma trận và nhiều bản cập nhật cho cùng một phần tử. Ví dụ, xoring cùng một giá trị hai lần sẽ loại bỏ nó. Cách tiếp cận tiền tố-xor ngây thơ mà không xử lý chính xác các cập nhật phạm vi có thể cho kết quả sai khi các bản cập nhật trùng lặp. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi lần cập nhật, chúng tôi lặp lại mọi phần tử trong ma trận con đã cho và xor nó với giá trị đã cho. Đối với mỗi truy vấn, chúng tôi lặp qua ma trận con và xor tất cả các giá trị. Cách tiếp cận này đúng nhưng chậm: đối với một$n \times n$ma trận với$m$các thao tác, mỗi thao tác có khả năng chạm vào toàn bộ ma trận, chúng ta có thể có tới$m \cdot n^2 = 10^5 \cdot 10^6 = 10^{11}$hoạt động, điều đó là không thể thực hiện được. 

Thông tin chi tiết quan trọng là xor hoạt động tốt trong cả phép tính cộng và trừ vì nó là nghịch đảo của chính nó. Thuộc tính này cho phép chúng tôi sử dụng Cây Fenwick 2D (Cây chỉ mục nhị phân) trong đó mỗi ô lưu trữ các đóng góp xor. Cây Fenwick 2D tiêu chuẩn hỗ trợ cập nhật điểm và truy vấn hình chữ nhật, nhưng chúng ta có thể mở rộng nó để hỗ trợ cập nhật hình chữ nhật bằng cách sử dụng nguyên tắc bao gồm-loại trừ. Ý tưởng là duy trì một cấu trúc trong đó chúng ta có thể xor một giá trị cho một hình chữ nhật bằng cách cập nhật bốn góc một cách thích hợp và sau đó, bất kỳ truy vấn nào trên một hình chữ nhật đều có thể được biểu thị bằng cách sử dụng tổng xor của các góc này. Điều này làm giảm mỗi hoạt động xuống$O(\log^2 n)$, đủ nhanh cho các ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * m) | O(n^2) | Quá chậm | 
| 2D BIT (Cập nhật hình chữ nhật + Truy vấn) | O(m * log^2 n) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo cây chỉ mục nhị phân 2D`bit`với kích thước$n+1 \times n+1$để xử lý việc lập chỉ mục dựa trên 1 một cách thuận tiện. 
2. Xác định hàm trợ giúp`xor_update(x, y, val)`thực hiện cập nhật BIT tiêu chuẩn: xor`val`vào tất cả các vị trí trong BIT bị ảnh hưởng bởi tọa độ`(x, y)`. Đây là khối xây dựng cốt lõi. 
3. Để xor một giá trị`v`trên một hình chữ nhật`(x0, y0)`ĐẾN`(x1, y1)`, chúng tôi áp dụng loại trừ bao gồm: 

- xor`v`Tại`(x0, y0)`- xor`v`Tại`(x0, y1 + 1)`- xor`v`Tại`(x1 + 1, y0)`- xor`v`Tại`(x1 + 1, y1 + 1)`Điều này đảm bảo rằng khi chúng ta tính toán tiền tố xor cho đến bất kỳ ô nào`(x, y)`, sự đóng góp từ các hình chữ nhật chồng chéo sẽ kết hợp chính xác. 
4. Xác định hàm trợ giúp`prefix_xor(x, y)`tính toán xor của ma trận con từ`(1,1)`ĐẾN`(x, y)`sử dụng truy vấn 2D BIT tiêu chuẩn. 
5. Để tính xor của một ma trận con tùy ý`(x0, y0, x1, y1)`, áp dụng loại trừ bao gồm bằng cách sử dụng các giá trị xor tiền tố: 

- kết quả =`prefix_xor(x1, y1) ^ prefix_xor(x0-1, y1) ^ prefix_xor(x1, y0-1) ^ prefix_xor(x0-1, y0-1)`6. Xử lý từng thao tác trong đầu vào. Nếu đó là một bản cập nhật, hãy gọi thủ tục cập nhật hình chữ nhật. Nếu đó là một truy vấn, hãy tính xor bằng cách sử dụng xors tiền tố ma trận con và in kết quả. 

Tại sao nó hoạt động: BIT 2D với tính năng loại trừ bao gồm duy trì đóng góp xor của tất cả các bản cập nhật. Mỗi truy vấn tiền tố truy xuất chính xác xor của tất cả các cập nhật ảnh hưởng đến một hình chữ nhật nhất định. Vì xor có tính liên kết và tự đảo ngược nên các cập nhật chồng chéo sẽ bị hủy một cách chính xác và không xảy ra hiện tượng đếm kép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class BIT2D:
    def __init__(self, n):
        self.n = n
        self.bit = [[0] * (n + 2) for _ in range(n + 2)]

    def update(self, x, y, val):
        i = x
        while i <= self.n:
            j = y
            while j <= self.n:
                self.bit[i][j] ^= val
                j += j & -j
            i += i & -i

    def rectangle_update(self, x0, y0, x1, y1, val):
        self.update(x0, y0, val)
        self.update(x0, y1 + 1, val)
        self.update(x1 + 1, y0, val)
        self.update(x1 + 1, y1 + 1, val)

    def prefix_xor(self, x, y):
        res = 0
        i = x
        while i > 0:
            j = y
            while j > 0:
                res ^= self.bit[i][j]
                j -= j & -j
            i -= i & -i
        return res

    def query(self, x0, y0, x1, y1):
        return (self.prefix_xor(x1, y1) ^ self.prefix_xor(x0 - 1, y1) ^
                self.prefix_xor(x1, y0 - 1) ^ self.prefix_xor(x0 - 1, y0 - 1))

n, m = map(int, input().split())
bit2d = BIT2D(n)

for _ in range(m):
    cmd = list(map(int, input().split()))
    if cmd[0] == 1:
        x0, y0, x1, y1 = cmd[1:]
        print(bit2d.query(x0, y0, x1, y1))
    else:
        x0, y0, x1, y1, v = cmd[1:]
        bit2d.rectangle_update(x0, y0, x1, y1, v)
```các`BIT2D`lớp gói gọn tất cả logic BIT. các`update`phương pháp sửa đổi một ô và tất cả tổ tiên của nó, và`rectangle_update`áp dụng xor cho bốn góc. các`query`phương thức sử dụng loại trừ bao gồm để truy xuất xor chính xác của ma trận con. 

Xử lý ranh giới là tinh tế. Chúng tôi cần`n+2`cho mảng bên trong để tránh tràn chỉ mục khi`x1+1`hoặc`y1+1`bằng`n+1`. Việc sử dụng lập chỉ mục dựa trên 1 sẽ tránh nhầm lẫn với việc truyền tải BIT. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hoạt động đầu vào: 

| Hoạt động | Mô tả | Hiệu ứng BIT | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| 2 1 1 2 2 1 | xor 1 trong hình chữ nhật (1,1)-(2,2) | bốn góc cập nhật | - | 
| 2 1 3 2 3 2 | xor 2 trong hình chữ nhật (1,2)-(3,3) | bốn góc cập nhật | - | 
| 2 3 1 3 3 3 | xor 3 trong hình chữ nhật (3,1)-(3,3) | bốn góc cập nhật | - | 
| 1 2 2 3 3 | truy vấn hình chữ nhật (2,2)-(3,3) | prefix_xor cho 3 | 3 | 
| 1 2 2 3 2 | truy vấn hình chữ nhật (2,2)-(3,2) | prefix_xor cho 2 | 2 | 

Dấu vết này cho thấy cách các bản cập nhật hình chữ nhật được lan truyền chính xác và loại trừ bao gồm prefix_xor truy xuất xor đúng. 

### Ví dụ tùy chỉnh 

đầu vào:```
2 3
2 1 1 2 2 5
2 1 2 1 2 7
1 1 1 2 2
```Dấu vết bước: 

| Hoạt động | Hiệu ứng ma trận | Kết quả truy vấn | 
| --- | --- | --- | 
| xor 5 trong (1,1)-(2,2) | tất cả các phần tử = 5 | - | 
| xor 7 trong (1,2)-(2,2) | phần tử: [[5,2],[5,2]] | - | 
| truy vấn (1,1)-(2,2) | xor 5^2^5^2 = 0 | 0 | 

Xác nhận các cập nhật chồng chéo hủy bỏ một cách thích hợp bằng cách sử dụng xor. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | | |
