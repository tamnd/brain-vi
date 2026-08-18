---
title: "CF 102268H - Định lý Hall"
description: "Chúng ta cần xây dựng một đồ thị hai bên có đúng n đỉnh ở mỗi cạnh. Đối với tập con A của các đỉnh bên trái, lân cận N(A) của nó bao gồm mọi đỉnh phải tiếp xúc với ít nhất một đỉnh của A."
date: "2026-08-17T18:51:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102268
codeforces_index: "H"
codeforces_contest_name: "300iq Contest 1"
rating: 0
weight: 102268
solve_time_s: 218
verified: false
draft: false
---

[CF 102268H - Định lý Hall](https://codeforces.com/problemset/problem/102268/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một đồ thị hai bên có đúng n đỉnh ở mỗi cạnh. Đối với tập con A gồm các đỉnh bên trái, lân cận N(A) của nó bao gồm mọi đỉnh bên phải được chạm bởi ít nhất một đỉnh của A. Tập con này rất quan trọng khi nó yêu cầu nhiều đỉnh bên phải khác biệt hơn mức thực tế có sẵn, nghĩa là ∣N(A)∣<∣A∣. 

Đầu vào cho ra n, với n<20, và số mục tiêu k, với 0<k<2 n. Có 2 n tập con ở vế trái, kể cả tập con rỗng. Tập con trống không bao giờ quan trọng vì cả kích thước của nó và kích thước vùng lân cận đều bằng 0. Đồ thị của chúng ta phải làm cho chính xác k trong số 2 n −1 tập con còn lại là tới hạn. 

Giá trị nhỏ n 20 là một gợi ý mạnh mẽ rằng việc liệt kê tập hợp con có thể được xác minh nhưng việc tìm kiếm tự do trên các biểu đồ là không đủ. Một đồ thị hai phần trên n+n đỉnh có n 2 cạnh có thể, do đó việc liệt kê các đồ thị sẽ cần 2 n 2 khả năng, vốn đã là 2 400 khi n=20. Chúng ta cần một cấu trúc trực tiếp mà mô tả của nó chỉ có kích thước đa thức. 

Có hai trường hợp ranh giới đặc biệt dễ xử lý sai. Với n=1,k=0, đồ thị phải chứa một cạnh. Tập con duy nhất không rỗng thì có một tập con lân cận và không quan trọng. Thay vào đó, một cấu trúc bất cẩn không tạo ra cạnh nào sẽ làm cho tập hợp con duy nhất trở nên quan trọng. Với n=1,k=1, đồ thị đúng không có cạnh, vì tập con đơn bên trái không có lân cận nào và rất quan trọng. Lỗi phổ biến khác là coi tập hợp con trống là quan trọng. Không bao giờ như vậy, vì vậy số tập con không rỗng không tới hạn là 2 n −1−k, không phải 2 n −k. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu trực tiếp có thể lấy biểu đồ ứng cử viên và liệt kê mọi tập hợp con của phía bên trái. Đối với mỗi tập hợp con, chúng ta có thể quét các đỉnh của nó và thu thập các đỉnh bên phải của chúng, sau đó so sánh hai số lượng chính. Nếu đồ thị được biểu diễn bằng ma trận kề n×n thì việc này thực hiện các phép toán O(n 2 2 n ). Với n=20, tức là khoảng 400⋅1.048.576, hay khoảng 4,2×10 8 phép kiểm cơ bản. Việc tìm kiếm chính đồ thị bằng cách liệt kê tất cả các tập cạnh có thể có sẽ tệ hơn rất nhiều, vì có thể có 2 n 2 đồ thị. 

Quan sát hữu ích là chúng ta không cần một biểu đồ tùy ý. Chúng ta có thể cố ý xây dựng một họ rất hạn chế mà các tập con quan trọng của nó dễ đếm. 

Gọi các đỉnh bên trái là 1,…,n và chọn số nguyên 

n ≥a 1 ​ ≥a 2 ​ ≥⋯ ≥a n ​ ≥0. 

Nối đỉnh bên trái i với đỉnh a i đầu tiên bên phải. Xét một tập con A khác rỗng và gọi i là đỉnh trái nhỏ nhất của nó. Vì dãy a i ​ không tăng nên mọi đỉnh được chọn khác t>i đều có t ​ ≤a i ​. Do đó hợp của tất cả các lân cận của chúng chính xác là đỉnh a i đầu tiên bên phải, vì vậy 

∣N(A)∣=a i ​ . 

Có n-i đỉnh sau i. Nếu chính xác j trong số chúng được chọn thì tập con đó có kích thước j+1, và nó không tới hạn chính xác khi 

j+1<a tôi ​ , 

hoặc j<a i ​ −1. Do đó số tập con không tới hạn có đỉnh nhỏ nhất là i là 

j=0 ∑ a i ​ −1 ​ ( j n−i ​ ). 

Tổng hợp tất cả các đỉnh nhỏ nhất có thể cho 

G= i=1 ∑ n ​ j=0 ∑ a i ​ −1 ​ ( j n−i ​ ), 

trong đó G là số tập con không rỗng, không tới hạn. Vì có 2 n −1 tập con khác rỗng nên ta cần 

G=2 n −1−k. 

Câu hỏi còn lại là làm thế nào để chọn a i ​. Tổng bên trong là tiền tố của một hàng trong tam giác Pascal. Xử lý các hàng từ n-1 xuống 0. Ở hàng r, hãy tham lam lấy 

( 0 r ​ ),( 1 r ​ ),( 2 r ​ ),… 

miễn là phần dư hiện tại đủ lớn. Lấy t số hạng đầu tiên hoàn toàn giống với việc đặt a i ​ =t tương ứng, vì điều đó góp phần 

j=0 ∑ t−1 ​ ( j r ​ ). 

Sau khi dừng, số dư nhỏ hơn hệ số nhị thức tiếp theo. Cụ thể, nó nhiều nhất là 2 r −1, chính xác là số lượng lớn nhất các tập con khác rỗng của r phần tử. Đối số tương tự sau đó có thể được áp dụng đệ quy cho các hàng còn lại.

Điều này mang lại một cấu trúc O(n 2 ) trực tiếp. Cấu trúc chính là tam giác Pascal cung cấp đủ lũy thừa chồng chéo của hai để biểu thị mọi mục tiêu có thể có trong khoảng từ 0 đến 2 n −1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n 2 2 n ) trên mỗi biểu đồ, 2 n 2 biểu đồ để tìm kiếm | O(n 2 ) | Quá chậm | 
| Xây dựng kết cấu | O(n 2 ) | O(n 2 ) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số lượng yêu cầu của các tập con không rỗng không quan trọng như 

G=2 n −1−k. 

Chúng tôi làm việc với các tập hợp con không quan trọng vì việc xây dựng tính chúng một cách tự nhiên. Tập hợp con trống bị loại trừ bởi số hạng 2 n −1. 
2. Xây dựng tam giác Pascal đến hàng n−1, lưu trữ 

C[r][j]=( j r ​ ). 

Vì n 20 nên số nguyên Python thông thường là quá đủ và giá trị lớn nhất chỉ là ( 9 19 ​ ). 
3. Bắt đầu với hàng Pascal cuối cùng, r=n−1, và liên kết nó với đỉnh bên trái 1. Quét j=0,1,…,r. Bất cứ khi nào phần dư G hiện tại ít nhất là C[r][j], hãy trừ giá trị này và cộng cạnh từ đỉnh trái 1 vào đỉnh phải j+1. 

Nếu chúng ta chọn j=0,…,t−1 thì vùng lân cận thu được của đỉnh bên trái 1 có đúng t đỉnh. Đóng góp của nó vào số lượng tập hợp con tốt chính xác là tổng tiền tố 

j=0 ∑ t−1 ​ ( j r ​ ). 
4. Ngay khi hệ số nhị thức tiếp theo quá lớn, hãy dừng xử lý hàng này và chuyển sang r−1. Hàng tiếp theo tương ứng với đỉnh 2 bên trái và quy trình tương tự tiếp tục. 

Lý do bước tham lam này an toàn là vì sau khi lấy tiền tố dài nhất có thể, giá trị còn lại sẽ nhỏ hơn hệ số nhị thức chưa sử dụng đầu tiên. Do đó, nó chắc chắn nhỏ hơn 2 r, nên nó có thể được biểu diễn bằng các hàng Pascal nhỏ hơn. 
5. Tiếp tục cho đến hàng 0. Mọi cạnh được chọn đều có dạng (i,j) với 1<i,j<n. Đối với đỉnh trái cố định, điểm cuối bên phải được chọn luôn là 1,2,…,a i ​, do đó không có cạnh trùng lặp. 
6. Xuất ra tất cả các cạnh đã chọn. Đồ thị được xây dựng có chính xác G tập con không rỗng, không tới hạn, do đó phần còn lại 

(2 n −1)−G=k 

tập hợp con là rất quan trọng. 

Tại sao nó hoạt động có thể được tóm tắt bằng một bất biến. Sau khi xử lý các hàng n−1,n−2,…,r+1, các cạnh đã được chọn đóng góp chính xác số lượng bị trừ khỏi G và mục tiêu còn lại có thể biểu thị bằng các hàng r,r−1,…,0. Khi hàng r được xử lý, chúng tôi lấy tiền tố nhị thức dài nhất có thể. Nếu không thể lấy số hạng tiếp theo thì số dư sẽ nhỏ hơn số hạng đó và do đó nhỏ hơn 2 r, là toàn bộ phạm vi giá trị được biểu thị bằng r hàng còn lại. Bất biến tồn tại cho đến khi phần còn lại bằng không. Đối số lân cận sau đó chuyển đổi từng độ dài tiền tố đã chọn thành số lượng chính xác các tập hợp con không quan trọng được yêu cầu. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def solve():    n, k = map(int, input().split())
    # Number of noncritical nonempty subsets we want.    need = (1 << n) - 1 - k
    # Pascal's triangle.    C = [[0] * (n + 1) for _ in range(n + 1)]    C[0][0] = 1    for i in range(1, n):        C[i][0] = C[i][i] = 1        for j in range(1, i):            C[i][j] = C[i - 1][j - 1] + C[i - 1][j]
    edges = []
    # Row r corresponds to left vertex n-r.    for r in range(n - 1, -1, -1):        left = n - r
        for j in range(r + 1):            if need >= C[r][j]:                need -= C[r][j]                edges.append((left, j + 1))            else:                break
    print(len(edges))    for u, v in edges:        print(u, v)
```Biến`need`là số tập hợp con không rỗng không tới hạn vẫn còn được thực hiện. Ban đầu nó là 2 n −1−k, bởi vì mọi tập con khác rỗng đều là quan trọng hoặc không tới hạn. 

Tam giác Pascal chỉ được xây dựng thông qua hàng n−1. Phép truy toán trực tiếp tính toán các hệ số nhị thức cần thiết cho quá trình phân hủy tham lam. Không có vấn đề tràn trong Python và ngay cả trong ngôn ngữ có chiều rộng cố định, các giá trị ở đây đủ nhỏ cho số nguyên 64 bit. 

Các quá trình vòng ngoài`r`từ`n - 1`xuống tới số không. Đỉnh trái tương ứng là`n - r`, do đó hàng n−1 tạo ra vùng lân cận của đỉnh 1, hàng n−2 tạo ra vùng lân cận của đỉnh 2, v.v. 

Bên trong một hàng, các cạnh được thêm vào theo thứ tự tăng dần của điểm cuối bên phải của chúng. Do đó, nếu một hàng có t cạnh thì chúng nằm chính xác ở các đỉnh 1,…,t. Đây là những gì mang lại cấu trúc vùng lân cận tiền tố được yêu cầu bởi đối số đếm. 

các`break`cũng rất đáng kể. Một lần`need < C[r][j]`, mọi số hạng sau đó trong hàng đó không nhất thiết phải lớn hơn đối với các hàng nhị thức tùy ý, do đó, việc triển khai bất cẩn không nên đơn giản cho rằng tất cả các hệ số sau đều không thể sử dụng được. Ở đây, việc xây dựng đặc biệt yêu cầu lấy tiền tố, vì vậy chúng tôi dừng lại ở thuật ngữ không có sẵn đầu tiên. Đối số toán học đảm bảo rằng phần còn lại có thể được xử lý bằng các hàng nhỏ hơn. 

Đầu ra chứa tối đa 1+2+⋯+n=210 cạnh cho n=20, do đó việc xây dựng nằm trong giới hạn một cách thoải mái. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp, đặt n=3 và k=5. Chúng tôi cần 

G=2 3 −1−5=2 

các tập con không rỗng, không tới hạn. 

| Hàng r | Đỉnh trái | j | ( j r ​ ) | Còn lại trước | Hành động | Còn lại sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 0 | 1 | 2 | Cộng (1,1) | 1 | 
| 2 | 1 | 1 | 2 | 1 | Dừng hàng | 1 | 
| 1 | 2 | 0 | 1 | 1 | Cộng (2,1) | 0 | 

Đồ thị thu được có các cạnh (1,1) và (2,1). Các tập con bên trái của nó hoạt động như sau. 

| Tập hợp con | Vùng lân cận | Kích cỡ | Phê bình? | 
| --- | --- | --- | --- | 
| {1} | {1} | 1,1 | Không | 
| {2} | {1} | 1,1 | Không | 
| {3} | ∅ | 1,0 | Có | 
| {1,2} | {1} | 2,1 | Có | 
| {1,3} | {1} | 2,1 | Có | 
| {2,3} | {1} | 2,1 | Có | 
| {1,2,3} | {1} | 3,1 | Có | 

Có chính xác năm tập con quan trọng, phù hợp với mục tiêu. Hai tập con không rỗng không tới hạn chính xác là hai tập được tính bằng phân rã tham lam. 

Đối với ví dụ thứ hai, lấy n=4,k=0. Chúng ta muốn mọi tập con không rỗng là không tới hạn, vì vậy 

G=2 4 −1=15. 

| Hàng r | Đỉnh trái | Hệ số được chọn | Đóng góp hàng | Còn lại | 
| --- | --- | --- | --- | --- | 
| 3 | 1 | 1,3,3,1 | 8 | 7 | 
| 2 | 2 | 1,2,1 | 4 | 3 | 
| 1 | 3 | 1,1 | 2 | 1 | 
| 0 | 4 | 1 | 1 | 0 | 

Mọi đỉnh bên trái đều được nối với cả 4 đỉnh bên phải. Do đó, mọi tập con khác rỗng có chính xác bốn lân cận, hoặc ít hơn chỉ khi đồ thị bị hạn chế, nhưng vì tất cả các đỉnh bên trái đều có lân cận đầy đủ nên một tập con có kích thước nhiều nhất là bốn luôn thỏa mãn ∣N(A)∣=4 ≥∣A∣. Do đó không có tập hợp con quan trọng nào. 

Ví dụ này thực hiện ranh giới ngược lại với mẫu. Quá trình tham lam sử dụng toàn bộ tam giác Pascal và tạo ra biểu đồ hai bên hoàn chỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n 2 ) | Tam giác Pascal lấy O(n 2 ) và phép quét tham lam thăm các hệ số O(n 2 ). | 
| Không gian | O(n 2 ) | Tam giác Pascal được lưu trữ có các mục O(n 2 ) và danh sách cạnh có nhiều nhất là O(n 2 ). | 

Với n 20 thì giá trị này cực kỳ nhỏ. Công trình chỉ thực hiện vài trăm phép tính hệ số và tạo ra tối đa 210 cạnh, do đó dễ dàng nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm 

Vì đầu ra không phải là duy nhất nên một thử nghiệm mạnh mẽ sẽ xác thực biểu đồ được tạo ra thay vì yêu cầu một danh sách cạnh cụ thể. Bộ khai thác bên dưới sử dụng chức năng xây dựng tương tự và sau đó liệt kê độc lập tất cả các tập hợp con còn lại để đếm các tập hợp con quan trọng.```python
Python# helper: run solution on input string, return output stringimport sysimport io

def construct(inp: str) -> str:    old_stdin = sys.stdin    sys.stdin = io.StringIO(inp)
    n, k = map(int, sys.stdin.readline().split())    need = (1 << n) - 1 - k
    C = [[0] * (n + 1) for _ in range(n + 1)]    C[0][0] = 1
    for i in range(1, n):        C[i][0] = C[i][i] = 1        for j in range(1, i):            C[i][j] = C[i - 1][j - 1] + C[i - 1][j]
    edges = []
    for r in range(n - 1, -1, -1):        left = n - r        for j in range(r + 1):            if need >= C[r][j]:                need -= C[r][j]                edges.append((left, j + 1))            else:                break
    sys.stdin = old_stdin
    out = [str(len(edges))]    out.extend(f"{u} {v}" for u, v in edges)    return "\n".join(out) + "\n"

def run(inp: str) -> str:    return construct(inp)

def validate(inp: str, out: str) -> None:    n, k = map(int, inp.split())
    lines = out.strip().splitlines()    m = int(lines[0])    assert len(lines) == m + 1
    adj = [0] * n    seen = set()
    for line in lines[1:]:        u, v = map(int, line.split())        assert 1 <= u <= n        assert 1 <= v <= n        assert (u, v) not in seen        seen.add((u, v))        adj[u - 1] |= 1 << (v - 1)
    critical = 0
    for mask in range(1 << n):        neighborhood = 0        for i in range(n):            if mask >> i & 1:                neighborhood |= adj[i]
        if neighborhood.bit_count() < mask.bit_count():            critical += 1
    assert critical == k

# Provided sample.sample = run("3 5")assert sample == "2\n1 1\n2 1\n", "sample 1"validate("3 5", sample)

# Minimum size, zero critical subsets.case = run("1 0")validate("1 0", case)

# Minimum size, all nonempty subsets critical.case = run("1 1")validate("1 1", case)

# All nonempty subsets must be noncritical.case = run("4 0")validate("4 0", case)

# Maximum n and a large target, exercising all subset sizes.case = run("20 1048575")validate("20 1048575", case)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 5`| Đồ thị mẫu có 2 cạnh | Cung cấp mẫu và thi công thông thường | 
|`1 0`| Một cạnh từ trái 1 sang phải 1 | Đầu vào nhỏ nhất và không có tập hợp con quan trọng | 
|`1 1`| Không có cạnh | Đầu vào nhỏ nhất và số lượng quan trọng tối đa có thể | 
|`4 0`| Hoàn thành K 4,4 ​ | Ranh giới trong đó mọi tập con khác rỗng đều không tới hạn | 
|`20 1048575`| Đồ thị không có cạnh | N tối đa, k=2 20 −1 tối đa và xử lý số nguyên lớn | 

## Vỏ cạnh 

cho`1 0`, số lượng tập hợp con không trống không quan trọng cần thiết là 

2 1 −1−0=1. 

Hệ số Pascal duy nhất là ( 0 0 ​ )=1 nên thuật toán thêm cạnh (1,1). Tập hợp con bên trái có một tập hợp con lân cận và không quan trọng, cho ra chính xác các tập con quan trọng bằng 0. 

Vì`1 1`, số lượng tập hợp con không trống không quan trọng được yêu cầu bằng 0. Vòng lặp tham lam không thể trừ đi bất cứ thứ gì nên nó không tạo ra cạnh nào. Tập hợp con duy nhất không trống có kích thước vùng lân cận bằng 0 và kích thước tập hợp con bằng 1, khiến nó trở nên quan trọng. Câu trả lời chứa chính xác một tập hợp con quan trọng. 

Vì`4 0`, số mục tiêu của các tập hợp con không quan trọng là 15. Thuật toán sử dụng mọi hệ số trong các hàng 3,2,1,0, tạo cho mỗi đỉnh trong số bốn đỉnh bên trái đều có bốn đỉnh bên phải là lân cận. Bất kỳ tập hợp con khác trống nào đều có kích thước vùng lân cận là bốn, trong khi kích thước của nó tối đa là bốn, vì vậy không có tập hợp con nào là quan trọng. 

Đối với `4 15), số mục tiêu của các tập con không quan trọng là bằng 0. Không có cạnh nào được chọn cả. Mọi tập con khác rỗng đều có một lân cận trống, vì vậy mỗi một trong 2 4 −1=15 tập con khác rỗng đều quan trọng. 

Vì`20,2^{20}-1`, thuật toán lại cần các tập con không tới hạn bằng 0 và đưa ra biểu đồ trống. Mọi tập con bên trái khác trống đều có kích thước vùng lân cận bằng 0, vì vậy tất cả 1.048.575 tập con khác trống đều rất quan trọng. Điều này cũng xác nhận rằng cấu trúc xử lý giới hạn trên của k mà không có bất kỳ số học trường hợp đặc biệt nào.
