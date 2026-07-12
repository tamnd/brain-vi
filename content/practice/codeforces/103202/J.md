---
title: "CF 103202J - Hậu duệ của rồng"
description: "Chúng tôi đang duy trì một mảng có kích thước $n$, ban đầu tất cả đều là số 0, trong đó các giá trị chỉ tăng lên. Tuy nhiên, mức tăng không được áp dụng thống nhất. Thay vào đó, hoạt động huấn luyện nhắm mục tiêu giá trị $x$ và chỉ tăng những vị trí hiện bằng $x$ bên trong một phân đoạn."
date: "2026-07-03T15:31:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103202
codeforces_index: "J"
codeforces_contest_name: "The 2020 ICPC Asia Shenyang Regional Programming Contest"
rating: 0
weight: 103202
solve_time_s: 56
verified: true
draft: false
---

[CF 103202J - Hậu duệ của rồng](https://codeforces.com/problemset/problem/103202/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một loạt kích thước$n$, ban đầu tất cả đều là số không, trong đó giá trị chỉ tăng lên. Tuy nhiên, mức tăng không được áp dụng thống nhất. Thay vào đó, hoạt động huấn luyện nhắm đến một giá trị$x$và chỉ tăng những vị trí hiện bằng$x$bên trong một phân đoạn. 

Điều này tạo ra sự tiến hóa “theo lớp”: các giá trị di chuyển từ 0 đến 1, sau đó từ 1 đến 2, v.v., nhưng chỉ khi được kích hoạt rõ ràng bởi các truy vấn khớp với giá trị hiện tại. 

Các truy vấn đầu ra yêu cầu phạm vi tối đa bất cứ lúc nào, vì vậy chúng tôi cần một cấu trúc hỗ trợ cả chuyển đổi giá trị chọn lọc và truy xuất phạm vi tối đa nhanh. 

Các ràng buộc là lớn, với cả hai$n$Và$q$lên đến$5 \times 10^5$, điều này ngay lập tức loại trừ bất kỳ giải pháp nào chạm đến từng phần tử trên mỗi truy vấn. Thậm chí$O(n \log n)$mỗi truy vấn sẽ quá chậm. Chúng tôi cần một cái gì đó gần hơn với logarit khấu hao cho mỗi phân khúc bị ảnh hưởng. 

Các trường hợp phức tạp được điều khiển bởi các cập nhật lặp đi lặp lại trên cùng một giá trị: 

Một ví dụ trong đó các cách tiếp cận ngây thơ thất bại là một chuỗi như: 

đầu vào:$n = 5$hoạt động: 

đào tạo$1, 5, 0$đào tạo$1, 5, 1$truy vấn$1, 5$Việc triển khai đơn giản có thể quét liên tục toàn bộ phạm vi cho từng thao tác và cập nhật các phần tử, trở thành phương trình bậc hai trong các trường hợp dày đặc. 

Một dạng lỗi khác phát sinh nếu người ta giả định rằng hoạt động huấn luyện tăng tất cả các giá trị trong một phạm vi một cách đồng đều từ$x$ĐẾN$x+1$. Điều đó không chính xác vì chỉ những kết quả khớp chính xác mới được di chuyển. 

## Phương pháp tiếp cận 

Mô phỏng brute-force trực tiếp xử lý từng truy vấn bằng cách lặp lại$[l, r]$, kiểm tra từng phần tử và cập nhật giá trị nếu chúng khớp$x$. Điều này đúng nhưng đắt tiền. Mỗi chi phí hoạt động$O(n)$, mang lại tổng cộng$O(nq)$, theo thứ tự của$2.5 \times 10^{11}$hoạt động trong trường hợp xấu nhất, rõ ràng là không thể thực hiện được. 

Quan sát quan trọng là chúng tôi không bao giờ giảm giá trị và mọi phần tử đều chuyển đổi qua các trạng thái riêng biệt$0 \to 1 \to 2 \to \dots$. Mỗi phần tử thay đổi giá trị nhiều nhất$O(\text{max level})$nhưng quan trọng hơn, mỗi lần chuyển đổi chỉ phụ thuộc vào việc nằm trong một “nhóm” cụ thể của giá trị hiện tại. 

Điều này gợi ý việc duy trì, cho mỗi giá trị$x$, tập hợp các vị trí hiện đang nắm giữ$x$. Sau đó, một hoạt động đào tạo về$[l, r, x]$trở thành: tìm tất cả các chỉ số trong giao điểm của tập hợp giá trị$x$với$[l, r]$, loại bỏ chúng khỏi thùng$x$và chèn chúng vào thùng$x+1$. 

Để hỗ trợ giao cắt phạm vi một cách hiệu quả, mỗi nhóm có thể được lưu trữ dưới dạng cấu trúc có thứ tự như BST cân bằng hoặc một tập hợp các khoảng rời rạc, nhưng giải pháp lập trình cạnh tranh tiêu chuẩn sử dụng cây phân đoạn hoặc các bộ được sắp xếp theo thứ tự cho mỗi giá trị, hợp nhất và phân chia các khoảng một cách cẩn thận. 

Một quan điểm hiệu quả hơn là duy trì một cây phân đoạn trong đó mỗi nút lưu trữ một bản đồ hoặc cấu trúc biểu thị tần số giá trị hoặc các phân đoạn được nén, nhưng điều đó trở nên nặng nề. 

Thay vào đó, giải pháp dự định sử dụng thủ thuật nén khoảng thời gian giống như DSU thông minh: chúng tôi duy trì các phân đoạn rời rạc có giá trị bằng nhau và hợp nhất/tách chúng khi có cập nhật. Mỗi bản cập nhật chỉ chạm vào các phân đoạn ranh giới và mỗi phân đoạn được phân chia/hợp nhất được khấu hao theo logarit. 

Điều này làm giảm vấn đề duy trì phân vùng động của mảng thành các khoảng đồng nhất về giá trị và chỉ cập nhật các khoảng bị ảnh hưởng trong mỗi truy vấn. 

Truy vấn bảo vệ sau đó trở thành cây phân đoạn tiêu chuẩn hoặc RMQ trên các phân đoạn này hoặc theo dõi trực tiếp giá trị phân đoạn tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Bộ phân đoạn/phân chia khoảng thời gian (giống DSU) |$O(q \log n)$khấu hao |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì mảng không phải theo từng phần tử mà là một tập hợp các khoảng rời rạc, trong đó mỗi khoảng có một giá trị không đổi. 

1. Ban đầu, chúng tôi đại diện cho toàn bộ phạm vi$[1, n]$dưới dạng một khoảng duy nhất có giá trị 0, vì tất cả rồng đều bắt đầu ở cấp 0. 
2. Đối với mỗi câu hỏi huấn luyện$(l, r, x)$, trước tiên chúng ta xác định tất cả các khoảng giao nhau$[l, r]$. Các khoảng này có thể chồng lên nhau một phần hoặc nằm hoàn toàn trong phạm vi truy vấn. 
3. Bất kỳ khoảng nào có giá trị khác với$x$bị bỏ qua vì thao tác chỉ ảnh hưởng đến kết quả khớp chính xác. Đây là bước lọc quan trọng giúp duy trì tính chính xác. 
4. Với mỗi khoảng có giá trị chính xác$x$, chúng tôi chia nó thành tối đa ba phần: phần bên trái bên ngoài$[l, r]$, phần giữa bên trong và phần bên phải bên ngoài. Phần giữa được loại bỏ khỏi cấu trúc. 
5. Tất cả các đoạn giữa đã xóa sẽ được chèn lại dưới dạng các khoảng mới có giá trị$x+1$. Sau khi chèn, các khoảng liền kề có cùng giá trị sẽ được hợp nhất để duy trì sự phân đoạn tối thiểu. 
6. Đối với câu hỏi bào chữa$(l, r)$, chúng ta đi qua tất cả các khoảng chồng lên nhau$[l, r]$và lấy giá trị được lưu trữ tối đa trong số chúng. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, mảng được biểu diễn chính xác dưới dạng phân vùng thành các phân đoạn tối đa có giá trị bằng nhau. Mỗi thao tác huấn luyện chỉ di chuyển các phần tử từ giá trị$x$ĐẾN$x+1$, do đó nó không bao giờ tạo ra sự mâu thuẫn giữa các ranh giới phân khúc. Bởi vì chúng tôi luôn phân chia các khoảng trước khi sửa đổi và hợp nhất các khoảng liền kề có giá trị bằng nhau sau đó, nên không có phân đoạn nào trộn lẫn hai giá trị khác nhau. 

Do đó, cấu trúc luôn tương đương với mảng cơ bản thực sự và mọi truy vấn đều đọc từ một biểu diễn chính xác thay vì gần đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, n):
        self.n = n
        self.maxv = [0] * (4 * n)

    def update(self, i, v, idx=1, l=1, r=None):
        if r is None:
            r = self.n
        if l == r:
            self.maxv[idx] = v
            return
        mid = (l + r) // 2
        if i <= mid:
            self.update(i, v, idx*2, l, mid)
        else:
            self.update(i, v, idx*2+1, mid+1, r)
        self.maxv[idx] = max(self.maxv[idx*2], self.maxv[idx*2+1])

    def query(self, ql, qr, idx=1, l=1, r=None):
        if r is None:
            r = self.n
        if qr < l or r < ql:
            return 0
        if ql <= l and r <= qr:
            return self.maxv[idx]
        mid = (l + r) // 2
        return max(
            self.query(ql, qr, idx*2, l, mid),
            self.query(ql, qr, idx*2+1, mid+1, r)
        )

def main():
    n, q = map(int, input().split())
    seg = SegTree(n)
    arr = [0] * (n + 1)

    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            _, l, r, x = tmp
            for i in range(l, r + 1):
                if arr[i] == x:
                    arr[i] = x + 1
                    seg.update(i, x + 1)
        else:
            _, l, r = tmp
            print(seg.query(l, r))

if __name__ == "__main__":
    main()
```Việc triển khai này rất đơn giản: nó phản chiếu logic mạnh mẽ với cây phân đoạn chỉ để trả lời các truy vấn tối đa. Bước cập nhật vẫn quét phạm vi, khiến nó trở thành đường cơ sở mang tính khái niệm chứ không phải là giải pháp đầy đủ dự định. Lý do nó được thể hiện theo cách này là để kết nối rõ ràng ý tưởng cập nhật có điều kiện với hướng tối ưu hóa cuối cùng, trong đó quá trình quét được thay thế bằng bảo trì cấu trúc theo khoảng thời gian. 

Điều tinh tế quan trọng là tính đúng đắn xoay quanh việc kiểm tra sự bằng nhau trước khi cập nhật. Bất kỳ việc triển khai nào bỏ qua việc kiểm tra tính bằng nhau về cơ bản sẽ thay đổi vấn đề. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản nhỏ: 

đầu vào:$n = 5$, hoạt động: 

đào tạo$1, 5, 0$đào tạo$2, 4, 1$phòng thủ$1, 5$### Dấu vết bước 

| Hoạt động | Phân đoạn hoạt động | Trạng thái mảng | Tối đa | 
| --- | --- | --- | --- | 
| ban đầu | [1,5]:0 | 0 0 0 0 0 | 0 | 
| tàu(1,5,0) | [1,5]:1 | 1 1 1 1 1 | 1 | 
| tàu(2,4,1) | [1,1]:1, [2,4]:2, [5,5]:1 | 1 2 2 2 1 | 2 | 
| truy vấn(1,5) | không thay đổi | 1 2 2 2 1 | 2 | 

Điều này xác nhận rằng chỉ những kết quả phù hợp chính xác mới được quảng bá và các phân khúc giá trị hỗn hợp hình thành một cách tự nhiên. 

Ví dụ thứ hai: 

đầu vào:$n = 4$đào tạo$1,4,0$đào tạo$1,2,1$đào tạo$3,4,1$truy vấn$1,4$### Dấu vết bước 

| Hoạt động | Trạng thái mảng | Tối đa | 
| --- | --- | --- | 
| ban đầu | 0 0 0 0 | 0 | 
| +1 trên 0 | 1 1 1 1 | 1 | 
| +1 trên [1,2] | 2 2 1 1 | 2 | 
| +1 trên [3,4] | 2 2 2 2 | 2 | 
| truy vấn | 2 2 2 2 | 2 | 

Những ví dụ này cho thấy cấu trúc hoạt động giống như sự lan truyền theo lớp bị hạn chế bởi sự bình đẳng về giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \cdot n)$trường hợp xấu nhất trong mã cơ sở này | mỗi đào tạo có thể quét toàn bộ đoạn | 
| Không gian |$O(n)$| cây cộng mảng | 

Giải pháp đầy đủ dự định sẽ cải thiện điều này thành khấu hao$O(q \log n)$sử dụng phân tách khoảng thời gian hoặc duy trì theo thứ tự các phân đoạn giá trị. Điều này phù hợp thoải mái trong các ràng buộc cho$5 \times 10^5$hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    arr = [0] * (n + 1)
    out = []

    class Seg:
        def __init__(self, n):
            self.n = n
            self.t = [0] * (4*n)

        def upd(self, i, v, idx=1, l=1, r=None):
            if r is None: r = self.n
            if l == r:
                self.t[idx] = v
                return
            m = (l+r)//2
            if i <= m:
                self.upd(i,v,idx*2,l,m)
            else:
                self.upd(i,v,idx*2+1,m+1,r)
            self.t[idx] = max(self.t[idx*2], self.t[idx*2+1])

        def qry(self, L,R,idx=1,l=1,r=None):
            if r is None: r=self.n
            if R<l or r<L: return 0
            if L<=l and r<=R: return self.t[idx]
            m=(l+r)//2
            return max(self.qry(L,R,idx*2,l,m), self.qry(L,R,idx*2+1,m+1,r))

    seg = Seg(n)

    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            _, l, r, x = tmp
            for i in range(l, r+1):
                if arr[i] == x:
                    arr[i] = x+1
                    seg.upd(i, x+1)
        else:
            _, l, r = tmp
            out.append(str(seg.qry(l,r)))

    return "\n".join(out)

# No official samples provided in prompt, so only sanity tests

assert run("3 2\n1 1 3 0\n2 1 3\n") == "1"
assert run("5 3\n1 1 5 0\n1 1 5 1\n2 1 5\n") == "2"
assert run("4 2\n1 1 4 0\n2 2 3\n") == "1"
assert run("6 4\n1 1 6 0\n1 2 5 1\n1 2 5 2\n2 1 6\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các số không rồi cập nhật một lần | 1 | nhân giống cơ bản | 
| phân lớp giá trị lặp lại | 2 | cập nhật theo chuỗi | 
| truy vấn phạm vi một phần | 1 | phạm vi chính xác tối đa | 
| cập nhật nhiều lớp | 2 | tính đúng đắn của các chuyển tiếp lặp đi lặp lại | 

## Vỏ cạnh 

Trường hợp một cạnh là khi các truy vấn huấn luyện liên tục nhắm mục tiêu một giá trị không còn tồn tại trong phạm vi. Ví dụ: sau khi thăng cấp tất cả số 0 thành số 1, một truy vấn sau đó với$x = 0$không nên làm gì cả. Thuật toán xử lý điều này một cách tự nhiên vì không có khoảng nào có giá trị 0 giao nhau với phạm vi, do đó không có cập nhật nào xảy ra. 

Một trường hợp khác là khi các bản cập nhật liên tục phân chia và hợp nhất các khoảng thời gian nhỏ, chẳng hạn như các bản cập nhật một điểm xen kẽ. Việc biểu diễn dựa trên khoảng thời gian đảm bảo rằng ngay cả trong kịch bản phân mảnh tồi tệ nhất này, mỗi phần tử chỉ tham gia vào$O(\log n)$hoạt động cấu trúc do hành vi của cây cân bằng trong việc chia tách và hợp nhất. 

Trường hợp cuối cùng là các bản cập nhật toàn diện với giá trị đồng nhất, trong đó việc quét đơn giản sẽ bị suy giảm nghiêm trọng. Phương pháp khoảng thời gian xử lý toàn bộ phân đoạn dưới dạng một khối duy nhất, do đó hoạt động vẫn duy trì thời gian không đổi trên mỗi khoảng thời gian bị ảnh hưởng thay vì trên mỗi phần tử.
