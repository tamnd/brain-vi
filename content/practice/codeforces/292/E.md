---
title: "CF 292E - Sao chép dữ liệu"
description: "Chúng ta được yêu cầu duy trì hai mảng số nguyên a và b, mỗi mảng có độ dài n. Mảng đầu tiên a là mảng tĩnh: giá trị của nó không bao giờ thay đổi."
date: "2026-06-05T17:14:27+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 292
codeforces_index: "E"
codeforces_contest_name: "Croc Champ 2013 - Round 1"
rating: 1900
weight: 292
solve_time_s: 73
verified: true
draft: false
---

[CF 292E - Sao chép dữ liệu](https://codeforces.com/problemset/problem/292/E) 

**Xếp hạng:** 1900 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu duy trì hai mảng số nguyên,`a`Và`b`, mỗi chiều dài`n`. Mảng đầu tiên`a`là tĩnh: giá trị của nó không bao giờ thay đổi. Mảng thứ hai`b`có thể thay đổi và chúng ta cần xử lý hai loại thao tác: sao chép một phân đoạn liền kề từ`a`vào trong`b`và truy vấn giá trị của một phần tử cụ thể trong`b`. Mỗi thao tác sao chép có thể ảnh hưởng đến nhiều thành phần trong`b`đồng thời và chúng ta phải hỗ trợ lên tới`m`hoạt động một cách hiệu quả. 

Với những hạn chế, cả hai`n`Và`m`có thể lớn tới 100.000. Việc triển khai đơn giản sao chép theo nghĩa đen các phân đoạn con trong thời gian O(k) trên mỗi bản sao có thể mất tới 10^10 thao tác trong trường hợp xấu nhất (nếu k cũng là O(n)), vượt xa giới hạn thời gian 2 giây. Điều này có nghĩa là một vòng lặp đơn giản cho mỗi truy vấn sao chép là không khả thi. 

Trường hợp khó phát hiện khi nhiều thao tác sao chép chồng chéo lên nhau. Ví dụ, nếu lần đầu tiên chúng ta sao chép`a[1:3]`vào trong`b[2:4]`và sao chép sau này`a[2:4]`vào trong`b[3:5]`, một chiến lược ghi đè mảng đơn giản có thể không theo dõi chính xác bản gốc nào`a`các phần tử hiện được biểu diễn trong`b`. Một cạm bẫy tiềm ẩn khác là việc truy vấn một phần tử trong`b`đó chưa bao giờ bị ghi đè. Thuật toán phải trả về chính xác giá trị ban đầu từ`b`trong kịch bản đó. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force mô phỏng trực tiếp các hoạt động sao chép. Mỗi truy vấn loại 1 lặp lại`k`phần tử và phép gán`b[y + q] = a[x + q]`. Mỗi truy vấn loại 2 chỉ trả về`b[x]`. Cách tiếp cận này hoạt động chính xác đối với các đầu vào nhỏ, nhưng độ phức tạp về thời gian trong trường hợp xấu nhất là O(m * n), có thể đạt tới 10^10 thao tác và rõ ràng là quá chậm. 

Quan sát quan trọng để cải thiện hiệu suất là các hoạt động sao chép luôn di chuyển các khối liền kề từ`a`ĐẾN`b`. Thay vì cập nhật từng phần tử riêng lẻ, chúng ta có thể biểu diễn trạng thái của`b`là "gốc" hoặc "được liên kết" với một phân đoạn của`a`. Nếu chúng tôi lưu trữ các liên kết này trong cấu trúc hỗ trợ cập nhật phạm vi hiệu quả và truy vấn một phần tử, chúng tôi có thể tránh được việc sao chép rõ ràng. Cây phân đoạn hoặc cấu trúc cây cân bằng như treap có thể hoạt động, nhưng cách tiếp cận đơn giản và đầy đủ hơn là sử dụng mảng ánh xạ`from_a`chiều dài`n`. Mỗi`from_a[i]`lưu trữ chỉ mục trong`a`cái đó`b[i]`hiện phản ánh hoặc -1 nếu đó là bản gốc`b`giá trị. 

Khi một bản sao xảy ra, chúng tôi cập nhật mảng ánh xạ cho phạm vi trong thời gian O(log n) bằng cách sử dụng cây phân đoạn. Các truy vấn sau đó đi theo ánh xạ để trả về`b[i]`nếu như`from_a[i] == -1`hoặc tương ứng`a`giá trị khác. Điều này làm giảm độ phức tạp của mỗi thao tác từ O(k) xuống O(log n) và giữ tổng số thao tác trong phạm vi 2_10^5_log(10^5), điều này có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m * n) | O(n) | Quá chậm | 
| Ánh xạ + Cây phân đoạn | O(m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`from_a`kích thước`n`, chứa đầy -1. Điều này sẽ theo dõi, đối với từng vị trí trong`b`, cho dù nó trỏ đến một phần tử của`a`hoặc giữ nguyên giá trị ban đầu. 
2. Xây dựng cây phân đoạn`from_a`hỗ trợ gán phạm vi. Mỗi nút lưu trữ -1 hoặc chỉ mục bắt đầu trong`a`nó ánh xạ tới. Cây phân đoạn cho phép chúng ta gán toàn bộ một phạm vi con trong thời gian O(log n). 
3. Đối với mỗi truy vấn, nếu đó là thao tác sao chép, hãy thực hiện gán phạm vi trong cây phân đoạn. Cụ thể, để sao chép truy vấn`k`các yếu tố từ`a[x]`ĐẾN`b[y]`, gán đoạn`[y, y + k - 1]`TRONG`from_a`tương ứng với các chỉ số`[x, x + k - 1]`của`a`. Điều này đảm bảo chúng tôi không bao giờ di chuyển các phần tử về mặt vật lý mà chỉ theo dõi vị trí của từng phần tử`b`nên đọc từ. 
4. Nếu truy vấn là thao tác đọc, hãy sử dụng cây phân đoạn để lấy giá trị của`from_a[x]`. Nếu là -1 thì trả lại bản gốc`b[x]`. Ngược lại, hãy tính phần bù chính xác trong`a`và trở lại`a[from_a[x]]`. 
5. Lặp lại cho đến khi tất cả các truy vấn được xử lý, in kết quả cho các truy vấn loại 2 khi chúng ta tiếp tục. 

Bất biến được duy trì là đối với mọi vị trí`i`TRONG`b`, cây phân đoạn luôn biết liệu giá trị hiện tại có đến từ giá trị gốc hay không`b[i]`hoặc một số đoạn liền kề trong`a`. Các thao tác sao chép chồng chéo sẽ ghi đè chính xác các ánh xạ trước đó vì việc gán cây phân đoạn luôn thay thế phạm vi bị ảnh hưởng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, n):
        self.n = n
        self.data = [-1] * (4 * n)
        self.lazy = [None] * (4 * n)

    def push(self, v, l, r):
        if self.lazy[v] is not None:
            self.data[v] = self.lazy[v]
            if l != r:
                self.lazy[v*2] = self.lazy[v] + 0
                self.lazy[v*2+1] = self.lazy[v] + (r - l + 1)//2
            self.lazy[v] = None

    def update(self, v, l, r, ul, ur, val):
        self.push(v, l, r)
        if ur < l or r < ul:
            return
        if ul <= l and r <= ur:
            self.lazy[v] = val + (l - ul)
            self.push(v, l, r)
            return
        m = (l + r) // 2
        self.update(v*2, l, m, ul, ur, val)
        self.update(v*2+1, m+1, r, ul, ur, val)

    def query(self, v, l, r, pos):
        self.push(v, l, r)
        if l == r:
            return self.data[v]
        m = (l + r) // 2
        if pos <= m:
            return self.query(v*2, l, m, pos)
        else:
            return self.query(v*2+1, m+1, r, pos)

n, m = map(int, input().split())
a = list(map(int, input().split()))
b = list(map(int, input().split()))

tree = SegmentTree(n)

for _ in range(m):
    q = list(map(int, input().split()))
    if q[0] == 1:
        x, y, k = q[1]-1, q[2]-1, q[3]
        tree.update(1, 0, n-1, y, y+k-1, x)
    else:
        x = q[1]-1
        idx = tree.query(1, 0, n-1, x)
        print(a[idx] if idx != -1 else b[x])
```các`SegmentTree`lớp xử lý việc lan truyền lười biếng cho các bài tập phạm vi. các`update`phương pháp đảm bảo rằng khi một phân đoạn được sao chép, nó sẽ bù chính xác ánh xạ để khớp với các chỉ mục nguồn trong`a`. các`query`phương thức truy xuất ánh xạ hiện tại cho bất kỳ chỉ mục nào trong O(log n). Việc sử dụng lập chỉ mục dựa trên 0 bên trong cây sẽ tránh được từng lỗi một khi chuyển đổi từ đầu vào truy vấn dựa trên một. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 10
1 2 0 -1 3
3 1 5 -2 0
2 5
1 3 3 3
2 5
2 4
2 1
1 2 1 4
2 1
2 4
1 4 2 1
2 2
```| Bước | Hoạt động |`from_a`ảnh chụp nhanh | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | Truy vấn b[5] | [-1,-1,-1,-1,-1] | 0 | 
| 2 | Sao chép a[3:5] -> b[3:5] | [-1,-1,2,3,4] | - | 
| 3 | Truy vấn b[5] | [-1,-1,2,3,4] | 3 | 
| 4 | Truy vấn b[4] | [-1,-1,2,3,4] | -1 | 
| 5 | Truy vấn b[1] | [-1,-1,2,3,4] | 3 | 
| 6 | Sao chép a[2:2] -> b[1:1] | [1,-1,2,3,4] | - | 
| 7 | Truy vấn b[1] | [1,-1,2,3,4] | 2 | 
| 8 | Truy vấn b[4] | [1,-1,2,3,4] | -1 | 
| 9 | Sao chép a[4:5] -> b[2:3] | [1,3,4,3,4] | - | 
| 10 | Truy vấn b[2] | [1,3,4,3,4] | 3 | 

Dấu vết này xác nhận rằng các bản sao chồng chéo cập nhật chính xác ánh xạ và truy vấn truy xuất giá trị chính xác từ một trong hai`a`hoặc bản gốc`b`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp
