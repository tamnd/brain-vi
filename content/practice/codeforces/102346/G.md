---
title: "CF 102346G - Lấy Niềm Tin"
description: "Chúng ta có một ma trận (N lần N). Hàng (i) đại diện cho vật trang trí (i) và cột (j) đại diện cho vị trí kệ (j). Giá trị (a{i,j}) đo mức độ tin cậy của Fulano đối với vật trang trí (i) ban đầu chiếm giữ vị trí (j)."
date: "2026-08-14T02:04:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102346
codeforces_index: "G"
codeforces_contest_name: "2019-2020 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102346
solve_time_s: 104
verified: true
draft: false
---

[CF 102346G - Có được sự tự tin](https://codeforces.com/problemset/problem/102346/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một ma trận (N \times N). Hàng (i) đại diện cho vật trang trí (i) và cột (j) đại diện cho vị trí kệ (j). Giá trị (a_{i,j}) đo mức độ tin cậy của Fulano đối với vật trang trí (i) ban đầu chiếm giữ vị trí (j). 

Chúng ta phải đặt mọi vật trang trí vào đúng một vị trí, sao cho mỗi hàng và mỗi cột được sử dụng đúng một lần. Nếu vật trang trí (i) được gán cho vị trí (p_i), điểm của sự sắp xếp hoàn chỉnh là 

[ 
\prod_{i=1}^{N} a_{i,p_i}. 
] 

Nhiệm vụ là xuất ra vật trang trí chiếm từng vị trí, với bất kỳ sự sắp xếp nào đạt được sản phẩm tối đa đều hợp lệ. 

Đây là phiên bản tích cực đại của bài toán gán. Kho lưu trữ vấn đề chính thức cung cấp (N \le 100), với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

Giới hạn (N \le 100) ngay lập tức loại trừ việc liệt kê các hoán vị. Có thể có (N!) sự sắp xếp và thậm chí việc đánh giá một sự sắp xếp có chi phí (O(N)), cho (O(N \cdot N!)) thời gian. Tại (N=100), đây là khoảng (100 \cdot 100! \approx 9,3 \times 10^{159}) phép toán cơ bản. Một thuật toán đa thức là cần thiết. 

Trường hợp cạnh đầu tiên là (N=1). Ví dụ,```
1
100
```chỉ có một câu trả lời khả thi,`1`. Việc triển khai giả định có ít nhất hai hàng hoặc cột có thể không thành công khi xây dựng các mảng khớp với nó. 

Một sai lầm phổ biến khác là cho phép hai đồ trang trí sử dụng cùng một vị trí. Coi như```
2
10 9
10 1
```Sự sắp xếp hợp lệ tốt nhất là`2 1`, cho (9 \cdot 10 = 90). Thủ tục tham lam lấy giá trị lớn nhất trong mỗi hàng một cách độc lập, chọn vị trí 1 cho cả hai đồ trang trí, đây không phải là một hoán vị hợp lệ. Ràng buộc chuyển nhượng phải được xử lý trên toàn cầu. 

Sự ràng buộc cũng xảy ra một cách tự nhiên. Vì```
2
5 5
5 5
```mọi hoán vị đều có tích (25), nên cả hai`1 2`Và`2 1`là đúng. Mã không được cho rằng tối ưu là duy nhất. 

Các giá trị cũng có thể lớn bằng 100. Đối với (N=100), một sản phẩm có thể lớn bằng (100^{100}), vượt xa các loại số nguyên có chiều rộng cố định trong các ngôn ngữ như C++. Chúng tôi không bao giờ tính toán trực tiếp sản phẩm này. Phép biến đổi được giải pháp sử dụng sẽ tránh hoàn toàn tình trạng tràn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi hoán vị của đồ trang trí (N). Đối với mỗi hoán vị, chúng tôi nhân (N) mục nhập ma trận đã chọn và giữ cách sắp xếp tốt nhất. Điều này đúng vì mọi cách sắp xếp hợp lệ có thể có đều được biểu diễn bằng chính xác một hoán vị, do đó cuối cùng sự sắp xếp tối ưu sẽ được kiểm tra. Giá của nó là (O(N \cdot N!)), không thể sử dụng được từ lâu (N=100). 

Cấu trúc của vấn đề cho chúng ta một lộ trình tốt hơn. Mọi sự sắp xếp hợp lệ đều chọn chính xác một ô từ mỗi hàng và mỗi cột, đây chính xác là một kết hợp hoàn hảo trong biểu đồ hai bên hoàn chỉnh. Điều bất thường duy nhất là trọng số phù hợp là tích số chứ không phải tổng. 

Logarit loại bỏ sự khác biệt đó. Vì mọi giá trị ma trận đều dương nên 

\sum_i \log(a_{i,p_i}). 
] 

Logarit tăng nghiêm ngặt nên cách sắp xếp có tích lớn nhất chính là cách sắp xếp có tổng logarit lớn nhất. Chúng ta đã chuyển bài toán này thành bài toán gán trọng số cực đại tiêu chuẩn. 

Chúng ta có thể giảm thiểu chi phí một cách tương đương 

[ 
c_{i,j}=-\log(a_{i,j}). 
] 

Thuật toán Hungary giải quyết bài toán gán này trong thời gian (O(N^3)). Việc triển khai dựa trên tiềm năng của nó duy trì sự khớp một phần và liên tục tìm thấy đường dẫn tăng cường cho hàng tiếp theo. Việc triển khai tiêu chuẩn sử dụng các mảng`u`,`v`,`p`,`way`, Và`minv`để duy trì các điện thế kép và xây dựng lại từng đường dẫn tăng cường. 

Bởi vì tất cả các giá trị đầu vào nằm trong khoảng từ 1 đến 100 nên logarit là những số hữu hạn nhỏ. Việc triển khai chương trình cạnh tranh dự định sử dụng logarit dấu phẩy động, cũng như cách tiếp cận giải pháp đã biết cho vấn đề này. 

Do đó, sự so sánh là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N \cdot N!)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N^3)) | (O(N^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ma trận độ tin cậy và thay thế mọi giá trị (a_{i,j}) bằng giá trị (-\log(a_{i,j})). Chúng tôi sử dụng dấu âm vì cách triển khai Hungary bên dưới được viết để gán chi phí tối thiểu, trong khi chúng tôi cần tối đa hóa điểm logarit. 

Kể từ khi 

\arg\min \sum_i -\log(a_{i,p_i}), 
] 

hoán vị tối ưu không thay đổi. 
2. Duy trì`p[j]`, Ở đâu`p[j]`là hàng hiện được gán cho cột (j). Cột 0 là cột ảo được sử dụng làm điểm bắt đầu của mọi lần tăng thêm. 
3. Xử lý từng hàng một. Khi hàng (i) được giới thiệu, hãy tạm thời gắn nó vào cột ảo và tìm kiếm đường dẫn tăng cường dẫn đến cột thực không được sử dụng. 

Kết quả khớp hiện tại đã tối ưu cho tất cả các hàng được xử lý trước đó. Đường dẫn tăng cường mới là cơ chế cho phép hàng (i) nhập vào phần khớp trong khi có thể di chuyển một số hàng được gán trước đó sang các cột khác nhau. 
4. Trong quá trình tìm kiếm, duy trì`minv[j]`, chi phí giảm nhỏ nhất hiện được biết để đạt được cột (j) và`way[j]`, cột trước đó trên đường dẫn đó. 

Chi phí giảm được là 

[ 
c_{i,j}-u_i-v_j. 
] 

Những tiềm năng`u`Và`v`làm cho chi phí giảm không âm trong giải pháp kép được duy trì, cho phép mở rộng đường tăng cường ngắn nhất một cách hiệu quả. 
5. Chọn cột chưa sử dụng có giá trị nhỏ nhất`minv`giá trị. Hãy để giá trị đó là`delta`. Cập nhật điện thế cho phần hiện có thể tiếp cận của cây xen kẽ và trừ đi`delta`từ phần còn lại`minv`các giá trị. 

Điều này duy trì tính khả thi của điện thế kép trong khi tạo ra ít nhất một cột mới có thể tiếp cận được với chi phí giảm bằng 0. 
6. Tiếp tục cho đến khi đạt đến cột chưa sử dụng. Lúc đó hãy làm theo`way`ngược từ cột đó. Đảo ngược các bài tập dọc theo đường dẫn xen kẽ kết quả. 

Kết quả khớp mới chứa nhiều hàng hơn trước, trong khi mỗi hàng được xử lý vẫn có chính xác một cột và mọi cột đã sử dụng vẫn có đúng một hàng. 
7. Sau khi tất cả (N) hàng đã được xử lý, mỗi cột có đúng một hàng được chỉ định. Mảng`p[1], p[2], ..., p[N]`đã ở định dạng mà đầu ra yêu cầu: đối với mỗi vị trí kệ (j),`p[j]`là vật trang trí được đặt ở đó. 

### Tại sao nó hoạt động 

Phép biến đổi logarit bảo toàn thứ tự của tất cả các tích có thể, do đó việc giải bài toán gán phép cộng đã biến đổi tương đương với việc giải bài toán ban đầu. Thuật toán Hungary duy trì sự so khớp hợp lệ cho các hàng được xử lý cùng với các thế năng kép có chi phí giảm tương thích với sự so khớp đó. Mỗi lần tăng thêm sẽ thêm chính xác một hàng mới mà không phá vỡ các ràng buộc gán và đường tăng thêm ngắn nhất sẽ mang lại chi phí chuyển đổi bổ sung tối thiểu có thể. Bằng quy nạp, sau khi xử lý hàng (i), việc so khớp là tối ưu trong số tất cả các phép gán của hàng (i) đầu tiên cho các cột riêng biệt. Sau phép tăng thứ (N), mọi hàng và cột đều được đưa vào, do đó kết quả khớp hoàn hảo đạt được là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve(data):
    it = iter(data.split())
    n = int(next(it))

    # cost[i][j] = -log(confidence)
    cost = [[0.0] * n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            x = int(next(it))
            cost[i][j] = -math.log(x)

    # Hungarian algorithm for minimum-cost assignment.
    #
    # p[j] = row currently assigned to column j.
    # way[j] = previous column in the augmenting path.
    u = [0.0] * (n + 1)
    v = [0.0] * (n + 1)
    p = [0] * (n + 1)
    way = [0] * (n + 1)

    for i in range(1, n + 1):
        p[0] = i
        j0 = 0

        minv = [float("inf")] * (n + 1)
        used = [False] * (n + 1)

        while True:
            used[j0] = True
            i0 = p[j0]

            delta = float("inf")
            j1 = 0

            for j in range(1, n + 1):
                if used[j]:
                    continue

                cur = cost[i0 - 1][j - 1] - u[i0] - v[j]

                if cur < minv[j]:
                    minv[j] = cur
                    way[j] = j0

                if minv[j] < delta:
                    delta = minv[j]
                    j1 = j

            for j in range(n + 1):
                if used[j]:
                    u[p[j]] += delta
                    v[j] -= delta
                else:
                    minv[j] -= delta

            j0 = j1

            if p[j0] == 0:
                break

        while j0 != 0:
            j1 = way[j0]
            p[j0] = p[j1]
            j0 = j1

    return " ".join(map(str, p[1:]))

def main():
    data = sys.stdin.buffer.read()
    if data:
        print(solve(data))

if __name__ == "__main__":
    main()
```Đầu vào được đọc dưới dạng chuỗi một byte và được chia một lần. Điều này giúp chi phí phân tích cú pháp ở mức nhỏ, điều này quan trọng vì ma trận chứa tới 10.000 giá trị. 

Ma trận được chuyển đổi ngay lập tức thành chi phí logarit âm. các`-1`điều chỉnh chỉ số trong`cost[i0 - 1][j - 1]`là cần thiết vì các mảng Hungary sử dụng tính năng lập chỉ mục dựa trên một, trong khi danh sách Python sử dụng tính năng lập chỉ mục dựa trên số 0. 

Các mảng`u`Và`v`là điện thế hàng và cột.`p`lưu trữ kết quả phù hợp hiện tại, trong khi`way`lưu trữ đủ thông tin trước đó để xây dựng lại đường dẫn tăng cường mà không lưu trữ toàn bộ đường dẫn một cách rõ ràng. Đây là công thức thế năng tiêu chuẩn (O(N^3)) của tiếng Hungary. 

cột ảo`0`không phải là một vị trí kệ thực sự. Cài đặt`p[0] = i`bắt đầu tăng cường từ vật trang trí hiện đang được xử lý. Nó không bao giờ được xuất hiện trong câu trả lời cuối cùng, đó là lý do tại sao kết quả sử dụng`p[1:]`. 

bản cập nhật```
u[p[j]] += delta
v[j] -= delta
```được ghép nối với```
minv[j] -= delta
```cho các cột không sử dụng. Việc bỏ qua một trong hai phần sẽ phá vỡ tính bất biến về chi phí giảm và có thể tạo ra kết quả khớp không hợp lệ hoặc không tối ưu. 

Không có tràn số nguyên vì mã không bao giờ tạo thành sản phẩm gốc. Giá trị lớn nhất được truyền cho`math.log`chỉ bằng 100, và thế Hungary biểu diễn trên tổng logarit có độ lớn nhỏ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Ma trận là```
1 15 37
42 8 25
77 2 1
```Thuật toán Hungary xử lý các đồ trang trí dưới dạng hàng. Sự phù hợp`p`được lập chỉ mục theo vị trí kệ, vì vậy sau mỗi lần tăng, nó sẽ trực tiếp cho chúng ta biết vật trang trí nào hiện đang chiếm giữ từng vị trí. 

| Bước | Đã thêm trang trí | Phân công hiện tại theo vị trí | Giải thích | 
| --- | --- | --- | --- | 
| 1 | 1 |`[1, 0, 0]`| Vật trang trí 1 chiếm vị trí 1 trong khớp từng phần hiện tại | 
| 2 | 2 |`[2, 0, 1]`| Vật trang trí 2 chiếm vị trí 1 và vật trang trí 1 chuyển sang vị trí 3 | 
| 3 | 3 |`[3, 1, 2]`| Đường dẫn tăng cường cuối cùng mang lại vị trí 1, 2, 3 cho đồ trang trí 3, 1, 2 | 

Sự sắp xếp cuối cùng là`3 1 2`. Sản phẩm của nó là 

[ 
77 \cdot 15 \cdot 25 = 28875. 
] 

Phần tăng cường cuối cùng là phần thú vị. Vật trang trí 3 thực sự thích vị trí 1 hơn, nhưng vật trang trí 2 đã sử dụng vị trí đó rồi. Thay vì loại bỏ xung đột một cách tham lam, đường tăng cường sẽ di chuyển vật trang trí 2 đến vị trí 3, từ đó di chuyển vật trang trí 1 đến vị trí 2. Đây chính xác là sự gán lại toàn cục mà thuật toán tham lam không thể thực hiện. 

### Mẫu 2 

Ma trận là```
15 1
33 42
```Thuật toán tiến hành như sau. 

| Bước | Đã thêm trang trí | Phân công hiện tại theo vị trí | Giải thích | 
| --- | --- | --- | --- | 
| 1 | 1 |`[1, 0]`| Vật trang trí 1 chiếm vị trí 1 | 
| 2 | 2 |`[1, 2]`| Vật trang trí 2 chiếm vị trí còn lại 2 | 

Kết quả đầu ra là`1 2`, với sản phẩm 

[ 
15 \cdot 42 = 630. 
] 

Không có xung đột nào yêu cầu đường tăng cường dài hơn một cạnh trong ví dụ này. Nó thể hiện trường hợp cơ bản của quá trình so khớp và xác nhận rằng đầu ra được lập chỉ mục theo vị trí chứ không phải theo đồ trang trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^3)) | Có (N) giai đoạn tăng cường và mỗi giai đoạn thực hiện công việc (O(N^2)) trong trường hợp xấu nhất | 
| Không gian | (O(N^2)) | Ma trận độ tin cậy được chuyển đổi sử dụng bộ nhớ (O(N^2)), trong khi mảng Hungary sử dụng (O(N)) | 

Đối với (N=100), (O(N^3)) có nghĩa là khoảng một triệu lần lặp vòng lặp bên trong, so với số lần sắp xếp giai thừa trong lực lượng vũ phu. Bản thân ma trận chỉ chứa 10.000 mục nhập, do đó yêu cầu bộ nhớ nằm trong giới hạn 256 MB. Tuyên bố cuộc thi được lưu trữ chỉ định giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm 

Dây nịt sau đây nhúng giống nhau`solve`triển khai để các thử nghiệm có thể được thực hiện trực tiếp. Vì có thể có nhiều hoán vị tối ưu nên các trường hợp tùy chỉnh được chọn sao cho cách sắp xếp dự kiến ​​được xác định duy nhất bất cứ khi nào sử dụng xác nhận đầu ra chính xác.```python
import sys
import io
import math

def solve(data):
    it = iter(data.split())
    n = int(next(it))

    cost = [[0.0] * n for _ in range(n)]

    for i in range(n):
        for j in range(n):
            x = int(next(it))
            cost[i][j] = -math.log(x)

    u = [0.0] * (n + 1)
    v = [0.0] * (n + 1)
    p = [0] * (n + 1)
    way = [0] * (n + 1)

    for i in range(1, n + 1):
        p[0] = i
        j0 = 0

        minv = [float("inf")] * (n + 1)
        used = [False] * (n + 1)

        while True:
            used[j0] = True
            i0 = p[j0]

            delta = float("inf")
            j1 = 0

            for j in range(1, n + 1):
                if used[j]:
                    continue

                cur = cost[i0 - 1][j - 1] - u[i0] - v[j]

                if cur < minv[j]:
                    minv[j] = cur
                    way[j] = j0

                if minv[j] < delta:
                    delta = minv[j]
                    j1 = j

            for j in range(n + 1):
                if used[j]:
                    u[p[j]] += delta
                    v[j] -= delta
                else:
                    minv[j] -= delta

            j0 = j1

            if p[j0] == 0:
                break

        while j0 != 0:
            j1 = way[j0]
            p[j0] = p[j1]
            j0 = j1

    return " ".join(map(str, p[1:]))

def run(inp: str) -> str:
    return solve(inp.encode()).strip()

# Provided sample 1
assert run(
    """3
1 15 37
42 8 25
77 2 1
"""
) == "3 1 2", "sample 1"

# Provided sample 2
assert run(
    """2
15 1
33 42
"""
) == "1 2", "sample 2"

# Minimum size
assert run(
    """1
100
"""
) == "1", "minimum-size case"

# All values equal, the deterministic Hungarian implementation returns identity
assert run(
    """3
7 7 7
7 7 7
7 7 7
"""
) == "1 2 3", "all-equal values"

# Greedy trap: row 1 and row 2 both prefer position 1,
# but the optimal valid permutation is 2 1.
assert run(
    """2
10 9
10 1
"""
) == "2 1", "greedy trap"

# Boundary values and a unique optimum
assert run(
    """2
100 1
1 100
"""
) == "1 2", "boundary values"

# Maximum-size case with a unique optimum.
# Diagonal entries are 100, all other entries are 1.
n = 100
rows = []
for i in range(n):
    row = ["1"] * n
    row[i] = "100"
    rows.append(" ".join(row))

max_case = str(n) + "\n" + "\n".join(rows) + "\n"
expected = " ".join(str(i) for i in range(1, n + 1))

assert run(max_case) == expected, "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 100`|`1`| Kích thước tối thiểu và lập chỉ mục dựa trên một | 
| Một ma trận (3 \times 3) chứa đầy`7`|`1 2 3`| Mối quan hệ và kết hợp xác định | 
|`2 / 10 9 / 10 1`|`2 1`| Phân công toàn cầu và sự thất bại của sự lựa chọn tham lam | 
|`2 / 100 1 / 1 100`|`1 2`| Giá trị đầu vào tối đa và tối ưu duy nhất | 
| (100 \time 100), đường chéo`100`, nơi khác`1`|`1 2 ... 100`| Tối đa (N), hiệu suất và lập chỉ mục ranh giới | 

## Vỏ cạnh 

Với (N=1), đầu vào```
1
100
```bắt đầu phần tăng thêm duy nhất với hàng 1. Cột 1 ngay lập tức trống, do đó đường dẫn chỉ chứa cột đó và`p[1]`trở thành 1. Kết quả trả về là`1`. Không có nhánh trường hợp đặc biệt trong thuật toán, điều này rất hữu ích vì bất biến khớp chung đã xử lý trường hợp nhỏ nhất. 

Đối với trường hợp xung đột tham lam```
2
10 9
10 1
```hàng đầu tiên ban đầu chiếm vị trí 1 vì (10 > 9). Khi hàng 2 được xử lý, vị trí tốt nhất của nó cũng là vị trí 1. Tìm kiếm đường dẫn tăng cường không chỉ đơn giản từ chối lựa chọn đó. Nó xem xét việc di chuyển hàng 1 đến vị trí 2, tạo ra hàng 1 được gán cho vị trí 2 và hàng 2 đến vị trí 1. Đầu ra là`2 1`, có tích là (9 \cdot 10 = 90), so với (10 \cdot 1 = 10) đối với phép gán hợp lệ khác. 

Đối với các giá trị bằng nhau,```
3
7 7 7
7 7 7
7 7 7
```mọi hoán vị hợp lệ đều có cùng một tích (7^3). Trong mỗi lần tăng cường, cột có sẵn đầu tiên được chọn một cách nhất quán, đưa ra`1 2 3`. Bài toán cho phép bất kỳ hoán vị tối ưu nào, do đó hành vi ràng buộc xác định này là hợp lệ. 

Đối với ranh giới giá trị tối đa,```
2
100 1
1 100
```chi phí logarit là hữu hạn vì mọi giá trị đều dương. Phép gán đường chéo có tích (10000), trong khi phép gán khác có tích (1). Chi phí chuyển đổi bảo toàn thứ tự này, do đó thuật toán trả về`1 2`. 

Đối với (N=100), tích của các giá trị đã chọn có thể là (100^{100}), nhưng quá trình triển khai không bao giờ lưu trữ số đó. Mỗi mục nhập ma trận trở thành`-math.log(100)`hoặc một giá trị dấu phẩy động nhỏ khác và thuật toán Hungary chỉ cộng và trừ các chi phí logarit này. Các mảng gán vẫn tuyến tính trong (N), trong khi ma trận chiếm bộ nhớ (O(N^2)). Đây chính xác là thang đo mà thuật toán gán (O(N^3)) phù hợp.
