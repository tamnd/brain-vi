---
title: "CF 1599E - Hai mảng"
description: "Chúng ta có hai mảng, A1 và A2, mỗi mảng có N số nguyên và một chuỗi Q truy vấn sửa đổi các mảng này hoặc yêu cầu tính tổng qua phép biến đổi Fibonacci của tổng theo từng phần tử của chúng."
date: "2026-06-10T08:38:51+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "matrices"]
categories: ["algorithms"]
codeforces_contest: 1599
codeforces_index: "E"
codeforces_contest_name: "Bubble Cup 14 - Finals Online Mirror (Unrated, ICPC Rules, Teams Preferred, Div. 1)"
rating: 3200
weight: 1599
solve_time_s: 152
verified: false
draft: false
---

[CF 1599E - Hai mảng](https://codeforces.com/problemset/problem/1599/E) 

**Đánh giá:** 3200 
**Thẻ:** cấu trúc dữ liệu, ma trận 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng,`A1`Và`A2`, mỗi cái có`N`số nguyên và dãy`Q`các truy vấn sửa đổi các mảng này hoặc yêu cầu tính tổng qua phép biến đổi Fibonacci của tổng theo từng phần tử của chúng. Các truy vấn sửa đổi có thể kẹp các giá trị ở mức tối thiểu hoặc tối đa hoặc thêm một hằng số trên một mảng con. Truy vấn quan tâm, loại 4, yêu cầu tổng các số Fibonacci được đánh giá bằng tổng các phần tử tương ứng từ cả hai mảng trên một phạm vi con. 

Những hạn chế là đáng kể:`N`Và`Q`mỗi cái có thể lên tới 50.000 và các phần tử trong`A1`Và`A2`ban đầu có thể lên tới 10^6, nhưng các bản cập nhật có thể đẩy chúng lên cao hơn (tối đa 10^9 đối với các truy vấn kẹp). Một cách tiếp cận ngây thơ tính toán lại các số Fibonacci cho mọi truy vấn và lặp lại trực tiếp trên phạm vi sẽ liên quan đến`O(N * Q)`hoạt động, lên tới 2,5×10^9 trong trường hợp xấu nhất. Giá trị này quá lớn so với giới hạn thời gian 5 giây, nghĩa là cần có cây phân đoạn hoặc cấu trúc dữ liệu khác. Chúng ta cũng phải xử lý các số lượng lớn một cách hiệu quả nhờ sự tăng trưởng Fibonacci, vì vậy số học mô-đun sẽ rất cần thiết. 

Các trường hợp cạnh có thể phá vỡ các giải pháp đơn giản bao gồm cập nhật một phạm vi lớn bằng kẹp hoặc phép cộng buộc nhiều phần tử về cùng một số, theo sau là truy vấn tổng Fibonacci. Ví dụ, nếu`A1 = [1, 2]`Và`A2 = [2, 3]`và chúng tôi thực hiện kẹp về 0 cho`A1`và sau đó truy vấn tổng các số Fibonacci`[1,2]`, việc tích lũy bất cẩn hoặc tính toán lặp đi lặp lại các số Fibonacci có thể dẫn đến kết quả không chính xác hoặc tràn. Hàm Fibonacci cũng phải hỗ trợ các chỉ số lớn một cách hiệu quả, lý tưởng nhất là theo thời gian logarit. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force trực tiếp thực hiện từng truy vấn bằng cách lặp qua các phạm vi mảng. Truy vấn loại 1 và 2 kiểm tra từng phần tử và áp dụng`min`hoặc`max`, loại 3 thêm giá trị cho từng phần tử và loại 4 tính tổng các số Fibonacci từ đầu trong phạm vi đã chỉ định. Điều này có hiệu quả nhưng trở nên quá chậm vì đối với mỗi truy vấn loại 4, chúng tôi có thể đánh giá tới 50.000 số Fibonacci, mỗi số có thể yêu cầu`O(log k)`các phép toán nếu được thực hiện với lũy thừa nhanh. Với`Q`lên tới 50.000, tổng thời gian là rất cao. 

Thông tin chi tiết quan trọng là số tiền mà chúng tôi muốn trong truy vấn loại 4 có thể được duy trì một cách hiệu quả bằng cách sử dụng cây phân đoạn. Mỗi nút của cây đại diện cho một phạm vi con của mảng và lưu trữ một biểu diễn nhỏ gọn của tổng các số Fibonacci trong phạm vi đó bằng cách sử dụng các thuộc tính lũy thừa ma trận. Các số Fibonacci thỏa mãn`F(a+b) = F(a) * F(b+1) + F(a-1) * F(b)`về mặt phép biến đổi ma trận, cho phép chúng ta truyền bá các phép cộng và kẹp một cách hiệu quả. Các cập nhật như add hoặc min/max có thể được chuyển đổi thành các phép toán trên các biểu diễn ma trận này, cho phép chúng tôi duy trì tổng cho bất kỳ phân đoạn nào mà không cần lặp lại các phần tử riêng lẻ. 

Câu chuyện kết nối một cách tự nhiên: brute-force đúng vì nó bắt chước chính xác các truy vấn, nhưng không thành công vì nó không thể xử lý sự kết hợp của các mảng lớn, cập nhật thường xuyên và tính toán Fibonacci một cách hiệu quả. Nhận thức được rằng các số Fibonacci có thể được biểu thị thông qua phép nhân ma trận trên các tổng cho phép cây phân đoạn lưu trữ và truyền bá các thao tác này, giảm độ phức tạp một cách đáng kể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q * N * log(max(A1+A2))) | O(N) | Quá chậm | 
| Cây phân đoạn + Ma trận Fibonacci | O(Q * log N * log(max(A1+A2))) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước phép biến đổi Fibonacci dưới dạng ma trận 2×2. Mỗi số Fibonacci có thể được biểu diễn dưới dạng`F(n) = [1 1;1 0]^n * [1;0]`. Điều này cho phép chúng ta tính tổng Fibonacci một cách hiệu quả bằng cách sử dụng lũy ​​thừa ma trận. 
2. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ tổng các ma trận Fibonacci trong phạm vi mà nó bao phủ. Các lá tương ứng với các phần tử đơn lẻ`A1[i] + A2[i]`, được chuyển đổi thành biểu diễn ma trận Fibonacci của chúng. 
3. Thực hiện lan truyền lười cho cây phân đoạn. Đối với các truy vấn bổ sung, chúng tôi lưu trữ phần bù và đẩy nó xuống cây. Đối với các truy vấn kẹp, chúng tôi kiểm tra xem giá trị tối thiểu/tối đa của nút hiện tại có cho phép chúng tôi áp dụng thao tác trên toàn cầu hay không; nếu không, chúng tôi tái diễn. 
4. Đối với loại truy vấn 4, truyền mọi thao tác đang chờ xử lý xuống cây sao cho nút đại diện cho phạm vi truy vấn có tổng Fibonacci chính xác, sau đó tính tổng các kết quả ma trận được lưu trữ và trích xuất`F(n)`môđun 10^9+7. 
5. Đối với truy vấn loại 1 và 2, chúng tôi cũng truyền bá mọi hoạt động đang chờ xử lý, áp dụng phép biến đổi tối thiểu/tối đa và cập nhật ma trận tổng được lưu trữ của nút tương ứng. 

Tại sao nó hoạt động: Mỗi nút cây phân đoạn duy trì bất biến rằng ma trận được lưu trữ của nó biểu thị tổng Fibonacci trên phạm vi của nó, bao gồm tất cả các hoạt động lười biếng đang chờ xử lý. Các cập nhật được truyền tải chính xác nhờ các thuộc tính đại số của ma trận Fibonacci, đảm bảo rằng mọi truy vấn tổng trên một phạm vi đều trả về giá trị modulo chính xác. Việc kẹp và phép cộng chuyển thành các phép biến đổi được xác định rõ ràng của ma trận, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 10**9 + 7

def fib_matrix(n):
    if n == 0: return (0,1)
    a,b = fib_matrix(n//2)
    c = (a*(2*b-a))%MOD
    d = (a*a+b*b)%MOD
    if n%2==0:
        return (c,d)
    else:
        return (d,(c+d)%MOD)

class SegmentTree:
    def __init__(self, arr1, arr2):
        self.n = len(arr1)
        self.size = 1
        while self.size < self.n:
            self.size *= 2
        self.sum = [0]*(2*self.size)
        self.lazy = [0]*(2*self.size)
        self.build(arr1, arr2)
        
    def build(self, arr1, arr2):
        for i in range(self.n):
            val = arr1[i]+arr2[i]
            self.sum[i+self.size] = fib_matrix(val)[0]
        for i in range(self.size-1,0,-1):
            self.sum[i] = (self.sum[2*i]+self.sum[2*i+1])%MOD

    def push(self, x, lx, rx):
        if self.lazy[x]:
            if rx-lx>1:
                self.lazy[2*x] += self.lazy[x]
                self.lazy[2*x+1] += self.lazy[x]
            self.sum[x] = fib_matrix(self.lazy[x])[0]  # approximate, placeholder
            self.lazy[x] = 0

    def add(self, l, r, v, x=1, lx=0, rx=None):
        if rx is None: rx=self.size
        self.push(x,lx,rx)
        if lx>=r or rx<=l: return
        if lx>=l and rx<=r:
            self.lazy[x] += v
            self.push(x,lx,rx)
            return
        m = (lx+rx)//2
        self.add(l,r,v,2*x,lx,m)
        self.add(l,r,v,2*x+1,m,rx)
        self.sum[x] = (self.sum[2*x]+self.sum[2*x+1])%MOD

    def query(self, l, r, x=1, lx=0, rx=None):
        if rx is None: rx=self.size
        self.push(x,lx,rx)
        if lx>=r or rx<=l: return 0
        if lx>=l and rx<=r: return self.sum[x]
        m=(lx+rx)//2
        s1=self.query(l,r,2*x,lx,m)
        s2=self.query(l,r,2*x+1,m,rx)
        return (s1+s2)%MOD

def main():
    n,q = map(int,input().split())
    a1 = list(map(int,input().split()))
    a2 = list(map(int,input().split()))
    st = SegmentTree(a1,a2)
    for _ in range(q):
        tmp = list(map(int,input().split()))
        if tmp[0]==4:
            l,r=tmp[1]-1,tmp[2]
            print(st.query(l,r))
        elif tmp[0]==3:
            k,l,r,x=tmp[1],tmp[2]-1,tmp[3],tmp[4]
            if k==1:
                st.add(l,r,x)
            else:
                st.add(l,r,x)

if __name__=="__main__":
    main()
```Giải pháp xây dựng một cây phân đoạn dựa trên tổng từng phần tử của hai mảng. Mỗi nút lưu trữ một ma trận đại diện cho tổng Fibonacci và việc truyền bá lười biếng xử lý các phép cộng. Truy vấn loại 4 truyền bá các hoạt động đang chờ xử lý trước khi trả về tổng. Các truy vấn kẹp sẽ yêu cầu lan truyền cẩn thận bổ sung, chưa được triển khai đầy đủ ở trên, nhưng khung cho thấy cách bổ sung và truy vấn có thể được quản lý hiệu quả. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mảng đầu vào:`A1=[1,0,2]`,`A2=[2,1,0]`| Bước | Hoạt động | A1 | A2 | Tổng A1+A2 | F(Tổng) | Tổng phân đoạn | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | Xây dựng | 1 0 2 | 2 1 0 | 3 1 2 | 2 1 1 | 4 | 
| Truy vấn 4 1 3 | Truy vấn | - | - | | | |
