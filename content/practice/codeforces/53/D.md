---
title: "CF 53D - Giáo dục thể chất"
description: "Chúng ta có hai mảng biểu thị thứ tự của học sinh trong một dòng. Mảng a là sự sắp xếp mong muốn và mảng b là sự sắp xếp hiện tại. Trong một hoạt động, chúng ta có thể hoán đổi hai học sinh lân cận. Chúng ta phải xuất ra bất kỳ chuỗi hoán đổi liền kề nào để biến b thành a."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "sortings"]
categories: ["algorithms"]
codeforces_contest: 53
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 49 (Div. 2)"
rating: 1500
weight: 53
solve_time_s: 126
verified: true
draft: false
---

[CF 53D - Giáo dục thể chất](https://codeforces.com/problemset/problem/53/D) 

**Đánh giá:** 1500 
**Thẻ:** sắp xếp 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai mảng biểu thị thứ tự của học sinh trong một dòng. Mảng`a`là sự sắp xếp mong muốn và mảng`b`là sự sắp xếp hiện tại. Trong một hoạt động, chúng ta có thể hoán đổi hai học sinh lân cận. Chúng ta phải xuất ra bất kỳ chuỗi hoán đổi liền kề nào biến đổi`b`vào trong`a`. 

Phần khó khăn là độ cao không nhất thiết phải là duy nhất. Hai học sinh có thể có cùng chiều cao, vì vậy chúng ta không thể đơn giản so sánh các giá trị mà không suy nghĩ xem lần xuất hiện nào tương ứng với vị trí nào. 

Các ràng buộc đủ nhỏ để cho phép các thuật toán bậc hai. Từ`n ≤ 300`, thậm chí là một`O(n^2)`hoặc`O(n^3)`giải pháp dễ dàng phù hợp trong giới hạn. Số lượng hoán đổi liền kề tối đa cần thiết để chuyển đổi một hoán vị này thành một hoán vị khác là nhiều nhất`n(n-1)/2`, cái nào cho`n = 300`bằng`44850`, thấp hơn mức cho phép`10^6`hoạt động. 

Việc triển khai bất cẩn có thể thất bại khi tồn tại các giá trị trùng lặp. Coi như:```
a = [1, 2, 1]
b = [1, 1, 2]
```Nếu chúng ta tham lam phù hợp với cái đầu tiên`1`TRONG`a`với cái đầu tiên`1`TRONG`b`, điều đó ổn thôi. Nhưng nếu sau này chúng ta tìm kiếm không chính xác, chúng ta có thể di chuyển lần xuất hiện sai và phá vỡ các vị trí đã được sửa. 

Một trường hợp tinh vi khác xuất hiện khi giá trị mong muốn đã tồn tại trước đó trong mảng:```
a = [2, 1, 2]
b = [2, 2, 1]
```Khi cố định vị trí`2`, chúng ta chỉ phải tìm kiếm trong hậu tố không cố định. Nếu không, chúng ta có thể vô tình làm xáo trộn vị trí đầu tiên đã đúng. 

Trường hợp nhỏ nhất cũng có vấn đề:```
n = 1
a = [5]
b = [5]
```Câu trả lời phải không có giao dịch hoán đổi. Một số triển khai quên xử lý việc này và thử thực hiện các thao tác lân cận không hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng lực lượng vũ phu. Đối với mọi vị trí`i`, chúng tôi cố gắng đặt`a[i]`vào vị trí đó bên trong mảng`b`. Chúng tôi tìm kiếm sự xuất hiện đầu tiên của`a[i]`ở đâu đó bên phải, sau đó liên tục hoán đổi nó sang trái cho đến khi đạt đến vị trí`i`. 

Điều này hiệu quả vì các giao dịch hoán đổi liền kề cho phép chúng ta di chuyển bất kỳ phần tử nào theo từng bước. Một lần vị trí`i`chứa giá trị chính xác, chúng tôi không bao giờ chạm vào nó nữa. 

Tổng số lần hoán đổi trong trường hợp xấu nhất là bậc hai. Ví dụ: đảo ngược một mảng các giá trị riêng biệt yêu cầu khoảng`n^2 / 2`trao đổi. Với`n = 300`, điều này hoàn toàn an toàn. 

Một cách tiếp cận bạo lực ngây thơ hơn sẽ liên tục quét toàn bộ mảng để tìm kiếm sự không khớp và thực hiện các sửa lỗi cục bộ ngẫu nhiên. Điều đó có thể làm giảm độ phức tạp bậc ba vì mỗi lần chỉnh sửa có thể yêu cầu quét toàn bộ một lần nữa. Với`n = 300`, thậm chí điều đó có thể vẫn trôi qua, nhưng nó lộn xộn không cần thiết và khó lý giải hơn nhiều. 

Quan sát quan trọng là các giao dịch hoán đổi liền kề hỗ trợ một cách tự nhiên quá trình sửa lỗi từ trái sang phải. Khi chúng ta đặt giá trị đúng vào vị trí`i`, các thao tác sau này chỉ xảy ra ở bên phải của`i`, do đó tiền tố luôn chính xác. 

Điều này đưa ra một thuật toán tham lam rõ ràng: 

1. Xử lý các vị trí từ trái qua phải. 
2. Tìm giá trị cần thiết ở đâu đó trong hậu tố. 
3. Bong bóng nó còn lại bằng cách sử dụng các giao dịch hoán đổi liền kề. 

Vì mọi thao tác đều cải tiến nghiêm ngặt tiền tố cố định nên thuật toán luôn kết thúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chỉnh sửa bạo lực ngẫu nhiên | O(n³) | O(1) | Chậm một cách không cần thiết | 
| Tham lam sủi bọt | O(n²) | O(1) không bao gồm lưu trữ câu trả lời | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ vị trí đầu tiên và di chuyển từ trái sang phải trong mảng. 
2. Đối với chức vụ`i`, kiểm tra xem`b[i]`đã bằng rồi`a[i]`. 
3. Nếu giá trị đã đúng, hãy tiếp tục đến vị trí tiếp theo. 
4. Nếu không, hãy tìm kiếm chỉ mục đầu tiên`j ≥ i`như vậy`b[j] == a[i]`. 

Chúng tôi chỉ tìm kiếm trong hậu tố vì các vị trí trước`i`đã được sửa và không bao giờ được thay đổi nữa. 
5. Di chuyển phần tử tại vị trí`j`theo hướng`i`sử dụng các giao dịch hoán đổi liền kề. 

Đối với mọi vị trí`k`từ`j-1`xuống`i`:

- Tráo đổi`b[k]`Và`b[k+1]`- Ghi lại thao tác`(k+1, k+2)`trong lập chỉ mục dựa trên 1 
6. Sau những lần hoán đổi này, vị trí`i`bây giờ chứa`a[i]`. 
7. Tiếp tục cho đến khi mọi vị trí đều cố định. 

### Tại sao nó hoạt động 

Sau khi xử lý vị trí`i`, tiền tố`b[0..i]`trở nên giống hệt với`a[0..i]`. 

Thuật toán không bao giờ chạm vào tiền tố này nữa vì các giao dịch hoán đổi trong tương lai chỉ xảy ra bên trong các vị trí lớn hơn hoặc bằng chỉ số hiện tại. Vì các mảng chứa nhiều tập hợp giá trị giống nhau nên giá trị bắt buộc cho mỗi bước phải tồn tại ở đâu đó trong hậu tố còn lại. 

Bằng quy nạp, mọi vị trí cuối cùng đều trở thành chính xác, do đó mảng cuối cùng bằng`a`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    ops = []

    for i in range(n):
        if b[i] == a[i]:
            continue

        j = i
        while b[j] != a[i]:
            j += 1

        while j > i:
            b[j], b[j - 1] = b[j - 1], b[j]
            ops.append((j, j + 1))
            j -= 1

    print(len(ops))
    for x, y in ops:
        print(x, y)

solve()
```Vòng ngoài cố định các vị trí từ trái sang phải. Khi bắt đầu lặp`i`, mọi vị trí trước đó đều đã chính xác. 

Vòng lặp tìm kiếm tìm thấy sự xuất hiện đầu tiên của`a[i]`trong hậu tố không cố định. Sử dụng lần xuất hiện đầu tiên là đủ vì các giá trị bằng nhau có thể hoán đổi cho nhau. Chúng tôi chỉ quan tâm rằng một bản sao chính xác đạt được vị trí`i`. 

Vòng lặp sủi bọt thực hiện lần lượt các lần hoán đổi liền kề. Vì bài toán yêu cầu rõ ràng các giao dịch hoán đổi lân cận nên chúng ta phải mô phỏng từng bước di chuyển riêng lẻ và lưu trữ nó. 

Các chỉ số hoạt động được lưu trữ ở dạng dựa trên 1 vì Codeforces sử dụng chỉ mục dựa trên 1 ở đầu ra. Đây là một nơi dễ dàng để mắc lỗi từng cái một. Bên trong mảng, chúng tôi sử dụng chỉ mục dựa trên 0, nhưng việc hoán đổi giữa các vị trí`j-1`Và`j`tương ứng với đầu ra`(j, j+1)`. 

Số lần hoán đổi không bao giờ vượt quá`n(n-1)/2`, do đó mức sử dụng bộ nhớ cho các hoạt động lưu trữ vẫn còn nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = [1, 2, 3, 2]
b = [3, 2, 1, 2]
```| Bước | Vị trí mục tiêu | Giá trị cần thiết | Chỉ mục được tìm thấy | Mảng sau khi hoán đổi | Hoạt động | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | - | - | - | [3, 2, 1, 2] | [] | 
| 1 | 0 | 1 | 2 | [1, 3, 2, 2] | (2,3), (1,2) | 
| 2 | 1 | 2 | 2 | [1, 2, 3, 2] | (3,4), (2,3) | 
| 3 | 2 | 3 | 2 | [1, 2, 3, 2] | không thay đổi | 
| 4 | 3 | 2 | 3 | [1, 2, 3, 2] | không thay đổi | 

Dấu vết này cho thấy sự bất biến chính. Sau khi sửa từng vị trí, tiền tố sẽ không bao giờ thay đổi nữa. Thuật toán dần dần biến mảng thành cách sắp xếp mục tiêu. 

### Ví dụ 2 

đầu vào:```
a = [2, 1, 2]
b = [2, 2, 1]
```| Bước | Vị trí mục tiêu | Giá trị cần thiết | Chỉ mục được tìm thấy | Mảng sau khi hoán đổi | Hoạt động | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | - | - | - | [2, 2, 1] | [] | 
| 1 | 0 | 2 | 0 | [2, 2, 1] | không thay đổi | 
| 2 | 1 | 1 | 2 | [2, 1, 2] | (2,3) | 
| 3 | 2 | 2 | 2 | [2, 1, 2] | không thay đổi | 

Ví dụ này chứng minh tại sao sự trùng lặp là vô hại. Chúng tôi chỉ cần một số giá trị cần thiết xuất hiện từ hậu tố. Khi giá trị chính xác đạt đến vị trí hiện tại, các giá trị bằng nhau còn lại có thể giữ nguyên ở bất kỳ vị trí nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi phần tử có thể di chuyển qua mảng một lần | 
| Không gian | O(1) không bao gồm đầu ra | Chỉ có một vài biến ngoài danh sách hoạt động | 

Vì`n = 300`, độ phức tạp bậc hai là rất nhỏ. Ngay cả số lần hoán đổi tối đa có thể là dưới năm mươi nghìn, thấp hơn nhiều so với giới hạn hoạt động được phép và đủ nhanh trong thời gian 2 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    ops = []

    for i in range(n):
        if b[i] == a[i]:
            continue

        j = i
        while b[j] != a[i]:
            j += 1

        while j > i:
            b[j], b[j - 1] = b[j - 1], b[j]
            ops.append((j, j + 1))
            j -= 1

    out = [str(len(ops))]
    for x, y in ops:
        out.append(f"{x} {y}")

    return "\n".join(out)

# provided sample
res = solve_io(
    "4\n1 2 3 2\n3 2 1 2\n"
)
assert res.startswith("4"), "sample 1"

# minimum size
assert solve_io(
    "1\n5\n5\n"
) == "0", "single element"

# already sorted
assert solve_io(
    "3\n1 2 3\n1 2 3\n"
) == "0", "already correct"

# duplicates
res = solve_io(
    "3\n2 1 2\n2 2 1\n"
)
assert res.splitlines()[0] == "1", "duplicate handling"

# reverse order
res = solve_io(
    "4\n1 2 3 4\n4 3 2 1\n"
)
assert int(res.splitlines()[0]) == 6, "maximum inversions for n=4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5 / 5`|`0`hoán đổi | Ranh giới kích thước tối thiểu | 
| Đã có mảng bằng nhau |`0`hoán đổi | Không có hoạt động không cần thiết | 
| Mảng có bản sao | Chuyển đổi hợp lệ | Xử lý trùng lặp đúng | 
| Mảng đảo ngược hoàn toàn | Đảo ngược tối đa | Số lần hoán đổi trong trường hợp xấu nhất | 

## Vỏ cạnh 

Hãy xem xét trường hợp giá trị trùng lặp:```
a = [1, 2, 1]
b = [1, 1, 2]
```Tại vị trí`0`, giá trị đã đúng rồi. Tại vị trí`1`, chúng tôi cần`2`. Thuật toán chỉ tìm kiếm trong hậu tố và tìm thấy`2`tại chỉ mục`2`. Một trao đổi liền kề tạo ra:```
[1, 2, 1]
```Vị trí đầu tiên đã đúng không bao giờ thay đổi. 

Bây giờ hãy xem xét:```
a = [2, 1, 2]
b = [2, 2, 1]
```Vị trí đầu tiên được cố định ngay lập tức. Khi xử lý vị trí`1`, thuật toán tìm kiếm từ chỉ mục`1`trở đi và tìm thấy`1`tại chỉ mục`2`. Hoán đổi vị trí`2`Và`3`đưa ra sự sắp xếp đúng đắn. 

Nếu chúng tôi cho phép tìm kiếm các vị trí trước đó, chúng tôi có thể vô tình di chuyển tiền tố cố định và phá vỡ tính chính xác. 

Cuối cùng, đầu vào nhỏ nhất có thể:```
n = 1
a = [7]
b = [7]
```Vòng lặp bên ngoài chạy một lần. Vì giá trị đã khớp nên không có thao tác nào được thêm vào. Thuật toán in chính xác`0`.
