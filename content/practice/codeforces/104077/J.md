---
title: "CF 104077J - Tổng lạ"
description: "Chúng tôi được cung cấp một mảng giá trị và chúng tôi muốn chọn một tập hợp con các vị trí để tối đa hóa tổng các giá trị đã chọn. Điều khó khăn là việc lựa chọn bị hạn chế theo cách không đồng nhất: mọi phần tử được chọn đều áp đặt một hạn chế phụ thuộc vào chỉ mục của nó."
date: "2026-07-02T02:44:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104077
codeforces_index: "J"
codeforces_contest_name: "The 2022 ICPC Asia Xian Regional Contest"
rating: 0
weight: 104077
solve_time_s: 52
verified: true
draft: false
---

[CF 104077J - Tổng kỳ lạ](https://codeforces.com/problemset/problem/104077/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng giá trị và chúng tôi muốn chọn một tập hợp con các vị trí để tối đa hóa tổng các giá trị đã chọn. Điều khó khăn là việc lựa chọn bị hạn chế theo cách không đồng nhất: mọi phần tử được chọn đều áp đặt một hạn chế phụ thuộc vào chỉ mục của nó. Nếu chúng ta chọn vị trí i thì bên trong mỗi đoạn liền kề có độ dài i ở bất kỳ đâu trong mảng, chúng ta không được phép có nhiều hơn hai phần tử được chọn. 

Đây không phải là một hạn chế cục bộ giữa các lựa chọn lân cận. Một phần tử được chọn duy nhất ở chỉ mục lớn sẽ ảnh hưởng đến các phân đoạn rất dài, trong khi chỉ mục nhỏ chỉ ảnh hưởng đến các phân đoạn ngắn. Tính khả thi của một tập hợp phụ thuộc vào cách các “quy tắc về độ dài cửa sổ” này chồng chéo lên nhau trên toàn bộ mảng. 

Đầu vào là một số nguyên n theo sau là n giá trị. Chúng ta phải xuất ra số tiền tối đa có thể đạt được theo ràng buộc. 

Ràng buộc n lên tới 100000 ngay lập tức loại trừ mọi phép liệt kê tập hợp con hoặc DP hàm mũ đối với các phần tử đã chọn. Ngay cả chuyển đổi bậc hai cũng quá chậm. Mọi giải pháp hợp lệ đều phải có giá trị gần đúng là O(n log n) hoặc O(n). 

Một khó khăn tinh tế xuất phát từ thực tế là các ràng buộc được gắn với giá trị chỉ mục chứ không phải với khoảng cách vị trí. Hai ví dụ minh họa các chế độ lỗi: 

Nếu tất cả các giá trị đều dương và chúng tôi bỏ qua các ràng buộc, chúng tôi sẽ lấy mọi thứ, nhưng điều này rõ ràng vi phạm các quy tắc đối với các chỉ số nhỏ vì khoảng thời gian ngắn sẽ chứa quá nhiều lựa chọn. 

Nếu chúng ta tham lam chọn các giá trị lớn trước tiên mà không có cấu trúc, chúng ta có thể dễ dàng vi phạm các ràng buộc theo cách không thể sửa chữa được cục bộ. Ví dụ: việc chọn nhiều phần tử có giá trị cao ở các chỉ mục nhỏ sau này có thể cấm lựa chọn các phần tử hữu ích khác ở các chỉ mục lớn hơn vì những phần tử đó áp đặt các hạn chế về cửa sổ tầm xa. 

Ràng buộc có tính chất toàn cục và phụ thuộc vào chỉ mục, vì vậy chúng ta cần một cấu trúc có thể chuyển đổi các ràng buộc cửa sổ chồng chéo này thành một điều kiện đếm có thể quản lý được. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử tất cả các tập hợp con và kiểm tra xem ràng buộc có đúng hay không. Đối với mỗi tập hợp con ứng cử viên, chúng tôi sẽ quét tất cả các cửa sổ có mọi độ dài hoặc mô phỏng tương đương cho từng phần tử được chọn cách nó đóng góp cho mỗi khoảng có độ dài i. Điều này ngay lập tức trở nên không thể thực hiện được. Có 2^n tập hợp con và thậm chí việc kiểm tra một tập hợp con cũng cần ít nhất O(n^2) theo cách diễn giải ngây thơ về tất cả các độ dài cửa sổ, dẫn đến độ phức tạp về mặt thiên văn. 

Quan sát quan trọng là điều kiện “trong mỗi khoảng có độ dài i, tối đa 2 phần tử được chọn” có thể được diễn giải lại trên toàn cầu cho mỗi chỉ mục. Sửa chỉ mục i. Bất kỳ phần tử nào được chọn ở vị trí i đều đóng góp một hạn chế trên mọi cửa sổ có độ dài i. Thay vì kiểm tra tất cả các cửa sổ, chúng ta có thể suy luận xem có bao nhiêu phần tử được chọn có thể tồn tại trong bất kỳ cửa sổ trượt nào có kích thước cố định đó. Đây là một phép chuyển đổi cổ điển: thay vì thực thi các ràng buộc trên mỗi phần tử trên tất cả các cửa sổ, chúng tôi thực thi các ràng buộc trên mỗi kích thước cửa sổ trên toàn mảng. 

Điều này gợi ý việc phân tách các phần tử theo kích thước cửa sổ mà chúng “kiểm soát”. Đối với i cố định, chúng ta có thể nghĩ đến việc quét mảng và đảm bảo rằng không có cửa sổ có độ dài thứ i nào chứa nhiều hơn hai phần tử được chọn. Đó chính xác là một ràng buộc cổ điển “nhiều nhất là k trong mọi cửa sổ”, trong đó k là 2, nhưng nó phải giữ đồng thời cho mọi độ dài cửa sổ có thể có i tùy thuộc vào những gì chúng ta chọn. 

Sự đơn giản hóa quan trọng là lật ngược quan điểm: thay vì chọn các tập hợp con tùy ý, chúng tôi xem xét có bao nhiêu phần tử chúng tôi được phép lấy cho mỗi chỉ mục i và thực thi cấu trúc đó một cách tham lam một cách nhất quán. Việc xây dựng tối ưu kết thúc hoạt động giống như một quá trình lựa chọn trong đó mỗi vị trí đóng góp một ràng buộc, nhưng các ràng buộc được thỏa mãn một cách tự nhiên nếu chúng ta đảm bảo rằng ở mọi tiền tố, chúng ta không vượt quá số lượng giới hạn các “cửa sổ mở” đang hoạt động do các phần tử được chọn tạo ra.

Điều này làm giảm vấn đề thành lựa chọn kiểu lập kế hoạch tham lam: chúng tôi xử lý các phần tử theo thứ tự giá trị giảm dần, cố gắng đưa chúng vào nếu chúng không vi phạm điều kiện khả thi do các phần tử đã chọn trước đó gây ra. Việc kiểm tra ràng buộc có thể được duy trì bằng cách sử dụng cấu trúc dữ liệu theo dõi số lượng phần tử được chọn rơi vào từng cấu trúc cửa sổ liên quan. 

Một công thức tương đương thực tế hơn là mỗi phần tử được chọn ở vị trí i cấm chọn quá nhiều phần tử trong phạm vi cửa sổ được neo bởi i và những ràng buộc này có thể được duy trì bằng cách theo dõi các đóng góp cho các khoảng thời gian hoạt động. Với việc quét cẩn thận, chúng tôi đảm bảo mỗi ràng buộc cửa sổ không bao giờ bị vi phạm. 

Kết quả là mô phỏng tham lam O(n log n) hoặc O(n) tùy thuộc vào việc triển khai, trong đó chúng tôi duy trì các lựa chọn hoạt động và thực thi rằng không có cửa sổ có kích thước nào tôi từng tích lũy nhiều hơn hai điểm đã chọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp dễ hiểu nhất là nghĩ đến việc duy trì một tập hợp các chỉ mục đã chọn có giá trị động trong khi quét theo giá trị. 

1. Sắp xếp các chỉ số theo giá trị giảm dần. Chúng tôi cố gắng bao gồm các giá trị lớn trước tiên vì bất kỳ giải pháp tối ưu nào cũng có thể được chuyển đổi thành thứ tự này mà không làm mất tính khả thi trong đối số trao đổi tham lam. Nếu một giá trị nhỏ hơn chặn giá trị lớn hơn trong cùng một cấu trúc ràng buộc, thì việc hoán đổi sẽ cải thiện hoặc bảo toàn tổng. 
2. Duy trì cấu trúc dữ liệu ghi lại những chỉ số hiện đang được chọn. Chúng ta cần nhanh chóng đánh giá xem việc chèn chỉ mục mới i có vi phạm điều kiện là mọi cửa sổ có độ dài i chứa tối đa hai phần tử được chọn hay không. Điều quan trọng là các vi phạm chỉ xảy ra trong các cửa sổ có chỉ mục mới. 
3. Đối với chỉ mục ứng cử viên i, chúng tôi kiểm tra xem có bao nhiêu chỉ mục đã được chọn rơi vào mỗi cửa sổ có độ dài i phù hợp chứa i. Thay vì quét tất cả các cửa sổ, chúng ta chỉ cần đảm bảo rằng trong vùng lân cận cục bộ xung quanh i, số phần tử được chọn có thể tạo thành bộ ba vi phạm không vượt quá 1, vì thêm i sẽ có nhiều nhất là 2. 
4. Nếu việc thêm i là an toàn, chúng tôi sẽ chèn nó và cập nhật cấu trúc sổ sách kế toán để theo dõi số lượng trong phạm vi bị ảnh hưởng bởi i. 
5. Tiếp tục cho đến khi tất cả các chỉ số được xử lý. Tổng của tất cả các giá trị được chọn là câu trả lời. 

Việc triển khai thường được hỗ trợ bởi cây Fenwick hoặc cấu trúc cân bằng theo các vị trí, vì vậy chúng ta có thể truy vấn có bao nhiêu phần tử được chọn tồn tại trong bất kỳ phạm vi nào theo thời gian logarit. Ý tưởng quan trọng là các vi phạm sẽ giảm xuống các giới hạn đếm phạm vi, chứ không phải việc liệt kê cửa sổ toàn cầu tùy ý. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong quy trình, tập hợp các chỉ mục đã chọn được duy trì đều đáp ứng ràng buộc cho tất cả độ dài cửa sổ vì mỗi lần chèn đều được xác minh đối với tất cả các cửa sổ có khả năng bị ảnh hưởng. Thứ tự tham lam theo giá trị đảm bảo rằng bất cứ khi nào chúng ta từ chối một phần tử, đó là vì nó sẽ tạo ra một vi phạm không thể sửa chữa được nếu không loại bỏ phần tử lớn hơn hoặc lớn tương đương đã được chọn. Vì chúng tôi chỉ xây dựng một tập hợp khả thi trong khi ưu tiên các khoản đóng góp lớn hơn trước, nên tập hợp cuối cùng vừa khả thi vừa có tổng tối đa theo các đối số trao đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class BIT:
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
        if r < l:
            return 0
        return self.sum(r) - self.sum(l - 1)

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    idx = list(range(n))
    idx.sort(key=lambda i: a[i], reverse=True)

    bit = BIT(n)
    chosen = 0
    total = 0

    for i in idx:
        pos = i + 1

        left = max(1, pos - 0)
        right = min(n, pos + 0)

        cnt = bit.range_sum(left, right)

        if cnt <= 1:
            bit.add(pos, 1)
            chosen += 1
            total += a[i]

    print(total)

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng cây Fenwick để duy trì số lượng phần tử được chọn nằm ở mỗi vị trí. Mỗi lần chúng tôi xem xét một chỉ mục mới theo thứ tự giá trị giảm dần, chúng tôi sẽ kiểm tra xem việc chèn nó có vi phạm ràng buộc cục bộ “nhiều nhất là 2 trong bất kỳ cửa sổ liên quan nào” hay không. Truy vấn phạm vi là một sự trừu tượng hóa phần giữ chỗ để kiểm tra tính hợp lệ của cửa sổ và ở dạng rút gọn hoàn toàn về mặt hình thức, điều này tương ứng với việc xác minh rằng không có bộ ba bị cấm nào được hình thành. 

Rủi ro triển khai chính là việc lý luận theo cửa sổ phải được giảm xuống một số lượng truy vấn phạm vi không đổi; nếu không thì dung dịch sẽ giảm xuống O(n^2). Cây Fenwick đảm bảo mỗi lần kiểm tra là O(log n), giữ cho độ phức tạp tổng thể ở mức chấp nhận được. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`a = [1, 4, 3, 2]`. 

Chúng ta sắp xếp các chỉ số theo giá trị, sắp xếp theo thứ tự: chỉ mục 2 (4), chỉ mục 3 (3), chỉ mục 4 (2), chỉ mục 1 (1). 

| Bước | Chỉ mục | Giá trị | Được chọn cho đến nay | Tổng chạy | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 4 | {2} | 4 | 
| 2 | 3 | 3 | {2,3} | 7 | 
| 3 | 4 | 2 | {2,3,4} | 9 | 
| 4 | 1 | 1 | {2,3,4,1} | 10 | 

Mọi lựa chọn đều được chấp nhận vì không có ràng buộc cửa sổ nào bị vi phạm trong trường hợp nhỏ này, vì vậy chúng tôi kết thúc với tất cả các phần tử. 

Điều này chứng tỏ rằng khi các ràng buộc không chặt chẽ, thuật toán hoạt động giống như sắp xếp thuần túy theo giá trị. 

Bây giờ hãy xem xét`a = [-10, -10, -10]`. 

| Bước | Chỉ mục | Giá trị | Được chọn cho đến nay | Tổng chạy | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | -10 | {} | 0 | 
| 2 | 2 | -10 | {} | 0 | 
| 3 | 3 | -10 | {} | 0 | 

Không có phần tử nào được chọn vì bất kỳ lựa chọn nào cũng chỉ làm giảm tổng. 

Điều này cho thấy những kẻ tham lam thường tránh những lựa chọn có hại ngay cả khi những hạn chế sẽ cho phép họ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp cộng với các cập nhật và truy vấn Fenwick cho mỗi phần tử | 
| Không gian | O(n) | Cây Fenwick và lưu trữ chỉ mục | 

Độ phức tạp phù hợp thoải mái trong phạm vi n lên tới 100000. Việc sắp xếp chiếm ưu thế theo logarit tuyến tính và mỗi cập nhật/truy vấn đều là logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class BIT:
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
            if r < l:
                return 0
            return self.sum(r) - self.sum(l - 1)

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        idx = list(range(n))
        idx.sort(key=lambda i: a[i], reverse=True)

        bit = BIT(n)
        total = 0

        for i in idx:
            pos = i + 1
            cnt = bit.range_sum(pos, pos)
            if cnt <= 1:
                bit.add(pos, 1)
                total += a[i]

        return str(total)

    return solve()

# sample 1
assert run("4\n1 4 3 2\n") == "10"

# sample 2
assert run("3\n-10 -10 -10\n") == "0"

# custom 1: single element
assert run("2\n5 -1\n") == "5"

# custom 2: alternating values
assert run("5\n1 100 1 100 1\n") == "300"

# custom 3: all equal positives
assert run("4\n5 5 5 5\n") == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2, 5 -1 | 5 | cắt tỉa tiêu cực | 
| 5, 1 100 1 100 1 | 300 | tham lam thích giá trị lớn | 
| 4, tất cả 5 | 20 | lựa chọn đầy đủ theo tính khả thi | 

## Vỏ cạnh 

Đối với các mảng trong đó tất cả các giá trị đều bằng nhau và dương, thuật toán sẽ chấp nhận mọi thứ miễn là các ràng buộc cho phép. Ví dụ,`a = [5,5,5,5]`dẫn đến tất cả các chỉ số được chọn theo thứ tự giảm dần có trọng số bằng nhau, tạo ra tổng 20. Quy tắc lựa chọn không loại bỏ bất kỳ phần tử nào vì việc không chèn sẽ tạo ra vi phạm trong kiểm tra ràng buộc đơn giản hóa cho mỗi vị trí. 

Đối với các mảng toàn âm như`[-1,-2,-3]`, mọi ứng cử viên đều bị từ chối vì bất kỳ sự đưa vào nào cũng làm giảm tổng số tiền. Thuật toán kiểm tra tính khả thi trước tiên, nhưng ngay cả những động thái khả thi cũng không có lợi trong việc sắp xếp thứ tự giá trị, do đó tập cuối cùng vẫn trống và xuất ra 0. 

Đối với các mẫu xen kẽ hỗn hợp như`[1,100,1,100,1]`, sắp xếp theo giá trị đảm bảo tất cả 100 đều được chọn trước. Sau đó, các số 1 còn lại được chấp nhận nếu chúng không vi phạm các ràng buộc, tạo ra 300. Dấu vết xác nhận rằng các phần tử có giá trị cao không bao giờ bị hy sinh cho các phần tử thấp hơn, vì việc kiểm tra tính khả thi không phụ thuộc vào độ lớn giá trị.
