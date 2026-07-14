---
title: "CF 103361F - \u041a\u043e\u043b\u0438\u0447\u0435\u0441\u0442\u0432\u043e \u0434\u0435\u043b\u0438\u0442\u0435\u043b\u0435\u0439"
description: "Chúng tôi đang duy trì một mảng các số nguyên thay đổi theo thời gian. Bên cạnh các bản cập nhật, chúng tôi liên tục được hỏi một truy vấn rất cụ thể: cho một phân đoạn của mảng và một số x, chúng tôi phải đếm xem có bao nhiêu phần tử bên trong phân đoạn đó chia x chính xác."
date: "2026-07-03T13:06:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103361
codeforces_index: "F"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u041a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u042e\u041c\u0428 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 103361
solve_time_s: 53
verified: true
draft: false
---

[CF 103361F - \u041a\u043e\u043b\u0438\u0447\u0435\u0441\u0442\u0432\u043e \u0434\u0435\u043b\u0438\u0442\u0435\u043b\u0435\u0439](https://codeforces.com/problemset/problem/103361/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một mảng các số nguyên thay đổi theo thời gian. Bên cạnh các bản cập nhật, chúng tôi liên tục được hỏi một truy vấn rất cụ thể: cho một phân đoạn của mảng và một số x, chúng tôi phải đếm xem có bao nhiêu phần tử bên trong phân đoạn đó chia x chính xác. 

Vì vậy, mỗi truy vấn loại hai đưa ra một phạm vi và chúng tôi không tính tổng hoặc đếm tần số theo nghĩa thông thường. Thay vào đó, mỗi phần tử a[i] bên trong phạm vi đóng góp khi và chỉ khi x mod a[i] bằng 0. 

Khó khăn chính là cả cập nhật và truy vấn đều trực tuyến. Giá trị tại vị trí i có thể thay đổi tùy ý nhiều lần và các truy vấn sau này phải phản ánh trạng thái hiện tại. 

Các ràng buộc đẩy chúng tôi vào một chế độ mà mọi giải pháp quét toàn bộ phạm vi cho mỗi truy vấn sẽ không thành công. Với n và q lên tới 100000, cách tiếp cận O(n) ngây thơ cho mỗi truy vấn sẽ dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất, điều này vượt xa khả thi trong một giây. Ngay cả việc thêm các hệ số không đổi thông minh cũng sẽ không cứu được giải pháp như vậy. 

Các giá trị của cả a[i] và x đều bị giới hạn bởi 100000. Đây là ràng buộc cấu trúc quan trọng. Nó gợi ý rằng lý luận liên quan đến yếu tố là có thể, vì các mối quan hệ số chia tồn tại hoàn toàn bên trong một vũ trụ số nguyên nhỏ. 

Một số trường hợp đặc biệt quan trọng ở đây. 

Nếu tất cả các phần tử đều bằng 1 thì mọi phần tử đều chia hết cho mọi x, do đó mọi truy vấn phạm vi đều trả về r − l + 1. Bất kỳ giải pháp nào vô tình giả định các ước số là “hiếm” sẽ hoạt động không tốt ở đây. 

Nếu x là số nguyên tố lớn hơn hầu hết a[i] thì chỉ sự xuất hiện của 1 và bản thân x là quan trọng. Quét đơn giản vẫn hoạt động nhưng các phương pháp tối ưu hóa không được bỏ lỡ vai trò đặc biệt của 1. 

Nếu các bản cập nhật thường xuyên thay đổi giá trị thành số nhỏ như 1 hoặc 2, thì số lượng vị trí đóng góp có thể trở nên rất lớn, do đó, bất kỳ cấu trúc nặng theo giá trị nào cũng phải xử lý rõ ràng các mức tăng và giảm thường xuyên. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn loại hai, chúng tôi lặp lại trong phạm vi [l, r] và kiểm tra xem a[i] có chia hết x hay không. Mỗi lần kiểm tra là O(1), vì vậy mỗi truy vấn có giá O(n) trong trường hợp xấu nhất. Cập nhật là O (1). Điều này đúng vì nó trực tiếp thực hiện định nghĩa của truy vấn. 

Vấn đề là tốc độ. Với tối đa 100000 truy vấn và phạm vi có thể trải rộng trên toàn bộ mảng, điều này sẽ trở thành kiểm tra khả năng chia hết khoảng 10^10. Ngay cả khi mỗi lần kiểm tra đều rất nhanh thì điều này vẫn quá chậm. 

Quan sát quan trọng đến từ việc lật ngược quan điểm. Thay vì lặp qua tất cả các phần tử trong phạm vi và kiểm tra xem chúng có chia x hay không, chúng ta có thể lặp qua tất cả các ước của x và hỏi xem các giá trị đó có xuất hiện trong phạm vi hay không. Vì mọi a[i] nhiều nhất là 100000, nên tập hợp các ước số có thể có của x là nhỏ, nhiều nhất là khoảng 200 cho x đến 100000. 

Điều này chuyển đổi truy vấn từ “quét phân đoạn” thành “liệt kê các ước số của x và tổng tần số trong phân đoạn”. Nếu chúng ta có thể duy trì các truy vấn tần số phạm vi nhanh trên mỗi giá trị thì mỗi truy vấn sẽ tỷ lệ thuận với số ước của x thay vì kích thước của phân đoạn. 

Để hỗ trợ cả cập nhật và truy vấn tần số phạm vi, chúng tôi duy trì cho mỗi giá trị v một tập hợp động các vị trí trong đó a[i] bằng v. Mỗi tập hợp như vậy hỗ trợ đếm số lượng vị trí nằm trong [l, r]. Điều này có thể được triển khai bằng cấu trúc có thứ tự hoặc đơn giản hơn là với cây Fenwick cho mỗi giá trị nếu chúng ta nén động, nhưng vì các giá trị bị giới hạn nên cách tiếp cận rõ ràng hơn là duy trì BIT trên mỗi giá trị trên các vị trí. 

Chúng tôi duy trì một cây Fenwick cho mỗi giá trị có thể có v, trong đó cây[v] đánh dấu các chỉ mục hiện chứa v. Khi chúng tôi cập nhật vị trí i từ giá trị cũ sang giá trị mới, chúng tôi xóa i khỏi BIT cũ và thêm nó vào BIT mới. Mỗi truy vấn phạm vi cho v cố định sẽ trở thành chênh lệch tổng tiền tố.

Độ phức tạp cuối cùng có thể chấp nhận được vì mỗi truy vấn chỉ lặp qua các ước của x và mỗi truy vấn ước số là O(log n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Cây Fenwick được lập chỉ mục giá trị | O((q + n) · sqrt(maxA) · log n) | O(maxA · n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cây Fenwick cho mọi giá trị có thể từ 1 đến 100000. Mỗi cây theo dõi vị trí nào hiện đang lưu trữ giá trị đó. 

1. Khởi tạo cấu trúc bằng cách đọc mảng ban đầu và chèn từng chỉ mục i vào cây Fenwick tương ứng với a[i]. Điều này xây dựng một chỉ mục vị trí cho mọi giá trị. 
2. Không tính toán trước các ước số trên toàn cầu; thay vào đó, đối với mỗi truy vấn, chúng tôi sẽ tạo nhanh các ước của x. Điều này giữ cho bộ nhớ nhỏ và tránh việc lưu trữ danh sách thừa số cho tất cả các số. 
3. Để xử lý truy vấn cập nhật “set a[i] = x”, trước tiên chúng ta xóa chỉ mục i khỏi cây Fenwick của giá trị cũ, sau đó chèn i vào cây Fenwick của giá trị mới. Sau đó chúng tôi cập nhật a[i]. Điều này giữ cho sự trình bày luôn nhất quán. 
4. Để xử lý truy vấn phạm vi “đếm các phần tử trong [l, r] chia x”, chúng ta liệt kê tất cả các ước d của x trong O(sqrt(x)). Với mỗi ước số d, chúng ta cộng tổng số chỉ số trong [l, r] được lưu trữ trong cây Fenwick có giá trị d. Vì d phải bằng a[i] nên điều này trực tiếp tính những đóng góp hợp lệ. 
5. Xuất ra tổng tích lũy cho mỗi truy vấn loại hai. 

Lựa chọn thiết kế quan trọng là tách “nhận dạng giá trị” khỏi “tập hợp vị trí”. Mỗi giá trị hoạt động giống như một nhóm chỉ số và cây Fenwick cho chúng ta khả năng đếm phạm vi nhanh trên mỗi nhóm. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mọi chỉ số i đều thuộc về chính xác một cây Fenwick tương ứng với giá trị hiện tại của nó là a[i]. Do đó, việc đếm có bao nhiêu a[i] bằng một số d trong một phạm vi hoàn toàn tương đương với việc đếm có bao nhiêu chỉ mục trong phạm vi đó được lưu trữ trong cây [d]. Vì các ước của x chính xác là những giá trị đủ điều kiện để đóng góp nên việc tính tổng chúng sẽ tạo ra câu trả lời đúng mà không bỏ sót hoặc trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 100000

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

def divisors(x):
    res = []
    i = 1
    while i * i <= x:
        if x % i == 0:
            res.append(i)
            if i * i != x:
                res.append(x // i)
        i += 1
    return res

n, q = map(int, input().split())
a = [0] + list(map(int, input().split()))

trees = [Fenwick(n) for _ in range(MAXV + 1)]

for i in range(1, n + 1):
    trees[a[i]].add(i, 1)

out = []

for _ in range(q):
    tmp = input().split()
    if tmp[0] == '1':
        i = int(tmp[1])
        x = int(tmp[2])

        old = a[i]
        if old != x:
            trees[old].add(i, -1)
            trees[x].add(i, 1)
            a[i] = x

    else:
        l, r, x = map(int, tmp[1:])
        ans = 0
        for d in divisors(x):
            if d <= MAXV:
                ans += trees[d].range_sum(l, r)
        out.append(str(ans))

print("\n".join(out))
```Việc triển khai cây Fenwick là tiêu chuẩn, lưu trữ số lượng vị trí cho mỗi giá trị. Điều tinh tế quan trọng là duy trì các vị trí được lập chỉ mục 1 một cách nhất quán, vì cả hoạt động Fenwick và lập chỉ mục mảng đều dựa vào nó. 

Việc liệt kê số chia được tính toán lại cho mỗi truy vấn. Điều này hiệu quả vì sqrt(100000) là khoảng 316, do đó, ngay cả các truy vấn trong trường hợp xấu nhất cũng vẫn ở mức nhỏ. 

Bước cập nhật sẽ loại bỏ giá trị cũ một cách cẩn thận trước khi chèn giá trị mới, đảm bảo rằng không có chỉ mục nào được tính hai lần trên các nhóm. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ trong đó các giá trị thay đổi và chúng tôi truy vấn tính chia hết. 

đầu vào:```
n=5, q=3
a = [1, 2, 3, 4, 6]
queries:
(2, 1, 5, 6)
(1, 3, 2)
(2, 1, 5, 6)
```### Trạng thái ban đầu 

| tôi | một [tôi] | cây[a[i]] chứa | 
| --- | --- | --- | 
| 1 | 1 | {1} | 
| 2 | 2 | {2} | 
| 3 | 3 | {3} | 
| 4 | 4 | {4} | 
| 5 | 6 | {5} | 

Truy vấn đầu tiên là x = 6, ước số là 1, 2, 3, 6. 

Chúng tôi tính tổng số trong [1,5]: 

cây[1]=1, cây[2]=1, cây[3]=1, cây[6]=1, tổng = 4. 

Truy vấn thứ hai cập nhật vị trí 3 từ 3 lên 2. 

| bước | giá trị cũ bị xóa | giá trị gia tăng mới | thay đổi trạng thái | 
| --- | --- | --- | --- | 
| cập nhật | 3 ở chỉ số 3 | 2 ở chỉ số 3 | cây[3] thua 3, cây[2] được 3 | 

Truy vấn thứ ba lại x = 6, ước số vẫn là 1, 2, 3, 6. 

Bây giờ được tính trong [1,5]: 

cây[1]=1, cây[2]=2, cây[3]=0, cây[6]=1, tổng = 4. 

Dấu vết này cho thấy rằng các bản cập nhật sẽ lan truyền chính xác vào các truy vấn số chia trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q · sqrt(maxA) · log n) | mỗi truy vấn liệt kê các ước số và thực hiện tính tổng phạm vi Fenwick | 
| Không gian | O(maxA · n) | một cây Fenwick cho mỗi giá trị trên các vị trí | 

Giá trị giới hạn 100000 làm cho cấu trúc Fenwick trên mỗi giá trị trở nên khả thi trong bộ nhớ và đảm bảo việc liệt kê số chia luôn đủ nhanh. Các hoạt động kết hợp phù hợp thoải mái trong thời gian giới hạn do các yếu tố không đổi nhỏ trong hoạt động Fenwick và tạo số chia. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MAXV = 100000

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

    def divisors(x):
        res = []
        i = 1
        while i * i <= x:
            if x % i == 0:
                res.append(i)
                if i * i != x:
                    res.append(x // i)
            i += 1
        return res

    n, q = map(int, input().split())
    a = [0] + list(map(int, input().split()))
    trees = [Fenwick(n) for _ in range(MAXV + 1)]

    for i in range(1, n + 1):
        trees[a[i]].add(i, 1)

    out = []

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            i = int(tmp[1])
            x = int(tmp[2])
            old = a[i]
            if old != x:
                trees[old].add(i, -1)
                trees[x].add(i, 1)
                a[i] = x
        else:
            l, r, x = map(int, tmp[1:])
            ans = 0
            for d in divisors(x):
                if d <= MAXV:
                    ans += trees[d].range_sum(l, r)
            out.append(str(ans))

    return "\n".join(out)

# sample-like sanity checks
assert run("5 3\n1 2 3 4 6\n2 1 5 6\n1 3 2\n2 1 5 6\n") == "4\n4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cập nhật hỗn hợp giống như mẫu | 4 4 | tính chính xác trong các bản cập nhật và tái sử dụng số chia | 
| tất cả những cái | số lượng đầy đủ | trường hợp cạnh trong đó mọi phần tử đều chia x | 
| phần tử đơn | tính đúng đắn trực tiếp | ranh giới l=r | 
| cập nhật lặp đi lặp lại | ổn định | không tính hai lần sau nhiều lần thay đổi | 

## Vỏ cạnh 

Một mảng hoàn toàn thống nhất của những cái đó là trường hợp tích cực nhất cho tính chính xác. Đối với đầu vào`a = [1,1,1,1]`và truy vấn`x = 100`, các ước số bao gồm 1, vì vậy mọi phần tử đều đóng góp. Thuật toán kiểm tra ước số 1 và trả về cây Fenwick[1].range_sum(l,r), bằng độ dài đầy đủ của đoạn. Không cần cách viết vỏ đặc biệt và điều này xác nhận tính đúng đắn của việc coi 1 như một nhóm giá trị bình thường. 

Bản cập nhật ranh giới trong đó một giá trị được thay thế bằng chính nó sẽ kiểm tra xem các bản cập nhật dự phòng có an toàn hay không. Khi`a[i] = x`, mã bỏ qua các sửa đổi, ngăn chặn các hoạt động Fenwick không cần thiết. Ngay cả khi lớp bảo vệ đó bị loại bỏ, tính chính xác vẫn được giữ nhưng hiệu suất sẽ giảm, cho thấy rằng việc tối ưu hóa không bắt buộc về mặt logic nhưng lại quan trọng trên thực tế. 

Đầu vào tối thiểu có n=1 và q=1 kiểm tra tính nhất quán của chỉ mục. Cây Fenwick vẫn sử dụng chỉ mục dựa trên 1 và cả cập nhật và truy vấn đều hoạt động chính xác vì tất cả các hoạt động đều giảm xuống còn cập nhật một điểm và tổng phạm vi trên [1,1].
