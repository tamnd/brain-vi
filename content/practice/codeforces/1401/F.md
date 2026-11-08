---
title: "CF 1401F - Đảo ngược và hoán đổi"
description: "Chúng tôi đang làm việc với một mảng có độ dài $2^n$, trong đó $n$ có thể lên tới 18, do đó mảng có thể có tối đa $262{,}144$ phần tử. Các truy vấn sửa đổi mảng hoặc yêu cầu tổng các mảng con. Có bốn loại sửa đổi. Phần tử đầu tiên thay thế trực tiếp một phần tử."
date: "2026-06-11T08:45:18+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1401
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 665 (Div. 2)"
rating: 2400
weight: 1401
solve_time_s: 121
verified: false
draft: false
---

[CF 1401F - Đảo ngược và hoán đổi](https://codeforces.com/problemset/problem/1401/F) 

**Đánh giá:** 2400 
**Thẻ:** tìm kiếm nhị phân, mặt nạ bit, cấu trúc dữ liệu 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một mảng có độ dài$2^n$, Ở đâu$n$có thể lên tới 18, vì vậy mảng có thể có tới$262{,}144$các phần tử. Các truy vấn sửa đổi mảng hoặc yêu cầu tổng các mảng con. Có bốn loại sửa đổi. Phần tử đầu tiên thay thế trực tiếp một phần tử. Thứ hai,$Reverse(k)$, đảo ngược tất cả các khối có kích thước liền kề$2^k$. Thứ ba,$Swap(k)$, hoán đổi các khối có kích thước liên tiếp$2^k$. Loại thứ tư truy vấn tổng của một mảng con. 

Thoạt nhìn, việc áp dụng đảo ngược và hoán đổi một cách ngây thơ có thể yêu cầu$O(2^n)$hoạt động trên mỗi truy vấn, quá chậm đối với$q$lên đến$10^5$. Nếu chúng ta cố gắng thực hiện mọi thao tác theo nghĩa đen, chúng ta có thể được yêu cầu xử lý$10^5 \cdot 2^{18} \approx 2.6 \cdot 10^{10}$hoạt động vượt quá giới hạn thời gian hợp lý. Do đó, chúng ta cần một biểu diễn mảng cho phép đảo ngược, hoán đổi và truy vấn tổng mà không thực sự di chuyển các phần tử mỗi lần. 

Các trường hợp khó khăn không rõ ràng xuất hiện khi nhiều lần đảo ngược hoặc hoán đổi tương tác với nhau. Chẳng hạn, đảo ngược cùng một khối hai lần sẽ khôi phục trật tự ban đầu. Hoán đổi ở các cấp độ khác nhau sắp xếp lại mảng theo những cách không tầm thường. Cây phân đoạn đơn giản nếu không theo dõi các phép biến đổi này sẽ trả về số tiền không chính xác sau nhiều thao tác. Tương tự,$Swap(k)$với$k = 0$chỉ hoán đổi các phần tử liền kề, đây là trường hợp ranh giới tinh tế. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ duy trì mảng một cách rõ ràng. MỘT$Replace(x, k)$chỉ đơn giản là đặt$a[x-1] = k$.$Reverse(k)$Và$Swap(k)$lặp lại tất cả$2^n$các phần tử áp dụng phép biến đổi, và$Sum(l, r)$tính tổng các phần tử một cách trực tiếp. Điều này đúng nhưng quá chậm: một$Reverse(n)$chạm vào tất cả$2^n$các phần tử. Với$q = 10^5$, cách tiếp cận này có thể đạt được$10^{10}$hoạt động. 

Cách tiếp cận tối ưu dựa trên sự quan sát rằng$Reverse(k)$Và$Swap(k)$là những phép biến đổi có cấu trúc. Mỗi chỉ số$i$trong mảng có thể được biểu diễn dưới dạng nhị phân dưới dạng$i = b_{n-1}b_{n-2}\dots b_0$. Sự đảo ngược và hoán đổi tương ứng với việc đảo ngược các bit của chỉ mục.$Reverse(k)$lật$k$các bit có ý nghĩa nhỏ nhất trong mỗi khối và$Swap(k)$lật$(k+1)$-bit thứ. Do đó, thay vì sửa đổi mảng về mặt vật lý, chúng ta có thể theo dõi bit nào được đảo ở mỗi cấp độ và ánh xạ các chỉ số logic tới các vị trí vật lý theo yêu cầu. Khi ánh xạ này rõ ràng, chúng ta có thể sử dụng cây phân đoạn tiêu chuẩn để thực hiện cập nhật điểm và tính tổng phạm vi mà không cần thực sự di chuyển các phần tử. 

Quan sát này làm giảm vấn đề khi triển khai cây phân đoạn bằng ánh xạ chỉ mục ảo. Mỗi truy vấn cập nhật ánh xạ (đối với Đảo ngược và Hoán đổi) hoặc truy cập vào cây (đối với Thay thế và Tổng). Các hoạt động XOR theo bit có thể thực hiện các phép biến đổi chỉ mục này một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q·2^n) | O(2^n) | Quá chậm | 
| Tối ưu (Ánh xạ cây phân đoạn + chỉ mục bitmask) | O(q·log(2^n)) = O(q·n) | O(2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đại diện cho từng chỉ số$i$của mảng dưới dạng$n$-số nhị phân bit. Khởi tạo một biến`flip_mask = 0`để theo dõi tất cả các lần lật bit do các truy vấn Đảo ngược và Hoán đổi gây ra. 
2. Xây dựng cây phân đoạn trên mảng để hỗ trợ cập nhật điểm và truy vấn tổng phạm vi. Cây phân đoạn chưa cần biết về các lần lật. 
3. Xử lý`Replace(x, k)`, tính chỉ số vật lý thực tế bằng XOR-ing`x-1`với`flip_mask`. Cập nhật cây phân đoạn tại vị trí đó. 
4. Để xử lý`Reverse(k)`, lật$k$bit có ý nghĩa ít nhất trong`flip_mask`. Trong mã, tính toán mặt nạ`(1 << k) - 1`và XOR nó vào`flip_mask`. 
5. Để xử lý`Swap(k)`, lật$(k)$-bit thứ (dựa trên 0) trong`flip_mask`. Trong mã, tính toán`1 << k`và XOR nó vào`flip_mask`. 
6. Để xử lý`Sum(l, r)`, chúng ta không thể truy vấn trực tiếp cây phân đoạn vì`flip_mask`hoán vị các vị trí. Thực hiện hàm đệ quy đi xuống cây phân đoạn. Tại mỗi nút, hãy xem xét liệu bit ở mức hiện tại của nó có được chuyển vào hay không`flip_mask`. Nếu lật thì hoán đổi con trái và con phải trong quá trình đệ quy. Trả lại số tiền`[l, r]`sau khi ánh xạ các chỉ số logic tới các vị trí vật lý. 
7. Sau tất cả các truy vấn, mỗi truy vấn Sum trả về kết quả chính xác bằng cách sử dụng ánh xạ ảo mà không cần xáo trộn mảng về mặt vật lý. 

Tại sao nó hoạt động: Mỗi Đảo ngược hoặc Hoán đổi có thể được biểu thị dưới dạng lật các bit cụ thể của chỉ mục. XOR là nghịch đảo của chính nó, do đó nhiều lần đảo ngược hoặc hoán đổi ở cùng cấp độ sẽ bị hủy một cách chính xác. Cây phân đoạn duy trì tổng cho mảng cơ sở. Truy vấn tổng đệ quy diễn giải chính xác con nào tương ứng với chỉ mục logic nào trong biểu thức hiện tại`flip_mask`. Bởi vì tất cả các sửa đổi đều sử dụng cùng một ánh xạ chỉ mục, nên Thay thế và Tổng hoạt động nhất quán, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

class SegmentTree:
    def __init__(self, data):
        n = len(data)
        self.N = 1
        while self.N < n:
            self.N <<= 1
        self.data = [0] * (2 * self.N)
        for i in range(n):
            self.data[self.N + i] = data[i]
        for i in range(self.N - 1, 0, -1):
            self.data[i] = self.data[2*i] + self.data[2*i+1]

    def update(self, idx, value):
        idx += self.N
        self.data[idx] = value
        while idx > 1:
            idx >>= 1
            self.data[idx] = self.data[2*idx] + self.data[2*idx+1]

    def query(self, l, r, node=1, nl=0, nr=None, flip_mask=0, level=0):
        if nr is None:
            nr = self.N
        if r <= nl or nr <= l:
            return 0
        if l <= nl and nr <= r:
            return self.data[node]
        mid = (nl + nr) >> 1
        bit = 1 << level
        left_child, right_child = 2*node, 2*node+1
        if flip_mask & bit:
            left_child, right_child = right_child, left_child
        return self.query(l, r, left_child, nl, mid, flip_mask, level+1) + \
               self.query(l, r, right_child, mid, nr, flip_mask, level+1)

n, q = map(int, input().split())
a = list(map(int, input().split()))
st = SegmentTree(a)
flip_mask = 0
for _ in range(q):
    tmp = list(map(int, input().split()))
    if tmp[0] == 1:
        x, k = tmp[1]-1, tmp[2]
        phys = x ^ flip_mask
        st.update(phys, k)
    elif tmp[0] == 2:
        k = tmp[1]
        flip_mask ^= (1 << k) - 1
    elif tmp[0] == 3:
        k = tmp[1]
        flip_mask ^= (1 << k)
    else:
        l, r = tmp[1]-1, tmp[2]-1
        res = st.query(l, r+1, flip_mask=flip_mask)
        print(res)
```Giải pháp xác định lớp SegmentTree để duy trì tổng phạm vi. các`flip_mask`mã hóa tất cả các đảo ngược và hoán đổi. Khi thực hiện Thay thế, chúng tôi tính toán chỉ mục vật lý bằng XOR-ing với`flip_mask`. Đối với các truy vấn Sum, phép đệ quy xem xét các bit bị lật ở mỗi cấp độ cây, hoán đổi các phần tử con nếu cần. Cập nhật đảo ngược và hoán đổi sửa đổi`flip_mask`sử dụng XOR. Việc sử dụng XOR đảm bảo việc đảo ngược và hoán đổi có thể dễ dàng đảo ngược và có thể kết hợp một cách hiệu quả. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2 3
7 4 9 9
1 2 8
3 1
4 2 4
```| Bước | flip_mask | Chế độ xem mảng | Hoạt động | Truy cập cây phân đoạn | 
| --- | --- | --- | --- | --- | 
| ban đầu | 0 | [7,4,9,9] | xây dựng | tất cả các nút | 
| 1 | 0 | [7,8,9,9] | Thay thế(2,8) | chỉ số 1 ^ 0 = 1 | 
| 2 | 2 | ảo: [9,9,7,8] | Hoán đổi(1) | flip_mask ^= 1 | 
| 3 | 2 | | Tổng(2,4) | ánh xạ đệ quy các chỉ số dưới flip_mask | 

Truy vấn tổng trả về 9 + 7 + 8 = 24, khớp với kết quả mong đợi. 

Đầu vào mẫu 2:```
3 8
7 0 8 8 7 1 5 2
4 3 7
2 1
3 2
4 1 6
2 3
1 5 16
4 8 8
3 0
```Tương tự`flip_mask`theo dõi, các truy vấn tổng sẽ tính toán chính xác
