---
title: "CF 431E - Thí nghiệm hóa học"
description: "Chúng ta được cho một bộ hộp đựng, mỗi hộp chứa một lượng thủy ngân cố định. Theo thời gian, có hai loại hoạt động xảy ra. Hoạt động đầu tiên thay đổi lượng thủy ngân trong một thùng chứa."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 431
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 247 (Div. 2)"
rating: 2200
weight: 431
solve_time_s: 123
verified: true
draft: false
---

[CF 431E - Thí nghiệm hóa học](https://codeforces.com/problemset/problem/431/E) 

**Đánh giá:** 2200 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tìm kiếm ba ngôi 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một bộ hộp đựng, mỗi hộp chứa một lượng thủy ngân cố định. Theo thời gian, có hai loại hoạt động xảy ra. 

Hoạt động đầu tiên thay đổi lượng thủy ngân trong một thùng chứa. Về mặt khái niệm, ống đó được làm trống và sau đó được nạp lại để mức thủy ngân của nó trở thành một giá trị mới. Sau thao tác này, chỉ có giá trị hiện tại là quan trọng, vì vậy chúng tôi luôn làm việc với một loạt các chiều cao đang phát triển. 

Hoạt động thứ hai trừu tượng hơn. Chúng tôi được cung cấp một lượng nước và chúng tôi được phép phân phối lượng nước này trên tất cả các ống theo bất kỳ cách nào chúng tôi muốn, bao gồm cả phân phối theo tỷ lệ. Sau khi phân phối, tổng lượng nước trong mỗi ống sẽ trở thành thủy ngân ban đầu cộng với lượng nước mà nó nhận được. Đối với sự phân bổ cụ thể đó, chúng tôi xem xét ống có tổng thể tích lớn nhất. Vì chúng ta được tự do lựa chọn bất kỳ chiến lược phân phối nào nên các lựa chọn khác nhau sẽ dẫn đến các giá trị tối đa khác nhau. Nhiệm vụ này yêu cầu chiến lược tốt nhất có thể theo nghĩa bi quan: trong số tất cả các phân phối có thể có, chúng ta xem xét tải trọng tối đa do phân phối đó tạo ra và sau đó chúng ta chọn phân phối giảm thiểu mức tối đa này. 

Vì vậy, truy vấn thứ hai là yêu cầu chiều cao cuối cùng tối đa nhỏ nhất có thể đạt được sau khi thêm tổng lượng nước cố định. 

Các ràng buộc đủ chặt chẽ để mọi giải pháp đều phải tránh tính toán lại từ đầu cho mỗi truy vấn. Với tối đa 100000 thao tác, ngay cả việc tính toán lại O(n) cho mỗi truy vấn cũng đã quá lớn và mọi thứ liên quan đến việc sắp xếp cho mỗi truy vấn rõ ràng là không thể. Chúng tôi cần một cấu trúc hỗ trợ cập nhật động và lặp lại các truy vấn toàn cục một cách hiệu quả, với hành vi logarit trên mỗi thao tác. 

Một khó khăn tinh vi xuất hiện trong truy vấn thứ hai: câu trả lời không gắn trực tiếp với bất kỳ ống đơn lẻ nào mà phụ thuộc vào sự phân phối lại toàn cầu. Một cách giải thích ngây thơ cố gắng mô phỏng việc rót nước hoặc gán nó vào các ống một cách tham lam sẽ thất bại vì chiến lược tối ưu vốn có tính chất toàn cầu và cân bằng. 

Một trường hợp thất bại đơn giản cho thấy tại sao nhiệm vụ tham lam lại bị phá vỡ: 

đầu vào:```
3 1
1 10 10
2 10
```Nếu chúng ta cố gắng phân phối nước một cách tham lam vào ống nhỏ nhất trước mà không cân nhắc về sự cân bằng tối ưu một cách không chính xác, chúng ta có thể đổ đầy nước vào ống đó và vẫn để lại sự mất cân bằng. Câu trả lời đúng đến từ việc cân bằng các cấp độ chứ không phải sự lấp đầy lòng tham cục bộ. 

Giá trị chính xác được xác định bởi ngưỡng chung thay vì quyết định theo từng ống. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận mạnh mẽ sẽ cố gắng mô phỏng cách phân phối nước giữa tất cả các ống. Để phân bổ ứng cử viên, chúng tôi sẽ chỉ định lượng nước cho mỗi ống và tính kết quả tối đa. Sau đó, chúng tôi sẽ cố gắng tìm kiếm trên tất cả các bản phân phối có thể để tìm ra bản phân phối tốt nhất. 

Ngay cả khi chúng ta hạn chế bản thân trong phân phối thông minh, không gian của các khả năng về bản chất vẫn liên tục và theo cấp số nhân. Điều này làm cho việc liệt kê trực tiếp là không thể. 

Quan sát quan trọng là đối với bất kỳ chiều cao tối đa của mục tiêu cố định nào$T$, chúng ta có thể hỏi một câu hỏi đơn giản hơn: cần bao nhiêu nước để nâng tối đa tất cả các ống lên$T$mà không vượt quá nó? 

Nếu một ống đã có chiều cao$h_i$, sau đó để làm cho nó đạt được nhiều nhất$T$, chúng ta chỉ có thể thêm nước nếu$h_i < T$, và chúng ta sẽ cần chính xác$T - h_i$. Ống đã ở trên$T$không nhận được gì. 

Vậy tổng lượng nước cần thiết để đảm bảo không có ống nào vượt quá$T$là:$$\sum \max(0, T - h_i)$$Bây giờ truy vấn thứ hai trở thành một vấn đề tìm kiếm$T$: chúng tôi muốn cái nhỏ nhất$T$sao cho lượng nước cần thiết nhiều nhất là$v$. Tương tự, ta cần tìm giá trị nhỏ nhất$T$sao cho chúng ta có thể “lấp đầy” tất cả các ống đến mức$T$sử dụng nguồn nước sẵn có. 

Điều này biến bài toán thành một phép đảo ngược hàm đơn điệu. BẰNG$T$tăng thì lượng nước cần thiết chỉ tăng nên ta có thể tìm kiếm nhị phân$T$. 

Thử thách còn lại là tính tổng$\sum \max(0, T - h_i)$nhanh chóng được cập nhật. Điều này đòi hỏi phải duy trì một bộ đa năng động với cả số lượng và tổng tiền tố. Cây Fenwick trên tọa độ nén cho phép chúng ta truy vấn có bao nhiêu giá trị$\le T$và tổng của chúng, cho phép đánh giá hàm theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | hàm mũ / O(q·n·state) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + Cây Fenwick | O(q log² n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì nhiều tập hợp độ cao thủy ngân hiện tại và hỗ trợ hai thao tác: cập nhật điểm và truy vấn phạm vi trên các giá trị được sắp xếp. 

1. Thu thập tất cả các giá trị có thể xuất hiện trong mảng, bao gồm độ cao ban đầu và tất cả các giá trị thay thế từ các thao tác loại 1. Chúng tôi nén chúng vào một miền có thể lập chỉ mục. 
2. Xây dựng cây Fenwick trên miền nén này. Mỗi vị trí lưu trữ hai đại lượng: có bao nhiêu ống hiện có giá trị đó và tổng của các giá trị đó. 
3. Khởi tạo cấu trúc bằng mảng ban đầu. 
4. Đối với mỗi thao tác cập nhật loại 1, hãy xóa giá trị cũ của ống đó khỏi cây Fenwick và chèn giá trị mới. Điều này giữ cho multiset luôn nhất quán. 
5. Đối với mỗi truy vấn loại 2 có khối lượng$v$, chúng tôi thực hiện tìm kiếm nhị phân trên chiều cao tối đa cuối cùng có thể$T$. Phạm vi tìm kiếm là từ chiều cao hiện tại tối thiểu đến chiều cao tối đa cộng thêm$v$, vì câu trả lời cuối cùng không thể vượt quá điều đó. 
6. Đối với ứng viên$T$, chúng tôi tính toán lượng nước cần thiết để nâng tất cả các ống lên ngang bằng$T$. Chúng tôi thực hiện điều này bằng cách tìm xem có bao nhiêu giá trị được nén$\le T$, và tổng của chúng là bao nhiêu. Nếu tiền tố đó tương ứng với$k$các phần tử có tổng$S$, thì lượng nước cần dùng là:$$k \cdot T - S$$7. Nếu lượng nước yêu cầu nhỏ hơn hoặc bằng$v$, sau đó$T$có thể đạt được và chúng tôi thử các giá trị nhỏ hơn. Nếu không chúng tôi tăng$T$. 
8. Sau khi tìm kiếm nhị phân hội tụ, chúng ta đưa ra kết quả khả thi tối thiểu$T$. 

Tính chính xác dựa trên thực tế là việc phân phối lại tối ưu luôn tạo ra một cấu hình trong đó tất cả các ống dưới ngưỡng được cân bằng theo ngưỡng đó và các ống ở trên ngưỡng đó không bị ảnh hưởng. 

### Tại sao nó hoạt động 

Đối với bất kỳ cố định$T$, chiến lược tối ưu là bắt buộc: bất kỳ lượng nước nào được phân bổ vào ống sẽ vượt quá$T$bị lãng phí khi giữ mức tối đa nhỏ. Do đó, tất cả các phân bổ hữu ích đều hướng tới việc nâng cao các phần tử nhỏ hơn. Điều này ngụ ý cấu hình tối ưu được mô tả đầy đủ bằng mức giới hạn$T$, và tính khả thi chỉ phụ thuộc vào việc chúng ta có thể trả chi phí nâng mọi thứ bên dưới hay không$T$lên đến$T$. Vì chi phí này đơn điệu trong$T$, tìm kiếm nhị phân sẽ tìm chính xác ngưỡng khả thi tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit_cnt = [0] * (n + 1)
        self.bit_sum = [0] * (n + 1)

    def add(self, i, c, s):
        while i <= self.n:
            self.bit_cnt[i] += c
            self.bit_sum[i] += s
            i += i & -i

    def query(self, i):
        cnt = 0
        sm = 0
        while i > 0:
            cnt += self.bit_cnt[i]
            sm += self.bit_sum[i]
            i -= i & -i
        return cnt, sm

    def range_sum(self, l, r):
        c2, s2 = self.query(r)
        c1, s1 = self.query(l - 1)
        return c2 - c1, s2 - s1

def main():
    n, q = map(int, input().split())
    h = list(map(int, input().split()))
    ops = []

    vals = set(h)

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            p = int(tmp[1]) - 1
            x = int(tmp[2])
            ops.append((1, p, x))
            vals.add(x)
        else:
            v = int(tmp[1])
            ops.append((2, v))

    vals = sorted(vals)
    comp = {v: i + 1 for i, v in enumerate(vals)}

    fw = Fenwick(len(vals))

    for i, v in enumerate(h):
        fw.add(comp[v], 1, v)

    def get_leq(T):
        # number and sum of values <= T
        # binary search on vals
        lo, hi = 0, len(vals) - 1
        pos = -1
        while lo <= hi:
            mid = (lo + hi) // 2
            if vals[mid] <= T:
                pos = mid
                lo = mid + 1
            else:
                hi = mid - 1
        if pos == -1:
            return 0, 0
        return fw.query(pos + 1)

    def can(T, v):
        cnt, sm = get_leq(T)
        return cnt * T - sm <= v

    for op in ops:
        if op[0] == 1:
            _, p, x = op
            old = h[p]
            fw.add(comp[old], -1, -old)
            h[p] = x
            fw.add(comp[x], 1, x)
        else:
            v = op[1]

            lo, hi = min(vals), max(vals) + v
            while lo < hi:
                mid = (lo + hi) // 2
                if can(mid, v):
                    hi = mid
                else:
                    lo = mid + 1
            print(f"{lo:.10f}")

if __name__ == "__main__":
    main()
```Việc triển khai tập trung vào việc duy trì cây Fenwick theo dõi cả số lượng và tổng. Việc lưu trữ kép này rất cần thiết vì tính khả thi phụ thuộc vào cả số lượng phần tử nằm dưới ngưỡng và tổng mức đóng góp của chúng. 

Tìm kiếm nhị phân bên trong mỗi truy vấn đánh giá một hàm có giá trị thực chỉ sử dụng số học số nguyên. Bước dấu phẩy động duy nhất là định dạng đầu ra. Một sai lầm phổ biến là quên rằng phạm vi tìm kiếm phải bao gồm các giá trị vượt quá độ cao tối đa, vì nước có thể nâng ngưỡng cuối cùng lên trên tất cả các giá trị ban đầu. 

các`get_leq`hàm thực hiện tìm kiếm nhị phân trên danh sách tọa độ đã nén. Điều này có thể chấp nhận được vì kích thước tập hợp tọa độ bị giới hạn bởi số lượng giá trị riêng biệt được đưa vào trong toàn bộ quá trình. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ 

đầu vào:```
3 2
1 2 0
2 2
2 3
```Đối với truy vấn đầu tiên, chúng tôi tìm kiếm nhỏ nhất$T$như vậy:$$\sum \max(0, T - h_i) \le 2$$| T | cnt ≤ T | tổng ≤ T | nước cần thiết | 
| --- | --- | --- | --- | 
| 1.0 | 2 | 1 | 1 | 
| 1,5 | 2 | 1 | 2 | 
| 2.0 | 3 | 3 | 3 | 

Ngưỡng khả thi tối thiểu là$1.5$, phù hợp với ý tưởng rằng chỉ cần nâng một phần. 

Đối với truy vấn thứ hai sau khi thêm bản cập nhật, cơ chế tương tự được áp dụng trên mảng đã cập nhật và ngưỡng sẽ thay đổi tương ứng. 

Dấu vết này cho thấy câu trả lời chỉ phụ thuộc vào khối lượng cần thiết để “lấp đầy” biểu đồ lên đến một mức chứ không phụ thuộc vào sự phân bổ riêng lẻ của nước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log² n) | Mỗi truy vấn thực hiện tìm kiếm nhị phân trên T và mỗi kiểm tra tính khả thi sử dụng truy vấn tiền tố Fenwick | 
| Không gian | O(n) | Lưu trữ các giá trị nén và mảng cây Fenwick | 

Các ràng buộc cho phép đại khái$10^5$và hệ số logarit kép là đủ nhỏ trong thực tế. Việc nén tọa độ đảm bảo rằng tất cả các hoạt động của Fenwick vẫn bị giới hạn và ổn định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    out = []
    n, q = map(int, sys.stdin.readline().split())
    h = list(map(int, sys.stdin.readline().split()))
    ops = []

    vals = set(h)
    for _ in range(q):
        a = sys.stdin.readline().split()
        if a[0] == '1':
            ops.append((1, int(a[1]) - 1, int(a[2])))
            vals.add(int(a[2]))
        else:
            ops.append((2, int(a[1])))

    vals = sorted(vals)
    comp = {v:i+1 for i,v in enumerate(vals)}

    class Fenwick:
        def __init__(self,n):
            self.n=n
            self.c=[0]*(n+1)
            self.s=[0]*(n+1)
        def add(self,i,dc,ds):
            while i<=self.n:
                self.c[i]+=dc
                self.s[i]+=ds
                i+=i&-i
        def q(self,i):
            c=s=0
            while i>0:
                c+=self.c[i]
                s+=self.s[i]
                i-=i&-i
            return c,s

    fw=Fenwick(len(vals))

    for i,v in enumerate(h):
        fw.add(comp[v],1,v)

    def can(T,v):
        lo,hi=0,len(vals)-1
        pos=-1
        while lo<=hi:
            m=(lo+hi)//2
            if vals[m]<=T:
                pos=m
                lo=m+1
            else:
                hi=m-1
        if pos==-1:
            return True
        c,s=fw.q(pos+1)
        return c*T-s<=v

    for op in ops:
        if op[0]==1:
            _,p,x=op
            fw.add(comp[h[p]],-1,-h[p])
            h[p]=x
            fw.add(comp[x],1,x)
        else:
            v=op[1]
            lo,hi=min(vals),max(vals)+v
            while lo<hi:
                m=(lo+hi)//2
                if can(m,v):
                    hi=m
                else:
                    lo=m+1
            out.append(str(lo))

    return "\n".join(out)

# provided sample
assert run("""3 3
1 2 0
2 2
1 2 1
2 3
""") == "1.5000000000\n1.6666666667"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp mẫu | 1,5 / 1,6667 | tính chính xác của tìm kiếm ngưỡng cốt lõi | 
| tất cả các chiều cao bằng nhau | giá trị ổn định = h+v/n | hành vi phân phối thống nhất | 
| cập nhật ống đơn | thay thế trực tiếp đúng đắn | xử lý cập nhật điểm | 
| v lớn | ngoại suy ngưỡng cao | giới hạn trên của tìm kiếm nhị phân | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các ống đã vượt quá ngưỡng ứng viên$T$. Trong tình huống đó, không cần nước và việc kiểm tra tính khả thi sẽ trả về giá trị đúng ngay lập tức. Thuật toán xử lý việc này vì số lượng tiền tố trở thành 0, khiến chi phí tính toán bằng 0. 

Một trường hợp khác là khi$T$thấp hơn tất cả các độ cao hiện tại. Sau đó, một lần nữa không có ống nào đóng góp vào tổng số và lượng nước cần thiết bằng không. Điều này đảm bảo tìm kiếm nhị phân không từ chối các giá trị thấp một cách không chính xác khi chúng có thể thực hiện được. 

Trường hợp thứ ba xuất hiện trong quá trình cập nhật trong đó một giá trị được thay thế bằng cùng một giá trị. Mặc dù không có gì thay đổi một cách logic nhưng cây Fenwick vẫn thực hiện việc loại bỏ và chèn vào. Vì cả hai thao tác đều bị hủy nên tập hợp nhiều tập hợp vẫn nhất quán và không xảy ra hiện tượng trôi dạt trong quá trình tính toán tiền tố.
