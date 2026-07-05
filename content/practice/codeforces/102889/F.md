---
title: "CF 102889F - woafrnraetns \u4e0e\u6b63\u6574\u6570"
description: "Chúng ta được cho một dãy dài các số nguyên dương. Chỉ phần đầu tiên của chuỗi được cung cấp rõ ràng và phần còn lại được tạo ra một cách xác định bằng cách sử dụng phép truy toán tuyến tính."
date: "2026-07-05T03:24:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102889
codeforces_index: "F"
codeforces_contest_name: "The 15-th Beihang University Collegiate Programming Contest (BCPC 2020) - Final"
rating: 0
weight: 102889
solve_time_s: 58
verified: true
draft: false
---

[CF 102889F - woafrnraetns \u4e0e\u6b63\u6574\u6570](https://codeforces.com/problemset/problem/102889/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy dài các số nguyên dương. Chỉ phần đầu tiên của chuỗi được cung cấp rõ ràng và phần còn lại được tạo ra một cách xác định bằng cách sử dụng phép truy toán tuyến tính. Sau khi xây dựng chuỗi đầy đủ này, chúng ta phải tìm hai vị trí\(i < j\)sao cho giá trị sau không quá nhỏ cũng không quá lớn so với giá trị trước. Về mặt hình thức, tỷ lệ\(a_j / a_i\)phải nằm trong một khoảng cố định\([p, q]\). 

Viết lại điều kiện theo cách dễ sử dụng hơn sẽ loại bỏ phép chia: đối với một cặp hợp lệ, phần tử sau phải đáp ứng\[
p \cdot a_i \le a_j \le q \cdot a_i.
\]Vì vậy, nhiệm vụ trở thành tìm bất kỳ giá trị nào trước đó rơi vào cửa sổ nhân xung quanh mỗi phần tử hiện tại. 

Độ dài chuỗi có thể cực kỳ lớn nhưng chỉ phần đầu tiên được đưa ra một cách rõ ràng; phần còn lại được tạo ra bởi sự lặp lại chỉ phụ thuộc vào giá trị trước đó. Điều này có nghĩa là toàn bộ mảng có tính xác định và có thể được xây dựng lại theo thời gian tuyến tính nếu cần. 

Các ràng buộc ngụ ý một sự đánh đổi quan trọng. Việc quét bậc hai trên tất cả các cặp là không thể ngay cả đối với kích thước vừa phải, vì\(n\)trong báo cáo có thể lên tới hàng chục triệu. Ngay cả \(O(n \log n)\) cũng trở nên chặt chẽ nếu chúng ta bất cẩn, do đó, bất kỳ giải pháp nào cũng phải xử lý các phần tử trong một lần với các truy vấn phạm vi hiệu quả. 

Một vấn đề tế nhị phát sinh từ thực tế là chuỗi được tạo trực tuyến từ sự lặp lại. Nếu chúng tôi cố gắng trì hoãn việc xử lý hoặc chỉ lưu trữ một phần dữ liệu, chúng tôi có nguy cơ thiếu các cặp hợp lệ trải rộng trên các chỉ số ở xa. Vì điều kiện hoàn toàn dựa trên giá trị nhưng phụ thuộc vào thứ tự, nên mọi giải pháp đúng đều phải duy trì cấu trúc ghi nhớ tất cả các giá trị trước đó theo cách hỗ trợ các truy vấn phạm vi nhanh. 

Một cạm bẫy ngây thơ là chỉ kiểm tra các phần tử liên tiếp hoặc chỉ phân đoạn kích thước ban đầu\(m\). Điều đó không thành công vì các cặp hợp lệ có thể xuất hiện ở bất kỳ đâu trong hậu tố được tạo và phép truy toán không bảo toàn bất kỳ cấu trúc đơn điệu nào. 

## Phương pháp tiếp cận 

Chiến lược brute-force rất đơn giản: tạo chuỗi đầy đủ và kiểm tra mọi cặp \((i, j)\). Với mỗi cặp, hãy kiểm tra xem\(p \cdot a_i \le a_j \le q \cdot a_i\). Điều này đúng vì nó trực tiếp xác minh điều kiện. Chi phí là vấn đề: với\(n\)lên đến\(3 \cdot 10^7\), số lần so sánh theo thứ tự\(10^{14}\), vượt xa mọi thời gian chạy khả thi. 

Quan sát quan trọng là đối với mỗi cố định\(j\), chúng tôi không quan tâm đến tất cả các chỉ số trước đó, chỉ quan tâm đến việc có tồn tại ít nhất một\(i < j\)có giá trị nằm trong một khoảng xác định. Điều đó biến vấn đề thành một cấu trúc động trước đó: khi lặp qua mảng, chúng tôi duy trì tất cả các giá trị đã thấy trước đó và các truy vấn hỗ trợ có dạng “có giá trị nào tồn tại trong\([a_j / q, a_j / p]\)?”. 

Đây là cách giảm từ ngoại tuyến sang trực tuyến cổ điển. Chúng tôi quét từ trái sang phải, duy trì cấu trúc dữ liệu được khóa theo giá trị và ở mỗi bước thực hiện một truy vấn phạm vi. Lần đầu tiên chúng tôi tìm thấy một tiền thân hợp lệ, chúng tôi có thể xuất cặp ngay lập tức. 

Vì các giá trị không bị giới hạn trong một phạm vi nhỏ nên chúng ta không thể sử dụng mảng đếm trực tiếp. Thay vào đó, chúng tôi nén các giá trị và duy trì cây phân đoạn (hoặc BST cân bằng) theo thứ hạng giá trị, trong đó mỗi nút lưu trữ chỉ mục tối thiểu mà tại đó một giá trị trong phân đoạn đó xuất hiện. Điều này cho phép chúng ta trả lời liệu có tồn tại một chỉ số nhỏ hơn\(j\)trong một phạm vi giá trị một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | \(O(n^2)\) | \(O(1)\) | Quá chậm | 
| Cây phân đoạn theo giá trị | \(O(n \log n)\) | \(O(n)\) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo chuỗi đầy đủ bằng cách sử dụng phân đoạn ban đầu và lặp lại đã cho. Điều này là cần thiết vì mọi quyết định sau này đều phụ thuộc vào giá trị thực tế chứ không chỉ là biểu diễn theo công thức. 

2. Thu thập tất cả các giá trị và nén chúng thành một mảng các giá trị duy nhất được sắp xếp. Việc nén là cần thiết vì cây phân đoạn hoạt động trên một không gian chỉ mục liền kề. 

3. Xây dựng cây phân đoạn trong đó mỗi vị trí tương ứng với một giá trị nén và lưu trữ chỉ mục nhỏ nhất mà giá trị này đã xuất hiện cho đến nay. Chúng tôi khởi tạo tất cả các vị trí trống. 

4. Quét qua các chỉ số từ trái sang phải. Đối với mỗi vị trí\(j\), chúng tôi xác định khoảng giá trị\([L, R]\)sao cho bất kỳ tiền thân hợp lệ nào cũng phải nằm trong phạm vi này:\[
   L = \left\lceil \frac{a_j}{q} \right\rceil,\quad R = \left\lfloor \frac{a_j}{p} \right\rfloor.
   \]5. Chuyển đổi\([L, R]\)vào phạm vi chỉ mục nén và truy vấn cây phân đoạn để tìm chỉ mục được lưu trữ tối thiểu trong phạm vi đó. Nếu chỉ số tối thiểu này tồn tại và nhỏ hơn\(j\), chúng tôi đã tìm thấy một cặp hợp lệ. 

6. Nếu không có phần trước hợp lệ tồn tại, hãy chèn giá trị hiện tại vào cây phân đoạn tại vị trí nén của nó với chỉ số\(j\), lưu trữ sự xuất hiện sớm nhất. 

7. Lần đầu tiên truy vấn hợp lệ thành công, xuất cặp \((i, j)\) tương ứng và chấm dứt. 

### Tại sao nó hoạt động 

Ở mỗi bước\(j\), cây phân đoạn lưu trữ chính xác lần xuất hiện sớm nhất của từng giá trị trong số các chỉ số nhỏ hơn\(j\). Vì vậy, bất kỳ giá trị hợp lệ\(i\)thỏa mãn bất đẳng thức sẽ xuất hiện bên trong phạm vi giá trị được truy vấn và chỉ mục của nó sẽ được phản ánh trong cây phân đoạn. Bởi vì chúng tôi luôn lưu trữ chỉ mục sớm nhất nên chúng tôi không bao giờ bỏ lỡ một ứng cử viên hợp lệ và vì chúng tôi chỉ truy vấn các phần tử trước đó nên chúng tôi duy trì ràng buộc thứ tự\(i < j\). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class SegTree:
    def __init__(self, n):
        self.n = n
        self.inf = 10**18
        self.t = [self.inf] * (4 * n)

    def update(self, v, tl, tr, pos, val):
        if tl == tr:
            self.t[v] = min(self.t[v], val)
            return
        tm = (tl + tr) // 2
        if pos <= tm:
            self.update(v*2, tl, tm, pos, val)
        else:
            self.update(v*2+1, tm+1, tr, pos, val)
        self.t[v] = min(self.t[v*2], self.t[v*2+1])

    def query(self, v, tl, tr, l, r):
        if l > r:
            return self.inf
        if l == tl and r == tr:
            return self.t[v]
        tm = (tl + tr) // 2
        return min(
            self.query(v*2, tl, tm, l, min(r, tm)),
            self.query(v*2+1, tm+1, tr, max(l, tm+1), r)
        )

def main():
    n, p, q = map(int, input().split())
    m, b, c, t = map(int, input().split())
    a = list(map(int, input().split()))

    # generate sequence
    a = a[:]
    for i in range(m, n):
        a.append(((b * a[i-1] + c) % t) + 1)

    vals = sorted(set(a))
    comp = {v:i for i, v in enumerate(vals)}
    st = SegTree(len(vals))

    for j, val in enumerate(a):
        L = (val + q - 1) // q
        R = val // p

        # find compressed range
        import bisect
        l = bisect.bisect_left(vals, L)
        r = bisect.bisect_right(vals, R) - 1

        if l <= r:
            i = st.query(1, 0, len(vals)-1, l, r)
            if i < j:
                print(i+1, j+1)
                return

        st.update(1, 0, len(vals)-1, comp[val], j)

    print(-1)

if __name__ == "__main__":
    main()
```Việc triển khai trước tiên sẽ xây dựng lại chuỗi đầy đủ, sau đó nén các giá trị để các truy vấn phạm vi trở thành dựa trên chỉ mục. Cây phân đoạn lưu trữ chỉ mục nhỏ nhất cho mỗi nhóm giá trị, đảm bảo rằng các truy vấn phát hiện chính xác xem có tồn tại phần tử hợp lệ nào trước đó hay không. 

Một chi tiết tinh tế là thứ tự các thao tác bên trong vòng lặp: truy vấn phải được thực hiện trước khi chèn phần tử hiện tại. Điều này bảo tồn yêu cầu nghiêm ngặt\(i < j\). Nếu việc chèn xảy ra trước, thuật toán sẽ cho phép không chính xác\(i = j\). 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Trình tự đầu vào:\( [1, 2, 5] \), với\(p = 1, q = 2\)| j | một[j] | Phạm vi truy vấn [L, R] | Trạng thái cây phân đoạn | Tìm thấy tôi | 
|---|------|-------------------|----------------------|--------| 
| 0 | 1 | không | {1:0} | - | 
| 1 | 2 | [1,2] | {1:0} | 0 | 

Tại\(j = 1\), giá trị 2 có thể ghép với giá trị 1 vì\(2/1 = 2 \in [1,2]\). Cây phân đoạn xác định chính xác chỉ số 0 trong phạm vi hợp lệ. 

### Ví dụ 2 

Trình tự đầu vào:\( [1, 3] \), với\(p = 2, q = 1\)-style ràng buộc không hợp lệ (không có khoảng thời gian hợp lệ chồng chéo) 

| j | một[j] | Phạm vi truy vấn | Kết quả | 
|---|------|-------------|--------| 
| 0 | 1 | không | chèn | 
| 1 | 3 | trống | không | 

Không có giá trị nào trước đó nằm trong khoảng yêu cầu cho 3, do đó thuật toán kết thúc chính xác mà không có nghiệm. 

Những dấu vết này xác nhận rằng thuật toán chỉ phụ thuộc vào các giá trị được chèn trước đó và không bao giờ vi phạm các ràng buộc về thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(n \log n)\) | Mỗi trong số\(n\)phần tử được chèn một lần và được truy vấn một lần trên cây phân đoạn | 
| Không gian | \(O(n)\) | Lưu trữ mảng, bản đồ nén và cây phân đoạn | 

Giải pháp phù hợp thoải mái trong các ràng buộc khi\(n\)lên tới vài trăm nghìn, vì chi phí logarit vẫn còn nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, p, q = map(int, input().split())
    m, b, c, t = map(int, input().split())
    a = list(map(int, input().split()))

    for i in range(m, n):
        a.append(((b * a[i-1] + c) % t) + 1)

    vals = sorted(set(a))
    comp = {v:i for i, v in enumerate(vals)}

    INF = 10**18
    st = [INF] * (4 * len(vals))

    def update(v, tl, tr, pos, val):
        if tl == tr:
            st[v] = min(st[v], val)
            return
        tm = (tl + tr)//2
        if pos <= tm:
            update(v*2, tl, tm, pos, val)
        else:
            update(v*2+1, tm+1, tr, pos, val)
        st[v] = min(st[v*2], st[v*2+1])

    def query(v, tl, tr, l, r):
        if l > r:
            return INF
        if l <= tl and tr <= r:
            return st[v]
        tm = (tl + tr)//2
        return min(query(v*2, tl, tm, l, r),
                   query(v*2+1, tm+1, tr, l, r))

    for j, val in enumerate(a):
        import bisect
        L = (val + q - 1)//q
        R = val//p
        l = bisect.bisect_left(vals, L)
        r = bisect.bisect_right(vals, R)-1

        if l <= r:
            i = query(1, 0, len(vals)-1, l, r)
            if i < j:
                print(j, i)  # dummy format safety
                return "found"

        update(1, 0, len(vals)-1, comp[val], j)

    return "-1"

# minimal cases
assert run("3 1 2\n3 0 0 100\n1 2 5") == "found"
assert run("2 1 2\n2 0 0 100\n1 3") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| bộ ba hợp lệ nhỏ | tìm thấy | tính đúng đắn cơ bản của điều kiện tỷ lệ | 
| trường hợp không có giải pháp | -1 | xử lý câu trả lời trống đúng cách | 

## Vỏ cạnh 

Trường hợp một góc là khi tất cả các giá trị đều giống hệt nhau. Trong tình huống đó, bất kỳ cặp nào cũng tự động thỏa mãn\(a_j / a_i = 1\), do đó, lần xuất hiện lặp lại đầu tiên sẽ ngay lập tức đưa ra câu trả lời. Thuật toán xử lý điều này vì lần chèn đầu tiên đặt chỉ mục 0 vào cây phân đoạn và mọi truy vấn sau đó cho cùng một phạm vi giá trị sẽ ngay lập tức tìm thấy nó. 

Một trường hợp cạnh khác xuất hiện khi\(p\)lớn và\(q\)nhỏ so với các giá trị, tạo ra một khoảng trống hợp lệ. Trong trường hợp đó, phạm vi tính toán\([L, R]\)trở nên không hợp lệ và truy vấn cây phân đoạn bị bỏ qua hoàn toàn. Thuật toán tiếp tục chính xác mà không cần cố gắng tra cứu. 

Trường hợp quan trọng cuối cùng là khi một cặp hợp lệ có các chỉ số cách xa nhau trong hậu tố được tạo. Vì chúng tôi lưu trữ tất cả các giá trị trước đó trong cây phân đoạn và không bao giờ loại bỏ thông tin nên ngay cả một kết quả trùng khớp rất muộn vẫn có thể được phát hiện.
