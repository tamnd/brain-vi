---
title: "CF 433C - Ghi chú ký ức của Ryouko"
description: "Các trang vở được đánh số từ 1 đến n. Trình tự a mô tả thứ tự mà Ryouko sẽ đọc thông tin. Nếu hai thông tin liên tiếp nằm ở trang a[i] và a[i+1], cô phải lần lượt Tổng nỗ lực là tổng các khoảng cách này trên tất cả các cặp liên tiếp."
date: "2026-06-07T02:41:06+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 433
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 248 (Div. 2)"
rating: 1800
weight: 433
solve_time_s: 265
verified: true
draft: false
---

[CF 433C - Ghi chú ký ức của Ryouko](https://codeforces.com/problemset/problem/433/C) 

**Đánh giá:** 1800 
**Tags:** triển khai, toán học, sắp xếp 
**Thời gian giải:** 4 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Các trang sổ tay được đánh số từ`1`ĐẾN`n`. Trình tự`a`mô tả thứ tự mà Ryouko sẽ đọc thông tin. Nếu có hai thông tin liên tiếp trên các trang`a[i]`Và`a[i+1]`, cô ấy phải quay lại`|a[i] - a[i+1]|`trang. 

Tổng nỗ lực là tổng của các khoảng cách này trên tất cả các cặp liên tiếp. 

Trước khi đọc, cô ấy có thể thực hiện nhiều nhất một thao tác. Cô chọn số trang`x`và số trang`y`, sao chép mọi thứ từ`x`lên`y`, và mỗi lần xuất hiện của`x`bên trong chuỗi trở thành`y`. Nhiệm vụ là tìm tổng chi phí lật trang tối thiểu có thể sau khi áp dụng thao tác này một lần hoặc không thay đổi bất kỳ điều gì. 

Kích thước đầu vào lớn. Cả hai`n`Và`m`có thể đạt được`100000`. Bản thân trình tự có thể chứa`100000`phần tử, do đó bất kỳ thuật toán nào thử tất cả các cặp`(x, y)`ngay lập tức là không thể. Có tới`10^10`những cặp như vậy. Ngay cả việc kiểm tra mọi trang mục tiêu có thể có cho từng giá trị trang cũng sẽ vượt quá giới hạn thời gian. 

Câu trả lời cũng có thể vượt quá phạm vi 32 bit. Một chuỗi độ dài`100000`có thể đóng góp đại khái`100000 × 100000`, vì vậy cần có số học 64 bit. 

Một số trường hợp khó xử lý. 

Coi như:```
3 4
2 2 2 2
```Chi phí đã bằng không. Không có chuyển tiếp giữa các trang khác nhau. Bất kỳ giải pháp nào giả định mọi giá trị đều tham gia vào ít nhất một cạnh có thể vô tình truy cập vào cấu trúc trống. 

Coi như:```
5 2
1 5
```Chi phí ban đầu là`4`. Trang hợp nhất`5`vào trang`1`tạo ra trình tự`[1,1]`và chi phí`0`. Giải pháp chỉ xem xét tần suất của các trang thay vì các trang lân cận sẽ bỏ lỡ cải tiến này. 

Coi như:```
5 5
1 2 1 2 1
```Trang`1`tương tác với trang`2`bốn lần. Những sự lặp lại đó quan trọng. Việc coi những người hàng xóm như một tập hợp thay vì nhiều tập hợp sẽ đánh giá thấp sự đóng góp của các chuyển đổi lặp đi lặp lại và tạo ra mức tăng sai. 

## Phương pháp tiếp cận 

Bắt đầu với ý tưởng trực tiếp nhất. 

Giả sử chúng ta chọn một trang`x`và thay thế mỗi lần xuất hiện của nó bằng một số trang`y`. Chúng ta có thể xây dựng lại trình tự và tính toán lại tổng chi phí. Làm điều này cho mỗi cặp`(x, y)`yêu cầu`O(n^2 m)`công việc. 

Với`n = m = 100000`, đây là đại khái`10^15`hoạt động, hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là việc thay thế trang`x`chỉ ảnh hưởng đến chuyển tiếp chạm`x`. 

Thực hiện chuyển đổi giữa các trang`u`Và`v`. 

Nếu không có điểm cuối nào bằng`x`, thì sau khi hợp nhất, quá trình chuyển đổi vẫn không thay đổi và đóng góp số tiền chính xác như nhau. Chỉ các cạnh tiếp giáp với`x`vấn đề. 

Đối với một giá trị trang cố định`x`, thu thập mọi trang xuất hiện bên cạnh`x`trong trình tự. Nếu quá trình chuyển đổi xảy ra nhiều lần, hãy giữ nhiều bản sao. Đặt nhiều hàng xóm này là:```
v1, v2, ..., vk
```Trước khi có bất kỳ thay đổi nào, phần tổng chi phí liên quan đến`x`là```
|x-v1| + |x-v2| + ... + |x-vk|
```Nếu chúng ta thay thế`x`qua`y`, nó trở thành```
|y-v1| + |y-v2| + ... + |y-vk|
```Bây giờ vấn đề trở thành: 

Đối với mỗi giá trị trang`x`, tìm giá trị`y`giảm thiểu```
Σ |y-vi|
```Đây là một thuộc tính cổ điển của các giá trị tuyệt đối. Mức tối thiểu đạt được ở bất kỳ trung vị nào của nhiều tập hợp. 

Vì vậy, với mỗi giá trị trang`x`: 

1. Tập hợp tất cả các giá trị trang lân cận. 
2. Tính toán đóng góp hiện tại của nó. 
3. Sắp xếp danh sách hàng xóm. 
4. Lấy trung vị của nó. 
5. Tính phần đóng góp nếu`x`đã được thay thế bởi trung vị đó. 
6. Sự khác biệt là sự cải thiện có được nhờ việc hợp nhất trang một cách tối ưu`x`. 

Sự hợp nhất tốt nhất đơn giản là sự cải tiến lớn nhất trong số tất cả các giá trị trang. 

Tổng kích thước của tất cả các danh sách hàng xóm chỉ là`2(m-1)`bởi vì mỗi cặp liền kề đóng góp một lần cho mỗi điểm cuối. Sắp xếp tất cả các danh sách cùng nhau`O(m log m)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²m) | O(m) | Quá chậm | 
| Tối ưu | O(m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng chi phí ban đầu bằng cách cộng`|a[i] - a[i+1]|`trên tất cả các cặp liền kề. 
2. Với mọi cặp liền kề`(a[i], a[i+1])`, nếu các giá trị khác nhau, hãy thêm từng điểm cuối vào danh sách lân cận của điểm cuối kia. 

Điều này ghi lại chính xác những chuyển đổi nào sẽ bị ảnh hưởng nếu một điểm cuối bị thay đổi. 
3. Đối với mỗi giá trị trang`x`từ`1`ĐẾN`n`, xử lý danh sách hàng xóm của nó. 
4. Tính toán```
old = Σ |x - v|
```trên tất cả hàng xóm`v`của`x`. 

Đây là sự đóng góp hiện tại của tất cả các chuyển tiếp chạm vào`x`. 
5. Sắp xếp danh sách hàng xóm và lấy giá trị trung bình của nó. 

Giá trị trung bình giảm thiểu tổng độ lệch tuyệt đối. 
6. Tính toán```
new = Σ |median - v|
```trên cùng một danh sách hàng xóm. 
7. Lợi ích từ việc thay thế trang một cách tối ưu`x`là```
gain = old - new
```8. Giữ mức tăng tối đa trên tất cả các giá trị trang. 
9. Câu trả lời là```
original_cost - maximum_gain
```### Tại sao nó hoạt động 

Mỗi quá trình chuyển đổi đóng góp độc lập vào tổng chi phí. Trang thay thế`x`chỉ thay đổi sự cố chuyển tiếp thành`x`; tất cả các chuyển đổi khác vẫn giữ nguyên. 

Đối với một trang cố định`x`, phần bị ảnh hưởng của mục tiêu chính xác là```
Σ |y - v|
```Ở đâu`v`trải rộng trên tất cả các lân cận của`x`, được tính theo bội số. Thuộc tính trung bình cổ điển nói rằng biểu thức này được giảm thiểu bằng cách chọn`y`bằng với trung vị của nhiều tập hợp. 

Vì mỗi giá trị trang có thể được tối ưu hóa độc lập và chúng tôi chỉ được phép thực hiện một thao tác hợp nhất, nên bước di chuyển tối ưu toàn cục là trang có mức giảm tốt nhất có thể là lớn nhất. Trừ đi mức giảm đó khỏi chi phí ban đầu sẽ cho ra tổng số tối thiểu có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    neigh = [[] for _ in range(n + 1)]
    total = 0

    for i in range(m - 1):
        u = a[i]
        v = a[i + 1]

        total += abs(u - v)

        if u != v:
            neigh[u].append(v)
            neigh[v].append(u)

    best_gain = 0

    for x in range(1, n + 1):
        if not neigh[x]:
            continue

        vec = neigh[x]

        old = 0
        for v in vec:
            old += abs(x - v)

        vec.sort()
        med = vec[len(vec) // 2]

        new = 0
        for v in vec:
            new += abs(med - v)

        best_gain = max(best_gain, old - new)

    print(total - best_gain)

solve()
```Vòng lặp đầu tiên tính toán chi phí ban đầu và đồng thời xây dựng danh sách hàng xóm. Mỗi cặp liền kề đóng góp vào chính xác hai danh sách, một danh sách cho mỗi điểm cuối. 

Các chuyển đổi trong đó cả hai điểm cuối đều bằng nhau sẽ bị bỏ qua khi xây dựng danh sách lân cận. Những chuyển đổi như vậy đã đóng góp bằng 0 và vẫn bằng 0 bất kể có bất kỳ sự hợp nhất nào, vì vậy chúng không thể ảnh hưởng đến việc tính toán mức tăng. 

Đối với mỗi giá trị trang,`old`đo lường chi phí hiện tại do tất cả các chuyển đổi sự cố đóng góp. Sau khi sắp xếp danh sách lân cận, phần tử ở giữa là trung vị. Việc thay thế trang bằng số trung vị này sẽ mang lại giá trị nhỏ nhất có thể có của chi phí bị ảnh hưởng, được tính như sau`new`. 

Sự cải thiện là`old - new`. Cải tiến lớn nhất tương ứng với hoạt động hợp nhất tốt nhất. 

Số nguyên Python tự động xử lý các giá trị lớn hơn 32 bit, do đó không cần xử lý tràn đặc biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 6
1 2 3 4 3 2
```Các chuyển tiếp liền kề là:```
1-2, 2-3, 3-4, 4-3, 3-2
```Chi phí ban đầu:```
1 + 1 + 1 + 1 + 1 = 5
```Danh sách hàng xóm: 

| Trang | Hàng xóm | 
| --- | --- | 
| 1 | [2] | 
| 2 | [1, 3, 3] | 
| 3 | [2, 4, 4, 2] | 
| 4 | [3, 3] | 

Xử lý: 

| Trang x | cũ | Trung vị | mới | Đạt được | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 0 | 1 | 
| 2 | 3 | 3 | 2 | 1 | 
| 3 | 4 | 4 | 4 | 0 | 
| 4 | 2 | 3 | 0 | 2 | 

Mức tăng tối đa là`2`. 

Câu trả lời cuối cùng:```
5 - 2 = 3
```Điều này tương ứng với việc thay thế trang`4`với trang`3`. 

### Ví dụ 2 

đầu vào:```
5 5
1 5 1 5 1
```Chi phí ban đầu:```
4 + 4 + 4 + 4 = 16
```Danh sách hàng xóm: 

| Trang | Hàng xóm | 
| --- | --- | 
| 1 | [5, 5, 5, 5] | 
| 5 | [1, 1, 1, 1] | 

Xử lý: 

| Trang x | cũ | Trung vị | mới | Đạt được | 
| --- | --- | --- | --- | --- | 
| 1 | 16 | 5 | 0 | 16 | 
| 5 | 16 | 1 | 0 | 16 | 

Mức tăng tối đa là`16`. 

Câu trả lời cuối cùng:```
16 - 16 = 0
```Dấu vết cho thấy tại sao những hàng xóm lặp lại phải được tính theo bội số. Mỗi lần xuất hiện đóng góp riêng vào chi phí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Tổng kích thước của tất cả các danh sách lân cận là O(m) và việc sắp xếp tất cả chúng tốn O(m log m) | 
| Không gian | O(m) | Danh sách lân cận lưu trữ tổng giá trị O(m) | 

Với`m ≤ 100000`, MỘT`O(m log m)`giải pháp là thoải mái trong giới hạn. Việc sử dụng bộ nhớ cũng tuyến tính ở kích thước đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    neigh = [[] for _ in range(n + 1)]
    total = 0

    for i in range(m - 1):
        u = a[i]
        v = a[i + 1]

        total += abs(u - v)

        if u != v:
            neigh[u].append(v)
            neigh[v].append(u)

    best = 0

    for x in range(1, n + 1):
        if not neigh[x]:
            continue

        vec = neigh[x]

        old = sum(abs(x - v) for v in vec)

        vec.sort()
        med = vec[len(vec) // 2]

        new = sum(abs(med - v) for v in vec)

        best = max(best, old - new)

    return str(total - best)

# provided sample
assert run("4 6\n1 2 3 4 3 2\n") == "3", "sample 1"

# minimum size
assert run("1 1\n1\n") == "0", "single element"

# all equal
assert run("3 4\n2 2 2 2\n") == "0", "all equal"

# simple merge to zero
assert run("5 2\n1 5\n") == "0", "single transition"

# repeated alternating values
assert run("5 5\n1 5 1 5 1\n") == "0", "multiplicity handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`0`| Kích thước đầu vào tối thiểu | 
|`3 4 / 2 2 2 2`|`0`| Danh sách hàng xóm trống | 
|`5 2 / 1 5`|`0`| Chuyển tiếp đơn | 
|`5 5 / 1 5 1 5 1`|`0`| Hàng xóm lặp đi lặp lại phải được tính nhiều lần | 

## Vỏ cạnh 

Hãy xem xét:```
1 1
1
```Không có cặp liền kề nên giá gốc là`0`. Mọi danh sách hàng xóm đều trống. Thuật toán bỏ qua tất cả các trang, giữ`best_gain = 0`và đầu ra`0`. 

Coi như:```
3 4
2 2 2 2
```Mọi sự khác biệt liền kề đều bằng không. Không có hàng xóm nào được ghi lại vì tất cả các giá trị liền kề đều bằng nhau. Thuật toán không bao giờ cố gắng tính trung vị của danh sách trống và trả về chính xác`0`. 

Coi như:```
5 2
1 5
```Chi phí ban đầu là`4`. Trang`1`có danh sách hàng xóm`[5]`, cho`old = 4`,`median = 5`,`new = 0`, nhận được`4`. Mức tăng tương tự xuất hiện cho trang`5`. Câu trả lời cuối cùng là`4 - 4 = 0`. 

Coi như:```
5 5
1 2 1 2 1
```Danh sách hàng xóm của trang`1`là`[2,2,2,2]`, không chỉ`{2}`. Thuật toán lưu trữ tất cả bốn bản sao. Đóng góp của nó là`4`, và thay thế`1`qua`2`loại bỏ tất cả bốn đơn vị chi phí. Việc coi những người hàng xóm như một tập hợp sẽ chỉ tính sai một lần xuất hiện và đánh giá thấp mức tăng.
