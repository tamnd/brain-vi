---
title: "CF 102191C - Sắp xếp chỗ ngồi"
description: "Có n học sinh ngồi quanh một vòng tròn. Hoán vị đầu vào đưa ra thứ tự vòng tròn hiện tại của chúng, do đó, ngoài mỗi cặp liên tiếp trong mảng, học sinh cuối cùng và đầu tiên cũng là hàng xóm của nhau."
date: "2026-08-20T01:03:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102191
codeforces_index: "C"
codeforces_contest_name: "PSUT Coding Marathon 2019"
rating: 0
weight: 102191
solve_time_s: 538
verified: false
draft: false
---

[CF 102191C - Sắp xếp chỗ ngồi](https://codeforces.com/problemset/problem/102191/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 58 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`học sinh ngồi quanh vòng tròn. Hoán vị đầu vào đưa ra thứ tự vòng tròn hiện tại của chúng, do đó, ngoài mỗi cặp liên tiếp trong mảng, học sinh cuối cùng và đầu tiên cũng là hàng xóm của nhau. 

Chúng ta cần tạo ra một thứ tự vòng tròn khác gồm chính xác những học sinh đó sao cho mọi cặp lân cận mới đều không lân cận theo cách sắp xếp cũ. Vì học sinh được biểu diễn bằng một hoán vị của`1..n`, chúng ta chỉ cần thay đổi thứ tự của các phần tử mảng hiện có. 

Khó khăn chính là ranh giới hình tròn. Nếu lệnh mới được`b[0], b[1], ..., b[n-1]`, chúng ta phải kiểm tra từng cặp`b[i], b[i+1]`và cả`b[n-1], b[0]`. Cấu trúc hoạt động cho phần tuyến tính nhưng vô tình đặt hàng xóm đầu tiên và hàng xóm cuối cùng cũ lại với nhau là không hợp lệ. 

Sự ràng buộc`n <= 3 * 10^5`loại trừ bất cứ điều gì thử nhiều hoán vị hoặc thực hiện các tìm kiếm đắt tiền lặp đi lặp lại. MỘT`O(n^2)`phương pháp đã yêu cầu về`9 * 10^10`kiểm tra cặp cơ bản ở kích thước tối đa, vượt xa giới hạn một giây. Chúng ta cần một cấu trúc chỉ xử lý mỗi học sinh một số lần không đổi, đưa ra`O(n)`thời gian. 

Có ba trường hợp đặc biệt nhỏ mà một mô hình ngây thơ có thể xử lý sai. Vì`n = 3`, ví dụ, đầu vào`1 2 3`có mọi cặp liền kề trong vòng tròn ban đầu, do đó không thể có vòng tròn mới và kết quả đúng là`-1`. Vì`n = 4`, nhập`1 2 3 4`cũng không thể: mỗi học sinh chỉ có một khả năng không phải là lân cận, đó là học sinh đối diện với các em, nên đồ thị cho phép gồm hai cạnh rời nhau và không thể tạo thành một đường tròn. Đầu ra đúng lại là`-1`. 

Một lỗi dễ mắc phải khác là quên cặp đóng vòng tròn. Vì`n = 5`với đầu vào`1 2 3 4 5`, sự sắp xếp`1 3 5 2 4`là hợp lệ. Các sai phân liên tiếp của nó ở các vị trí ban đầu đều là hai modulo năm, kể cả cặp cuối cùng`4,1`. Cấu trúc chỉ kiểm tra các phần tử liền kề trong mảng được in có thể bỏ sót điều kiện cuối cùng này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thử các hoán vị của học sinh cho đến khi thỏa mãn điều kiện. Mọi hoán vị đều có thể được kiểm tra`O(n)`thời gian vì có chính xác`n`cặp hàng xóm tròn để kiểm tra. có`n!`hoán vị, vì vậy việc tìm kiếm toàn diện cần`O(n * n!)`hoạt động trong trường hợp xấu nhất. Ở mức hạn chế tối đa, điều này có nghĩa là đại khái`300000 * 300000!`kiểm tra cặp, điều này không khả thi từ xa. 

Quan sát hữu ích là ID sinh viên thực tế không quan trọng. Điều quan trọng là vị trí của mỗi học sinh trong vòng tròn cũ. Hai học sinh bị cấm chính xác khi vị trí cũ của họ khác nhau một khoảng`1`modulo`n`. Vì vậy, trước tiên chúng ta có thể xây dựng một hoán vị của các vị trí cũ rồi sử dụng các vị trí đó để lấy ID sinh viên. 

Một cách rất tự nhiên để tách những người hàng xóm bị cấm là chiếm tất cả các vị trí chẵn trước và tất cả các vị trí lẻ sau đó. Trong mỗi nhóm, các vị trí liên tiếp cách nhau hai nên chúng tự động được an toàn. Đối với số lẻ`n`, quá trình chuyển đổi giữa hai nhóm và quá trình chuyển đổi cuối cùng trở lại vị trí 0 cũng có sự khác biệt hai modulo`n`, nên công trình được thi công ngay. 

Thậm chí`n`, cấu trúc tương tự có một cạnh tròn có vấn đề. Hoán đổi hai vị trí lẻ cuối cùng sẽ khắc phục chính xác vấn đề đó trong khi vẫn duy trì sự khác biệt an toàn ở mọi nơi khác. Điều này đưa ra quyết định theo thời gian không đổi và xây dựng tuyến tính. 

Việc xây dựng cũng giải thích ngay tại sao`n < 5`là không thể. Vì`n = 3`Và`n = 4`, phần bù của chu trình ban đầu không chứa chu trình Hamilton. Bắt đầu từ`n = 5`, việc xây dựng tính chẵn lẻ cho một cách rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n * n!)`|`O(n)`| Quá chậm | 
| Xây dựng tối ưu |`O(n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cách sắp xếp hình tròn cũ thành`a`. Chúng tôi làm việc với các vị trí dựa trên số 0 vì việc xây dựng dựa trên tính chẵn lẻ của vị trí. 
2. Nếu`n < 5`, in`-1`. Đối với ba học sinh, mỗi cặp đã liền kề nhau và đối với bốn học sinh, các cạnh duy nhất được phép kết nối các học sinh đối diện nhau, không thể tạo thành một chu trình chứa tất cả mọi người. 
3. Xây dựng một chuỗi mới bằng cách đảm nhận các vị trí`0, 2, 4, ...`đầu tiên, tiếp theo là vị trí`1, 3, 5, ...`. Chúng ta đang cố tình tách các vị trí có cùng tính chẵn lẻ vì khoảng cách vòng tròn ban đầu của chúng là hai chứ không phải một. 
4. Nếu`n`là số lẻ thì giữ nguyên dãy đó. Phần vị trí chẵn có hiệu là hai, phần vị trí lẻ cũng có hiệu là hai, phần chuyển từ vị trí chẵn cuối cùng sang vị trí lẻ đầu tiên có khoảng cách tròn là hai, và vị trí lẻ cuối cùng trở về vị trí 0 cũng có khoảng cách tròn là hai. 
5. Nếu`n`chẵn, hoán đổi hai phần tử cuối cùng của chuỗi được xây dựng. Trước khi hoán đổi, cạnh nguy hiểm duy nhất là kết nối vòng tròn từ vị trí lẻ cuối cùng`n - 1`trở lại vị trí số 0. Sau khi hoán đổi, hai vị trí cuối cùng trở thành`n - 1, n - 3`, và ranh giới mới là từ`n - 3`về 0, khoảng cách của nó là ba. Cạnh bị ảnh hưởng khác có khoảng cách hai, vì vậy cả hai đều hợp lệ. 
6. Chuyển đổi các vị trí đã xây dựng trở lại ID sinh viên bằng cách sử dụng các mục nhập tương ứng của`a`, và in chúng theo thứ tự vòng tròn kết quả. 

### Tại sao nó hoạt động 

Điều bất biến là mọi cặp vị trí liên tiếp theo thứ tự được xây dựng đều có khoảng cách hình tròn ban đầu khác với một. Đối với số lẻ`n`, tất cả các khoảng cách như vậy là hai. Thậm chí`n`, các chuyển đổi chẵn lẻ giống nhau thông thường có khoảng cách hai, quá trình chuyển đổi giữa hai nhóm chẵn lẻ có khoảng cách ba, cặp hoán đổi có khoảng cách hai và quá trình chuyển đổi cuối cùng trở về 0 có khoảng cách ba. Vì một cặp lân cận cũ được đặc trưng chính xác bằng khoảng cách tròn 1, nên không có cặp mới nào trước đây là hàng xóm của nhau. Mỗi vị trí ban đầu xuất hiện đúng một lần nên kết quả cũng là một hoán vị của tất cả học sinh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    if n < 5:
        print(-1)
        return

    ans = []

    for i in range(0, n, 2):
        ans.append(a[i])

    for i in range(1, n, 2):
        ans.append(a[i])

    if n % 2 == 0:
        ans[-1], ans[-2] = ans[-2], ans[-1]

    print(*ans)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên thu thập mọi vị trí dựa trên số 0. Vòng lặp thứ hai thu thập mọi vị trí lẻ. Chúng cùng nhau chứa mỗi học sinh chính xác một lần, do đó không cần mảng truy cập riêng biệt. 

Đối với số lẻ`n`, trình tự đã đúng rồi. Thậm chí`n`,`ans[-1]`Và`ans[-2]`là hai học sinh ở vị trí lẻ cuối cùng nên việc hoán đổi chúng chính là sự điều chỉnh được mô tả trong công thức. 

Không có phép tính nào liên quan đến các giá trị lớn hơn`n`và số nguyên Python không có vấn đề tràn. Ranh giới quan trọng được thể hiện ngầm bằng cách xây dựng, vì vậy chúng tôi không cần một bước xác thực riêng. Đặc biệt, việc hoán đổi phải xảy ra sau khi cả hai nhóm chẵn lẻ đã được thêm vào, vì nó làm thay đổi phần cuối của chuỗi vòng tròn. 

Giải pháp không cần phân biệt mã sinh viên với chức vụ. Từ`a`là một hoán vị, mọi thứ tự vị trí hợp lệ sẽ ngay lập tức trở thành thứ tự hợp lệ của sinh viên. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là:```
8
6 1 3 5 7 8 4 2
```Việc xây dựng sử dụng các vị trí dựa trên số không. Từ`n`là chẵn, trước tiên chúng ta lấy tất cả các vị trí chẵn, sau đó là tất cả các vị trí lẻ và cuối cùng hoán đổi hai phần tử cuối cùng. 

| Bước | Vị trí chẵn | Vị trí lẻ | Trình tự hiện tại | 
| --- | --- | --- | --- | 
| Bắt đầu |`[]`|`[]`|`[]`| 
| Thêm sự kiện |`6 3 7 4`|`[]`|`6 3 7 4`| 
| Thêm tỷ lệ cược |`6 3 7 4`|`1 5 8 2`|`6 3 7 4 1 5 8 2`| 
| Trao đổi cuối cùng hai |`6 3 7 4`|`1 5 2 8`|`6 3 7 4 1 5 2 8`| 

Sự sắp xếp cuối cùng là`6 3 7 4 1 5 2 8`. Vị trí cũ của nó là`0, 2, 4, 6, 1, 3, 7, 5`và mọi hiệu tròn giữa các vị trí liên tiếp đều không`1`cũng không`7`. Do đó không có cặp lân cận mới nào là cặp lân cận cũ. Mẫu chính thức có cách sắp xếp hợp lệ khác, điều này được cho phép vì bài toán chấp nhận bất kỳ giải pháp nào. 

Đối với Mẫu 2, đầu vào là:```
3
1 3 2
```Thuật toán dừng trước khi xây dựng bất cứ thứ gì vì có ít hơn năm học sinh. 

| Bước |`n`| Quyết định | Đầu ra | 
| --- | --- | --- | --- | 
| Đọc đầu vào |`3`|`n < 5`|`-1`| 
| Kết thúc |`3`| Không thể xây dựng |`-1`| 

Với ba học sinh, vòng tròn ban đầu đã làm cho mọi cặp đều liền kề nhau. Không có sẵn cặp nào cho bất kỳ cạnh tròn mới nào, vì vậy việc từ chối trường hợp này là đúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Mỗi vị trí đầu vào được thêm một lần và trường hợp chẵn thực hiện một lần hoán đổi. | 
| Không gian |`O(n)`| Mảng đầu vào và mảng đầu ra đều chứa`n`ID sinh viên. | 

Tại`n = 3 * 10^5`, thuật toán chỉ thực hiện một lượng công việc không đổi cho mỗi học sinh và lưu trữ một vài mảng số nguyên có kích thước tuyến tính. Điều này thoải mái phù hợp với giới hạn 1 giây và 256 MB, trong khi các phương pháp tiếp cận giai thừa hoặc bậc hai không thể xử lý đầu vào tối đa. 

## Trường hợp thử nghiệm 

Đầu ra không phải là duy nhất, vì vậy các thử nghiệm phải xác minh rằng sự sắp xếp được tạo ra là một hoán vị và mọi cặp lân cận hình tròn mới không phải là cặp lân cận hình tròn cũ. So sánh đầu ra với một chuỗi hợp lệ cố định sẽ từ chối các giải pháp đúng khác một cách không chính xác. 

Vấn đề đảm bảo rằng đầu vào là một hoán vị. Do đó, một thử nghiệm "tất cả các giá trị bằng nhau" chẳng hạn như`5 / 1 1 1 1 1`không phải là trường hợp thử nghiệm Codeforces hợp lệ và không nên được sử dụng để kiểm tra chính giải pháp đã gửi. Nó chỉ có thể kiểm tra hành vi trên đầu vào không đúng định dạng, nằm ngoài hợp đồng có vấn đề.```python
import io
import sys

def solve_case(inp: str) -> str:
    data = list(map(int, inp.split()))
    n = data[0]
    a = data[1:]

    if n < 5:
        return "-1"

    ans = []

    for i in range(0, n, 2):
        ans.append(a[i])

    for i in range(1, n, 2):
        ans.append(a[i])

    if n % 2 == 0:
        ans[-1], ans[-2] = ans[-2], ans[-1]

    return " ".join(map(str, ans))

def valid(inp: str, out: str) -> bool:
    data = list(map(int, inp.split()))
    n = data[0]
    a = data[1:]

    result = out.split()

    if n < 5:
        return result == ["-1"]

    if len(result) != n:
        return False

    result = list(map(int, result))

    if sorted(result) != sorted(a):
        return False

    old_pos = {x: i for i, x in enumerate(a)}

    for i in range(n):
        x = old_pos[result[i]]
        y = old_pos[result[(i + 1) % n]]
        diff = (x - y) % n

        if diff == 1 or diff == n - 1:
            return False

    return True

# Provided sample 1
sample1 = """8
6 1 3 5 7 8 4 2
"""
out = solve_case(sample1)
assert valid(sample1, out), "sample 1"

# Provided sample 2
sample2 = """3
1 3 2
"""
assert solve_case(sample2) == "-1", "sample 2"

# Minimum possible n, impossible.
case3 = """4
1 2 3 4
"""
assert solve_case(case3) == "-1", "n=4 must be impossible"

# Smallest possible solvable case.
case4 = """5
1 2 3 4 5
"""
out = solve_case(case4)
assert valid(case4, out), "n=5 construction"

# Even n, catches the special final swap.
case5 = """6
1 2 3 4 5 6
"""
out = solve_case(case5)
assert valid(case5, out), "even n boundary"

# Large valid input, exercising the O(n) construction.
n = 300000
a = list(range(1, n + 1))
case6 = str(n) + "\n" + " ".join(map(str, a)) + "\n"
out = solve_case(case6)
assert valid(case6, out), "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 1 3 2`|`-1`| Kích thước tối thiểu và hoàn toàn không thể | 
|`4 / 1 2 3 4`|`-1`| Trường hợp chẵn không thể rõ ràng hơn | 
|`5 / 1 2 3 4 5`| Bất kỳ hoán vị hợp lệ nào | Trường hợp nhỏ nhất có thể giải được và ranh giới hình tròn | 
|`6 / 1 2 3 4 5 6`| Bất kỳ hoán vị hợp lệ nào | Xây dựng kích thước đồng đều và hoán đổi cuối cùng | 
|`300000 / 1 2 ... 300000`| Bất kỳ hoán vị hợp lệ nào | Hạn chế tối đa và hiệu suất tuyến tính | 

## Vỏ cạnh 

cho`n = 3`, coi như:```
3
1 3 2
```Mỗi cặp học sinh đứng cạnh nhau trong vòng tròn ban đầu. Học sinh`1`ở bên cạnh`3`Và`2`, học sinh`3`ở bên cạnh`1`Và`2`, và sinh viên`2`ở bên cạnh`3`Và`1`. Một vòng tròn mới sẽ cần ba cạnh được phép, nhưng không có cạnh nào cả. Thuật toán phát hiện`n < 5`ngay lập tức và in`-1`. 

Vì`n = 4`, coi như:```
4
1 2 3 4
```Người không phải là hàng xóm duy nhất của`1`là`3`, người không phải là hàng xóm duy nhất của`2`là`4`, và ngược lại. Do đó đồ thị được phép chỉ bao gồm`1-3`Và`2-4`. Nó không thể chứa một chu trình bốn đỉnh. Sớm như vậy`n < 5`kiểm tra bản in chính xác`-1`. 

Đối với trường hợp nhỏ nhất có thể giải được, hãy xem xét:```
5
1 2 3 4 5
```Phần vị trí chẵn là`1 3 5`, và phần vị trí lẻ là`2 4`, cho:```
1 3 5 2 4
```Các vị trí ban đầu là`0, 2, 4, 1, 3`. Sự khác biệt vòng tròn là`2, 2, 2, 2, 2`, do đó mỗi cặp mới cách nhau hai vị trí trong vòng tròn cũ. Đặc biệt, cặp cuối cùng`4,1`là an toàn, thực hiện ranh giới hình tròn. 

Thậm chí`n`, việc điều chỉnh đặc biệt là cần thiết. Với:```
6
1 2 3 4 5 6
```nhóm chẵn lẻ ban đầu cho:```
1 3 5 2 4 6
```trận chung kết`6,1`cặp đôi bị cấm vì ban đầu những sinh viên đó là hàng xóm. Sau khi hoán đổi hai phần tử cuối cùng, chúng ta nhận được:```
1 3 5 2 6 4
```Các vị trí cũ là`0,2,4,1,5,3`. Sự khác biệt vòng tròn là`2,2,3,4,2,3`, không cái nào trong số đó là`1`hoặc`5`. Việc hoán đổi sẽ thay đổi chính xác phần công trình mà lẽ ra sẽ thất bại. 

Đối với kích thước đầu vào tối đa, cấu trúc tương tự không phụ thuộc vào giá trị của ID sinh viên mà chỉ phụ thuộc vào vị trí của chúng. Với`n = 300000`, cả hai vòng lặp chẵn lẻ cùng nhau xử lý chính xác`300000`vị trí, theo sau là một lần hoán đổi. Không có vòng lặp lồng nhau và không có xác nhận lặp lại, do đó thời gian chạy vẫn tuyến tính ngay cả ở đầu vào lớn nhất được phép.
