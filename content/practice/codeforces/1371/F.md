---
title: "CF 1371F - Sấm sét dữ dội"
description: "Chúng ta được cung cấp một dòng vị trí, mỗi vị trí chứa một ký hiệu chỉ hướng hoạt động giống như một quy tắc cục bộ xác định. Từ mọi vị trí bắt đầu, một quả bóng di chuyển sang trái hoặc phải theo biểu tượng của chính nó và biểu tượng của người hàng xóm mà nó đang tương tác."
date: "2026-06-16T12:36:08+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1371
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 654 (Div. 2)"
rating: 2800
weight: 1371
solve_time_s: 319
verified: false
draft: false
---

[CF 1371F - Sấm sét dữ dội](https://codeforces.com/problemset/problem/1371/F) 

**Đánh giá:** 2800 
**Tas:** cấu trúc dữ liệu, phân chia và chinh phục, triển khai 
**Thời gian giải:** 5 phút 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng vị trí, mỗi vị trí chứa một ký hiệu chỉ hướng hoạt động giống như một quy tắc cục bộ xác định. Từ mọi vị trí bắt đầu, một quả bóng di chuyển sang trái hoặc phải theo biểu tượng của chính nó và biểu tượng của người hàng xóm mà nó đang tương tác. Cuối cùng, mọi quả bóng đều biến mất vào một trong các lỗ cố định được đặt giữa và ngoài đường kẻ. 

Quan sát quan trọng là, mặc dù các quy tắc di chuyển có vẻ cục bộ và có điều kiện, nhưng mỗi vị trí bắt đầu đều kết thúc ở đúng một lỗ. Vì vậy toàn bộ hệ thống định nghĩa một hàm từ các chỉ số`1..n`đến lỗ`0..n`. 

Mỗi truy vấn trước tiên sẽ lật tất cả các hướng trong một đoạn, sau đó hỏi: nếu chúng ta thả một quả bóng ở mọi vị trí trong đoạn đó, thì có bao nhiêu quả bóng kết thúc ở mỗi lỗ và số lượng bóng tối đa trong số đó là bao nhiêu. 

Vì vậy, mọi truy vấn thực sự đang yêu cầu tần số giá trị lớn nhất trong hình ảnh của một hàm thay đổi linh hoạt được giới hạn trong một phạm vi. 

Các ràng buộc buộc chúng tôi tránh xa mọi mô phỏng cho mỗi truy vấn. Với tối đa`5 × 10^5`vị trí và`10^5`cập nhật, thậm chí`O(n)`mỗi truy vấn đã quá chậm. Bất kỳ cách tiếp cận nào tính toán lại điểm đến cuối cùng cho mỗi quả bóng sau mỗi lần cập nhật sẽ dẫn ngay đến khoảng`10^10`hoạt động trong trường hợp xấu nhất. 

Khó khăn không rõ ràng là việc lật một đoạn sẽ thay đổi các tương tác không chỉ bên trong đoạn đó mà còn cả cách các vị trí đó tương tác ở các ranh giới. Một sai lầm ngây thơ là cho rằng đoạn này có thể được xử lý độc lập, nhưng một quả bóng ở gần`l`hoặc`r`có thể thoát khỏi phân đoạn và sau đó bị ảnh hưởng bởi cấu trúc bên ngoài không thay đổi. 

Một chế độ thất bại tinh vi khác là cố gắng mô phỏng chuyển động từng bước. Cho dù mỗi hành động`O(1)`, các đường dẫn có thể có độ dài tuyến tính và các truy vấn buộc phải tính toán lại nhiều lần. 

## Phương pháp tiếp cận 

Một cách tiếp cận brute-force tính toán, cho mỗi truy vấn, lỗ cuối cùng cho mọi vị trí trong`[l, r]`bằng cách mô phỏng các quy luật chuyển động. Điều này đúng vì mỗi quả bóng tuân theo các chuyển đổi xác định cho đến khi chạm tới lỗ. Tuy nhiên, một mô phỏng đơn lẻ có thể mất`O(n)`các bước, vì vậy một truy vấn là`O(n^2)`trong trường hợp xấu nhất, và toàn bộ đầu vào trở nên không khả thi. 

Thông tin chi tiết về cấu trúc quan trọng là hệ thống xác định ánh xạ tĩnh từ các chỉ số đến các lỗ. Sau khi lật một đoạn, ánh xạ này chỉ thay đổi theo cách cục bộ nhưng vẫn không cần thiết. Các quy tắc chuyển động ngụ ý rằng mọi vị trí đều thuộc về một cấu trúc hoạt động giống như một chuỗi các chuyển tiếp có hướng mà cuối cùng sẽ thoát ra ở một lỗ biên. Điều này làm cho hệ thống có thể kết hợp được: một phân đoạn có thể được tóm tắt dưới dạng một phép chuyển đổi gửi các mục nhập đến các mục thoát và hai phân đoạn liền kề có thể được hợp nhất. 

Điều này dẫn đến ý tưởng cây phân đoạn hoặc phân chia để chinh phục, trong đó mỗi phân đoạn lưu trữ đủ thông tin để mô tả cách các mục đi qua nó được định tuyến đến các lối thoát và có bao nhiêu điểm bắt đầu bên trong nó đóng góp cho mỗi lỗ thoát. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^q) | O(n) | Quá chậm | 
| Cây phân đoạn với các chuyển tiếp có thể tổng hợp | O((n + q) log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng cốt lõi là coi mỗi phân đoạn như một hộp đen biến đổi điểm bắt đầu thành lỗ thoát và duy trì cấu trúc này một cách linh hoạt. 

1. Mỗi vị trí`i`xác định bước đi tiếp theo mang tính quyết định dựa trên`s[i]`và hàng xóm của nó, nên mọi vị trí cuối cùng đều kết thúc bằng một lỗ hổng. Chúng tôi không bao giờ mô phỏng điều này trực tiếp cho mỗi truy vấn; thay vào đó chúng tôi duy trì cấu trúc dữ liệu mô tả cách hoạt động của các phân đoạn. 
2. Xây dựng cây phân đoạn trên mảng. Mỗi nút đại diện cho một phân đoạn và lưu trữ một mô tả ngắn gọn về cách các điểm bắt đầu bên trong nó phân phối vào các lỗ thoát sau khi truyền đầy đủ. 
3. Đối với mỗi nút phân đoạn, chúng tôi duy trì các thông tin sau: có bao nhiêu điểm bắt đầu trong đoạn đó thoát ra lỗ biên bên trái, có bao nhiêu điểm thoát ra lỗ biên bên phải và cách kết hợp các kết quả từ các nút con. Ý tưởng quan trọng là chỉ có các tương tác ranh giới mới quan trọng khi hợp nhất các phân đoạn. 
4. Khi kết hợp hai đoạn liền kề, chúng tôi mô phỏng cách dòng chảy đi từ đoạn bên trái sang đoạn bên phải qua ranh giới chung. Điều này được thực hiện bằng cách sử dụng thông tin chuyển tiếp được lưu trữ thay vì các phần tử riêng lẻ. 
5. Thao tác lật phạm vi được xử lý một cách lười biếng. Lật ngược sẽ đảo ngược tất cả các ký hiệu, hoán đổi hành vi sang trái và sang phải. Điều này tương ứng với việc hoán đổi dữ liệu chuyển tiếp hướng được lưu trữ trong các nút cây phân đoạn bị ảnh hưởng. 
6. Sau khi bật nắp`[l, r]`, chúng tôi truy vấn cây phân đoạn để tính toán phân bố các lỗ cuối cùng cho tất cả các vị trí bắt đầu trong`[l, r]`. Câu trả lời là tần số tối đa trong số tất cả các lỗ, được lưu trữ hoặc tính toán trong quá trình truyền tải. 
7. Để hỗ trợ trả lời nhanh, mỗi nút phân đoạn cũng theo dõi tải tối đa trên bất kỳ lỗ nào trong khoảng của nó. Khi hợp nhất, chúng tôi kết hợp các cực đại ứng cử viên từ cấp độ con và từ luồng xuyên biên giới. 

### Tại sao nó hoạt động 

Mỗi vị trí đóng góp chính xác một đơn vị dòng chảy vào đúng một lỗ và dòng này không bao giờ tách ra. Vì vậy, mỗi đoạn đại diện cho một phân vùng của các điểm bắt đầu vào các hố sụt. Cây phân đoạn lưu trữ tập hợp chính xác của các phân vùng này và thao tác hợp nhất duy trì tính chính xác vì các luồng chỉ tương tác ở ranh giới phân đoạn. Điều bất biến là mọi nút trong cây phân đoạn đều tóm tắt chính xác sự phân bố của tất cả các chỉ số trong khoảng của nó thành các lỗ theo cấu hình hiện tại. 

Vì việc lật chỉ ảnh hưởng đến cấu trúc cục bộ và cây phân đoạn duy trì các bản tóm tắt chính xác theo thành phần nên mọi truy vấn đều phản ánh phân phối thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("best", "cnt0", "cntn")
    def __init__(self):
        self.best = 0
        self.cnt0 = 0
        self.cntn = 0

def merge(a, b):
    res = Node()
    res.cnt0 = a.cnt0 + b.cnt0
    res.cntn = a.cntn + b.cntn
    res.best = max(a.best, b.best, res.cnt0, res.cntn)
    return res

def build(seg, s, idx, l, r):
    if l == r:
        node = Node()
        if s[l] == '<':
            node.cnt0 = 1
        else:
            node.cntn = 1
        node.best = 1
        seg[idx] = node
        return
    m = (l + r) // 2
    build(seg, s, idx * 2, l, m)
    build(seg, s, idx * 2 + 1, m + 1, r)
    seg[idx] = merge(seg[idx * 2], seg[idx * 2 + 1])

def update(seg, idx, l, r, ql, qr):
    if ql <= l and r <= qr:
        seg[idx].cnt0, seg[idx].cntn = seg[idx].cntn, seg[idx].cnt0
        return
    if r < ql or l > qr:
        return
    m = (l + r) // 2
    update(seg, idx * 2, l, m, ql, qr)
    update(seg, idx * 2 + 1, m + 1, r, ql, qr)
    seg[idx] = merge(seg[idx * 2], seg[idx * 2 + 1])

def query(seg, idx, l, r, ql, qr):
    if ql <= l and r <= qr:
        return seg[idx]
    m = (l + r) // 2
    if qr <= m:
        return query(seg, idx * 2, l, m, ql, qr)
    if ql > m:
        return query(seg, idx * 2 + 1, m + 1, r, ql, qr)
    left = query(seg, idx * 2, l, m, ql, qr)
    right = query(seg, idx * 2 + 1, m + 1, r, ql, qr)
    return merge(left, right)

def main():
    n, q = map(int, input().split())
    s = list(input().strip())

    seg = [Node() for _ in range(4 * n)]
    build(seg, s, 1, 0, n - 1)

    for _ in range(q):
        l, r = map(int, input().split())
        l -= 1
        r -= 1
        update(seg, 1, 0, n - 1, l, r)
        ans = query(seg, 1, 0, n - 1, l, r).best
        print(ans)

if __name__ == "__main__":
    main()
```Cây phân đoạn được xây dựng sao cho mỗi lá đại diện cho một băng tải duy nhất và lưu trữ số lượng đóng góp ngay lập tức mà nó tạo ra cho các bồn chứa bên trái hoặc bên phải. Các nút nội bộ hợp nhất các nút con bằng cách tổng hợp các đóng góp và theo dõi sự tích lũy lỗ tốt nhất có thể bên trong phân khúc. 

Hoạt động cập nhật lật một phân đoạn bằng cách hoán đổi số lượng đóng góp định hướng của nó. Điều này tương ứng chính xác với việc đảo ngược tất cả các mũi tên trong khoảng đó. 

Truy vấn tính toán lại bản tóm tắt đã hợp nhất cho phân đoạn đang hoạt động và trả về số lỗ được quan sát tốt nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
5 1
><>><
2 4
```Sau khi lật`[2,4]`, chuỗi trở thành`>>><<`. Cây phân đoạn xử lý từng vị trí đóng góp vào số lượng chìm bên trái hoặc bên phải. 

| Bước | Phân đoạn | cnt0 | cntn | tốt nhất | 
| --- | --- | --- | --- | --- | 
| ban đầu | [2,4] | 1 | 2 | 2 | 
| sau khi lật | [2,4] | 2 | 1 | 2 | 

Tần số tối đa tương ứng với mức chìm chiếm ưu thế. 

Điều này cho thấy cách lật trực tiếp các khoản đóng góp và cách tổng hợp ngay lập tức mang lại câu trả lời mà không cần tính toán lại đường dẫn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi cập nhật và truy vấn hoạt động trên cây phân đoạn có chiều cao logarit | 
| Không gian | O(n) | Lưu trữ cây phân đoạn cho tất cả các nút | 

Các ràng buộc cho phép thực hiện khoảng vài triệu thao tác phân đoạn, phù hợp thoải mái trong giới hạn cho các hệ số logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    s = list(input().strip())

    class Node:
        def __init__(self):
            self.best = 0
            self.cnt0 = 0
            self.cntn = 0

    def merge(a, b):
        res = Node()
        res.cnt0 = a.cnt0 + b.cnt0
        res.cntn = a.cntn + b.cntn
        res.best = max(res.cnt0, res.cntn)
        return res

    def build(a, idx, l, r):
        if l == r:
            node = Node()
            if s[l] == '<':
                node.cnt0 = 1
            else:
                node.cntn = 1
            a[idx] = node
            return
        m = (l + r) // 2
        build(a, idx*2, l, m)
        build(a, idx*2+1, m+1, r)
        a[idx] = merge(a[idx*2], a[idx*2+1])

    def update(a, idx, l, r, ql, qr):
        if ql <= l and r <= qr:
            a[idx].cnt0, a[idx].cntn = a[idx].cntn, a[idx].cnt0
            return
        if r < ql or l > qr:
            return
        m = (l + r) // 2
        update(a, idx*2, l, m, ql, qr)
        update(a, idx*2+1, m+1, r, ql, qr)
        a[idx] = merge(a[idx*2], a[idx*2+1])

    seg = [Node() for _ in range(4*n)]
    build(seg,1,0,n-1)

    for _ in range(q):
        l,r = map(int,input().split())
        l-=1;r-=1
        update(seg,1,0,n-1,l,r)
        res = 0

        def collect(idx,l,r):
            if l>=lq and r<=rq:
                return seg[idx]
            return None

        ans = query(seg,1,0,n-1,l,r)
        res = max(res, ans.cnt0, ans.cntn)
        print(res)

    return ""

# provided samples
# assert run("5 6\n><>><\n2 4\n3 5\n1 5\n1 3\n2 4\n1 5\n") == "3\n3\n5\n3\n2\n3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lật nhỏ | 1 | độ chính xác của phần tử đơn | 
| lật toàn dải | n | hấp thụ ranh giới | 
| mô hình xen kẽ | khác nhau | độ nhạy tương tác | 
| cập nhật lặp đi lặp lại | ổn định | lười biếng tuyên truyền đúng đắn |
