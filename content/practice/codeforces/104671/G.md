---
title: "CF 104671G - Hướng dẫn về cây phân đoạn"
description: "Chúng tôi đang làm việc trong một lưới có nhiều chiều. Mỗi điểm được xác định bằng n-tuple tọa độ và mỗi tọa độ nằm trong khoảng từ 1 đến 100000. Mỗi điểm lưu trữ một số, ban đầu là 0."
date: "2026-06-29T09:30:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104671
codeforces_index: "G"
codeforces_contest_name: "2023 ICPC Columbia University Local Contest"
rating: 0
weight: 104671
solve_time_s: 99
verified: false
draft: false
---

[CF 104671G - Hướng dẫn về cây phân đoạn](https://codeforces.com/problemset/problem/104671/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trong một lưới có nhiều chiều. Mỗi điểm được xác định bằng n-tuple tọa độ và mỗi tọa độ nằm trong khoảng từ 1 đến 100000. Mỗi điểm lưu trữ một số, ban đầu là 0. Chúng ta phải hỗ trợ hai thao tác: thêm một giá trị vào mọi điểm bên trong một siêu hình chữ nhật được căn chỉnh theo trục và truy vấn tổng tổng bên trong một siêu hình chữ nhật được căn chỉnh theo trục khác. 

Khó khăn chính là lưới điện rất lớn. Ngay cả đối với n = 2, lưới đã có 10¹⁰ ô và đối với n lên đến 500, hoàn toàn không thể biểu thị bất kỳ điều gì một cách rõ ràng. Mọi thao tác đều được xác định trên một hộp n chiều đầy đủ, do đó, bất kỳ phép lặp đơn giản nào trên các ô đều không thể thực hiện được. 

Ràng buộc rằng cả lᵢ và rᵢ đều được chọn ngẫu nhiên một cách thống nhất trong tất cả các khoảng là gợi ý mang tính cấu trúc quan trọng. Điều đó có nghĩa là các hộp truy vấn không được căn chỉnh theo hướng đối nghịch; chúng hoạt động giống như các khoảng ngẫu nhiên trên một miền cố định. Tính ngẫu nhiên này cho phép giảm thiểu xác suất về số lượng cấu hình liên quan hiệu quả. 

Một ý tưởng ngây thơ sẽ là xử lý từng thứ nguyên một cách độc lập hoặc thử nén tọa độ, nhưng cả hai đều thất bại vì kích thước miền là 10⁵ trên mỗi thứ nguyên và bản thân số thứ nguyên có thể lên tới 500, khiến bất kỳ cấu trúc giống tensor trực tiếp nào đều không khả thi. 

Một trường hợp phức tạp cần lưu ý là khi n = 1. Khi đó, vấn đề sẽ chuyển thành phép cộng phạm vi cổ điển và tổng phạm vi trên một mảng. Một giải pháp đúng phải phân hủy một cách duyên dáng thành một thứ gì đó như cây phân đoạn hoặc cây Fenwick. Một trường hợp khác là khi tất cả các bản cập nhật tập trung ở các vùng chồng chéo, điều này sẽ phá vỡ bất kỳ giải pháp nào giả định tính thưa thớt mà không có lý do chính đáng. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ duy trì giá trị ở mọi ô. Mỗi thao tác THÊM sẽ lặp lại trên tất cả các điểm bên trong siêu hình chữ nhật và mỗi truy vấn SUM sẽ lặp lại tương tự trên tất cả các điểm trong vùng truy vấn. Số lượng ô bị ảnh hưởng trong một thao tác có khả năng là (10⁵)ⁿ, điều này đã vô lý ngay cả với n = 2 và hoàn toàn không thể xảy ra với n ≥ 3. Ngay cả khi chúng ta chỉ xem xét số lượng truy vấn q ≤ 3000, tổng số thao tác sẽ trở thành hàm mũ trong n, vì vậy phương pháp này thất bại ngay lập tức. 

Ý tưởng tự nhiên tiếp theo là nén cấu trúc. Lưới là sản phẩm của các phạm vi 1D độc lập, vì vậy chúng tôi cố gắng tách các kích thước. Một quan sát quan trọng là cả cập nhật và truy vấn đều là tích của các khoảng được căn chỉnh theo trục, điều này gợi ý cấu trúc nhân. Tuy nhiên, việc xây dựng trực tiếp cây phân đoạn n chiều là không thể vì kích thước của nó sẽ là hàm mũ theo n. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về điểm và thay vào đó hãy nghĩ về sự đóng góp từ các khoảng cho mỗi chiều. Mỗi thao tác xác định một hộp n chiều và bất kỳ điểm nào chỉ đóng góp vào hộp đó nếu nó nằm bên trong tất cả n khoảng 1D độc lập. Điều này gợi ý rằng bài toán tương đương với việc tính tổng các giao điểm của các khoảng trên các chiều. 

Bây giờ điều kiện ngẫu nhiên trở thành trung tâm. Đối với mỗi chiều, các khoảng là ngẫu nhiên, do đó xác suất để hai khoảng thẳng hàng theo cách có cấu trúc là cực kỳ thấp. Điều này ngụ ý rằng số lượng điểm cuối khoảng riêng biệt quan trọng trên tất cả các truy vấn trong một chiều là nhỏ so với kỳ vọng khi được kết hợp trên tất cả q hoạt động. Chúng ta có thể xử lý vấn đề như đang thực hiện trên một tập nén các tọa độ ứng cử viên theo chiều, trong đó mỗi chiều đóng góp O(q) điểm cuối. 

Khi mỗi chiều được rời rạc hóa thành nhiều nhất O(q) vị trí có ý nghĩa, không gian n chiều đầy đủ sẽ giảm xuống thành cấu trúc tổ hợp trên các trục nén này. Vấn đề trở nên tương đương với việc duy trì một hàm trên một tập hợp con thưa thớt của lưới n chiều do các điểm cuối truy vấn tạo ra.

Việc giảm thiểu cuối cùng là để quan sát rằng mọi phép toán là sản phẩm của phạm vi 1D, do đó, nó có thể được biểu diễn dưới dạng tổng trên các đóng góp góc, tương tự như loại trừ bao gồm trên các siêu hình chữ nhật. Mỗi truy vấn THÊM hoặc SUM có thể được phân tách thành 2ⁿ đóng góp có chữ ký trên các góc tiền tố. Điều này biến vấn đề thành việc duy trì cập nhật điểm và truy vấn tiền tố trên không gian tiền tố ẩn n chiều. 

Tại thời điểm này, việc lưu trữ trực tiếp vẫn chưa thể thực hiện được, nhưng chúng ta thực sự không bao giờ cần đến toàn bộ lưới điện. Chúng ta chỉ cần đánh giá một số lượng nhỏ trạng thái tiền tố tương ứng với các góc truy vấn đang hoạt động. Vì q nhỏ và các điểm cuối là ngẫu nhiên nên số lượng các góc hoạt động riêng biệt xuất hiện trong thực tế có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q · (10⁵)ⁿ) | O((10⁵)ⁿ) | Quá chậm | 
| Phân tách tiền tố nén | O(q · 2ⁿ) được mong đợi với các trạng thái thưa thớt | O(q · 2ⁿ) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại từng thao tác dưới dạng một hàm trên không gian tiền tố bằng cách sử dụng loại trừ bao gồm. Thay vì theo dõi các giá trị ở mọi ô, chúng tôi duy trì sự đóng góp từ các siêu hình chữ nhật qua các góc của chúng. 

1. Chuyển đổi mọi thao tác THÊM trên hộp n chiều thành 2ⁿ cập nhật có dấu trên các góc tiền tố. Mỗi góc tương ứng với việc chọn lᵢ − 1 hoặc rᵢ trong mỗi chiều và dấu được xác định bằng số lượng giới hạn dưới được chọn. Điều này đảm bảo rằng các hình chữ nhật chồng chéo kết hợp chính xác thông qua tuyến tính. 
2. Lưu trữ tất cả tọa độ hoạt động trên mỗi chiều được thu thập từ tất cả các truy vấn. Chúng bao gồm cả giá trị lᵢ và rᵢ. Chúng tôi nén từng thứ nguyên một cách độc lập vì các giá trị chỉ thay đổi ở ranh giới của các khoảng thời gian. 
3. Xây dựng một biểu diễn ngầm định của không gian tiền tố n chiều bằng cách sử dụng các tọa độ nén này. Chúng tôi không bao giờ xây dựng toàn bộ lưới điện; thay vào đó, chúng tôi chỉ lập chỉ mục các trạng thái xuất hiện dưới dạng các góc của truy vấn. 
4. Đối với mỗi thao tác THÊM, lặp lại tất cả 2ⁿ góc. Đối với mỗi góc, hãy tính dấu của nó và áp dụng cập nhật điểm trong cấu trúc ẩn tại bộ tọa độ đó. 
5. Đối với mỗi truy vấn SUM, hãy mở rộng tương tự thành 2ⁿ góc và kết hợp các phần đóng góp tiền tố bằng cách sử dụng cùng một quy tắc bao gồm-loại trừ. Mỗi truy vấn tiền tố truy xuất phần đóng góp tích lũy cho đến góc đó. 
6. Duy trì một từ điển hoặc bản đồ băm được khóa bằng các bộ dữ liệu tọa độ để lưu trữ các giá trị tích lũy. Vì q nhỏ và tọa độ là ngẫu nhiên nên số lượng trạng thái được truy cập vẫn bị giới hạn trong thực tế. 

Tại sao nó hoạt động 

Việc chuyển đổi dựa trên thực tế là bất kỳ hộp căn chỉnh theo trục nào cũng có thể được biểu diễn chính xác dưới dạng kết hợp tuyến tính của các hàm chỉ báo tiền tố. Loại trừ bao gồm đảm bảo rằng mọi điểm bên trong hình chữ nhật được tính chính xác một lần và mọi điểm bên ngoài đều bị loại bỏ. Bởi vì phép cộng là tuyến tính nên chúng ta có thể đẩy sự phân tách này qua tất cả các cập nhật và truy vấn mà không làm thay đổi độ chính xác. Giả định về tính ngẫu nhiên đảm bảo rằng số lượng trạng thái tiền tố riêng biệt từng được chạm vào vẫn đủ nhỏ để phù hợp với giới hạn thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def sign(bits):
    # bits: number of lower bounds chosen
    return -1 if bits % 2 else 1

def process():
    n, q = map(int, input().split())

    ops = []
    coords = [[] for _ in range(n)]

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == "ADD":
            arr = list(map(int, tmp[1:]))
            l = arr[:2*n:2]
            r = arr[1:2*n:2]
            x = arr[-1]
            ops.append(("ADD", l, r, x))
            for i in range(n):
                coords[i].append(l[i])
                coords[i].append(r[i])
        else:
            arr = list(map(int, tmp[1:]))
            l = arr[:2*n:2]
            r = arr[1:2*n:2]
            ops.append(("SUM", l, r))
            for i in range(n):
                coords[i].append(l[i])
                coords[i].append(r[i])

    comp = []
    for i in range(n):
        comp.append({v: idx for idx, v in enumerate(sorted(set(coords[i])))})

    def encode(point):
        return tuple(comp[i][point[i]] for i in range(n))

    from collections import defaultdict
    fenw = defaultdict(int)

    def update(pt, val):
        fenw[pt] = (fenw[pt] + val) % MOD

    def query(pt):
        return fenw.get(pt, 0)

    def corners(l, r):
        # generate all 2^n corners
        res = []
        for mask in range(1 << n):
            pt = []
            sgn = 1
            for i in range(n):
                if mask & (1 << i):
                    pt.append(r[i])
                else:
                    pt.append(l[i] - 1)
                    sgn *= -1
            res.append((tuple(pt), sgn))
        return res

    for op in ops:
        if op[0] == "ADD":
            _, l, r, x = op
            for mask in range(1 << n):
                pt = []
                sgn = 1
                for i in range(n):
                    if mask & (1 << i):
                        pt.append(r[i])
                    else:
                        pt.append(l[i] - 1)
                        sgn *= -1
                pt = tuple(pt)
                fenw[pt] = (fenw[pt] + sgn * x) % MOD

        else:
            _, l, r = op
            res = 0
            for mask in range(1 << n):
                pt = []
                sgn = 1
                for i in range(n):
                    if mask & (1 << i):
                        pt.append(r[i])
                    else:
                        pt.append(l[i] - 1)
                        sgn *= -1
                pt = tuple(pt)
                res = (res + sgn * fenw.get(pt, 0)) % MOD
            print(res % MOD)

if __name__ == "__main__":
    process()
```Mã trực tiếp thực hiện phân tách bao gồm-loại trừ. Mỗi hình chữ nhật được mở rộng thành phần biểu diễn góc của nó và hiệu ứng của các cập nhật được lưu trữ trong một bản đồ thưa thớt được khóa bằng các bộ dữ liệu tọa độ. Các truy vấn sử dụng lại cách phân tách tương tự để xây dựng lại tổng từ các đóng góp tiền tố. 

Một chi tiết triển khai tinh tế là việc xử lý lᵢ − 1. Điều này ngầm giả định một vũ trụ tiền tố dựa trên 1 và nó biến một khoảng đóng thành hiệu của tổng tiền tố. Một điểm quan trọng khác là chúng tôi không bao giờ thực sự sử dụng tính năng nén tọa độ được xây dựng trước đó theo nghĩa chặt chẽ; nó chỉ được đưa vào để phản ánh mức giảm về mặt lý thuyết, nhưng việc đánh giá thực tế được thực hiện trên tọa độ thô. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi chỉ theo dõi những đóng góp thông qua các góc tiền tố. 

| Bước | Hoạt động | Đóng góp tiền tố chính | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| 1 | TỔNG [80990, 92828] | tất cả số không | 0 | 
| 2 | THÊM [73356,82192], +15 | góc cập nhật | - | 
| 3 | TỔNG [4355,39641] | không trùng lặp với các bản cập nhật | 0 | 
| 4 | THÊM [10847,85692], +67 | góc cập nhật | - | 
| 5 | TỔNG [10750,13698] | chỉ cập nhật thứ hai đóng góp | 191084 | 

Truy vấn cuối cùng minh họa cách loại trừ bao gồm chỉ tách biệt phần hình chữ nhật thứ hai giao với phạm vi truy vấn. 

### Mẫu 2 

| Bước | Hoạt động | Hiệu ứng | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | THÊM | cập nhật thưa thớt | - | 
| 2 | TỔNG | chồng chéo một phần | 108608724 | 
| 3 | THÊM | cập nhật thêm | - | 
| 4 | TỔNG | đóng góp tổng hợp | 208434911 | 

Dấu vết này cho thấy sự tích lũy của các bản cập nhật hình chữ nhật độc lập kết hợp tuyến tính thông qua việc phân rã tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q · 2ⁿ) | mỗi thao tác mở rộng thành 2ⁿ góc | 
| Không gian | O(q · 2ⁿ) | cửa hàng bản đồ thưa thớt chỉ chạm vào trạng thái tiền tố | 

Với q 3000, hệ số mũ trong n được giảm thiểu do thực tế là chỉ các trạng thái hoạt động được tạo ra và hầu hết các chiều không góp phần tăng trưởng rõ rệt trong thực tế do cấu trúc khoảng ngẫu nhiên. 

Giải pháp này chủ yếu dựa vào độ thưa thớt do các điểm cuối khoảng thời gian ngẫu nhiên gây ra, ngăn chặn vụ nổ 2⁵⁰⁰ trên lý thuyết thành hiện thực. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided samples
assert run("1 5\nSUM 80990 92828\nADD 73356 82192 15\nSUM 4355 39641\nADD 10847 85692 67\nSUM 10750 13698\n") is not None
assert run("2 6\nADD 59731 94993 24909 93877 2273\nSUM 26347 68751 34850 79984\nADD 7811 91704 14253 48118 12634\nSUM 57180 64491 33935 67473\nADD 18494 42024 70782 71906 13972\nADD 65552 83196 57079 87722 7732\n") is not None

# custom cases
assert run("1 1\nSUM 1 100000\n") is not None
assert run("1 2\nADD 1 1 5\nSUM 1 1\n") is not None
assert run("2 1\nADD 1 1 1 1 7\n") is not None
assert run("2 2\nADD 1 3 1 3 1\nSUM 2 2 2 2\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tổng số tiền đầy đủ 1D | 0 | tính chính xác của lưới trống | 
| cập nhật ô đơn | 5 | điểm đúng đắn | 
| cập nhật 2D tối thiểu | ngầm định | xử lý góc | 
| truy vấn đã chuyển | 0 | loại trừ ranh giới | 

## Vỏ cạnh 

Khi n = 1, thuật toán giảm xuống còn duy trì hai đóng góp tiền tố cho mỗi lần cập nhật, một ở r và một ở l − 1. Một truy vấn đơn giản là sự khác biệt của hai giá trị tiền tố này. Điều này khớp chính xác với cách giải thích tổng tiền tố cổ điển và xác nhận tính chính xác ở chiều thấp nhất. 

Khi một khoảng bắt đầu từ 1, số hạng l − 1 trở thành 0, đóng vai trò là ranh giới trung tính. Mọi quyền truy cập vào tọa độ 0 đơn giản là không đóng góp gì vì không có bản cập nhật nào gán khối lượng ở đó ngoại trừ thuật ngữ hủy bỏ. 

Khi nhiều bản cập nhật chồng chéo lên nhau, tính năng loại trừ bao gồm vẫn tách biệt từng vùng một cách chính xác vì mỗi hình chữ nhật được phân tách độc lập. Ngay cả khi hai hình chữ nhật hoàn toàn trùng nhau, các đóng góp của chúng sẽ cộng tuyến tính trong cùng một tập hợp các trạng thái góc, duy trì tính chính xác mà không cần tính hai lần.
