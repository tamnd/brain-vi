---
title: "CF 102201A - A Cộng Bằng B"
description: "Chúng ta bắt đầu với hai số nguyên dương A và B, mỗi số có nhiều nhất là (10^{18}). Trong một thao tác, chúng ta có thể nhân đôi giá trị hoặc thêm giá trị này vào giá trị khác. Mục tiêu không phải là giảm thiểu số lượng hoạt động."
date: "2026-08-18T10:20:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102201
codeforces_index: "A"
codeforces_contest_name: "Moscow Pre-Finals Workshop 2019. KAIST Contest"
rating: 0
weight: 102201
solve_time_s: 707
verified: true
draft: false
---

[CF 102201A - A Cộng Bằng B](https://codeforces.com/problemset/problem/102201/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 11 phút 47 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với hai số nguyên dương,`A`Và`B`, mỗi cái nhiều nhất là (10^{18}). Trong một thao tác, chúng ta có thể nhân đôi giá trị hoặc thêm giá trị này vào giá trị khác. Mục tiêu không phải là giảm thiểu số lượng hoạt động. Chúng ta chỉ cần tạo ra một số chuỗi hợp lệ gồm tối đa 5000 thao tác để cuối cùng làm cho hai giá trị bằng nhau. 

Đầu ra là một chuỗi các tên hoạt động. Vì thẩm phán mô phỏng các hoạt động đó nên nhiệm vụ của chúng tôi mang tính xây dựng: chúng tôi cần tìm một quy trình đáng tin cậy luôn kết thúc đủ nhanh. 

Giới hạn trên lớn của (10^{18}) loại trừ mọi cách tiếp cận cố gắng tăng các số cho đến khi một giá trị chung được chọn thuận tiện nào đó. Giá trị như vậy có thể rất lớn và ngay cả việc tìm kiếm logarit trên các mục tiêu có thể cũng không tự nhiên gắn liền với các hoạt động được phép. Giới hạn hoạt động 5000 cũng cho chúng ta biết rằng giải pháp dự định phải có mức độ giảm mạnh thay vì chỉ dựa vào việc chấm dứt cuối cùng. 

Có một sự tương đương hữu ích ẩn giấu trong các phép nhân đôi. Giả sử về mặt khái niệm rằng trạng thái hiện tại của chúng ta là`(A, B)`Và`A`là chẵn. Nếu chúng ta xuất`B+=B`, trạng thái thực tế trở thành`(A, 2B)`. Chia cả hai tọa độ của trạng thái thực tế này cho 2 sẽ được`(A/2, B)`và việc nhân cả hai tọa độ với cùng một hằng số dương sẽ không thay đổi liệu chúng có thể được làm bằng nhau bằng cùng một chuỗi các phép tính cộng và nhân đôi hay không. Như vậy, xét về tỉ số giữa hai số,`B+=B`chúng ta hãy đối xử bình đẳng`A`như thể chúng ta đã chia tay`A`bằng 2. Đối xứng,`A+=A`cho phép chúng ta chia một cách khái niệm một số chẵn`B`bằng 2. 

Điều này đưa ra các trường hợp cạnh chính. 

Đối với đầu vào`5 5`, các số đã bằng nhau nên kết quả đúng chỉ đơn giản là`0`. Việc triển khai bất cẩn luôn thực hiện phép cộng trước khi kiểm tra sự bằng nhau sẽ tạo ra các hoạt động không cần thiết và thậm chí có thể di chuyển khỏi mục tiêu. 

Đối với đầu vào`2 3`, về mặt khái niệm chúng ta có thể chia`A`bằng 2 bằng cách thực hiện`B+=B`, đưa ra trạng thái chuẩn hóa tương đương`(1, 3)`. Sau đó`B+=A`tương ứng với`(1, 4)`, Và`A`có thể tăng gấp đôi hai lần để đạt được`4`. Mẫu sử dụng chính xác ý tưởng này, tạo ra bốn thao tác. Việc triển khai bất cẩn chỉ cố gắng cộng số nhỏ hơn vào số lớn hơn có thể tăng giá trị vô thời hạn thay vì khai thác lũy thừa của hai. 

Đối với đầu vào`1 1`, câu trả lời lại là`0`. Điều này nắm bắt các triển khai đi vào vòng lặp chuẩn hóa mà không kiểm tra trước xem các số có bằng nhau hay không. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực đơn giản sẽ coi mọi hoạt động có thể là một nhánh và tìm kiếm một trạng thái có`A == B`. Từ bất kỳ trạng thái nào cũng có bốn lựa chọn, vì vậy việc tìm kiếm lên tới độ sâu 5000 có 

[ 
1+4+4^2+\dots+4^{5000} 
] 

các chuỗi thao tác ứng cử viên, chỉ riêng cấp độ sâu nhất chứa (4^{5000}) khả năng. Ngay cả với việc cắt tỉa tích cực, không có không gian trạng thái hữu hạn hữu ích vì số lượng có thể tăng lên mà không bị giới hạn. Cách tiếp cận này là hoàn toàn không thực tế. 

Tìm kiếm brute-force có một thuộc tính hữu ích: mọi hoạt động đều duy trì tính tích cực và mọi đường dẫn thành công cuối cùng đều đạt đến hai giá trị bằng nhau. Vấn đề là tìm ra một con đường như vậy mà không cần khám phá một số lượng lớn các lựa chọn. 

Quan sát quan trọng là việc nhân đôi có thể được sử dụng như một phép chia về mặt khái niệm cho hai. Khi chúng tôi quyết định làm việc với các giá trị có hệ số tỷ lệ chung, mọi số chẵn có thể được chia cho hai bằng một thao tác được phép. Điều này có nghĩa là chúng ta có thể liên tục loại bỏ các thừa số của 2 khỏi cả hai số. 

Cuối cùng cả hai giá trị đều là số lẻ. Nếu chúng bằng nhau thì chúng ta đã hoàn thành. Ngược lại, giả sử`A < B`. Vì cả hai đều kỳ quặc,`A+B`là chẵn. Chúng tôi biểu diễn`B+=A`, thay đổi cặp thành`(A, A+B)`. Giá trị thứ hai bây giờ là số chẵn, vì vậy về mặt khái niệm chúng ta có thể chia nó cho hai nhiều lần cho đến khi nó trở thành số lẻ. 

Sau một phép chia, giá trị thứ hai mới là 

[ 
\frac{A+B}{2}. 
]

Bởi vì`A < B`, cái này hoàn toàn nhỏ hơn`B`. Quan trọng hơn, sự khác biệt được giảm đi ít nhất một nửa: 

[ 
\frac{A+B}{2}-A=\frac{B-A}{2}. 
] 

Nếu có thể có nhiều hơn một phép chia thì giá trị thứ hai thậm chí còn nhỏ hơn, do đó hiệu số ít nhất cũng giảm đi nhiều. 

Do đó, mỗi khi cả hai giá trị đều lẻ và không bằng nhau, một phép cộng theo sau là một số phép chia sẽ làm giảm đáng kể sự khác biệt của chúng. Vì chênh lệch ban đầu nhỏ hơn (10^{18}) nên chỉ có thể có khoảng 60 vòng rút gọn như vậy. 

Lập luận tương tự cũng đưa ra một giới hạn thoải mái về tổng số thao tác. Giữa hai vòng cộng, mỗi giá trị được chia cho hai cho đến khi trở thành số lẻ. Vì các giá trị không bao giờ vượt quá (10^{18}) sau khi chuẩn hóa nên cần tối đa khoảng 60 phép chia như vậy trong một vòng. Tổng số vẫn ở mức dưới 5000. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(4^{5000})) chuỗi ứng cử viên | (O(5000)) trên mỗi đường dẫn tìm kiếm, tổng thể có khả năng theo cấp số nhân | Quá chậm | 
| Tối ưu | (O(\log \max(A,B))) vòng rút gọn, với công việc chuẩn hóa logarit | (O(\log \max(A,B))) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`A`Và`B`. Nếu chúng đã bằng nhau thì xuất ra các phép toán bằng 0. Không có sự chuyển đổi là cần thiết. 
2. Trong khi`A`chẵn, ghi lại`B+=B`và thay thế về mặt khái niệm`A`qua`A/2`. Hoạt động được ghi sẽ nhân đôi tọa độ khác, do đó cặp thực tế là thừa số chung của hai lớn hơn cặp khái niệm. Hành vi bình đẳng không thay đổi theo tỷ lệ chung đó. 
3. Trong khi`B`chẵn, ghi lại`A+=A`và thay thế về mặt khái niệm`B`qua`B/2`. Đây là phiên bản đối xứng của bước trước. 
4. Nếu các giá trị chuẩn hóa bằng nhau thì dừng lại. Cả hai giá trị hiện đều là số lẻ, do đó mọi trạng thái không bằng nhau còn lại đều có cấu trúc đặc biệt đơn giản. 
5. Nếu`A < B`, ghi`B+=A`. Giá trị khái niệm của`B`trở thành`A+B`, đó là số chẵn vì cả hai đầu vào đều là số lẻ. 
6. Chia cái mới`B`bằng hai lần, ghi âm`A+=A`cho mọi phân chia khái niệm. Kết quả lẻ`B`nhỏ hơn lần trước`B`và khoảng cách giữa hai giá trị đã giảm đi ít nhất một nửa. 
7. Nếu`B < A`, thực hiện phép toán đối xứng`A+=B`, sau đó chia liên tục`A`bằng hai bằng cách ghi âm`B+=B`. 
8. Lặp lại quá trình chuẩn hóa và bổ sung cho đến khi hai giá trị khái niệm trở nên bằng nhau. Xuất tất cả các hoạt động được ghi lại theo thứ tự ban đầu. 

Điều bất biến là cặp khái niệm luôn biểu thị cặp thực tế cho đến khi nhân cả hai tọa độ với cùng một lũy thừa dương của hai. Việc chia tỷ lệ chung như vậy không thay đổi liệu chuỗi thao tác được phép trong tương lai có thể làm cho tọa độ bằng nhau hay không. Mỗi thao tác được ghi lại chính xác là thao tác thực tế thực hiện sự chuyển đổi khái niệm tương ứng. Khi cả hai giá trị đều là số lẻ và không bằng nhau, việc cộng giá trị nhỏ hơn vào giá trị lớn hơn sẽ làm cho giá trị lớn hơn đó trở thành chẵn, sau đó sự phân chia theo khái niệm sẽ làm giảm sự khác biệt ít nhất theo hệ số hai. Vì hiệu là số nguyên không âm nên quá trình này cuối cùng phải đạt đến số 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b = map(int, input().split())
    operations = []

    while a != b:
        while a % 2 == 0:
            # Conceptually: a /= 2.
            # Real operation: double b, preserving the ratio.
            operations.append("B+=B")
            a //= 2

        while b % 2 == 0:
            # Conceptually: b /= 2.
            # Real operation: double a, preserving the ratio.
            operations.append("A+=A")
            b //= 2

        if a == b:
            break

        if a < b:
            # Both are odd here, so a + b is even.
            operations.append("B+=A")
            b += a
        else:
            operations.append("A+=B")
            a += b

    print(len(operations))
    sys.stdout.write("\n".join(operations))

if __name__ == "__main__":
    solve()
```các`operations`list lưu trữ các lệnh thực tế phải được in. Các biến`a`Và`b`được coi là có chủ ý như các giá trị khái niệm được chuẩn hóa thay vì các giá trị bằng chữ thu được bằng cách mô phỏng mọi lệnh. 

Khi`a`là số chẵn, mã chia nó cho hai và ghi lại`B+=B`. Trong thực tế, tăng gấp đôi`B`sẽ sản xuất`(a, 2b)`. Đây chính xác là gấp đôi trạng thái khái niệm`(a/2, b)`, do đó hai biểu diễn có cùng tỷ lệ và hành vi bình đẳng trong tương lai giống nhau. 

Lý luận tương tự cũng được áp dụng khi`b`là chẵn, sử dụng`A+=A`. 

Khi cả hai giá trị đều là số lẻ, mã sẽ thêm giá trị nhỏ hơn vào giá trị lớn hơn. Ví dụ, nếu`a < b`, hoạt động`B+=A`thay đổi`b`ĐẾN`a+b`, tức là chẵn. Lần lặp tiếp theo ngay lập tức đi vào vòng chia và loại bỏ tất cả các thừa số của 2. 

Thứ tự của hai vòng chuẩn hóa không có ý nghĩa quan trọng đối với tính chính xác. Sau vòng lặp đầu tiên,`a`là số lẻ, và sau vòng lặp thứ hai,`b`thật kỳ quặc. Nếu hai giá trị bằng nhau trong quá trình chuẩn hóa, vòng lặp bên ngoài sẽ kết thúc trước khi thực hiện phép cộng không cần thiết. 

Số nguyên Python không bị tràn, do đó các giá trị được tạo tạm thời bằng phép cộng khái niệm sẽ an toàn. Quan trọng hơn, các giá trị chuẩn hóa vẫn bị giới hạn, vì sau khi cộng hai giá trị lẻ và chia kết quả cho ít nhất hai, giá trị mới nhiều nhất là giá trị lớn hơn trước đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với đầu vào mẫu`2 3`, thuật toán bắt đầu bằng`(2, 3)`. Giá trị đầu tiên là số chẵn, vì vậy về mặt khái niệm, chúng tôi chia nó cho hai trong khi ghi`B+=B`. 

| Bước | A | B | Hoạt động | 
| --- | --- | --- | --- | 
| 0 | 2 | 3 | bắt đầu | 
| 1 | 1 | 3 |`B+=B`| 
| 2 | 1 | 4 |`B+=A`| 
| 3 | 1 | 2 |`A+=A`| 
| 4 | 1 | 1 |`A+=A`| 

Trạng thái khái niệm kết thúc ở`(1, 1)`. Trong quá trình thực thi thực tế, các trạng thái tương ứng là`(2, 6)`,`(2, 8)`,`(4, 8)`, Và`(8, 8)`, do đó trình tự được in thực sự làm cho các giá trị ban đầu bằng nhau. 

Dấu vết thể hiện sự bất biến về tỷ lệ. Các giá trị chuẩn hóa có thể trở nên nhỏ hơn mặc dù mọi thao tác thực tế chỉ tăng một số. 

### Mẫu 2 

Xem xét đầu vào`1 5`. 

| Bước | A | B | Hoạt động | 
| --- | --- | --- | --- | 
| 0 | 1 | 5 | bắt đầu | 
| 1 | 1 | 6 |`B+=A`| 
| 2 | 1 | 3 |`A+=A`| 
| 3 | 1 | 4 |`B+=A`| 
| 4 | 1 | 2 |`A+=A`| 
| 5 | 1 | 1 |`A+=A`| 

Sự thay đổi bổ sung đầu tiên`(1,5)`ĐẾN`(1,6)`. Bởi vì cái mới`B`là số chẵn, về mặt khái niệm, chúng tôi chia nó để có được`(1,3)`. Quá trình tương tự sau đó biến đổi`(1,3)`ĐẾN`(1,1)`. 

Sự khác biệt bắt nguồn từ`4`ĐẾN`2`ĐẾN`0`, minh họa số đo giảm dần được sử dụng trong chứng minh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\log^2 \max(A,B))) trong giới hạn bảo toàn trực tiếp | Có các vòng giảm (O(\log \max(A,B))) và mỗi vòng có thể loại bỏ (O(\log \max(A,B))) hệ số của hai | 
| Không gian | (O(\log \max(A,B))) | Danh sách thao tác chỉ chứa các lệnh được tạo | 

Đối với các giá trị tối đa (10^{18}), có ít hơn 60 lần chia đôi có ý nghĩa trên mỗi thang đo. Giới hạn bảo thủ là dưới 4000 phép toán, thoải mái nằm trong mức yêu cầu 5000. Việc triển khai Python cũng chỉ thực hiện vài nghìn phép tính số nguyên, do đó, giới hạn 1 giây là đủ dễ dàng. 

## Trường hợp thử nghiệm 

Đầu ra không phải là duy nhất, vì vậy các thử nghiệm không nên so sánh chuỗi đầu ra thô với một chuỗi được xác định trước. Thay vào đó, trình trợ giúp kiểm tra sẽ phân tích cú pháp các thao tác được tạo và mô phỏng chúng trên các giá trị ban đầu, kiểm tra xem các giá trị cuối cùng có bằng nhau không và không có quá 5000 thao tác được tạo ra.```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    a, b = map(int, input().split())
    operations = []

    while a != b:
        while a % 2 == 0:
            operations.append("B+=B")
            a //= 2

        while b % 2 == 0:
            operations.append("A+=A")
            b //= 2

        if a == b:
            break

        if a < b:
            operations.append("B+=A")
            b += a
        else:
            operations.append("A+=B")
            a += b

    print(len(operations))
    sys.stdout.write("\n".join(operations))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def valid(inp: str, out: str) -> bool:
    a, b = map(int, inp.split())
    lines = out.strip().splitlines()

    if not lines:
        return False

    n = int(lines[0])
    if n < 0 or n > 5000:
        return False
    if len(lines) != n + 1:
        return False

    allowed = {"A+=A", "A+=B", "B+=A", "B+=B"}

    for op in lines[1:]:
        if op not in allowed:
            return False

        if op == "A+=A":
            a += a
        elif op == "A+=B":
            a += b
        elif op == "B+=A":
            b += a
        else:
            b += b

    return a == b

# provided sample
sample = "2 3"
assert valid(sample, run(sample)), "sample 1"

# minimum-size input
case = "1 1"
assert valid(case, run(case)), "already equal"

# both values are powers of two
case = "1 1024"
assert valid(case, run(case)), "power-of-two ratio"

# odd values requiring several add-and-normalize rounds
case = "1 5"
assert valid(case, run(case)), "odd values"

# maximum input boundary
case = "1000000000000000000 999999999999999999"
assert valid(case, run(case)), "maximum-size values"

# asymmetric values with many factors of two
case = "576460752303423488 1"
assert valid(case, run(case)), "large power of two"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3`| Bất kỳ chuỗi hợp lệ nào | Cung cấp mẫu và thi công cơ bản | 
|`1 1`|`0`hoạt động | Ranh giới đã bằng nhau | 
|`1 1024`| Bất kỳ chuỗi hợp lệ nào | Giảm một nửa khái niệm lặp đi lặp lại | 
|`1 5`| Bất kỳ chuỗi hợp lệ nào | Phép cộng lẻ theo sau là chuẩn hóa | 
|`1000000000000000000 999999999999999999`| Bất kỳ chuỗi hợp lệ nào | Cường độ đầu vào tối đa | 
|`576460752303423488 1`| Bất kỳ chuỗi hợp lệ nào | Nhiều quyền hạn của hai và xử lý số lượng hoạt động | 

## Vỏ cạnh 

cho`5 5`, điều kiện bên ngoài`while a != b`là sai ngay lập tức. Danh sách thao tác vẫn trống nên chương trình sẽ in`0`. Đây là loại hành vi đúng duy nhất cần thiết ở đây vì trạng thái ban đầu đã thỏa mãn mục tiêu. 

Vì`2 3`, sự chuẩn hóa đầu tiên thấy rằng`A`là chẵn. Chương trình ghi lại`B+=B`và thay đổi trạng thái khái niệm thành`(1,3)`. Vì cả hai giá trị bây giờ đều là số lẻ và không bằng nhau nên nó ghi lại`B+=A`, cho`(1,4)`. Việc chuẩn hóa tiếp theo sẽ chia`B`hai lần, cho`(1,1)`. Trình tự thực tế là`B+=B`,`B+=A`,`A+=A`,`A+=A`và trạng thái thực phát triển từ`(2,3)`ĐẾN`(2,6)`, sau đó`(2,8)`, sau đó`(4,8)`, và cuối cùng`(8,8)`. 

Vì`1 5`, cả hai giá trị đều bắt đầu lẻ. Từ`1 < 5`, chương trình thực hiện`B+=A`, thu được trạng thái khái niệm`(1,6)`. Giá trị chẵn bây giờ có thể giảm đi một nửa, tạo ra`(1,3)`. Sự khác biệt đã giảm từ`4`ĐẾN`2`. Quá trình tương tự thay đổi`(1,3)`ĐẾN`(1,1)`. Điều này chứng tỏ tại sao một cặp lẻ không nên tiếp tục cộng giá trị nhỏ hơn mà không chuẩn hóa. 

Vì`1000000000000000000 999999999999999999`, các con số đều ở cường độ tối đa cho phép và khác nhau một. Cả hai đều là số lẻ nên thuật toán cộng giá trị nhỏ hơn với giá trị lớn hơn, làm cho giá trị lớn hơn trở thành chẵn. Sau đó nó liên tục loại bỏ các yếu tố của hai về mặt khái niệm. Mỗi vòng như vậy sẽ giảm mạnh sự khác biệt và các giá trị chuẩn hóa không bao giờ tăng vượt quá thang đo ban đầu. Do đó, kích thước đầu vào lớn được xử lý mà không có bất kỳ sự tràn hoặc tìm kiếm nào trên các giá trị mục tiêu lớn. 

Vì`576460752303423488 1`, số đầu tiên là (2^{59}). Thuật toán có thể loại bỏ lũy thừa của hai lũy thừa cùng một lúc bằng cách sử dụng`B+=B`, giảm bớt khái niệm`A`từ (2^{59}) đến`1`. Hai giá trị sau đó trở nên bằng nhau. Điều này thực hiện chuỗi chuẩn hóa đơn giản dài nhất và xác nhận rằng danh sách thao tác vẫn thấp hơn nhiều so với giới hạn 5000 thao tác.
