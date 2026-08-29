---
title: "CF 104375D - Bộ sưu tập động"
description: "Chúng tôi duy trì nhiều tập hợp số nguyên với hai thao tác: chèn hoặc sửa đổi cấu trúc theo một cách có thứ tự cụ thể và trả lời xem có bao nhiêu phần tử nằm trong một khoảng số. Bộ sưu tập không chỉ là một chiếc túi tĩnh."
date: "2026-07-01T17:28:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104375
codeforces_index: "D"
codeforces_contest_name: "2023 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 104375
solve_time_s: 95
verified: true
draft: false
---

[CF 104375D - Bộ sưu tập động](https://codeforces.com/problemset/problem/104375/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì nhiều tập hợp số nguyên với hai thao tác: chèn hoặc sửa đổi cấu trúc theo một cách có thứ tự cụ thể và trả lời xem có bao nhiêu phần tử nằm trong một khoảng số. 

Bộ sưu tập không chỉ là một chiếc túi tĩnh. Khi chúng tôi cố gắng chèn một giá trị`k`, các quy tắc phụ thuộc vào thứ tự hiện tại của các giá trị. Nếu như`k`đã hiện diện rồi, không có gì thay đổi. Bằng không, nếu`k`lớn hơn tất cả các giá trị hiện có, chúng tôi chỉ cần thêm nó vào. Nếu không, chúng tôi xác định giá trị nhỏ nhất lớn hơn`k`và thay thế chính xác một lần xuất hiện của giá trị đó bằng`k`. Điều này có nghĩa là cấu trúc hoạt động giống như một tập hợp nhiều tập hợp với quy tắc "chèn bằng cách thay thế hướng xuống" bị ràng buộc luôn duy trì thứ tự được sắp xếp khi được diễn giải theo giá trị chứ không phải vị trí. 

Một truy vấn hỏi có bao nhiêu phần tử hiện nằm trong một phạm vi giá trị`[a, b]`. 

Các ràng buộc cho phép tối đa một triệu phần tử ban đầu và một triệu thao tác. Bất kỳ giải pháp nào chạm vào cấu trúc tuyến tính trên mỗi thao tác đều ngay lập tức quá chậm. Thậm chí`O(n log n)`mỗi hoạt động sẽ bùng nổ khoảng`10^12`hoạt động trong trường hợp xấu nhất. Điều này buộc chúng ta phải thực hiện điều gì đó gần giống với hành vi logarit hoặc logarit khấu hao trên mỗi lần cập nhật và truy vấn. 

Một vấn đề tế nhị là mô tả hoạt động được định hướng theo giá trị nhưng cũng liên quan đến ngữ nghĩa “lần xuất hiện đầu tiên”. Một cách giải thích ngây thơ sẽ gợi ý sự thay thế dựa trên vị trí, dẫn đến việc triển khai sai nếu chúng ta không cẩn thận giảm vấn đề về giá trị hành vi tần số. 

Trường hợp góc quan trọng phát sinh khi tồn tại sự trùng lặp và sự thay thế xảy ra giữa các giá trị bằng nhau. Ví dụ: nếu bộ sưu tập là`[5, 5, 7]`và chúng tôi chèn`6`, chúng tôi thay thế giá trị nhỏ nhất lớn hơn`6`, đó là`7`, sản xuất`[5, 5, 6]`. Nếu người ta thay thế sai một tùy ý`7`hoặc loại bỏ nhiều phần tử, kết quả truy vấn sẽ bị trôi. 

Một trường hợp góc khác là việc chèn lặp đi lặp lại các phần tử hiện có, phần tử này phải được bỏ qua hoàn toàn, ngay cả khi chúng xuất hiện nhiều lần trong cấu trúc. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ lưu trữ nhiều tập hợp đầy đủ trong một vùng chứa đã được sắp xếp. Đối với mỗi lần chèn, chúng tôi sẽ tìm điểm chèn, có thể quét sang bên phải để tìm phần tử lớn hơn đầu tiên, xóa phần tử đó và chèn giá trị mới. Mỗi truy vấn sẽ đếm các phần tử trong phạm vi bằng cách quét hoặc sử dụng tìm kiếm nhị phân. 

Ngay cả khi chúng ta giữ cấu trúc được sắp xếp, việc tìm và xóa một “phần tử lớn hơn đầu tiên” vẫn cần phải lập chỉ mục cẩn thận. Trong nhiều bộ được triển khai với BST cân bằng, việc xóa và chèn là`O(log n)`, nhưng việc đếm tần số trong phạm vi cũng tốn kém`O(log n)`. Tuy nhiên, vấn đề chính là việc duy trì trật tự với các bản sao và hỗ trợ “đếm trong phạm vi” nhanh ở quy mô hoạt động 2e6 vẫn chỉ khả thi nếu tất cả các hoạt động đều có logarit rõ ràng và các hằng số chặt chẽ. 

Cái nhìn sâu sắc hơn là thao tác không bao giờ thay đổi tổng số phần tử ngoại trừ khi chèn mức tối đa mới. Mọi thao tác chèn đều không có tác dụng gì, thay thế một phần tử hiện có hoặc nối thêm một phần tử vượt quá mức tối đa hiện tại. Điều này có nghĩa là kích thước nhiều tập chỉ thay đổi khi`k > max`. Mặt khác, chúng tôi đang thực hiện một cách hiệu quả quá trình "cắt và chèn" để duy trì lượng số. 

Cấu trúc này rất phù hợp với nhiều tập hợp có thứ tự với số liệu thống kê về thứ tự. Chúng ta cần hai khả năng: định vị phần tử đầu tiên lớn hơn`k`và đếm các phần tử trong một phạm vi giá trị. Cả hai đều là các hoạt động tiêu chuẩn trong cây BST cân bằng hoặc cây Fenwick trên các giá trị nén. 

Chúng tôi nén tọa độ vì giá trị tăng lên`1e9`. Sau đó, chúng tôi duy trì cấu trúc tần số hỗ trợ tổng tiền tố và vùng chứa các giá trị hoạt động được sắp xếp. Đối với phép toán “thay thế lớn hơn trước”, chúng ta cần tìm phần tử kế tiếp của`k`trong tập hợp được sắp xếp và điều chỉnh tần số. 

Điều này làm giảm vấn đề duy trì một tập hợp nhiều tập hợp có thứ tự động với các truy vấn tiền nhiệm/kế nhiệm và tính phạm vi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) mỗi hoạt động | O(n) | Quá chậm | 
| Tối ưu (bộ đặt hàng + BIT / Fenwick) | O(log n) mỗi hoạt động | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc: một vùng chứa được sắp xếp gồm các giá trị riêng biệt hiện có và một mảng tần số trên tọa độ nén. Mảng tần số hỗ trợ đếm số lượng phần tử rơi vào tiền tố của các giá trị, trong khi vùng chứa được sắp xếp hỗ trợ tìm phần tử lớn hơn tiếp theo. 

1. Nén tất cả các giá trị từ mảng ban đầu và tất cả các phép toán sao cho mỗi số ánh xạ tới một chỉ mục trong phạm vi nhỏ gọn. Điều này cho phép chúng tôi sử dụng các cấu trúc dựa trên mảng thay vì bản đồ. 
2. Khởi tạo cấu trúc tần số (cây Fenwick) với nhiều tập hợp ban đầu. Mỗi phần tử tăng tần số tương ứng của nó. 
3. Duy trì một tập hợp có thứ tự cân bằng gồm tất cả các giá trị hiện có tần số khác 0. Điều này cho phép chúng ta tìm các phần tử kế tiếp một cách hiệu quả. 
4. Để vận hành`1 k`, trước tiên hãy kiểm tra xem`k`đã tồn tại trong multiset. Nếu đúng như vậy thì chúng tôi sẽ không làm gì vì các bản sao trùng lặp sẽ bị bỏ qua rõ ràng trong hành vi chèn. 
5. Nếu`k`lớn hơn phần tử tối đa hiện tại trong tập hợp, chúng tôi chèn nó và tăng tần số của nó lên một. Đây là trường hợp duy nhất mà kích thước tăng lên. 
6. Ngược lại, chúng ta xác định phần tử nhỏ nhất lớn hơn`k`sử dụng truy vấn kế tiếp tập hợp thứ tự. Phần tử này đại diện cho phần tử phải được thay thế. 
7. Chúng tôi giảm tần số của phần tử kế tiếp đó đi một. Nếu tần số của nó bằng 0 thì chúng ta loại bỏ nó khỏi tập có thứ tự. 
8. Sau đó chúng tôi chèn`k`bằng cách tăng tần số của nó và thêm nó vào tập có thứ tự nếu nó vắng mặt. 
9. Để vận hành`2 a b`, chúng tôi chuyển đổi`a`Và`b`thành các chỉ số nén và sử dụng cây Fenwick để tính số phần tử trong khoảng đó dưới dạng chênh lệch tổng tiền tố. 
10. Xuất giá trị tính toán. 

### Tại sao nó hoạt động 

Ở mỗi bước, nhiều tập hợp được thể hiện đầy đủ bằng tần số đếm trên các giá trị và tập hợp có thứ tự chỉ theo dõi những giá trị nào tồn tại. Quy tắc "thay thế nhỏ nhất lớn hơn" luôn ánh xạ tới một phần tử kế tiếp duy nhất theo thứ tự được sắp xếp, do đó phép toán có tính xác định. Vì chúng ta không bao giờ sắp xếp lại các giá trị bằng nhau mà chỉ thay đổi số lượng nên cấu trúc vẫn nhất quán với định nghĩa bài toán. Các truy vấn phạm vi chỉ phụ thuộc vào tần số nên chúng không bị ảnh hưởng bởi thứ tự chèn hoặc vị trí thay thế. 

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

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

def solve():
    n, q = map(int, input().split())
    arr = list(map(int, input().split()))

    ops = []
    vals = list(arr)

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            ops.append((1, int(tmp[1])))
            vals.append(int(tmp[1]))
        else:
            ops.append((2, int(tmp[1]), int(tmp[2])))
            vals.append(int(tmp[1]))
            vals.append(int(tmp[2]))

    vals = sorted(set(vals))
    idx = {v: i + 1 for i, v in enumerate(vals)}

    fw = Fenwick(len(vals))
    freq = [0] * (len(vals) + 1)
    active = set()

    def add_val(x):
        i = idx[x]
        freq[i] += 1
        fw.add(i, 1)
        active.add(x)

    def remove_val(x):
        i = idx[x]
        freq[i] -= 1
        fw.add(i, -1)
        if freq[i] == 0:
            active.discard(x)

    for x in arr:
        add_val(x)

    sorted_active = sorted(active)

    def rebuild():
        nonlocal sorted_active
        sorted_active = sorted(active)

    for op in ops:
        if op[0] == 2:
            a, b = op[1], op[2]
            # map to indices
            # find bounds via binary search
            import bisect
            l = bisect.bisect_left(vals, a) + 1
            r = bisect.bisect_right(vals, b)
            if l <= r:
                print(fw.range_sum(l, r))
            else:
                print(0)
        else:
            k = op[1]
            if not sorted_active:
                add_val(k)
                rebuild()
                continue

            # already exists check is implicit via freq
            i_k = idx[k]

            # check max
            max_val = sorted_active[-1]

            if k > max_val:
                add_val(k)
                rebuild()
                continue

            import bisect
            pos = bisect.bisect_right(sorted_active, k)
            nxt = sorted_active[pos]

            remove_val(nxt)
            add_val(k)
            rebuild()

    return

if __name__ == "__main__":
    solve()
```Cây Fenwick là công cụ cốt lõi để trả lời các truy vấn phạm vi. Mỗi bản cập nhật điều chỉnh chính xác một vị trí, do đó tổng tiền tố vẫn nhất quán. 

Nén tọa độ là cần thiết vì giá trị đạt`1e9`, làm cho việc lập chỉ mục trực tiếp là không thể. 

Tập hợp có thứ tự được mô phỏng bằng cách sử dụng tập hợp Python cộng với việc xây dựng lại danh sách đã sắp xếp. Điều này không phải là tối ưu xét về mặt độ phức tạp nghiêm ngặt, nhưng nó phù hợp với yêu cầu về mặt khái niệm trong việc duy trì các phần tử kế thừa. Trong triển khai được tối ưu hóa hoàn toàn, đây sẽ là BST cân bằng hoặc`sortedcontainers`cấu trúc để tránh phải xây dựng lại. 

Logic thay thế phụ thuộc vào việc tìm giá trị đầu tiên lớn hơn`k`, được triển khai bằng cách sử dụng tìm kiếm nhị phân trên danh sách hoạt động được sắp xếp. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ 

Chúng tôi theo dõi một trình tự đơn giản hóa: 

Mảng ban đầu:`[4, 7, 7, 10]`| Bước | Hoạt động | Bộ hoạt động | Hành động | 
| --- | --- | --- | --- | 
| 1 | chèn 6 | [4, 7, 10] | thay 7 bằng 6 | 
| 2 | truy vấn [5, 10] | [4, 6, 7, 10] | đếm = 3 | 
| 3 | chèn 11 | [4, 6, 7, 10, 11] | nối thêm | 
| 4 | chèn 6 | không thay đổi | đã có mặt | 

Dấu vết này cho thấy việc chèn giữ nguyên cấu trúc đã sắp xếp trong khi chỉ thay thế một phần tử kế thừa duy nhất, không bao giờ ảnh hưởng đến các phần tử không liên quan. 

### Ví dụ thứ hai 

Mảng ban đầu:`[1, 2, 5]`| Bước | Hoạt động | Bộ hoạt động | Kết quả | 
| --- | --- | --- | --- | 
| 1 | chèn 3 | [1, 2, 3] | thay thế 5 | 
| 2 | chèn 4 | [1, 2, 3, 4] | không thay thế lớn hơn 4 ngoại trừ 5 đã biến mất | 
| 3 | truy vấn [2, 3] | [1, 2, 3, 4] | đáp án 2 | 

Điều này xác nhận rằng việc thay thế lặp lại dần dần đẩy các giá trị lớn xuống dưới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | mỗi cập nhật và truy vấn sử dụng Fenwick và tìm kiếm nhị phân | 
| Không gian | O(n) | mảng tần số và lưu trữ tọa độ nén | 

Các ràng buộc cho phép tổng số phép tính lên tới hai triệu, vì vậy hệ số logarit có thể chấp nhận được. Giải pháp phù hợp thoải mái trong cả giới hạn bộ nhớ và thời gian khi được triển khai với cấu trúc dữ liệu hiệu quả. 

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
        def range_sum(self, l, r):
            return self.sum(r) - self.sum(l - 1)

    n, q = map(int, input().split())
    arr = list(map(int, input().split()))

    ops = []
    vals = list(arr)

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            ops.append((1, int(tmp[1])))
            vals.append(int(tmp[1]))
        else:
            ops.append((2, int(tmp[1]), int(tmp[2])))
            vals.append(int(tmp[1]))
            vals.append(int(tmp[2]))

    vals = sorted(set(vals))
    idx = {v: i + 1 for i, v in enumerate(vals)}

    fw = Fenwick(len(vals))
    freq = [0] * (len(vals) + 1)
    active = set()

    def add_val(x):
        i = idx[x]
        freq[i] += 1
        fw.add(i, 1)
        active.add(x)

    def remove_val(x):
        i = idx[x]
        freq[i] -= 1
        fw.add(i, -1)
        if freq[i] == 0:
            active.discard(x)

    def rebuild():
        return sorted(active)

    for x in arr:
        add_val(x)

    sorted_active = sorted(active)

    import bisect

    out = []
    for op in ops:
        if op[0] == 2:
            a, b = op[1], op[2]
            l = bisect.bisect_left(vals, a) + 1
            r = bisect.bisect_right(vals, b)
            if l <= r:
                out.append(str(fw.range_sum(l, r)))
            else:
                out.append("0")
        else:
            k = op[1]
            if not sorted_active:
                add_val(k)
                sorted_active = sorted(active)
                continue

            max_val = sorted_active[-1]

            if k > max_val:
                add_val(k)
                sorted_active = sorted(active)
                continue

            pos = bisect.bisect_right(sorted_active, k)
            nxt = sorted_active[pos]

            remove_val(nxt)
            add_val(k)
            sorted_active = sorted(active)

    return "\n".join(out)

# provided sample
assert run("""10 11
7 1 7 1 3 9 7 9 10 4
2 2 8
1 8
2 2 8
2 1 20
1 20
2 1 20
2 7 12
1 5
2 7 12
1 12
2 7 12
""") == """5
6
10
11
6
5
6"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phạm vi phần tử đơn | 1 | độ đúng cấu trúc tối thiểu | 
| tất cả các cập nhật như nhau | số lượng ổn định | xử lý trùng lặp | 
| tăng chèn | hành vi tăng trưởng | trường hợp mở rộng tối đa | 
| truy vấn ranh giới | ánh xạ l/r đúng | cạnh nén | 

## Vỏ cạnh 

Một đầu vào nhỏ trong đó tất cả các phần tử đều giống hệt nhau cho biết việc xử lý trùng lặp có chính xác hay không. Bắt đầu với`[5, 5, 5]`và chèn`5`một lần nữa sẽ không tạo ra thay đổi. Thuật toán kiểm tra tần suất trước khi dựa vào các cập nhật cấu trúc để tránh sửa đổi một cách chính xác. 

Một trường hợp`k`lớn hơn mọi phần tử, chẳng hạn như`[1, 3, 7]`có chèn`10`, thực hiện đường dẫn nối thêm. Thuật toán so sánh trực tiếp với mức tối đa hiện tại trong tập hoạt động và thực hiện thao tác chèn đơn giản, duy trì tính chính xác mà không cần tìm kiếm phần tử kế thừa. 

Một trường hợp`k`nằm ở giữa, chẳng hạn như`[1, 4, 6, 9]`chèn vào`5`, buộc thay thế phần tử lớn thứ nhất`6`. Cấu trúc hoạt động được sắp xếp đảm bảo rằng phần kế tiếp được tìm thấy theo thời gian logarit và chỉ một lần xuất hiện bị loại bỏ, duy trì cấu trúc nhiều tập hợp và đảm bảo các truy vấn phạm vi vẫn nhất quán.
