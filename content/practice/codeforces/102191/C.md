---
title: "CF 102191C - Sắp xếp chỗ ngồi"
description: "Chúng ta có một chỗ ngồi hình tròn được biểu thị bằng hoán vị a là 1..n. Các sinh viên a[i] và a[(i+1) mod n] là hàng xóm trong cách sắp xếp cũ."
date: "2026-08-25T05:19:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102191
codeforces_index: "C"
codeforces_contest_name: "PSUT Coding Marathon 2019"
rating: 0
weight: 102191
solve_time_s: 1651
verified: false
draft: false
---

[CF 102191C - Sắp xếp chỗ ngồi](https://codeforces.com/problemset/problem/102191/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 27 phút 31 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chỗ ngồi hình tròn được biểu thị bằng một hoán vị`a`của`1..n`. các sinh viên`a[i]`Và`a[(i+1) mod n]`là hàng xóm trong sự sắp xếp cũ. Chúng ta cần in một hoán vị khác của cùng những học sinh đó sao cho mọi cặp hàng xóm trong vòng tròn mới không liền kề trong vòng tròn cũ. Các phần tử đầu tiên và cuối cùng của hoán vị được in ra cũng là các phần tử lân cận, vì vậy cặp bao quanh phải thỏa mãn cùng một điều kiện. Vấn đề ban đầu có`3 <= n <= 3 * 10^5`và yêu cầu bất kỳ sự sắp xếp hợp lệ nào, hoặc`-1`khi không tồn tại. citturn0search0 

Bản thân ID sinh viên không còn quan trọng nữa khi đã biết hoán vị cũ. Điều quan trọng là vị trí của mỗi học sinh trong vòng tròn cũ. Nếu hai vị trí cũ chứ đừng nói đến giai thừa. Một giải pháp nên thực hiện một lượng công việc không đổi cho mỗi học sinh, điều đó có nghĩa là`O(n)`xây dựng là mục tiêu tự nhiên. Python có thể thoải mái xử lý vài trăm nghìn số nguyên trong thời gian tuyến tính, trong khi việc thử hoán vị sẽ hoàn toàn không khả thi. 

Có hai trường hợp nhỏ không thể xảy ra. Vì`n = 3`, mọi cặp học sinh đều đã kề nhau ở hình tròn cũ nên không có cặp nào có thể kề nhau ở hình tròn mới. Ví dụ,`3 / 1 2 3`có các cặp hình tròn duy nhất có thể`{1,2}`,`{2,3}`, Và`{3,1}`, tất cả đều bị cấm, nên câu trả lời là`-1`. Vì`n = 4`, chu kỳ cũ là`1-2-3-4-1`. Phần bù của nó chỉ chứa các cạnh`1-3`Và`2-4`, tạo thành hai cặp không liên kết với nhau nên không thể sắp xếp theo vòng tròn. Như vậy`4 / 1 2 3 4`cũng yêu cầu`-1`. 

Sai lầm dễ mắc thứ hai là quên cặp được tạo bởi phần tử đầu ra đầu tiên và cuối cùng. Vì`n = 5`, sự sắp xếp`1 3 5 2 4`hoạt động: mỗi cặp liên tiếp khác nhau hai vị trí modulo năm, bao gồm`4`Và`1`. Cấu trúc chỉ kiểm tra các phần tử liên tiếp bên trong có thể vô tình chấp nhận một cặp cuối cùng không hợp lệ. 

Các giá trị chẵn của`n`cần điều chỉnh riêng. Vì`n = 6`, chỉ cần lấy vị trí lẻ theo sau là vị trí chẵn sẽ cho`1 3 5 2 4 6`, nhưng cặp cuối cùng`6,1`bao gồm những người hàng xóm cũ. Trình tự đã sửa`1 3 5 2 6 4`cũng tránh cặp đó. Đây là lý do trường hợp chẵn không thể sử dụng một cách mù quáng cách xây dựng giống như trường hợp lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force trực tiếp là tạo ra mọi hoán vị của học sinh và kiểm tra xem nó có`n`các cặp hàng xóm hình tròn đều khác với các cặp hàng xóm cũ. Điều này đúng vì mọi chỗ ngồi mới đều được xem xét và chỗ ngồi hợp lệ đầu tiên có thể được trả lại. Vấn đề là số lượng ứng viên. có`n!`hoán vị và kiểm tra một ứng cử viên`Theta(n)`thời gian, cho`Theta(n * n!)`hoạt động trong trường hợp xấu nhất. Tại`n = 3 * 10^5`, thậm chí viết ra biểu thức`300000 * 300000!`đã mô tả một con số vượt xa mọi thứ mà một chương trình có thể liệt kê. 

Quan sát hữu ích là chúng ta không cần phải suy luận gì về ID sinh viên. Hãy coi các vị trí cũ là`0, 1, ..., n-1`. Hai vị trí bị cấm liên tiếp trong đáp án khi khoảng cách vòng tròn của chúng là`1`. Do đó chúng ta chỉ cần một hoán vị các vị trí trong đó mỗi cặp liên tiếp có khoảng cách khác nhau`1`. 

Đối với số lẻ`n`, lấy tất cả các vị trí được lập chỉ mục chẵn theo sau là tất cả các vị trí được lập chỉ mục lẻ, sử dụng chỉ mục dựa trên 0. Ở các vị trí dựa trên một, đây là`1, 3, 5, ..., 2, 4, 6, ...`. Trong mỗi nhóm, các vị trí liên tiếp cách nhau hai. Tại ranh giới giữa các nhóm, sự khác biệt cũng ít nhất là hai modulo`n`bởi vì`n`thật kỳ quặc. Cặp cuối cùng có cùng tính chất. 

Thậm chí`n >= 6`, trình tự tương tự có chính xác một quá trình chuyển đổi có vấn đề, đó là sự chuyển đổi từ vị trí chẵn cuối cùng trở lại vị trí đầu tiên. Hoán đổi hai phần tử cuối cùng sẽ sửa nó. Tương đương, đối với`n = 6`chúng tôi có được`1 3 5 2 6 4`, và cho`n = 8`chúng tôi có được`1 3 5 7 2 8 6 4`. Bây giờ mỗi cặp lân cận có khoảng cách vị trí cũ ít nhất là hai. 

Cấu trúc này độc lập với các giá trị được lưu trữ trong hoán vị. Chúng tôi xây dựng thứ tự yêu cầu của các vị trí cũ, sau đó xuất ra các sinh viên chiếm giữ các vị trí đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu |`O(n * n!)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị vòng cũ`a`. Chúng tôi sẽ xây dựng thứ tự các chỉ số của nó thay vì cố gắng thao túng trực tiếp ID sinh viên. 

2. Nếu`n < 5`, in`-1`. Vì`n = 3`mọi cặp đều bị cấm, trong khi đối với`n = 4`các cặp duy nhất được phép tạo thành hai cạnh rời nhau, do đó không trường hợp nào có thể tạo thành chu trình Hamilton. 

3. Nếu`n`là lẻ, thu thập vị trí`0, 2, 4, ...`đầu tiên, tiếp theo là vị trí`1, 3, 5, ...`. Trong ký hiệu dựa trên một đây là`1, 3, 5, ..., 2, 4, 6, ...`. 

4. Nếu`n`chẵn, xây dựng dãy giống nhau và hoán đổi hai vị trí cuối cùng của nó. Việc hoán đổi sẽ loại bỏ quá trình chuyển đổi bao quanh có vấn đề duy nhất được tạo bởi thứ tự chẵn-lẻ chưa được sửa đổi. 

5. Chuyển thứ tự vị trí đã xây dựng thành mã sinh viên bằng cách lấy`a[position]`cho mỗi vị trí được lựa chọn. In các ID này theo thứ tự. Từ`a`là một hoán vị, mỗi học sinh xuất hiện đúng một lần. 

Tại sao nó hoạt động: bên trong nhóm vị trí lẻ và bên trong nhóm vị trí chẵn, các vị trí cũ liên tiếp cách nhau đúng hai, vì vậy những học sinh đó trước đây không phải là hàng xóm của nhau. Đối với số lẻ`n`, hai điểm chuyển tiếp biên cũng có khoảng cách đường tròn ít nhất là hai. Thậm chí`n`, việc hoán đổi hai vị trí cuối cùng sẽ thay đổi hai chuyển tiếp bị ảnh hưởng sao cho khoảng cách vòng tròn của chúng cũng ít nhất là hai. Việc xây dựng là hoán vị của tất cả các vị trí cũ nên không có học sinh nào bị mất hoặc trùng lặp. Do đó, mọi cặp hàng xóm hình tròn mới đều không liền kề nhau ở chỗ ngồi cũ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(data=None):
    if data is None:
        n = int(input())
        a = list(map(int, input().split()))
    else:
        it = iter(data.split())
        n = int(next(it))
        a = [int(next(it)) for _ in range(n)]

    if n < 5:
        return "-1\n"

    order = list(range(0, n, 2)) + list(range(1, n, 2))

    if n % 2 == 0:
        order[-1], order[-2] = order[-2], order[-1]

    ans = [a[i] for i in order]
    return " ".join(map(str, ans)) + "\n"

if __name__ == "__main__":
    sys.stdout.write(solve())
```Nhánh đầu tiên xử lý hai kích thước không thể ngay lập tức. Không cần phải kiểm tra hoán vị thực tế vì tính khả thi của`n = 3`Và`n = 4`chỉ phụ thuộc vào cấu trúc vòng tròn. 

các`order`biểu thức tạo ra tất cả các vị trí chẵn dựa trên 0, theo sau là tất cả các vị trí lẻ dựa trên 0. Sử dụng chỉ số thay vì giá trị là chi tiết triển khai chính. Hoán vị đầu vào có thể chứa các sinh viên theo bất kỳ thứ tự nào, nhưng vị trí của nó luôn có cùng cấu trúc kề. 

Thậm chí`n`,`order[-1]`Và`order[-2]`là hai phần tử cuối cùng của dãy được xây dựng. Hoán đổi hai cái này chính xác là sự điều chỉnh cần thiết cho trường hợp chẵn. Việc lập chỉ mục phủ định của Python làm cho điều này trở nên độc lập với giá trị thực của`n`, và sớm hơn`n < 5`kiểm tra đảm bảo rằng các vị trí này tồn tại. 

Cuối cùng,`a[i]`ánh xạ từng vị trí cũ trở lại ID sinh viên của nó. Từ`a`được đảm bảo là một hoán vị, điều này tạo ra một hoán vị khác mà không yêu cầu một mảng hoặc một tập hợp đã truy cập. 

Không cần số học số nguyên liên quan đến các tích lớn, do đó việc tràn số nguyên không phải là vấn đề. Việc xây dựng và kết quả cuối cùng đều chạm vào mỗi học sinh một số lần không đổi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với hoán vị đầu vào`6 1 3 5 7 8 4 2`, chúng tôi có`n = 8`, vì vậy việc xây dựng kích thước chẵn được sử dụng. 

| Bước | Thứ tự vị trí | Đầu ra của sinh viên | 
|---|---|---| 
| Bắt đầu |`0 1 2 3 4 5 6 7`|`6 1 3 5 7 8 4 2`| 
| Vị trí lẻ trước |`0 2 4 6 1 3 5 7`|`6 3 7 4 1 5 8 2`| 
| Hoán đổi hai vị trí cuối cùng |`0 2 4 6 1 3 7 5`|`6 3 7 4 1 5 2 8`| 

Kết quả đầu ra là`6 3 7 4 1 5 2 8`. Nó khác với đầu ra mẫu, điều này được cho phép vì bài toán chấp nhận bất kỳ sự sắp xếp hợp lệ nào. Trình tự vị trí cũ là`0,2,4,6,1,3,7,5`. Khoảng cách lân cận hình tròn của nó là`2,2,2,3,2,4,2,3`, vì vậy không có gì là kề cận cũ. 

### Mẫu 2 

cho`n = 3`, thuật toán dừng trước khi xây dựng đơn hàng. 

| Bước |`n`| Kết quả | 
|---|---:|---| 
| Đọc đầu vào |`3`|`n < 5`| 
| Kiểm tra tính khả thi |`3 < 5`| in`-1`| 

Mỗi cặp trong vòng tròn ba người đều là cặp hàng xóm cũ, vì vậy không thể có sự sắp xếp vòng tròn mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian |`O(n)`| Việc xây dựng thứ tự vị trí và ánh xạ nó tới ID đều mất thời gian tuyến tính. | 
| Không gian |`O(n)`| Thứ tự vị trí và câu trả lời chứa`n`các phần tử. | 

Với`n <= 3 * 10^5`, thuật toán chỉ thực hiện một số lần duyệt không đổi trên vài trăm nghìn số nguyên. Điều này thoải mái trong giới hạn 1 giây và 256 MB dự định, trong khi mọi cấu trúc giai thừa hoặc bậc hai đều bị loại trừ bởi kích thước đầu vào. 

## Trường hợp thử nghiệm 

Trình kiểm tra bên dưới xác thực thuộc tính thay vì dựa vào một đầu ra hợp lệ cụ thể. Đây là cách phù hợp để kiểm tra một vấn đề mang tính xây dựng vì nhiều kết quả đầu ra có thể đúng. Xác nhận mẫu 1 cũng kiểm tra kết quả xác định chính xác được tạo ra bởi quá trình triển khai ở trên.```python
import io

def solve(data=None):
    if data is None:
        import sys
        input = sys.stdin.readline
        n = int(input())
        a = list(map(int, input().split()))
    else:
        it = iter(data.split())
        n = int(next(it))
        a = [int(next(it)) for _ in range(n)]

    if n < 5:
        return "-1\n"

    order = list(range(0, n, 2)) + list(range(1, n, 2))

    if n % 2 == 0:
        order[-1], order[-2] = order[-2], order[-1]

    ans = [a[i] for i in order]
    return " ".join(map(str, ans)) + "\n"

def run(inp: str) -> str:
    return solve(inp)

def valid(inp: str, out: str) -> bool:
    tokens = inp.split()
    n = int(tokens[0])
    a = list(map(int, tokens[1:]))

    if out.strip() == "-1":
        return n < 5

    b = list(map(int, out.split()))

    if len(b) != n or sorted(b) != sorted(a):
        return False

    pos = {x: i for i, x in enumerate(a)}

    for i in range(n):
        x = pos[b[i]]
        y = pos[b[(i + 1) % n]]
        d = (x - y) % n
        if d == 1 or d == n - 1:
            return False

    return True

# Provided sample 1.
sample1 = """8
6 1 3 5 7 8 4 2
"""
assert run(sample1) == "6 3 7 4 1 5 2 8\n"
assert valid(sample1, run(sample1))

# Provided sample 2.
sample2 = """3
1 3 2
"""
assert run(sample2) == "-1\n"
assert valid(sample2, run(sample2))

# Minimum possible n.
case3 = """5
1 2 3 4 5
"""
assert run(case3) == "1 3 5 2 4\n"
assert valid(case3, run(case3))

# Smallest even n for which a solution exists.
case4 = """6
1 2 3 4 5 6
"""
assert run(case4) == "1 3 5 2 6 4\n"
assert valid(case4, run(case4))

# Largest allowed n.
n = 300000
a = list(range(1, n + 1))
case5 = str(n) + "\n" + " ".join(map(str, a)) + "\n"
out5 = run(case5)
assert valid(case5, out5)

# Repeated values are not a valid input for this problem.
# The statement guarantees that the second line is a permutation,
# so an all-equal test is deliberately excluded rather than pretending
# that it is a legal test case.
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|`3 / 1 3 2`|`-1`| Trường hợp không thể tối thiểu | 
|`4 / 1 2 3 4`|`-1`| Trường hợp bất khả thi khác | 
|`5 / 1 2 3 4 5`|`1 3 5 2 4`| Công trình lẻ và ranh giới hình tròn | 
|`6 / 1 2 3 4 5 6`|`1 3 5 2 6 4`| Ngay cả việc xây dựng và trao đổi cuối cùng | 
|`300000 / 1 2 ... 300000`| Bất kỳ hoán vị hợp lệ nào | Hiệu suất kích thước tối đa và xử lý ranh giới | 

Một đầu vào hoàn toàn bằng nhau như`5 / 7 7 7 7 7`không thể là trường hợp kiểm thử cho bài toán đã nêu vì đầu vào được đảm bảo là một hoán vị của`1..n`. Việc coi nó như một trường hợp bình thường sẽ kiểm tra hành vi bên ngoài hợp đồng của vấn đề chứ không phải là một trường hợp đặc biệt của thuật toán. 

## Vỏ cạnh 

cho`n = 3`, coi như`3 / 1 2 3`. Các cạnh tròn cũ là`{1,2}`,`{2,3}`, Và`{3,1}`, bao gồm mọi cặp học sinh có thể có. Thuật toán in ngay lập tức`-1`, tránh mọi nỗ lực xây dựng một chu trình bất khả thi. 

Vì`n = 4`, coi như`4 / 1 2 3 4`. Cặp đôi không phải hàng xóm cũ duy nhất là`1-3`Và`2-4`. Một chỗ ngồi hình tròn mới sẽ cần bốn cạnh được phép trong khi hai cạnh được phép này bị ngắt kết nối, do đó không có giải pháp nào tồn tại. các`n < 5`điều kiện in chính xác`-1`. 

Đối với số lẻ`n = 5`, coi như`5 / 1 2 3 4 5`. Thứ tự vị trí là`0,2,4,1,3`, tương ứng với`1 3 5 2 4`. Sự khác biệt bên trong là hai vị trí, trong khi cặp bao quanh`4,1`cũng có khoảng cách tròn hai. Do đó, công trình xử lý ranh giới mà không có bất kỳ sự điều chỉnh đặc biệt nào. 

Thậm chí`n = 6`, thứ tự chẵn lẻ ban đầu sẽ là`1 3 5 2 4 6`. Cặp cuối cùng của nó`6,1`bị cấm vì những học sinh đó là hàng xóm trong vòng tròn cũ. Hoán đổi hai vị trí cuối cùng mang lại`1 3 5 2 6 4`. Các quá trình chuyển đổi bị ảnh hưởng trở thành`2-6`Và`4-1`, có khoảng cách vòng tròn lần lượt là bốn và ba, do đó vi phạm bao quanh sẽ biến mất. 

Các nhãn sinh viên thực tế có thể hoàn toàn tùy ý. Ví dụ: hoán vị mẫu bắt đầu bằng`6`còn hơn là`1`, nhưng công trình vẫn chỉ hoạt động trên vị trí của nó. Đây là lý do tại sao không cần sắp xếp, ánh xạ theo giá trị sinh viên hoặc tìm kiếm theo ID. 
:::
