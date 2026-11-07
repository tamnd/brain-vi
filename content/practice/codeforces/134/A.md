---
title: "CF 134A - Số trung bình"
description: "Chúng ta được cho một mảng các số nguyên dương. Đối với mọi vị trí, chúng tôi loại bỏ phần tử đó và tính giá trị trung bình số học của các số còn lại. Chúng ta phải tìm tất cả các vị trí trong đó giá trị bị loại bỏ chính xác bằng giá trị trung bình đó. Giả sử mảng là [1, 2, 3, 4, 5]."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 134
codeforces_index: "A"
codeforces_contest_name: "Codeforces Testing Round 3"
rating: 1200
weight: 134
solve_time_s: 99
verified: true
draft: false
---

[CF 134A - Số trung bình](https://codeforces.com/problemset/problem/134/A) 

**Đánh giá:** 1200 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Đối với mọi vị trí, chúng tôi loại bỏ phần tử đó và tính giá trị trung bình số học của các số còn lại. Chúng ta phải tìm tất cả các vị trí trong đó giá trị bị loại bỏ chính xác bằng giá trị trung bình đó. 

Giả sử mảng là`[1, 2, 3, 4, 5]`. Nếu chúng ta loại bỏ`3`, các số còn lại là`1, 2, 4, 5`. Tổng của họ là`12`, và trung bình của họ là`12 / 4 = 3`, vậy chỉ mục`3`là hợp lệ. 

Kích thước mảng có thể đạt tới`2 · 10^5`, điều này ngay lập tức loại trừ mọi giải pháp tính lại tổng từ đầu cho mọi vị trí. Một thuật toán bậc hai sẽ thực hiện xung quanh`4 · 10^10`trong trường hợp xấu nhất, vượt xa giới hạn thời gian 1 giây. Chúng ta cần một cái gì đó gần với thời gian tuyến tính. 

Bản thân các giá trị này là nhỏ, nhiều nhất`1000`, nhưng điều đó không đủ để biện minh cho bất kỳ vòng lặp lồng nhau đắt tiền nào. Điểm nghẽn là số lượng phần tử chứ không phải độ lớn của các con số. 

Một sai lầm dễ mắc phải là sử dụng số học dấu phẩy động để tính giá trị trung bình. Điều kiện là sự bình đẳng chính xác và việc so sánh dấu phẩy động có thể gây ra các vấn đề về độ chính xác. Mọi thứ đều có thể được xử lý bằng số nguyên. 

Hãy xem xét đầu vào này:```
3
1 2 3
```Nếu chúng ta loại bỏ`2`, số trung bình còn lại là`(1 + 3) / 2 = 2`, vậy chỉ mục`2`là hợp lệ. 

Việc triển khai bất cẩn có thể tính trung bình dưới dạng số dấu phẩy động và so sánh chúng trực tiếp, điều này không cần thiết và rủi ro. 

Một trường hợp tinh vi khác là khi tổng còn lại không chia hết cho`n - 1`.```
4
1 1 1 2
```Đối với phần tử cuối cùng, giá trị trung bình còn lại là`(1 + 1 + 1) / 3 = 1`, Vì thế`2`không hợp lệ. 

Đối với phần tử đầu tiên, giá trị trung bình còn lại là`(1 + 1 + 2) / 3 = 4/3`, không phải là số nguyên. Vì các phần tử mảng là số nguyên nên chỉ mục này không thể hoạt động. Việc quên kiểm tra tính chia hết có thể dẫn đến câu trả lời sai. 

Các mảng trong đó tất cả các giá trị đều bằng nhau cũng đáng được kiểm tra:```
5
7 7 7 7 7
```Mọi chỉ mục đều hợp lệ vì loại bỏ bất kỳ`7`để lại trung bình`7`. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. Đối với mỗi chỉ số`i`, di dời`a[i]`, tính tổng các phần tử còn lại, chia cho`n - 1`, và kiểm tra xem kết quả có bằng không`a[i]`. 

Lực lượng vũ phu là chính xác vì nó tuân theo định nghĩa theo nghĩa đen. Vấn đề là tổng kết lặp đi lặp lại. Tính số tiền còn lại`O(n)`thời gian cho một vị trí và chúng tôi lặp lại điều này cho tất cả`n`chức vụ, trao`O(n^2)`tổng số phức tạp. 

Với`n = 2 · 10^5`, điều này trở thành đại khái:```
2 · 10^5 × 2 · 10^5 = 4 · 10^10
```hoạt động quá chậm. 

Quan sát quan trọng là tổng của các phần tử còn lại có thể được tính ngay lập tức nếu chúng ta đã biết tổng của mảng. 

Cho phép:```
total = sum of all elements
```Nếu chúng ta loại bỏ`a[i]`, sau đó:```
remaining_sum = total - a[i]
```Điều kiện trở thành:```
a[i] = (total - a[i]) / (n - 1)
```Thay vì thực sự tính giá trị trung bình dưới dạng giá trị dấu phẩy động, chúng ta sắp xếp lại phương trình:```
a[i] × (n - 1) = total - a[i]
```hoặc tương đương:```
a[i] × n = total
```Đây là toàn bộ vấn đề được rút gọn thành một đẳng thức đơn giản. 

Bây giờ mọi vị trí có thể được kiểm tra độc lập trong thời gian không đổi, đưa ra`O(n)`giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và tính tổng của nó. 

Tổng tổng cho phép chúng ta tính tổng của các phần tử còn lại sau khi loại bỏ bất kỳ vị trí nào trong thời gian không đổi. 
2. Tạo danh sách trống`answer`để lưu trữ các chỉ số hợp lệ. 

Chúng ta phải xuất các chỉ số theo thứ tự tăng dần, do đó việc quét từ trái sang phải sẽ giữ nguyên thứ tự được yêu cầu một cách tự nhiên. 
3. Đối với mọi chỉ số`i`, kiểm tra xem:```
a[i] × n == total
```Điều này xuất phát trực tiếp từ việc sắp xếp lại điều kiện trung bình. 
4. Nếu điều kiện được giữ, hãy nối thêm`i + 1`vào danh sách câu trả lời. 

Vấn đề sử dụng lập chỉ mục dựa trên 1, trong khi mảng Python sử dụng lập chỉ mục dựa trên 0. 
5. In số chỉ mục hợp lệ. 
6. Tự in các chỉ số. 

### Tại sao nó hoạt động 

Đối với một chỉ mục`i`là hợp lệ, giá trị`a[i]`phải bằng trung bình cộng của tất cả các phần tử khác:```
a[i] = (total - a[i]) / (n - 1)
```Nhân cả hai vế với`n - 1`mang lại:```
a[i](n - 1) = total - a[i]
```Di chuyển các điều khoản với nhau:```
a[i]n = total
```Thuật toán kiểm tra chính xác điều kiện này cho mọi chỉ mục. Vì phương trình tương đương về mặt toán học với định nghĩa ban đầu nên mọi chỉ số được báo cáo đều đúng và mọi chỉ số đúng đều được báo cáo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    total = sum(a)

    ans = []

    for i in range(n):
        if a[i] * n == total:
            ans.append(i + 1)

    print(len(ans))

    if ans:
        print(*ans)

solve()
```Bước đầu tiên tính tổng số tiền một lần. Đây là sự tối ưu hóa quan trọng giúp loại bỏ nhu cầu tính tổng lặp đi lặp lại. 

Điều kiện:```
a[i] * n == total
```là phiên bản sắp xếp lại của phương trình trung bình ban đầu. Sử dụng số học số nguyên sẽ tránh được tất cả các vấn đề về dấu phẩy động. 

Danh sách câu trả lời lưu trữ các chỉ số dựa trên 1 vì đó là những gì câu lệnh yêu cầu. quên đi`+1`là một lỗi thường gặp. 

Dòng đầu ra thứ hai chỉ được in khi có ít nhất một chỉ mục hợp lệ. Vấn đề cho phép bỏ qua dòng hoặc in một dòng trống khi số câu trả lời bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 4 5
```Tổng số tiền là:```
15
```| Chỉ mục | Giá trị | Kiểm tra`value × n`| Tổng cộng | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 15 | Không | 
| 2 | 2 | 10 | 15 | Không | 
| 3 | 3 | 15 | 15 | Có | 
| 4 | 4 | 20 | 15 | Không | 
| 5 | 5 | 25 | 15 | Không | 

Đầu ra:```
1
3
```Ví dụ này cho thấy điều kiện đơn giản hóa đang hoạt động. Thay vì tính toán lại các giá trị trung bình nhiều lần, mỗi chỉ số sẽ rút gọn thành một phép nhân và một phép so sánh. 

### Ví dụ 2 

đầu vào:```
5
7 7 7 7 7
```Tổng số tiền là:```
35
```| Chỉ mục | Giá trị | Kiểm tra`value × n`| Tổng cộng | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 35 | 35 | Có | 
| 2 | 7 | 35 | 35 | Có | 
| 3 | 7 | 35 | 35 | Có | 
| 4 | 7 | 35 | 35 | Có | 
| 5 | 7 | 35 | 35 | Có | 

Đầu ra:```
5
1 2 3 4 5
```Điều này xác nhận rằng thuật toán xử lý chính xác các mảng trong đó mọi vị trí đều thỏa mãn điều kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để tính tổng và một lượt để kiểm tra tất cả các chỉ số | 
| Không gian | O(1) không bao gồm đầu ra | Chỉ có một vài biến được sử dụng | 

Giải pháp thoải mái phù hợp với các hạn chế. Ngay cả đối với`n = 2 · 10^5`, thuật toán chỉ thực hiện một vài lần tuyến tính trên mảng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = io.StringIO()
    sys.stdout = out

    n = int(input())
    a = list(map(int, input().split()))

    total = sum(a)

    ans = []

    for i in range(n):
        if a[i] * n == total:
            ans.append(i + 1)

    print(len(ans))

    if ans:
        print(*ans)

    sys.stdout = sys.__stdout__

    return out.getvalue()

# provided sample
assert run("5\n1 2 3 4 5\n") == "1\n3\n", "sample 1"

# minimum size, no valid index
assert run("2\n1 2\n") == "0\n", "minimum size"

# all equal values
assert run("4\n6 6 6 6\n") == "4\n1 2 3 4\n", "all equal"

# exactly one valid index
assert run("3\n1 2 3\n") == "1\n2\n", "single valid index"

# no index because average is fractional
assert run("4\n1 1 1 2\n") == "0\n", "fractional averages"

# larger mixed case
assert run("6\n2 2 2 2 2 2\n") == "6\n1 2 3 4 5 6\n", "all positions valid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 2`|`0`| Kích thước mảng tối thiểu | 
|`4 / 6 6 6 6`|`4 / 1 2 3 4`| Mọi vị trí đều hợp lệ | 
|`3 / 1 2 3`|`1 / 2`| Chỉ mục hợp lệ duy nhất | 
|`4 / 1 1 1 2`|`0`| Trung bình còn lại phân số | 
|`6 / 2 2 2 2 2 2`|`6 / 1 2 3 4 5 6`| Trường hợp hoàn toàn bằng nhau lớn hơn | 

## Vỏ cạnh 

Hãy xem xét trường hợp trung bình còn lại là phân số. 

đầu vào:```
4
1 1 1 2
```Tổng số tiền là`5`. 

Đối với từng vị trí: 

| Chỉ mục | Giá trị | Kiểm tra`value × n`| Tổng cộng | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 5 | Không | 
| 2 | 1 | 4 | 5 | Không | 
| 3 | 1 | 4 | 5 | Không | 
| 4 | 2 | 8 | 5 | Không | 

Đầu ra:```
0
```Thuật toán không bao giờ thực hiện phép chia, do đó, nó tự nhiên xử lý chính xác các giá trị trung bình không nguyên. 

Bây giờ hãy xem xét một mảng trong đó tất cả các giá trị đều giống hệt nhau. 

đầu vào:```
5
9 9 9 9 9
```Tổng số tiền là`45`. 

Đối với mọi chỉ số:```
9 × 5 = 45
```nên mọi vị trí đều được chấp nhận. 

Đầu ra:```
5
1 2 3 4 5
```Điều này xác nhận rằng điều kiện đẳng thức nắm bắt chính xác mối quan hệ trung bình ngay cả khi nhiều chỉ số thỏa mãn nó cùng một lúc. 

Cuối cùng, hãy xem xét kích thước mảng nhỏ nhất có thể. 

đầu vào:```
2
3 3
```Tổng số tiền là`6`. 

Đối với cả hai vị trí:```
3 × 2 = 6
```Cả hai chỉ số đều hợp lệ vì loại bỏ một`3`để lại một phần tử còn lại`3`, trung bình của nó vẫn là`3`. 

Đầu ra:```
2
1 2
```Điều này xác minh rằng công thức cũng hoạt động chính xác với giá trị nhỏ nhất được phép`n`.
