---
title: "CF 102168A - \u0421\u0440\u0435\u0434\u043d\u0435\u0435 \u0430\u0440\u0438\u0444\u043c\u0435\u0442\u0438\u0447\u0435\u0441\u043a\u043e\u0435"
description: "Chúng tôi duy trì nhiều tập hợp các số nguyên không âm trong khi xử lý các phép toán theo một thứ tự cố định. Thao tác bổ sung sẽ chèn một lần xuất hiện của một giá trị, trong khi thao tác xóa sẽ loại bỏ chính xác một lần xuất hiện của giá trị hiện có."
date: "2026-08-19T07:18:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102168
codeforces_index: "A"
codeforces_contest_name: "\u041b\u0438\u0447\u043d\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u043e\u0433\u043e \u0443\u043d\u0438\u0432\u0435\u0440\u0441\u0438\u0442\u0435\u0442\u0430 \u0441\u0440\u0435\u0434\u0438 \u043d\u043e\u0432\u0438\u0447\u043a\u043e\u0432 2018-2019"
rating: 0
weight: 102168
solve_time_s: 90
verified: true
draft: false
---

[CF 102168A - \u0421\u0440\u0435\u0434\u043d\u0435\u0435 \u0430\u0440\u0438\u0444\u043c\u0435\u0442\u0438\u0447\u0435\u0441\u043a\u043e\u0435](https://codeforces.com/problemset/problem/102168/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì nhiều tập hợp các số nguyên không âm trong khi xử lý các phép toán theo một thứ tự cố định. Thao tác bổ sung sẽ chèn một lần xuất hiện của một giá trị, trong khi thao tác xóa sẽ loại bỏ chính xác một lần xuất hiện của giá trị hiện có. Cho phép các giá trị trùng lặp, do đó, việc xóa một bản sao không được xóa tất cả các bản sao trừ khi chỉ có một bản sao. 

Sau mỗi thao tác, chúng ta cần giá trị trung bình số học của tất cả các giá trị hiện được lưu trữ. Nếu nhiều tập hợp trống, câu trả lời bắt buộc là 0. 

Định nghĩa toán học trực tiếp là 

[ 
\text{average}=\frac{\text{tổng của tất cả các phần tử}}{\text{số phần tử}}. 
] 

Khó khăn chính là có thể có tới 200.000 thao tác. Với giới hạn hai giây, một thuật toán quét toàn bộ nhiều tập hợp sau mỗi thao tác sẽ thực hiện theo thứ tự công việc (n^2). Với (n=200000), con số đó có thể đạt khoảng 40 tỷ lượt truy cập phần tử, vượt xa mức thực tế. Chúng ta cần mỗi phép toán chỉ yêu cầu công hằng số hoặc công logarit. 

Giá trị (x) có thể lớn bằng (10^9) và có thể có 200.000 phần tử cùng một lúc. Do đó, tổng số tiền của chúng có thể đạt tới (2\cdot10^{14}). Các số nguyên Python xử lý phạm vi này một cách chính xác và thậm chí một số dấu phẩy động có độ chính xác kép cũng có thể biểu thị mọi số nguyên lên tới (2^{53}), lớn hơn (2\cdot10^{14}). Phép chia cuối cùng cũng an toàn đối với sai số tương đối hoặc tuyệt đối bắt buộc của (10^{-9}). 

Trường hợp cạnh đầu tiên là một multiset trống. Ví dụ,```
1
+ 0
```sản xuất```
0.0
```bởi vì phần tử duy nhất bằng 0. Tổng quát hơn, sau khi thêm và sau đó loại bỏ phần tử duy nhất,```
2
+ 5
- 5
```câu trả lời là```
5.0
0.0
```Việc triển khai bất cẩn luôn chia cho kích thước hiện tại mà không kiểm tra số 0 sẽ cố gắng chia cho 0 sau thao tác thứ hai. 

Trường hợp cạnh thứ hai là một tập hợp nhiều giá trị trùng lặp. Coi như```
3
+ 2
+ 2
- 2
```Đầu ra đúng là```
2.0
2.0
2.0
```Việc xóa chỉ xóa một lần xuất hiện, do đó một lần`2`vẫn còn. Việc triển khai xử lý cấu trúc như một tập hợp thông thường có thể loại bỏ hoàn toàn giá trị và báo cáo số 0 không chính xác. 

Trường hợp cạnh thứ ba là một giá trị lớn. Ví dụ,```
2
+ 1000000000
+ 1000000000
```có câu trả lời```
1000000000.0
1000000000.0
```Giải pháp sử dụng loại số nguyên hẹp không cần thiết có thể bị tràn khi tính tổng. Số nguyên có độ chính xác tùy ý của Python tránh được vấn đề đó. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu tuân theo định nghĩa về ý nghĩa số học theo nghĩa đen. Giữ lại các phần tử hiện tại, thực hiện thao tác chèn hoặc xóa được yêu cầu, sau đó lặp lại mọi phần tử còn lại để tính tổng của nó và chia cho số phần tử. Điều này đúng vì nó xây dựng lại chính xác số lượng được yêu cầu sau mỗi thao tác. 

Vấn đề là việc quét lặp đi lặp lại. Nếu tập hợp chứa khoảng (n/2) phần tử cho hầu hết (n) phép toán thì tổng khối lượng công việc là bậc hai. Trong trường hợp xấu nhất, số phần tử được truy cập theo thứ tự 

[ 
1+2+\dots+(n-1)=\frac{n(n-1)}2. 
] 

Đối với (n=200000), đây là khoảng 20 tỷ lượt truy cập ngay cả trong kịch bản quy mô tăng dần đơn giản này. Một cấu trúc giữ cho nhiều tập lớn xuyên suốt vẫn có thể tạo ra thứ tự bậc hai tương tự. Giải pháp như vậy không thể phù hợp với thời hạn. 

Quan sát mở ra giải pháp nhanh hơn là việc chèn hoặc xóa sẽ thay đổi tổng và tổng số phần tử chỉ ở một vị trí. Chúng ta không cần kiểm tra các nguyên tố khác vì sự đóng góp của chúng đối với cả hai đại lượng không thay đổi. 

Giả sử tổng hiện tại là (S) và kích thước hiện tại là (C). Việc thêm (x) sẽ thay đổi chúng thành (S+x) và (C+1). Loại bỏ một lần xuất hiện của (x) sẽ thay đổi chúng thành (S-x) và (C-1). Câu trả lời sau đó có thể được tính ngay lập tức dưới dạng (S/C). 

Thực sự không cần phải lưu trữ toàn bộ multiset cho vấn đề này. Câu lệnh đảm bảo rằng mọi thao tác xóa đều hợp lệ, vì vậy chúng ta không cần cấu trúc dữ liệu để xác minh xem (x) có tồn tại hay không. Chúng ta chỉ cần tổng hợp và số phần tử. Các giá trị trùng lặp cũng không yêu cầu xử lý đặc biệt vì mỗi lần xuất hiện đều góp phần độc lập vào tổng và số đếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(1)) không gian phụ trợ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`total = 0`Và`count = 0`. Các biến này biểu thị tổng của tất cả các lần xuất hiện hiện tại và tổng số của chúng. 
2. Đọc thao tác tiếp theo và giá trị của nó`x`. Nếu hoạt động là`+`, thêm vào`x`ĐẾN`total`và tăng`count`bởi một. Điều này phản ánh chính xác việc chèn một lần xuất hiện mới. 
3. Nếu hoạt động`-`, trừ`x`từ`total`và giảm`count`bởi một. Đầu vào đảm bảo rằng sự cố này có thể được loại bỏ, do đó không cần kiểm tra tư cách thành viên hoặc bản đồ tần số. 
4. Sau khi áp dụng thao tác, hãy kiểm tra`count`. Nếu nó bằng 0, xuất ra`0.0`, bởi vì giá trị trung bình số học của tập hợp trống được xác định bằng 0. Ngược lại, xuất ra`total / count`. 
5. Lặp lại quy trình này cho tất cả (n) thao tác. Mỗi phép toán chỉ sử dụng một số lượng phép toán số học không đổi, do đó quá trình xử lý hoàn chỉnh là tuyến tính. 

### Tại sao nó hoạt động 

Điều bất biến là sau mỗi thao tác được xử lý,`total`bằng tổng của mọi lần xuất hiện hiện tại trong nhiều tập hợp, trong khi`count`bằng số lần xuất hiện đó. Ban đầu cả hai giá trị đều đúng cho multiset trống. Việc chèn thêm chính xác (x) vào tổng và chính xác một lần xuất hiện vào số đếm. Việc xóa sẽ loại bỏ chính xác (x) khỏi tổng và chính xác một lần xuất hiện khỏi số đếm. Như vậy bất biến vẫn đúng sau mỗi phép toán. Khi nhiều tập hợp không trống,`total / count`chính xác là trung bình số học của nó, và khi`count`bằng 0, thuật toán đưa ra giá trị 0 được xác định riêng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    total = 0
    count = 0
    output = []

    for _ in range(n):
        op, x = input().split()
        x = int(x)

        if op == '+':
            total += x
            count += 1
        else:
            total -= x
            count -= 1

        if count == 0:
            output.append("0.0")
        else:
            output.append(str(total / count))

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```Hai biến trạng thái trực tiếp thực hiện bất biến từ hướng dẫn.`total`được cập nhật trước khi tính toán câu trả lời, vì giá trị trung bình được yêu cầu là dành cho nhiều tập hợp sau thao tác hiện tại chứ không phải trước thao tác đó. 

Nhánh xóa sử dụng`total -= x`Và`count -= 1`. Không có từ điển tần số vì đầu vào đảm bảo rõ ràng rằng việc xóa luôn đề cập đến một lần xuất hiện hiện có. Việc duy trì các tần số không cần thiết sẽ làm tăng mức sử dụng bộ nhớ mà không góp phần đưa ra câu trả lời. 

các`count == 0`việc kiểm tra phải xảy ra trước khi chia. Bài toán xác định rõ ràng giá trị trung bình của một tập hợp trống bằng 0, do đó trường hợp này không thể xử lý bằng phép chia thông thường. 

Số học số nguyên của Python giữ nguyên`total`chính xác ngay cả khi nó đạt đến (2\cdot10^{14}). Phép chia tạo ra một giá trị dấu phẩy động. Độ chính xác tương đối của nó nằm trong phạm vi dung sai (10^{-9}) cần thiết cho phạm vi câu trả lời có thể có. 

Thu thập các câu trả lời trong`output`và viết chúng một lần sẽ tránh thực hiện thao tác đầu ra cấp hệ thống riêng biệt cho mỗi dòng. Điều này rất hữu ích khi có 200.000 câu trả lời. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, trạng thái quan trọng là tổng hiện tại và số lần xuất hiện. Các phần tử riêng lẻ không bao giờ cần phải được lưu trữ. 

| Hoạt động |`total`|`count`| Trung bình | 
| --- | --- | --- | --- | 
|`+ 1`| 1 | 1 | 1.0 | 
|`+ 2`| 3 | 2 | 1,5 | 
|`+ 2`| 5 | 3 | 1.6666666666666667 | 
|`+ 0`| 5 | 4 | 1,25 | 
|`+ 3`| 8 | 5 | 1.6 | 
|`- 1`| 7 | 4 | 1,75 | 
|`+ 4`| 11 | 5 | 2.2 | 
|`- 2`| 9 | 4 | 2,25 | 
|`- 0`| 9 | 3 | 3.0 | 

Bản sao`2`các giá trị chứng minh tại sao nhiều tập hợp lại quan trọng. Sau thao tác thứ ba, có hai lần xuất hiện riêng biệt của`2`, và sau khi loại bỏ một trong số chúng vẫn còn một cái khác`2`đóng góp cho cả hai`total`Và`count`. Bất biến theo dõi các lần xuất hiện thay vì các giá trị riêng biệt, do đó, nó xử lý tình huống này một cách tự nhiên. 

Đối với Mẫu 2, mọi thao tác đều cộng một giá trị liên tiếp gần với (10^9). 

| Hoạt động |`total`|`count`| Trung bình | 
| --- | --- | --- | --- | 
|`+ 999999001`| 999999001 | 1 | 999999001.0 | 
|`+ 999999002`| 1999998003 | 2 | 999999001.5 | 
|`+ 999999003`| 2999997006 | 3 | 999999002.0 | 
|`+ 999999004`| 3999996010 | 4 | 999999002.5 | 
|`+ 999999005`| 4999995015 | 5 | 999999003.0 | 

Ví dụ này thực hiện phạm vi trên của các giá trị. Tổng vẫn là một số nguyên Python chính xác và phép chia cuối cùng cho kết quả trung bình cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi hoạt động thực hiện một lượng công việc không đổi. | 
| Không gian | (O(n)) | Chuỗi câu trả lời yêu cầu bộ nhớ (O(n)); bản thân thuật toán sử dụng trạng thái phụ trợ (O(1)). | 

Với (n\leq200000), xử lý tuyến tính có nghĩa là khoảng một lần chuyển qua đầu vào và một lượng số học không đổi cho mỗi phép toán. Điều này dễ dàng tương thích với giới hạn hai giây. Đầu ra được lưu trữ cũng chỉ tuyến tính về số lượng thao tác, trong khi không cần biểu diễn bản thân multiset. 

## Trường hợp thử nghiệm 

Dây thử nghiệm bên dưới sử dụng tương tự`solve()`logic trong khi chuyển hướng đầu vào tiêu chuẩn. Vì cách trình bày văn bản dấu phẩy động có thể khác nhau trong khi cả hai đều đáp ứng giới hạn lỗi bắt buộc, nên trình trợ giúp sẽ so sánh các câu trả lời bằng số thay vì yêu cầu định dạng thập phân giống hệt nhau. Các mẫu được cung cấp được bao gồm, theo sau là các trường hợp cho nhiều tập hợp trống, các bản sao, giá trị biên và đầu vào có kích thước tối đa.```python
import sys
import io
import math

def solve():
    n = int(input())

    total = 0
    count = 0
    output = []

    for _ in range(n):
        op, x = input().split()
        x = int(x)

        if op == '+':
            total += x
            count += 1
        else:
            total -= x
            count -= 1

        if count == 0:
            output.append("0.0")
        else:
            output.append(str(total / count))

    sys.stdout.write("\n".join(output))

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    try:
        sys.stdin = io.StringIO(inp)
        input = sys.stdin.readline

        old_stdout = sys.stdout
        sys.stdout = io.StringIO()
        try:
            solve()
            return sys.stdout.getvalue()
        finally:
            sys.stdout = old_stdout
    finally:
        sys.stdin = old_stdin
        input = old_input

def assert_output(inp: str, expected):
    actual = run(inp).strip().splitlines()
    assert len(actual) == len(expected)

    for a, e in zip(actual, expected):
        assert math.isclose(
            float(a),
            float(e),
            rel_tol=1e-9,
            abs_tol=1e-9
        ), f"expected {e}, got {a}"

# Sample 1
sample1 = """\
9
+ 1
+ 2
+ 2
+ 0
+ 3
- 1
+ 4
- 2
- 0
"""

assert_output(sample1, [
    "1.0",
    "1.5",
    "1.66666666667",
    "1.25",
    "1.6",
    "1.75",
    "2.2",
    "2.25",
    "3.0",
])

# Sample 2
sample2 = """\
5
+ 999999001
+ 999999002
+ 999999003
+ 999999004
+ 999999005
"""

assert_output(sample2, [
    "999999001.0",
    "999999001.5",
    "999999002.0",
    "999999002.5",
    "999999003.0",
])

# Minimum size
assert_output("""\
1
+ 0
""", ["0.0"])

# Empty multiset after deletion, followed by another insertion
assert_output("""\
4
+ 5
- 5
+ 10
- 10
""", ["5.0", "0.0", "10.0", "0.0"])

# All values equal, with duplicate deletion
assert_output("""\
6
+ 7
+ 7
+ 7
- 7
- 7
- 7
""", ["7.0", "7.0", "7.0", "7.0", "7.0", "0.0"])

# Boundary values
assert_output("""\
4
+ 1000000000
+ 0
- 1000000000
- 0
""", ["1000000000.0", "500000000.0", "0.0", "0.0"])

# Maximum-size case: 200000 equal elements.
# This checks that the implementation remains linear and handles
# the largest allowed number of operations.
n = 200000
max_case = str(n) + "\n" + "+ 1\n" * n
max_expected = ["1.0"] * n

assert_output(max_case, max_expected)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / + 0`|`0.0`| Đầu vào tối thiểu và phần tử có giá trị bằng 0 | 
|`+ 5, - 5, + 10, - 10`|`5.0, 0.0, 10.0, 0.0`| Xử lý nhiều phần trống sau khi xóa | 
| Ba lần thêm và ba lần xóa`7`| Sáu câu trả lời bằng`7.0`, kết thúc bằng`0.0`| Các lần xuất hiện trùng lặp và chuyển sang trạng thái trống | 
|`+ 1000000000, + 0, - 1000000000, - 0`|`1000000000.0, 500000000.0, 0.0, 0.0`| Giá trị tối đa, số 0 và thứ tự xóa | 
| 200.000 bổ sung`1`| 200.000 câu trả lời bằng`1.0`| Số lượng hoạt động tối đa và hiệu suất tuyến tính | 

## Vỏ cạnh 

Multiset trống được xử lý rõ ràng. Đối với đầu vào```
2
+ 5
- 5
```hoạt động đầu tiên mang lại`total = 5`Và`count = 1`, vậy câu trả lời là`5.0`. Hoạt động thứ hai thay đổi trạng thái thành`total = 0`Và`count = 0`. Phép chia bị bỏ qua và thuật toán đưa ra`0.0`, khớp với định nghĩa đặc biệt cho nhiều tập hợp trống. 

Các giá trị trùng lặp được biểu diễn tự động vì`count`đếm số lần xuất hiện thay vì số lượng riêng biệt. Vì```
3
+ 2
+ 2
- 2
```các tiểu bang là`(2, 1)`,`(4, 2)`, Và`(2, 1)`vì`(total, count)`. Mỗi câu trả lời đều`2.0`. Không cần phải phân biệt sự xuất hiện vật lý nào đã bị loại bỏ vì các giá trị bằng nhau có đóng góp giống hệt nhau. 

Giá trị 0 không yêu cầu xử lý số học đặc biệt. Vì```
2
+ 5
+ 0
```trạng thái thay đổi từ`(5, 1)`ĐẾN`(5, 2)`, sản xuất`5.0`Và`2.5`. Số 0 làm tăng số phần tử mà không thay đổi tổng, chính xác như dự đoán của biểu diễn tổng hợp. 

Giá trị tối đa cũng không gây tràn trong Python. Với```
2
+ 1000000000
+ 1000000000
```tổng số trở thành`2000000000`, và số đếm trở thành`2`, cho`1000000000.0`. Trên 200.000 lần chèn như vậy, tổng số có thể đạt tới`200000000000000`, mà Python đại diện chính xác. 

Số lượng thao tác tối đa không yêu cầu cấu trúc thuật toán lớn hơn. Với 200.000 bổ sung, mọi hoạt động vẫn bao gồm một bản cập nhật cho`total`, một bản cập nhật cho`count`, và một phép chia. Tổng công việc tăng trưởng tuyến tính, đây là đặc tính làm cho giải pháp phù hợp với giới hạn đầu vào.
