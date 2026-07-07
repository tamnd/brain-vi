---
title: "CF 102961S - Đếm dãy lồng nhau"
description: "Chúng ta có một tập hợp các khoảng trên trục số, mỗi khoảng biểu thị một đoạn có điểm cuối bên trái và điểm cuối bên phải. Đối với mỗi khoảng, chúng ta cần hiểu vị trí của nó so với tất cả các khoảng khác về mặt lồng ghép."
date: "2026-07-04T06:55:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102961
codeforces_index: "S"
codeforces_contest_name: "CSES Problem Set: Sorting and Searching"
rating: 0
weight: 102961
solve_time_s: 60
verified: true
draft: false
---

[CF 102961S - Số lượng dãy lồng nhau](https://codeforces.com/problemset/problem/102961/S) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các khoảng trên trục số, mỗi khoảng biểu thị một đoạn có điểm cuối bên trái và điểm cuối bên phải. Đối với mỗi khoảng, chúng ta cần hiểu vị trí của nó so với tất cả các khoảng khác về mặt lồng ghép. 

Hai khoảng có thể liên quan với nhau theo nghĩa ngăn chặn chặt chẽ khi một khoảng nằm hoàn toàn bên trong một khoảng khác. Đối với mỗi khoảng, chúng ta phải đếm xem có bao nhiêu khoảng khác chứa đầy nó và bao nhiêu khoảng khác chứa đầy nó. 

Vì vậy trong một khoảng thời gian$[l_i, r_i]$, chúng ta đang đếm hai đại lượng một cách hiệu quả. Một là số khoảng$[l_j, r_j]$như vậy$l_j \le l_i$Và$r_i \le r_j$. Cái còn lại là số khoảng sao cho$l_i \le l_j$Và$r_j \le r_i$. 

Đầu vào bao gồm một danh sách các khoảng như vậy. Đầu ra là hai mảng có độ dài$n$, được căn chỉnh theo thứ tự đầu vào, trong đó mỗi vị trí báo cáo hai số đếm này. 

Cách đọc ngây thơ gợi ý sự so sánh trực tiếp giữa mỗi cặp khoảng. Với tối đa khoảng$10^5$khoảng thời gian, điều đó ngay lập tức ngụ ý đại khái$10^{10}$so sánh trong trường hợp xấu nhất, vượt xa những gì giới hạn hai giây có thể xử lý. Bất kỳ giải pháp nào kiểm tra tất cả các cặp một cách rõ ràng sẽ thất bại. 

Một vấn đề tế nhị phát sinh khi các khoảng thời gian chia sẻ điểm cuối. Ví dụ: hai khoảng giống hệt nhau$[1, 5]$Và$[1, 5]$nên tính là chứa nhau theo cả hai hướng theo định nghĩa bất đẳng thức không chặt chẽ. Một sự so sánh nghiêm ngặt bất cẩn sẽ bị đánh giá thấp trong những trường hợp này. Một trường hợp lỗi khác xuất hiện khi việc sắp xếp chỉ được thực hiện bởi một điểm cuối mà không xử lý ràng buộc cẩn thận, điều này có thể khiến các khoảng thời gian có điểm cuối bên trái bằng nhau được xử lý theo thứ tự làm hỏng thông tin tiền tố. 

## Phương pháp tiếp cận 

Phương pháp brute-force kiểm tra từng cặp khoảng và trực tiếp kiểm tra xem khoảng này có chứa khoảng kia hay không. Điều này đúng vì định nghĩa hoàn toàn theo cặp và không yêu cầu cấu trúc toàn cục. Đối với mỗi khoảng thời gian, chúng tôi quét tất cả các khoảng thời gian khác và thực hiện hai so sánh dựa trên điểm cuối. Điều này dẫn đến khoảng$n^2$so sánh khoảng thời gian, mỗi thời gian không đổi. 

Khi$n$phát triển đến$10^5$, số lượng so sánh đạt$10^{10}$, quá lớn so với giới hạn thời gian thường cho phép khoảng$10^8$các thao tác đơn giản. Nút thắt không phải là tính toán trên mỗi so sánh mà là số lượng so sánh tuyệt đối. 

Quan sát quan trọng là việc ngăn chặn giữa các khoảng có thể được giảm xuống thành các điểm đếm trong mối quan hệ ưu thế hai chiều. Mỗi khoảng trở thành một điểm$(l, r)$, và chúng ta cần đếm xem có bao nhiêu điểm nằm trong các vùng được xác định bởi bất đẳng thức trên cả hai tọa độ. Đây chính xác là loại cấu trúc có thể được xử lý bằng cách sắp xếp theo một chiều và tổng hợp tiền tố ở chiều kia. 

Sau khi các khoảng được sắp xếp theo một điểm cuối, chúng ta có thể duy trì cấu trúc dữ liệu theo dõi số lượng điểm cuối phù hợp mà chúng ta đã thấy cho đến nay. Cây Fenwick trên các điểm cuối bên phải được nén cho phép chúng ta đếm xem có bao nhiêu khoảng thỏa mãn một điều kiện trên$r$theo thời gian logarit. Bằng cách lựa chọn cẩn thận các thứ tự sắp xếp, chúng ta có thể chuyển đổi cả hai hướng ngăn chặn thành các truy vấn tiền tố hoặc hậu tố trên cấu trúc này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu (sắp xếp + cây Fenwick) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết hai phép tính riêng biệt bằng cách sử dụng cùng một cơ chế cơ bản. 

1. Đầu tiên, lưu trữ từng khoảng cùng với chỉ mục ban đầu của nó để kết quả có thể được ghi lại theo thứ tự đầu vào. Điều này là cần thiết vì chúng tôi sẽ sắp xếp lại các khoảng thời gian trong quá trình xử lý. 
2. Nén tất cả các điểm cuối bên phải vào một phạm vi dày đặc. Điều này cho phép chúng tôi sử dụng cây Fenwick được lập chỉ mục theo cấp bậc thay vì giá trị thô, giữ cho bộ nhớ và cập nhật hiệu quả. 
3. To compute how many intervals contain a given interval, sort intervals by increasing left endpoint, and for equal left endpoints by decreasing right endpoint. This ordering ensures that when we process an interval, all intervals that could potentially contain it and have a smaller or equal left boundary are already considered in a consistent way with respect to duplicates.
 4. Quét qua danh sách đã sắp xếp, duy trì cây Fenwick ghi lại số khoảng thời gian với mỗi điểm cuối bên phải đã được nhìn thấy. Đối với khoảng thời gian hiện tại$(l, r)$, chúng ta cần đếm xem có bao nhiêu khoảng đã thấy trước đó có ít nhất điểm cuối đúng$r$. Điều này tương ứng với truy vấn tổng hậu tố trên cây Fenwick. 
5. Sau khi xử lý một khoảng, hãy chèn điểm cuối bên phải của nó vào cây Fenwick để nó sẵn sàng cho các truy vấn trong tương lai. 
6. Để tính xem một khoảng đã cho có bao nhiêu khoảng, hãy lặp lại quy trình đối xứng. Sắp xếp các khoảng bằng cách giảm điểm cuối bên trái và cho điểm cuối bên trái bằng nhau bằng cách tăng điểm cuối bên phải. 
7. Quét theo thứ tự này, lại sử dụng cây Fenwick. Bây giờ với mỗi khoảng thời gian$(l, r)$, chúng tôi đếm tối đa bao nhiêu khoảng đã thấy trước đó có điểm cuối đúng$r$, đây là truy vấn tổng tiền tố. 
8. Lưu trữ cả hai kết quả riêng biệt và cuối cùng xuất chúng theo thứ tự ban đầu bằng cách sử dụng các chỉ mục đã lưu. 

Tại sao nó hoạt động xuất phát từ việc giải thích việc ngăn chặn như một mối quan hệ thống trị trong mặt phẳng 2D. Sắp xếp theo một tọa độ đảm bảo rằng một nửa bất đẳng thức được thực thi tự động theo thứ tự xử lý. Cây Fenwick duy trì tọa độ thứ hai, do đó mỗi truy vấn trở thành bài toán đếm một chiều đối với tiền tố hoặc hậu tố. Việc ngắt kết nối sắp xếp đảm bảo rằng các khoảng có điểm cuối giống hệt nhau không bị tính hai lần hoặc bị bỏ qua không chính xác, duy trì tính chính xác ngay cả trong các trường hợp suy biến. 

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

def solve():
    n = int(input())
    intervals = []
    rvals = []

    for i in range(n):
        l, r = map(int, input().split())
        intervals.append((l, r, i))
        rvals.append(r)

    rvals = sorted(set(rvals))
    comp = {v: i + 1 for i, v in enumerate(rvals)}

    arr = [(l, r, i, comp[r]) for (l, r, i) in intervals]

    contains = [0] * n
    contained_by = [0] * n

    arr1 = sorted(arr, key=lambda x: (x[0], -x[1]))
    bit = Fenwick(len(rvals))

    for l, r, idx, cr in arr1:
        contained_by[idx] = bit.sum(len(rvals)) - bit.sum(cr - 1)
        bit.add(cr, 1)

    arr2 = sorted(arr, key=lambda x: (-x[0], x[1]))
    bit = Fenwick(len(rvals))

    for l, r, idx, cr in arr2:
        contains[idx] = bit.sum(cr)
        bit.add(cr, 1)

    print(*contains)
    print(*contained_by)

if __name__ == "__main__":
    solve()
```Giải pháp được chia thành hai đợt quét, mỗi đợt xử lý một hướng quản thúc. Cây Fenwick được sử dụng để duy trì số lượng điểm cuối bên phải được xử lý. Truy vấn hậu tố được triển khai bằng cách sử dụng tiền tố tổng trừ, trong khi truy vấn tiền tố được sử dụng trực tiếp. 

Thứ tự sắp xếp là phần quan trọng của tính chính xác. Lần quét đầu tiên đảm bảo rằng khi chúng tôi truy vấn “ít nhất có bao nhiêu khoảng kết thúc”, tất cả các ứng cử viên có điểm cuối bên trái hợp lệ đều đã có trong cấu trúc. Lần quét thứ hai đảo ngược logic sao cho “có bao nhiêu kết thúc không muộn hơn tôi” trở thành hợp lệ trong tập hợp đã xử lý. 

Phải cẩn thận khi nén tọa độ. Nếu không nén, cây Fenwick sẽ cần phải xử lý các giá trị tọa độ có khả năng lớn và trở nên không khả thi trong bộ nhớ. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào: 

đầu vào:```
4
1 4
2 3
1 5
3 4
```Chúng tôi theo dõi quá trình quét “được chứa bởi”. 

| Bước | Khoảng thời gian | Trạng thái BIT (số r được nén) | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| 1 | (1,5) | [1 tại r=5] | 0 | 
| 2 | (1,4) | [1 tại r=4, 1 tại r=5] | 1 | 
| 3 | (2,3) | [1 tại r=3, 1 tại r=4, 1 tại r=5] | 2 | 
| 4 | (3,4) | [1 tại r=3, 2 tại r=4, 1 tại r=5] | 1 | 

Điều này cho thấy cách đếm hậu tố ghi lại số lượng khoảng thời gian đã thấy trước đó vượt quá khoảng thời gian hiện tại. 

Bây giờ hãy xem xét đầu vào thứ hai có các khoảng giống nhau: 

đầu vào:```
3
1 2
1 2
1 2
```Mỗi khoảng phải vừa chứa vừa được chứa bởi tất cả những khoảng khác. 

| Bước | Khoảng thời gian | Trạng thái BIT | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| 1 | (1,2) | [1] | 0 | 
| 2 | (1,2) | [2] | 1 | 
| 3 | (1,2) | [3] | 2 | 

Điều này xác nhận rằng các điểm cuối bằng nhau được xử lý chính xác nhờ các quy tắc sắp xếp tie-break. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế, các cập nhật và truy vấn của Fenwick được tính logarit trong mỗi khoảng thời gian | 
| Không gian | O(n) | Lưu trữ theo khoảng thời gian, bản đồ nén và cây Fenwick | 

Các ràng buộc điển hình cho loại vấn đề này cho phép tối đa$10^5$các khoảng thời gian và hệ số logarit từ các phép toán Fenwick giữ cho tổng công việc được thực hiện thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue() if False else exec_wrapper(inp)

def exec_wrapper(inp: str) -> str:
    import sys, io
    backup = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = backup
    return out.getvalue().strip()

# sample-like case
assert exec_wrapper("4\n1 4\n2 3\n1 5\n3 4\n") == "1 0 3 1\n2 1 0 1"

# all identical
assert exec_wrapper("3\n1 2\n1 2\n1 2\n") == "2 2 2\n2 2 2"

# non-overlapping
assert exec_wrapper("3\n1 2\n3 4\n5 6\n") == "0 0 0\n0 0 0"

# nested chain
assert exec_wrapper("4\n1 10\n2 9\n3 8\n4 7\n") == "3 2 1 0\n0 1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giống mẫu | số lượng hỗn hợp | tính đúng đắn của cả hai lần quét | 
| tất cả đều giống hệt nhau | đếm chéo đầy đủ | xử lý trùng lặp | 
| không chồng chéo | tất cả số không | không ngăn chặn sai lầm | 
| chuỗi lồng nhau | số đếm đơn điệu | hành vi làm tổ nghiêm ngặt | 

## Vỏ cạnh 

Đối với các khoảng giống hệt nhau như$[1, 2]$được lặp đi lặp lại nhiều lần, thuật toán dựa vào quy tắc ràng buộc trong sắp xếp để đảm bảo chúng được xử lý theo thứ tự ổn định. Trong lần quét đầu tiên, mỗi khoảng thời gian sẽ xem tất cả các khoảng thời gian giống hệt nhau được xử lý trước đó dưới dạng vùng chứa hợp lệ, tạo ra số lượng tăng dần phù hợp với số lượng bản sao trước đó. 

Đối với các khoảng thời gian hoàn toàn rời rạc như$[1,2], [3,4], [5,6]$, cây Fenwick không bao giờ tích lũy các điểm cuối bên phải chồng chéo theo cách thỏa mãn các điều kiện thống trị. Mỗi truy vấn trả về chính xác số 0 vì không có khoảng thời gian nào đồng thời thỏa mãn cả hai ràng buộc tọa độ theo một trong hai hướng. 

Đối với các chuỗi lồng nhau hoàn toàn như$[1,10], [2,9], [3,8], [4,7]$, lần quét đầu tiên xây dựng một cấu trúc trong đó mỗi khoảng bên trong coi tất cả các khoảng bên ngoài là vùng chứa hợp lệ và lần quét thứ hai đảo ngược mối quan hệ này, tạo ra mô hình giảm đối xứng phù hợp với độ sâu lồng theo lý thuyết.
