---
title: "CF 253B - Vật Lý Thực Hành"
description: "Chúng ta được cung cấp một danh sách các kết quả đo từ một thí nghiệm vật lý. Vasya muốn giữ lại càng nhiều số đo càng tốt, nhưng tập còn lại phải thỏa mãn một điều kiện: giá trị còn lại lớn nhất không được lớn hơn hai lần giá trị còn lại nhỏ nhất."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 253
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 154 (Div. 2)"
rating: 1400
weight: 253
solve_time_s: 194
verified: true
draft: false
---

[CF 253B - Thực hành Vật lý](https://codeforces.com/problemset/problem/253/B) 

**Đánh giá:** 1400 
**Thẻ:** tìm kiếm nhị phân, dp, sắp xếp, hai con trỏ 
**Thời gian giải:** 3 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các kết quả đo từ một thí nghiệm vật lý. Vasya muốn giữ lại càng nhiều số đo càng tốt, nhưng tập còn lại phải thỏa mãn một điều kiện: giá trị còn lại lớn nhất không được lớn hơn hai lần giá trị còn lại nhỏ nhất. 

Nhiệm vụ là loại bỏ số phép đo tối thiểu sao cho các số còn lại thỏa mãn:$y \le 2x$Ở đâu`x`là giá trị còn lại tối thiểu và`y`là giá trị lớn nhất còn lại 

Một cách khác để suy nghĩ về vấn đề này là: trong số tất cả các tập hợp con có giá trị lớn nhất nhiều nhất là gấp đôi giá trị nhỏ nhất, hãy tìm tập hợp con lớn nhất có thể có. Khi chúng tôi biết số lượng phép đo tối đa mà chúng tôi có thể giữ lại, câu trả lời chỉ đơn giản là:`removed = n - kept`Kích thước đầu vào đạt`10^5`, điều này ngay lập tức loại bỏ các nghiệm bậc hai. Bất kỳ cách tiếp cận nào kiểm tra từng cặp ranh giới một cách độc lập và quét lại toàn bộ mảng sẽ thực hiện xung quanh`10^10`hoạt động trong trường hợp xấu nhất, vượt xa thời hạn. Chúng ta cần thứ gì đó xung quanh`O(n log n)`hoặc`O(n)`sau khi sắp xếp. 

Bản thân các giá trị này là nhỏ, nhiều nhất`5000`, Nhưng`n`đủ lớn để chúng ta vẫn nên tập trung vào các thuật toán có quy mô phù hợp với số lượng phần tử. 

Có một số trường hợp khó bỏ sót. 

Hãy xem xét một mảng trong đó tất cả các giá trị đều hợp lệ:```
5
2 2 3 4 4
```Câu trả lời đúng là`0`vì giá trị lớn nhất là`4`và nhỏ nhất là`2`, Vì thế`4 ≤ 2 * 2`. 

Việc triển khai bất cẩn có thể luôn cố gắng thu nhỏ các cửa sổ một cách không cần thiết và loại bỏ các phần tử mặc dù không cần loại bỏ. 

Một trường hợp phức tạp khác là khi các bản sao xuất hiện xung quanh ranh giới:```
6
1 2 2 2 4 4
```Toàn bộ mảng không hợp lệ vì`4 > 2 * 1`. Nhưng sau khi loại bỏ chỉ`1`, mảng còn lại trở thành hợp lệ. Câu trả lời đúng là`1`. 

Việc triển khai hai con trỏ không chính xác có thể di chuyển con trỏ trái quá mạnh và bỏ lỡ cửa sổ tối ưu. 

Trường hợp quan trọng thứ ba là kích thước đầu vào tối thiểu:```
2
1 5000
```Các tập hợp con còn lại hợp lệ duy nhất có kích thước`1`, từ`5000 > 2`. Câu trả lời là`1`. 

Điều này nắm bắt các triển khai giả định phải có ít nhất hai yếu tố. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản sau khi mảng được sắp xếp. Giả sử chúng ta sửa giá trị nhỏ nhất còn lại tại chỉ mục`i`. Sau đó chúng ta có thể thử mọi chỉ số kết thúc có thể`j ≥ i`và kiểm tra xem:$a_j \le 2a_i$Nếu điều kiện đúng thì mảng con từ`i`ĐẾN`j`là hợp lệ vì việc sắp xếp đảm bảo mọi phần tử giữa chúng cũng nằm trong cùng một phạm vi. 

Điều này hiệu quả vì mọi giải pháp tối ưu luôn có thể được biểu diễn dưới dạng một phân đoạn liền kề theo thứ tự được sắp xếp. Nếu chúng ta giữ giá trị`3`Và`7`, chúng tôi sẽ không bao giờ loại trừ một giá trị`5`ngồi giữa chúng sau khi phân loại. 

Phương pháp brute-force kiểm tra tất cả`O(n^2)`cặp`(i, j)`. Với`n = 10^5`, điều này trở nên đại khái`10^10`kiểm tra, tốc độ này quá chậm. 

Quan sát quan trọng là sau khi mảng được sắp xếp, phạm vi hợp lệ cho điểm cuối bên trái cố định sẽ mở rộng một cách đơn điệu. Nếu một cửa sổ`[l, r]`hợp lệ thì tăng`r`cuối cùng có thể phá vỡ điều kiện, nhưng giảm`r`sẽ không bao giờ đau. 

Cấu trúc đơn điệu đó chính xác là mục đích của hai con trỏ. 

Chúng tôi duy trì một cửa sổ trượt`[l, r]`trên mảng đã được sắp xếp. Đối với mọi`r`, chúng tôi di chuyển`l`chỉ chuyển tiếp khi điều kiện bị vi phạm. Tại thời điểm đó, cửa sổ hiện tại trở thành phân đoạn hợp lệ lớn nhất kết thúc tại`r`. 

Vì mỗi con trỏ chỉ di chuyển về phía trước nên tổng công việc sau khi sắp xếp là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng đầu vào và sắp xếp nó. 

Việc sắp xếp biến bài toán thành việc tìm đoạn liền kề dài nhất trong đó giá trị lớn nhất tối đa gấp đôi giá trị nhỏ nhất. 
2. Khởi tạo hai con trỏ. 

Cho phép`l = 0`. Chúng tôi sẽ lặp lại`r`từ trái sang phải. 
3. Đối với mỗi`r`, mở rộng phía bên phải của cửa sổ. 

Cửa sổ hiện tại là`[l, r]`. 
4. Trong khi cửa sổ không hợp lệ, hãy di chuyển`l`phía trước. 

Cửa sổ không hợp lệ khi:$a_r > 2a_l$Vì mảng đã được sắp xếp nên tăng dần`l`tăng giá trị tối thiểu của cửa sổ, làm cho điều kiện dễ thỏa mãn hơn. 
5. Sau khi khôi phục tính hợp lệ, hãy tính kích thước cửa sổ. 

Độ dài phân đoạn hợp lệ hiện tại là:```
r - l + 1
```Theo dõi độ dài hợp lệ tối đa được thấy cho đến nay. 
6. Sau khi xử lý xong tất cả các vị trí, in:```
n - maximum_valid_length
```Đây là số lần xóa tối thiểu cần thiết. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mọi giải pháp hợp lệ đều tương ứng với một mảng con liền kề. Nếu hai phần tử được giữ lại thỏa mãn điều kiện thì mọi giá trị giữa chúng cũng tự động thỏa mãn điều kiện đó vì thứ tự sắp xếp sẽ duy trì phạm vi. 

Cửa sổ trượt duy trì sự bất biến mà phân đoạn hiện tại`[l, r]`luôn hợp lệ sau khi vòng lặp bên trong kết thúc. Đối với mỗi điểm cuối bên phải`r`, thuật toán tìm giá trị nhỏ nhất có thể`l`giữ cho phân đoạn hợp lệ, điều này cũng cung cấp cửa sổ hợp lệ lớn nhất kết thúc tại`r`. 

Bởi vì mọi phân đoạn tối ưu có thể xuất hiện dưới dạng một cửa sổ nào đó trong quá trình quét nên thuật toán không thể bỏ lỡ câu trả lời tốt nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    l = 0
    best = 0

    for r in range(n):
        while a[r] > 2 * a[l]:
            l += 1

        best = max(best, r - l + 1)

    print(n - best)

solve()
```Bước đầu tiên sắp xếp các phép đo. Nếu không sắp xếp, sẽ không có cách hiệu quả nào để duy trì giá trị tối thiểu và tối đa của phân khúc ứng cử viên trong khi vẫn duy trì tính liên tục của các phần tử được chọn. 

Con trỏ trái`l`chỉ tiến về phía trước. Điều này quan trọng đối với hiệu quả. Nếu nó có thể di chuyển lùi lại thì độ phức tạp sẽ không còn tuyến tính sau khi sắp xếp. 

Tình trạng bên trong`while`vòng lặp rất nghiêm ngặt:```
a[r] > 2 * a[l]
```sử dụng`>=`ở đây sẽ sai vì sự bình đẳng được cho phép. Ví dụ,`[2, 4]`là hợp lệ. 

Kích thước cửa sổ được tính như sau:```
r - l + 1
```Đây là một chi tiết cổ điển. Cả hai điểm cuối đều được bao gồm trong phân đoạn hiện tại. 

Câu trả lời cuối cùng trừ đi phân đoạn được giữ hợp lệ lớn nhất trong tổng số phép đo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
4 5 3 8 3 7
```Sau khi sắp xếp:```
[3, 3, 4, 5, 7, 8]
```| r | một [r] | l sau khi điều chỉnh | Cửa sổ | Kích thước cửa sổ | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 0 | [3] | 1 | 1 | 
| 1 | 3 | 0 | [3, 3] | 2 | 2 | 
| 2 | 4 | 0 | [3, 3, 4] | 3 | 3 | 
| 3 | 5 | 0 | [3, 3, 4, 5] | 4 | 4 | 
| 4 | 7 | 4 | [7] | 1 | 4 | 
| 5 | 8 | 4 | [7, 8] | 2 | 4 | 

Phân đoạn hợp lệ lớn nhất có kích thước`4`, vì vậy chúng tôi loại bỏ`6 - 4 = 2`số đo. 

Dấu vết này cho thấy con trỏ trái có thể nhảy một số vị trí như thế nào khi điều kiện tỷ lệ trở nên không hợp lệ. 

### Ví dụ 2 

đầu vào:```
6
1 2 2 2 4 4
```Mảng được sắp xếp:```
[1, 2, 2, 2, 4, 4]
```| r | một [r] | l sau khi điều chỉnh | Cửa sổ | Kích thước cửa sổ | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | [1] | 1 | 1 | 
| 1 | 2 | 0 | [1, 2] | 2 | 2 | 
| 2 | 2 | 0 | [1, 2, 2] | 3 | 3 | 
| 3 | 2 | 0 | [1, 2, 2, 2] | 4 | 4 | 
| 4 | 4 | 1 | [2, 2, 2, 4] | 4 | 4 | 
| 5 | 4 | 1 | [2, 2, 2, 4, 4] | 5 | 5 | 

Sự lựa chọn tối ưu chỉ loại bỏ`1`. 

Ví dụ này giải thích tại sao chúng ta không nên di chuyển con trỏ trái nhiều hơn mức cần thiết. Một lần`l`trở thành`1`, cửa sổ sẽ hợp lệ trở lại và giữ kích thước tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, cửa sổ trượt là tuyến tính | 
| Không gian | O(1) | Chỉ có một vài biến ngoài mảng đầu vào | 

Giải pháp dễ dàng phù hợp trong giới hạn. Sắp xếp`10^5`số nguyên rất nhanh trong Python và quá trình quét hai con trỏ chạm vào mỗi phần tử nhiều nhất hai lần. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    l = 0
    best = 0

    for r in range(n):
        while a[r] > 2 * a[l]:
            l += 1

        best = max(best, r - l + 1)

    print(n - best)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run("6\n4 5 3 8 3 7\n") == "2\n", "sample 1"

# minimum size
assert run("2\n1 5000\n") == "1\n", "minimum size"

# already valid
assert run("5\n2 2 3 4 4\n") == "0\n", "already valid"

# all equal
assert run("6\n7 7 7 7 7 7\n") == "0\n", "all equal"

# off-by-one boundary
assert run("4\n2 4 4 4\n") == "0\n", "equality allowed"

# single bad small element
assert run("6\n1 2 2 2 4 4\n") == "1\n", "remove smallest"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 5000`|`1`| Hành vi kích thước tối thiểu | 
|`5 / 2 2 3 4 4`|`0`| Mảng đã hợp lệ | 
|`6 / 7 7 7 7 7 7`|`0`| Xử lý trùng lặp | 
|`4 / 2 4 4 4`|`0`| Ranh giới bình đẳng`y = 2x`| 
|`6 / 1 2 2 2 4 4`|`1`| Đúng chuyển động của con trỏ trái | 

## Vỏ cạnh 

Xét trường hợp ranh giới đẳng thức:```
4
2 4 4 4
```Sau khi sắp xếp, mảng vẫn giữ nguyên. Khi`r`đạt đến đầu tiên`4`, điều kiện được kiểm tra là:```
4 > 2 * 2
```trở thành:```
4 > 4
```Điều này là sai nên cửa sổ vẫn hợp lệ. Thuật toán giữ chính xác tất cả các phần tử và đầu ra`0`. 

Bây giờ hãy xem xét trường hợp không khớp cực độ:```
2
1 5000
```Tại`r = 1`, điều kiện trở thành:```
5000 > 2
```điều đó đúng, vậy`l`tiến tới`1`. Cửa sổ hợp lệ trở thành`[5000]`với kích thước`1`. Đầu ra của thuật toán`2 - 1 = 1`. 

Cuối cùng, hãy xem xét các phần tử lặp lại gần ranh giới:```
6
1 2 2 2 4 4
```Khi`r`đạt đến đầu tiên`4`, mức tối thiểu hiện tại`1`không còn hoạt động vì:```
4 > 2
```Thuật toán tiến bộ`l`một lần, tạo giá trị tối thiểu`2`. Bây giờ điều kiện trở thành:```
4 > 4
```đó là sai, do đó cửa sổ hợp lệ lớn được giữ nguyên. Điều này xác nhận rằng cửa sổ trượt chỉ co lại ở mức cần thiết, điều này cần thiết để đạt được sự tối ưu.
