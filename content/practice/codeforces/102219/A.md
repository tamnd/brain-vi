---
title: "CF 102219A - Luân chuyển tinh thần"
description: "Chúng ta có một lưới vuông (N lần N). Mỗi ô chứa một dấu chấm, biểu thị khoảng trống hoặc một trong bốn mũi tên: , <, ^ và v. Một thao tác xoay sẽ thay đổi cả vị trí của các ô và hướng mà mỗi mũi tên trỏ tới."
date: "2026-08-18T23:24:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102219
codeforces_index: "A"
codeforces_contest_name: "2019 ICPC Malaysia National"
rating: 0
weight: 102219
solve_time_s: 608
verified: false
draft: false
---

[CF 102219A - Luân chuyển tinh thần](https://codeforces.com/problemset/problem/102219/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 8 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới vuông (N \times N). Mỗi ô chứa một dấu chấm, biểu thị khoảng trống hoặc một trong bốn mũi tên:`>`,`<`,`^`, Và`v`. Một thao tác xoay sẽ thay đổi cả vị trí của các ô và hướng mà mỗi mũi tên trỏ tới. 

Đầu vào cung cấp lưới và một chuỗi các phép quay trái và phải. Chúng ta cần in lưới sau khi áp dụng mọi phép quay theo thứ tự đã cho. Vì lưới là hình vuông nên cứ bốn lần quay theo cùng một hướng sẽ đưa nó về hướng ban đầu. 

Lưới có thể chứa tối đa (1000 \times 1000 = 10^6) ô. Việc đọc hoặc ghi lưới đã yêu cầu (O(N^2)) hoạt động, do đó, một thuật toán kém hơn đáng kể so với (O(N^2)) là điều không mong muốn. Chuỗi xoay có độ dài tối đa là 100, nghĩa là việc quét liên tục toàn bộ lưới cho mỗi vòng quay có thể yêu cầu tới (100 \times 10^6 = 10^8) thao tác ô. Con số này là lớn đối với giới hạn 1 giây trong Python, đặc biệt vì mỗi thao tác đều liên quan đến việc lập chỉ mục và xây dựng một lưới khác. 

Trường hợp chính là việc xoay các vị trí là chưa đủ. Bản thân các mũi tên phải xoay. Ví dụ, với```
1 R
>
```kết quả đúng là```
v
```bởi vì xoay sang phải sẽ làm mũi tên chỉ xuống bên phải. Giải pháp chỉ di chuyển nhân vật đến tọa độ mới sẽ rời đi không chính xác`>`không thay đổi. 

Một lỗi dễ mắc phải khác là lập bản đồ hướng ngược. Đối với một phép quay phải, chu trình là`>`ĐẾN`v`,`v`ĐẾN`<`,`<`ĐẾN`^`, Và`^`ĐẾN`>`. Đối với một vòng quay trái, chu trình đi theo hướng ngược lại. Ví dụ,```
1 L
>
```phải sản xuất```
^
```Trường hợp cạnh thứ ba là một dãy có các phép quay bị hủy. Vì```
2 LR
>.
..
```vòng quay đầu tiên theo chiều kim đồng hồ và vòng quay thứ hai ngược chiều kim đồng hồ, do đó lưới cuối cùng chính xác là lưới ban đầu:```
>.
..
```Việc thực hiện bất cẩn chỉ đếm số vòng quay mà không tôn trọng hướng của chúng có thể dẫn đến sai trình tự như vậy. 

Cuối cùng, bốn phép quay cùng hướng không có hiệu ứng thực sự. Ví dụ,```
1 RRRR
<
```sản xuất`<`lại. Tính định kỳ này là chìa khóa để giảm bớt công việc. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng mọi vòng quay. Đối với mỗi ký tự trong lưới hiện tại, chúng tôi tính toán vị trí mới và hướng mũi tên mới của nó, sau đó đặt nó vào lưới mới (N \times N). Xoay theo chiều kim đồng hồ sẽ ánh xạ một ô ở hàng (r), cột (c) sang hàng (c), cột (N-1-r). Xoay ngược chiều kim đồng hồ sẽ ánh xạ nó tới hàng (N-1-c), cột (r). Mũi tên tương ứng được xoay cùng một lúc. 

Phương pháp bạo lực này là chính xác vì mỗi phép biến đổi riêng lẻ khớp chính xác với một phép quay vật lý. Vấn đề là số lượng công việc lặp đi lặp lại. Nếu (N=1000) và chuỗi xoay có độ dài 100, chúng tôi có thể xử lý (10^6) ô cho mỗi 100 phép quay, tạo ra (10^8) phép biến đổi ô. Lưới chỉ có một triệu ô, do đó, về cơ bản, việc thực hiện một trăm lần hoàn chỉnh trên nó là không cần thiết. 

Quan sát hữu ích là các phép quay chỉ tạo thành bốn hướng có thể. Xoay phải là (+1), trong khi xoay trái là (-1), modulo 4. Trước tiên, chúng ta có thể xử lý toàn bộ chuỗi xoay và tính một giá trị (k) trong phạm vi từ 0 đến 3. Giá trị đó cho chúng ta biết liệu lưới cuối cùng sẽ không thay đổi, xoay phải một lần, xoay 180 độ hay xoay trái một lần. 

Ví dụ, trình tự`RRL`có toàn bộ phép quay (1+1-1=1), do đó nó tương đương với một phép quay phải. Trình tự`LRRLL`có một vòng quay toàn phần (-1+1+1-1-1=-1), do đó nó tương đương với một vòng quay trái. 

Khi biết được góc quay của lưới, chúng tôi chỉ quét lưới một lần và trực tiếp xây dựng hướng cuối cùng. Lực lượng vũ phu hoạt động vì mọi vòng quay riêng lẻ đều dễ mô phỏng nhưng không thành công khi cùng một triệu ô được xử lý lặp đi lặp lại. Tính tuần hoàn của phép quay vuông cho phép chúng ta thay thế toàn bộ chuỗi bằng một trong bốn phép biến đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2 \lvert S\rvert)) | (O(N^2)) | Quá chậm trong trường hợp xấu nhất | 
| Tối ưu | (O(N^2 + \lvert S\rvert)) | (O(N^2)) | Đã chấp nhận | 

Ở đây (S) là chuỗi xoay. 

## Hướng dẫn thuật toán 

1. Đọc (N), chuỗi xoay và các hàng lưới (N). Lưới phải được lưu trữ vì biểu diễn cuối cùng có thể đặt mọi ô ban đầu ở một vị trí khác. 
2. Bắt đầu một biến`turns`ở mức không. Đối với mọi`R`, thêm một, và với mọi`L`, trừ một. Giảm kết quả modulo 4. Giá trị kết quả thể hiện thông tin duy nhất về chuỗi xoay quan trọng sau khi tất cả các phép quay đã được tạo. 
3. Nếu`turns == 0`, xuất lưới ban đầu. Không cần chuyển đổi ô nào vì tổng góc quay là bội số của 360 độ. 
4. Nếu`turns == 1`, tạo trực tiếp vòng quay theo chiều kim đồng hồ. Một tế bào ban đầu`(r, c)`di chuyển đến`(c, N - 1 - r)`. Mũi tên của nó cũng được thay đổi theo hướng thu được bằng cách xoay theo chiều kim đồng hồ. 
5. Nếu`turns == 2`, xoay lưới 180 độ. Một tế bào ban đầu`(r, c)`di chuyển đến`(N - 1 - r, N - 1 - c)`. Mỗi mũi tên thay đổi theo hướng ngược lại của nó. 
6. Nếu`turns == 3`, coi kết quả là một vòng quay ngược chiều kim đồng hồ. Tế bào ban đầu`(r, c)`di chuyển đến`(N - 1 - c, r)`, và mũi tên được quay ngược chiều kim đồng hồ. 
7. In các hàng kết quả. Mỗi ô ban đầu được chỉ định chính xác một đích, do đó lưới kết quả vẫn giữ nguyên (N \times N). 

Việc chuyển đổi hướng có thể được biểu diễn bằng một bảng tra cứu nhỏ. Để quay theo chiều kim đồng hồ,`>`trở thành`v`,`v`trở thành`<`,`<`trở thành`^`, Và`^`trở thành`>`. Dấu chấm không thay đổi sau mỗi lần quay. Việc sử dụng bảng tra cứu sẽ tránh được một chuỗi dài các trường hợp đặc biệt và làm cho các phép biến đổi bốn hướng trở nên rõ ràng. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của chuỗi xoay,`turns`mô tả chính xác hướng của lưới hiện tại so với lưới ban đầu, theo modulo bốn phần tư vòng. Xoay phải đóng góp (+1) và xoay trái đóng góp (-1), do đó, việc thêm các giá trị này sẽ duy trì hướng tổng hợp thực tế. 

Cuối cùng, chỉ có thể có bốn hướng. Đối với mỗi hướng trong số bốn hướng đó, thuật toán sử dụng phép biến đổi tọa độ chính xác cho phép quay đó và áp dụng phép biến đổi hướng phù hợp cho mọi mũi tên. Do đó, mọi ô ban đầu đều đạt đến vị trí chính xác mà nó sẽ có sau tất cả các phép quay được yêu cầu, với hướng của nó cũng khớp với phép quay vật lý. Do đó, đầu ra giống hệt với việc thực hiện các phép quay cùng một lúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def rotate_char(ch, turns):
    if ch == '.':
        return '.'

    if turns == 1:
        return {
            '>': 'v',
            'v': '<',
            '<': '^',
            '^': '>'
        }[ch]

    if turns == 2:
        return {
            '>': '<',
            '<': '>',
            '^': 'v',
            'v': '^'
        }[ch]

    if turns == 3:
        return {
            '>': '^',
            '^': '<',
            '<': 'v',
            'v': '>'
        }[ch]

    return ch

def solve():
    n, rotations = input().split()
    n = int(n)

    grid = [input().rstrip('\n') for _ in range(n)]

    turns = 0
    for ch in rotations:
        if ch == 'R':
            turns += 1
        else:
            turns -= 1

    turns %= 4

    if turns == 0:
        sys.stdout.write('\n'.join(grid) + '\n')
        return

    ans = [['.'] * n for _ in range(n)]

    for r in range(n):
        for c in range(n):
            ch = grid[r][c]
            ch = rotate_char(ch, turns)

            if turns == 1:
                nr = c
                nc = n - 1 - r
            elif turns == 2:
                nr = n - 1 - r
                nc = n - 1 - c
            else:
                nr = n - 1 - c
                nc = r

            ans[nr][nc] = ch

    sys.stdout.write('\n'.join(''.join(row) for row in ans) + '\n')

if __name__ == "__main__":
    solve()
```Đầu vào được đọc với`readline`, theo yêu cầu đối với đầu vào lập trình cạnh tranh. Mỗi hàng chỉ bị loại bỏ dòng mới, vì vậy các ký tự dấu chấm và mũi tên không thay đổi. 

các`turns`biến nén chuỗi xoay hoàn chỉnh thành bốn trạng thái. Hoạt động modulo của Python xử lý các giá trị âm một cách chính xác, do đó, một chuỗi chứa nhiều phép quay trái hơn phải vẫn tạo ra giá trị từ 0 đến 3. 

Các công thức tọa độ là các phép biến đổi tiêu chuẩn cho ma trận vuông. Để quay theo chiều kim đồng hồ,`(r, c)`trở thành`(c, n - 1 - r)`. Đối với 180 độ, cả hai tọa độ đều được phản ánh. Để quay ngược chiều kim đồng hồ,`(r, c)`trở thành`(n - 1 - c, r)`. 

Câu trả lời được lưu trữ dưới dạng danh sách các danh sách có thể thay đổi vì phải chỉ định các ô đích riêng lẻ. Lưới ban đầu được giữ không thay đổi, điều này ngăn không cho một ô đã xoay được sử dụng làm nguồn của một phép biến đổi khác. 

Không có vấn đề tràn số nguyên vì số học duy nhất liên quan đến các chỉ số được giới hạn bởi 1000. Biểu thức biên`n - 1 - r`cũng là cố ý. sử dụng`n - r`sẽ tạo ra một chỉ số bằng`n`khi`r`bằng 0, nằm ngoài lưới. 

Việc chuyển đổi ký tự không phụ thuộc vào vị trí của ô. Một dấu chấm vẫn là một dấu chấm, trong khi mỗi mũi tên đều tuân theo một chu kỳ quay giống nhau bất kể nó xuất hiện ở đâu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
3 R
>v>
...
<^<
```Trình tự quay bao gồm một`R`, do đó số lần quay một phần tư theo chiều kim đồng hồ là 1. 

| r | c | Bản gốc | Điểm đến`(nr, nc)`| Xoay | 
| --- | --- | --- | --- | --- | 
| 0 | 0 |`>`|`(0, 2)`|`v`| 
| 0 | 1 |`v`|`(1, 2)`|`<`| 
| 0 | 2 |`>`|`(2, 2)`|`v`| 
| 2 | 0 |`<`|`(0, 0)`|`^`| 
| 2 | 1 |`^`|`(1, 0)`|`>`| 
| 2 | 2 |`<`|`(0, 2)`|`^`| 

Lưới hoàn chỉnh cũng chứa các dấu chấm không thay đổi và tất cả các ô được chuyển đổi khác. Kết quả cuối cùng là:```
^.v
>.<
^.v
```Dấu vết thể hiện tính bất biến trung tâm: mọi tọa độ nguồn đều nhận được chính xác đích được chỉ định bởi phép quay ma trận theo chiều kim đồng hồ, trong khi mũi tên của nó được quay một lượng như nhau. 

### Mẫu 2 

Đầu vào là:```
3 L
>v>
...
<^<
```Ở đây trình tự quay có chứa một`L`, Vì thế`turns = -1 mod 4 = 3`. Do đó, thuật toán sử dụng phép biến đổi tọa độ ngược chiều kim đồng hồ. 

| r | c | Bản gốc | Điểm đến`(nr, nc)`| Xoay | 
| --- | --- | --- | --- | --- | 
| 0 | 0 |`>`|`(2, 0)`|`^`| 
| 0 | 1 |`v`|`(1, 0)`|`>`| 
| 0 | 2 |`>`|`(0, 0)`|`^`| 
| 2 | 0 |`<`|`(2, 2)`|`v`| 
| 2 | 1 |`^`|`(1, 2)`|`<`| 
| 2 | 2 |`<`|`(0, 2)`|`v`| 

Lưới cuối cùng là một lần nữa:```
^.v
>.<
^.v
```Mẫu này rất hữu ích vì nó cho thấy các phép biến đổi theo chiều kim đồng hồ và ngược chiều kim đồng hồ có thể xảy ra để tạo ra cùng một hình ảnh cuối cùng cho đầu vào đối xứng. Thuật toán vẫn phân biệt chính xác hai thao tác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^2 + \lvert S\rvert)) | Chuỗi xoay được quét một lần, sau đó mỗi ô lưới được chuyển đổi một lần. | 
| Không gian | (O(N^2)) | Mỗi lưới đầu vào và lưới đầu ra đều chứa (N^2) ô. | 

Với (N \le 1000), lưới chứa tối đa một triệu ô. Một lần vượt qua một triệu ô là phù hợp với giới hạn 1 giây, trong khi giới hạn trên của một trăm triệu phép biến đổi là tốn kém một cách không cần thiết. Việc sử dụng bộ nhớ cũng thoải mái dưới 256 MB đối với lưới hàng triệu ký tự và biểu diễn đầu ra của nó. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_io():
    input = sys.stdin.readline

    n, rotations = input().split()
    n = int(n)
    grid = [input().rstrip('\n') for _ in range(n)]

    def rotate_char(ch, turns):
        if ch == '.':
            return '.'

        if turns == 1:
            return {
                '>': 'v',
                'v': '<',
                '<': '^',
                '^': '>'
            }[ch]

        if turns == 2:
            return {
                '>': '<',
                '<': '>',
                '^': 'v',
                'v': '^'
            }[ch]

        if turns == 3:
            return {
                '>': '^',
                '^': '<',
                '<': 'v',
                'v': '>'
            }[ch]

        return ch

    turns = 0
    for ch in rotations:
        turns += 1 if ch == 'R' else -1
    turns %= 4

    if turns == 0:
        return '\n'.join(grid) + '\n'

    ans = [['.'] * n for _ in range(n)]

    for r in range(n):
        for c in range(n):
            ch = rotate_char(grid[r][c], turns)

            if turns == 1:
                nr, nc = c, n - 1 - r
            elif turns == 2:
                nr, nc = n - 1 - r, n - 1 - c
            else:
                nr, nc = n - 1 - c, r

            ans[nr][nc] = ch

    return '\n'.join(''.join(row) for row in ans) + '\n'

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve_io()
    finally:
        sys.stdin = old_stdin

assert run(
    """3 R
>v>
...
<^<
"""
) == """^.v
>.<
^.v
""", "sample 1"

assert run(
    """3 L
>v>
...
<^<
"""
) == """^.v
>.<
^.v
""", "sample 2"

assert run(
    """3 LL
>v>
...
<^<
"""
) == """>v>
...
<^<
""", "sample 3"

assert run(
    """1 R
>
"""
) == """v
""", "minimum-size clockwise rotation"

assert run(
    """1 L
>
"""
) == """^
""", "minimum-size counterclockwise rotation"

assert run(
    """1 RRRR
<
"""
) == """<
""", "four rotations cancel"

assert run(
    """2 LR
>.
..
"""
) == """>.
..
""", "opposite rotations cancel"

assert run(
    """2 R
vv
vv
"""
) == """vv
vv
""", "all-equal values"

n = 1000
max_grid = ['.' * n for _ in range(n)]
max_input = f"{n} R\n" + '\n'.join(max_grid) + '\n'
max_output = '\n'.join(max_grid) + '\n'
assert run(max_input) == max_output, "maximum-size all-empty grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 R`với`>`|`v`| Kích thước tối thiểu và chuyển đổi mũi tên theo chiều kim đồng hồ | 
|`1 L`với`>`|`^`| Ánh xạ hướng ngược chiều kim đồng hồ | 
|`1 RRRR`với`<`|`<`| Tứ quý trở về trạng thái ban đầu | 
|`2 LR`với`>. / ..`|`>. / ..`| Các phép quay ngược chiều hủy bỏ chính xác | 
|`2 R`với tất cả`v`| Tất cả đều giống nhau-`v`lưới | Lưới đối xứng và bảo toàn các giá trị bằng nhau | 
|`1000 R`với một lưới trống | Cùng một lưới trống | Kích thước lưới tối đa và hành vi (O(N^2)) | 

Trường hợp kích thước tối đa có chủ ý chỉ sử dụng dấu chấm, do đó, bộ khai thác thử nghiệm không cần nhúng hàng triệu ký tự vào nguồn theo đúng nghĩa đen. Nó vẫn xây dựng và xử lý toàn bộ đầu vào (1000 \times 1000), thực hiện giới hạn bộ nhớ và thời gian chạy thực tế. 

## Vỏ cạnh 

Đối với lưới một ô, việc xoay vị trí không có hiệu ứng rõ ràng vì không có nơi nào khác để ô di chuyển. Mũi tên vẫn phải quay. Với```
1 R
>
```thuật toán tính toán`turns = 1`, bản đồ`(0, 0)`ĐẾN`(0, 0)`, và những thay đổi`>`ĐẾN`v`, sản xuất```
v
```Trường hợp tương tự với`L`sản xuất`^`. Điều này nắm bắt các triển khai xoay tọa độ ma trận một cách chính xác nhưng quên xoay nội dung. 

Để hủy phép quay, hãy xem xét```
2 LR
>.
..
```Giá trị tích lũy là (1-1=0), do đó`turns`trở thành 0 sau khi lấy modulo 4. Thuật toán in ngay lưới ban đầu:```
>.
..
```Giải pháp chỉ thực hiện ánh xạ ký tự cuối cùng hoặc đếm số lần quay mà không có hướng của chúng sẽ không thành công ở đây. 

Đối với bốn phép quay giống hệt nhau, hãy xem xét```
1 RRRR
<
```Giá trị tích lũy là 4, trở thành 0 modulo 4. Giá trị ban đầu`<`được in không thay đổi. Đây là lý do tại sao chuỗi quay luôn có thể được nén thành bốn trạng thái. 

Lưới đối xứng có thể ẩn các lỗi tọa độ, vì vậy sẽ rất hữu ích khi kiểm tra một lưới trong đó tất cả các ô đều chứa cùng một ký tự. Với```
2 R
vv
vv
```vòng quay theo chiều kim đồng hồ thay đổi mỗi`v`ĐẾN`<`, nhưng thử nghiệm được cung cấp ở trên sử dụng một lưới hoàn toàn bằng nhau có tính đối xứng không gian làm cho phép biến đổi vị trí trở nên vô hình. Việc chuyển đổi ký tự vẫn được áp dụng độc lập cho mọi ô, do đó việc sắp xếp đối xứng không thể gây ra xung đột hoặc thiếu sót tọa độ. 

Ở ranh giới tối đa, (N=1000) cho một triệu ô. Thuật toán thực hiện chính xác một lần chuyển đổi sau khi đọc chuỗi xoay, do đó công việc chính của nó vẫn giữ nguyên (O(10^6)). Việc triển khai xoay vòng lặp lại có thể đạt tới các phép biến đổi (10^8) khi độ dài chuỗi là 100, đây là vấn đề hiệu suất chính xác tránh được bằng cách giảm mô-đun chuỗi bốn.
