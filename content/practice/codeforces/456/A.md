---
title: "CF 456A - Laptop"
description: "Chúng tôi được tặng một bộ sưu tập máy tính xách tay. Mỗi chiếc laptop đều có một mức giá và một giá trị chất lượng. Giá cả đều khác biệt, và chất lượng đều khác biệt. Dima tin rằng một chiếc laptop đắt tiền hơn luôn phải có chất lượng tốt hơn. Alex tuyên bố rằng điều này không hẳn là đúng."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "sortings"]
categories: ["algorithms"]
codeforces_contest: 456
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 260 (Div. 2)"
rating: 1100
weight: 456
solve_time_s: 88
verified: true
draft: false
---

[CF 456A - Máy tính xách tay](https://codeforces.com/problemset/problem/456/A) 

**Đánh giá:** 1100 
**Thẻ:** sắp xếp 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ sưu tập máy tính xách tay. Mỗi chiếc laptop đều có một mức giá và một giá trị chất lượng. Giá cả đều khác biệt, và chất lượng đều khác biệt. 

Dima tin rằng một chiếc laptop đắt tiền hơn luôn phải có chất lượng tốt hơn. Alex tuyên bố rằng điều này không hẳn là đúng. Ông nói rằng có thể tồn tại hai chiếc máy tính xách tay mà chiếc rẻ hơn thực sự có chất lượng cao hơn chiếc đắt tiền hơn. 

Nhiệm vụ của chúng tôi là xác định xem tuyên bố của Alex có đúng với bộ máy tính xách tay nhất định hay không. 

Đầu vào bao gồm tối đa$10^5$máy tính xách tay. Đối với mỗi máy tính xách tay, chúng tôi nhận được hai số nguyên: giá và chất lượng của nó. Đầu ra là`"Happy Alex"`nếu tồn tại ít nhất một cặp trong đó máy tính xách tay rẻ hơn có chất lượng cao hơn máy tính xách tay đắt tiền hơn. Nếu không chúng tôi in`"Poor Alex"`. 

Ràng buộc$n \le 10^5$ngay lập tức loại trừ việc kiểm tra từng cặp máy tính xách tay. Một thuật toán bậc hai sẽ thực hiện khoảng$$\frac{10^5 \cdot (10^5 - 1)}{2} \approx 5 \times 10^9$$so sánh trong trường hợp xấu nhất, vượt xa những gì có thể chạy trong vòng một giây. Chúng ta cần thứ gì đó xung quanh$O(n \log n)$hoặc tốt hơn. 

Một điểm tinh tế là chỉ số máy tính xách tay không quan trọng. Chỉ có mối quan hệ giữa giá cả và chất lượng mới quan trọng. Máy tính xách tay xuất hiện sớm hơn trong đầu vào không có nghĩa là nó rẻ hơn hoặc đắt hơn. 

Hãy xem xét ví dụ này:```
3
10 100
20 200
30 300
```Đầu ra đúng là:```
Poor Alex
```Mỗi sự tăng giá đều đi kèm với sự gia tăng về chất lượng. 

Bây giờ hãy xem xét:```
3
10 300
20 200
30 100
```Đầu ra đúng là:```
Happy Alex
```Laptop giá 10 có chất lượng tốt hơn laptop giá 20 và 30. 

Một lỗi dễ mắc phải khác là chỉ so sánh các dòng đầu vào lân cận:```
3
30 300
10 200
20 100
```Đầu ra đúng là:```
Happy Alex
```Sự mâu thuẫn chỉ trở nên rõ ràng sau khi đặt mua máy tính xách tay theo giá. Thứ tự đầu vào ban đầu không chứa cấu trúc hữu ích. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. Đối với mỗi máy tính xách tay, hãy so sánh nó với mọi máy tính xách tay khác. Bất cứ khi nào chúng tôi tìm thấy hai máy tính xách tay trong đó một chiếc có giá nhỏ hơn nhưng chất lượng cao hơn, tuyên bố của Alex đã được chứng minh và chúng tôi có thể dừng lại. 

Điều này hiệu quả vì nó kiểm tra rõ ràng điều kiện chính xác từ báo cáo vấn đề. Thật không may, với$10^5$máy tính xách tay, số lượng cặp khoảng$5 \times 10^9$. Ngay cả việc triển khai rất nhanh cũng không thể xử lý nhiều so sánh như vậy trong thời gian giới hạn. 

Nhận xét quan trọng là điều kiện chỉ phụ thuộc vào thứ tự giá. Nếu chúng ta sắp xếp tất cả các máy tính xách tay theo giá thì mỗi máy tính xách tay ở bên phải sẽ đắt hơn mọi máy tính xách tay ở bên trái. 

Giả sử chúng ta sắp xếp theo giá và thu được chuỗi chất lượng:```
100 200 150
```Chất lượng giảm từ 200 xuống 150 trong khi giá tăng. Điều đó ngay lập tức đưa ra một cặp hợp lệ. Laptop có chất lượng 200 rẻ hơn laptop có chất lượng 150. 

Tổng quát hơn, sau khi sắp xếp theo giá, niềm tin của Dima là đúng khi và chỉ khi chất lượng cũng tăng lên một cách nghiêm ngặt. Vì tất cả các giá trị chất lượng đều khác biệt nên bất kỳ sự giảm chất lượng nào cũng có nghĩa là chúng tôi đã tìm thấy một chiếc máy tính xách tay rẻ hơn với chất lượng tốt hơn. 

Vì vậy, chúng ta chỉ cần sắp xếp theo giá và quét chất lượng thu được. Nếu ở bất kỳ vị trí nào chất lượng hiện tại nhỏ hơn chất lượng trước đó thì Alex đã đúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả máy tính xách tay dưới dạng`(price, quality)`cặp. 
2. Sắp xếp laptop theo giá tăng dần. 

Sau khi phân loại, mọi máy tính xách tay đều xuất hiện trước tất cả các máy tính xách tay đắt tiền hơn. 
3. Quét danh sách đã sắp xếp từ trái sang phải. 
4. Đối với mỗi cặp liền kề, hãy so sánh phẩm chất của chúng. 
5. Nếu laptop hiện tại có chất lượng kém hơn laptop trước, hãy in`"Happy Alex"`và chấm dứt. 

Laptop trước đây rẻ hơn do được sắp xếp theo thứ tự nhưng chất lượng lại cao hơn. Đây chính xác là tình huống mà Alex mô tả. 
6. Nếu quá trình quét kết thúc mà không thấy chất lượng giảm sút, hãy in`"Poor Alex"`. 

### Tại sao nó hoạt động 

Sau khi phân loại theo giá, các laptop được sắp xếp từ rẻ nhất đến đắt nhất. Nếu chất lượng giảm đi khi di chuyển sang phải thì máy tính xách tay rẻ hơn có chất lượng cao hơn máy tính xách tay đắt tiền hơn, điều này chứng tỏ khẳng định của Alex. 

Ngược lại, nếu chất lượng không bao giờ giảm thì chất lượng sẽ tăng cùng với giá cả. Vì tất cả các đặc tính đều khác biệt, điều này có nghĩa là mọi máy tính xách tay đắt tiền hơn cũng có chất lượng cao hơn. Không có cặp mâu thuẫn nào có thể tồn tại. 

Thuật toán sẽ kiểm tra chính xác thuộc tính này nên sẽ đưa ra câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
laptops = []

for _ in range(n):
    price, quality = map(int, input().split())
    laptops.append((price, quality))

laptops.sort()

for i in range(1, n):
    if laptops[i][1] < laptops[i - 1][1]:
        print("Happy Alex")
        break
else:
    print("Poor Alex")
```Phần đầu tiên đọc tất cả các mô tả về máy tính xách tay thành một danh sách các cặp. Cuộc gọi tới`sort()`sắp xếp chúng theo giá vì giá là phần tử đầu tiên của mỗi bộ dữ liệu. 

Quá trình quét bắt đầu từ máy tính xách tay thứ hai. Đối với mọi vị trí, chúng tôi so sánh chất lượng của nó với chất lượng của máy tính xách tay trước đó theo thứ tự giá. 

Việc giảm chất lượng là đủ để chứng minh cho tuyên bố của Alex. Khi tìm thấy mức giảm như vậy, câu trả lời sẽ được biết và chúng tôi chấm dứt ngay lập tức. 

các`for-else`việc xây dựng ở đây rất thuận tiện. các`else`khối chỉ thực thi nếu vòng lặp hoàn thành mà không gặp phải`break`, có nghĩa là không tìm thấy sự giảm chất lượng. 

Không có lo ngại về tràn vì tất cả các giá trị tối đa đều$10^5$, and the algorithm performs only comparisons and sorting.

 ## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 2
2 1
```Sau khi sắp xếp theo giá: 

| Chỉ mục | Giá | Chất lượng | 
| --- | --- | --- | 
| 0 | 1 | 2 | 
| 1 | 2 | 1 | 

Quét: 

| tôi | Chất lượng trước đó | Chất lượng hiện tại | Giảm bớt? | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | Có | 

Chất lượng giảm từ 2 xuống 1 trong khi giá tăng từ 1 lên 2. Laptop rẻ hơn thì tốt hơn laptop đắt hơn nên câu trả lời là`"Happy Alex"`. 

### Ví dụ 2 

đầu vào:```
4
1 10
2 20
3 30
4 40
```Sau khi sắp xếp theo giá: 

| Chỉ mục | Giá | Chất lượng | 
| --- | --- | --- | 
| 0 | 1 | 10 | 
| 1 | 2 | 20 | 
| 2 | 3 | 30 | 
| 3 | 4 | 40 | 

Quét: 

| tôi | Chất lượng trước đó | Chất lượng hiện tại | Giảm bớt? | 
| --- | --- | --- | --- | 
| 1 | 10 | 20 | Không | 
| 2 | 20 | 30 | Không | 
| 3 | 30 | 40 | Không | 

Không thấy giảm. Mỗi lần tăng giá đều đi kèm với việc tăng chất lượng, vì vậy câu trả lời là`"Poor Alex"`. 

Ví dụ này xác nhận tính bất biến rằng các phẩm chất vẫn tăng theo thứ tự được sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Lưu trữ tất cả các cặp máy tính xách tay | 

Với$n = 10^5$, MỘT$O(n \log n)$giải pháp dễ dàng đủ nhanh. Việc sử dụng bộ nhớ cũng nhỏ, chỉ cần lưu trữ trong$10^5$cặp. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    laptops = [tuple(map(int, input().split())) for _ in range(n)]

    laptops.sort()

    for i in range(1, n):
        if laptops[i][1] < laptops[i - 1][1]:
            print("Happy Alex")
            return

    print("Poor Alex")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
assert run("2\n1 2\n2 1\n") == "Happy Alex", "sample 1"

# minimum size
assert run("1\n5 10\n") == "Poor Alex", "single laptop"

# already increasing
assert run("4\n1 10\n2 20\n3 30\n4 40\n") == "Poor Alex", "perfect agreement"

# contradiction appears after sorting
assert run("3\n30 300\n10 200\n20 100\n") == "Happy Alex", "must sort first"

# contradiction far apart
assert run("5\n1 10\n2 20\n3 30\n4 40\n5 15\n") == "Happy Alex", "late decrease"

# two laptops only, opposite ordering
assert run("2\n100 200\n200 100\n") == "Happy Alex", "smallest non-trivial case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Máy tính xách tay đơn | Alex tội nghiệp | Không có cặp nào tồn tại | 
| Nghiêm túc tăng phẩm chất | Alex tội nghiệp | Niềm tin của Dima được giữ vững | 
| Đầu vào chưa được sắp xếp | Alex vui vẻ | Sắp xếp theo giá là cần thiết | 
| Giảm chất lượng muộn | Alex vui vẻ | Quét phải tiếp tục qua toàn bộ mảng | 
| Hai máy tính xách tay đối diện | Alex vui vẻ | Mâu thuẫn hợp lệ nhỏ nhất | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
5 10
```Sau khi phân loại vẫn chỉ còn lại một chiếc laptop. Quá trình quét không bao giờ thực hiện vì không có cặp nào để so sánh. Thuật toán in chính xác:```
Poor Alex
```vì yêu cầu của Alex yêu cầu hai máy tính xách tay. 

Hãy xem xét một đầu vào trong đó sự mâu thuẫn bị che giấu bởi thứ tự ban đầu:```
3
30 300
10 200
20 100
```Sắp xếp theo giá sẽ tạo ra:```
(10, 200)
(20, 100)
(30, 300)
```Trong quá trình quét, chất lượng giảm từ 200 xuống 100 ở phần tử thứ hai. Thuật toán ngay lập tức xuất ra:```
Happy Alex
```Điều này cho thấy tại sao việc kiểm tra trực tiếp thứ tự đầu vào sẽ không chính xác. 

Xét trường hợp mâu thuẫn xuất hiện ở gần cuối:```
5
1 10
2 20
3 30
4 40
5 15
```Thứ tự sắp xếp không thay đổi. Quá trình quét cho thấy chất lượng ngày càng tăng cho đến lần so sánh cuối cùng:```
40 -> 15
```Vì chất lượng giảm trong khi giá tăng nên thuật toán in:```
Happy Alex
```Điều này chứng tỏ rằng mọi so sánh liền kề trong thứ tự giá phải được kiểm tra cho đến khi tìm thấy vi phạm.
