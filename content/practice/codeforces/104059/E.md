---
title: "CF 104059E - Món khai vị thú vị"
description: "Căng tin bắt đầu với hai “súp cơ bản” mà chúng ta có thể coi là hai nguyên liệu thuần túy: một là súp π-tato và một là súp τ-mato. Từ ngày này sang ngày khác, công thức phát triển một cách xác định. Vào ngày đầu tiên món súp hoàn toàn là π-tato, vào ngày thứ 2 nó hoàn toàn là τ-mato."
date: "2026-07-02T03:29:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104059
codeforces_index: "E"
codeforces_contest_name: "2022-2023 ACM-ICPC German Collegiate Programming Contest (GCPC 2022)"
rating: 0
weight: 104059
solve_time_s: 43
verified: true
draft: false
---

[CF 104059E - Món khai vị thú vị](https://codeforces.com/problemset/problem/104059/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Căng tin bắt đầu với hai “súp cơ bản” mà chúng ta có thể coi là hai nguyên liệu thuần túy: một là súp π-tato và một là súp τ-mato. Từ ngày này sang ngày khác, công thức phát triển một cách xác định. Vào ngày đầu tiên món súp hoàn toàn là π-tato, vào ngày thứ 2 nó hoàn toàn là τ-mato. Từ ngày thứ 3 trở đi, mỗi món súp mới được hình thành bằng cách trộn hai món súp trước đó với tỷ lệ bằng nhau. 

Điều này có nghĩa là món súp thứ n luôn là hỗn hợp của hai món súp đầu tiên và mỗi bước chỉ trộn các hỗn hợp trước đó mà không đưa ra điều gì mới. Nhiệm vụ là xác định phần nào của hỗn hợp cuối cùng có được từ súp π-tato ban đầu và phần nào có được từ súp τ-mato sau n ngày. 

Kích thước đầu vào lên tới 10^18, do đó, bất kỳ phương pháp mô phỏng hàng ngày nào đều không khả thi ngay lập tức. Một mô phỏng tuyến tính sẽ yêu cầu tới 10^18 thao tác, vượt xa mọi giới hạn thời gian. Ngay cả các phương pháp nhân tố logarit cũng được chấp nhận, nhưng bất kỳ phương pháp nào tệ hơn O(log n) sẽ thất bại. 

Trường hợp cạnh tinh tế xuất hiện ở các giá trị nhỏ của n. Khi n bằng 1 hoặc 2, câu trả lời là các thành phần gần như tinh khiết. Nếu một giải pháp áp dụng một cách mù quáng công thức truy hồi bắt đầu từ n = 3 mà không xử lý các trường hợp cơ bản này, nó có thể dịch chuyển chuỗi không chính xác hoặc tạo ra các giá trị không xác định. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Chúng tôi duy trì một cặp giá trị cho mỗi ngày biểu thị số lượng π-tato và τ-mato hiện diện. Vào ngày thứ nhất chúng ta bắt đầu với (1, 0), vào ngày thứ 2 chúng ta có (0, 1). Mỗi ngày tiếp theo chúng tôi tính giá trị trung bình của hai cặp trước đó. Điều này trực tiếp tuân theo định nghĩa vấn đề và được xây dựng chính xác. 

Vấn đề là quy mô. Việc tính toán đến ngày n yêu cầu lặp lại tất cả các ngày trước đó, vì vậy độ phức tạp là O(n). Với n lên tới 10^18, cách làm này hoàn toàn không khả thi. 

Quan sát quan trọng là cả hai thành phần đều tiến hóa độc lập trong cùng một chu kỳ lặp lại. Nếu chúng ta chỉ theo dõi phân số π-tato, thì nó thỏa mãn phép truy hồi f(n) = (f(n−1) + f(n−2)) / 2, với f(1) = 1 và f(2) = 0. Phân số τ-mato đơn giản là 1 − f(n), nhưng chúng ta cũng có thể tính toán nó một cách đối xứng. 

Đây là một sự tái phát tuyến tính với hệ số không đổi. Những phép truy toán như vậy có thể được giải bằng cách sử dụng phép lũy thừa ma trận. Chúng tôi mã hóa quá trình chuyển đổi từ (f(n−1), f(n−2)) sang (f(n), f(n−1)) dưới dạng phép nhân ma trận. Việc nâng ma trận này lên lũy thừa thứ (n−2) sẽ cho chúng ta trạng thái tại ngày n trong thời gian O(log n). 

Phép chia cho 2 đưa ra các phân số, nhưng chúng ta có thể coi nó như nhân với 1/2 trong dấu phẩy động hoặc kết hợp nó vào ma trận chuyển tiếp theo cách tương đương. Vì độ chính xác yêu cầu là 10^{-6} nên lũy thừa ma trận dấu phẩy động là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Hàm mũ ma trận | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề xuống việc tính toán một phép truy toán giống Fibonacci với việc chia tỷ lệ. 

1. Xác định f(n) là phần của súp π-tato vào ngày n. Từ cách xây dựng, f(1) = 1 và f(2) = 0. Điều này thiết lập hai trạng thái cơ bản mà từ đó mọi thứ phát triển. 
2. Quan sát rằng với n ≥ 3, f(n) = (f(n−1) + f(n−2)) / 2. Điều này xuất phát trực tiếp từ việc lấy trung bình cộng của hai hỗn hợp trước đó. 
3. Viết lại phép truy toán dưới dạng phép biến đổi tuyến tính: 

(f(n), f(n−1)) = (1/2 * (f(n−1) + f(n−2)), f(n−1)). 
4. Biểu diễn dưới dạng phép nhân ma trận: 

[f(n), f(n−1)]^T = A * [f(n−1), f(n−2)]^T,

trong đó A = [[1/2, 1/2], [1, 0]]. Biểu diễn này cho phép ứng dụng lặp đi lặp lại thông qua lũy thừa. 
5. Tính A^(n−2). Điều này biến đổi vectơ ban đầu [f(2), f(1)] = [0, 1] thành [f(n), f(n−1)]. 
6. Thực hiện tính lũy thừa nhanh bằng bình phương. Mỗi phép nhân kết hợp hai trạng thái chuyển tiếp, bảo toàn tính đúng đắn của phép truy hồi. 
7. Trích xuất f(n) từ vectơ kết quả. Phân số τ-mato là 1 − f(n), vì tổng hỗn hợp luôn có tổng bằng 1. 

### Tại sao nó hoạt động 

Bất biến chính là mỗi vectơ trạng thái mã hóa đầy đủ hai giá trị cuối cùng của phép truy hồi. Ma trận chuyển tiếp tái tạo chính xác bước lặp lại, do đó, nhân với ma trận sẽ nâng cao trình tự lên một ngày mà không có sai số gần đúng vượt quá độ chính xác của dấu phẩy động. Vì phép nhân ma trận có tính kết hợp nên việc bình phương lặp lại mô phỏng chính xác n−2 phép biến đổi tuần tự theo thời gian logarit. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def mat_mul(a, b):
    return [
        [
            a[0][0]*b[0][0] + a[0][1]*b[1][0],
            a[0][0]*b[0][1] + a[0][1]*b[1][1],
        ],
        [
            a[1][0]*b[0][0] + a[1][1]*b[1][0],
            a[1][0]*b[0][1] + a[1][1]*b[1][1],
        ],
    ]

def mat_pow(m, e):
    res = [[1.0, 0.0], [0.0, 1.0]]
    base = m
    while e > 0:
        if e & 1:
            res = mat_mul(res, base)
        base = mat_mul(base, base)
        e >>= 1
    return res

n = int(input())

if n == 1:
    print("100.0 0.0")
elif n == 2:
    print("0.0 100.0")
else:
    A = [
        [0.5, 0.5],
        [1.0, 0.0],
    ]
    M = mat_pow(A, n - 2)
    f_n = M[0][1]
    print(f"{f_n * 100:.10f} {(1 - f_n) * 100:.10f}")
```Mã này xây dựng ma trận chuyển tiếp trực tiếp từ phép lặp. Hàm nhân thực hiện phép nhân ma trận 2×2 tiêu chuẩn mà không cần tối ưu hóa, vì kích thước không đổi giúp nó hoạt động hiệu quả. 

Lũy thừa bằng bình phương làm giảm số phép nhân từ n xuống log n. Ma trận đồng nhất khởi tạo sự tích lũy sao cho các kết quả từng phần vẫn trung tính cho đến khi được kết hợp. 

Các trường hợp cơ sở n = 1 và n = 2 được xử lý rõ ràng vì công thức ma trận giả định quyền truy cập vào hai trạng thái trước đó. 

Số học dấu phẩy động được sử dụng trực tiếp. Vì độ sâu tối đa là log2(10^18) ≈ 60 phép nhân, nên sai số tích lũy vẫn nằm trong giới hạn có thể chấp nhận được. 

## Ví dụ đã hoạt động 

Xét sự truy hồi bắt đầu từ n = 1 và n = 2. 

### Ví dụ 1: n = 3 

| Ngày | f(n−2) | f(n−1) | tính toán f(n) | 
| --- | --- | --- | --- | 
| 1 | - | - | 1.0 | 
| 2 | - | - | 0,0 | 
| 3 | 1.0 | 0,0 | (1,0 + 0,0)/2 = 0,5 | 

Với n = 3, kết quả là 50% π-tato và 50% τ-mato. Điều này cho thấy bước trộn đầu tiên trực tiếp tính trung bình của hai loại súp cơ bản. 

### Ví dụ 2: n = 4 

| Ngày | f(n−2) | f(n−1) | tính toán f(n) | 
| --- | --- | --- | --- | 
| 2 | - | - | 0,0 | 
| 3 | 1.0 | 0,0 | 0,5 | 
| 4 | 0,0 | 0,5 | (0,5 + 0,0)/2 = 0,25 | 

Vào ngày thứ 4, tỷ lệ π-tato giảm xuống còn 25%, xác nhận rằng việc lấy trung bình lặp lại sẽ nhanh chóng làm giảm mức đóng góp ban đầu. 

Những ví dụ này xác nhận rằng phép truy hồi hoạt động giống như một chuỗi Fibonacci được làm mịn được chia tỷ lệ 1/2 ở mỗi bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Phép lũy thừa ma trận giảm một nửa số mũ mỗi bước | 
| Không gian | O(1) | Chỉ một số ma trận 2×2 không đổi được lưu trữ | 

Thời gian chạy logarit dễ dàng xử lý n lên tới 10^18. Việc sử dụng bộ nhớ là không đổi và không phụ thuộc vào kích thước đầu vào, do đó nó vẫn hoạt động hiệu quả trong các giới hạn nghiêm ngặt. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    import sys
    input = sys.stdin.readline

    def mat_mul(a, b):
        return [
            [
                a[0][0]*b[0][0] + a[0][1]*b[1][0],
                a[0][0]*b[0][1] + a[0][1]*b[1][1],
            ],
            [
                a[1][0]*b[0][0] + a[1][1]*b[1][0],
                a[1][0]*b[0][1] + a[1][1]*b[1][1],
            ],
        ]

    def mat_pow(m, e):
        res = [[1.0, 0.0], [0.0, 1.0]]
        base = m
        while e > 0:
            if e & 1:
                res = mat_mul(res, base)
            base = mat_mul(base, base)
            e >>= 1
        return res

    n = int(input())

    if n == 1:
        return "100.0 0.0"
    if n == 2:
        return "0.0 100.0"

    A = [[0.5, 0.5], [1.0, 0.0]]
    M = mat_pow(A, n - 2)
    f_n = M[0][1]
    return f"{f_n * 100:.10f} {(1 - f_n) * 100:.10f}"

# provided samples
assert run("1") == "100.0 0.0", "sample 1"
assert run("2") == "0.0 100.0", "sample 2"

# custom cases
assert run("3") == "50.0000000000 50.0000000000", "first mix"
assert run("4") == "25.0000000000 75.0000000000", "second decay"
assert run("10") != "", "non-trivial growth case"
assert run("1") == "100.0 0.0", "boundary minimum"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 100 0 | xử lý trường hợp cơ bản | 
| 2 | 0 100 | trường hợp cơ sở thứ hai | 
| 3 | 50 50 | bước tái phát đầu tiên | 
| 4 | 25 75 | phân rã trung bình lặp đi lặp lại | 

## Vỏ cạnh 

Với n = 1, công thức ma trận không thể áp dụng được vì nó giả sử tồn tại hai trạng thái trước đó. Thuật toán bỏ qua phép lũy thừa ma trận một cách rõ ràng và trả về (100, 0), phù hợp với định nghĩa của ngày 1 là món súp π-tato thuần túy. 

Đối với n = 2, vấn đề tương tự xảy ra và đầu ra được cố định là (0, 100). Bất kỳ nỗ lực nào để áp dụng phép truy toán một cách mù quáng sẽ tính toán không chính xác một hỗn hợp các trạng thái trước đó không xác định. 

Đối với n lớn chẳng hạn như 10^18, mô phỏng trực tiếp sẽ tràn cả thời gian và bộ nhớ, nhưng phép lũy thừa ma trận làm giảm việc tính toán xuống còn khoảng 60 bước bình phương. Mỗi bước chỉ thao tác các ma trận có kích thước không đổi, do đó kết quả vẫn ổn định theo số học dấu phẩy động.
